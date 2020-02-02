# Full node & validator

## Connect to seed node

* Run this on another machine

```bash
# run execution engine grpc server
./CasperLabs/execution-engine/target/release/casperlabs-engine-grpc-server $HOME/.casperlabs/.casper-node.sock

# init node
nodef init <node_name> --chain-id testnet
```

* edit ~/.nodef/config/config.toml

```text
...
# Comma separated list of seed nodes to connect to
seeds = "" -> "<genesis node's ID>@<genesis node's IP>:26656"
...
```

* replace `~/.nodef/config/genesis.json` to genesis node's one what you saved above.
* copy `~/.nodef/config/manifest.toml` to manifest node's one what you saved above.

## Running validator

* Run on this fullly synchronized node
* Create a wallet

```bash
clif keys add bryan # select password
```

* Create validator

```bash
clif executionlayer create-validator \
--from bryan \
--pubkey $(nodef tendermint show-validator) \
--moniker valiator-bryan
```

* Bonding amount

```text
clif executionlayer bond \
--from bryan \
--amount 1000000 \
--fee 100000000 \
--gas-price 30000000
```

