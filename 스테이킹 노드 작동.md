## Overview

Starts with EVT 4.x versions, staking is supported and every stake holder can take part in. For more detail about staking design in everiToken, you can refer to: [Staking](https://www.everitoken.io/developers/key_concepts/staking).

Before creating the validator you need to setup staking TestNet node(`evtd`) and wallet node(`evtwd`) locally.

Here're the brief instruments for creating one Testnet staking node without postgres support(complex querying). And be sure to install docker environment properly. Also check this [document](https://www.everitoken.io/developers/apis,_sdks_and_tools/docker_reference) to get and set up docker_ops.py.

> `Postgres` is an optional module to support complex queries in everiToken blockchain and it's not necessary for a node. Refer this [document](https://www.everitoken.io/developers/apis,_sdks_and_tools/docker_reference) to know more.

### 1. Setup docker network
```bash
./docker_ops.py network init
```

### 2. Setup evtd container
```bash
docker pull everitoken/evt-staking:latest
 
./docker_ops.py evtd init

./docker_ops.py evtd create -t staking --http-port=8888 --p2p-port=7888 --host=0.0.0.0 -- --http-validate-host=false --verbose-http-errors --plugin=evt::evt_link_plugin --p2p-peer-address=t-hk1.evtn.us:9899 --delete-all-blocks

./docker_ops.py evtd start
```

Use `./docker_ops.py evtd logs` to check logs of evtd container and it should be syncing blocks now. It's need to wait for syncing all the blocks before staking.

> Use the `latency` to check if it catches the latest block (less than 500ms).

### 3. Setup evtwd container
```bash
./docker_ops.py evtwd init

./docker_ops.py evtwd create -t staking

./docker_ops.py evtwd start
```

And then create a new wallet
```bash
./docker_ops.py evtc wallet create
```
> Be sure to remember the password returned and you cannot open the wallet without that password.

Now create you key pair:
``` bash
./docker_ops.py evtc wallet create_key
```

And get the private key for that public key:
``` bash
./docker_ops.py evtc wallet private_keys
```

### 4. Register validator
Before following steps, you need get some evt first:
```bash
./docker_ops.py evtc wallet import 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3
./docker_ops.py evtc http://evtd:8888 fungible issue @1 "1000000.00000 S#1" -p @0
```

Not it's time to register a new validator! Use the following command to register a new validator called `vt` with the commission of `30%`.

> Be sure to check the node is catched up latest blocks, otherwise you cannot register validator.

``` bash
./docker_ops.py evtc -u http://evtd:8888 validator create tv @1 @1 0.3 -p @1
```
> `@0` is a shorthand refers the first public key listed in `./docker_ops.py evtc wallet keys`. Similar `@1` refers the second one.

> You can also use `./docker_ops.py evtc validator create -h` to see the detail of the that command

Use the following command to query your newly created validator.

```bash
./docker_ops.py evtc -u http://evtd:8888 get validator tv
```

Now you can invite other people to stake tokens into your validator. Or you can stake yourself.

```bash
./docker_ops.py evtc -u http://evtd:8888 assets stake @1 tv "100000.00000 S#1" active 0 -p @1
```

Above will stake 100 EVTs with `active` type into validator `tv`. And because the initial NAV(net asset value) of each validator is `1 EVT`. So you will get 100 active shares of `tv` validator.

If you want to get an additional ROI for your staking you can choose to stake with `fixed` type. The following command is to stake 100 EVTs with 30 days locked.
```bash
./docker_ops.py evtc -u http://evtd:8888 assets stake @1 tv "100000.00000 S#1" fixed 30 -p @1
```

Then you use following command to query all the shares you've staked.
```bash
./docker_ops.py evtc -u http://evtd:8888 get shares @1
```

### 5. Get staking rewards
Once you've created a validator and has 100'000 shares you can start receiving staking rewards and it's very ease to do.

Stop evtd container first
```bash
./docker_ops.py evtd stop
```

And create another one with staking parameters
```bash
./docker_ops.py evtd create -t staking --http-port=8888 --p2p-port=7888 --host=0.0.0.0 -- --http-validate-host=false --verbose-http-errors --plugin=evt::evt_link_plugin --p2p-peer-address=t-hk1.evtn.us:9899 --plugin=evt::staking_plugin --staking-validator=tv --staking-payer=[PUBKEY] --staking-signature-provider="[PUBKEY]=KEY:[PRIVKEY]"
```

> Replace `[PUBKEY]` and `[PRIVKEY]` with your key pair created in step 3.

Congrats! Now you will receive staking rewards every 76.8 minutes.
