---
title: "irssi + blowfish = fun"
date: 2013-07-09
tags:
  - general
  - howto
---
Who doesnt want to encrypt their IRC Messages?

<!--more-->

**The installation**

``` bash

sudo apt-get -y install irssi irssi-scripts
sudo apt-get -y install libcrypt-cbc-perl libcrypt-blowfish-perl

```

*ubuntu and debian*

``` bash

sudo apt-get -y install libcompress-zlib-perl

```

*mint*

``` bash

sudo apt-get -y remove irssi
sudo apt-get -y install build-essential libglib2.0-dev
sudo apt-get -y install libncurses-dev libperl-dev
cd /usr/src
wget http://www.irssi.org/files/irssi-0.8.15.tar.gz
tar -zxvf irssi-0.8.15.tar.gz
cd irssi-0.8.15
./configure
make
sudo make install

```

*ubuntu and debian*

Download [blowjob](https://scripts.irssi.org/scripts/blowjob.pl) and place it in your ~/.irssi/plugins directory.

*mint*

Download [blowjob](https://scripts.irssi.org/scripts/blowjob.pl) and place it in your /usr/share/irssi/scripts directory.

**The Usage (basic)**

/setkey [key\_value]

/blow [message]

