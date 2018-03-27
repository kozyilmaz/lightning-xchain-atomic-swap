# Lightning
[Lightning Installation Guide](https://github.com/lightningnetwork/lnd/blob/master/docs/INSTALL.md) is used to build the dependencies (`Go`), currency daemons (`btcd` and `ltcd`) and the actual `lnd` daemon.  

## Install Lightning Dependencies
Download latest package from [offical Go repository](https://golang.org/dl/) and uncompress to `/usr/local`
```shell
$ sudo tar -C /usr/local -xzf go1.10.linux-amd64.tar.gz
```

Add the following lines to the end of `$HOME/.bashrc`
```shell
# add Go paths
export GOPATH=$HOME/go
export PATH=/usr/local/go/bin:$GOPATH/bin:$PATH
```

Install `dep`
```shell
$ go get -u github.com/golang/dep/cmd/dep
```

## Build Lightning Daemon

#### Official `lnd`
Build and install official `lnd` daemon version `v0.4-beta`
```shell
$ git clone -b v0.4-beta https://github.com/lightningnetwork/lnd $GOPATH/src/github.com/lightningnetwork/lnd
$ cd $GOPATH/src/github.com/lightningnetwork/lnd
$ dep ensure
$ go install . ./cmd/...
```

## Setup Blockchain Clients

#### for Bitcoin
Launch `bitcoind` or `bitcoin-qt`
```shell
$ $HOME/xu/root/bin/bitcoin-qt -datadir=$HOME/xu/bitcoin -testnet -server -daemon -rpcuser=kek -rpcpassword=kek -zmqpubrawblock=tcp://127.0.0.1:28332 -zmqpubrawtx=tcp://127.0.0.1:28332
```

Query via JSON-RPC
```shell
$ $HOME/xu/root/bin/bitcoin-cli -testnet -rpcuser=kek -rpcpassword=kek getblockcount
```

#### for Litecoin
Launch `bitcoind` or `bitcoin-qt`
```shell
$ $HOME/xu/root/bin/litecoin-qt -datadir=$HOME/xu/litecoin -testnet -server -daemon -rpcuser=kek -rpcpassword=kek -zmqpubrawblock=tcp://127.0.0.1:28333 -zmqpubrawtx=tcp://127.0.0.1:28333
```

Query via JSON-RPC
```shell
$ $HOME/xu/root/bin/litecoin-cli -testnet -rpcuser=kek -rpcpassword=kek getblockcount
```


## Running Lightning Daemon
After testnet sync is done, following command can be use to launch `lnd` for testing  
Lightning data is stored in `$HOME/.lnd` by default
```shell
$ lnd --rpclisten=localhost:10001 --listen=localhost:10011 --restlisten=localhost:8001 --datadir=test_data --logdir=test_log --debuglevel=info --nobootstrap --no-macaroons --bitcoin.active --bitcoin.testnet --bitcoin.node=bitcoind --bitcoind.dir=$HOME/xu/bitcoin --bitcoind.rpcuser=kek --bitcoind.rpcpass=kek --bitcoind.zmqpath=tcp://127.0.0.1:28332 --litecoin.active --litecoin.testnet --litecoin.node=litecoind --litecoind.dir=$HOME/xu/litecoin --litecoind.rpcuser=kek --litecoind.rpcpass=kek --litecoind.zmqpath=tcp://127.0.0.1:28333
```
