## 什么是达世币

[达世币](https://www.dash.org/)：一个支持即时交易，专注于保护隐私的数字货币。


## 概述

DASH是以比特币为基础，它的代码是fork了bitcoin的。

### 比特币遇到的问题

- 确认时间长
- 只提供低层次的隐私保护
- ASIC矿机导致的中心化问题
- 矿池（存在51%攻击的可能）

### 解决方案

- 使用InstantSend进行即时交易
- PrivateSend（匿名发送）服务
- X11算法
- 推出一个拥有两层（矿工和Masternode）的奖励性网络

### 即时交易

InstantSend系统可以先将款项锁在交易中。当主节点网络能够达成共识，便能自动确认交易，否则则经由正常挖矿的渠道以确认交易。如果交易与过往交易发生冲突，交易将被拒绝。

### X11算法

X11由11轮SHA3算法组成，每轮哈希计算的结果都被提交到区块链的下一轮计算去。使用多轮算法，可以减少专门为数字货币挖矿设计的ASIC使用的概率。

这些算法是：blake、bmw、groestl、jh、keccak、skein、luffa、cubehash、shavite、simd、echo

### 匿名支付

PrivateSend是一个基于CoinJoin的硬币混合服务。它将多方的资金合并在一起对外发送。匿名支付的交易都会取整数，所以所有的交易输入都会被使用。任何超出部分都会被用于交易手续费。

原理：

1. 主节点接收到混币请求。
2. 当有另外2个人请求参与混币的时候，主节点将进行混币（每轮的混币限制为1000DASH，面额是0.01达世币，0.1达世币，1达世币和10达世币）。
3. 主节点将币混在一起，并给这三个用户的钱包指令，让它们把这些现在已经转化了的币回付给自己。你的钱包将这些面额直接付给自己，但是地址已经改变（称为变更地址）。
4. 多轮以上流程。


### 主节点

- 主节点拥有者必须持有1000个达世币，经电子签名后向网络广播后认证。
- 45%的区块奖励被分配支付给主节点网络。
- 使用特定的确定算法创建主节点的伪随机排序。
- 没有一人能控制整个主节点网络。
    - 非信任制的Quorum(法定人数)。
    - 服务量证明机制。
- PrivateSend（匿名发送）和 InstantSend（即时支付）服务。


### 参数

- 代币符号：DASH
- 原始块产生于2014年1月18日，东部时间晚11点
- 区块时间2.6分钟, 区块大小2MB, 约56笔交易每秒
- 每年区块奖励减产率7.14%
- 总发行量在17.74M到18.92M之间
- 激励机制：矿工45%、主节点45%、达世自治组织10%

### 钱包

- Wookey钱包达世版本
- Dash Core钱包
- 达世币轻钱包
- 达世币移动端钱包（安卓、iOS）
- 达世币Copay钱包（商家钱包）
- 冷钱包（纸钱包、硬件钱包）
- 其他钱包（网页钱包、短信钱包、第三方钱包）

### 浏览器

#### 主网

- [Dash CoinExplorer](https://www.coinexplorer.net/DASH)
- [Dashblockexplorer](https://dashblockexplorer.com/)
- [BitInfoCharts](https://bitinfocharts.com/dash/explorer/)
- [DashExplorer](https://explorer.dash.org/chain/Dash)

#### 测试网络

- https://test.explorer.dash.org - by flare
- https://test.insight.dash.siampm.com - by thelazier
- https://test.explorer.dashninja.pl - by elbereth
- http://test.insight.masternode.io:3001 - by coingun
- http://insight.test.dash.crowdnode.io
- https://testnet-insight.dashevo.org/insight


## 开发者

### Dash Core钱包

Dash Core钱包由达世币官方全权发行，支持达世币的所有功能，包括即时支付和匿名支付，以及远程过程调用RPC控制台和管理功能。

源码地址：https://github.com/dashpay/dash

#### 源码编译

基于Docker进行编译。

拉取Ubuntu镜像：`docker pull ubuntu`

运行Ubuntu镜像：`docker run -it --name dash_ubuntu ubuntu`

对Ubuntu进行一些基础安装：

```
apt-get update && apt-get install -y sudo && rm -rf /var/lib/apt/lists/*
sudo apt-get update
apt-get install vim
apt-get install software-properties-common
```

参考[编译Dash Core - Linux](https://docs.dash.org/zh_CN/stable/developers/compiling.html)编译代码。


用docker的commit命令，打成image后的结果：

```
~ docker images

REPOSITORY                                     TAG                 IMAGE ID            CREATED             SIZE
dash-ubuntu                                    latest              203426ab7b55        18 hours ago        3.38GB
ubuntu                                         latest              93fd78260bd1        4 weeks ago         86.2MB
```

> Tips: 这里是生成了docker的镜像，生产环境中可以直接使用官方打好的二进制文件。

#### 命令和参数

##### 启动dash服务

```
root@wookey:~# dashd --daemon --testnet --server --rest --rpcuser=wookey --rpcpassword=wookey
Dash Core server starting
```
这样，就启动了一个测试网络，并设置了rpc的用户名密码。这里的用户名密码必须设置，不然运行不了`dash-cli`。


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
这样就获得帮助命令。

dash-cli的一些示例：

```
dash-cli --rpcuser=wookey --rpcpassword=wookey --testnet getblockchaininfo
dash-cli --rpcuser=wookey --rpcpassword=wookey --testnet getblockcount
dash-cli --rpcuser=wookey --rpcpassword=wookey --testnet stop 
```

这里，运行dash-cli命令必须添加`--rpcuser=wookey --rpcpassword=wookey --testnet`，除非在`dash.conf`配置了。

例如：
```
root@wookey:~/.dashcore# cat dash.conf
rpcuser=wookey
rpcpassword=********
```

参考官方的文档：[命令和参数](https://docs.dash.org/zh_CN/stable/wallets/dashcore/cmd-rpc.html#command-line-arguments)


### Dashd for Docker

Dash Core官方提供了Docker镜像。

### Quick Start

拉取docker镜像
```
docker pull dashpay/dashd
```

创建Docker数据卷，用来保存区块链的数据
```
docker volume create --name=dashd-data
```

运行镜像
```
 docker run -v dashd-data:/dash --name=dashd-node -d \
     -p 9999:9999 \
     -p 127.0.0.1:9998:9998 \
     dashpay/dashd
```

验证正在运行的容器，dashd节点正在下载区块链
```
docker ps
```

查看输出
```
docker logs -f dashd-node
```

进入运行中的容器
```
docker exec -it dashd-node bash -l
```

执行dash-cli命令
```
dash-cli getinfo
```

> Tips: 不知道是否是网络的原因还是啥的，运行20多分钟后才开始同步节点。

### 运行测试网络

```
docker run -v dashd-data:/dash --name=dashd-node -d \
  --env TESTNET=1 \
  -p 9999:9999 \
  -p 127.0.0.1:9998:9998 \
  dashpay/dashd
```



### 区块的同步

为什么会有这个模块？因为很久才开始同步区块（主网和测试网络都一样），久到我都开始怀疑人生了......

主网的:

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

测试网络的:

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


## 参考与学习

- [Dash Docs](https://docs.dash.org)
- [Developer Documentation](https://dash-docs.github.io/en/)