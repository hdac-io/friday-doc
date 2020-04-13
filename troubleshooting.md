# Troubleshooting

## If there is nothing what I want in below...

* Occurs & dies
  * Copy the error stack from `clif`, `nodef`, and `casperlabs-engine-grpc-server`
  * And paste it to our [issue tracker](https://github.com/hdac-io/friday/issues)
  * Or \#validator of [Discord](https://discord.gg/wT9Dhtd)
* Occurs & freezes
  * Copy the error stack, recent 100 blocks, and the current block number of `nodef`, and `casperlabs-engine-grpc-server`
  * And paste it to our [issue tracker](https://github.com/hdac-io/friday/issues)
  * Or \#validator of [Discord](https://discord.gg/wT9Dhtd)
  * And backup `~/.nodef` directory. Other information could be needed for further investigation

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

## Node dies with wrong AppHash error

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
* If AppHash error is reproduced even when the grpc execution engine is running, please register as an issue to github.

## `clif` doesn't returns JSON

```text
hdac@10-23-133-199:~$ clif keys show bbrryyaa --bech cons
- name: bbrryyaa
  type: local
  address: fridayvalcons1q8xwx45zeyfy4pkgnz0nxy3px866x2x7fc5v98
  pubkey: fridayvalconspub1addwnpepq0w2gae0l0g8t4twsp336x7k04u06w787njmxxfs0splsksm3kz2s5ttp7c
  mnemonic: ""
  threshold: 0
  pubkeys: []
```

* `clif config json true`
* `clif config indent true`

## Querying validator list doesn't work

```text
hdac@10-23-133-199:~$ clif query tendermint-validator-set
panic: runtime error: invalid memory address or nil pointer dereference
[signal SIGSEGV: segmentation violation code=0x1 addr=0x20 pc=0xc9bdb5]

goroutine 1 [running]:
github.com/hdac-io/tendermint/lite/proxy.GetCertifiedCommit(0x363c5, 0x140e400, 0xc0001198f0, 0x0, 0x0, 0xa, 0xc00022ca80, 0xedd860, 0xc0002401a0)
	/home/shawnjzlee/.gvm/pkgsets/go1.14/global/pkg/mod/github.com/hdac-io/tendermint@v0.0.0-20200102054348-9a58be0e600b/lite/proxy/query.go:140 +0x105
github.com/hdac-io/friday/client/context.CLIContext.Verify(0xc000187730, 0x140e400, 0xc0001198f0, 0x0, 0x0, 0x13d7f40, 0xc000010018, 0x109fe5e, 0x4, 0x0, ...)
	/home/shawnjzlee/Desktop/friday/client/context/query.go:116 +0x66
github.com/hdac-io/friday/client/rpc.GetValidators(0xc000187730, 0x140e400, 0xc0001198f0, 0x0, 0x0, 0x13d7f40, 0xc000010018, 0x109fe5e, 0x4, 0x0, ...)
	/home/shawnjzlee/Desktop/friday/client/rpc/validators.go:130 +0x33c
github.com/hdac-io/friday/client/rpc.ValidatorCommand.func1(0xc000b23b80, 0x1ddde40, 0x0, 0x0, 0x0, 0x0)
	/home/shawnjzlee/Desktop/friday/client/rpc/validators.go:48 +0xf5
github.com/spf13/cobra.(*Command).execute(0xc000b23b80, 0x1ddde40, 0x0, 0x0, 0xc000b23b80, 0x1ddde40)
	/home/shawnjzlee/.gvm/pkgsets/go1.14/global/pkg/mod/github.com/spf13/cobra@v0.0.5/command.go:826 +0x453
github.com/spf13/cobra.(*Command).ExecuteC(0xc0001ae280, 0x89e770, 0xc0001ae280, 0x109ee63)
	/home/shawnjzlee/.gvm/pkgsets/go1.14/global/pkg/mod/github.com/spf13/cobra@v0.0.5/command.go:914 +0x2fb
github.com/spf13/cobra.(*Command).Execute(...)
	/home/shawnjzlee/.gvm/pkgsets/go1.14/global/pkg/mod/github.com/spf13/cobra@v0.0.5/command.go:864
github.com/hdac-io/tendermint/libs/cli.Executor.Execute(0xc0001ae280, 0x1299328, 0x2, 0xc000b25580)
	/home/shawnjzlee/.gvm/pkgsets/go1.14/global/pkg/mod/github.com/hdac-io/tendermint@v0.0.0-20200102054348-9a58be0e600b/libs/cli/setup.go:89 +0x3c
main.main()
	/home/shawnjzlee/Desktop/friday/cmd/clif/main.go:79 +0x613

```

* Reported as [https://github.com/hdac-io/friday/issues/100](https://github.com/hdac-io/friday/issues/100)
* Workaround: `clif config trust-node true`

