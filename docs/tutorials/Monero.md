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
to Advanced mode, then log back into your wallet, it should have saved your keys, but in case it didn't please use your keys to log back in
