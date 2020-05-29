---
layout: post
title: macOS crash/reboot after waking from sleep with external monitor
comments: false
---

A bug in macOS 10.15.(4/5) makes (at least) 16" MBPs to crash and reboot when waking from sleep when an external monitor is connected.

The workaround is simple however. Just set the Prevent computer from sleeping automatically when display is off in Energy Savings in System Preferences:

(Update 2020-5-29: Some reports that turning off the "Power Nap" feature will also solve this: https://discussions.apple.com/thread/251225148)

<img alt="Preferences Pane" src="/images/preferences-energy-pane.png" />