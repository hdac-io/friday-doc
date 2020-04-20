# Stored contract

## What is "Stored contract"

New Hdac mainnet supports 4 kinds of execution:

1. By WebAssembly file
2. By URef \(Kind of Stored contract hash\)
3. By Hash \(Kind of Stored contract hash\)

In [Simple value store](simple-value-store-current/) and [Simple token contract](simple-token-contract.md), we can execute contract by WebAssembly file. But if do that in every execution, the size of transaction goes huge and it can cause performance decrease of our main network. The contract binary needs to be stored and can be called by address.

## Goal

* Know how to store your contract
* Know how to execute your contract by hash-like address
* Know how to query the state executed by the stored contract

## Preparation

* Duplicate project [Simple token contract](simple-token-contract.md) and rename to `simple_token_install`
* Revise `Cargo.toml`

#### As-is

```kotlin
[package]
name = "simple_token" // <- here
version = "0.1.0"
authors = ["Bryan <psy2848048@gmail.com>"]
edition = "2018"
```

#### To-be

```kotlin
[package]
name = "simple_token_install" // <- here
version = "0.1.0"
authors = ["Bryan <psy2848048@gmail.com>"]
edition = "2018"
```

## Implementation

* Change import

#### As-is

```rust
use alloc::string::String;
```

#### To-be

```rust
use alloc::{string::String, collections::BTreeMap};
```

* Rename `call()` method to what you want

#### As-is

```rust
#[no_mangle]
pub extern "C" fn call() {
    ...
}
```

#### To-be

```rust
#[no_mangle]
pub extern "C" fn simple_token_ext() {
    ...
}
```

{% hint style="danger" %}
**DO NOT REMOVE `#[no_mangle]`**! If you remove that, Rust optimizer rename the name of the function and the pointer cannot find the function.
{% endhint %}

* Implement another `call()` method

{% hint style="warning" %}
You may change to what you want, but you should align the value of `SIMPLE_TOKEN_EXT` and the name of the method.
{% endhint %}

```rust
#[no_mangle]
pub extern "C" fn simple_token_ext() { // <- This name and..
    ...
}

// this name should be aligned!
const SIMPLE_TOKEN_EXT: &str = "simple_token_ext";
const SIMPLE_TOKEN_KEY: &str = "simple_token";

#[no_mangle]
pub extern "C" fn call() {
    let pointer = storage::store_function_at_hash(SIMPLE_TOKEN_EXT, BTreeMap::Defaut());
    runtime::put_key(SIMPLE_TOKEN_KEY, pointer.into());
}
```

Here is the whole contract:

```rust
#![no_std]

extern crate alloc;

use alloc::{string::String, collections::BTreeMap};
use core::convert::TryInto;

use contract::{
    contract_api::{runtime, storage, TURef},
    unwrap_or_revert::UnwrapOrRevert,
};
use types::{ApiError, Key, U512};


fn update_purse(address: String, amount: U512) {
    let amount_turef: TURef<U512> = storage::new_turef(amount);
    let amount_uref: Key = amount_turef.into();

    runtime::remove_key(address.as_str());
    runtime::put_key(address.as_str(), amount_uref);
}

fn load_or_create_purse(address: String) -> U512 {
    if !runtime::has_key(address.as_str()) {
        update_purse(address, U512::from(0));
        return U512::zero();
    }

    let balance_uref = runtime::get_key(address.as_str()).unwrap_or_revert_with(ApiError::GetKey);
    let balance_turef: TURef<U512> = balance_uref.try_into().unwrap_or_revert();
    //let balance_turef = TURef::from(balance_uref);

    let balance = storage::read(balance_turef.clone())
        .unwrap_or_revert_with(ApiError::Read)
        .unwrap_or_revert_with(ApiError::ValueNotFound);

    balance
}

fn inc_token(address: String, amount: U512) {
    let mut curr_balance = load_or_create_purse(address.clone());
    curr_balance += amount;
    update_purse(address, curr_balance);
}

fn dec_token(address: String, amount: U512) {
    let mut curr_balance = load_or_create_purse(address.clone());
    curr_balance -= amount;
    update_purse(address, curr_balance);
}

pub fn mint(address: String, amount: U512) {
    inc_token(address, amount);
}

pub fn transfer(from_address: String, to_address: String, amount: U512) {
    dec_token(from_address, amount);
    inc_token(to_address, amount);
}

#[no_mangle]
pub extern "C" fn simple_token_ext() {
    let method: String = runtime::get_arg(0)
        .unwrap_or_revert_with(ApiError::MissingArgument)
        .unwrap_or_revert_with(ApiError::InvalidArgument);

    match method.as_str() {
        "mint" => {
            let address: String = runtime::get_arg(1)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            let amount: U512 = runtime::get_arg(2)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            mint(address, amount);
        }

        "transfer" => {
            let from_address: String = runtime::get_arg(1)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            let to_address: String = runtime::get_arg(2)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            let amount: U512 = runtime::get_arg(3)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            transfer(from_address, to_address, amount);
        }
        _ => runtime::revert(ApiError::InvalidArgument),
    }
}

const SIMPLE_TOKEN_EXT: &str = "simple_token_ext";
const SIMPLE_TOKEN_KEY: &str = "simple_token";

#[no_mangle]
pub extern "C" fn call() {
    let pointer = storage::store_function_at_hash(SIMPLE_TOKEN_EXT, BTreeMap::Defaut());
    runtime::put_key(SIMPLE_TOKEN_KEY, pointer.into());
}
```

## Build your contract

* Move to `$FRIDAY_DIR/Casperlabs/execution_engine`
* Execute:

```bash
make build-contract-rs/simple_token_install

# /Users/bryan/.cargo/bin/cargo build \
#                --release  \
#                --package simple_token_install \
#                --target wasm32-unknown-unknown
# warning: /Users/bryan/gerrit/Casperlabs/execution-engine/Cargo.toml: unused manifest key: profile.release.overrides
#   Compiling simple_store v0.1.0 (/Users/bryan/gerrit/Casperlabs/execution-engine/contracts/examples/simple_token_install)
#    Finished release [optimized] target(s) in 0.34s
```

And you may find your contract binary at `$FRIDAY_DIR/Casperlabs/execution-engine/target/wasm32-unknown-unknown/release`

with the name "store\_token\_install.wasm"

## Install the contract

Just run this contract without parameter

```bash
clif contract run wasm simple_token_install.wasm '' 0.02 50000000 --from anna
```

## Check address of the contract

TBD

