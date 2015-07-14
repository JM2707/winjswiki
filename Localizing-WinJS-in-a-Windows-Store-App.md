These instructions are for use with a Windows Store App.

WinJS provides localized strings in a separate package called `winjs-localization`. You can acquire this package using the same mechanisms available for acquiring WinJS: npm, bower, NuGet, or direct download.

1. Acquire the version of `winjs-localization` that is compatible with your version of WinJS from your preferred package manager:

  #### npm

  ```
  > npm install winjs-localization
  ```

  #### Bower

  ```
  > bower install winjs-localization
  ```

  #### NuGet

  ```
  > Install-Package WinJS-Localization
  ```

  #### Direct Download

  Download `winjs-localization.zip` from https://github.com/winjs/winjs/releases/latest.

2. Add the `winjs-localization` directory to your WinJS project in Visual Studio.
3. `winjs-localization` contains 1 folder per language. Delete the folders for the languages your app will **not** be available in.

That's it. Your app will now use the localized WinJS strings for the appropriate language.