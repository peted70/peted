---
layout: post
title: HoloLens2 local anchors with OpenXR in Unity
date: 2022-04-03 10:32
author: peted70
comments: true
categories: [HoloLens, Mixed Reality, MR, Uncategorized, unity]
---
<!-- wp:image {"id":8721,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/04/content-.jpg" alt="" class="wp-image-8721"/><figcaption>Anchoring a scene</figcaption></figure>
<!-- /wp:image -->

<!-- wp:verse -->
<pre class="wp-block-verse">Sample Code
Jump straight to the <a href="https://github.com/peted70/LocalAnchors" target="_blank" rel="noreferrer noopener">Github repo here</a></pre>
<!-- /wp:verse -->

<!-- wp:paragraph -->
<p>I recently required the functionality of creating a single, persistent local <a rel="noreferrer noopener" href="https://docs.microsoft.com/en-us/windows/mixed-reality/design/spatial-anchors" target="_blank">spatial anchor</a> on HoloLens2. The purpose is to provide stability and a persistent real-world location for a hierarchy of 3d model content. In this case there was no need for compatibility with other device types or for persistence across application instances. As a result <a rel="noreferrer noopener" href="https://azure.microsoft.com/en-gb/services/spatial-anchors/#overview" target="_blank">Azure Spatial Anchors</a> are not really needed and everything can happen locally on a HoloLens2 device without network access. In addition, for my use case only one anchor was needed as the content is relatively small-scale and all bounded within a 3m radius. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>For other scenarios, such as cross-device sharing consider <a rel="noreferrer noopener" href="https://azure.microsoft.com/en-gb/services/spatial-anchors/#overview" target="_blank">Azure Spatial Anchors</a> and if you need to anchor larger holograms then look at the <a href="https://docs.microsoft.com/en-us/mixed-reality/world-locking-tools/" target="_blank" rel="noreferrer noopener">World Locking Tools</a>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>We'll use the <a rel="noreferrer noopener" href="https://docs.microsoft.com/en-us/windows/mixed-reality/develop/unity/welcome-to-mr-feature-tool" target="_blank">Mixed Reality Toolkit Feature Tool</a> to configure the project with the MRTK and the OpenXR Plugin</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8719,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2022/04/mrtk-features.png" alt="" class="wp-image-8719"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>It is worth noting that since the last time I looked the APIs for this stuff have changed; I recall adding a script <strong>WorldAnchor</strong> to a gameObject to associate it with a spatial anchor and then using <strong>WorldAnchorStore</strong> to persist the anchor.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>In order to access the correct APIs you would need to add the OpenXR plugin (I used the Mixed Reality Toolkit feature tool to do this).</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8715,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/03/oxr-plugin-.png" alt="" class="wp-image-8715"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The newer APIs we will use for this post are: </p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p><a href="https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@4.1/api/UnityEngine.XR.ARFoundation.ARAnchorManager.html" target="_blank" rel="noreferrer noopener">ARAnchorManager</a>, <a href="https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@4.1/api/UnityEngine.XR.ARFoundation.ARAnchor.html" target="_blank" rel="noreferrer noopener">ARAnchor</a> AR Foundation namespace<br><a href="https://docs.microsoft.com/en-us/windows/mixed-reality/develop/unity/spatial-anchors-in-unity?tabs=anchorstore#persistent-world-locking" target="_blank" rel="noreferrer noopener">XRAnchorStore</a> is in the OpenXR namespace</p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>Also, there is an official sample <a rel="noreferrer noopener" href="https://github.com/microsoft/OpenXR-Unity-MixedReality-Samples/tree/main/BasicSample" target="_blank">here</a> upon which this post is based but is slightly simplified here as required for my simpler scenario.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>First, I created an <strong>AnchorScript.cs</strong> and added it to my scene.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8716,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2022/03/anchor-script.png" alt="" class="wp-image-8716"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The ARAnchorManger and ARSessionOrigin and both in ARFoundation and will get added automatically when adding AnchorScript to a gameObject since they are required components. The AnchorScript itself references 3 items:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Placing Game Object - this has an <a href="https://docs.microsoft.com/en-us/windows/mixed-reality/mrtk-unity/features/ux-building-blocks/object-manipulator?view=mrtkunity-2021-05" target="_blank" rel="noreferrer noopener">ObjectManipulator</a> script attached and will be used to position where we want the local anchor to be in 3d space</li><li>Anchor Prefab - This does not require a graphical element but I used a small coordinate system gizmo so we can identify the root of the anchors coordinate system</li><li>Actual Content - This is a reference to the content that we want to anchor</li></ul>
<!-- /wp:list -->

<!-- wp:heading -->
<h2>Usage</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The transparent cube is used to position a local anchor in space so you can use far interactions or near interactions to position the cube and when the interaction ends a local anchor is created at that position and rotation.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The small axis mesh will show where there is currently a local anchor.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The content under the&nbsp;<code>ActualContent</code>&nbsp;parent gameObject will be re-parented under the anchored gameObject root. Subsequently introduced content would also be parented unfer that gameObject.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you close the app and restart the anchor and the content will be positioned relative to where you left it.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Map Manager</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>The <a rel="noreferrer noopener" href="https://docs.microsoft.com/en-us/windows/mixed-reality/develop/advanced-concepts/using-the-windows-device-portal#map-manager" target="_blank">Map Manager</a> part of the HoloLens developer portal provides a view onto the anchor store. You can use this to diagnose creation and deletion of anchors and save/load maps and anchors.</p>
<!-- /wp:paragraph -->
