# Enable binder for Linux
Most Linux distributions do not have Binder IPC enabled by default, so additional steps are required to use it.

If you are able to build the Linux kernel yourself, you can enable Binder IPC by adding the following kernel configuration options:
```
CONFIG_ASHMEM=y
CONFIG_ANDROID=y
CONFIG_ANDROID_BINDER_IPC=y
CONFIG_ANDROID_BINDERFS=y
```

Let's examine methods to activate Binder IPC in some representative Linux distributions.