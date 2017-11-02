# Create an Android Things Product


## In this document

1.  [Before you begin](#before_you_begin)
2.  [Create a new product](#create_an_android_things_product)
3.  [Delete an existing product](#delete_an_android_things_product)
4.  [What's next](#whats-next)


## Before you begin

* * *

Gather the following information:

*   **Google account**—A Google account to associate with the product
*   **Product name**—The internal name you want to use to refer to your product. We recommend that this name be different from the final marketing name. Developers can see this name in the console, but consumers don't see it. Maximum characters: 70
*   **System on Module (SOM) type**—[Hardware platform](https://developer.android.google.cn/things/hardware/developer-kits.html) on which you are building your Android Things product
*   **Product description**—A brief paragraph that provides more detail to identify the product and its function. Maximum characters: 100

## Create a new product

* * *

To create your product:

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

## Delete an existing product

* * *

If you delete a product, the product information is deleted. Deletions cannot be reverted.

After deletion, devices flashed from that product will no longer receive [updates](https://developer.android.google.cn/things/console/update.html).

To delete a product:

1.  Open the [Android Things Console](https://partner.android.com/things/console).

2.  Find the product in the list. Do either of the following:

    *   Point to the product name with the mouse. Click the trash can icon that appears.
    *   Select the product. Click the trash can icon near the upper right of the screen.
3.  In the **Delete Product** dialog, click **Delete**.

## What's next

* * *

The next step is to [configure the product settings](https://developer.android.google.cn/things/console/configure.html).

