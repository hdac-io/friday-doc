# Join a Network

## Setting Up a New Node

### Prerequisites

* Build friday & Casperlabs EE binary [Build](build.md)
* Do not execute any instruction in [Run node from genesis](deploy-your-own-friday-testnet.md)
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

```yaml
# Comma separated list of seed nodes to connect to
seeds = "" -> "<genesis node's ID>@<genesis node's IP>:26656"
```

* Replace `~/.nodef/config/genesis.json` of [prerequisites](join-a-network.md#prerequisites) to config folder \(Default: `$HOME/.nodef/config`\)
* copy `~/.nodef/config/manifest.toml` of [prerequisites](join-a-network.md#prerequisites) to config folder

```bash
cp %LAUNCH_GIT_FOLDER/manifest.toml ~/.nodef/config
```

* Start your node and it syncs blocks from seed nodes

```bash
nodef start
```

* If it doesn't start properly, you may execute reset and restart

```bash
nodef unsafe-reset-all
nodef start
```



