---
layout: post
title: Normalize HipChat usernames
comments: false
---

When exporting data from HipChat Server or Data Center you will end up with unusable usernames due to the fact that HipChat doesn't care about usernames, it uses email address as it's authentication id.

It will create usernames (or mention names (mention_name variable) as it calls it) from your users full names, i.e. Firstname Lastname becomes FirstnameLastname, it will then allow users to change this into whatever they like.
Now trying to import all these users into another system that uses AD/LDAP for authentication will most likely not work since your AD/LDAP will probably (most likely) not be setup to use accountnames constructed like FirstnameLastname. When importing into for instance Mattermost it will complain about email address already used with another username and bail out.

Let's look at an example:

Data in AD:

Firstname Lastname, username, email address<br>
John Doe, johnd, john.doe@company.com

Data in HC:

Firstname Lastname, mention name, email address<br>
John Doe, JohnDoe, john.doe@company.com

Now, any _proper_ auth system will recognize these two as _different_ users with the same fullname and email address, but since only email address is signinficant in HipChat - it will see it as the same user with different nicknames.

Rocket.chat will import these users, but they won't be able to login using their username, since there won't be a user johnd in the system. The admin of your Rocket.chat instance will have to _manually_ reset usernames one-by-one.

Mattermost will *not* import these users, as it will already have the email address in the system and see johnd as a conflicting user since JohnDoe already has that email address.

With a little help from a couple of friends I've written a <a href="https://github.com/jby/python/blob/master/normalize_hc_username.py">python script</a> that will take the exported users.json, match the email addresses of all users and replace the mention_name with the users proper username from a CSV-export of users from our AD.

After that I recommend using the <a href="https://github.com/ergon/migratemost">Migratemost script</a> to convert your data from HipChat json to Mattermost jsonl, if you are going to import into Mattermost.

The import into Rocket.chat is much more straight forward, with a GUI in the admin panel. You *will* however have to re-pack the export before importing it into Rocket.chat, since it wants a decrypted tar.gz-file or a zip-file.
