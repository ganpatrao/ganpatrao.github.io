--- 
layout: post
title: The Samsung Galaxy 3, Android, USB and Linux
categories: 
- Linux
- Troubleshooting
tags: 
- adb
- android
- device debugging
- linux
- samsung galaxy 3
status: publish
type: post
published: true
meta: 
  _wpas_done_twitter: "1"
  _wpas_done_fb: "1"
---
This is a hack to get the Samsung Galaxy 3 to mount as a USB drive on Linux. For some reason, it doesn't react to being plugged in to my computer, apart from a meek beep and an indication that it's slyly sucking power from the USB port.
<ol>
	<li>Disconnect your phone from the computer.</li>
	<li>Settings -&gt; About Phone -&gt; USB Settings -&gt; <strong>Ask on connection</strong></li>
	<li>Settings -&gt; Applications -&gt; Development: Check both <strong>USB debugging</strong> and <strong>Stay awake</strong>.</li>
	<li>Dial <strong>*#7284#</strong> to open <strong>PhoneUtils</strong>.</li>
	<li>Set both the UART and USB modes to<strong> PDA</strong> instead of Modem.</li>
	<li>Connect the phone to the computer.</li>
	<li>You should now receive some sort of USB notification on your phone; you know what to do from here.</li>
</ol>
This worked for me! Do post in your comments if it worked for you too, or not.

<em>You may need to change the PhoneUtils settings back to the old ones when using your phone as a USB modem.</em>
