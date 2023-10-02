---
layout: single
title: WCF web api + developer APi keys
date: 2011-10-20 15:27
author_profile: true
comments: true
categories: [WCF]
header:
    teaserlogo: 'assets/images/'
    teaser: 'assets/images/'
---
<p>I found myself with the requirement of needing custom authorization for a rest-style web API. The requirements would be something along the lines of Netflix, Twitter, etc. APIs whereby there is a need to identify an application making calls to the service. Most of these APIs seem to follow a familiar pattern of dishing out a developer API key consisting of a private key and a public key. If you’re not familiar with the basics of public key cryptography see <a title="http://en.wikipedia.org/wiki/Public-key_cryptography" href="http://en.wikipedia.org/wiki/Public-key_cryptography">http://en.wikipedia.org/wiki/Public-key_cryptography</a>.</p>  <p>This post is not about that, though! The purpose for this is to show the code I ended with to integrate the solution into WCF Web API (<a title="http://wcf.codeplex.com/" href="http://wcf.codeplex.com/">http://wcf.codeplex.com/</a>). An additional requirement was that some of the API required the auth and some of it didn’t.</p>  <p>I also investigated the OAuth 1.0 spec and it appeared to support my scenario but with extra, unnecessary steps. (I haven’t yet investigated OAuth 2.0 fully).</p>  <p>There appears to be a lot of information floating around on this subject (see <a title="http://weblogs.asp.net/cibrax/archive/2011/04/15/http-message-channels-in-wcf-web-apis-preview-4.aspx" href="http://weblogs.asp.net/cibrax/archive/2011/04/15/http-message-channels-in-wcf-web-apis-preview-4.aspx">http://weblogs.asp.net/cibrax/archive/2011/04/15/http-message-channels-in-wcf-web-apis-preview-4.aspx</a> and <a title="http://haacked.com/archive/2011/10/19/implementing-an-authorization-attribute-for-wcf-web-api.aspx" href="http://haacked.com/archive/2011/10/19/implementing-an-authorization-attribute-for-wcf-web-api.aspx">http://haacked.com/archive/2011/10/19/implementing-an-authorization-attribute-for-wcf-web-api.aspx</a>, amongst others). </p>  <p>Anyway, I’ll run through the steps of what I did for this so far….</p>  <p>So first, I created a blank ASP.NET MVC3 application. I added some routes in the RegisterRoutes method in Global.asax.cs, one for each API:</p>    <div id="codeSnippetWrapper" class="csharpcode-wrapper">   <div id="codeSnippet" class="csharpcode">     <pre class="alt"><span id="lnum1" class="lnum">   1:</span> routes.Add(<span class="kwrd">new</span> ServiceRoute(<span class="str">&quot;api/authedapi&quot;</span>,</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum2" class="lnum">   2:</span>     <span class="kwrd">new</span> HttpServiceHostFactory</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum3" class="lnum">   3:</span>         {</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum4" class="lnum">   4:</span>             Configuration =</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum5" class="lnum">   5:</span>                 <span class="kwrd">new</span> AuthHttpConfiguration(Container)</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum6" class="lnum">   6:</span>                     {</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum7" class="lnum">   7:</span>                         EnableTestClient = <span class="kwrd">true</span>,</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum8" class="lnum">   8:</span>                         CreateInstance = ResolveServiceInstances</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum9" class="lnum">   9:</span>                     }</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum10" class="lnum">  10:</span>         },</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum11" class="lnum">  11:</span>     <span class="kwrd">typeof</span> (AuthedApi)));</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum12" class="lnum">  12:</span> routes.Add(<span class="kwrd">new</span> ServiceRoute(<span class="str">&quot;api/keys&quot;</span>,</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum13" class="lnum">  13:</span>     <span class="kwrd">new</span> HttpServiceHostFactory</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum14" class="lnum">  14:</span>         {</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum15" class="lnum">  15:</span>             Configuration =</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum16" class="lnum">  16:</span>                 <span class="kwrd">new</span> HttpConfiguration</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum17" class="lnum">  17:</span>                     {</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum18" class="lnum">  18:</span>                         EnableTestClient = <span class="kwrd">true</span>,</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum19" class="lnum">  19:</span>                         CreateInstance = ResolveServiceInstances</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum20" class="lnum">  20:</span>                     }</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum21" class="lnum">  21:</span>         },</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum22" class="lnum">  22:</span>     <span class="kwrd">typeof</span> (ApiKeys)));</pre>
<!--CRLF--></div>
</div>

<p>Things to note about this code are:</p>

<ul>
  <li>I have set the CreateInstance delegate to a function which uses my IOC container to resolve the types. </li>

  <li>The configuration for the un-authed api is just the standard way to set up a WCF web API route – the authedapi, however uses a custom Configuration type derived from HttpConfiguration. It does this in order to set up a custom ServiceAuthorizationManager object on the Service Authorization Behavior:</li>
</ul>

<div id="codeSnippet" class="csharpcode">
  <pre class="alt"><span id="lnum1" class="lnum">   1:</span> <span class="kwrd">public</span> <span class="kwrd">class</span> AuthHttpConfiguration : HttpConfiguration</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum2" class="lnum">   2:</span> {</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum3" class="lnum">   3:</span>     <span class="kwrd">private</span> <span class="kwrd">readonly</span> IUnityContainer _container;</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum4" class="lnum">   4:</span>&#160; </pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum5" class="lnum">   5:</span>     <span class="kwrd">public</span> AuthHttpConfiguration(IUnityContainer container)</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum6" class="lnum">   6:</span>     {</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum7" class="lnum">   7:</span>         _container = container;</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum8" class="lnum">   8:</span>     }</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum9" class="lnum">   9:</span>&#160; </pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum10" class="lnum">  10:</span>     <span class="kwrd">protected</span> <span class="kwrd">override</span> <span class="kwrd">void</span> OnConfigureServiceHost(HttpServiceHost serviceHost)</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum11" class="lnum">  11:</span>     {</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum12" class="lnum">  12:</span>         ServiceDebugBehavior serviceDebugBehavior = serviceHost.Description.Behaviors.OfType&lt;ServiceDebugBehavior&gt;().FirstOrDefault();</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum13" class="lnum">  13:</span>         <span class="kwrd">if</span> (serviceDebugBehavior != <span class="kwrd">null</span>)</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum14" class="lnum">  14:</span>             serviceDebugBehavior.IncludeExceptionDetailInFaults = <span class="kwrd">true</span>;</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum15" class="lnum">  15:</span>&#160; </pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum16" class="lnum">  16:</span>         <span class="kwrd">foreach</span> (var behavior <span class="kwrd">in</span> serviceHost.Description.Behaviors.OfType&lt;ServiceAuthorizationBehavior&gt;())</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum17" class="lnum">  17:</span>         {</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum18" class="lnum">  18:</span>             behavior.ServiceAuthorizationManager = _container.Resolve&lt;MyServiceAuthorizationManager&gt;();</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum19" class="lnum">  19:</span>         }</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum20" class="lnum">  20:</span>         <span class="kwrd">base</span>.OnConfigureServiceHost(serviceHost);</pre>
<!--CRLF-->

  <pre class="alt"><span id="lnum21" class="lnum">  21:</span>     }</pre>
<!--CRLF-->

  <pre class="alteven"><span id="lnum22" class="lnum">  22:</span> }</pre>
<!--CRLF--></div>

<p>Also note that I use an IOC container to resolve the types. (WCF web API lends itself nicely to the use of dependency injection and unit testing).</p>

<p>Plugging in the custom authorization manager (<a title="http://msdn.microsoft.com/en-us/library/ms731774.aspx" href="http://msdn.microsoft.com/en-us/library/ms731774.aspx">http://msdn.microsoft.com/en-us/library/ms731774.aspx</a>) allows each requests auth to be handled in a central location. Authorization decisions are made in the <strong>CheckAccessCore</strong> method, which returns <strong>true</strong> when access is granted and <strong>false</strong> when access is denied.</p>

<p>So, here’s the shell of MyServiceAuthorizationManager…</p>

<div id="codeSnippetWrapper" class="csharpcode-wrapper">
  <div id="codeSnippet" class="csharpcode">
    <pre class="alt"><span id="lnum1" class="lnum">   1:</span> <span class="kwrd">public</span> <span class="kwrd">class</span> MyServiceAuthorizationManager : ServiceAuthorizationManager</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum2" class="lnum">   2:</span>  {</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum3" class="lnum">   3:</span>      <span class="kwrd">private</span> <span class="kwrd">readonly</span> IApiKeyRepository _apiKeyRepository;</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum4" class="lnum">   4:</span>&#160; </pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum5" class="lnum">   5:</span>      <span class="kwrd">public</span> MyServiceAuthorizationManager(IApiKeyRepository repository)</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum6" class="lnum">   6:</span>      {</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum7" class="lnum">   7:</span>          _apiKeyRepository = repository;</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum8" class="lnum">   8:</span>      }</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum9" class="lnum">   9:</span>&#160; </pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum10" class="lnum">  10:</span>      <span class="kwrd">protected</span> <span class="kwrd">override</span> <span class="kwrd">bool</span> CheckAccessCore(OperationContext operationContext)</pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum11" class="lnum">  11:</span>      {</pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum12" class="lnum">  12:</span>         <span class="rem">// logic to determine auth status for request...</span></pre>
<!--CRLF-->

    <pre class="alt"><span id="lnum13" class="lnum">  13:</span>      } </pre>
<!--CRLF-->

    <pre class="alteven"><span id="lnum14" class="lnum">  14:</span> }</pre>
<!--CRLF--></div>
</div>

<p>The logic in CheckAccessCore is something like the following in my scenario:</p>

<ul>
  <li>Retrieve the auth-related data from the http request (probably from the http auth header or contained in the query string). This will contain the public key and also the request signature.</li>

  <li>This contains the public key so look up the private key from the public key in a key repository (The public key is sent in the request. The private key is stored separately by both parties).</li>

  <li>Generate a signature using the public and private keys and compare it to the one sent in the request.</li>

  <li>If they match return true, otherwise return false.
    <br /></li>
</ul>

<p>This seems to work for me for now but I don’t yet know how this compares with the other methods or indeed whether OAuth 2.0 will better facilitate the requirements for this scenario.</p>
