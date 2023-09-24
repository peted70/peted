---
layout: post
title: HoloLens Intro &ndash; Resources to Get Started
date: 2016-10-30 18:09
author: peted70
comments: true
categories: [HoloLens, HoloLens, MR]
---
<p>(Originally posted here <a title="https://blogs.technet.microsoft.com/uktechnet/2016/10/12/hololens-is-available-to-purchase-for-developers-and-enterprise-customers-in-the-uk-heres-everything-you-need-to-know/" href="https://blogs.technet.microsoft.com/uktechnet/2016/10/12/hololens-is-available-to-purchase-for-developers-and-enterprise-customers-in-the-uk-heres-everything-you-need-to-know/">https://blogs.technet.microsoft.com/uktechnet/2016/10/12/hololens-is-available-to-purchase-for-developers-and-enterprise-customers-in-the-uk-heres-everything-you-need-to-know/</a>)</p> <blockquote> <p>HoloLens is available to purchase for developers and enterprise customers in the UK – here’s everything you need to know to get started</p></blockquote> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/10/hololens1.png"><img title="hololens" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="hololens" src="http://peted.azurewebsites.net/wp-content/uploads/2016/10/hololens_thumb1.png" width="742" height="376"></a> </p> <blockquote> <p>Although I work for Microsoft I am not on the HoloLens engineering team and as a result try not to speculate about engineering topics. I have a deep interest in virtual, mixed and augmented reality and by extension the HoloLens. Any opinions expressed in this article are mine.</p></blockquote> <p>The whole design of the Microsoft HoloLens is centred around it being a fully mobile device with no need to be tethered to another computer; it is a full Windows 10 PC which has an extremely deep understanding of its environment. It is worn on the head and the optics allow viewing holograms mixed with the real world as if they were actually there which it achieves very effectively with detailed, crisp and bright holograms. It knows where the surfaces that make up your environment are and can remember that information enabling the interaction of digital holograms with the real world. If I ‘throw’ a digital ball it can bounce along my real coffee table, fall off and bounce or roll along the floor. In addition, if I leave holograms in a particular location, say I leave a tiger on the worktop in my kitchen, when I return to my house sometime later the tiger will still be there. The HoloLens is a truly unique device which solves a host of traditionally ‘hard’ problems, i.e. inside-out tracking which enables the device to know it’s position and rotation in 3D space (a.k.a six degrees of freedom tracking). This enables the device to have a full understanding of where it is in the world – so holograms stay exactly where you left them and you can view them from any angle or position. </p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/10/kitchen-tiger.png"><img title="kitchen-tiger" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="kitchen-tiger" src="http://peted.azurewebsites.net/wp-content/uploads/2016/10/kitchen-tiger_thumb.png" width="741" height="437"></a></p> <h2>UX paradigms</h2> <p>The main user experience paradigms used in the HoloLens are Gaze, Gesture and Voice. Gaze is very intuitive in that the ‘cursor’ position is defined by where the user is looking or more accurately, which way their head is facing.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/10/ggv.png"><img title="ggv" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="ggv" src="http://peted.azurewebsites.net/wp-content/uploads/2016/10/ggv_thumb.png" width="749" height="293"></a></p> <p>Then to ‘action’ an item being gazed at a gesture or voice command may be used, for example to select the item I might carry out an air tap or simply say ‘select’. Both the gestures and the voice recognition work impressively well.</p> <h2>2D apps + UWP</h2> <p>Since the HoloLens runs Windows 10 it follows that apps built with the Universal Windows Platform will also be supported. They are and are experienced as 2D surfaces projected into the 3D environment. They can be resized, pinned to surfaces in your room and they can even follow in your line of sight as you walk around.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/10/2dapp.png"><img title="2dapp" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="2dapp" src="http://peted.azurewebsites.net/wp-content/uploads/2016/10/2dapp_thumb.png" width="731" height="389"></a></p> <h2>Scenarios / Skype</h2> <p>You can call up your Skype contacts from the HoloLens and their Skype video feed will appear as a holographic screen in your view. If your contact is using Skype for Windows they will be able to see what you are seeing and draw holograms into your world. </p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/10/skype.png"><img title="skype" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="skype" src="http://peted.azurewebsites.net/wp-content/uploads/2016/10/skype_thumb.png" width="729" height="415"></a></p> <p>Both your contact and you have the ability to use inking and draw lines/arrows, etc. really taking remote assistance to a new level. Skype for Windows requires an add-in to enable drawing</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/10/skype-plugin.png"><img title="skype plugin" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="skype plugin" src="http://peted.azurewebsites.net/wp-content/uploads/2016/10/skype-plugin_thumb.png" width="734" height="643"></a></p> <p>See <a title="https://www.microsoft.com/microsoft-hololens/en-us/apps/skype" href="https://www.microsoft.com/microsoft-hololens/en-us/apps/skype">https://www.microsoft.com/microsoft-hololens/en-us/apps/skype</a> for further details on usage and scenarios.</p> <h2>Development environment</h2> <p>The most important thing to call out here is that you don’t need a device to get started with understanding the development platform as you can use the HoloLens emulator to run and debug your app code. This will allow you to get a head-start before your device arrives or to get a deep insight into what might be possible and what coding and design skills might be required.If you are looking to get started on 2D apps or already have an existing UWP app you want to try then you will need Visual Studio 2015 with the latest update and the HoloLens emulator from the links below and you will have enough to build, debug and deploy. You can find documentation and guidance for UWP at <a href="http://dev.windows.com" target="_blank">Windows Dev Center</a>&nbsp; </p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/10/emulator.png"><img title="emulator" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="emulator" src="http://peted.azurewebsites.net/wp-content/uploads/2016/10/emulator_thumb.png" width="827" height="518"></a></p> <p>To get started with a 3D app the most popular option is using Unity for which the <a href="https://developer.microsoft.com/en-us/windows/holographic/academy" target="_blank">Holographic Academy</a> tutorials are a great place to start or If you are not familiar at all with Unity the official Unity website has some great resources to get you started <a title="https://unity3d.com/learn/" href="https://unity3d.com/learn/">https://unity3d.com/learn/</a>&nbsp;</p> <p>You will need the latest Visual Studio 2015 update, the HoloLens emulator or a HoloLens device and for development of 3D apps the latest Unity technical preview for HoloLens. Install the tools from here:<a href="https://developer.microsoft.com/en-us/windows/holographic/install_the_tools">https://developer.microsoft.com/en-us/windows/holographic/install_the_tools</a>  <p>The HoloLens emulator install also installs Visual Studio templates for C++ and Direct3D or C# and <a href="http://sharpdx.org/" target="_blank">SharpDX</a> if you prefer lower-level coding but if you are unfamiliar this is a steep learning curve. Here are some resources if that is your preferred route:</p> <p><a title="https://code.msdn.microsoft.com/windowsapps/Visual-Studio-3D-Starter-54ec8d19" href="https://code.msdn.microsoft.com/windowsapps/Visual-Studio-3D-Starter-54ec8d19" target="_blank">Visual Studio 3D Starter Kit</a></p> <p><a title="https://github.com/Microsoft/DirectX-Graphics-Samples" href="https://github.com/Microsoft/DirectX-Graphics-Samples" target="_blank">DirectX Samples</a></p> <p><a title="https://github.com/Microsoft/DirectXTK" href="https://github.com/Microsoft/DirectXTK" target="_blank">DirectX Toolkit</a></p> <p><a title="https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx" href="https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx" target="_blank">DirectX Graphics and Gaming Documentation</a></p> <p>The HoloLens forums can be found here <a href="https://forums.hololens.com/">https://forums.hololens.com/</a> if you want to ask questions and find other developers and here are some other fantastic resources which will help with HoloLens development.</p> <p><a href="https://github.com/microsoft/HoloToolkit">HoloToolkit</a> – tools which solve common dev scenarios  <p><a href="https://github.com/microsoft/HoloToolkit-Unity">HoloToolkit-Unity</a> – same as above but Unity-compatible  <p><a href="https://github.com/microsoft/GalaxyExplorer">Galaxy Exporer</a> – a fully working sample  <p><a href="https://developer.microsoft.com/en-us/windows/holographic/academy" target="_blank">Holographic Academy</a> – comprehensive tutorials  <p>The Official HoloLens website <a href="https://www.hololens.com">https://www.hololens.com</a> which has lots more information including ordering a HoloLens device.  <p>&nbsp;</p> <p>Pete Daukintis <a title="https://twitter.com/peted70" href="https://twitter.com/peted70" target="_blank">twitter</a>&nbsp;<a href="https://www.linkedin.com/in/peterdaukintis" target="_blank">LinkedIn</a></p>