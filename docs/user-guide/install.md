Installing
==========

## Building from source

See [developer section documentation](../devs/building/requirements.md) for how to build i2pd from source on your OS.


## Installation from prebuilt packages

The easiest way to install i2pd is by using precompiled packages and binaries.
Go to the [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest) and choose a file for your operating system.


## Windows

Check for [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest) and choose a file for your system architecture:
* `i2pd_*_win32_mingw.zip` -- for x86 systems
* `i2pd_*_win64_mingw.zip` -- for x86_64 (x64) systems
* `i2pd_*_winxp_mingw.zip` -- Windows XP compatible build
* `setup_i2pd_*.exe` -- Installer with automatic detection of used system and correct extraction of a configuration files


## Android

You can get application on F-Droid:

[<img src="https://fdroid.gitlab.io/artwork/badge/get-it-on.png"
     alt="Get it on F-Droid"
     height="80">](https://f-droid.org/packages/org.purplei2p.i2pd/)

Alternatively, you can install i2pd by using F-Droid repository run by PurpleI2P community member [R4SAS](https://twitter.com/i2pr4sas): [repository homepage](https://fdroid.i2pd.xyz/)

Also, packages can be found on i2pd-android repository [releases page](https://github.com/PurpleI2P/i2pd-android/releases/latest)


## Docker images

You can use our [prebuilt docker images](https://hub.docker.com/r/purplei2p/i2pd/).

    docker pull purplei2p/i2pd


## Linux

### Distro-agnostic GUI

You can install i2pd GUI from Flathub using `flatpak`:

    sudo apt install flatpak
    flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
    flatpak install flathub website.i2pd.i2pd

### Arch

i2pd packages are available at Arch's [extra repository](https://archlinux.org/packages/extra/x86_64/i2pd/) for release version, and [AUR](https://aur.archlinux.org/packages/i2pd-git/) for nightly builds.

You can download them by running 

     pacman -S i2pd --noconfirm

### Gentoo

i2pd [has a working ebuild in the main gentoo repository](https://packages.gentoo.org/packages/net-vpn/i2pd). As of May 2018, the ebuild 
is still listed as "unstable", and thus will request an exception in your package.keywords if you are using gentoo under the "stable" branch.
To install i2pd, enter the command:
    
    emerge --ask net-vpn/i2pd

If you use `gcc` to compile packages and would like to enable cmake's hardening features, use the `i2p-hardening` flag (recommended).
If you intend to use the websocket server, enable the `websocket` flag.

### Debian

Look for Debian packages at the [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest).

Alternatively, you can install i2pd by using repository run by PurpleI2P community member [R4SAS](https://twitter.com/i2pr4sas).

Install apt-transport-https package

    sudo apt-get install apt-transport-https

Automatically add repository

    wget -q -O - https://repo.i2pd.xyz/.help/add_repo | sudo bash -s -

After that you can install i2pd as any other software package:

    sudo apt-get update
    sudo apt-get install i2pd

Look for more information about Debian repository [here](https://repo.i2pd.xyz/.help/readme.html).

### Fedora/CentOS

You can install i2pd from [repository](https://copr.fedorainfracloud.org/coprs/supervillain/i2pd/) 
run by PurpleI2P community member [villain](https://twitter.com/el_villano_loco) and maintained by [R4SAS](https://twitter.com/i2pr4sas).

#### Centos 7:

    curl -s https://copr.fedorainfracloud.org/coprs/supervillain/i2pd/repo/epel-7/supervillain-i2pd-epel-7.repo -o /etc/yum.repos.d/i2pd-epel-7.repo
    yum install epel-release -y
    yum install i2pd -y
    systemctl enable --now i2pd

#### Fedora:

    dnf copr enable supervillain/i2pd
    dnf install i2pd -y
    systemctl enable --now i2pd

### Ubuntu

You can install binary packages from the [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest). 

Alternatively, you can use [PPA repository](https://launchpad.net/~purplei2p/+archive/ubuntu/i2pd) or repository provided [below](#debian), run by PurpleI2P community member [R4SAS](https://twitter.com/i2pr4sas).

    sudo add-apt-repository ppa:purplei2p/i2pd
    sudo apt-get update
    sudo apt-get install i2pd

## FreeBSD

You can install i2pd from [ports](https://www.freshports.org/security/i2pd/).

## MacOS X

You can install i2pd from [brew](https://brew.sh/) package manager:

    brew install i2pd

or use statically precompiled binary from [latest release page](https://github.com/PurpleI2P/i2pd/releases/latest).

