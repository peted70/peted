---
layout: post
title: wp7–JSON parsing
date: 2012-04-04 12:59
author: peted70
comments: true
categories: [datacontractjsonserializer, json, serialization, Windows Phone 7, wp7]
---
<p>Peter Daukintis</p>  <p>I’m writing this mainly as a central place to store this code as I need to use it frequently – hopefully it will help others also.</p>  <p>Supposing I have a web service that I want to call that returns me some JSON data. I want to get that from the service and into a class which I will use as a domain model in my application. The first thing to do is to make sure that we understand the hierarchy that the JSON data represents. So for example,</p>  <div style="margin:0;display:inline;float:none;padding:0;" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:b2457821-2587-447a-af12-6d7212a200af" class="wlWriterEditableSmartContent"> <div style="border:#000080 1px solid;color:#000;font-family:'Courier New', Courier, Monospace;font-size:10pt;"> <div style="background-color:#000000;overflow:auto;padding:2px 5px;"><span style="color:#f3f3f3;">{ IsSuccess: </span><span style="color:#a2c4fd;">true</span><span style="color:#f3f3f3;"> } </span></div> </div> </div>  <p>is an object with a boolean property ‘IsSuccess’ which in this case is true. So we can read the {}’s as enclosing an object.</p>  <div style="margin:0;display:inline;float:none;padding:0;" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:787e4301-ffa9-4b9f-8122-827ef204cd04" class="wlWriterEditableSmartContent"> <div style="border:#000080 1px solid;color:#000;font-family:'Courier New', Courier, Monospace;font-size:10pt;"> <div style="background-color:#000000;overflow:auto;padding:2px 5px;"><span style="color:#f3f3f3;">[{ IsSuccess: </span><span style="color:#a2c4fd;">true</span><span style="color:#f3f3f3;"> },{ IsSuccess: </span><span style="color:#a2c4fd;">false</span><span style="color:#f3f3f3;"> }]  </span></div> </div> </div>  <p>This represents an array of objects; read the []’s as enclosing an array.</p>  <p>Here’s a slightly more complicated example:</p>  <div style="margin:0;display:inline;float:none;padding:0;" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:16e84931-5f6b-4910-bec7-b108b91fdc14" class="wlWriterEditableSmartContent"> <div style="border:#000080 1px solid;color:#000;font-family:'Courier New', Courier, Monospace;font-size:10pt;"> <div style="background-color:#000000;overflow:auto;padding:2px 5px;"><span style="color:#75ffa2;">// start</span><br> <span style="color:#f3f3f3;">{</span><br>    <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;FleetsCollection&quot;</span><span style="color:#f3f3f3;">:[</span><br>       <span style="color:#f3f3f3;">{</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;FleetId&quot;</span><span style="color:#f3f3f3;">:2,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;Nickname&quot;</span><span style="color:#f3f3f3;">:</span><span style="color:#6afd51;">&quot;2007 Ninja ZX6R&quot;</span><span style="color:#f3f3f3;">,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;PictureFileName&quot;</span><span style="color:#f3f3f3;">:</span><span style="color:#6afd51;">&quot;jvmlfdaq.rkr2.jpg&quot;</span><span style="color:#f3f3f3;">,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;AverageMpg&quot;</span><span style="color:#f3f3f3;">:43.90925,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;MaxMpg&quot;</span><span style="color:#f3f3f3;">:47.945</span><br>       <span style="color:#f3f3f3;">},</span><br>       <span style="color:#f3f3f3;">{</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;FleetId&quot;</span><span style="color:#f3f3f3;">:44,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;Nickname&quot;</span><span style="color:#f3f3f3;">:</span><span style="color:#6afd51;">&quot;Luminous Neptune&quot;</span><span style="color:#f3f3f3;">,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;PictureFileName&quot;</span><span style="color:#f3f3f3;">:</span><span style="color:#6afd51;">&quot;ochufm0c.ohm2.png&quot;</span><span style="color:#f3f3f3;">,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;AverageMpg&quot;</span><span style="color:#f3f3f3;">:29.4285,</span><br>          <span style="color:#f3f3f3;"></span><span style="color:#6afd51;">&quot;MaxMpg&quot;</span><span style="color:#f3f3f3;">:30.341</span><br>       <span style="color:#f3f3f3;">}</span><br>    <span style="color:#f3f3f3;">]</span><br> <span style="color:#f3f3f3;">}</span></div> </div> </div>  <p>(Note – useful JSON formatter here <a title="http://jsonformatter.curiousconcept.com/" href="http://jsonformatter.curiousconcept.com/">http://jsonformatter.curiousconcept.com/</a>)</p>  <p>I can interpret this as an object with a property ‘FleetsCollection’ which is a list of objects which have further properties. In fact, if you take your JSON string data you can convert it to a c# class using this online tool <a title="http://json2csharp.com/" href="http://json2csharp.com/">http://json2csharp.com/</a></p>  <p>For our example, the online tool generates,</p>  <div style="margin:0;display:inline;float:none;padding:0;" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:38c6629a-18e0-4daa-9d9a-96992ed35d83" class="wlWriterEditableSmartContent"> <div style="border:#000080 1px solid;color:#000;font-family:'Courier New', Courier, Monospace;font-size:10pt;"> <div style="background-color:#000000;overflow:auto;padding:2px 5px;"><span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#a2c4fd;">class</span><span style="color:#f3f3f3;"> </span><span style="color:#fdf8b9;">FleetsCollection</span><br> <span style="color:#f3f3f3;">{</span><br>     <span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#a2c4fd;">int</span><span style="color:#f3f3f3;"> FleetId { </span><span style="color:#a2c4fd;">get</span><span style="color:#f3f3f3;">; </span><span style="color:#a2c4fd;">set</span><span style="color:#f3f3f3;">; }</span><br>     <span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#a2c4fd;">string</span><span style="color:#f3f3f3;"> Nickname { </span><span style="color:#a2c4fd;">get</span><span style="color:#f3f3f3;">; </span><span style="color:#a2c4fd;">set</span><span style="color:#f3f3f3;">; }</span><br>     <span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#a2c4fd;">string</span><span style="color:#f3f3f3;"> PictureFileName { </span><span style="color:#a2c4fd;">get</span><span style="color:#f3f3f3;">; </span><span style="color:#a2c4fd;">set</span><span style="color:#f3f3f3;">; }</span><br>     <span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#a2c4fd;">double</span><span style="color:#f3f3f3;"> AverageMpg { </span><span style="color:#a2c4fd;">get</span><span style="color:#f3f3f3;">; </span><span style="color:#a2c4fd;">set</span><span style="color:#f3f3f3;">; }</span><br>     <span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#a2c4fd;">double</span><span style="color:#f3f3f3;"> MaxMpg { </span><span style="color:#a2c4fd;">get</span><span style="color:#f3f3f3;">; </span><span style="color:#a2c4fd;">set</span><span style="color:#f3f3f3;">; }</span><br> <span style="color:#f3f3f3;">}</span><br> <br> <span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#a2c4fd;">class</span><span style="color:#f3f3f3;"> </span><span style="color:#fdf8b9;">RootObject</span><br> <span style="color:#f3f3f3;">{</span><br>     <span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">public</span><span style="color:#f3f3f3;"> </span><span style="color:#fdf8b9;">List</span><span style="color:#f3f3f3;">&lt;</span><span style="color:#fdf8b9;">FleetsCollection</span><span style="color:#f3f3f3;">&gt; FleetsCollection { </span><span style="color:#a2c4fd;">get</span><span style="color:#f3f3f3;">; </span><span style="color:#a2c4fd;">set</span><span style="color:#f3f3f3;">; }</span><br> <span style="color:#f3f3f3;">}</span></div> </div> </div>  <p> Then, we can read the data in using the following code:</p>  <div style="margin:0;display:inline;float:none;padding:0;" id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:8119fb86-efb4-43ff-9e98-3404f9758fc3" class="wlWriterEditableSmartContent"> <div style="border:#000080 1px solid;color:#000;font-family:'Courier New', Courier, Monospace;font-size:10pt;"> <div style="background-color:#000000;overflow:auto;padding:2px 5px;"><span style="color:#f3f3f3;"></span><span style="color:#a2c4fd;">var</span><span style="color:#f3f3f3;"> dataContractJsonSerializer = </span><span style="color:#a2c4fd;">new</span><span style="color:#f3f3f3;"> </span><span style="color:#fdf8b9;">DataContractJsonSerializer</span><span style="color:#f3f3f3;">(</span><span style="color:#a2c4fd;">typeof</span><span style="color:#f3f3f3;">(</span><span style="color:#fdf8b9;">RootObject</span><span style="color:#f3f3f3;">));</span><br> <span style="color:#f3f3f3;"></span><span style="color:#fdf8b9;">RootObject</span><span style="color:#f3f3f3;"> readObject = (</span><span style="color:#fdf8b9;">RootObject</span><span style="color:#f3f3f3;">)dataContractJsonSerializer.ReadObject(memoryStream);</span></div> </div> </div>