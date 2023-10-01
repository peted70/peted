---
layout: single
title: Microsoft Graph Auth on HoloLens&ndash;Device Code Flow
date: 2019-01-17 16:42
author: peted70
comments: true
categories: [HoloLens, HoloLens, mixedreality, msgraph, oauth2, oauth2]
---
<a href="http://peted.azurewebsites.net/wp-content/uploads/2019/01/device-code.png"><img style="display: inline; background-image: none;" title="device-code" src="http://peted.azurewebsites.net/wp-content/uploads/2019/01/device-code_thumb.png" alt="device-code" width="670" height="526" border="0" /></a>

I was working with a sample that I had <a href="https://peted.azurewebsites.net/microsoft-graph-auth-on-hololens/" target="_blank" rel="noopener">previously written</a> using the Microsoft Auth Library which was originally used as an example of delegated auth on HoloLens but I recently extended the sample to also show 'device code flow' which you can see in the OAuth 2.0 spec here <a title="https://oauth.net/2/grant-types/device-code/" href="https://oauth.net/2/grant-types/device-code/">https://oauth.net/2/grant-types/device-code/</a>. This flow allows the auth to happen on a second device which may be more convenient if typing passwords or codes is required on a HoloLens device given that the keyboard uses a gaze + air-tap input mechanism.

<iframe src="https://www.youtube.com/embed/pKO8AxSIyGw" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe>
<blockquote>The video shows the device code flow in action using a HoloLens and a mobile phone as the second device.</blockquote>
So, I select the 'code flow' option by gazing and air-tapping (voice commands are also available). The flow is initiated by a call to <strong>AcquireTokenWithDeviceCodeAsync</strong> which is a method on the <strong>PublicClientApplication</strong> type from the MSAL library.

<script src="https://gist.github.com/peted70/9819e7cadd1d76eb1e8341c434e137eb.js"></script>

The UI then shows a url and a code. On my phone (or other device) I navigate to the url in a browser and type in the code. I can then authenticate with my work account credentials and a token is returned to my app so I can use that in a call to the Microsoft Graph API to retrieve emails which I then display when clicking on the envelope models on the left.

The repo for this sample can be found <a href="https://github.com/peted70/msal-hololens" target="_blank" rel="noopener">here</a>
