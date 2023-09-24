---
layout: post
title: Shader Stereo instancing HoloLens
date: 2019-03-14 10:34
author: peted70
comments: true
categories: [HoloLens, HoloLens, performance, shaders, stereoscopic, unity, unity3d]
---
<a href="http://peted.azurewebsites.net/wp-content/uploads/2019/03/single-eye.jpg"><img style="display: inline; background-image: none;" title="single-eye" src="http://peted.azurewebsites.net/wp-content/uploads/2019/03/single-eye_thumb.jpg" alt="single-eye" width="670" height="504" border="0" /></a>

Whilst recently experimenting with the <a href="https://github.com/Microsoft/MixedRealityToolkit-Unity" target="_blank" rel="noopener">Mixed Reality Toolkit for Unity vNext</a> for HoloLens app development
<blockquote>The vNext version of the toolkit will be a requirement for HoloLens2 development so this would be a good time to start investigating if you haven't done that yet. See https://github.com/Microsoft/MixedRealityToolkit-Unity/blob/mrtk_release/Documentation/GettingStartedWithTheMRTK.md</blockquote>
I noticed that the shaders I was using to render signed distance field text by the <strong>TextMeshPro</strong> component was rendering in one eye only. In fact I have been through this a number of times so this time I went through the simple steps required to fix it and made a note of it, so...

I've mentioned this before (<a href="http://peted.azurewebsites.net/hololensthe-path-to-60fps/">http://peted.azurewebsites.net/hololensthe-path-to-60fps/</a>) but using single-pass rendering mode, in my experience gives a huge performance benefit for HoloLens apps without much pain. I will use TextMeshPro as an example but this could be applied to any shader.
<blockquote>If you're not familiar with TextMeshPro it is an efficient way to render text using signed distance fields which keep your text resolution independent and always looking crisply rendered. More info [here](https://blogs.unity3d.com/2018/10/16/making-the-most-of-textmesh-pro-in-unity-2018/)</blockquote>
Now rendering stereo views for an immersive experience can be done in a number of different ways but it is very efficient to use as there is no need to send the data to the GPU twice (one for each eye). Also the geometry is rendered once for one eye followed immediately by the other; i.e. the rendering doesn't draw the whole scene to one eye followed by the whole scene to the other. This has some caching implications too which typically adds up to more efficient rendering and better performance. See Unity's documentation (<a href="https://docs.unity3d.com/Manual/SinglePassStereoRenderingHoloLens.html">https://docs.unity3d.com/Manual/SinglePassStereoRenderingHoloLens.html</a>)
<blockquote>I'd suggest turning this setting on for HoloLens applications built in Unity and if you are using DirectX the Visual Studio Holographic Template shows how to achieve similar using DrawIndexedInstanced.</blockquote>
In each environment, shader support is required and I converted the <strong>TextMeshPro</strong> shader so that it worked for both eyes.

These are the updated parts:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2019/03/code-snippet-1.png"><img style="display: inline; background-image: none;" title="code-snippet-1" src="http://peted.azurewebsites.net/wp-content/uploads/2019/03/code-snippet-1_thumb.png" alt="code-snippet-1" width="670" height="340" border="0" /></a>

<a href="http://peted.azurewebsites.net/wp-content/uploads/2019/03/code-snippet-2.png"><img style="display: inline; background-image: none;" title="code-snippet-2" src="http://peted.azurewebsites.net/wp-content/uploads/2019/03/code-snippet-2_thumb.png" alt="code-snippet-2" width="670" height="270" border="0" /></a>

<a href="http://peted.azurewebsites.net/wp-content/uploads/2019/03/code-snippet-3.png"><img style="display: inline; background-image: none;" title="code-snippet-3" src="http://peted.azurewebsites.net/wp-content/uploads/2019/03/code-snippet-3_thumb.png" alt="code-snippet-3" width="670" height="170" border="0" /></a>

And if you want to dig deeper you can download the Unity shader source and you will find the helper macros for supporting stereo instancing in <strong>UnityInstancing.gcinc</strong>

Unity information about what is required can be read here(<a href="https://docs.unity3d.com/Manual/GPUInstancing.html">https://docs.unity3d.com/Manual/GPUInstancing.html</a>)

Here is the TextMeshPro shader after I converted it to work[TMP_SDF-MobileInstanced.shader (<a href="https://gist.github.com/peted70/271bd916f3547c78ab7521ac2776755b">https://gist.github.com/peted70/271bd916f3547c78ab7521ac2776755b</a>)
