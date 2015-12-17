The Update DOM Pattern is a pattern that some WinJS controls follow including the [SplitView](https://github.com/winjs/winjs/blob/8e8a3b0621fe9f3acd7f815d7e961136c2f220fa/src/js/WinJS/Controls/SplitView/_SplitView.ts#L691-L812), [AppBar](https://github.com/winjs/winjs/blob/8e8a3b0621fe9f3acd7f815d7e961136c2f220fa/src/js/WinJS/Controls/AppBar/_AppBar.ts#L510-L582), and [ToolBar](https://github.com/winjs/winjs/blob/8e8a3b0621fe9f3acd7f815d7e961136c2f220fa/src/js/WinJS/Controls/ToolBar/_ToolBar.ts#L398-L532). Writing code that manipulates the DOM can be tricky and error prone and the goal of this pattern is to make it easier to understand your UI code and avoid bugs.

### Guidelines
  
- Never read from the DOM
- Isolate all DOM writes to a single method: `updateDom`

### Rationale

- Never read from the DOM
  - Reading from the DOM is expensive because it can cause the browser to perform a layout. Consequently, it's best to avoid reading from the DOM.
  - Some controls like to use the DOM as a place to store state. They write some state into the DOM and read it back later. This tends to result in code that is hard to understand because it mixes the intent of the state with its representation in the DOM. It's better to store the state in instance variables which makes the intent clearer. Therefore, there's no reason to read state out of the DOM.
  - Think of the DOM as one big global variable that everyone is always banging on. There's little guarantee that any state your store there won't be overwritten by someone else.
- Isolate all DOM writes to a single method: `updateDom`
  - This frees most of the code in the control from having to worry about which pieces of UI need to be updated when changing a particular instance variable.
  - Enables UI code to focus on states instead of state transitions. Traditionally, code that changes an instance variable has to be aware of which pieces of UI it will affect. With the Update DOM pattern, the code in `updateDom` just has to worry about how the current state of the control should be transoformed into its representation in the DOM. Thus, you're thinking about states instead of state transitions and there are far fewer states than transitions.
  - Makes it easy to disable DOM updates for a period of time. This is useful when the control is playing an animation. This can be achieved by turning `updateDom` into a no-op during the animation.

### Exceptions to the Guidelines

There are a few common exceptions to the guidelines.

  1. **Measuring** We read from the DOM when measuring the size or position of DOM elements. There's no way to avoid this. To minimize the cost of measuring, we cache measurements in instance variables and only remeasure from the DOM when the cache is stale.
  2. **Initialization** When the control is created, it needs to create and initialize some DOM elements. This is typically done in a function called `initializeDom`. `initializeDom` should focus on state that is static throughout the lifetime of the control. Any DOM attributes that may change should be managed entirely by `updateDom`.
  3. **Animations** It's common to write to the DOM for animations and it's common to put this code outside of `updateDom`. The reason for this is that the animation code takes ownership of the DOM during the animation. While the animation is running, `updateDom` does not run. Once that animation has finished, ownership of the DOM is transfered back to `updateDom` and `updateDom` begins running again.

### Motivating Example

#### Traditional Approach

To better understand this pattern, let's build a small control without the pattern using a more traditional approach. This typically involves spreading the code that updates the DOM throughout the entire control.

Suppose we're building a paged list control. The list shows one page at a time and has previous and next arrows to flip pages. When we're on the last page, we shouldn't show the next arrow because there's no next page to go to. Obviously, we need to decide to hide or show the next arrow whenever we change pages:

```js
goToPage: function (page) {
  this.page = page;
  this.element.scrollLeft = this._scrollPositionForPage(page);
  if (page + 1 === this.getPageCount()) {
    this.nextArrow.style.display = "none";
  } else {
    this.nextArrow.style.display = "";
  }
}
```

Another feature of the list control is that the data is editable: you can add and remove items. Let's write some code for that:

```js
removeItem: function (index) {
  this.element.removeChild(index);
}
```

Now let's consider a scenario. Suppose the user is on the second to last page (page 3) so the next arrow is shown. On the last page (page 4), there is only one item. The last item gets deleted so now the user is on the last page. However, the next page arrow is still shown! Why is that? The user didn't change pages: they are still on page 3. Because the user didn't change pages, the `goToPage` function wasn't run and so the next arrow's visibility wasn't updated.

The problem is that not only can changing pages affect the next arrow's visibility but removing list items can also affect it. One possible fix would be to write code to update the next arrow's visibility in `removeItem`.

```js
removeItem: function (index) {
  this.element.removeChild(index);
  if (this.page + 1 === this.getPageCount()) {
    this.nextArrow.style.display = "none";
  } else {
    this.nextArrow.style.display = "";
  }
}
```

We could clean it up a little to avoid duplication by moving the next arrow visibility code into its own function and calling that function from `goToPage` and `removeItem`:

```js
_updateNextArrowVisibility: function () {
  if (this.page + 1 === this.getPageCount()) {
    this.nextArrow.style.display = "none";
  } else {
    this.nextArrow.style.display = "";
  }
},

goToPage: function (page) {
  this.page = page;
  this.element.scrollLeft = this._scrollPositionForPage(page);
  this._updateNextArrowVisibility();
},

removeItem: function (index) {
  this.element.removeChild(index);
  this._updateNextArrowVisibility();
}
```

However, this doesn't change the fact that both `goToPage` and `removeItem` have to know that they could affect the next arrow's visibility. This style of code doesn't scale well:
  - As you add new features, they may also affect the next arrow's visibility and have to call `updateNextArrowVisibility`. For some features, it may not be immediately obvious that they need to do that. For example, it wasn't clear at first that the list editing code should update the next arrow.
  - As you add new pieces of UI, old features may affect them. For example, you may add some new UI that needs to get updated from `goToPage` and/or `removeItem`.

As a control gets large, it can become difficult to keep track of which features influence which pieces of UI. As you add features, you'd rather worry about the details of the feature rather than tracking all of the pieces of UI it might influence.

#### Using the Update DOM Pattern

Now let's see how the example would be written with the Update DOM pattern. Let's start by rewriting `goToPage` and `removeItem`. As noted earlier, in this pattern DOM writes are restricted to the `updateDom` method. Therefore, `goToPage` and `removeItem` do not have to worry about what pieces of UI they influence: they just change some internal state and let `updateDom` handle the rest.

```js
goToPage: function(page) {
  this.page = page;
  this._updateDom();
},

removeItem: function (index) {
  this.items.splice(index, 1);
  this._updateDom();
}
```

`updateDom` is responsible for inspecting the internal state of the control and updating the DOM to match it.

Most of the time, only a small part of the UI will need to update but `updateDom` is responsible for updating the entire UI for the control. To ensure `updateDom` is cheap for small updates, it only writes to the DOM when that part of the DOM is actually out of date. It achieves this by storing a copy of its currently rendered state in an instance variable, `updateDom_rendered`, comparing that state with the desired state, and only writing to the DOM if they don't match.

The verbose variable name, `updateDom_rendered`, was chosen to emphasize that it's only designed to be used within the `updateDom` method.

```js
_updateDom: function () {
  if (!this._updateDom_rendered) {
    // State private to _updateDom. No other method should make use of it.
    //
    // Nothing has been rendered yet so these are all initialized to undefined. Because
    // they are undefined, the first time _updateDom is called, they will all be
    // rendered.
    this._updateDom_rendered = {
      nextArrowVisible: undefined,
      scrollLeft: undefined,
      items: undefined
    };
  }
  
  var rendered = this._updateDom_rendered;
  
  var showingLastPage = this.page + 1 === this.getPageCount();
  var nextArrowVisible = !showingLastPage;
  if (rendered.nextArrowVisible !== nextArrowVisible) {
    this.nextArrow.style.display = nextArrowVisible ? "" : "none";
    rendered.nextArrowVisible = nextArrowVisible;
  }
  
  var scrollLeft = this._scrollPositionForPage(this.page);
  if (rendered.scrollLeft !== scrollLeft) {
    this.element.scrollLeft = scrollLeft;
    rendered.scrollLeft = scrollLeft;
  }
  
  // If needed, update the DOM to match this.items. The details of how this is done
  // are not important for understanding the Update DOM concept and so they're omitted.
}
```

### Summary

The Update DOM pattern takes complexity that is traditionally spread throughout your entire control and isolates it into a single method called `updateDom`. This means that most of the code in your control no longer has to worry about:
  - The performance cost of reading/writing to the DOM
  - Which pieces of UI need to be updated when changing a particular instance variable

An additional benefit of this pattern is that it makes it easy to disable DOM updates for a period of time. This is commonly used to avoid updating the DOM while animations are playing.

### Examples

If you'd like to see some examples of the Update DOM pattern in the WinJS code, take a look at these controls:

  - [SplitView](https://github.com/winjs/winjs/blob/8e8a3b0621fe9f3acd7f815d7e961136c2f220fa/src/js/WinJS/Controls/SplitView/_SplitView.ts#L691-L812)
  - [SplitViewPaneToggle](https://github.com/winjs/winjs/blob/237e56d476faf6930f7d50a6857405d058614e27/src/js/WinJS/Controls/SplitViewPaneToggle/_SplitViewPaneToggle.ts#L186-L234)
  - [AppBar](https://github.com/winjs/winjs/blob/8e8a3b0621fe9f3acd7f815d7e961136c2f220fa/src/js/WinJS/Controls/AppBar/_AppBar.ts#L510-L582)
  - [ToolBar](https://github.com/winjs/winjs/blob/8e8a3b0621fe9f3acd7f815d7e961136c2f220fa/src/js/WinJS/Controls/ToolBar/_ToolBar.ts#L398-L532)