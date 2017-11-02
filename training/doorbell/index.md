# Building a Cloud Doorbell

## Dependencies and Prerequisites

*   [Android Studio 3.0](https://developer.android.google.cn/studio/index.html) or later
*   [Android Things developer board](https://developer.android.google.cn/things/hardware/developer-kits.html)
*   Supported camera module
*   [Peripheral Kit](https://developer.android.google.cn/things/hardware/developer-kits.html#featured_peripherals)
*   Android device running Android 2.3 (API Level 9) or higher

## You should also read

*   [Firebase Realtime Database](https://firebase.google.cn/docs/database/)
*   [Google Cloud Vision API](https://cloud.google.com/vision/)
*   [Android Camera API](https://developer.android.google.cn/reference/android/hardware/camera2/package-summary.html)

The Android Doorbell sample demonstrates how to create a “smart” doorbell. The sample captures a doorbell button press from a user, obtains an image of the user via a camera peripheral, processes the image data using Google’s Cloud Vision API, and uploads the image and event data to a cloud database where it can be viewed by a companion app.

By walking through the steps to create this sample, you’ll learn:

*   How to capture a button press event using Peripheral I/O.
*   How to access a camera peripheral from an Android Things app using standard Android camera APIs.
*   How to use Google Cloud Vision from an Android Things app to do image analysis.
*   How to communicate between an Android Things app and a companion app using Firebase Realtime Database.

