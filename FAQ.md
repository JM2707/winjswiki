# Frequently Asked Questions

## I'm building an app targeting Windows Phone, can I use WinJS 3.0?

Yes, you can. However, WinJS 3.0 was focused around cross-platform compatibility and working correctly inside of all supported browsers. No new controls were added since WinJS 2.1.

We are currently working to converge what we previously called our "phone" and "desktop" builds so that you can use the same version of WinJS across all supported devices and platforms, but this work won't be finished until the next major release of WinJS.

## I'm building a Universal App targeting Windows and Windows Phone, can I use WinJS 3.0?

Yes, you can. However, WinJS 3.0 was focused around cross-platform compatibility and working correctly inside of all supported browsers. The feature set is not significantly changed from 2.0/2.1 with some exceptions:

* 3.0 supports custom builds.
* 3.0 allows the AppBar to be "hidden" on desktop apps with a visual affordance similar to Windows Phone AppBars.
* 3.0 no longer uses the "built-in" AppBar for Windows Phone. There is additional work planned in this space, but some developers specifically targeting Windows Phone may want to stick with the 2.1 functionality until that work is finished.
* 3.0 allows desktop apps to use a Pivot control.

We are currently working to converge what we previously called our "phone" and "desktop" builds so that you can use the same version of WinJS across all supported devices and platforms, but this work won't be finished until the next major release of WinJS.


## I'm building a Cordova app, can I use WinJS 3.0?

Absolutely! This is the first release of WinJS specifically intended to support additional platforms.

# I'm building a website/web app, can I use WinJS 3.0?

Absolutely! We have made major investments in cross-browser compatibility with 3.0. Check our our support matrix here:

https://github.com/winjs/winjs/wiki/Browser-Support

