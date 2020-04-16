# Simple value store \(Future\)

## Goal

* Knows how to create your own project
* Knows how to call a smart contract
* Knows how to pass parameters into the contract

## Prepare environment for your development

* Be prepare [Rust](https://www.rust-lang.org/tools/install)
* Go to the cloned `friday` repository and installed, go to:
  * `$FRIDAY_DIR/Casperlabs/execution_engine/cargo-casperlabs`
* Execute the command below

```text
cargo install cargo-casperlabs --path=.
```

* Change to your expected root path of the project and create your project

```text
cargo casperlabs simple_store
```

* Prepare rust environment and toolchain for WASM compile

```text
cd simple_store
rustup install $(cat rust-toolchain)
rustup target add --toolchain=$(cat rust-toolchain) wasm32-unknown-unknown
```

## Project hierarchy

The folder `hello_world` is created. The structure is as like below:

```text
simple_store/
├── contract
│   ├── .cargo
│   │   └── config
│   ├── Cargo.toml          // Setting for Package manager
│   ├── rust-toolchain      // Rust version (nightly)
│   └── src
│       └── lib.rs          // Main file of the project
└── tests
    ├── build.rs
    ├── Cargo.toml
    ├── rust-toolchain
    └── src
        └── integration_tests.rs
```

## Build your contract

In `contract/src/lib.rs`, a simple value storage contract is built already. In this time, not seeing in detail anatomy, but just build and ship into network.

* Compile the contract

```bash
# at hello_world/contract/src
cargo build --release
```

Then, your contract is built at `simple_store/contract/target/wasm32-unknown-unknown/release/contract.wasm`

## Run smart contract on your network

#### Prerequisite

You should check that the network is built and running.

{% hint style="info" %}
If you not, please [check it here to run your own testnet in local.](../../../first-step/deploy-your-own-friday-testnet.md)
{% endhint %}

Now, let's run your contract into your local network.

```bash
clif contract run <type> <wasm-path>|<uref>|<name>|<hash> <argument> <fee> <gas_price> --from <from>
```

```bash
clif contract run wasm ./contract.wasm '"bryan"' 0.01 30000000 --from $YOUR_WALLET_ALIAS

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

Then, you succeeded to run your first contract! Then, let's check the value.

```bash

```

