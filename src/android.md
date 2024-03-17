**rsbinder** supports not only Linux but also Android. Since Android already has an environment prepared for binder communication, **rsbinder**, **rsbinder-aidl**, and **rsbinder-hub** are utilized for development. There is no need to create a binder device or run a service manager, so **rsbinder-tools** are not used.

For building in the Android environment, it is necessary to install the NDK, and an additional Rust build environment that utilizes the NDK is required.

[Android Build](./android-build.md)

## Compatibility with Android Versions
There are compatibility issues with Binder IPC depending on the Android version. The current findings indicate there are compatibility issues before and after Android 12.

If your software needs to work on both Android 11 and 12, you must set the Android version using the rsbinder::set_android_version() API.

The Android Version information can be checked and set as follows.

```
use android_system_properties::AndroidSystemProperties;

let properties = AndroidSystemProperties::new();

if let Some(version) = properties.get("ro.build.version.release") {
    rsbinder::set_android_version(version.parse())
}
```
