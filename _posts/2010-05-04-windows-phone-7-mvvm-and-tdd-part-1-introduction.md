---
layout: post
title: Windows Phone 7, MVVM and TDD (Part 1 - Introduction)
date: 2010-05-04 14:36
author: peted70
comments: true
categories: [MVVM, TDD, Windows Phone 7]
---
<div id="msgcns!4F1B7368284539E5!220" class="bvMsg"><p>by Peter Daukintis</p> <p>Before jumping into Windows Phone 7 development I need to define some ideas about a baseline architecture so I have a clear development direction to move in. This series of posts will document my thinking in this area. This is not an introductory post on any of the individual technologies and I will assume the reader has some knowledge of the basics. Here’s a list of pre-requisites:</p> <ul> <li>Windows Phone developer tools CTP – April Refresh <a title="http://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=cabcd5ed-7dfc-4731-9d7e-3220603cad14" href="http://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=cabcd5ed-7dfc-4731-9d7e-3220603cad14">http://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=cabcd5ed-7dfc-4731-9d7e-3220603cad14</a> (I am using VS2010 Ultimate with the SDK + Tools)  <li>MVVM Light Toolkit <a href="http://blog.galasoft.ch/archive/2010/04/03/mvvm-light-toolkit-v3-sp1-for-windows-phone-7.aspx">http://blog.galasoft.ch/archive/2010/04/03/mvvm-light-toolkit-v3-sp1-for-windows-phone-7.aspx</a>  <li>Silverlight Unit Testing Framework - <a title="http://code.msdn.microsoft.com/silverlightut" href="http://code.msdn.microsoft.com/silverlightut">http://code.msdn.microsoft.com/silverlightut</a></li></ul> <p>I intend to develop a very simple phone application using Silverlight to demonstrate and explore the architecture. The application will present a login screen and allow a user to log in. It will present some kind of busy indicator and when logged in will navigate to another screen (well, I said it would be simple :-)).</p> <p>I will develop in a test-driven manner using a model-view-viewmodel architecture to support this. I will try to document where I have made choices along the way since I expect there will be many alternative solutions.</p> <p>Just so we’re all on the same page here’s a sneak preview of the end result.</p> <p><a href="https://omlweq.bay.livefilestore.com/y1m6DK-8R_VtsSqb9RDOGUReVMj0BpG_D1HWuYaEl-GTMJPCx7VQRQKo_pggb2KkzsdWwldeb0jLbQPHKC68NHSg90N4M4goa46ecWHLMKkqx05q2Er8NFWUa0aVhrYkXOsuJTZUHbV3I7Aw3hK1JM-Fg/WP7EmulatorMenu3.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7EmulatorMenu" border="0" alt="WP7EmulatorMenu" src="https://omlweq.bay.livefilestore.com/y1mXcEaUM5-MwjB2gLK9yINdcGW4i8f9HupZjefHB7e1h4ROg2K8Z1mk5ZPrDDedn8VJ-4ab_fY0hHu3dlBrchbZiChJ30HCwkeq293XEgbshIMI8SBXz8IM2gBppoAIjyD9aNMiRdr-KCc1IVkHStqyA/WP7EmulatorMenu_thumb1.png" width="287" height="492" /></a></p> <p>Here’s a screen grab of the WP7 emulator showing the WP7Login Application and also WP7Login.Tests application. For further information about the tools and the phone emulator see <a title="http://developer.windowsphone.com/windows-phone-7-series/" href="http://developer.windowsphone.com/windows-phone-7-series/">http://developer.windowsphone.com/windows-phone-7-series/</a></p> <p align="center"><a href="https://omlweq.bay.livefilestore.com/y1mkexNFFIkENoo3wbH0IPbDrHEwpRayS7rQ7BtHWxyE3RM9x-wrOeH0DYWon29E8jrNB7Tx8_cfyDgsyQ0RKoQ89XWpSz3f8XCSUKpH-E7OK8DO4UEiOVL3mAoRZKGd1NcuXGWtTrhoTrklz9mMgQBYA/WP7LoginScreen4.png" rel="WLPP"><img style="display:inline;border-width:0;" title="WP7LoginScreen" border="0" alt="WP7LoginScreen" src="https://omlweq.bay.livefilestore.com/y1m5TJAOBHaoRNVwRknpZ_WIxHN6nwxuJMfA8qIvlELcZrP_-Y8nYROggZO5bENW_hRAO28aKcJqVe_4aSBt2k6-ZfqVN0LofVxU3gVEEDL-K5iURLKJACL1417Fc0Y0PjOR7XyUqmaopF2GEVIKljGoQ/WP7LoginScreen_thumb2.png" width="297" height="519" /></a> </p> <p></p> <p>Here’s the application login screen.</p> <p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7loginloading4.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7LoginLoading" border="0" alt="WP7LoginLoading" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7loginloading4.png?w=169" width="312" height="550" /></a> </p> <p>While it’s loading…</p> <p><a href="http://babaandthepigman.files.wordpress.com/2010/05/wp7welcome4.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7Welcome" border="0" alt="WP7Welcome" src="http://babaandthepigman.files.wordpress.com/2010/05/wp7welcome4.png?w=171" width="308" height="537" /></a> </p> <p>…and finally transitioning into the welcome screen.</p> <p>The unit tests will run on the phone emulator and will look like this…</p> <p><a href="http://babaandthepigman.files.wordpress.com/2010/05/wp7loginunittests4.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7LoginUnitTests" border="0" alt="WP7LoginUnitTests" src="http://babaandthepigman.files.wordpress.com/2010/05/wp7loginunittests4.png?w=168" width="308" height="545" /></a> </p> <p>That wraps the introduction.</p> <p></p>Technorati Tags: <a href="http://technorati.com/tags/MVVM" rel="tag">MVVM</a>,<a href="http://technorati.com/tags/Part" rel="tag">Part</a>,<a href="http://technorati.com/tags/Introduction" rel="tag">Introduction</a>,<a href="http://technorati.com/tags/development" rel="tag">development</a>,<a href="http://technorati.com/tags/ideas" rel="tag">ideas</a>,<a href="http://technorati.com/tags/architecture" rel="tag">architecture</a>,<a href="http://technorati.com/tags/direction" rel="tag">direction</a>,<a href="http://technorati.com/tags/series" rel="tag">series</a>,<a href="http://technorati.com/tags/area" rel="tag">area</a>,<a href="http://technorati.com/tags/reader" rel="tag">reader</a>,<a href="http://technorati.com/tags/knowledge" rel="tag">knowledge</a>,<a href="http://technorati.com/tags/basics" rel="tag">basics</a>,<a href="http://technorati.com/tags/Here" rel="tag">Here</a>,<a href="http://technorati.com/tags/requisites" rel="tag">requisites</a>,<a href="http://technorati.com/tags/tools" rel="tag">tools</a>,<a href="http://technorati.com/tags/April" rel="tag">April</a>,<a href="http://technorati.com/tags/Refresh" rel="tag">Refresh</a>,<a href="http://technorati.com/tags/FamilyID" rel="tag">FamilyID</a>,<a href="http://technorati.com/tags/Ultimate" rel="tag">Ultimate</a>,<a href="http://technorati.com/tags/Toolkit" rel="tag">Toolkit</a>,<a href="http://technorati.com/tags/archive" rel="tag">archive</a>,<a href="http://technorati.com/tags/Unit" rel="tag">Unit</a>,<a href="http://technorati.com/tags/Framework" rel="tag">Framework</a>,<a href="http://technorati.com/tags/code" rel="tag">code</a>,<a href="http://technorati.com/tags/user" rel="tag">user</a>,<a href="http://technorati.com/tags/indicator" rel="tag">indicator</a>,<a href="http://technorati.com/tags/manner" rel="tag">manner</a>,<a href="http://technorati.com/tags/Just" rel="tag">Just</a>,<a href="http://technorati.com/tags/preview" rel="tag">preview</a>,<a href="http://technorati.com/tags/result" rel="tag">result</a>,<a href="http://technorati.com/tags/Application" rel="tag">Application</a>,<a href="http://technorati.com/tags/wraps" rel="tag">wraps</a>,<a href="http://technorati.com/tags/technologies" rel="tag">technologies</a>,<a href="http://technorati.com/tags/solutions" rel="tag">solutions</a>,<a href="http://technorati.com/tags/developer" rel="tag">developer</a>,<a href="http://technorati.com/tags/microsoft" rel="tag">microsoft</a>,<a href="http://technorati.com/tags/aspx" rel="tag">aspx</a>,<a href="http://technorati.com/tags/login" rel="tag">login</a>,<a href="http://technorati.com/tags/emulator" rel="tag">emulator</a><br /> <p></p>Windows Live Tags: <a href="http://windows.live.com/connect/tag/MVVM" rel="clubhouseTag">MVVM</a>,<a href="http://windows.live.com/connect/tag/Part" rel="clubhouseTag">Part</a>,<a href="http://windows.live.com/connect/tag/Introduction" rel="clubhouseTag">Introduction</a>,<a href="http://windows.live.com/connect/tag/development" rel="clubhouseTag">development</a>,<a href="http://windows.live.com/connect/tag/ideas" rel="clubhouseTag">ideas</a>,<a href="http://windows.live.com/connect/tag/architecture" rel="clubhouseTag">architecture</a>,<a href="http://windows.live.com/connect/tag/direction" rel="clubhouseTag">direction</a>,<a href="http://windows.live.com/connect/tag/series" rel="clubhouseTag">series</a>,<a href="http://windows.live.com/connect/tag/area" rel="clubhouseTag">area</a>,<a href="http://windows.live.com/connect/tag/reader" rel="clubhouseTag">reader</a>,<a href="http://windows.live.com/connect/tag/knowledge" rel="clubhouseTag">knowledge</a>,<a href="http://windows.live.com/connect/tag/basics" rel="clubhouseTag">basics</a>,<a href="http://windows.live.com/connect/tag/Here" rel="clubhouseTag">Here</a>,<a href="http://windows.live.com/connect/tag/requisites" rel="clubhouseTag">requisites</a>,<a href="http://windows.live.com/connect/tag/tools" rel="clubhouseTag">tools</a>,<a href="http://windows.live.com/connect/tag/April" rel="clubhouseTag">April</a>,<a href="http://windows.live.com/connect/tag/Refresh" rel="clubhouseTag">Refresh</a>,<a href="http://windows.live.com/connect/tag/FamilyID" rel="clubhouseTag">FamilyID</a>,<a href="http://windows.live.com/connect/tag/Ultimate" rel="clubhouseTag">Ultimate</a>,<a href="http://windows.live.com/connect/tag/Toolkit" rel="clubhouseTag">Toolkit</a>,<a href="http://windows.live.com/connect/tag/archive" rel="clubhouseTag">archive</a>,<a href="http://windows.live.com/connect/tag/Unit" rel="clubhouseTag">Unit</a>,<a href="http://windows.live.com/connect/tag/Framework" rel="clubhouseTag">Framework</a>,<a href="http://windows.live.com/connect/tag/code" rel="clubhouseTag">code</a>,<a href="http://windows.live.com/connect/tag/user" rel="clubhouseTag">user</a>,<a href="http://windows.live.com/connect/tag/indicator" rel="clubhouseTag">indicator</a>,<a href="http://windows.live.com/connect/tag/manner" rel="clubhouseTag">manner</a>,<a href="http://windows.live.com/connect/tag/Just" rel="clubhouseTag">Just</a>,<a href="http://windows.live.com/connect/tag/preview" rel="clubhouseTag">preview</a>,<a href="http://windows.live.com/connect/tag/result" rel="clubhouseTag">result</a>,<a href="http://windows.live.com/connect/tag/Application" rel="clubhouseTag">Application</a>,<a href="http://windows.live.com/connect/tag/wraps" rel="clubhouseTag">wraps</a>,<a href="http://windows.live.com/connect/tag/technologies" rel="clubhouseTag">technologies</a>,<a href="http://windows.live.com/connect/tag/solutions" rel="clubhouseTag">solutions</a>,<a href="http://windows.live.com/connect/tag/developer" rel="clubhouseTag">developer</a>,<a href="http://windows.live.com/connect/tag/microsoft" rel="clubhouseTag">microsoft</a>,<a href="http://windows.live.com/connect/tag/aspx" rel="clubhouseTag">aspx</a>,<a href="http://windows.live.com/connect/tag/login" rel="clubhouseTag">login</a>,<a href="http://windows.live.com/connect/tag/emulator" rel="clubhouseTag">emulator</a>  </div>