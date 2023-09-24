---
layout: post
title: Fake Microsoft Band
date: 2015-12-01 19:11
author: peted70
comments: true
categories: [MS Band, MS Band, uwp, UWP]
---
<p>Along with my colleague at Microsoft, David Gristwood I have been working on and off on an IOT reference architecture using data from the Microsoft Band. Dave focuses on the Azure side of things and I am traditionally a more client-side developer so we teamed up to create the <strong>Band on the Run project</strong>. More about that project at a later time but for now I just wanted to share some of the client-side code for developing a band app without connecting up a physical band. This has proven to be fairly useful for us as it has allowed us to generate band data without the need to ensure we had charged up our bands / left our bands somewhere else, etc. More importantly, it has allowed us to prototype an app which consumes data from more than one band easily.</p> <p>The official Microsoft Band SDK (<a title="https://developer.microsoftband.com/bandSDK" href="https://developer.microsoftband.com/bandSDK">https://developer.microsoftband.com/bandSDK</a>) has been designed making use of interfaces, for example</p><script src="https://gist.github.com/peted70/d3aa3db316252230599c.js"></script> <p>The IBandService can easily be faked as can all of the other functionality exposed by the SDK. I have generated fake concrete classes for all of the interfaces I needed for the Band on the Run app and have added the source to a git repo here <a title="https://github.com/BandOnTheRun/fake-band" href="https://github.com/BandOnTheRun/fake-band">https://github.com/BandOnTheRun/fake-band</a> . Those that I didn’t need throw <em>NotImplementedException</em> for the time being. If you use this project please feel free to fill those implementations as you go! The library is also available as a nuget package (<em>Install-Package FakeBand</em>) I was mainly concerned with the real-time sensor data so I used an Rx timer to simulate the data updates. Here are some screenshots of the Band on the Run client app in action:</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2015/12/BOTR-app.png"><img title="BOTR-app" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="BOTR-app" src="http://peted.azurewebsites.net/wp-content/uploads/2015/12/BOTR-app_thumb.png" width="743" height="454"></a></p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2015/12/BOTR-detail.png"><img title="BOTR-detail" style="border-top: 0px; border-right: 0px; background-image: none; border-bottom: 0px; padding-top: 0px; padding-left: 0px; border-left: 0px; display: inline; padding-right: 0px" border="0" alt="BOTR-detail" src="http://peted.azurewebsites.net/wp-content/uploads/2015/12/BOTR-detail_thumb.png" width="729" height="321"></a></p> <p>In the app I used dependency injection to choose at run-time between the fakes and the real SDK version:</p><script src="https://gist.github.com/peted70/cee73b11c16873388de2.js"></script> <p>and the FakeBandService is implemented like this:</p><script src="https://gist.github.com/peted70/5380d4ab541bb74d119a.js"></script> <p>For reference the code consuming the fake implementations is all here <a title="https://github.com/BandOnTheRun/ms-band-azure" href="https://github.com/BandOnTheRun/ms-band-azure">https://github.com/BandOnTheRun/ms-band-azure</a></p>