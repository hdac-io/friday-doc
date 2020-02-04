# Deploy Your Own Friday Testnet

[![Travis](https://travis-ci.com/hdac-io/friday.svg?token=bhU3g7FdixBp5h3M2its&branch=master)](https://travis-ci.com/hdac-io/friday/branches) [![codecov](https://codecov.io/gh/hdac-io/friday/branch/master/graph/badge.svg?token=hQEgzmULjh)](https://codecov.io/gh/hdac-io/friday)

{% hint style="info" %}
Before setting up your node, make sure you've [install Friday binaries.](installation.md)
{% endhint %}

## Setup

**Setup a genesis status and run a genesis node**  
Follow the steps below to create genesis block file and set initial validator.

```bash
# run execution engine grpc server
./CasperLabs/execution-engine/target/release/casperlabs-engine-grpc-server $HOME/.casperlabs/.casper-node.sock

# init node
nodef init  --chain-id testnet

# copy execution engine chain configurations
cp ./x/executionlayer/resources/manifest.toml ~/.nodef/config

# create a wallet key
clif keys add elsa # select password
clif keys add anna # select password

# add genesis node
nodef add-genesis-account $(clif keys show elsa -a) 5000000000000dummy,100000000stake
nodef add-genesis-account $(clif keys show anna -a) 5000000000000dummy,100000000stake
nodef add-el-genesis-account $(clif keys show elsa -a) "5000000000000" "100000000"
nodef add-el-genesis-account $(clif keys show anna -a) "5000000000000" "100000000"
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

## Genesis node start

```text
nodef start
```

* Note your genesis node's ID and genesis file
* You can get your node ID using clif

```text
clif status | grep \"id\"
```

* Your genesis file exists \`~/.nodef/config/genesis.json\`

**Congratulations!** Now Hdac testnet is on running. From now on, you may make a fun with Hdac token. You may proceed to next chapter.

