---
layout: single
title: Azure + Silverlight 4 + RIA Services + MVC2 (Part 3)
date: 2010-02-21 02:44
author_profile: true
comments: true
categories: [ASP.NET MVC, Azure, RIA Services, Silverlight]
header:
    teaserlogo: 'assets/images/'
    teaser: 'assets/images/'
---
<div id="msgcns!4F1B7368284539E5!182" class="bvMsg"><p>by Peter Daukintis</p> <p>Next up is getting the authentication service to use azure table storage.</p> <p>To do this you need to get the additional azure code samples from here <a title="http://code.msdn.microsoft.com/windowsazuresamples" href="http://code.msdn.microsoft.com/windowsazuresamples">http://code.msdn.microsoft.com/windowsazuresamples</a> and include the asp providers project into the solution (this will need to be up converted as the project is for vs2008). Then include a reference to this in the host website project.</p> <p>For my scenario, where I am not running my app as a cloud service, I need to add settings to my web.config file to provide settings for the various providers. It was quite challenging to work out what exactly was required here but I got things working with the following in my web.config:</p><pre>    <span style="color:#efef8f;">&lt;membership </span><span style="color:#dfdfbf;">defaultProvider</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;TableStorageMembershipProvider&quot; </span><span style="color:#dfdfbf;">userIsOnlineTimeWindow </span><span style="color:#efef8f;">= </span><span style="color:#cc9393;">&quot;20&quot;</span><span style="color:#efef8f;">&gt;
      &lt;providers&gt;
        &lt;clear/&gt;

        &lt;add </span><span style="color:#dfdfbf;">name</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;TableStorageMembershipProvider&quot;
             </span><span style="color:#dfdfbf;">type</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Microsoft.Samples.ServiceHosting.AspProviders.TableStorageMembershipProvider&quot;
             </span><span style="color:#dfdfbf;">description</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Membership provider using table storage&quot;
             </span><span style="color:#dfdfbf;">enablePasswordRetrieval</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;false&quot;
             </span><span style="color:#dfdfbf;">enablePasswordReset</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;true&quot;
             </span><span style="color:#dfdfbf;">requiresQuestionAndAnswer</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;false&quot;
             </span><span style="color:#dfdfbf;">minRequiredPasswordLength</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;1&quot;
             </span><span style="color:#dfdfbf;">minRequiredNonalphanumericCharacters</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;0&quot;
             </span><span style="color:#dfdfbf;">requiresUniqueEmail</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;true&quot;
             </span><span style="color:#dfdfbf;">passwordFormat</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Clear&quot;
             </span><span style="color:#dfdfbf;">applicationName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;YourAppName&quot;
             </span><span style="color:#dfdfbf;">accountName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;youracctname&quot;
             </span><span style="color:#dfdfbf;">sharedKey</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;xxxxxx&quot;
             </span><span style="color:#dfdfbf;">allowInsecureRemoteEndpoints</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;true&quot;
             </span><span style="color:#dfdfbf;">tableServiceBaseUri</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://youracctname.table.core.windows.net/&quot;
                </span><span style="color:#efef8f;">/&gt;

      &lt;/providers&gt;
    &lt;/membership&gt;
    &lt;roleManager </span><span style="color:#dfdfbf;">enabled</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;true&quot; </span><span style="color:#dfdfbf;">defaultProvider</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;TableStorageRoleProvider&quot; </span><span style="color:#dfdfbf;">cacheRolesInCookie</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;true&quot; </span><span style="color:#dfdfbf;">cookieName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;.ASPXROLES&quot; </span><span style="color:#dfdfbf;">cookieTimeout</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;30&quot;
                     </span><span style="color:#dfdfbf;">cookiePath</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;/&quot; </span><span style="color:#dfdfbf;">cookieRequireSSL</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;false&quot; </span><span style="color:#dfdfbf;">cookieSlidingExpiration </span><span style="color:#efef8f;">= </span><span style="color:#cc9393;">&quot;true&quot;
                     </span><span style="color:#dfdfbf;">cookieProtection</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;All&quot; </span><span style="color:#efef8f;">&gt;
      &lt;providers&gt;
        &lt;clear/&gt;
        &lt;add </span><span style="color:#dfdfbf;">name</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;TableStorageRoleProvider&quot;
             </span><span style="color:#dfdfbf;">type</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Microsoft.Samples.ServiceHosting.AspProviders.TableStorageRoleProvider&quot;
             </span><span style="color:#dfdfbf;">description</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Role provider using table storage&quot;
             </span><span style="color:#dfdfbf;">allowInsecureRemoteEndpoints</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;true&quot;
             </span><span style="color:#dfdfbf;">applicationName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;YourAppName&quot;
             </span><span style="color:#dfdfbf;">accountName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;youracctname&quot;
             </span><span style="color:#dfdfbf;">sharedKey</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;xxxxxx&quot;
             </span><span style="color:#dfdfbf;">tableServiceBaseUri</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://youracctname.table.core.windows.net/&quot;
                </span><span style="color:#efef8f;">/&gt;
      &lt;/providers&gt;
    &lt;/roleManager&gt;
    &lt;sessionState </span><span style="color:#dfdfbf;">mode</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Custom&quot; </span><span style="color:#dfdfbf;">customProvider</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;TableStorageSessionStateProvider&quot;</span><span style="color:#efef8f;">&gt;
      &lt;providers&gt;
        &lt;clear /&gt;
        &lt;add </span><span style="color:#dfdfbf;">name</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;TableStorageSessionStateProvider&quot;
             </span><span style="color:#dfdfbf;">type</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Microsoft.Samples.ServiceHosting.AspProviders.TableStorageSessionStateProvider&quot;
             </span><span style="color:#dfdfbf;">allowInsecureRemoteEndpoints</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;true&quot;
             </span><span style="color:#dfdfbf;">applicationName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;YourAppName&quot;
             </span><span style="color:#dfdfbf;">accountName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;youracctname&quot;
             </span><span style="color:#dfdfbf;">sharedKey</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;xxxxxx&quot;
             </span><span style="color:#dfdfbf;">tableServiceBaseUri</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://youracctname.table.core.windows.net/&quot;
             </span><span style="color:#dfdfbf;">blobServiceBaseUri</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://youracctname.blob.core.windows.net/&quot;
             </span><span style="color:#efef8f;">/&gt;
      &lt;/providers&gt;
    &lt;/sessionState&gt;

</span></pre><a href="http://11011.net/software/vspaste"></a>
<p>I struggled a bit to get this to work but changing the passwordFormat to Clear made everything work so I have assumed that there’s a bug with the Hashed passwords currently.</p>
<p>By the way, I used this tool <a title="http://azurestorageexplorer.codeplex.com/" href="http://azurestorageexplorer.codeplex.com/">http://azurestorageexplorer.codeplex.com/</a> with great success for confirming whether data was being written/read correctly and whether the tables were being created successfully.</p>  </div>
