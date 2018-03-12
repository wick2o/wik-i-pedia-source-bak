---
title: "enwiki Username Wordlist"
date: 2013-07-08
tags:
  - general
---
How to convert enwiki into a wordlist.

<!--more-->

**Download Wikipedia Archive**

You can find the archive dumps on [http://dumps.wikimedia.org/enwiki/](http://dumps.wikimedia.org/enwiki/) I used the firefox addon called 'DownThemAll' to make the downloading easier and less time consuming.


{{< figure src="/2013-07-08-enwiki-username-wordlist/screenshot.gif" title="" >}}
<br>

**Uncompressed Archives**

This part takes awhile. There are a lot of files and each one has holds a 64GB XML file. I choose to do one at a time as the previous file was being processed.

**The Processing**

``` bash

export NUM=02
egrep -i '.*' enwiki-20130604-$NUM > enwiki-20130604-$NUM-users.txt
sed -i 's/^.*<username>//g' enwiki-20130604-$NUM-users.txt
sed -i 's/<\/username>$//g' enwiki-20130604-$NUM-users.txt
cat enwiki-20130604-*-users.txt | sort -n | uniq -c | sort -rn > enwiki-users-freq.txt

```
or

``` bash

for i in {01..156};
do
  7z e enwiki-20130604-$i.7z
  egrep -i '.*' enwiki-20130604-$i > enwiki-20130604-$i-users.txt
  sed -i 's/^.*<username>//g' enwiki-20130604-$i-users.txt
  sed -i 's/<\/username\>$//g' enwiki-20130604-$i-users.txt
  rm enwiki-20130604-$i
done
cat enwiki-20130604-*-users.txt | sort -n | uniq -c | sort -rn > enwiki-users-freq.txt

```

**The Results**

The resulting wordlist can be found on the github project [enwiki-wl](http://github.com/wick2o/enwiki-wl)


