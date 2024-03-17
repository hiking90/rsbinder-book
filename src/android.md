**rsbinder** supports not only Linux but also Android. Since Android already has an environment prepared for binder communication, **rsbinder**, **rsbinder-aidl**, and **rsbinder-hub** are utilized for development. There is no need to create a binder device or run a service manager, so **rsbinder-tools** are not used.

For building in the Android environment, it is necessary to install the NDK, and an additional Rust build environment that utilizes the NDK is required.

[Android Build](./android-build.md)

## Compatibility with Android Versions
안드로이드 버전에 따른 Binder IPC에 대한 호환성 문제가 있다.
현재 확인된 사항은 Android 12를 기준으로 그 이전, 이후의 호환성 문제가 있다.

당신의 SW가 Android 11과 12에서 모두 동작해야 한다면, Android 버전을 rsbinder::set_android_version() api를 통해 설정해야 한다.

Android Version 정보는 다음과 같이 확인하고, 설정할 수 있다.

```
use android_system_properties::AndroidSystemProperties;

let properties = AndroidSystemProperties::new();

if let Some(version) = properties.get("ro.build.version.release") {
    rsbinder::set_android_version(version.parse())
}
```
