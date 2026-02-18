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

[Reload tunnels config](http://127.0.0.1:7070/?page=commands) or restart i2pd, then connect to irc://127.0.0.1:6669 with your IRC client.

### Recommended for better anonymity:
Disable all `CTCP` replies/responses and `DCC` (send/receive) in your IRC client settings.

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

3) Securing UnrealIRCd

   By default if you run an I2Pd service, I2P will connect to the IRCd at localhost using IP 127.0.0.1
   
   This is bad for two reasons:
   
   First, you would be unable to separate I2P traffic from other localhost traffic.
   Second, all I2P users would be unbanable because 127.0.0.1 is exempt from all bans, including glines.

   So, we can fake host to separate localhost traffic from i2pd traffic.

   To do this, we will create the directory that UnrealIRCd will access and create the socket file:

```
    mkdir /etc/i2pd/unrealircd
    chown unrealircd:unrealircd /etc/i2pd/unrealircd
    chmod 750 /etc/i2pd/unrealircd
```
   NOTE: This assumes your IRCd user is called unrealircd. If not, change the unrealircd:unrealircd in the chown command of above.

   If you are on Debian/Ubuntu and have AppArmor installed (you probably do!) then run the next few commands. If you don't do this then everything will fail mysteriously later.

   Still as root, run:
```
    echo "/etc/i2pd/unrealircd/ip2d_ircd.socket rw," >>/etc/apparmor.d/local/system_i2pd
    apparmor_parser -r /etc/apparmor.d/system_i2pd
```

   Configure UnrealIRCd, adding this to your unrealircd.conf file:
```
    listen {
    file "/etc/i2pd/unrealircd/i2pd_ircd.socket";
    mode 0777;
    spoof-ip 127.0.0.3;
    }
```

   And to turn off ban checking:
```
   except ban {
    mask { ip 127.0.0.3; }
    type { blacklist; connect-flood; maxperip; handshake-data-flood; }
    }
```
   
   We will create a communication that act like bridge between a TCP/IP on port 5555 and UNIX socket located at "/etc/i2pd/unrealircd/i2pd_ircd.socket".

   Just run:
```
    socat TCP-LISTEN:5555,bind=localhost,reuseaddr,fork UNIX-CONNECT:/etc/i2pd/unrealircd/i2pd_ircd.socket &
```
   This way, when users connecting on I2P tunnel client address, they will be redirect to 127.0.0.1:5555 that will bridge to Unix Socket created by UnrealIrcd, that come up with an IP 127.0.0.3 and exempt them from ban checking.

4) Restart i2pd.

5) Find b32 destination of your anonymous IRC server.

   Go to webconsole -> [I2P tunnels page](http://127.0.0.1:7070/?page=i2p_tunnels). Look for Sever tunnels and you will see address like \<long random string\>.b32.i2p next to anon-chatserver.

   Clients will use this address to connect to your server anonymously.
   