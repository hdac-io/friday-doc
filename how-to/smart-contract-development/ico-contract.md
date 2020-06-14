# ICO contract

## Goal

* Know how to write ICO contract

## Logic

```kotlin
# Exchange rate 1 : 1000

1 Hdac                 -------->    ICO contract token table
friday1asdgafacc1                 friday1asdgafacc1     1000 
```

ICO is a kind of fundraising. If you send your Hdac token to the contract, it checks how much you transferred, check the exchange rate, and record the number to the contract.

You can make your own ICO contract with the snippet below.

## Basic structure

```rust
Proxy contract (outer)
|- Wrapping contract caller
|- Transfer-relating activity

Logic contract (insider)
|- Invest & get dApp token
|- Set & get the wallet of the contract
|- Receive deposited Hdac token
|- Transfer
|- ...
```

{% hint style="info" %}
If you don't know what is proxy contract & logic contract and their need, please check [this article](context-and-proxy-contract.md).
{% endhint %}

## Snippet

{% hint style="warning" %}
These are non-working snippet. The purpose is just showing the logic and structure.
{% endhint %}

### Logic contract

```rust
const exchange_rate: U512 = U512::from(1000);
const ico_main_purse: PublicKey = PublicKey::from([1u8; 32]);
const main_purse_name: &str = "main";

pub fn set_main_purse() {
    let contract_purse = system::create_purse();
    let contract_purse_uref = storage::new_uref(contract_purse);
    runtime::put_key(main_purse_name, contract_purse_uref.into());
}

fn get_main_purse() -> PublicKey {
    let condstructing_uref =
        runtime::get_key("main")
            .unwrap_or_revert_with(ApiError::GetKey)
            .try_into()
            .unwrap_or_revert();

    storage::read(condstructing_uref)
        .unwrap_or_revert_with(ApiError::Read)
        .unwrap_or_revert_with(ApiError::ValueNotFound)
}

pub fn get_token(amount: U512) {
    let requester_account = runtime::get_caller();
    let requester_purse = account::get_main_purse();

    // mint(): from simple_token contract
    mint(requester_account, amount * exchange_rate);
    system::transfer_from_purse_to_account(requester_purse, get_main_purse(), amount);
}

pub fn transfer(to_account: PublicKey, amount: U512) {
    // use the logic of simple token contract
}

...

#[no_mangle]
pub extern "C" fn simple_token_ext() {
    let method: String = runtime::get_arg(0)
        .unwrap_or_revert_with(ApiError::MissingArgument)
        .unwrap_or_revert_with(ApiError::InvalidArgument);

    match method.as_str() {
        "set_contract_purse" => {
            set_main_purse();
        }
        "get_contract_purse" => {
            let main_purse = get_main_purse();
        }
        "get_token" => {
            let amount: U512 = runtime::get_arg(1)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            get_token(amount);
        }
        "transfer" => {
            let to_address: PublicKey = runtime::get_arg(1)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            let amount: U512 = runtime::get_arg(2)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);
                
            transfer(to_address, amount);
        }
        ...
    }
}

#[no_mangle]
pub extern "C" fn call() {
    set_main_purse();

    let pointer = storage::store_function_at_hash(SIMPLE_TOKEN_EXT, BTreeMap::default());
    runtime::put_key(SIMPLE_TOKEN_KEY, pointer.into());
}
```

`set_main_purse()` and `get_main_purse()` are used for security and convenience. The `PublicKey` of the contract owner should be stored, but it should be run only first and not be modified. You can implement it if you run `set_main_purse()` in installation process. You can check this logic in `call()`.

`get_token()` is quite simple.

* Get account and purse information
* Using exchange rate, mint the token to the caller
* Transfer Hdac from caller to the contract owner

### Proxy contract

```rust
pub enum Api {
    GetToken(Key, U512),
    Transfer(Key, PublicKey, U512),
}

impl Api {
    pub fn from_args() -> Self {
        let method_name: String = runtime::get_arg(0)
            .unwrap_or_revert_with(ApiError::MissingArgument)
            .unwrap_or_revert_with(ApiError::InvalidArgument);

        match method_name.as_str() {
            "get_token" => {
                let swap_hash: Key = runtime::get_arg(1)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);
                let amount: U512 = runtime::get_arg(2)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);

                Api::GetToken(swap_hash, amount)
            }
            "transfer" => {
                let swap_hash: Key = runtime::get_arg(1)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);
                let to_address: PublicKey = runtime::get_arg(2)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);                 
                let amount: U512 = runtime::get_arg(3)
                    .unwrap_or_revert_with(ApiError::MissingArgument)
                    .unwrap_or_revert_with(ApiError::InvalidArgument);

                Api::Transfer(swap_hash, to_address, amount)
            }
            _ => runtime::revert(Error::UnknownProxyApi),
        }
    }

    pub fn invoke(&self) {
        match self {
            Self::GetToken(swap_hash, amount) => {
                let swap_ref = swap_hash.to_contract_ref().unwrap_or_revert();
                let contract_purse = runtime::call_contract::<_, URef>(
                    swap_ref.clone(),
                    ("get_contract_purse",),
                );
                runtime::call_contract::<_, ()>(
                    swap_ref,
                    (
                        "get_token",
                        *amount * 1000,
                    ),
                );

                let transfer_res = system::transfer_from_purse_to_purse(
                    account::get_main_purse(),
                    contract_purse,
                    *amount,
                );

                match transfer_res {
                    Ok(_) => (),
                    Err(err) => runtime::revert(err),
                }
            }
            Self::Transfer(swap_hash, to_address, amount) => {
                let swap_ref = swap_hash.to_contract_ref().unwrap_or_revert();
                runtime::call_contract(
                    swap_ref,
                    (
                        "transfer",
                        *to_address,
                        *amount,
                    ),
                )
            }
        }
    }
}

```

You may check  `system:transfer_from_purse_to_purse`. Logically, it seems to be placed in logic contract, but it places at proxy contract. It's because of the context of `account::get_main_purse()`. You should aware that getting purse of the user & transfer should be perform at proxy contract!

{% hint style="info" %}
[This contract](https://github.com/hdac-io/swap) could be a good example of working contract.
{% endhint %}

