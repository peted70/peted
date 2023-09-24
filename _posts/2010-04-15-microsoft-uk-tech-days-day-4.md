---
layout: post
title: Microsoft UK Tech days (day 4)
date: 2010-04-15 20:45
author: peted70
comments: true
categories: [UK Tech Days]
---
<div id="msgcns!4F1B7368284539E5!208" class="bvMsg"><p>by Peter Daukintis</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/techdays3.jpg" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="tech days" border="0" alt="tech days" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/techdays3.jpg?w=300" width="467" height="282" /></a></p> <p>The cinema screen used for the presentations at the Fulham Vue Cinema. </p> <p>This was the client-based tech day:</p> <h4>Windows 7</h4> <p>The Windows 7 Code Pack provides a managed wrapper around the COM-based interfaces provided for many of the Windows 7 differentiating client application features. These include jump lists, file dialogs, sensor APIs Direct3D, linguistic services, etc. <a title="http://code.msdn.microsoft.com/WindowsAPICodePack" href="http://code.msdn.microsoft.com/WindowsAPICodePack">http://code.msdn.microsoft.com/WindowsAPICodePack</a>. Sensor support in Windows 7, such as .NET 4.0 location APIs, light sensors, etc.</p> <h4>WPF 4.0</h4> <p>Another great presentation from Ian Griffiths on WPF 4.0 and supporting features in VS2010. VS2010 has improved data-binding, better intellisense and an improved designer. The new features include a User Interface for styles and resources, a user interface for editing data-binding and drag and drop support for the data sources window. </p> <p>It is also possible to get ViewModel data-binding support from within VS2010 using the xaml namespace in the following way:</p> <p>d:DataContext=”&#123;d:DesignInstance vm:myViewModel&#125;”</p> <p>where d is the xamlnamespace and vm is the namespace with your view model in. This will then allow the data-binding to be wired up via the new user interface elements. This is pretty cool and should decrease the chances of making mistakes when setting up the bindings manually.</p> <h5>Graphics</h5> <ul> <li>Text rendering now provides much more control for the developer and the trade-offs between clarity and fidelity can be carefully selected in the code.  <li>More things are offloaded to the GPU – although no details are available on what??  <li>Pixel Shader 3.0 support  <li>Bitmap caching support  <li>Animation easing support (from Silverlight)  <li>Layout rounding  <li>Windows 7 features have been added – multi-touch support, Windows 7 dialogs, jump lists via xaml.</li></li></ul> <h5>Data-Binding</h5> <ul> <li>Run text items can now be data-bound (didn’t work in 3.5)  <li>Dynamic objects can be data-bound (support for c# dynamic keyword)  <li>Input Binding (keypress, mousedown, etc) now works.  <li>Datagrid, calendar, date picker now all built-in controls.  <li>Now includes Visual State Manager (from Silverlight). WPF strategy of using triggers, although very flexible considered less designer-friendly.</li></li></ul> <p>WPF XAML parsing has been improved and the internals are now more visible. Also, there is now one parser that is used by the various clients; blend, visual studio, etc – this was not the case previously! </p> <p></p>Technorati Tags: <a href="http://technorati.com/tags/Microsoft" rel="tag">Microsoft</a>,<a href="http://technorati.com/tags/Tech" rel="tag">Tech</a>,<a href="http://technorati.com/tags/Code" rel="tag">Code</a>,<a href="http://technorati.com/tags/Pack" rel="tag">Pack</a>,<a href="http://technorati.com/tags/wrapper" rel="tag">wrapper</a>,<a href="http://technorati.com/tags/client" rel="tag">client</a>,<a href="http://technorati.com/tags/features" rel="tag">features</a>,<a href="http://technorati.com/tags/sensor" rel="tag">sensor</a>,<a href="http://technorati.com/tags/APIs" rel="tag">APIs</a>,<a href="http://technorati.com/tags/services" rel="tag">services</a>,<a href="http://technorati.com/tags/WindowsAPICodePack" rel="tag">WindowsAPICodePack</a>,<a href="http://technorati.com/tags/location" rel="tag">location</a>,<a href="http://technorati.com/tags/Another" rel="tag">Another</a>,<a href="http://technorati.com/tags/presentation" rel="tag">presentation</a>,<a href="http://technorati.com/tags/Griffiths" rel="tag">Griffiths</a>,<a href="http://technorati.com/tags/data" rel="tag">data</a>,<a href="http://technorati.com/tags/User" rel="tag">User</a>,<a href="http://technorati.com/tags/Interface" rel="tag">Interface</a>,<a href="http://technorati.com/tags/ViewModel" rel="tag">ViewModel</a>,<a href="http://technorati.com/tags/DataContext" rel="tag">DataContext</a>,<a href="http://technorati.com/tags/DesignInstance" rel="tag">DesignInstance</a>,<a href="http://technorati.com/tags/chances" rel="tag">chances</a>,<a href="http://technorati.com/tags/Graphics" rel="tag">Graphics</a>,<a href="http://technorati.com/tags/Text" rel="tag">Text</a>,<a href="http://technorati.com/tags/Pixel" rel="tag">Pixel</a>,<a href="http://technorati.com/tags/Shader" rel="tag">Shader</a>,<a href="http://technorati.com/tags/Bitmap" rel="tag">Bitmap</a>,<a href="http://technorati.com/tags/Animation" rel="tag">Animation</a>,<a href="http://technorati.com/tags/Layout" rel="tag">Layout</a>,<a href="http://technorati.com/tags/items" rel="tag">items</a>,<a href="http://technorati.com/tags/Dynamic" rel="tag">Dynamic</a>,<a href="http://technorati.com/tags/objects" rel="tag">objects</a>,<a href="http://technorati.com/tags/Input" rel="tag">Input</a>,<a href="http://technorati.com/tags/Datagrid" rel="tag">Datagrid</a>,<a href="http://technorati.com/tags/calendar" rel="tag">calendar</a>,<a href="http://technorati.com/tags/Visual" rel="tag">Visual</a>,<a href="http://technorati.com/tags/State" rel="tag">State</a>,<a href="http://technorati.com/tags/Manager" rel="tag">Manager</a>,<a href="http://technorati.com/tags/strategy" rel="tag">strategy</a>,<a href="http://technorati.com/tags/XAML" rel="tag">XAML</a>,<a href="http://technorati.com/tags/Also" rel="tag">Also</a>,<a href="http://technorati.com/tags/clients" rel="tag">clients</a>,<a href="http://technorati.com/tags/studio" rel="tag">studio</a>,<a href="http://technorati.com/tags/interfaces" rel="tag">interfaces</a>,<a href="http://technorati.com/tags/dialogs" rel="tag">dialogs</a>,<a href="http://technorati.com/tags/namespace" rel="tag">namespace</a><br /> <p></p>Windows Live Tags: <a href="http://windows.live.com/connect/tag/Microsoft" rel="clubhouseTag">Microsoft</a>,<a href="http://windows.live.com/connect/tag/Tech" rel="clubhouseTag">Tech</a>,<a href="http://windows.live.com/connect/tag/Code" rel="clubhouseTag">Code</a>,<a href="http://windows.live.com/connect/tag/Pack" rel="clubhouseTag">Pack</a>,<a href="http://windows.live.com/connect/tag/wrapper" rel="clubhouseTag">wrapper</a>,<a href="http://windows.live.com/connect/tag/client" rel="clubhouseTag">client</a>,<a href="http://windows.live.com/connect/tag/features" rel="clubhouseTag">features</a>,<a href="http://windows.live.com/connect/tag/sensor" rel="clubhouseTag">sensor</a>,<a href="http://windows.live.com/connect/tag/APIs" rel="clubhouseTag">APIs</a>,<a href="http://windows.live.com/connect/tag/services" rel="clubhouseTag">services</a>,<a href="http://windows.live.com/connect/tag/WindowsAPICodePack" rel="clubhouseTag">WindowsAPICodePack</a>,<a href="http://windows.live.com/connect/tag/location" rel="clubhouseTag">location</a>,<a href="http://windows.live.com/connect/tag/Another" rel="clubhouseTag">Another</a>,<a href="http://windows.live.com/connect/tag/presentation" rel="clubhouseTag">presentation</a>,<a href="http://windows.live.com/connect/tag/Griffiths" rel="clubhouseTag">Griffiths</a>,<a href="http://windows.live.com/connect/tag/data" rel="clubhouseTag">data</a>,<a href="http://windows.live.com/connect/tag/User" rel="clubhouseTag">User</a>,<a href="http://windows.live.com/connect/tag/Interface" rel="clubhouseTag">Interface</a>,<a href="http://windows.live.com/connect/tag/ViewModel" rel="clubhouseTag">ViewModel</a>,<a href="http://windows.live.com/connect/tag/DataContext" rel="clubhouseTag">DataContext</a>,<a href="http://windows.live.com/connect/tag/DesignInstance" rel="clubhouseTag">DesignInstance</a>,<a href="http://windows.live.com/connect/tag/chances" rel="clubhouseTag">chances</a>,<a href="http://windows.live.com/connect/tag/Graphics" rel="clubhouseTag">Graphics</a>,<a href="http://windows.live.com/connect/tag/Text" rel="clubhouseTag">Text</a>,<a href="http://windows.live.com/connect/tag/Pixel" rel="clubhouseTag">Pixel</a>,<a href="http://windows.live.com/connect/tag/Shader" rel="clubhouseTag">Shader</a>,<a href="http://windows.live.com/connect/tag/Bitmap" rel="clubhouseTag">Bitmap</a>,<a href="http://windows.live.com/connect/tag/Animation" rel="clubhouseTag">Animation</a>,<a href="http://windows.live.com/connect/tag/Layout" rel="clubhouseTag">Layout</a>,<a href="http://windows.live.com/connect/tag/items" rel="clubhouseTag">items</a>,<a href="http://windows.live.com/connect/tag/Dynamic" rel="clubhouseTag">Dynamic</a>,<a href="http://windows.live.com/connect/tag/objects" rel="clubhouseTag">objects</a>,<a href="http://windows.live.com/connect/tag/Input" rel="clubhouseTag">Input</a>,<a href="http://windows.live.com/connect/tag/Datagrid" rel="clubhouseTag">Datagrid</a>,<a href="http://windows.live.com/connect/tag/calendar" rel="clubhouseTag">calendar</a>,<a href="http://windows.live.com/connect/tag/Visual" rel="clubhouseTag">Visual</a>,<a href="http://windows.live.com/connect/tag/State" rel="clubhouseTag">State</a>,<a href="http://windows.live.com/connect/tag/Manager" rel="clubhouseTag">Manager</a>,<a href="http://windows.live.com/connect/tag/strategy" rel="clubhouseTag">strategy</a>,<a href="http://windows.live.com/connect/tag/XAML" rel="clubhouseTag">XAML</a>,<a href="http://windows.live.com/connect/tag/Also" rel="clubhouseTag">Also</a>,<a href="http://windows.live.com/connect/tag/clients" rel="clubhouseTag">clients</a>,<a href="http://windows.live.com/connect/tag/studio" rel="clubhouseTag">studio</a>,<a href="http://windows.live.com/connect/tag/interfaces" rel="clubhouseTag">interfaces</a>,<a href="http://windows.live.com/connect/tag/dialogs" rel="clubhouseTag">dialogs</a>,<a href="http://windows.live.com/connect/tag/namespace" rel="clubhouseTag">namespace</a>  </div>