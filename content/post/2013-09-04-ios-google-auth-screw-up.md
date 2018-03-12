---
title: "iOS Google Authenticator Screw Up"
date: 2013-09-04
tags:
  - howto
---
If you have not heard the news, [Google screwed up their iphone app](http://techcrunch.com/2013/09/04/dont-install-the-google-authenticator-for-ios-update-unless-you-want-your-stored-user-accounts-wiped/). If you were unlucky (like i was) and updated to the latest version it wiped all of your accounts. I didn't get hit too hard, I only lost access to two WordPress blogs. Below is how I quickly fixed it.

<!--more-->

**Step 1:**

Login and/or SSH into your host (This will be different depending on your hosting provider) and gain access to your database.

**Step 2:**

Get a list of all active plugins.

``` sql

SELECT option_value FROM wp_options WHERE option_name = 'active_plugins';

```

You are going to want to save this value (makes things easier later).

**Step 3:**

Clear the active plugins.

``` sql

UPDATE wp_options SET option_value = '' WHERE option_name = 'active_plugins';

```
<br>
**Step 4:**

You may now log into your WordPress blog without Google Authentication. Now you have two options here. Go back and use the WordPress GUI to re-enable your plugins OR if you saved the value from step 2 above you can do the next step.

**Step 5:**

Quickly re-enable your plugins.

``` sql

UPDATE wp_options SET option_value = '[YOUR STRING HERE]' WHERE option_name = 'active_plugins';

```

