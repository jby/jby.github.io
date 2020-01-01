---
layout: post
title: Synology DS918+ running Plex
comments: false
---

So, I decided to retire my old Mac mini with external USB-drives runnning <a href="https://plex.tv/">Plex</a>, with <a href="https://sonarr.tv/">Sonarr</a>, <a href="https://radarr.video/">Radarr</a>, <link type="a" href="https://deluge-torrent.org/">Deluge</a> (previously), <a href="https://transmissionbt.com/">Transmission</a>, <a href="https://sabnzbd.org/">SABNzb</a> and <a href="https://github.com/Jackett/Jackett">Jackett</a> for downloading and organizing content.

I now have a <a href="https://www.synology.com/en-global/products/DS918+">Synology DS918+</a> with 4 8TB drives instead.

I'm using the native, though manually downloaded and installed, Plex package. And I'm running Sonarr, Radarr and Jacket in <a href="https://www.docker.com/">Docker</a> containers, all using the built-in <a href="https://www.synology.com/en-global/dsm/packages/DownloadStation">Download station</a> in the <a href="https://www.synology.com/en-global/dsm">Synology DSM</a>.

There are a couple of hurdles that you have to overcome, but nothing too difficult.

<ul>
	<li><a href="https://drfrankenstein.co.uk/">Dr Frankensteins setup instructions</a></li>
	<li>Make sure you have all the right permissions, most is found in the above instructions</li>
	<li>The sonarr and radarr users each have to login to your DSM to setup the Download Station.</li>
	<li>Also, the Download Station can unpack (extract) downloaded archives, see <a href="https://www.synology.com/en-global/knowledgebase/DSM/help/DownloadStation/auto_unzip">the documentation</a></li>
	<li>After you've enabled auto extraction as an admin user - each of your downloading users (sonarr and radarr in my case) has to enable it for themselves as well. Login to the DSM as each of these users, start the Downloading station app and enable auto extraction in the settings.
</ul>
