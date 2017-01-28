---
published: false
---
## Teardown of a badly coded Paypal phisher

So, while working on a new client project, I found in an inconspicuous subdirectory (`secure/Secureme`) some very suspicious files, that when put together contained a laughably bad phisher for Paypal logins. The source code for this piece of art is located [here](https://github.com/jamjar919/paypal-phisher).

### The index page

So first off the wily attacker makes a rather obvious mistake in his `index.php` - visiting it will redirect you to `secureme/Secure/login?cmd=_signin&dispatch=8d2b748725d52dd90dea7bc8f&locale=en_` when obviously the redirect was meant to go to `login.php`. Looking at the code...

    <?php
        include("blocker.php");
        include("detect.php");
        $random = rand(0,100000000000);
        $dis = substr(md5($random), 0, 25);
        header('Location: login?cmd=_signin&dispatch='.$dis.'& locale=en_'.$countrycode.'');
     ?>

...it's obvious that's what the attacker meant to do. But what about these URL parameters? Surely they must be important? Well, it turns out that the attacker decided that because the official Paypal website has URL parameters (And not even the same ones!), that the phisher must also have URL parameters. These are simply hardcoded strings combined with a randomised bit of an MD5 string. 