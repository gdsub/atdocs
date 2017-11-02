# Control Builds for an Android Things Product


## In this document

1.  [Create a bundle](#create_a_bundle)
2.  [Create a build](#create_a_build)
3.  [Flash the device](#flash_device)
4.  [Delete a bundle or build](#delete_a_bundle_or_build)
5.  [What's next](#whats-next)


The **FACTORY IMAGES** tab in the [Android Things Console](https://partner.android.com/things/console) allows you to create application bundles for device builds. You can download these builds to flash on your device. Use this tab to see the build history for a product.

## Create a bundle

* * *

To create a bundle for your product:

1.  If you are not already on this tab, open the [Android Things Console](https://partner.android.com/things/console), click a product you previously [configured](https://developer.android.google.cn/things/console/configure.html), and click the **FACTORY IMAGES** tab.

    ![Create a build bundle](https://developer.android.google.cn/things/images/console/build.png)

2.  Click **UPLOAD** to upload your [bundle](https://developer.android.google.cn/things/console/app_bundle.html). After you upload the bundle, click the button next to its bundle ID in the **Bundles** table to select it.

    If you don't have a bundle, select **Empty bundle** in the table. If you select an empty bundle, you can test your application by sideloading it using the [adb tool](https://developer.android.google.cn/tools/help/adb.html).

3.  In the **Android Things versions** table, select an OS version.

## Create a build

* * *

Once you have selected a bundle and OS version, click **CREATE BUILD CONFIGURATION** to create a build.

When the build is complete, a new entry will appear in the **Build configuration list** table. Each row in the table contains a **Download build** link.

![Build list](https://developer.android.google.cn/things/images/console/build_list.png)

## Flash the device

* * *

Click the **Download build** link to download the factory image (i.e., the created build).

Find your device in the table of supported [hardware platforms](https://developer.android.google.cn/things/hardware/developer-kits.html). Then click the **Get Started** link to find instructions on how to flash your device with the downloaded image.

## Delete a bundle or build

* * *

To permanently delete a bundle or a build for your product:

1.  Select a bundle from the **Bundles** table or a build from the **Build configuration list** table.

2.  Click the trash can icon.

## What's next

* * *

After you [flash a device](#flash_device), you can update the build with OTA [updates](https://developer.android.google.cn/things/console/update.html).

<aside class="note">**Note:** <span>You must flash the device with an image from the Android Things Console in order for the device to receive updates.</span></aside>

