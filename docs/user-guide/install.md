Installing
==========

Building from source
--------------------

See [developer section documentation](../devs/building/requirements.md) for how to build i2pd from source on your OS.


Windows, Android, Mac OS X  
--------------------------

The easiest way to install i2pd is by using precompiled binaries. 
Go to the [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest) and choose a file for your operating system.


## Docker images

You can use our [prebuilt docker images](https://hub.docker.com/r/purplei2p/i2pd/).

    docker pull purplei2p/i2pd


## Ubuntu

You can install binary packages from the [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest). 

Alternatively, you can use [PPA repository](https://launchpad.net/~purplei2p/+archive/ubuntu/i2pd) or repository provided [below](#debian), run by PurpleI2P community member [R4SAS](https://twitter.com/i2pr4sas).

    sudo add-apt-repository ppa:purplei2p/i2pd
    sudo apt-get update
    sudo apt-get install i2pd


## Debian

Look for Debian packages at the [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest).

Alternatively, you can install i2pd by using repository run by PurpleI2P community member [R4SAS](https://twitter.com/i2pr4sas).

Install apt-transport-https package

    sudo apt-get install apt-transport-https

Automaticly add repository

    wget -q -O - https://repo.i2pd.xyz/.help/add_repo | sudo bash -s -

After that you can install i2pd as any other software package:

    apt-get update
    apt-get install i2pd

Look for more information about Debian repository [here](https://repo.i2pd.xyz/.help/readme.txt).

## Fedora/CentOS

You can install i2pd from [repository](https://copr.fedorainfracloud.org/coprs/supervillain/i2pd/) 
run by PurpleI2P community member [villain](https://twitter.com/el_villano_loco).

### Centos 7:

    curl -s https://copr.fedorainfracloud.org/coprs/supervillain/i2pd/repo/epel-7/supervillain-i2pd-epel-7.repo -o /etc/yum.repos.d/i2pd-epel-7.repo
    yum install epel-release -y
    yum install i2pd -y
    systemctl enable i2pd
    systemctl start i2pd

### Fedora:

    dnf copr enable supervillain/i2pd
    dnf install i2pd -y
    systemctl enable i2pd
    systemctl start i2pd


## ArchLinux

i2pd packages are available at AUR: [release version](https://aur.archlinux.org/packages/i2pd/),
[nightly builds](https://aur.archlinux.org/packages/i2pd-git/)

## Gentoo Linux

Install from Gentoo repository: [i2pd package](https://packages.gentoo.org/packages/net-vpn/i2pd).


FreeBSD
-------

You can install i2pd from [ports](https://www.freshports.org/security/i2pd/).

