# rpc-docs
Minimal documentation of mainchain &amp; testchain template

# Mainchain
```
setgenerate
broadcastnews
createopreturntransaction 
getopreturndata
addwithdrawal
clearwithdrawalvotes
countsidechaindeposits
createbmmcriticaldatatx
createcriticaldatatx
createsidechaindeposit
createsidechainproposal
getactivesidechaincount
getaveragefee
getscdbdataforblock
getsidechainactivationstatus
gettotalscdbhash
getworkscore 
havefailedwithdrawal
havespentwithdrawal
listactivesidechains
listcachedwithdrawaltx
listfailedbmm
listfailedwithdrawals
listpreviousblockhashes
listsidechainactivationstatus
listsidechainctip
listsidechaindeposits
listsidechainproposals
listspentwithdrawals
listwithdrawalstatus 
listwithdrawalvotes
receivewithdrawalbundle
setwithdrawalvote
verifybmm
verifydeposit
```

# Testchain
```
createwt
createwithdrawalrefundrequest
formatdepositaddress
getaveragemainchainfees
getmainchainblockcount
getmainchainblockhash
getwithdrawal
getwithdrawalbundle
listmywithdrawals
rebroadcastwithdrawalbundle
refreshbmm
refundallwithdrawals![bmm](https://github.com/LayerTwo-Labs/rpc-docs/assets/8107318/e6814a14-98b8-445a-b6cd-55dbf624b8e2)

updatemainblockcache
verifymainblockcache
```


# refreshbmm additional notes

![bmm](https://github.com/LayerTwo-Labs/rpc-docs/assets/8107318/677ee7d1-367c-440c-8cff-87619d10f2ba)
The Qt version does not use any RPC but we should be able to recreate it via RPC.

How the re-create the sidechain page BMM table via RPC:

`MC txid` get this from refreshbmm - "txid" field

`MC Block` get this from the mainchain with the `getblockcount` RPC and add 1 to it

`SC Block` get this from the sidechain with the `getblockcount` RPC and add 1 to it

`Txns` get this from refreshbmm - "ntxn" field

`Fees` get this from refreshbmm - "fees" field

`Bid Amount` This is set by the user on the GUI, or as the "amount" argument for refreshbmm

`Profit` this is the fees minus bmm bid amount

`Status` This is updated every block depending on if our request failed or was included, for example when you call refreshbmm if the previous bmm attempt was successful, the status is updated on the table. 
