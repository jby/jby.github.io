---
layout: post
title: Password policy forces old passwords to change
comments: false
---

I've been playing with setting a password policy for all our Mac users with a configuration profile from <a href="https://www.jamf.com/products/jamf-pro/">Jamf Pro</a>.

If you set a max password age in such a policy macOS will look at the key passwordLastSetTime:
```
	<key>passwordLastSetTime</key>
	<real>1546446982.760783</real>
```
Where the last set time is expressed in <a href="https://en.wikipedia.org/wiki/Unix_time">POSIX time</a>
If the password is older than the, with profile set policy, max age then macOS will expire it immediately and force the user to reset it at next login/screen unlock.
The use of the old password will not be permitted for (at least) any GUI-admin tasks, commandline <a href="https://en.wikipedia.org/wiki/Sudo">sudo</a> seems unaffected.
