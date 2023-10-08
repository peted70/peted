---
layout: single
title: GLB reading and writing
date: 2018-05-01 16:20
author_profile: true
comments: true
categories: [3D, 3D, 3dmodels, c++, glb, gltf]
excerpt:  "Just a quick post as I am looking at a useful Nuget package Microsoft.GLTF.cpp and wanted to note down my quick experiment with it. For an unrelated project I wrote a c++ parser for the GLB format (binary version of GLTF) and wanted to replace it with something better"
header:
    teaserlogo: 'assets/images/2018/05/gltf.png'
    teaser: 'assets/images/2018/05/gltf.png'
---
<a href="{{ site.baseurl }}/assets/images/2018/05/gltf.png"><img style="display: inline; background-image: none;" title="gltf" src="{{ site.baseurl }}/assets/images/2018/05/gltf_thumb.png" alt="gltf" width="664" height="243" border="0" /></a>

Just a quick post as I am looking at a useful Nuget package Microsoft.GLTF.cpp and wanted to note down my quick experiment with it. For an unrelated project I wrote a c++ parser for the GLB format (binary version of GLTF) and wanted to replace it with something better. If you are unfamiliar with the format I reference it here <a title="http://peted.azurewebsites.net/holograms-catalogueloading-models/" href="http://peted.azurewebsites.net/holograms-catalogueloading-models/">http://peted.azurewebsites.net/holograms-catalogueloading-models/</a> where I used a c# GLTF loader within Unity and also you can find the GLTF spec here <a title="https://github.com/KhronosGroup/glTF/tree/master/specification/" href="https://github.com/KhronosGroup/glTF/tree/master/specification/">https://github.com/KhronosGroup/glTF/tree/master/specification/</a>.
<blockquote>To cut a long story short if you are working in the 3D industry you will most-likely have winced when people from other sectors bring up their industries open standard file format. GLTF is the answer to that and I suspect we should all get behind it.</blockquote>
Anyway, back to the loader:

<a href="{{ site.baseurl }}/assets/images/2018/05/nuget.png"><img style="display: inline; background-image: none;" title="nuget" src="{{ site.baseurl }}/assets/images/2018/05/nuget_thumb.png" alt="nuget" width="670" height="307" border="0" /></a> I loaded the Nuget package via Visual Studio’s integration and looked in it’s README file to find a sample project there which shows how to save/load a glb file. I extended that a little and added the facility to print out the scene hierarchy with node names and vertex counts for each mesh. The result looks like this:

<a href="{{ site.baseurl }}/assets/images/2018/05/output.png"><img style="display: inline; background-image: none;" title="output" src="{{ site.baseurl }}/assets/images/2018/05/output_thumb.png" alt="output" width="667" height="584" border="0" /></a>

when loading in the sample GLTF Lantern file.
<blockquote>There are some reference models here <a title="https://github.com/KhronosGroup/glTF-Sample-Models" href="https://github.com/KhronosGroup/glTF-Sample-Models">https://github.com/KhronosGroup/glTF-Sample-Models</a> where you can find the Lantern and others.</blockquote>
The code is very simple and with the clutter removed looks a bit like this; first creating a GLTFDocument:

<script src="https://gist.github.com/peted70/3c6da290e0b21c9e2557b107804c201e.js"></script>Then, using the document to iterate and access the data as described by the spec above, mostly by looking up objects using their index into a list of objects, such as meshes, nodes and scenes, etc. The following shows a function that recurses through the scene graph  Note, how the meshes are looked up using the mesh ID.

The sample code is here <a title="https://github.com/peted70/glb-commandline-sample" href="https://github.com/peted70/glb-commandline-sample">https://github.com/peted70/glb-commandline-sample</a>

<script src="https://gist.github.com/peted70/c1f75eaef31e0542361af0685ae98c7f.js"></script>
