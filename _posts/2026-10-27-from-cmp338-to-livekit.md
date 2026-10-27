---
layout: single
classes: wide
title: "From CMP338 to LiveKit: where my talking avatars started"
excerpt: "At re:Invent 2024 I presented a MetaHuman avatar you could talk to. It could move its lips, but it had no emotions and no gestures. Part 1 of a three-part series on building expressive, real-time AI avatars."
date: 2026-10-27 09:00
author_profile: true
comments: true
categories: [AI, Avatars, AWS, LiveKit, MetaHuman, Unreal Engine]
header:
    teaserlogo: 'assets/images/2026/10/metahuman-3-identity.png'
    teaser: 'assets/images/2026/10/metahuman-3-identity.png'
---
*Part 1 of a three-part series on building expressive, real-time AI avatars. [Part 2]({% post_url 2026-10-27-a-behaviour-stream-for-livekit-agents %}) gets a LiveKit voice agent running; [part 3]({% post_url 2026-10-27-giving-my-avatar-a-face-and-a-body %}) builds the avatar.*

![Fitting a MetaHuman to a scan of my face]({{ site.baseurl }}/assets/images/2026/10/metahuman-3-identity.png)

## Where this started: CMP338 at re:Invent 2024

At re:Invent 2024, I presented **CMP338, "Generative AI-enabled 3D Avatar Assistants on AWS"**. It was a whiteboarding session about avatars you can talk to. The demos included a 2D retail assistant, a 3D avatar that translates speech into American Sign Language, and an LLM chat avatar built on Unreal Engine MetaHumans.

<iframe src="https://www.linkedin.com/embed/feed/update/urn:li:ugcPost:7267558763111288835?collapsed=1" height="542" width="504" frameborder="0" allowfullscreen="" title="Embedded post"></iframe>

![The LLM chat avatar demo: devices on the left, Unreal Engine with a MetaHuman in the middle, Amazon Bedrock on the right]({{ site.baseurl }}/assets/images/2026/10/reinvent-overview.png)

The cloud pipeline was all AWS:

1. Speech went in through **Amazon Transcribe**.
2. A **Lambda** function built a prompt and sent it to **Amazon Bedrock**, using Bedrock knowledge bases and your choice of model.
3. The answer went to **Amazon Polly** for a voice.
4. A lip-sync step ran on EC2.
5. The MetaHuman was rendered on G instances and streamed to the browser over **Pixel Streaming** (WebRTC).

![Cloud pipeline: Transcribe, Lambda, Bedrock with knowledge bases, Polly, then lip sync on EC2]({{ site.baseurl }}/assets/images/2026/10/reinvent-cloud-pipeline.png)

The avatar itself ran inside an **Unreal Engine** game. Plugins brought in the AWS C++ SDK, MetaHumans and Pixel Streaming. Each rendered frame was streamed to a laptop or phone over WebRTC, or to a headset over OpenXR:

![Engine architecture: Unreal Engine game with AWS SDK, MetaHumans and Pixel Streaming plugins, streaming over WebRTC or OpenXR]({{ site.baseurl }}/assets/images/2026/10/reinvent-engine-architecture.png)

On AWS, the GPU instances sat in an Auto Scaling group in a private subnet behind an Application Load Balancer, with TURN and signalling servers for WebRTC. 3D assets came from the Visual Asset Management System (VAMS):

![Cloud architecture: VPC with public and private subnets, load balancer, G instances, TURN and signalling servers, VAMS]({{ site.baseurl }}/assets/images/2026/10/reinvent-cloud-architecture.png)

### Scanning my face into a MetaHuman

The MetaHuman in the demo was me. Getting there took a few free tools:

1. **Capture:** I shot a short video of my head on a Google Pixel 7, capturing my head from all sides.

   ![A frame from the capture video]({{ site.baseurl }}/assets/images/2026/10/metahuman-capture-frame.jpg)

2. **Reconstruct:** [Meshroom](https://alicevision.org/#meshroom) (open source) ran structure-from-motion over the video frames. It matched features across the images, triangulated a point cloud, and built a textured mesh from it. Reality Capture or Polycam would do the same job.

   ![Meshroom: the video frames on the left, the reconstructed point cloud and camera positions on the right]({{ site.baseurl }}/assets/images/2026/10/metahuman-1-meshroom.png)

3. **Clean up:** I imported the textured mesh into Blender and cropped away everything except the face.

   ![The scanned mesh in Blender before cropping]({{ site.baseurl }}/assets/images/2026/10/metahuman-2-blender.png)

4. **Fit:** In Unreal Engine with the MetaHuman plugin, **Mesh to MetaHuman** fits the MetaHuman topology to the scan. The result is a fully rigged face and skeleton.

   ![MetaHuman Identity: the scan on the left, the fitted MetaHuman mesh on the right]({{ site.baseurl }}/assets/images/2026/10/metahuman-3-identity.png)

5. **Tweak:** Finally I adjusted skin, hair and beard in **MetaHuman Creator**.

   ![The finished MetaHuman in MetaHuman Creator]({{ site.baseurl }}/assets/images/2026/10/metahuman-4-creator.png)

   Trying on eyes, hair and beards in MetaHuman Creator:

   <video src="{{ site.baseurl }}/assets/images/2026/10/metahuman-creator.mp4" poster="{{ site.baseurl }}/assets/images/2026/10/metahuman-creator-poster.jpg" controls muted loop playsinline width="100%"></video>


### Keeping voice and face in sync in Unreal Engine

Getting the MetaHuman to *speak* turned out to be the fiddly part. Each answer arrived at runtime as audio plus the blend-shape animation for the lips. The two had to play back together, frame for frame, however long the answer was.

Unreal's timeline was the right tool for playing them together, but it's designed mostly as an editor feature: something you author ahead of time, not something you build while the game is running. So I had to construct the timeline dynamically for every answer, adding the audio and the blend-shape tracks at runtime, so the voice and the lips stayed in sync. Working out how to do that took a while.

I still have the repo. If you're trying to do the same thing, get in touch and I'm happy to share how it was done.

## What was missing

The talk opened with my MetaHuman on the big screen, driven live by an iPhone tracking my face.

It worked, and people enjoyed talking to it. But the avatar only moved its lips. It had **no emotions and no hand gestures**. It didn't look at the person talking, nod while they spoke, or shrug when it didn't know something. A lot of what makes a conversation feel alive just wasn't there.

The architecture also shaped what was possible. The MetaHuman was rendered in the cloud and streamed as video, so each viewer's experience depended on a GPU instance somewhere. And everything the avatar did had to be decided inside that one Unreal application.

## Where this series goes

This time I wanted to try a different shape:

- **A voice agent on LiveKit.** Its Agents framework handles speech-to-text, the LLM, text-to-speech and turn-taking in one pipeline.
- **A behaviour stream.** Next to its voice, the agent sends small messages: emotion, gaze, gestures, listening cues.
- **Rendering on the device.** Each viewer's own browser, headset or game engine acts out the stream. Lip sync is worked out locally from the audio.

### Why not the MetaHuman?

Rendering on the device changes which avatar makes sense. A MetaHuman is a high-end Unreal Engine character. That's why the re:Invent demo needed GPU instances and Pixel Streaming to reach a browser. For an avatar that runs in an ordinary browser tab or on a Quest, I wanted something much lighter. So this series uses my **Ready Player Me** avatar: a small glTF file that loads straight into three.js. It also follows the same conventions the behaviour stream relies on, a Mixamo-style skeleton and ARKit blend-shape names, so the same messages can drive either kind of avatar.

The MetaHuman isn't gone. Because each renderer interprets the same stream, a MetaHuman in Unreal is one of the renderers I want to drive with it later. Lighter avatar first, then back to where this started.

[Part 2]({% post_url 2026-10-27-a-behaviour-stream-for-livekit-agents %}) explains the behaviour stream and gets the LiveKit agent running from scratch. [Part 3]({% post_url 2026-10-27-giving-my-avatar-a-face-and-a-body %}) turns my own Ready Player Me avatar, which couldn't even open its mouth, into one that smiles, talks with its hands and shifts its weight.
