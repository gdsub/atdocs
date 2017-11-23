# Overview


Android Things makes developing connected embedded devices easy by providing the same Android development tools, best-in-class Android framework, and Google APIs that make developers successful on mobile.

Android Things 通过提供相同的 Android 开发工具，一流的 Android 框架以及 Google 接口使得开发者毫不费力地开发嵌入式设备，在移动平台上获得成功。

![""](https://developer.android.google.cn/things/images/platform-architecture.png)

Apps for embedded devices bring developers closer to hardware peripherals and drivers than phones and tablets. In addition, embedded devices typically present a single app experience to users. This document goes over the major additions, omissions, and differences between core Android development and Android Things.

嵌入式设备的应用程序令开发者较手机及平板电脑更加贴近硬件外围设备和驱动程序。此外，嵌入式设备通常向用户提供单一应用程序体验。本文档详细介绍了核心 Android 开发及 Android Things 之间的重大更新，遗漏和不同之处。

Android Things extends the core Android framework with additional APIs provided by the Things Support Library. These APIs allow apps to integrate with new types of hardware not found on mobile devices.

Android Things 由 Things Support Library 提供的新增接口以扩展了核心 Android 框架。这些接口允许应用程序与不是以移动设备为基础的的新型硬件结合在一起。

The Android Things platform is also streamlined for single application use. System apps are not present, and your app is launched automatically on startup to immerse your users in the app experience.

Android Things 平台也被简化为单一应用程序使用，不再提供系统应用程序，并且你的应用程序在系统启动时自动启动，令用户沉浸在应用程序的体验当中。

## Things Support Library

## Things 支持库

* * *

### Peripheral I/O API

### 外设 I/O 接口

The Peripheral I/O APIs let your apps communicate with sensors and actuators using industry standard protocols and interfaces. The following interfaces are supported: GPIO, PWM, I2C, SPI, UART.

外设 I/O 接口让你的应用程序使用工业标准协议和接口与传感器和执行器通讯。支持下列接口：GPIO, PWM, I2C, SPI, UART。

See the [Peripheral I/O API Guides](https://developer.android.google.cn/things/sdk/pio/index.html) for more information on how to use the APIs.

关于如何使用接口的更多信息请看 [外设I/O接口指南](https://developer.android.google.cn/things/sdk/pio/index.html)

### User Driver API

### 用户驱动程序接口

User drivers extend existing Android framework services and allow apps to inject hardware events into the framework that other apps can access using the standard Android APIs.

用户驱动程序扩展了现存的 Android 框架服务，并且允许应用程序将硬件事件注入到其他应用程序可以使用标准 Android 接口访问的框架中。

See the [User Driver API Guides](https://developer.android.google.cn/things/sdk/drivers/index.html) for more information on how to use the APIs.

关于如何使用接口的更多信息请看 [用户驱动程序接口指南](https://developer.android.google.cn/things/sdk/drivers/index.html)

## Behavior Changes

## 行为改变

* * *

### Core application packages

### 核心应用程序包

Android Things doesn't include the standard suite of system apps and content providers. Avoid using [common intents](https://developer.android.google.cn/guide/components/intents-common.html) as well as the following content provider APIs in your apps:

Android Things 不包含标准的系统应用程序套件和内容提供商。避免在你的应用程序中使用 [常用Intent](https://developer.android.google.cn/guide/components/intents-common.html) 以及下列内容提供商的接口：

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

### 显示是可选择的

Android Things supports graphical user interfaces using the same [UI toolkit](https://developer.android.google.cn/guide/topics/ui/index.html) available to traditional Android applications. In graphical mode, the application window occupies the full real estate of the display. Android Things does not include the system status bar or navigation buttons, giving applications full control over the visual user experience.

Android Things 支持同于传统Android应用程序提供的 [UI工具包](https://developer.android.google.cn/guide/topics/ui/index.html) 图形用户界面。在图形模式下，应用程序窗口的显示占据全部屏幕。 Android Things 不包含系统状态栏或导航按钮，给予应用程序完全控制视觉的用户体验。

However, Android Things does not _require_ a display. On devices where a graphical display is not present, activities are still a primary component of your Android Things app. This is because the framework delivers all [input events](https://developer.android.google.cn/guide/topics/ui/ui-events.html) to the foreground activity, which has focus. Your app cannot receive key events or motion events through any other application component, such as a [service](https://developer.android.google.cn/guide/components/services.html).

然而， Android Things 不需要显示设备 ，在没有图形显示的设备上，Activity 依然是Android Things应用程序的主要组件。这是因为框架将所有的 [输入事件](https://developer.android.google.cn/guide/topics/ui/ui-events.html) 传递给获取焦点的前台Activity。你的应用程序无法通过诸如  [service](https://developer.android.google.cn/guide/components/services.html) 等其他任何应用组件接收关键事件或动作事件。

### Home activity support

### Home activity 支持

Android Things expects one application to expose a "home activity" in its manifest as the main entry point for the system to automatically launch on boot. This activity must contain an intent filter that includes both [CATEGORY_DEFAULT](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_DEFAULT) and `IOT_LAUNCHER`.

Android Things 要求一个应用程序在其 manifest 文件中公开一个 “home activity” 作为系统自启动引导时的主要入口。这个 Activity 必须包含一个同时包括 [CATEGORY_DEFAULT](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_DEFAULT) 和 `IOT_LAUNCHER` 的 Intent 过滤器。

For ease of development, this same activity should include a [CATEGORY_LAUNCHER](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_LAUNCHER) intent filter so Android Studio can launch it as the default activity when deploying or debugging.

为了便于开发，这一 Activity 应包含一个 [CATEGORY_LAUNCHER](https://developer.android.google.cn/reference/android/content/Intent.html#CATEGORY_LAUNCHER) Intent 过滤器， Android Studio 在部署和调试的时候可以将其作为默认 Activity 启动。

~~~Java
    <application    
        android:label="@string/app_name">    
        <activity android:name=".HomeActivity">        
            <!-- Launch activity as default from Android Studio -->        
            <intent-filter>            
                <action android:name="android.intent.action.MAIN"/>            
                <category android:name="android.intent.category.LAUNCHER"/>       
            </intent-filter>        
            <!-- Launch activity automatically on boot -->        
            <intent-filter>            
                <action android:name="android.intent.action.MAIN"/>            
                <category android:name="android.intent.category.IOT_LAUNCHER"/>            
                <category android:name="android.intent.category.DEFAULT"/>        
             </intent-filter>    
        </activity>
    </application>
~~~

### Support for Google services

### 支持 Google Services

Android Things supports a subset of the [Google APIs for Android](https://developers.google.cn/android/). The following table breaks down API support in Android Things:

Android Things 支持 [Google APIs for Android](https://developers.google.cn/android/) 的一个子集。下表分解了 Android Things 中的接口支持：

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


<table>

<tbody>

<tr>

<th>支持的接口</th>

<th>不可用的接口</th>

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

<sup id="fn1">1\. 不包括开源 FirebaseUI 认证组件。</sup>

<aside class="note">**Note:** <span>As a general rule, APIs that require user input or authentication credentials aren't available to apps.</span></aside><br />
<br />

**注意:** 一般来说，需要用户输入或身份验证凭证的接口不适用于应用程序。
 
Each release of Android Things bundles the latest stable version of [Google Play Services](https://developer.android.google.cn/google/play-services/index.html), and requires at least version **11.0.0** of the client SDK. Android Things does not include the [Google Play Store](https://developer.android.google.cn/distribute/googleplay/index.html), which is responsible for automatically updating Play Services on the device. Because the Play Services version on the device is static, apps cannot target a client SDK greater 
than the version bundled with the target release.

每一批发布 Android Things 的 [Google Play Services](https://developer.android.google.cn/google/play-services/index.html) 最新稳定版，要求客户端SDK版本至少是 **11.0.0** 。 Android Things 不包括 [Google Play Store](https://developer.android.google.cn/distribute/googleplay/index.html) ，其负责自动更新设备上 的Google Play Services。因为设备上的 Google Play Services 版本是静态的，所以应用程序不能标记客户端SDK版本大于目标发布的捆绑版本。

 **Note:** During developer preview, the bundled version for each release is listed in the [release notes](https://developer.android.google.cn/things/preview/releases.html).
    
 **注意:** 在开发者预览版中, 每个版本的捆绑版本都列在了 [发行说明](https://developer.android.google.cn/things/preview/releases.html) 中。 


#### Cloud IoT Core

#### 云物联网核心

![](https://developer.android.google.cn/things/images/32px_Cloud_IoT_Colored.png)

[Cloud IoT Core](http://cloud.google.com/iot-core) is a fully managed service that allows you to easily and securely connect, manage, and ingest data from millions of globally dispersed devices. Cloud IoT Core, in combination with other services on Google Cloud IoT platform, provides a complete solution for collecting, processing, analyzing, and visualizing IoT data in real time to support improved operational efficiency.

[云物联网核心](http://cloud.google.com/iot-core) 是一个完整的可以让你轻松，安全地链接，管理，并从全球数以百万计的分布式设备中获取数据的托管服务。云物联网核心，与 Google 云物联网平台上的其他服务结合，提供了一个以支持提高应用效率的用于实时收集，处理，分析和可视化物联网数据的解决方案。

### Permissions

### 权限

[Declare permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#permissions) that you need in your app's manifest file. [Normal permissions](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) are granted at install time. Dangerous permissions are granted on the next device reboot and do not require [run time checks](https://developer.android.google.cn/training/permissions/requesting.html). This applies to new app installs and updated `<uses-permission>` elements in existing apps.

你需要在你的应用程序中的 manifest 文件中 [声明权限](https://developer.android.google.cn/guide/topics/permissions/requesting.html#permissions) 在安装时授予 [正常权限](https://developer.android.google.cn/guide/topics/permissions/requesting.html#normal-dangerous) 在下一个设备重启时授予危险权限，不需要 [运行时校验](https://developer.android.google.cn/training/permissions/requesting.html) 这适用于新应用程序安装和更新现有应用程序的 `<uses-permission>` 元素。

<aside class="note">**Note:**<span>During development, dangerous permissions are granted at install time when using Android Studio 3.0 and later.</span></aside><br />
<br />

**注意:** 用 Android Studio 3.0 及后续版本开发应用时，危险权限只能在安装时授予。

### Notifications

### 通知

Since there is no system-wide status bar and window shade in Android Things, [notifications](https://developer.android.google.cn/guide/topics/ui/notifiers/notifications.html) are not supported. Avoid calling the [NotificationManager](https://developer.android.google.cn/reference/android/app/NotificationManager.html) APIs in your apps.

由于在 Android Things 中没有系统的状态栏和窗口阴影，[通知](https://developer.android.google.cn/guide/topics/ui/notifiers/notifications.html) 不被支持，在你的应用程序中不要调用 [NotificationManager](https://developer.android.google.cn/reference/android/app/NotificationManager.html) 接口。
