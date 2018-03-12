---
title: "Stopping a small botnet"
date: 2013-09-18
tags:
  - howto
  - general
---
Today I woke up to an unresponsive website. When I started to look at the Apache access.log file I found a ton of IPs trying to bruteforce my wp-admin.php creds. I knew they weren't going to get anywhere (thank you google auth), but it was doing a good job at DDOS.

<!--more-->

Below is the oneliner I used to quickly end their fun.

```

tail -f /var/log/apache2/access.log | while read line; do echo "$line" | grep -i "post /wp-login.php" | cut -d " " -f1 | xargs -r iptables -I INPUT -j DROP -s; done

```


