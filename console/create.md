# Create an Android Things Product

# 创建一个 Android Things 产品

## In this document

## 本文包括

1.  [Before you begin](#before_you_begin)
2.  [Create a new product](#create_an_android_things_product)
3.  [Delete an existing product](#delete_an_android_things_product)
4.  [What's next](#whats-next)


1.  [准备工作](#准备工作)
2.  [创建一个新产品](#创建一个新产品)
3.  [删除一个已有产品](#删除一个已有产品)
4.  [下一步](#下一步)

## Before you begin

## 准备工作

* * *

Gather the following information:

请准备好以下内容：

*   **Google account**—A Google account to associate with the product
*   **Google 账号** — 准备一个 Google 账号用于关联产品
*   **Product name**—The internal name you want to use to refer to your product. We recommend that this name be different from the final marketing name. Developers can see this name in the console, but consumers don't see it. Maximum characters: 70
*   **产品名称** — 一个您希望内部使用的产品名称。我们推荐将内部产品名称和最终发售时的产品名称区分开来。产品名称在管理中心中对开发者可见，但对用户不可见。字数上限：70 个字符
*   **System on Module (SOM) type**—[Hardware platform](https://developer.android.google.cn/things/hardware/developer-kits.html) on which you are building your Android Things product
*   **模块化系统 (SOM) 类型** — 选择在哪一个[硬件平台](https://developer.android.google.cn/things/hardware/developer-kits.html)之上开发 Android Things 产品
*   **Product description**—A brief paragraph that provides more detail to identify the product and its function. Maximum characters: 100
*   **产品描述** — 一小段对产品和其功能的描述。字数上限：100 个字符

## Create a new product

## 创建一个新产品

* * *

To create your product:

创建一个产品的步骤如下：

1.  Open the [Android Things Console](https://partner.android.com/things/console).

2.  If the **Welcome** page appears, sign in, agree to the terms of service, and click **Continue**.

    Sign in with the Google account that you want to associate with your Android Things product.

    The Android Things Product page appears. It lists the Android Things products associated with this Google account, if any. For new users, the Android Things Product page is empty.

3.  Click **CREATE A PRODUCT**.

    ![Create new product](https://developer.android.google.cn/things/images/console/create_new_product.png)

    The **Create new product** dialog appears.

4.  Type the product name for your product.

    This is an internal name that users won't see. Collaborating developers can use this name before deciding on the actual product name. You can change the name at any time after project creation.

5.  Select your hardware platform from the **SOM type** list.

6.  (Optional) Select the checkbox to include Google Play Services.

    Google Play Services is the set of Google-specific services and APIs that multiple Android apps may depend on, but that are not part of the open-source Android platform.

7.  Enter the required OEM partition size.

    <aside class="note">**Note:** <span>You cannot change the SOM type, Google Play Services inclusion, or OEM partition size for a product after you create it.</span></aside>

8.  (Optional) Type a description of your product.

    You can change this later, if needed.

9.  Click **CREATE**.



1. 打开 [Android Things 管理中心](https://partner.android.com/things/console)。

2. 如果显示**欢迎**页面，请先登录，并同意服务条款，然后点击 **Continue** 按钮。

    请选择希望与您 Android Things 产品相关联的 Google 账号进行登录。

    Android Things 产品页显示后，里面会列出所有与您 Google 账户相关联的 Android Things 产品。对于新用户来说，Android Things 产品页是空的。

3. 点击 **CREATE A PRODUCT**。

    会弹出 **Create new product** 对话框。

4. 输入产品名称。

    这是一个内部产品代号，对用户不可见。相互协作的开发者们可以在正式产品名称确定之前使用该名称。项目创建之后，您仍可以在任何时间修改产品名称。

5. 从 **SOM type** 列表中选择硬件平台。

6. （可选项）选中对话框以包含 Google Play 服务。

    Google Play 服务是一套由 Google 提供的服务和 API，各种 Android 应用都依赖于它，但 Google Play 服务并不在 Android 开放源代码平台之中。

7. 输入所需的 OEM 分区大小。

    <aside class="note">**注意：** <span>在您的产品创建完成之后，您不可以再修改 SOM 类型、是否包含 Google Play 服务，以及 OEM 分区大小。</span></aside>

8. （可选项）输入您的产品描述

    如果需要，您可以之后再做更改。

9. 点击 **CREATE**。

## Delete an existing product

## 删除一个已有产品

* * *

If you delete a product, the product information is deleted. Deletions cannot be reverted.

如果您删除了一个产品，产品信息也随之被删除。删除操作不可恢复。

After deletion, devices flashed from that product will no longer receive [updates](https://developer.android.google.cn/things/console/update.html).

执行了删除操作之后，写入该产品的设备不会再收到[更新提醒](https://developer.android.google.cn/things/console/update.html)。

To delete a product:

删除操作步骤如下：

1.  Open the [Android Things Console](https://partner.android.com/things/console).

2.  Find the product in the list. Do either of the following:

    *   Point to the product name with the mouse. Click the trash can icon that appears.
    *   Select the product. Click the trash can icon near the upper right of the screen.
3.  In the **Delete Product** dialog, click **Delete**.



1. 打开 [Android Things 管理中心](https://partner.android.com/things/console)。
2. 在产品列表中找到要删除的产品项。以下两种操作任选其一：

    *   将鼠标移动到产品名称上。点击出现的垃圾桶图标。
    *   点击产品名称进入详情。点击屏幕上部出现的垃圾桶图标。
3. 在**产品删除**对话框中，点击 **Delete**。

## What's next

## 下一步

* * *

The next step is to [configure the product settings](https://developer.android.google.cn/things/console/configure.html).

下一步是[产品配置](https://developer.android.google.cn/things/console/configure.html)。

