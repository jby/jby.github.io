---
layout: post
title: JSS tomcat behind Apache proxy
---

So, I've now spent two days setting upp an apache proxy in front of the built-in tomcat 8 in my new JSS.

The tricky part is tomcat, whose server.xml-file comes cluttered with all possible options at install.

This is what the important part of my server.xml looks like now:

```xml
<Connector
  URIEncoding="UTF-8"
  port="8080"
  address="127.0.0.1"
  executor="tomcatThreadPool"
  protocol="HTTP/1.1"
  scheme="https"
  connectionTimeout="20000"
  proxyName="jss.company.com"
  proxyPort="443"
  redirectPort="443"
  />
```
