---
layout: post
title: Windows Phone 7, MVVM and TDD (Part 5 – creating the basic UI)
date: 2010-05-06 09:10
author: peted70
comments: true
categories: [MVVM, TDD, Windows Phone 7]
---
<div id="msgcns!4F1B7368284539E5!255" class="bvMsg"><p>by Peter Daukintis</p> <p>Okay, so we haven’t touched on any of the user interface yet but this supports the theory that we should be able to develop the user interface independently of the code. So, these are the areas we need to create for the user interface:</p> <ul> <li>Create the user name and password inputs  <li>Create a way to execute the login command  <li>Create a busy indicator UserControl  <li>Create a transition to a welcome screen after successfully logging in</li></ul> <p>I am going to use Expression Blend 4 RC for this part of the post as I can work more quickly inside Blend – also you will need <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=47f5c718-9dec-4557-9687-619c0fdd3d4f&amp;displaylang=en">Microsoft Expression Blend Add-in Preview 2 for Windows Phone</a> and <a href="http://www.microsoft.com/downloads/details.aspx?FamilyID=86370108-4c14-42ee-8855-226e5dd9b85b">Microsoft Expression Blend Software Development Kit Preview 2 for Windows Phone</a> all of which can be downloaded here <a title="http://www.microsoft.com/expression/windowsphone/" href="http://www.microsoft.com/expression/windowsphone/">http://www.microsoft.com/expression/windowsphone/</a>. </p> <p>Opening the solution up in Expression Blend looks like this:</p> <p><a href="https://omlweq.bay.livefilestore.com/y1mk_XpNWok2KSwBGwNZ5EtoJPzaF_VOKEXaV0U5VZWPrYpFZTgUY0zJEx_uNsIhnCIHB_kDDstCW6N5HEr-aK8TFnYB3og1sEBhT1VC-de8JfK82pCTGtKG2QFEyTKpOfrlpuw8pGnHL7Ks9RbshIeAw/WP7LoginRxpressionBlend3.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7LoginRxpressionBlend" border="0" alt="WP7LoginRxpressionBlend" src="https://omlweq.bay.livefilestore.com/y1msuPR9BMLsB5yGYAUr0w7XxQ_0aUqUG9M_egiRT9XlcqUPSO3C0Oj059_ghXWDhzsTLEa-tMQpYzNkSB3lmT1YDiY_vCb5NP4DP4h_bgavBsstHKL_TpWIlZKVNoNhSAgWr7n3_MkrXFoj3YHNCSD8Q/WP7LoginRxpressionBlend_thumb1.png" width="735" height="461" /></a> </p> <p align="left">So, let’s start by dragging some input controls and a button:</p><pre>            <span style="color:#d2d200;">&lt;TextBlock Text=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">Welcome</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;
                       </span><span style="color:#d2d200;">Style=&quot;&#123;</span><span style="color:cyan;">StaticResource </span><span style="color:white;">PhoneTextNormalStyle</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot;
                       </span><span style="color:#d2d200;">HorizontalAlignment=</span><span style="color:white;">&quot;Center&quot;
                       </span><span style="color:#d2d200;">VerticalAlignment=</span><span style="color:white;">&quot;Center&quot;
                       </span><span style="color:#d2d200;">FontSize=</span><span style="color:white;">&quot;30&quot; </span><span style="color:#d2d200;">Margin=</span><span style="color:white;">&quot;0,592,-12,6&quot; </span><span style="color:#d2d200;">/&gt;
            &lt;Button Content=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">LoginButtonName</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; </span><span style="color:#d2d200;">Margin=</span><span style="color:white;">&quot;187,239,7,0&quot; </span><span style="color:#d2d200;">VerticalAlignment=</span><span style="color:white;">&quot;Top&quot; </span><span style="color:#d2d200;">Height=</span><span style="color:white;">&quot;114&quot;</span><span style="color:#d2d200;">&gt;
            &lt;/Button&gt;
            &lt;TextBox Height=</span><span style="color:white;">&quot;31&quot; </span><span style="color:#d2d200;">HorizontalAlignment=</span><span style="color:white;">&quot;Left&quot; </span><span style="color:#d2d200;">Margin=</span><span style="color:white;">&quot;137,57,0,0&quot; 
                     </span><span style="color:#d2d200;">Text=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">UsernameText</span><span style="color:#d2d200;">, </span><span style="color:white;">Mode</span><span style="color:#d2d200;">=</span><span style="color:lime;">TwoWay</span><span style="color:#d2d200;">, </span><span style="color:white;">UpdateSourceTrigger</span><span style="color:#d2d200;">=</span><span style="color:lime;">Explicit</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; 
                     </span><span style="color:#d2d200;">VerticalAlignment=</span><span style="color:white;">&quot;Top&quot; </span><span style="color:#d2d200;">Width=</span><span style="color:white;">&quot;336&quot;</span><span style="color:#d2d200;">&gt;
            &lt;/TextBox&gt;
            &lt;PasswordBox Height=</span><span style="color:white;">&quot;31&quot; </span><span style="color:#d2d200;">HorizontalAlignment=</span><span style="color:white;">&quot;Left&quot; </span><span style="color:#d2d200;">Margin=</span><span style="color:white;">&quot;137,144,0,0&quot; 
                     </span><span style="color:#d2d200;">Password=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">PasswordText</span><span style="color:#d2d200;">, </span><span style="color:white;">Mode</span><span style="color:#d2d200;">=</span><span style="color:lime;">TwoWay</span><span style="color:#d2d200;">, </span><span style="color:white;">UpdateSourceTrigger</span><span style="color:#d2d200;">=</span><span style="color:lime;">Explicit</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; 
                     </span><span style="color:#d2d200;">VerticalAlignment=</span><span style="color:white;">&quot;Top&quot; </span><span style="color:#d2d200;">Width=</span><span style="color:white;">&quot;336&quot; </span><span style="color:#d2d200;">PasswordChar=</span><span style="color:white;">&quot;*&quot;</span><span style="color:#d2d200;">&gt;
            &lt;/PasswordBox&gt;
            &lt;TextBlock HorizontalAlignment=</span><span style="color:white;">&quot;Left&quot; </span><span style="color:#d2d200;">Margin=</span><span style="color:white;">&quot;18,79,0,0&quot;
                       </span><span style="color:#d2d200;">Text=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">UsernameLabel</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; </span><span style="color:#d2d200;">VerticalAlignment=</span><span style="color:white;">&quot;Top&quot; </span><span style="color:#d2d200;">TextAlignment=</span><span style="color:white;">&quot;Right&quot; </span><span style="color:#d2d200;">/&gt;
            &lt;TextBlock HorizontalAlignment=</span><span style="color:white;">&quot;Left&quot; </span><span style="color:#d2d200;">Margin=</span><span style="color:white;">&quot;33,167,0,0&quot;
                       </span><span style="color:#d2d200;">Text=&quot;&#123;</span><span style="color:cyan;">Binding </span><span style="color:white;">PasswordLabel</span><span style="color:#d2d200;">&#125;</span><span style="color:white;">&quot; </span><span style="color:#d2d200;">VerticalAlignment=</span><span style="color:white;">&quot;Top&quot; </span><span style="color:#d2d200;">/&gt;

</span></pre>
<p>and adding some additional properties to our view model:</p><pre>        <span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">ApplicationTitle </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#c89191;">&quot;Application&quot;</span><span style="color:#d2d200;">; &#125; &#125;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">ListName </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#c89191;">&quot;Login:&quot;</span><span style="color:#d2d200;">; &#125; &#125;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">LoginButtonName </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#c89191;">&quot;Login&quot;</span><span style="color:#d2d200;">; &#125; &#125;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">Welcome </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#c89191;">&quot;Welcome to My Application&quot;</span><span style="color:#d2d200;">; &#125; &#125;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">UsernameLabel </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#c89191;">&quot;User Name :&quot;</span><span style="color:#d2d200;">; &#125; &#125;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">PasswordLabel </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">return </span><span style="color:#c89191;">&quot;Password :&quot;</span><span style="color:#d2d200;">; &#125; &#125;

</span></pre>
<p>results in the following user interface (the view model is already bound to the DataContext of the page using the MVVM Light ViewModelLocator class):</p>
<p><a href="http://babaandthepigman.files.wordpress.com/2010/05/wp7basicui5.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7BasicUI" border="0" alt="WP7BasicUI" src="http://babaandthepigman.files.wordpress.com/2010/05/wp7basicui5.png?w=175" width="470" height="802" /></a> <a href="http://11011.net/software/vspaste"></a></p>
<p>So, the text input parameters are bound we need to bind the click from the login button to the Login Command exposed by the MainViewModel. We will wire the button up using the EventToCommand behavior included in the MVVM Light framework. Silverlight behaviors allow some functionality to be associated with a FrameworkElement by dragging it from the toolbox onto the element in Expression Blend. To achieve this carry out the following:</p>
<ul>
<li>First install and reference the assemblies here <a href="http://blog.galasoft.ch/archive/2010/04/03/mvvm-light-toolkit-v3-sp1-for-windows-phone-7.aspx">http://blog.galasoft.ch/archive/2010/04/03/mvvm-light-toolkit-v3-sp1-for-windows-phone-7.aspx</a> as the Silverlight behaviors are not available in Blend without these. 
<li>Open the Assets panel in Blend, click on behaviors – you should see the EventToCommand behavior. </li></li></ul>
<p><a href="https://omlweq.bay.livefilestore.com/y1mOmdpSDmSVVaKpUsGsW33HrH0uyw8PUZIxOUEw_WBKCb7-0XQ45XQCXxuENmruOB-LbBiBBT4dnliktrgUjhSoqSXMVEGUJ1IbQANbsMpyrMOqdl-lXs6FMZSUHuZ1fha3WyOQUMEGfhh1Gt2SWPaSQ/WP7LoginBlendBehaviors7.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7LoginBlendBehaviors" border="0" alt="WP7LoginBlendBehaviors" src="https://omlweq.bay.livefilestore.com/y1mLall_a27k9w3BzIfittg-Et1jP_m3tZKYEDKm8MiS6YluJvJWL9rQ65VMv98aGFNeK2-W0-UYm91MHqrsepFDjyBsFRtp0tbznV-ZtydD2wnyqu_ACZjWl9aDU2Z7qNjkTbTMUBdtgAXcQ1LN-6ALg/WP7LoginBlendBehaviors_thumb3.png" width="526" height="518" /></a> </p>
<li>Drag EventToCommand out onto the Login button. 
<li>With the EventToCommand object selected in the Objects &amp; Timelines view configure it via the properties pane. 
<p> </p>
<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7logincommandprops3.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7LoginCommandProps" border="0" alt="WP7LoginCommandProps" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7logincommandprops3.png?w=198" width="324" height="487" /></a> </p>
<li>Click on the small blob to the right of the Command input box and choose data-binding from the options to display the data-binding dialog: 
<p> </p>
<p><a href="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7logindatabindingdlg4.png" rel="WLPP"><img style="display:block;float:none;margin-left:auto;margin-right:auto;border-width:0;" title="WP7LoginDataBindingDlg" border="0" alt="WP7LoginDataBindingDlg" src="http://peted.azurewebsites.net/wp-content/uploads/2010/09/wp7logindatabindingdlg4.png?w=300" width="562" height="539" /></a> </p>
<p></p>
<p>from here you can see all of the view model properties and can select the LoginCommand property to complete the data-binding….nice.</p>
<ul>
<li>Set the MustToggleIsEnabled to true as we want to automatically disable the button if the command is unavailable and also set the Event Name to Click if it isn’t already.</li></ul>
<p>Unfortunately, this is not enough for the behaviour I was looking for since in Silverlight there is no option to set the UpdateSourceTrigger to fire when the property changes, the best we can do is when the control loses focus. As a result the login button does not disable/enable when the fields are empty – until focus is lost from the input controls. To remedy this I have added some custom behaviors:</p><pre><span style="color:#eaeaac;">namespace </span><span style="color:#f8ffc6;">WP7Login</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Behaviors
</span><span style="color:#d2d200;">&#123;
    </span><span style="color:#eaeaac;">public class </span><span style="color:#f0dfaf;">DataBindTextBoxOnPropertyChangedBehaviour </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">Behavior</span><span style="color:#d2d200;">&lt;</span><span style="color:#f0dfaf;">TextBox</span><span style="color:#d2d200;">&gt;
    &#123;
        </span><span style="color:#eaeaac;">public </span><span style="color:#f0dfaf;">BindingExpression </span><span style="color:#f8ffc6;">BindingExpression </span><span style="color:#d2d200;">&#123; </span><span style="color:#eaeaac;">get</span><span style="color:#d2d200;">; </span><span style="color:#eaeaac;">set</span><span style="color:#d2d200;">; &#125;

        </span><span style="color:#eaeaac;">protected override void </span><span style="color:#f8ffc6;">OnAttached</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#eaeaac;">base</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">OnAttached</span><span style="color:#d2d200;">();

            </span><span style="color:#f8ffc6;">BindingExpression </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">AssociatedObject</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">GetBindingExpression</span><span style="color:#d2d200;">(</span><span style="color:#f0dfaf;">TextBox</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">TextProperty</span><span style="color:#d2d200;">);
            </span><span style="color:#f8ffc6;">AssociatedObject</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">TextChanged </span><span style="color:#d2d200;">+= </span><span style="color:#f8ffc6;">AssociatedObject_TextChanged</span><span style="color:#d2d200;">;
        &#125;

        </span><span style="color:#eaeaac;">protected override void </span><span style="color:#f8ffc6;">OnDetaching</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#eaeaac;">base</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">OnDetaching</span><span style="color:#d2d200;">();
            </span><span style="color:#f8ffc6;">BindingExpression </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">;
        &#125;

        </span><span style="color:#eaeaac;">private void </span><span style="color:#f8ffc6;">AssociatedObject_TextChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">sender</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">TextChangedEventArgs </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">BindingExpression </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#f8ffc6;">BindingExpression</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">UpdateSource</span><span style="color:#d2d200;">();
            &#125;
        &#125;
    &#125;
&#125;
</span></pre>
<p> </p>
<p>and a similar one aimed at a password box. Once recompiled these will be available via the Assets panel in Blend and can be dragged onto the user name input and the password input boxes appropriately.</p>
<p>Also, CanExecute in MainViewModel must be implemented</p><pre>        <span style="color:#eaeaac;">private bool </span><span style="color:#f8ffc6;">CanExecuteLogin</span><span style="color:#d2d200;">()
        &#123;
            </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">UsernameText</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Length </span><span style="color:#d2d200;">&gt; </span><span style="color:#8acccf;">0 </span><span style="color:#d2d200;">&amp;&amp; </span><span style="color:#f8ffc6;">PasswordText</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Length </span><span style="color:#d2d200;">&gt; </span><span style="color:#8acccf;">0</span><span style="color:#d2d200;">;
        &#125;

</span></pre>
<p>And also the RaiseExecuteChanged event must be raised in response to the data in the text boxes changing – this can be achieved by the following:</p><pre>            <span style="color:#f8ffc6;">PropertyChanged </span><span style="color:#d2d200;">+= </span><span style="color:#f8ffc6;">MainViewModel_PropertyChanged</span><span style="color:#d2d200;">;

</span></pre>
<p><a href="http://11011.net/software/vspaste"></a>Subscribe to the property changed event on the view model with this handler:</p><pre>        <span style="color:#eaeaac;">void </span><span style="color:#f8ffc6;">MainViewModel_PropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">sender</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">PropertyChangedEventArgs </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">)
        &#123;
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">PropertyName </span><span style="color:#d2d200;">== </span><span style="color:#c89191;">&quot;UsernameText&quot; </span><span style="color:#d2d200;">|| 
                </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">PropertyName </span><span style="color:#d2d200;">== </span><span style="color:#c89191;">&quot;PasswordText&quot;</span><span style="color:#d2d200;">)
            &#123;
                </span><span style="color:#eaeaac;">var </span><span style="color:#f8ffc6;">relayCommand </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">LoginCommand </span><span style="color:#eaeaac;">as </span><span style="color:#f0dfaf;">RelayCommand</span><span style="color:#d2d200;">;
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">relayCommand </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
                &#123;
                    </span><span style="color:#f8ffc6;">relayCommand</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">RaiseCanExecuteChanged</span><span style="color:#d2d200;">();
                &#125;
            &#125;
        &#125;
</span></pre>
<p>Finally we get the required behaviour – if either username text box or the password box is empty the login button is disabled otherwise it is enabled as we type.</p>
<p>So, that’s the basic user interface in place – next we move to the loading screen and the transition when logged in….</p></li>
<p></p>Technorati Tags: <a href="http://technorati.com/tags/MVVM" rel="tag">MVVM</a>,<a href="http://technorati.com/tags/Part" rel="tag">Part</a>,<a href="http://technorati.com/tags/Peter" rel="tag">Peter</a>,<a href="http://technorati.com/tags/Daukintis" rel="tag">Daukintis</a>,<a href="http://technorati.com/tags/Okay" rel="tag">Okay</a>,<a href="http://technorati.com/tags/haven" rel="tag">haven</a>,<a href="http://technorati.com/tags/user" rel="tag">user</a>,<a href="http://technorati.com/tags/interface" rel="tag">interface</a>,<a href="http://technorati.com/tags/supports" rel="tag">supports</a>,<a href="http://technorati.com/tags/theory" rel="tag">theory</a>,<a href="http://technorati.com/tags/code" rel="tag">code</a>,<a href="http://technorati.com/tags/Create" rel="tag">Create</a>,<a href="http://technorati.com/tags/password" rel="tag">password</a>,<a href="http://technorati.com/tags/indicator" rel="tag">indicator</a>,<a href="http://technorati.com/tags/UserControl" rel="tag">UserControl</a>,<a href="http://technorati.com/tags/transition" rel="tag">transition</a>,<a href="http://technorati.com/tags/Expression" rel="tag">Expression</a>,<a href="http://technorati.com/tags/Blend" rel="tag">Blend</a>,<a href="http://technorati.com/tags/Microsoft" rel="tag">Microsoft</a>,<a href="http://technorati.com/tags/Preview" rel="tag">Preview</a>,<a href="http://technorati.com/tags/Software" rel="tag">Software</a>,<a href="http://technorati.com/tags/Development" rel="tag">Development</a>,<a href="http://technorati.com/tags/solution" rel="tag">solution</a>,<a href="http://technorati.com/tags/TextBlock" rel="tag">TextBlock</a>,<a href="http://technorati.com/tags/Text" rel="tag">Text</a>,<a href="http://technorati.com/tags/Welcome" rel="tag">Welcome</a>,<a href="http://technorati.com/tags/Style" rel="tag">Style</a>,<a href="http://technorati.com/tags/StaticResource" rel="tag">StaticResource</a>,<a href="http://technorati.com/tags/PhoneTextNormalStyle" rel="tag">PhoneTextNormalStyle</a>,<a href="http://technorati.com/tags/HorizontalAlignment" rel="tag">HorizontalAlignment</a>,<a href="http://technorati.com/tags/Center" rel="tag">Center</a>,<a href="http://technorati.com/tags/VerticalAlignment" rel="tag">VerticalAlignment</a>,<a href="http://technorati.com/tags/FontSize" rel="tag">FontSize</a>,<a href="http://technorati.com/tags/Margin" rel="tag">Margin</a>,<a href="http://technorati.com/tags/Button" rel="tag">Button</a>,<a href="http://technorati.com/tags/Content" rel="tag">Content</a>,<a href="http://technorati.com/tags/LoginButtonName" rel="tag">LoginButtonName</a>,<a href="http://technorati.com/tags/TextBox" rel="tag">TextBox</a>,<a href="http://technorati.com/tags/Left" rel="tag">Left</a>,<a href="http://technorati.com/tags/UsernameText" rel="tag">UsernameText</a>,<a href="http://technorati.com/tags/Mode" rel="tag">Mode</a>,<a href="http://technorati.com/tags/TwoWay" rel="tag">TwoWay</a>,<a href="http://technorati.com/tags/UpdateSourceTrigger" rel="tag">UpdateSourceTrigger</a>,<a href="http://technorati.com/tags/Explicit" rel="tag">Explicit</a>,<a href="http://technorati.com/tags/Width" rel="tag">Width</a>,<a href="http://technorati.com/tags/PasswordBox" rel="tag">PasswordBox</a>,<a href="http://technorati.com/tags/PasswordText" rel="tag">PasswordText</a>,<a href="http://technorati.com/tags/PasswordChar" rel="tag">PasswordChar</a>,<a href="http://technorati.com/tags/UsernameLabel" rel="tag">UsernameLabel</a>,<a href="http://technorati.com/tags/TextAlignment" rel="tag">TextAlignment</a>,<a href="http://technorati.com/tags/PasswordLabel" rel="tag">PasswordLabel</a>,<a href="http://technorati.com/tags/ApplicationTitle" rel="tag">ApplicationTitle</a>,<a href="http://technorati.com/tags/Application" rel="tag">Application</a>,<a href="http://technorati.com/tags/ListName" rel="tag">ListName</a>,<a href="http://technorati.com/tags/Login" rel="tag">Login</a>,<a href="http://technorati.com/tags/Name" rel="tag">Name</a>,<a href="http://technorati.com/tags/DataContext" rel="tag">DataContext</a>,<a href="http://technorati.com/tags/ViewModelLocator" rel="tag">ViewModelLocator</a>,<a href="http://technorati.com/tags/Command" rel="tag">Command</a>,<a href="http://technorati.com/tags/MainViewModel" rel="tag">MainViewModel</a>,<a href="http://technorati.com/tags/wire" rel="tag">wire</a>,<a href="http://technorati.com/tags/EventToCommand" rel="tag">EventToCommand</a>,<a href="http://technorati.com/tags/framework" rel="tag">framework</a>,<a href="http://technorati.com/tags/FrameworkElement" rel="tag">FrameworkElement</a>,<a href="http://technorati.com/tags/element" rel="tag">element</a>,<a href="http://technorati.com/tags/reference" rel="tag">reference</a>,<a href="http://technorati.com/tags/archive" rel="tag">archive</a>,<a href="http://technorati.com/tags/Open" rel="tag">Open</a>,<a href="http://technorati.com/tags/Assets" rel="tag">Assets</a>,<a href="http://technorati.com/tags/panel" rel="tag">panel</a>,<a href="http://technorati.com/tags/Drag" rel="tag">Drag</a>,<a href="http://technorati.com/tags/Objects" rel="tag">Objects</a>,<a href="http://technorati.com/tags/Timelines" rel="tag">Timelines</a>,<a href="http://technorati.com/tags/pane" rel="tag">pane</a>,<a href="http://technorati.com/tags/Click" rel="tag">Click</a>,<a href="http://technorati.com/tags/data" rel="tag">data</a>,<a href="http://technorati.com/tags/LoginCommand" rel="tag">LoginCommand</a>,<a href="http://technorati.com/tags/Event" rel="tag">Event</a>,<a href="http://technorati.com/tags/behaviour" rel="tag">behaviour</a>,<a href="http://technorati.com/tags/option" rel="tag">option</a>,<a href="http://technorati.com/tags/result" rel="tag">result</a>,<a href="http://technorati.com/tags/custom" rel="tag">custom</a>,<a href="http://technorati.com/tags/Behaviors" rel="tag">Behaviors</a>,<a href="http://technorati.com/tags/DataBindTextBoxOnPropertyChangedBehaviour" rel="tag">DataBindTextBoxOnPropertyChangedBehaviour</a>,<a href="http://technorati.com/tags/Behavior" rel="tag">Behavior</a>,<a href="http://technorati.com/tags/BindingExpression" rel="tag">BindingExpression</a>,<a href="http://technorati.com/tags/AssociatedObject" rel="tag">AssociatedObject</a>,<a href="http://technorati.com/tags/GetBindingExpression" rel="tag">GetBindingExpression</a>,<a href="http://technorati.com/tags/sender" rel="tag">sender</a>,<a href="http://technorati.com/tags/TextChangedEventArgs" rel="tag">TextChangedEventArgs</a>,<a href="http://technorati.com/tags/UpdateSource" rel="tag">UpdateSource</a>,<a href="http://technorati.com/tags/Once" rel="tag">Once</a>,<a href="http://technorati.com/tags/Also" rel="tag">Also</a>,<a href="http://technorati.com/tags/CanExecute" rel="tag">CanExecute</a>,<a href="http://technorati.com/tags/CanExecuteLogin" rel="tag">CanExecuteLogin</a>,<a href="http://technorati.com/tags/Length" rel="tag">Length</a>,<a href="http://technorati.com/tags/response" rel="tag">response</a>,<a href="http://technorati.com/tags/Subscribe" rel="tag">Subscribe</a>,<a href="http://technorati.com/tags/handler" rel="tag">handler</a>,<a href="http://technorati.com/tags/PropertyChangedEventArgs" rel="tag">PropertyChangedEventArgs</a>,<a href="http://technorati.com/tags/PropertyName" rel="tag">PropertyName</a>,<a href="http://technorati.com/tags/RelayCommand" rel="tag">RelayCommand</a>,<a href="http://technorati.com/tags/areas" rel="tag">areas</a>,<a href="http://technorati.com/tags/parameters" rel="tag">parameters</a>,<a href="http://technorati.com/tags/options" rel="tag">options</a>,<a href="http://technorati.com/tags/boxes" rel="tag">boxes</a><br />
<p></p>Windows Live Tags: <a href="http://windows.live.com/connect/tag/MVVM" rel="clubhouseTag">MVVM</a>,<a href="http://windows.live.com/connect/tag/Part" rel="clubhouseTag">Part</a>,<a href="http://windows.live.com/connect/tag/Peter" rel="clubhouseTag">Peter</a>,<a href="http://windows.live.com/connect/tag/Daukintis" rel="clubhouseTag">Daukintis</a>,<a href="http://windows.live.com/connect/tag/Okay" rel="clubhouseTag">Okay</a>,<a href="http://windows.live.com/connect/tag/haven" rel="clubhouseTag">haven</a>,<a href="http://windows.live.com/connect/tag/user" rel="clubhouseTag">user</a>,<a href="http://windows.live.com/connect/tag/interface" rel="clubhouseTag">interface</a>,<a href="http://windows.live.com/connect/tag/supports" rel="clubhouseTag">supports</a>,<a href="http://windows.live.com/connect/tag/theory" rel="clubhouseTag">theory</a>,<a href="http://windows.live.com/connect/tag/code" rel="clubhouseTag">code</a>,<a href="http://windows.live.com/connect/tag/Create" rel="clubhouseTag">Create</a>,<a href="http://windows.live.com/connect/tag/password" rel="clubhouseTag">password</a>,<a href="http://windows.live.com/connect/tag/indicator" rel="clubhouseTag">indicator</a>,<a href="http://windows.live.com/connect/tag/UserControl" rel="clubhouseTag">UserControl</a>,<a href="http://windows.live.com/connect/tag/transition" rel="clubhouseTag">transition</a>,<a href="http://windows.live.com/connect/tag/Expression" rel="clubhouseTag">Expression</a>,<a href="http://windows.live.com/connect/tag/Blend" rel="clubhouseTag">Blend</a>,<a href="http://windows.live.com/connect/tag/Microsoft" rel="clubhouseTag">Microsoft</a>,<a href="http://windows.live.com/connect/tag/Preview" rel="clubhouseTag">Preview</a>,<a href="http://windows.live.com/connect/tag/Software" rel="clubhouseTag">Software</a>,<a href="http://windows.live.com/connect/tag/Development" rel="clubhouseTag">Development</a>,<a href="http://windows.live.com/connect/tag/solution" rel="clubhouseTag">solution</a>,<a href="http://windows.live.com/connect/tag/TextBlock" rel="clubhouseTag">TextBlock</a>,<a href="http://windows.live.com/connect/tag/Text" rel="clubhouseTag">Text</a>,<a href="http://windows.live.com/connect/tag/Welcome" rel="clubhouseTag">Welcome</a>,<a href="http://windows.live.com/connect/tag/Style" rel="clubhouseTag">Style</a>,<a href="http://windows.live.com/connect/tag/StaticResource" rel="clubhouseTag">StaticResource</a>,<a href="http://windows.live.com/connect/tag/PhoneTextNormalStyle" rel="clubhouseTag">PhoneTextNormalStyle</a>,<a href="http://windows.live.com/connect/tag/HorizontalAlignment" rel="clubhouseTag">HorizontalAlignment</a>,<a href="http://windows.live.com/connect/tag/Center" rel="clubhouseTag">Center</a>,<a href="http://windows.live.com/connect/tag/VerticalAlignment" rel="clubhouseTag">VerticalAlignment</a>,<a href="http://windows.live.com/connect/tag/FontSize" rel="clubhouseTag">FontSize</a>,<a href="http://windows.live.com/connect/tag/Margin" rel="clubhouseTag">Margin</a>,<a href="http://windows.live.com/connect/tag/Button" rel="clubhouseTag">Button</a>,<a href="http://windows.live.com/connect/tag/Content" rel="clubhouseTag">Content</a>,<a href="http://windows.live.com/connect/tag/LoginButtonName" rel="clubhouseTag">LoginButtonName</a>,<a href="http://windows.live.com/connect/tag/TextBox" rel="clubhouseTag">TextBox</a>,<a href="http://windows.live.com/connect/tag/Left" rel="clubhouseTag">Left</a>,<a href="http://windows.live.com/connect/tag/UsernameText" rel="clubhouseTag">UsernameText</a>,<a href="http://windows.live.com/connect/tag/Mode" rel="clubhouseTag">Mode</a>,<a href="http://windows.live.com/connect/tag/TwoWay" rel="clubhouseTag">TwoWay</a>,<a href="http://windows.live.com/connect/tag/UpdateSourceTrigger" rel="clubhouseTag">UpdateSourceTrigger</a>,<a href="http://windows.live.com/connect/tag/Explicit" rel="clubhouseTag">Explicit</a>,<a href="http://windows.live.com/connect/tag/Width" rel="clubhouseTag">Width</a>,<a href="http://windows.live.com/connect/tag/PasswordBox" rel="clubhouseTag">PasswordBox</a>,<a href="http://windows.live.com/connect/tag/PasswordText" rel="clubhouseTag">PasswordText</a>,<a href="http://windows.live.com/connect/tag/PasswordChar" rel="clubhouseTag">PasswordChar</a>,<a href="http://windows.live.com/connect/tag/UsernameLabel" rel="clubhouseTag">UsernameLabel</a>,<a href="http://windows.live.com/connect/tag/TextAlignment" rel="clubhouseTag">TextAlignment</a>,<a href="http://windows.live.com/connect/tag/PasswordLabel" rel="clubhouseTag">PasswordLabel</a>,<a href="http://windows.live.com/connect/tag/ApplicationTitle" rel="clubhouseTag">ApplicationTitle</a>,<a href="http://windows.live.com/connect/tag/Application" rel="clubhouseTag">Application</a>,<a href="http://windows.live.com/connect/tag/ListName" rel="clubhouseTag">ListName</a>,<a href="http://windows.live.com/connect/tag/Login" rel="clubhouseTag">Login</a>,<a href="http://windows.live.com/connect/tag/Name" rel="clubhouseTag">Name</a>,<a href="http://windows.live.com/connect/tag/DataContext" rel="clubhouseTag">DataContext</a>,<a href="http://windows.live.com/connect/tag/ViewModelLocator" rel="clubhouseTag">ViewModelLocator</a>,<a href="http://windows.live.com/connect/tag/Command" rel="clubhouseTag">Command</a>,<a href="http://windows.live.com/connect/tag/MainViewModel" rel="clubhouseTag">MainViewModel</a>,<a href="http://windows.live.com/connect/tag/wire" rel="clubhouseTag">wire</a>,<a href="http://windows.live.com/connect/tag/EventToCommand" rel="clubhouseTag">EventToCommand</a>,<a href="http://windows.live.com/connect/tag/framework" rel="clubhouseTag">framework</a>,<a href="http://windows.live.com/connect/tag/FrameworkElement" rel="clubhouseTag">FrameworkElement</a>,<a href="http://windows.live.com/connect/tag/element" rel="clubhouseTag">element</a>,<a href="http://windows.live.com/connect/tag/reference" rel="clubhouseTag">reference</a>,<a href="http://windows.live.com/connect/tag/archive" rel="clubhouseTag">archive</a>,<a href="http://windows.live.com/connect/tag/Open" rel="clubhouseTag">Open</a>,<a href="http://windows.live.com/connect/tag/Assets" rel="clubhouseTag">Assets</a>,<a href="http://windows.live.com/connect/tag/panel" rel="clubhouseTag">panel</a>,<a href="http://windows.live.com/connect/tag/Drag" rel="clubhouseTag">Drag</a>,<a href="http://windows.live.com/connect/tag/Objects" rel="clubhouseTag">Objects</a>,<a href="http://windows.live.com/connect/tag/Timelines" rel="clubhouseTag">Timelines</a>,<a href="http://windows.live.com/connect/tag/pane" rel="clubhouseTag">pane</a>,<a href="http://windows.live.com/connect/tag/Click" rel="clubhouseTag">Click</a>,<a href="http://windows.live.com/connect/tag/data" rel="clubhouseTag">data</a>,<a href="http://windows.live.com/connect/tag/LoginCommand" rel="clubhouseTag">LoginCommand</a>,<a href="http://windows.live.com/connect/tag/Event" rel="clubhouseTag">Event</a>,<a href="http://windows.live.com/connect/tag/behaviour" rel="clubhouseTag">behaviour</a>,<a href="http://windows.live.com/connect/tag/option" rel="clubhouseTag">option</a>,<a href="http://windows.live.com/connect/tag/result" rel="clubhouseTag">result</a>,<a href="http://windows.live.com/connect/tag/custom" rel="clubhouseTag">custom</a>,<a href="http://windows.live.com/connect/tag/Behaviors" rel="clubhouseTag">Behaviors</a>,<a href="http://windows.live.com/connect/tag/DataBindTextBoxOnPropertyChangedBehaviour" rel="clubhouseTag">DataBindTextBoxOnPropertyChangedBehaviour</a>,<a href="http://windows.live.com/connect/tag/Behavior" rel="clubhouseTag">Behavior</a>,<a href="http://windows.live.com/connect/tag/BindingExpression" rel="clubhouseTag">BindingExpression</a>,<a href="http://windows.live.com/connect/tag/AssociatedObject" rel="clubhouseTag">AssociatedObject</a>,<a href="http://windows.live.com/connect/tag/GetBindingExpression" rel="clubhouseTag">GetBindingExpression</a>,<a href="http://windows.live.com/connect/tag/sender" rel="clubhouseTag">sender</a>,<a href="http://windows.live.com/connect/tag/TextChangedEventArgs" rel="clubhouseTag">TextChangedEventArgs</a>,<a href="http://windows.live.com/connect/tag/UpdateSource" rel="clubhouseTag">UpdateSource</a>,<a href="http://windows.live.com/connect/tag/Once" rel="clubhouseTag">Once</a>,<a href="http://windows.live.com/connect/tag/Also" rel="clubhouseTag">Also</a>,<a href="http://windows.live.com/connect/tag/CanExecute" rel="clubhouseTag">CanExecute</a>,<a href="http://windows.live.com/connect/tag/CanExecuteLogin" rel="clubhouseTag">CanExecuteLogin</a>,<a href="http://windows.live.com/connect/tag/Length" rel="clubhouseTag">Length</a>,<a href="http://windows.live.com/connect/tag/response" rel="clubhouseTag">response</a>,<a href="http://windows.live.com/connect/tag/Subscribe" rel="clubhouseTag">Subscribe</a>,<a href="http://windows.live.com/connect/tag/handler" rel="clubhouseTag">handler</a>,<a href="http://windows.live.com/connect/tag/PropertyChangedEventArgs" rel="clubhouseTag">PropertyChangedEventArgs</a>,<a href="http://windows.live.com/connect/tag/PropertyName" rel="clubhouseTag">PropertyName</a>,<a href="http://windows.live.com/connect/tag/RelayCommand" rel="clubhouseTag">RelayCommand</a>,<a href="http://windows.live.com/connect/tag/areas" rel="clubhouseTag">areas</a>,<a href="http://windows.live.com/connect/tag/parameters" rel="clubhouseTag">parameters</a>,<a href="http://windows.live.com/connect/tag/options" rel="clubhouseTag">options</a>,<a href="http://windows.live.com/connect/tag/boxes" rel="clubhouseTag">boxes</a>  </li></div>