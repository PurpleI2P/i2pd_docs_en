Using RetroShare with i2pd
==========================

RetroShare is a rich set of P2P applications, which can work on top of anonymous I2P network.
Quoting [Wikipedia](https://en.wikipedia.org/wiki/RetroShare):

> RetroShare is open source software for encrypted filesharing, serverless email, instant messaging, online chat, and BBS, based on a friend-to-friend network built on GNU Privacy Guard (GPG).

In order to make RetroShare work with i2pd, you need to create a new server tunnel in [tunnels.conf](../user-guide/tunnels.md) like this: 

    [retroshare]  
    type=server  
    host=127.0.0.1  
    port=10123            # pick random port of your choice 
    keys=retroshare.dat  

Make sure that Socks proxy is enabled, it listens at ``127.0.0.1:4447`` by default. If not, look at [i2pd.conf documentation](../user-guide/configuration.md).

Restart i2pd with new configuration. 

Find .b32.i2p address of your RetroShare node.
Go to webconsole -> [I2P tunnels page](http://127.0.0.1:7070/?page=i2p_tunnels). Look for Sever tunnels and you will see address like \<long random string\>.b32.i2p next to "retroshare".

Now when everything is prepared, you can run RetroShare itself and create a new profile.
Mark it as **hidden** and enter your **.b32.i2p address** and **port** you have specified at tunnels.conf.

More information about RetroShare at their [website](http://retroshare.cc).
