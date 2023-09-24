---
layout: post
title: HoloLens&ndash;The Path to 60fps (Normal Maps)
date: 2017-04-10 09:20
author: peted70
comments: true
categories: [Blender, fps, HoloLens, HoloLens, low poly, normal map, normals, optimization, texture]
---
Since composing the last post: <a title="http://peted.azurewebsites.net/hololensthe-path-to-60fps/" href="http://peted.azurewebsites.net/hololensthe-path-to-60fps/" target="_blank" rel="noopener">HoloLens–The Path to 60fps</a> I have carried out some more work on HoloLens apps with an important requirement for as high quality rendering as possible but of course the goal is always to maintain 60 frames per second. This time around we were looking to render glass containers with associated labelling which needed to be clear and legible and the glass should look as realistic as possible in the available time.
<blockquote>I’m going to use <a href="https://www.blender.org/" target="_blank" rel="noopener">Blender</a> for demonstration which, if you haven’t come across it before is a free, open source 3D creation software package</blockquote>
<h3>Content Pipeline</h3>
As with a lot of project scenarios we don’t always have a greenfield where models and textures, etc. can be created from scratch; often the assets will already exist but not necessarily in a form we can instantly consume easily on a HoloLens. In this case there were pre-existing assets that were highly dense in terms of faces and vertices and also somewhat wasteful in terms of topology with geometry inside other geometry. These assets were used for high-quality renders of products for marketing materials so performance was not a consideration at all. To give a feel for the details, one glass container consisted of nearly one million polygons including embossed writing on the glass constructed of triangles.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/image.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="image" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/image_thumb.png" alt="image" width="733" height="694" border="0" /></a>
<blockquote>Check out the underside of the bottle – this is all polygons!</blockquote>
We also had a requirement to be able to read and examine the labels applied to the container with high fidelity.  Based on these requirements we knew that we needed to:
<blockquote>Decimate the polygon count of the mesh

Experiment with shaders to give a glass look

Ensure that the experience all ran with as high a frame rate as possible given the other requirements</blockquote>
At the outset we used the HoloLens <a href="https://developer.microsoft.com/en-us/windows/mixed-reality/using_the_windows_device_portal" target="_blank" rel="noopener">device portal</a> to record a baseline for the performance – this turned out to run at below 20fps for one container – we hoped to be able to create a series of containers in the final experience so clearly this needed improvement.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/devportal.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="devportal" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/devportal_thumb.png" alt="devportal" width="723" height="561" border="0" /></a>

&nbsp;
<h3>Shaders for Glass</h3>
We replaced all of the existing shaders which came across from the initial FBX files as standard Unity shaders with optimized shaders from the <a href="https://github.com/Microsoft/HoloToolkit-Unity" target="_blank" rel="noopener">HoloToolkit for Unity</a>. We used the <strong>BlinnPhong Configurable Transparent</strong> shader so we could have specular highlights and could also experiment with the opacity of the glass.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/shadersettings1.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="shadersettings1" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/shadersettings1_thumb.png" alt="shadersettings1" width="739" height="442" border="0" /></a>

The shader gives control over the highlights and also the alpha blending modes:
<h3><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/shadersettings2.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="shadersettings2" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/shadersettings2_thumb.png" alt="shadersettings2" width="741" height="250" border="0" /></a></h3>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/bottles.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="bottles" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/bottles_thumb.png" alt="bottles" width="736" height="410" border="0" /></a>
<h3>Normal Maps</h3>
&nbsp;

There was a huge amount of details in the original model which we wanted to capture for some added realism. This included embossed writing on the surface of the glass and also very fine serrations on the cap of the containers. Enter normal maps which define how the lighting interacts with the surface based not on the geometry of the mesh but the directions of the normals specified by the normal map. Since we had a very dense model with full detail it is possible to generate the normal map from that model and apply it to a model which retains the overall shape of the original but is specified with a vastly reduced triangle count. Here are the steps we carried out using Blender to achieve this on the cap but can be applied to the model as a whole:
<blockquote>Original bottle – this was not the 1M polygon model but one that had been derived from it but still retaining most of the details (not the embossing) which has 30k + polygons and comes in a few different pieces (the labels and the cap are separate meshes)</blockquote>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/clip_image0017.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="clip_image001[7]" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/clip_image0017_thumb.png" alt="clip_image001[7]" width="748" height="481" border="0" /></a>

Let’s see how we can apply a normal map to a cylinder in Blender to create a low polygon model of the cap shown in the screenshot above. The cap itself has approximately 28k triangles.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/cap.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="cap" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/cap_thumb.png" alt="cap" width="737" height="453" border="0" /></a>

The mesh we are starting with here is not particularly ‘clean’ so I first ran a tool in Blender to remove duplicate vertices called <strong>Remove Doubles.</strong>

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/removeduplicates.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="removeduplicates" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/removeduplicates_thumb.png" alt="removeduplicates" width="730" height="403" border="0" /></a>

This took the number of faces down to about 11k

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/afterremoveduplicates.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="afterremoveduplicates" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/afterremoveduplicates_thumb.png" alt="afterremoveduplicates" width="729" height="354" border="0" /></a>

Next I carried out the following steps:
<blockquote>- Created a cylinder which was roughly the same size as the cap

- With the cylinder selected - create a <strong>smart uv unwrap</strong> - shortcut key u will bring up the menu item to run this

- Then create a new image in the UV editor and save it</blockquote>
Select the original high-res cap model and then shift select the cylinder
<blockquote>The order of this selection is important as it defines the source and target for rendering the normal map</blockquote>
and then carry out the <strong>bake </strong>operation with these settings:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/createnormalmapsettings.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="createnormalmapsettings" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/createnormalmapsettings_thumb.png" alt="createnormalmapsettings" width="722" height="770" border="0" /></a>

This should give the result shown below – note the normal map with shading which reflects the details in the high definition cap.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/normalmap.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="normalmap" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/normalmap_thumb.png" alt="normalmap" width="739" height="497" border="0" /></a>

First, save the image and then:
<blockquote>- Create a new material on the cylinder

- Add the previously saved normal map to the material as a texture

- Set preview render mode to material (so we can see the effect of our changes).

- The texture will be there on the surface of the cap as an RGB texture wrapped onto cylinder

- Set the texture to be a normal map</blockquote>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/textureotnormalmap.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="textureotnormalmap" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/textureotnormalmap_thumb.png" alt="textureotnormalmap" width="723" height="990" border="0" /></a>

Change the influence setting for the texture from diffuse rgb to normal

<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/04/caps.png"><img style="background-image: none; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-width: 0px;" title="caps" src="http://peted.azurewebsites.net/wp-content/uploads/2017/04/caps_thumb.png" alt="caps" width="732" height="499" border="0" /></a>

This shows the caps side by side and our new cap is down to approximately 100 triangles and also responds as expected to dynamic lighting changes.

So you can see this is a useful technique to use when detailed models are required but the cost of processing a large number of vertices is prohibitive such as on HoloLens. We can apply this technique to the bottle as a whole and retain the embossing on the surface of the glass.
