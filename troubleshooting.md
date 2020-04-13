# Troubleshooting

## Sporadically occurring error while the network is running

```text
E[2020-04-08|04:55:35.269] Error dialing seed                           module=p2p
err="connection with a42bad638bbd8fdcf1dd0a3cfe77748267fb180a@15.165.67.139:26656 has been established or dialed" seed=a42bad638bbd8fdcf1dd0a3cfe77748267fb180a@15.165.67.139:26656
E[2020-04-08|04:55:35.269] Couldn't connect to any seeds                module=p2p 
```

* If block still proceeds, you may **ignore** the error.
* It occurs when you cannot connect to seed node, the node finds to other node to get sync.

## Block synching stops in initial sync

```text
E[2020-04-08|04:55:35.269] Error dialing seed                           module=p2p
err="connection with a42bad638bbd8fdcf1dd0a3cfe77748267fb180a@15.165.67.139:26656 has been established or dialed" seed=a42bad638bbd8fdcf1dd0a3cfe77748267fb180a@15.165.67.139:26656
E[2020-04-08|04:55:35.269] Couldn't connect to any seeds
```

* It occurs when all seed nodes are down. Please contract in [Discord](https://discord.gg/wT9Dhtd) channel or one of validators.

## Node dies With wrong AppHash error

```text
() panic: Failed to process committed block (59819:D183E54C94140D167F5CC299A5762EE6586A8769494253521A3C97374995F9DF): Wrong Block.Header.AppHash.  Expected 6132538CF42759823104EC06EB43D6BCF9A6D8270C8544A85180C8ED892AB5B5, got 709C8C0841616EBE1E3A19ADC5AF4E81C314661381CB64545D994C29019121E4

goroutine 102 [running]:
github.com/hdac-io/tendermint/blockchain/v0.(*BlockchainReactor).poolRoutine(0xc000ae41a0)
        /root/work/pkg/mod/github.com/hdac-io/tendermint@v0.0.0-20200102054348-9a58be0e600b/blockchain/v0/reactor.go:344 +0x12ff
created by github.com/hdac-io/tendermint/blockchain/v0.(*BlockchainReactor).OnStart
        /root/work/pkg/mod/github.com/hdac-io/tendermint@v0.0.0-20200102054348-9a58be0e600b/blockchain/v0/reactor.go:118 +0x84 ()
```

* Please check your gRPC execution engine \(`casperlabs-engine-grpc-server`\)
* Kill both of processes and reset by `nodef unsafe-reset-all` 
* You should run `casperlabs-engine-grpc-server`first, and run `nodef start` later

