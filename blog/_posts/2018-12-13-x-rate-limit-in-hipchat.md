---
layout: post
title: Increase RateLimit in on-prem hosted HipChat server
---

We are programatically sending messages to and creating rooms in our HipChat server using API tokens.
Sometimes you might need to increase the <a href="https://developer.atlassian.com/server/hipchat/hipchat-rest-api-rate-limits/">RateLimit</a> to be able to process the number of API requests that the server can handle for any particular API token.

This is how we do it:

Start out by finding out the API token for the proper user:

HipChat web -> login as the API user -> Click avatar in upper right corner -> choose "Account Settings" -> "API access" -> your tokens will be listed or navigate to https://FQDN_of_your_hipchat_server/account/api after login

ssh into your HipChat Server as admin and run this:
```
sudo dont-blame-hipchat
cd /opt/atlassian/hipchat/sbin/
export HIPCHATTOKEN=<your_API_v2_token>
export HIPCHATRATELIMIT=10000
/opt/atlassian/hipchat/virtualenv/bin/python _ratelimit.py
curl --include --insecure https://127.0.0.1/v2/user?auth_token=your_apiv2_auth_token | grep Ratelimit
```
The last curl will tell you what it's set to.

:exclamation: <em>Note:</em> This is neither recommended nor supported by Atlassian (or maybe Slack nowadays)