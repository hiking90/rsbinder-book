# Android Build
## NDK Install
The NDK can be installed using Android Studio, or it can be downloaded and installed from the following URL.

* [NDK guides](https://developer.android.com/ndk/guides)
* [NDK downloads](https://developer.android.com/ndk/downloads)

## Cargo NDK Install
To build rsbinder using the NDK, the installation of [cargo-ndk](https://crates.io/crates/cargo-ndk) is required.

## Build **rsbinder** with NDK
After installing cargo-ndk, various targets were installed with ```$ rustup target add```. Use this target information to build with the following command.

```
$ cargo ndk -t x86_64-linux-android build
```

