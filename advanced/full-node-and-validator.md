# Full node & validator

## Connect to full node

### Prerequisites

* Build friday & Casperlabs EE binary [Build](../first-step/build.md)
* Do not execute any instruction in [Run node from genesis](../first-step/run-node-from-genesis.md)
* Check launch information from [launch repository](https://github.com/hdac-io/launch)
* Take `genesis.json` and `manifest.toml` file from [launch repository](https://github.com/hdac-io/launch)

### Instructions

* Run this on another machine
  * You may set `node_name` what you want
  * You should input `chain_id` information described in [launch repository](https://github.com/hdac-io/launch)

```bash
# run execution engine grpc server
./CasperLabs/execution-engine/target/release/casperlabs-engine-grpc-server $HOME/.casperlabs/.casper-node.sock

# init node
nodef init <node_name> --chain-id <chain_id>
```

* Edit ~/.nodef/config/config.toml
  * Seed IP & ID are described in [launch repository](https://github.com/hdac-io/launch)

```text
...
# Comma separated list of seed nodes to connect to
seeds = "" -> "<genesis node's ID>@<genesis node's IP>:26656"
...
```

* Replace `~/.nodef/config/genesis.json` of [prerequisites](full-node-and-validator.md#prerequisites) to config folder \(Default: `$HOME/.nodef/config`\)
* copy `~/.nodef/config/manifest.toml` of [prerequisites](full-node-and-validator.md#prerequisites) to config folder

```bash
cp %LAUNCH_GIT_FOLDER/manifest.toml ~/.nodef/config
```

* Start your node and it syncs blocks from seed nodes

```text
nodef start
```

* If it doesn't start properly, you may execute reset and restart

```text
nodef unsafe-reset-all
nodef start
```

## Register as a validator

* [Run on this fully synchronized node](full-node-and-validator.md#connect-to-full-node)
* Create a wallet

```bash
clif keys add welcomeval # select password
```

* [Create validator](../cli/hdac-specific.md#create-validator)

```bash
clif hdac create-validator \
--from welcomeval \
--pubkey $(nodef tendermint show-validator) \
--moniker valiator-bryan
```

* Bonding amount

```text
clif hdac bond --wallet welcomeval 1000000 100000000 30000000
```

### CAUTION

If you want to disconnect your validating node, please execute [unbond](../cli/hdac-specific.md#unbond-hdac-token) first!

