---
layout: post
title: Simple textbox validation wp7
date: 2010-10-21 09:58
author: peted70
comments: true
categories: [Windows Phone 7]
---
<p>Since wp7 is based on Silverlight 3 then input validation can be done (love or loathe it) via exceptions. Here’s a simple example…</p>  <pre class="code">        <span style="color:#d2d200;">&lt;Grid x:Name=</span><span style="color:white;">&quot;ContentPanel&quot; </span><span style="color:#d2d200;">Grid.Row=</span><span style="color:white;">&quot;1&quot;</span><span style="color:#d2d200;">&gt;
            &lt;Border x:Name=</span><span style="color:white;">&quot;errorBorder&quot; </span><span style="color:#d2d200;">Background=</span><span style="color:white;">&quot;Red&quot; </span><span style="color:#d2d200;">Opacity=</span><span style="color:white;">&quot;0&quot;</span><span style="color:#d2d200;">&gt;&lt;/Border&gt;
            &lt;TextBox Margin=</span><span style="color:white;">&quot;8&quot; 
                     </span><span style="color:#d2d200;">Text=&quot;{</span><span style="color:cyan;">Binding </span><span style="color:white;">MyText</span><span style="color:#d2d200;">, </span><span style="color:white;">Mode</span><span style="color:#d2d200;">=</span><span style="color:lime;">TwoWay</span><span style="color:#d2d200;">, </span><span style="color:white;">NotifyOnValidationError</span><span style="color:#d2d200;">=</span><span style="color:lime;">True</span><span style="color:#d2d200;">, </span><span style="color:white;">ValidatesOnExceptions</span><span style="color:#d2d200;">=</span><span style="color:lime;">True</span><span style="color:#d2d200;">}</span><span style="color:white;">&quot; 
                     </span><span style="color:#d2d200;">BindingValidationError=</span><span style="color:white;">&quot;ContentPanel_BindingValidationError&quot;</span><span style="color:#d2d200;">&gt;
                &lt;TextBox.InputScope&gt;
                    &lt;InputScope&gt;
                        &lt;InputScope.Names&gt;
                            &lt;InputScopeName&gt;</span><span style="color:#c89191;">Default</span><span style="color:#d2d200;">&lt;/InputScopeName&gt;
                        &lt;/InputScope.Names&gt;
                    &lt;/InputScope&gt;
                &lt;/TextBox.InputScope&gt;
            &lt;/TextBox&gt;
        &lt;/Grid&gt;

</span></pre>

<p>&#160;</p>

<p>Note that the Textbox is bound to a text property and NotifyOnValidationError and ValidatesOnExceptions properties have been set to true. Also, the binding will be triggered when the TextBox loses focus. Setting NotifyOnValdationError means that my event handler for BindingValidationError event will be called when a validation error is detected. Following from that, a validation error will be detected when my bound property setter (for ‘MyText’) throws an exception. Here’s the code for that….</p>

<p>&#160;</p>

<pre class="code">   <span style="color:#eaeaac;">public partial class </span><span style="color:#f0dfaf;">MainPage </span><span style="color:#d2d200;">: </span><span style="color:#f0dfaf;">PhoneApplicationPage</span><span style="color:#d2d200;">, </span><span style="color:#2b91af;">INotifyPropertyChanged
    </span><span style="color:#d2d200;">{
        </span><span style="color:#eaeaac;">public event </span><span style="color:#2b91af;">PropertyChangedEventHandler </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public void </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">string </span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">)
        {
            </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">PropertyChanged </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">null</span><span style="color:#d2d200;">)
            {
                </span><span style="color:#f8ffc6;">PropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">, </span><span style="color:#eaeaac;">new </span><span style="color:#f0dfaf;">PropertyChangedEventArgs</span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">propertyName</span><span style="color:#d2d200;">));
            }
        }

        </span><span style="color:#eaeaac;">private string </span><span style="color:#f8ffc6;">_myText</span><span style="color:#d2d200;">;

        </span><span style="color:#eaeaac;">public string </span><span style="color:#f8ffc6;">MyText
        </span><span style="color:#d2d200;">{
            </span><span style="color:#eaeaac;">get </span><span style="color:#d2d200;">{ </span><span style="color:#eaeaac;">return </span><span style="color:#f8ffc6;">_myText</span><span style="color:#d2d200;">; }
            </span><span style="color:#eaeaac;">set
            </span><span style="color:#d2d200;">{
                </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#f8ffc6;">_myText </span><span style="color:#d2d200;">!= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">)
                {
                    </span><span style="color:#eaeaac;">if </span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Contains</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;.&quot;</span><span style="color:#d2d200;">))
                    {
                        </span><span style="color:#eaeaac;">throw new </span><span style="color:#f0dfaf;">Exception</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;Cannot contain '.'&quot;</span><span style="color:#d2d200;">);
                    }

                    </span><span style="color:#f8ffc6;">_myText </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">value</span><span style="color:#d2d200;">;
                    </span><span style="color:#f8ffc6;">OnPropertyChanged</span><span style="color:#d2d200;">(</span><span style="color:#c89191;">&quot;MyText&quot;</span><span style="color:#d2d200;">);
                }
            }
        }

        </span><span style="color:lime;">// Constructor
        </span><span style="color:#eaeaac;">public </span><span style="color:#f8ffc6;">MainPage</span><span style="color:#d2d200;">()
        {
            </span><span style="color:#f8ffc6;">InitializeComponent</span><span style="color:#d2d200;">();
            </span><span style="color:#f8ffc6;">DataContext </span><span style="color:#d2d200;">= </span><span style="color:#eaeaac;">this</span><span style="color:#d2d200;">;
        }

        </span><span style="color:#eaeaac;">private void </span><span style="color:#f8ffc6;">ContentPanel_BindingValidationError</span><span style="color:#d2d200;">(</span><span style="color:#eaeaac;">object </span><span style="color:#f8ffc6;">sender</span><span style="color:#d2d200;">, </span><span style="color:#f0dfaf;">ValidationErrorEventArgs </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">)
        {
            </span><span style="color:#f8ffc6;">errorBorder</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Opacity </span><span style="color:#d2d200;">= </span><span style="color:#f8ffc6;">e</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Action </span><span style="color:#d2d200;">== </span><span style="color:#2b91af;">ValidationErrorEventAction</span><span style="color:#d2d200;">.</span><span style="color:#f8ffc6;">Removed </span><span style="color:#d2d200;">? </span><span style="color:#8acccf;">0 </span><span style="color:#d2d200;">: </span><span style="color:#8acccf;">100</span><span style="color:#d2d200;">;
        }
    }

</span></pre>

<p>&#160;</p>

<p>&#160;</p>

<p>I haven’t been very creative with the visuals here…</p>

<p>Note that I have placed all of this code in the code-behind purely for convenience – I would usually implement this with a view model.</p>
