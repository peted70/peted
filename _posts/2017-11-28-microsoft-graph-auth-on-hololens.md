---
layout: post
title: Microsoft Graph Auth on HoloLens
date: 2017-11-28 18:06
author: peted70
comments: true
categories: [HoloLens, HoloLens, mrtk, msgraph, msgraph, oauth2, oauth2, unity, unity3d]
---
<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_165052_HoloLens.jpg"><img width="740" height="418" title="20171128_165052_HoloLens" style="display: inline; background-image: none;" alt="20171128_165052_HoloLens" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_165052_HoloLens_thumb.jpg" border="0"></a><p>So, I guessed that sooner or later I'm going to want to need to access Microsoft Graph APIs from a HoloLens and I was writing some similar code for a different environment I thought I may as well combine the two and write it up. I have a few examples of using OAuth2 on this blog over the years with the most recent using ADAL (Azure Active Directory Auth Libraries) which comes in lots of different forms (see <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-authentication-libraries" target="_blank">https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-authentication-libraries</a>). These support Azure AD v1.0 but I'm much more interested in Azure AD v2.0 libraries Microsoft Authentication Library (MSAL) - <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-libraries" target="_blank">https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-libraries</a> . An advantage using Azure AD v2.0 is that we can log in with either a personal Microsoft Account or an Organization account and have an API respond by detecting which one we are using and return the appropriate data. This makes the code I need to write to access my cloud file storage the same for both account types and make it all much simpler. There are additional advantages such as better standardisation and dynamic scopes but I will leave that to the docs to explain. Instead I'll just talk through how I used the MSAL in a HoloLens app.<h2>Register the App</h2><p>First go to <a href="https://apps.dev.microsoft.com/#/appList" target="_blank">https://apps.dev.microsoft.com/#/appList</a> where you can register a 'converged' app (one that uses both account types). All I did for this one was register a new app, add a platform which I chose as 'Native Application' and made a note of the Application Id.<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/apps.dev_.png"><img width="747" height="410" title="apps.dev" style="display: inline; background-image: none;" alt="apps.dev" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/apps.dev_thumb.png" border="0"></a><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/appsettings.png"><img width="741" height="794" title="appsettings" style="display: inline; background-image: none;" alt="appsettings" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/appsettings_thumb.png" border="0"></a><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/native.png"><img width="743" height="318" title="native" style="display: inline; background-image: none;" alt="native" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/native_thumb.png" border="0"></a><h2>MSAL</h2><p>The MSAL library comes in the form of a Nuget package and since I am using Unity to author my HoloLens app I will set up a couple of things:<blockquote><p>- I created a C# Holographic App using the Visual Studio templates you get when you download the HoloLens emulator and added the Nuget package to it. I built the app and looked in the output folder to get the right MSAL binaries for a UWP app<p>- I copied these into my Unity project inside a Plugins folder and I configured the Inspector properties of the binary to 'not process' and just to work for WSA.<p>(I also surrounded all referencing code with a #if !UNITY_EDITOR &amp;&amp; UNITY_WSA #endif so it would only be compiled for the UWP app and not inside the editor).</p></blockquote><p>I made a 'note to self' that if this was a real project I might put this behind an interface and create a mock implementation for the editor.<p>I also configured my Unity project to use Experimental .NET 4.6 support (see <a href="http://peted.azurewebsites.net/holograms-catalogueto-the-cloud/" target="_blank">http://peted.azurewebsites.net/holograms-catalogueto-the-cloud/</a> for more details). This makes it possible to use the MSAL library and also some nice programming features such as async/await.<p>Some simple code to interface with the MSAL library would look like this:<p><script src="https://gist.github.com/peted70/468c6c35292a44aa160e3111f885536c.js"></script>Whilst this will get us quickly up and running it is somewhat incomplete. If we call something like this each time we run our app the user will need to authenticate each time which could involve multi-factor auth and whilst this could get tiring on any app typing characters on a HoloLens will get 'old' quickly. The MSAL helps us out with a couple of things; first, it keeps a User cache with tokens and also it will automate the process of getting a refresh token when our access token expires.<blockquote><p>Note, if you don't know these concepts all is revealed in the OAuth2 specification which you can see here <a href="https://oauth.net/2/" target="_blank">https://oauth.net/2/</a></p></blockquote><p>In general, to take advantage of these we can have our app persist a user id string which we can use to pass a user back to the MSAL library. Also, we can use the AcquireTokenSilentAsync library call which will attempt to get us back an access token without making any network calls. We can fall back to AcquireTokenAsync if this method fails which will have the user authenticate again. Once we have an access token we can unlock the Microsoft Graph API.<h2>Microsoft Graph</h2><p>For this simple sample I will just use the Microsoft Graph (<a href="http://graph.microsoft.com/" target="_blank">http://graph.microsoft.com/</a>) to retrieve some email but of course we could use similar code to do quite a few other things: discover the user profile and a graph of colleagues/contacts, discover and work with files in the cloud, calendars, OneNote, etc. Here's what the basic code looks like using HttpClient:<p><script src="https://gist.github.com/peted70/83ac2b627ee0f948f0e6fd5504409c29.js"></script>I then use JsonUtility (<a href="https://docs.unity3d.com/ScriptReference/JsonUtility.html" target="_blank">https://docs.unity3d.com/ScriptReference/JsonUtility.html</a>) to deserialize the response string. I facilitated this by logging the response string to the console, copying it to the clipboard and using Visual Studio's Paste Special &gt; Paste as Json to create some C# classes. I also found that JsonUtility doesn't like properties so I converted all of those to fields, set a Serializable attribute on each class and the the following worked:<blockquote><p>var email = JsonUtility.FromJson&lt;Rootobject&gt;(respStr); </p></blockquote><h2>Unity HoloLens App</h2><p>The final thing was how I put this into a HoloLens app. So, first I used the Mixed Reality Toolkit (<a href="https://github.com/Microsoft/MixedRealityToolkit-Unity" target="_blank">https://github.com/Microsoft/MixedRealityToolkit-Unity</a>) and used it to configure my project for HoloLens. Then I decided to use speech command to sign in and out (the easiest option to hook up). <blockquote><p>To use speech recognition with the MRTK just have your MonoBehaviour implement the ISpeechHandler interface – mine looks like this:</p></blockquote><script src="https://gist.github.com/peted70/1c5df7389f3f534c0e490ff06e819305.js"></script><p>Then just used 3D Text Meshes in my scene to display the output. In order to support the OAuth2 Authorization Code flow it is common to use a browser or embedded browser window to facilitate the redirects and communication involved. I'm not sure if there is a browser control I can use in a Unity app for this purpose, I guess there is but I used the option of making my app a UWP XAML app which enables me to get redirected to the 2D browser to facilitate the OAuth flow and I get returned to my 3D app when it is complete. I used a lot of logging as for some reason Visual Studio won't let me debug the MRTK at the moment. Here's a flow through the app:<blockquote><p>- user uses app for the first time<p>- user says 'Sign in'<p>- the flow starts and the user is prompted for their credentials<p>- once authenticated we get returned to the 3D app <p>- a call is made to the Graph API to get the top 5 email messages<p>- these are shown in a TextMesh</p></blockquote><p>c<a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171111_HoloLens.jpg"><img width="723" height="408" title="20171128_171111_HoloLens" style="display: inline; background-image: none;" alt="20171128_171111_HoloLens" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171111_HoloLens_thumb.jpg" border="0"></a></p><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171117_HoloLens.jpg"><img width="730" height="412" title="20171128_171117_HoloLens" style="display: inline; background-image: none;" alt="20171128_171117_HoloLens" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171117_HoloLens_thumb.jpg" border="0"></a></p><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171129_HoloLens.jpg"><img width="738" height="417" title="20171128_171129_HoloLens" style="display: inline; background-image: none;" alt="20171128_171129_HoloLens" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171129_HoloLens_thumb.jpg" border="0"></a></p><p><a href="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171146_HoloLens.jpg"><img width="746" height="421" title="20171128_171146_HoloLens" style="display: inline; background-image: none;" alt="20171128_171146_HoloLens" src="http://peted.azurewebsites.net/wp-content/uploads/2017/11/20171128_171146_HoloLens_thumb.jpg" border="0"></a></p><p>The code repo is here <a href="https://github.com/peted70/msal-hololens">https://github.com/peted70/msal-hololens</a>