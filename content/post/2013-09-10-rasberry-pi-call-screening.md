---
title: "Raspberry Pi: Call Screening"
date: 2013-09-10
tags:
  - howto
---
Lets get our call sceening on with our Raspberry Pi.

<!--more-->

**Assumeptions:**

 1. You have a Raspberry Pi with a default Raspbian install.
 2. You have a Trendnet USB Modem from Amazon.

**Step 1:**

Install needed pre-packaged software

```bash

sudo apt-get -y install vim gcc tmux screen

```
<br>
**Step 2:**

Download and setup jcblock software

```bash

cd /usr/src
wget http://jcblock.cvs.sourceforge.net/viewvc/jcblock/?view=tar -O jblock.tar.gz
cd jcblock
mv jcblock/ ../jcb
cd ..
rm -rf jcblock
cd jcb

mv blacklist.dat.example blacklist.dat
mv whitelist.dat.example whitelist.dat
mv callerID.dat.example callerID.dat

vim jcblock.c
change: #define DO_TONES to //#define DO_TONES

vim makejcblock
comment out the existing gcc command and replace it with..
gcc -o jcblock jcblock.c truncate.c -lm

chmod +x makejcblock
./makejcblock

```
<br>
**Step 3:**

Fixup the default lists.

```

vim whitelist.dat
Comment out the two default whitelisted numbers (They are missing "?" at the end of the number and will crash the software")

vim blacklist.dat
Add your phone number but watch out for the special formatting and don't forget the "?" at the end.
```
<br>
**Step 4:**

Start the software

``` bash

/usr/src/jcb/jcblock -p /dev/ttyACMO

```
Testing
Call your home line from your cell phone. If all goes well it may ring once or twice and then pickup and then hangout on your cell phone while allowing all over numbers in normally.

**Additional Notes**

I had you install screen and/or tmux so that you can run the software, and then detach the console and allow the software to run.


