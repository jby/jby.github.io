---
layout: post
title: Re-install Configuration Profiles in macOS 10.15 Catalina
comments: false
---

We're using a Configuration Profile for 802.1x authorization. It binds to the "First available" ethernet, which means if you use a different **type** of adapter (USB/Thunderbolt/etc. are different types), it won't use the profile. You either have to be without ethernet (usually not a viable option) or re-install the profile for the correct type of ethernet adapter.

Before 10.15 Catalina we could just remove the 802.1x profile and have our Jamf Pro re-install it. This could be done by the user without help from an admin (our users all have local admin rights).

Now in 10.15 the profile is no longer manually removable from System Preferences, causing somewhat of a problem for us.

Now we have to login to our Jamf Pro instance, find the profile and make an exclusion for any affected host, pushing the profile making the MDM client remove it, and then re-scope the host for the profile (**with** the correct type of adapter connected) making the MDM client re-install it.

It's not _that_ much more work, but it requires someone with admin rights in our Jamf Pro instance to perform it instead of the user.