# Building a Cloud Doorbell

##创建一个云门铃

## Dependencies and Prerequisites

##依赖和预备知识

*   [Android Studio 3.0](https://developer.android.google.cn/studio/index.html) or later
*   [Android Studio 3.0](https://developer.android.google.cn/studio/index.html)  或更新版本
*   [Android Things developer board](https://developer.android.google.cn/things/hardware/developer-kits.html)
*   [Android Things 开发板](https://developer.android.google.cn/things/hardware/developer-kits.html)
*   Supported camera module
*   支持相机模块
*   [Peripheral Kit](https://developer.android.google.cn/things/hardware/developer-kits.html#featured_peripherals)
*   [外设套件](https://developer.android.google.cn/things/hardware/developer-kits.html#featured_peripherals)
*   Android device running Android 2.3 (API Level 9) or higher
*   Android 设备运行在 Android 2.3 （API Level 9）或更高

## You should also read

##你需要阅读如下资料

*   [Firebase Realtime Database](https://firebase.google.cn/docs/database/)
*   [Firebase 实时数据库](https://firebase.google.cn/docs/database/)
*   [Google Cloud Vision API](https://cloud.google.com/vision/)
*   [Google Cloud Vision API](https://cloud.google.com/vision/)
*   [Android Camera API](https://developer.android.google.cn/reference/android/hardware/camera2/package-summary.html)
*   [Android Camera API](https://developer.android.google.cn/reference/android/hardware/camera2/package-summary.html)

The Android Doorbell sample demonstrates how to create a “smart” doorbell. The sample captures a doorbell button press from a user, obtains an image of the user via a camera peripheral, processes the image data using Google’s Cloud Vision API, and uploads the image and event data to a cloud database where it can be viewed by a companion app.

Android 门铃例子演示了如果创建一个“智能”的门铃。在实例中，我们的设备会捕捉用户按下门铃按钮，获取一张用户通过相机外设获取的用户照片，使用 Google’s Cloud Vision API 处理图片数据，并且上传图片和事件数据到可以通过配套应用查看的云数据库中。

By walking through the steps to create this sample, you’ll learn:

通过创建这个例子，你将学习：

*   How to capture a button press event using Peripheral I/O.
*   如何使用外部 I/O 设备捕捉按钮的按压事件。
*   How to access a camera peripheral from an Android Things app using standard Android camera APIs.
*   如何在一个使用标准 Android camera APIs 的 Android Things 应用中使用相机外设。
*   How to use Google Cloud Vision from an Android Things app to do image analysis.
*   如何使用 Google Cloud Vision 在 Android Things应用中分析图片。
*   How to communicate between an Android Things app and a companion app using Firebase Realtime Database.
*   如何在一个 Android Things 应用和一个使用了 Firebase 实时数据库的配套应用中进行通讯。

