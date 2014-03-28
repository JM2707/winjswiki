In order build WinJS, ensure that you have [git](http://git-scm.com/downloads) and [Node.js](http://nodejs.org/download/) installed.

Clone a copy of the master WinJS git repo:
```
git clone https://github.com/winjs/winjs.git
```

Change to the `winjs` directory:
```
cd winjs
```

Install the [grunt command-line interface](https://github.com/gruntjs/grunt-cli) globally:
```
npm install -g grunt-cli
```

Grunt dependencies are installed separately in each cloned git repo and install the dependencies with:
```
npm install
```

Run the following and the WinJS JavaScript and CSS files will be put in the `bin` directory:
```
grunt
```
