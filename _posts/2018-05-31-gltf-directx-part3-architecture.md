---
layout: post
title: glTF &amp; DirectX part3 - architecture
date: 2018-05-31 14:12
author: peted70
comments: true
categories: [3D, 3D, 3dmodels, c++, DirectX, directx, gltf, glTF, UWP, uwp]
---
<blockquote>
<h4><a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/corset.png"><img style="display: inline; background-image: none;" title="corset" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/corset_thumb.png" alt="corset" width="625" height="366" border="0" /></a></h4>
<h4><span style="color: #ffffff;">Following the initial intro post see </span><a href="http://peted.azurewebsites.net/gltf-directx/" target="_blank" rel="noopener">http://peted.azurewebsites.net/gltf-directx/</a> <span style="color: #ffffff;">I’m going to write a few follow up posts to highlight some of my learnings from writing this sample. This one is on the design of the code sample. All of the code can be found here </span><a title="https://github.com/Microsoft/glTF-DXViewer" href="https://github.com/Microsoft/glTF-DXViewer">https://github.com/Microsoft/glTF-DXViewer</a></h4>
</blockquote>
<h4>Architecture</h4>
The foundations of the app are a C++, UWP app which has a <a href="https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.swapchainpanel" target="_blank" rel="noopener">SwapchainPanel</a> within which it can host the DirectX content loaded from a glTF/glB file
<blockquote>A glB file has the same data as a glTF file but it is combined into one single self-contained binary. To parse these you can read the JSON document and then reference the buffers, textures, etc. from other locations in the file.</blockquote>
If you choose the appropriate workload when installing Visual Studio 2017 you will have a project template available which

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image001-2.png"><img style="display: inline; background-image: none;" title="clip_image001" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image001_thumb-2.png" alt="clip_image001" width="636" height="383" border="0" /></a>

Has a 'DirectX 11 and XAML App (Universal Windows)' which is how the sample code started life. The template has quite a lot of boilerplate code, most of which is still present in the sample, but it has a XAML layer for describing 2D UI elements and a SwapChainPanel which hosts the DirectX content. The XAML part of the app employs an MVVM-style which binds ViewModel data to the UI declaratively using x:Bind syntax in the XAML (see below for more MVVM).

The SwapChainPanel hosts data to be rendered using DirectX and that data is mostly tied up within the SceneGaph. The SceneGraph itself is a basic composite structure where its contents are represented by a GraphNode base class. A GraphNode can have children and so a graph can be formed by linking parents with children and also a GraphNode can be subclassed to provide a visual representation. This mirrors a scene hierarchy specified in the <a href="https://github.com/KhronosGroup/glTF/tree/master/specification/2.0" target="_blank" rel="noopener">glTF specification</a> and provides hierarchical transformation inheritance such as you would find in scene descriptions in most graphical software. The scene hierarchy is visualised by the <a href="https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/tree-view" target="_blank" rel="noopener">TreeView</a> in the right-hand panel which shows the parent and child nodes with different icons and the relationships between them. The ViewModels communicate with the scene graph using the Observer pattern.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image002-2.png"><img style="display: inline; background-image: none;" title="clip_image002" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image002_thumb-2.png" alt="clip_image002" width="661" height="412" border="0" /></a>

The data for the app is represented as a glTF/glB file and it is loaded into memory as a scene graph. The base class for this is a GraphNode, with GraphContainerNode adding transforms and children such that transforms from a parent are inherited by the children. The main subclass is MeshNode which deals with the buffers for rendering a mesh; e,g, index and vertex buffers, keeping a reference to a material, which in turn references a shader and shader inputs which are most-commonly textures.
<h4></h4>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image003-1.png"><img style="display: inline; background-image: none;" title="clip_image003" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image003_thumb-1.png" alt="clip_image003" width="660" height="392" border="0" /></a>
<h5>MVVM</h5>
I used the Model-View-ViewModel pattern as it helps to keep the UI separate from any logic. The sample has a ViewModel layer which gets databound to the visual elements described in XAML markup and the UI updates are handled by the app runtime. Databinding is all done with x:Bind so the binding code is all generated at Runtime. The ViewModels derive from ViewModelBase which has code to help with change notifications to the UI layer by implementing INotifyPropertyChanged.
<h4>Design</h4>
I have utilised aspects of Fluent design including Acrylic material and reveal highlights.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image004-1.png"><img style="display: inline; background-image: none;" title="clip_image004" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image004_thumb-1.png" alt="clip_image004" width="670" height="381" border="0" /></a>

Notice how the control panel above has a nice translucent effect which allows my desktop background to visible beneath. Also, as I move my mouse over the controls they appear as if lit from the position of the mouse cursor. More on fluent design here <a href="https://docs.microsoft.com/en-us/windows/uwp/design/fluent-design-system/">https://docs.microsoft.com/en-us/windows/uwp/design/fluent-design-system/</a> . Using Acrylic was as simple as swapping the Brush used by the required XAML element (see <a href="https://docs.microsoft.com/en-us/windows/uwp/design/style/acrylic">https://docs.microsoft.com/en-us/windows/uwp/design/style/acrylic</a>). For the reveal highlight styles can be used, i.e. I used ButtonRevealStyle here:
<blockquote>&lt;Button Grid.ColumnSpan="2" x:Name="colorPickerButton" Content="Pick a color" Style="{StaticResource ButtonRevealStyle}" /&gt;</blockquote>
<h4>Dependencies</h4>
Initially used vcpkg and Nuget to help manage the dependencies for this project. Vcpkg provides a way to manage versioning and configuring Visual Studio for using Open Source C and C++ libraries. I initially started using boost::signals2 but since this is a code sample I think it helps to reduce dependencies and I replaced it with Subject a custom template class supporting the bare minimum of a publish/subscribe mechanism. Although I am no longer relying on vcpkg it has a large portfolio of projects supported and in other projects I found it supported pretty much everything I needed. Further details are here <a href="https://github.com/Microsoft/vcpkg">https://github.com/Microsoft/vcpkg</a> I also used Nuget to pull in Microsoft.glTF.CPP which is published by Microsoft and handles serialising and deserialising glTF files. This screenshot shows Nuget integration with Visual Studio so I can search the Nuget package source from within the IDE and installing takes care of any admin the package needs to integrate with your project as well as its dependencies and also additions like signing license agreements if needed.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image005.png"><img style="display: inline; background-image: none;" title="clip_image005" src="http://peted.azurewebsites.net/wp-content/uploads/2018/05/clip_image005_thumb.png" alt="clip_image005" width="670" height="378" border="0" /></a>

Microsoft have also published an open source glTF toolkit <a href="https://github.com/Microsoft/glTF-Toolkit">https://github.com/Microsoft/glTF-Toolkit</a> to help creation, optimisation and conversion of glTF assets for Windows Mixed Reality.
