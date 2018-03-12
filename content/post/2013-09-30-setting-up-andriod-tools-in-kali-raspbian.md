---
title: "Android Tools in Kali + Raspbian"
date: 2013-09-30
tags:
  - howto
---
How I got Android Tools working in Kali and Raspbian

<!--more-->

**Step 1:**

The Setup

```bash

sudo mkdir /opt/android && cd /opt/android
sudo echo "deb-src http://debian.ens-cachan.fr/ftp/debian/ sid main contrib non-free" >> /etc/apt/sources.list

```

**Step 2:**

The Installation

``` bash

sudo apt-get update
sudo apt-get -y build-dep android-tools
sudo apt-get -y source --build android-tools
sudo dpkg -i android-tools-*.deb

```

**Step 3:**

The Cleanup

``` bash

cd ~
sudo rm -rf /opt/android
sudo sed -i 's/.*cachan\.fr.*//' /etc/apt/sources.list
sudo apt-get update
sudo apt-get clean

```

