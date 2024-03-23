Building on Unix systems
=============================

This doc is trying to cover:

* [Debian/Ubuntu](#debian-ubuntu) (contains packaging instructions)
* [Fedora/Centos](#fedora-centos)
* [Mac OS X](#mac-os-x)
* [FreeBSD](#freebsd)

Make sure you have all required dependencies for your system successfully installed.
See [this](requirements.md) page for common requirements.

If so then we are ready to go!
Let's clone the repository and start building the i2pd:

	git clone https://github.com/PurpleI2P/i2pd.git

Generic build process looks like this (with cmake):

	cd i2pd/build
	cmake <cmake options> . # see "CMake Options" section below
	make                    # you may add VERBOSE=1 to cmdline for debugging

..or with quick-and-dirty way with just make:

	cd i2pd/
	make

Then run the tests:

	cd i2pd/tests/
	make

After successful build i2pd could be installed with:

	make install

CMake Options
-------------

Available CMake options(each option has a form of `-D<key>=<value>`, for more information see `man 1 cmake`):

* `CMAKE_BUILD_TYPE` build profile (Debug/Release, default: no optimization or debug symbols)
* `WITH_BINARY`      build i2pd itself (default: ON)
* `WITH_LIBRARY`     build libi2pd (default: ON)
* `WITH_STATIC`      build static versions of library and i2pd binary (default: OFF)
* `WITH_UPNP`        build with UPnP support (requires libminiupnp, default: OFF)
* `WITH_AESNI`       build with AES-NI support (default: ON)
* `WITH_HARDENING`   enable hardening features (gcc only, default: OFF)
* `WITH_MESHNET`     build for cjdns test network (makes impossible to use the application with the main network, default: OFF)
* `WITH_ADDRSANITIZER`   build with Address Sanitizer (default: OFF)
* `WITH_THREADSANITIZER` build with Thread Sanitizer (default: OFF)


Also there is `-L` flag for CMake that could be used to list current cached options:

	cmake -L

Debian/Ubuntu
-------------

You will need a compiler and other tools that could be installed with `build-essential`, `debhelper` and `cmake` packages:

	sudo apt-get install build-essential debhelper cmake

Also you will need a bunch of development libraries:

	sudo apt-get install \
	    libboost-date-time-dev \
	    libboost-filesystem-dev \
	    libboost-program-options-dev \
	    libboost-system-dev \
	    libssl-dev \
	    zlib1g-dev

If you need UPnP support miniupnpc development library should be installed (don't forget to rerun CMake with needed option):

	sudo apt-get install libminiupnpc-dev

You may also build deb-package with the following:

	sudo apt-get install fakeroot devscripts dh-apparmor
	cd i2pd
	debuild --no-tgz-check -us -uc -b
Arch/Manjaro
-------------
You will need a compiler and other tools that could be installed with `base-devel` package:
	
	sudo pacman -S base-devel

Also you will need a bunch of libraries
	
	sudo pacman -S boost zlib openssl
Fedora/Centos
-------------

You will need a compiler and other tools to perform a build:

	sudo yum install make cmake gcc gcc-c++

Also you will need a bunch of development libraries

	sudo yum install boost-devel openssl-devel

If you need UPnP support miniupnpc development library should be installed (don't forget to rerun CMake with needed option):

	sudo yum install miniupnpc-devel

Latest Fedora systems using [DNF](https://en.wikipedia.org/wiki/DNF_(software)) instead of YUM by default, you may prefer to use DNF, but YUM should be ok

Centos 7 has CMake 2.8.11 in the official repositories that too old to build i2pd, CMake >=3.7 is required.
But you can use cmake3 from the epel repository:

	yum install epel-release -y
	yum install make cmake3 gcc gcc-c++ miniupnpc-devel boost-devel openssl-devel -y

...and then use 'cmake3' instead 'cmake'.

Mac OS X
--------

Requires [homebrew](http://brew.sh)

	brew install boost openssl@1.1

To build:

	make HOMEBREW=1 -j8 # uses 8 threads

To install into the system root (`/`):

	sudo make install HOMEBREW=1

To install into the Homebrew root (`/usr/local/`):

	sudo make install HOMEBREW=1 PREFIX=/usr/local

FreeBSD
-------

For 10.X  use clang. You would also need devel/boost-libs, security/openssl and devel/gmake ports.
Type gmake, it invokes Makefile.bsd, make necessary changes there is required.

Branch 9.X has gcc v4.2, that is too old (not supports -std=c++11)

Required ports:

* `devel/cmake`
* `devel/boost-libs`
* `lang/gcc47`(or later version)

To use newer compiler you should set these variables(replace "47" with your actual gcc version):

	export CC=/usr/local/bin/gcc47
	export CXX=/usr/local/bin/g++47
