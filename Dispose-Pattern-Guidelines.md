## 1. What is a disposable element or control?

A disposable element or control defines a dispose function that will get invoked when their logical parent is disposed or dispose is explicitly invoked on the disposable element or control itself.

### How to make an element disposable

```js
var element = document.createElement("div");
document.body.appendChild(element);

// Pretend we have some async work that may not finish before the end of the
// element's lifecycle
var initPromise = initAsync(element);

WinJS.Utilities.markDisposable(element, function() {
    // Note: Idempotency and sub-tree disposal are handled for you
    initPromise.cancel();
});
```

#### Template for disposable controls

For disposable controls, start with this skeleton code.

```js
var myControl = WinJS.Class.define(function(element, options) {
    WinJS.Utilities.addClass(element, "win-disposable");
    this.element = element;
}, {
    dispose: function() {
        // Requirement: Idempotency (no side-effects on subsequent calls)
        if(this._disposed) {
            return;
        }
        this._disposed = true;

        // Requirement: Bottom-up dispose order (clean up children first)
        // One way is to do the following:
        WinJS.Utilities.disposeSubTree(this.element);
        // However, this helper may not cover your scenario, for example if
        // this control is a control that hosts arbitrary user-supplied 
        // content that you do not wish to dispose.

        // Now clean up yourself
    }
});
```

## 2. Difference between the two implementation methods

As seen in the examples above, for controls we implement dispose "manually" because having the `.dispose` function on the prototype is a slight perf-gain, enables documentation (IntelliSence and Doc Comments), and makes debugging easier by being able to "Go to definition" of the dispose function.

The `markDisposable` way of implementing dispose should be used when elements are created **dynamically**, for example in binding and virtualization that require cleanup. 

## 3. Checklist for implementing dispose when authoring controls

- Do:
  - Define a dispose function and add `win-disposable` class to the host element even if you have nothing to dispose in your control
  - Add the `win-disposable` class to the host element in the constructor, if possible around the same time the `.winControl` expando is created on the host element.
  - Make sure the idempotency and disposal order requirements are fulfilled
  - Release and null out any resources and members that would not be automatically released by the garbage collector (gesture recognizer, global event handlers, WinRT objects, etc)
  - Make sure async callbacks won’t execute after dispose, this can be done by:
    - Canceling all ongoing operations (promises, scheduled jobs, animations, timeouts, intervals, xhr requests, etc) that may call back into code that have side effects
    - Making sure all asynchronous callback handlers do a dispose check, especially anonymous async callback handlers that are defined within the closure of another asynchronous callback handler
    - Doing both
- Don't:
  - Do not create a dispose function on the host element
  - For controls that host a well-defined set of child elements, such as the AppBar, do not use the `disposeSubTree` helper, instead, invoke dispose on each hosted child element explicitly.
  - You do not need to and should not null out or remove event listeners that would be automatically garbage collected (event handlers on private members and private members that are implementation details of the control, etc)
  - Do not clean up the host element including clearing out the innerHTML, removal of event listeners, deletion of any expandos on it, removal of class names, etc
  - Do not use the markDisposable way for implementing the dispose pattern for controls, see above.
  - Do not suggest that the control is reverted to a reusable state after disposal
- Decide for yourself whether you do or don't:
  - Be careful when handling the disposal of resources passed to the control; disposal here depends on the spec of the control (sub-controls, commands, templates, etc)
  - Be careful when dealing with dispose for controls that have a static footprint, such as the ListView's static `controlsToDispose` array, Overlay’s static event handlers, and the BackButton's singleton pattern


