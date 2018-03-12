---
title: "How to handle multiple github accounts"
date: 2014-02-18
tags:
  - howto
  - general
---
I've always just worked with one GitHub account (my personal one), and then came
 octopress.  I wanted to convert all the sites I manage from WordPress to octopress.  Everything went well until running rake deploy.  

<!--more-->

**Step 1**: Create a new SSH key

I had to generate a new SSH key for the second GitHub account.

``` bash 

ssh-keygen -t rsa -C "your-email-address"

```

Be sure not to over-write your existing key (~/.ssh/id\_rsa). I named mine id\_rsa\_ACCOUNT.

**Step 2**: Attach the new key to the GitHub Account.

I'm not going to go into any details here, it should be pretty obvious what you need to do.

**Step 3**: Create a config file

This config file is where all the magic happens.  This is what allows you to specify which account to use when pushing things to GitHub.

``` bash

touch ~/.ssh/config
vim ~/.ssh/config

```

Add the following to the file

``` bash

Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa

Host github-ACCOUNT
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_ACCOUNT

```
<br>
**Step 4**: Testing

Now when pushing to your different accounts you can specify which one.

**Example**:

``` bash

git remote add origin git@github-ACCOUNT:username/repo.git
git push origin master

```



