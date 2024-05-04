Monero
==================

## Connect to remote nodes.

To connect Monero nodes, you have to configure the wallet software:

In [Feather wallet](http://rwzulgcql2y3n6os2jhmhg6un2m33rylazfnzhf56likav47aylq.b32.i2p/): File -> Settings -> Network -> Proxy > i2p, with this option all network traffic will be routed over i2p, including websocket connection (if enabled).

## Connection between other I2P nodes.

Warning: The usage of I2P network is still considered experimental;

Sync the blockchain over I2P isn't supported and there are a few pessimistic cases where privacy is leaked.

The design is intended to maximize privacy of the source of a transaction by broadcasting it over an anonymity network, while relying on clearnet for the remainder of messages to make surrounding node attacks (via sybil) more difficult. 


1) Configure i2pd to create server type tunnel and put in your `./tunnels.conf` file:

    [monero-node]
    type = server
    host = 127.0.0.1
    port = 18085 # Anonymous inbound port
    keys = monero-node.dat

2) Restart i2pd or send `SIGHUP` signal.

3) Find your b32 address of the node.

   Go to webconsole -> [I2P tunnels page](http://127.0.0.1:7070/?page=i2p_tunnels). Look for Server tunnels and you will see address like \<long random string\>.b32.i2p next to monero-node.
   Or get with curl: `curl http://127.0.0.1:7070/?page=i2p_tunnels 2>&1 | grep -Eo "[a-zA-Z0-9./?=_%:-]*" | grep "18085"`

   The node is now available in Invisible Internet. You can check the node's status by visiting \<long random string\>.b32.i2p/get_info.

4) Update your monero.conf file;

    anonymous-inbound=XXX.b32.i2p,127.0.0.1:18085
    tx-proxy=i2p,127.0.0.1:4447,disable_noise

Replace XXX with your b32 address.


## Setup I2P service on already running public node.

Open wallet inteface (the "RPC") allows anyone to connect their wallets to Monero network through the node. This is useful for users who don't run their own nodes and connecting over i2p. 

If you already have a node with public RPC and now want to run i2p service, follow those steps:

1) Make sure the node is already configured for public use and running. (For an example you setted up `monero.conf` such as `public-node=1` with `restricted-rpc=1`)

2) Configure i2pd to create server type tunnel and put in your `./tunnels.conf` file:

    [monero-node-rpc]
    type = server
    host = 127.0.0.1
    port = 18089 # Restricted RPC port
    keys = monero-node-rpc.dat # or same key file with P2P if you want one and same address.

3) Restart i2pd or send `SIGHUP` signal.

4) Find your b32 address of the node.

   Go to webconsole -> [I2P tunnels page](http://127.0.0.1:7070/?page=i2p_tunnels). Look for Server tunnels and you will see address like \<long random string\>.b32.i2p next to monero-node.
   Or get with curl: `curl http://127.0.0.1:7070/?page=i2p_tunnels 2>&1 | grep -Eo "[a-zA-Z0-9./?=_%:-]*" | grep "18089"`

   The node is now available in Invisible Internet. You can check the node's status by visiting \<long random string\>.b32.i2p/get_info.

6) (Optional) Register short and memorable .i2p domain on [reg.i2p](http://reg.i2p).

7) (Optional) Publish the node on [monero.fail](https://monero.fail).
