In order to build WinJS, ensure that you have [git](http://git-scm.com/downloads) and [Node.js](http://nodejs.org/download/) installed.

Clone a copy of the master WinJS git repo:
```
git clone https://github.com/winjs/winjs.git
```

Change to the `winjs` directory:
```
cd winjs
```

Optionally, choose a stable revision to build instead of the tip of master. See the [list of releases](https://github.com/winjs/winjs/releases) for other options. 
```
git checkout release/4.0.0-preview
```

Install the [grunt command-line interface](https://github.com/gruntjs/grunt-cli) globally:
```
npm install -g grunt-cli
```

> **Note:** You may need to use sudo (for OSX, *nix, BSD etc) or run your command shell as Administrator (for Windows) to install Grunt globally.


Grunt dependencies are installed separately in each cloned git repo. Install the dependencies with:
```
npm install
```

Edit `src/js/WinJS.js` to include only the modules that you want. Note that `WinJS/Core/_WinJS` is required.

Run the following and the WinJS JavaScript and CSS files will be put in the `bin` directory:
```
grunt
```
