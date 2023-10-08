---
layout: single
title: databinding BingMaps Control Wp7 MVVM
date: 2010-10-06 14:36
author_profile: true
comments: true
categories: [MVVM, Windows Phone 7]
header:
    teaserlogo: 'assets/images/'
    teaser: 'assets/images/'
---
<p>Ok, so a similar treatment for the Bing Maps control as for previous posts on the <a href="http://babaandthepigman.wordpress.com/2010/09/20/data-binding-pivot-control-wp7-mvvm/" target="_blank">Pivot</a> and <a href="http://babaandthepigman.wordpress.com/2010/09/25/data-binding-panorama-control-wp7-mvvm/" target="_blank">Panorama</a> controls. So starting out with a standard Windows Phone Application project…</p>  <p>Straight into the xaml:</p>  <pre class="code">            <span style="color:#d2d200;">&lt;Maps:Map x:Name=</span><span style="color:white;">&quot;map&quot; </span><span style="color:#d2d200;">Grid.Row=</span><span style="color:white;">&quot;1&quot;</span><span style="color:#d2d200;">&gt;
                &lt;Maps:MapItemsControl ItemsSource=&quot;{</span><span style="color:cyan;">Binding </span><span style="color:white;">PushPins</span><span style="color:#d2d200;">}</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">&gt;
                    &lt;Maps:MapItemsControl.ItemTemplate&gt;
                        &lt;DataTemplate&gt;
                            &lt;Maps:Pushpin Location=&quot;{</span><span style="color:cyan;">Binding </span><span style="color:white;">Location</span><span style="color:#d2d200;">}</span><span style="color:white;">&quot; 
                                          </span><span style="color:#d2d200;">Template=&quot;{</span><span style="color:cyan;">StaticResource </span><span style="color:white;">PushpinControlTemplate1</span><span style="color:#d2d200;">}</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">&gt;
                            &lt;/Maps:Pushpin&gt;
                        &lt;/DataTemplate&gt;
                    &lt;/Maps:MapItemsControl.ItemTemplate&gt;
                &lt;/Maps:MapItemsControl&gt;
            &lt;/Maps:Map&gt;
</span></pre>

<p>From this you can see I intend to bind the Map control’s ItemSource to my PushPins collection. Each PushPin item will have a bound location and I have a custom template for the PushPin UI. Let’s take a look at the template first:</p>

<pre class="code">    <span style="color:#d2d200;">&lt;phone:PhoneApplicationPage.Resources&gt;
        &lt;ControlTemplate x:Key=</span><span style="color:white;">&quot;PushpinControlTemplate1&quot; </span><span style="color:#d2d200;">TargetType=</span><span style="color:white;">&quot;Maps:Pushpin&quot;</span><span style="color:#d2d200;">&gt;
            &lt;Grid Height=</span><span style="color:white;">&quot;116&quot; </span><span style="color:#d2d200;">Width=</span><span style="color:white;">&quot;155&quot;</span><span style="color:#d2d200;">&gt;
                &lt;Ellipse Fill=</span><span style="color:white;">&quot;#FFA3A3BE&quot; </span><span style="color:#d2d200;">Stroke=</span><span style="color:white;">&quot;Black&quot; </span><span style="color:#d2d200;">Margin=</span><span style="color:white;">&quot;0&quot;</span><span style="color:#d2d200;">/&gt;
                &lt;Image Source=&quot;{</span><span style="color:cyan;">Binding </span><span style="color:white;">PinSource</span><span style="color:#d2d200;">}</span><span style="color:white;">&quot;</span><span style="color:#d2d200;">/&gt;
            &lt;/Grid&gt;
        &lt;/ControlTemplate&gt;
    &lt;/phone:PhoneApplicationPage.Resources&gt;
</span><span style="color:#d2d200;">
</span></pre>

<p>The template contains an ellipse and an Image with it’s source data bound also so we can vary the display at run-time for each item.</p>

<p>Here’s my view model:</p>

<pre class="code">    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">ViewModel </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">{
        </span><span style="color:#eaeaac;">public event </span><span style="color:#2b91af;">PropertyChangedEventHandler </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">)
        {
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">PropertyChanged </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            {
                </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PropertyChangedEventArgs</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">));
            }
        }

        </span><span style="color:#eaeaac;">private </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">PushPinModel</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">_pushPins</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">PushPinModel</span><span style="color:#d2d200;">&gt; </span><span style="color:#f8ffc6;">PushPins
        </span><span style="color:#d2d200;">{
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">{ </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_pushPins</span><span style="color:#d2d200;">; }
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">{
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_pushPins </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                {
                    </span><span style="color:#f8ffc6;">_pushPins </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;PushPins&quot;</span><span style="color:#d2d200;">);
                }
            }
        }
    }</span></pre>

<p>Very simple, just an ObservableCollection of PushPinModel’s:</p>

<pre class="code">    <span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">PushPinModel </span><span style="color:#d2d200;">: </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">{
        </span><span style="color:#eaeaac;">public </span><span style="color:#f8ffc6;">PushPinModel</span><span style="color:#d2d200;">() { ; }
        </span><span style="color:#eaeaac;">private </span><span style="color:#f0dfaf;">GeoCoordinate </span><span style="color:#f8ffc6;">_location</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">private string </span><span style="color:#f8ffc6;">_pinSource</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">PinSource
        </span><span style="color:#d2d200;">{
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">{ </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_pinSource</span><span style="color:#d2d200;">; }
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">{
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_pinSource </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                {
                    </span><span style="color:#f8ffc6;">_pinSource </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;PinSource&quot;</span><span style="color:#d2d200;">);
                }
            }
        }

        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">GeoCoordinate </span><span style="color:#f8ffc6;">Location
        </span><span style="color:#d2d200;">{
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">{ </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_location</span><span style="color:#d2d200;">; }
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">{
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_location </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                {
                    </span><span style="color:#f8ffc6;">_location </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Location&quot;</span><span style="color:#d2d200;">);
                }
            }
        }

        </span><span style="color:#eaeaac;">public event </span><span style="color:#2b91af;">PropertyChangedEventHandler </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">)
        {
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">PropertyChanged </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            {
                </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PropertyChangedEventArgs</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">));
            }
        }
    }
</span><span style="color:#d2d200;">
</span></pre>

<p>Again, very simple a GeoCoordinate to describe the map location and a string property describing the image to display on the push pin.</p>

<p>Nearly done, just need to set the DataContext with some dummy data…</p>

<p>&#160;</p>

<pre class="code"><span style="color:#f8ffc6;">DataContext </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">ViewModel
    </span><span style="color:#d2d200;">{
        </span><span style="color:#f8ffc6;">PushPins </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">ObservableCollection</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">PushPinModel</span><span style="color:#d2d200;">&gt;
                        {
                            </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PushPinModel
                                </span><span style="color:#d2d200;">{
                                    </span><span style="color:#f8ffc6;">Location </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">GeoCoordinate</span><span style="color:#d2d200;">(</span><span style="color:#8acccf;">51.569593</span><span style="color:#d2d200;">, </span><span style="color:#8acccf;">10.103504</span><span style="color:#d2d200;">),
                                    </span><span style="color:#f8ffc6;">PinSource </span><span style="color:#d2d200;">= </span><span style="color:#c89191;">&quot;ApplicationIcon.png&quot;
                                </span><span style="color:#d2d200;">},
                            </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PushPinModel
                                </span><span style="color:#d2d200;">{
                                    </span><span style="color:#f8ffc6;">Location </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">GeoCoordinate</span><span style="color:#d2d200;">(-</span><span style="color:#8acccf;">45.569593</span><span style="color:#d2d200;">, </span><span style="color:#8acccf;">1.103504</span><span style="color:#d2d200;">),
                                    </span><span style="color:#f8ffc6;">PinSource </span><span style="color:#d2d200;">= </span><span style="color:#c89191;">&quot;ApplicationIcon.png&quot;
                                </span><span style="color:#d2d200;">}
                        }
    };
</span><span style="color:#d2d200;">
</span></pre>

<p>And here’s the result.</p>

<p><a href="{{ site.baseurl }}/assets/images/2010/10/bingmaps.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;border-top:0;border-right:0;padding-top:0;" title="bingmaps" border="0" alt="bingmaps" src="{{ site.baseurl }}/assets/images/2010/10/bingmaps_thumb.png" width="432" height="845" /></a></p>


Technorati Tags: <a href="http://technorati.com/tags/BingMaps" rel="tag">BingMaps</a>,<a href="http://technorati.com/tags/Control" rel="tag">Control</a>,<a href="http://technorati.com/tags/MVVM" rel="tag">MVVM</a>,<a href="http://technorati.com/tags/treatment" rel="tag">treatment</a>,<a href="http://technorati.com/tags/Pivot" rel="tag">Pivot</a>,<a href="http://technorati.com/tags/Panorama" rel="tag">Panorama</a>,<a href="http://technorati.com/tags/Application" rel="tag">Application</a>,<a href="http://technorati.com/tags/Name" rel="tag">Name</a>,<a href="http://technorati.com/tags/Grid" rel="tag">Grid</a>,<a href="http://technorati.com/tags/MapItemsControl" rel="tag">MapItemsControl</a>,<a href="http://technorati.com/tags/ItemsSource" rel="tag">ItemsSource</a>,<a href="http://technorati.com/tags/PushPins" rel="tag">PushPins</a>,<a href="http://technorati.com/tags/ItemTemplate" rel="tag">ItemTemplate</a>,<a href="http://technorati.com/tags/DataTemplate" rel="tag">DataTemplate</a>,<a href="http://technorati.com/tags/Pushpin" rel="tag">Pushpin</a>,<a href="http://technorati.com/tags/Location" rel="tag">Location</a>,<a href="http://technorati.com/tags/Template" rel="tag">Template</a>,<a href="http://technorati.com/tags/StaticResource" rel="tag">StaticResource</a>,<a href="http://technorati.com/tags/From" rel="tag">From</a>,<a href="http://technorati.com/tags/ItemSource" rel="tag">ItemSource</a>,<a href="http://technorati.com/tags/collection" rel="tag">collection</a>,<a href="http://technorati.com/tags/item" rel="tag">item</a>,<a href="http://technorati.com/tags/custom" rel="tag">custom</a>,<a href="http://technorati.com/tags/PhoneApplicationPage" rel="tag">PhoneApplicationPage</a>,<a href="http://technorati.com/tags/Resources" rel="tag">Resources</a>,<a href="http://technorati.com/tags/ControlTemplate" rel="tag">ControlTemplate</a>,<a href="http://technorati.com/tags/TargetType" rel="tag">TargetType</a>,<a href="http://technorati.com/tags/Width" rel="tag">Width</a>,<a href="http://technorati.com/tags/Ellipse" rel="tag">Ellipse</a>,<a href="http://technorati.com/tags/Fill" rel="tag">Fill</a>,<a href="http://technorati.com/tags/Black" rel="tag">Black</a>,<a href="http://technorati.com/tags/Margin" rel="tag">Margin</a>,<a href="http://technorati.com/tags/Image" rel="tag">Image</a>,<a href="http://technorati.com/tags/Source" rel="tag">Source</a>,<a href="http://technorati.com/tags/PinSource" rel="tag">PinSource</a>,<a href="http://technorati.com/tags/data" rel="tag">data</a>,<a href="http://technorati.com/tags/Here" rel="tag">Here</a>,<a href="http://technorati.com/tags/ViewModel" rel="tag">ViewModel</a>,<a href="http://technorati.com/tags/event" rel="tag">event</a>,<a href="http://technorati.com/tags/PropertyChangedEventHandler" rel="tag">PropertyChangedEventHandler</a>,<a href="http://technorati.com/tags/PropertyChangedEventArgs" rel="tag">PropertyChangedEventArgs</a>,<a href="http://technorati.com/tags/ObservableCollection" rel="tag">ObservableCollection</a>,<a href="http://technorati.com/tags/PushPinModel" rel="tag">PushPinModel</a>,<a href="http://technorati.com/tags/_pushPins" rel="tag">_pushPins</a>,<a href="http://technorati.com/tags/Very" rel="tag">Very</a>,<a href="http://technorati.com/tags/GeoCoordinate" rel="tag">GeoCoordinate</a>,<a href="http://technorati.com/tags/_location" rel="tag">_location</a>,<a href="http://technorati.com/tags/_pinSource" rel="tag">_pinSource</a>,<a href="http://technorati.com/tags/Again" rel="tag">Again</a>,<a href="http://technorati.com/tags/DataContext" rel="tag">DataContext</a>,<a href="http://technorati.com/tags/ApplicationIcon" rel="tag">ApplicationIcon</a>,<a href="http://technorati.com/tags/result" rel="tag">result</a>,<a href="http://technorati.com/tags/propertyName" rel="tag">propertyName</a>

<br />


WordPress Tags: <a href="http://wordpress.com/tag/BingMaps" rel="Tag">BingMaps</a>,<a href="http://wordpress.com/tag/Control" rel="Tag">Control</a>,<a href="http://wordpress.com/tag/MVVM" rel="Tag">MVVM</a>,<a href="http://wordpress.com/tag/treatment" rel="Tag">treatment</a>,<a href="http://wordpress.com/tag/Pivot" rel="Tag">Pivot</a>,<a href="http://wordpress.com/tag/Panorama" rel="Tag">Panorama</a>,<a href="http://wordpress.com/tag/Application" rel="Tag">Application</a>,<a href="http://wordpress.com/tag/Name" rel="Tag">Name</a>,<a href="http://wordpress.com/tag/Grid" rel="Tag">Grid</a>,<a href="http://wordpress.com/tag/MapItemsControl" rel="Tag">MapItemsControl</a>,<a href="http://wordpress.com/tag/ItemsSource" rel="Tag">ItemsSource</a>,<a href="http://wordpress.com/tag/PushPins" rel="Tag">PushPins</a>,<a href="http://wordpress.com/tag/ItemTemplate" rel="Tag">ItemTemplate</a>,<a href="http://wordpress.com/tag/DataTemplate" rel="Tag">DataTemplate</a>,<a href="http://wordpress.com/tag/Pushpin" rel="Tag">Pushpin</a>,<a href="http://wordpress.com/tag/Location" rel="Tag">Location</a>,<a href="http://wordpress.com/tag/Template" rel="Tag">Template</a>,<a href="http://wordpress.com/tag/StaticResource" rel="Tag">StaticResource</a>,<a href="http://wordpress.com/tag/From" rel="Tag">From</a>,<a href="http://wordpress.com/tag/ItemSource" rel="Tag">ItemSource</a>,<a href="http://wordpress.com/tag/collection" rel="Tag">collection</a>,<a href="http://wordpress.com/tag/item" rel="Tag">item</a>,<a href="http://wordpress.com/tag/custom" rel="Tag">custom</a>,<a href="http://wordpress.com/tag/PhoneApplicationPage" rel="Tag">PhoneApplicationPage</a>,<a href="http://wordpress.com/tag/Resources" rel="Tag">Resources</a>,<a href="http://wordpress.com/tag/ControlTemplate" rel="Tag">ControlTemplate</a>,<a href="http://wordpress.com/tag/TargetType" rel="Tag">TargetType</a>,<a href="http://wordpress.com/tag/Width" rel="Tag">Width</a>,<a href="http://wordpress.com/tag/Ellipse" rel="Tag">Ellipse</a>,<a href="http://wordpress.com/tag/Fill" rel="Tag">Fill</a>,<a href="http://wordpress.com/tag/Black" rel="Tag">Black</a>,<a href="http://wordpress.com/tag/Margin" rel="Tag">Margin</a>,<a href="http://wordpress.com/tag/Image" rel="Tag">Image</a>,<a href="http://wordpress.com/tag/Source" rel="Tag">Source</a>,<a href="http://wordpress.com/tag/PinSource" rel="Tag">PinSource</a>,<a href="http://wordpress.com/tag/data" rel="Tag">data</a>,<a href="http://wordpress.com/tag/Here" rel="Tag">Here</a>,<a href="http://wordpress.com/tag/ViewModel" rel="Tag">ViewModel</a>,<a href="http://wordpress.com/tag/event" rel="Tag">event</a>,<a href="http://wordpress.com/tag/PropertyChangedEventHandler" rel="Tag">PropertyChangedEventHandler</a>,<a href="http://wordpress.com/tag/PropertyChangedEventArgs" rel="Tag">PropertyChangedEventArgs</a>,<a href="http://wordpress.com/tag/ObservableCollection" rel="Tag">ObservableCollection</a>,<a href="http://wordpress.com/tag/PushPinModel" rel="Tag">PushPinModel</a>,<a href="http://wordpress.com/tag/_pushPins" rel="Tag">_pushPins</a>,<a href="http://wordpress.com/tag/Very" rel="Tag">Very</a>,<a href="http://wordpress.com/tag/GeoCoordinate" rel="Tag">GeoCoordinate</a>,<a href="http://wordpress.com/tag/_location" rel="Tag">_location</a>,<a href="http://wordpress.com/tag/_pinSource" rel="Tag">_pinSource</a>,<a href="http://wordpress.com/tag/Again" rel="Tag">Again</a>,<a href="http://wordpress.com/tag/DataContext" rel="Tag">DataContext</a>,<a href="http://wordpress.com/tag/ApplicationIcon" rel="Tag">ApplicationIcon</a>,<a href="http://wordpress.com/tag/result" rel="Tag">result</a>,<a href="http://wordpress.com/tag/propertyName" rel="Tag">propertyName</a>
