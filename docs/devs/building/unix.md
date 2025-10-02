Building on Unix-like systems
=============================

This doc covers:

* [Debian/Ubuntu](#debian-ubuntu) (includes packaging)
* [Arch/Manjaro](#arch-manjaro)
* [Fedora/RHEL/CentOS Stream](#fedora-rhel-centos-stream)
* [macOS](#macos)
* [FreeBSD](#freebsd)
* [OpenBSD](#openbsd)
* [Solaris/OpenIndiana](#solaris-openindiana)

Make sure you have all required dependencies installed for your system.
See [this](requirements.md) page for common requirements.

## Clone and build (CMake)

```bash
git clone https://github.com/PurpleI2P/i2pd.git
cd i2pd
mkdir -p build && cd build
cmake <cmake options> ..
cmake --build . -j
```

…or the quick-and-dirty way with plain make:

```bash
cd i2pd/
make
```

Run tests:

```bash
cd i2pd/tests/
make
```

Install after a successful build:

```bash
sudo make install
```

## CMake Options

Pass options as `-D<key>=<value>` (see `man 1 cmake`):

* `CMAKE_BUILD_TYPE` build profile (Debug/Release, default: none)
* `WITH_BINARY`      build i2pd (default: ON)
* `WITH_LIBRARY`     build libi2pd (default: ON)
* `WITH_STATIC`      build static lib and binary (default: OFF)
* `WITH_UPNP`        build with UPnP (requires miniupnpc, default: OFF)
* `WITH_AESNI`       build with AES-NI (default: ON)
* `WITH_HARDENING`   enable hardening (GCC/Clang, default: OFF)
* `WITH_MESHNET`     build for cjdns test net (not for main net, default: OFF)
* `WITH_ADDRSANITIZER`   enable ASan (default: OFF)
* `WITH_THREADSANITIZER` enable TSan (default: OFF)

List cached options:

```bash
cmake -L
```

## Debian/Ubuntu

Tools:

```bash
sudo apt install build-essential debhelper cmake
```

Libraries:

```bash
sudo apt install \
  libboost-date-time-dev \
  libboost-filesystem-dev \
  libboost-program-options-dev \
  libboost-system-dev \
  libssl-dev \
  zlib1g-dev
```

UPnP (optional):

```bash
sudo apt install libminiupnpc-dev
```

Build a `.deb`:

```bash
sudo apt install fakeroot devscripts dh-apparmor
cd i2pd
debuild --no-tgz-check -us -uc -b
```

(UPnP dev package name: `libminiupnpc-dev`.)

## Arch/Manjaro

Tools:

```bash
sudo pacman -S --needed base-devel cmake
```

Libraries:

```bash
sudo pacman -S --needed boost openssl zlib
```

## Fedora/RHEL/CentOS Stream

Tools:

```bash
sudo dnf install make cmake gcc gcc-c++
```

Libraries:

```bash
sudo dnf install boost-devel openssl-devel zlib-devel
```

UPnP (optional):

```bash
sudo dnf install miniupnpc-devel
```

Static builds (optional):

```bash
sudo dnf install boost-static
```

(UPnP/Boost static package names: `miniupnpc-devel`, `boost-static`. On some RHEL-like systems you may need to enable EPEL/CRB.)

## macOS

Requires Homebrew and Xcode Command Line Tools.

Install deps:

```bash
brew install boost openssl@3 cmake make   
```

Build:

```bash
make HOMEBREW=1 -j8
```

Install to system root (`/`):

```bash
sudo make install HOMEBREW=1
```

Install to the Homebrew prefix (Intel `/usr/local`, Apple Silicon `/opt/homebrew`):

```bash
sudo make install HOMEBREW=1 PREFIX="$(brew --prefix)"
```

## FreeBSD

Use the base compiler (Clang). Install tools and libraries as root:

```bash
pkg install boost-libs cmake gmake
```

UPnP (optional):

```bash
pkg install miniupnpc
```

Build with GNU make if using the BSD Makefile path:

```bash
gmake
```

(Note: FreeBSD ships OpenSSL and zlib in the base system; you don’t need extra SSL packages.)

## OpenBSD

Use the base compiler (Clang). Install tools and libraries as root:

```bash
pkg_add boost cmake gmake
```

UPnP (optional):

```bash
pkg_add miniupnpc
```

Build with GNU make if using the BSD Makefile path:

```bash
gmake
```

(Note: OpenBSD ships LibreSSL and zlib in the base system; you don’t need extra SSL packages.)

## Solaris/OpenIndiana

Install required packages:

```bash
pkg install developer/gcc-14
pkg install developer/build/cmake
pkg install system/library/boost
pkg install developer/build/gnu-make
```

Then use `gmake` if invoking Makefiles directly.

(Modern OpenIndiana provides GCC 14; package FMRIs like `developer/gcc-14` and `system/library/boost` follow IPS naming.)
