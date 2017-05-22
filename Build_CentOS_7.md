# Building the MonetaryUnit daemon for CentOS 7

Here's a simple how-to for building and installing the MonetaryUnit daemon (mued) for a CentOS 7 server system. We will build the daemon without graphical support, as this is intended for a headless server.

We need to install some additional packages from the `EPEL repository` to build the MonetaryUnit daemon.

_From the fedoraproject.org:
What is Extra Packages for Enterprise Linux (or EPEL)?
Extra Packages for Enterprise Linux (or EPEL) is a Fedora Special Interest Group that creates, maintains, and manages a high quality set of additional packages for Enterprise Linux, including, but not limited to, Red Hat Enterprise Linux (RHEL), CentOS and Scientific Linux (SL), Oracle Linux (OL).
EPEL packages are usually based on their Fedora counterparts and will never conflict with or replace packages in the base Enterprise Linux distributions. EPEL uses much of the same infrastructure as Fedora, including buildsystem, bugzilla instance, updates manager, mirror manager and more._

### Installing the EPEL repository
In the console, log in as root and run the setup commands:

    yum update && yum upgrade
    wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    yum install epel-release-latest-7.rpm

### Installing the support packages and dependencies

    yum install autoconf automake gcc-c++ libdb4-cxx libdb4-cxx-devel boost-devel openssl-devel



===== Download, Build and Install Bitcoin  =====
Now we will download, extract, build, and install Bitcoin the source.  In this article we are downloading Bitcoin version 0.9.4

<code console>
cd /usr/src
wget https://github.com/bitcoin/bitcoin/archive/v0.9.4.tar.gz
tar zxvf v0.9.4.tar.gz
cd bitcoin-0.9.4
./autogen.sh
./configure --prefix=/opt/bitcoin PKG_CONFIG_PATH=/opt/openssl/lib/pkgconfig LIBS=-Wl,-rpath,/opt/openssl/lib
make
make install


Build and Install OpenSSL Required Dependency
Unfortunately the openssl thats provided with CentOS is lacking EC Libraries so we are going to have to download, build, and install a separate copy of OpenSSL

cd /usr/src
wget https://www.openssl.org/source/openssl-1.0.1l.tar.gz
tar zxvf openssl-1.0.1l.tar.gz
cd openssl-1.0.1l
export CFLAGS="-fPIC"
./config --prefix=/opt/openssl shared enable-ec enable-ecdh enable-ecdsa
make all
make install
