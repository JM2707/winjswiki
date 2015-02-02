# WinJS Project History

## Pre-Open Source WinJS
Not many know, but WinJS was actually born in the browser – and in the beginning, goals were lofty and skies were definitely blue. As the project moved forward, vision became clearer and scenarios became crisper. And like all engineering projects, scope, risk, and resources also shaped the vision of the first release WinJS 1.0 with Windows 8.
Since then, WinJS has been subsequently released three more times:
* WinJS 2.0 with Windows 8.1
* WinJS Xbox 1.0 with Xbox One
* WinJS Phone 2.1 with Windows Phone 8.1

From these releases, the team has gathered important feedback from developers who use WinJS for building their applications; as well as, truly understanding just how much developers push and bend APIs to the extreme. This battle testing was a necessity for measuring and evaluating the reliability of the features of WinJS. Also with shipping multiple releases, the team was able to grasp and understand the full impacts of performance, compatibility, and migration for developers moving their applications forward.

The team will be applying all the lessons learned from our previous releases as we move forward with WinJS.

## Open Source WinJS
The number one feature request for WinJS was being able to run it cross-platform and in the browser. The team believes that WinJS should be compatible with the tools, libraries, and solutions that many web developers use and love today. So here are just some of things the team has been working on to achieve this:
* The WinJS project is now an MS Open Tech open source project and hosted on GitHub
* The WinJS build infrastructure has been moved over to use [GruntJS](http://gruntjs.com/)
* CSS files are now being generated with [Less](http://lesscss.org/)
* Unit tests are runnable using [QUnit](http://qunitjs.com/)
* Our source files use the [AMD](http://requirejs.org/docs/whyamd.html) module pattern

WinJS does not have a strong dependency on WinRT. WinJS has adapted to use mechanisms other than WinRT if it’s not available, meaning that it can truly run cross-platform and cross-browser which you can try via: http://try.buildwinjs.com in the browser of your choice. The team still has plenty of work to do here, so try it out, find issues, and file bugs.

Our team is thrilled that we are bringing WinJS back to its browser roots and excited to see where the web takes us.