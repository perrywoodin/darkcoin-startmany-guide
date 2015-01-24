#start-many Setup Guide

## Two Options for Setting up your Wallet
There are many ways to setup a wallet to support start-many. This guide will walk through two of them.

1. Importing an existing wallet (recommended if you are consolidating wallets).
2. Sending 1,000 DRK to new wallet addresses.

## Option 1. Importing an existing wallet

This is the way to go if you are consolidating multiple wallets into one that supports start-many. 

### From your single-instance MasterNode Wallet

Open your QT Wallet and go to console (from the menu select Tools => Debug Console)

Dump the private key from your MasterNode's pulic key.

```
walletpassphrase [your_wallet_passphrase] 600
dumpprivkey [mn_public_key]
```

Copy the resulting priviate key. You'll use it in the next step.

### From your multi-instance MasterNode Wallet

Open your QT Wallet and go to console (from the menu select Tools => Debug Console)

Import the private key from the step above.

```
walletpassphrase [your_wallet_passphrase] 600
importprivkey [single_instance_private_key]
```

The wallet will re-scan and you will see your available balance increase by the amount that was in the imported wallet.

Skip Option 2. and go to Create masternode.conf file

## Option 2. Starting with a new wallet

If you used Option 1 above, then you can skip down to Create masternode.conf file.

### Create New Wallet Addresses

1. Open the QT Wallet.
2. Click the Receive tab.
3. Fill in the form to request a payment.
    * Label: mn01
    * Amount: 1000 (optional)
    * Click *Request payment*
5. Click the *Copy Address* button

Create a new wallet address for each MasterNode.

Close your QT Wallet.

### Send 1,000 DRK to New Addresses

Just like setting up a standard MN. Send exactly 1,000 DRK to each new address created above.

### Create New Keys

Open your QT Wallet and go to console (from the menu select Tools => Debug Console)

Issue the following:

```masternode genkey```

Close your QT Wallet.

## Create masternode.conf file

Remember... this is local. Make sure your QT is not running.

Create the masternode.conf file in the same directory as your wallet.dat.

Copy the private key and correspondig collateral output transaction that holds the 1K DRK.

The private key may be an existing key from Option 1, or a newly generated key from Option 2. 

### Get the collateral output

Open your QT Wallet and go to console (from the menu select Tools => Debug Console)

Issue the following:

```masternode outputs```

Make note of the hash (which is your collaterla_output) and index.

### Enter your MasterNode details into your masternode.conf file
[From the darkcoin github repo](https://github.com/darkcoin/darkcoin/blob/master/doc/masternode_conf.md)

The new masternode.conf format consists of a space seperated text file. Each line consisting of an alias, IP address followed by port, masternode private key, collateral output transaction id and collateral output index.

```
alias ipaddress:port masternode_private_key collateral_output collateral_output_index
```

Example:

```
mn01 127.0.0.1:9999 93HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg 2bcd3c84c84f87eaa86e4e56834c92927a07f9e18718810b92e0d0324456a67c 0
mn02 127.0.0.2:9999 93WaAb3htPJEV8E9aQcN23Jt97bPex7YvWfgMDTUdWJvzmrMqey aa9f1034d973377a5e733272c3d0eced1de22555ad45d6b24abadff8087948d4 0
```

## What about the darkcoin.conf file?

If you are using a masternode.conf file you no longer need the darkcoin.conf file. The exception is if you need custom settings (thanks oblox). 

## Update darkcoin.conf on server

If you generated a new private key, you will need to update the remote darkcoin.conf files.

Shut down the daemon and then edit the file.

```sudo nano .darkcoin/darkcoin.conf```

### Edit the masternodeprivkey
If you generated a new private key, you will need to update the masternodeprivkey value in your remote darkcoin.conf file.

## Start your MasterNodes

### Remote

If your remote server is not running, start your remote daemon as you normally would. 

I usually confirm that remote is on the correct block by issuing:

```darkcoind getinfo```

And compare with the official explorer at http://explorer.darkcoin.io/chain/Darkcoin

### Local

Finally... time to start from local.

#### Open up your QT Wallet

From the menu select Tools => Debug Console

If you want to review your masternode.conf setting before starting the MasterNodes, issue the following in the Debug Console:

```masternode list-conf```

Give it the eye-ball test. If satisfied, you can start your nodes one of two ways.

1. masternode start-alias [alias_from_masternode.conf]. Example ```masternode start-alias mn01```
2. masternode start-many
