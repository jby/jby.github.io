---
layout: post
title: Automatic authentication of 802.1x for wired Macs
---

When I became responsible for the Macs we were using a *manually* created, *individual* cert (in the AD) for each Mac connecting to our internal network.

Via <a href="https://macmule.com/2015/09/06/osx-ad-certificate-requests-some-tips/">this post</a> by <a href="https://macmule.com">Ben Toms</a> and some guidance by James Turner I managed to automate the creation of (still) individual certs.

We have primarily Windows desktops and laptops in our company, and for them it "just works" when bound to our AD, why couldn't I get that with the Mac's as well?

So, together with one of our Windows techs I created a Config Profile containing our CA-root Cert and config to use EAP-TLS to get authenticated, it does however require an AD-computer object, which I create by binding to the AD beforehand, then it's just a question of adding the computer object to the correct SG in the AD to get an IP-address on the correct subnet (depending on what department you work at).

I've had a lot of help from <a href="https://gitlab.com/Mactroll">Joel Rennich</a> in trying to understand this, but I'm not comfortable enough with my understanding about how this actually work to be able to present it to a technical crowd yet.
