---
layout: single
title: glTF &amp; DirectX
date: 2018-05-25 00:00
author_profile: true
comments: true
categories: [3D, c#, coroutines, DX11, fluent, glb, gltf, glTF, treeview, uwp, UWP]
excerpt: "As we begin to witness the ascent of mixed reality into the next generation primary human-computer interaction environment it's not entirely unreasonable to wonder how the existing infrastructure for 3D graphics..."
header:
    teaserlogo: 'assets/images/2018/05/screenshot1.png'
    teaser: 'assets/images/2018/05/screenshot1.png'
---
<h4>3D File Formats</h4>
<a href="{{ site.baseurl }}/assets/images/2018/05/screenshot1.png"><img style="display: inline; background-image: none;" title="screenshot1" src="{{ site.baseurl }}/assets/images/2018/05/screenshot1_thumb.png" alt="screenshot1" width="670" height="440" border="0" /></a>

As we begin to witness the ascent of mixed reality into the next generation primary human-computer interaction environment it's not entirely unreasonable to wonder how the existing infrastructure for 3D graphics would bear the load of ubiquitous usage. I'm thinking about file interchange and runtime formats which have a tax associated with them in that in order to develop or use any kind of 3D software pipeline there is inherently the issue of how to use different formats, how to convert between them and how to do all of this whilst retaining the features you need for the job in hand. These are implementation details that we could do without and need to be enveloped into the infrastructure if we are going to move up an abstraction layer. Over the last 30 years or so any 3D project will have been concerned with low-level details about how to interchange 3D scenes, moving assets from one tool to another with the plethora of proprietary formats and converters, etc, etc. Also, each industry vertical has resulted in the expansion of specific creation tools and formats. If there was one accepted, open and standard format for this it would free up the time to think about more productive things.

<a href="{{ site.baseurl }}/assets/images/2018/05/clip_image001.png"><img style="display: inline; background-image: none;" title="clip_image001" src="{{ site.baseurl }}/assets/images/2018/05/clip_image001_thumb.png" alt="clip_image001" width="669" height="726" border="0" /></a>

At some point in the near future a content creator will be able to create using an immersive display technology, so the content could be there with them in their work space in 3D, and use natural inputs collected by sensors in the real world. They could interact with the digital content in a natural way, like a sculptor with a block of clay, or some yet-to-be-discovered new way enabled by the technology or a combination of both. Either way, it is vital that they are not hampered by details such as how the data is stored. So, enter into the mix glTF, billed as "the jpeg of 3D" serves to fulfil this role as a complete 3D scene representation. We are seeing momentum in the industry for glTF; we can load them into Office products, <a href="https://www.simplygon.com/" target="_blank" rel="noopener">Simplygon</a>, <a href="https://www.foundry.com/products/modo" target="_blank" rel="noopener">Modo</a> <a href="https://and">and</a> <a href="https://www.blender.org/" target="_blank" rel="noopener">Blender</a> <a href="https://support">support</a> is available just to name a few (for many more see <a href="https://www.khronos.org/gltf/">https://www.khronos.org/gltf/</a>)

<a href="{{ site.baseurl }}/assets/images/2018/05/clip_image002.png"><img style="display: inline; background-image: none;" title="clip_image002" src="{{ site.baseurl }}/assets/images/2018/05/clip_image002_thumb.png" alt="clip_image002" width="670" height="448" border="0" /></a>

Now, glTF stands for graphics library transmission format and the first two letters seem to suggest an affinity to OpenGL and whilst true that its roots lie with OpenGL there is nothing that ties the format to any particular graphics library. Whilst there are plenty of open source samples of glTF loaders/viewers for WebGL; <a href="https://www.babylonjs.com/" target="_blank" rel="noopener">BabylonJS</a>, <a href="https://threejs.org/" target="_blank" rel="noopener">three.js</a>, etc. there doesn't seem to be too many for DirectX.
<h5>DirectX Viewer</h5>
The sample viewer, written in C++ uses DirectX11 to render a glTF file. The sample application itself started as a way for me to understand the file format better and originally was just a glTF file parser but I gradually added features and will continue to do so. It is fundamentally a port from the Khronos sample Physically-Based Rendering in glTF 2.0 using WebGL- <a href="https://github.com/KhronosGroup/glTF-WebGL-PBR">https://github.com/KhronosGroup/glTF-WebGL-PBR</a> which details the physically-based rendering approach employed by glTF. Also, you can find the glTF 2.0 specification here <a href="https://github.com/KhronosGroup/glTF/tree/master/specification/2.0">https://github.com/KhronosGroup/glTF/tree/master/specification/2.0</a> for reference and comprehensive documentation.

The code for this sample is here <a title="https://github.com/Microsoft/glTF-DXViewer" href="https://github.com/Microsoft/glTF-DXViewer">https://github.com/Microsoft/glTF-DXViewer</a> and I will go over some of the more interesting implementation details in subsequent posts.
