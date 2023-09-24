---
layout: post
title: Future Tech Now&ndash;Virtual Reality Show
date: 2018-04-10 08:43
author: peted70
comments: true
categories: [ants, HoloLens, HoloLens, mixedreality, unity, Unity, Windows Mixed Reality]
---
<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2018/04/ant-unity.png"><img width="670" height="351" title="ant-unity" style="display: inline; background-image: none;" alt="ant-unity" src="http://peted.azurewebsites.net/wp-content/uploads/2018/04/ant-unity_thumb.png" border="0"></a>I recently gave a developer talk at <a href="https://futuretechnow.co.uk/" target="_blank">Future Tech Now</a> and promised to share some of the content I used in the talk. If you want to follow along you will need to <a href="https://docs.microsoft.com/en-us/windows/mixed-reality/install-the-tools" target="_blank">install the tools</a>.</p><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2018/04/image.png"><img width="670" height="451" title="image" style="display: inline; background-image: none;" alt="image" src="http://peted.azurewebsites.net/wp-content/uploads/2018/04/image_thumb.png" border="0"></a></p><p>The focus of the talk was mainly around building a Mixed Reality app in Unity. One of the most interesting aspects of the Windows Mixed Reality platform is that you can use the tools to build an app that work in a number of different MR scenarios. I built an app that runs on HoloLens and also on an immersive headset ( I am using the Dell Visor currently). There are obvious differences across an app running on a device that has holograms interacting with the real world and one in which everything is virtual, including the differences in the input systems and navigation. The tools abstract the differences somewhat making it fairly straight-forward to have an app that spans the Mixed Reality spectrum.</p><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2018/04/ants.jpg"><img width="656" height="371" title="ants" style="display: inline; background-image: none;" alt="ants" src="http://peted.azurewebsites.net/wp-content/uploads/2018/04/ants_thumb.jpg" border="0"></a></p><p>Here’s the app running on a HoloLens. It consists of an angry, red ant that you can size and position using components recently added to the Mixed Reality Toolkit for Unity. These include a bounding box which you can use to rotate and scale your 3D model with an app bar to allow you to control that experience.</p><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2018/04/image-1.png"><img width="542" height="613" title="image" style="display: inline; background-image: none;" alt="image" src="http://peted.azurewebsites.net/wp-content/uploads/2018/04/image_thumb-1.png" border="0"></a></p><p>Once happy with the size and position of your ant you can use voice commands ‘multiply’ and ‘run’ to first clone the ant into 10 ants and subsequently to make them run randomly around your space.&nbsp; Here’s a clip of the app running in the Dell HMD.</p><p><iframe width="560" height="315" src="https://www.youtube.com/embed/4ja01EjRTlk?ecver=1" frameborder="0" allowfullscreen="" allow="autoplay; encrypted-media"></iframe></p><p>And here’s a 10 minute walkthrough of how it was made with the <a href="https://github.com/Microsoft/MixedRealityToolkit-Unity" target="_blank">Mixed Reality Toolkit</a> and Unity.</p><p><iframe width="560" height="315" src="https://www.youtube.com/embed/Wp8Pk5nGYWk?ecver=1" frameborder="0" allowfullscreen="" allow="autoplay; encrypted-media"></iframe></p><p>And you can find the code <a href="https://github.com/peted70/VRShowDemo" target="_blank">here</a> on Github.</p>