---
layout: post
title: WebVR + HoloLens
date: 2018-06-15 14:57
author: peted70
comments: true
categories: [3D, 3D, a-frame, a-frame, gltf, glTF, HoloLens, HoloLens, WebVR, webvr]
---
Microsoft Edge has support for WebVR as you can see here <a href="https://caniuse.com/#search=webvr">https://caniuse.com/#search=webvr</a>

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/06/caniuse.png"><img style="display: inline; background-image: none;" title="caniuse" src="http://peted.azurewebsites.net/wp-content/uploads/2018/06/caniuse_thumb.png" alt="caniuse" width="670" height="337" border="0" /></a>
<blockquote>The support across the main browsers looks like this at the time of writing but what is not captured here is that in order to use WebVR on a HoloLens device you need to switch on WebVR support in the about:flags.</blockquote>
<iframe src="https://www.youtube.com/embed/Vz9JqsV7HcM" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen">&lt;/iframe</iframe>

Since RS4 was made available to HoloLens devices (see https://docs.microsoft.com/en-us/windows/mixed-reality/hololens-rs4-preview) we can run WebVR apps on there.  <a href="http://media.tojicode.com/q3bsp/" target="_blank" rel="noopener">Here's</a> quake in WebVR and running on a HoloLens

<iframe src="https://www.youtube.com/embed/mjkEjSM_sAY" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe>
<h3>Hello World</h3>
If you are interested in the WebVR spec then take a look <a href="https://immersive-web.github.io/webvr/spec/1.1/" target="_blank" rel="noopener">here</a> for all of the details. To get a glTF model we can use Paint3D to access the Remix online model catalogue, import a model and then export as glb.

<iframe src="https://www.youtube.com/embed/cfVRHw198Io" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

I just want to get up and running as quickly as possible so I'm going to use <a href="https://aframe.io/" target="_blank" rel="noopener">a-frame</a> where I can very easily place a glTF model into my WebVR scene declaratively using HTML like this: <script src="https://gist.github.com/peted70/8576d609ef10d1e65b74c8a73c5a8a31.js"></script>

I have hosted that on the <a href="https://peted70.github.io/webvr-hololens/" target="_blank" rel="noopener">Github page</a> for this <a href="https://github.com/peted70/webvr-hololens" target="_blank" rel="noopener">Github repo</a>

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/06/desktop-screen.png"><img style="display: inline; background-image: none;" title="desktop-screen" src="http://peted.azurewebsites.net/wp-content/uploads/2018/06/desktop-screen_thumb.png" alt="desktop-screen" width="670" height="398" border="0" /></a>  So, put on your HoloLens, go to that page and click on the HMD icon in the bottom right and you will see a giant funky sun in front of you!
