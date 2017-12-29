# Lightning Cross-Chain Swap

### Finding a Swap Route
Exchange A checks queries a swap route to exchange `1000 Satoshi` for `0.001 LTC` with Exchange B (1:100 is fixed for now)
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons queryswaproutes --dest=0248a05db7c3996df2699fca9a9a1f843c723b50a6178e805416150b199b5c44bc --in_amt=100000 --in_ticker=LTC --out_ticker=BTC
{
    "routes": [
	{
	    "total_time_lock": 1256832,
	    "total_fees": "0",
	    "total_amt": "1000",
	    "hops": [
		{
		    "chan_id": "1381475887161802752",
		    "chan_capacity": "16000000",
		    "amt_to_forward": "1000",
		    "fee": "0",
		    "expiry": 1256688
		},
		{
		    "chan_id": "333714973170008064",
		    "chan_capacity": "10000000",
		    "amt_to_forward": "100000",
		    "fee": "0",
		    "expiry": 309485
		}
	    ]
	}
    ]
}
```

Exchange B creates invoice for `1000 Satoshi` payment of Exchange A at first hop
```shell
$ lncli --rpcserver=localhost:10002 --no-macaroons addinvoice --value=1000 --ticker=BTC
{
    "r_hash": "d08c14452b4b2ec5445643e997d2636a7e707a30ea618696fc18267c44d393e5",
    "pay_req": "lntb10u1pdytlzupp56zxpg3ftfvhv23zkg05e05nrdfl8q73safscd9hurqn8c3xnj0jsdqqcqzysgrejyhn4s9ppdglyrypysw72rhz7xl92wwjhkczhv5c97vysu7e8t503speqk2za8ck0lwvewpfrmff3kpvxalrv3xjvvd97cdxmm5qp3ur4yw"
}
```

Exchange A executes swap but failed
```shell
$ lncli --rpcserver=localhost:10001 --no-macaroons queryswaproutes --dest=0248a05db7c3996df2699fca9a9a1f843c723b50a6178e805416150b199b5c44bc --in_amt=100000 --in_ticker=LTC --out_ticker=BTC |lncli --rpcserver=localhost:10001 --no-macaroons sendtoroute --payment_hash d08c14452b4b2ec5445643e997d2636a7e707a30ea618696fc18267c44d393e5
[lncli] rpc error: code = Unknown desc = FeeInsufficient(fee=1000000 mSAT, update=(lnwire.ChannelUpdate) {
 Signature: (*btcec.Signature)(0xc42018e830)({
  R: (*big.Int)(0xc420182d60)(52389999433167180508687453273346618215311437170888547227928361470815627027377),
  S: (*big.Int)(0xc420182d80)(29581608095120752442210305074686183581038286723203003451096381832864661096470)
 }),
 ChainHash: (chainhash.Hash) (len=32 cap=32) 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943,
 ShortChannelID: (lnwire.ShortChannelID) {
  BlockHeight: (uint32) 1256445,
  TxIndex: (uint32) 12,
  TxPosition: (uint16) 0
 },
 Timestamp: (uint32) 1514468158,
 Flags: (uint16) 1,
 TimeLockDelta: (uint16) 144,
 HtlcMinimumMsat: (lnwire.MilliSatoshi) 0 mSAT,
 BaseFee: (uint32) 1000,
 FeeRate: (uint32) 1
}
```

Error log from Exchange A is as follows
```shell
2017-12-29 11:27:25.744 [INF] DISC: Broadcasting batch of 11 new announcements
2017-12-29 11:27:31.615 [INF] CRTR: Found edge with chanid=f4f1f34b7257685a90a6967fe2024afe237dfbd184d82398064abf7e0e25b970
2017-12-29 11:27:31.651 [INF] CRTR: Found edge with chanid=77e00f502c138363f21573007cea39b62fbb0aabee19e3c8148792b163ea5110
2017-12-29 11:27:31.673 [INF] RPCS: rpcroute: (*lnrpc.Route)(0xc4201809f0)(total_time_lock:1256832 total_amt:1000 hops:<chan_id:1381475887161802752 chan_capacity:16000000 amt_to_forward:1000 expiry:1256688 > hops:<chan_id:333714973170008064 chan_capacity:10000000 amt_to_forward:100000 expiry:309489 > )

2017-12-29 11:27:31.674 [INF] RPCS: route[0]: hop=0 expiry=1256688
2017-12-29 11:27:31.674 [INF] RPCS: route[0]: hop=1 expiry=309489
2017-12-29 11:27:31.674 [INF] CRTR: forwarding to chain: 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943
2017-12-29 11:27:31.674 [INF] CRTR: forwarding with amt: 1000000 mSAT
2017-12-29 11:27:31.674 [INF] CRTR: forwarding with cltv: 1256688
2017-12-29 11:27:31.674 [INF] CRTR: forwarding to chain: 4966625a4b2851d9fdee139e56211a0d88575f59ed816ff5e6a63deb4e3e29a0
2017-12-29 11:27:31.674 [INF] CRTR: forwarding with amt: 100000000 mSAT
2017-12-29 11:27:31.674 [INF] CRTR: forwarding with cltv: 309489
2017-12-29 11:27:31.675 [INF] CRTR: sending payment to switch along route of length=2
2017-12-29 11:27:31.676 [INF] HSWC: Setting first hop destination to: 77e00f502c138363f21573007cea39b62fbb0aabee19e3c8148792b163ea5110
2017-12-29 11:27:31.676 [INF] HSWC: packet src={0 0 0} dest=77e00f502c138363f21573007cea39b62fbb0aabee19e3c8148792b163ea5110 destNode=[2 72 160 93 183 195 153 109 242 105 159 202 154 154 31 132 60 114 59 80 166 23 142 128 84 22 21 11 25 155 92 68 188] amt=1000000 mSAT
2017-12-29 11:27:31.676 [INF] HSWC: Found short={1256445 12 0} link=77e00f502c138363f21573007cea39b62fbb0aabee19e3c8148792b163ea5110, bandwidth=15990312000 mSAT
2017-12-29 11:27:31.730 [INF] PEER: active channel found?: true
2017-12-29 11:27:31.730 [INF] PEER: retrieving service for chain 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943
2017-12-29 11:27:31.731 [INF] PEER: active channel found?: true
2017-12-29 11:27:31.731 [INF] PEER: retrieving service for chain 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943
2017-12-29 11:27:31.736 [INF] PEER: active channel found?: true
2017-12-29 11:27:31.736 [INF] PEER: retrieving service for chain 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943
2017-12-29 11:27:31.737 [INF] PEER: active channel found?: true
2017-12-29 11:27:31.737 [INF] PEER: retrieving service for chain 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943
2017-12-29 11:27:31.742 [INF] PEER: active channel found?: true
2017-12-29 11:27:31.743 [INF] PEER: retrieving service for chain 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943
2017-12-29 11:27:31.745 [ERR] CRTR: Attempt to send payment to route d08c14452b4b2ec5445643e997d2636a7e707a30ea618696fc18267c44d393e5 failed: FeeInsufficient(fee=1000000 mSAT, update=(lnwire.ChannelUpdate) {
 Signature: (*btcec.Signature)(0xc42018e830)({
  R: (*big.Int)(0xc420182d60)(52389999433167180508687453273346618215311437170888547227928361470815627027377),
  S: (*big.Int)(0xc420182d80)(29581608095120752442210305074686183581038286723203003451096381832864661096470)
 }),
 ChainHash: (chainhash.Hash) (len=32 cap=32) 000000000933ea01ad0ee984209779baaec3ced90fa3f408719526f8d77f4943,
 ShortChannelID: (lnwire.ShortChannelID) {
  BlockHeight: (uint32) 1256445,
  TxIndex: (uint32) 12,
  TxPosition: (uint16) 0
 },
 Timestamp: (uint32) 1514468158,
 Flags: (uint16) 1,
 TimeLockDelta: (uint16) 144,
 HtlcMinimumMsat: (lnwire.MilliSatoshi) 0 mSAT,
 BaseFee: (uint32) 1000,
 FeeRate: (uint32) 1
}

2017-12-29 11:27:34.113 [INF] HSWC: Sent 0 satoshis received 0 satoshis in the last 10 seconds (0.2 tx/sec)
```

