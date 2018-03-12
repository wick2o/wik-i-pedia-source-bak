---
title: "Raspberry Pi: Preseeding"
date: 2013-09-10
tags:
  - howto
---
I have been playing around with multiple projects on a Raspberry Pi and got tired of answering the install questions over and over again. I'm going to assume you know how to do a default install of Raspbian using the [installer](http://www.raspbian.org/RaspbianInstaller).

<!--more-->

**Step 1:**

Modify the cmdline.txt file on the SD card after extracting the installer. You are going to want to append the following after .rootfstype=ext4 root You will also want to replace .[IP HERE]. with your tftpd server.
*Note: This is all one line.*
<br><br>
```
debian-installer/locale=en_US keyboard-configuration/xkb-keymap=us netcfg/dhcp_timeout=60 netcfg/get_hostname=raspbian netcfg/get_domain=unassigned-domain preseed/url=tftp://[IP HERE]/preseed.cfg
```
<br>
**Step 2:**

Creating a preseed.cfg file. I used the following, your mileage may very.
*Note: Watch for word wrapping.*

```

############################################
# DESCRIPTION OF OPTIONS USED IN THIS FILE #
############################################
# Bypass prompt
d-i mirror/country string manual
#
# Set Raspbian mirror
d-i mirror/http/hostname string mirrordirector.raspbian.org
#
# Set Raspbian mirror directory
d-i mirror/http/directory string /raspbian/
#
# No http proxy specified; If you need one, enter it after the "string"
d-i mirror/http/proxy string
#
# Continue the install without loading kernel modules?
d-i anna/no_kernel_modules boolean true
#
# Set root password as "root"
d-i passwd/root-password password toor
d-i passwd/root-password-again password toor
#
# Bypass normal user account creation; Set to "true" or comment out if you want to create a user
d-i passwd/make-user boolean false
#
# Select your time zone:
d-i time/zone string US/Eastern
#
# Bypass prompt
d-i partman/choose_partition select finish
#
# Write the changes to disk?
d-i partman/confirm boolean true
#
# Continue without installing a kernel?
bootstrap-base base-installer/kernel/skip-install boolean true
#
# Install displays a warning because the security package is not associated with Raspbian.
# Passing the installer an emtpy string on where to find this repository bypasses the error.
d-i apt-setup/security_host string
#
# Participate in the package usage survey?
d-i popularity-contest/participate boolean false
#
# Choose software to install: SSH server, Standard system utilities
d-i tasksel/first multiselect SSH server, Standard system utilities
#
# Continue without boot loader
d-i nobootloader/confirmation_common boolean true

```

**Step 3:**

Boot from SD card and go get some coffee.

**Final Thoughts**

Since I was using windows at the time of this setup I used [TFTPD](http://tftpd32.jounin.net/tftpd32_download.html) as my server, but I'm sure any ole TFTPD or HTTP url will work.


