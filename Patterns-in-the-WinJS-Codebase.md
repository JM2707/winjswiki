This page is a work in progress.

### Styling Hover
#### Guidance

When styling hover, you must consider the following rules:

1. (LESS) Hover rules should be expressed inside of the `ColorsHover` LESS mixin. Note that the specificity of rules within the `ColorsHover` mixin increases by 1 element and 1 class.
2. (LESS) Non-hover rules which include `:hover` in their selector **do not** go into the `ColorsHover` mixin. `:focus` and `:active` rules commonly fall into this category. For such rules, you'll need to consider the specificity of rules within the `ColorsHover` mixin and it's common to need to boost the specificity of the rule by 1 class. The convention is to duplicate the last class of the rule.
3. (JavaScript/TypeScript) Modules which style hover must include the [`_Hoverable` module](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/js/WinJS/Utilities/_Hoverable.js).
  - **TypeScript Caveat**: In TypeScript, it's not enough to merely `require` the `_Hoverable` module. Because the `_Hoverable` module isn't actually used, TypeScript will omit the `_Hoverable` module from the generated JavaScript. Consequently, you have to access a property of the `_Hoverable` module to ensure that TypeScript will include it in the generated JavaScript. Like this:

    ```ts
    import _Hoverable = require("../../Utilities/_Hoverable");
    
    // Force-load Dependencies
    _Hoverable.isHoverable;
    ```

Let's do an example to illustrate rules `(1)` and `(2)`. Suppose we start out with rules that look like this ([specificity calculator](http://specificity.keegan.st/)):

```less
.Colors(@theme) {
  // Hover rule
  // specificity: 0,0,3,0
  .win-appbar .win-overflow-button:hover {
    background-color: @listHover;
  }

  // Active rule (non-hover rule -- includes :hover just to make
  // sure we beat the :hover rule)
  // specificity: 0,0,4,0
  .win-appbar .win-overflow-button:active:hover {
    background-color: @listActive;
}
```

As instructed by `(1)`, we take the hover rule and put it into the `ColorsHover` mixin.

```less
.ColorsHover(@theme) {
  // hover rule
  // specificity: 0,0,4,1
  .win-appbar .win-overflow-button:hover {
    background-color: @listHover;
  }
}
```

The active rule is a non-hover rule so it stays outside of the `ColorsHover` mixin as per `(2)`. Recall that the `ColorsHover` mixin causes the rule's specificity to be boosted by 1 class and 1 element. To make sure the `active` rule continues to beat the `hover` rule, we boost its specificity by 1 class by duplicating its last class:

```less
.Colors(@theme) {
  // active rule
  // specificity: 0,0,5,0
  .win-appbar .win-overflow-button.win-overflow-button:active:hover {
    background-color: @listHover;
  }
}
```

#### Rationale

The motivation for this funky hover styling guidance is that `:hover` styles get stuck rendered on an element after tapping on it with touch in webkit ([#288](/winjs/winjs/issues/288)). The fix is to disable WinJS's hover styles in such browsers when the user is using touch. The `:hover` styling guidance allows WinJS to enable and disable its hover rules as it sees fit.

#### Mechanics

The `_Hoverable` module adds the `win-hoverable` class to the `html` element during start up. If a touch event is detected in webkit, the `win-hoverable` class is removed. WinJS's hover rules are only in affect when the `win-hoverable` class is present. Thus WinJS's hover styles are disabled when removing the `win-hoverable` class.

The `ColorsHover` mixin prefixes rules with `html.win-hoverable`. For example:

```less
.ColorsHover(@theme) {
  .win-appbar .win-overflow-button:hover {
    background-color: @listHover;
  }
}
```

expands to:

```less
html.win-hoverable .win-appbar .win-overflow-button:hover {
  background-color: @listHover;
}
```

That prefixing explains why rules inside of the `ColorsHover` mixin have their specificity increased by 1 class (`.win-hoverable`) and 1 element (`html`).

### getComputedStyle
#### Guidance
**Don't** call `_Global.getComputedStyle`.

Instead, call `_ElementUtilities._getComputedStyle`.

For JavaScript code, we have [a jscs rule](https://github.com/winjs/winjs/blob/3356f7d77de52035b21db1623e59efd1abe65178/tasks/utilities/disallow-direct-get-computed-style.js) to enforce this guidance.

#### Rationale

Firefox's implementation of `getComputedStyle` has a bug where it returns `null` when called within an iframe that is `display:none`. This violates the `getComputedStyle` contract. `_ElementUtilities._getComputedStyle` is a helper which upholds the contract of always returning an object whose keys are CSS attributes that map to strings.

See [#1253](/winjs/winjs/issues/1253) for more details.

### Pointer/Touch Events
#### Guidance

**Don't** register for [pointer events](http://www.w3.org/TR/pointerevents/) directly. Never register for [touch events](http://www.w3.org/TR/touch-events/) directly.

Instead, register for pointer events via `_ElementUtilities._addEventListener`. This helper generates synthetic pointer events which work in all browsers regardless of whether they support pointer events or only touch events.

For JavaScript code, we have [a jscs rule](https://github.com/winjs/winjs/blob/3356f7d77de52035b21db1623e59efd1abe65178/tasks/utilities/disallow-direct-pointer-events.js) to enforce this guidance.
#### Rationale

Some browsers that WinJS cares about do not support pointer events. The `_ElementUtilities._addEventListener` helper enables you to sign up for pointer events that work in all browsers. For browsers that do not support pointer events, this helper polyfills them.

### Focus/Blur Events
#### Guidance

**Don't** register for focus/blur events on elements directly (e.g. `focus`, `blur`, `focusin`, `focus out`).

Instead, register for `focusin` and `focusout` on elements using `_ElementUtilities._addEventListener`.

**Exception:** This helper is for focus/blur events on *elements* so if you want to listen to `blur` on `window`, you should just sign up for `blur` on `window` directly rather than using this helper.

**Caveat:** Under some circumstances, the helper may appear to fail to fire a `focusout` event. The helper is built on top of the browser's native `focus`/`focusin` events only so if there's a scenario where the browser fires its native `blur`/`focusout` event but not its native `focus`/`focusin` event, then the helper will appear to fail to fire a `focusout` event. For example, this can happen if an element loses focus due to it being deleted which causes the browser to move focus to `body`. Because `body` doesn't have a `tabIndex` by default, the browser will not fire a `focus`/`focusin` event and consequently the helper will not fire a `focusout` event.

#### Rationale

The behavior of the focus/blur events varies between browsers. Some examples of the variations:
  - Some browsers do not support `focusin` or `focusout`. They only support `focus` and `blur`.
  - In most browsers, `focus` and `blur` fire synchronously while in IE they fire asynchronously.
  - In IE, within the `blur` event handler, `document.activeElement` is the element that will be receiving focus. In most other browsers, `document.activeElement` is null in that case. This makes it difficult to determine what element will be receiving focus within the `blur` event handler.

The `_ElementUtilities._addEventListener` helper provides implementations of `focusout` and `focusin` which work consistently in all browsers. It provides a polyfill for browsers that do not support these events. Some characteristics of the implementation:
  - The events fire synchronously
  - Using `target` and `relatedTarget` off of the `eventObject`, you can determine both who is losing and who is gaining focus from within either event.
  - `document.activeElement` has a consistent value within these event handlers in all browsers.

### Feature Detection
#### Guidance
**Don't** detect platforms. Examples:

```js
// DON'T do these things:
if (hasWinRT) { ... }
if (isFirefox) { ... }
if (isXbox) { ... }
```

Instead, detect the particular features you are interested in using:

```js
// DO these things 

if (_WinRT.Windows.UI.ViewManagement.InputPane) { ... }

var supportsCssGrid = !!("-ms-grid-row" in _Global.document.documentElement.style);
if (supportsCssGrid) { ... }
```

##### Notes on WinRT

When using a `WinRT` API, ensure that the API appears in [`_WinRT.js`](https://github.com/winjs/winjs/blob/a51bc901243b9c4eb646bd414e707cd0aa8ce30c/src/js/WinJS/Core/_WinRT.js). For example, if you wanted to use `Windows.UI.ViewManagement.InputPane`, put that API into `_WinRT.js` and then you can feature detect it like this:

```js
if (_WinRT.Windows.UI.ViewManagement.InputPane) {
  // Use Windows.UI.ViewManagement.InputPane
}
```

The `_WinRT` module ensures that it's safe to "dot into" each object of the API. Without the `_WinRT` module, you'd have to write much more verbose feature detection code. For example:

```js
// _WinRT module saves you from this kind of verbosity
if (Windows && Windows.UI && Windows.UI.ViewManagement && Windows.UI.ViewManagement.InputPane) {
  // Use Windows.UI.ViewManagement.InputPane
}
```

#### Rationale

Detecting features rather than detecting platforms has a number of benefits including:
  - If a platform adds support for a feature that WinJS uses, WinJS will begin taking advantage of that feature in that platform without any change to the WinJS code.
  - If a platform removes support for a feature WinJS uses, WinJS will continue to work on that platform without any change to the WinJS code.

We've even gone so far as to [feature detect bugs](https://github.com/winjs/winjs/blob/a51bc901243b9c4eb646bd414e707cd0aa8ce30c/src/js/WinJS/Controls/ListView/_Layouts.js#L217-L227). Essentially, if we detect that a bug exists, we run code that works around the bug. When the browser fixes the bug, WinJS will automatically stop using the workaround in that browser. This is useful if there are downsides (e.g. performance) to the workaround.

### Listening to Global Events
#### Guidance
**Don't** sign up for global events directly. For example:

```js
// DON'T do this
window.addEventListener("wheel", handler);
```

Instead, sign up thru one of the helpers in `_ElementUtilities`. For example:

```js
// DO this
_ElementUtilities._globalListener.addEventListener(element, "wheel", handler);
```

There are a number of [different helpers in `_ElementUtilities`](https://github.com/winjs/winjs/blob/14ac97cfceebf46fed769e7c95fdad7507b68cc5/src/js/WinJS/Utilities/_ElementUtilities.js#L1281-L1285) for different global objects including:
  - `window`: `_globalListener`
    - `window` `resize`: `_resizeNotifier` (this should probably just be `_globalListener` but it predates everything else)
  - `document.documentElement`: `_documentElementListener`
  - `Windows.UI.ViewManagement.InputPane.getForCurrentView()`: `_inputPaneListener`

There's also [`Application._applicationListener`](https://github.com/winjs/winjs/blob/14ac97cfceebf46fed769e7c95fdad7507b68cc5/src/js/WinJS/Application.js#L854-L857) for listening to events on `WinJS.Application`.

If you need to register for events on a global object which isn't listed, it's easy to create a new helper using [`GenericListener`](https://github.com/winjs/winjs/blob/14ac97cfceebf46fed769e7c95fdad7507b68cc5/src/js/WinJS/Utilities/_ElementUtilities.js#L794-L893).

#### Rationale

This approach avoids memory leaks. If you were to register for an event on a global object directly and for some reason failed to unregister the handler when your object was done being used, your object would be leaked by the global object.

For example, suppose a control signs up for `window` resize directly. If the control was thrown away without `dispose` being called on it, the `window` would prevent the control from ever being garbage collected because the control is still listening to `window` resize.

The `_ElementUtilities` helpers work by indirectly signing clients up to the event. `_ElementUtilities` is the only one that registers for the event directly so only it can be leaked. However, this is okay because `_ElementUtilities` will exist for the lifetime of the application anyway. Elements interested in a particular event add a special class name to themselves indicating that they're interested in the event. When the event fires, `_ElementUtilities` queries the DOM for elements with the unique class name for that event and notifies those elements that the event has fired. In this way, elements sign up indirectly for events and avoid the risk of being leaked.

### Styling Accent Color
#### Guidance
For a control to utilize accent color, it should:

1. Import the [accent color module](https://github.com/winjs/winjs/blob/3356f7d77de52035b21db1623e59efd1abe65178/src/js/WinJS/_Accents.ts)
2. Declare its accent color styles using the `createAccentRule` function. This function is designed to resemble declarative styling via CSS. Its signature looks like this:

  ```js
  createAccentRule(selector: string, props: { name: string; value: ColorTypes; }[])
  ```

  Here's an example usage. Suppose you wanted to style the `outline-color` of the element with class name `win-contentdialog-dialog` to be the accent color. Here's how you would do that:

  ```js
  _Accents.createAccentRule(
    ".win-contentdialog-dialog", [
      { name: "outline-color", value: _Accents.ColorTypes.accent }
    ]
  );
  ```

  Controls **should not call** `createAccentRule` [lazily](#lazy-modules). Instead, they should call `createAccentRule` eagerly when the WinJS JavaScript files are being loaded (i.e. outside of the lazy portion of their module). This ensures that `createAccentRule` will batch up accent color styles for all of the controls and dynamically generate a stylesheet one time. Dynamically generating CSS rules is expensive because it requires the browser to recalculate CSS formatting for the whole page so `createAccentRule` ideally only does this one time.

#### Rationale

In Windows 10, users are able to select a systemwide accent color which some WinJS controls consume. Ideally, controls would style against the accent color in CSS using a CSS variable. However, such a variable is not currently available.

The platform only exposes the accent color via the `Windows.UI.ViewManagement.UISettings.getColorValue` API. If each control had to call this API directly, it would result in unnecessarily complicated code. Each control would have to worry about updating all of its accent colors when the user switched accent colors. Certain styling scenarios would be extra complicated requiring the control to sign up for several events (e.g. using accent color on hover would require registering for the `pointerenter` and `pointerleave` events).

Instead, we've opted for the `createAccentRule` API which enables controls to express accent color styles in a declarative way resembling CSS.

### Animations
#### Guidance

**Don't** use `WinJS.UI.executeAnimation` for doing programmatic animations.

Instead, use `WinJS.UI.executeTransition`.

#### Rationale

`executeAnimation` is based on CSS animations. To guarantee performance, you need to handwrite the keyframe and include it in the WinJS stylesheet. If you fail to do that, `executeAnimation` will generate the keyframe on the fly and add it to a dynamically generated stylesheet. This is a very expensive operation which will cause the browser to recalculate CSS formatting for the entire page.

Instead, use `executeTransition` which is based on CSS transitions. There are no keyframes involved so there's no risk of accidentally making `executeTransition` expensive.

### Flexbox (LESS)
#### Guidance

**Don't** use flexbox styles directly. For example:

```less
// DON'T do this
.win-someelement {
  display: flex;
  flex-direction: column;
}
```

Instead, use WinJS's [LESS mixin for flexbox](https://github.com/winjs/winjs/blob/3356f7d77de52035b21db1623e59efd1abe65178/src/less/mixins.less). For example:

```less
// DO this
.win-someelement {
  #flex > .display-flex();
  #flex > .flex-direction(column);
}
```

#### Rationale

Different browsers support different notations for flexbox. For example, some browsers require vendor-prefixed names (e.g. `-webkit-flex`). IE10 strays furthest from the current standard. It supports an older version of flexbox which has all of the same functionality but under completely different names (e.g. `align-items: flex-start` is written as `-ms-flex-align: start`).

The LESS flexbox mixin takes care of handling all of these variations in flexbox notation for you so you don't have to worry about them.

### Manipulating Styles (JavaScript)
#### Guidance

**Don't** manipulate the CSS attributes [in this list](https://github.com/winjs/winjs/blob/3356f7d77de52035b21db1623e59efd1abe65178/src/js/WinJS/Core/_BaseUtils.js#L89-L123) directly. For example:

```js
// DON'T do this
someElement.style.transform = "";
```

Instead, get the name of the property thru the `_BaseUtils._browserStyleEquivalents` helper and then manipulate that. For example:

```js
// DO this
var transformName = _BaseUtils._browserStyleEquivalents["transform"].scriptName;
someElement.style[transformName] = "";
```

#### Rationale

Some browsers only support vendor-prefixed versions of properties. For example, Safari supports `-webkit-transform` but not `transform`. The `_BaseUtils._browserStyleEquivalents` helper maps unprefixed CSS attribute names to the name that will work in the current browser.

### Lazy Modules
#### Guidance

When defining a module, run as much of the code lazily/on-demand as possible. For example, suppose we were defining the `WinJS.UI.ShinyWidget`. During start up, the only code that should run is enough code to publish a `WinJS.UI.ShinyWidget` property. The code for defining constants, helpers, the `ShinyWidget` class, etc. should only run the first time somebody tries to access the `WinJS.UI.ShinyWidget` property.

##### JavaScript

**Don't** write code like this because all of the code will run during start up:

```js
define([
    '../Core/_Base',
], function shinyWidgetInit(_Base) {
    "use strict";

    // DON'T write code like this. All of this code will run
    // during start up.

    var constant1 = ...;
    var constant2 = ...;
    var constant3 = ...;

    function helper1() {
        ...
    }
    function helper2() {
        ...
    }
    function helper3() {
        ...
    }

    _Base.Namespace.define("WinJS.UI", {
        ShinyWidget: _Base.Class.define(function shinyWidget_ctor(element, options) {
            ...
        }, {
            instanceMember1: function () { },
            instanceMember2: function () { }
        }, {
            staticMember1: function () { },
            staticMember2: function () { },
        }
    });
});
```

Instead, use the `_Base.Namespace._lazy` so that none of the code inside of `_lazy` is run during start up -- it only gets run on demand the first time the `WinJS.UI.ShinyWidget` property is accessed.

```js
define([
    '../Core/_Base',
], function shinyWidgetInit(_Base) {
    "use strict";

    // DO write code like this.

    _Base.Namespace.define("WinJS.UI", {
        ShinyWidget: _Base.Namespace._lazy(function () {

            // All of this code will run on demand the first time
            // the WinJS.UI.ShinyWidget property is accessed.

            var constant1 = ...;
            var constant2 = ...;
            var constant3 = ...;

            function helper1() {
                ...
            }
            function helper2() {
                ...
            }
            function helper3() {
                ...
            }

            var ShinyWidget = _Base.Class.define(function shinyWidget_ctor(element, options) {
                ...
            }, {
                instanceMember1: function () { },
                instanceMember2: function () { }
            }, {
                staticMember1: function () { },
                staticMember2: function () { },
            }
        });
        return ShinyWidget;
    });
});
```

##### TypeScript

Because TypeScript has built-in syntax for defining modules, we can't use the `_Base.Namespace._lazy` helper to make modules lazy like we can in JavaScript. We've come up with an alternate pattern which involves creating two files to make a TypeScript module lazy: one file which is loaded eagerly and the other file which is loaded lazily. Let's do the `WinJS.UI.ShinyWidget` example from above in TypeScript.

First, we'll look at the pattern of the eagerly loaded file. For controls, the convention is that this file goes in the `Controls` folder and its name **does not** begin with an underscore which indicates that it is a public module.

```ts
// Eagerly loaded file
// src/js/WinJS/Controls/ShinyWidget.ts

// All code that should be run during start up goes in here.

import _Base = require('../Core/_Base');

// Note that no members of _ShinyWidget are used in this file.
// It's only used for type information. Consequently, TypeScript
// will not include _ShinyWidget in the generated JavaScript and
// thus this file will not force the _ShinyWidget module to be
// loaded eagerly.
import _ShinyWidget = require('./ShinyWidget/_ShinyWidget');

var module: typeof _ShinyWidget = null;

_Base.Namespace.define("WinJS.UI", {
    ShinyWidget: {
        get: () => {
            if (!module) {
                // Load the _ShinyWidget module on demand the first time
                // somebody tries to access the WinJS.UI.ShinyWidget
                // property.
                require(["./ShinyWidget/_ShinyWidget"], (m: typeof _ShinyWidget) => {
                    module = m;
                });
            }
            return module.ShinyWidget;
        }
    }
});
```

Now let's look at the lazily loaded file. For controls, the convention is that we create a folder that has the same name as the control (e.g. `ShinyWidget`) and this folder goes into the `Controls` folder. Then the lazily loaded file goes inside of the `ShinyWidget` folder and the file name is the control name prefixed with an underscore (e.g. `_ShinyWidget.ts`) to indicate that it is not public -- consumers should reference the eagerly loaded file rather than the lazily loaded file.

```ts
// Lazily loaded file
// src/js/WinJS/Controls/ShinyWidget/_ShinyWidget.ts

// All code that should be run on demand the first time the
// WinJS.UI.ShinyWidget property is accessed should go in here.

var constant1 = ...;
var constant2 = ...;
var constant3 = ...;

function helper1() {
    ...
}
function helper2() {
    ...
}
function helper3() {
    ...
}

export class ShinyWidget {
    static staticMember1(): void { }
    static staticMember2(): void { }
    static staticMember3(): void { }

    instanceMember1(): void { }
    instanceMember2(): void { }
    instanceMember3(): void { }
}
```

#### Rationale

The benefit of lazy modules is in making start up time faster. Prior to lazy modules, all WinJS initialization code was run during start up (e.g. code to define constants, helper functions, classes). After lazy modules were introduced, only the minimum amount of code is run during start up to define the API surface of WinJS (i.e. everything under the `WinJS` namespace). All of the other initialization code is run on demand as the app accesses properties of the `WinJS` namespace. This improved WinJS's start up performance noticeably.

### d.ts Files

#### Guidance

WinJS has a few different categories of TypeScript d.ts files:

##### [`winjs.d.ts`](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/typings/winjs/winjs.d.ts)

This file represents the public API surface of WinJS (i.e. the `WinJS` namespace). It is consumed by apps written in TypeScript as well as by the WinJS unit tests which are written in TypeScript.

The [dts-verifier](https://github.com/winjs/winjs/tree/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/tools/dts-verifier) tool helps us ensure that `winjs.d.ts` is an accurate representation of the WinJS API surface.

##### [`winjs.dev.d.ts`](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/tests/TestLib/winjs.dev.d.ts)

This file is consumed by the WinJS unit tests which are written in TypeScript. This file should contain any private WinJS APIs that are needed by the unit tests.

##### Per Module d.ts Files

Some WinJS modules written in JavaScript have d.ts files associated with them. Some examples:
  - [_ElementUtilities.js](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/js/WinJS/Utilities/_ElementUtilities.js) has [_ElementUtilities.d.ts](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/js/WinJS/Utilities/_ElementUtilities.d.ts)
  - [Promise.js](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/js/WinJS/Promise.js) has [Promise.d.ts](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/js/WinJS/Promise.d.ts)
  - [Animations.js](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/js/WinJS/Animations.js) has [Animation.d.ts](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/js/WinJS/Animations.d.ts)

These kinds of d.ts files are needed for WinJS modules which are written in JavaScript and are consumed by WinJS modules which are written in TypeScript.  These d.ts files are only consumed internally by WinJS code.

#### Summary

Internal WinJS code uses WinJS thru modules whereas external code (e.g. apps, unit tests) uses WinJS thru the `WinJS` namespace which gets published off of `window`.

### Localization

#### Localizing a String

Whenever a string needs to be localized, you add it to the resjson file for US English: [`strings/en-us/Microsoft.WinJS.resjson`](https://github.com/winjs/winjs/blob/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/strings/en-us/Microsoft.WinJS.resjson)

The en-us resjson file contains key value pairs where the key represents a unique name for the string and the value is the english translation of the string.

The en-us resjson file regularly gets handed off to the localization team who ensures the strings are translated into the roughly 100 languages that WinJS supports. The resjson file for each language is stored in the [`strings`](https://github.com/winjs/winjs/tree/17a5ffe0c440d43e9997eb2effb13a1727e4fcaf/src/strings) folder.

#### Consuming a String

To use the localized version of a string, you generally have an object called `strings` near the top of your file which has a key per localized string used by the file. For example:

```js
var strings = {
    get closeOverlay() { return _Resources._getWinJSString("ui/closeOverlay").value; },
};
```

And when you need to use the localized string, you write: `strings.closeOverlay`.

To understand how an app makes use of WinJS's localized resources, see:
  - Windows Store App: [Localizing WinJS in a Windows Store App](Localizing-WinJS-in-a-Windows-Store-App)
  - Web app: [#1163](/winjs/winjs/issues/1163)