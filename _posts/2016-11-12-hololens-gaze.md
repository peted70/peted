---
layout: post
title: HoloLens Gaze
date: 2016-11-12 12:52
author: peted70
comments: true
categories: [HoloLens, HoloLens, HoloToolkit, Unity, unity3d]
---
<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/11/Gaze.png"><img title="Gaze" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="Gaze" src="http://peted.azurewebsites.net/wp-content/uploads/2016/11/Gaze_thumb.png" width="706" height="810"></a></p> <blockquote> <p>This is the second part of a series see <a title="https://peted.azurewebsites.net/hololens-end-to-end-in-unity/" href="https://peted.azurewebsites.net/hololens-end-to-end-in-unity/">https://peted.azurewebsites.net/hololens-end-to-end-in-unity/</a> Steps to create the demo project that I used in my Future Decoded UK 2016 presentation</p></blockquote> <p>Gaze is a way to direct your attention in the HoloLens environment – think of it as a mouse cursor but it is directed by the orientation of your head (not your eyes). This does take a short period of time to become accustomed to but then feels pleasingly natural thereafter. In order to understand at which point we are gazing at it is useful to have some kind of a visualisation and for the demo I used a cursor which is supplied as a <a href="https://docs.unity3d.com/Manual/Prefabs.html" target="_blank">prefab</a> in the <a href="https://github.com/Microsoft/HoloToolkit" target="_blank">HoloToolkit</a>. </p> <blockquote> <p>To get set up with the HoloToolkit in Unity see <a title="https://mtaulty.com/2016/11/10/hitchiking-the-holotoolkit-unity-leg-1/" href="https://mtaulty.com/2016/11/10/hitchiking-the-holotoolkit-unity-leg-1/">https://mtaulty.com/2016/11/10/hitchiking-the-holotoolkit-unity-leg-1/</a></p></blockquote> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/11/basiccursor.png"><img title="basiccursor" style="border-left-width: 0px; border-right-width: 0px; background-image: none; border-bottom-width: 0px; padding-top: 0px; padding-left: 0px; display: inline; padding-right: 0px; border-top-width: 0px" border="0" alt="basiccursor" src="http://peted.azurewebsites.net/wp-content/uploads/2016/11/basiccursor_thumb.png" width="729" height="305"></a></p> <p>The <strong>BasicCursor</strong> prefab consists of a mesh, a shader (for the shadow) and a script to position the cursor.</p> <h2>GazeManager</h2> <p>The <strong>BasicCursor</strong> script depends on having a <strong>GazeManager</strong> instance. In order to achieve this you can create an empty game object (I usually rename this <strong>Managers</strong>) in your scene, find the <strong>GazeManager</strong> script in the HoloToolkit (Tip: use the project pane search box) and drag the <strong>GazeManager</strong> script onto your empty game object. This will ensure that the code inside the <strong>GazeManager</strong> script is executed at runtime and the <strong>BasicCursor</strong> script will be able to correctly reference the <strong>GazeManager</strong> instance. </p> <p>The <strong>GazeManager</strong> keeps track of the first object hit by the vector with origin at the users head position and in a direction in which the head is pointing. </p> <h2>BasicCursor</h2> <p>The <strong>BasicCursor</strong> script positions and orients its cursor mesh according to the hit object as tracked by the <strong>GazeManager. </strong>Its position is set to the hit location and the orientation is set in the direction of the normal at the hit point on the mesh.</p><iframe height="315" src="https://www.youtube.com/embed/iL9ovolF3Y8" frameborder="0" width="560" allowfullscreen></iframe> <blockquote> <p>All resources used are available on Github <a href="https://github.com/peted70/fd-holodemo">https://github.com/peted70/fd-holodemo</a> (see the <a href="https://github.com/peted70/fd-holodemo/tree/GAZE" target="_blank">GAZE</a> branch)</p></blockquote>