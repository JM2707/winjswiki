##Tests for Pull Requests
Testing is part of our pull request requirements. Generally, for any pull requests that affect source code to be considered, tests must be supplied and/or modified as part of the pull request. New features will require new tests to be authored and the modification of existing features will require old tests to be updated.

##History of WinJS Tests
At the time WinJS went open source, there were about 4,500 tests in the project. These tests were originally authored against an internal testing tool. In order to enable developers to run these tests we have built a shim layer to make the existing tests work with QUnit instead. For now, all new tests should follow the same style and syntax as the existing tests until we finalize our testing story.

##Authoring Tests
###Finding the right place for your tests
* Locate the folder under the /tests directory that best suits your change
* Open the test file that best suits your change
* Append your test function after the last test in the file

###Anatomy of a test file
```js
// At the top of the file, there are reference tags that define dependencies for this file,
// generally these do not need to be modified.
/// <reference path="..." />

// The following part is the body of the test, here we define the test namespace and test functions.
var WinJSTests = WinJSTests || {}
WinJSTests.ListViewTests = function () {
    this.setUp = function () {
        // This is the per-test setup function which gets called before each test.
    };

    this.tearDown = function () {
        // This is the per-test teardown function which gets called after each test.
    };

    this.testFeatureA = function () {
        // This is a synchronous test function, it must be assigned to the test module
        // as a property that starts with the word 'test'.

        // A synchronous test is considered as completed once the function returns or
        // an exception is thrown.
    };

    // If a test is testing for exceptions to be thrown, the expected exception messages
    // can be tested against using the following syntax.
    this.testFeatureB = function () {
        throw "An expected exception";

        // A synchronous test is considered as completed once the function returns or
        // an exception is thrown.
    };
    this.testFeatureB["LiveUnit.ExpectedException"] = [
        { message: "An expected exception" },
        { message: "Another expected exception" }
    ];

    this.testFeatureC = function (complete) {
        // This is an asynchronous test function, indicated by the 'complete' argument. The
        // 'complete' argument is a function that is to be invoked when this test completes.
        setTimeout(function () {
            complete();
        }, 1000);

        // An asynchronous test function is NOT considered as completed when it returns, the
        // 'complete' callback must be invoked to signal completion.
    };

    this.featureDTest = function () {
        // This is NOT a test function since it is not assigned to a member starting with the
        // word 'test'. However, these functions can be used as utility functions for actual tests.
    };
};

LiveUnit.registerTestClass("WinJSTests.ListViewTests");
```
### Assertion APIs
All assertion APIs are exposed through the `LiveUnit.Assert` namespace, i.e. `LiveUnit.Assert.areEqual(...)`
* areEqual(expected, actual, message)
* areNotEqual(left, right, message)
* fail(message)
* isFalse(falsy, message)
* isTrue(truthy, message)
* isNull(obj, message) // undefined is also accepted
* isNotNull(obj, message)

### Unit Tests Status 
Please refer to [http://try.buildwinjs.com/#status](http://try.buildwinjs.com/#status) for the latest status of the unit tests, and the list of known issues.