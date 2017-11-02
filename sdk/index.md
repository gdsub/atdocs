# Overview


Android Things makes developing connected embedded devices easy by providing the same Android development tools, best-in-class Android framework, and Google APIs that make developers successful on mobile.

![""](https://developer.android.google.cn/things/images/platform-architecture.png)

Apps for embedded devices bring developers closer to hardware peripherals and drivers than phones and tablets. In addition, embedded devices typically present a single app experience to users. This document goes over the major additions, omissions, and differences between core Android development and Android Things.

Android Things extends the core Android framework with additional APIs provided by the Things Support Library. These APIs allow apps to integrate with new types of hardware not found on mobile devices.

The Android Things platform is also streamlined for single application use. System apps are not present, and your app is launched automatically on startup to immerse your users in the app experience.

## Things Support Library

* * *

### Peripheral I/O API

The Peripheral I/O APIs let your apps communicate with sensors and actuators using industry standard protocols and interfaces. The following interfaces are supported: GPIO, PWM, I2C, SPI, UART.

See the [Peripheral I/O API Guides](https://developer.android.google.cn/things/sdk/pio/index.html) for more information on how to use the APIs.

### User Driver API

User drivers extend existing Android framework services and allow apps to inject hardware events into the framework that other apps can access using the standard Android APIs.

See the [User Driver API Guides](https://developer.android.google.cn/things/sdk/drivers/index.html) for more information on how to use the APIs.

## Behavior Changes

* * *

### Core application packages

Android Things doesn't include the standard suite of system apps and content providers. Avoid using [common intents](https://developer.android.google.cn/guide/components/intents-common.html) as well as the following content provider APIs in your apps:

[CalendarContract](https://developer.android.google.cn/reference/android/provider/CalendarContract.html)  
[ContactsContract](https://developer.android.google.cn/reference/android/provider/ContactsContract.html)  
[DocumentsContract](https://developer.android.google.cn/reference/android/provider/DocumentsContract.html)  
[DownloadManager](https://developer.android.google.cn/reference/android/app/DownloadManager.html)  
[MediaStore](https://developer.android.google.cn/reference/android/provider/MediaStore.html)  
[Settings](https://developer.android.google.cn/reference/android/provider/Settings.html)  
[Telephony](https://developer.android.google.cn/reference/android/provider/Telephony.html)  
[UserDictionary](https://developer.android.google.cn/reference/android/provider/UserDictionary.html)  
[VoicemailContract](https://developer.android.google.cn/reference/android/provider/VoicemailContract.html)  

### Displays are optional

Android Things supports graphical user interfaces using the same [UI toolkit](https://developer.android.google.cn/guide/topics/ui/index.html) available to traditional Android applications. In graphical mode, the application window occupies the full real estate of the display. Android Things does not include the system status bar or navigation buttons, giving applications full control over the visual user experience.

However, Android Things does not _require_ a display. On devices where a graphical display is not present, activities are still a primary component of your Android Things app. This is because the framework delivers all [input events](https://developer.android.google.cn/guide/topics/ui/ui-events.html) to the foreground activity, which has focus. Your app cannot receive key events or motion events through any other application component, such as a [service](https://developer.android.google.cn/guide/components/services.html).

### Home activity support

Android Things expects one application to expose a "home activity" in its manifest as the main entry point for the system to automatically launch on boot. This activity must contain an intent filter that includes both [CATEGORY_DEFAULT](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_DEFAULT) and `IOT_LAUNCHER`.

For ease of development, this same activity should include a [CATEGORY_LAUNCHER](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_LAUNCHER) intent filter so Android Studio can launch it as the default activity when deploying or debugging.

    <application    android:label="@string/app_name">    <activity android:name=".HomeActivity">        <!-- Launch activity as default from Android Studio -->        <intent-filter>            <action android:name="android.intent.action.MAIN"/>            <category android:name="android.intent.category.LAUNCHER"/>        </intent-filter>        <!-- Launch activity automatically on boot -->        <intent-filter>            <action android:name="android.intent.action.MAIN"/>            <category android:name="android.intent.category.IOT_LAUNCHER"/>            <category android:name="android.intent.category.DEFAULT"/>        </intent-filter>    </activity></application>

### Support for Google services

Android Things supports a subset of the [Google APIs for Android](https://developers.google.cn/android/). The following table breaks down API support in Android Things:

<table>

<tbody>

<tr>

<th>Supported APIs</th>

<th>Unavailable APIs</th>

</tr>

<tr>

<td style="width: 50%;">

[Awareness](https://developers.google.cn/awareness/)  

[Cast](https://developers.google.cn/cast/)  

[Google Analytics for Firebase](https://firebase.google.cn/docs/analytics/)  

[Firebase Authentication](https://firebase.google.cn/docs/auth/)<sup>1</sup>  

[Firebase Cloud Messaging (FCM)](https://firebase.google.cn/docs/cloud-messaging/)  

[Firebase Crash Reporting](https://firebase.google.cn/docs/crash/)  

[Firebase Realtime Database](https://firebase.google.cn/docs/database/)  

[Firebase Remote Config](https://firebase.google.cn/docs/remote-config/)  

[Firebase Storage](https://firebase.google.cn/docs/storage/)  

[Fit](https://developers.google.cn/fit/)  

[Instance ID](https://developers.google.cn/instance-id/)  

[Location](https://developers.google.cn/location-context/)  

[Maps](https://developers.google.cn/maps/)  

[Nearby](https://developers.google.cn/nearby/)  

[Places](https://developers.google.cn/places/)  

[Mobile Vision](https://developers.google.cn/vision/)  

[SafetyNet](https://developer.android.google.cn/training/safetynet/index.html)</td>

<td style="width: 50%;">

[AdMob](https://firebase.google.cn/docs/admob/)  

[Android Pay](https://developers.google.cn/android-pay/)  

[Drive](https://developers.google.cn/drive/)  

[Firebase App Indexing](https://firebase.google.cn/docs/app-indexing/)  

[Firebase Dynamic Links](https://firebase.google.cn/docs/dynamic-links/)  

[Firebase Invites](https://firebase.google.cn/docs/invites/)  

[Firebase Notifications](https://firebase.google.cn/docs/notifications/)  

[Play Games](https://developers.google.cn/games/services/)  

[Sign-In](https://developers.google.cn/identity/)</td>

</tr>

</tbody>

</table>

<sup id="fn1">1\. Does not include the open source FirebaseUI Auth component.</sup>

<aside class="note">**Note:** <span>As a general rule, APIs that require user input or authentication credentials aren't available to apps.</span></aside>

Each release of Android Things bundles the latest stable version of [Google Play Services](https://developer.android.google.cn/google/play-services/index.html), and requires at least version **11.0.0** of the client SDK. Android Things does not include the [Google Play Store](https://developer.android.google.cn/distribute/googleplay/index.html), which is responsible for automatically updating Play Services on the device. Because the Play Services version on the device is static, apps cannot target a client SDK greater than the version bundled with the target release.

<aside class="note">**Note:** <span>During developer preview, the bundled version for each release is listed in the [release notes](https://developer.android.google.cn/things/preview/releases.html).</span></aside>

#### Cloud IoT Core

![](https://developer.android.google.cn/things/images/32px_Cloud_IoT_Colored.png)

[Cloud IoT Core](http://cloud.google.com/iot-core) is a fully managed service that allows you to easily and securely connect, manage, and ingest data from millions of globally dispersed devices. Cloud IoT Core, in combination with other services on Google Cloud IoT platform, provides a complete solution for collecting, processing, analyzing, and visualizing IoT data in real time to support improved operational efficiency.

### Permissions

[Declare permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#permissions) that you need in your app's manifest file. [Normal permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) are granted at install time. Dangerous permissions are granted on the next device reboot and do not require [run time checks](https://developer.android.google.cn/training/permissions/requesting.html). This applies to new app installs and updated `<uses-permission>` elements in existing apps.

<aside class="note">**Note:** <span>During development, dangerous permissions are granted at install time when using Android Studio 3.0 and later.</span></aside>

### Notifications

Since there is no system-wide status bar and window shade in Android Things, [notifications](https://developer.android.google.cn/guide/topics/ui/notifiers/notifications.html) are not supported. Avoid calling the [NotificationManager](https://developer.android.google.cn/reference/android/app/NotificationManager.html) APIs in your apps.

