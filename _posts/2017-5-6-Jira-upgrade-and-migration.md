--
layout: posts
title: Jira upgrade and migration
--

I have spent a couple of weeks planning and testing an upgrade and migration of our production-instance of <a href="https://atlassian.com/software/jira/">JIRA</a>.

Previously it ran in a <a href="https://www.linux-kvm.org/page/Main_Page">KVM-VM</a> with <a href="https://www.centos.org/">CentOS</a> 6, <a href="https://www.apache.org/">apache</a> 2.2 and local <a href="https://www.mysql.com/">MySQL</a> database.

Now it runs in another KVM-VM with CentOS 7, apache 2.4 and a remote <a href="https://www.postgresql.org/">PostgreSQL</a> database.

Jira itself was upgraded from 7.2.0 to 7.3.4 and <a href="https://www.atlassian.com/software/jira/service-desk">Jira Servicedesk</a> was upgraded to 3.4.1 in the process.

* Install and configure PostgreSQL on a dedicated DB-server
* Install and prepare a new KVM-VM
* Install latest version of Jira in new VM and connect it to the Postgres DB
* Take an XML-backup of running production Jira
* Restore the XML-backup into new Jira, thereby populating the Postgres DB and hence migrate
* Update DNS to reflect new reality
* Done!!
* Have beer!
