This page is a work in progress.

### Styling Hover

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

### Listening to Global Events

### Styling Accent Color

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
```

#### Rationale

Different browsers support different notations for flexbox. For example, some browsers require vendor-prefixed names (e.g. `-webkit-flex`). IE10 strays furthest from the current standard. It supports an older version of flexbox which has all of the same functionality but under completely different names (e.g. `align-items: flex-start` is written as `-ms-flex-align: start`).

The LESS flexbox mixin takes care of handling all of these variations in flexbox notation for you so you don't have to worry about them.

### Manipulating Styles (JavaScript)