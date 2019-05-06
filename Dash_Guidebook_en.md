## What is Dash

[DASH](https://www.dash.org/):Instant transactions and micro-fees. Any amount, anytime, anywhere.


## Overview

Dash is based on Bitcoin and compatible with many key components of the Bitcoin ecosystem.


### Bitcoin's problems

- Long confirmation time
- Poor privacy
- ASIC
- Minning pool concentration


### Dash's solution

- InstantSend
- PrivateSend
- X11 hashing algorithm
- Two-tier incentivized network, known as the masternode network


### InstantSend

Once a quorum has been formed, the inputs of the transaction are locked to only be spendable in a specific transaction, a transaction lock takes about four seconds to be set currently on the network.If consensus is reached on a lock by the masternode network, all conflicting transactions or conflicting blocks would be rejected thereafter, unless they matched the exact transaction ID of the lock in place.


### X11 hashing algorithm

X11 is a widely used hashing algorithm, which takes a different approach, known as algorithm chaining. X11 consists of all 11 SHA3 contestants, each hash is calculated then submitted to the next algorithm in the chain. By utilizing multiple algorithms, the likelihood that an ASIC is created for the currency is minimal until a later part of its life cycle.


### PrivateSend

PrivateSend is an improved and extended version of the CoinJoin. In addition to the core concept of CoinJoin, we employ a series of improvements such as decentralization, strong anonymity by using a chaining approach, denominations and passive ahead-of-time mixing.

Process：

1. Masternodes receive a mixing request.
2. When two other people send similar requests, a mixing session begins.(The limit of each round is 1000 DASH, these denominations are 0.01 Dash, 0.1 DASH, 1 DASH and 10 DASH)
3. The masternode mixes up the inputs and instructs all three users' wallets to pay the now-transformed input back to themselves. Your wallet pays that denomination directly to itself, but in a different address (called a change address).
4. Repeat above processes a number of times.


### Masternode

- Masternode owners must have possession of 1000 DASH, which they prove by signing a message and broadcasting to the network.
- 45% of the block reward is allocated to pay the masternode network.
- A special deterministic algorithm is used to create a pseudo-random ordering of the masternodes.
- No one can control the entire network of masternodes
    - Trustless Quorums
    - Roles and Proof-Of-Service
- PrivateSend and InstantSend services.


### Specifications

- Name：DASH
- Release date: 11PM EST, 18th January 2014 / No premine
- Block generation: 2.5 minutes, block size: 2MB, approx 56 seconds each transaction
- 7.14% decrease in the number of coins generated per year
- Total coins: between 17.74M and 18.92M
- Rewards：miner 45%, masternodes 45%, the budget system 10%


### Wallets

- Wookey Wallet for Dash
- Dash Core Wallet
- Dash light Wallets
- Mobile Wallets（Android, iOS）
- Dash Copay Wallet（Merchant wallet）
- Cold wallets（Paper Wallet, Hardware Wallets）
- Other wallets（Web wallets, Text wallets, 3rd Party Wallets）

### Explorers

#### Mainnet

- [Dash CoinExplorer](https://www.coinexplorer.net/DASH)
- [Dashblockexplorer](https://dashblockexplorer.com/)
- [BitInfoCharts](https://bitinfocharts.com/dash/explorer/)
- [DashExplorer](https://explorer.dash.org/chain/Dash)


#### Testnet

- https://test.explorer.dash.org - by flare
- https://test.insight.dash.siampm.com - by thelazier
- https://test.explorer.dashninja.pl - by elbereth
- http://test.insight.masternode.io:3001 - by coingun
- http://insight.test.dash.crowdnode.io
- https://testnet-insight.dashevo.org/insight




## Developer

### Dash Core Wallet

Dash Core Wallet is the full official release of Dash, and supports all Dash features as they are released, including InstantSend and PrivateSend, as well as an RPC console and governance features. 

Source address：https://github.com/dashpay/dash

#### Compiling

Compiling based on Docker.

Pull the Ubuntu image `docker pull ubuntu`

Run Ubuntu image：`docker run -it --name dash_ubuntu ubuntu`

Installation for Ubuntu:

```
apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*
sudo apt-get update
apt-get install vim
apt-get install software-properties-common
```

Refer to [Compiling Dash Core - Linux](https://docs.dash.org/zh_CN/stable/developers/compiling.html)


Here is the image of docker:

```
~ docker images

REPOSITORY                                     TAG                 IMAGE ID            CREATED             SIZE
dash-ubuntu                                    latest              203426ab7b55        18 hours ago        3.38GB
ubuntu                                         latest              93fd78260bd1        4 weeks ago         86.2MB
```

> Tips: here we generate docker's image, in production environment we can directly use the binary file which is provided by Dash Core.

#### Command line arguments

##### Start Dash Core Daemon

```
root@wookey:~# dashd --daemon --testnet --server --rest --rpcuser=wookey --rpcpassword=wookey
Dash Core server starting
```

In this way, we start a test network and set the username and password for rpc. The username and password here must be set, or it won't work.

`dash-cli`


##### dash-cli

```
root@wookey:~# dash-cli --rpcuser=wookey --rpcpassword=wookey --testnet help
== Addressindex ==
getaddressbalance
getaddressdeltas
getaddressmempool
getaddresstxids
getaddressutxos

== Blockchain ==
getbestblockhash
getblock "blockhash" ( verbose )
getblockchaininfo
getblockcount
getblockhash height
getblockhashes timestamp
.....
```

Then we get some help commands.

Examples for dash-cli：

```
dash-cli --rpcuser=wookey --rpcpassword=wookey --testnet getblockchaininfo
dash-cli --rpcuser=wookey --rpcpassword=wookey --testnet getblockcount
dash-cli --rpcuser=wookey --rpcpassword=wookey --testnet stop 
```

Here, run dash-cli command must add `--rpcuser=wookey --rpcpassword=wookey --testnet`, otherwise you have setup in `dash.conf`.

For examples：
```
root@wookey:~/.dashcore# cat dash.conf
rpcuser=wookey
rpcpassword=********
```

Refer to this document: 
(https://docs.dash.org/zh_CN/stable/wallets/dashcore/cmd-rpc.html#command-line-arguments)


### Dashd for Docker

Dash Core provides a image

### Quick Start

Docker pull
```
docker pull dashpay/dashd
```

Volume create
```
docker volume create --name=dashd-data
```

Run
```
 docker run -v dashd-data:/dash --name=dashd-node -d \
     -p 9999:9999 \
     -p 127.0.0.1:9998:9998 \
     dashpay/dashd
```

Is it running
```
docker ps
```

Show logs
```
docker logs -f dashd-node
```

Into the docker
```
docker exec -it dashd-node bash -l
```

Exec dash-cli command
```
dash-cli getinfo
```

> Tips: Network connection may takes 20 minutes, before block sync.

### Run testnet

```
docker run -v dashd-data:/dash --name=dashd-node -d \
  --env TESTNET=1 \
  -p 9999:9999 \
  -p 127.0.0.1:9998:9998 \
  dashpay/dashd
```



### Block sync

It takes such a long time...

Mainnet:

```
2018-12-20 08:18:53 Pre-allocating up to position 0x100000 in rev00000.dat
2018-12-20 08:18:53 UpdateTip: new best=000007d91d1254d60e2dd1ae580383070a4ddffa4c64c2eeb4a2f9ecc0414343 height=1 version=0x00000002 log2_work=21.00002201 tx=2 date='2014-01-19 03:54:41' progress=0.000000 cache=0.0MiB(1txo)
2018-12-20 08:18:53 ProcessNewBlock : ACCEPTED
2018-12-20 08:18:53 UpdateTip: new best=00000bafcc571ece7c5c436f887547ef41b574e10ef7cc6937873a74ef1efeae height=2 version=0x00000002 log2_work=21.58498451 tx=3 date='2014-01-19 03:54:46' progress=0.000000 cache=0.0MiB(2txo)
2018-12-20 08:18:53 ProcessNewBlock : ACCEPTED
2018-12-20 08:18:53 UpdateTip: new best=00000269705556d97bc6d2e1a02c9b9031b2415192ed1ef820f384ce0d8de0ff height=3 version=0x00000002 log2_work=22.00002201 tx=4 date='2014-01-19 03:55:01' progress=0.000001 cache=0.0MiB(3txo)
2018-12-20 08:18:53 ProcessNewBlock : ACCEPTED
2018-12-20 08:18:53 UpdateTip: new best=000006fce46cbe25996ed9a11092622cccdc37bd423992f44d2165b7e89b67e4 height=4 version=0x00000002 log2_work=22.32195011 tx=5 date='2014-01-19 03:55:10' progress=0.000001 cache=0.0MiB(4txo)
2018-12-20 08:18:53 ProcessNewBlock : ACCEPTED
2018-12-20 08:18:53 UpdateTip: new best=0000097f56152f7f68faaccc0426725e0b6c507170432d48b685403637ba10a2 height=5 version=0x00000002 log2_work=22.58498451 tx=6 date='2014-01-19 03:55:13' progress=0.000001 cache=0.0MiB(5txo)
2018-12-20 08:18:53 ProcessNewBlock : ACCEPTED
2018-12-20 08:18:53 UpdateTip: new best=00000c1fa7a288cc75fe47ee2ca5f249b2cfc83e7ddbf1657e18ab183f88b668 height=6 version=0x00000002 log2_work=22.80737694 tx=7 date='2014-01-19 03:55:22' progress=0.000001 cache=0.0MiB(6txo)
2018-12-20 08:18:53 ProcessNewBlock : ACCEPTED
2018-12-20 08:18:53 UpdateTip: new best=00000fba5440df1090d1b37dc42581b5abb909a0b89b4b6da6245236e6ccf434 height=7 version=0x00000002 log2_work=23.00002201 tx=8 date='2014-01-19 03:55:23' progress=0.000001 cache=0.0MiB(7txo)

...
```

Testnet:

```
2018-12-20 09:09:40 Pre-allocating up to position 0x100000 in rev00000.dat
2018-12-20 09:09:40 UpdateTip: new best=0000047d24635e347be3aaaeb66c26be94901a2f962feccd4f95090191f208c1 height=1 version=0x00000002 log2_work=21.00001169 tx=2 date='2014-04-28 19:19:31' progress=0.000000 cache=0.0MiB(1txo)
2018-12-20 09:09:40 ProcessNewBlock : ACCEPTED
2018-12-20 09:09:40 UpdateTip: new best=00000c6264fab4ba2d23990396f42a76aa4822f03cbc7634b79f4dfea36fccc2 height=2 version=0x00000002 log2_work=21.58497764 tx=3 date='2014-04-28 19:19:32' progress=0.000001 cache=0.0MiB(2txo)
2018-12-20 09:09:40 ProcessNewBlock : ACCEPTED
2018-12-20 09:09:40 UpdateTip: new best=0000057d5c945acbe476bc17bbbaeb2fc1c1b18673e7582c48ac04af61f4d811 height=3 version=0x00000002 log2_work=22.00001685 tx=4 date='2014-04-28 19:19:34' progress=0.000001 cache=0.0MiB(3txo)
2018-12-20 09:09:40 ProcessNewBlock : ACCEPTED
2018-12-20 09:09:40 UpdateTip: new best=000002258bd58bf4cdcde282abc030437c103dbb12d2a7dbc978d07bcf386b42 height=4 version=0x00000002 log2_work=22.32194598 tx=5 date='2014-04-28 19:19:42' progress=0.000001 cache=0.0MiB(4txo)
2018-12-20 09:09:40 ProcessNewBlock : ACCEPTED
2018-12-20 09:09:40 UpdateTip: new best=00000b1406007f9148c4c173e2314437a315b79b5b79e73ed534a2e8b18a43fc height=5 version=0x00000002 log2_work=22.58498107 tx=6 date='2014-04-28 19:19:44' progress=0.000001 cache=0.0MiB(5txo)
2018-12-20 09:09:40 ProcessNewBlock : ACCEPTED
2018-12-20 09:09:40 UpdateTip: new best=00000d93de4b31ec550d324876ee7c502a073911972d26c2c990b94d9d5b5a44 height=6 version=0x00000002 log2_work=22.80737399 tx=7 date='2014-04-28 19:19:47' progress=0.000001 cache=0.0MiB(6txo)

...
```


## Reference and study

- [Dash Docs](https://docs.dash.org)
- [Developer Documentation](https://dash-docs.github.io/en/)