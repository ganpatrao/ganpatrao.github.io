--- 
layout: post
title: "pyFreeSMS: A Python API"
categories: 
- Code
- Python
tags: 
- 160by2
- pyFreeSMS
- python
status: publish
type: post
published: true
meta: 
  _wpas_done_fb: "1"
  _wpas_done_twitter: "1"
  _oembed_c7dbd74c6d7494df7e6d0354baeafe9f: "{{unknown}}"
  _oembed_adcbaef352d5aa655ce9da04950433a6: "{{unknown}}"
  _oembed_d6d51a50cab290a22287c9f2854ce2da: "{{unknown}}"
  _oembed_49dd9d4bd797ed859c2af3d91ed0cb83: "{{unknown}}"
  _oembed_6c34d7725f277105f92dc0d8926a919a: "{{unknown}}"
  _oembed_f917596309ee4ee147b1356017c6f989: "{{unknown}}"
  _oembed_628d1f0827f7d24a281543119e554fc3: "{{unknown}}"
  _oembed_14f611d5036cfc12aae892382ca39893: "{{unknown}}"
  _oembed_985c5d9a6ffe432f8f4a34e091cc3849: "{{unknown}}"
---
This is a Python API to send free messages via the many online free SMS providers. It currently works only with 160by2; I'll add support for Way2SMS et all soon, though I'm hoping people interested in a specific provider will extend this API themselves.

<strong>How Will This Help Me?</strong>

This is primarily intended for developers looking to send messages from their Python applications. For instance, I'm currently developing an <a href="http://passportbug.appspot.com" target="_blank">application</a> that periodically checks my passport application status and messages it to me.

<!--more-->

<strong>Getting It</strong>

1. Go <a href="https://github.com/emaadmanzoor/pyFreeSMS/" target="_blank">here</a>, click on the <strong>Downloads </strong>button at the top-right, and select the archive format that you're comfortable with.

2. Extracting the archive should now show you the <strong>pyFreeSMS</strong> folder and a <strong>README</strong> file.

<strong>Using It</strong>

1. Copy the <strong>pyFreeSMS</strong> folder to the directory where your code resides.

2. <strong>Import</strong> the API:
<pre>from pyFreeSMS import py160by2</pre>
3. Send your message with the <strong>sendmessage(username, password, targetPhoneNumber, message) function</strong>, replacing your account authentication details<sup>*</sup> where necessary:
<pre>py160by2.sendMessage('usrname','pwd','123','msg')</pre>
<strong>Contributing</strong>

Considering the number of free SMS providers out there, I'm hoping there are a lot of you who might want to extend this API to your liking. To start off, you'll first need to <strong>install Git:</strong>
<pre>sudo apt-get install git-core<strong>
</strong></pre>
Then you'll need to <strong>get the code:</strong>
<pre>git clone \
https://emaadmanzoor@github.com/emaadmanzoor/pyFreeSMS.git</pre>
This will download the required source code to the current directory. To<strong> commit</strong> your extended code and added modules, it'd be best to read up the friendly documentation at <a href="http://github.com" target="_blank">github</a> itself.
