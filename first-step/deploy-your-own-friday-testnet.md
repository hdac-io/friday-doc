# Deploy Your Own Friday Testnet

{% hint style="info" %}
Before setting up your node, make sure you have [install Friday binaries.](installation.md)
{% endhint %}

## Prepare

Follow the steps below to create genesis block file and set initial validator.

```bash
# run execution engine grpc server
./CasperLabs/execution-engine/target/release/casperlabs-engine-grpc-server -t 8 $HOME/.casperlabs/.casper-node.sock&

# init node
nodef init testnode --chain-id testnet

# copy execution engine chain configurations
cp ./x/executionlayer/resources/manifest.toml ~/.nodef/config

# create a wallet key
clif keys add elsa # select password
clif keys add anna # select password

# add genesis node
nodef add-genesis-account $(clif keys show elsa -a) 100000000stake
nodef add-genesis-account $(clif keys show anna -a) 100000000stake
nodef add-el-genesis-account elsa "1000000000000000000000000000" "1000000000000000000"
nodef add-el-genesis-account anna "1000000000000000000000000000" "1000000000000000000"
nodef load-chainspec ~/.nodef/config/manifest.toml

# apply default clif configure
clif config chain-id testnet
clif config output json
clif config indent true
clif config trust-node true

# prepare genesis status
nodef gentx --name elsa # insert password
nodef collect-gentxs
nodef validate-genesis
```

## Start Your Own Genesis Node

```bash
nodef start
```



**Congratulations!** Now Hdac testnet is on running. From now on, you may make a fun with Hdac token. You may proceed to next chapter.

