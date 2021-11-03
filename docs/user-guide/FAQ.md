## What is I2P?

I2P (Invisible Internet Protocol) is a universal anonymous network layer. 
All communications over I2P are anonymous and end-to-end encrypted, participants
don't reveal their real IP addresses. 

I2P client is a software used for building and using anonymous I2P 
networks. Such networks are commonly used for anonymous peer-to-peer 
applications (filesharing, cryptocurrencies) and anonymous client-server 
applications (websites, instant messengers, chat-servers).

I2P allows people from all around the world to communicate and share information
without restrictions.

A deeper technical introduction of I2P can be found [here](https://geti2p.net/en/docs/how/tech-intro).

## How i2pd differs from original I2P implementation?

While [Java I2P](https://geti2p.net) and i2pd are both clients for the I2P network, i2pd has some big differences and advantages:

* Java I2P has built-in applications for torrents, e-mail and so on. i2pd is just a router which you can use with other software through I2CP interface.
* i2pd does not require Java. It's written in C++.
* i2pd consumes less memory and CPU.
* i2pd can be compiled everywhere gcc or clang presented (including Raspberry and routers).
* i2pd has some major optimizations for faster cryptography which leads to less consumption of processor time and energy.

## Why is the I2P network so slow and unstable sometimes?

By design, in the I2P network, your connection gets encrypted through a chain of 6 
random computers in the way to its final destination. If one of those computers
is shut down, or experiences connectivity problems, it can take some time for your
I2P router to discover and fix that issue. 

## Help! i2pd is not working. What do I do?

First of all, synchronize system clock on your machine with Internet.

If that does not help, on Linux machines, check the number of open file descriptors
allowed to a process. In a terminal, run:

    ulimit -n

Correct value for regular i2pd node is 4096, for a floodfill it is 8192. 
Set it before you run i2pd like so:

    ulimit -n 4096 && ./i2pd

If that doesn't help, then you may have found a bug.
Contact developers with IRC or create an issue on GitHub.

## What is good tunnel creation success rate value?

Average values are 15% - 40%. Larger is better.

## Is there a place I can use to find running I2P websites?

Sure, there is a list [here](http://identiguy.i2p.xyz/)

## How can I better integrate my router to the network?

Edit your settings: set correct bandwidth and share rate. 

Run i2pd for a long time, download and seed some popular torrents.

## What browser should I use to browse I2P websites?

Use any open source browser - for example, Firefox or a Chromium based alternative. Create separate profile for I2P ([firefox instructions](https://support.mozilla.org/en-US/kb/profile-manager-create-and-remove-firefox-profiles)), 
and try not to mix clearnet browsing with I2P. Learn how to configure your browser for better privacy and security.

Configuring [privoxy](https://wiki.archlinux.org/index.php/Privoxy) for I2P/onion/clearnet browsing at the same time is a good idea.

i2pd's socks proxy has an option to pass all non-I2P traffic to the Tor socks proxy. Make sure you know what are you doing when using this!

## What is a floodfill mode?

Floodfill mode is a special mode, which contributes to the I2P network more.
You may want to enable floodfill mode if you have stable uptime and high bandwidth
to share.

## How is I2P different from Tor?

I2P and Tor have some similarities, but they are completely different technologies.

Tor is designed to act as an anonymous proxy for the regular Internet, and I2P 
is specifically designed to make a virtual anonymous network for hidden services 
and p2p applications. 

Tor Project was started by the US military and receives most of its funds
from the government, while I2P was started by a community of independent civilians.

Tor is highly centralized by design, while I2P is designed to be decentralized and distributed.

## Can use i2pd as a proxy for regular Internet?

Not out of the box. You better use [Tor](https://www.torproject.org/) for that.

