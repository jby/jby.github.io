---
layout: post
title: Confluence upgrade and migration
---

I have spent a couple of weeks planning and testing an upgrade and migration of our production-instance of <a href="https://atlassian.com/software/confluence/">Confluence</a>.

Previously it ran in a <a href="https://www.linux-kvm.org/page/Main_Page">KVM-VM</a> with <a href="https://www.centos.org/">CentOS</a> 6, <a href="https://www.apache.org/">apache</a> 2.2 and local <a href="https://www.mysql.com/">MySQL</a> database.

Now it runs in another KVM-VM with CentOS 7, apache 2.4 which is now required, since Atlassian have added a <a href="https://www.atlassian.com/blog/confluence/collaborative-editing-confluence-6-0"collaborative editing</a> feature, kind of like google docs, and a remote <a href="https://www.postgresql.org/">PostgreSQL</a> database.

Confluence itself was upgraded from 5.8.5 to 6.1.3.

* Install and configure PostgreSQL on a dedicated DB-server
* Install and prepare a new KVM-VM
* Install latest version of Confluence in new VM and connect it to the Postgres DB
* Take an XML-backup of running production Confluence
* Restore the XML-backup into new Confluence, thereby populating the Postgres DB and hence migrate
* Update DNS to reflect new reality
* Done!!
* Have beer!
