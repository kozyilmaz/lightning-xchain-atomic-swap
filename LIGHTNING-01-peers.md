# Lightning Peer Setup

## Exchange A

#### Launch `lnd`
Create a separate directory and launch `lnd` for Exchange A that uses both `Bitcoin` and `Litecoin` chains
```shell
$ mkdir -p $HOME/exchange-a
$ cd $HOME/exchange-a
$ lnd --rpcport=10001 --peerport=10011 --restport=8001 --datadir=test_data --logdir=test_log --debuglevel=info --nobootstrap --no-macaroons --bitcoin.active --bitcoin.testnet --bitcoin.rpcuser=kek --bitcoin.rpcpass=kek --litecoin.active --litecoin.testnet --litecoin.rpcuser=kek --litecoin.rpcpass=kek
```

#### Create Wallets and Addresses
Create `Bitcoin` and `Litecoin` wallets for Exchange A
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons create
Input wallet password:
Confirm wallet password:
```

If wallets are already created then `unlock`
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons unlock
Input wallet password:
```

Check status of Exchange A
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons getinfo
{
    "identity_pubkey": "0279a2f2b499a5345f17d35eff76d5ab45d252560f63347b96e42f8b80045ca435",
    "alias": "",
    "num_pending_channels": 0,
    "num_active_channels": 0,
    "num_peers": 0,
    "block_height": 1289903,
    "block_hash": "00000000000001f85d5daec8acaccca62cc614f4abd15e9843b4d74f10df67d4",
    "synced_to_chain": true,
    "testnet": true,
    "chains": [
	"bitcoin",
	"litecoin"
    ]
}
```

Create Exchange A Segwit addresses for both `Bitcoin` and `Litecoin`
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons newaddress np2wkh --ticker=BTC
{
    "address": "2N4UnghXyeACby68RB1Y5Zs8mMfZarJjTjs"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons newaddress np2wkh --ticker=LTC
{
    "address": "2N8xVqBfEukWj1DGFTCmyNir1oymUfiMqbr"
}
```

Query Exchange A wallet balances for both `Bitcoin` and `Litecoin` after creation
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "0"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "0"
}
```

Send [0.65 BTC](https://testnet.blockchain.info/tx/3be0f3dec739aa3dd93ebf36ed8a17f3040101933b319d7f97ceeca8268681d2) and [10 LTC](https://chain.so/tx/LTCTEST/5b05f3ac3ebef9e02aa2a036383212df62709ee9c2fc7a0b09ed77e1dd5043c9) to Exchange A addresses via testnet faucets (see [README.bitcoin](README.bitcoin.md/#bitcoin-testnet-faucet) and [README.litecoin](README.litecoin.md/#litecoin-testnet-faucet))
Query Exchange A wallet balances for both `Bitcoin` and `Litecoin` after funding
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "65000000"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "1000000000"
}
```



## Exchange B

#### Launch `lnd`
Create a separate directory and launch `lnd` for Exchange A that uses both `Bitcoin` and `Litecoin` chains
```shell
$ mkdir -p $HOME/exchange-b
$ cd $HOME/exchange-b
$ lnd --rpcport=10002 --peerport=10012 --restport=8002 --datadir=test_data --logdir=test_log --debuglevel=info --nobootstrap --no-macaroons --bitcoin.active --bitcoin.testnet --bitcoin.rpcuser=kek --bitcoin.rpcpass=kek --litecoin.active --litecoin.testnet --litecoin.rpcuser=kek --litecoin.rpcpass=kek
```

#### Create Wallets and Addresses
Create `Bitcoin` and `Litecoin` wallets for Exchange B
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons create
Input wallet password:
Confirm wallet password:
```

If wallets are already created then `unlock`
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons unlock
Input wallet password:
```

Check status of Exchange B
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons getinfo
{
    "identity_pubkey": "026832da661d53ee23e88909b70ed1768825deb22f26b0d5519ad0c78a1e528676",
    "alias": "",
    "num_pending_channels": 0,
    "num_active_channels": 0,
    "num_peers": 0,
    "block_height": 1289904,
    "block_hash": "00000000bfb31081eb06af7c6ea1e02f70688f3a35afb7c472cb5f579f78d09b",
    "synced_to_chain": true,
    "testnet": true,
    "chains": [
	"litecoin",
	"bitcoin"
    ]
}
```

Create Segwit addresses for both `Bitcoin` and `Litecoin`
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons newaddress np2wkh --ticker=BTC
{
    "address": "2MtR8MbDiFsWP6CyGoCjZe8c9e6NWaGn7VR"
}
$ lncli --rpcserver=localhost:10002 --no-macaroons newaddress np2wkh --ticker=LTC
{
    "address": "2N6Pv2ixMRgYe5xY1CNfTYVTehmLB1zwaFu"
}
```

Query Exchange B wallet balances for both `Bitcoin` and `Litecoin` after creation
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "0"
}
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "0"
}
```

Send [0.325 BTC](https://testnet.blockchain.info/tx/b8307069de984b143cbd4539c325de7640ba55d9f381db7f8857cd50b4fa4393) and [5 LTC](https://chain.so/tx/LTCTEST/e5e045aa9cacee5dff539cea65b76e374f4abfec9e515826e6dc6e96467d516b) to Exchange B addresses via testnet faucets (see [README.bitcoin](README.bitcoin.md/#bitcoin-testnet-faucet) and [README.litecoin](README.litecoin.md/#litecoin-testnet-faucet))
Query Exchange B wallet balances for both `Bitcoin` and `Litecoin` after funding
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "32500000"
}
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "500000000"
}
```
