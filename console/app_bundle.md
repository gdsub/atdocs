# Create a Bundle

Bundles are saved to the OEM partition on the device.

A bundle is a zip file that contains the following:

*   `bootanimation.zip`—[Boot animation](https://source.android.google.cn/devices/tech/ota/device_code#boot-animation) located in the root directory
*   `<var><user-space driver></var>.apk`—User-space driver as a service (`action=BOOT_COMPLETED`)
*   `<var><main></var>.apk`— (Required) The apk for the main entry point (`action=MAIN, category=IOT_LAUNCHER`)
*   `<var><sub></var>.apk`—One of any number of apks that is launched by the main apk
