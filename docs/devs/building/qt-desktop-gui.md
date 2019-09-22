# Qt Desktop GUI build instructions

## Under Windows

### With Qt Creator

TBD

### Without Qt Creator

All commands should be run under MSYS2 mingw32.

```
pacman -S git make tar mingw-w64-i686-gcc mingw-w64-i686-boost mingw-w64-i686-libpng mingw-w64-i686-openssl mingw-w64-i686-zlib
pacman -S mingw-w64-i686-qt5-static
pacman -S openssl-devel mingw-w64-i686-miniupnpc
mkdir -p /c/dev/
cd /c/dev/
git clone https://github.com/PurpleI2P/i2pd.git
cd i2pd
git checkout openssl
export MINGW='mingw32'
export PATH=/$MINGW/bin:/usr/bin:/mingw32/qt5-static/bin
cd qt/i2pd_qt
qmake
make USE_UPNP=yes
```

#### Caveats

It is important to restrict PATH as described above. If you have Strawberry Perl and/or Mercurial installed, it will pick up gcc & openssl from the wrong places.

If you are an Arch Linux user, refrain from updating system with `pacman -Syu`. Always update runtime separately as described on the home page, otherwise you might end up with DLLs incompatibility problems.

## Under Ubuntu

### With Qt Creator

```
sudo apt install qtcreator qt5-default build-essential g++ make libcrypto++-dev libssl-dev libboost-all-dev libminiupnpc-dev libwebsocketpp-dev libqt5gui5 git
mkdir git
cd git
git clone https://github.com/PurpleI2P/i2pd.git
```

Then, open Qt Creator. Open `git/i2pd/qt/i2pd_qt/i2pd_qt.pro`; set build make parameter
`-jNUMBER-OF-CPU-CORES`, e.g. `-j7`. Then, finally, press Make button.

### Without Qt Creator

```
sudo apt-get install build-essential g++ make libcrypto++-dev libssl-dev libboost-all-dev libminiupnpc-dev libwebsocketpp-dev qt5-default libqt5gui5 git zlib1g-dev
mkdir git
cd git
git clone https://github.com/PurpleI2P/i2pd.git
cd i2pd/qt/i2pd_qt
qmake
make USE_UPNP=yes
```

## Building a Flatpak package

```
sudo apt install flatpak flatpak-builder
flatpak --user remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak --user install flathub org.kde.Sdk/x86_64/5.11     # required SDK version might be higher, recheck the manifest 
git clone https://github.com/flathub/website.i2pd.i2pd && cd website.i2pd.i2pd 
flatpak-builder --user --install --force-clean i2pd_build_dir website.i2pd.i2pd.json
```
