# Test & debug your contract

## Goal

* Know how to write test
* Know how to debug your contract

## Preparation

* Complete [simple token contract](simple-token-contract.md)
* Go to `$FRIDAY_PATH/CasperLabs/execution-engine/engine-tests/src/test`
* Create directory `simple_token` and go to the directory
* `touch mod.rs`
* Fill the `mod.rs` as below:

```rust
use engine_core::engine_state::genesis::GenesisAccount;
use engine_shared::motes::Motes;
use engine_test_support::{
    internal::{utils, ExecuteRequestBuilder, InMemoryWasmTestBuilder},
    DEFAULT_ACCOUNT_INITIAL_BALANCE,
};
use types::{account::PublicKey, U512, Key};

const CONTRACT_SIMPLE_TOKEN: &str = "simple_token.wasm";
const METHOD_MINT: &str = "mint";
const METHOD_TRANSFER: &str = "transfer";
```

* Change directory to upper level
* Open `mod.rs` and add one more line `mod simple_token;`

## Write test code

Move to `$FRIDAY_PATH/CasperLabs/execution-engine/engine-tests/src/test/simple_token` and write inside of `mod.rs`

Let's start from here:

```rust
#[ignore]
#[test]
fn should_run_successful_simple_token_mint_and_transfer() {
    // Will implement from here
}
```

{% hint style="info" %}
`#[ignore]`: As this test is a contract test, not engine test, Casperlabs marks it as ignore in Execution Engine perspective. And it runs with `--ignore` flag.

`#[test]`: This method is a test method so that it will not be included with engine compile.
{% endhint %}

### Set genesis setting

```rust
    const GENESIS_VALIDATOR_STAKE: u64 = 10000_u64;
    const GENESIS_ACCOUNT: [u8; 32] = [1u8; 32];
    const MINT_AMOUNT: u64 = 10000;
    const TRANSFER_AMOUNT: u64 = 5000;
    const ACC_1: &str = "friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k";
    const ACC_2: &str = "friday1qt8k20h3hmdx0qulgpppnlsg92hjjtvn59qkyd";

    let accounts = vec![
        GenesisAccount::new(
            PublicKey::new(GENESIS_ACCOUNT),
            Motes::new(DEFAULT_ACCOUNT_INITIAL_BALANCE.into()),
            Motes::new(GENESIS_VALIDATOR_STAKE.into()),
        ),
    ];

    let mut builder = InMemoryWasmTestBuilder::default();
    let result = builder
        .run_genesis(&utils::create_genesis_config(accounts))
        .finish();

    println!("Preparation done");
```

It works for:

* setting genesis accounts
  * with its address \(`PublicKey`\)
  * and initial balance
  * and its staked amount
* and set as a genesis setting

### Send transaction

```rust
println!("Making mint tx for 1st account...");

let token_mint_request_1 = ExecuteRequestBuilder::standard(
    GENESIS_ACCOUNT,
    CONTRACT_SIMPLE_TOKEN,
    (String::from(METHOD_MINT), String::from(ACC_1), U512::from(MINT_AMOUNT)),
)
.build();

let mut builder = InMemoryWasmTestBuilder::from_result(result);
let result = builder
    .exec(token_mint_request_1)
    .expect_success()
    .commit()
    .finish();

println!("Mint done");
```

This is a routine to make and send a transaction.

* Organize transaction request with
  * executor account
  * the name of WASM file
  * and its parameters
* And execute it

If you intend raising error, you can implement as:

```rust
let result = builder.exec(vote_request).commit().finish();
let response = result
    .builder()
    .get_exec_response(0)
    .expect("should have a response")
    .to_owned();

let error_message = utils::get_error_message(response);

assert!(error_message.contains(&format!(
    "Revert({})",
    u32::from(ApiError::ProofOfStake(39))
)));
```

### Query value

```rust
println!("Compare queried value & requested value");
let account_value = builder
    .query(None, Key::Account(GENESIS_ACCOUNT), &[ACC_1])
    .expect("should query account");
let cl_value = account_value.as_cl_value().expect("should be CLValue");
let value: U512 = cl_value
    .clone()
    .into_t()
    .expect("should cast CLValue to integer");
assert_eq!(value, U512::from(MINT_AMOUNT));
```

After execution, the value is stored into a named key of the account. So, the step will be:

* Query with account and the named key
* Get CLValue from that and convert into the value we stored
* Compare with what you are expected

Using this procedure, we recommend to implement

* mint tokens to 2nd account
* transfer from 1st account to 2nd account
* and check the value

The whole code is:

```rust
use engine_core::engine_state::genesis::GenesisAccount;
use engine_shared::motes::Motes;
use engine_test_support::{
    internal::{utils, ExecuteRequestBuilder, InMemoryWasmTestBuilder},
    DEFAULT_ACCOUNT_INITIAL_BALANCE,
};
use types::{account::PublicKey, U512, Key};

const CONTRACT_SIMPLE_TOKEN: &str = "simple_token.wasm";
const METHOD_MINT: &str = "mint";
const METHOD_TRANSFER: &str = "transfer";

#[ignore]
#[test]
fn should_run_successful_simple_token_mint_and_transfer() {
    const GENESIS_VALIDATOR_STAKE: u64 = 10000_u64;
    const GENESIS_ACCOUNT: [u8; 32] = [1u8; 32];
    const MINT_AMOUNT: u64 = 10000;
    const TRANSFER_AMOUNT: u64 = 5000;
    const ACC_1: &str = "friday1jk2zrqqa98pwax7cq0xgkqw67qk2p8nhcpup8k";
    const ACC_2: &str = "friday1qt8k20h3hmdx0qulgpppnlsg92hjjtvn59qkyd";

    let accounts = vec![
        GenesisAccount::new(
            PublicKey::new(GENESIS_ACCOUNT),
            Motes::new(DEFAULT_ACCOUNT_INITIAL_BALANCE.into()),
            Motes::new(GENESIS_VALIDATOR_STAKE.into()),
        ),
    ];

    let mut builder = InMemoryWasmTestBuilder::default();
    let result = builder
        .run_genesis(&utils::create_genesis_config(accounts))
        .finish();

    println!("Preparation done");

    println!("Making mint tx for 1st account...");

    let token_mint_request_1 = ExecuteRequestBuilder::standard(
        GENESIS_ACCOUNT,
        CONTRACT_SIMPLE_TOKEN,
        (String::from(METHOD_MINT), String::from(ACC_1), U512::from(MINT_AMOUNT)),
    )
    .build();

    let mut builder = InMemoryWasmTestBuilder::from_result(result);
    let result = builder
        .exec(token_mint_request_1)
        .expect_success()
        .commit()
        .finish();

    println!("Mint done");

    println!("Compare queried value & requested value");
    let account_value = builder
        .query(None, Key::Account(GENESIS_ACCOUNT), &[ACC_1])
        .expect("should query account");
    let cl_value = account_value.as_cl_value().expect("should be CLValue");
    let value: U512 = cl_value
        .clone()
        .into_t()
        .expect("should cast CLValue to integer");
    assert_eq!(value, U512::from(MINT_AMOUNT));

    println!("Mint for 1st account done!");

    /////////////////////////////////////////

    println!("Making mint tx for 2nd account...");
    let token_mint_request_2 = ExecuteRequestBuilder::standard(
        GENESIS_ACCOUNT,
        CONTRACT_SIMPLE_TOKEN,
        (String::from(METHOD_MINT), String::from(ACC_2), U512::from(MINT_AMOUNT)),
    )
    .build();

    let mut builder = InMemoryWasmTestBuilder::from_result(result);
    let result = builder
        .exec(token_mint_request_2)
        .expect_success()
        .commit()
        .finish();

    println!("Mint done");

    println!("Compare queried value & requested value");
    let account_value = builder
        .query(None, Key::Account(GENESIS_ACCOUNT), &[ACC_2])
        .expect("should query account");
    let cl_value = account_value.as_cl_value().expect("should be CLValue");
    let value: U512 = cl_value
        .clone()
        .into_t()
        .expect("should cast CLValue to integer");
    assert_eq!(value, U512::from(MINT_AMOUNT));

    println!("Mint for 2nd account done!");

    /////////////////////////////////////////
    
    println!("Making transfer tx...");
    let transfer_request = ExecuteRequestBuilder::standard(
        GENESIS_ACCOUNT,
        CONTRACT_SIMPLE_TOKEN,
        (String::from(METHOD_TRANSFER), String::from(ACC_1), String::from(ACC_2), U512::from(TRANSFER_AMOUNT)),
    )
    .build();

    let mut builder = InMemoryWasmTestBuilder::from_result(result);
    let result = builder
        .exec(transfer_request)
        .expect_success()
        .commit()
        .finish();

    println!("Transfer done");

    println!("Compare queried value & requested value");
    let account_value = builder
        .query(None, Key::Account(GENESIS_ACCOUNT), &[ACC_1])
        .expect("should query account");
    let cl_value = account_value.as_cl_value().expect("should be CLValue");
    let value: U512 = cl_value
        .clone()
        .into_t()
        .expect("should cast CLValue to integer");
    assert_eq!(value, U512::from(MINT_AMOUNT-TRANSFER_AMOUNT));

    let account_value = builder
        .query(None, Key::Account(GENESIS_ACCOUNT), &[ACC_2])
        .expect("should query account");
    let cl_value = account_value.as_cl_value().expect("should be CLValue");
    let value: U512 = cl_value
        .clone()
        .into_t()
        .expect("should cast CLValue to integer");
    assert_eq!(value, U512::from(MINT_AMOUNT+TRANSFER_AMOUNT));
}
```

## Debugging

If you're not using debugger, the most simple method is checking console output. But unfortunately, it doesn't support console output. Instead, you can use `runtime::revert()` method.

If you want to find the point of the error:

* Open your contract code
* Guess the point
* Insert `runtime::revert()`
  * Import use `types::ApiError` or implement your custom error
  * You may input the parameter from `types::ApiError` or your custom error
* Compile your contract
* Run your contract with test code
* If the contract reverts with the error code you inputted, it means that the contract runs without error before the revert you inputted.

