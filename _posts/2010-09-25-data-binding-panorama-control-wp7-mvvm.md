---
layout: single
title: Data Binding Panorama Control WP7 MVVM
date: 2010-09-25 12:31
author_profile: true
comments: true
categories: [MVVM, Windows Phone 7]
header:
    teaserlogo: 'assets/images/'
    teaser: 'assets/images/'
---
<p></p>
<div id="msgcns!4F1B7368284539E5!306" class="bvMsg">
<p>by Peter Daukintis</p>
<p>Ok, this is just a quick follow-up to a previous post <a target="_blank" href="http://babaandthepigman.spaces.live.com/blog/cns!4F1B7368284539E5!298.entry">Data Binding Pivot Control</a> just as confirmation that this approach will work for the WP7 Panorama Control as well. Starting with the project from the previous post, I literally opened the xaml and changed the word ‘pivot’ to ‘panorama’ to end with this:</p>
<pre style="background-color:#202020;">        <span style="color:#d2d200;">&lt;controls:Panorama ItemsSource="{</span><span style="color:cyan;">Binding </span><span style="color:white;">PageCollection</span><span style="color:#d2d200;">}</span><span style="color:white;">"
                        </span><span style="color:#d2d200;">Title=</span><span style="color:white;">"MY APPLICATION"
                        </span><span style="color:#d2d200;">HorizontalContentAlignment=</span><span style="color:white;">"Stretch"
                        </span><span style="color:#d2d200;">VerticalContentAlignment=</span><span style="color:white;">"Stretch"
                        </span><span style="color:#d2d200;">&gt;
            &lt;controls:Panorama.HeaderTemplate&gt;
                &lt;DataTemplate&gt;
                    &lt;Grid x:Name=</span><span style="color:white;">"grid"</span><span style="color:#d2d200;">&gt;
                        &lt;TextBlock TextWrapping=</span><span style="color:white;">"Wrap"
                                   </span><span style="color:#d2d200;">Text="{</span><span style="color:cyan;">Binding </span><span style="color:white;">TitleText</span><span style="color:#d2d200;">}</span><span style="color:white;">"
                                   </span><span style="color:#d2d200;">d:LayoutOverrides=</span><span style="color:white;">"Width, Height" </span><span style="color:#d2d200;">/&gt;
                    &lt;/Grid&gt;
                &lt;/DataTemplate&gt;
            &lt;/controls:Panorama.HeaderTemplate&gt;
            &lt;controls:Panorama.ItemTemplate&gt;
                &lt;DataTemplate&gt;
                    &lt;WindowsPhonePivotApplication1:DataTemplateSelector Content="{</span><span style="color:cyan;">Binding</span><span style="color:#d2d200;">}</span><span style="color:white;">" </span><span style="color:#d2d200;">/&gt;
                &lt;/DataTemplate&gt;
            &lt;/controls:Panorama.ItemTemplate&gt;
        &lt;/controls:Panorama&gt;

</span></pre>
<p>and that was it!</p>
<p><a href="https://omlweq.bay.livefilestore.com/y1mbFrXevqQ4UkrO6Usm8xRkMpaiDhR6xLy4Zt_wgFaMyot_wwhY1pij5B3QrAA65_hJVi7uKp26FOoDz0YWshD1xqxf0fw_LVRevCDZXcE7VuAKl21LlCRBBNm4r2W9sOJpsFZp8ZpAKTAIT8wH0L1EA/panobinding[4].png?download&amp;psid=1" rel="WLPP"><img title="panobinding" style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border:0;" src="{{ site.baseurl }}/assets/images/2010/09/panobinding_thumb5b25d.png?w=167" border="0" alt="panobinding" width="296" height="530" align="left" /></a></p>
<p><a href="https://omlweq.bay.livefilestore.com/y1mFl-ppWZLk-XC8V47q8nyU-AEpfQczbY9443s4YP1_1xwKkcubAnoM5wGzbtZAR13YIu0TWr6mlSNA9pTbYW3jRqeMcMXDDAvZCHrmUNvEEHQ3DXtY_Bo9CkcPssNSMNR5dcpz43kXCtgl0a5hgjUkw/panobinding2[4].png?download&amp;psid=1" rel="WLPP"><img title="panobinding2" style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border:0;" src="https://omlweq.bay.livefilestore.com/y1ma7RA1SX38koMkzk8oBqR9VHpG8hgxC7Ax06dFHCHKD0jbvUcOwjk4uuleHHcwilXOMYCxtw3co_yDxzWS14om8HN71ddJh9BAaZUtpiY3u5O-3yjvZbviQvmM60SvWDRhiZPxrjZFbB4OTJb7Siznw/panobinding2_thumb[2].png?download&amp;psid=1" border="0" alt="panobinding2" width="310" height="532" align="left" /></a></p>
<p>Technorati Tags: <a href="http://technorati.com/tags/Data" rel="tag">Data</a>,<a href="http://technorati.com/tags/Panorama" rel="tag">Panorama</a>,<a href="http://technorati.com/tags/Control" rel="tag">Control</a>,<a href="http://technorati.com/tags/MVVM" rel="tag">MVVM</a>,<a href="http://technorati.com/tags/Beta" rel="tag">Beta</a>,<a href="http://technorati.com/tags/Pivot" rel="tag">Pivot</a>,<a href="http://technorati.com/tags/confirmation" rel="tag">confirmation</a>,<a href="http://technorati.com/tags/ItemsSource" rel="tag">ItemsSource</a>,<a href="http://technorati.com/tags/PageCollection" rel="tag">PageCollection</a>,<a href="http://technorati.com/tags/Title" rel="tag">Title</a>,<a href="http://technorati.com/tags/APPLICATION" rel="tag">APPLICATION</a>,<a href="http://technorati.com/tags/HorizontalContentAlignment" rel="tag">HorizontalContentAlignment</a>,<a href="http://technorati.com/tags/Stretch" rel="tag">Stretch</a>,<a href="http://technorati.com/tags/VerticalContentAlignment" rel="tag">VerticalContentAlignment</a>,<a href="http://technorati.com/tags/HeaderTemplate" rel="tag">HeaderTemplate</a>,<a href="http://technorati.com/tags/DataTemplate" rel="tag">DataTemplate</a>,<a href="http://technorati.com/tags/Grid" rel="tag">Grid</a>,<a href="http://technorati.com/tags/Name" rel="tag">Name</a>,<a href="http://technorati.com/tags/TextBlock" rel="tag">TextBlock</a>,<a href="http://technorati.com/tags/Wrap" rel="tag">Wrap</a>,<a href="http://technorati.com/tags/Text" rel="tag">Text</a>,<a href="http://technorati.com/tags/TitleText" rel="tag">TitleText</a>,<a href="http://technorati.com/tags/LayoutOverrides" rel="tag">LayoutOverrides</a>,<a href="http://technorati.com/tags/Width" rel="tag">Width</a>,<a href="http://technorati.com/tags/ItemTemplate" rel="tag">ItemTemplate</a>,<a href="http://technorati.com/tags/DataTemplateSelector" rel="tag">DataTemplateSelector</a>,<a href="http://technorati.com/tags/Content" rel="tag">Content</a></p>
<p>Windows Live Tags: <a href="http://windows.live.com/connect/tag/Data" rel="clubhouseTag">Data</a>,<a href="http://windows.live.com/connect/tag/Panorama" rel="clubhouseTag">Panorama</a>,<a href="http://windows.live.com/connect/tag/Control" rel="clubhouseTag">Control</a>,<a href="http://windows.live.com/connect/tag/MVVM" rel="clubhouseTag">MVVM</a>,<a href="http://windows.live.com/connect/tag/Beta" rel="clubhouseTag">Beta</a>,<a href="http://windows.live.com/connect/tag/Pivot" rel="clubhouseTag">Pivot</a>,<a href="http://windows.live.com/connect/tag/confirmation" rel="clubhouseTag">confirmation</a>,<a href="http://windows.live.com/connect/tag/ItemsSource" rel="clubhouseTag">ItemsSource</a>,<a href="http://windows.live.com/connect/tag/PageCollection" rel="clubhouseTag">PageCollection</a>,<a href="http://windows.live.com/connect/tag/Title" rel="clubhouseTag">Title</a>,<a href="http://windows.live.com/connect/tag/APPLICATION" rel="clubhouseTag">APPLICATION</a>,<a href="http://windows.live.com/connect/tag/HorizontalContentAlignment" rel="clubhouseTag">HorizontalContentAlignment</a>,<a href="http://windows.live.com/connect/tag/Stretch" rel="clubhouseTag">Stretch</a>,<a href="http://windows.live.com/connect/tag/VerticalContentAlignment" rel="clubhouseTag">VerticalContentAlignment</a>,<a href="http://windows.live.com/connect/tag/HeaderTemplate" rel="clubhouseTag">HeaderTemplate</a>,<a href="http://windows.live.com/connect/tag/DataTemplate" rel="clubhouseTag">DataTemplate</a>,<a href="http://windows.live.com/connect/tag/Grid" rel="clubhouseTag">Grid</a>,<a href="http://windows.live.com/connect/tag/Name" rel="clubhouseTag">Name</a>,<a href="http://windows.live.com/connect/tag/TextBlock" rel="clubhouseTag">TextBlock</a>,<a href="http://windows.live.com/connect/tag/Wrap" rel="clubhouseTag">Wrap</a>,<a href="http://windows.live.com/connect/tag/Text" rel="clubhouseTag">Text</a>,<a href="http://windows.live.com/connect/tag/TitleText" rel="clubhouseTag">TitleText</a>,<a href="http://windows.live.com/connect/tag/LayoutOverrides" rel="clubhouseTag">LayoutOverrides</a>,<a href="http://windows.live.com/connect/tag/Width" rel="clubhouseTag">Width</a>,<a href="http://windows.live.com/connect/tag/ItemTemplate" rel="clubhouseTag">ItemTemplate</a>,<a href="http://windows.live.com/connect/tag/DataTemplateSelector" rel="clubhouseTag">DataTemplateSelector</a>,<a href="http://windows.live.com/connect/tag/Content" rel="clubhouseTag">Content</a></p>
</div>

