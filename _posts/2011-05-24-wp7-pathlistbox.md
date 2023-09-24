---
layout: post
title: wp7 pathlistbox
date: 2011-05-24 23:59
author: peted70
comments: true
categories: [Windows Phone 7]
---
<p>Since the new Mango developer tools were just released I wanted to write a quick post on PathListBox since I have been looking for an excuse to give it a try. PathListBox allows layout of list box items along an arbitrary path (see <a href="http://msdn.microsoft.com/en-us/library/microsoft.expression.controls.pathlistbox(v=expression.40).aspx">PathListBox</a>). So, after installing the Mango developer tools <a title="http://www.microsoft.com/downloads/en/details.aspx?FamilyID=77586864-ab15-40e1-bc38-713a95a56a05&amp;displaylang=en" href="http://www.microsoft.com/downloads/en/details.aspx?FamilyID=77586864-ab15-40e1-bc38-713a95a56a05&amp;displaylang=en">http://www.microsoft.com/downloads/en/details.aspx?FamilyID=77586864-ab15-40e1-bc38-713a95a56a05&amp;displaylang=en</a>, I created a standard Windows Phone 7 project (targeting 7.1). I opened it in Blend and using the pen tool drew a path and removed any fill or stroke colours…</p>  <p>&#160;</p>  <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox1.png"><img style="background-image:none;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;margin-right:auto;padding-top:0;border-width:0;" title="pathlistbox1" border="0" alt="pathlistbox1" src="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox1_thumb.png" width="411" height="584" /></a></p>  <p>&#160;</p>  <p>I then added a PathListBox from the tool box and in it’s property pane, under the Layout Paths section clicked the target icon next to ‘Select an object to use as a Layout path’, and then choose the path. Now any items subsequently added to the PathListBox will be positioned on this path. </p>  <p>&#160;</p>  <p>I created a ViewModel class and bound it to the DataContext of the MainPage…</p>  <p>&#160;</p>  <pre class="code">        <span style="color:#dac6a5;">public </span><span style="color:#d7d7c8;">MainPage()
        {
            DataContext = </span><span style="color:#dac6a5;">new </span><span style="color:#efefaf;">ViewModel</span><span style="color:#d7d7c8;">();            
            InitializeComponent();
        }

</span></pre>


<div id="codeSnippetWrapper">&#160;</div>

<p>The ViewModel and Items started out like the following:</p>

<p>&#160;</p>

<pre class="code"><span style="color:#dac6a5;">public class </span><span style="color:#efefaf;">ViewModel
</span><span style="color:#d7d7c8;">{
    </span><span style="color:#dac6a5;">private int </span><span style="color:#d7d7c8;">_count;
    </span><span style="color:#dac6a5;">public </span><span style="color:#d7d7c8;">ObservableCollection&lt;Item&gt; Items { </span><span style="color:#dac6a5;">get</span><span style="color:#d7d7c8;">; </span><span style="color:#dac6a5;">set</span><span style="color:#d7d7c8;">; }

    </span><span style="color:#dac6a5;">public </span><span style="color:#d7d7c8;">ViewModel()
    {
        Items = </span><span style="color:#dac6a5;">new </span><span style="color:#d7d7c8;">ObservableCollection&lt;Item&gt;();
        AddItem();
        AddItem();
        AddItem();
        AddItem();
        AddItem();
        AddItem();
    }

    </span><span style="color:#dac6a5;">public void </span><span style="color:#d7d7c8;">AddItem(</span><span style="color:#dac6a5;">object </span><span style="color:#d7d7c8;">parameter = </span><span style="color:#dac6a5;">null</span><span style="color:#d7d7c8;">)
    {
        Items.Add(</span><span style="color:#dac6a5;">new </span><span style="color:#d7d7c8;">Item { Name = </span><span style="color:#dca3a3;">&quot;Item &quot; </span><span style="color:#d7d7c8;">+ _count++ });
    }

    </span><span style="color:#dac6a5;">public void </span><span style="color:#d7d7c8;">RemoveItem(</span><span style="color:#dac6a5;">object </span><span style="color:#d7d7c8;">parameter)
    {
        Items.RemoveAt(Items.Count - </span><span style="color:#8cd0d3;">1</span><span style="color:#d7d7c8;">);
    }
}

</span><span style="color:#dac6a5;">public class </span><span style="color:#efefaf;">Item
</span><span style="color:#d7d7c8;">{
    </span><span style="color:#dac6a5;">public string </span><span style="color:#d7d7c8;">Name { </span><span style="color:#dac6a5;">get</span><span style="color:#d7d7c8;">; </span><span style="color:#dac6a5;">set</span><span style="color:#d7d7c8;">; }
}

</span></pre>


<div id="codeSnippetWrapper">&#160;</div>

<p>which gave some data to bind the view to.</p>

<p>I then bound the ItemsSource of the PathListBox to our Items list in xaml and edited the Distribution property to give an even layout along the path, as follows:</p>

<p>&#160;</p>

<pre class="code">           <span style="color:#d7d7c8;">&lt;mec:PathListBox HorizontalAlignment=</span><span style="color:#dca3a3;">&quot;Left&quot;
                             </span><span style="color:#d7d7c8;">Height=</span><span style="color:#dca3a3;">&quot;100&quot;
                             </span><span style="color:#d7d7c8;">VerticalAlignment=</span><span style="color:#dca3a3;">&quot;Top&quot;
                             </span><span style="color:#d7d7c8;">Width=</span><span style="color:#dca3a3;">&quot;100&quot;
                             </span><span style="color:#d7d7c8;">ItemsSource=</span><span style="color:#dca3a3;">&quot;{Binding Items}&quot;</span><span style="color:#d7d7c8;">&gt;
                &lt;mec:PathListBox.LayoutPaths&gt;
                    &lt;mec:LayoutPath SourceElement=</span><span style="color:#dca3a3;">&quot;{Binding ElementName=path}&quot; </span><span style="color:#d7d7c8;">Distribution=</span><span style="color:#dca3a3;">&quot;Even&quot;</span><span style="color:#d7d7c8;">/&gt;
                &lt;/mec:PathListBox.LayoutPaths&gt;
            &lt;/mec:PathListBox&gt;

</span></pre>


<div id="codeSnippetWrapper">&#160;</div>

<p>At this stage, running in the Emulator gave,</p>

<p>&#160;</p>

<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox3.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;padding-top:0;" title="pathlistbox3" border="0" alt="pathlistbox3" src="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox3_thumb.png" width="276" height="452" /></a></p>



<p>So, we need to tell the ListBox how to display each data item. We can do this in Blend by editing the PayjListBox’s ItemTemplate. I just added a TextBlock bound to the Name property of the Item class.</p>

<p>&#160;</p>

<pre class="code">        <span style="color:#efefaf;">&lt;</span><span style="color:#dac6a5;">DataTemplate </span><span style="color:#efefaf;">x:Key=</span><span style="color:#dca3a3;">&quot;DataTemplate1&quot;</span><span style="color:#efefaf;">&gt;
            &lt;</span><span style="color:#dac6a5;">Grid</span><span style="color:#efefaf;">&gt;
                &lt;</span><span style="color:#dac6a5;">TextBlock </span><span style="color:#efefaf;">Text=&quot;{</span><span style="color:#dac6a5;">Binding </span><span style="color:#efefaf;">Name}</span><span style="color:#dca3a3;">&quot; </span><span style="color:#efefaf;">/&gt;
            &lt;/</span><span style="color:#dac6a5;">Grid</span><span style="color:#efefaf;">&gt;
        &lt;/</span><span style="color:#dac6a5;">DataTemplate</span><span style="color:#efefaf;">&gt;

</span></pre>


<div id="codeSnippetWrapper">&#160;</div>

<p>This gave,</p>

<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox4.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;padding-top:0;" title="pathlistbox4" border="0" alt="pathlistbox4" src="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox4_thumb.png" width="265" height="455" /></a></p>

<p>&#160;</p>

<p>Which is a fairly interesting list box, but what really brings it to life is adding a FluidMoveBehavior to the Layout Panel and then triggering some layout updates. I did this as follows:</p>

<p>Edit the PathListBox’s ItemPanel template in Blend and drag n drop a FluidMoveBehavior (from the Assets Pane) onto the PathPanel.</p>

<p>&#160;</p>

<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox5.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;padding-top:0;" title="pathlistbox5" border="0" alt="pathlistbox5" src="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox5_thumb.png" width="473" height="205" /></a></p>

<p>&#160;</p>

<p>You can select the FluidMoveBehavior and edit some of it’s properties to control the nature of the transitions it will produce.</p>

<p>&#160;</p>

<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox6.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;padding-top:0;" title="pathlistbox6" border="0" alt="pathlistbox6" src="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox6_thumb.png" width="333" height="403" /></a></p>

<p>&#160;</p>

<p>You must set the ‘Applies To’ property to ‘Children’ so that the transitions are applied to the children of the layout panel. In order to see the animations I added some buttons for sorting the list and adding and removing items from the list. I made use of the new Commanding support and added ICommand implementing properties on my ViewModel which I bound to the buttons.</p>

<p>&#160;</p>

<pre class="code">            <span style="color:#efefaf;">&lt;</span><span style="color:#dac6a5;">StackPanel </span><span style="color:#efefaf;">Orientation=</span><span style="color:#dca3a3;">&quot;Horizontal&quot;
                        </span><span style="color:#efefaf;">Grid.Row=</span><span style="color:#dca3a3;">&quot;1&quot;
                        </span><span style="color:#efefaf;">d:IsHidden=</span><span style="color:#dca3a3;">&quot;True&quot;</span><span style="color:#efefaf;">&gt;
                &lt;</span><span style="color:#dac6a5;">Button </span><span style="color:#efefaf;">Content=</span><span style="color:#dca3a3;">&quot;Sort&quot;
                        </span><span style="color:#efefaf;">Command=&quot;{</span><span style="color:#dac6a5;">Binding </span><span style="color:#efefaf;">SortCommand}</span><span style="color:#dca3a3;">&quot; </span><span style="color:#efefaf;">/&gt;
                &lt;</span><span style="color:#dac6a5;">Button </span><span style="color:#efefaf;">Content=</span><span style="color:#dca3a3;">&quot;+&quot;
                        </span><span style="color:#efefaf;">Command=&quot;{</span><span style="color:#dac6a5;">Binding </span><span style="color:#efefaf;">AddCommand}</span><span style="color:#dca3a3;">&quot; </span><span style="color:#efefaf;">/&gt;
                &lt;</span><span style="color:#dac6a5;">Button </span><span style="color:#efefaf;">Content=</span><span style="color:#dca3a3;">&quot;-&quot;
                        </span><span style="color:#efefaf;">Command=&quot;{</span><span style="color:#dac6a5;">Binding </span><span style="color:#efefaf;">RemoveCommand}</span><span style="color:#dca3a3;">&quot; </span><span style="color:#efefaf;">/&gt;
            &lt;/</span><span style="color:#dac6a5;">StackPanel</span><span style="color:#efefaf;">&gt;
</span></pre>


<div id="codeSnippetWrapper">&#160;</div>

<pre class="code">            <span style="color:#d7d7c8;">SortCommand = </span><span style="color:#dac6a5;">new </span><span style="color:#efefaf;">ProxyCmd</span><span style="color:#d7d7c8;">(Sort);
            RemoveCommand = </span><span style="color:#dac6a5;">new </span><span style="color:#efefaf;">ProxyCmd</span><span style="color:#d7d7c8;">(RemoveItem);
            AddCommand = </span><span style="color:#dac6a5;">new </span><span style="color:#efefaf;">ProxyCmd</span><span style="color:#d7d7c8;">(AddItem);

</span></pre>


<p>&#160;</p>

<div id="codeSnippetWrapper">Where ProxyCmd is just a class which implements ICommand by calling a delegate that you pass it. Also, the sorting was implemented using CollectionViewSource.</div>

<p>&#160;</p>

<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox7.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:block;float:none;margin-left:auto;border-top:0;margin-right:auto;border-right:0;padding-top:0;" title="pathlistbox7" border="0" alt="pathlistbox7" src="http://peted.azurewebsites.net/wp-content/uploads/2011/05/pathlistbox7_thumb.png" width="342" height="578" /></a></p>

<p>&#160;</p>

<p>The result is a a pleasing, dynamic, fully functioning ListBox. </p>

<p>The source code is available here <a title="http://cid-4f1b7368284539e5.office.live.com/self.aspx/.Public/MangoPathListBox/MangoPathListBox.zip" href="http://cid-4f1b7368284539e5.office.live.com/self.aspx/.Public/MangoPathListBox/MangoPathListBox.zip">http://cid-4f1b7368284539e5.office.live.com/self.aspx/.Public/MangoPathListBox/MangoPathListBox.zip</a></p>

<p>&#160;</p>

<p>&#160;</p>


Technorati Tags: <a href="http://technorati.com/tags/Mango" rel="tag">Mango</a>,<a href="http://technorati.com/tags/tools" rel="tag">tools</a>,<a href="http://technorati.com/tags/PathListBox" rel="tag">PathListBox</a>,<a href="http://technorati.com/tags/items" rel="tag">items</a>,<a href="http://technorati.com/tags/path" rel="tag">path</a>,<a href="http://technorati.com/tags/FamilyID" rel="tag">FamilyID</a>,<a href="http://technorati.com/tags/Blend" rel="tag">Blend</a>,<a href="http://technorati.com/tags/tool" rel="tag">tool</a>,<a href="http://technorati.com/tags/pane" rel="tag">pane</a>,<a href="http://technorati.com/tags/Layout" rel="tag">Layout</a>,<a href="http://technorati.com/tags/icon" rel="tag">icon</a>,<a href="http://technorati.com/tags/Select" rel="tag">Select</a>,<a href="http://technorati.com/tags/ViewModel" rel="tag">ViewModel</a>,<a href="http://technorati.com/tags/DataContext" rel="tag">DataContext</a>,<a href="http://technorati.com/tags/MainPage" rel="tag">MainPage</a>,<a href="http://technorati.com/tags/InitializeComponent" rel="tag">InitializeComponent</a>,<a href="http://technorati.com/tags/_count" rel="tag">_count</a>,<a href="http://technorati.com/tags/ObservableCollection" rel="tag">ObservableCollection</a>,<a href="http://technorati.com/tags/Item" rel="tag">Item</a>,<a href="http://technorati.com/tags/AddItem" rel="tag">AddItem</a>,<a href="http://technorati.com/tags/parameter" rel="tag">parameter</a>,<a href="http://technorati.com/tags/Name" rel="tag">Name</a>,<a href="http://technorati.com/tags/RemoveItem" rel="tag">RemoveItem</a>,<a href="http://technorati.com/tags/RemoveAt" rel="tag">RemoveAt</a>,<a href="http://technorati.com/tags/Count" rel="tag">Count</a>,<a href="http://technorati.com/tags/data" rel="tag">data</a>,<a href="http://technorati.com/tags/ItemsSource" rel="tag">ItemsSource</a>,<a href="http://technorati.com/tags/Distribution" rel="tag">Distribution</a>,<a href="http://technorati.com/tags/HorizontalAlignment" rel="tag">HorizontalAlignment</a>,<a href="http://technorati.com/tags/Left" rel="tag">Left</a>,<a href="http://technorati.com/tags/VerticalAlignment" rel="tag">VerticalAlignment</a>,<a href="http://technorati.com/tags/Width" rel="tag">Width</a>,<a href="http://technorati.com/tags/LayoutPaths" rel="tag">LayoutPaths</a>,<a href="http://technorati.com/tags/LayoutPath" rel="tag">LayoutPath</a>,<a href="http://technorati.com/tags/SourceElement" rel="tag">SourceElement</a>,<a href="http://technorati.com/tags/ElementName" rel="tag">ElementName</a>,<a href="http://technorati.com/tags/Emulator" rel="tag">Emulator</a>,<a href="http://technorati.com/tags/ListBox" rel="tag">ListBox</a>,<a href="http://technorati.com/tags/PayjListBox" rel="tag">PayjListBox</a>,<a href="http://technorati.com/tags/ItemTemplate" rel="tag">ItemTemplate</a>,<a href="http://technorati.com/tags/TextBlock" rel="tag">TextBlock</a>,<a href="http://technorati.com/tags/DataTemplate" rel="tag">DataTemplate</a>,<a href="http://technorati.com/tags/Grid" rel="tag">Grid</a>,<a href="http://technorati.com/tags/Text" rel="tag">Text</a>,<a href="http://technorati.com/tags/life" rel="tag">life</a>,<a href="http://technorati.com/tags/FluidMoveBehavior" rel="tag">FluidMoveBehavior</a>,<a href="http://technorati.com/tags/Panel" rel="tag">Panel</a>,<a href="http://technorati.com/tags/Edit" rel="tag">Edit</a>,<a href="http://technorati.com/tags/ItemPanel" rel="tag">ItemPanel</a>,<a href="http://technorati.com/tags/template" rel="tag">template</a>,<a href="http://technorati.com/tags/Assets" rel="tag">Assets</a>,<a href="http://technorati.com/tags/PathPanel" rel="tag">PathPanel</a>,<a href="http://technorati.com/tags/nature" rel="tag">nature</a>,<a href="http://technorati.com/tags/Children" rel="tag">Children</a>,<a href="http://technorati.com/tags/ICommand" rel="tag">ICommand</a>,<a href="http://technorati.com/tags/StackPanel" rel="tag">StackPanel</a>,<a href="http://technorati.com/tags/Orientation" rel="tag">Orientation</a>,<a href="http://technorati.com/tags/Horizontal" rel="tag">Horizontal</a>,<a href="http://technorati.com/tags/IsHidden" rel="tag">IsHidden</a>,<a href="http://technorati.com/tags/True" rel="tag">True</a>,<a href="http://technorati.com/tags/Button" rel="tag">Button</a>,<a href="http://technorati.com/tags/Content" rel="tag">Content</a>,<a href="http://technorati.com/tags/Sort" rel="tag">Sort</a>,<a href="http://technorati.com/tags/Command" rel="tag">Command</a>,<a href="http://technorati.com/tags/SortCommand" rel="tag">SortCommand</a>,<a href="http://technorati.com/tags/AddCommand" rel="tag">AddCommand</a>,<a href="http://technorati.com/tags/RemoveCommand" rel="tag">RemoveCommand</a>,<a href="http://technorati.com/tags/ProxyCmd" rel="tag">ProxyCmd</a>,<a href="http://technorati.com/tags/Where" rel="tag">Where</a>,<a href="http://technorati.com/tags/implements" rel="tag">implements</a>,<a href="http://technorati.com/tags/Also" rel="tag">Also</a>,<a href="http://technorati.com/tags/CollectionViewSource" rel="tag">CollectionViewSource</a>,<a href="http://technorati.com/tags/result" rel="tag">result</a>,<a href="http://technorati.com/tags/Paths" rel="tag">Paths</a>,<a href="http://technorati.com/tags/transitions" rel="tag">transitions</a>,<a href="http://technorati.com/tags/animations" rel="tag">animations</a>,<a href="http://technorati.com/tags/developer" rel="tag">developer</a>

<br />


WordPress Tags: <a href="http://wordpress.com/tag/Mango" rel="Tag">Mango</a>,<a href="http://wordpress.com/tag/tools" rel="Tag">tools</a>,<a href="http://wordpress.com/tag/PathListBox" rel="Tag">PathListBox</a>,<a href="http://wordpress.com/tag/items" rel="Tag">items</a>,<a href="http://wordpress.com/tag/path" rel="Tag">path</a>,<a href="http://wordpress.com/tag/FamilyID" rel="Tag">FamilyID</a>,<a href="http://wordpress.com/tag/Blend" rel="Tag">Blend</a>,<a href="http://wordpress.com/tag/tool" rel="Tag">tool</a>,<a href="http://wordpress.com/tag/pane" rel="Tag">pane</a>,<a href="http://wordpress.com/tag/Layout" rel="Tag">Layout</a>,<a href="http://wordpress.com/tag/icon" rel="Tag">icon</a>,<a href="http://wordpress.com/tag/Select" rel="Tag">Select</a>,<a href="http://wordpress.com/tag/ViewModel" rel="Tag">ViewModel</a>,<a href="http://wordpress.com/tag/DataContext" rel="Tag">DataContext</a>,<a href="http://wordpress.com/tag/MainPage" rel="Tag">MainPage</a>,<a href="http://wordpress.com/tag/InitializeComponent" rel="Tag">InitializeComponent</a>,<a href="http://wordpress.com/tag/_count" rel="Tag">_count</a>,<a href="http://wordpress.com/tag/ObservableCollection" rel="Tag">ObservableCollection</a>,<a href="http://wordpress.com/tag/Item" rel="Tag">Item</a>,<a href="http://wordpress.com/tag/AddItem" rel="Tag">AddItem</a>,<a href="http://wordpress.com/tag/parameter" rel="Tag">parameter</a>,<a href="http://wordpress.com/tag/Name" rel="Tag">Name</a>,<a href="http://wordpress.com/tag/RemoveItem" rel="Tag">RemoveItem</a>,<a href="http://wordpress.com/tag/RemoveAt" rel="Tag">RemoveAt</a>,<a href="http://wordpress.com/tag/Count" rel="Tag">Count</a>,<a href="http://wordpress.com/tag/data" rel="Tag">data</a>,<a href="http://wordpress.com/tag/ItemsSource" rel="Tag">ItemsSource</a>,<a href="http://wordpress.com/tag/Distribution" rel="Tag">Distribution</a>,<a href="http://wordpress.com/tag/HorizontalAlignment" rel="Tag">HorizontalAlignment</a>,<a href="http://wordpress.com/tag/Left" rel="Tag">Left</a>,<a href="http://wordpress.com/tag/VerticalAlignment" rel="Tag">VerticalAlignment</a>,<a href="http://wordpress.com/tag/Width" rel="Tag">Width</a>,<a href="http://wordpress.com/tag/LayoutPaths" rel="Tag">LayoutPaths</a>,<a href="http://wordpress.com/tag/LayoutPath" rel="Tag">LayoutPath</a>,<a href="http://wordpress.com/tag/SourceElement" rel="Tag">SourceElement</a>,<a href="http://wordpress.com/tag/ElementName" rel="Tag">ElementName</a>,<a href="http://wordpress.com/tag/Emulator" rel="Tag">Emulator</a>,<a href="http://wordpress.com/tag/ListBox" rel="Tag">ListBox</a>,<a href="http://wordpress.com/tag/PayjListBox" rel="Tag">PayjListBox</a>,<a href="http://wordpress.com/tag/ItemTemplate" rel="Tag">ItemTemplate</a>,<a href="http://wordpress.com/tag/TextBlock" rel="Tag">TextBlock</a>,<a href="http://wordpress.com/tag/DataTemplate" rel="Tag">DataTemplate</a>,<a href="http://wordpress.com/tag/Grid" rel="Tag">Grid</a>,<a href="http://wordpress.com/tag/Text" rel="Tag">Text</a>,<a href="http://wordpress.com/tag/life" rel="Tag">life</a>,<a href="http://wordpress.com/tag/FluidMoveBehavior" rel="Tag">FluidMoveBehavior</a>,<a href="http://wordpress.com/tag/Panel" rel="Tag">Panel</a>,<a href="http://wordpress.com/tag/Edit" rel="Tag">Edit</a>,<a href="http://wordpress.com/tag/ItemPanel" rel="Tag">ItemPanel</a>,<a href="http://wordpress.com/tag/template" rel="Tag">template</a>,<a href="http://wordpress.com/tag/Assets" rel="Tag">Assets</a>,<a href="http://wordpress.com/tag/PathPanel" rel="Tag">PathPanel</a>,<a href="http://wordpress.com/tag/nature" rel="Tag">nature</a>,<a href="http://wordpress.com/tag/Children" rel="Tag">Children</a>,<a href="http://wordpress.com/tag/ICommand" rel="Tag">ICommand</a>,<a href="http://wordpress.com/tag/StackPanel" rel="Tag">StackPanel</a>,<a href="http://wordpress.com/tag/Orientation" rel="Tag">Orientation</a>,<a href="http://wordpress.com/tag/Horizontal" rel="Tag">Horizontal</a>,<a href="http://wordpress.com/tag/IsHidden" rel="Tag">IsHidden</a>,<a href="http://wordpress.com/tag/True" rel="Tag">True</a>,<a href="http://wordpress.com/tag/Button" rel="Tag">Button</a>,<a href="http://wordpress.com/tag/Content" rel="Tag">Content</a>,<a href="http://wordpress.com/tag/Sort" rel="Tag">Sort</a>,<a href="http://wordpress.com/tag/Command" rel="Tag">Command</a>,<a href="http://wordpress.com/tag/SortCommand" rel="Tag">SortCommand</a>,<a href="http://wordpress.com/tag/AddCommand" rel="Tag">AddCommand</a>,<a href="http://wordpress.com/tag/RemoveCommand" rel="Tag">RemoveCommand</a>,<a href="http://wordpress.com/tag/ProxyCmd" rel="Tag">ProxyCmd</a>,<a href="http://wordpress.com/tag/Where" rel="Tag">Where</a>,<a href="http://wordpress.com/tag/implements" rel="Tag">implements</a>,<a href="http://wordpress.com/tag/Also" rel="Tag">Also</a>,<a href="http://wordpress.com/tag/CollectionViewSource" rel="Tag">CollectionViewSource</a>,<a href="http://wordpress.com/tag/result" rel="Tag">result</a>,<a href="http://wordpress.com/tag/Paths" rel="Tag">Paths</a>,<a href="http://wordpress.com/tag/transitions" rel="Tag">transitions</a>,<a href="http://wordpress.com/tag/animations" rel="Tag">animations</a>,<a href="http://wordpress.com/tag/developer" rel="Tag">developer</a>
