---
layout: post
title: Data Binding Pivot Control WP7 MVVM
date: 2010-09-20 00:49
author: peted70
comments: true
categories: [MVVM, Windows Phone 7]
---
<div id="msgcns!4F1B7368284539E5!298" class="bvMsg"><p>by Peter Daukintis</p> <p>Ok, having had access to the official WP7 Pivot control in the RTM Tools for Windows Phone 7 for a few days I thought it was time to explore and particularly in it’s data binding capabilities. I knocked up a quick and dirty example for a forum response but felt the need to explore a bit further…</p> <p>The example consisted of binding the Pivot control to a homogenous view model collection. All well and good but in real life the requirements would be more like handling a heterogenous data source and binding the page headers, etc..</p> <p>So, we start with the control bound to a PageCollection</p><pre><span style="color:#d2d200;">&lt;controls:Pivot ItemsSource=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">PageCollection</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; 
                        </span><span style="color:#d2d200;">Title=</span><span style="color:white;">&quot;MY APPLICATION&quot; 
                        </span><span style="color:#d2d200;">HorizontalContentAlignment=</span><span style="color:white;">&quot;Stretch&quot; 
                        </span><span style="color:#d2d200;">VerticalContentAlignment=</span><span style="color:white;">&quot;Stretch&quot; 
                        </span><span style="color:#d2d200;">&gt;
</span></pre>
<p> </p>
<p>The PageCollection is defined as follows:</p><pre><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">MainViewModel </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">PageViewModel</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">_pageCollection</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">PageViewModel</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">PageCollection
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_pageCollection</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">&#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_pageCollection </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">_pageCollection </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">NotifyPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;PageCollection&quot;</span><span style="color:#d2d200;">);
                &#125;
            &#125;
        &#125;
</span></pre>
<p> </p>
<p>However, we are going to want to have differing objects in this collection so I will also create some PageViewModel derived types:</p><pre><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">PageViewModel </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private string </span><span style="color:#f8ffc6;">_titleText</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">TitleText
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_titleText</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">&#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_titleText </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">_titleText </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;TitleText&quot;</span><span style="color:#d2d200;">);
                &#125;
            &#125;
        &#125;

        </span><span style="color:#eaeaac;">public event </span><span style="color:#2b91af;">PropertyChangedEventHandler </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">PropertyChanged </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PropertyChangedEventArgs</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">));
            &#125;
        &#125;
    &#125;

    </span><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">ListPageViewModel </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">PageViewModel
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#eaeaac;">string</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">_contextCollection</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#eaeaac;">string</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">ContextCollection
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_contextCollection</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">&#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_contextCollection </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">_contextCollection </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;ContextCollection&quot;</span><span style="color:#d2d200;">);
                &#125;
            &#125;
        &#125;
    &#125;

    </span><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">ImagePageViewModel </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">PageViewModel
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private </span><span style="color:#f0dfaf;">Uri </span><span style="color:#f8ffc6;">_uri</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">Uri </span><span style="color:#f8ffc6;">Uri
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_uri</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">&#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_uri </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">_uri </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Uri&quot;</span><span style="color:#d2d200;">);
                &#125;
            &#125;
        &#125;
    &#125;
</span></pre>
<p>One has a list of strings and another has a Uri which will define the location of an image.</p>
<p>In my main view model constructor I will create the runtime data (consisting of one instance of each type)</p><pre>        <span style="color:#eaeaac;">public </span><span style="color:#f8ffc6;">MainViewModel</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#f8ffc6;">PageCollection </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">PageViewModel</span><span style="color:#d2d200;">&gt;
                                 &#123;
                                     </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">ListPageViewModel
                                         </span><span style="color:#d2d200;">&#123;
                                             </span><span style="color:#f8ffc6;">TitleText </span><span style="color:#d2d200;">= </span><span style="color:#c89191;">&quot;List Page&quot;</span><span style="color:#d2d200;">,
                                             </span><span style="color:#f8ffc6;">ContextCollection </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#eaeaac;">string</span><span style="color:#d2d200;">&gt; &#123; </span><span style="color:#c89191;">&quot;item1&quot;</span><span style="color:#d2d200;">, </span><span style="color:#c89191;">&quot;item2&quot; </span><span style="color:#d2d200;">&#125;
                                         &#125;,
                                     </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">ImagePageViewModel
                                         </span><span style="color:#d2d200;">&#123;
                                             </span><span style="color:#f8ffc6;">TitleText </span><span style="color:#d2d200;">= </span><span style="color:#c89191;">&quot;Image Page&quot;</span><span style="color:#d2d200;">,
                                             </span><span style="color:#f8ffc6;">Uri </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">Uri</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Images/pic.png&quot;</span><span style="color:#d2d200;">, </span><span style="color:#2b91af;">UriKind</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Relative</span><span style="color:#d2d200;">)
                                         &#125;
                                 &#125;;
        &#125;

</span></pre>
<p>And to bind the page header I’ll add a header template</p><pre><span style="color:#d2d200;">&lt;controls:Pivot ItemsSource=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">PageCollection</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; 
                        </span><span style="color:#d2d200;">Title=</span><span style="color:white;">&quot;MY APPLICATION&quot; 
                        </span><span style="color:#d2d200;">HorizontalContentAlignment=</span><span style="color:white;">&quot;Stretch&quot; 
                        </span><span style="color:#d2d200;">VerticalContentAlignment=</span><span style="color:white;">&quot;Stretch&quot; 
                        </span><span style="color:#d2d200;">&gt;
            &lt;controls:Pivot.HeaderTemplate&gt;
                &lt;DataTemplate&gt;
                    &lt;Grid x:Name=</span><span style="color:white;">&quot;grid&quot;</span><span style="color:#d2d200;">&gt;
                        &lt;TextBlock TextWrapping=</span><span style="color:white;">&quot;Wrap&quot;
                                   </span><span style="color:#d2d200;">Text=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">TitleText</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;
                                   </span><span style="color:#d2d200;">d:LayoutOverrides=</span><span style="color:white;">&quot;Width, Height&quot; </span><span style="color:#d2d200;">/&gt;
                    &lt;/Grid&gt;
                &lt;/DataTemplate&gt;

            &lt;/controls:Pivot.HeaderTemplate&gt;
</span><span style="color:silver;">        </span><span style="color:#d2d200;">&lt;/controls:Pivot&gt;
</span></pre>
<p> </p>
<p>and we’re also going to need a data template selector to allow us to select a data template at runtime depending on the type of the instance we are binding. This is a little tricky since this is not natively supported in Silverlight but fortunately not too hard to implement. I drew upon this <a href="http://geekswithblogs.net/tkokke/archive/2009/09/28/datatemplateselector-in-silverlight.aspx">http://geekswithblogs.net/tkokke/archive/2009/09/28/datatemplateselector-in-silverlight.aspx</a> and <a title="http://forums.silverlight.net/forums/t/190667.aspx" href="http://forums.silverlight.net/forums/t/190667.aspx">http://forums.silverlight.net/forums/t/190667.aspx</a> to implement the following (see those posts for explanation): </p><pre>   <span style="color:#eaeaac;">public static class </span><span style="color:#f0dfaf;">Extensions
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">public static </span><span style="color:#d2d200;">T </span><span style="color:#f8ffc6;">FindResource</span><span style="color:#d2d200;">&lt;</span><span style="color:#f8ffc6;">T</span><span style="color:#d2d200;">&gt;(</span><span style="color:#eaeaac;">this </span><span style="color:#f0dfaf;">DependencyObject </span><span style="color:#f8ffc6;">initial</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">key</span><span style="color:#d2d200;">) </span><span style="color:#eaeaac;">where </span><span style="color:#d2d200;">T : </span><span style="color:#f0dfaf;">DependencyObject
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#f0dfaf;">DependencyObject </span><span style="color:#f8ffc6;">current </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">initial</span><span style="color:#d2d200;">;

            </span><span style="color:#eaeaac;">while </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">current </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">current </span><span style="color:#eaeaac;">is </span><span style="color:#f0dfaf;">FrameworkElement</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">((</span><span style="color:#f8ffc6;">current </span><span style="color:#eaeaac;">as </span><span style="color:#f0dfaf;">FrameworkElement</span><span style="color:#d2d200;">).</span><span style="color:#f8ffc6;">Resources</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Contains</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">key</span><span style="color:#d2d200;">))
                    &#123;
                        </span><span style="color:#eaeaac;">return </span><span style="color:#d2d200;">(T)(</span><span style="color:#f8ffc6;">current </span><span style="color:#eaeaac;">as </span><span style="color:#f0dfaf;">FrameworkElement</span><span style="color:#d2d200;">).</span><span style="color:#f8ffc6;">Resources</span><span style="color:#d2d200;">[</span><span style="color:#f8ffc6;">key</span><span style="color:#d2d200;">];
                    &#125;
                &#125;

                </span><span style="color:#f8ffc6;">current </span><span style="color:#d2d200;">= </span><span style="color:#f0dfaf;">VisualTreeHelper</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">GetParent</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">current</span><span style="color:#d2d200;">);
            &#125;

            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">Application</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Current</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Resources</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Contains</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">key</span><span style="color:#d2d200;">))
            &#123;
                </span><span style="color:#eaeaac;">return </span><span style="color:#d2d200;">(T)</span><span style="color:#f0dfaf;">Application</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Current</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Resources</span><span style="color:#d2d200;">[</span><span style="color:#f8ffc6;">key</span><span style="color:#d2d200;">];
            &#125;

            </span><span style="color:#eaeaac;">return default</span><span style="color:#d2d200;">(T);
        &#125;
    &#125;

    </span><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">DataTemplateSelector </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">ContentControl
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">protected override void </span><span style="color:#f8ffc6;">OnContentChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">oldContent</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">newContent</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#f8ffc6;">ContentTemplate </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">FindResource</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">DataTemplate</span><span style="color:#d2d200;">&gt;(</span><span style="color:#f8ffc6;">newContent</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">GetType</span><span style="color:#d2d200;">().</span><span style="color:#f8ffc6;">FullName</span><span style="color:#d2d200;">);
        &#125;
    &#125;

</span></pre>
<p>which then allowed me to declare the ItemTemplate for the Pivot control like this:</p><pre>            <span style="color:#d2d200;">&lt;controls:Pivot.ItemTemplate&gt;
                &lt;DataTemplate&gt;
                    &lt;WindowsPhonePivotApplication1:DataTemplateSelector Content=&quot;&#123;</span><span style="color:cyan;">Binding</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; </span><span style="color:#d2d200;">/&gt;
                &lt;/DataTemplate&gt;
            &lt;/controls:Pivot.ItemTemplate&gt;

</span></pre>
<p>and to add the data templates (one for each derived type) like this:</p><pre>    <span style="color:#d2d200;">&lt;phone:PhoneApplicationPage.Resources&gt;
        &lt;DataTemplate x:Key=</span><span style="color:white;">&quot;WindowsPhonePivotApplication1.ViewModels.ListPageViewModel&quot;</span><span style="color:#d2d200;">&gt;
            &lt;ListBox ItemsSource=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">ContextCollection</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">&gt;
                
            &lt;/ListBox&gt;
        &lt;/DataTemplate&gt;
        &lt;DataTemplate x:Key=</span><span style="color:white;">&quot;WindowsPhonePivotApplication1.ViewModels.ImagePageViewModel&quot;</span><span style="color:#d2d200;">&gt;
            &lt;Image Source=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">Uri</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">&gt;&lt;/Image&gt;
        &lt;/DataTemplate&gt;

</span></pre>
<p>and that was it – here is the result…</p>
<p><a href="https://omlweq.bay.livefilestore.com/y1mmdO68y578s4Z4jShc09YtTQV7JIhOIUnXmGnqX91tmuQaZJATGb95U0QsGiVdBy3gSdQ2f8oUxewiuEJ774NME1RQPuoKFbmmj8vV3J4uXHsvxUmj1GbngvKTL6d1TQM6F3eNqww_kSm3q8ihLIuRQ/pivotpage1[5].png?download&amp;psid=1" rel="WLPP"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:inline;border-top:0;border-right:0;padding-top:0;" title="pivotpage1" border="0" alt="pivotpage1" src="https://omlweq.bay.livefilestore.com/y1mbo55cRkRJnF8N8lyWkdmSFvL8MLxSb8G1o44pHMzi5zjN-P_QIfwRXaVVYAuK6rNi6C2p2LH-DZUQyerW_hfVYpYnvDjQm9fLkVwg1j3Y3HMMCGU6n_ZVaqLm9kBTMn-nJRrw5MIOuHBRJm1Yd983Q/pivotpage1_thumb[3].png?download&amp;psid=1" width="339" height="632" /></a>       <a href="http://babaandthepigman.files.wordpress.com/2010/09/pivotpage25b35d.png" rel="WLPP"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:inline;border-top:0;border-right:0;padding-top:0;" title="pivotpage2" border="0" alt="pivotpage2" src="http://babaandthepigman.files.wordpress.com/2010/09/pivotpage25b35d.png?w=162" width="343" height="629" /></a></p>
Technorati Tags: <a href="http://technorati.com/tags/Pivot" rel="tag">Pivot</a>,<a href="http://technorati.com/tags/Control" rel="tag">Control</a>,<a href="http://technorati.com/tags/MVVM" rel="tag">MVVM</a>,<a href="http://technorati.com/tags/Beta" rel="tag">Beta</a>,<a href="http://technorati.com/tags/data" rel="tag">data</a>,<a href="http://technorati.com/tags/example" rel="tag">example</a>,<a href="http://technorati.com/tags/forum" rel="tag">forum</a>,<a href="http://technorati.com/tags/response" rel="tag">response</a>,<a href="http://technorati.com/tags/collection" rel="tag">collection</a>,<a href="http://technorati.com/tags/life" rel="tag">life</a>,<a href="http://technorati.com/tags/requirements" rel="tag">requirements</a>,<a href="http://technorati.com/tags/PageCollection" rel="tag">PageCollection</a>,<a href="http://technorati.com/tags/ItemsSource" rel="tag">ItemsSource</a>,<a href="http://technorati.com/tags/Title" rel="tag">Title</a>,<a href="http://technorati.com/tags/APPLICATION" rel="tag">APPLICATION</a>,<a href="http://technorati.com/tags/HorizontalContentAlignment" rel="tag">HorizontalContentAlignment</a>,<a href="http://technorati.com/tags/Stretch" rel="tag">Stretch</a>,<a href="http://technorati.com/tags/VerticalContentAlignment" rel="tag">VerticalContentAlignment</a>,<a href="http://technorati.com/tags/MainViewModel" rel="tag">MainViewModel</a>,<a href="http://technorati.com/tags/ObservableCollection" rel="tag">ObservableCollection</a>,<a href="http://technorati.com/tags/PageViewModel" rel="tag">PageViewModel</a>,<a href="http://technorati.com/tags/_pageCollection" rel="tag">_pageCollection</a>,<a href="http://technorati.com/tags/objects" rel="tag">objects</a>,<a href="http://technorati.com/tags/_titleText" rel="tag">_titleText</a>,<a href="http://technorati.com/tags/TitleText" rel="tag">TitleText</a>,<a href="http://technorati.com/tags/event" rel="tag">event</a>,<a href="http://technorati.com/tags/PropertyChangedEventHandler" rel="tag">PropertyChangedEventHandler</a>,<a href="http://technorati.com/tags/PropertyChangedEventArgs" rel="tag">PropertyChangedEventArgs</a>,<a href="http://technorati.com/tags/ListPageViewModel" rel="tag">ListPageViewModel</a>,<a href="http://technorati.com/tags/_contextCollection" rel="tag">_contextCollection</a>,<a href="http://technorati.com/tags/ContextCollection" rel="tag">ContextCollection</a>,<a href="http://technorati.com/tags/ImagePageViewModel" rel="tag">ImagePageViewModel</a>,<a href="http://technorati.com/tags/_uri" rel="tag">_uri</a>,<a href="http://technorati.com/tags/location" rel="tag">location</a>,<a href="http://technorati.com/tags/image" rel="tag">image</a>,<a href="http://technorati.com/tags/instance" rel="tag">instance</a>,<a href="http://technorati.com/tags/List" rel="tag">List</a>,<a href="http://technorati.com/tags/Page" rel="tag">Page</a>,<a href="http://technorati.com/tags/Images" rel="tag">Images</a>,<a href="http://technorati.com/tags/Relative" rel="tag">Relative</a>,<a href="http://technorati.com/tags/header" rel="tag">header</a>,<a href="http://technorati.com/tags/template" rel="tag">template</a>,<a href="http://technorati.com/tags/HeaderTemplate" rel="tag">HeaderTemplate</a>,<a href="http://technorati.com/tags/DataTemplate" rel="tag">DataTemplate</a>,<a href="http://technorati.com/tags/Grid" rel="tag">Grid</a>,<a href="http://technorati.com/tags/Name" rel="tag">Name</a>,<a href="http://technorati.com/tags/TextBlock" rel="tag">TextBlock</a>,<a href="http://technorati.com/tags/Wrap" rel="tag">Wrap</a>,<a href="http://technorati.com/tags/Text" rel="tag">Text</a>,<a href="http://technorati.com/tags/LayoutOverrides" rel="tag">LayoutOverrides</a>,<a href="http://technorati.com/tags/Width" rel="tag">Width</a>,<a href="http://technorati.com/tags/archive" rel="tag">archive</a>,<a href="http://technorati.com/tags/explanation" rel="tag">explanation</a>,<a href="http://technorati.com/tags/FindResource" rel="tag">FindResource</a>,<a href="http://technorati.com/tags/DependencyObject" rel="tag">DependencyObject</a>,<a href="http://technorati.com/tags/FrameworkElement" rel="tag">FrameworkElement</a>,<a href="http://technorati.com/tags/Resources" rel="tag">Resources</a>,<a href="http://technorati.com/tags/VisualTreeHelper" rel="tag">VisualTreeHelper</a>,<a href="http://technorati.com/tags/GetParent" rel="tag">GetParent</a>,<a href="http://technorati.com/tags/Current" rel="tag">Current</a>,<a href="http://technorati.com/tags/DataTemplateSelector" rel="tag">DataTemplateSelector</a>,<a href="http://technorati.com/tags/ContentControl" rel="tag">ContentControl</a>,<a href="http://technorati.com/tags/ContentTemplate" rel="tag">ContentTemplate</a>,<a href="http://technorati.com/tags/GetType" rel="tag">GetType</a>,<a href="http://technorati.com/tags/FullName" rel="tag">FullName</a>,<a href="http://technorati.com/tags/ItemTemplate" rel="tag">ItemTemplate</a>,<a href="http://technorati.com/tags/Content" rel="tag">Content</a>,<a href="http://technorati.com/tags/PhoneApplicationPage" rel="tag">PhoneApplicationPage</a>,<a href="http://technorati.com/tags/ViewModels" rel="tag">ViewModels</a>,<a href="http://technorati.com/tags/ListBox" rel="tag">ListBox</a>,<a href="http://technorati.com/tags/Source" rel="tag">Source</a>,<a href="http://technorati.com/tags/result" rel="tag">result</a>,<a href="http://technorati.com/tags/forums" rel="tag">forums</a>,<a href="http://technorati.com/tags/Extensions" rel="tag">Extensions</a>,<a href="http://technorati.com/tags/templates" rel="tag">templates</a>,<a href="http://technorati.com/tags/propertyName" rel="tag">propertyName</a>,<a href="http://technorati.com/tags/runtime" rel="tag">runtime</a>,<a href="http://technorati.com/tags/aspx" rel="tag">aspx</a><br />
Windows Live Tags: <a href="http://windows.live.com/connect/tag/Pivot" rel="clubhouseTag">Pivot</a>,<a href="http://windows.live.com/connect/tag/Control" rel="clubhouseTag">Control</a>,<a href="http://windows.live.com/connect/tag/MVVM" rel="clubhouseTag">MVVM</a>,<a href="http://windows.live.com/connect/tag/Beta" rel="clubhouseTag">Beta</a>,<a href="http://windows.live.com/connect/tag/data" rel="clubhouseTag">data</a>,<a href="http://windows.live.com/connect/tag/example" rel="clubhouseTag">example</a>,<a href="http://windows.live.com/connect/tag/forum" rel="clubhouseTag">forum</a>,<a href="http://windows.live.com/connect/tag/response" rel="clubhouseTag">response</a>,<a href="http://windows.live.com/connect/tag/collection" rel="clubhouseTag">collection</a>,<a href="http://windows.live.com/connect/tag/life" rel="clubhouseTag">life</a>,<a href="http://windows.live.com/connect/tag/requirements" rel="clubhouseTag">requirements</a>,<a href="http://windows.live.com/connect/tag/PageCollection" rel="clubhouseTag">PageCollection</a>,<a href="http://windows.live.com/connect/tag/ItemsSource" rel="clubhouseTag">ItemsSource</a>,<a href="http://windows.live.com/connect/tag/Title" rel="clubhouseTag">Title</a>,<a href="http://windows.live.com/connect/tag/APPLICATION" rel="clubhouseTag">APPLICATION</a>,<a href="http://windows.live.com/connect/tag/HorizontalContentAlignment" rel="clubhouseTag">HorizontalContentAlignment</a>,<a href="http://windows.live.com/connect/tag/Stretch" rel="clubhouseTag">Stretch</a>,<a href="http://windows.live.com/connect/tag/VerticalContentAlignment" rel="clubhouseTag">VerticalContentAlignment</a>,<a href="http://windows.live.com/connect/tag/MainViewModel" rel="clubhouseTag">MainViewModel</a>,<a href="http://windows.live.com/connect/tag/ObservableCollection" rel="clubhouseTag">ObservableCollection</a>,<a href="http://windows.live.com/connect/tag/PageViewModel" rel="clubhouseTag">PageViewModel</a>,<a href="http://windows.live.com/connect/tag/_pageCollection" rel="clubhouseTag">_pageCollection</a>,<a href="http://windows.live.com/connect/tag/objects" rel="clubhouseTag">objects</a>,<a href="http://windows.live.com/connect/tag/_titleText" rel="clubhouseTag">_titleText</a>,<a href="http://windows.live.com/connect/tag/TitleText" rel="clubhouseTag">TitleText</a>,<a href="http://windows.live.com/connect/tag/event" rel="clubhouseTag">event</a>,<a href="http://windows.live.com/connect/tag/PropertyChangedEventHandler" rel="clubhouseTag">PropertyChangedEventHandler</a>,<a href="http://windows.live.com/connect/tag/PropertyChangedEventArgs" rel="clubhouseTag">PropertyChangedEventArgs</a>,<a href="http://windows.live.com/connect/tag/ListPageViewModel" rel="clubhouseTag">ListPageViewModel</a>,<a href="http://windows.live.com/connect/tag/_contextCollection" rel="clubhouseTag">_contextCollection</a>,<a href="http://windows.live.com/connect/tag/ContextCollection" rel="clubhouseTag">ContextCollection</a>,<a href="http://windows.live.com/connect/tag/ImagePageViewModel" rel="clubhouseTag">ImagePageViewModel</a>,<a href="http://windows.live.com/connect/tag/_uri" rel="clubhouseTag">_uri</a>,<a href="http://windows.live.com/connect/tag/location" rel="clubhouseTag">location</a>,<a href="http://windows.live.com/connect/tag/image" rel="clubhouseTag">image</a>,<a href="http://windows.live.com/connect/tag/instance" rel="clubhouseTag">instance</a>,<a href="http://windows.live.com/connect/tag/List" rel="clubhouseTag">List</a>,<a href="http://windows.live.com/connect/tag/Page" rel="clubhouseTag">Page</a>,<a href="http://windows.live.com/connect/tag/Images" rel="clubhouseTag">Images</a>,<a href="http://windows.live.com/connect/tag/Relative" rel="clubhouseTag">Relative</a>,<a href="http://windows.live.com/connect/tag/header" rel="clubhouseTag">header</a>,<a href="http://windows.live.com/connect/tag/template" rel="clubhouseTag">template</a>,<a href="http://windows.live.com/connect/tag/HeaderTemplate" rel="clubhouseTag">HeaderTemplate</a>,<a href="http://windows.live.com/connect/tag/DataTemplate" rel="clubhouseTag">DataTemplate</a>,<a href="http://windows.live.com/connect/tag/Grid" rel="clubhouseTag">Grid</a>,<a href="http://windows.live.com/connect/tag/Name" rel="clubhouseTag">Name</a>,<a href="http://windows.live.com/connect/tag/TextBlock" rel="clubhouseTag">TextBlock</a>,<a href="http://windows.live.com/connect/tag/Wrap" rel="clubhouseTag">Wrap</a>,<a href="http://windows.live.com/connect/tag/Text" rel="clubhouseTag">Text</a>,<a href="http://windows.live.com/connect/tag/LayoutOverrides" rel="clubhouseTag">LayoutOverrides</a>,<a href="http://windows.live.com/connect/tag/Width" rel="clubhouseTag">Width</a>,<a href="http://windows.live.com/connect/tag/archive" rel="clubhouseTag">archive</a>,<a href="http://windows.live.com/connect/tag/explanation" rel="clubhouseTag">explanation</a>,<a href="http://windows.live.com/connect/tag/FindResource" rel="clubhouseTag">FindResource</a>,<a href="http://windows.live.com/connect/tag/DependencyObject" rel="clubhouseTag">DependencyObject</a>,<a href="http://windows.live.com/connect/tag/FrameworkElement" rel="clubhouseTag">FrameworkElement</a>,<a href="http://windows.live.com/connect/tag/Resources" rel="clubhouseTag">Resources</a>,<a href="http://windows.live.com/connect/tag/VisualTreeHelper" rel="clubhouseTag">VisualTreeHelper</a>,<a href="http://windows.live.com/connect/tag/GetParent" rel="clubhouseTag">GetParent</a>,<a href="http://windows.live.com/connect/tag/Current" rel="clubhouseTag">Current</a>,<a href="http://windows.live.com/connect/tag/DataTemplateSelector" rel="clubhouseTag">DataTemplateSelector</a>,<a href="http://windows.live.com/connect/tag/ContentControl" rel="clubhouseTag">ContentControl</a>,<a href="http://windows.live.com/connect/tag/ContentTemplate" rel="clubhouseTag">ContentTemplate</a>,<a href="http://windows.live.com/connect/tag/GetType" rel="clubhouseTag">GetType</a>,<a href="http://windows.live.com/connect/tag/FullName" rel="clubhouseTag">FullName</a>,<a href="http://windows.live.com/connect/tag/ItemTemplate" rel="clubhouseTag">ItemTemplate</a>,<a href="http://windows.live.com/connect/tag/Content" rel="clubhouseTag">Content</a>,<a href="http://windows.live.com/connect/tag/PhoneApplicationPage" rel="clubhouseTag">PhoneApplicationPage</a>,<a href="http://windows.live.com/connect/tag/ViewModels" rel="clubhouseTag">ViewModels</a>,<a href="http://windows.live.com/connect/tag/ListBox" rel="clubhouseTag">ListBox</a>,<a href="http://windows.live.com/connect/tag/Source" rel="clubhouseTag">Source</a>,<a href="http://windows.live.com/connect/tag/result" rel="clubhouseTag">result</a>,<a href="http://windows.live.com/connect/tag/forums" rel="clubhouseTag">forums</a>,<a href="http://windows.live.com/connect/tag/Extensions" rel="clubhouseTag">Extensions</a>,<a href="http://windows.live.com/connect/tag/templates" rel="clubhouseTag">templates</a>,<a href="http://windows.live.com/connect/tag/propertyName" rel="clubhouseTag">propertyName</a>,<a href="http://windows.live.com/connect/tag/runtime" rel="clubhouseTag">runtime</a>,<a href="http://windows.live.com/connect/tag/aspx" rel="clubhouseTag">aspx</a>  </div>
