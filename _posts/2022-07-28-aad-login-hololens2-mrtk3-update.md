---
layout: single
title: AAD Login HoloLens2 - MRTK3 Update
excerpt: "What's the first thing that anyone creating any enterprise app will need to work out?"
date: 2022-07-28 19:27
author_profile: true
comments: true
categories: [Azure, HoloLens, HoloLens, mrtk3, unity, Unity]
header:
    teaserlogo: 'assets/images/2022/07/eye.png'
    teaser: 'assets/images/2022/07/eye.png'

---
<!-- wp:image {"id":8741,"width":840,"height":597,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="{{ site.baseurl }}/assets/images/2022/07/eye.png" alt="" class="wp-image-8741" width="840" height="597"/><figcaption>Eye</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>What's the first thing that anyone creating any enterprise app will need to work out?</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>**Auth**</strong>.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>You can't really create an app that will actually be used in a real scenario without having thought this through. Embedding access keys into your app won't cut it and we often need to be able to support multiple users in a secure way.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The only secure way to share a HoloLens device is to use AAD identities <a href="https://docs.microsoft.com/en-us/hololens/hololens-identity#lets-talk-about-setting-up-user-identity-for-hololens-2" target="_blank" rel="noreferrer noopener">https://docs.microsoft.com/en-us/hololens/hololens-identity#lets-talk-about-setting-up-user-identity-for-hololens-2</a>. Alongside this you can authenticate using iris recognition. I wrote a previous post around that here <a href="{{ site.baseurl }}/aad/azure/hololens/login/mixedreality/oauth2/uncategorized/2020/07/17/aad-login-on-hololens-2.html" target="_blank" rel="noreferrer noopener">https://petexr.com/aad/azure/hololens/login/mixedreality/oauth2/uncategorized/2020/07/17/aad-login-on-hololens-2.html</a> where I also covered other auth methods including the use of MSAL. This time I will focus on using the auth token that is already present on a HoloLens device if you re using AAD to login to the device itself. In addition, I neglected to cover a common scenario which is using the token to protect a custom web API.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So, we will walk through setting up the Azure side of things and be left with a sample that will retrieve the token without the need to re-authenticate and use that token to access a custom web API. I'm going to use MRTK3 and Unity 2021.3 and the code will be using <strong>WebAuthenticationCoreManager</strong> which is UWP-only and so this solution is not cross-platform in any way and won't run in the Unity editor. If you are new to <a rel="noreferrer noopener" href="https://github.com/microsoft/MixedRealityToolkit-Unity" target="_blank">MRTK</a> you can find mrtk3 on a branch (mrtk3) at that repository. For convenience, I would suggest using device code flow in the editor which you can easily implement using MSAL (See <a href="{{ site.baseurl }}/hololens/mixedreality/msgraph/oauth2/2019/01/17/microsoft-graph-auth-on-hololens-device-code-flow.html" target="_blank" rel="noreferrer noopener">https://petexr.com/hololens/mixedreality/msgraph/oauth2/2019/01/17/microsoft-graph-auth-on-hololens-device-code-flow.html</a>).</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p><em>I haven't checked but it could be that MSAL uses similar on HoloLens now so this could be part of a cross-platform solution.</em></p><p>Custom Web API</p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>So, to illustrate we can just use the boilerplate web API project that Visual Studio generates for us:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8742,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/newprojvs22.png" alt="" class="wp-image-8742"/><figcaption>Visual Studio New Project</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Configure it to use <strong>Windows.Identity</strong> middleware:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8743,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/netcore.png" alt="" class="wp-image-8743"/><figcaption>Identity Platform</figcaption></figure>
<!-- /wp:image -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p><em>Note. before publishing the Web API I made a single change:</em></p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p><em>I added a single `[Authorize]` attribute on the containing Controller class:</em></p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>namespace aad_hl2_webapi.Controllers
{
    &#91;Authorize]
    &#91;ApiController]
    &#91;Route("&#91;controller]")]
    public class WeatherForecastController : ControllerBase
    {

</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>This will activate the Auth middleware for web calls into this class and ensure the entry points get called with a valid OAuth token or a `401 Unauthorized` will get returned.</p>
<!-- /wp:paragraph -->

<!-- wp:heading {"level":3} -->
<h3>Publish</h3>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>And we'll just publish it to an Azure app Service. I quite like the wizard in Visual Studio that helps with this job:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8744,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/publish.png" alt="" class="wp-image-8744"/><figcaption>publish</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>After following instructions to configure resource groups and app service settings we can publish the web API with a click.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8745,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/app-service.png" alt="" class="wp-image-8745"/><figcaption>App Service</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The Web API has a single GET which returns a random weather forecast:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;HttpGet]
public IEnumerable&lt;WeatherForecast> Get()
{
    var rng = new Random();
    return Enumerable.Range(1, 5).Select(index => new WeatherForecast
    {
        Date = DateTime.Now.AddDays(index),
        TemperatureC = rng.Next(-20, 55),
        Summary = Summaries&#91;rng.Next(Summaries.Length)]
    })
    .ToArray();
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>One final thing to configure for the API itself are the App settings which look like this in the local appsettings.json file:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>{
  "AzureAd": {
    "Instance": "https://login.microsoftonline.com/",
    "Domain": "qualified.domain.name",
    "TenantId": "22222222-2222-2222-2222-222222222222",
    "ClientId": "11111111-1111-1111-11111111111111111"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>I don't intend to run the API locally so won't edit these here but instead will need to set these settings up for the deployed API. I'll come back to this later when I know what the AAD app registration settings are that I can use.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>AAD App Registrations</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We are going to need two AAD App registrations for this:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8746,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/aad-appreg.png" alt="" class="wp-image-8746"/><figcaption> AAD App Registration</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>- One for the Web API. This exposes some API scopes that I created for the purpose of illustration.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8747,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/web-api-appreg.png" alt="" class="wp-image-8747"/><figcaption>Web API App Registration</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>- One for the UWP HoloLens app. This declares the scopes from the web API</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8748,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/hl-appreg.png" alt="" class="wp-image-8748"/><figcaption>HoloLens App Registration</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2>Configure the Web API</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>We need to configure the Web API with the relevant settings from the web API app registration.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8749,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/webapi-properties.png" alt="" class="wp-image-8749"/><figcaption>Web API App Registration Properties</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>And we're going to set those directly as properties on the deployed App Service:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8750,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/deployed-webapi-properties.png" alt="" class="wp-image-8750"/><figcaption>Deployed Properties</figcaption></figure>
<!-- /wp:image -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p><em>Note. My Web API is deployed to a different subscription to the AAD I am using.</em></p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p><em>Note. I have the following settings:</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>> </em><strong>**AzureAd:Audience**</strong><em> This is set to the Application ID URI from the hl2webapi app registration. (I'm not 100% if this is needed or not but suspect it is)</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>> </em><strong>**AzureAd:ClientId**</strong><em> This is set to the Application ID URI from the hl2webapi app registration. (My token didn't work until I set this to be the api://xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx format of the Application ID Uri and it took me a while to figure this out!)</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>> </em><strong>**AzureAd:Instance**</strong><em> This is set to https://login.microsoftonline.com/</em></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><em>> The rest are self-explanatory.</em></p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Unity HoloLens App</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>In order to retrieve the AAD token from the HoloLens device we can use the following code (with error handling and logging removed for brevity):</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>public async void Login()
{
#if ENABLE_WINMD_SUPPORT
    WebAccountProvider wap =
        await WebAuthenticationCoreManager.FindAccountProviderAsync("https://login.microsoft.com", Authority);

    WebTokenRequest wtr = new WebTokenRequest(wap, string.Empty, ClientId.ToString());
    wtr.Properties.Add("resource", Resource);
    
    WebTokenRequestResult tokenResponse = await WebAuthenticationCoreManager.GetTokenSilentlyAsync(wtr);
    
    if (tokenResponse.ResponseStatus == WebTokenRequestStatus.Success)
    {
        foreach (var resp in tokenResponse.ResponseData)
        {
            var name = resp.WebAccount.UserName;
            AccessToken = resp.Token;
            var account = resp.WebAccount;
            Username = account.UserName;
        }

        Debug.Log($"Access Token: {AccessToken}");
    }
#endif
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>We are getting close now....</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I nearly forgot that we also need to configure a Redirect URI.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If you execute the following code on the HoloLens you can log out a value you can use as the redirect URI:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>string URI = string.Format("ms-appx-web://Microsoft.AAD.BrokerPlugIn/{0}",
WebAuthenticationBroker.GetCurrentApplicationCallbackUri().Host.ToUpper());
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>So, copy that value and head back over to the HoloLens AAD app registration and add it as a redirect URI:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8751,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/redirect-uri.png" alt="" class="wp-image-8751"/><figcaption>Redirect URI</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So, unless I have forgotten anything important, you should now be able to run the HoloLens app logged in with any user in your AAD tenant, and have it successfully call the weather API. To illustrate this a bit I have fleshed out my sample a bit using MRTK3.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>MRTK3 Sample</h2>
<!-- /wp:heading -->

<!-- wp:image {"id":8752,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/unity-scene.png" alt="" class="wp-image-8752"/><figcaption>Unity Scene</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So, the gist of this is that there is a menu with two buttons; one to retrieve the auth token and the other to issue a call to the weather API using that token to gain access.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The results will be data bound using MRTK3's new data binding stack.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8753,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/unity-play.png" alt="" class="wp-image-8753"/><figcaption>Unity Player</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>There is a WebAPiDataSource script based on the MRTK3 data binding samples:</p>
<!-- /wp:paragraph -->

<!-- wp:code -->
<pre class="wp-block-code"><code>using Microsoft.MixedReality.Toolkit.Data;
using Microsoft.MixedReality.Toolkit.UX;
using System.Collections;
using UnityEngine;
using UnityEngine.Networking;

public class WebApiDataSource : DataSourceGOBase
{
    public delegate void RequestSuccessDelegate(string jsonText, object requestRef);
    public delegate void RequestFailureDelegate(string errorString, object requestRef);

    public DataSourceJson DataSourceJson { get { return DataSource as DataSourceJson; } }

    &#91;Tooltip("URL for a custom Web API")]
    &#91;SerializeField]
    private string url = "https://aad-hololens-api.azurewebsites.net/weatherforecast";

    &#91;Tooltip("Auth data")]
    &#91;SerializeField]
    private Auth auth;

    &#91;Tooltip("Dialog Prefab")]
    &#91;SerializeField]
    private Dialog DialogPrefabSmall;

    /// &lt;summary>
    /// Set the text that will be parsed and used to build the memory based DOM.
    /// &lt;/summary>
    /// &lt;param name="jsonText">THe json string to parse.&lt;/param>
    public void SetJson(string jsonText)
    {
        DataSourceJson.UpdateFromJson(jsonText);
    }

    /// &lt;inheritdoc/>
    public override IDataSource AllocateDataSource()
    {
        return new DataSourceJson();
    }

    public void FetchData()
    {
        if (string.IsNullOrEmpty(auth.AccessToken))
        {
            Dialog.InstantiateFromPrefab(DialogPrefabSmall,
                new DialogProperty("Error", "No token to call the API with.",
                DialogButtonHelpers.OK), true, true);
        }

        StartCoroutine(StartJsonRequest(url, auth.AccessToken));
    }
    
    public IEnumerator StartJsonRequest(string uri, string accessToken, 
        RequestSuccessDelegate successDelegate = null, RequestFailureDelegate failureDelegate = null, 
        object requestRef = null)
    {
        using (UnityWebRequest webRequest = UnityWebRequest.Get(uri))
        {
            webRequest.SetRequestHeader("Authorization", "Bearer " + accessToken);
                
            // Request and wait for the desired page.
            yield return webRequest.SendWebRequest();

#if UNITY_2020_2_OR_NEWER
            if (webRequest.result == UnityWebRequest.Result.ProtocolError || 
                webRequest.result == UnityWebRequest.Result.ConnectionError)
#else
                if (webRequest.isHttpError || webRequest.isNetworkError)
#endif
            {
                if (failureDelegate != null)
                {
                    failureDelegate.Invoke(webRequest.error, requestRef);
                }
            }
            else
            {
                string jsonText = webRequest.downloadHandler.text;

                DataSourceJson.UpdateFromJson(jsonText);
                if (successDelegate != null)
                {
                    successDelegate.Invoke(jsonText, requestRef);
                }
            }
        }
    }
}
</code></pre>
<!-- /wp:code -->

<!-- wp:paragraph -->
<p>And providing you have hooked up scripts for defining data consumers (in this case a Data Consumer Collection). For me this looks like this:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8754,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/mrtk3-db.png" alt="" class="wp-image-8754"/><figcaption>MRTK3 Data Binding Components</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Then you can provide a prefab as an item template to represent each item in the list:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8755,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2022/07/item-template.png" alt="" class="wp-image-8755"/><figcaption>Item Template</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>and you are good to go!</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The source for the sample can be found <a href="https://github.com/peted70/aad-hololens-mrtk3" target="_blank" rel="noreferrer noopener">here</a>.</p>
<!-- /wp:paragraph -->
