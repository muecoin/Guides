    This document is an initial draft, please be careful as it may contain errors.

# Running the MonetaryUnit client wallet on MacOS, OSX

Running the MUE client on MacOS works very well and can be used for every day wallet uses or as an occastional cold wallet.

Please grab the latest MacOS/OSX client form the Monetary Unit code repository: https://github.com/muecoin/MUECore/releases/latest

### Install the MUE client

Unpack the MacOS zip-file, and drag the 'MonetaryUnit-Qt.app' to your /Applications/ folder.
    

### Start the client

Depending on your security settings in MacOS, you may see a secuity warning that you cannot run the program due to an untrusted developer verification.

In `Finder`, go to your `Applications` and `ctrl-click` or `right-click` on the `MonetaryUnit-QT.app` and choose `open`. Choose to open the app, and the MonetaryUnit client will launch.    

By default, the blockchain and wallet files are stored in `~/Library/Application Support/MonetaryUnitCore`
In this foilder, your `wallet.dat` file is located. It is your wallet containing your funds. Please keep this file safe and ensure it is backed up properly and securely!

To go to this folder in `Finder`, open the Finder file explorer, and in the top menu, click on `View`. Now press the `Option` key, and the `Library` foloder will appear in the list. Just click on it and it will open in Finder.

Browse to your `Application Support` folder, and there you will find the `MonetaryUnitCore` folder with your `wallet.dat`file and other important `mue.conf` and `masternode.conf` files.

<a href="Images/ubuntu-mue-qt.png"><img src="Images/ubuntu-mue-qt.png" width="400" ></a>

Congratulations! Your MonetaryUnit QT-wallet is all ready for use.
