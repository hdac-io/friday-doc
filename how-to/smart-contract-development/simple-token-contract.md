# Simple token contract

## Goal

* Knows how to store my data in key-value store
* Knows how to call method of the smart contract
* Knows how to develop smart contract with token

{% hint style="info" %}
This example is just for your understanding token flow. Do not use this example at your actual token implementation. For being actual, we recommend to implement ERC-20 standard logic with overflow control.
{% endhint %}

## Preparation

* Create your project as described in [here](simple-value-store-current.md#prepare-environment-for-your-development)
* Replace `Cargo.toml` as like [this](simple-value-store-current.md#prepare-environment-for-your-development)
* Replace your `lib.rs` as like below

```rust
#![no_std]

extern crate alloc;

use alloc::string::String;
use core::convert::TryInto;

use contract::{
    contract_api::{runtime, storage},
    unwrap_or_revert::UnwrapOrRevert,
};
use types::{ApiError, U512, URef};
```

## Feature lists

* Storage
  * Key - String address / Token - U512 number
* Methods
  * `load_or_create_purse(address: String) -> U512` - Load balance of the given address. If not exist, create a new one
  * `update_purse(address: String, amount: U512)` - Save the value to the given address
  * `inc_token(address: String, amount: U512)` - Increase the given amount of the token to the given address
  * `dec_token(address: String, amount: U512)` - Decrease the given amount of the token to the given address
  * `pub mint(address: String, amount: U512)` - Mint token to the given address. Use `inc_token()`
  * `pub transfer(from_address: String, to_address: String, amount: U512)` - Transfer token to the each given address. Use `inc_token()` and `dec_token()`
* Limitation
  * For simple implementation, no overflow & underflow check of each balance.
  * Just use address as string, not `PublicKey` form.
  * Store on root `named_key` storage, not using `ContractRef`.

## Implementation

### `update_purse()`

```rust
fn update_purse(address: String, amount: U512) {
    let amount_uref: URef = storage::new_uref(amount);

    runtime::remove_key(address.as_str());
    runtime::put_key(address.as_str(), amount_uref.into());
}
```

Though this snippet, you can learn how to store a value in the state of the contract.

`storage::new_uref()` returns **U**nforgeable **Ref**rence with custom value. In this example, the type of the return is `URef`, but many types are also available to store: `String, BTreeMap, Vec<U512>, Vec<String>, PublicKey, Key, ContractRef...`

Then, it should be changed into `Key::URef` type. `.into()` helps for it.

Now, you are ready to map them. Your account has one `named_list: BTreeMap<Key, URef>`, and you can place your data that you need to keep to the state. Using `put_key()`, you can map and store the data and its reference.

{% hint style="info" %}
If you store `ContractRef`, it can be a stored contract, a hash-formed address will be given, and it can be called from everywhere. Then, you can run dApp by that. It can be handled in [here](stored-contract.md) later.
{% endhint %}

### `load_or_create_purse()`

```rust
fn load_or_create_purse(address: String) -> U512 {
    if !runtime::has_key(address.as_str()) {
        update_purse(address, U512::from(0));
        return U512::zero();
    }

    let balance_uref: URef = runtime::get_key(address.as_str())
        .unwrap_or_revert_with(ApiError::GetKey)
        .try_into()
        .unwrap_or_revert();

    let balance = storage::read(balance_uref.clone())
        .unwrap_or_revert_with(ApiError::Read)
        .unwrap_or_revert_with(ApiError::ValueNotFound);

    balance
}
```

It loads how much money the given address has. If the purse does not exists, a new purse will be created.

You may check how to load the value from key, which is opposite process of above. Get `URef` from `Key`, and load data from storage.

You may see many `unwrap_or_revert()` in there. They are kind of expansion of standard function `unwrap()` . If `Err(obj)` returns, execution reverts with the error code given.

### Rest of the features

```rust
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
```

These are the amount of the holding token updaters. It loads the value, updates the number, and store into the state of the contract.

```rust
pub fn mint(address: String, amount: U512) {
    inc_token(address, amount);
}

pub fn transfer(from_address: String, to_address: String, amount: U512) {
    dec_token(from_address, amount);
    inc_token(to_address, amount);
}
```

These are interface functions, `mint()` and `transfer()` . `mint()` is producing money and give to the given account. `transfer()` deducts to the sender, and adds to the receiver.

### Main caller

```rust
#[no_mangle]
pub extern "C" fn call() {
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
```

This is the main pattern how to connect to features from `call()` function.

* Get the name of the method, and route using `match`
* Get more arguments if needed, and call feature functions.

Here is the whole code:

```rust
#![no_std]

extern crate alloc;

use alloc::string::String;
use core::convert::TryInto;

use contract::{
    contract_api::{runtime, storage},
    unwrap_or_revert::UnwrapOrRevert,
};
use types::{ApiError, U512, URef};


fn update_purse(address: String, amount: U512) {
    let amount_uref: URef = storage::new_uref(amount);
    runtime::remove_key(address.as_str());
    runtime::put_key(address.as_str(), amount_uref.into());
}

fn load_or_create_purse(address: String) -> U512 {
    if !runtime::has_key(address.as_str()) {
        update_purse(address, U512::from(0));
        return U512::zero();
    }

    let balance_uref: URef = runtime::get_key(address.as_str())
        .unwrap_or_revert_with(ApiError::GetKey)
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
pub extern "C" fn call() {
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

```

## Build your contract

* Make your own `Makefile` and build wasm contract as written in [here](simple-value-store-current.md#build-your-contract)

## Run & check smart contract on your network

#### Prerequisite

You should check that the network is built and running.

{% hint style="info" %}
If you not, please [check it here to run your own testnet in local.](../../first-step/deploy-your-own-friday-testnet.md)
{% endhint %}

Now, let's run your contract into your local network.

* Check your wallet first. Please be prepare 2 wallets.

```bash
➜  release git:(private/bryan) ✗ clif keys list
[
  {
    "name": "anna",
    "type": "local",
    "address": "friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k",
    "pubkey": "fridaypub1addwnpepqg68kudhzgffqp6ydd3ges7ldqs2xtnw642j09a9k38yul8ygg5qstq6pyq"
  },
  {
    "name": "elsa",
    "type": "local",
    "address": "friday1qt8k20h3hmdx0qulgpppnlsg92hjjtvn59qkyd",
    "pubkey": "fridaypub1addwnpepqww38nuvtlavaj5mpq4xfktus9yw4um23j8lq7vdw669xjnws58lwzktn0d"
  }
]%
```

We will use the value of `address` field.

* Mint 100,000 tokens into two accounts.

We should organize input parameters into JSON array. For calling mint contract, you should organize `[method: String, address: String, amount: U512]` You may find each form of the type in [here](https://github.com/CasperLabs/CasperLabs/blob/dev/docs/CONTRACTS.md#contract-argument-details).

* String: `{"name":"surname","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"Nakamoto"}}}`
* U512: `{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"123456"}}}}`

{% hint style="info" %}
As they will support Enum, there is `name` field. But currently it doesn't affect to data input.
{% endhint %}

According to this type description, the organized JSON parameter can be below:

```javascript
[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"mint"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1mc2dz6wmq678nhu360yf8yngq4657hret8zf3kx7c3tts0aweuasnjt3fk"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"123456"}}}}]
```

By this rule,

```bash
clif contract run <type> <wasm-path>|<uref>|<name>|<hash> <argument> <fee> --from <from>
```

You can run the contract twice with each different parameters:

```bash
clif contract run wasm simple_token.wasm '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"mint"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1mc2dz6wmq678nhu360yf8yngq4657hret8zf3kx7c3tts0aweuasnjt3fk"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"100000"}}}}]' 0.02 --from anna
clif contract run wasm simple_token.wasm '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"mint"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1pvajn4cu8w4futm7angklhhnrlyceqek4hghxu6fv2gj4x74tafsljmnyh"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"100000"}}}}]' 0.02 --from anna
```

{% hint style="info" %}
If you want to learn how to organize JSON, please check the document [Contract argument details](contract-argument-details.md).
{% endhint %}

After a few seconds, let's query the value. You may check the same result both of accounts.

```bash
# clif contract query address <contract_owner> <query_parameter>
clif contract query address $(clif keys show anna -a) $(clif keys show anna -a)
clif contract query address $(clif keys show anna -a) $(clif keys show elsa -a)

#{
#  "stringValue": "100000"
#}
```

Then, let's try to transfer. You should organize `[method: String, from_address: String, to_address: String, amount: U512]` I believe you can organize the JSON input parameter. :\) Let's try to send 50000 tokens.

```bash
clif contract run wasm simple_token.wasm '[{"name":"method","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"transfer"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1mc2dz6wmq678nhu360yf8yngq4657hret8zf3kx7c3tts0aweuasnjt3fk"}}},{"name":"address","value":{"cl_type":{"simple_type":"STRING"},"value":{"str_value":"friday1pvajn4cu8w4futm7angklhhnrlyceqek4hghxu6fv2gj4x74tafsljmnyh"}}},{"name":"amount","value":{"cl_type":{"simple_type":"U512"},"value":{"u512":{"value":"50000"}}}}]' 0.02 --from anna
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

Hurrah! Now you've learned how to develop & run token system by smart contract! You can use it with some safe math and more protection logics.



