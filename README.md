#StartMany Setup Guid

## Create New Wallet
Close your QT wallet.

Moving your existint wallet.dat (and darkcoin.conf if you have one) into a backup directory.

Restart the QT wallet. Doing so will create a new wallet.dat.

## Create New Wallet Addresses

1. Open the QT Wallet.
2. Click the Receive tab.
3. Fill in the form to request a payment.
    * Label: mn01
    * Amount: 1000 (optional)
    * Click *Request payment*
5. Click the *Copy Address* button

Create a new wallet address for each MasterNode.

Close your QT Wallet.

## Send 1,000 DRK to New Addresses

### Shut down your MasterNode
I don't know if this step is entirely necessary, but before I move 1,000 DRK from my old MasterNode to a new address, I shut down the daemon.

Log into your MasterNode server and:

```darkcoind stop```

### Send funds from old MasterNode to new wallet address

Just like setting up a standard MN. Send exactly 1,000 DRK to each new address created above.

## Create Private Keys

Open your QT Wallet.

You need to create a private key for each of the new wallet addresses created above. This is a simple as dumpprivkey.

```walletpassphrase [your_wallet_passphrase] 600```
```dumpprivkey [wallet_address_from_above]```

Close your QT Wallet.

## Create masternode.conf file

Remember... this is local. Make sure your QT is not running.

Create the masternode.conf file in the same directory as your wallet.dat.

Copy the private key and correspondig collateral output transaction that holds the 1K DRK.

### Get the collateral output

Go to one of the block chain explorers. Like http://explorer.darkcoin.io/chain/Darkcoin

Enter the wallet address for your new node into the search box.

You should see a list of one (or more) transactions. Click the transaction line item that displays the 1,000 dark you sent to the address.

Copy the hash, and note the index. The has is your collateral output that will be used in the masternode.conf file

### Enter your MasterNode details into your masternode.conf file

```
mn01 127.0.0.1:9999 priv_key collateral_output 0
mn02 127.0.0.2:9999 priv_key collateral_output 0
```

## Create a darkcoin.conf file

Remember... this is local. Make sure your QT is not running.

You should already have a darkcoin.conf file from your exisiting node. I simply copied it into my new wallet.

## Update darkcoin.conf on server

```sudo nano .darkcoin/darkcoin.conf```

### Edit the masternodeprivkey
Since you generated a new private key for your new wallet address, you will need to update the masternodeprivkey value in your remote darkcoin.conf file.

## Start your MasterNodes

### Remote

Start your remote daemon as you normally would. 

I usually confirm that remote is on the correct block by issuing:

```darkcoind getinfo```

And comparing with the official explorer at http://explorer.darkcoin.io/chain/Darkcoin

### Local

Finally... time to start from local.

#### Open up your QT Wallet

From the menu select Tools => Debug Console

If you want to review your masternode.conf setting before starting the MasterNodes, issue the following in the Debug Console:

```masternode list-conf```

Give it the eye-ball test. If satisfied, you can start your nodes one of two ways.

1. masternode start-alias [alias_from_masternode.conf]. Example ```masternode start-alias mn01```
2. masternode start-many
