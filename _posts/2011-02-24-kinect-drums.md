---
layout: post
title: kinect drums
date: 2011-02-24 10:48
author: peted70
comments: true
categories: [Kinect]
---
<p>Whilst exploring the possibilities opened by the Kinect device I made a demo using the OpenNI framework <a title="http://www.openni.org/" href="http://www.openni.org/">http://www.openni.org/</a> with PrimeSense NITE components.</p>  <p>&#160;</p>  <div style="width:448px;display:block;float:none;margin:0 auto;padding:0;" id="scid:5737277B-5D6D-4f48-ABFC-DD9C333F4C5D:039823d5-48e7-4177-ba4c-4a6b3b05cc0c" class="wlWriterEditableSmartContent"><div>[youtube=http://www.youtube.com/watch?v=9b3Z4aT-kqk&amp;w=448&amp;h=252&amp;hd=1]</div><div style="width:448px;clear:both;font-size:.8em;">Kinect Drums</div></div>  <p>As you can see from the video it allows tracking of multiple ‘players’. I started out by using ManagedNITE and using the hand tracking it provides but had some trouble getting a second hand to track (it worked but seemed less responsive than the first hand and was difficult to start the tracking). </p>  <p>This is roughly how the demo was made:</p>  <ul>   <li>modification of the NITE c++ sample which does user tracking (Players) to track multiple users</li>    <li>wrapping this in a c+/cli component for consumption in a .NET environment</li>    <li>consumed the .NET component in a WPF 3D desktop application</li> </ul>
