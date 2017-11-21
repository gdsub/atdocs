# Update Builds for an Android Things Product

## Android Things 产品镜像的升级

## In this document

## 本文包括

1.  [Push a build](#push-a-build)
2.  [View push history](#view-all-updates)


1. [推送一个构建文件](#推送一个构建文件)
2. [查看推送历史记录](#查看推送历史记录)

The **OTA UPDATES** tab in the [Android Things Console](https://partner.android.com/things/console) allows you to view and push over-the-air updates to devices.

在 [Android Things 管理中心](https://partner.android.com/things/console)的 **OTA UPDATES** 标签页中，您可以为设备推送无线更新，并查看历史记录。

## Push a build

## 推送一个构建文件

* * *

To push a build for your product:

为您的产品推送构建文件，步骤如下：

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



1. 如果您还没有处在本标签，请先打开 [Android Things 管理中心](https://partner.android.com/things/console)，点击您之前已准备好[构建文件](https://developer.android.google.cn/things/console/build.html)的产品项，然后点击 **OTA UPDATES** 标签。

2. 请点击 **START A NEW UPDATE**。

3. 在 **Bundles** 表格中，选中一个已有的 Bundle 或者点击 **UPLOAD** 上传一个[新的 Bundle](https://developer.android.google.cn/things/console/app_bundle.html)。请注意，空 Bundle 不支持无线更新。

   ![Update a
   build](https://developer.android.google.cn/things/images/console/update_push.png)

4. 在 **Android Things versions** 表格中，选择操作系统版本。

   <aside class="note">**注意：** <span>您所选择的操作系统版本不能低于您之前进行推送时的系统版本（比如，在此之前您选择过为版本号为 0.5 的操作系统进行了推送，这次更新试图选择版本号为 0.4.1的操作系统）。</span></aside>

5. 点击 **PUSH UPDATE**，并确认信息无误。

6. 点击 **PUSH**。该构建文件会被推送到所有的设备上，更新所有设备一般需要几个小时的时间。您可以在 **Current build** 表格查看构建文件的详情，包括已更新的设备数量等。

   ![View current
   build](https://developer.android.google.cn/things/images/console/current_build_list.png)

### How build updates work

###构建文件更新流程

The following sequence describes the update process:

更新过程大致如下：

1.  After you push an update, the new version becomes ready for download.

2.  `update_engine` is the part of the operating system that looks for updates. It checks for new versions every 5 hours.

3.  The device downloads the update and installs it to one of the [A/B partitions](https://source.android.google.cn/devices/tech/ota/ab_updates).

4.  `update_engine` signals that the device is ready for a reboot.

5.  The device reboots to the new version.

    <aside class="note">**Note:** <span>Currently, you must trigger the reboot on the device. Run `adb shell` followed by `reboot`.</span></aside>



1. 当您推送了一个更新，新版本就会变为待下载状态。

2. `update_engine` 是操作系统的一部分，用于检查更新。它每隔5个小时检查一次新版本。

3. 设备下载该更新并安装到 [A/B 分区](https://source.android.google.cn/devices/tech/ota/ab_updates)之一。

4. `update_engine` 发出信号，表示设备可以重新启动。

5. 设备重新启动，进入新版本的镜像。

   <aside class="note">**注意：** <span>现阶段，您需要手动重新启动设备，执行 `adb shell` 命令，紧接着执行 `reboot` 命令。</span></aside>

OTA updates for OEM apps are ignored if apps are sideloaded. When the device is rebooting, the operating system checks if a main apk (with `action=MAIN, category=IOT_LAUNCHER`) exists in the user data partition. If one exists, the device runs this apk. If one does not exist, the system checks for an apk in the OEM partition.

如果 OEM 应用通过线刷刷入，无线更新时会被忽略掉。当设备更新重启时，操作系统会在数据分区检查是否有主 apk （带有 `action=MAIN, category=IOT_LAUNCHER` 的apk）存在。如果存在，设备会运行该 apk。如果不存在，操作系统会在 OEM 分区检查 apk 文件。

To make sure the device uses the updated apk, remove a sideloaded apk with `adb uninstall <var>package_name</var>` and reboot the device.

为了确保设备使用的是更新过的 apk，请将线刷的 apk 通过 `adb uninstall <var>package_name</var>` 命令移除，并重启设备。

OS updates are not ignored.

操作系统的更新不会被忽略掉。

## View push history

## 查看推送历史记录

* * *

You can view all builds that have been pushed. These builds are no longer active.

您可以查看所有推送过的构建文件。但这些文件不再有效。

![View push
history](https://developer.android.google.cn/things/images/console/update_build_list.png)

