Using RetroShare with i2pd
==========================


Retroshare is I2P capable.  
Their [configuration instructions](https://retroshare.readthedocs.io/en/latest/tutorial/i2p-hidden-rs-node/) for Java I2P.
In order to do it for i2pd, you must create a new server tunnels in tunnels.conf like this  

[Retroshare]  
type=server  
host=127.0.0.1  
port="port you wish to use"  
keys=retroshare.dat  

You can specify [more options](https://github.com/PurpleI2P/i2pd/wiki/tunnels.conf)  
Also keep SOCKS proxy enabled. It's enabled by default.

Start i2pd with this new tunnels, and get it's .b32.i2p address from http://127.0.0.1:7070/?page=i2p_tunnels.
Now you are ready to create hidden profile for Retroshare.
Mark it as hidden and enter your .b32.i2p and port you specified for tunnel.

