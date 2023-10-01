---
layout: single
title: WP7 looping selector
date: 2010-09-22 23:15
author: peted70
comments: true
categories: [Windows Phone 7]
---
<div id="msgcns!4F1B7368284539E5!301" class="bvMsg"><p>by Peter Daukintis </p> <p>After release of the RTM software tools for WP7 I was pleased to see the looping controls manifested as DatePicker and TimePicker since I was considering creating similar…These are built on top of the LoopingSelector control which has the usual support for ItemTemplates and data binding.</p> <p>So, after adding a reference to Microsoft.Phone.Controls.Toolkit I added this to my xaml:</p> <p><span style="color:#d2d200;"><pre><span style="color:#d2d200;">&lt;Primitives:LoopingSelector ItemSize=</span><span style="color:white;">&quot;100,160&quot; </span><span style="color:#d2d200;">x:Name=</span><span style="color:white;">&quot;loop&quot; </span><span style="color:#d2d200;">DataSource=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">Data</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">&gt;
                &lt;Primitives:LoopingSelector.ItemTemplate&gt;
                    &lt;DataTemplate&gt;
                        &lt;StackPanel&gt;                        
                            &lt;TextBlock Text=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">Selected</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">&gt;&lt;/TextBlock&gt;
                            &lt;Image Source=</span><span style="color:white;">&quot;ApplicationIcon.png&quot;</span><span style="color:#d2d200;">&gt;&lt;/Image&gt;
                            &lt;TextBlock Text=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">TextItem</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">&gt;&lt;/TextBlock&gt;
                        &lt;/StackPanel&gt;
                    &lt;/DataTemplate&gt;
                &lt;/Primitives:LoopingSelector.ItemTemplate&gt;
            &lt;/Primitives:LoopingSelector&gt; 
</span></pre><br /></span>

<p>This is the class declaration for the data source (implementing ILoopingSelectorDataSource expected by the LoopingSelector):</p><pre>    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">DataSrc </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">ILoopingSelectorDataSource</span><span style="color:#d2d200;">, </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">public event </span><span style="color:#2b91af;">PropertyChangedEventHandler </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">PropertyChanged </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PropertyChangedEventArgs</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">));
            &#125;
        &#125;

        </span><span style="color:#eaeaac;">private </span><span style="color:#f0dfaf;">DataItem </span><span style="color:#f8ffc6;">_selected</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">DataItem </span><span style="color:#f8ffc6;">Selected
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_selected</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">&#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_selected </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">_selected </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Selected&quot;</span><span style="color:#d2d200;">);
                &#125;
            &#125;
        &#125;

        </span><span style="color:#eaeaac;">public object </span><span style="color:#f8ffc6;">GetNext</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">relativeTo</span><span style="color:#d2d200;">) 
        &#123;
            </span><span style="color:#f0dfaf;">DataItem </span><span style="color:#f8ffc6;">num </span><span style="color:#d2d200;">= (</span><span style="color:#f0dfaf;">DataItem</span><span style="color:#d2d200;">)</span><span style="color:#f8ffc6;">relativeTo</span><span style="color:#d2d200;">;
            </span><span style="color:#eaeaac;">return new </span><span style="color:#f0dfaf;">DataItem </span><span style="color:#d2d200;">&#123; </span><span style="color:#f8ffc6;">Selected </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">num</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Selected </span><span style="color:#d2d200;">+ </span><span style="color:#8acccf;">1 </span><span style="color:#d2d200;">&#125;;
        &#125;

        </span><span style="color:#eaeaac;">public object </span><span style="color:#f8ffc6;">GetPrevious</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">relativeTo</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#f0dfaf;">DataItem </span><span style="color:#f8ffc6;">num </span><span style="color:#d2d200;">= (</span><span style="color:#f0dfaf;">DataItem</span><span style="color:#d2d200;">)</span><span style="color:#f8ffc6;">relativeTo</span><span style="color:#d2d200;">;
            </span><span style="color:#eaeaac;">return new </span><span style="color:#f0dfaf;">DataItem </span><span style="color:#d2d200;">&#123; </span><span style="color:#f8ffc6;">Selected </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">num</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Selected </span><span style="color:#d2d200;">- </span><span style="color:#8acccf;">1 </span><span style="color:#d2d200;">&#125;;
        &#125;

        </span><span style="color:#eaeaac;">public object </span><span style="color:#f8ffc6;">SelectedItem </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">Selected</span><span style="color:#d2d200;">; &#125; </span><span style="color:#eaeaac;">set </span><span style="color:#d2d200;">&#123; </span><span style="color:#f8ffc6;">Selected </span><span style="color:#d2d200;">= (</span><span style="color:#f0dfaf;">DataItem</span><span style="color:#d2d200;">)</span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">; &#125; &#125;

        </span><span style="color:#eaeaac;">public event </span><span style="color:#2b91af;">EventHandler</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">SelectionChangedEventArgs</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">SelectionChanged</span><span style="color:#d2d200;">;
    &#125;

</span></pre>
<p>where the DataItem is defined as follows:</p><pre>    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">DataItem </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">private int </span><span style="color:#f8ffc6;">_selected</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public int </span><span style="color:#f8ffc6;">Selected
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_selected</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">&#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_selected </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">_selected </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Selected&quot;</span><span style="color:#d2d200;">);
                &#125;
            &#125;
        &#125;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">TextItem
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#c89191;">&quot;label for &quot; </span><span style="color:#d2d200;">+ </span><span style="color:#f8ffc6;">Selected</span><span style="color:#d2d200;">; &#125;
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

</span></pre>
<p>and my viewmodel like this…</p><pre>    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">VM </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">&#123;
        </span><span style="color:#eaeaac;">public event </span><span style="color:#2b91af;">PropertyChangedEventHandler </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">PropertyChanged </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PropertyChangedEventArgs</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">));
            &#125;
        &#125;

        </span><span style="color:#eaeaac;">private </span><span style="color:#f0dfaf;">DataSrc </span><span style="color:#f8ffc6;">_data</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">DataSrc </span><span style="color:#f8ffc6;">Data
        </span><span style="color:#d2d200;">&#123;
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_data</span><span style="color:#d2d200;">; &#125;
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">&#123;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_data </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">_data </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Data&quot;</span><span style="color:#d2d200;">);
                &#125;
            &#125;
        &#125;
    &#125;

</span></pre>
<p>resulting in a pretty cool and flexible spin box-like control.</p>
<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/loopingselector5b65d.png" rel="WLPP"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;border-top:0;border-right:0;padding-top:0;" title="loopingselector" border="0" alt="loopingselector" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/loopingselector5b65d.png?w=189" width="491" height="773" /></a></p>
Technorati Tags: <a href="http://technorati.com/tags/Beta" rel="tag">Beta</a>,<a href="http://technorati.com/tags/Peter" rel="tag">Peter</a>,<a href="http://technorati.com/tags/Daukintis" rel="tag">Daukintis</a>,<a href="http://technorati.com/tags/tools" rel="tag">tools</a>,<a href="http://technorati.com/tags/DatePicker" rel="tag">DatePicker</a>,<a href="http://technorati.com/tags/TimePicker" rel="tag">TimePicker</a>,<a href="http://technorati.com/tags/LoopingSelector" rel="tag">LoopingSelector</a>,<a href="http://technorati.com/tags/ItemTemplates" rel="tag">ItemTemplates</a>,<a href="http://technorati.com/tags/data" rel="tag">data</a>,<a href="http://technorati.com/tags/reference" rel="tag">reference</a>,<a href="http://technorati.com/tags/Microsoft" rel="tag">Microsoft</a>,<a href="http://technorati.com/tags/Controls" rel="tag">Controls</a>,<a href="http://technorati.com/tags/Toolkit" rel="tag">Toolkit</a>,<a href="http://technorati.com/tags/ItemSize" rel="tag">ItemSize</a>,<a href="http://technorati.com/tags/Name" rel="tag">Name</a>,<a href="http://technorati.com/tags/loop" rel="tag">loop</a>,<a href="http://technorati.com/tags/DataSource" rel="tag">DataSource</a>,<a href="http://technorati.com/tags/ItemTemplate" rel="tag">ItemTemplate</a>,<a href="http://technorati.com/tags/DataTemplate" rel="tag">DataTemplate</a>,<a href="http://technorati.com/tags/StackPanel" rel="tag">StackPanel</a>,<a href="http://technorati.com/tags/TextBlock" rel="tag">TextBlock</a>,<a href="http://technorati.com/tags/Text" rel="tag">Text</a>,<a href="http://technorati.com/tags/Image" rel="tag">Image</a>,<a href="http://technorati.com/tags/Source" rel="tag">Source</a>,<a href="http://technorati.com/tags/ApplicationIcon" rel="tag">ApplicationIcon</a>,<a href="http://technorati.com/tags/TextItem" rel="tag">TextItem</a>,<a href="http://technorati.com/tags/declaration" rel="tag">declaration</a>,<a href="http://technorati.com/tags/ILoopingSelectorDataSource" rel="tag">ILoopingSelectorDataSource</a>,<a href="http://technorati.com/tags/DataSrc" rel="tag">DataSrc</a>,<a href="http://technorati.com/tags/event" rel="tag">event</a>,<a href="http://technorati.com/tags/PropertyChangedEventHandler" rel="tag">PropertyChangedEventHandler</a>,<a href="http://technorati.com/tags/PropertyChangedEventArgs" rel="tag">PropertyChangedEventArgs</a>,<a href="http://technorati.com/tags/DataItem" rel="tag">DataItem</a>,<a href="http://technorati.com/tags/GetNext" rel="tag">GetNext</a>,<a href="http://technorati.com/tags/GetPrevious" rel="tag">GetPrevious</a>,<a href="http://technorati.com/tags/SelectedItem" rel="tag">SelectedItem</a>,<a href="http://technorati.com/tags/EventHandler" rel="tag">EventHandler</a>,<a href="http://technorati.com/tags/SelectionChangedEventArgs" rel="tag">SelectionChangedEventArgs</a>,<a href="http://technorati.com/tags/_data" rel="tag">_data</a>,<a href="http://technorati.com/tags/propertyName" rel="tag">propertyName</a><br />
Windows Live Tags: <a href="http://windows.live.com/connect/tag/Beta" rel="clubhouseTag">Beta</a>,<a href="http://windows.live.com/connect/tag/Peter" rel="clubhouseTag">Peter</a>,<a href="http://windows.live.com/connect/tag/Daukintis" rel="clubhouseTag">Daukintis</a>,<a href="http://windows.live.com/connect/tag/tools" rel="clubhouseTag">tools</a>,<a href="http://windows.live.com/connect/tag/DatePicker" rel="clubhouseTag">DatePicker</a>,<a href="http://windows.live.com/connect/tag/TimePicker" rel="clubhouseTag">TimePicker</a>,<a href="http://windows.live.com/connect/tag/LoopingSelector" rel="clubhouseTag">LoopingSelector</a>,<a href="http://windows.live.com/connect/tag/ItemTemplates" rel="clubhouseTag">ItemTemplates</a>,<a href="http://windows.live.com/connect/tag/data" rel="clubhouseTag">data</a>,<a href="http://windows.live.com/connect/tag/reference" rel="clubhouseTag">reference</a>,<a href="http://windows.live.com/connect/tag/Microsoft" rel="clubhouseTag">Microsoft</a>,<a href="http://windows.live.com/connect/tag/Controls" rel="clubhouseTag">Controls</a>,<a href="http://windows.live.com/connect/tag/Toolkit" rel="clubhouseTag">Toolkit</a>,<a href="http://windows.live.com/connect/tag/ItemSize" rel="clubhouseTag">ItemSize</a>,<a href="http://windows.live.com/connect/tag/Name" rel="clubhouseTag">Name</a>,<a href="http://windows.live.com/connect/tag/loop" rel="clubhouseTag">loop</a>,<a href="http://windows.live.com/connect/tag/DataSource" rel="clubhouseTag">DataSource</a>,<a href="http://windows.live.com/connect/tag/ItemTemplate" rel="clubhouseTag">ItemTemplate</a>,<a href="http://windows.live.com/connect/tag/DataTemplate" rel="clubhouseTag">DataTemplate</a>,<a href="http://windows.live.com/connect/tag/StackPanel" rel="clubhouseTag">StackPanel</a>,<a href="http://windows.live.com/connect/tag/TextBlock" rel="clubhouseTag">TextBlock</a>,<a href="http://windows.live.com/connect/tag/Text" rel="clubhouseTag">Text</a>,<a href="http://windows.live.com/connect/tag/Image" rel="clubhouseTag">Image</a>,<a href="http://windows.live.com/connect/tag/Source" rel="clubhouseTag">Source</a>,<a href="http://windows.live.com/connect/tag/ApplicationIcon" rel="clubhouseTag">ApplicationIcon</a>,<a href="http://windows.live.com/connect/tag/TextItem" rel="clubhouseTag">TextItem</a>,<a href="http://windows.live.com/connect/tag/declaration" rel="clubhouseTag">declaration</a>,<a href="http://windows.live.com/connect/tag/ILoopingSelectorDataSource" rel="clubhouseTag">ILoopingSelectorDataSource</a>,<a href="http://windows.live.com/connect/tag/DataSrc" rel="clubhouseTag">DataSrc</a>,<a href="http://windows.live.com/connect/tag/event" rel="clubhouseTag">event</a>,<a href="http://windows.live.com/connect/tag/PropertyChangedEventHandler" rel="clubhouseTag">PropertyChangedEventHandler</a>,<a href="http://windows.live.com/connect/tag/PropertyChangedEventArgs" rel="clubhouseTag">PropertyChangedEventArgs</a>,<a href="http://windows.live.com/connect/tag/DataItem" rel="clubhouseTag">DataItem</a>,<a href="http://windows.live.com/connect/tag/GetNext" rel="clubhouseTag">GetNext</a>,<a href="http://windows.live.com/connect/tag/GetPrevious" rel="clubhouseTag">GetPrevious</a>,<a href="http://windows.live.com/connect/tag/SelectedItem" rel="clubhouseTag">SelectedItem</a>,<a href="http://windows.live.com/connect/tag/EventHandler" rel="clubhouseTag">EventHandler</a>,<a href="http://windows.live.com/connect/tag/SelectionChangedEventArgs" rel="clubhouseTag">SelectionChangedEventArgs</a>,<a href="http://windows.live.com/connect/tag/_data" rel="clubhouseTag">_data</a>,<a href="http://windows.live.com/connect/tag/propertyName" rel="clubhouseTag">propertyName</a>  </div>
