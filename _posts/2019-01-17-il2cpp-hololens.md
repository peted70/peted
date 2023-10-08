---
layout: single
title: IL2CPP + HoloLens
date: 2019-01-17 17:35
author_profile: true
comments: true
categories: [HoloLens, HoloLens, Unity]
excerpt: "When I build an IL2CPP project in Unity it creates a native C++ Visual Studio project which is generated from the C# that you write in your Unity scripts. So effectively converts .NET code into native C++.</blockquote>
There is a managed debugger so you can continue to work with C# in a debugging experience..."
header:
    teaserlogo: 'assets/images/2019/01/managed-debugger.png'
    teaser: 'assets/images/2019/01/managed-debugger.png'
---
Following the Unity announcement about deprecating the .NET backend I have been slowly turning my attention towards using IL2CPP instead which some time in the future will be the only option for Unity HoloLens development. Of course, there are supported LTS versions but I guess it will often be the case that as frameworks and SDKs move forwards they would tend towards supporting newer features and functionality. Either way, as a HoloLens dev it wouldn’t help to at least be prepared.
<blockquote>Just to give a very high-level description of what this means; using the .NET backend generates a .NET UWP project when building my Unity project for HoloLens. This means debugging C# code in Visual Studio and deploying a .NET (or .NET native) app to a HoloLens device. When I build an IL2CPP project in Unity it creates a native C++ Visual Studio project which is generated from the C# that you write in your Unity scripts. So effectively converts .NET code into native C++.</blockquote>
There is a managed debugger so you can continue to work with C# in a debugging experience. In the Unity build settings if you check 'Wait for managed Debugger' then

<a href="{{ site.baseurl }}/assets/images/2019/01/managed-debugger.png"><img style="display: inline; background-image: none;" title="managed debugger" src="{{ site.baseurl }}/assets/images/2019/01/managed-debugger_thumb.png" alt="managed debugger" width="629" height="520" border="0" /></a>

when you run the resulting app on the HoloLens it will put up a dialog which will wait giving you a chance to hook up the managed debugger.

<a href="{{ site.baseurl }}/assets/images/2019/01/20190117_144756_HoloLens.jpg"><img style="display: inline; background-image: none;" title="20190117_144756_HoloLens" src="{{ site.baseurl }}/assets/images/2019/01/20190117_144756_HoloLens_thumb.jpg" alt="20190117_144756_HoloLens" width="670" height="378" border="0" /></a>

I usually open two instances of Visual Studio; one with the native code and from the other choose the menu option Debug &gt; Attach Unity Debugger and use that to debug C# code

<a href="{{ site.baseurl }}/assets/images/2019/01/Attach-Unity-Debugger.png"><img style="display: inline; background-image: none;" title="Attach Unity Debugger" src="{{ site.baseurl }}/assets/images/2019/01/Attach-Unity-Debugger_thumb.png" alt="Attach Unity Debugger" width="644" height="420" border="0" /></a>

I can then set breakpoints in my C# scripts as expected. Over the last few Unity versions I have been using this experience has been steadily improving. It seemed initially to be slow and sometimes the debugger wouldn't catch my first-chance exceptions. This works well in 2018.3.0f2 though which I am currently using.
<h2>MSAL Sample</h2>
I was working with a sample that I had <a href="https://peted.azurewebsites.net/microsoft-graph-auth-on-hololens/" target="_blank" rel="noopener">previously written</a> using the <a href="https://github.com/AzureAD/microsoft-authentication-library-for-dotnet" target="_blank" rel="noopener">Microsoft Auth Library</a> which was originally used as an example of delegated auth on HoloLens but I recently extended to also show 'device code flow' which allows the auth to happen on a second device which may be more convenient if typing passwords or codes is required.

In order to use the MSAL library I downloaded the Nuget package directly from the Nuget website and then chose the relevant dll to include directly into my Unity project. The MSAL library is a .NET library so you may be wondering how this works with IL2CPP. So, the .NET assembly will get converted into C++ which is included in the resulting project.

The device code auth flow works ok in the Unity editor since it doesn't have the complication of requiring a browser to be present in the app. It also worked using the .NET backend but when I switched over to IL2CPP things stopped working and I was hit with a Exception in the managed debugger.
<blockquote>Error on deserializing read-only members in the class: No set method for property 'ErrorDesription' in type 'Microsoft.Identity.Core.Oauth2.Oauth2ResponseBase'</blockquote>
Decompiling the original assembly revealed that there was a setter for that property so where’d it go? As it turns out, the IL2CPP process will strip out any code that it detects to be unused, i.e. not referenced elsewhere. This results in less code, faster build times, etc. Detecting unused code that may actually be used by reflection is tricky though and this code can get stripped which is exactly why I got the exception above. No worries though, since it’s not a huge leap to diagnose and even easier to fix. The Unity forum staff pointed me at the link.xml file, which if you create one in your Assets folder can enable you to take some control over which code gets stripped. Adding the following xml prevents all code stripping from the auth library and fixes my first issue:

<script src="https://gist.github.com/peted70/b7e2b0a0eec27fef876a2f5b056a1148.js"></script>Now, my sample still didn’t work as this time I was getting a NullReferenceException in the managed debugger. This didn’t reveal any clues as to what the problem was so I was forced to turn towards the native debugger and the generated C++.  There are some great tips <a href="https://blogs.unity3d.com/2015/05/20/il2cpp-internals-debugging-tips-for-generated-code/" target="_blank" rel="noopener">here</a> on navigating the generated code, catching exceptions and viewing strings, etc. So, I spent a bit of time stepping through the call stack and I wouldn’t recommend it as an experience as it is fairly verbose and easy to get lost. I then turned on first chance exceptions and discovered where the exception was being thrown.

<a href="{{ site.baseurl }}/assets/images/2019/01/exception1.png"><img style="display: inline; background-image: none;" title="exception1" src="{{ site.baseurl }}/assets/images/2019/01/exception1_thumb.png" alt="exception1" width="670" height="229" border="0" /></a> and stepping a few frames back up the call stack reveals the following:

<a href="{{ site.baseurl }}/assets/images/2019/01/exception2.png"><img style="display: inline; background-image: none;" title="exception2" src="{{ site.baseurl }}/assets/images/2019/01/exception2_thumb.png" alt="exception2" width="670" height="304" border="0" /></a>

Now, this is C++ code generated from System.Runtime.Serialization and more specifically this function System.Runtime.Serialization.Json.JsonFormatWriterInterpreter::TryWritePrimitive and it is using reflection itself to find a method that will be used in the implementation. Of course, that method has been stripped out and so this code fails. Adding an entry to the link.xml file for System.Runtime.Serialization fixes this but leads me to think that as a consumer of this code I shouldn’t really be concerned with it’s internals in this way.

<script src="https://gist.github.com/peted70/fa134d7cc0513b7103d3bdfd448109ea.js"></script>And finally I have a working sample which you can find <a href="https://github.com/peted70/msal-hololens" target="_blank" rel="noopener">here</a>.
