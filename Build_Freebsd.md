pkg install ca_root_nss autotools pkgconf gmake boost-libs openssl db48 git libevent


----------------------------------------------------------------


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


------------------------------------------------------------------