WinJS has a number of dismissable controls such as:
  - AppBar
  - ContentDialog
  - Flyout
  - Menu
  - SplitView

### Guidance

Each of these dismissable controls must be a direct child of the `body` element.

(Note that the ToolBar is a dismissable but it doesn't have this requirement. It is specifically designed to be usable anywhere in the DOM.)

### Rationale

The guidance is motivated by [stacking contexts](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Understanding_z_index/The_stacking_context). When a dismissable opens, WinJS puts the full screen click eater element into the body which prevents users from interacting with anything other than the dismissable while the dismissable is open. WinJS sets `z-indicies` on the click eater and the dismissable so that the dismissable is on top of the click eater. For this to work, it's important that both of these elements are in the body's stacking context. The safest way to achieve that is to put the dismissable directly into the body. It's risky to parent the dismissable under a different element because if any of the dismissable's ancestors happens to be a stacking context (other than the body), then the dismissable will not interact properly with the click eater.