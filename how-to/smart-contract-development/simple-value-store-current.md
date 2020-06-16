# Simple value store

## Goal

* Knows how to create your own project
* Knows how to call a smart contract
* Knows how to pass parameters into the contract
* Knows how to query the stored value of the contract

## Prepare environment for your development

* Be prepare [Rust](https://www.rust-lang.org/tools/install) language environment and its development knowledge

Execute it for creating your project

```bash
cargo new simple_store --lib
#  Created library `simple_store` package
```

Initialize as a Git repository

```bash
git init
```

The folder `simple_store` is created. The structure is as like below:

```text
simple_store/
├── src
│   ├── lib.rs          // Main file of the library
├── Cargo.toml          // Setting for Package manager
```

Add text file named: `rust-toolchain`

```bash
nightly-2020-03-19
```

Then, file structure will be:

```text
simple_store/
├── src
│   ├── lib.rs          // Main file of the library
├── Cargo.toml          // Setting for Package manager
├── rust-toolchain      // Rust toolchain
```

Open `Cargo.toml` and revise as like the below:

```go
[package]
name = "simple_store"
version = "0.1.0"
authors = ["Your_name <yourname@hdac.io>"]
edition = "2018" // DO NOT CHANGE this. Related with Rust version

[lib]
crate-type = ["lib", "cdylib"]
bench = false
doctest = false
test = false

[features]
std = ["contract/std", "types/std"]
lib = []
enable-bonding = []

[dependencies]
contract = { git="https://github.com/hdac-io/CasperLabs", branch="master", package = "casperlabs-contract", features = ["std"] }
types = { git="https://github.com/hdac-io/CasperLabs", branch="master", package = "casperlabs-types", features = ["std"] }

```

And open `src/lib.rs` , and replace the code as like below

```rust
#![no_std]

extern crate alloc;

use alloc::string::String;

use contract::{
    contract_api::{runtime, storage, URef},
    unwrap_or_revert::UnwrapOrRevert,
};
use types::{ApiError, Key};
```

Now, your development environment is ready! Let's start our the very first contract!

## Store function

```rust
const KEY: &str = "special_value";

fn store(value: String) {
    let value_ref: URef = storage::new_uref(value);
    runtime::put_key(KEY, value_ref.into());
}
```

This function stores your input into key of the contract.  
`storage::new_uref()` stores the value and return **U**nforgeable **Ref**erence. And this URef object is changed into `Key` by `.into()` method. Then, `KEY` is mapped to the stored value above. When you query, you may get the stored value by the `KEY` "special\_value".

## Call function, the entry point of the execution

```rust
#[no_mangle]
pub extern "C" fn call() {
    let value: String = runtime::get_arg(0)
        .unwrap_or_revert_with(ApiError::MissingArgument)
        .unwrap_or_revert_with(ApiError::InvalidArgument);

    store(value);
}
```

When Execution engine executes the contract, it finds `call()` method and routed by the parameters. So, it can be thought of as `main()` function of the typical programming language.

You may see `runtime::get_arg()` . It gets byted parameters from external input. And two following `.unwrap_or_revert_with()` mean that the contract execution should be terminated if there is an error in `get_arg()` , and finishes with the return code as given in parameter.

Here is the whole code:

```rust
#![no_std]

extern crate alloc;

use alloc::string::String;

use contract::{
    contract_api::{runtime, storage, URef},
    unwrap_or_revert::UnwrapOrRevert,
};
use types::{ApiError, Key};

const KEY: &str = "special_value";

fn store(value: String) {
    // Store `value` under a new unforgeable reference.
    let value_ref: URef = storage::new_uref(value);
    
    // Store this key under the name "special_value" in context-local storage.
    runtime::put_key(KEY, value_ref.into());
}

// All session code must have a `call` entrypoint.
#[no_mangle]
pub extern "C" fn call() {
    // Get the optional first argument supplied to the argument.
    let value: String = runtime::get_arg(0)
        // Unwrap the `Option`, returning an error if there was no argument supplied.
        .unwrap_or_revert_with(ApiError::MissingArgument)
        // Unwrap the `Result` containing the deserialized argument or return an error if there was
        // a deserialization error.
        .unwrap_or_revert_with(ApiError::InvalidArgument);

    store(value);
}

```

## Build your contract

### Build your own Makefile

Please write your `Makefile` on your source root:

```bash
CARGO  = $(or $(shell which cargo),  $(HOME)/.cargo/bin/cargo)
RUSTUP = $(or $(shell which rustup), $(HOME)/.cargo/bin/rustup)

RUST_TOOLCHAIN := $(shell cat rust-toolchain)

build:
	$(CARGO) build \
	        --release $(filter-out --release, $(CARGO_FLAGS)) \
	        --target wasm32-unknown-unknown

.PHONY: setup
setup: rust-toolchain
	$(RUSTUP) update
	$(RUSTUP) toolchain install $(RUST_TOOLCHAIN)
	$(RUSTUP) target add --toolchain $(RUST_TOOLCHAIN) wasm32-unknown-unknown

```

### Build your WASM contract

Please execute it the first time

```bash
make setup
```

And, you may build it with:

```bash
make build
```

Your compiled result is placed in `./target/wasm32-unknown-unknown/store_value.wasm`.

## Run smart contract on your network

#### Prerequisite

You should check that the network is built and running.

{% hint style="info" %}
If you not, please [check it here to run your own testnet in local.](../../first-step/deploy-your-own-friday-testnet.md)
{% endhint %}

Now, let's run your contract into your local network.  
I'll put "bryan" as an input parameter \(String\). The command is like the below:

{% hint style="info" %}
If you have curious about the look-like-complex input parameter, check [this document](https://github.com/CasperLabs/CasperLabs/blob/dev/docs/CONTRACTS.md#contract-argument-details).
{% endhint %}

```bash
clif contract run <type> <wasm-path>|<uref>|<name>|<hash> <argument> <fee> --from <from>
```

```bash
clif contract run wasm simple_store.wasm \ 
'[{"name": "it_doesn_t_matter", "value": {"string_value": "bryan"}}' \ 
0.02 --from <your_wallet_alias>

confirm transaction before signing and broadcasting [y/N]: y
Password to sign with 'anna':
{
  "height": "0",
  "txhash": "E79988AFF298576C0D8CCC85856A3FADB7ED32B129E2AFD540504F09F8427177",
  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\"
:\"executionengine\"}]}]}]",
  "logs": [
    {
      "msg_index": 0,
      "success": true,
      "log": "",
      "events": [
        {
          "type": "message",
          "attributes": [
            {
              "key": "action",
              "value": "executionengine"
            }
          ]
        }
      ]
    }
  ]
}
```

Get `txhash` and check the tx is executed well or not

```bash
clif query tx E79988AFF298576C0D8CCC85856A3FADB7ED32B129E2AFD540504F09F8427177 | grep success

#  "raw_log": "[{\"msg_index\":0,\"success\":true,\"log\":\"\",\"events\":[{\"type\":\"message\",\"attributes\":[{\"key\":\"action\",\"value\":\"executionengine\"}]}]}]",
#      "success": true,
```

If there is no special error log, the execution succeeds!

## Query your value

You succeeded to store "bryan" at "special\_value". Then, it's time to query.  
You may execute as like below:

```bash
clif contract query address $(clif keys show <wallet_alias> -a) special_value

# {
#   "value": "bryan"
# }
```

Hurrah! Now you've learned how to develop, build, run, and query of your smart contract!

