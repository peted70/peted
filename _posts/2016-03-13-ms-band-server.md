---
layout: post
title: MS Band Server &ndash; sensor data in a browser
date: 2016-03-13 12:58
author: peted70
comments: true
categories: [band, Microsoft, MS Band, nodejs, processing.js, sdk, uwp]
---
<p>Chatting recently with <a href="https://twitter.com/mikebrondbjerg">Mike Brondbjerg</a> recently about some data visualisation ideas around using the <a href="https://www.microsoft.com/microsoft-band/">Microsoft Band</a> and we came to the conclusion that getting live sensor data from the band into the browser would open up the opportunity to use the data to drive some visualisation experiences using javascript. We had <a href="http://processingjs.org/">processing.js</a> in mind but there are a whole slew of javascript visualisation libraries available.I had created a project to get Kinect data into the browser a few months ago (see <a title="http://peted.azurewebsites.net/kinect-4-windows-v2-in-the-browser/" href="http://peted.azurewebsites.net/kinect-4-windows-v2-in-the-browser/" target="_blank">Kinect 4 Windows V2 – in the browser</a>) so thought it would make a good starting point. I couldn’t consume the real-time sensor data using the <a href="https://developer.microsoftband.com/bandSDK" target="_blank">Microsoft Band SDK</a> in a console app this time as it is dependent on a non-compatible .NET (no such problems with the Kinect SDK as it is based on WinRT). This meant using a modern Windows App to consume data from the band, send the data over a web socket to a web socket server which broadcasts the data to any client that wishes to connect – in this case a wep page using processing.js in a browser.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/03/bandtest.png"><img title="bandtest" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="bandtest" src="http://peted.azurewebsites.net/wp-content/uploads/2016/03/bandtest_thumb.png" width="738" height="683"></a></p> <p>This all runs on your local machine (or indeed across the web if latency isn’t an issue). Once you have this running there is plenty of scope for creativity; use the heart rate data to drive a visualisation or crowdsource positional data using accelerometer and gyroscope to create a collaborative artwork. Instructions for setting up are at the readme here <a title="https://github.com/peted70/BandServerApp" href="https://github.com/peted70/BandServerApp">https://github.com/peted70/BandServerApp</a></p> <p>Mike published a test visualisation here </p> <blockquote class="twitter-tweet" data-lang="en"> <p lang="en" dir="ltr">Yesterday, experimenting with realtime <a href="https://twitter.com/hashtag/dataviz?src=hash">#dataviz</a> of MS Band BPM in Processing via WebSockets. <a href="https://t.co/ep7ygzAAk7">https://t.co/ep7ygzAAk7</a> <a href="https://t.co/438UVvS4td">pic.twitter.com/438UVvS4td</a></p>— Mike Brondbjerg (@mikebrondbjerg) <a href="https://twitter.com/mikebrondbjerg/status/707158906146066432">March 8, 2016</a></blockquote> <p><script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script></p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/03/mikesvid.png"><img title="mikesvid" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="mikesvid" src="http://peted.azurewebsites.net/wp-content/uploads/2016/03/mikesvid_thumb.png" width="734" height="412"></a></p> <p>Don’t worry if you have no band available for dev – use the <a href="http://peted.azurewebsites.net/fake-microsoft-band/" target="_blank">Fake Band</a> instead.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2016/03/nobandnoproblem.png"><img title="nobandnoproblem" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="nobandnoproblem" src="http://peted.azurewebsites.net/wp-content/uploads/2016/03/nobandnoproblem_thumb.png" width="552" height="552"></a></p> <p>Github project is here <a title="https://github.com/peted70/BandServerApp" href="https://github.com/peted70/BandServerApp">https://github.com/peted70/BandServerApp</a></p>