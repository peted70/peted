---
layout: post
title: Kinect V2 - Simple Sound Detection C++
date: 2015-04-19 17:52
author: peted70
comments: true
categories: [audio, c#, c++, kinect, Kinect]
---
<p>This is a very simple post showing an implementation of the detection of a sound, any sound above a threshold, using the Kinect V2 SDK. The sample is written as a C++ XAML Windows Store app. The code uses the usual pattern for retrieving data from the Kinect via the SDK; that is, </p> <blockquote> <p>Get Sensor </p> <p>Choose Datasource</p> <p>Open a Reader on the DataSource</p> <p>Subscribe to the FrameArrived event on the Reader</p> <p>Open the Sensor</p></blockquote> <p>This looks like this:</p> <div id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:420dda13-2b7b-4365-b37e-444124eaa8a7" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"> <div class="le-pavsc-container"> <div style="background: #ddd; overflow: auto"> <ol start="1" style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;"> <li><span style="background:#1e1e1e;color:#569cd6">void</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#4ec9b0">MainPage</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">OnLoaded</span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#c8c8c8">Platform</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#4ec9b0">Object</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">^</span><span style="background:#1e1e1e;color:#7f7f7f">sender</span><span style="background:#1e1e1e;color:#b4b4b4">,</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#c8c8c8">Windows</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">UI</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">Xaml</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#4ec9b0">RoutedEventArgs</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">^</span><span style="background:#1e1e1e;color:#7f7f7f">e</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li class="le-pavsc-even"><span style="background:#1e1e1e;color:#dcdcdc">{</span></li> <li>    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dadada">_kinect</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#4ec9b0">KinectSensor</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">GetDefault</span><span style="background:#1e1e1e;color:#b4b4b4">();</span></li> <li class="le-pavsc-even">&nbsp;</li> <li>    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">auto</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#c8c8c8">audioSource</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#dadada">_kinect</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#c8c8c8">AudioSource</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li class="le-pavsc-even">&nbsp;</li> <li>    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#57a64a">// Allocate byte array to hold the audio data</span></li> <li class="le-pavsc-even">    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dadada">_audioBuffer</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">ref</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">new</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#4ec9b0">Array</span><span style="background:#1e1e1e;color:#b4b4b4">&lt;</span><span style="background:#1e1e1e;color:#4ec9b0">byte</span><span style="background:#1e1e1e;color:#b4b4b4">&gt;(</span><span style="background:#1e1e1e;color:#c8c8c8">audioSource</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#c8c8c8">SubFrameLengthInBytes</span><span style="background:#1e1e1e;color:#b4b4b4">);</span></li> <li>&nbsp;</li> <li class="le-pavsc-even">    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dadada">_reader</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#c8c8c8">audioSource</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#c8c8c8">OpenReader</span><span style="background:#1e1e1e;color:#b4b4b4">();</span></li> <li>&nbsp;</li> <li class="le-pavsc-even">    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dadada">_reader</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#c8c8c8">FrameArrived</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">+=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">ref</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">new</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#c8c8c8">Windows</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">Foundation</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#4ec9b0">TypedEventHandler</span><span style="background:#1e1e1e;color:#b4b4b4">&lt;</span><span style="background:#1e1e1e;color:#c8c8c8">WindowsPreview</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">Kinect</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#4ec9b0">AudioBeamFrameReader</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">^,</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#c8c8c8">WindowsPreview</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">Kinect</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#4ec9b0">AudioBeamFrameArrivedEventArgs</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">^&gt;(</span><span style="background:#1e1e1e;color:#569cd6">this</span><span style="background:#1e1e1e;color:#b4b4b4">,</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">&amp;</span><span style="background:#1e1e1e;color:#4ec9b0">MainPage</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#c8c8c8">OnFrameArrived</span><span style="background:#1e1e1e;color:#b4b4b4">);</span></li> <li>    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dadada">_kinect</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#c8c8c8">Open</span><span style="background:#1e1e1e;color:#b4b4b4">();</span></li> <li class="le-pavsc-even"><span style="background:#1e1e1e;color:#dcdcdc">}</span></li> </ol> </div> </div> </div> <p>&nbsp;</p> <p>The the FrameArrived handler goes something like this:</p> <blockquote> <p>Get BeamFrame Collection </p> <p>GetBeamFrame Collection first member and iterate over the SubFrames within</p> <p>For each valid SubFrame copy out the Audio data</p> <p>Process the audio data</p></blockquote> <div id="scid:9ce6104f-a9aa-4a17-a79f-3a39532ebf7c:82a7e456-6e97-4c39-81a6-dd8856700f92" class="wlWriterEditableSmartContent" style="float: none; padding-bottom: 0px; padding-top: 0px; padding-left: 0px; margin: 0px; display: inline; padding-right: 0px"> <div class="le-pavsc-container"> <div style="background: #ddd; overflow: auto"> <ol start="1" style="background: #000000; margin: 0 0 0 2.5em; padding: 0 0 0 5px;"> <li><span style="background:#1e1e1e;color:#569cd6">void</span><span style="background:#1e1e1e;color:#dcdcdc"> MainPage</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#dcdcdc">OnFrameArrived</span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">WindowsPreview</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#dcdcdc">Kinect</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#dcdcdc">AudioBeamFrameReader </span><span style="background:#1e1e1e;color:#b4b4b4">^</span><span style="background:#1e1e1e;color:#dcdcdc">sender</span><span style="background:#1e1e1e;color:#b4b4b4">,</span><span style="background:#1e1e1e;color:#dcdcdc"> WindowsPreview</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#dcdcdc">Kinect</span><span style="background:#1e1e1e;color:#b4b4b4">::</span><span style="background:#1e1e1e;color:#dcdcdc">AudioBeamFrameArrivedEventArgs </span><span style="background:#1e1e1e;color:#b4b4b4">^</span><span style="background:#1e1e1e;color:#dcdcdc">args</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li class="le-pavsc-even"><span style="background:#1e1e1e;color:#dcdcdc">{</span></li> <li>    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">auto</span><span style="background:#1e1e1e;color:#dcdcdc"> beamCollection </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> args</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">FrameReference</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">AcquireBeamFrames</span><span style="background:#1e1e1e;color:#b4b4b4">();</span></li> <li class="le-pavsc-even">    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">if</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">beamCollection </span><span style="background:#1e1e1e;color:#b4b4b4">==</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">nullptr</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">return</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li class="le-pavsc-even">    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">if</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">beamCollection</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">Size </span><span style="background:#1e1e1e;color:#b4b4b4">&lt;</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">1</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">return</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li class="le-pavsc-even">&nbsp;</li> <li>    <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">for</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#569cd6">auto</span><span style="background:#1e1e1e;color:#dcdcdc"> subFrame </span><span style="background:#1e1e1e;color:#b4b4b4">:</span><span style="background:#1e1e1e;color:#dcdcdc"> beamCollection</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">GetAt</span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#b5cea8">0</span><span style="background:#1e1e1e;color:#b4b4b4">)-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">SubFrames</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li class="le-pavsc-even">    <span style="background:#1e1e1e;color:#dcdcdc">{</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">if</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">subFrame </span><span style="background:#1e1e1e;color:#b4b4b4">==</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">nullptr</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li class="le-pavsc-even">            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">continue</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li>&nbsp;</li> <li class="le-pavsc-even">        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#57a64a">// Continue until timeout exceeded</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">if</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">_lastDetected</span><span style="background:#1e1e1e;color:#b4b4b4">.</span><span style="background:#1e1e1e;color:#dcdcdc">Duration </span><span style="background:#1e1e1e;color:#b4b4b4">!=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">0.0</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">&amp;&amp;</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">subFrame</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">RelativeTime</span><span style="background:#1e1e1e;color:#b4b4b4">.</span><span style="background:#1e1e1e;color:#dcdcdc">Duration </span><span style="background:#1e1e1e;color:#b4b4b4">&lt;</span><span style="background:#1e1e1e;color:#dcdcdc"> _lastDetected</span><span style="background:#1e1e1e;color:#b4b4b4">.</span><span style="background:#1e1e1e;color:#dcdcdc">Duration </span><span style="background:#1e1e1e;color:#b4b4b4">+</span><span style="background:#1e1e1e;color:#dcdcdc"> TIMEOUT</span><span style="background:#1e1e1e;color:#b4b4b4">))</span></li> <li class="le-pavsc-even">            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">continue</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li>&nbsp;</li> <li class="le-pavsc-even">        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dcdcdc">subFrame</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">CopyFrameDataToArray</span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">_audioBuffer</span><span style="background:#1e1e1e;color:#b4b4b4">);</span></li> <li>&nbsp;</li> <li class="le-pavsc-even">        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">double</span><span style="background:#1e1e1e;color:#dcdcdc"> sum </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">0</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">int</span><span style="background:#1e1e1e;color:#dcdcdc"> stride </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">sizeof</span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#569cd6">float</span><span style="background:#1e1e1e;color:#b4b4b4">);</span></li> <li class="le-pavsc-even">        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">float</span><span style="background:#1e1e1e;color:#dcdcdc"> sq </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">0.0f</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">int</span><span style="background:#1e1e1e;color:#dcdcdc"> sampleCounter </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">0</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li class="le-pavsc-even">&nbsp;</li> <li>        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#57a64a">// Loop over audio data bytes...</span></li> <li class="le-pavsc-even">        <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">for</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#569cd6">int</span><span style="background:#1e1e1e;color:#dcdcdc"> i </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">0</span><span style="background:#1e1e1e;color:#b4b4b4">;</span><span style="background:#1e1e1e;color:#dcdcdc"> i </span><span style="background:#1e1e1e;color:#b4b4b4">&lt;</span><span style="background:#1e1e1e;color:#dcdcdc"> _audioBuffer</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">Length</span><span style="background:#1e1e1e;color:#b4b4b4">;</span><span style="background:#1e1e1e;color:#dcdcdc"> i </span><span style="background:#1e1e1e;color:#b4b4b4">+=</span><span style="background:#1e1e1e;color:#dcdcdc"> stride</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc">{</span></li> <li class="le-pavsc-even">            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dcdcdc">memcpy</span><span style="background:#1e1e1e;color:#b4b4b4">(&amp;</span><span style="background:#1e1e1e;color:#dcdcdc">sq</span><span style="background:#1e1e1e;color:#b4b4b4">,</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">&amp;</span><span style="background:#1e1e1e;color:#dcdcdc">_audioBuffer</span><span style="background:#1e1e1e;color:#b4b4b4">[</span><span style="background:#1e1e1e;color:#dcdcdc">i</span><span style="background:#1e1e1e;color:#b4b4b4">],</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#569cd6">sizeof</span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#569cd6">float</span><span style="background:#1e1e1e;color:#b4b4b4">));</span></li> <li>            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dcdcdc">sum </span><span style="background:#1e1e1e;color:#b4b4b4">+=</span><span style="background:#1e1e1e;color:#dcdcdc"> sq </span><span style="background:#1e1e1e;color:#b4b4b4">*</span><span style="background:#1e1e1e;color:#dcdcdc"> sq</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li class="le-pavsc-even">            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dcdcdc">sampleCounter</span><span style="background:#1e1e1e;color:#b4b4b4">++;</span></li> <li>            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">if</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">sampleCounter </span><span style="background:#1e1e1e;color:#b4b4b4">&lt;</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">40</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li class="le-pavsc-even">                <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">continue</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li>            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">float</span><span style="background:#1e1e1e;color:#dcdcdc"> energy </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#569cd6">float</span><span style="background:#1e1e1e;color:#b4b4b4">)</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#b5cea8">10.0</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">*</span><span style="background:#1e1e1e;color:#dcdcdc"> log10</span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">sum </span><span style="background:#1e1e1e;color:#b4b4b4">/</span><span style="background:#1e1e1e;color:#dcdcdc"> sampleCounter</span><span style="background:#1e1e1e;color:#b4b4b4">));</span></li> <li class="le-pavsc-even">            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dcdcdc">sampleCounter </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> sum </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b5cea8">0</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li>&nbsp;</li> <li class="le-pavsc-even">            <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">if</span><span style="background:#1e1e1e;color:#dcdcdc"> </span><span style="background:#1e1e1e;color:#b4b4b4">(</span><span style="background:#1e1e1e;color:#dcdcdc">energy </span><span style="background:#1e1e1e;color:#b4b4b4">&gt;</span><span style="background:#1e1e1e;color:#dcdcdc"> ENERGY_THRESHOLD</span><span style="background:#1e1e1e;color:#b4b4b4">)</span></li> <li>            <span style="background:#1e1e1e;color:#dcdcdc">{</span></li> <li class="le-pavsc-even">                <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#dcdcdc">_lastDetected </span><span style="background:#1e1e1e;color:#b4b4b4">=</span><span style="background:#1e1e1e;color:#dcdcdc"> subFrame</span><span style="background:#1e1e1e;color:#b4b4b4">-&gt;</span><span style="background:#1e1e1e;color:#dcdcdc">RelativeTime</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li>&nbsp;</li> <li class="le-pavsc-even">                <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#57a64a">// Sound detected!! Do Stuff....</span></li> <li>                <span style="background:#1e1e1e;color:#dcdcdc"></span><span style="background:#1e1e1e;color:#569cd6">break</span><span style="background:#1e1e1e;color:#b4b4b4">;</span></li> <li class="le-pavsc-even">            <span style="background:#1e1e1e;color:#dcdcdc">}</span></li> <li>        <span style="background:#1e1e1e;color:#dcdcdc">}</span></li> <li class="le-pavsc-even">    <span style="background:#1e1e1e;color:#dcdcdc">}</span></li> <li><span style="background:#1e1e1e;color:#dcdcdc">}</span></li> </ol> </div> </div> </div> <p>The sample is here <a title="https://github.com/peted70/kinectv2-detectsound.git" href="https://github.com/peted70/kinectv2-detectsound.git">https://github.com/peted70/kinectv2-detectsound.git</a></p>