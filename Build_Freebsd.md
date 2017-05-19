# Building the MonetaryUnit daemon on FreeeBSD 11

As there are no binaries available for FreeBSD, the user may build the source themselves and run the MUE daemon on a FreeBSD server.
This guide show how to build the MUE daemon for FreeBSD 11. Other versions of FreeBSD may be possible, but it is outside the scope of this guide.

### Setup the building environement and install the dependencies

    pkg install ca_root_nss autotools pkgconf gmake boost-libs openssl db48 git libevent

create a small run script for the building, copy and paste the comands below:

    nano -w build-mued.sh


    #!/bin/csh
    setenv CC "clang"
    setenv CXX "clang++"
    setenv BDB_CPPFLAGS "-I/usr/local/include -I/usr/local/include/db48"
    setenv BDB_LIBS "-ldb_cxx-4.8"
    setenv CFLAGS "-I/usr/local/include -I/usr/local/include/db48"
    setenv CXXFLAGS "-I/usr/local/include -I/usr/local/include/db48"
    setenv CPPFLAGS "-I/usr/local/include -I/usr/local/include/db48"
    setenv LDFLAGS "-L/usr/local/lib -L/usr/local/lib/db48"

    ./autogen.sh
    ./configure --with-incompatible-bdb --enable-debug --with-gui=no --disable-shared --with-pic --without-miniupnpc --disable-tests --enable-hardening

    # Workaround 1277
    sed -i '' 's/^LEVELDB_TARGET_FLAGS = .*$/LEVELDB_TARGET_FLAGS = -DOS_FREEBSD/' src/Makefile
    gmake

### Get the source files

    wget https://www.monetaryunit.org/downloads/mue-source64-1.0.0.1.tar.gz
    tar -zxvf mue-source64-1.0.0.1.tar.gz

Copy the help script to the source folder

    cp build-mued.sh mue-source64-1.0.0.1

and run the script:

    cd mue-source64
    sh build-mued.sh

When the building is complete, the binaries will be in Mue/src/

### Run the daemon

With the binaries now done, run the daemon from its folder:

    cd src
    ./mued -daemon
    
and run the command line interface with

    mue-cli getinfo

Congratulations! You have successfully built the Monetaryunit daemon on a FreeBSD host!
