Anonymous chat servers
======================

## Connect to anonymous IRC server

You can connect to IRC servers in I2P by using **Socks proxy**. By default, it listens at ``127.0.0.1:4447`` 
(look at [configuration docs](../user-guide/configuration.md) for details).
Configure your IRC client to use this Socks proxy and connect to I2P servers just like to any other servers.

*Alternatively*, you may want to create **client I2P tunnel** to specific server.
This way, i2pd will "bind" IRC server port on your computer and you will be able to connect to server without modifying any IRC client settings.

To connect to IRC server at *irc.ilita.i2p:6667*, add this to ~/.i2pd/tunnels.conf:

    [IRC2]
    type = client
    address = 127.0.0.1
    port = 6669 
    destination = irc.ilita.i2p
    destinationport = 6667
    #keys = irc-client-key.dat

Restart i2pd, then connect to irc://127.0.0.1:6669 with your IRC client.

## Running anonymous IRC server

1) Run your IRC server software and find out which host:port it uses (for example, 127.0.0.1:5555).

   For small private IRC servers you can use [miniircd](https://github.com/jrosdahl/miniircd), for large public networks [UnreadIRCd](https://www.unrealircd.org/).

2) Configure i2pd to create IRC server tunnel.

   Simplest case, if your server does not support WebIRC, add this to ~/.i2pd/tunnels.conf:

```
[anon-chatserver]
type = irc
host = 127.0.0.1     
port = 5555
keys = chatserver-key.dat
```

   And that is it.

   Alternatively, if your IRC server supports WebIRC, for example, UnreadIRCd, put this into UnrealIRCd config:

```
webirc {
    mask 127.*.*.*;
    password your_password;
};
```

   Also change line:

    modes-on-connect "+ixw";

   to

    modes-on-connect "+iw";

   And this in ~/.i2pd/tunnels.conf:

    [anon-chatserver]
    type = irc
    host = 127.0.0.1
    port = 5555
    keys = chatserver-key.dat
    webircpassword = your_password

3) Restart i2pd.

4) Find b32 destination of your anonymous IRC server.

   Go to webconsole -> [I2P tunnels page](http://127.0.0.1:7070/?page=i2p_tunnels). Look for Sever tunnels and you will see address like \<long random string\>.b32.i2p next to anon-chatserver.

   Clients will use this address to connect to your server anonymously.

