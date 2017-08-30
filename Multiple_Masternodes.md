# Multiple Masternodes, controlled from a single cold wallet

It is fairly simple to run multiple MUE masternodes from a single cold wallet. This short guide will go through the steps how to achieve this. 

First of all, have one masternode setup complete. Start by adding the masternodes one by one as that makes troubleshooting much easier if something is wrong.

Follow the [masternode setup guide](https://github.com/muecoin/Guides/blob/master/masternode_setup.md)
for getting a masternode up and running.

To add more masternodes, make a new `500 000 MUE` collateral transaction in the cold storage MUE wallet. Make sure that the older 500 000 MUE transaction(s) are locked to avoid breaking the active masternode(s). Once the new transaction has been made, lock it in the cold wallet by right-clicking on it and selecting `Lock unspent`.

Check the output of `masternode genkey` and `masternode outputs`. The new masternode output will be the new collateral transaction for the new masternode. Compare with what transactions are already present in the masternode.conf file. The new transactions outputs from the `masternode outputs`command are the new transaction details that need to be added to the masternode.conf file.

Add these values along with the VPS ip-number to the local cold wallet masternode.conf file, one row per masternode.
it shall look something like this:

    # Masternode config file
    # Format: alias IP:port masternodeprivkey collateral_output_txid collateral_output_index
    # Example: mn1 127.0.0.2:19683 93HaYBVUCYjEMeeH1Y4sBGLALQZE1Yc1K64xiqgX37tGBDQL8Xg 2bcd3c84c84f87eaa86e4e56834c92927a07f9e18718810b92e0d0324456a67c 0
    MN1 123.34.45.121:19683 7qm78Bklkd8NeiqGVTwRoXJWRTZkm9yH79L2FbRTp9NrQaTKRcGZ f9a2013b00205098ad8b192d9ac68e7d55e47eb80c860972a404a51bfbc100ac 1
    MN2 123.34.45.123:19683 7rAKDBwknNHH726cdMJK2Y3NnSjSycUyUosXVKH3GzLpvWgWUaQm  7c6a913da3b942bcd3b8f2c81fd94c72f344216718640713b66309c2fede1acf 0
    MN3 123.37.41.156:19683 7qm78BkY2ww9874NNwxhoXJWRTZkm9yH79L2FbRTp9NrQaTKRcGZ f9a2013b00205098ad8b192d9a11ahd225e47eb80c860972a404a51bfbc100ac 0
    MN4 32.44.123.123:19683 7rAKDBa5HpwTUiEFMJK2Y3NnSjSycUyUosXVKH3GzLpvWgWUaQm  7c6a913da3b94abdd16253c81fd94c72f344216718640713b66309c2fede1acf 1


Restart your local cold storage wallet, and in the masternode tab, select the masternodes to start with `Start alias` or use the `Start many` button to start many at once - if they all are waiting to be enabled. Remeber that restarting a masterode may place it in the back of the payment queue, so only restart the masternode that needs restarting.

Go back and check your masternodes with 

    mue-cli masternode debug
    Masternode successfully started

Congratulations!
