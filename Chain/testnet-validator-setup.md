# Join the Public Testnet

> This chain is a test-net
> GENESIS PUBLISHED
> PEERS PUBLISHED


## Hardware Requirements
* **Minimal**
    * 1 GB RAM
    * 25 GB HDD
    * 1.4 GHz CPU
* **Recommended**
    * 2 GB RAM
    * 100 GB HDD
    * 2.0 GHz x2 CPU

## Operating System
* Linux/Windows/MacOS(x86)
* **Recommended**
    * Linux(x86_64)

## Installation Steps
>Prerequisite: go1.15+ required. [ref](https://golang.org/doc/install)

>Prerequisite: git. [ref](https://github.com/git/git)

>Optional requirement: GNU make. [ref](https://www.gnu.org/software/make/manual/html_node/index.html)

* Clone git repository
```shell
git clone https://github.com/persistenceOne/persistenceCore.git
```
* Checkout release tag
```shell
git fetch --tags
git checkout v3.1.1
```
* Install
```shell
cd persistenceCore
make all
```

### Generate keys

`persistenceCore keys add [key_name]`

or

`persistenceCore keys add [key_name] --recover` to regenerate keys with your [BIP39](https://github.com/bitcoin/bips/tree/master/bip-0039) mnemonic


## Validator setup

### Before genesis: NOW CLOSED

* [Install](#installation-steps) persistence core application
* Initialize node
```shell
persistenceCore init {{NODE_NAME}} --chain-id test-core-1
persistenceCore add-genesis-account {{KEY_NAME}} 100000000000000uxprt
persistenceCore gentx {{KEY_NAME}} 10000000uxprt \
--chain-id test-core-1 \
--moniker="{{VALIDATOR_NAME}}" \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.07 \
--details="XXXXXXXX" \
--security-contact="XXXXXXXX" \
--website="XXXXXXXX"
```
* Copy the contents of `${HOME}/.persistenceCore/config/gentx/gentx-XXXXXXXX.json`.
* Fork the [repository](https://github.com/persistenceOne/genesisTransactions)
* Create a file `gentx-{{VALIDATOR_NAME}}.json` under the test-core-1/gentxs folder in the forked repo, paste the copied text into the file. Find reference file gentx-examplexxxxxxxx.json in the same folder.
* Run `persistenceCore tendermint show-node-id` and copy your nodeID.
* Run `ifconfig` or `curl ipinfo.io/ip` and copy your publicly reachable IP address.
* Create a file `peers-{{VALIDATOR_NAME}}.json` under the test-core-1/peers folder in the forked repo, paste the copied text from the last two steps into the file. Find reference file peers-examplexxxxxxxx.json in the same folder.
* Create a Pull Request to the `master` branch of the [repository](https://github.com/persistenceOne/genesisTransactions)
>**NOTE:** the Pull Request will be merged by the maintainers to confirm the inclusion of the validator at the genesis. The final genesis file will be published under the file test-core-1/final_genesis.json.
* Replace the contents of your `${HOME}/.persistenceCore/config/genesis.json` with that of test-core-1/final_genesis.json.
* Add `persistent_peers` or `seeds` in `${HOME}/.persistenceCore/config/config.toml` from test-core-1/final_peers.json.
* Start node
```shell
persistenceCore start
```

### Post genesis

* [Install](#installation-steps) persistence core application
* Initialize node
```shell
persistenceCore init {{NODE_NAME}}
```
* Replace the contents of your `${HOME}/.persistenceCore/config/genesis.json` with that of test-core-1/final_genesis.json from the `master` branch of [repository](https://github.com/persistenceOne/genesisTransactions).
* Add `persistent_peers` or `seeds` in `${HOME}/.persistenceCore/config/config.toml` from test-core-1/final_peers.json from the `master` branch of [repository](https://github.com/persistenceOne/genesisTransactions).
* Start node
```shell
persistenceCore start
```
* Acquire $XPRT by sending a message to the community [telegram](https://t.me/PersistenceOneChat).
* Run `persistenceCore tendermint show-validator` and copy your consensus public key.
* Send a create-validator transaction
```
persistenceCore tx staking create-validator \
--from {{KEY_NAME}} \
--amount XXXXXXXXuxprt \
--pubkey persistencevalconspubXXXXXXXX
--chain-id test-core-1 \
--moniker="{{VALIDATOR_NAME}}" \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.07 \
--min-self-delegation="1" \
--details="XXXXXXXX" \
--security-contact="XXXXXXXX" \
--website="XXXXXXXX"
```
## Version
This chain is currently running on persistenceCore [v3.1.1](https://github.com/persistenceOne/persistenceCore/releases/tag/v3.1.1)
Commit Hash: `d557c8f98f2440f9fe82dcf5a30b05b714dee25a`
>Note: If your node is running on an older version of the application, please update it to this version at the earliest to avoid being exposed to security vulnerabilities /defects.

## Binary
The binary can be downloaded from [here](https://github.com/persistenceOne/persistenceCore/releases/tag/v3.1.1).

## Explorer
The explorer for this chain is hosted [here](test-core-1.explorer.persistence.one)

## Wallet
The wallet application for this chain is hosted [here](test-core-1.wallet.persistence.one)
