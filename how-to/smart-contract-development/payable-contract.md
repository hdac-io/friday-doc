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

## Snippet

```rust
const exchange_rate: U512 = U512::from(1000);
const ico_main_purse: PublicKey = PublicKey::from([1u8; 32]);
const main_purse_name: &str = "main";

pub fn set_main_purse() {
    let contstructing_account = runtime::get_caller();

    let contstructing_account_turef: TURef<PublicKey> = storage::new_turef(contstructing_account);
    let contstructing_account_uref: Key = contstructing_account_turef.into();

    if runtime::has_key(main_purse_name) {
        runtime::revert(ApiError::DuplicateKey);
    }
    runtime::put_key(main_purse_name, contstructing_account_uref);
}

fn get_main_purse() -> PublicKey {
    let condstructing_uref = runtime::get_key("main").unwrap_or_revert_with(ApiError::GetKey);
    let condstructing_turef: TURef<PublicKey> = condstructing_uref.try_into().unwrap_or_revert();

    storage::read(condstructing_turef.clone())
        .unwrap_or_revert_with(ApiError::Read)
        .unwrap_or_revert_with(ApiError::ValueNotFound)
}

pub fn get_token(amount: U512) {
    let requester_account = runtime::get_caller();
    let requester_purse = account::get_main_purse();

    // mint(): from simple_token contract
    // You cannot use mint() from simple_token contract directly
    // Need to revise address type from String to PublicKey
    // types::account::PublicKey
    mint(requester_account, amount * exchange_rate);
    system::transfer_from_purse_to_account(requester_purse, get_main_purse(), amount);
}

...

#[no_mangle]
pub extern "C" fn simple_token_ext() {
    let method: String = runtime::get_arg(0)
        .unwrap_or_revert_with(ApiError::MissingArgument)
        .unwrap_or_revert_with(ApiError::InvalidArgument);

    match method.as_str() {
        ...
        "get_token" => {
            let amount: U512 = runtime::get_arg(3)
                .unwrap_or_revert_with(ApiError::MissingArgument)
                .unwrap_or_revert_with(ApiError::InvalidArgument);

            get_token(amount);
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

