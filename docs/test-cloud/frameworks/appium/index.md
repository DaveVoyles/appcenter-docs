---
title: Preparing Appium Tests for Upload
description: How to upload Appium tests to App Center Test
keywords: app center, test cloud, test
author: lucen-ms
ms.author: lucen
ms.date: 11/30/2020
ms.topic: article
ms.assetid: 898eec94-dfbb-4b10-a72b-b86d3bcf7ff7
ms.service: vs-appcenter
ms.custom: test
---

# Preparing Appium Tests for Upload
[!INCLUDE [Retirement announcement for App Center](~/includes/retirement.md)]
The steps to prepare an app and its test suite for upload vary depending on the test framework. This guide how to prepare Appium tests using Java with JUnit for upload to App Center. For guidance on authoring Appium tests, see the [Appium documentation](https://appium.io/docs/en/latest/).

Note the following limitations for Appium support:

> [!NOTE]
> WebView context, Chrome Web Driver and Chrome browser testing with `browserName` capability are supported!

* No support for TestNG (only JUnit tests are supported).
* No support for Android 4.2 or prior. No support for deprecated UIAutomator driver.
* No support for iOS 9.2.1 or prior. No support for deprecated UIAutomation iOS driver.
* No support for [JUnit @Category attribute](http://maven.apache.org/surefire/maven-surefire-plugin/examples/junit.html). (Can use [Include/Exclude](http://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html) instead)
* Maven version must be at least 3.3.9.
* Current Appium version is 1.22.0. It is regulary updated with new releases.
* JUnit 4.9 - 4.12 is supported; we don't support JUnit 5.
* Tests must target precisely one app. (`MobileCapabilityType.FULL_RESET` is supported)

> [!NOTE] 
> In some cases, tests can still work in App Center if using unsupported tooling or features. However, that unsupported functionality isn't QA'd in future updates and could break without warning.

## Prerequisites
Tests will be run using Maven Surefire, which requires tests to follow [certain naming conventions](https://maven.apache.org/surefire/maven-surefire-plugin/examples/inclusion-exclusion.html):

```text
"**/Test*.java" - includes all of its subdirectories and all Java filenames that start with "Test".
"**/*Test.java" - includes all of its subdirectories and all Java filenames that end with "Test".
"**/*Tests.java" - includes all of its subdirectories and all Java filenames that end with "Tests".
"**/*TestCase.java" - includes all of its subdirectories and all Java filenames that end with "TestCase".
```

Before attempting to upload to App Center Test, make sure that running tests locally on your machine using Maven works:

```text
➜  AppiumTest git:(main) ✗ mvn verify
...
Running MainTest
started: SimpleTest (MainTest)
Setting up capabilities
failed
finished
Tests run: 2, Failures: 0, Errors: 2, Skipped: 0, Time elapsed: 0.728 sec <<< FAILURE!
SimpleTest(MainTest)  Time elapsed: 0.594 sec  <<< ERROR!
...
```

If you're unable to run the tests using command line locally, tests will also not work in App Center Test.

## 1. Changes to the build system

### Step 1 - Add dependency

You'll need to add a dependency for the Appium test extensions:

```xml
<dependency>
    <groupId>com.microsoft.appcenter</groupId>
    <artifactId>appium-test-extension</artifactId>
    <version>1.6</version>
</dependency>
```

This code will ensure the enhanced Android and iOS drivers are available at compile time. The enhanced drivers are provided primarily to enable the `label` feature. See Step 4 for more detail on the `label` feature.

### Step 2 - Add upload profile

Copy [this snippet](https://github.com/Microsoft/AppCenter-Test-Appium-Java-Extensions/blob/master/uploadprofilesnippet.xml) into your `pom.xml` in the `<profiles>` tag. If there's no `<profiles>` section in your pom, make one.
The profile, when activated, will pack your test classes and all dependencies into the `target/upload` folder, ready to be uploaded to Test.

## 2. Changes to the tests

### Step 1 - Add imports

Import these packages into your test classes:

```java
import com.microsoft.appcenter.appium.Factory;
import com.microsoft.appcenter.appium.EnhancedAndroidDriver;
import org.junit.rules.TestWatcher;
import org.junit.Rule;
```

### Step 2 - Instantiate the TestWatcher

Insert this declaration in each of your test classes:

```java
    @Rule
    public TestWatcher watcher = Factory.createWatcher();
```

### Step 3 - Update your driver declaration

Replace your *declaration* of `AndroidDriver<MobileElement>` with `EnhancedAndroidDriver<MobileElement>` or `IOSDriver<MobileElement>` with `EnhancedIOSDriver<MobileElement>`

```java
    private static EnhancedAndroidDriver<MobileElement> driver;
```

### Step 4 - Update your driver instantiations

Replace the way you *instantiate* your driver, such that lines in the form of:

```java
    driver = new AndroidDriver<MobileElement>(url, capabilities);
```

...are changed to:

```java
    driver = Factory.createAndroidDriver(url, capabilities);
```

Using these drivers will still allow you to run your tests locally without additional modifications, but enables you to "label" test steps in your test execution using `driver.label("text")`. The text and a screenshot from the device will be visible in test report in App Center.

It's recommended to call `driver.label` in the `@After` method, which takes a screenshot of the app final state. An example `@After` method for a test could look like this code:

```java
    @After
    public void TearDown(){
        driver.label("Stopping App");
        driver.quit();
    }
```

## 3. Upload to App Center Test

Steps to upload a test:

1. Generate an App Center Test upload command using the instructions at [starting a test run](~/test-cloud/starting-a-test-run.md).
2. Pack your test classes and all dependencies into the `target/upload` folder:
   ```shell
   mvn -DskipTests -P prepare-for-upload package
   ```

3. Run the upload command:
   ```shell
   appcenter test run appium --app "APP_ID" --devices "DEVICE_SET_ID" --app-path PATH_TO_FILE.apk  --test-series "main" --locale "en_US" --build-dir target/upload
   ```

## 4. Performance Troubleshooting
Tests on devices in App Center will execute slightly slower than on a local device. Normally, slower execution is outweighed by having more devices available, allowing parallel test runs.

There are three main sources of slower test runs: re-signing, reinstallation, and network tasks.

### Re-signing (on iOS)
Before being installed on the iOS device, your app goes through a process called re-signing. This process is necessary to make the provisioning profile match the device in the cloud. Re-signing does take some time, typically ~1-2 minutes. Rarely, re-signing also causes performance degradation because re-signed apps are cached. The time consuming process will only run once per binary.

If your Continuous Delivery setup updates the IPA version before building and testing, then the binary will be different for each test and the re-signing penalty will occur more often.

### Reinstallation
On a shared device cloud, it's important for us to guarantee that devices are cleaned between each test. The next customer using the device may be someone from another organization. In App Center Test, the app is automatically uninstalled after the completion of your test run. 

It's possible to omit `MobileCapabilityType.FULL_RESET` and set `MobileCapabilityType.NO_RESET` to `true` to speed up test execution. See [Reset Strategies](https://appium.github.io/appium.io/docs/en/writing-running-appium/other/reset-strategies/) for details.

### Network Tasks
Local network tasks are faster because the server is closer and more dedicated to the remote host.
