# v4.0

## New features
* Added the `AutoSuggestBox` control
* Added the `ContentDialog` control
* Added `Pivot` custom header areas
* Added the `SplitView` control
* Added the `ToolBar` control
* Added the `XYFocus` utility
* Added the ability to alternate styles of `ListView` items in a performant manner
* Added granular `ListView` virtualization options
* Added the `SplitViewPaneToggle` control
* New `ListView` selection model

## Breaking changes
### Core
* (from 4.0-Preview) WinJS.js has been split back into base.js and ui.js

### Utilities
* The `WinJS.Utilities.isPhone` property will return undefined

### AppBar
* Removed the `showCommands` method
* Removed the `hideCommands` method
* Removed the `disabled` property
* Removed the `layout` property
* Removed the `sticky` property
* Renamed the `onafterhide` event to `onafterclose`
* Renamed the `onaftershow` event to `onafteropen`
* Renamed the `onbeforehide` event to `onbeforeclose`
* Renamed the `onbeforeshow` event to `onbeforeopen`
* Renamed the `hide` method to `close`
* Renamed the `show` method to `open`
* Renamed the `commands` property to `data`
* Renamed the `hidden` property to `opened`

### NavBar
* Removed the `disabled` property
* Removed the `layout` property
* Removed the `sticky` property
* Renamed the `onafterhide` event to `onafterclose`
* Renamed the `onaftershow` event to `onafteropen`
* Renamed the `onbeforehide` event to `onbeforeclose`
* Renamed the `onbeforeshow` event to `onbeforeopen`
* Renamed the `hide` method to `close`
* Renamed the `show` method to `open`
* Renamed the `hidden` property to `opened`

### Typography
* Renamed the `.win-type-xx-large` class to `.win-type-header` or `.win-h1`
* Renamed the `.win-type-x-large` class to `.win-type-subheader` or `.win-h2`
* Renamed the `.win-type-large` class to `.win-type-title` or `.win-h3`
* Renamed the `.win-type-medium` class to `.win-type-subtitle` or `.win-h4`
* Renamed the `.win-type-small` class to `.win-type-body` or `.win-h6`
* Renamed the `.win-type-x-small` class to `.win-type-base` or `.win-h5`
* Renamed the `.win-type-xx-small` class to `.win-type-caption`

## Notable changes
### Utilities
* Added the values `NavigationView`, `NavigationMenu`, `NavigationUp`, `NavigationDown`, `NavigationLeft`, `NavigationRight`, `NavigationAccept`, `NavigationCancel`, `GamepadA`, `GamepadB`, `GamepadX`, `GamepadY`, `GamepadRightShoulder`, `GamepadLeftShoulder`, `GamepadLeftTrigger`, `GamepadRightTrigger`, `GamepadDPadUp`, `GamepadDPadDown`, `GamepadDPadLeft`, `GamepadDPadRight`, `GamepadMenu`, `GamepadView`, `GamepadLeftThumbstick`, `GamepadRightThumbstick`, `GamepadLeftThumbStickUp`, `GamepadLeftThumbstickDown`, `GamepadLeftThumbstickRight`, `GamepadLeftThumbstickLeft`, `GamepadRightThumbstickUp`, `GamepadRightThumbstickDown`, `GamepadRightThumbstickRight`, `GamepadRightThumbstickLeft` to the `Key` enum

### AppBar
* Added the interface `ICommand`
* Added the `ClosedDisplayMode` enum
* Added the `Placement` enum
* Added the `forceLayout` method
* Added the `closedDisplayMode` property

### AppBarCommand
* Added the `priority` property
* Added the values `primary` and `secondary` to the `section` property

### ListView
* Added the values `item`, `header`, and `footer` to the `ObjectType` enum
* Added the `footer` property
* Added the `header` property
* Added the `maxLeadingPages` property
* Added the `maxTrailingPages` property

### Pivot
* Added the `customLeftHeader` property
* Added the `customRightHeader` property

### SplitViewTogglePane
* Added the `.win-splitviewpanetoggle` class

### Typography
* Added the `.win-type-body` class
* Added the `.win-link` class
* Added the `.win-code` class
* Added the `.win-ellipsis` class
* Added the `.win-button` class
* Added the `.win-button-primary` class
* Added the `.win-button-file` class
* Added the `.win-textbox` class
* Added the `.win-textarea` class
* Added the `.win-dropdown` class
* Added the `.win-checkbox` class
* Added the `.win-radio` class
* Added the `.win-progress-bar` class
* Added the `.win-progress-ring` class
* Added the `.win-slider` class
* Added the `.win-vertical` class

### XYFocus
* Added the `.win-xbox` class
* (from 4.0-Preview) Removed the `enableXYFocus` method
* (from 4.0-Preview) Removed the `disableXYFocus` method

## Deprecations
* Deprecated the `swipeBehavior` property of `ItemContainer` and `ListView`
* Deprecated the `SettingsFlyout` control
* Deprecated the `SearchBox` control
* Deprecated the values `global` and `selection` from the `section` property in `AppBarCommand`