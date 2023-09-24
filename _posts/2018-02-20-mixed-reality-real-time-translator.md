---
layout: post
title: mixed reality real-time translator
date: 2018-02-20 16:29
author: peted70
comments: true
categories: [AI, AI, cognitive, Cognitive, HoloLens, mixedreality, MR, translator, Unity, VR]
---
<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/headline.png"><img style="display: inline; background-image: none;" title="headline" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/headline_thumb.png" alt="headline" width="670" height="259" border="0" /></a>
<blockquote>Did you know that there is an API that you can call which will accept as input an audio stream, say from a microphone, that will translate any spoken sentences into another language and return not only a final audio output file of the result but also partial textual information as the context of the meaning of the sentence being uttered changes? Just let that sink in for a second; whilst we speak our words can be understood by others who wouldn't usually understand us and all happening in real-time as we utter those sentences.</blockquote>
The API in question is in the suite of Microsoft cognitive services hidden away amongst face detection and recognition and natural language understanding services. All of which add up to a thought-provoking mix of Artificial Intelligence functionality that can be used to power Mixed Reality scenarios. For true Mixed Reality not only is digital content blended seamlessly with the real world in a visual sense but the flow of information must travel back in the other direction with an evolving understanding of the physical environment.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/cogservices.png"><img style="display: inline; background-image: none;" title="cogservices" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/cogservices_thumb.png" alt="cogservices" width="670" height="195" border="0" /></a>

The <a href="https://azure.microsoft.com/en-gb/services/cognitive-services/" target="_blank" rel="noopener">cognitive services</a> are mainly REST APIs and thus can be used from pretty much any computing device with a http stack, although the real-time speech also uses WebSockets to stream voice data over the internet. The API we are going to use is the Translator Speech API which supports translating from and to 11 different languages:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/languagesupport.png"><img style="display: inline; background-image: none;" title="languagesupport" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/languagesupport_thumb.png" alt="languagesupport" width="658" height="208" border="0" /></a>

Since most of the Mixed Reality development I see is powered by Unity, particularly the HoloLens development, I am going to focus on that for this post. So the first hurdle is how do we make REST API calls from Unity?
<h3>REST in Unity</h3>
There are two main Unity classes for making http calls; <a href="https://docs.unity3d.com/ScriptReference/WWW.html" target="_blank" rel="noopener">WWW</a> and <a href="https://docs.unity3d.com/ScriptReference/Networking.UnityWebRequest.html" target="_blank" rel="noopener">UnityWebRequest</a> . WWW is slightly higher level than UnityWebRequest and actually uses it in it's implementation so you can think of it as a wrapper. As the scripting environment in Unity is .NET we could also use HttpWebRequest, WebClient or HttpClient. With the Experimental .NET 4.6 support in Unity
<blockquote>Under Edit &gt; Project Settings &gt; Player &gt; Other Settings</blockquote>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/projplayersettings.png"><img style="display: inline;" title="projplayersettings" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/projplayersettings_thumb.png" alt="projplayersettings" width="590" height="768" /></a>

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/expnet.png"><img style="display: inline; background-image: none;" title="expnet" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/expnet_thumb.png" alt="expnet" width="670" height="234" border="0" /></a>

(see <a href="http://peted.azurewebsites.net/holograms-catalogue/" target="_blank" rel="noopener">http://peted.azurewebsites.net/holograms-catalogue/</a> for more details) we can use the newer HttpClient with it's Task-based, asynchronous design meaning we can use async/await features which the experimental support unlocks for us. Since I write these posts for instructional purposes the interface is much cleaner and easier to understand but you could replace these calls using any of the other http clients previously mentioned. Here's an example of making an Http GET request with HttpClient:
<blockquote>If you find that HttpClient is somehow not available and you get the error</blockquote>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/httpmissing.png"><img style="display: inline; background-image: none;" title="httpmissing" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/httpmissing_thumb.png" alt="httpmissing" width="670" height="62" border="0" /></a>
<blockquote>in the Unity console</blockquote>
Then if you add a file called mcs.rsp with the contents: -r:System.Net.Http.dll in your Assets folder...

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/mcscsp.png"><img style="display: inline; background-image: none;" title="mcscsp" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/mcscsp_thumb.png" alt="mcscsp" width="670" height="673" border="0" /></a>

and then reload your project.
<h3>Accessing Microphone Data in Unity</h3>
If we are going to stream audio data to an API then we need a mechanism to retrieve the raw audio bits from the correct microphone device. Let's look at the Unity API way to do this:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/unitymic.png"><img style="display: inline; background-image: none;" title="unitymic" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/unitymic_thumb.png" alt="unitymic" width="670" height="240" border="0" /></a>

This shows the Microphone class which is a collection of static functions that we can use. So to select a particular device we can inspect the description strings in Microphone.devices and then the other static functions refer to that device by its string name or null for the default device. I'm just going to use the first mic in the list using
<blockquote>_mic = Microphone.devices[0];</blockquote>
but we could, of course expose a choice to the user if there are multiple device.

To start recording from the device we need to associate an AudioClip object with the microphone with the required parameters, including sample rate, buffer length, etc. Once recording we can use AudioClip.GetData inside our update loop to retrieve buffered audio data generated from the microphone. When sending this data to an API we have two main considerations;
<ul>
 	<li>What is the format required by the API and will we need to convert (the data returned by Unity is a float array ranging from -1 to 1).</li>
 	<li>How often do we want to send the data and how will we compose it into chunks for sending.</li>
</ul>
The real-time Translator API requires mono, signed 16bit PCM sampled at 16khz. Well, the data provided by Unity is already mono and we can request the sample rate when we initially called Microphone.Start so that just leaves us with a conversion from

float [-1.0, 1.0] -&gt; short [-32768, 32767]

Here's the conversion function which populates a provided Stream:

Once the provided stream contains enough data to be considered a 'chunk' we can send it to the API.
<blockquote>Note. In the sample code I have abstracted where the audio data gets sent so I can provide a list of audio consumers. I used this whilst developing the sample to test by checking that I could reconstruct a valid WAV file from the supplied data. That code is still in the sample for testing purposes.</blockquote>
<h3>Translator API</h3>
You can get an overview of the Translator APIs here <a href="https://docs.microsofttranslator.com/" target="_blank" rel="noopener">https://docs.microsofttranslator.com/</a>. This is a summary of what's on offer:

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/translatorapioverview.png"><img style="display: inline; background-image: none;" title="translatorapioverview" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/translatorapioverview_thumb.png" alt="translatorapioverview" width="670" height="201" border="0" /></a>

We are going to focus on the speech part of things the details are here <a href="https://docs.microsofttranslator.com/speech-translate.html" target="_blank" rel="noopener">https://docs.microsofttranslator.com/speech-translate.html</a> in the official documentation. I'm just going to pick out the significant parts based on building the sample that goes with this post for more details and setting up the Azure backend to be able to use the service please refer to the documentation referenced above.
<ol>
 	<li>Azure Subscription</li>
 	<li>Activate a Translator Speech API on the Azure portal</li>
 	<li>Copy the Keys from that service</li>
 	<li>Set up a WebSocket communication with the API with details of 'from' and 'to' languages</li>
 	<li>Stream audio data over the WebSocket</li>
 	<li>Retrieve results from the WebSocket
<ol>
 	<li>Text Results including partial and final results</li>
 	<li>Final audio file with translation</li>
</ol>
</li>
</ol>
<h3>Authentication</h3>
Let's have a look at some of the main code used to drive the sample starting with Auth:

You can acquire an access token either using your API key directly in the request header or you can make a separate request to <a href="https://api.cognitive.microsoft.com/sts/v1.0/issueToken" target="_blank" rel="noopener">https://api.cognitive.microsoft.com/sts/v1.0/issueToken</a> to retrieve a time-limited access token. Either way in a production app you should avoid keeping your API key in your app and protect it by using it via a server.
<h3>WebSockets</h3>
Initially I tried to set the WebSocket connection up using the System.Net.WebSockets class but although I could create a connection to the Translator API I never retrieved any results back. As a result I implemented the connection using the UWP WebSockets implementation and as a result the sample won't work in the Unity editor. I hope to revisit this and fix but for now I just want the sample to run on a HoloLens so didn't worry about it too much.
<h3>Architecture of the Sample Code</h3>
In order to support some reusability of components like the microphone audio data access the code has been factored accordingly. There is a producer/consumer-like model whereby the MicrophoneAudioGetter component is a Monobehaviour into which you can plug consumers. In the sample, there is only one consumer and that is another component which proxies the audio data to the Translator API. Any number of consumers could be plugged in by creating a C# script and implementing the abstract class AudioConsumer and then adding the script component to a GameObject and referencing that in the consumers list of the Microphone Audio Getter.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/microphone-audio-getter.png"><img style="display: inline; background-image: none;" title="microphone-audio-getter" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/microphone-audio-getter_thumb.png" alt="microphone-audio-getter" width="670" height="179" border="0" /></a>
<blockquote>The Translator API consumer is the one used in the sample and that provides properties via the Editor to configure the API; Api Key, From Language, To Language and Voice:</blockquote>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/APIConsumerEditor.png"><img style="display: inline; background-image: none;" title="APIConsumerEditor" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/APIConsumerEditor_thumb.png" alt="APIConsumerEditor" width="670" height="276" border="0" /></a>

There are ten languages to choose from and various different male/female voices to choose from depending on the 'to' language selected.

Now, the Translator API sends out a couple of events; one when textual translation is received and one for audio. So again we have configured two lists of receivers, one for each event. So in order to receive wither event it is necessary to add a C# script which implements either AudioReceivedHandler or TextReceivedHandler and add a reference to the attached GameObject to the appropriate property in the Translator API behaviour.
<blockquote>I've added two simple handlers to the sample.</blockquote>
<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/TextAUdioReceivers.png"><img style="display: inline; background-image: none;" title="TextAUdioReceivers" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/TextAUdioReceivers_thumb.png" alt="TextAUdioReceivers" width="670" height="154" border="0" /></a>
<blockquote>The text one updates a TextMesh and the Audio handler writes the data to an AudioSource.</blockquote>
To activate the recording in the sample I added a 3D model of a parrot which I found on <a href="https://www.remix3d.com/" target="_blank" rel="noopener">Remix3D</a>, opened up in 3D Paint and exported as an FBX file. I opened it up in Blender and resized and reset the transformations, added it to my Unity project and used the ButtonMesh component from the Mixed Reality Toolkit to make it interactible.

<a href="http://peted.azurewebsites.net/wp-content/uploads/2018/02/parrotinblender.png"><img style="display: inline; background-image: none;" title="parrotinblender" src="http://peted.azurewebsites.net/wp-content/uploads/2018/02/parrotinblender_thumb.png" alt="parrotinblender" width="670" height="476" border="0" /></a>The sample project can be found here <a href="https://github.com/peted70/mr-realtime-translator" target="_blank" rel="noopener">https://github.com/peted70/mr-realtime-translator</a> and the ReadMe has instructions about usage of the components.

Here are some results using Mixed Reality Capture from a HoloLens device:

<iframe src="https://www.youtube.com/embed/F_8NxyhbT_Y" width="560" height="315" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

In summary, this is just how to wire up the APIs and hopefully something reusable. I can imagine a whole host of ways in which this could help improve Mixed Reality apps for example, by translating input to APIs that have been written expecting one specific language and also using contextual information such as gaze and proximity to translate audio voice in real-time in a collaborative Mixed Reality scenario.
