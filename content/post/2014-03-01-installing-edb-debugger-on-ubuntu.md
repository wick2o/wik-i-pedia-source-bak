---
title: "Installing edb debugger on Ubuntu"
date: 2014-03-01
tags:
  - general
  - linux
---

First lets start by installing the needed packages.

<!--more-->

**Step 1**: Install the needed packages

``` bash

sudo apt-get -y install libqt4-dev libboost1.48-all-dev subversion

```

**Step 2**: Building edb-debugger

``` bash

svn checkout http://edb-debugger.googlecode.com/svn/trunk edb-debugger
cd edb-debugger
qmake
make
sudo make install

```

**Step 3**: Locate edb

``` bash

whereis edb

```

**Step 4**: Create home directory

``` bash

cd ~
mkdir .edb

```

**Step 5**: Setting up directories

``` bash

sudo edb

```

Once the program has launched, goto Directories under Preferences.

 * Set Symbol Directory: /home/$USER/.edb
 * Plugin Directory: /lib64/edb/ (Found in step 3)
 * Session Directory: /home/$USER/.edb


**Step 6**: Close and then restart edb

Happy debugging!

