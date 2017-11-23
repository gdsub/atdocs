# Create a Bundle

## 创建一个 Bundle

Bundles are saved to the OEM partition on the device.

Bundle 保存在设备的 OEM 分区中。

A bundle is a zip file that contains the following:

Bundle 是一个 zip 文件，里面包括：

*   `bootanimation.zip`—[Boot animation](https://source.android.google.cn/devices/tech/ota/device_code#boot-animation) located in the root directory
*   `<var><user-space driver></var>.apk`—User-space driver as a service (`action=BOOT_COMPLETED`)
*   `<var><main></var>.apk`— (Required) The apk for the main entry point (`action=MAIN, category=IOT_LAUNCHER`)
*   `<var><sub></var>.apk`—One of any number of apks that is launched by the main apk



- `bootanimation.zip` — 位于根目录的 [启动动画](https://source.android.google.cn/devices/tech/ota/device_code#boot-animation)
- `<var><user-space driver></var>.apk `— 作为服务 (`action=BOOT_COMPLETED`) 的用户级驱动
- `<var><main></var>.apk` — （必须包含）主入口 apk 文件 (`action=MAIN, category=IOT_LAUNCHER`)
- `<var><sub></var>.apk` — 由主 apk 启动的若干 apk 文件
