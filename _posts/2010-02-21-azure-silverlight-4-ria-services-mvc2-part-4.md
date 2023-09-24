---
layout: post
title: Azure + Silverlight 4 + RIA Services + MVC2 (Part 4)
date: 2010-02-21 20:37
author: peted70
comments: true
categories: [ASP.NET MVC, Azure, RIA Services, Silverlight]
---
<div id="msgcns!4F1B7368284539E5!184" class="bvMsg"><p>by Peter Daukintis</p> <p>So, now I know how to set up a basic project, store and retrieve my data in Azure Table storage and have incorporated the asp.net providers that use Table Storage it’s time to turn my attention towards finding the right architecture to make the most out of RIA Services. I would like to use the PRISM libraries with an MVVM approach but I’m not sure how well these play with RIA Services in a Silverlight Business Application.</p> <p>The first step was to get a version of PRISM that works with vs2010 – this I found here <a title="http://blogs.southworks.net/dschenkelman/2009/11/09/prism-2-composite-application-guidance-for-wpf-silverlight-migrated-to-visual-studio-2010-beta-2/" href="http://blogs.southworks.net/dschenkelman/2009/11/09/prism-2-composite-application-guidance-for-wpf-silverlight-migrated-to-visual-studio-2010-beta-2/">http://blogs.southworks.net/dschenkelman/2009/11/09/prism-2-composite-application-guidance-for-wpf-silverlight-migrated-to-visual-studio-2010-beta-2/</a></p> <p>Then I imagined that the solution would consist of using PRISM’s module support to load each tab of the Silverlight Business Application on demand. This should support only downloading xap’s that the user has elected to view. I wasn’t searching for a fully dynamic solution – just a middle ground where the application is split up into separate modules – and the right environment in which to improve the design later.</p> <p>The solution I went for was to create hyperlinks and related xaml files which fit with the Silverlight 4 navigation scenario but to define a PRISM ‘region’ inside each navigation page. If you are not familiar with PRISM regions or in general I would suggest that you take a look at Mike Taulty’s series here <a title="http://mtaulty.com/CommunityServer/blogs/mike_taultys_blog/archive/2009/10/27/prism-and-silverlight-screencasts-on-channel-9.aspx" href="http://mtaulty.com/CommunityServer/blogs/mike_taultys_blog/archive/2009/10/27/prism-and-silverlight-screencasts-on-channel-9.aspx">http://mtaulty.com/CommunityServer/blogs/mike_taultys_blog/archive/2009/10/27/prism-and-silverlight-screencasts-on-channel-9.aspx</a>. The regions will act as a placeholder for the user interface which will later be supplied by the modules. So, effectively each tab is hard-coded and it’s view defines a region to be filled in later.</p> <p>Here’s an example of one of the xaml pages:</p><pre><span style="color:#efef8f;">&lt;navigation:Page </span><span style="color:#dfdfbf;">x:Class</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;PrismNav.Views.AudioModule&quot;
           </span><span style="color:#dfdfbf;">xmlns</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
           </span><span style="color:#dfdfbf;">xmlns:x</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;
           </span><span style="color:#dfdfbf;">xmlns:d</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://schemas.microsoft.com/expression/blend/2008&quot;
           </span><span style="color:#dfdfbf;">xmlns:mc</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
           </span><span style="color:#dfdfbf;">mc:Ignorable</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;d&quot;
           </span><span style="color:#dfdfbf;">xmlns:navigation</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;clr-namespace:System.Windows.Controls;assembly=System.Windows.Controls.Navigation&quot;
           </span><span style="color:#dfdfbf;">xmlns:Regions</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;clr-namespace:Microsoft.Practices.Composite.Presentation.Regions;assembly=Microsoft.Practices.Composite.Presentation&quot; 
           </span><span style="color:#dfdfbf;">d:DesignWidth</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;640&quot; </span><span style="color:#dfdfbf;">d:DesignHeight</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;480&quot;
           </span><span style="color:#dfdfbf;">Title</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Audio Module&quot;</span><span style="color:#efef8f;">&gt;
  &lt;Grid </span><span style="color:#dfdfbf;">x:Name</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;LayoutRoot&quot;</span><span style="color:#efef8f;">&gt;
    &lt;ItemsControl </span><span style="color:#dfdfbf;">x:Name</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Module2Region&quot; </span><span style="color:#dfdfbf;">Regions:RegionManager.RegionName</span><span style="color:#efef8f;">=</span><span style="color:#cc9393;">&quot;Module2Region&quot;</span><span style="color:#efef8f;">/&gt;
  &lt;/Grid&gt;
&lt;/navigation:Page&gt;

</span></pre>
<p><a href="http://11011.net/software/vspaste"></a>and here’s an example of the link in the main xaml file:</p><pre>                        <span style="color:#d2d200;">&lt;Rectangle x:Name=</span><span style="color:white;">&quot;Divider3&quot; </span><span style="color:#d2d200;">Style=&quot;&#123;</span><span style="color:cyan;">StaticResource </span><span style="color:white;">DividerStyle</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">/&gt;

                        &lt;HyperlinkButton x:Name=</span><span style="color:white;">&quot;AudioModuleLink&quot; </span><span style="color:#d2d200;">Style=&quot;&#123;</span><span style="color:cyan;">StaticResource </span><span style="color:white;">LinkStyle</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; 
                                     </span><span style="color:#d2d200;">NavigateUri=</span><span style="color:white;">&quot;/AudioModule&quot; </span><span style="color:#d2d200;">TargetName=</span><span style="color:white;">&quot;ContentFrame&quot; </span><span style="color:#d2d200;">Content=</span><span style="color:white;">&quot;Audio Module&quot;</span><span style="color:#d2d200;">/&gt;

</span></pre><a href="http://11011.net/software/vspaste"></a>
<p>One of these are added for each required tab.</p>
<p>Some extra steps are required to initialise the PRISM environment:</p>
<p>Bootstrapper – add a bootstrapper class to the project to let PRISM know about the main window and also to intialise a module catalog.</p><pre>    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">BootStrapper </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">UnityBootstrapper
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">protected override </span><span style="color:#f0dfaf;">DependencyObject </span><span style="color:#f8ffc6;">CreateShell</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#f0dfaf;">Shell </span><span style="color:#f8ffc6;">shell </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">Container</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Resolve</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">Shell</span><span style="color:#d2d200;">&gt;(); 
            </span><span style="color:#f0dfaf;">Application</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Current</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">RootVisual </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">shell</span><span style="color:#d2d200;">; 
            </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">shell</span><span style="color:#d2d200;">;
        &#125;

        </span><span style="color:#eaeaac;">protected override </span><span style="color:#2b91af;">IModuleCatalog </span><span style="color:#f8ffc6;">GetModuleCatalog</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#f0dfaf;">ModuleCatalog </span><span style="color:#f8ffc6;">moduleCatalog </span><span style="color:#d2d200;">= </span><span style="color:#f0dfaf;">ModuleCatalog</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">CreateFromXaml</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">Uri</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;PrismNav;component/ModuleCatalog.xaml&quot;</span><span style="color:#d2d200;">, </span></pre>
<blockquote><pre><span style="color:#d2d200;"></span><span style="color:#2b91af;">             UriKind</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Relative</span><span style="color:#d2d200;">));
            </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">moduleCatalog</span><span style="color:#d2d200;">;
        &#125;
    &#125;
&#125;</span></pre></blockquote>
<p><span style="color:#d2d200;"><font color="#ffffff">Where the Shell is the main page of the Silverlight app renamed to Shell.xaml (to adhere to convention). It is also necessary to kick off a call to run the bootstrapper code – this can be done like this</font></span></p><pre>            <span style="color:#eaeaac;">var </span><span style="color:#f8ffc6;">bootStrapper </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">BootStrapper</span><span style="color:#d2d200;">();
            </span><span style="color:#f8ffc6;">bootStrapper</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Run</span><span style="color:#d2d200;">();
            </span><span style="color:#f8ffc6;">_unityContainer </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">bootStrapper</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Container</span><span style="color:#d2d200;">;</span><span style="color:#d2d200;">

</span><span style="color:#d2d200;"><font color="#ffffff"></font></span></pre>
<p><span style="color:#d2d200;"><font color="#ffffff">inserted into the Application_Startup method in App.xaml.cs (Note that I take a reference to the bootstrapper’s unity container here – this I use to resolve creation of the Shell view class in InitializeRootVisual() as in the generated code it uses new Shell() with no ctor parameters but I have modified my Shell class to be constructor injected with the IModuleManager interface).</font></span></p>
<p><span style="color:#d2d200;"><font color="#ffffff">The module catalog you can add to the app and it’s contents are like this:</font></span></p><pre><span style="color:#d2d200;">m:ModuleCatalog xmlns=</span><span style="color:white;">&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
                 </span><span style="color:#d2d200;">xmlns:x=</span><span style="color:white;">&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot; 
                 </span><span style="color:#d2d200;">xmlns:m=</span><span style="color:white;">&quot;clr-namespace:Microsoft.Practices.Composite.Modularity;assembly=Microsoft.Practices.Composite&quot;</span><span style="color:#d2d200;">&gt;
    &lt;m:ModuleInfoGroup Ref=</span><span style="color:white;">&quot;HomeModule.xap&quot; </span><span style="color:#d2d200;">InitializationMode=</span><span style="color:white;">&quot;OnDemand&quot;</span><span style="color:#d2d200;">&gt;
        &lt;m:ModuleInfo ModuleName=</span><span style="color:white;">&quot;HomeModule.InitModule&quot;
                      </span><span style="color:#d2d200;">ModuleType=</span><span style="color:white;">&quot;HomeModule.InitModule, HomeModule, Version=1.0.0.0&quot;</span><span style="color:#d2d200;">/&gt;
    &lt;/m:ModuleInfoGroup&gt;
    &lt;m:ModuleInfoGroup Ref=</span><span style="color:white;">&quot;AudioModule.xap&quot; </span><span style="color:#d2d200;">InitializationMode=</span><span style="color:white;">&quot;OnDemand&quot;</span><span style="color:#d2d200;">&gt;
        &lt;m:ModuleInfo ModuleName=</span><span style="color:white;">&quot;AudioModule.InitModule&quot;
                      </span><span style="color:#d2d200;">ModuleType=</span><span style="color:white;">&quot;AudioModule.InitModule, AudioModule, Version=1.0.0.0&quot;</span><span style="color:#d2d200;">/&gt;
    &lt;/m:ModuleInfoGroup&gt;
&lt;/m:ModuleCatalog&gt;

</span></pre>
<p>This describes where the code for the modules are and some extra settings describing how the module should be loaded, etc..Now, onto creating a module:</p><pre><span style="color:#eaeaac;">using </span><span style="color:#f8ffc6;">Microsoft</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Practices</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Composite</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Modularity</span><span style="color:#d2d200;">;
</span><span style="color:#eaeaac;">using </span><span style="color:#f8ffc6;">Microsoft</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Practices</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Composite</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Regions</span><span style="color:#d2d200;">;

</span><span style="color:#eaeaac;">namespace </span><span style="color:#f8ffc6;">AudioModule
</span><span style="color:#d2d200;">&#123;
    </span><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">InitModule </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">IModule
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private readonly </span><span style="color:#2b91af;">IRegionManager </span><span style="color:#f8ffc6;">_regionManager</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f8ffc6;">InitModule</span><span style="color:#d2d200;">(</span><span style="color:#2b91af;">IRegionManager </span><span style="color:#f8ffc6;">regionManager</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#f8ffc6;">_regionManager </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">regionManager</span><span style="color:#d2d200;">;
        &#125;

        </span><span style="color:#dfaf8f;">#region </span><span style="color:#d2d200;">IModule Members

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">Initialize</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#f8ffc6;">_regionManager</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">RegisterViewWithRegion</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Module2Region&quot;</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">typeof</span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">Audio</span><span style="color:#d2d200;">));
        &#125;

        </span><span style="color:#dfaf8f;">#endregion
    </span><span style="color:#d2d200;">&#125;
&#125;

</span></pre>
<p>Add a new Silverlight application to the solution and empty out it’s contents and add in the code above which provides the PRISM module definition. Add a xaml file which will be the ui you wish to be injected into the region and the Initialise call above registers this modules interest in the ‘module2Region’.</p>
<p>So, now we have two ends of the story – these need to be connected together and here’s how to do it.</p>
<p>When the tabbed hyperlinks are clicked on a Silverlight Business Application there is some code in the code behind for the main shell file which is triggered – see the ContentFrame_Navigated method. This is just used to set the states of the hyperlink controls but is a handy place to load the modules. This can be done as below:</p><pre>        <span style="color:#eaeaac;">private void </span><span style="color:#f8ffc6;">ContentFrame_Navigated</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">sender</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">NavigationEventArgs </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">blah </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Uri</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">ToString</span><span style="color:#d2d200;">();
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">blah</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">EndsWith</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Module&quot;</span><span style="color:#d2d200;">))
            &#123;
                </span><span style="color:#f8ffc6;">blah </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">blah</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Remove</span><span style="color:#d2d200;">(</span><span style="color:#8acccf;">0</span><span style="color:#d2d200;">, </span><span style="color:#8acccf;">1</span><span style="color:#d2d200;">);
                </span><span style="color:#f8ffc6;">_moduleManager</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">LoadModule</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">blah </span><span style="color:#d2d200;">+ </span><span style="color:#c89191;">&quot;.InitModule&quot;</span><span style="color:#d2d200;">);
            &#125;

            </span><span style="color:#eaeaac;">foreach </span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">UIElement </span><span style="color:#f8ffc6;">child </span><span style="color:#eaeaac;">in </span><span style="color:#f8ffc6;">LinksStackPanel</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Children</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#f0dfaf;">HyperlinkButton </span><span style="color:#f8ffc6;">hb </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">child </span><span style="color:#eaeaac;">as </span><span style="color:#f0dfaf;">HyperlinkButton</span><span style="color:#d2d200;">;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">hb </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null </span><span style="color:#d2d200;">&amp;&amp; </span><span style="color:#f8ffc6;">hb</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">NavigateUri </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">hb</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">NavigateUri</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">ToString</span><span style="color:#d2d200;">().</span><span style="color:#f8ffc6;">Equals</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Uri</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">ToString</span><span style="color:#d2d200;">()))
                    &#123;
                        </span><span style="color:#f0dfaf;">VisualStateManager</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">GoToState</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">hb</span><span style="color:#d2d200;">, </span><span style="color:#c89191;">&quot;ActiveLink&quot;</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">true</span><span style="color:#d2d200;">);
                    &#125;
                    </span><span style="color:#eaeaac;">else
                    </span><span style="color:#d2d200;">&#123;
                        </span><span style="color:#f0dfaf;">VisualStateManager</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">GoToState</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">hb</span><span style="color:#d2d200;">, </span><span style="color:#c89191;">&quot;InactiveLink&quot;</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">true</span><span style="color:#d2d200;">);
                    &#125;
                &#125;
            &#125;
        &#125;

</span></pre>
<p>Please note that I have just used a ‘quick and dirty’ naming convention to get the module name from the Uri – this obviously doesn’t scale too well – but will do for now!</p>
<p>And that’s it – although it’s not perfect it is the start of a modular architecture. In my next post I will investigate how to have the RIA Services code play nicely with this modular architecture and allow it to generate my client side service calls + entities.  </p>  </div>
