## Archived Repository

**This repository was a gift from me to the Rust and open-source community. It is no longer actively maintained and has been archived. Feel free to fork and continue development on your own.**

crabi_test2
crabi_test2 is an experimental Rust library designed to facilitate advanced exploration and understanding of Application Binary Interface (ABI) concepts, particularly as they relate to cross-language interoperability and Rust’s Foreign Function Interface (FFI).

Key Features
Data Structure Layouts: Examples of #[repr(C)] data structures to understand how Rust data can be made ABI-compliant.
Cross-Language Function Calls: extern "C" functions that show how data is passed and modified between Rust and other languages.
Function Callbacks: Examples of passing function pointers between Rust and C for event-driven programming.
Error Handling: Demonstrations of handling errors gracefully when interfacing with C.
Memory Management: Examples covering safe memory management practices across the FFI boundary.
Practical Tests: Built-in tests to validate behavior and illustrate common pitfalls in ABI design and implementation.
Use Cases
crabi_test2 is suitable for:

Developers interested in systems programming and understanding how Rust can interface with C and other languages.
Experiments and educational purposes, offering a foundation to explore the implications of ABI stability or the lack thereof in Rust.
Installation
To install crabi_test2, add it to your Cargo.toml:

[dependencies]
crabi_test2 = "0.1.0"
thiserror = "1.0"
Example Usage
use crabi_test2::{SimpleData, modify_simple_data, CallbackData, register_callback, trigger_callback, ComplexData, process_complex_data};

fn main() {
    // Example usage of SimpleData and modify_simple_data
    let mut data = SimpleData {
        value: 0,
        flag: false,
    };

    // Call the function to modify the data
    modify_simple_data(&mut data as *mut _);

    // Print the modified data
    println!("Value: {}, Flag: {}", data.value, data.flag);

    // Example usage of CallbackData
    extern "C" fn callback_fn(val: i32) {
        println!("Callback called with value: {}", val);
    }

    let mut callback_data = CallbackData {
        value: 42,
        callback: None,
    };

    // Register and trigger the callback
    register_callback(&mut callback_data as *mut _, callback_fn);
    trigger_callback(&mut callback_data as *mut _);

    // Example usage of ComplexData and process_complex_data
    extern "C" fn complex_callback(data: *const ComplexData) {
        unsafe {
            println!("ComplexData Callback with ID: {}", (*data).id);
        }
    }

    let mut complex_data = ComplexData {
        id: 1,
        name: "Example".as_ptr(),
        name_len: 7,
        values: [1.0, 2.0, 3.0, 4.0, 5.0],
        callback: Some(complex_callback),
    };

    process_complex_data(&mut complex_data as *mut
License
Distributed under the MIT or Apache 2.0 license.

Author
Ben Santora 
