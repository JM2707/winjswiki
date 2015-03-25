## Building WinJS from stable NPM modules

Install the WinJS modules
```
npm install winjs-modules@4.0.0-preview
```

Move into the winjs-modules directory before building.

Unix
```
$ cd node_modules/winjs-modules/
```

Windows
```
> cd node_modules\winjs-modules\
```

Copy the example config file into config.js. Then make any desired changes to config.js.

Unix
```
$ cp ./example.config.js ./config.js
```

Windows
```
> copy example.config.js config.js
```

Edit WinJS-custom.js to include only the modules you want. 
> **NOTE:** `WinJS/Core` and `WinJS/Core/_WinJS` are required.

Build WinJS

```
node build-winjs.js
```

You can find the custom build of WinJS in `bin\WinJS.js` and include it in your application.

