# Overview
Welcome to **rsbinder**!

**rsbinder** is a Rust library and toolset that enables you to utilize Binder IPC on Linux and Android OS.

Binder IPC is an object-oriented IPC (Inter-Process Communication) mechanism that Google added to the Linux kernel for Android. Android uses Binder IPC for all process communication, and since 2015, it has been integrated into the Linux kernel, making it available on all Linux systems.

However, since it is rarely used outside of Android, it is disabled by default in most Linux distributions.

### Key Features of Binder IPC:

- Object-oriented: Binder IPC provides a clean and intuitive object-oriented API for inter-process communication.
- Efficient: Binder IPC is designed for high performance and low overhead.
- Secure: Binder IPC provides strong security features to prevent unauthorized access and tampering.
- Versatile: Binder IPC can be used for a variety of purposes, including remote procedure calls, data sharing, and event notification.

Before using Binder IPC, you must enable the feature in the kernel. Please refer to [Enable binder for Linux](./ch02-00-enable-binder-for-linux.md) for detailed instructions on setting it up.
