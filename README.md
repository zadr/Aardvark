# Aardvark

[![CI Status](https://img.shields.io/github/actions/workflow/status/square/aardvark/ci.yml?branch=master)](https://github.com/square/Aardvark/actions?query=workflow%3ACI+branch%3Amaster)
[![Carthage Compatibility](https://img.shields.io/badge/carthage-✓-e2c245.svg)](https://github.com/Carthage/Carthage/)
[![License](https://img.shields.io/cocoapods/l/Aardvark.svg)](https://cocoapods.org/pods/Aardvark)
[![Platform](https://img.shields.io/cocoapods/p/Aardvark.svg)](https://cocoapods.org/pods/Aardvark)

[![Aardvark Version](https://img.shields.io/cocoapods/v/Aardvark.svg?label=Aardvark)](https://cocoapods.org/pods/Aardvark)
[![CoreAardvark Version](https://img.shields.io/cocoapods/v/CoreAardvark.svg?label=CoreAardvark)](https://cocoapods.org/pods/CoreAardvark)
[![AardvarkLoggingUI Version](https://img.shields.io/cocoapods/v/AardvarkLoggingUI.svg?label=AardvarkLoggingUI)](https://cocoapods.org/pods/AardvarkLoggingUI)
[![AardvarkMailUI Version](https://img.shields.io/cocoapods/v/AardvarkMailUI.svg?label=AardvarkMailUI)](https://cocoapods.org/pods/AardvarkMailUI)

Aardvark makes it dead simple to create actionable bug reports.

Aardvark is made up of a collection of frameworks that provide different bug reporting and logging components.

* **CoreAardvark** - The core structures for Aardvark. Safe to run in app extensions.
* **Aardvark** - The core tools for building a bug report.
* **AardvarkMailUI** - A bug reporter implementation that sends the bug report via an email composer.
* **AardvarkLoggingUI** - UI components for viewing Aardvark logs in an iOS app.

## Getting Started

There are only three steps to get Aardvark logging and bug reporting up and running.

### 1) Install Aardvark

The easiest way to get Aardvark up and running is to add a dependency on the AardvarkMailUI framework.

#### Using [CocoaPods](https://cocoapods.org)

```
pod 'AardvarkMailUI'
```

#### Using [Carthage](https://github.com/Carthage/Carthage)

```
github "Square/Aardvark"
```

#### Using Git Submodules

Manually checkout the submodule with `git submodule add git@github.com:Square/Aardvark.git`, drag Aardvark.xcodeproj to your project, and add AardvarkMailUI as a build dependency.

### 2) Set up email bug reporting with a single method call

It is best to do this when you load your application’s UI.

In Swift:

```swift
Aardvark.addDefaultBugReportingGestureWithEmailBugReporter(withRecipient:)
```

In Objective-C:

```objc
[Aardvark addDefaultBugReportingGestureWithEmailBugReporterWithRecipient:]
```

### 3) Replace calls to `print` with `log`.

In Swift, replace calls to `print` with `log`. In Objective-C, replace calls to `NSLog` with `ARKLog`.

## Reporting Bugs

After doing the above, your users can report a bug by making a two-finger long-press gesture. This gesture triggers a UIAlert asking the user what went wrong. When the user enters this information, an email bug report is generated complete with an attached app screenshot and a text file containing the last 2000 logs. Screenshots are created and stored within Aardvark and do not require camera roll access.

[![Bug Report Flow](BugReportFlow.gif)](BugReportFlow.gif)

Want to look at logs on device? Add the AardvarkLoggingUI framework as a dependency and push an instance of [ARKLogTableViewController](Sources/AardvarkLoggingUI/Log%20Viewing/ARKLogTableViewController.h) onto the screen to view your logs.

## Performance

Logs are distributed to loggers on an internal background queue that will never slow down your app. Logs observed by the log store are incrementally appended to disk and not stored in memory.

## Exception Logging

To turn on logging of uncaught exceptions, call `ARKEnableLogOnUncaughtException()`. When an uncaught exception occurs, the stack trace will be logged to the default log distributor. To test this out in the sample app, hold one finger down on the screen for at least 5 seconds.

Once the exception is logged, it will be propogated to any existing uncaught exception handler. By default, the exception will be logged to the default log distributor. To log to a different distributor, call `ARKEnableLogOnUncaughtExceptionToLogDistributor(...)`. You can enable logging to multiple log distributors by calling the method multiple times.

## Customize Aardvark

Want to customize how bug reports are filed? Pass your own object conforming to the [ARKBugReporter](Sources/Aardvark/Bug%20Reporting/ARKBugReporter.h) protocol and the desired subclass of `UIGestureRecognizer` to `[Aardvark addBugReporter:triggeringGestureRecognizerClass:]`. You can further customize how bug reports will be triggered by modifying the returned gesture recognizer.

Want to change how logs are formatted? Or use different log files for different features? Or send your logs to third party services? Check out the [logging documentation](Documentation/Logging.md).

Want to log with Aardvark but don’t want to use Aardvark’s bug reporting tool? Change the dependency to be on `Aardvark` and skip step #2 in the Getting Started guide.

## Extensions

* [AardvarkReveal](https://github.com/cashapp/AardvarkReveal) provides components for generating a bug report attachment containing a [Reveal](https://revealapp.com/) file, which can be a huge help in debugging UI issues.
* [AardvarkCrashReport](https://github.com/cashapp/AardvarkCrashReport) provides components for generating a bug report attachment containing a crash report, either from a crash on the prior app launch or a live report showing the current state of each thread.

## Requirements

* Xcode 11.0 or later
* iOS 12.0 or later
* watchOS 4.0 or later

## Contributing

We’re glad you’re interested in Aardvark, and we’d love to see where you take it. Please read our [contributing guidelines](Contributing.md) prior to submitting a Pull Request.

Thanks, and happy logging!
