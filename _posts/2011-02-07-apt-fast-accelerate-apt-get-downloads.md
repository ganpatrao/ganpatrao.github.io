--- 
layout: post
title: "apt-fast: Accelerate Apt-Get Downloads"
categories: 
- Linux
- Software
tags: 
- accelerate apt-get
- apt-fast
- apt-get
status: publish
type: post
published: true
meta: 
  _wpas_done_fb: "1"
  _wpas_mess: "apt-fast: Accelerate Apt-Get Downloads: http://wp.me/pDRsK-4Q"
  _wpas_skip_twitter: "1"
---
apt-fast accelerates your apt-get downloads using axel as a download manager. Axel segments the downloads and fetches them from multiple servers; typical download manager stuff. The developer claims an <strong>increase in speed up to 26 times</strong>. To use this:

<strong>A. Install apt-fast</strong>

1. Add <strong>ppa:tldm217/tahutek.net</strong> to your Software Repositories
2. sudo apt-get update
3. sudo apt-get install apt-fast

<strong>B. Configure Your Proxy</strong>

1. sudo gedit /etc/axelrc
2. Look for the line:
<pre># http_proxy =</pre>
Change this to:
<pre>http_proxy = http://10.1.8.30:8080</pre>
Ensure you remove the leading <strong>"#"</strong> and replace <strong>"10.1.8.30:8080" </strong>with your <strong>own proxy address and port</strong>.

3. Save and close gedit

<strong>C. Try it out</strong>

<strong>sudo apt-fast update</strong>
<strong> sudo apt-fast upgrade</strong>
<strong> sudo apt-fast install &lt;your package here&gt;</strong>
