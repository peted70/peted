---
layout: post
title: TDD Introduction
date: 2010-03-31 00:50
author: peted70
comments: true
categories: [TDD]
---
<div id="msgcns!4F1B7368284539E5!202" class="bvMsg"><h5>by Peter Daukintis</h5> <h3>Test Driven Development</h3> <p>This is a basic introduction to test-driven development describing related technologies in a simple, straightforward way. The learning curve can sometimes appear steep due to the range of associated technologies and architectural concepts that need to be understood. This post is a quick overview with some simple examples. </p> <h4>Basic Principles</h4> <p>TDD supports agile processes and coping with changing business requirements. It does this by enabling and promoting sound, flexible architectures which support unit testing. The test frameworks this allows, in turn, enables refactoring by providing confidence that existing functionality is not being broken. It supports a ‘clear separation of concerns’ via inversion of control patterns (<a title="http://www.martinfowler.com/articles/injection.html" href="http://www.martinfowler.com/articles/injection.html">http://www.martinfowler.com/articles/injection.html</a>).  This enables dependencies to be easily replaced, promotes flexible architectures and enables runtime composition of logical components. </p> <h4>Dependencies</h4><pre><span style="background:#2e2e2e;color:#d2d200;">        </span><span style="background:#2e2e2e;color:#eaeaac;">public </span><span style="background:#2e2e2e;color:#2b91af;">IEnumerable</span><span style="background:#2e2e2e;color:#d2d200;">&lt;</span><span style="background:#2e2e2e;color:#f0dfaf;">Stuff</span><span style="background:#2e2e2e;color:#d2d200;">&gt; </span><span style="background:#2e2e2e;color:#f8ffc6;">GetFilteredStuff</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#eaeaac;">string </span><span style="background:#2e2e2e;color:#f8ffc6;">filter</span><span style="background:#2e2e2e;color:#d2d200;">)
        &#123;
            </span><span style="background:#2e2e2e;color:#f8ffc6;">DatabaseLayer dbl </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#eaeaac;">new </span><span style="background:#2e2e2e;color:#f8ffc6;">DatabaseLayer</span><span style="background:#2e2e2e;color:#d2d200;">();
            </span><span style="background:#2e2e2e;color:#eaeaac;">var </span><span style="background:#2e2e2e;color:#f8ffc6;">stuff </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#f8ffc6;">dbl</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">GetStuffFromDB</span><span style="background:#2e2e2e;color:#d2d200;">();
            </span><span style="background:#2e2e2e;color:#eaeaac;">return </span><span style="background:#2e2e2e;color:#f8ffc6;">stuff</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Where</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">stuff </span><span style="background:#2e2e2e;color:#d2d200;">=&gt; </span><span style="background:#2e2e2e;color:#f8ffc6;">stuff</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Name</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">startsWith</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">filter</span><span style="background:#2e2e2e;color:#d2d200;">));
        &#125;
</span></pre><a href="http://11011.net/software/vspaste"></a>
<p> </p>
<p>This code illustrates a fixed dependency on the concrete database layer class. This prevents creation of a unit test without a database.</p>
<p>Using an interface we can remove the dependency:</p><pre><span style="background:#2e2e2e;color:#d2d200;">        </span><span style="background:#2e2e2e;color:#eaeaac;">public </span><span style="background:#2e2e2e;color:#2b91af;">IEnumerable</span><span style="background:#2e2e2e;color:#d2d200;">&lt;</span><span style="background:#2e2e2e;color:#f0dfaf;">Stuff</span><span style="background:#2e2e2e;color:#d2d200;">&gt; </span><span style="background:#2e2e2e;color:#f8ffc6;">GetFilteredStuff</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#eaeaac;">string </span><span style="background:#2e2e2e;color:#f8ffc6;">filter</span><span style="background:#2e2e2e;color:#d2d200;">, </span><span style="background:#2e2e2e;color:#2b91af;">IDatabaseRepository </span><span style="background:#2e2e2e;color:#f8ffc6;">repository</span><span style="background:#2e2e2e;color:#d2d200;">)
        &#123;
            </span><span style="background:#2e2e2e;color:#eaeaac;">if </span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">repository</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">AllowsFiltering</span><span style="background:#2e2e2e;color:#d2d200;">())
            &#123;
                </span><span style="background:#2e2e2e;color:#eaeaac;">var </span><span style="background:#2e2e2e;color:#f8ffc6;">stuff </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#f8ffc6;">repository</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">GetStuffFromDb</span><span style="background:#2e2e2e;color:#d2d200;">();
                </span><span style="background:#2e2e2e;color:#eaeaac;">return </span><span style="background:#2e2e2e;color:#f8ffc6;">stuff</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Where</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">s </span><span style="background:#2e2e2e;color:#d2d200;">=&gt; </span><span style="background:#2e2e2e;color:#f8ffc6;">s</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Name</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">StartsWith</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">filter</span><span style="background:#2e2e2e;color:#d2d200;">));
            &#125;
            </span><span style="background:#2e2e2e;color:#eaeaac;">return null</span><span style="background:#2e2e2e;color:#d2d200;">;
        &#125;
</span></pre>
<p>Now the dependency is gone and we can unit test the method by providing a stub class which implements the IDatabaseRepository interface.</p><pre><span style="background:#2e2e2e;color:#d2d200;">        [</span><span style="background:#2e2e2e;color:#f0dfaf;">TestMethod</span><span style="background:#2e2e2e;color:#d2d200;">]
        </span><span style="background:#2e2e2e;color:#eaeaac;">public void </span><span style="background:#2e2e2e;color:#f8ffc6;">GetFilteredStuff_GivenValidFilter_ReturnsCorrectFilteredResult
        </span><span style="background:#2e2e2e;color:#d2d200;">&#123;
            </span><span style="background:#2e2e2e;color:#f8ffc6;">IDatabaseRepsitory db </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#eaeaac;">new </span><span style="background:#2e2e2e;color:#f8ffc6;">StubDatabaseRepository</span><span style="background:#2e2e2e;color:#d2d200;">();
            </span><span style="background:#2e2e2e;color:#f8ffc6;">var result </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#f8ffc6;">db</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">GetFilteredStuff</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#c89191;">&quot;Bob&quot;</span><span style="background:#2e2e2e;color:#d2d200;">, </span><span style="background:#2e2e2e;color:#f8ffc6;">db</span><span style="background:#2e2e2e;color:#d2d200;">);

            </span><span style="background:#2e2e2e;color:lime;">// Assert expected result
        </span><span style="background:#2e2e2e;color:#d2d200;">&#125;
</span></pre>
<h4>Unit Testing</h4>
<p>So, working this way I can test each individual unit of logic. This promotes a flexible architecture and single responsibility classes and methods. If we were to test the original method with the database dependency by supplying a known database this would be integration testing. 
<h4>Tools</h4>
<p>As TDD has become more popular a network of support tools has flourished which really enable the viability of TDD. These are the main categories:</p>
<ul>
<li>Inversion of Control Containers (e.g. Unity - <a title="http://unity.codeplex.com/Wikipage" href="http://unity.codeplex.com/Wikipage">http://unity.codeplex.com/Wikipage</a>) 
<li>Mocking libraries (e.g. Rhino Mocks - <a title="http://www.ayende.com/projects/rhino-mocks.aspx" href="http://www.ayende.com/projects/rhino-mocks.aspx">http://www.ayende.com/projects/rhino-mocks.aspx</a>) 
<li>Refactoring tools (e.g. Resharper - <a title="http://www.jetbrains.com/resharper/" href="http://www.jetbrains.com/resharper/">http://www.jetbrains.com/resharper/</a>)</li></ul>
<p>Let’s look into each category in turn….</p>
<h4>Inversion of Control Containers for Dependency Injection</h4>
<p>To remove dependencies we can eliminate 'new myConcreteClass' by passing around interfaces but something has to be responsible for generating the concrete classes. In order to centralise the creation code we can make use of the factory pattern. Luckily we don't need to write the code for the factory in this scenario since there are numerous implementations freely available. These are known as IOC (Inversion of control) containers. e.g. Unity, StructureMap, etc. 
<h4>Mocking Libraries</h4>
<p>Note. The terminology related to using fake implementations can be confusing: I will refer to a stub as a fake which I can configure but I will not assert against its internal state. A mock is more advanced and can track when it’s methods are called, how many times they are called, with which parameters, etc.. These values can be asserted against in your unit tests. </p>
<p>In my unit tests I can use stubbed implementations for the dependent interfaces. Sometimes I would want to generate concrete stubs. However, I can use a Mocking library to automatically generate a stub. I can configure the generated stub to return what I want it to return. 
<p>Most mocking libraries work by automatically re-implementing your interface via reflection and so are limited to stubbingmocking interfaces. Some libraries, such as Typemock Isolator and MS Moles use the .NET profiler APIs <a title="http://msdn.microsoft.com/en-us/magazine/cc301725.aspx" href="http://msdn.microsoft.com/en-us/magazine/cc301725.aspx">http://msdn.microsoft.com/en-us/magazine/cc301725.aspx</a> or IL rewriting and can stub/mock any .NET class. 
<p>Here’s an example of using a stub to test the previous example (using R|hino Mocks):<pre><span style="background:#2e2e2e;color:#d2d200;">        [</span><span style="background:#2e2e2e;color:#f0dfaf;">TestMethod</span><span style="background:#2e2e2e;color:#d2d200;">]
        </span><span style="background:#2e2e2e;color:#eaeaac;">public void </span><span style="background:#2e2e2e;color:#f8ffc6;">GetFilteredStuff_GivenValidFilter_ReturnsCorrectFilteredResult</span><span style="background:#2e2e2e;color:#d2d200;">()
        &#123;
            </span><span style="background:#2e2e2e;color:#eaeaac;">var </span><span style="background:#2e2e2e;color:#f8ffc6;">mocks </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#eaeaac;">new </span><span style="background:#2e2e2e;color:#f0dfaf;">MockRepository</span><span style="background:#2e2e2e;color:#d2d200;">();
            </span><span style="background:#2e2e2e;color:#eaeaac;">var </span><span style="background:#2e2e2e;color:#f8ffc6;">repo </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#f8ffc6;">mocks</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Stub</span><span style="background:#2e2e2e;color:#d2d200;">&lt;</span><span style="background:#2e2e2e;color:#2b91af;">IDatabaseRepository</span><span style="background:#2e2e2e;color:#d2d200;">&gt;();

            </span><span style="background:#2e2e2e;color:#eaeaac;">using </span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">mocks</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Record</span><span style="background:#2e2e2e;color:#d2d200;">())
            &#123;
                </span><span style="background:#2e2e2e;color:#f8ffc6;">repo</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Stub</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">t </span><span style="background:#2e2e2e;color:#d2d200;">=&gt; </span><span style="background:#2e2e2e;color:#f8ffc6;">t</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">AllowsFiltering</span><span style="background:#2e2e2e;color:#d2d200;">()).</span><span style="background:#2e2e2e;color:#f8ffc6;">Return</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#eaeaac;">true</span><span style="background:#2e2e2e;color:#d2d200;">);
                </span><span style="background:#2e2e2e;color:#f8ffc6;">repo</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Stub</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">t </span><span style="background:#2e2e2e;color:#d2d200;">=&gt; </span><span style="background:#2e2e2e;color:#f8ffc6;">t</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">GetStuffFromDb</span><span style="background:#2e2e2e;color:#d2d200;">()).</span><span style="background:#2e2e2e;color:#f8ffc6;">Return</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#eaeaac;">new </span><span style="background:#2e2e2e;color:#f0dfaf;">List</span><span style="background:#2e2e2e;color:#d2d200;">&lt;</span><span style="background:#2e2e2e;color:#f0dfaf;">Stuff</span><span style="background:#2e2e2e;color:#d2d200;">&gt;
                                                              &#123;
                                                                  </span><span style="background:#2e2e2e;color:#eaeaac;">new </span><span style="background:#2e2e2e;color:#f0dfaf;">Stuff </span><span style="background:#2e2e2e;color:#d2d200;">&#123; </span><span style="background:#2e2e2e;color:#f8ffc6;">Name </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#c89191;">&quot;Bob&quot; </span><span style="background:#2e2e2e;color:#d2d200;">&#125;,
                                                                  </span><span style="background:#2e2e2e;color:#eaeaac;">new </span><span style="background:#2e2e2e;color:#f0dfaf;">Stuff </span><span style="background:#2e2e2e;color:#d2d200;">&#123; </span><span style="background:#2e2e2e;color:#f8ffc6;">Name </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#c89191;">&quot;Fred&quot; </span><span style="background:#2e2e2e;color:#d2d200;">&#125;
                                                              &#125;);
            &#125;
            </span><span style="background:#2e2e2e;color:#eaeaac;">using </span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#f8ffc6;">mocks</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">Playback</span><span style="background:#2e2e2e;color:#d2d200;">())
            &#123;
                </span><span style="background:#2e2e2e;color:#eaeaac;">var </span><span style="background:#2e2e2e;color:#f8ffc6;">mc </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#eaeaac;">new </span><span style="background:#2e2e2e;color:#f0dfaf;">MyConcreteClass</span><span style="background:#2e2e2e;color:#d2d200;">();
                </span><span style="background:#2e2e2e;color:#eaeaac;">var </span><span style="background:#2e2e2e;color:#f8ffc6;">result </span><span style="background:#2e2e2e;color:#d2d200;">= </span><span style="background:#2e2e2e;color:#f8ffc6;">mc</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">GetFilteredStuff</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#c89191;">&quot;Bob&quot;</span><span style="background:#2e2e2e;color:#d2d200;">, </span><span style="background:#2e2e2e;color:#f8ffc6;">repo</span><span style="background:#2e2e2e;color:#d2d200;">);
                </span><span style="background:#2e2e2e;color:#f0dfaf;">Assert</span><span style="background:#2e2e2e;color:#d2d200;">.</span><span style="background:#2e2e2e;color:#f8ffc6;">AreEqual</span><span style="background:#2e2e2e;color:#d2d200;">(</span><span style="background:#2e2e2e;color:#8acccf;">1</span><span style="background:#2e2e2e;color:#d2d200;">, </span><span style="background:#2e2e2e;color:#f8ffc6;">result</span><span style="background:#2e2e2e;color:#d2d200;">);
            &#125;
        &#125;
</span></pre>
<p> </p>
<p>Notice that the mocking library allows me to dynamically generate the stub class which implements the IDatabaseRepository interface.</p>
<h4>Refactoring Tools</h4>
<p>Resharper is a refactoring and code productivity tool which integrates with visual studio and provides the following features:</p>
<ul>
<li>Refactoring 
<li>Code Analysis - on the fly (squiggly lines) 
<li>Unit testing tools 
<li>Code generation tools 
<li>Code formatting 
<li>Shared coding standards</li></li></li></ul>
<p>Here’s an example of it’s usage <a title="Windows Media demo" href="http://www.jetbrains.com/resharper/documentation/presentation/codingSession/CodingSession.wmv">Windows Media demo</a></p>
<p>I personally use resharper and have found it to give a great productivity boost and generally supports my TDD coding amongst other things. (Please email me for a 10% discsount code on resharper licenses – <a href="mailto:babaandthepigman@hotmail.co.uk">babaandthepigman@hotmail.co.uk</a> ).</p>
<h4>Calculator Kata</h4>
<p>To practise some TDD skills this gives an interesting exercise <a title="http://osherove.com/tdd-kata-1/" href="http://osherove.com/tdd-kata-1/">http://osherove.com/tdd-kata-1/</a> for a very simple string calculator (and some videos of people carrying out the exercise).</p>
<p>That’s it for the intro….</p>
<p></p>Technorati Tags: <a href="http://technorati.com/tags/Introduction" rel="tag">Introduction</a>, <a href="http://technorati.com/tags/Test-driven" target="_blank">test-driven</a>,<a href="http://technorati.com/tags/development" rel="tag">development</a>,<a href="http://technorati.com/tags/overview" rel="tag">overview</a>,<a href="http://technorati.com/tags/Basic" rel="tag">Basic</a>,<a href="http://technorati.com/tags/Principles" rel="tag">Principles</a>,<a href="http://technorati.com/tags/supports" rel="tag">supports</a>,<a href="http://technorati.com/tags/requirements" rel="tag">requirements</a>,<a href="http://technorati.com/tags/unit" rel="tag">unit</a>,<a href="http://technorati.com/tags/separation" rel="tag">separation</a>,<a href="http://technorati.com/tags/injection" rel="tag">injection</a>,<a href="http://technorati.com/tags/composition" rel="tag">composition</a>,<a href="http://technorati.com/tags/components" rel="tag">components</a>,<a href="http://technorati.com/tags/IEnumerable" rel="tag">IEnumerable</a>,<a href="http://technorati.com/tags/GetFilteredStuff" rel="tag">GetFilteredStuff</a>,<a href="http://technorati.com/tags/DatabaseLayer" rel="tag">DatabaseLayer</a>,<a href="http://technorati.com/tags/GetStuffFromDB" rel="tag">GetStuffFromDB</a>,<a href="http://technorati.com/tags/Where" rel="tag">Where</a>,<a href="http://technorati.com/tags/Name" rel="tag">Name</a>,<a href="http://technorati.com/tags/code" rel="tag">code</a>,<a href="http://technorati.com/tags/dependency" rel="tag">dependency</a>,<a href="http://technorati.com/tags/database" rel="tag">database</a>,<a href="http://technorati.com/tags/layer" rel="tag">layer</a>,<a href="http://technorati.com/tags/creation" rel="tag">creation</a>,<a href="http://technorati.com/tags/interface" rel="tag">interface</a>,<a href="http://technorati.com/tags/IDatabaseRepository" rel="tag">IDatabaseRepository</a>,<a href="http://technorati.com/tags/repository" rel="tag">repository</a>,<a href="http://technorati.com/tags/StartsWith" rel="tag">StartsWith</a>,<a href="http://technorati.com/tags/method" rel="tag">method</a>,<a href="http://technorati.com/tags/implements" rel="tag">implements</a>,<a href="http://technorati.com/tags/TestMethod" rel="tag">TestMethod</a>,<a href="http://technorati.com/tags/GetFilteredStuff_GivenValidFilter_ReturnsCorrectFilteredResult" rel="tag">GetFilteredStuff_GivenValidFilter_ReturnsCorrectFilteredResult</a>,<a href="http://technorati.com/tags/IDatabaseRepsitory" rel="tag">IDatabaseRepsitory</a>,<a href="http://technorati.com/tags/StubDatabaseRepository" rel="tag">StubDatabaseRepository</a>,<a href="http://technorati.com/tags/result" rel="tag">result</a>,<a href="http://technorati.com/tags/Assert" rel="tag">Assert</a>,<a href="http://technorati.com/tags/logic" rel="tag">logic</a>,<a href="http://technorati.com/tags/architecture" rel="tag">architecture</a>,<a href="http://technorati.com/tags/classes" rel="tag">classes</a>,<a href="http://technorati.com/tags/integration" rel="tag">integration</a>,<a href="http://technorati.com/tags/Tools" rel="tag">Tools</a>,<a href="http://technorati.com/tags/Inversion" rel="tag">Inversion</a>,<a href="http://technorati.com/tags/Control" rel="tag">Control</a>,<a href="http://technorati.com/tags/Containers" rel="tag">Containers</a>,<a href="http://technorati.com/tags/Wikipage" rel="tag">Wikipage</a>,<a href="http://technorati.com/tags/Rhino" rel="tag">Rhino</a>,<a href="http://technorati.com/tags/Resharper" rel="tag">Resharper</a>,<a href="http://technorati.com/tags/category" rel="tag">category</a>,<a href="http://technorati.com/tags/factory" rel="tag">factory</a>,<a href="http://technorati.com/tags/scenario" rel="tag">scenario</a>,<a href="http://technorati.com/tags/StructureMap" rel="tag">StructureMap</a>,<a href="http://technorati.com/tags/Libraries" rel="tag">Libraries</a>,<a href="http://technorati.com/tags/Note" rel="tag">Note</a>,<a href="http://technorati.com/tags/terminology" rel="tag">terminology</a>,<a href="http://technorati.com/tags/times" rel="tag">times</a>,<a href="http://technorati.com/tags/Sometimes" rel="tag">Sometimes</a>,<a href="http://technorati.com/tags/library" rel="tag">library</a>,<a href="http://technorati.com/tags/Most" rel="tag">Most</a>,<a href="http://technorati.com/tags/reflection" rel="tag">reflection</a>,<a href="http://technorati.com/tags/Some" rel="tag">Some</a>,<a href="http://technorati.com/tags/Typemock" rel="tag">Typemock</a>,<a href="http://technorati.com/tags/Isolator" rel="tag">Isolator</a>,<a href="http://technorati.com/tags/APIs" rel="tag">APIs</a>,<a href="http://technorati.com/tags/magazine" rel="tag">magazine</a>,<a href="http://technorati.com/tags/Here" rel="tag">Here</a>,<a href="http://technorati.com/tags/example" rel="tag">example</a>,<a href="http://technorati.com/tags/MockRepository" rel="tag">MockRepository</a>,<a href="http://technorati.com/tags/Stub" rel="tag">Stub</a>,<a href="http://technorati.com/tags/Record" rel="tag">Record</a>,<a href="http://technorati.com/tags/Return" rel="tag">Return</a>,<a href="http://technorati.com/tags/List" rel="tag">List</a>,<a href="http://technorati.com/tags/Playback" rel="tag">Playback</a>,<a href="http://technorati.com/tags/MyConcreteClass" rel="tag">MyConcreteClass</a>,<a href="http://technorati.com/tags/AreEqual" rel="tag">AreEqual</a>,<a href="http://technorati.com/tags/Notice" rel="tag">Notice</a>,<a href="http://technorati.com/tags/tool" rel="tag">tool</a>,<a href="http://technorati.com/tags/studio" rel="tag">studio</a>,<a href="http://technorati.com/tags/features" rel="tag">features</a>,<a href="http://technorati.com/tags/Analysis" rel="tag">Analysis</a>,<a href="http://technorati.com/tags/generation" rel="tag">generation</a>,<a href="http://technorati.com/tags/usage" rel="tag">usage</a>,<a href="http://technorati.com/tags/demo" rel="tag">demo</a>,<a href="http://technorati.com/tags/Calculator" rel="tag">Calculator</a>,<a href="http://technorati.com/tags/Kata" rel="tag">Kata</a>,<a href="http://technorati.com/tags/technologies" rel="tag">technologies</a>,<a href="http://technorati.com/tags/concepts" rel="tag">concepts</a>,<a href="http://technorati.com/tags/examples" rel="tag">examples</a>,<a href="http://technorati.com/tags/articles" rel="tag">articles</a>,<a href="http://technorati.com/tags/dependencies" rel="tag">dependencies</a>,<a href="http://technorati.com/tags/methods" rel="tag">methods</a>,<a href="http://technorati.com/tags/categories" rel="tag">categories</a>,<a href="http://technorati.com/tags/interfaces" rel="tag">interfaces</a>,<a href="http://technorati.com/tags/parameters" rel="tag">parameters</a>,<a href="http://technorati.com/tags/Moles" rel="tag">Moles</a>,<a href="http://technorati.com/tags/skills" rel="tag">skills</a>,<a href="http://technorati.com/tags/architectures" rel="tag">architectures</a>,<a href="http://technorati.com/tags/aspx" rel="tag">aspx</a>,<a href="http://technorati.com/tags/implementations" rel="tag">implementations</a>,<a href="http://technorati.com/tags/repo" rel="tag">repo</a><br />
<p></p>Windows Live Tags: <a href="http://windows.live.com/connect/tag/Introduction" rel="clubhouseTag">Introduction</a>,<a href="http://windows.live.com/connect/tag/Test-driven" target="_blank">test-driven</a>,<a href="http://windows.live.com/connect/tag/development" rel="clubhouseTag">development</a>,<a href="http://windows.live.com/connect/tag/overview" rel="clubhouseTag">overview</a>,<a href="http://windows.live.com/connect/tag/Basic" rel="clubhouseTag">Basic</a>,<a href="http://windows.live.com/connect/tag/Principles" rel="clubhouseTag">Principles</a>,<a href="http://windows.live.com/connect/tag/supports" rel="clubhouseTag">supports</a>,<a href="http://windows.live.com/connect/tag/requirements" rel="clubhouseTag">requirements</a>,<a href="http://windows.live.com/connect/tag/unit" rel="clubhouseTag">unit</a>,<a href="http://windows.live.com/connect/tag/separation" rel="clubhouseTag">separation</a>,<a href="http://windows.live.com/connect/tag/injection" rel="clubhouseTag">injection</a>,<a href="http://windows.live.com/connect/tag/composition" rel="clubhouseTag">composition</a>,<a href="http://windows.live.com/connect/tag/components" rel="clubhouseTag">components</a>,<a href="http://windows.live.com/connect/tag/IEnumerable" rel="clubhouseTag">IEnumerable</a>,<a href="http://windows.live.com/connect/tag/GetFilteredStuff" rel="clubhouseTag">GetFilteredStuff</a>,<a href="http://windows.live.com/connect/tag/DatabaseLayer" rel="clubhouseTag">DatabaseLayer</a>,<a href="http://windows.live.com/connect/tag/GetStuffFromDB" rel="clubhouseTag">GetStuffFromDB</a>,<a href="http://windows.live.com/connect/tag/Where" rel="clubhouseTag">Where</a>,<a href="http://windows.live.com/connect/tag/Name" rel="clubhouseTag">Name</a>,<a href="http://windows.live.com/connect/tag/code" rel="clubhouseTag">code</a>,<a href="http://windows.live.com/connect/tag/dependency" rel="clubhouseTag">dependency</a>,<a href="http://windows.live.com/connect/tag/database" rel="clubhouseTag">database</a>,<a href="http://windows.live.com/connect/tag/layer" rel="clubhouseTag">layer</a>,<a href="http://windows.live.com/connect/tag/creation" rel="clubhouseTag">creation</a>,<a href="http://windows.live.com/connect/tag/interface" rel="clubhouseTag">interface</a>,<a href="http://windows.live.com/connect/tag/IDatabaseRepository" rel="clubhouseTag">IDatabaseRepository</a>,<a href="http://windows.live.com/connect/tag/repository" rel="clubhouseTag">repository</a>,<a href="http://windows.live.com/connect/tag/StartsWith" rel="clubhouseTag">StartsWith</a>,<a href="http://windows.live.com/connect/tag/method" rel="clubhouseTag">method</a>,<a href="http://windows.live.com/connect/tag/implements" rel="clubhouseTag">implements</a>,<a href="http://windows.live.com/connect/tag/TestMethod" rel="clubhouseTag">TestMethod</a>,<a href="http://windows.live.com/connect/tag/GetFilteredStuff_GivenValidFilter_ReturnsCorrectFilteredResult" rel="clubhouseTag">GetFilteredStuff_GivenValidFilter_ReturnsCorrectFilteredResult</a>,<a href="http://windows.live.com/connect/tag/IDatabaseRepsitory" rel="clubhouseTag">IDatabaseRepsitory</a>,<a href="http://windows.live.com/connect/tag/StubDatabaseRepository" rel="clubhouseTag">StubDatabaseRepository</a>,<a href="http://windows.live.com/connect/tag/result" rel="clubhouseTag">result</a>,<a href="http://windows.live.com/connect/tag/Assert" rel="clubhouseTag">Assert</a>,<a href="http://windows.live.com/connect/tag/logic" rel="clubhouseTag">logic</a>,<a href="http://windows.live.com/connect/tag/architecture" rel="clubhouseTag">architecture</a>,<a href="http://windows.live.com/connect/tag/classes" rel="clubhouseTag">classes</a>,<a href="http://windows.live.com/connect/tag/integration" rel="clubhouseTag">integration</a>,<a href="http://windows.live.com/connect/tag/Tools" rel="clubhouseTag">Tools</a>,<a href="http://windows.live.com/connect/tag/Inversion" rel="clubhouseTag">Inversion</a>,<a href="http://windows.live.com/connect/tag/Control" rel="clubhouseTag">Control</a>,<a href="http://windows.live.com/connect/tag/Containers" rel="clubhouseTag">Containers</a>,<a href="http://windows.live.com/connect/tag/Wikipage" rel="clubhouseTag">Wikipage</a>,<a href="http://windows.live.com/connect/tag/Rhino" rel="clubhouseTag">Rhino</a>,<a href="http://windows.live.com/connect/tag/Resharper" rel="clubhouseTag">Resharper</a>,<a href="http://windows.live.com/connect/tag/category" rel="clubhouseTag">category</a>,<a href="http://windows.live.com/connect/tag/factory" rel="clubhouseTag">factory</a>,<a href="http://windows.live.com/connect/tag/scenario" rel="clubhouseTag">scenario</a>,<a href="http://windows.live.com/connect/tag/StructureMap" rel="clubhouseTag">StructureMap</a>,<a href="http://windows.live.com/connect/tag/Libraries" rel="clubhouseTag">Libraries</a>,<a href="http://windows.live.com/connect/tag/Note" rel="clubhouseTag">Note</a>,<a href="http://windows.live.com/connect/tag/terminology" rel="clubhouseTag">terminology</a>,<a href="http://windows.live.com/connect/tag/times" rel="clubhouseTag">times</a>,<a href="http://windows.live.com/connect/tag/Sometimes" rel="clubhouseTag">Sometimes</a>,<a href="http://windows.live.com/connect/tag/library" rel="clubhouseTag">library</a>,<a href="http://windows.live.com/connect/tag/Most" rel="clubhouseTag">Most</a>,<a href="http://windows.live.com/connect/tag/reflection" rel="clubhouseTag">reflection</a>,<a href="http://windows.live.com/connect/tag/Some" rel="clubhouseTag">Some</a>,<a href="http://windows.live.com/connect/tag/Typemock" rel="clubhouseTag">Typemock</a>,<a href="http://windows.live.com/connect/tag/Isolator" rel="clubhouseTag">Isolator</a>,<a href="http://windows.live.com/connect/tag/APIs" rel="clubhouseTag">APIs</a>,<a href="http://windows.live.com/connect/tag/magazine" rel="clubhouseTag">magazine</a>,<a href="http://windows.live.com/connect/tag/Here" rel="clubhouseTag">Here</a>,<a href="http://windows.live.com/connect/tag/example" rel="clubhouseTag">example</a>,<a href="http://windows.live.com/connect/tag/MockRepository" rel="clubhouseTag">MockRepository</a>,<a href="http://windows.live.com/connect/tag/Stub" rel="clubhouseTag">Stub</a>,<a href="http://windows.live.com/connect/tag/Record" rel="clubhouseTag">Record</a>,<a href="http://windows.live.com/connect/tag/Return" rel="clubhouseTag">Return</a>,<a href="http://windows.live.com/connect/tag/List" rel="clubhouseTag">List</a>,<a href="http://windows.live.com/connect/tag/Playback" rel="clubhouseTag">Playback</a>,<a href="http://windows.live.com/connect/tag/MyConcreteClass" rel="clubhouseTag">MyConcreteClass</a>,<a href="http://windows.live.com/connect/tag/AreEqual" rel="clubhouseTag">AreEqual</a>,<a href="http://windows.live.com/connect/tag/Notice" rel="clubhouseTag">Notice</a>,<a href="http://windows.live.com/connect/tag/tool" rel="clubhouseTag">tool</a>,<a href="http://windows.live.com/connect/tag/studio" rel="clubhouseTag">studio</a>,<a href="http://windows.live.com/connect/tag/features" rel="clubhouseTag">features</a>,<a href="http://windows.live.com/connect/tag/Analysis" rel="clubhouseTag">Analysis</a>,<a href="http://windows.live.com/connect/tag/generation" rel="clubhouseTag">generation</a>,<a href="http://windows.live.com/connect/tag/usage" rel="clubhouseTag">usage</a>,<a href="http://windows.live.com/connect/tag/demo" rel="clubhouseTag">demo</a>,<a href="http://windows.live.com/connect/tag/Calculator" rel="clubhouseTag">Calculator</a>,<a href="http://windows.live.com/connect/tag/Kata" rel="clubhouseTag">Kata</a>,<a href="http://windows.live.com/connect/tag/technologies" rel="clubhouseTag">technologies</a>,<a href="http://windows.live.com/connect/tag/concepts" rel="clubhouseTag">concepts</a>,<a href="http://windows.live.com/connect/tag/examples" rel="clubhouseTag">examples</a>,<a href="http://windows.live.com/connect/tag/articles" rel="clubhouseTag">articles</a>,<a href="http://windows.live.com/connect/tag/dependencies" rel="clubhouseTag">dependencies</a>,<a href="http://windows.live.com/connect/tag/methods" rel="clubhouseTag">methods</a>,<a href="http://windows.live.com/connect/tag/categories" rel="clubhouseTag">categories</a>,<a href="http://windows.live.com/connect/tag/interfaces" rel="clubhouseTag">interfaces</a>,<a href="http://windows.live.com/connect/tag/parameters" rel="clubhouseTag">parameters</a>,<a href="http://windows.live.com/connect/tag/Moles" rel="clubhouseTag">Moles</a>,<a href="http://windows.live.com/connect/tag/skills" rel="clubhouseTag">skills</a>,<a href="http://windows.live.com/connect/tag/architectures" rel="clubhouseTag">architectures</a>,<a href="http://windows.live.com/connect/tag/aspx" rel="clubhouseTag">aspx</a>,<a href="http://windows.live.com/connect/tag/implementations" rel="clubhouseTag">implementations</a>,<a href="http://windows.live.com/connect/tag/repo" rel="clubhouseTag">repo</a>  </p></p></div>