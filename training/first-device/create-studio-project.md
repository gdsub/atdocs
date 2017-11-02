# Create an Android Things Project

## This lesson teaches you to

1.  Create a Things Project
2.  Create a Home Activity

## You should also read

*   [Projects Overview](https://developer.android.google.cn/studio/projects/index.html)


Things apps use the same structure as those designed for phones and tablets. This similarity means you can modify your existing apps to also run on embedded things or create new apps based on what you already know about building apps for Android.

This lesson describes how to prepare your development environment for Android Things, and the required changes to enable app to run on embedded things.

## Prerequisites

* * *

Before you begin building apps for Things, you must:

*   [Update your SDK tools to version 25.0.3 or higher](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) The updated SDK tools enable you to build and test apps for Things.
*   [Update your SDK with Android 8.0 (Oreo), API 26 or higher](https://developer.android.google.cn/studio/intro/update.html#sdk-manager) The updated platform version provides new APIs for Things apps.

## Get started

* * *

To get started quickly, [create your app project](https://developer.android.google.cn/studio/projects/create-project.html) using the new project wizard:

*   Select **Android Things** as the only form factor.
*   In order to access new APIs for Things, target **Android 8.0 (Oreo)**, API level 26 or higher.
*   Name the new empty activity **HomeActivity**.

The new project wizard automatically adds two necessary items to your Android Studio project: a dependency to the Android Things support [library](#library) and a default launch [activity](#activity).

## Added library

* * *

Android Things devices expose APIs through support libraries that are not part of the Android SDK. The new project wizard automatically adds a dependency to the support library to your app-level `build.gradle` file:

        dependencies {        ...        compileOnly 'com.google.android.things:androidthings:+'    }

The wizard adds `<uses-library>` to your app's manifest file to make this prebuilt library available to the app's classpath at run time.

## Added home activity

* * *

An application intending to run on an embedded device must declare an activity in its manifest as the main entry point after the device boots. Note that the wizard applies an intent filter containing the following attributes:

*   **Action**: [ACTION_MAIN](https://developer.android.google.cn/reference/android/content/Intent.html#ACTION_MAIN)
*   **Category**: [CATEGORY_DEFAULT](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_DEFAULT)
*   **Category**: `IOT_LAUNCHER`

For ease of development, this same activity includes a [CATEGORY_LAUNCHER](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_LAUNCHER) intent filter so Android Studio can launch it as the default activity when deploying or debugging.

    <application>    <uses-library android:name="com.google.android.things"/>    <activity android:name=".HomeActivity">        <!-- Launch activity as default from Android Studio -->        <intent-filter>            <action android:name="android.intent.action.MAIN"/>            <category android:name="android.intent.category.LAUNCHER"/>        </intent-filter>        <!-- Launch activity automatically on boot -->        <intent-filter>            <action android:name="android.intent.action.MAIN"/>            <category android:name="android.intent.category.IOT_LAUNCHER"/>            <category android:name="android.intent.category.DEFAULT"/>        </intent-filter>    </activity></application>

