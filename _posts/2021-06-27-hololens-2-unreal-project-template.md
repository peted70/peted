---
layout: single
title: HoloLens 2 Unreal Project Template
excerpt: "Each time I wanted to create a HoloLens 2 Unreal Engine project I found myself recreating a blank level, setting up a GameMode and configuring the project. Like any programmer doing anything twice that can be automated is unpalatable, so..."
date: 2021-06-27 16:22
author_profile: true
comments: true
categories: [HoloLens, HoloLens, Mixed Reality, mrtk, unreal engine, unreal engine]
header:
    teaserlogo: 'assets/images/2021/06/template.png'
    teaser: 'assets/images/2021/06/template.png'
---
<!-- wp:paragraph -->
<p>Each time I wanted to create a HoloLens 2 Unreal Engine project I found myself recreating a blank level, setting up a GameMode and configuring the project. Like any programmer doing anything twice that can be automated is unpalatable, so...</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8609,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/template.png" alt="" class="wp-image-8609"/><figcaption>Unreal Template Selector</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>TLDR;</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Navigate a browser here <a href="https://github.com/peted70/hl2-ue-template">peted70/hl2-ue-template (github.com)</a> and carry out the instructions to install a project template configured with:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Plugins (<a rel="noreferrer noopener" href="https://github.com/microsoft/MixedReality-UXTools-Unreal" target="_blank">UX Tools</a>, <a rel="noreferrer noopener" href="https://github.com/microsoft/MixedReality-GraphicsTools-Unreal" target="_blank">Graphics Tools</a>, <a rel="noreferrer noopener" href="https://github.com/microsoft/Microsoft-OpenXR-Unreal" target="_blank">OpenXR</a>)</li><li>Recommended settings for a HoloLens application</li><li>A map containing a cube that you can use your hands to interact with</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>That's it for the short version but stick around here for some more details about how this was created.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Creating a Project</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>Here are the steps I usually go through:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Create a new Project </li></ul>
<!-- /wp:list -->

<!-- wp:image {"id":8612,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/insert-project.png" alt="" class="wp-image-8612"/><figcaption>New Project</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Whilst I am creating test and sample projects I will often add and enable plugins:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>UX Tools &amp; Examples</li><li>Graphics Tools &amp; Examples</li><li>OpenXR</li></ul>
<!-- /wp:list -->

<!-- wp:paragraph -->
<p>Some of these would come from Unreal marketplace and I would have them preinstalled to the engine and some I would clone from Github and copy into my project's Plugins folder.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8616,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/plugins2.png" alt="" class="wp-image-8616"/><figcaption>Plugins</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>After restarting Unreal you will be prompted to recompile:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8617,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/recompile.png" alt="" class="wp-image-8617"/><figcaption>Recompile</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Then I would follow roughly the guide <a rel="noreferrer noopener" href="http://Now follow the instructions to set up a project for HL2 in the Unreal Getting Started guide (https://docs.microsoft.com/en-us/windows/mixed-reality/develop/unreal/tutorials/unreal-uxt-ch1)" target="_blank">here</a> to configure for HoloLens. Then I, added a 'player start' at (0, 0, 0), a cube with a simple textured material. So, the level looks like this:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8619,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/level.png" alt="" class="wp-image-8619"/><figcaption>MR Level</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>And I have these items in my content folder:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8620,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/content.png" alt="" class="wp-image-8620"/><figcaption>Content</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2>Template</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>This is a bit time-consuming to carry out each time I want to create a quick test or sample project so I decided to automate it using a project template <a href="https://docs.unrealengine.com/4.26/en-US/Resources/Templates/">https://docs.unrealengine.com/4.26/en-US/Resources/Templates/</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The basic idea is to copy the project you want the template to be based on into a folder here <strong>[Unreal install location]\[Unreal version]]\Templates</strong> and configure it using a <strong>TemplateDefs.ini</strong> file. There are plenty of examples in that folder already so if you want to create a template I would suggest looking at those as they are well commented.</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>Note. </p><p>Unfortunately, it is possible that you would hit the issue around windows path names being too long</p><p>See <a href="https://docs.microsoft.com/en-us/windows/win32/fileio/naming-a-file#maximum-path-length-limitation">https://docs.microsoft.com/en-us/windows/win32/fileio/naming-a-file#maximum-path-length-limitation</a></p><p>I used the instructions here <a href="https://docs.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=cmd#enable-long-paths-in-windows-10-version-1607-and-later">https://docs.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=cmd#enable-long-paths-in-windows-10-version-1607-and-later</a> to get around this issue.</p><p>And also, in the Unreal Editor, check the “Enable Long Paths Support” in the experimental settings dialog.</p><p>The registry key Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem\LongPathsEnabled (Type: REG_DWORD) must exist and be set to 1</p><p>If you hit an issue with long path names when compiling for the first time then just moving to a shorter root path for your project is a possible workaround.</p></blockquote>
<!-- /wp:quote -->

<!-- wp:heading -->
<h2>Create a New HL2 Project with the Template</h2>
<!-- /wp:heading -->

<!-- wp:image {"id":8623,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/create-project.png" alt="" class="wp-image-8623"/><figcaption>Create New Project</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":8625,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/select-template.png" alt="" class="wp-image-8625"/><figcaption>Select Template</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":8627,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/settings.png" alt="" class="wp-image-8627"/><figcaption>Project Settings</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>When you hit create the project files will get copied over from the template and the plugins will be compiled (this could take a while). After that you will have a project that looks a bit like this. Hitting play will enable you to use the dummy hands to interact with the cube:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8628,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/06/player.png" alt="" class="wp-image-8628"/><figcaption>Player</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>In order to deploy to a device you will need a developer certificate which you can generate by navigating to <strong>Project Settings &gt; Platforms &gt; HoloLens</strong> and choose to generate a new certificate. Then you can create an app package by choosing <strong>File &gt; Package Project &gt; HoloLens</strong> and you can subsequently sideload the app package using the HoloLens Developer Portal - details <a href="https://docs.microsoft.com/en-us/hololens/app-deploy-app-installer" target="_blank" rel="noreferrer noopener">here</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p></p>
<!-- /wp:paragraph -->
