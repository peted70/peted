---
layout: single
title: Simplygon in Azure
excerpt: "Before I begin I want to credit team members from Microsoft CSE (commercial Software Engineering) as the effort was shared..."
date: 2020-09-03 12:02
author_profile: true
comments: true
categories: [3D, Azure, c#, simlygon, Uncategorized]
header:
    teaserlogo: 'assets/images/2020/09/main-simplygon.png'
    teaser: 'assets/images/2020/09/main-simplygon.png'
---
<!-- wp:image {"id":8491,"sizeSlug":"full"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2020/09/main-simplygon.png" alt="" class="wp-image-8491"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Before I begin I want to credit team members from Microsoft CSE (commercial Software Engineering) as the effort was shared:\n\n- Bart Jansen, Carlos Sardo, Martin Tirion, Claus Matzinger, Jan Tielens, Laurent Ellerbach\n\nI wrote a [previous blog post]({{site.baseurl}}/decimation/hololens/holotoolkit/mesh/mixedreality/mr/simplygon/unity/2017/06/18/hololensoptimising-with-simplygon.html) about Simplygon a few years ago and the idea of optimising 3D meshes to enable running on a low powered device such as a HoloLens or mobile phone has cropped up on a number of occasions since. The content in the more recent projects have the additional requirement of being loaded into an app dynamically when requested by the app user. Most projects designed to run across these kinds of mobile devices usually need to address the question of whether to render content on a server or locally on the device. Sometimes that choice will be obvious and other times it will be more complicated and represent a trade-off between many competing factors.\n"} -->
<div class="wp-block-jetpack-markdown"><p>Before I begin I want to credit team members from Microsoft CSE (commercial Software Engineering) as the effort was shared:</p>
<ul>
<li>Bart Jansen, Carlos Sardo, Martin Tirion, Claus Matzinger, Jan Tielens, Laurent Ellerbach</li>
</ul>
<p>I wrote a <a href="{{ site.baseurl }}/3d/azure/c%23/simlygon/uncategorized/2020/09/03/simplygon-in-azure.html">previous blog post</a> about Simplygon a few years ago and the idea of optimising 3D meshes to enable running on a low powered device such as a HoloLens or mobile phone has cropped up on a number of occasions since. The content in the more recent projects have the additional requirement of being loaded into an app dynamically when requested by the app user. Most projects designed to run across these kinds of mobile devices usually need to address the question of whether to render content on a server or locally on the device. Sometimes that choice will be obvious and other times it will be more complicated and represent a trade-off between many competing factors.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8492,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/simplygon.png" alt="" class="wp-image-8492"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"## Rendering on the Server\n\nOn one hand if you have large 3D models with dense geometry consisting of millions of triangles where you need to see all of the fine details and the content isn't well suited for optimisation then server-side rendering might be the right choice for you. You might need to account for the infrastructure and maintenance work required to set up a server-side rendering solution and you also will need to understand the costs associated with running cloud services for this kind of a solution. You may need to scale across millions of users or you may only need to spin up and down services at a much smaller scale. If the latter applies then [Azure Remote Rendering](https://azure.microsoft.com/en-gb/services/remote-rendering/) would be a suitable consideration. If the former, then maybe [3D Streaming Toolkit](https://github.com/3DStreamingToolkit/3DStreamingToolkit) or a custom client/server rendering solution might be more cost-effective."} -->
<div class="wp-block-jetpack-markdown"><h2>Rendering on the Server</h2>
<p>On one hand if you have large 3D models with dense geometry consisting of millions of triangles where you need to see all of the fine details and the content isn't well suited for optimisation then server-side rendering might be the right choice for you. You might need to account for the infrastructure and maintenance work required to set up a server-side rendering solution and you also will need to understand the costs associated with running cloud services for this kind of a solution. You may need to scale across millions of users or you may only need to spin up and down services at a much smaller scale. If the latter applies then <a href="https://azure.microsoft.com/en-gb/services/remote-rendering/">Azure Remote Rendering</a> would be a suitable consideration. If the former, then maybe <a href="https://github.com/3DStreamingToolkit/3DStreamingToolkit">3D Streaming Toolkit</a> or a custom client/server rendering solution might be more cost-effective.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8493,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/arr-engine.png" alt="" class="wp-image-8493"/><figcaption>Azure Remote Rendering</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"## Rendering on the Client\n\nOn the other hand your original content may consist of very dense geometry; maybe it was created using photogrammetry or using a scanner, but the 3D models and the use case may be suited to geometry and material optimisation. Using techniques such as re-meshing, normal map baking and texture atlases to reduce draw calls in addition to 3D geometry compression can facilitate efficient on-device rendering and also network transmission.\n"} -->
<div class="wp-block-jetpack-markdown"><h2>Rendering on the Client</h2>
<p>On the other hand your original content may consist of very dense geometry; maybe it was created using photogrammetry or using a scanner, but the 3D models and the use case may be suited to geometry and material optimisation. Using techniques such as re-meshing, normal map baking and texture atlases to reduce draw calls in addition to 3D geometry compression can facilitate efficient on-device rendering and also network transmission.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8494,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/main.png" alt="" class="wp-image-8494"/><figcaption>Geometry Optimisation</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"This post will focus on how to fit those techniques into a cloud content pipeline using Simplygon. Often the system would require an 'authoring' step which would allow the content owner to associate metadata and seed 3D model content into a processing pipeline which could run on-premises or in the cloud. So following on from the previous post where we showed how to create a content pipeline on Azure using all of the power of Blender through it's Python scripting interface we'll illustrate how to have an optimisation step within the pipeline. This might look something like this:"} -->
<div class="wp-block-jetpack-markdown"><p>This post will focus on how to fit those techniques into a cloud content pipeline using Simplygon. Often the system would require an 'authoring' step which would allow the content owner to associate metadata and seed 3D model content into a processing pipeline which could run on-premises or in the cloud. So following on from the previous post where we showed how to create a content pipeline on Azure using all of the power of Blender through it's Python scripting interface we'll illustrate how to have an optimisation step within the pipeline. This might look something like this:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8460,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/08/content-pipeline.png" alt="" class="wp-image-8460"/><figcaption>Content Pipeline</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"We didn't mention the optimise node in there before really but we can make use of the Simplygon SDK to write some code there to provide the model optimisations.\n\n\u003e I focused on the Simplygon desktop application [previously]({{site.baseurl}}/decimation/hololens/holotoolkit/mesh/mixedreality/mr/simplygon/unity/2017/06/18/hololensoptimising-with-simplygon.html) which is great for testing and proving out scenarios but we're going to need a more automated approach. In addition the Simplygon offering has since moved to version 9 and support for the desktop app is deprecated in favour of improvements to the SDK. The remeshing processor has with more intelligent hole filling \u0026 cavity removal and generally better quality results. The SDK is now also available in C# and Python as well as C++.\n\n## Code\n\nWe're going to be using C# and we'll run the C# code from within an Azure function similar to how we set up Blender. We won't be using Linux containers this time as the Simplygon SDK has a dependency on Windows.\n\n\u003e You can view all of the code for the Azure Function [here](https://github.com/peted70/az-func-simplygon) where you can also find a **'Deploy to Azure'** button to test this in your own subscription. Note that you would need a Simplygon license key which you would be prompted for after pressing the button.\n\nThe Simplygon SDK comes with a suite of examples (see [Simplygon Samples](https://documentation.simplygon.com/SimplygonSDK_9.0.6500.0/api/examples/gettingstarted.html#prerequisites)) for each different language and we're going to follow pretty closely the example of **Remeshing With Material Casting**. I'm just going to look at code used in our sample but for full documentation you can visit [here](https://documentation.simplygon.com/)."} -->
<div class="wp-block-jetpack-markdown"><p>We didn't mention the optimise node in there before really but we can make use of the Simplygon SDK to write some code there to provide the model optimisations.</p>
<blockquote>
<p>I focused on the Simplygon desktop application <a href="{{ site.baseurl }}/3d/azure/c%23/simlygon/uncategorized/2020/09/03/simplygon-in-azure.html">previously</a> which is great for testing and proving out scenarios but we're going to need a more automated approach. In addition the Simplygon offering has since moved to version 9 and support for the desktop app is deprecated in favour of improvements to the SDK. The remeshing processor has with more intelligent hole filling &amp; cavity removal and generally better quality results. The SDK is now also available in C# and Python as well as C++.</p>
</blockquote>
<h2>Code</h2>
<p>We're going to be using C# and we'll run the C# code from within an Azure function similar to how we set up Blender. We won't be using Linux containers this time as the Simplygon SDK has a dependency on Windows.</p>
<blockquote>
<p>You can view all of the code for the Azure Function <a href="https://github.com/peted70/az-func-simplygon">here</a> where you can also find a <strong>'Deploy to Azure'</strong> button to test this in your own subscription. Note that you would need a Simplygon license key which you would be prompted for after pressing the button.</p>
</blockquote>
<p>The Simplygon SDK comes with a suite of examples (see <a href="https://documentation.simplygon.com/SimplygonSDK_9.0.6500.0/api/examples/gettingstarted.html#prerequisites">Simplygon Samples</a>) for each different language and we're going to follow pretty closely the example of <strong>Remeshing With Material Casting</strong>. I'm just going to look at code used in our sample but for full documentation you can visit <a href="https://documentation.simplygon.com/">here</a>.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8495,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/remeshing-sample.png" alt="" class="wp-image-8495"/><figcaption>Remeshing Sample </figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"We can create an Azure Function in the same way as the previous Blender post and we are going to reuse the code to accept a URI to a zip archive which contains the 3D model. For details see the [post]({{ site.baseurl }}/3d/3dmodels/azure/azure%20function/blender/c%23/2020/08/26/blender-in-azure.html). But instead of calling down into the Blender executable which is installed in the Linux container we are going to call the Simplygon SDK directly.\n\nThe samples come with a SimplygonLoader class that verifies the local environment and checks the license. So, the first thing is to make a call to that:"} -->
<div class="wp-block-jetpack-markdown"><p>We can create an Azure Function in the same way as the previous Blender post and we are going to reuse the code to accept a URI to a zip archive which contains the 3D model. For details see the <a href="{{ site.baseurl }}/3d/3dmodels/azure/azure%20function/blender/c%23/2020/08/26/blender-in-azure.html">post</a>. But instead of calling down into the Blender executable which is installed in the Linux container we are going to call the Simplygon SDK directly.</p>
<p>The samples come with a SimplygonLoader class that verifies the local environment and checks the license. So, the first thing is to make a call to that:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>using (var sg = Simplygon.Loader.InitSimplygon(out var errorCode, out var errorMessage))
{
    if (errorCode != Simplygon.EErrorCodes.NoError)
        return new BadRequestObjectResult($"Failed! {errorCode} - {errorMessage}");

    // Find the first .glTF file in the input and process it.
    var fileToProcess = Directory.GetFiles(zipDir, "*.glTF", SearchOption.AllDirectories)
        .FirstOrDefault();

    remeshedFile = await RunRemeshingWithMaterialCastingAsync(sg, log, fileToProcess, ToOutputPath(outputDir, fileToProcess), data.OnScreenSize);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"The first part of our processing involves **remeshing** which will result in a retopologised geometry rather than one that consists of the original vertices but with some strategic ones removed. The Simplygon [docs](https://documentation.simplygon.com/SimplygonSDK_9.0.6500.0/api/concepts/processors/remeshing.html) provides more details.\n\nThe crux of the code requires that we set up a Simplygon scene and we can set the scene on a Simplygon RemeshingProcessor object which we can configure and then run.  "} -->
<div class="wp-block-jetpack-markdown"><p>The first part of our processing involves <strong>remeshing</strong> which will result in a retopologised geometry rather than one that consists of the original vertices but with some strategic ones removed. The Simplygon <a href="https://documentation.simplygon.com/SimplygonSDK_9.0.6500.0/api/concepts/processors/remeshing.html">docs</a> provides more details.</p>
<p>The crux of the code requires that we set up a Simplygon scene and we can set the scene on a Simplygon RemeshingProcessor object which we can configure and then run.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>Simplygon.spScene sgScene = sgSceneImporter.GetScene();

// Create the remeshing processor.
using (Simplygon.spRemeshingProcessor sgRemeshingProcessor = sg.CreateRemeshingProcessor())
{
    sgRemeshingProcessor.SetScene(sgScene);

    using (Simplygon.spRemeshingSettings sgRemeshingSettings = sgRemeshingProcessor.GetRemeshingSettings())
    {
        // Set on-screen size target for remeshing.
        sgRemeshingSettings.SetOnScreenSize(onScreenSize);

    }
    // Start the remeshing process.
    await Task.Run(sgRemeshingProcessor.RunProcessing);
}
</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"The second stage of processing involves **material casting** and for us this will result in the input textures being down-rez'd and mapped correctly back onto the retoplogised geometry. Each kind of material channel (e.g. ambient occlusion, normal map, etc.) comes with a different set of input parameters. I'll just include here one of the processing blocks for the metallness channel and you can refer to the [code repo](https://github.com/peted70/az-func-simplygon) for how the other channels are configured:"} -->
<div class="wp-block-jetpack-markdown"><p>The second stage of processing involves <strong>material casting</strong> and for us this will result in the input textures being down-rez'd and mapped correctly back onto the retoplogised geometry. Each kind of material channel (e.g. ambient occlusion, normal map, etc.) comes with a different set of input parameters. I'll just include here one of the processing blocks for the metallness channel and you can refer to the <a href="https://github.com/peted70/az-func-simplygon">code repo</a> for how the other channels are configured:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>string MetalnessTextureFilePath;
using (Simplygon.spColorCaster sgMetalnessCaster = sg.CreateColorCaster())
{
    sgMetalnessCaster.SetMappingImage(sgRemeshingProcessor.GetMappingImage());
    sgMetalnessCaster.SetSourceMaterials(sgScene.GetMaterialTable());
    sgMetalnessCaster.SetSourceTextures(sgScene.GetTextureTable());
    sgMetalnessCaster.SetOutputFilePath("MetalnessTexture");

    using (Simplygon.spColorCasterSettings sgMetalnessCasterSettings = sgMetalnessCaster.GetColorCasterSettings())
    {
        sgMetalnessCasterSettings.SetMaterialChannel("Metalness");
        sgMetalnessCasterSettings.SetOutputImageFileFormat(Simplygon.EImageOutputFormat.JPEG);
    }

    sgMetalnessCaster.RunProcessing();
    MetalnessTextureFilePath = sgMetalnessCaster.GetOutputFilePath();
}
</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"When the processing is complete we can zip up the resulting files and return them as a **FileStream**."} -->
<div class="wp-block-jetpack-markdown"><p>When the processing is complete we can zip up the resulting files and return them as a <strong>FileStream</strong>.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>using (var outputZip = new ZipArchive(stream, ZipArchiveMode.Create, true))
{
    outputZip.CreateEntryFromDirectory(outputDir.FullName);
}

stream.Position = 0;

return new FileStreamResult(stream, System.Net.Mime.MediaTypeNames.Application.Zip)
{
    FileDownloadName = "remeshed.zip"
};
</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"## Testing the Code\n\nSo, we have shown how to build and deploy two example nodes for a 3D content pipeline but in order to test this I currently need to carry out the following:\n\n1. Take the model and textures from [here](https://github.com/peted70/az-func-blender/tree/master/TestModels) and add all contents of that folder to a zip archive as it is organized in the folder\n\n"} -->
<div class="wp-block-jetpack-markdown"><h2>Testing the Code</h2>
<p>So, we have shown how to build and deploy two example nodes for a 3D content pipeline but in order to test this I currently need to carry out the following:</p>
<ol>
<li>Take the model and textures from <a href="https://github.com/peted70/az-func-blender/tree/master/TestModels">here</a> and add all contents of that folder to a zip archive as it is organized in the folder</li>
</ol>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8509,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/input-zip.png" alt="" class="wp-image-8509"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"2. Manually uploaded to blob storage or some other accessible storage"} -->
<div class="wp-block-jetpack-markdown"><ol start="2">
<li>Manually uploaded to blob storage or some other accessible storage</li>
</ol>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8510,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/zip-in-blob.png" alt="" class="wp-image-8510"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"3. Use the resulting URI (I generated a SAS URI using the [Azure portal](https://ms.portal.azure.com/) as input to run the Blender Azure Function running either locally or in Azure and download the resulting zip archive (You could use [Postman](https://www.postman.com/) with a POST/GET request or a browser with a GET request)"} -->
<div class="wp-block-jetpack-markdown"><ol start="3">
<li>Use the resulting URI (I generated a SAS URI using the <a href="https://ms.portal.azure.com/">Azure portal</a> as input to run the Blender Azure Function running either locally or in Azure and download the resulting zip archive (You could use <a href="https://www.postman.com/">Postman</a> with a POST/GET request or a browser with a GET request)</li>
</ol>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8511,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/generate-sas.png" alt="" class="wp-image-8511"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":8512,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/postman-blender.png" alt="" class="wp-image-8512"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"4. Upload that zip archive to blob storage manually (so we can get another URI). So repeat the above step but with the downloaded archive.\n\n5. Use the resulting SAS URI as input to the Simplygon Azure Function running locally on PC or deployed to Azure\n\n6. Download and check the resulting zip archive"} -->
<div class="wp-block-jetpack-markdown"><ol start="4">
<li>
<p>Upload that zip archive to blob storage manually (so we can get another URI). So repeat the above step but with the downloaded archive.</p>
</li>
<li>
<p>Use the resulting SAS URI as input to the Simplygon Azure Function running locally on PC or deployed to Azure</p>
</li>
<li>
<p>Download and check the resulting zip archive</p>
</li>
</ol>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8507,"sizeSlug":"full"} -->
<figure class="wp-block-image size-full"><img src="{{ site.baseurl }}/assets/images/2020/09/results-high.png" alt="" class="wp-image-8507"/><figcaption>Shows&nbsp;the&nbsp;original&nbsp;mesh&nbsp;both&nbsp;shaded&nbsp;and&nbsp;wireframe&nbsp;against&nbsp;the&nbsp;processed&nbsp;result</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"And here is the output returned in the zip archive:"} -->
<div class="wp-block-jetpack-markdown"><p>And here is the output returned in the zip archive:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8497,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="{{ site.baseurl }}/assets/images/2020/09/archive-result.png" alt="" class="wp-image-8497"/><figcaption> Archive Result</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"## Further Investigation\n\n- Orchestrate the pipeline using either an Azure Logic App or durable function\n- Use Blender node to render a resulting simplified model into an mp4 so it can be easily reviewed\n- Investigate network transmission + 3D compression techniques"} -->
<div class="wp-block-jetpack-markdown"><h2>Further Investigation</h2>
<ul>
<li>Orchestrate the pipeline using either an Azure Logic App or durable function</li>
<li>Use Blender node to render a resulting simplified model into an mp4 so it can be easily reviewed</li>
<li>Investigate network transmission + 3D compression techniques</li>
</ul>
</div>
<!-- /wp:jetpack/markdown -->
