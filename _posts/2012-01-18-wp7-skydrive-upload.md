---
layout: post
title: wp7 skydrive upload
date: 2012-01-18 15:05
author: peted70
comments: true
categories: [skydrive, Windows Phone 7, wp7dev]
---
<p>   <div style="display:inline;float:none;margin:0;padding:0;" id="scid:0767317B-992E-4b12-91E0-4F059A8CECA8:25cafb44-75f6-4a59-877b-4f5883b7b142" class="wlWriterEditableSmartContent">Technorati Tags: <a href="http://technorati.com/tags/wp7dev" rel="tag">wp7dev</a></div> Download the sdk, which at the time of writing is here <a title="http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=28195" href="http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=28195">http://www.microsoft.com/download/en/details.aspx?displaylang=en&amp;id=28195</a></p>  <p>Next, head over to <a title="https://manage.dev.live.com/Applications/Index" href="https://manage.dev.live.com/Applications/Index">https://manage.dev.live.com/Applications/Index</a> and add an application. This will provide you with an application Id and a client secret token. You also need to configure a valid redirect url as required by the OAuth handshake.</p>  <p>&#160;</p>  <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2012/01/liveappreg.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;padding-top:0;" title="liveappreg" border="0" alt="liveappreg" src="http://peted.azurewebsites.net/wp-content/uploads/2012/01/liveappreg_thumb.png" width="552" height="346" /></a></p>  <p>&#160;</p>  <p>Add a reference to Microsoft.Live and Microsoft.Live.Controls. (included with the SDK)</p>  <p>Add a SignInButton to your page.</p>  <p>&#160;</p> <span style="color:gray;">   <pre class="code"><span style="color:gray;">&lt;</span><span style="color:#afc81c;">Controls</span><span style="color:gray;">:</span><span style="color:#afc81c;">SignInButton </span><span style="color:#498091;">x</span><span style="color:gray;">:</span><span style="color:#498091;">Name</span><span style="color:gray;">=</span><span style="background:#374626;color:#99b478;">&quot;signInButton&quot;</span><span style="color:#498091;"> Height</span><span style="color:gray;">=</span><span style="background:#374626;color:#99b478;">&quot;120&quot;
</span><span style="color:#d6ded4;">                                   </span><span style="color:#498091;">ClientId</span><span style="color:gray;">=</span><span style="background:#374626;color:#99b478;">&quot;XXXXXXXXXXXXXXXX&quot;
</span><span style="color:#d6ded4;">                                   </span><span style="color:#498091;">SessionChanged</span><span style="color:gray;">=</span><span style="background:#374626;color:#99b478;">&quot;SignInButtonSessionChanged&quot;
</span><span style="color:#d6ded4;">                                   </span><span style="color:#498091;">Scopes</span><span style="color:gray;">=</span><span style="background:#374626;color:#99b478;">&quot;wl.skydrive_update&quot;
</span><span style="color:#d6ded4;">                                   </span><span style="color:#498091;">RenderTransformOrigin</span><span style="color:gray;">=</span><span style="background:#374626;color:#99b478;">&quot;0.5,0.5&quot;</span><span style="color:#d6ded4;"> 
                                   </span><span style="color:#498091;">d</span><span style="color:gray;">:</span><span style="color:#498091;">IsHidden</span><span style="color:gray;">=</span><span style="background:#374626;color:#99b478;">&quot;True&quot;</span><span style="color:gray;">&gt;
</span></pre>

  <p>&#160;</p>

  
Now, you should be able to click on the sign in button, get redirected to the live login page.&#160; After logging in you will be required to give consent for the requested authorisation. This will look like this:</span>

<p>&#160;</p>

<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2012/01/authorise.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;padding-top:0;" title="authorise" border="0" alt="authorise" src="http://peted.azurewebsites.net/wp-content/uploads/2012/01/authorise_thumb.png" width="265" height="464" /></a>&#160;</p>

<p>We then wire up a handler to the SignInButton SessionChanged event and if the sign in was successful we will get a LiveConnectSession object passed in the event.</p>

<pre class="code">            <span style="color:#498091;">if </span><span style="color:#d6ded4;">(</span><span style="color:#c7c7a5;">e</span><span style="color:gray;">.</span><span style="color:#c7c7a5;">Status </span><span style="color:gray;">== </span><span style="color:#70d17e;">LiveConnectSessionStatus</span><span style="color:gray;">.</span><span style="color:#c7c7a5;">Connected</span><span style="color:#d6ded4;">)
            {
                </span><span style="color:#afc81c;">VisualStateManager</span><span style="color:gray;">.</span><span style="color:#c7c7a5;">GoToState</span><span style="color:#d6ded4;">(</span><span style="color:#498091;">this</span><span style="color:#d6ded4;">, </span><span style="background:#1d2514;color:#99b478;">&quot;SignedIn&quot;</span><span style="color:#d6ded4;">, </span><span style="color:#498091;">true</span><span style="color:#d6ded4;">);
                </span><span style="color:#c7c7a5;">_liveConnectSession </span><span style="color:gray;">= </span><span style="color:#c7c7a5;">e</span><span style="color:gray;">.</span><span style="color:#c7c7a5;">Session</span><span style="color:#d6ded4;">;
            }

</span></pre>

<p>&#160;</p>

<p>A LiveConnectClient object can be constructed from the session object and used to carry out standard operations, i.e. upload, delete, copy, etc.</p>

<p>&#160;</p>

<p>Find the project here:</p>

<p><a title="https://skydrive.live.com/redir.aspx?cid=4f1b7368284539e5&amp;resid=4F1B7368284539E5!435&amp;parid=4F1B7368284539E5!123" href="https://skydrive.live.com/redir.aspx?cid=4f1b7368284539e5&amp;resid=4F1B7368284539E5!435&amp;parid=4F1B7368284539E5!123">https://skydrive.live.com/redir.aspx?cid=4f1b7368284539e5&amp;resid=4F1B7368284539E5!435&amp;parid=4F1B7368284539E5!123</a></p>
