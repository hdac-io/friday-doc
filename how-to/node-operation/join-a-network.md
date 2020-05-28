# Join a Network

{% hint style="info" %}
Before setting up your node, make sure you have [install Friday binaries.](../../first-step/installation.md)

Do not [Deploy Your Own Friday Testnet](../../first-step/deploy-your-own-friday-testnet.md).
{% endhint %}

## Setting Up a New Node

### Prerequisites

* Check launch information from [launch repository](https://github.com/hdac-io/launch)
* Take `genesis.json` ,  `chain-id` and seed information from [launch repository](https://github.com/hdac-io/launch)

### Instructions

* First, run execution engine grpc server on your Friday directory

  ```bash
  ./CasperLabs/execution-engine/target/release/casperlabs-engine-grpc-server $HOME/.casperlabs/.casper-node.sock -z
  ```

* Init your node

  ```bash
  nodef init <node_name> <consensus_module> --chain-id <chain_id>
  ```

  * You may set `node_name` what you want
  * You should input `chain_id` information described in [launch repository](https://github.com/hdac-io/launch)
  * The consensus\_module must be entered by selecting friday or tendermint. It should match the chain, so check line 3-4 of genesis.json.

* Edit `config.toml`  file of your config directory \(default: `$HOME/.nodef/config`\)

  ```bash
  # Comma separated list of seed nodes to connect to
  seeds = "" -> "<genesis node's ID>@<genesis node's IP>:26656"
  ```

  * Seed IP & ID are described in [launch repository](https://github.com/hdac-io/launch)

* Replace `genesis.json` file of [launch repository](https://github.com/hdac-io/launch) to your config directory
* Start your node

  ```bash
  nodef start
  ```

* Finally, your node syncs blocks from network and you can use your node when block sync finished

### Trouble Shooting

* If your node doesn't start properly, you may execute reset and do instruction again

  ```bash
  nodef unsafe-reset-all
  ```

