--- 
layout: post
title: "Verilog On Linux: Defenestrating Modelsim"
categories: 
- Code
- Software
tags: 
- linux
- modelsim
- verilog
status: publish
type: post
published: true
meta: 
  _wpas_done_twitter: "1"
  _wpas_done_fb: "1"
  _wp_old_slug: verilog-on-linux-defenestrating-modelsim
---
This tutorial's to the victims of the Modelsim user experience, forced to design hardware in chains and gracefully handle the precious rear end of the popular musty racehorse that's Mentor Graphics' stable-star. If you're on Linux, freedom's a nice option.


<blockquote>Defenestrate<em> /diˈfɛn<img src="http://sp.dictionary.com/dictstatic/dictionary/graphics/luna/thinsp.png" alt="" border="0" />əˌstreɪt/</em> (verb) : to throw (a person or thing) out of a window. The word originated from a couple of incidents in Prague, back in the 14th century, when a bunch of guys stormed in and tossed seven town officials out the window (quite literally).</blockquote>



I'll walk you through the steps to start coding in Verilog on your Linux box:

<!--more-->
<ol>
	<li>Install <a href="http://www.icarus.com/eda/verilog/" target="_blank">Icarus Verilog Simulator</a> and <a href="http://en.wikipedia.org/wiki/GTKWave" target="_blank">GTKWave</a></li>
[sourcecode lang="bash" light="true"]sudo apt-get install verilog gtkwave[/sourcecode]
	<li>Copy this sample code and save it as <strong>counter.v</strong>:</li>
[sourcecode lang="javascript"]module counter(out, clk, reset);

  parameter WIDTH = 8;

  output [WIDTH-1 : 0] out;
  input            clk, reset;

  reg [WIDTH-1 : 0]   out;
  wire            clk, reset;

  always @(posedge clk)
    out &lt;= out + 1;

  always @reset
    if (reset)
      assign out = 0;
    else
      deassign out;

endmodule // counter[/sourcecode]
	<li>Copy this sample code and save it as <strong>counter_tb.v</strong></li>
[sourcecode lang="javascript"]module test;

 /* Make a reset that pulses once. */
 reg reset = 0;
 initial begin
 $dumpfile(&quot;test.vcd&quot;);
 $dumpvars(0,test);

 # 17 reset = 1;
 # 11 reset = 0;
 # 29 reset = 1;
 # 5  reset =0;
 # 513 $finish;
 end

 /* Make a regular pulsing clock. */
 reg clk = 0;
 always #1 clk = !clk;

 wire [7:0] value;
 counter c1 (value, clk, reset);

 initial
 $monitor(&quot;At time %t, value = %h (%0d)&quot;,
 $time, value, value);
endmodule // test
[/sourcecode]
	<li>Open a terminal in the same directory and compile the code with this command:</li>
[sourcecode lang="bash" light="true"]iverilog -o dsn counter_tb.v counter.v[/sourcecode]
	<li>Run the code with this command:</li>
[sourcecode lang="bash" light="true"]vvp dsn[/sourcecode]
	<li>Simulate the generated output with this:</li>
[sourcecode lang="bash" light="true"]gtkwave test.vcd &amp;[/sourcecode]
Expand <strong>test</strong> on the top-left panel and select <strong>c1</strong>, then drag the signals from the bottom-left (wire, reg) to the Signals panel to it's immediate right. This should get you out on the right side of the bed with Verilog on Linux. Your next steps lie <a href="http://iverilog.wikia.com" target="_blank">here</a>. All sample code and instructions are merely distilled from the extensive documentation at the wiki linked to above. Cheers!</ol>
