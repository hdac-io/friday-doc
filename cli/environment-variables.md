# Environment Variables

By default, uppercase environment variables with the following prefixes will replace lowercase command-line flags:

* `FR` \(for Friday flags\)
* `TM` \(for Tendermint flags\)
* `BC` \(for democli or basecli flags\)

For example, the environment variable `FR_CHAIN_ID` will map to the command line flag `--chain-id`. Note that while explicit command-line flags will take precedence over environment variables, environment variables will take precedence over any of your configuration files. For this reason, it is imperative that you lock down your environment such that any critical parameters are defined as flags on the CLI or prevent modification of any environment variables.

