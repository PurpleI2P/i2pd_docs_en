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

Deeper technical introduction of I2P can be found [here](https://geti2p.net/en/docs/how/tech-intro).

## How i2pd differs from original I2P implementation?

While [Java I2P](https://geti2p.net) is being original client for I2P network, i2pd has some big differences and advantages:

* Java I2P has built-in applications for torrents, e-mail and so on. i2pd is just a router which you can use with other software through I2CP interface.
* i2pd does not require Java. It's written in C++.
* i2pd consumes less memory and CPU.
* i2pd can be compiled everywhere gcc or clang presented (including Raspberry and routers).
* i2pd has some major optimizations for faster cryptography which leads to less consumption of processor time and energy.

## What can I use i2pd for?

i2pd can be used to organize anonymous network layer for any of your applications.

You can run websites, create public and private chatrooms, use instant 
messaging and E-Mail, share files -- do everything with strong privacy and 
security layer.

## Why is network so slow and unstable sometimes?

Because it's secure design, your network connection goes encrypted through a chain of 6 
random computers in the way to it's final destination. If one of those computers
is shut down, or experiences connectivity problems -- it can take some time for
I2P router to discover and fix that issue. 

## Help! i2pd is not working. What to do?

First of all, synchronize system clock on your machine with Internet.

If that does not help, on Linux machines, check the number of open file descriptors
allowed to a process. Run:

    ulimit -n

Correct value for regular i2pd node is 4096, for a floodfill it is 8192. 
Set it before you run i2pd like that:

    ulimit -n 4096 && ./i2pd

If that doesn't help, then you may have found a bug.
Contact developers with IRC or create an issue on GitHub.

## What is good tunnel creation success rate value?

Average values are 15% - 40%. Larger is better.

## Are there any alive I2P websites?

Sure, there is a list of alive websites [here](http://identiguy.i2p.xyz/)

## How can I better integrate my router to the network?

Edit your settings: set correct bandwidth and share rate. 

Run i2pd for a long time, download and seed some popular torrents.

## What browser should I use to browse I2P websites?

Use any open source browser - for example, Firefox or Chromium based. Create separate profile for I2P ([firefox instructions](https://support.mozilla.org/en-US/kb/profile-manager-create-and-remove-firefox-profiles)), try not to mix clearnet browsing with I2P. Learn how to configure your browser for better privacy and security.

Good idea is to configure [privoxy](https://wiki.archlinux.org/index.php/Privoxy) for I2P/onion/clearnet browsing at the same time.

i2pd socks proxy has an option to pass all non-i2p traffic to Tor socks proxy. Make sure you know what are you doing!

## What is a floodfill mode?

Floodfill mode is a special mode, which contributes to I2P network more.
You may want to enable floodfill mode if you have stable uptime and high bandwidth
to share.

## How is I2P different from Tor?

I2P and Tor has some similarities, but they are completely different in every way possible.

Tor is designed to act as anonymous proxy for the regular Internet, I2P is 
designed to create anonymous network layer with its own private resources.

Tor Project was started by US military and has a long history of receiving funds
from the government, while I2P was started by community of independent civilians.

Tor is highly centralized by design, while I2P is decentralized and distributed.

## Can use i2pd as a proxy for regular Internet?

Not out of the box. You better use [Tor](https://www.torproject.org/) for that.

