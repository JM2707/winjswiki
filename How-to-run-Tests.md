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

### Testing in WWAs
* Open WWAUnitTestsApp.sln in winjs/projects/WWAUnitTestsApp/ with Visual Studio 2013
* Start debugging via F5

### Expected Test Behaviors
* Some UI tests inject DOM elements and explicitly hit test against absolute coordinates, to prevent these tests from failing due to user interactions, you will not be able to interact with the QUnit UI while the tests are running. In general, it is advised to not interact with the test page during a test run as it can produce false negatives.
* There are several tests that test for the loss and gain of focus, these tests typically fail if you interacted with the browser in even the slightest way. They should pass when re-running them.

### Tests Status 
Please refer to [http://winjs.azurewebsites.com/#status](http://try.buildwinjs.com/#status) for the latest status of the unit tests and the list of known issues.