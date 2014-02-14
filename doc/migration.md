## Migrate your adjust SDK for iOS to v3.0.0 from v2.1.x or 2.2.x

We renamed the main class `AdjustIo` to `Adjust`. Follow these steps to update
all adjust SDK calls.

1. Right click on the old `AdjustIo` source folder and select `Delete`. Confirm
   `Move to Trash`.
2. From the Xcode menu select `Find → Find and Replace in Project...` to bring
   up the project wide search and replace. Enter `AdjustIo` into the search
   field and `Adjust` into the replace field. Press enter to start the search.
   Press the preview button and deselect all matches you don't want to replace.
   Press the replace button to replace all `Adjust` imports and calls.

       ![][rename]

3. Download version v3.0.0 and drag the new folder `Adjust` into your Xcode
   Project Navigator.

       ![][drag]

4. Build your project to confirm that everything is properly connected again.

The adjust SDK v3.0.0 added delegate callbacks. Check out the [README] for
details.


## Additional steps if you come from v2.0.x

In the Project Navigator open the source file your Application Delegate. Add
the `import` statement at the top of the file. In the `didFinishLaunching` or
`didFinishLaunchingWithOptions` method of your App Delegate add the following
calls to `Adjust`:

```objc
#import "Adjust.h"
// ...
[Adjust appDidLaunch:@"{YourAppToken}"];
[Adjust setLogLevel:AILogLevelInfo];
[Adjust setEnvironment:AIEnvironmentSandbox];
```
![][delegate]

Replace `{YourAppToken}` with your App Token. You can find in your [dashboard].

You can increase or decrease the amount of logs you see by calling
`setLogLevel:` with one of the following parameters:

```objc
[Adjust setLogLevel:AILogLevelVerbose]; // enable all logging
[Adjust setLogLevel:AILogLevelDebug];   // enable more logging
[Adjust setLogLevel:AILogLevelInfo];    // the default
[Adjust setLogLevel:AILogLevelWarn];    // disable info logging
[Adjust setLogLevel:AILogLevelError];   // disable warnings as well
[Adjust setLogLevel:AILogLevelAssert];  // disable errors as well
```

Depending on whether or not you build your app for testing or for production
you must call `setEnvironment:` with one of these parameters:

```objc
[Adjust setEnvironment:AIEnvironmentSandbox];
[Adjust setEnvironment:AIEnvironmentProduction];
```

**Important:** This value should be set to `AIEnvironmentSandbox` if and only
if you or someone else is testing your app. Make sure to set the environment to
`AIEnvironmentProduction` just before you publish the app. Set it back to
`AIEnvironmentSandbox` when you start testing it again.

We use this environment to distinguish between real traffic and artificial
traffic from test devices. It is very important that you keep this value
meaningful at all times! Especially if you are tracking revenue.

## Additional steps if you come from v1.x

1. The `appDidLaunch` method now expects your App Token instead of your App ID.
   You can find your App Token in your [dashboard].

2. The adjust SDK for iOS 3.0.0 uses [ARC][arc]. If you haven't done already,
   we recommend [transitioning your project to use ARC][transition] as well. If
   you don't want to use ARC, you have to enable ARC for all files of the
   adjust SDK. Please consult the [README] for details.

3. Remove all calls to `[+Adjust setLoggingEnabled:]`. Logging is now enabled
   by default and its verbosity can be changed with the new `[Adjust
   setLogLevel:]` method. See the [README] for details.

4. Rename all calls to `[+Adjust userGeneratedRevenue:...]` to `[+Adjust
   trackRevenue:...]`. We renamed these methods to make the names more
   consistent. The amount parameter is now of type `double`, so you can drop
   the `f` suffixes in number literals (`12.3f` becomes `12.3`).

[README]: ../README.md
[rename]: https://raw.github.com/adeven/adjust_sdk/master/Resources/ios/rename.png
[drag]: https://raw.github.com/adeven/adjust_sdk/master/Resources/ios/drag3.png
[delegate]: https://raw.github.com/adeven/adjust_sdk/master/Resources/ios/delegate3.png
[arc]: http://en.wikipedia.org/wiki/Automatic_Reference_Counting
[transition]: http://developer.apple.com/library/mac/#releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html
[dashboard]: http://adjust.io