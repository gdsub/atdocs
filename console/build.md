# Control Builds for an Android Things Product

## 管理 Andorid Things 产品的构建文件

## In this document

## 本文包括

1.  [Create a bundle](#create_a_bundle)
2.  [Create a build](#create_a_build)
3.  [Flash the device](#flash_device)
4.  [Delete a bundle or build](#delete_a_bundle_or_build)
5.  [What's next](#whats-next)


1.  [创建一个 Bundle](#创建一个 Bundle)
2.  [创建一个构建文件](#创建一个构建文件)
3.  [向设备烧写镜像文件](#向设备烧写镜像文件)
4.  [删除一个 Bundle 或构建文件](#删除一个 Bundle 或构建文件)
5.  [下一步](#下一步)

The **FACTORY IMAGES** tab in the [Android Things Console](https://partner.android.com/things/console) allows you to create application bundles for device builds. You can download these builds to flash on your device. Use this tab to see the build history for a product.

在 [Android Things 管理中心](https://partner.android.com/things/console)的 **FACTORY IMAGES** 标签中，您可以在设备的构建镜像里创建应用 Bundle。您可以下载这些构建好的镜像，烧写到设备中。您还可以在本标签页中看到产品的构建历史记录。

## Create a bundle

## 创建一个 Bundle

* * *

To create a bundle for your product:

为您的产品创建一个 Bundle 的步骤如下：

1.  If you are not already on this tab, open the [Android Things Console](https://partner.android.com/things/console), click a product you previously [configured](https://developer.android.google.cn/things/console/configure.html), and click the **FACTORY IMAGES** tab.

    ![Create a build bundle](https://developer.android.google.cn/things/images/console/build.png)

2.  Click **UPLOAD** to upload your [bundle](https://developer.android.google.cn/things/console/app_bundle.html). After you upload the bundle, click the button next to its bundle ID in the **Bundles** table to select it.

    If you don't have a bundle, select **Empty bundle** in the table. If you select an empty bundle, you can test your application by sideloading it using the [adb tool](https://developer.android.google.cn/tools/help/adb.html).

3.  In the **Android Things versions** table, select an OS version.



1. 如果您还没有处在本标签，请先打开 [Android Things 管理中心](https://partner.android.com/things/console)，点击您之前所 [配置](https://developer.android.google.cn/things/console/configure.html)的产品，然后点击 **FACTORY IMAGES** 标签。

2. 点击 **UPLOAD** 上传您的 [Bundle](https://developer.android.google.cn/things/console/app_bundle.html)。上传完毕之后，请在 **Bundles** 表格中，点击 bundle ID 一旁的按钮以选中该 Bundle。

    如果您还没有上传过 Bundle，可选择表格中的 **Empty bundle**。如果您选择了空 Bundle，您可以使用 [adb 工具](https://developer.android.google.cn/tools/help/adb.html)通过线刷的方式测试您的应用。

3. 在 **Android Things versions** 表格中，选择一个操作系统版本。


## Create a build

## 创建一个构建文件

* * *

Once you have selected a bundle and OS version, click **CREATE BUILD CONFIGURATION** to create a build.

当您选择好了 Bundle 和操作系统版本之后，可点击 **CREATE BUILD CONFIGURATION** 创建一个构建文件。

When the build is complete, a new entry will appear in the **Build configuration list** table. Each row in the table contains a **Download build** link.

当构建完成，一个新的文件项会出现在 **Build configuration list** 表格中。表格中的每一行都有一个 **Download build** 下载链接。

![Build list](https://developer.android.google.cn/things/images/console/build_list.png)

## Flash the device

## 向设备烧写镜像文件

* * *

Click the **Download build** link to download the factory image (i.e., the created build).

点击 **Download build** 链接即可下载出厂镜像（也就是刚刚创建的构建文件）。

Find your device in the table of supported [hardware platforms](https://developer.android.google.cn/things/hardware/developer-kits.html). Then click the **Get Started** link to find instructions on how to flash your device with the downloaded image.

请在我们所支持的 [硬件平台](https://developer.android.google.cn/things/hardware/developer-kits.html) 中找到您的设备型号。点击对应的 **Get Started** 链接，根据指示说明将下载好的镜像烧写到设备中。

## Delete a bundle or build

## 删除一个 Bundle 或构建文件

* * *

To permanently delete a bundle or a build for your product:

永久删除产品下的一个 Bundle 或者构建文件，步骤如下：

1.  Select a bundle from the **Bundles** table or a build from the **Build configuration list** table.
2.  Click the trash can icon.



1. 在 **Bundles** 表格中选中一个 Bundle，或在 **Build configuration list** 表格中选中一个构建文件。
2. 点击垃圾桶图标。

## What's next

## 下一步

* * *

After you [flash a device](#flash_device), you can update the build with OTA [updates](https://developer.android.google.cn/things/console/update.html).

在 [为设备烧写镜像](#flash_device) 之后，您就可以使用无线 (OTA) 的方式进行 [更新](https://developer.android.google.cn/things/console/update.html)了。

<aside class="note"> **Note:** <span>You must flash the device with an image from the Android Things Console in order for the device to receive updates.</span></aside>

<aside class="note"> **注意：** <span>为了使设备能够收到更新，您为设备烧写的镜像一定要下载自 Android Things 管理中心。</span></aside>

