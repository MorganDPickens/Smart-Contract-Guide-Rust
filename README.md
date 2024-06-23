# Smart-Contract-Guide-Rust# My Smart Contract

This repository contains a simple smart contract implemented in Rust using the Ink! framework for Substrate-based blockchains.

## Prerequisites

- Rust (nightly version)
- Substrate development environment
- Ink! CLI

## Getting Started

### Setup

1. **Install Rust Nightly:**

    ```sh
    rustup default nightly
    ```

2. **Install Ink! CLI:**

    ```sh
    cargo install cargo-contract
    ```

3. **Create a New Ink! Project:**

    ```sh
    cargo contract new my_contract
    cd my_contract
    ```

### Implementation

Open the `lib.rs` file in the `my_contract` directory and replace its content with the following:

```rust
#![cfg_attr(not(feature = "std"), no_std)] // Ensures the contract can be compiled to WebAssembly (Wasm)

#[ink::contract] // Macro to define the Ink! smart contract
mod my_contract {
    #[ink(storage)] // Attribute to mark the storage struct
    pub struct MyContract {
        value: i32, // The storage item to hold a single integer value
    }

    impl MyContract {
        #[ink(constructor)] // Marks this function as the constructor
        pub fn new(init_value: i32) -> Self {
            Self { value: init_value } // Initializes the storage with the given value
        }

        #[ink(message)] // Marks this function as a message (callable by external users)
        pub fn get(&self) -> i32 {
            self.value // Returns the stored value
        }

        #[ink(message)] // Marks this function as a message (callable by external users)
        pub fn set(&mut self, new_value: i32) {
            self.value = new_value; // Sets the storage value to the new value
        }
    }

    #[cfg(test)] // Compile and run the following code only when testing
    mod tests {
        use super::*; // Import everything from the outer module

        #[ink::test] // Marks this function as a test
        fn it_works() {
            let mut contract = MyContract::new(42); // Create a new contract instance with initial value 42
            assert_eq!(contract.get(), 42); // Check if the initial value is correct
            contract.set(100); // Change the value to 100
            assert_eq!(contract.get(), 100); // Check if the new value is correct
        }
    }
}
