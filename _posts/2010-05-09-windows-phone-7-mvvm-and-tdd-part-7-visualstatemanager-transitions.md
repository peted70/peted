---
layout: post
title: Windows Phone 7, MVVM and TDD (Part 7 – VisualStateManager transitions)
date: 2010-05-09 11:30
author: peted70
comments: true
categories: [MVVM, TDD, Windows Phone 7]
---
<div id="msgcns!4F1B7368284539E5!268" class="bvMsg"> <p>by Peter Daukintis</p> <p>So all that remains is to transition between the login ui to a welcome screen. I guess there are a number of ways of approaching this but I have decided to use the visual state manager. First, I take a copy of the ContentGrid and delete all of it’s contents, I add two text blocks – one with the text welcome in and the other data bound to the username text property in the view model.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7logincontentgrid3.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7LoginContentGrid" border="0" alt="WP7LoginContentGrid" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7logincontentgrid3.png?w=300" width="482" height="392" /></a> </p> <p></p> <p>Next, we navigate to the States panel in Expression Blend and create a new VisualStateGroup, to which we add two states; LoggedIn and NotLoggedIn.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7vsmui3.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7VSMUI" border="0" alt="WP7VSMUI" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7vsmui3.png?w=300" width="434" height="194" /></a> </p> <p>First, we will animate the login controls away. Select the LoggedIn state (this will put the state into recording mode), set the centre of projection to 0, 0, 0 and rotate the grid 90 degrees about the y-axis. This should produce a barn-door like animation. Click on the ‘Turn on transition preview’ button at the top of the States panel. Then clicking between the two states should display the animation.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7barndoor4.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7BarnDoor" border="0" alt="WP7BarnDoor" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7barndoor4.png?w=251" width="535" height="637" /></a> </p> <p>Now, select the NotLoggedIn state and also select the ContentGrid_Copy and set its Opacity to zero. Now click on LoggedIn to preview the animation which should fade up the Welcome text as the login controls swing away. Also, with the NotLoggedIn state selected I scale the Welcome text text block up to 3 in x and y axes and I do a similar with the user name text block. This will result into the text elements flying into the screen whilst gradually fading in.</p> <p><a href="http://babaandthepigman.files.wordpress.com/2010/05/wp7animating4.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7Animating" border="0" alt="WP7Animating" src="http://babaandthepigman.files.wordpress.com/2010/05/wp7animating4.png?w=205" width="384" height="561" /></a> </p> <p>This shows the animation part way through. But one problem remains – how do we trigger this from the View Model ?</p> <p>Behaviors to the rescue again – there are some Expression samples here <a title="http://expressionblend.codeplex.com/releases/view/30080" href="http://expressionblend.codeplex.com/releases/view/30080">http://expressionblend.codeplex.com/releases/view/30080</a> amongst them is the DataStateBehavior. This enables, as it suggests, driving state changes from data. I downloaded the source and included it in my solution, Blend discovers it and makes it available via it’s behaviors dialog. You just drag the behavior onto the page and configure it’s parameters. I bound it to an IsLoggedIn property in my view model. Here’s the generated xaml</p><pre>        <span style="color:#d2d200;">&lt;i:Interaction.Behaviors&gt;
            &lt;MvvmLight3_Behaviours:DataStateBehavior 
            Binding=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">IsLoggedIn</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; 
            </span><span style="color:#d2d200;">FalseState=</span><span style="color:white;">&quot;NotLoggedIn&quot; 
            </span><span style="color:#d2d200;">TrueState=</span><span style="color:white;">&quot;LoggedIn&quot; 
            </span><span style="color:#d2d200;">Value=</span><span style="color:white;">&quot;true&quot;  </span><span style="color:#d2d200;">/&gt;

</span></pre>
<p>Another useful behavior is the GoToStateAction which you can use to configure the initial state.</p>
<p>And that is the end of this series of posts – I hope this has provided some ideas in how you might go about MVVM + TDD on Windows Phone 7.</p>
<p></p>Technorati Tags: <a href="http://technorati.com/tags/MVVM" rel="tag">MVVM</a>,<a href="http://technorati.com/tags/Part" rel="tag">Part</a>,<a href="http://technorati.com/tags/VisualStateManager" rel="tag">VisualStateManager</a>,<a href="http://technorati.com/tags/Peter" rel="tag">Peter</a>,<a href="http://technorati.com/tags/Daukintis" rel="tag">Daukintis</a>,<a href="http://technorati.com/tags/transition" rel="tag">transition</a>,<a href="http://technorati.com/tags/manager" rel="tag">manager</a>,<a href="http://technorati.com/tags/ContentGrid" rel="tag">ContentGrid</a>,<a href="http://technorati.com/tags/contents" rel="tag">contents</a>,<a href="http://technorati.com/tags/text" rel="tag">text</a>,<a href="http://technorati.com/tags/data" rel="tag">data</a>,<a href="http://technorati.com/tags/panel" rel="tag">panel</a>,<a href="http://technorati.com/tags/Expression" rel="tag">Expression</a>,<a href="http://technorati.com/tags/Blend" rel="tag">Blend</a>,<a href="http://technorati.com/tags/VisualStateGroup" rel="tag">VisualStateGroup</a>,<a href="http://technorati.com/tags/LoggedIn" rel="tag">LoggedIn</a>,<a href="http://technorati.com/tags/NotLoggedIn" rel="tag">NotLoggedIn</a>,<a href="http://technorati.com/tags/Select" rel="tag">Select</a>,<a href="http://technorati.com/tags/mode" rel="tag">mode</a>,<a href="http://technorati.com/tags/projection" rel="tag">projection</a>,<a href="http://technorati.com/tags/grid" rel="tag">grid</a>,<a href="http://technorati.com/tags/axis" rel="tag">axis</a>,<a href="http://technorati.com/tags/barn" rel="tag">barn</a>,<a href="http://technorati.com/tags/door" rel="tag">door</a>,<a href="http://technorati.com/tags/animation" rel="tag">animation</a>,<a href="http://technorati.com/tags/Click" rel="tag">Click</a>,<a href="http://technorati.com/tags/Turn" rel="tag">Turn</a>,<a href="http://technorati.com/tags/preview" rel="tag">preview</a>,<a href="http://technorati.com/tags/ContentGrid_Copy" rel="tag">ContentGrid_Copy</a>,<a href="http://technorati.com/tags/zero" rel="tag">zero</a>,<a href="http://technorati.com/tags/Welcome" rel="tag">Welcome</a>,<a href="http://technorati.com/tags/Also" rel="tag">Also</a>,<a href="http://technorati.com/tags/user" rel="tag">user</a>,<a href="http://technorati.com/tags/result" rel="tag">result</a>,<a href="http://technorati.com/tags/View" rel="tag">View</a>,<a href="http://technorati.com/tags/Model" rel="tag">Model</a>,<a href="http://technorati.com/tags/Behaviors" rel="tag">Behaviors</a>,<a href="http://technorati.com/tags/DataStateBehavior" rel="tag">DataStateBehavior</a>,<a href="http://technorati.com/tags/solution" rel="tag">solution</a>,<a href="http://technorati.com/tags/IsLoggedIn" rel="tag">IsLoggedIn</a>,<a href="http://technorati.com/tags/Here" rel="tag">Here</a>,<a href="http://technorati.com/tags/Interaction" rel="tag">Interaction</a>,<a href="http://technorati.com/tags/FalseState" rel="tag">FalseState</a>,<a href="http://technorati.com/tags/TrueState" rel="tag">TrueState</a>,<a href="http://technorati.com/tags/Value" rel="tag">Value</a>,<a href="http://technorati.com/tags/Another" rel="tag">Another</a>,<a href="http://technorati.com/tags/GoToStateAction" rel="tag">GoToStateAction</a>,<a href="http://technorati.com/tags/series" rel="tag">series</a>,<a href="http://technorati.com/tags/ideas" rel="tag">ideas</a>,<a href="http://technorati.com/tags/transitions" rel="tag">transitions</a>,<a href="http://technorati.com/tags/degrees" rel="tag">degrees</a>,<a href="http://technorati.com/tags/parameters" rel="tag">parameters</a>,<a href="http://technorati.com/tags/login" rel="tag">login</a>,<a href="http://technorati.com/tags/behavior" rel="tag">behavior</a><br />
<p></p>Windows Live Tags: <a href="http://windows.live.com/connect/tag/MVVM" rel="clubhouseTag">MVVM</a>,<a href="http://windows.live.com/connect/tag/Part" rel="clubhouseTag">Part</a>,<a href="http://windows.live.com/connect/tag/VisualStateManager" rel="clubhouseTag">VisualStateManager</a>,<a href="http://windows.live.com/connect/tag/Peter" rel="clubhouseTag">Peter</a>,<a href="http://windows.live.com/connect/tag/Daukintis" rel="clubhouseTag">Daukintis</a>,<a href="http://windows.live.com/connect/tag/transition" rel="clubhouseTag">transition</a>,<a href="http://windows.live.com/connect/tag/manager" rel="clubhouseTag">manager</a>,<a href="http://windows.live.com/connect/tag/ContentGrid" rel="clubhouseTag">ContentGrid</a>,<a href="http://windows.live.com/connect/tag/contents" rel="clubhouseTag">contents</a>,<a href="http://windows.live.com/connect/tag/text" rel="clubhouseTag">text</a>,<a href="http://windows.live.com/connect/tag/data" rel="clubhouseTag">data</a>,<a href="http://windows.live.com/connect/tag/panel" rel="clubhouseTag">panel</a>,<a href="http://windows.live.com/connect/tag/Expression" rel="clubhouseTag">Expression</a>,<a href="http://windows.live.com/connect/tag/Blend" rel="clubhouseTag">Blend</a>,<a href="http://windows.live.com/connect/tag/VisualStateGroup" rel="clubhouseTag">VisualStateGroup</a>,<a href="http://windows.live.com/connect/tag/LoggedIn" rel="clubhouseTag">LoggedIn</a>,<a href="http://windows.live.com/connect/tag/NotLoggedIn" rel="clubhouseTag">NotLoggedIn</a>,<a href="http://windows.live.com/connect/tag/Select" rel="clubhouseTag">Select</a>,<a href="http://windows.live.com/connect/tag/mode" rel="clubhouseTag">mode</a>,<a href="http://windows.live.com/connect/tag/projection" rel="clubhouseTag">projection</a>,<a href="http://windows.live.com/connect/tag/grid" rel="clubhouseTag">grid</a>,<a href="http://windows.live.com/connect/tag/axis" rel="clubhouseTag">axis</a>,<a href="http://windows.live.com/connect/tag/barn" rel="clubhouseTag">barn</a>,<a href="http://windows.live.com/connect/tag/door" rel="clubhouseTag">door</a>,<a href="http://windows.live.com/connect/tag/animation" rel="clubhouseTag">animation</a>,<a href="http://windows.live.com/connect/tag/Click" rel="clubhouseTag">Click</a>,<a href="http://windows.live.com/connect/tag/Turn" rel="clubhouseTag">Turn</a>,<a href="http://windows.live.com/connect/tag/preview" rel="clubhouseTag">preview</a>,<a href="http://windows.live.com/connect/tag/ContentGrid_Copy" rel="clubhouseTag">ContentGrid_Copy</a>,<a href="http://windows.live.com/connect/tag/zero" rel="clubhouseTag">zero</a>,<a href="http://windows.live.com/connect/tag/Welcome" rel="clubhouseTag">Welcome</a>,<a href="http://windows.live.com/connect/tag/Also" rel="clubhouseTag">Also</a>,<a href="http://windows.live.com/connect/tag/user" rel="clubhouseTag">user</a>,<a href="http://windows.live.com/connect/tag/result" rel="clubhouseTag">result</a>,<a href="http://windows.live.com/connect/tag/View" rel="clubhouseTag">View</a>,<a href="http://windows.live.com/connect/tag/Model" rel="clubhouseTag">Model</a>,<a href="http://windows.live.com/connect/tag/Behaviors" rel="clubhouseTag">Behaviors</a>,<a href="http://windows.live.com/connect/tag/DataStateBehavior" rel="clubhouseTag">DataStateBehavior</a>,<a href="http://windows.live.com/connect/tag/solution" rel="clubhouseTag">solution</a>,<a href="http://windows.live.com/connect/tag/IsLoggedIn" rel="clubhouseTag">IsLoggedIn</a>,<a href="http://windows.live.com/connect/tag/Here" rel="clubhouseTag">Here</a>,<a href="http://windows.live.com/connect/tag/Interaction" rel="clubhouseTag">Interaction</a>,<a href="http://windows.live.com/connect/tag/FalseState" rel="clubhouseTag">FalseState</a>,<a href="http://windows.live.com/connect/tag/TrueState" rel="clubhouseTag">TrueState</a>,<a href="http://windows.live.com/connect/tag/Value" rel="clubhouseTag">Value</a>,<a href="http://windows.live.com/connect/tag/Another" rel="clubhouseTag">Another</a>,<a href="http://windows.live.com/connect/tag/GoToStateAction" rel="clubhouseTag">GoToStateAction</a>,<a href="http://windows.live.com/connect/tag/series" rel="clubhouseTag">series</a>,<a href="http://windows.live.com/connect/tag/ideas" rel="clubhouseTag">ideas</a>,<a href="http://windows.live.com/connect/tag/transitions" rel="clubhouseTag">transitions</a>,<a href="http://windows.live.com/connect/tag/degrees" rel="clubhouseTag">degrees</a>,<a href="http://windows.live.com/connect/tag/parameters" rel="clubhouseTag">parameters</a>,<a href="http://windows.live.com/connect/tag/login" rel="clubhouseTag">login</a>,<a href="http://windows.live.com/connect/tag/behavior" rel="clubhouseTag">behavior</a>  </div>