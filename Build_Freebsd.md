# Building the MonetaryUnit daemon on FreeeBSD 11

    Update 2017-11-03: Modified building instructions for DB5 and default location of the binaries

As there are no binaries available for FreeBSD, the user may build the source themselves and run the MUE daemon on a FreeBSD server.
This guide show how to build the MUE daemon for FreeBSD 11. Other versions of FreeBSD may be possible, but it is outside the scope of this guide.

### Setup the building environement and install the dependencies

    pkg install ca_root_nss autotools pkgconf gmake boost-libs openssl db5 git libevent

create a small run script for the building, copy and paste the comands below:

    nano -w build-mued.sh


    #!/bin/csh
    setenv CC "clang"
    setenv CXX "clang++"
    setenv BDB_CPPFLAGS "-I/usr/local/include -I/usr/local/include/db5"
    setenv CFLAGS "-I/usr/local/include -I/usr/local/include/db5"
    setenv CXXFLAGS "-I/usr/local/include -I/usr/local/include/db5"
    setenv CPPFLAGS "-I/usr/local/include -I/usr/local/include/db5"
    setenv LDFLAGS "-L/usr/local/lib -L/usr/local/lib/db5"

    ./autogen.sh
    ./configure --with-incompatible-bdb --enable-debug --with-gui=no --disable-shared --with-pic --without-miniupnpc --disable-tests --enable-hardening

    # Workaround 1277
    sed -i '' 's/^LEVELDB_TARGET_FLAGS = .*$/LEVELDB_TARGET_FLAGS = -DOS_FREEBSD/' src/Makefile
    gmake

### Get the source files

    git clone https://github.com/muecoin/MUECore.git
    
Copy the help script to the source folder

    chmod 755 build_mued.sh
    cp build-mued.sh MUECore/

and run the script:

    cd MUECore
    ./build-mued.sh

When the building is complete, the binaries will be in Mue/src/

### Run the daemon

With the binaries now done, run the daemon from its folder:

    cd src

Copy the binaries as root

    su 
    cp mued /usr/local/bin/
    cp mue-* /usr/local/bin/
    
and... lets start the MUE daemon!

    mued -daemon
    
and run the command line interface with

    mue-cli getinfo

Congratulations! You have successfully built the Monetaryunit daemon on a FreeBSD host!
