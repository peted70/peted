---
layout: post
title: Azure Speech Studio for Mixed Reality
date: 2021-09-27 12:13
author: peted70
comments: true
categories: [azure, Azure, HoloLens, HoloLens, mixedreality, speech, unreal engine, unreal engine]
---
<!-- wp:image {"id":8653,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/mr-voiceover.png" alt="" class="wp-image-8653"/></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2>Introduction</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>It is not uncommon that a Mixed Reality experience requires more user guidance than traditional applications; partially because the user may not be familiar with the capabilities of the application but also because they may not be accustomed to using Mixed Reality applications in general. Sometimes you need to encourage a user to stand in the correct place or interact with a hologram in a particular way. How you can achieve this in a Mixed Reality app may differ according to time/budget and resources available. I think this breaks down into a handful of different categories that can be used separately or in a more symbiotic way:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8661,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/clippy.png" alt="" class="wp-image-8661"/></figure>
<!-- /wp:image -->

<!-- wp:list -->
<ul><li>Use of an assistant. This can range from a full AI-driven 3D avatar to the baseline speech and text which is the subject of this article.</li><li>Novel user experience techniques. I'm always reminded of the use of the hummingbird in the HoloLens out-of-box setup which is used to encourage the user to hold out their hands. They are rewarded when the hummingbird lands on their outstretched hand.</li><li>Highlighting objects to interact with or using spatialised sound effects to direct a user's attention to a particular spot. Or using other attention direction techniques such as light rays and thought bubbles.</li></ul>
<!-- /wp:list -->

<!-- wp:image {"id":8662,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/hummingbird.png" alt="" class="wp-image-8662"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>We are going to focus on the use of speech and text which is really the lowest common denominator but which provides a huge usability boost over leaving the users to figure it out for themselves. So what we are talking about is a text element floating in 3D space that coincides with a voiceover or put another way, audio instructions with subtitles. The text might float a small distance from the user and follow along with them as they move. I suspect it varies from user to user but, for me hearing instructions seems to work better and feels less demanding than reading them. Either way, a lot of HoloLens apps I have used use this technique to some degree.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Now, some voices are more pleasant to listen to than others and mine is in the latter category so the first barrier would be finding/paying a voice actor to record the tracks potentially in multiple languages using some half-decent recording equipment. Once recorded, of course any changes would mean another recording session. This might not be worthwhile for a lower budget app.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>If only we had some way to easily generate a human voice from some text…</p>
<!-- /wp:paragraph -->

<!-- wp:embed {"url":"https://youtu.be/5hyI_dM5cGo","type":"video","providerNameSlug":"youtube","responsive":true,"className":"wp-embed-aspect-4-3 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-4-3 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://youtu.be/5hyI_dM5cGo
</div></figure>
<!-- /wp:embed -->

<!-- wp:paragraph -->
<p>Luckily the technology has evolved a bit since 1939 and fairly recent advances with deep learning have pushed the capabilities on somewhat since the early days and I think is has become viable to use a text to speech service to generate a decent sounding voice over.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Let's instead focus on a particular voice tool, look at its capabilities and see how we can leverage it for a simple use case.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I looked around for something quick and easy to use maybe a year or so ago and ending up recording my own voice so I wasn't that hopeful but then I recently checked out Azure Speech Services.</p>
<!-- /wp:paragraph -->

<!-- wp:heading -->
<h2>Azure Speech Services</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>I'm not going to delve into the finer details of this service so will leave some links so you can get a better understanding of the capabilities beyond those that we use here:</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://techcommunity.microsoft.com/t5/azure-ai/azure-text-to-speech-updates-at-build-2021/ba-p/2382981" data-type="URL" data-id="https://techcommunity.microsoft.com/t5/azure-ai/azure-text-to-speech-updates-at-build-2021/ba-p/2382981" target="_blank" rel="noreferrer noopener">Azure Text-to-Speech updates at //Build 2021</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://azure.microsoft.com/en-us/services/cognitive-services/text-to-speech/#features" data-type="URL" data-id="https://azure.microsoft.com/en-us/services/cognitive-services/text-to-speech/#features" target="_blank" rel="noreferrer noopener">Try it out!</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://speech.microsoft.com/audiocontentcreation" data-type="URL" data-id="https://speech.microsoft.com/audiocontentcreation" target="_blank" rel="noreferrer noopener">Audio Content Creation Lite</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><a href="https://www.youtube.com/watch?v=HG7HxkTYGzw" data-type="URL" data-id="https://www.youtube.com/watch?v=HG7HxkTYGzw" target="_blank" rel="noreferrer noopener">What's new with Speech: Custom Neural Voice now in GA</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So, let's see how we can use this and put it into an application. I'm going to use <a href="https://www.unrealengine.com/" data-type="URL" data-id="https://www.unrealengine.com/" target="_blank" rel="noreferrer noopener">Unreal Engine</a> to demonstrate this as in my most-recent <a href="https://www.vam.ac.uk/event/ozWDmGEQ/ldf-sept-2021-sonzai-mr-installation" data-type="URL" data-id="https://www.vam.ac.uk/event/ozWDmGEQ/ldf-sept-2021-sonzai-mr-installation" target="_blank" rel="noreferrer noopener">side project </a>I created a reusable element on top of <a href="https://github.com/microsoft/MixedReality-UXTools-Unreal" data-type="URL" data-id="https://github.com/microsoft/MixedReality-UXTools-Unreal" target="_blank" rel="noreferrer noopener">Ux Tools</a> to achieve this.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Note that this would be very simple to replicate using Unity.</strong></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The first step would be to create the audio and text for our script. This is what we will use as an example:</p>
<!-- /wp:paragraph -->

<!-- wp:quote -->
<blockquote class="wp-block-quote"><p>"Hi, I'm Pete, welcome to my Mixed Reality application"</p><p>"Please say the word 'antidisestablishmentarianism' to begin"</p></blockquote>
<!-- /wp:quote -->

<!-- wp:paragraph -->
<p>You need to have an Azure account and add a Speech service resource before you can use Speech Studio. There are some instructions <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-studio-overview" data-type="URL" data-id="https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/speech-studio-overview" target="_blank" rel="noreferrer noopener">here </a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Once you have speech studio set up you can start an audio project:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8663,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/text-to-speech.png" alt="" class="wp-image-8663"/><figcaption>Text to Speech Landing Page</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>When inside your project you will see this screen:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8664,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/speech-studio.png" alt="" class="wp-image-8664"/><figcaption>Speech Studio Manage Files Page</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>There are a few different ways into this but I'm just going to start by creating a new file:<br>I'll start by pasting in my text…</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8665,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ss-voices.png" alt="" class="wp-image-8665"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>You can choose a voice. I wanted a UK accent so I chose Ryan (United Kingdom). You can check out the default results in the video below:</p>
<!-- /wp:paragraph -->

<!-- wp:embed {"url":"https://www.youtube.com/watch?v=vEovjyY2u5k","type":"video","providerNameSlug":"youtube","responsive":true,"className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://www.youtube.com/watch?v=vEovjyY2u5k
</div><figcaption>Default results after choosing a voice</figcaption></figure>
<!-- /wp:embed -->

<!-- wp:paragraph -->
<p>Now, you can also take full control over how this audio sounds, the intonation, rate, pitch, etc. of each spoken word and also control the length of pauses between words. For this example, I'm happy with the sound and pacing but for a recent project I ended up making some changes and ending up with audio I was very happy with.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Then export the audio/text to your library or to disk.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8666,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/export-to-library.png" alt="" class="wp-image-8666"/><figcaption>Export to Audio Library</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This creates a wav file and a text file for each paragraph that can be downloaded.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8667,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/export-to-disk.png" alt="" class="wp-image-8667"/><figcaption>Export to Disk</figcaption></figure>
<!-- /wp:image -->

<!-- wp:heading -->
<h2>A Reusable Voiceover Widget</h2>
<!-- /wp:heading -->

<!-- wp:paragraph -->
<p>(using Unreal Engine and UxTools)</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8668,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-template.png" alt="" class="wp-image-8668"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This will bootstrap a HoloLens project configured with plugins for <a rel="noreferrer noopener" href="https://github.com/microsoft/Microsoft-OpenXR-Unreal), [UxTools](https://github.com/microsoft/MixedReality-UXTools-Unreal" data-type="URL" data-id="https://github.com/microsoft/Microsoft-OpenXR-Unreal), [UxTools](https://github.com/microsoft/MixedReality-UXTools-Unreal" target="_blank">OpenXR</a>, <a href="https://github.com/microsoft/MixedReality-UXTools-Unreal" data-type="URL" data-id="https://github.com/microsoft/MixedReality-UXTools-Unreal">UxTools</a> and <a rel="noreferrer noopener" href="https://github.com/microsoft/MixedReality-GraphicsTools-Unreal" data-type="URL" data-id="https://github.com/microsoft/MixedReality-GraphicsTools-Unreal" target="_blank">Graphics Tools</a>. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I used my project template that I created in a previous post (see <a href="https://peted.azurewebsites.net/hololens-2-unreal-project-template/">Unreal Project Template</a>. Alternatively, you could follow the official tutorial <a href="https://docs.microsoft.com/en-us/windows/mixed-reality/develop/unreal/tutorials/unreal-uxt-ch1">here</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>These are the steps required:</p>
<!-- /wp:paragraph -->

<!-- wp:list -->
<ul><li>Create a new Blueprint based on Actor</li><li>Add a <strong>Uxt Follow</strong> component to the Blueprint</li><li>Also, add an <strong>Audio</strong> component and a <strong>Uxt Text Render</strong> component</li></ul>
<!-- /wp:list -->

<!-- wp:image {"id":8671,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-components.png" alt="" class="wp-image-8671"/><figcaption>Unreal Blueprint Components</figcaption></figure>
<!-- /wp:image -->

<!-- wp:list -->
<ul><li>Create a Struct to hold data in a DataTable (the struct will hold a Sound Wave, the text for the voice over and a string indicating the 'next' voice to select)</li></ul>
<!-- /wp:list -->

<!-- wp:image {"id":8673,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-create-struct.png" alt="" class="wp-image-8673"/><figcaption>Create a struct</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":8672,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-struct.png" alt="" class="wp-image-8672"/><figcaption>Unreal struct</figcaption></figure>
<!-- /wp:image -->

<!-- wp:list -->
<ul><li>Now create a DataTable and populate it with instances of the new struct (you can drag the wav files into your content folder and then configure a Sound Wave to refer to those assets) and also copy the text from Azure Speech Studio into each instance.</li></ul>
<!-- /wp:list -->

<!-- wp:image {"id":8674,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-create-dt.png" alt="" class="wp-image-8674"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>When you create the DataTable make sure to choose the structure as the data type for the DataTable.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8675,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-create-dt2.png" alt="" class="wp-image-8675"/><figcaption>Choose Struct</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>As long as you have dragged the audio files into your project Content folder and then you can set up each data row like this...</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8676,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-dt-entry.png" alt="" class="wp-image-8676"/><figcaption>DataTable entry</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Note that I use '&lt;br>' to insert line breaks into the Uxt TextRender object that I use to render the text. You should end up with a DataTable that looks like this.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8678,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-dt.png" alt="" class="wp-image-8678"/><figcaption>DataTable</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Go back to the blueprint we created earlier and set a reference to the DataTable as a variable in the blueprint.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8679,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-dt-variable.png" alt="" class="wp-image-8679"/><figcaption>DataTable variable</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Set the variable as Instance Editable and Blueprint Read-only. We can then set it externally in an instance of our BP that we will create in the level. Then, drag an instance of the blueprint into the level</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8680,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-create-in-leve.png" alt="" class="wp-image-8680"/><figcaption>Blueprint in Level</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>And set the DataTable instance we just created into the reference variable we added to the blueprint. Now we can access the data table from within the event graph of the blueprint. Set Auto Receive Input to player 0 so we will have inputs routed to the actor (we will use some voice commands later…)</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8681,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-receive-input.png" alt="" class="wp-image-8681"/><figcaption>Blueprint Input</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So, let's write the blueprint code to make it all work…<br>First we'll create a custom event called Initialise:</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8682,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-bp-init.png" alt="" class="wp-image-8682"/><figcaption> BP Initialise</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>A couple of things to do here; cache the row names for the DataTable into a variable so we can iterate these and then hook up an event handler for when the audio player state changes. Notice that we are also passing the 'NextVoice' into the Custom event and initialise this variable to the first voice.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8683,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-bp-init2.png" alt="" class="wp-image-8683"/><figcaption>Bind Audio State Changed</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>We'll leave AudioPlayerStateChanged blank for a while whilst we create a custom event for switching voices.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8684,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-bp-select-voice.png" alt="" class="wp-image-8684"/><figcaption>Select a voice</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This one is slightly more involved but consists of a custom event that takes a voice name as a parameter and will configure the audio player to play the associated speech sound, set the text in the text render component and start the audio playing. We also set the NextVoice variable so that the next time the Audio Player State changes we will know which voice to set.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><br>Then finally we can fill in the Audio Player State Changed</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8685,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-bp-player-state-changed.png" alt="" class="wp-image-8685"/><figcaption>Audio Player State Changed</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Now staying in the Blueprint but navigating to the Viewport and selecting the UxtTextRender component we can set some settings to configure the text to appear at the right size, etc.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8686,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-bp-uxttext.png" alt="" class="wp-image-8686"/><figcaption>Uxt Text Render</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I set some dummy text there so I could see something being rendered, then set the World Size up to 4.0, Centred the Horizontal Alignment. Note. The use of line breaks in the input text.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Now, we just need to kick things off. I'll use begin play for that…<br>So, initialise and then select the first voice.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8687,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2021/09/ue-bp-start.png" alt="" class="wp-image-8687"/><figcaption>Begin Play</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I won't put in the voice command as this post is now longer than intended but that is just a case of declaring one in the input settings for your application…</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Now, package up the project for HoloLens,</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":8688,"sizeSlug":"full","linkDestination":"none"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2021/09/package.png" alt="" class="wp-image-8688"/><figcaption>packaging for HoloLens</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Once packaged we can sideload the app using the HoloLens <a href="https://docs.microsoft.com/en-us/windows/mixed-reality/develop/platform-capabilities-and-apis/using-the-windows-device-portal" data-type="URL" data-id="https://docs.microsoft.com/en-us/windows/mixed-reality/develop/platform-capabilities-and-apis/using-the-windows-device-portal" target="_blank" rel="noreferrer noopener">developer </a>portal</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>And the results of this look like this....</p>
<!-- /wp:paragraph -->

<!-- wp:embed {"url":"https://youtu.be/lY8tctIqygU","type":"video","providerNameSlug":"youtube","responsive":true,"className":"wp-embed-aspect-16-9 wp-has-aspect-ratio"} -->
<figure class="wp-block-embed is-type-video is-provider-youtube wp-block-embed-youtube wp-embed-aspect-16-9 wp-has-aspect-ratio"><div class="wp-block-embed__wrapper">
https://youtu.be/lY8tctIqygU
</div></figure>
<!-- /wp:embed -->

<!-- wp:paragraph -->
<p>And there is a project over on Github with the source <a href="https://github.com/peted70/mr-voiceover" data-type="URL" data-id="https://github.com/peted70/mr-voiceover" target="_blank" rel="noreferrer noopener">here</a>.</p>
<!-- /wp:paragraph -->
