# Qt Desktop GUI build instructions

## Under Windows

### With Qt Creator

TBD

* Run Git Bash and then:

```
mkdir git
cd git
git clone https://github.com/PurpleI2P/i2pd.git
cd i2pd
git checkout openssl
```

* Download [qt-opensource-windows-x86-5.10.1.exe](http://download.qt.io/official_releases/qt/5.10/5.10.1/qt-opensource-windows-x86-5.10.1.exe)
and install

TBD

### Without Qt Creator

/c/Qt/Qt5.10.1/5.10.1/mingw53_32/bin/
 
pacman -S mingw-w64-i686-qt-creator mingw-w64-x86_64-qt-creator

TBD

## Under Ubuntu

### With Qt Creator

TBD

### Without Qt Creator

```
sudo apt-get install build-essential g++ make libcrypto++-dev libssl-dev libboost-all-dev libminiupnpc-dev libwebsocketpp-dev qt5-default libqt5gui5 git
mkdir git
cd git
git clone https://github.com/PurpleI2P/i2pd.git
cd i2pd/qt/i2pd_qt
qmake
make USE_UPNP=yes
```
