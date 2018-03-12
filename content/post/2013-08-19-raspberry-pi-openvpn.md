---
title: "Raspberry Pi: OpenVPN"
date: 2013-08-19
tags:
  - howto
---
This is the story of how I got my Raspberry Pi to be my OpenVPN Server. I am using Raspbian as my Pi OS.

<!--more-->

**Step 1:**

Check to make sure you are completely updated.

``` bash

su root
apt-get update
apt-get -y dist-upgrade
apt-get clean

```

**Step 2:**

Install the needed software.

``` bash

apt-get -y install openvpn openssl
apt-get clean

```

**Step 3:**

Setup your easy-rsa environment.

``` bash

cd /etc/openvpn
cp -r /usr/share/doc/openvpn/examples/easy-rsa/2.0 ./easy-rsa
vim /easy-rsa/vars

after export EASY_RSA="`pwd`"
add
export EASY_RSA="/etc/openvpn/easy-rsa"
find KEY_SIZE and change it from 1024 to 2048

press escape
Save and close (:wq)

cd easy-rsa
source vars
./clean-all
./pkitool --initca
ln -s openssl-1.0.0.cnf openssl.cnf

```

**Step 4:**

Now we generate some encryption keys.

``` bash

./build-ca OpenVPN
./build-server server
./build-key client-wik

```

**Step 5:**

Generate DH Parameters, 2048 bit key. Warning: this step takes quite a bit of time.

``` bash

./build-dh

```

**Step 6:**

Creating the server configuration file.

``` bash

cd /etc/openvpn
vim openvpn.conf

proto udp
dev tun
port 1194
ca /etc/openvpn/easy-rsa/keys/ca.crt
cert /etc/openvpn/easy-rsa/keys/server.crt
key /etc/openvpn/easy-rsa/keys/server.key
dh /etc/openvpn/easy-rsa/keys/dh2048.pem
user nobody
group nogroup
server 10.8.0.0 255.255.255.0
persist-key
persist-tun
status /var/log/openvpn-status.log
verb 3
#uncomment below if you want vpn-users to see each other
#client-to-client
push "redirect-gateway def1"
#setup dns servers for google
#push "dhcp-options DNS 8.8.8.8"
#push "dhcp-options DNS 8.8.4.4"
#setup dns servers for internal dns server
push "dhcp-options DNS 10.8.0.1"
log-append /var/log/openvpn
comp-lzo

press escape
Save and close (:wq)

```

**Step 7:**

Enable internet-forwarding for the VPN Clients. Warning: Replace eth0 with the correct network device as needed.

``` bash

echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
vim /etc/sysctl.conf

uncomment # from net.ipv4.ip_forward=1

Press escape
Save and close (:wq)

crontab -e
@reboot iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE MASQUERADE

Press Cntl+x and then Y to save

```

**Step 8:**

Creating the client.s ovpn file.

``` bash

cd /etc/openvpn/easy-rsa/keys
vim client-wik.ovpn

setenv FORWARD_COMPATIBLE 1
dev tun
dev-type tun
ns-cert-type server
reneg-sec 604800
sndbuf 100000
rcvbuf 100000
client
proto udp
remote SERVER IP HERE 1194
resolv-retry infinite
server-poll-timeout 4
nobind
persist-key
persist-tun
#LZO commands are pushed by the server at connection time
#the below linle does't disable LOZ
comp-lzo no
verb 3
setenv PUSH_PEER-INFO

<ca>
YOUR CA CERT HERE
</ca>

<cert>
YOUR USER CER CERT HERE
</cert>

<key>
YOUR USER KEY CERT HERE
</key>

key-direction 1

Press escape
Save and close (:wq)

```

**Finally.**

Start openvpn with /etc/initi.d/openvpn start

Time for testing. Copy your client-wik.ovpn file to an external device (I used an iphone for testing with the openvpn app). All worked as planned, I was connected, and had the correct external ip address.

**UFW Notes**

Just some friendly notes, as UFW likes to manage things for you.

``` bash

ufw allow 1194

vim /etc/default/ufw

set default_forward_policy = "accept"
save and close

vim /etc/ufw/before.rules
after the first comment add the following:

# nat table rules
*nat
:PREROUTING ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
COMMIT
save and close

ufw disable
ufw enable

```

