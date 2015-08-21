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

### Feature Detection

### Listening to Global Events

### Styling Accent Color