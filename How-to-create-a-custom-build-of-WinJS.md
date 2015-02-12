## Building WinJS from stable NPM modules
Install [RequireJS](http://www.requirejs.org/). The [r.js](http://www.requirejs.org/docs/optimization.html) optimizer is used to build the modules
```
npm install -g requirejs
```
Install the WinJS modules
```
npm install winjs-modules
```

Make a copy of the included example.build.js into your root directory. This file contains the default configuration to build WinJS with r.js optimizer. 

Unix
```
$ cp node_modules/winjs-modules/example.build.js ./build.js
```

Windows
```
> copy node_modules\winjs-modules\example.build.js .\build.js
```

Make a copy of the WinJS-custom.js file. This is the file that defines the modules that you will include into your custom build of WinJS.

Unix
```
cp node_modules/winjs-modules/WinJS-custom.js .
```

Windows
```
copy node_modules\winjs-modules\WinJS-custom.js .
```

Edit your copy of WinJS-custom.js to include only the modules you want. `WinJS/Core` and `WinJS/Core/_WinJS` are required.

Build WinJS with the r.js optimizer
```
r.js -o build.js
```

> **Note:** On Windows you may need to append '.cmd' to 'r.js' so the command will look like 'r.js.cmd -o build.js'

You can find the custom build of WinJS in `bin\WinJS.js` and include it in your application.

### Limitations / Known Issues
*  Currently the modular build doesn't custom-compile CSS since the CSS has not been modularized yet. Pre-built CSS is inside the "css" directory.

