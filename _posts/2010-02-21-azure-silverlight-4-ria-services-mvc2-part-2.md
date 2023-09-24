---
layout: post
title: Azure + Silverlight 4 + RIA Services + MVC2 (Part 2)
date: 2010-02-21 02:11
author: peted70
comments: true
categories: [ASP.NET MVC, Azure, RIA Services, Silverlight]
---
<div id="msgcns!4F1B7368284539E5!181" class="bvMsg"><p>by Peter Daukintis </p> <p>Ok, with Azure out of the equation until .NET 4 is supported I will turn my attention first to work out how to achieve data storage in the cloud whilst considering the overall architecture.</p> <p>Firstly, I added an empty Domain Service Class by selecting the web hosting project and selecting Add New Item…</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/adddomainserviceclass5b35d.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="AddDomainServiceClass" border="0" alt="AddDomainServiceClass" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/adddomainserviceclass5b35d.png?w=300" width="460" height="319" /></a> </p> <p>This results in the following dialog:</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/emptydomainserviceclassdlg5b35d.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="EmptyDomainServiceClassDlg" border="0" alt="EmptyDomainServiceClassDlg" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/emptydomainserviceclassdlg5b35d.png?w=245" width="328" height="399" /></a> </p> <p>From which I elected to choose &lt;empty domain service class&gt; since I wanted my storage to be hosted on Azure Tables which is not supported out of the box.</p> <p>This generates an empty domain service class, which I fleshed out a little with the following code:</p><pre>    <span style="color:#d2d200;">[</span><span style="color:#f0dfaf;">EnableClientAccess</span><span style="color:#d2d200;">()]
    </span><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">TestDomainService </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">DomainService
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private readonly </span><span style="color:#2b91af;">ITestEntityRepository </span><span style="color:#f8ffc6;">_testEntityRepository</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f8ffc6;">TestDomainService</span><span style="color:#d2d200;">(</span><span style="color:#2b91af;">ITestEntityRepository </span><span style="color:#f8ffc6;">repository</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#f8ffc6;">_testEntityRepository </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">repository</span><span style="color:#d2d200;">;
        &#125;

        </span><span style="color:#eaeaac;">public </span><span style="color:#2b91af;">IQueryable</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">TestEntity</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">GetEntities</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_testEntityRepository</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">GetTestEntities</span><span style="color:#d2d200;">();
        &#125;

        [</span><span style="color:#f0dfaf;">Invoke</span><span style="color:#d2d200;">]
        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">AddTestEntity</span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">TestEntity </span><span style="color:#f8ffc6;">testEntity</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#f8ffc6;">_testEntityRepository</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">AddTestEntity</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">testEntity</span><span style="color:#d2d200;">);
        &#125;
    &#125;
&#125;</span></pre>
<p></p>
<p>which is about as simple as I could make it whilst introducing a repository interface to support testability and adhere to a clear separation of concerns. Leaving the code like this will result in problems as the RIA services code generation layer requires a default constructor for the domain service class. the intention is to inject the concrete repository at runtime using an IOC container.  I used Unity as my IOC container of choice. the mechanism provided as a hook for creation of domain service classes is the IDomainServiceFactory interface (see <a title="http://msdn.microsoft.com/en-us/library/system.web.domainservices.idomainservicefactory(VS.91).aspx" href="http://msdn.microsoft.com/en-us/library/system.web.domainservices.idomainservicefactory(VS.91).aspx">http://msdn.microsoft.com/en-us/library/system.web.domainservices.idomainservicefactory(VS.91).aspx</a>). You can set the DomainService.Factory property to a concrete implementation of your own IDomainServiceFactory interface like the following:</p><pre>    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">DomainServiceFactory </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">IDomainServiceFactory
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">DomainService </span><span style="color:#f8ffc6;">CreateDomainService</span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">Type </span><span style="color:#f8ffc6;">domainServiceType</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">DomainServiceContext </span><span style="color:#f8ffc6;">context</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">var </span><span style="color:#f8ffc6;">service </span><span style="color:#d2d200;">= </span><span style="color:#f0dfaf;">Global</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">IocContainer</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Resolve</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">domainServiceType</span><span style="color:#d2d200;">) </span><span style="color:#eaeaac;">as </span><span style="color:#f0dfaf;">DomainService</span><span style="color:#d2d200;">;
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">service </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#f8ffc6;">service</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Initialize</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">context</span><span style="color:#d2d200;">);
            &#125;
            </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">service</span><span style="color:#d2d200;">;
        &#125;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">ReleaseDomainService</span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">DomainService </span><span style="color:#f8ffc6;">domainService</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#f8ffc6;">domainService</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Dispose</span><span style="color:#d2d200;">();
        &#125;
    &#125;
</span></pre>
<p><span style="color:#d2d200;"></span></p>
<p>Now, this ensures that Unity will resolve the dependencies on the domain service context. So we just need to wire this in and register our dependencies with Unity and we should be back in business.</p><pre>        <span style="color:#eaeaac;">protected void </span><span style="color:#f8ffc6;">Application_Start</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">sender</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">EventArgs </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">IocContainer </span><span style="color:#d2d200;">== </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
                </span><span style="color:#f8ffc6;">IocContainer </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">UnityContainer</span><span style="color:#d2d200;">();
            </span><span style="color:#f0dfaf;">DomainService</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Factory </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">DomainServiceFactory</span><span style="color:#d2d200;">();

            </span><span style="color:#f8ffc6;">IocContainer</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">RegisterType</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">TestDomainService</span><span style="color:#d2d200;">&gt;();
            </span><span style="color:#f8ffc6;">IocContainer</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">RegisterType</span><span style="color:#d2d200;">&lt;</span><span style="color:#2b91af;">ITestEntityRepository</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">TestEntityRepository</span><span style="color:#d2d200;">&gt;();
        &#125;

</span></pre><a href="http://11011.net/software/vspaste"></a>
<p>Now that takes care of some of the plumbing, but what about the actual implementation for the Azure Table Storage?</p>
<p>Well, borrowing some code from here <a title="http://social.msdn.microsoft.com/Forums/en/windowsazure/thread/efdc0bcc-918c-4034-be1d-ecb13a601e7e" href="http://social.msdn.microsoft.com/Forums/en/windowsazure/thread/efdc0bcc-918c-4034-be1d-ecb13a601e7e">http://social.msdn.microsoft.com/Forums/en/windowsazure/thread/efdc0bcc-918c-4034-be1d-ecb13a601e7e</a> gives us an idea of how to implement the repository;</p><pre>    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">TestEntityRepository </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">ITestEntityRepository
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private static readonly </span><span style="color:#f0dfaf;">TestDomainContext </span><span style="color:#f8ffc6;">Ctx</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">static </span><span style="color:#f8ffc6;">TestEntityRepository</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#f0dfaf;">CloudStorageAccount</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">SetConfigurationSettingPublisher</span><span style="color:#d2d200;">((</span><span style="color:#f8ffc6;">configName</span><span style="color:#d2d200;">, </span><span style="color:#f8ffc6;">configSetter</span><span style="color:#d2d200;">) =&gt;
                                                                 </span><span style="color:#f8ffc6;">configSetter</span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">ConfigurationManager</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">AppSettings</span><span style="color:#d2d200;">[</span><span style="color:#f8ffc6;">configName</span><span style="color:#d2d200;">]));

            </span><span style="color:#f0dfaf;">CloudStorageAccount </span><span style="color:#f8ffc6;">account </span><span style="color:#d2d200;">= </span><span style="color:#f0dfaf;">CloudStorageAccount</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">FromConfigurationSetting</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;&lt;your name here&gt;&quot;</span><span style="color:#d2d200;">);

            </span><span style="color:#eaeaac;">var </span><span style="color:#f8ffc6;">tableClient </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">CloudTableClient</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">account</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">TableEndpoint</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">ToString</span><span style="color:#d2d200;">(), </span><span style="color:#f8ffc6;">account</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Credentials</span><span style="color:#d2d200;">);
            </span><span style="color:#f8ffc6;">tableClient</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">CreateTableIfNotExist</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;TestEntities&quot;</span><span style="color:#d2d200;">);
            </span><span style="color:#f8ffc6;">Ctx </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">TestDomainContext</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">account</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">TableEndpoint</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">ToString</span><span style="color:#d2d200;">(), </span><span style="color:#f8ffc6;">account</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Credentials</span><span style="color:#d2d200;">);
        &#125;

        </span><span style="color:#eaeaac;">public </span><span style="color:#2b91af;">IQueryable</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">TestEntity</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">GetTestEntities</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">Ctx</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">TestEntities</span><span style="color:#d2d200;">;
        &#125;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">AddTestEntity</span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">TestEntity </span><span style="color:#f8ffc6;">testEntity</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#f8ffc6;">Ctx</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">AddTestEntity</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">testEntity</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Name</span><span style="color:#d2d200;">, </span><span style="color:#f8ffc6;">testEntity</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Age</span><span style="color:#d2d200;">);
        &#125;</span></pre><pre><span style="color:#d2d200;">   </span><span style="color:#d2d200;">&#125;
</span></pre><a href="http://11011.net/software/vspaste"></a>
<p>Where the TestEntity looks like this:</p><pre>    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">TestEntity </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">TableServiceEntity
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">public </span><span style="color:#f8ffc6;">TestEntity</span><span style="color:#d2d200;">()
        &#123; &#125;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f8ffc6;">TestEntity</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">partitionKey</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">rowKey</span><span style="color:#d2d200;">)
            : </span><span style="color:#eaeaac;">base</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">partitionKey</span><span style="color:#d2d200;">, </span><span style="color:#f8ffc6;">rowKey</span><span style="color:#d2d200;">)
        &#123; &#125;

        [</span><span style="color:#f0dfaf;">Key</span><span style="color:#d2d200;">]
        </span><span style="color:#eaeaac;">public override string </span><span style="color:#f8ffc6;">PartitionKey
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return base</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">PartitionKey</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">base</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">PartitionKey </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">; &#125;
        &#125;

        [</span><span style="color:#f0dfaf;">Key</span><span style="color:#d2d200;">]
        </span><span style="color:#eaeaac;">public override string </span><span style="color:#f8ffc6;">RowKey
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return base</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">RowKey</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">base</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">RowKey </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">; &#125;
        &#125;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">Name </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get</span><span style="color:#d2d200;">; </span><span style="color:#eaeaac;">set</span><span style="color:#d2d200;">; &#125;
        </span><span style="color:#eaeaac;">public int </span><span style="color:#f8ffc6;">Age </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get</span><span style="color:#d2d200;">; </span><span style="color:#eaeaac;">set</span><span style="color:#d2d200;">; &#125;
    &#125;

</span></pre><a href="http://11011.net/software/vspaste"></a>
<p>And the TestDomainContext used by the Concrete repository looks like this:</p>
<blockquote>
<p><span style="color:#eaeaac;">public class</span><span style="color:#f0dfaf;">TestDomainContext </span><span style="color:#d2d200;">:</span><span style="color:#f0dfaf;">TableServiceContext<br />    </span><span style="color:#d2d200;">&#123;<br />        </span><span style="color:#eaeaac;">public</span><span style="color:#f8ffc6;">TestDomainContext</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">baseAddress</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">StorageCredentials </span><span style="color:#f8ffc6;">credentials</span><span style="color:#d2d200;">)<br />            : </span><span style="color:#eaeaac;">base</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">baseAddress</span><span style="color:#d2d200;">, </span><span style="color:#f8ffc6;">credentials</span><span style="color:#d2d200;">)<br />        &#123;<br />        &#125;<br /><br />        </span><span style="color:#eaeaac;">public </span><span style="color:#2b91af;">IQueryable</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">TestEntity</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">TestEntities<br />        </span><span style="color:#d2d200;">&#123;<br />            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">CreateQuery</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">TestEntity</span><span style="color:#d2d200;">&gt;(</span><span style="color:#c89191;">&quot;TestEntities&quot;</span><span style="color:#d2d200;">); &#125;<br />        &#125;<br /><br />        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">AddTestEntity</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">name</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">int </span><span style="color:#f8ffc6;">age</span><span style="color:#d2d200;">)<br />        &#123;<br />            </span><span style="color:#eaeaac;">var </span><span style="color:#f8ffc6;">c </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">TestEntity</span><span style="color:#d2d200;">((</span><span style="color:#2b91af;">DateTime</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">MaxValue</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Ticks </span><span style="color:#d2d200;">- </span><span style="color:#2b91af;">DateTime</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Now</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Ticks</span><span style="color:#d2d200;">).</span><span style="color:#f8ffc6;">ToString</span><span style="color:#d2d200;">(), </span><span style="color:#2b91af;">Guid</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">NewGuid</span><span style="color:#d2d200;">().</span><span style="color:#f8ffc6;">ToString</span><span style="color:#d2d200;">())<br />            &#123;<br />                </span><span style="color:#f8ffc6;">Name </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">name</span><span style="color:#d2d200;">,<br />                </span><span style="color:#f8ffc6;">Age </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">age<br />            </span><span style="color:#d2d200;">&#125;;<br />            </span><span style="color:#f8ffc6;">AddObject</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;TestEntities&quot;</span><span style="color:#d2d200;">, </span><span style="color:#f8ffc6;">c</span><span style="color:#d2d200;">);<br />    </span><span style="color:#d2d200;">        </span><span style="color:#f8ffc6;">SaveChanges</span><span style="color:#d2d200;">();<br /></span><span style="color:#d2d200;">        &#125;<br />    &#125;<br /></span></p>
<p>I just needed to set up the account info in my web.config file to give a working solution:</p></blockquote><pre>      <span style="color:#efef8f;">&lt;add </span><span style="color:#dfdfbf;">key</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;your_key&quot; </span><span style="color:#dfdfbf;">value</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;DefaultEndpointsProtocol=https;AccountName=your_acct_name;AccountKey=”xxxxxxxxxx&quot; </span><span style="color:#efef8f;">/&gt;

</span></pre><a href="http://11011.net/software/vspaste"></a>
<p>That’s it for now – in the next post I will try to configure the application to use azure table storage for authentication….</p>  </div>
