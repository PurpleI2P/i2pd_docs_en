i2pd configuration
==================

i2pd can be configured via either command-line arguments or config files.
Modifying `i2pd.conf` is just a way of passing command line arguments to i2pd at boot;
for example, running i2pd with argument `--port=10123` and setting option `port = 10123` in config file will have
the same effect.

There are two separate config files: `i2pd.conf` and `tunnels.conf`. `i2pd.conf` is the main configuration file, where
you configure all options. `tunnels.conf` is the tunnel configuration file, where you configure I2P hidden services
and client tunnels. the `tunnels.conf` options are documented [here](tunnels.md).

`i2pd.conf` accepts INI-like syntax, such as the following : <key> = <value>.
Comments are "#", not ";" as you might expect. See [boost ticket](https://svn.boost.org/trac/boost/ticket/808)
All command-line parameters are allowed as keys, but for those which contains dot (.), there is a special syntax.

For example:

i2pd.conf:

    # comment
    log = true # use stdout (default)
    ipv6 = true
    # settings for specific module
    [httpproxy]
    port = 4444
    # ^^ this will be --httproxy.port= in cmdline
    # another comment
    [sam]
    enabled = true

You are also encouraged to see the commented config with examples of all options in ``contrib/i2pd.conf``.

Options specified on the command line take precedence over those in the config file.
If you are upgrading your very old router (< 2.3.0) see also [this](config_opts_after_2.3.0.md) page.

Available options
-----------------

Run `./i2pd --help` to show builtin help message (default value of option will be shown in braces)

### General options

Option                                 | Description
-------------------------------------- | --------------------------------------
conf                                   | Config file (default: ~/.i2pd/i2pd.conf or /var/lib/i2pd/i2pd.conf). This parameter will be silently ignored if the specified config file does not exist.
tunconf                                | Tunnels config file (default: ~/.i2pd/tunnels.conf or /var/lib/i2pd/tunnels.conf)
pidfile                                | Where to write pidfile (default: i2pd.pid, not used in Windows)
log                                    | Logs destination: stdout, file, syslog (stdout if not set or invalid) (if daemon, stdout/unspecified are replaced by file in some cases)
logfile                                | Path to logfile (default - autodetect)
loglevel                               | Log messages above this level (debug, info, warn, error, none; default - warn)
logclftime                             | Write full CLF-formatted date and time to log (default: false (write only time))
datadir                                | Path to storage of i2pd data (RouterInfos, destinations keys, peer profiles, etc ...)
host                                   | Router external IP for incoming connections (default: auto if SSU2 is enabled)
port                                   | Port to listen for incoming connections (default: auto (random))
daemon                                 | Router will go to background after start (default: true)
service                                | Router will use system folders like '/var/lib/i2pd' (on unix) or 'C:\ProgramData\i2pd' (on Windows). Ignored on MacOS and Android (default: false)
ifname                                 | Network interface to bind to
ifname4                                | Network interface to bind to for IPv4
ifname6                                | Network interface to bind to for IPv6
address4                               | Local address to bind to for IPv4
address6                               | Local address to bind to for clearnet IPv6
nat                                    | If true, assume we are behind NAT (default: true)
ipv4                                   | Enable communication through IPv4 (default: true)
ipv6                                   | Enable communication through clearnet IPv6 (default: false)
notransit                              | Router will not accept transit tunnels, disabling transit traffic completely. G router cap will be published (default: false)
floodfill                              | Router will be floodfill (default: false)
bandwidth                              | Bandwidth limit: integer in (kilobytes per second) or letters: L (32), O (256), P (2048), X (unlimited).
share                                  | Max % of bandwidth limit for transit. 0-100 (default: 100)
family                                 | Name of a family, router belongs to
netid                                  | Network ID, router belongs to. Main I2P is 2.

#### Notes

`datadir` and `service` options are only used as arguments for i2pd, these options have no effect when set in `i2pd.conf`.

### NTCP2

Option                                 | Description
-------------------------------------- | --------------------------------------
ntcp2.enabled                          | Enable NTCP2 (default: true)
ntcp2.published                        | Enable incoming NTCP2 connections (default: true)
ntcp2.port                             | Port to listen for incoming NTCP2 connections (default: auto - port from general section)
ntcp2.addressv6                        | External IPv6 for incoming connections
ntcp2.proxy                            | Specify proxy server for NTCP2. Should be http://address:port or socks://address:port

### SSU2

Option                                 | Description
-------------------------------------- | --------------------------------------
ssu2.enabled                           | Enable SSU2 (default: true)
ssu2.published                         | Enable incoming SSU2 connections. (default: true)
ssu2.port                              | Port to listen for incoming SSU2 connections (default: auto - 'port' from general section)
ssu2.proxy                             | Specify UDP socks5 proxy server for SSU2. Should be socks://address:port  
ssu2.mtu4                              | MTU for local ipv4. (default: auto)
ssu2.mtu6                              | MTU for local ipv6. (default: auto)
    
All options below still possible in cmdline, but better write it in config file:

### HTTP webconsole

Option                                 | Description
-------------------------------------- | --------------------------------------
http.enabled                           | If webconsole is enabled. (default: true)
http.address                           | The address to listen on (HTTP server)
http.port                              | The port to listen on (HTTP server) (default: 7070)
http.auth                              | Enable basic HTTP auth for webconsole (default: false)
http.user                              | Username for basic auth (default: i2pd)
http.pass                              | Password for basic auth (default: random, see logs)
http.strictheaders                     | Enable strict host checking on WebUI. (default: true)
http.hostname                          | Expected hostname for WebUI (default: localhost)

### HTTP proxy

Option                                 | Description
-------------------------------------- | --------------------------------------
httpproxy.enabled                      | If HTTP proxy is enabled. (default: true)
httpproxy.address                      | The address to listen on (HTTP Proxy)
httpproxy.port                         | The port to listen on (HTTP Proxy) (default: 4444)
httpproxy.addresshelper                | Enable address helper (jump). (default: true)
httpproxy.keys                         | Optional keys file for HTTP proxy local destination
httpproxy.signaturetype                | Signature type for new keys if keys file is set. (default: 7)
httpproxy.inbound.length               | Inbound tunnels length if keys is set. (default: 3)
httpproxy.inbound.quantity             | Inbound tunnels quantity if keys is set. (default: 5)
httpproxy.inbound.lengthVariance       | Inbound tunnels length variance if keys is set. (default: 0)
httpproxy.outbound.length              | Outbound tunnels length if keys is set. (default: 3)
httpproxy.outbound.quantity            | Outbound tunnels quantity if keys is set. (default: 5)
httpproxy.outbound.lengthVariance      | Outbound tunnels length variance if keys is set. (default: 0)
httpproxy.outproxy                     | HTTP proxy upstream out proxy url (like http://exit.stormycloud.i2p)
httpproxy.addresshelper                | Enable or disable addresshelper. (default: true)
httpproxy.senduseragent                | Pass through user's User-Agent if enabled. (deafult: false)
httpproxy.i2cp.leaseSetType            | Type of LeaseSet to be sent. 1, 3 or 5. (default: 3)
httpproxy.i2cp.leaseSetEncType         | Comma separated encryption types to be used in LeaseSet type 3 or 5
httpproxy.i2p.streaming.profile        | HTTP Proxy bandwidth usage profile. 1 - bulk(high), 2- interactive(low). (default: 1)

### Socks proxy

Option                                 | Description
-------------------------------------- | --------------------------------------
socksproxy.enabled                     | If SOCKS proxy is enabled. (default: true)
socksproxy.address                     | The address to listen on (SOCKS Proxy)
socksproxy.port                        | The port to listen on (SOCKS Proxy). (default: 4447)
socksproxy.keys                        | Optional keys file for SOCKS proxy local destination
socksproxy.signaturetype               | Signature type for new keys if keys file is set. (default: 7)
socksproxy.inbound.length              | Inbound tunnels length if keys is set. (default: 3)
socksproxy.inbound.quantity            | Inbound tunnels quantity if keys is set. (default: 5)
socksproxy.inbound.lengthVariance      | Inbound tunnels length variance if keys is set. (default: 0)
socksproxy.outbound.length             | Outbound tunnels length if keys is set. (default: 3)
socksproxy.outbound.quantity           | Outbound tunnels quantity if keys is set. (default: 5)
socksproxy.outbound.lengthVariance     | Outbound tunnels length variance if keys is set. (default: 0)
socksproxy.outproxy.enabled            | Enable or disable SOCKS5 outproxy. (default: false)
socksproxy.outproxy                    | Address of outproxy(IP or local). Requests outside I2P will go there.
socksproxy.outproxyport                | Outproxy port. If 0 socksproxy.outproxy contains path to local socket
socksproxy.i2cp.leaseSetType           | Type of LeaseSet to be sent. 1, 3 or 5. (default: 3)
socksproxy.i2cp.leaseSetEncType        | Comma separated encryption types to be used in LeaseSet type 3 or 5
socksproxy.i2p.streaming.profile       | SOCKS Proxy bandwidth usage profile. 1 - bulk(high), 2- interactive(low). (deafult: 1)

### SAM interface

Option                                 | Description
-------------------------------------- | --------------------------------------
sam.enabled                            | If SAM is enabled. (default: true)
sam.address                            | The address to listen on (SAM bridge)
sam.port                               | Port of SAM bridge. Usually 7656. SAM is off if not specified
sam.singlethread                       | If false every SAM session runs in own thread. (default: true)

### BOB interface

Option                                 | Description
-------------------------------------- | --------------------------------------
bob.enabled                            | If BOB is enabled. (default: false)
bob.address                            | The address to listen on (BOB command channel)
bob.port                               | Port of BOB command channel. Usually 2827. BOB is off if not specified

### I2CP interface

Option                                 | Description
-------------------------------------- | --------------------------------------
i2cp.enabled                           | If I2CP is enabled. (default: true)
i2cp.address                           | The address to listen on or an abstract address for Android LocalSocket
i2cp.port                              | Port of I2CP server. Usually 7654. Ignored for Andorid
i2cp.singlethread                      | If false every I2CP session runs in own thread. (default: true)
i2cp.inboundlimit                      | Client inbound limit in KBps to return in BandwidthLimitsMessage. Router's bandwidth if 0. (default:0)
i2cp.outboundlimit                     | Client outbound limit in KBps to return in BandwidthLimitsMessage. Router's bandwidth if 0. (default:0)

### I2PControl interface

Option                                 | Description
-------------------------------------- | --------------------------------------
i2pcontrol.enabled                     | If I2P control is enabled. (default: false)
i2pcontrol.address                     | The address to listen on (IP or path to local socket)
i2pcontrol.port                        | Port of I2P control service. Usually 7650. If 0 i2pcontrol.address contains path to local socket
i2pcontrol.password                    | I2P control authentication password. (default: itoopie)
i2pcontrol.cert                        | I2P control HTTPS certificate file name. (default: i2pcontrol.crt.pem)
i2pcontrol.key                         | I2P control HTTPS certificate key file name. (default: i2pcontrol.key.pem)

### UPNP

Option                                 | Description
-------------------------------------- | --------------------------------------
upnp.enabled                           | Enable or disable UPnP, false by default for CLI and true for GUI (Windows, Android)
upnp.name                              | Name i2pd appears in UPnP forwardings list. (default: I2Pd)

### Cryptography

Option                                 | Description
-------------------------------------- | --------------------------------------
precomputation.elgamal                 | Use ElGamal precomputated tables. (default: false for x86-64 and true for other platforms)

### Reseeding

Option                                 | Description
-------------------------------------- | --------------------------------------
reseed.verify                          | Verify .su3 signature. (default: false)
reseed.urls                            | Reseed URLs, separated by comma
reseed.yggurls                         | Reseed Yggdrasil's URLs, separated by comma
reseed.file                            | Path to local .su3 file or HTTPS URL to reseed from
reseed.zipfile                         | Path to local .zip file to reseed from
reseed.threshold                       | Minimum number of known routers before requesting reseed. (default: 25)
reseed.proxy                           | Url for https/socks reseed proxy

### Addressbook options

Option                                 | Description
-------------------------------------- | --------------------------------------
addressbook.enabled                    | Enable or disable AddressBook. (default: true)  
addressbook.defaulturl                 | AddressBook subscription URL. Only used to initialize the AddressBook.
addressbook.subscriptions              | AddressBook subscriptions URLs, separated by comma. Note that defaulturl is not added to subscriptions URLs
addressbook.hostsfile                  | File to dump AddressesBook in hosts.txt format

### Limits

Option                                 | Description
-------------------------------------- | --------------------------------------
limits.transittunnels                  | Override maximum number of transit tunnels. (default: 10000)
limits.openfiles                       | Limit number of open file descriptors (default: 0 - use system limit)
limits.coresize                        | Maximum size of corefile in Kb (default: 0 - use system limit)
limits.zombies                         | Minimum percentage of successfully created tunnels under which tunnel cleanup is paused (default [%]: 0.00)

### Trust options

Option                                 | Description
-------------------------------------- | --------------------------------------
trust.enabled                          | Enable explicit trust options. (default: false)
trust.family                           | Make direct I2P connections only to routers in specified Family.
trust.routers                          | Make direct I2P connections only to routers specified here. Comma separated list of base64 identities.
trust.hidden                           | Should we hide our router from other routers? (default: false)

### Exploratory tunnels

Option                                 | Description
-------------------------------------- | --------------------------------------
exploratory.inbound.length             | Exploratory inbound tunnels length. (default: 2)
exploratory.inbound.quantity           | Exploratory inbound tunnels quantity. (default: 3)
exploratory.outbound.length            | Exploratory outbound tunnels length. (default: 2)
exploratory.outbound.quantity          | Exploratory outbound tunnels quantity. (default: 3)

### Shared local destination

Option                                 | Description
-------------------------------------- | --------------------------------------
shareddest.inbound.length              | Shared local destination inbound tunnels length. (default: 3)
shareddest.inbound.quantity            | Shared local destination inbound tunnels quantity. (default: 3)
shareddest.outbound.length             | Shared local destination outbound tunnels length. (default: 3)
shareddest.outbound.quantity           | Shared local destination outbound tunnels quantity. (default: 3)
shareddest.i2cp.leaseSetType           | Shared local destination's LeaseSet type. (default: 3)
shareddest.i2cp.leaseSetEncType        | Shared local destination's LeaseSet encryption type. (default: 0,4)
shareddest.i2p.streaming.profile       | Shared local destination bandwidth usage profile. (default: 2)

### Time sync

Option                                 | Description
-------------------------------------- | --------------------------------------
nettime.enabled                        | Enable NTP sync. (default: false)
nettime.ntpservers                     | Comma-separated list of NTP server. (default: pool.ntp.org)
nettime.ntpsyncinterval                | NTP time sync interval in hours. (default: 72)

### Network information persist

Option                                 | Description
-------------------------------------- | --------------------------------------
persist.profiles                       | Enable peer profile persisting to disk. (default: true)

### Meshnets transports

Option                                 | Description
-------------------------------------- | --------------------------------------
meshnets.yggdrasil                     | Support transports through the Yggdrasil (default: false)
meshnets.yggaddress                    | Local Yggdrasil's address to publish

### Windows-specific options

Option                                 | Description
-------------------------------------- | --------------------------------------
insomnia                               | Prevent system from sleeping
close                                  | Action on close: minimize, exit, ask

### UNIX-specific options

Option                                 | Description
-------------------------------------- | --------------------------------------
unix.handle_sigtstp                    | Handle SIGTSTP and SIGCONT signals. (default: false)

`handle_sigtstp` enables handling of SIGTSTP and SIGCONT signals (*since 2.43.0*).

SIGTSTP can be called using Ctrl+Z in terminal with running i2pd in it or by `kill` command. When signal recveived, i2pd will switch to offline mode and stop sending traffic and cleaning of netDb. All active tunnels will be frozen.

To restore network connectivity you need to send SIGCONT signal to the proccess. Tunnels will be cleared if they should expire during the frozen state and new ones will be built.

Local addressbook
-----------------

There is also a special addressbook config file in the working directory at `addressbook/local.csv`.
It lets your routers to work as a resolver for 3ld domains if you run that 2ld address on your router and it' presented in addressbook.
Only i2pd can resolve such addresses, by sending special datagram requests.
