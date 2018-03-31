# Lightning
[Lightning Installation Guide](https://github.com/lightningnetwork/lnd/blob/master/docs/INSTALL.md) is used to build the dependencies (`Go`), currency daemons (`btcd` and `ltcd`) and the actual `lnd` daemon.  

## Install Lightning Dependencies
Download 1.9.5 package from [offical Go repository](https://golang.org/dl/) and uncompress to `/usr/local`
```shell
$ sudo tar -C /usr/local -xzf go1.9.5.linux-amd64.tar.gz
```

Add the following lines to the end of `$HOME/.bashrc`
```shell
# add Go paths
export GOPATH=$HOME/go
export PATH=/usr/local/go/bin:$GOPATH/bin:$PATH
```

## Build Lightning Daemon

Official `lnd` does not support cross chain swaps, so a specific fork/branch should be built

#### Cross-chain swap enabled `lnd`
Install `Glide`
```shell
$ go get -u github.com/Masterminds/glide
```

To build and install xchain-swap enabled experimental `lnd` daemon referenced [here](https://blog.lightning.engineering/announcement/2017/11/16/ln-swap.html)
```shell
$ git clone -b swapz https://github.com/cfromknecht/lnd.git $GOPATH/src/github.com/lightningnetwork/lnd
$ cd $GOPATH/src/github.com/lightningnetwork/lnd
$ glide install
$ go install . ./cmd/...
```

#### Bitcoin full node implementation `btcd`
Build and install `btcd` Bitcoin full node implementation
```shell
$ git clone https://github.com/roasbeef/btcd $GOPATH/src/github.com/roasbeef/btcd
$ cd $GOPATH/src/github.com/roasbeef/btcd
$ glide install
$ go install . ./cmd/...
```

#### Litecoin full node implementation `ltcd`
Build and install `ltcd` Litecoin full node implementation
```shell
$ git clone https://github.com/ltcsuite/ltcd.git $GOPATH/src/github.com/ltcsuite/ltcd
$ cd $GOPATH/src/github.com/ltcsuite/ltcd
$ glide install
$ go install . ./cmd/...
```

## Setup Blockchain Clients

#### for Bitcoin
Running the following command will create `rpc.cert`  
Bitcoin testnet blockchain is downloaded to `$HOME/.btcd` by default
```shell
$ btcd --testnet --txindex --rpcuser=kek --rpcpass=kek
```

Sync progress may be tracked as follows
```shell
$ btcctl --testnet --rpcuser=kek --rpcpass=kek getinfo
```

#### for Litecoin
Running the following command will create `rpc.cert`  
Litecoin testnet blockchain is downloaded to `$HOME/.ltcd` by default
```shell
$ ltcd --testnet --txindex --rpcuser=kek --rpcpass=kek
```

Sync progress may be tracked as follows
```shell
$ ltcctl --testnet --rpcuser=kek --rpcpass=kek getinfo
```

## Running Lightning Daemon
After testnet sync is done, following command can be use to launch `lnd` for testing  
Lightning data is stored in `$HOME/.lnd` by default
```shell
$ lnd --debuglevel=debug --nobootstrap --bitcoin.active --bitcoin.testnet --bitcoin.rpcuser=kek --bitcoin.rpcpass=kek --litecoin.active --litecoin.testnet --litecoin.rpcuser=kek --litecoin.rpcpass=kek
```
