Monero
==================

## Connect to nodes.

To connect Monero nodes, you have to configure the wallet software:

In [Feather wallet](http://rwzulgcql2y3n6os2jhmhg6un2m33rylazfnzhf56likav47aylq.b32.i2p/): File -> Settings -> Network -> Proxy > i2p, with this option all network traffic will be routed over i2p, including websocket connection (if enabled).

## Setup I2P service on already running public node.

Open wallet inteface (the "RPC") allows anyone to connect their wallets to Monero network through the node. This is useful for users who don't run their own nodes and connecting over i2p. 

If you already have a node with public RPC and now want to run i2p service, follow those steps:

1) Make sure the node is already configured for public use and running. (For an example you setted up `restricted-rpc=1`)

2) Configure i2pd to create HTTP server tunnel. Put in your ~/.i2pd/tunnels.conf file:

    [monero-node-rpc]
    type = http
    host = 127.0.0.1
    port = 18081 # Restricted RPC port
    keys = monero.dat

3) Restart i2pd or send `SIGHUP` signal.

4) Find b32 destination of the node.

   Go to webconsole -> [I2P tunnels page](http://127.0.0.1:7070/?page=i2p_tunnels). Look for Server tunnels and you will see address like \<long random string\>.b32.i2p next to monero-node-rpc.

   The node is now available in Invisible Internet. You can check the node's status by visiting \<long random string\>.b32.i2p/get_info.

6) (Optional) Register short and memorable .i2p domain on [reg.i2p](http://reg.i2p).

7) (Optional) Publish the node on [monero.fail](https://monero.fail).
