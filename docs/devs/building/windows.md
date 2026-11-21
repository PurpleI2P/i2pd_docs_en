Building on Windows
=========================

There are two approaches available to build i2pd on Windows. The best
one depends on your needs and personal preferences. One is to use
msys2 and [unix alike infrastructure](unix.md). Another
one is to use Visual Studio. While there might be no difference for
end users of i2pd daemon, developers, however, shall be wary of
differences in C++ name mangling between the two compilers when making
a choice to be able to link their software against libi2pd.

If you are a stranger to C++ with no development tools installed on
your system and your only goal is to have i2pd up and running from the
most recent source, consider using msys2. Although it relies on
command line operations, it should be straightforward.

In this guide, we will
assume that you typically have your projects in C:\dev\ as your
development location for the sake of convenience. Adjust paths
accordingly if it is not the case. Note that msys uses unix-alike
paths like /c/dev/ for C:\dev\.

msys2
-----

Download and run the installer from [msys2.org](https://www.msys2.org/).
MSYS2 requires 64-bit Windows 10 (version 1809) or newer.

- Open a shell for the UCRT64 environment (the installer opens one when it finishes).
- Install all prerequisites and download i2pd source:

        pacman -S base-devel git mingw-w64-ucrt-x86_64-gcc mingw-w64-ucrt-x86_64-boost mingw-w64-ucrt-x86_64-openssl
        mkdir -p /c/dev
        cd /c/dev
        git clone https://github.com/PurpleI2P/i2pd.git
        cd i2pd
        make DEBUG=no

Leave out `DEBUG=no` if you want a debug build. You should be able to run ./i2pd .

MSYS2 no longer provides Boost and miniupnpc packages for 32-bit (i686) builds,
so building a 32-bit i2pd requires building those libraries yourself. See i2pd's
[Windows CI workflow](https://github.com/PurpleI2P/i2pd/blob/openssl/.github/workflows/build-windows.yml)
for the steps.

### Caveats

By default, MSYS2 shells do not inherit the Windows `PATH`. If you have
changed this (for example with `MSYS2_PATH_TYPE=inherit`), programs on your
Windows `PATH` can be picked up instead of the MSYS2 ones. For example,
Strawberry Perl bundles its own compiler and libraries.

### UPnP

You can install it through the MSYS2 and build with `USE_UPNP` key.

	pacman -S mingw-w64-ucrt-x86_64-miniupnpc
	make USE_UPNP=yes DEBUG=no

If you already built i2pd without UPnP, run `make clean` first.

Using Visual Studio
-------------------

Note: i2pd's continuous integration does not currently test Visual Studio
builds, so these instructions may need adjusting. If you just want a working
build, use MSYS2.

### Installing dependencies through vcpkg

Requirements:

* [vcpkg](https://vcpkg.io/) (NOTE: Due to Microsoft's telemetry policies, it is strongly advised to opt out of data collection by referring to [this document](https://github.com/microsoft/vcpkg#telemetry).)
* [Visual Studio Community Edition](https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx) (tested with VS2022)

This part assumes that you know how to use and integrate vcpkg into Visual Studio.
Read [this](https://learn.microsoft.com/en-us/vcpkg/) if you are unsure.
Navigate to the vcpkg directory, and run(change x64-windows to x86-windows for a 32bit build):

    .\vcpkg install boost:x64-windows miniupnpc:x64-windows openssl:x64-windows zlib:x64-windows

You may now skip directly to the [build process](#creating-visual-studio-project).

### Without vcpkg

You will need [CMake](https://cmake.org/), and to build or download
Boost, OpenSSL and zlib yourself, and optionally [MiniUPnP](https://github.com/miniupnp/miniupnp)
for UPnP support. For building OpenSSL with Visual Studio, see OpenSSL's
[NOTES-WINDOWS.md](https://github.com/openssl/openssl/blob/master/NOTES-WINDOWS.md).
OpenSSL 3.5 or newer is required for i2pd's post-quantum encryption support.
When configuring i2pd, tell CMake where the libraries are with `-DBoost_ROOT`,
`-DOPENSSL_ROOT_DIR` and `-DZLIB_ROOT`.

### Creating Visual Studio project

Start CMake GUI, navigate to i2pd directory, choose building directory,  e.g. ./out, and configure options.

Alternatively, if you feel adventurous, try that from the command line

	mkdir i2pd\out
	cd i2pd\out
	cmake ..\build -DCMAKE_INSTALL_PREFIX:PATH=C:\dev\Debug_Win32_stage

Add `-DWITH_UPNP=ON` to build with UPnP support. If miniupnpc can't be found, configuration fails.
vcpkg users may refer to [this](https://learn.microsoft.com/en-us/vcpkg/users/buildsystems/cmake-integration) for CMake flags.

### Building i2pd

You can open generated solution/project with Visual Studio and build from there,
alternatively you can use `cmake --build . --config Release --target install` or
[MSBuild tool](https://msdn.microsoft.com/en-us/library/dd293626.aspx)

	msbuild i2pd.sln /p:Configuration=Release
