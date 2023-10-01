---
layout: single
title: glTF + DirectX part 2
date: 2018-05-25 23:50
author: peted70
comments: true
categories: [3D, 3D, 3dmodels, c#, c++, directx, DirectX, glb, glTF, gltf]
---
<blockquote>
<h4>Following the initial intro post see <a title="http://peted.azurewebsites.net/gltf-directx/" href="http://peted.azurewebsites.net/gltf-directx/">http://peted.azurewebsites.net/gltf-directx/</a> I’m going to write a few follow up posts to highlight some of my learnings from writing this sample</h4>
</blockquote>
<h4>glTF</h4>
glTF has been designed with some fundamental principles in mind;
<h5>Physically-based rendering</h5>
Often it has not been a priority to provide rendering that looks the same across different platforms but the more the rendering is based on physically correct principles then visual consistency naturally follows. So instead of fudge factors and magic numbers to make a material look right in a particular scene glTF embraces physically based rendering to help with consistency. For more info on PBR see <a href="http://blog.selfshadow.com/publications/s2012-shading-course/burley/s2012_pbs_disney_brdf_notes_v3.pdf">http://blog.selfshadow.com/publications/s2012-shading-course/burley/s2012_pbs_disney_brdf_notes_v3.pdf</a> and bear in mind that PBR encompasses a range of different techniques and implementation choices rather than just one or two.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image001-1.png"><img style="display: inline; background-image: none;" title="clip_image001" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image001_thumb-1.png" alt="clip_image001" width="652" height="366" border="0" /></a>
<h5>Buffer Management</h5>
glTF is designed to not require expensive and difficult parsing of the data instead choosing a representation close to how the graphics APIs will require the data. As an example of that consider vertex and index buffers; these exist in glTF as binary data in a file that can be referenced directly as a block of memory and handed off to the GPU using, in DirectX terms by passing the memory to <a href="https://msdn.microsoft.com/en-us/library/windows/desktop/ff476501(v=vs.85).aspx" target="_blank" rel="noopener">CreateBuffer</a>.
<h5>glTF Support</h5>
Just the basic features of glTF are supported for now so the sample does not currently support animations, vertex skinning or any glTF extensions. There is a great set of sample models that you can use for development here <a href="https://github.com/KhronosGroup/glTF-Sample-Models">https://github.com/KhronosGroup/glTF-Sample-Models</a>. Here's a few of them rendered by the sample:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image002-1.png"><img style="display: inline; background-image: none;" title="clip_image002" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image002_thumb-1.png" alt="clip_image002" width="670" height="434" border="0" /></a>
<h4>Loader</h4>
Initially, I started the project with my own file parser but I replaced that with the <a href="https://www.nuget.org/packages/Microsoft.glTF.CPP" target="_blank" rel="noopener">Microsoft.glTF.CPP</a> library obtained via Nuget which includes deserialisation of a glTF and also serialisation which I am not currently using. I have an example of the library's usage here <a href="http://peted.azurewebsites.net/glb-reading-and-writing/">http://peted.azurewebsites.net/glb-reading-and-writing/</a>
<h4>Environment Map</h4>
In order to carry out the image-based lighting in the sample the code loads in a 'Texture Cube' which can be referenced in a pixel shader to lookup values from the environment and blend with the current colour on the surface being rendered. The pixel shader can sample the texture cube at a reflection vector on a point on the surface and factor that into the lighting calculation. There is one set of images used for the texture cube in the sample code and you can see its effect on the Boombox model:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image003.png"><img style="display: inline; background-image: none;" title="clip_image003" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image003_thumb.png" alt="clip_image003" width="661" height="385" border="0" /></a>

This technique can be used for lighting a model consistently with it’s environment and/or providing realistic reflections.
<h4>Selective PBR rendering</h4>
In the same way that the Khronos sample allows you to switch parts of the PBR shader on/off you can do the same here and this can give an insight into the different parts of the PBR shader which at first glance can seem overwhelming. Here are some of the different parts of the pixel shader shown on the Damaged Helmet sample model:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image004.png"><img style="display: inline; background-image: none;" title="clip_image004" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image004_thumb.png" alt="clip_image004" width="670" height="182" border="0" /></a>
<h4>sRGB</h4>
I'm not going to delve into the details of sRGB but here's some info about how it relates to DirectX <a href="https://msdn.microsoft.com/en-us/library/windows/desktop/hh972627(v=vs.85).aspx">https://msdn.microsoft.com/en-us/library/windows/desktop/hh972627(v=vs.85).aspx</a> and for a more general understanding <a href="https://en.wikipedia.org/wiki/SRGB">https://en.wikipedia.org/wiki/SRGB</a>. Suffice to say that it is important to understand what colour space you are carrying out pixel shader operations in. Since all of the sample code is here <a title="https://github.com/Microsoft/glTF-DXViewer" href="https://github.com/Microsoft/glTF-DXViewer">https://github.com/Microsoft/glTF-DXViewer</a> you can check out how the colour space is dealt with both within the shader calculations and when setting up the rendering buffers in DirectX.
<blockquote>For the next post we’ll look into the  software architecture as even this small sample is quite instructive in how you might arrange the code for a 3D application.</blockquote>
