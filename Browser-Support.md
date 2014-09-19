##WinJS Browser Support
This the following table represents the set of browser and platform configurations that WinJS intends to support.

This means:
* The project is actively tested against this table
* Issues or bugs found in these configurations will be triaged accordingly and fixed in subsequent releases
* If contributing, it is expected pull requests are tested against this table
* Any new feature development can rely on standard native capabilities exposed across these configurations

|   | IE10 | IE11 | Chrome* | Firefox* | Safari* | Android Browser |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| __Windows 7 SP1__ | x | x | x | x | | |
| __Windows 8__ | x | | x | x | | |
| __Windows 8.1__ | x | x | x | x | 
| __Windows Phone 8__ | x | | | | |
| __Windows Phone 8.1__ | | x | | | |
| __Mac OS X Mavericks__ | | | x | x | x |
| __Android Jelly Bean__ | | | x | | | x |
| __Android Kitkat__ | | | x |
| __iOS 7__ | | | | | x |
>*Indicates the current version of the browser

Now this doesn’t mean that WinJS cannot be hosted in other configurations.

There are large portions of WinJS that are written in pure JavaScript and for the most part will run in other environments.

What this does mean is that if an issue is found in a configuration that is not defined in our support table, it’s highly unlikely it will be fixed in a subsequent release. However, if you do find a bug, please submit an issue with your specific configuration. This can help the team understand the various ways WinJS is being used by the community and can give us feedback into planning subsequent releases. 
 
As we move forward with WinJS, it is expected that more configurations will be added or tweaked. 

To see the project's current progress visit the [status](http://try.buildwinjs.com/#status) page on [http://try.buildwinjs.com](http://try.buildwinjs.com)


 