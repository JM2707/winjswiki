WinJS tests are separated into several folders, each folder usually tests one module such as a control, or multiple modules of similar concepts such as all UI collection controls and helper classes.

WinJS tests are run using QUnit and are outputted to the /bin folder after a successful build. The /bin/tests/tests.html page contains links to QUnit test pages for each test folder. Each test folder (i.e. /bin/tests/base) contains a 'test.html' file that is the QUnit test page.

To run tests, open or navigate to a folder's test page **and press the start button** - The tests do not auto-start to give developers a chance to set breakpoints for debugging.

### Quick Way
```
grunt test
```
This compiles WinJS and opens /bin/tests/tests.html page.

### Manual Way
* Run grunt
* Open 'tests.html' page in /bin/tests
* -or-
* Open 'test.html' page inside a specific test folder (i.e. /bin/tests/base/test.html)