BitZeny Core integration/staging tree
=====================================

http://bitzeny.org

* Copyright (c) 2014      BitZeny Core Developers
* Copyright (c) 2009-2014 Bitcoin Core Developers
* Copyright (c) 2013-2014 DarkCoin Developers (DarkGravityWave3)
* Copyright (c) 2014      Alexander Peslyak   (Yescrypt)

License
-------

BitZeny Core is released under the terms of the MIT license. See [COPYING](COPYING) for more
information or see http://opensource.org/licenses/MIT.

Build bitzenyd on Ubuntu 14.04
-------------------

    sudo apt-get install build-essential
    sudo apt-get install libtool autotools-dev autoconf
    sudo apt-get install libssl-dev
    sudo apt-get install libboost-all-dev
    sudo apt-get install pkg-config
    sudo add-apt-repository ppa:bitcoin/bitcoin
    sudo apt-get update
    sudo apt-get install libdb4.8-dev
    sudo apt-get install libdb4.8++-dev
    
    git clone https://github.com/bitzeny/bitzeny.git
    cd bitzeny
    ./autogen.sh
    ./configure --without-miniupnpc --without-gui --disable-tests
    make

Build bitzeny* for Windows 32bit on Ubuntu (Checked on 17.04)
-------------------------------------------------------------

    sudo apt-get install g++-mingw-w64-i686 mingw-w64-i686-dev
    sudo apt-get install curl git build-essential libtool autoconf pkg-config bsdmainutils
    git clone --recursive [URL to this repository]
    cd bitzeny/depends
    ./buildwin32.sh
    cd ..
    ./autogen.sh
    CONFIG_SITE=$PWD/depends/i686-w64-mingw32/share/config.site ./configure --prefix=$PWD/depends/i686-w64-mingw32 --enable-upnp-default LIBS=-lpthread
    make
    make install

Then you can see .exe files in bitzeny/depends/i686-w64-mingw32/bin directory.

Build bitzeny* for Windows 64bit on Ubuntu (Checked on 17.04)
-------------------------------------------------------------

    sudo apt-get install g++-mingw-w64-x86-64 mingw-w64-x86-64-dev
    sudo apt-get install curl git build-essential libtool autoconf pkg-config bsdmainutils
    git clone --recursive [URL to this repository]
    cd bitzeny/depends
    ./buildwin64.sh
    cd ..
    ./autogen.sh
    CONFIG_SITE=$PWD/depends/x86_64-w64-mingw32/share/config.site ./configure --prefix=$PWD/depends/x86_64-w64-mingw32 --enable-upnp-default LIBS=-lpthread
    make
    make install

Then you can see .exe files in bitzeny/depends/x86_64-w64-mingw32/bin directory.

Development tips and tricks
---------------------------

**compiling for debugging**

Run configure with the --enable-debug option, then make. Or run configure with
CXXFLAGS="-g -ggdb -O0" or whatever debug flags you need.

**debug.log**

If the code is behaving strangely, take a look in the debug.log file in the data directory;
error and debugging message are written there.

The -debug=... command-line option controls debugging; running with just -debug will turn
on all categories (and give you a very large debug.log file).

The Qt code routes qDebug() output to debug.log under category "qt": run with -debug=qt
to see it.

**testnet and regtest modes**

Run with the -testnet option to run with "play bitcoins" on the test network, if you
are testing multi-machine code that needs to operate across the internet.

If you are testing something that can run on one machine, run with the -regtest option.
In regression test mode blocks can be created on-demand; see qa/rpc-tests/ for tests
that run in -regest mode.

**DEBUG_LOCKORDER**

Bitcoin Core is a multithreaded application, and deadlocks or other multithreading bugs
can be very difficult to track down. Compiling with -DDEBUG_LOCKORDER (configure
CXXFLAGS="-DDEBUG_LOCKORDER -g") inserts run-time checks to keep track of what locks
are held, and adds warning to the debug.log file if inconsistencies are detected.
