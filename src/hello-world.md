# Hello World!
This tutorial will guide you through creating a simple Binder service that echoes a string back to the client, and a client program that uses the service.

## Create a new Rust project
Create a new Rust project using Cargo:
```
$ cargo new --lib hello
```
Create a library project for the common library used by both the client and service:

## Modify Cargo.toml
In the hello project's Cargo.toml, add the following dependencies:

```
[package]
name = "hello"
version = "0.1.0"
publish = false
edition = "2021"

[dependencies]
rsbinder = "0.2.0"
lazy_static = "1"
async-trait = "0.1"
env_logger = "0.11"

[build-dependencies]
rsbinder-aidl = "0.2.0"

```
Add rsbinder, lazy_static, and async-trait to [dependencies], and add rsbinder-aidl to [build-dependencies].

## Create a AIDL File
Create an aidl folder in the project's top directory to manage AIDL files:
```
$ mkdir -p aidl/hello
$ touch IHello.aidl
```
The reason for creating an additional **hello** folder is to create a space for the **hello** package.

Create an IHello.aidl file with the following contents:
```
package hello;

// Defining the IHello Interface
interface IHello {
    // Defining the echo() Function.
    // The function takes a single parameter of type String and returns a value of type String.
    String echo(in String hello);
}
```
For more information on AIDL syntax, refer to the [Android AIDL documentation](https://source.android.com/docs/core/architecture/aidl).

## Create the build.rs
Create a build.rs file to compile the AIDL file and generate Rust code:
```
use std::path::PathBuf;

fn main() {
    rsbinder_aidl::Builder::new()
        .source(PathBuf::from("aidl/hello/IHello.aidl"))
        .output(PathBuf::from("hello.rs"))
        .generate().unwrap();
}
```
This uses **rsbinder-aidl** to specify the AIDL source file("**IHello.aidl**") and the generated Rust file name("**hello.rs**"), and then generates the code.

## Create a common library for Client and Service
For the Client and Service, create a library that includes the Rust code generated from AIDL.

Create src/lib.rs and add the following content.
```
// Include the code hello.rs generated from AIDL.
include!(concat!(env!("OUT_DIR"), "/hello.rs"));

// Set up to use the APIs provided in the code generated for Client and Service.
pub use crate::hello::IHello::*;

// Define the name of the service to be registered in the HUB(service manager).
pub const SERVICE_NAME: &str = "my.hello";
```

## Create a service
Let's configure the src/bin/hello_service.rs file as follows.
```
use env_logger::Env;
use rsbinder::*;

use hello::*;

// Define the name of the service to be registered in the HUB(service manager).
struct IHelloService;

// Implement the IHello interface for the IHelloService.
impl Interface for IHelloService {
    // Reimplement the dump method. This is optional.
    fn dump(&self, writer: &mut dyn std::io::Write, _args: &[String]) -> Result<()> {
        writeln!(writer, "Dump IHelloService")?;
        Ok(())
    }
}

// Implement the IHello interface for the IHelloService.
impl IHello for IHelloService {
    // Implement the echo method.
    fn echo(&self, echo: &str) -> rsbinder::status::Result<String> {
        Ok(echo.to_owned())
    }
}

fn main() -> std::result::Result<(), Box<dyn std::error::Error>> {
    env_logger::Builder::from_env(Env::default().default_filter_or("warn")).init();

    // Initialize ProcessState with the default binder path and the default max threads.
    ProcessState::init_default();

    // Start the thread pool.
    // This is optional. If you don't call this, only one thread will be created to handle the binder transactions.
    ProcessState::start_thread_pool();

    // Create a binder service.
    let service = BnHello::new_binder(IHelloService{});

    // Add the service to binder service manager.
    hub::add_service(SERVICE_NAME, service.as_binder())?;

    // Join the thread pool.
    // This is a blocking call. It will return when the thread pool is terminated.
    Ok(ProcessState::join_thread_pool()?)
}
```

## Create a client
Create the src/bin/hello_client.rs file and configure it as follows.
```
use env_logger::Env;
use rsbinder::*;
use hello::*;

fn main() -> std::result::Result<(), Box<dyn std::error::Error>> {
    env_logger::Builder::from_env(Env::default().default_filter_or("warn")).init();

    // Initialize ProcessState with the default binder path and the default max threads.
    ProcessState::init_default();

    // Create a Hello proxy from binder service manager.
    let hello: rsbinder::Strong<dyn IHello> = hub::get_interface(SERVICE_NAME)
        .expect(&format!("Can't find {SERVICE_NAME}"));

    // Call echo method of Hello proxy.
    println!("Result: {}", hello.echo("Hello World!")?);

    Ok(())
}
```

## Project folder and file structure
```
.
├── Cargo.toml
├── aidl
│   └── hello
│       └── IHello.aidl
├── build.rs
└── src
    ├── bin
    │   ├── hello_client.rs
    │   └── hello_service.rs
    └── lib.rs
```