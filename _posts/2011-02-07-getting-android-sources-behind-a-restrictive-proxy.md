--- 
layout: post
title: Getting Android Sources Behind A Restrictive Proxy
categories: 
- Android
- Code
- Linux
- Troubleshooting
tags: 
- android source
- repo sync behind proxy
- restrictive proxy
- squid
status: publish
type: post
published: true
meta: 
  _wpas_done_fb: "1"
  _wpas_skip_twitter: "1"
---
<p style="text-align:center;"><a href="http://halfclosed.files.wordpress.com/2011/02/squid-vs-android.jpg"><img class="aligncenter size-full wp-image-397" title="squid-vs-android" src="http://halfclosed.files.wordpress.com/2011/02/squid-vs-android.jpg" alt="" width="500" height="210" /></a></p>
<p style="text-align:left;"><a href="http://halfclosed.files.wordpress.com/2011/02/squid-vs-android.jpg"></a>I'll have to assume you're suffocated by both the following bottlenecks in getting the Android Open Source Project code:</p>

<ul>
	<li style="text-align:left;">Blocked <strong>git:// </strong>protocol and port</li>
	<li style="text-align:left;">A limit on the amount you're allowed to download</li>
</ul>
The Android sources amount to around <strong>6GB</strong> in total, so anonymous proxy programs like <a href="http://www.your-freedom.net/" target="_blank"><strong> </strong>Your-Freedom</a> will choke up after a fixed time limit; and <strong>repo sync (git) does not resume downloads</strong> between projects; though you may resume from the last project you downloaded. To smoothen out the rough edges, do this:
<ol>
	<li>Set your git proxy using this command, replacing what's necessary: <strong>git config --global http.proxy 10.1.8.30:8080
</strong></li>
	<li>Follow the steps <a href="http://source.android.com/source/download.html" target="_blank">here</a> until you reach <strong>Getting The Files</strong>: this is the part that won't work behind a restrictive proxyy.</li>
	<li>Switch to the directory where you initially ran <strong>repo init -u </strong>on the command line and then type in <strong>ls -a</strong>; you should be able to see a <strong>.repo </strong>folder. If you don't, it means your <strong>repo init -u</strong> failed for some reason.</li>
	<li>Type in <strong>gedit .repo/manifest.xml </strong>and change <strong>line 4</strong> to read: <strong>fetch="https://android.git.kernel.org/"</strong></li>
	<li>Type in <strong>gedit .repo/repo/repo </strong>and change <strong>line 5</strong> to read: <strong>REPO_URL='http://android.git.kernel.org/tools/repo.git'</strong></li>
	<li>Download the <strong>modified repo script <a href="https://docs.google.com/leaf?id=0B76d_EyUt5JwNDU5NzFiNzMtYWQ1NC00ZmNlLThiNjQtY2M1NTM3MDQzMzBi&amp;hl=en" target="_blank">here</a></strong> and replace your old repo script with the modified one.</li>
	<li>Continue with <strong>Getting The Files </strong>at the <a href="http://source.android.com/source/download.html">Android Open Source Project website</a> and things should be working fine.</li>
</ol>
<em>Image Credits: <a href="http://www.androidstickers.com/" target="_blank">Android Stickers</a></em>
