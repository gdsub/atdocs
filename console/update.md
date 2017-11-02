# Update Builds for an Android Things Product


## In this document

1.  [Push a build](#push-a-build)
2.  [View push history](#view-all-updates)

The **OTA UPDATES** tab in the [Android Things Console](https://partner.android.com/things/console) allows you to view and push over-the-air updates to devices.

## Push a build

* * *

To push a build for your product:

1.  If you are not already on this tab, open the [Android Things Console](https://partner.android.com/things/console), click a product for which you previously created a [device build](https://developer.android.google.cn/things/console/build.html), and click the **OTA UPDATES** tab.

2.  Click **START A NEW UPDATE**.

3.  In the **Bundles** table, select an existing bundle or click **UPLOAD** to upload a new [one](https://developer.android.google.cn/things/console/app_bundle.html). Note that empty bundles are not supported for OTA updates.

    ![Update a
    build](https://developer.android.google.cn/things/images/console/update_push.png)

4.  In the **Android Things versions** table, select an OS version.

    <aside class="note">**Note:** <span>You cannot select an OS version that is earlier than one selected for a previous update (for example, trying to select OS version 0.4.1 after pushing an update containing OS version 0.5).</span></aside>

5.  Click **PUSH UPDATE**. Verify that the information is correct.

6.  Click **PUSH**. The build will be pushed to all devices; it can take several hours for all devices to be updated. You can view the build status, including the number of devices that have been updated, in the **Current build** table.

    ![View current
    build](https://developer.android.google.cn/things/images/console/current_build_list.png)

### How build updates work

The following sequence describes the update process:

1.  After you push an update, the new version becomes ready for download.
2.  `update_engine` is the part of the operating system that looks for updates. It checks for new versions every 5 hours.
3.  The device downloads the update and installs it to one of the [A/B partitions](https://source.android.google.cn/devices/tech/ota/ab_updates).
4.  `update_engine` signals that the device is ready for a reboot.
5.  The device reboots to the new version.

    <aside class="note">**Note:** <span>Currently, you must trigger the reboot on the device. Run `adb shell` followed by `reboot`.</span></aside>

OTA updates for OEM apps are ignored if apps are sideloaded. When the device is rebooting, the operating system checks if a main apk (with `action=MAIN, category=IOT_LAUNCHER`) exists in the user data partition. If one exists, the device runs this apk. If one does not exist, the system checks for an apk in the OEM partition.

To make sure the device uses the updated apk, remove a sideloaded apk with `adb uninstall <var>package_name</var>` and reboot the device.

OS updates are not ignored.

## View push history

* * *

You can view all builds that have been pushed. These builds are no longer active.

![View push
history](https://developer.android.google.cn/things/images/console/update_build_list.png)

