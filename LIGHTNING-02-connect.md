# Lightning Peer Connection
First Exchange A and Exchange B should connect at network level e.g. becoming peers of each other  
In order to do that `identity_pubkey` variable and `peerport` argument is used  

## Checking Connection State

#### Exchange A pre-connection
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons getinfo
{
    "identity_pubkey": "0279a2f2b499a5345f17d35eff76d5ab45d252560f63347b96e42f8b80045ca435",
    "alias": "",
    "num_pending_channels": 0,
    "num_active_channels": 0,
    "num_peers": 0,
    "block_height": 1289908,
    "block_hash": "00000000d3924d33d7a20da5608516d145261ce78388979816af095f0446c78b",
    "synced_to_chain": true,
    "testnet": true,
    "chains": [
	"litecoin",
	"bitcoin"
    ]
}
$ lncli --rpcserver=localhost:10001 --no-macaroons listpeers
{
    "peers": []
}
```

#### Exchange B pre-connection
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons getinfo
{
    "identity_pubkey": "026832da661d53ee23e88909b70ed1768825deb22f26b0d5519ad0c78a1e528676",
    "alias": "",
    "num_pending_channels": 0,
    "num_active_channels": 0,
    "num_peers": 0,
    "block_height": 1289909,
    "block_hash": "0000000000000376f45b56843cb7b4d44b526bf5285191df937bce31fdec31ef",
    "synced_to_chain": true,
    "testnet": true,
    "chains": [
	"bitcoin",
	"litecoin"
    ]
}
$ lncli --rpcserver=localhost:10002 --no-macaroons listpeers
{
    "peers": []
}
```


## Establishing Connection
By using Exchange A's `identity_pubkey`, host and port number `peerport`, Exchange B establish connection using the command below
```shell
$ export X_A_ID_PUBKEY=0279a2f2b499a5345f17d35eff76d5ab45d252560f63347b96e42f8b80045ca435
$ export HOST=127.0.0.1
# peerport for Exchange A was set to 10011 in LIGHTNING-01-peers.md
$ export PEER_PORT=10011
$ lncli --rpcserver=localhost:10002 --no-macaroons connect $X_A_ID_PUBKEY@$HOST:$PEER_PORT
{
    "peer_id": 0
}
```

#### Exchange A post-connection
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons listpeers
{
    "peers": [
	{
	    "pub_key": "026832da661d53ee23e88909b70ed1768825deb22f26b0d5519ad0c78a1e528676",
	    "peer_id": 1,
	    "address": "127.0.0.1:45634",
	    "bytes_sent": "149",
	    "bytes_recv": "149",
	    "sat_sent": "0",
	    "sat_recv": "0",
	    "inbound": false,
	    "ping_time": "0"
	}
    ]
}
```

#### Exchange B post-connection
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons listpeers
{
    "peers": [
	{
	    "pub_key": "0279a2f2b499a5345f17d35eff76d5ab45d252560f63347b96e42f8b80045ca435",
	    "peer_id": 1,
	    "address": "127.0.0.1:10011",
	    "bytes_sent": "175",
	    "bytes_recv": "175",
	    "sat_sent": "0",
	    "sat_recv": "0",
	    "inbound": true,
	    "ping_time": "511"
	}
    ]
}
```
