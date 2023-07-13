Create a sidechain, deposit, withdraw
====



First we will propose and activate a sidechain
=== 

Create sidechain proposal 
---
`./mainchain/src/drivechain-cli --regtest createsidechainproposal 0 "testchain" "testchain for integration test"`

Look at our proposals
---
`./mainchain/src/drivechain-cli --regtest listsidechainproposals`

Mine a block so that our proposal will be committed
---
`./mainchain/src/drivechain-cli --regtest generate 1`

View proposal status
---
`./mainchain/src/drivechain-cli --regtest listsidechainactivationstatus`

Are any sidechains active yet?
---
`./mainchain/src/drivechain-cli --regtest listactivesidechains`

Generate a bunch of blocks to activate the sidechain
---
`./mainchain/src/drivechain-cli --regtest generate 100`

Is the sidechain active now?
---
./mainchain/src/drivechain-cli --regtest listactivesidechains

Now we will deposit to the sidechain
===
Get a sidechain deposit address
---
`ADDRESS=./testchain/src/testchain-cli --regtest getnewaddress sidechain legacy`
`DEPOSITADDRESS=./testchain/src/testchain-cli --regtest formatdepositaddress $ADDRESS`
`./mainchain/src/drivechain-cli --regtest createsidechaindeposit 0 $DEPOSITADDRESS 1 0.01`

Generate blocks to process the deposit (mainchain & refreshbmm on sidechain)
---
Do until deposit processes (should be at least 2 blocks):

  `./mainchain/src/drivechain-cli --regtest generate 1`
  
  `./testchain/src/testchain-cli --regtest refreshbmm $BMM_BID`

Check sidechain balance:
---
`./testchain/src/testchain-cli --regtest getbalance`





Now we will withdraw from the sidechain
===

Get mainchain address & sidechain refund address
---
`MAINCHAIN_ADDRESS=./mainchain/src/drivechain-cli --regtest getnewaddress mainchain legacy`
`REFUND_ADDRESS=./testchain/src/testchain-cli --regtest getnewaddress refund legacy`

Create withdrawal
---
`./testchain/src/testchain-cli --regtest createwithdrawal $MAINCHAIN_ADDRESS $REFUND_ADDRESS 0.5 0.1 0.1`

Mine enough mainchain & sidechain blocks to create the withdrawal bundle
---
Do until withdrawal bundle created and you see the hash on the bottom of the sidechain GUI and also on the withdrawal tab on the mainchain (should be at least 3 blocks):

  `./mainchain/src/drivechain-cli --regtest generate 1`
  
  `./testchain/src/testchain-cli --regtest refreshbmm $BMM_BID`

Check on mainchain if we are tracking the bundle
---
`./mainchain/src/drivechain-cli --regtest listwithdrawalstatus 0`

You can check the workscore with this RPC as well
---
`./mainchain/src/drivechain-cli --regtest getworkscore 0 $HASHBUNDLE`

By default the mainchain should upvote withdrawal bundles that were received from our sidechain node, but votes can be set this way as well
---
`./mainchain/src/drivechain-cli --regtest setwithdrawalvote upvote 0 $HASHBUNDLE`

Mine enough mainchain blocks for withdrawal bundle payout (should be at least 131)
---
  `./mainchain/src/drivechain-cli --regtest generate 131`
  
