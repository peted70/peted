---
layout: post
title: glTF &amp; DirectX final part
date: 2018-06-04 12:02
author: peted70
comments: true
categories: [3D, 3D, 3dmodels, c#, c++, code, DirectX, glb, gltf, glTF, UWP, uwp, viewer]
---
<blockquote>
<h4><span style="color: #ffffff;"><a href="http://peted.azurewebsites.net/wp-content/uploads/2018/06/barramundi.png"><img style="display: inline; background-image: none;" title="barramundi" src="http://peted.azurewebsites.net/wp-content/uploads/2018/06/barramundi_thumb.png" alt="barramundi" width="625" height="402" border="0" /></a></span></h4>
<h4><span style="color: #ffffff;">Following the initial intro post see</span> <a href="http://peted.azurewebsites.net/gltf-directx/">http://peted.azurewebsites.net/gltf-directx/</a> <span style="color: #ffffff;">I have written a few follow-up posts to highlight some of my learnings from writing this sample. This one is the final post and contains topics I thought were interesting but didn’t warrant a separate post. The full source code for the sample is here  </span><a href="https://github.com/Microsoft/glTF-DXViewer">https://github.com/Microsoft/glTF-DXViewer</a></h4>
</blockquote>
<h4>C++ features</h4>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/06/cpp_logo.png"><img style="display: inline; background-image: none;" title="cpp_logo" src="http://peted.azurewebsites.net/wp-content/uploads/2018/06/cpp_logo_thumb.png" alt="cpp_logo" width="217" height="244" border="0" /></a>

I spent many years programming with C++ in my earlier career but over the last few years for me it has taken a back seat to C# and javascript. C++ as a language has moved on considerably since I started using it and having spent some time back with modern C++ I can say that it covers a huge surface from very low-level to high-level constructs so you can work at that higher level only dropping down to the nuts and bolts when really needed. I really enjoy using modern C++ features such as range-for, auto and the built-in smart pointers and whilst not part of standard C++ have used this project to explore other features such as coroutines. I do still seem to spend a lot of time searching the internet for which headers/namespaces to find particular classes in though.
<h5>Co-routines</h5>
Having been using C# a fair bit and async/await becomes second nature for orchestrating any code of an asynchronous nature. Coroutines whilst not in the C++ standard as yet they are available in the Visual C++ compiler (and also, I believe in the Clang compiler) are hopefully headed for C++ 20. Here's the current (as of the time of writing) ISO C++ draft proposal for coroutines <a href="http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/n4649.pdf">http://www.open-std.org/jtc1/sc22/wg21/docs/papers/2017/n4649.pdf</a> and without going into the weeds of how this is implemented by the compiler I am going to briefly cover my usage as relates to this code sample.

<script src="https://gist.github.com/peted70/0c943c8c59749c65df9ecd778f5247cf.js"></script>So, this code will call the two methods in there whose names are postfixed with Async – which is a naming convention to identify async calls – and return from the outer method immediately scheduling the code after the co_await into a callback. This allows asynchronous code to be written in a sequential manner similar to non-asynchronous code. This results in much cleaner-looking code which, as a result is clearer and easier to understand.
<blockquote>In Visual Studio 2017 15.7.1 I noticed an Access Violation when running the code above. This is due to the compiler optimizing variables required by the coroutine body. The optimization can easily be disabled using the compiler switch /d2CoroOptsWorkaround.</blockquote>
<h5>Variadic Templates</h5>
Another C++ feature I hadn't used before is variadic templates, introduced into the C++ 11 Standard. These allow template definitions to be parameterized with any number of arbitrary-typed arguments. To illustrate, I wanted to create a very simple notification system where I could associate an event with any method/function call. With this I could implement messages to communicate between ViewModels so when some data changes observers could be notified by hooking up their own functions.<script src="https://gist.github.com/peted70/9156ae3f9f77114ae678e6d257d682f9.js"></script> Then usage of this type looks like this:<script src="https://gist.github.com/peted70/88417293fb30eeb78d5bcc6e7f4200e6.js"></script>An opaque token is generated when registering an observer which can subsequently be used to deregister.
<h4>Shaders</h4>
The PBR shaders are ported from the Khronos PBR shader sample here <a href="https://github.com/KhronosGroup/glTF-WebGL-PBR">https://github.com/KhronosGroup/glTF-WebGL-PBR</a> from GLSL -&gt; HLSL. I ported these 'by hand' as it's fairly straight-forward and I wanted to go through the process. There is a handy porting guide here <a href="https://docs.microsoft.com/en-us/windows/uwp/gaming/glsl-to-hlsl-reference">https://docs.microsoft.com/en-us/windows/uwp/gaming/glsl-to-hlsl-reference</a>. You might also get some leverage using ANGLE <a href="https://github.com/Microsoft/angle">https://github.com/Microsoft/angle</a> which with a bit of coercion you can access its translation of the shaders. I tried this but I found the result difficult to read so continued without using this method.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/06/image.png"><img style="display: inline; background-image: none;" title="image" src="http://peted.azurewebsites.net/wp-content/uploads/2018/06/image_thumb.png" alt="image" width="404" height="455" border="0" /></a>

The shaders in the sample are compiled dynamically using the D3DCompileFromFile function and depending on the nature of the mesh that the material containing the shader is associated with a number of different compiler defines will get used. For example, if the mesh is associated with a material which in turn is associated with a metallic/roughness texture (as opposed to single values) then we can define the symbol HAS_METALROUGHNESSMAP and selectively compile shader code based on it. So the shader file itself might include code that looks like this:

<script src="https://gist.github.com/peted70/56e380c07b42e127a01cbb1280fb4bb4.js"></script>So the texture lookups will only happen if there is a texture to lookup into!

Now, of course if we have already compiled the shader with a particular filename and the same set of compiler defines then we don't want to compile again so there is a ShaderCache class to handle looking up the shaders based on a hash of the file name and defines used. Here's the function used to load shaders for a mesh:<script src="https://gist.github.com/peted70/1c02c88fbba39c4d1d5b1a0159d024de.js"></script>
<h4></h4>
<h4>Graphics Diagnostics</h4>
When programming against a low-level graphics API like OpenGL or DirectX it can sometimes be tricky to diagnose why things don't work so hopefully if you are looking to incorporate glTF into your existing DirectX renderer this sample will be helpful. I extensively used the Visual Studio Graphics debugging tools which are not only brilliant for optimisation and performance profiling but when you have written some graphics code and are just faced with a blank screen they give you a detailed window onto the graphics pipeline.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/06/clip_image001.png"><img style="display: inline; background-image: none;" title="clip_image001" src="http://peted.azurewebsites.net/wp-content/uploads/2018/06/clip_image001_thumb.png" alt="clip_image001" width="670" height="417" border="0" /></a>

Circled on the left is a call stack of events on the GPU and selecting one of these, in the screenshot I have selected the draw call for rendering the mesh, you can view the pipeline stages (circled at the bottom). This is great for diagnosing mistakes in input layout and the vertex shader. Also circled is a play button next to the pixel shader which enables shader debugging including placing breakpoints inside your shader code.
<h4>Streams</h4>
There are some interesting challenges when it comes to getting a stream from a UWP file picker to a native code environment. We can get an absolute file path from the picked file but we get an 'Access Denied' when using native file handling APIs. So, in order to pass a stream across from WinRT to native C++ it is necessary to make a transition from something like an <a href="https://docs.microsoft.com/en-us/uwp/api/windows.storage.streams.irandomaccessstream" target="_blank" rel="noopener">IRandomAccessStream</a> interface provided by a <a href="https://docs.microsoft.com/en-us/uwp/api/windows.storage.storagefile" target="_blank" rel="noopener">StorageFile</a> to an istream which is an equivalent way of handling stream access in a native C++ environment. It is possible to achieve this by accessing the IStorageHandleAccess interface which is implemented by the StorageFile type (<a href="https://msdn.microsoft.com/en-us/library/windows/desktop/mt765063(v=vs.85).aspx">https://msdn.microsoft.com/en-us/library/windows/desktop/mt765063(v=vs.85).aspx</a>). You can coerce this into a native C++ istream using code similar to the following:

<script src="https://gist.github.com/peted70/b18e5c120dfbe3a7d24ffa9d133d6714.js"></script>From there I can pass the istream to the glTF deserializer. This avoids the need to copy the file or implement a C++ streambuf in terms of IRandomAccessStream. Another interesting issue is that when loading a glTF file which references textures, vertices, etc. in separate files it is not immediately obvious how we will be granted permission to access those loose files given the UWP sandbox. The glTF serializer/deserializer abstracts stream handling behind an IStreamReader interface so when a loose file is requested we are passed a uri string to our implementation of IStreamReader and we can pass back an istream. Now, the relative path we can reconstruct from the uri string we can pass to StorageFile::GetFileFromPathAsync to get a StorageFile and then we can again use code similar to the snippet above to convert that to an istream. But what if we don't have permission to open the file at that path? Well, there is a relatively recent addition to restricted capabilities called broadFileSystemAccess (see <a href="https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions#accessing-additional-locations">https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions#accessing-additional-locations</a>). You can add this to your app manifest and then the user can decide whether or not your app can access the broader file system. Here are the user settings in Settings &gt; File System

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/06/clip_image002.png"><img style="display: inline; background-image: none;" title="clip_image002" src="http://peted.azurewebsites.net/wp-content/uploads/2018/06/clip_image002_thumb.png" alt="clip_image002" width="663" height="504" border="0" /></a>
<h4>Additions</h4>
I am intending to extend the sample by updating the official repo with:
<ul>
 	<li>More glTF features</li>
 	<li>Supporting loading more of the glTF sample models</li>
</ul>
I will also fork the official sample to work on some features for my own research:
<ul>
 	<li>VR Viewer</li>
 	<li>Render to HoloLens</li>
 	<li>Mesh decimation</li>
 	<li>Port to DX12</li>
 	<li>Edit and Save files</li>
</ul>
If there is an addition you'd like to add please submit a pull request to the repo <a href="https://github.com/Microsoft/glTF-DXViewer" target="_blank" rel="noopener">https://github.com/Microsoft/glTF-DXViewer</a> and/or ask for it in the Issues section on Github.
