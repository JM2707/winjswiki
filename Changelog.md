# v4.3.1

## Bug Fixes
### General
* Added `getNavManager` helper to avoid errors when running in Windows 8 compat mode (#1479)


# v4.3.0

## Highlights
### Accessibility Improvements
* Intrinsic controls are more accessible in high contrast mode
* AppBar/ToolBar are more accessible to narrator users
* Menu is more accessible to narrator users
* Pivot is more accessible in high contrast mode
* ToggleSwitch is more accessible in high contrast mode

### TV/Xbox UI Controls
* We now build tv.js and ui-light-tv.css and ui-dark-tv.css, respectively. This file currently only contains the ScrollViewer control which is a scrolling container control, optimized for gamepad input modality.

## Bug Fixes
### General
* Fixed the detection logic for CommonJS (#1354)
* Fixed a crash when loading WinJS in a WebView (#1324)

### AppBar/ToolBar
* Fixed an issue where narrator would excessively announce MenuCommands (#1381, #1400)

### ListView
* Disabled ListView/ItemContainer pressed animations as the animations have been causing issues with invocation/selection near the edge of the control (#1319)

### Pivot
* PivotItems measurements are now more accurate (#1356)
* Fixed an issue where the header focus visual would not display (#1383)

### Rating
* Fixed an issue where using arrow keys to change the ratings would not work (#1365)

### SplitView
* Fixed an issue with tabbing through and closing the SplitView (#1367)

### XYFocus
* IFrames with just a focusable body are now focusable with XYFocus (#1339)
* Fixes an issue where preventDefault would not stop XYFocus from executing (#1378)


# v4.2

## New features
* Added the `SplitViewCommand` control ([6113bbc](https://github.com/winjs/winjs/commit/6113bbc6cc41cccecc9519c4fd14e5d1027a2344))
* Added the `VUI` utility ([adae969](https://github.com/winjs/winjs/commit/adae969d663a0cb3c699a9f3b55a604804c0bc5e))

## Deprecations
* Deprecated the `NavBar`, `NavBarContainer` and `NavBarCommand` controls ([02266ee](https://github.com/winjs/winjs/commit/02266eed1790fee551e98b19ec634f7964d37a9f))

## Notable changes
### AppBar
* Fixed: Elements may be rendered on top of a closed `AppBar` ([7fecf50](https://github.com/winjs/winjs/commit/7fecf504e1000bafdb0457b5d5673581f23936a0), [#1203](https://github.com/winjs/winjs/issues/1203))
* Fixed: Focuses wrong command when programmatically opened ([46f7228](https://github.com/winjs/winjs/commit/46f72285d6c7ee2c855f42cfc1f3322629b4b08f), [#1300](https://github.com/winjs/winjs/issues/1300))
* Fixed: Improve logic for displaying the overflow button ([7cb5776](https://github.com/winjs/winjs/commit/7cb57760beeb0197eb80d33e201edc9b55d505e2), [#982](https://github.com/winjs/winjs/issues/982), [#1170](https://github.com/winjs/winjs/issues/1170), [#1340](https://github.com/winjs/winjs/issues/1340))
* Fixed: Tabbing should not move focus out of opened `AppBar` ([2619e6f](https://github.com/winjs/winjs/commit/2619e6fb5ff86b5c50b72254ff1909d321360bf6), [#1073](https://github.com/winjs/winjs/issues/1073))
* Fixed: Toggling `hidden` property on `Commands` now properly animates ([7b2da7a](https://github.com/winjs/winjs/commit/7b2da7aa9a2aecd56a4bcbc6f91297b2569e203d), [#1055](https://github.com/winjs/winjs/issues/1055))

### ContentDialog
* Fixed: Improve response to the InputPane ([5a1dc55](https://github.com/winjs/winjs/commit/5a1dc55f460543d3b699fa2aa41bfdb152c81867), [#1149](https://github.com/winjs/winjs/issues/1149), [#910](https://github.com/winjs/winjs/issues/910))
* Fixed: Make the `title` a header ([cf7098f](https://github.com/winjs/winjs/commit/cf7098f22f0d23b6fa5128f9f33bc75e7618c2b8), [#1176](https://github.com/winjs/winjs/issues/1176))

### Hub
* Fixed: `Hub` doesn't leave room for the app title ([288cffc](https://github.com/winjs/winjs/commit/288cffca8d5e09f92e9d3cca05251abf032fd5f4), [#1224](https://github.com/winjs/winjs/issues/1224))
* Fixed: Mousewheel scrolling issue on Chrome ([ca91896](https://github.com/winjs/winjs/commit/ca9189662de36b115c4b9074994c5b2b9ce340b1), [#1273](https://github.com/winjs/winjs/issues/1273))

### NavBar
* Fixed: Elements may be rendered on top of a closed `NavBar` ([7fecf50](https://github.com/winjs/winjs/commit/7fecf504e1000bafdb0457b5d5673581f23936a0), [#1203](https://github.com/winjs/winjs/issues/1203))

### Pivot
* Fixed: Header animation goes the wrong direction ([63f6140](https://github.com/winjs/winjs/commit/63f6140543dfac828229025f9baa6198bf78deb5), [#1264](https://github.com/winjs/winjs/issues/1264))
* Fixed: `selectionchanged` event fired before the `selectionIndex` is updated ([8973734](https://github.com/winjs/winjs/commit/89737344bc23a3196de63d81b0552de6f05daba4), [#1317](https://github.com/winjs/winjs/issues/1317))
* Fixed: Touch not being able to interact with custom headers ([549b60f](https://github.com/winjs/winjs/commit/549b60ffcb0594e32900892087cb9643d496657e), [#1263](https://github.com/winjs/winjs/issues/1263))

### SplitView
* Fixed: Add public `invoked` event to `SplitViewCommand` ([6f9f4f2](https://github.com/winjs/winjs/commit/6f9f4f2bcffcce6aa99444753791a18a8168d6cc))
* Fixed: Make `SplitViewCommand` work great when put inside a `SplitView` pane ([05b83d9](https://github.com/winjs/winjs/commit/05b83d91682e897a92c77c3cbe8388e803985547), [#1258](https://github.com/winjs/winjs/issues/1258))
* Fixed: `SplitViewCommand` has the wrong background color ([32fc70e](https://github.com/winjs/winjs/commit/32fc70e97d38861cf1a726e28d66c8de6ce0303b), [#1309](https://github.com/winjs/winjs/issues/1309))

### Styling
* Fixed: Add a spacer element to the end of the overflow area to create 24px of space after the last menu command in `AppBar` and `ToolBar` ([26bee7a](https://github.com/winjs/winjs/commit/26bee7a9c606f362dcb7e4189b04ac99663a16e8))
* Fixed: Disabled submit buttons color ([dda39e6](https://github.com/winjs/winjs/commit/dda39e6e3467b476939ff9a5278392e71c827c38), [#1304](https://github.com/winjs/winjs/issues/1304))
* Fixed: Update `SplitView` for high contrast ([9f942f0](https://github.com/winjs/winjs/commit/9f942f0e78d0e9339227aee8244fc95d0fe05b36))
* Fixed: Update `SplitViewPaneToggle` for high contrast ([cdc550a](https://github.com/winjs/winjs/commit/cdc550a12ff378ef19c67cbf129f2ea8f18e8daa))

### ToolBar
* Fixed: Focuses wrong command when programmatically opened ([46f7228](https://github.com/winjs/winjs/commit/46f72285d6c7ee2c855f42cfc1f3322629b4b08f), [#1300](https://github.com/winjs/winjs/issues/1300))
* Fixed: Improve logic for displaying the overflow button ([7cb5776](https://github.com/winjs/winjs/commit/7cb57760beeb0197eb80d33e201edc9b55d505e2), [#916](https://github.com/winjs/winjs/issues/916), [#982](https://github.com/winjs/winjs/issues/982), [#1170](https://github.com/winjs/winjs/issues/1170), [#1210](https://github.com/winjs/winjs/issues/1210), [#1340](https://github.com/winjs/winjs/issues/1340))
* Fixed: Tabbing should not move focus out of opened `ToolBar` ([2619e6f](https://github.com/winjs/winjs/commit/2619e6fb5ff86b5c50b72254ff1909d321360bf6), [#1073](https://github.com/winjs/winjs/issues/1073))
* Fixed: Toggling `hidden` property on `Commands` now properly animates ([7b2da7a](https://github.com/winjs/winjs/commit/7b2da7aa9a2aecd56a4bcbc6f91297b2569e203d), [#1055](https://github.com/winjs/winjs/issues/1055))

### Typography
* Fixed: Globalization fonts were missing from the intrinsics CSS ([c95741c](https://github.com/winjs/winjs/commit/c95741ce1930f11ca04a5eb13940f6db6a27d935), [#1282](https://github.com/winjs/winjs/issues/1282))

### XYFocus
* Fixed: Exception when using `XYFocus` with no active elements ([2bc9a40](https://github.com/winjs/winjs/commit/2bc9a406c7c3bb75fe91dc07f47c36fd647ce577), [#1311](https://github.com/winjs/winjs/issues/1311))
* Fixed: Exception thrown when an `iframe` is removed from the DOM before the `XYFocus` registration message is handled ([0d4f090](https://github.com/winjs/winjs/commit/0d4f090d410439cbe4bade1d87ca1e8f1841a9aa), [#1312](https://github.com/winjs/winjs/issues/1312))
* Fixed: Input elements are automatically treated as toggle mode enabled ([ddf9627](https://github.com/winjs/winjs/commit/ddf962750f4bb23dfe5f4acd238a004f4bc73cea))
* Fixed: Un-registering disposed `iframes` ([702e625](https://github.com/winjs/winjs/commit/702e625b5e80a08eb179174ee615512c7501aabf), [#1291](https://github.com/winjs/winjs/issues/1291))
* Implemented a toggle mode ([52a7b49](https://github.com/winjs/winjs/commit/52a7b4966143c42954cf4dee260d84e8ebe96751))

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