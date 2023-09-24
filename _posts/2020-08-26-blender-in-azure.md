---
layout: post
title: Blender in Azure
date: 2020-08-26 16:22
author: peted70
comments: true
categories: [3D, 3D, 3dmodels, Azure, azure, azure function, blender, Blender, c#]
---
<!-- wp:image {"id":8485,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/blender-logo-1024x486.png" alt="" class="wp-image-8485"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Blender has been receiving some well-deserved attention recently. In particular some big companies (including Microsoft) who understand the importance of this 3D content creation tool as we move towards a spatial computing-oriented landscape.\n\nBlender has been hitting the news with some notable additions to the [Blender Development Fund](https://fund.blender.org/)"} -->
<div class="wp-block-jetpack-markdown"><p>Blender has been receiving some well-deserved attention recently. In particular some big companies (including Microsoft) who understand the importance of this 3D content creation tool as we move towards a spatial computing-oriented landscape.</p>
<p>Blender has been hitting the news with some notable additions to the <a href="https://fund.blender.org/">Blender Development Fund</a></p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8459,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/blenderdevfund-1024x375.png" alt="" class="wp-image-8459"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"\u003e Blender has been around for 25 years or so as a free and open source 3D content creation and rendering tool. It has become a cornerstone in the 3D pipeline and it's freeness sets it apart from some of it's competitors. The industry investment is a signal that Blender is a key piece of technology for the future landscape and another indicator that the tipping point for adoption of spatial computing is in the not-too-distant future.  \n\nIn this post I will explore running Blender in an Azure function in order to automate elements of a 3D model pipeline in a scalable and cost-effective way. I will provide a code repo of all of the elements required from the Azure Function code to an example Docker file describing the container that we will run the Azure function in to some example Python scripts which allow automation of Blender functionality.\n\n## Content Pipeline\n\nJust to set the scene I'll give a simple illustrative example of what I mean by a content pipeline and give some examples of what it might be used for. So, imagine that I am starting with some high-resolution 3D models that have been created with some scanning hardware and the model has too much geometry to run efficiently on a mobile device such as HoloLens or a mobile phone. I also have some software that will optimise the model for the target devices but it only takes the glTF file format as input. So if I can create a pipeline node that will convert my input file to and from glTF then I can string nodes together as below to create my content pipeline."} -->
<div class="wp-block-jetpack-markdown"><blockquote>
<p>Blender has been around for 25 years or so as a free and open source 3D content creation and rendering tool. It has become a cornerstone in the 3D pipeline and it's freeness sets it apart from some of it's competitors. The industry investment is a signal that Blender is a key piece of technology for the future landscape and another indicator that the tipping point for adoption of spatial computing is in the not-too-distant future.</p>
</blockquote>
<p>In this post I will explore running Blender in an Azure function in order to automate elements of a 3D model pipeline in a scalable and cost-effective way. I will provide a code repo of all of the elements required from the Azure Function code to an example Docker file describing the container that we will run the Azure function in to some example Python scripts which allow automation of Blender functionality.</p>
<h2>Content Pipeline</h2>
<p>Just to set the scene I'll give a simple illustrative example of what I mean by a content pipeline and give some examples of what it might be used for. So, imagine that I am starting with some high-resolution 3D models that have been created with some scanning hardware and the model has too much geometry to run efficiently on a mobile device such as HoloLens or a mobile phone. I also have some software that will optimise the model for the target devices but it only takes the glTF file format as input. So if I can create a pipeline node that will convert my input file to and from glTF then I can string nodes together as below to create my content pipeline.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8460,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/content-pipeline-1024x168.png" alt="" class="wp-image-8460"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Blender can very easily be used to automate 3D file format conversions but can also be used to automate more complex scenarios such as:\n\n- Generating synthetic data for input to train Machine Learning models to do object recognition\n\n- Generating normal maps by baking lighting information from a high density 3D model onto a normal map to be used with a model that has reduced geometry so we can preserve a lot of the lighting detail.\n\n- Generating a rendered movie of a camera orbiting around a 3D model so that the result can be checked as part of a model quality checking process\n\nThese are a few useful examples that spring to mind as I have real-world experience with these but of course, the combinations are endless and given a set of processing nodes can be configured to fit the scenario.\n\nThe rest of this post will be concerned with all aspects of setting up a pipeline like this. The moving parts we will need to understand for this are:\n\n- How do we automate Blender to carry out the processing for nodes we might need?\n\n- How can we host the processing in the cloud? We are going to be using the Azure cloud for this.\n\n## Blender\n\nBlender can be used for 3D modeling, sculpting, creating and applying materials, rendering, character rigging, particle simulation, animation, 2D animation, editing and compositing.\n\n\u003e For Windows 10 users; Blender also appears in the Windows 10 Store so you can make use of auto-updates\n\n### Blender Automation\n\nSo, as well as all of the rich functionality for 3D content creation we can also automate it using the Python scripting interface. Blender has an embedded Python interpreter and a Python library exposing most of the functionality. The first step would be to open the Scripting tab which can be found along the top of the application to the far right.\n"} -->
<div class="wp-block-jetpack-markdown"><p>Blender can very easily be used to automate 3D file format conversions but can also be used to automate more complex scenarios such as:</p>
<ul>
<li>
<p>Generating synthetic data for input to train Machine Learning models to do object recognition</p>
</li>
<li>
<p>Generating normal maps by baking lighting information from a high density 3D model onto a normal map to be used with a model that has reduced geometry so we can preserve a lot of the lighting detail.</p>
</li>
<li>
<p>Generating a rendered movie of a camera orbiting around a 3D model so that the result can be checked as part of a model quality checking process</p>
</li>
</ul>
<p>These are a few useful examples that spring to mind as I have real-world experience with these but of course, the combinations are endless and given a set of processing nodes can be configured to fit the scenario.</p>
<p>The rest of this post will be concerned with all aspects of setting up a pipeline like this. The moving parts we will need to understand for this are:</p>
<ul>
<li>
<p>How do we automate Blender to carry out the processing for nodes we might need?</p>
</li>
<li>
<p>How can we host the processing in the cloud? We are going to be using the Azure cloud for this.</p>
</li>
</ul>
<h2>Blender</h2>
<p>Blender can be used for 3D modeling, sculpting, creating and applying materials, rendering, character rigging, particle simulation, animation, 2D animation, editing and compositing.</p>
<blockquote>
<p>For Windows 10 users; Blender also appears in the Windows 10 Store so you can make use of auto-updates</p>
</blockquote>
<h3>Blender Automation</h3>
<p>So, as well as all of the rich functionality for 3D content creation we can also automate it using the Python scripting interface. Blender has an embedded Python interpreter and a Python library exposing most of the functionality. The first step would be to open the Scripting tab which can be found along the top of the application to the far right.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8463,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/blender-scripting-small-1024x556.gif" alt="" class="wp-image-8463"/><figcaption>Blender Scripting</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"This configures Blender into a scripting-friendly workspace where you have the following windows:\n\n- 3D viewport\n\n\u003e The usual 3D viewport but allows you to visualise script commands as you run them\n\n- Python console\n\n\u003e You can run commands here and also explore the bpy library. type **bpy.** and then press **TAB** for an autocomplete list\n\n- Info window\n\n\u003e As you carry out user operations in Blender the associated script will get output here. You can copy the output from here directly into your script. (Note. this won't always give you what you want as some of the operations are highly context sensitive but it provides a good starting point)\n\n- Text Editor (where we write the Python code)\n\n\u003e Enter the text for the script here, always starting with **import bpy**. Under the templates menu you can find examples of python scripting from creating add-ons to UI-driven scripts. (We're only interested in **Background Job** for now).\n"} -->
<div class="wp-block-jetpack-markdown"><p>This configures Blender into a scripting-friendly workspace where you have the following windows:</p>
<ul>
<li>3D viewport</li>
</ul>
<blockquote>
<p>The usual 3D viewport but allows you to visualise script commands as you run them</p>
</blockquote>
<ul>
<li>Python console</li>
</ul>
<blockquote>
<p>You can run commands here and also explore the bpy library. type <strong>bpy.</strong> and then press <strong>TAB</strong> for an autocomplete list</p>
</blockquote>
<ul>
<li>Info window</li>
</ul>
<blockquote>
<p>As you carry out user operations in Blender the associated script will get output here. You can copy the output from here directly into your script. (Note. this won't always give you what you want as some of the operations are highly context sensitive but it provides a good starting point)</p>
</blockquote>
<ul>
<li>Text Editor (where we write the Python code)</li>
</ul>
<blockquote>
<p>Enter the text for the script here, always starting with <strong>import bpy</strong>. Under the templates menu you can find examples of python scripting from creating add-ons to UI-driven scripts. (We're only interested in <strong>Background Job</strong> for now).</p>
</blockquote>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8466,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/python-templates-730x1024.png" alt="" class="wp-image-8466"/><figcaption>Python Templates</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"I'm not going to attempt a tutorial of this as there are many online already but I have included some useful tips.\n\n#### Visual Studio Code Extension\n\nOne last tip is to point you at the VS Code Extension for Blender Development.\n\n"} -->
<div class="wp-block-jetpack-markdown"><p>I'm not going to attempt a tutorial of this as there are many online already but I have included some useful tips.</p>
<h4>Visual Studio Code Extension</h4>
<p>One last tip is to point you at the VS Code Extension for Blender Development.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8467,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/vscode-ext-1024x474.png" alt="" class="wp-image-8467"/><figcaption>VS Code Extension</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"## Example Content Pipeline Script\n\nI'll now present the example script I'm going to use for this post.\n\nThe script is an example of how you can run blender from the command line (in background mode with no interface) to automate tasks. This example will\n\n\u003e - load a .obj file\n\u003e - load albedo, normal and ambient occlusion/roughness/metallic maps\n\u003e - create a Principled BSDF material using those textures as input\n\u003e - output the file as either an obj + obj material (without the PBR textures since those are unsupported in obj format) or a glTF file with the PBR textures assigned.\n\nFor now, to run this you would need to have blender installed and it's executable location in your PATH. Then you can run it like this where all args after the \u002d\u002d are passed to the script. File output options are currently gltf and obj.\n"} -->
<div class="wp-block-jetpack-markdown"><h2>Example Content Pipeline Script</h2>
<p>I'll now present the example script I'm going to use for this post.</p>
<p>The script is an example of how you can run blender from the command line (in background mode with no interface) to automate tasks. This example will</p>
<blockquote>
<ul>
<li>load a .obj file</li>
<li>load albedo, normal and ambient occlusion/roughness/metallic maps</li>
<li>create a Principled BSDF material using those textures as input</li>
<li>output the file as either an obj + obj material (without the PBR textures since those are unsupported in obj format) or a glTF file with the PBR textures assigned.</li>
</ul>
</blockquote>
<p>For now, to run this you would need to have blender installed and it's executable location in your PATH. Then you can run it like this where all args after the -- are passed to the script. File output options are currently gltf and obj.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>blender -b -P ./scripts/objmat.py -- -i ./TestModels/barramundi.obj -o gltf</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"The result should be a copy of the file in a folder named *converted* with *_converted* appended to the filename and with an additional .mtl file in the same folder as the original file. Alternatively, there will be a glTF file there which references the loose textures. The script recursively searches folders from the original file location looking for textures by name, i.e. albedo, normal and orm. There is a distinct lack of error handling as I just wanted to use this as a handy example.\n\nNotes. Some 3D software supports an ORM map in an obj material by adding the line\n\n\u003e map_ORM ORM.png\n\nto the .mtl file. This is non-standard and un-supported by some 3D tools. For this kind of PBR support glTF can be used.\n\nTo be honest it doesn't really matter what the script does as long as we can show a script which takes some input and process it to provide some measurable output, and I have this script written and tested already.  \n\nHere's the script being run from the command line and also showing the output:"} -->
<div class="wp-block-jetpack-markdown"><p>The result should be a copy of the file in a folder named <em>converted</em> with <em>_converted</em> appended to the filename and with an additional .mtl file in the same folder as the original file. Alternatively, there will be a glTF file there which references the loose textures. The script recursively searches folders from the original file location looking for textures by name, i.e. albedo, normal and orm. There is a distinct lack of error handling as I just wanted to use this as a handy example.</p>
<p>Notes. Some 3D software supports an ORM map in an obj material by adding the line</p>
<blockquote>
<p>map_ORM ORM.png</p>
</blockquote>
<p>to the .mtl file. This is non-standard and un-supported by some 3D tools. For this kind of PBR support glTF can be used.</p>
<p>To be honest it doesn't really matter what the script does as long as we can show a script which takes some input and process it to provide some measurable output, and I have this script written and tested already.</p>
<p>Here's the script being run from the command line and also showing the output:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8469,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/blender-script-1-1024x679.gif" alt="" class="wp-image-8469"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"\u003e The sample input is provided in the Github repo\n\nHere's the main part of the script:\n"} -->
<div class="wp-block-jetpack-markdown"><blockquote>
<p>The sample input is provided in the Github repo</p>
</blockquote>
<p>Here's the main part of the script:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>def load_obj_and_create_material(input_file, outputFormat):

    # Clear existing objects.
    bpy.ops.wm.read_factory_settings(use_empty=True)
    
    # Clear the current scene - useful if we want to run inside blender and retain preferences
    # for item in bpy.context.scene.objects:
    #    bpy.data.objects.remove(item, do_unlink=True)
    bpy.ops.import_scene.obj(filepath=input_file)
    
    #obj_object = bpy.context.selected_objects&#91;0] ####&lt;--Fix
    # make sure to get all imported objects
    obs = &#91; o for o in bpy.context.scene.objects if o.select_get() ]

    print('Imported objects: count = ' + str(len(obs)))
    for ob in obs:
        print(ob.name)

    numImportedPolygons = 0
    for ob in obs:
        numImportedPolygons += len(ob.data.polygons)
        
    print('Number of imported polygons = ' + str(numImportedPolygons))

    newmat = bpy.data.materials.new('newmat')
    newmat.use_nodes = True
    node_tree = newmat.node_tree

    # asign the new material to each imported mesh
    for ob in obs:
        # Assign it to object
        if ob.data.materials:
            # assign to 1st material slot
            ob.data.materials&#91;0] = newmat
        else:
            # no slots
            ob.data.materials.append(newmat)

    nodes = node_tree.nodes
    pbdf = nodes.get("Principled BSDF")
    
    albedoFile = ''
    normalFile = ''
    ORMFile = ''

    # we want to locate and load image by name so albedo, normal and ORM
    for dirpath, _, files in os.walk(os.path.dirname(input_file)):
        for filename in files:
            filenameLower = filename.lower()
            if filenameLower.endswith('.png'):
                print(filename)
                if IsAlbedo(filenameLower):
                    albedoFile = os.path.abspath(os.path.join(dirpath, filename))
                elif IsNormal(filenameLower):
                    normalFile = os.path.abspath(os.path.join(dirpath, filename))
                elif IsORM(filenameLower):
                    ORMFile = os.path.abspath(os.path.join(dirpath, filename))

    links = node_tree.links

    if albedoFile:
        # Create albedo node and wire it up
        img = bpy.data.images.load(albedoFile)
        albedoNode = newmat.node_tree.nodes.new(type='ShaderNodeTexImage')
        albedoNode.image = img
        links.new(albedoNode.outputs&#91;'Color'], pbdf.inputs&#91;'Base Color'])

    if normalFile:
        # Create normal map node and wire it up
        img = bpy.data.images.load(normalFile)
        
        normalImageNode = newmat.node_tree.nodes.new(type='ShaderNodeTexImage')
        normalImageNode.image = img
        normalImageNode.image.colorspace_settings.name = 'Non-Color'

        normalMapNode = newmat.node_tree.nodes.new(type='ShaderNodeNormalMap')
        links.new(normalImageNode.outputs&#91;'Color'], normalMapNode.inputs&#91;'Color'])
        links.new(normalMapNode.outputs&#91;'Normal'], pbdf.inputs&#91;'Normal'])
    
    if ORMFile:    
        # Create the ORM mapping and wire it up to the BSDF shader
        img = bpy.data.images.load(ORMFile)
        ormNode = newmat.node_tree.nodes.new(type='ShaderNodeTexImage')
        ormNode.image = img
        
        # pipe the output from the image node into an RGB splitter node
        rgbSplitterNode = newmat.node_tree.nodes.new(type='ShaderNodeSeparateRGB')
        
        links = node_tree.links
        links.new(ormNode.outputs&#91;'Color'], rgbSplitterNode.inputs&#91;'Image'])
        links.new(rgbSplitterNode.outputs&#91;'G'], pbdf.inputs&#91;'Roughness'])        
        links.new(rgbSplitterNode.outputs&#91;'B'], pbdf.inputs&#91;'Metallic'])        
            
    # finally we need to export the obj again and hopefully it will have our material
    dirName = os.path.dirname(input_file)
    dirName = os.path.join(dirName, "converted")
    baseName = os.path.basename(input_file)
    filenameWithoutExt, _ = os.path.splitext(baseName)

    if not os.path.exists(dirName):
        os.makedirs(dirName)

    outputFormat = outputFormat.lower()

    outpathWithoutExt = os.path.join(dirName, filenameWithoutExt + '_converted')

    if outputFormat == 'gltf':
        output_file = outpathWithoutExt + '.gltf'
        print('Output File = ' + output_file)
        bpy.ops.export_scene.gltf(filepath=output_file,export_format='GLTF_SEPARATE',export_image_format='JPEG')
    elif outputFormat == 'obj':
        output_file = outpathWithoutExt + '.obj'
        print('Output File = ' + output_file)
        bpy.ops.export_scene.obj(filepath=output_file)
</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"and here's a graphical representation of how the script is setting up the PBR material.\n"} -->
<div class="wp-block-jetpack-markdown"><p>and here's a graphical representation of how the script is setting up the PBR material.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8470,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/obj-shader-1024x513.png" alt="" class="wp-image-8470"/><figcaption>.obj PBR Material</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"## Running on Azure\n\nSo, we have our unit of processing that we can currently run from the command line. (I'm developing this on Windows but this would also run on Linux or Mac).\n\nI decided to set up a Docker container with Blender installed and a selection of python scripts copied over and ready to be called.\n\nThis choice, while seemingly simple involved some trade-offs and considerations:\n\n- An Azure function hosted in a custom container requires an App Service Plan and does not run in the usual Consumption Plan providing true pay-as-you-go.\n\n- Using a container approach makes the solution flexible and the service layer can be switched more easily and the containers can be run locally on a development PC or on a local network.\n\n- Azure Container Instances presented another possible solution possibly using a Logic App or Durable Function to coordinate the containers.\n\nEither way, I decided to start first by creating a custom Docker container.\n\n### Docker\n\n"} -->
<div class="wp-block-jetpack-markdown"><h2>Running on Azure</h2>
<p>So, we have our unit of processing that we can currently run from the command line. (I'm developing this on Windows but this would also run on Linux or Mac).</p>
<p>I decided to set up a Docker container with Blender installed and a selection of python scripts copied over and ready to be called.</p>
<p>This choice, while seemingly simple involved some trade-offs and considerations:</p>
<ul>
<li>
<p>An Azure function hosted in a custom container requires an App Service Plan and does not run in the usual Consumption Plan providing true pay-as-you-go.</p>
</li>
<li>
<p>Using a container approach makes the solution flexible and the service layer can be switched more easily and the containers can be run locally on a development PC or on a local network.</p>
</li>
<li>
<p>Azure Container Instances presented another possible solution possibly using a Logic App or Durable Function to coordinate the containers.</p>
</li>
</ul>
<p>Either way, I decided to start first by creating a custom Docker container.</p>
<h3>Docker</h3>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS installer-env

COPY . /src/dotnet-function-app
RUN cd /src/dotnet-function-app &amp;&amp; \
    mkdir -p /home/site/wwwroot &amp;&amp; \
    dotnet publish *.csproj --output /home/site/wwwroot

# To enable ssh &amp; remote debugging on app service change the base image to the one below
# FROM mcr.microsoft.com/azure-functions/dotnet:3.0-appservice
FROM mcr.microsoft.com/azure-functions/dotnet:3.0
ENV AzureWebJobsScriptRoot=/home/site/wwwroot \
    AzureFunctionsJobHost__Logging__Console__IsEnabled=true

COPY --from=installer-env &#91;"/home/site/wwwroot", "/home/site/wwwroot"]

# get latest python &amp; blender related dependencies
RUN apt-get update &amp;&amp; apt-get install -y --no-install-recommends apt-utils python3 python3-virtualenv \
python3-dev python3-pip libx11-6 libxi6 libxxf86vm1 libxfixes3 libxrender1 unzip wget bzip2 xz-utils \
&amp;&amp; rm -rf /var/lib/apt/lists/*

# get the dependencies for the script
RUN mkdir -p /local/
ADD scripts /local/scripts/

# get the blender 2.81a and setup the paths
RUN cd /tmp &amp;&amp; wget -q https://download.blender.org/release/Blender2.83/blender-2.83.4-linux64.tar.xz \
&amp;&amp; ls -al \
&amp;&amp; tar xvf /tmp/blender-2.83.4-linux64.tar.xz -C /usr/bin/ \
&amp;&amp; rm -r /tmp/blender-2.83.4-linux64.tar.xz

# copy the shared lib for blender
RUN cp /usr/bin/blender-2.83.4-linux64/lib/lib* /usr/local/lib/ &amp;&amp; ldconfig

# ENTRYPOINT &#91;"/usr/bin/blender-2.83.4-linux64/blender", "-b", "--version"]

</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown -->
<div class="wp-block-jetpack-markdown"></div>
<!-- /wp:jetpack/markdown -->

<!-- wp:jetpack/markdown {"source":"This started it's life as a result of me following the guide [here](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-function-linux-custom-image?tabs=bash%2Cportal\u0026pivots=programming-language-csharp) which walks through creating a function hosted on Linux using a custom container.\n\nWhen running,\n"} -->
<div class="wp-block-jetpack-markdown"><p>This started it's life as a result of me following the guide <a href="https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-function-linux-custom-image?tabs=bash%2Cportal&amp;pivots=programming-language-csharp">here</a> which walks through creating a function hosted on Linux using a custom container.</p>
<p>When running,</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>func init az-func-blender --docker</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"followed by navigating into the *az-func-blender* folder and executing"} -->
<div class="wp-block-jetpack-markdown"><p>followed by navigating into the <em>az-func-blender</em> folder and executing</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>func new --name RunBlenderScripts --template "HTTP trigger"</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"ou will get created a boilerplate Dockerfile and Function definition. The HTTP Trigger means I can call this from an http request but there is a range of triggers that could be used (see [Work with Azure Functions Core Tools](https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=windows%2Ccsharp%2Cbash) for more details. I added to the Dockerfile:\n\n- Getting the latest python \u0026 blender related dependencies\n\n- Copying the python Blender scripts from a local folder\n\n- Downloading and installing Blender\n\nSetup up continuous integration, debugging and VScode.commnds to run locally, etc..\n\n### Azure Function Code\n\nWith the error handling and logging removed to aid readability my Azure Function code looks like this:"} -->
<div class="wp-block-jetpack-markdown"><p>ou will get created a boilerplate Dockerfile and Function definition. The HTTP Trigger means I can call this from an http request but there is a range of triggers that could be used (see <a href="https://docs.microsoft.com/en-us/azure/azure-functions/functions-run-local?tabs=windows%2Ccsharp%2Cbash">Work with Azure Functions Core Tools</a> for more details. I added to the Dockerfile:</p>
<ul>
<li>
<p>Getting the latest python &amp; blender related dependencies</p>
</li>
<li>
<p>Copying the python Blender scripts from a local folder</p>
</li>
<li>
<p>Downloading and installing Blender</p>
</li>
</ul>
<p>Setup up continuous integration, debugging and VScode.commnds to run locally, etc..</p>
<h3>Azure Function Code</h3>
<p>With the error handling and logging removed to aid readability my Azure Function code looks like this:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>&#91;FunctionName("RunBlenderScripts")]
public static async Task&lt;IActionResult> Run(
    &#91;HttpTrigger(AuthorizationLevel.Anonymous, "post", "get", Route = null)] HttpRequest req,
    ILogger log)
{
    var data = await ProcessParameters(req);

    var workingDir = Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
    var root = Path.GetPathRoot(workingDir);

    // Download the zip file containing the payload.
    //
    HttpResponseMessage response = null;
    using (var http = new HttpClient())
    {
        response = await http.GetAsync(data.InputZipUri);
    }

    // Extract the input zip archive into a temp location
    //
    var zipDir = root + TempRootDir + Guid.NewGuid();
    using (var za = new ZipArchive(await response.Content.ReadAsStreamAsync(), ZipArchiveMode.Read))
    {
        za.ExtractToDirectory(zipDir, true);
    }

    // find the obj file in the root of the extracted archive
    //
    DirectoryInfo DirectoryInWhichToSearch = new DirectoryInfo(zipDir);
    FileInfo objFile = DirectoryInWhichToSearch.GetFiles("*.obj").Single();

    // objFilePath parameter
    var ObjFilePathParameter = objFile.FullName;
    var OutputFormatParameter = data.OutputFormat;

    if (!File.Exists(BlenderExeLocation))
        return new StatusCodeResult((int)HttpStatusCode.InternalServerError);

    var commandArguments = "-b -P /local/scripts/objmat.py -- -i " + ObjFilePathParameter + " -o " + OutputFormatParameter;
    log.LogInformation($"commandArguments = {commandArguments}");
    var processInfo = new ProcessStartInfo(BlenderExeLocation, commandArguments);

    processInfo.CreateNoWindow = true;
    processInfo.UseShellExecute = false;

    processInfo.RedirectStandardError = true;
    processInfo.RedirectStandardOutput = true;

    Process process = Process.Start(processInfo);

    string output = string.Empty;
    string err = string.Empty;

    if (process != null)
    {
        // Read the output (or the error)
        output = process.StandardOutput.ReadToEnd();
        err = process.StandardError.ReadToEnd();
        process.WaitForExit();
    }

    var stream = new MemoryStream();

    // Ensure using true for the leaveOpen parameter otherwise the memory stream will
    // get disposed before w are done with it.
    //
    using (var OutputZip = new ZipArchive(stream, ZipArchiveMode.Create, true))
    {
        OutputZip.CreateEntryFromDirectory(zipDir);
    }

    // Rewind
    //
    stream.Position = 0;

    return new FileStreamResult(stream, System.Net.Mime.MediaTypeNames.Application.Zip)
    {
        FileDownloadName = ArchiveName
    };
}
</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"\u003e I retained support for both GET and POST for ease of testing\n\nThe function requires a url input identifying the location of a zip archive which contains the input files for the Blender script. The code then unzips the archive to disk and the unzipped location is passed into the command line for Blender to run our script with the input location argument also passing the output format required. If you recall the python script writes the process output to a folder called *converted* and the contents of that folder are then added to a new zip archive and returned.\n\n### Development Cycle\n\nSo, we have a function to call and a container to host the function. It would be useful now to understand the development cycle including; debugging, deployment and testing.\n\n#### Running Locally\n\nIf working with Visual Studio Code (which I was, mostly whilst developing so will focus here) you can install a Docker extension. More info [here](https://code.visualstudio.com/docs/containers/overview) on working with containers in VS Code.\n\n"} -->
<div class="wp-block-jetpack-markdown"><blockquote>
<p>I retained support for both GET and POST for ease of testing</p>
</blockquote>
<p>The function requires a url input identifying the location of a zip archive which contains the input files for the Blender script. The code then unzips the archive to disk and the unzipped location is passed into the command line for Blender to run our script with the input location argument also passing the output format required. If you recall the python script writes the process output to a folder called <em>converted</em> and the contents of that folder are then added to a new zip archive and returned.</p>
<h3>Development Cycle</h3>
<p>So, we have a function to call and a container to host the function. It would be useful now to understand the development cycle including; debugging, deployment and testing.</p>
<h4>Running Locally</h4>
<p>If working with Visual Studio Code (which I was, mostly whilst developing so will focus here) you can install a Docker extension. More info <a href="https://code.visualstudio.com/docs/containers/overview">here</a> on working with containers in VS Code.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8471,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/docker-vscode-ext-1024x300.png" alt="" class="wp-image-8471"/><figcaption>VS Code Docker Extension</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"If we were to start in debugging session from VS Code at this point the Docker extension would throw up this error:\n"} -->
<div class="wp-block-jetpack-markdown"><p>If we were to start in debugging session from VS Code at this point the Docker extension would throw up this error:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8472,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/no-running-containers.png" alt="" class="wp-image-8472"/><figcaption>No running containers</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"So how to run a container locally? Well, after heading to [Docker.com](https://www.docker.com/), installing Docker Desktop and becoming familiar with the command line tools I was able to run a local container mostly using the commands below:\n"} -->
<div class="wp-block-jetpack-markdown"><p>So how to run a container locally? Well, after heading to <a href="https://www.docker.com/">Docker.com</a>, installing Docker Desktop and becoming familiar with the command line tools I was able to run a local container mostly using the commands below:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>docker build .
docker images
docker tag &lt;image id> peted70/blender-functions:v1
docker run peted70/blender-functions:v1
docker ps
docker exec -it &lt;container name> /bin/bash</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"\u003e - **Docker build** will build a container image from the Dockerfile.\n\u003e - Using **docker images** will list all local images.\n\u003e - **docker tag** will apply a friendly name to the image\n\u003e - **docker run** will run a container based on the specified image\n\u003e - **docker ps** will show running containers\n"} -->
<div class="wp-block-jetpack-markdown"><blockquote>
<ul>
<li><strong>Docker build</strong> will build a container image from the Dockerfile.</li>
<li>Using <strong>docker images</strong> will list all local images.</li>
<li><strong>docker tag</strong> will apply a friendly name to the image</li>
<li><strong>docker run</strong> will run a container based on the specified image</li>
<li><strong>docker ps</strong> will show running containers</li>
</ul>
</blockquote>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8473,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/docker-cli-small.gif" alt="" class="wp-image-8473"/></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Once you have a running container then theoretically you can attach the debugger and hit some breakpoints in your Azure Function code. At the time of writing I didn't quite get this to work in VS Code. It starts well by prompting me to attach to running local containers:\n"} -->
<div class="wp-block-jetpack-markdown"><p>Once you have a running container then theoretically you can attach the debugger and hit some breakpoints in your Azure Function code. At the time of writing I didn't quite get this to work in VS Code. It starts well by prompting me to attach to running local containers:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8474,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/vscode-attach-1024x463.png" alt="" class="wp-image-8474"/><figcaption>VS Code Attach</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"And when I select the appropriate container I am prompted to copy over the remote debugging tools:\n"} -->
<div class="wp-block-jetpack-markdown"><p>And when I select the appropriate container I am prompted to copy over the remote debugging tools:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8475,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/vscode-copy-debug-tools-1024x271.png" alt="" class="wp-image-8475"/><figcaption>Copy Remote Debugging Tools </figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"But this process never quite completes correctly for me:\n"} -->
<div class="wp-block-jetpack-markdown"><p>But this process never quite completes correctly for me:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8476,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/Debug-error-vscode-1024x419.png" alt="" class="wp-image-8476"/><figcaption>Debugging Error</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"I haven't had time yet to investigate further and I switched over to VS2019 for debugging.\n\n\u003e One thing to note here is that you can run these tools in Linux on Windows with WSL2 (Windows Subsystem for Linux). [Here's](https://www.hanselman.com/blog/HowToSetUpDockerWithinWindowsSystemForLinuxWSL2OnWindows10.aspx) a guide by Scott Hanselmann on just that.\n"} -->
<div class="wp-block-jetpack-markdown"><p>I haven't had time yet to investigate further and I switched over to VS2019 for debugging.</p>
<blockquote>
<p>One thing to note here is that you can run these tools in Linux on Windows with WSL2 (Windows Subsystem for Linux). <a href="https://www.hanselman.com/blog/HowToSetUpDockerWithinWindowsSystemForLinuxWSL2OnWindows10.aspx">Here's</a> a guide by Scott Hanselmann on just that.</p>
</blockquote>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8477,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/vscode-wsl2-1024x565.png" alt="" class="wp-image-8477"/><figcaption>VS Code WSL2</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Note the 'little-bit-too' subtle WSL:Ubuntu indicator reminding you that you are running linux!\n\nDuring development for this project I switched about a bit between Windows/Linux and it can get a little bit confusing. One issue is that when writing code in my Azure function and referencing the file system in the .NET code there I need a good way to work cross-platform. I suspect relative file paths are the way forward but didn't quite get how to access the Blender executable correctly.\n\n#### Deployment to Azure\n\nSo, we need to deploy our Azure Function, our Docker image and somehow run a container based on the image.\n\n##### Azure Function App\n\nI'm going to defer to [this documentation](https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-function-linux-custom-image?tabs=bash%2Cportal\u0026pivots=programming-language-csharp) that I mentioned earlier to help set up an Azure Function App and push your Azure function to it.\n\nI used this command quite a lot:"} -->
<div class="wp-block-jetpack-markdown"><p>Note the 'little-bit-too' subtle WSL:Ubuntu indicator reminding you that you are running linux!</p>
<p>During development for this project I switched about a bit between Windows/Linux and it can get a little bit confusing. One issue is that when writing code in my Azure function and referencing the file system in the .NET code there I need a good way to work cross-platform. I suspect relative file paths are the way forward but didn't quite get how to access the Blender executable correctly.</p>
<h4>Deployment to Azure</h4>
<p>So, we need to deploy our Azure Function, our Docker image and somehow run a container based on the image.</p>
<h5>Azure Function App</h5>
<p>I'm going to defer to <a href="https://docs.microsoft.com/en-us/azure/azure-functions/functions-create-function-linux-custom-image?tabs=bash%2Cportal&amp;pivots=programming-language-csharp">this documentation</a> that I mentioned earlier to help set up an Azure Function App and push your Azure function to it.</p>
<p>I used this command quite a lot:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:code -->
<pre class="wp-block-code"><code>func azure functionapp publish</code></pre>
<!-- /wp:code -->

<!-- wp:jetpack/markdown {"source":"and I also used the Azure portal to help me determine when a new function had been published, when a new container image was available and some other developer tools that you can find in the Developer Tools section of your Function App.\n\nThey also link to Kudu (which you can access via https://myfunctionapp.scm.azurewebsites.net/) so you can open a shell and access logs, etc.\n"} -->
<div class="wp-block-jetpack-markdown"><p>and I also used the Azure portal to help me determine when a new function had been published, when a new container image was available and some other developer tools that you can find in the Developer Tools section of your Function App.</p>
<p>They also link to Kudu (which you can access via https://myfunctionapp.scm.azurewebsites.net/) so you can open a shell and access logs, etc.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8478,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/kudu-1024x626.png" alt="" class="wp-image-8478"/><figcaption>Kudu</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"##### Docker Hub Continuous Deployment\n\nAs well as running the container locally we can push to a container registry. I'm using Docker Hub but [Azure Container Registry](https://azure.microsoft.com/en-us/services/container-registry/) might be worth consideration also:\n"} -->
<div class="wp-block-jetpack-markdown"><h5>Docker Hub Continuous Deployment</h5>
<p>As well as running the container locally we can push to a container registry. I'm using Docker Hub but <a href="https://azure.microsoft.com/en-us/services/container-registry/">Azure Container Registry</a> might be worth consideration also:</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8479,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/docker-hub-1024x284.png" alt="" class="wp-image-8479"/><figcaption>Docker Hub</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"We can also set this up so that when the Dockerfile in the Github repo changes a new build of the container is kicked off.\n"} -->
<div class="wp-block-jetpack-markdown"><p>We can also set this up so that when the Dockerfile in the Github repo changes a new build of the container is kicked off.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8480,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/docker-hub-builds-1024x520.png" alt="" class="wp-image-8480"/><figcaption>Docker Hub Builds</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"You can also let your Azure Function App know when there is a new image by configuring a web hook."} -->
<div class="wp-block-jetpack-markdown"><p>You can also let your Azure Function App know when there is a new image by configuring a web hook.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8481,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/az-func-app-cicd-1024x528.png" alt="" class="wp-image-8481"/><figcaption>Function App CICD</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"#### Testing\n\nI mostly used [Postman](https://www.postman.com/) for creating the http requests required for testing the function but any good http client will do and you can even send a test request from the Azure portal. I left support for http GET also so I could test from a browser."} -->
<div class="wp-block-jetpack-markdown"><h4>Testing</h4>
<p>I mostly used <a href="https://www.postman.com/">Postman</a> for creating the http requests required for testing the function but any good http client will do and you can even send a test request from the Azure portal. I left support for http GET also so I could test from a browser.</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8482,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/azure-portal-test-af-1024x964.png" alt="" class="wp-image-8482"/><figcaption>Azure Portal Test</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"And here's similar in Postman,"} -->
<div class="wp-block-jetpack-markdown"><p>And here's similar in Postman,</p>
</div>
<!-- /wp:jetpack/markdown -->

<!-- wp:image {"id":8483,"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="http://peted.azurewebsites.net/wp-content/uploads/2020/08/postman-post-1024x309.png" alt="" class="wp-image-8483"/><figcaption>Test using Postman</figcaption></figure>
<!-- /wp:image -->

<!-- wp:jetpack/markdown {"source":"Notice the little drop-down on the **Send** button for **Send \u0026 Download** which I used to download the resulting zip archive.\n\n\u003e Note that the Input Zip Uri is specified as a SAS uri which helps to control access to the blob and can be configured and created in the [Azure Portal](https://portal.azure.com/)\n\n## Further Work\n\nI realise that this post in now becoming quite long so I am going to wrap up but I also wanted to include other areas that I would have liked to move onto given the time:\n\n- Cost analysis\n\n- Scaling analysis\n\n- Azure Functions vs Azure Container Instances\n\n- Extended, useful Blender scripts and an interface to selectively run functionality in those scripts\n\n- Considerations for rendering\n"} -->
<div class="wp-block-jetpack-markdown"><p>Notice the little drop-down on the <strong>Send</strong> button for <strong>Send &amp; Download</strong> which I used to download the resulting zip archive.</p>
<blockquote>
<p>Note that the Input Zip Uri is specified as a SAS uri which helps to control access to the blob and can be configured and created in the <a href="https://portal.azure.com/">Azure Portal</a></p>
</blockquote>
<h2>Further Work</h2>
<p>I realise that this post in now becoming quite long so I am going to wrap up but I also wanted to include other areas that I would have liked to move onto given the time:</p>
<ul>
<li>
<p>Cost analysis</p>
</li>
<li>
<p>Scaling analysis</p>
</li>
<li>
<p>Azure Functions vs Azure Container Instances</p>
</li>
<li>
<p>Extended, useful Blender scripts and an interface to selectively run functionality in those scripts</p>
</li>
<li>
<p>Considerations for rendering</p>
</li>
</ul>
</div>
<!-- /wp:jetpack/markdown -->
