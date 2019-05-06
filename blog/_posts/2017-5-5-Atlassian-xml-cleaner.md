---
layout: post
title: Atlassian xml-cleaner
---

Sometimes when doing export/import via XML of <a href="https://www.atlassian.com">Atlassian</a> products (<a href="https://www.atlassian.com/software/jira">Jira</a>, <a href="https://www.atlassian.com/software/confluence">Confluence</a> etc.) you might run into a problem when importing because of invalid characters in the XML-file.

That's usually solved by using the atlassian-xml-cleaner JAR.

It can be found <a href="https://confluence.atlassian.com/jira/removing-invalid-characters-from-xml-backups-12079.html">here</a>.

And if your Jira/Confluence doesn't allow you to login after trying a restore and failing.  Just restart it and it will come up as unconfigured (at least Jira) and run the cleaner on your dump and then just select "Import existing data" to setup Jira again from the dump
