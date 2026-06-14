Connect your Monero wallet to a remote monero node on i2p
=========================================================

### One thing before we start, I AM NOT LIABLE FOR ANY LOST OF MONERO, ALWAYS REMEMBER TO BACKUP YOUR KEYS

In this tutorial I will show you how to configure your Monero wallet for i2p.
This guide also works for selfhosted nodes (be it will a few changes of ip's), 
but for this tutorial I will stick to remote nodes. 
Why you would want to do this, you might want to do this if you want more
privacy, and to have your ip hidden when making payments with Monero, you might 
also want to do this because you can, and thats cool too.

> NOTE: this guide will be for the Monero GUI wallet that can be downloaded
> from the Monero website https://www.getmonero.org/downloads/ any other wallet
> might now work when following this guide


## Creating the Tunnels that your GOING to need
Open your tunnels.conf file, add this to the bottom of it:
```
[monero]
type = client
destination = (You will need to put the b32 address for whatever node you want to use, you can find nodes on monero.fail, or use the stormycloud one, which is the one I personally use)
port = 18089 (or whatever port you node is using)
adress = 0.0.0.0 (you may also use 127.0.0.1, but I have my i2p node running on another computer, so I will use 0.0.0.0)
keys = monero.dat (you may change the name of this file, but always keep the .dat)
```
### Then reboot your node with (This only works for linux not windows, so if your on windows you just need to restart your node, should be easy)
```
sudo systemctl restart i2pd
```
This only works for systemd systems, but I feel as if your running another init system, you should beable to know what your doing with this command.

##
## Now that you have setup the Tunnel we can now setup your wallet
If you haven't downloaded the Monero wallet from https://www.getmonero.org/downloads/ then go download it, and come back here. Now that you have your Monero wallet up and running
we are going to need to put it into Advanced mode, to do that we will go into settings, then click close wallet (OH WAIT BEFORE you do this, remember to have your keys for your wallet backed up,
it should be an issue, but just in case (also you should have your keys backed up anyways)), now at the bottom of the window you should see a button that says, change wallet mode, click this and change
to Advanced mode, then log back into your wallet, it should have saved your keys, but in case it didn't please use your keys to log back in.

##
## Now that your wallet is in Advanced mode, we can connect it to the I2P Monero node
To do this, we have to go back into the setting of your wallet, once in settings go to the Node tab at the top, and click remote node (it might ask about stopping the local node, if it does then say you
want to stop the download or the local node, this is unless you want a public node but I'm doing this guide for remote nodes, so, yeah), then add click add remote node, once you have click this you will need to add the IP of your i2pd node, if your node is running on the same computer as the wallet the IP will be 127.0.0.1, and if its on a another computer it will be that computers IP. For the port number
it will be whatever you put in the tunnels.conf file under port, and for username and password, just leave blank. 

### Now you should be able to connect to the node, you can click the connect button in the button left, or in settings click the remote node you added, now your wallet should sync with the block chain thats running on that node, and your good to go. 
Thank you, and have a nice day. And now that you can make payments more anonymously, no ones going to know that you donated to i2pd. Hey just saying.
