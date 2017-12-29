# Lightning Payment

## Bitcoin Payment
Exchange B creates invoice for `1000 Satoshi`
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons addinvoice --value=1000 --ticker=BTC
{
    "r_hash": "ded910abaaf18047541bcee8d58e24da98f9ef87c124a993904da99d891c5cbf",
    "pay_req": "lntb10u1pdytm2qpp5mmv3p2a27xqyw4qmem5dtr3ym2v0nmu8cyj2nyusfk5emzgutjlsdqqcqzysw345mnfc55vrqkjpj6rw3227wpnsm5lj6ujaaq8z0rvlzzluevjhd2xdx8gcrcundxpqa698ej6akr0ptem8jacuekkxwdzd85hf2wspyrfnwn"
}
```

Exchange A pays `1000 Satoshi` using encoded payment request
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons sendpayment --pay_req=lntb10u1pdytm2qpp5mmv3p2a27xqyw4qmem5dtr3ym2v0nmu8cyj2nyusfk5emzgutjlsdqqcqzysw345mnfc55vrqkjpj6rw3227wpnsm5lj6ujaaq8z0rvlzzluevjhd2xdx8gcrcundxpqa698ej6akr0ptem8jacuekkxwdzd85hf2wspyrfnwn
{
    "payment_error": "",
    "payment_preimage": "2e2384405a8f6771c30f7b560c62914b07ef7e9f5ce42772a1da6facc92dd551",
    "payment_route": {
	"total_time_lock": 1256822,
	"total_amt": 1000,
	"hops": [
	    {
		"chan_id": 1381475887161802752,
		"chan_capacity": 16000000,
		"amt_to_forward": 1000,
		"expiry": 1256822
	    }
	]
    }
}
```

#### Exchange A's Balances & Channel Status Pre Payment
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "83991564"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "10000000"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons listchannels
{
    "channels": [
	{
	    "active": true,
	    "remote_pubkey": "0248a05db7c3996df2699fca9a9a1f843c723b50a6178e805416150b199b5c44bc",
	    "channel_point": "1051ea63b1928714c8e319eeab0abb2fb639ea7c007315f26383132c500fe077:0",
	    "chan_id": "1381475887161802752",
	    "capacity": "16000000",
	    "local_balance": "15991312",
	    "remote_balance": "0",
	    "commit_fee": "8688",
	    "commit_weight": "600",
	    "fee_per_kw": "12000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "0",
	    "total_satoshis_received": "0",
	    "num_updates": "26",
	    "pending_htlcs": []
	},
	{
	    "active": true,
	    "remote_pubkey": "0248a05db7c3996df2699fca9a9a1f843c723b50a6178e805416150b199b5c44bc",
	    "channel_point": "70b9250e7ebf4a069823d884d1fb7d23fe4a02e27f96a6905a6857724bf3f1f4:0",
	    "chan_id": "333714973170008064",
	    "capacity": "10000000",
	    "local_balance": "0",
	    "remote_balance": "9963800",
	    "commit_fee": "36200",
	    "commit_weight": "552",
	    "fee_per_kw": "50000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "0",
	    "total_satoshis_received": "0",
	    "num_updates": "0",
	    "pending_htlcs": []
	}
    ]
}
```

#### Exchange A's Balances & Channel Status Post Payment
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=BTC{
    "balance": "83991564"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "10000000"
}
$ lncli --rpcserver=localhost:10001 --no-macaroons listchannels
{
    "channels": [
	{
	    "active": true,
	    "remote_pubkey": "0248a05db7c3996df2699fca9a9a1f843c723b50a6178e805416150b199b5c44bc",
	    "channel_point": "1051ea63b1928714c8e319eeab0abb2fb639ea7c007315f26383132c500fe077:0",
	    "chan_id": "1381475887161802752",
	    "capacity": "16000000",
	    "local_balance": "15990312",
	    "remote_balance": "1000",
	    "commit_fee": "8688",
	    "commit_weight": "724",
	    "fee_per_kw": "12000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "1000",
	    "total_satoshis_received": "0",
	    "num_updates": "28",
	    "pending_htlcs": []
	},
	{
	    "active": true,
	    "remote_pubkey": "0248a05db7c3996df2699fca9a9a1f843c723b50a6178e805416150b199b5c44bc",
	    "channel_point": "70b9250e7ebf4a069823d884d1fb7d23fe4a02e27f96a6905a6857724bf3f1f4:0",
	    "chan_id": "333714973170008064",
	    "capacity": "10000000",
	    "local_balance": "0",
	    "remote_balance": "9963800",
	    "commit_fee": "36200",
	    "commit_weight": "552",
	    "fee_per_kw": "50000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "0",
	    "total_satoshis_received": "0",
	    "num_updates": "0",
	    "pending_htlcs": []
	}
    ]
}
```

#### Exchange B's Balances & Channel Status Pre Payment
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "130000000"
}
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "989964850"
}
$ lncli --rpcserver=localhost:10002 --no-macaroons listchannels
{
    "channels": [
	{
	    "active": true,
	    "remote_pubkey": "026a2f91860f43b03aff44246652a464e68a678251d6d6e0f24a8c4398b8333aa7",
	    "channel_point": "1051ea63b1928714c8e319eeab0abb2fb639ea7c007315f26383132c500fe077:0",
	    "chan_id": "1381475887161802752",
	    "capacity": "16000000",
	    "local_balance": "0",
	    "remote_balance": "15991312",
	    "commit_fee": "8688",
	    "commit_weight": "552",
	    "fee_per_kw": "12000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "0",
	    "total_satoshis_received": "0",
	    "num_updates": "26",
	    "pending_htlcs": []
	},
	{
	    "active": true,
	    "remote_pubkey": "026a2f91860f43b03aff44246652a464e68a678251d6d6e0f24a8c4398b8333aa7",
	    "channel_point": "70b9250e7ebf4a069823d884d1fb7d23fe4a02e27f96a6905a6857724bf3f1f4:0",
	    "chan_id": "333714973170008064",
	    "capacity": "10000000",
	    "local_balance": "9963800",
	    "remote_balance": "0",
	    "commit_fee": "36200",
	    "commit_weight": "600",
	    "fee_per_kw": "50000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "0",
	    "total_satoshis_received": "0",
	    "num_updates": "0",
	    "pending_htlcs": []
	}
    ]
}
```

#### Exchange B's Balances & Channel Status Post Payment
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=BTC
{
    "balance": "130000000"
}
$ lncli --rpcserver=localhost:10002 --no-macaroons walletbalance --ticker=LTC
{
    "balance": "989964850"
}
$ lncli --rpcserver=localhost:10002 --no-macaroons listchannels
{
    "channels": [
	{
	    "active": true,
	    "remote_pubkey": "026a2f91860f43b03aff44246652a464e68a678251d6d6e0f24a8c4398b8333aa7",
	    "channel_point": "1051ea63b1928714c8e319eeab0abb2fb639ea7c007315f26383132c500fe077:0",
	    "chan_id": "1381475887161802752",
	    "capacity": "16000000",
	    "local_balance": "1000",
	    "remote_balance": "15990312",
	    "commit_fee": "8688",
	    "commit_weight": "724",
	    "fee_per_kw": "12000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "0",
	    "total_satoshis_received": "1000",
	    "num_updates": "28",
	    "pending_htlcs": []
	},
	{
	    "active": true,
	    "remote_pubkey": "026a2f91860f43b03aff44246652a464e68a678251d6d6e0f24a8c4398b8333aa7",
	    "channel_point": "70b9250e7ebf4a069823d884d1fb7d23fe4a02e27f96a6905a6857724bf3f1f4:0",
	    "chan_id": "333714973170008064",
	    "capacity": "10000000",
	    "local_balance": "9963800",
	    "remote_balance": "0",
	    "commit_fee": "36200",
	    "commit_weight": "600",
	    "fee_per_kw": "50000",
	    "unsettled_balance": "0",
	    "total_satoshis_sent": "0",
	    "total_satoshis_received": "0",
	    "num_updates": "0",
	    "pending_htlcs": []
	}
    ]
}
```


## Litecoin Payment
Exchange A creates invoice for `5000 Litoshi`
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons addinvoice --value=1000 --ticker=LTC
{
    "r_hash": "95e9e8b5cb5cc1122d964a077bf9438ee7388548e70d8bccd7b014e63683ed63",
    "pay_req": "lntl10u1pdyt7w4pp5jh573dwttnq3ytvkfgrhh72r3mnn3p2guuxchnxhkq2wvd5ra43sdqqcqzysp7kkazvh7r3j93t23stnf0y7zjpjwg73cnyr5prvg94jaec05l8qm9j7mvj8akkul976q7yyqzrdc2kztmuczcfdp3p4xpk6g4y25zqqn7xyzm"
}
```

Exchange B pays `5000 Satoshi` using encoded payment request
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons sendpayment --pay_req=lntl10u1pdyt7w4pp5jh573dwttnq3ytvkfgrhh72r3mnn3p2guuxchnxhkq2wvd5ra43sdqqcqzysp7kkazvh7r3j93t23stnf0y7zjpjwg73cnyr5prvg94jaec05l8qm9j7mvj8akkul976q7yyqzrdc2kztmuczcfdp3p4xpk6g4y25zqqn7xyzm
[lncli] rpc error: code = Unknown desc = unknown network

```

Error log from Exchange B is here
```shell
2017-12-29 11:16:52.411 [INF] LTND: SENDING PAYMENT
2017-12-29 11:16:58.951 [INF] CRTR: Pruning channel graph using block 4d0fa6989c27117acc31bf376ae3b4fd1cc5dbffd3432ac13a8450fc67030d08 (height=309402)
2017-12-29 11:16:58.952 [INF] NTFN: New block: height=309402, sha=4d0fa6989c27117acc31bf376ae3b4fd1cc5dbffd3432ac13a8450fc67030d08
2017-12-29 11:16:58.953 [INF] CRTR: Block 4d0fa6989c27117acc31bf376ae3b4fd1cc5dbffd3432ac13a8450fc67030d08 (height=309402) closed 0 channels
2017-12-29 11:17:03.176 [INF] CRTR: New channel discovered! Link connects 032e1ac80325004b380b44166d136e09a7e544981593a17c29fd1f1d2b2cc795b0 and 03a709604a6dd6c4fb28448df74470248840923bf39c29703aea1aa1b5ba9cd8e2 with ChannelPoint(f42e643a312a13f3eb2fad4f6f1c832388bb25f6d2e64d54c53c932f422612d6:0): chan_id=1349790161090576384, capacity=0.16777216 BTC
2017-12-29 11:17:03.187 [INF] CRTR: New channel discovered! Link connects 026fd582ff6fab565fa3db3e6b2a399ebc54cf07257fd1b86a5dc0fbc17bb9b719 and 02b27f95da8f97e7be9159705fc79ca6ae5ddcfafcc22df47b9454729d2f7f8dfe with ChannelPoint(3a2563f27081c4c17031d2051a53e26d072a42a2e913b12f2b23c4b4d78730f2:0): chan_id=1380849165535215616, capacity=0.001 BTC
2017-12-29 11:17:03.367 [ERR] DISC: unable to validate channel update short_chan_id=1352857798515425280: edge not found
2017-12-29 11:17:03.368 [INF] CRTR: New channel update applied: (*channeldb.ChannelEdgePolicy)(0xc4202b76e0)({
 Signature: (*btcec.Signature)(0xc42004df40)({
  R: (*big.Int)(0xc4209ccb00)(106731067912169633524566679907340094787463676397320836375361873486392370814877),
  S: (*big.Int)(0xc4209ccb20)(3073031417162679460012535653968710976311016959250886873741661671816923026796)
 }),
 ChannelID: (uint64) 1353719815635795968,
 LastUpdate: (time.Time) 2017-11-30 11:23:15 +0300 +03,
 Flags: (uint16) 1,
 TimeLockDelta: (uint16) 144,
 MinHTLC: (lnwire.MilliSatoshi) 0 mSAT,
 FeeBaseMSat: (lnwire.MilliSatoshi) 1000 mSAT,
 FeeProportionalMillionths: (lnwire.MilliSatoshi) 1 mSAT,
 Node: (*channeldb.LightningNode)(<nil>),
 db: (*channeldb.DB)(<nil>)
})

2017-12-29 11:17:03.371 [INF] CRTR: New channel update applied: (*channeldb.ChannelEdgePolicy)(0xc42053ac60)({
 Signature: (*btcec.Signature)(0xc42004df80)({
  R: (*big.Int)(0xc4209ccc60)(71779586915899577497428481671441220051313341240178729511982576368288022962649),
  S: (*big.Int)(0xc4209ccc80)(11974236527839345426864016902233300933995861314059481271255312235247737853418)
 }),
 ChannelID: (uint64) 1380849165535215616,
 LastUpdate: (time.Time) 2017-12-29 11:14:44 +0300 +03,
 Flags: (uint16) 1,
 TimeLockDelta: (uint16) 144,
 MinHTLC: (lnwire.MilliSatoshi) 10000 mSAT,
 FeeBaseMSat: (lnwire.MilliSatoshi) 10000 mSAT,
 FeeProportionalMillionths: (lnwire.MilliSatoshi) 100 mSAT,
 Node: (*channeldb.LightningNode)(<nil>),
 db: (*channeldb.DB)(<nil>)
})

2017-12-29 11:17:03.373 [INF] CRTR: New channel update applied: (*channeldb.ChannelEdgePolicy)(0xc4206925a0)({
 Signature: (*btcec.Signature)(0xc42004dfa0)({
  R: (*big.Int)(0xc4209ccd00)(30079525372974575652862759704451444392163024061029221153142421909793426954943),
  S: (*big.Int)(0xc4209ccd20)(52220152253010310716308475080202813325924115268847716574171826763872410556997)
 }),
 ChannelID: (uint64) 1321998905170329600,
 LastUpdate: (time.Time) 2017-09-27 04:57:06 +0300 +03,
 Flags: (uint16) 1,
 TimeLockDelta: (uint16) 144,
 MinHTLC: (lnwire.MilliSatoshi) 0 mSAT,
 FeeBaseMSat: (lnwire.MilliSatoshi) 1000 mSAT,
 FeeProportionalMillionths: (lnwire.MilliSatoshi) 1 mSAT,
 Node: (*channeldb.LightningNode)(<nil>),
 db: (*channeldb.DB)(<nil>)
})

2017-12-29 11:17:03.374 [INF] CRTR: New channel update applied: (*channeldb.ChannelEdgePolicy)(0xc42053ade0)({
 Signature: (*btcec.Signature)(0xc42004dfc0)({
  R: (*big.Int)(0xc4209ccda0)(20961642778862117589227177221233774469460108155950242689603279026114998184101),
  S: (*big.Int)(0xc4209ccdc0)(9545465154386889894998768016597700138720574775631026565867791329054385056454)
 }),
 ChannelID: (uint64) 1321998905170329600,
 LastUpdate: (time.Time) 2017-09-27 04:57:05 +0300 +03,
 Flags: (uint16) 0,
 TimeLockDelta: (uint16) 144,
 MinHTLC: (lnwire.MilliSatoshi) 0 mSAT,
 FeeBaseMSat: (lnwire.MilliSatoshi) 1000 mSAT,
 FeeProportionalMillionths: (lnwire.MilliSatoshi) 1 mSAT,
 Node: (*channeldb.LightningNode)(<nil>),
 db: (*channeldb.DB)(<nil>)
})

```
