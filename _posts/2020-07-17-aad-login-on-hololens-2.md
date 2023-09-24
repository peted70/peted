---
layout: post
title: AAD Login on HoloLens 2
date: 2020-07-17 17:50
author: peted70
comments: true
categories: [aad, Azure, HoloLens, HoloLens, login, mixedreality, oauth2, oauth2, Uncategorized]
---
<!-- wp:image {"id":8416,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/eye.png" alt="" class="wp-image-8416"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Here's a short video of the sample app that goes with this post:"} -->
<div class="wp-block-jetpack-markdown"><p>Here's a short video of the sample app that goes with this post:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:core-embed/youtube {"url":"https://www.youtube.com/watch?v=7yz7hTNMxBY","type":"video","providerNameSlug":"youtube","className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://www.youtube.com/watch?v=7yz7hTNMxBY
</div><figcaption>Video of the sample</figcaption></figure>
<!-- /wp:core-embed/youtube -->

<!-- wp:jetpack/markdown {"source":"The code is all found on Github [here](https://github.com/peted70/aad-hololens)\n\nOne of the coolest aspects of HoloLens2 is the iris login as it removes the need for typing on a virtual keyboard. So a cool feature which avoids the need for the coolest feature! Although hand-tracked typing is arguably the coolest feature it can become tiring when needed over and over...\n\nSo set up your HoloLens2 device using AAD credentials and then configure the device to use Iris login as shown below:\n"} -->
<div class="wp-block-jetpack-markdown"><p>The code is all found on Github <a href="https://github.com/peted70/aad-hololens">here</a></p>
<p>One of the coolest aspects of HoloLens2 is the iris login as it removes the need for typing on a virtual keyboard. So a cool feature which avoids the need for the coolest feature! Although hand-tracked typing is arguably the coolest feature it can become tiring when needed over and over...</p>
<p>So set up your HoloLens2 device using AAD credentials and then configure the device to use Iris login as shown below:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8419,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/hololens-iris-settings-.png" alt="" class="wp-image-8419"/><figcaption>HoloLens2 Iris Login Settings</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"An app to configure will run and guide you to watch a moving animation."} -->
<div class="wp-block-jetpack-markdown"><p>An app to configure will run and guide you to watch a moving animation.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8427,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/scan-complete-.png" alt="" class="wp-image-8427"/><figcaption>Scan Complete</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"\u003e You can now add up to 64 other users logging in via AAD and have the device be shared by signing in an out. We'll see how this can be extended into an app later in the article.\n\n\nI have previously written a little about the topic of retrieving Azure Active Directory (AAD) OAuth2 identity and access tokens on a HoloLens device.\n\n\u003e If this topic is new to you I would suggest reading through the identity platform overview [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-overview)\n\nI focused on specific topics such as how to retrieve a token to access the Microsoft Graph API using Microsoft Authentication Library for .NET (MSAL) using the OAuth delegated flow and device code flows. You can see code and read about those [here](http://peted.azurewebsites.net/microsoft-graph-auth-on-hololens/) and [here](http://peted.azurewebsites.net/microsoft-graph-auth-on-hololens-device-code-flow/). There are some complications and potential blockers for those trying to set this up for the first time and, in my experience there is quite a lot of misunderstanding around the topics of auth in general. In my role at Microsoft I have worked through some of these issues for customers and I get asked a lot about how to set up auth correctly to access Graph APIs, your own APIs and Mixed Reality services. Often samples for services will skirt the issue by using secrets embedded into a client side application which you can configure with your own set of secrets for your service instances. This is fine for a demo and a first-try of the services in question but as soon as you turn your thoughts to developing production code falls down immediately and the first task you will be faced with is how to secure access to your services. I am going to cover the OAuth 2.0 Authorization Code Grant which you can read about here [OAuth Grant Types](https://oauth.net/2/grant-types/authorization-code/) if you want to understand the details of the flow.\n\n\u003e In brief, the scenario I am talking about is when an end user provides permissions for an app to access services that the user has access to on their behalf. The end user does this by authenticating and consenting to a set of permissions known as scopes. The consented scopes are encapsulated within the access token itself.\n\nI won't cover, for instance Client Credentials flow which is used in an app to service scenario not involving a user.\n\nSo, it seems pretty simple so far so why the need for this post? These are the complications that I associate with this:\n\n- Which APIs or framework should I use?\n\n- The acquisition of an OAuth token requires a browser doesn't it? So, how can I do this in Unity without buying a browser plugin?\n\n- How do I configure my Azure Active Directory to access services and use role-based access control?\n\n- I'm already logged into the HoloLens can I not just access the token already on the device?\n\nI won't focus on some of the other topics like IL2CPP stripping code from .NET libraries as I have covered that in a previous post [IL2CPP + HoloLens](http://peted.azurewebsites.net/il2cpp-hololens/), so you may want to be on the lookout for those kind of issues. I will leave you with a code repository that you can borrow code from to set up the particular scenario that you are interested in. The code and the rest of this post will be concerned with the different ways that access and id tokens can be retrieved, back-end configuration and an illustration of how you might get an AAD token for some of the new Mixed Reality services such as Azure Spatial Anchors and Azure Remote Rendering.\n\n## Sample Walkthrough\n\n\u003e This just describes some aspects of how I created the demo application. If this isn't of interest just skip these sections as I will make my way towards the various frameworks and APIs used.\n\nFor the sample I have used Unity 2019.4.0f1.\nFirst add Nuget 2.0.0 to your project.\nThen, add MRTK v2.4.0 to your project using nuget. It looks like nuget support has been dropped for MRTK v2.4.0 so download the packages and import into your project.\n"} -->
<div class="wp-block-jetpack-markdown"><blockquote>
<p>You can now add up to 64 other users logging in via AAD and have the device be shared by signing in an out. We'll see how this can be extended into an app later in the article.</p>
</blockquote>
<p>I have previously written a little about the topic of retrieving Azure Active Directory (AAD) OAuth2 identity and access tokens on a HoloLens device.</p>
<blockquote>
<p>If this topic is new to you I would suggest reading through the identity platform overview <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-overview">here</a></p>
</blockquote>
<p>I focused on specific topics such as how to retrieve a token to access the Microsoft Graph API using Microsoft Authentication Library for .NET (MSAL) using the OAuth delegated flow and device code flows. You can see code and read about those <a href="http://peted.azurewebsites.net/microsoft-graph-auth-on-hololens/">here</a> and <a href="http://peted.azurewebsites.net/microsoft-graph-auth-on-hololens-device-code-flow/">here</a>. There are some complications and potential blockers for those trying to set this up for the first time and, in my experience there is quite a lot of misunderstanding around the topics of auth in general. In my role at Microsoft I have worked through some of these issues for customers and I get asked a lot about how to set up auth correctly to access Graph APIs, your own APIs and Mixed Reality services. Often samples for services will skirt the issue by using secrets embedded into a client side application which you can configure with your own set of secrets for your service instances. This is fine for a demo and a first-try of the services in question but as soon as you turn your thoughts to developing production code falls down immediately and the first task you will be faced with is how to secure access to your services. I am going to cover the OAuth 2.0 Authorization Code Grant which you can read about here <a href="https://oauth.net/2/grant-types/authorization-code/">OAuth Grant Types</a> if you want to understand the details of the flow.</p>
<blockquote>
<p>In brief, the scenario I am talking about is when an end user provides permissions for an app to access services that the user has access to on their behalf. The end user does this by authenticating and consenting to a set of permissions known as scopes. The consented scopes are encapsulated within the access token itself.</p>
</blockquote>
<p>I won't cover, for instance Client Credentials flow which is used in an app to service scenario not involving a user.</p>
<p>So, it seems pretty simple so far so why the need for this post? These are the complications that I associate with this:</p>
<ul>
<li>
<p>Which APIs or framework should I use?</p>
</li>
<li>
<p>The acquisition of an OAuth token requires a browser doesn't it? So, how can I do this in Unity without buying a browser plugin?</p>
</li>
<li>
<p>How do I configure my Azure Active Directory to access services and use role-based access control?</p>
</li>
<li>
<p>I'm already logged into the HoloLens can I not just access the token already on the device?</p>
</li>
</ul>
<p>I won't focus on some of the other topics like IL2CPP stripping code from .NET libraries as I have covered that in a previous post <a href="http://peted.azurewebsites.net/il2cpp-hololens/">IL2CPP + HoloLens</a>, so you may want to be on the lookout for those kind of issues. I will leave you with a code repository that you can borrow code from to set up the particular scenario that you are interested in. The code and the rest of this post will be concerned with the different ways that access and id tokens can be retrieved, back-end configuration and an illustration of how you might get an AAD token for some of the new Mixed Reality services such as Azure Spatial Anchors and Azure Remote Rendering.</p>
<h2>Sample Walkthrough</h2>
<blockquote>
<p>This just describes some aspects of how I created the demo application. If this isn't of interest just skip these sections as I will make my way towards the various frameworks and APIs used.</p>
</blockquote>
<p>For the sample I have used Unity 2019.4.0f1.
First add Nuget 2.0.0 to your project.
Then, add MRTK v2.4.0 to your project using nuget. It looks like nuget support has been dropped for MRTK v2.4.0 so download the packages and import into your project.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8412,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/buildsettings-.png" alt="" class="wp-image-8412"/><figcaption>Build Settings</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Set app package name and capabilities to allow debugging on device via the unity debugging tools so we can connect using the managed debugger\n\u003eI have trained myself to set the package name to avoid clashes with other apps I am developing"} -->
<div class="wp-block-jetpack-markdown"><p>Set app package name and capabilities to allow debugging on device via the unity debugging tools so we can connect using the managed debugger</p>
<blockquote>
<p>I have trained myself to set the package name to avoid clashes with other apps I am developing</p>
</blockquote>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8442,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/configure-1-.png" alt="" class="wp-image-8442"/><figcaption>Configure</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":8434,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/xrplugin-management-.png" alt="" class="wp-image-8434"/><figcaption>XR Plugin Management</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"This also involves removing any legacy XR packages using the Unity package manager, if necessary..\n\nadd MRTK to the scene"} -->
<div class="wp-block-jetpack-markdown"><p>This also involves removing any legacy XR packages using the Unity package manager, if necessary..</p>
<p>add MRTK to the scene</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8436,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/add-to-scene-1-.png" alt="" class="wp-image-8436"/><figcaption>Add MRTK</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Turn off spatial awareness by cloning the MRTK configuration and turning Enable Spatial Awareness off.\nAdd a near menu prefab to your scene, empty it's contents (you will need to unpack the prefab) and resize the remaining items so that the backplate is bigger, the visual cues are re-positioned and the pin is re-positioned."} -->
<div class="wp-block-jetpack-markdown"><p>Turn off spatial awareness by cloning the MRTK configuration and turning Enable Spatial Awareness off.
Add a near menu prefab to your scene, empty it's contents (you will need to unpack the prefab) and resize the remaining items so that the backplate is bigger, the visual cues are re-positioned and the pin is re-positioned.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8444,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/empty-near-menu-1-.png" alt="" class="wp-image-8444"/><figcaption>Near Menu</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Find the RadialSet prefab in the MRTK and drag an instance under the near menu game object,\nadd a GridObjectCollection component to the RadialSet and add a toggle button for each option we want to represent (one for each login method).\n\nSearch the project panel for toggle to find the PressableButtonHoloLens2Toggle and drag one under the RadialSet.\n\nLayout the toggle buttons"} -->
<div class="wp-block-jetpack-markdown"><p>Find the RadialSet prefab in the MRTK and drag an instance under the near menu game object,
add a GridObjectCollection component to the RadialSet and add a toggle button for each option we want to represent (one for each login method).</p>
<p>Search the project panel for toggle to find the PressableButtonHoloLens2Toggle and drag one under the RadialSet.</p>
<p>Layout the toggle buttons</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8430,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/toggle-layout.png" alt="" class="wp-image-8430"/><figcaption>Layout Toggles</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Adjust the backplate and grip visuals to arrive at:"} -->
<div class="wp-block-jetpack-markdown"><p>Adjust the backplate and grip visuals to arrive at:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8431,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/toggles.png" alt="" class="wp-image-8431"/><figcaption>Toggles</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Rename and configure toggle buttons in the InteractableToggleCollection component on the RadialSet"} -->
<div class="wp-block-jetpack-markdown"><p>Rename and configure toggle buttons in the InteractableToggleCollection component on the RadialSet</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8422,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/interactabletogglecollection-.png" alt="" class="wp-image-8422"/><figcaption>Toggle Collection</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Then we are going to make this into a tabbed dialog by adding a parent node to the RadialSet (I called this TabbedDialog) and then adding a script to that node. Here's a snippet from TabbedDialog.cs showing how the content visibility is synchronised with the tab selection."} -->
<div class="wp-block-jetpack-markdown"><p>Then we are going to make this into a tabbed dialog by adding a parent node to the RadialSet (I called this TabbedDialog) and then adding a script to that node. Here's a snippet from TabbedDialog.cs showing how the content visibility is synchronised with the tab selection.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>    private void OnSelectedChanged()
    {
        // Retrieve the current Index here...
        var newId = SyncPanelToCurrentIndex();
        if (!string.IsNullOrEmpty(newId))
            SelectedChanged.Invoke(newId);
    }

    private void OnEnable()
    {
        if (_selectionChanged == null)
            _selectionChanged = OnSelectedChanged;

        tabs.OnSelectionEvents.AddListener(_selectionChanged);
        SyncPanelToCurrentIndex();
    }

    string SyncPanelToCurrentIndex()
    {
        int idx = tabs.CurrentIndex;
        if (idx &lt; 0 &amp;&amp; idx >= Panels.Length)
            return string.Empty;

        foreach (var panel in Panels)
        {
            panel.SetActive(false);
        }

        Panels&#91;idx].SetActive(true);
        return Panels&#91;idx].GetComponent&lt;ProviderId>().Id;
    }</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"The content pages are arbitrary game objects and are given an ID by adding the ProviderId script. This way the ID can be passed around to switch the content page and also the Identity provider."} -->
<div class="wp-block-jetpack-markdown"><p>The content pages are arbitrary game objects and are given an ID by adding the ProviderId script. This way the ID can be passed around to switch the content page and also the Identity provider.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8443,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/content-pages-1.png" alt="" class="wp-image-8443"/><figcaption>Content Pages</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"The video below shows the tabbed dialog being used to switch the content pages in the Unity Game preview."} -->
<div class="wp-block-jetpack-markdown"><p>The video below shows the tabbed dialog being used to switch the content pages in the Unity Game preview.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:core-embed/youtube {"url":"https://www.youtube.com/watch?v=cOxb61Tk1hM","type":"video","providerNameSlug":"youtube","className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed-youtube wp-block-embed is-type-video is-provider-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://www.youtube.com/watch?v=cOxb61Tk1hM
</div></figure>
<!-- /wp:core-embed/youtube -->

<!-- wp:jetpack/markdown {"source":"We can use this UI to plug in different auth providers and switch them at runtime.\n\n## APIs and Frameworks\n\n### Web Authentication Manager\n\nHere we can access the APIs which enable you to retrieve the current user's AAD token and also to invoke a biometric check against the current physical user. this would guard against another user picking up a device and accessing sensitive information. You can switch the iris check on/off in the sample with a toggle switch."} -->
<div class="wp-block-jetpack-markdown"><p>We can use this UI to plug in different auth providers and switch them at runtime.</p>
<h2>APIs and Frameworks</h2>
<h3>Web Authentication Manager</h3>
<p>Here we can access the APIs which enable you to retrieve the current user's AAD token and also to invoke a biometric check against the current physical user. this would guard against another user picking up a device and accessing sensitive information. You can switch the iris check on/off in the sample with a toggle switch.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8433,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/use-iris-login.png" alt="" class="wp-image-8433"/><figcaption>Use Iris Login</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"When this is selected and you sign in your iris will be re-scanned and you will see this dialog."} -->
<div class="wp-block-jetpack-markdown"><p>When this is selected and you sign in your iris will be re-scanned and you will see this dialog.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8424,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/iris-check-.png" alt="" class="wp-image-8424"/><figcaption>Iris Check</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"With the switch toggled to the 'off' position you go through a flow that acquires the currently logged in user's AAD token so you won't get a prompt to login. This way you can share a HL2 device amongst multiple people and ensure that if one user forgets to sign out they will not leak sensitive data. More information about setting up a device for multiple users can be found [here](https://docs.microsoft.com/en-us/hololens/hololens-multiple-users).\n\n### Windows Account Provider\n\nI have experimented here as I found a scenario where I wanted to switch between different known accounts but I couldn't get the account settings pane to show so haven't completed this part but may return to it later.\n\n### Microsoft Authentication Library\n\nAside from the built-in APIs this would be the next favourite method of gaining an AAD token and that would be the suggestion of the Microsoft Identity team. Plus it supports device code flow which is sometimes handy depending on your scenario. Also it allows you to continue to develop your app against real APIs from the Unity editor.\n\n### Active Directory Authentication Library\n\nWhen we first try to use ADAL we get hit with the following error:\n\n\u003e Error on deserializing read-only members in the class: No set method for property 'TenantDiscoveryEndpoint' in type 'Microsoft.IdentityModel.Clients.ActiveDirectory.InstanceDiscoveryResponse'.\n\nThis looks suspiciously like an IL2CPP code stripping issue. This occurs as byproduct of the optimisation used when the .NET binary is converted to C++. Unused code is stripped out in that process and sometimes code that isn't detected as reachable gets removed. Look out for code that is only referenced in a reflection scenario. See [here](http://peted.azurewebsites.net/il2cpp-hololens/) for further details. The short answer is to add a link.xml stating which types to not strip.\n\n\u003e You can see the link.xml for the app [here](https://github.com/peted70/aad-hololens/blob/master/Assets/link.xml)\n\nAlthough ADAL is not the recommended way forward for legacy and cross-platform reasons you may need to use it and so I have included it in my sample.\n\n\u003e If you are using ADAL but would like to migrate to MSAL then the differences are distilled in [this](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Adal-to-Msal) guide.\n\n### Web Authentication Broker\n\n[WebAuthenticationBroker](https://docs.microsoft.com/en-us/windows/uwp/security/web-authentication-broker) is a broker component that manages the http redirects retrieving the resulting AAD token that would usually be managed by a browser for a web application. As a result there is a bit more work to do to construct the original url than the other libraries.\n\n## Settings\n\nThe settings for auth are stored in a json file in the Assets folder"} -->
<div class="wp-block-jetpack-markdown"><p>With the switch toggled to the 'off' position you go through a flow that acquires the currently logged in user's AAD token so you won't get a prompt to login. This way you can share a HL2 device amongst multiple people and ensure that if one user forgets to sign out they will not leak sensitive data. More information about setting up a device for multiple users can be found <a href="https://docs.microsoft.com/en-us/hololens/hololens-multiple-users">here</a>.</p>
<h3>Windows Account Provider</h3>
<p>I have experimented here as I found a scenario where I wanted to switch between different known accounts but I couldn't get the account settings pane to show so haven't completed this part but may return to it later.</p>
<h3>Microsoft Authentication Library</h3>
<p>Aside from the built-in APIs this would be the next favourite method of gaining an AAD token and that would be the suggestion of the Microsoft Identity team. Plus it supports device code flow which is sometimes handy depending on your scenario. Also it allows you to continue to develop your app against real APIs from the Unity editor.</p>
<h3>Active Directory Authentication Library</h3>
<p>When we first try to use ADAL we get hit with the following error:</p>
<blockquote>
<p>Error on deserializing read-only members in the class: No set method for property 'TenantDiscoveryEndpoint' in type 'Microsoft.IdentityModel.Clients.ActiveDirectory.InstanceDiscoveryResponse'.</p>
</blockquote>
<p>This looks suspiciously like an IL2CPP code stripping issue. This occurs as byproduct of the optimisation used when the .NET binary is converted to C++. Unused code is stripped out in that process and sometimes code that isn't detected as reachable gets removed. Look out for code that is only referenced in a reflection scenario. See <a href="http://peted.azurewebsites.net/il2cpp-hololens/">here</a> for further details. The short answer is to add a link.xml stating which types to not strip.</p>
<blockquote>
<p>You can see the link.xml for the app <a href="https://github.com/peted70/aad-hololens/blob/master/Assets/link.xml">here</a></p>
</blockquote>
<p>Although ADAL is not the recommended way forward for legacy and cross-platform reasons you may need to use it and so I have included it in my sample.</p>
<blockquote>
<p>If you are using ADAL but would like to migrate to MSAL then the differences are distilled in <a href="https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Adal-to-Msal">this</a> guide.</p>
</blockquote>
<h3>Web Authentication Broker</h3>
<p><a href="https://docs.microsoft.com/en-us/windows/uwp/security/web-authentication-broker">WebAuthenticationBroker</a> is a broker component that manages the http redirects retrieving the resulting AAD token that would usually be managed by a browser for a web application. As a result there is a bit more work to do to construct the original url than the other libraries.</p>
<h2>Settings</h2>
<p>The settings for auth are stored in a json file in the Assets folder</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8428,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/settings-.png" alt="" class="wp-image-8428"/><figcaption>App Settings</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"So edit this file with your own settings and rename it to app-settings.json and the sample code will pick it up.\n\n## Configure the Backend\n\nIt is worth scanning the docs [here](https://docs.microsoft.com/en-us/hololens/hololens-identity) and although we can see from those docs that you can login to your HL2 with a local account or an MSA things get more interesting when we consider Azure Active Directory because at the time of writing it is possible to have 64 AAD user accounts with the following options:\n\n- Azure web credential provider\n- Azure Authenticator App\n- Biometric (Iris) – HoloLens 2 only\n- PIN – Optional for HoloLens (1st gen)required for HoloLens 2\n- Password\n\nOf most interest to me being iris login with multiple accounts.\n\n\u003e Just getting an OAuth token and displaying it isn't too interesting as really the aim in a real app would most-likely be to access a custom API or some other Azure resource. So, to show that things are working as expected we'll take the AAD token and exchange it for and access token for Azure Mixed Reality services. We'll use that access token to make a call to the Azure Remote Rendering Service to enumerate rendering sessions just to check that the call succeeds. This access token could be used to access any MR services, e.g. Azure Spatial Anchors, that the logged in user has access to. We'll look into how to configure the application and also provide access to a particular user next.\n\n### Test\n\nI added a button to run the test which calls the ARR service and shows the results of whether the call succeeded or not.\n\n\u003e After getting an AAD token you can press the button which will exchange the AAD token for an MR access token using the Mixed Reality Secure Token Service (STS). This access token will subsequently be in a call to the ARR service. The test UI will show green if the http status is successful and red otherwise.\n\n### Register the App\n\nI've covered this a few times on my blog so will just include the basic steps here:\n\nIn the [Azure portal](https://portal.azure.com) if you don't have an AAD set up there is a quickstart [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-create-new-tenant) to help get you up and running. Once created you can register a new application:"} -->
<div class="wp-block-jetpack-markdown"><p>So edit this file with your own settings and rename it to app-settings.json and the sample code will pick it up.</p>
<h2>Configure the Backend</h2>
<p>It is worth scanning the docs <a href="https://docs.microsoft.com/en-us/hololens/hololens-identity">here</a> and although we can see from those docs that you can login to your HL2 with a local account or an MSA things get more interesting when we consider Azure Active Directory because at the time of writing it is possible to have 64 AAD user accounts with the following options:</p>
<ul>
<li>Azure web credential provider</li>
<li>Azure Authenticator App</li>
<li>Biometric (Iris) – HoloLens 2 only</li>
<li>PIN – Optional for HoloLens (1st gen)required for HoloLens 2</li>
<li>Password</li>
</ul>
<p>Of most interest to me being iris login with multiple accounts.</p>
<blockquote>
<p>Just getting an OAuth token and displaying it isn't too interesting as really the aim in a real app would most-likely be to access a custom API or some other Azure resource. So, to show that things are working as expected we'll take the AAD token and exchange it for and access token for Azure Mixed Reality services. We'll use that access token to make a call to the Azure Remote Rendering Service to enumerate rendering sessions just to check that the call succeeds. This access token could be used to access any MR services, e.g. Azure Spatial Anchors, that the logged in user has access to. We'll look into how to configure the application and also provide access to a particular user next.</p>
</blockquote>
<h3>Test</h3>
<p>I added a button to run the test which calls the ARR service and shows the results of whether the call succeeded or not.</p>
<blockquote>
<p>After getting an AAD token you can press the button which will exchange the AAD token for an MR access token using the Mixed Reality Secure Token Service (STS). This access token will subsequently be in a call to the ARR service. The test UI will show green if the http status is successful and red otherwise.</p>
</blockquote>
<h3>Register the App</h3>
<p>I've covered this a few times on my blog so will just include the basic steps here:</p>
<p>In the <a href="https://portal.azure.com">Azure portal</a> if you don't have an AAD set up there is a quickstart <a href="https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-create-new-tenant">here</a> to help get you up and running. Once created you can register a new application:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8438,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/app-reg-1-.png" alt="" class="wp-image-8438"/><figcaption>App Registration</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Select 'New Registration' and just name it without worrying about the other settings. You will be shown a page which has settings that you will need for the app-settings.json file we mentioned above (maybe fill those now). The other important thing is to register a redirect uri for your app.\n\n### Redirect URI\n\nThe easiest way to discover the redirect URI for the sample app is to run it and the app will write the uri to th unity log.\n\n\u003e Note, that the running instance of your app needs to be terminated before the log is flushed. You can terminate the app and download the unity log using the [HoloLens device portal](https://docs.microsoft.com/en-us/windows/mixed-reality/using-the-windows-device-portal)."} -->
<div class="wp-block-jetpack-markdown"><p>Select 'New Registration' and just name it without worrying about the other settings. You will be shown a page which has settings that you will need for the app-settings.json file we mentioned above (maybe fill those now). The other important thing is to register a redirect uri for your app.</p>
<h3>Redirect URI</h3>
<p>The easiest way to discover the redirect URI for the sample app is to run it and the app will write the uri to th unity log.</p>
<blockquote>
<p>Note, that the running instance of your app needs to be terminated before the log is flushed. You can terminate the app and download the unity log using the <a href="https://docs.microsoft.com/en-us/windows/mixed-reality/using-the-windows-device-portal">HoloLens device portal</a>.</p>
</blockquote>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8432,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/unity-log-.png" alt="" class="wp-image-8432"/><figcaption>Unity Log</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"This code:"} -->
<div class="wp-block-jetpack-markdown"><p>This code:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>string URI = string.Format("ms-appx-web://Microsoft.AAD.BrokerPlugIn/{0}", WebAuthenticationBroker.GetCurrentApplicationCallbackUri().Host.ToUpper());
Logger.Log("Redirect URI: " + URI);</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"will log out a redirect URI for a UWP app.\n\n\u003e If I have this right the ADAL implementation required a slightly different format so I registered both:"} -->
<div class="wp-block-jetpack-markdown"><p>will log out a redirect URI for a UWP app.</p>
<blockquote>
<p>If I have this right the ADAL implementation required a slightly different format so I registered both:</p>
</blockquote>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>string URI = string.Format("ms-app://{0}", WebAuthenticationBroker.GetCurrentApplicationCallbackUri().Host.ToUpper());
Logger.Log("Redirect URI: " + URI);</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"To configure access for a user navigate to the Mixed Reality service you want to grant access to"} -->
<div class="wp-block-jetpack-markdown"><p>To configure access for a user navigate to the Mixed Reality service you want to grant access to</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8440,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/arr-settings-1-.png" alt="" class="wp-image-8440"/><figcaption>ARR Settings</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"More info [here](https://docs.microsoft.com/en-us/azure/remote-rendering/how-tos/create-an-account) along with instructions on how to set up an Azure Remote Rendering account. I've set up my test user as an owner."} -->
<div class="wp-block-jetpack-markdown"><p>More info <a href="https://docs.microsoft.com/en-us/azure/remote-rendering/how-tos/create-an-account">here</a> along with instructions on how to set up an Azure Remote Rendering account. I've set up my test user as an owner.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8439,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/arr-access-1-.png" alt="" class="wp-image-8439"/><figcaption>ARR Access</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Along with this I also need to give my app the necessary API permissions to access the Mixed Reality services. I can navigate to my app registration page on the Azure portal."} -->
<div class="wp-block-jetpack-markdown"><p>Along with this I also need to give my app the necessary API permissions to access the Mixed Reality services. I can navigate to my app registration page on the Azure portal.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8437,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/api-permissions-1-.png" alt="" class="wp-image-8437"/><figcaption>API Permissions</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"and I can request a permission by searching for Microsoft Mixed..."} -->
<div class="wp-block-jetpack-markdown"><p>and I can request a permission by searching for Microsoft Mixed...</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8426,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/request-permission-.png" alt="" class="wp-image-8426"/><figcaption>Request Permission</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Selecting that and choosing 'Delegated Permission'..."} -->
<div class="wp-block-jetpack-markdown"><p>Selecting that and choosing 'Delegated Permission'...</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8435,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/add-permission-1-.png" alt="" class="wp-image-8435"/><figcaption>Add Permission</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"and choose **mixedreality.signin** permission.\n\n\u003e This will also be the scope that you will request for your AAD token.\n\nAnd finally I granted access to my tenant:"} -->
<div class="wp-block-jetpack-markdown"><p>and choose <strong>mixedreality.signin</strong> permission.</p>
<blockquote>
<p>This will also be the scope that you will request for your AAD token.</p>
</blockquote>
<p>And finally I granted access to my tenant:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8420,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/grant-admin-1-.png" alt="" class="wp-image-8420"/><figcaption>Grant Admin</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"## Diagnosing Token Issues\n\nIf at some point you get stuck on some AAD error then having an understanding of the OAuth flow that you are using is vital and you can also use [https://jwt.ms/](https://jwt.ms/) to paste in a token and have a look at it's contents/claims to ensure they match with what you are expecting."} -->
<div class="wp-block-jetpack-markdown"><h2>Diagnosing Token Issues</h2>
<p>If at some point you get stuck on some AAD error then having an understanding of the OAuth flow that you are using is vital and you can also use <a href="https://jwt.ms/">https://jwt.ms/</a> to paste in a token and have a look at it's contents/claims to ensure they match with what you are expecting.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8425,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/07/jwt-.png" alt="" class="wp-image-8425"/><figcaption>jwt token</figcaption></figure>
<!-- /wp:image -->
