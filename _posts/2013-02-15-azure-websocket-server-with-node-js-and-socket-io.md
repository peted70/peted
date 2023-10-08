---
layout: single
title: Azure websocket server with node.js and socket.io
date: 2013-02-15 15:44
author_profile: true
comments: true
categories: [azure, Azure, nodejs, Nodejs, socketio, websockets, workerrole]
header:
    teaserlogo: 'assets/images/'
    teaser: 'assets/images/'
---
<p>Peter Daukintis</p>  <p>If you haven’t previously done this download your Azure publish settings file from here <a title="https://windows.azure.com/download/publishprofile.aspx" href="https://windows.azure.com/download/publishprofile.aspx">https://windows.azure.com/download/publishprofile.aspx</a></p>  <p>This will contain your management certificate and details for each of your Azure subscriptions.</p>  <p>&#160;</p>  <p>If you have multiple Azure subscriptions ensure that you have set the ‘current’ one to be the one you wish to use:</p>  <p><strong>Select-AzureSubscription –SubscriptionName your_subscription_name</strong></p>  <p>You can confirm this is set by using,</p>  <p><strong>Get-AzureSubscription -Current</strong>&#160;</p>  <p>Then, to create the Azure project</p>  <p><strong>New-AzureServiceProject your_project_name</strong></p>  <p>This will create the files needed by the Azure cloud project (including deployment settings, cloud/local configuration files and a service definition file). </p>  <p><strong>Add-AzureNodeWorkerRole</strong></p>  <p>This will add a WorkerRole folder which contains the Node project</p>  <p><a href="{{ site.baseurl }}/assets/images/2013/02/ps2.png"><img title="ps2" style="border-top:0;border-right:0;background-image:none;border-bottom:0;float:none;padding-top:0;padding-left:0;margin-left:auto;border-left:0;display:block;padding-right:0;margin-right:auto;" border="0" alt="ps2" src="{{ site.baseurl }}/assets/images/2013/02/ps2_thumb.png" width="658" height="113" /></a></p>  <p>then, from within the WorkRole folder, </p>  <p><strong>npm install azure </strong>(optional – enables access to azure services within your node project see <a title="https://npmjs.org/package/azure" href="https://npmjs.org/package/azure">https://npmjs.org/package/azure</a>)</p>  <p><strong>npm install socket.io</strong></p>  <p>navigate to the worker role folder and replace the code in server.js with the following:</p>  <div class="csharpcode">   <pre class="alt"><span class="lnum">   1:  </span><span class="kwrd">var</span> http = require(<span class="str">'http'</span>); </pre>

  <pre><span class="lnum">   2:  </span><span class="kwrd">var</span> io = require(<span class="str">'socket.io'</span>); </pre>

  <pre class="alt"><span class="lnum">   3:  </span><span class="kwrd">var</span> port = process.env.port || 1337; </pre>

  <pre><span class="lnum">   4:  </span><span class="kwrd">var</span> server = http.createServer(<span class="kwrd">function</span> (req, res) { </pre>

  <pre class="alt"><span class="lnum">   5:  </span>    res.writeHead(200, { <span class="str">'Content-Type'</span>: <span class="str">'text/plain'</span> }); </pre>

  <pre><span class="lnum">   6:  </span>    res.end(<span class="str">'Hello World 2n'</span>); </pre>

  <pre class="alt"><span class="lnum">   7:  </span>}).listen(port);</pre>

  <pre><span class="lnum">   8:  </span>&#160;</pre>

  <pre class="alt"><span class="lnum">   9:  </span><span class="kwrd">var</span> socket = io.listen(server); </pre>

  <pre><span class="lnum">  10:  </span>socket.on(<span class="str">'connection'</span>, <span class="kwrd">function</span> (client) { </pre>

  <pre class="alt"><span class="lnum">  11:  </span>    socket.emit(<span class="str">'news'</span>, { hello: <span class="str">'world'</span> }); </pre>

  <pre><span class="lnum">  12:  </span>    client.on(<span class="str">'message'</span>, <span class="kwrd">function</span>(){ </pre>

  <pre class="alt"><span class="lnum">  13:  </span>        console.log(<span class="str">'message arrive'</span>); </pre>

  <pre><span class="lnum">  14:  </span>        client.send(<span class="str">'some message'</span>); </pre>

  <pre class="alt"><span class="lnum">  15:  </span>    });</pre>

  <pre><span class="lnum">  16:  </span>&#160;</pre>

  <pre class="alt"><span class="lnum">  17:  </span>    client.on(<span class="str">'disconnect'</span>, <span class="kwrd">function</span>(){ </pre>

  <pre><span class="lnum">  18:  </span>        console.log(<span class="str">'connection closed'</span>); </pre>

  <pre class="alt"><span class="lnum">  19:  </span>    }); </pre>

  <pre><span class="lnum">  20:  </span>});</pre>

  <pre class="alt"><span class="lnum">  21:  </span>&#160;</pre>
</div>

.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }

<p>
  <br />then</p>

<p><strong>Publish-AzureServiceProject</strong></p>

<p>Note that subsequent publish commands will result in an upgrade…</p>

<p><strong></strong></p>

<p><a href="{{ site.baseurl }}/assets/images/2013/02/publish.png"><img title="publish" style="border-top:0;border-right:0;background-image:none;border-bottom:0;float:none;padding-top:0;padding-left:0;margin-left:auto;border-left:0;display:block;padding-right:0;margin-right:auto;" border="0" alt="publish" src="{{ site.baseurl }}/assets/images/2013/02/publish_thumb.png" width="669" height="331" /></a></p>

<p>&#160;</p>

<p><strong>Create a .NET client to test the server</strong></p>

<p>Create a new c# console app in visual studio, add SocketIO .NET client Nuget package (Install-Package SocketIO4Net.Client)</p>

<p><a href="{{ site.baseurl }}/assets/images/2013/02/socketionetclientnuget.png"><img title="SocketIONetClientNuget" style="border-top:0;border-right:0;background-image:none;border-bottom:0;float:none;padding-top:0;padding-left:0;margin-left:auto;border-left:0;display:block;padding-right:0;margin-right:auto;" border="0" alt="SocketIONetClientNuget" src="{{ site.baseurl }}/assets/images/2013/02/socketionetclientnuget_thumb.png" width="647" height="463" /></a></p>

<p>Paste the following into the console app…</p>

<div class="csharpcode">
  <pre class="alt"><span class="lnum">   1:  </span>        <span class="kwrd">static</span> <span class="kwrd">void</span> Main(<span class="kwrd">string</span>[] args)</pre>

  <pre><span class="lnum">   2:  </span>        {</pre>

  <pre class="alt"><span class="lnum">   3:  </span>            Console.WriteLine(<span class="str">&quot;Starting TestSocketIOClient Example...&quot;</span>);</pre>

  <pre><span class="lnum">   4:  </span>&#160;</pre>

  <pre class="alt"><span class="lnum">   5:  </span>            <span class="kwrd">using</span> (var socket = <span class="kwrd">new</span> Client(<span class="str">&quot;http://your_project_name.cloudapp.net/&quot;</span>))</pre>

  <pre><span class="lnum">   6:  </span>            {</pre>

  <pre class="alt"><span class="lnum">   7:  </span>                socket.Opened += SocketOpened;</pre>

  <pre><span class="lnum">   8:  </span>                socket.Message += SocketMessage;</pre>

  <pre class="alt"><span class="lnum">   9:  </span>                socket.SocketConnectionClosed += SocketConnectionClosed;</pre>

  <pre><span class="lnum">  10:  </span>                socket.Error += SocketError;</pre>

  <pre class="alt"><span class="lnum">  11:  </span>&#160;</pre>

  <pre><span class="lnum">  12:  </span>                socket.Connect();</pre>

  <pre class="alt"><span class="lnum">  13:  </span>&#160;</pre>

  <pre><span class="lnum">  14:  </span>                Console.ReadLine();</pre>

  <pre class="alt"><span class="lnum">  15:  </span>            }</pre>

  <pre><span class="lnum">  16:  </span>        }</pre>

  <pre class="alt"><span class="lnum">  17:  </span>&#160;</pre>

  <pre><span class="lnum">  18:  </span>        <span class="kwrd">private</span> <span class="kwrd">static</span> <span class="kwrd">void</span> SocketConnectionClosed(<span class="kwrd">object</span> sender, EventArgs e)</pre>

  <pre class="alt"><span class="lnum">  19:  </span>        {</pre>

  <pre><span class="lnum">  20:  </span>            Console.WriteLine(<span class="str">&quot;CLOSED&quot;</span>);</pre>

  <pre class="alt"><span class="lnum">  21:  </span>        }</pre>

  <pre><span class="lnum">  22:  </span>&#160;</pre>

  <pre class="alt"><span class="lnum">  23:  </span>        <span class="kwrd">private</span> <span class="kwrd">static</span> <span class="kwrd">void</span> SocketError(<span class="kwrd">object</span> sender, ErrorEventArgs e)</pre>

  <pre><span class="lnum">  24:  </span>        {</pre>

  <pre class="alt"><span class="lnum">  25:  </span>            Console.WriteLine(e.Message);</pre>

  <pre><span class="lnum">  26:  </span>        }</pre>

  <pre class="alt"><span class="lnum">  27:  </span>&#160;</pre>

  <pre><span class="lnum">  28:  </span>        <span class="kwrd">private</span> <span class="kwrd">static</span> <span class="kwrd">void</span> SocketMessage(<span class="kwrd">object</span> sender, MessageEventArgs e)</pre>

  <pre class="alt"><span class="lnum">  29:  </span>        {</pre>

  <pre><span class="lnum">  30:  </span>            Console.WriteLine(e.Message.MessageType.ToString());</pre>

  <pre class="alt"><span class="lnum">  31:  </span>        }</pre>

  <pre><span class="lnum">  32:  </span>&#160;</pre>

  <pre class="alt"><span class="lnum">  33:  </span>        <span class="kwrd">private</span> <span class="kwrd">static</span> <span class="kwrd">void</span> SocketOpened(<span class="kwrd">object</span> sender, EventArgs e)</pre>

  <pre><span class="lnum">  34:  </span>        {</pre>

  <pre class="alt"><span class="lnum">  35:  </span>            Console.WriteLine(<span class="str">&quot;OPENED&quot;</span>);</pre>

  <pre><span class="lnum">  36:  </span>        }</pre>
</div>

.csharpcode, .csharpcode pre
{
	font-size: small;
	color: black;
	font-family: consolas, "Courier New", courier, monospace;
	background-color: #ffffff;
	/*white-space: pre;*/
}
.csharpcode pre { margin: 0em; }
.csharpcode .rem { color: #008000; }
.csharpcode .kwrd { color: #0000ff; }
.csharpcode .str { color: #006080; }
.csharpcode .op { color: #0000c0; }
.csharpcode .preproc { color: #cc6633; }
.csharpcode .asp { background-color: #ffff00; }
.csharpcode .html { color: #800000; }
.csharpcode .attr { color: #ff0000; }
.csharpcode .alt 
{
	background-color: #f4f4f4;
	width: 100%;
	margin: 0em;
}
.csharpcode .lnum { color: #606060; }

<p>That’s it.</p>
