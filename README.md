# ike-for-ubuntu
Instructions and patch for compiling IKE software (Shrew Soft VPN) on

  * Ubuntu 20.04
  * Ubuntu 20.10

All credits to [Fabrizio Allevi](https://www.linkedin.com/in/fabrizio-allevi-3201431a/it-it?trk=people-guest_people_search-card&originalSubdomain=it), https://sites.google.com/view/uptolinux/ike_20-04.

## Disclaimer
This is a personal procedure that worked for some guys on their systems. Nobody is taking any responsibility if in your case this does not work or causes any damage. Apply these instructions only under your responsibility and at your own risk.

## Instructions

### Ubuntu 20.04

    sudo -i
    cd /usr/local/src/
    wget https://www.shrew.net/download/ike/ike-2.2.1-release.tbz2
    tar xjf ike-2.2.1-release.tbz2
    apt install dos2unix wget
    find ike/ -type f | xargs -r dos2unix
    cd ike/
    wget https://raw.githubusercontent.com/MaxChinni/ike-for-ubuntu/master/ike.patch
    patch -p1 < ike.patch
    apt install build-essential libssl-dev libaudio-dev libcups2-dev cmake libedit-dev g++ flex bison
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DQTGUI=NO -DETCDIR=/etc -DNATT=YES
    make
    make install
    mv /etc/iked.conf.sample /etc/iked.conf

### Ubuntu 20.10

    sudo -i
    cd /usr/local/src/
    wget https://www.shrew.net/download/ike/ike-2.2.1-release.tbz2
    tar xjf ike-2.2.1-release.tbz2
    apt install dos2unix wget
    find ike/ -type f | xargs -r dos2unix
    cd ike/
    wget https://raw.githubusercontent.com/MaxChinni/ike-for-ubuntu/master/ike.patch
    patch -p1 < ike.patch
    wget -O CMakeLists.txt https://raw.githubusercontent.com/MaxChinni/ike-for-ubuntu/master/CMakeLists.txt.impish
    apt install build-essential libssl-dev libaudio-dev libcups2-dev cmake libedit-dev g++ flex bison
    cmake -DCMAKE_INSTALL_PREFIX=/usr -DQTGUI=NO -DETCDIR=/etc -DNATT=YES -S. -Wno-dev
    make
    make install
    mv /etc/iked.conf.sample /etc/iked.conf

## Usage

1. Copy your configuration files under your `~/.ike/sites/` directory (i.e. ``~/.ike/sites/work.vpn``)
2. Start the daemon

        sudo iked
3. Connect to the VPN, i.e.

        ikec -r work.vpn
