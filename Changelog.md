# v4.1

## Deprecations
* Fixed: Added deprecation warning to `createResultSuggestionImage` API for `SearchBox` ([2c6b4d0](https://github.com/winjs/winjs/commit/2c6b4d078fa035c80d7b46f29ef0f9c37616ee14), [#1268](https://github.com/winjs/winjs/issues/1268))

## Notable changes
### AppBar
* Fixed: Fails to overflow when `closedDisplayMode === "none"` ([4000e32](https://github.com/winjs/winjs/commit/4000e327ee010fc95c9983495b15806ef104e50f), [#1183](https://github.com/winjs/winjs/issues/1183))
* Fixed: Initializing with `closedDisplayMode: minimal` will layout commands incorrectly ([4000e32](https://github.com/winjs/winjs/commit/4000e327ee010fc95c9983495b15806ef104e50f), [#1220](https://github.com/winjs/winjs/issues/1220))

### Core
* Fixed: Type-To-Search related crash when involving iframes ([c2e5c92](https://github.com/winjs/winjs/commit/c2e5c9262a8c075ddce629f88bff44b81c4e3390), [#1131](https://github.com/winjs/winjs/issues/1131))

### Flyout
* Added `showAt` API ([d6a622a](https://github.com/winjs/winjs/commit/d6a622a8f797fe15c3a539dc5b53fb7fb4a6ff81))
* Fixed: Click-eater blocks light dismissal until closed ([14bc964](https://github.com/winjs/winjs/commit/14bc964525aebb1657c28ef1129e7189703bfa3b), [#1256](https://github.com/winjs/winjs/issues/1256))

### Hub
* Fixed: Crashes when the header is an element with no `classList` API ([7004d49](https://github.com/winjs/winjs/commit/7004d499bd199628e129490b29ee653988f2cc65), [#1254](https://github.com/winjs/winjs/issues/1254))

### ListView
* Disabled animations when `ListView` or `ItemContainer` are in selection mode ([61d854d](https://github.com/winjs/winjs/commit/61d854da2388777e11b94d69c743c868c5b51464))

### Menu
* Added `showAt` API ([d6a622a](https://github.com/winjs/winjs/commit/d6a622a8f797fe15c3a539dc5b53fb7fb4a6ff81))

### Pivot
* Implemented new animations ([1ed0d4a](https://github.com/winjs/winjs/commit/1ed0d4abaac22469260309867dfc91578f19fac9))

### Styling
* Fixed: WinJS crashes when loaded in a `display: none` iframe on Firefox by defaulting to ui-light.css ([a045428](https://github.com/winjs/winjs/commit/a045428815cda2f6b005e1590af64598c07ff972), [#1253](https://github.com/winjs/winjs/issues/1253))
* Updated `AppBar` for high contrast ([189d656](https://github.com/winjs/winjs/commit/189d656635e73a59a7180404f9542431a551e902))
* Updated `ContentDialog` for high contrast ([e9cf1c6](https://github.com/winjs/winjs/commit/e9cf1c6a4e3809dc414de5ef41137911c9ce2b07))
* Updated range control styling ([a88502d](https://github.com/winjs/winjs/commit/a88502d46bb19a920b96d33c5b21ba51aff55191))
* Updated `ToggleSwitch` styles ([60522c1](https://github.com/winjs/winjs/commit/60522c1232a7b76241f54de31a72b466e77c290b))
* Updated `ToolBar` for high contrast ([189d656](https://github.com/winjs/winjs/commit/189d656635e73a59a7180404f9542431a551e902))

### XYFocus

* Fixed: Crashes when loaded in a WebWorker ([0b06c40](https://github.com/winjs/winjs/commit/0b06c40d35bbcfaf902c25bbe682967978b3630f), [#1250](https://github.com/winjs/winjs/issues/1250))

# v4.0.1

## Core
* Fixed: Accent color sometimes not applied ([d3c0820](https://github.com/winjs/winjs/commit/d3c0820130b5d9f1fbdaa0ed9db40af0533e668e), [#1202](https://github.com/winjs/winjs/issues/1202))

# v4.0

## New features
* Added the `AutoSuggestBox` control
* Added the `ContentDialog` control
* Added `Pivot` custom header areas
* Added the `SplitView` control
* Added the `ToolBar` control
* Added the `XYFocus` utility
* Added the ability to [zebra stripe](http://en.wikipedia.org/wiki/Zebra_striping_(computer_graphics)) `ListView` items
* Added granular `ListView` virtualization options
* Added the `SplitViewPaneToggle` control
* Added a new `ListView` selection model

## Breaking changes
### Core
* (from 4.0-Preview) WinJS.js has been split back into base.js and ui.js

### Styling
* Styling of intrinsic elements is [no longer by default](https://github.com/winjs/winjs/wiki/Styling-HTML-Controls)

### Utilities
* The `WinJS.Utilities.isPhone` property will return undefined

### AppBar
* Removed the `disabled` property
* Removed the `hideCommands` method
* Removed the `layout` property
* Removed the `showCommands` method
* Removed the `sticky` property. Set the `closedDisplayMode` property to `compact` or `full` to replicate similar behavior.
* Removed the `.win-commandlayout` CSS class
* Renamed the `commands` property to `data`
* Renamed the `hidden` property to `opened`
* Renamed the `hide` method to `close`
* Renamed the `onafterhide` event to `onafterclose`
* Renamed the `onaftershow` event to `onafteropen`
* Renamed the `onbeforehide` event to `onbeforeclose`
* Renamed the `onbeforeshow` event to `onbeforeopen`
* Renamed the `show` method to `open`
* Renamed the `.win-appbar-hidden` CSS class to `.win-appbar-closed`

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

### SplitView
* (from 4.0-Preview) Renamed the `HiddenDisplayMode` enum to `ClosedDisplayMode`
* (from 4.0-Preview) Renamed the `hiddenDisplayMode` property to `closedDisplayMode`
* (from 4.0-Preview) Renamed the `hidePane` method to `closePane`
* (from 4.0-Preview) Renamed the `onafterhide` event to `onafterclose`
* (from 4.0-Preview) Renamed the `onaftershow` event to `onafteropen`
* (from 4.0-Preview) Renamed the `onbeforehide` event to `onbeforeclose`
* (from 4.0-Preview) Renamed the `onbeforeshow` event to `onbeforeopen`
* (from 4.0-Preview) Renamed the `paneHidden` property to `paneOpened`
* (from 4.0-Preview) Renamed the `ShownDisplayMode` enum to `OpenedDisplayMode`
* (from 4.0-Preview) Renamed the `shownDisplayMode` property to `openedDisplayMode`
* (from 4.0-Preview) Renamed the `showPane` method to `openPane`
* (from 4.0-Preview) Renamed the `.win-splitview-pane-shown` CSS class to `.win-splitview-pane-opened`
* (from 4.0-Preview) Renamed the `.win-splitview-pane-hidden` CSS class to `.win-splitview-pane-closed`

### Typography
* Renamed the `.win-type-xx-large` class to `.win-type-header` or `.win-h1`
* Renamed the `.win-type-x-large` class to `.win-type-subheader` or `.win-h2`
* Renamed the `.win-type-large` class to `.win-type-title` or `.win-h3`
* Renamed the `.win-type-medium` class to `.win-type-subtitle` or `.win-h4`
* Renamed the `.win-type-small` class to `.win-type-body` or `.win-h6`
* Renamed the `.win-type-x-small` class to `.win-type-base` or `.win-h5`
* Renamed the `.win-type-xx-small` class to `.win-type-caption`

### XYFocus
*XYFocus is now enabled by default*
* (from 4.0-Preview) Removed the `enableXYFocus` method
* (from 4.0-Preview) Removed the `disableXYFocus` method

## Deprecations
* Deprecated the `swipeBehavior` property of `ItemContainer` and `ListView`
* Deprecated the `SettingsFlyout` control
* Deprecated the `SearchBox` control. Instead, use the `AutoSuggestBox`.
* Deprecated the values `global` and `selection` from the `section` property in `AppBarCommand`

## Notable changes
### Utilities
* Added the values `NavigationView`, `NavigationMenu`, `NavigationUp`, `NavigationDown`, `NavigationLeft`, `NavigationRight`, `NavigationAccept`, `NavigationCancel`, `GamepadA`, `GamepadB`, `GamepadX`, `GamepadY`, `GamepadRightShoulder`, `GamepadLeftShoulder`, `GamepadLeftTrigger`, `GamepadRightTrigger`, `GamepadDPadUp`, `GamepadDPadDown`, `GamepadDPadLeft`, `GamepadDPadRight`, `GamepadMenu`, `GamepadView`, `GamepadLeftThumbstick`, `GamepadRightThumbstick`, `GamepadLeftThumbStickUp`, `GamepadLeftThumbstickDown`, `GamepadLeftThumbstickRight`, `GamepadLeftThumbstickLeft`, `GamepadRightThumbstickUp`, `GamepadRightThumbstickDown`, `GamepadRightThumbstickRight`, `GamepadRightThumbstickLeft` to the `Key` enum

### AppBar
* Added the `ICommand` interface
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