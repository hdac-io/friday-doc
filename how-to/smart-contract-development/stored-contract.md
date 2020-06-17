# Stored contract

## What is "Stored contract"

New Hdac mainnet supports 4 kinds of execution:

1. By WebAssembly file
2. By URef \(Kind of Stored contract hash\)
3. By Hash \(Kind of Stored contract hash\)

In [Simple value store](simple-value-store-current.md) and [Simple token contract](simple-token-contract.md), we can execute contract by WebAssembly file. But if do that in every execution, the size of transaction goes huge and it can cause performance decrease of our main network. The contract binary needs to be stored and can be called by address.

## Goal

* Know how to store your contract
* Know how to execute your contract by hash-like address
* Know how to query the state executed by the stored contract

## Preparation

* `nodef unsafe-reset-all` and `nodef start`
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

In `call()` , you are storing `simple_token_ext()` method into the Key named `simple_token` . After installed, you can call this contract by `simple_token` !

Here is the whole contract:

```rust
#![no_std]

extern crate alloc;

use alloc::{string::String, collections::BTreeMap};
use core::convert::TryInto;

use contract::{
    contract_api::{runtime, storage},
    unwrap_or_revert::UnwrapOrRevert,
};
use types::{ApiError, Key, U512, URef};


fn update_purse(address: String, amount: U512) {
    let amount_uref = storage::new_uref(amount);

    runtime::remove_key(address.as_str());
    runtime::put_key(address.as_str(), amount_uref.into());
}

fn load_or_create_purse(address: String) -> U512 {
    if !runtime::has_key(address.as_str()) {
        update_purse(address, U512::from(0));
        return U512::zero();
    }

    let balance_uref: URef = runtime::get_key(address.as_str())
        .unwrap_or_revert_with(ApiError::GetKey);
        .try_into()
        .unwrap_or_revert();

    let balance = storage::read(balance_uref.clone())
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

* Make your own `Makefile` and build wasm contract as written in [here](simple-value-store-current.md#build-your-contract)

## Install the contract

Just run this contract without parameter

```bash
clif contract run wasm simple_token_install.wasm '' 0.02 --from anna
```

## Run contract by named key

In above, I told that the method is stored at  key `simple_token`. Let's check it is real or not.

```bash
clif contract run name simple_token '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"mint"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1mc2dz6wmq678nhu360yf8yngq4657hret8zf3kx7c3tts0aweuasnjt3fk"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"100000"}}}}]' 0.02 --from anna
clif contract run name simple_token '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"mint"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1pvajn4cu8w4futm7angklhhnrlyceqek4hghxu6fv2gj4x74tafsljmnyh"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"100000"}}}}]' 0.02 --from anna
```

After a few seconds, let's query the value. You may check the same result both of accounts.

```bash
# clif contract query address <contract_owner> <query_parameter>
clif contract query address $(clif keys show anna -a) $(clif keys show anna -a)
clif contract query address $(clif keys show anna -a) $(clif keys show elsa -a)

{
  "bigInt": {
    "value": "100000",
    "bitWidth": 512
  }
}
```

Both results are same although no WASM file is used from this execution.

And let's run transfer.

```bash
clif contract run name simple_token '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"transfer"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1mc2dz6wmq678nhu360yf8yngq4657hret8zf3kx7c3tts0aweuasnjt3fk"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1pvajn4cu8w4futm7angklhhnrlyceqek4hghxu6fv2gj4x74tafsljmnyh"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"50000"}}}}]' 0.02 --from anna
```

Wait for a few second, and let's check the value.

```bash
# Sender
{
  "bigInt": {
    "value": "50000",
    "bitWidth": 512
  }
}

# Receiver
{
  "bigInt": {
    "value": "150000",
    "bitWidth": 512
  }
}
```

It's same as the before tutorial! Proved that contract can be stored into Hdac mainnet!

## Check address of the contract

But there is a problem.

```bash
Address
|- Named key 1
|- Named key 2
...
```

Named key is rely on deployer's address. So, if you execute contract by name, Hdac mainnet trys to find name key from your address. It means that you cannot execute other user's contract. The contract can be executed only by the deployer. Then, how can Hdac mainnet run dApp?

Don't worry! The contract has another unique name **Hash**. It is thought of as an address.

You can check your named keys and its address as below

```bash
clif contract query address $(clif keys show <wallet_allias> -a) ""
```

And you may get the output as like below:

```javascript
{
  "account": {
    "publicKey": "0Go0RqMOZZkkIM4zQZN9hjITclbRGeUBi250rXzrmYU=",
    "mainPurse": {
      "uref": "bvgOE0xOgQAb+9I8Kj0UKQ4QcbjKxgO52wjDMC/YY6s=",
      "accessRights": "READ_ADD_WRITE"
    },
    "namedKeys": [
      {
        "name": "mint",
        "key": {
          "uref": {
            "uref": "fridaycontracturef12gz99r4u32p22tcuvmuvhcg4nw5s5clnw5ena3qye75e8gt68kns0pr6lj",
            "accessRights": "READ"
          }
        }
      },
      {
        "name": "pos",
        "key": {
          "uref": {
            "uref": "fridaycontracturef1qydead29jg44yktvptwh6ql9yuy2xzgytwrf2zcqjxjf5mmurqaq9ymwvw",
            "accessRights": "READ"
          }
        }
      },
      {
        "name": "friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k",
        "key": {
          "uref": {
            "uref": "m5XBQBVg3DG5GoW0IU8IJIk1vXJsUUc571WNIGmAfsk=",
            "accessRights": "READ_ADD_WRITE"
          }
        }
      },
      {
        "name": "friday1qt8k20h3hmdx0qulgpppnlsg92hjjtvn59qkyd",
        "key": {
          "uref": {
            "uref": "VprGQF1N6/6WXNt9EnyPENlPifNP0YfNJetpfT2lCxE=",
            "accessRights": "READ_ADD_WRITE"
          }
        }
      },
      {
        "name": "simple_token",
        "key": {
          "hash": {
            "hash": "fridaycontracthash1qydead29jg44yktvptwh6ql9yuy2xzgytwrf2zcqjxjf5mmurqaq9ymwvw/D63U="
          }
        }
      }
    ],
    "associatedKeys": [
      {
        "publicKey": "0Go0RqMOZZkkIM4zQZN9hjITclbRGeUBi250rXzrmYU=",
        "weight": 1
      }
    ],
    "actionThresholds": {
      "deploymentThreshold": 1,
      "keyManagementThreshold": 1
    }
  }
}
```

Can you see `simple_token`? Your contract is stored at this hash and anyone can call by this hash!

```bash
clif contract run hash fridaycontracthash1qydead29jg44yktvptwh6ql9yuy2xzgytwrf2zcqjxjf5mmurqaq9ymwvw '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"mint"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1mc2dz6wmq678nhu360yf8yngq4657hret8zf3kx7c3tts0aweuasnjt3fk"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"100000"}}}}]' 0.02 --from anna
clif contract run hash fridaycontracthash1qydead29jg44yktvptwh6ql9yuy2xzgytwrf2zcqjxjf5mmurqaq9ymwvw '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"mint"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1mc2dz6wmq678nhu360yf8yngq4657hret8zf3kx7c3tts0aweuasnjt3fk"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"100000"}}}}]' 0.02 --from anna
```

`KOdSXthj7pri1WpwtqPlmG5jO16T8FrquvWcq2/D63U=` is now your address of the contract!

