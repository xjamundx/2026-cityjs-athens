---
theme: "@ktym4a/slidev-theme-ktym4a"
title: Agentic Image Generation
info: |
  Jamund Ferguson, CityJS Athens 2026
  Organized from the preserved source notes in source-notes-verbatim.txt.
drawings:
  persist: false
transition: slide-left
mdc: true
themeConfig:
  baseColor: lavender
  colorPattern: rotation
---

<div class="opening-slide">
  <div class="opening-copy">
    <p class="opening-kicker">Beyond the Prompt</p>
    <h1 class="opening-title">
      <span>Building a multi-step</span>
      <span>image generation workflow</span>
    </h1>
    <p class="opening-subtitle">With the Cloudflare Agents SDK</p>
  </div>

  <div class="opening-footer">
    <div class="text-left">
      <strong>Jamund Ferguson</strong>
      <a href="https://www.x.com/anxietytech">@anxietytech</a>
    </div>
    <div class="opening-event">
      <span>CityJS Athens</span>
      <strong>2026 🇬🇷</strong>
    </div>
  </div>
</div>

<!--
Hi, I'm Jamund Ferguson. I'm currently at Axle Energy, but I spent the last two years at Stability AI leading the front-end team and building UIs for image generation. Primarily a tool called Brand Studio.
-->

---

<div class="absolute inset-6 grid grid-cols-[7fr_5fr] items-center gap-8">
  <div class="h-full min-w-0 flex items-center justify-center">
    <img
      src="./assets/brandstudio-slow.gif"
      alt="Brand Studio workflow"
      class="block max-h-full w-full rounded-xl object-contain"
    />
  </div>
  <div class="pl-2">
    <h2 class="!mt-0 !mb-4">Brand Studio</h2>
    <p>Generates marketing images <br/> from a text prompt box</p>
  </div>
</div>

<!--
I mainly worked on a tool called brand studio, which helps companies edit and generate images used in marketing campaigns.

As you can see here we throw up a big text box in front of people and asks them what they want to create.

-->

---

<h2><span class="text-white text-shadow-lg">4 UX Challenges</span></h2>

<div class="grid grid-cols-2 h-100 gap-4 text-black font-bold" style="font-size: 1.5em; line-height: 1.28">
  <div class="bg-red p-4 text-shadow-xl shadow-sm shadow-black text-white">
    1. It's really hard to write a good prompt
  </div>
  <div class="bg-orange p-4 text-shadow-xl shadow-black shadow-lg text-white">
    2. There are a tonne of different image models to choose from
  </div>
  <div class="bg-yellow p-4 text-shadow-lg shadow-lg shadow-black text-white">
    3. People want to see their brand, or in some cases themselves, represented in the output
    </div>
  <div class="bg-blue p-4 text-shadow-lg shadow-black text-white">
    4. An overall lack of creative control over the image generation process
  </div>
</div>


---

<h1><span class="text-yellow text-shadow-lg">HAVE YOU SEEN THIS MAN?</span></h1>

<!--
Trying to describe an image to a diffusion model is kind of like those police sketches where you have to describe the features that you want them to draw and they do their best to interpret it
-->
<div class="flex flex-row gap-4 p-4">
  <figure class="w-100"s>
    <img src="./assets/police-sketch.avif" class="h-80" />
    <figcaption><a class="text-xs" href="https://www.independent.co.uk/news/weird-news/worst-police-sketch-ever-police-hunt-oddly-spherical-man-9075276.html">The Independent: Worst police sketch ever? Police hunt oddly spherical man
  </a></figcaption>
  </figure>
  <div>
  <div class="absolute text-[2.5em] leading-[.75] text-shadow-sm">
Humans in general are fairly bad at describing people in great detail.
  <br/><br/>But the details really matter.
  </div>
  </div>
</div>

<!--
  This is kind of how I think about prompting an image generation model. <br/><br/> Like you're describing a criminal a sketch arist. <br/><br/>
-->


---

## ❌ Writing Bad Prompts

<img
  v-click.hide="1"
  src="./assets/chat.png"
  alt="People trying to chat with an image-generation prompt"
  class="absolute inset-0 m-auto block max-h-full max-w-[70%] rounded-xl object-contain transition-opacity duration-500"
/>

<img
  v-click="1"
  src="./assets/prompt-shoe.png"
  alt="People trying to chat with an image-generation prompt"
  class="absolute inset-0 m-auto block max-h-full max-w-[90%] rounded-xl object-contain transition-opacity duration-500"
/>

<!--

People have been kind of trained to treat prompt boxes like this, like chat boxes, and expect the history to remember the things that they talked about in the past. They also expect that it should be able to access the internet to add more context, which none of that's true in our diffusion models.

On the opposite end of the spectrum, people write these really tiny prompts with no details and expect to get some kind of high quality output, which is unrealistic.
-->

---
---

<h2 className="!m-0 !p-0 uppercase">Too Many Models</h2>

<div class="flex flex-row gap-4">

![a](./assets/models-1.png)

![b](./assets/models-2.png)

![c](./assets/models-3.png)

</div>

<!-- It's overwheleming right, and each have different strengths and weaknesses that aren't obvious to our customers -->

---

<center>
  <img src="./assets/dont-make-me-think.jpg" class="h-120" />
</center>

<!--
We're breaking the golden rule of UX
-->

---

<center>
  <h2 class="uppercase text-shadow-lg"><span class="text-violet-200">People Want To Sell Their Own Stuff</span></h2>
  <img src="./assets/ai-slop-chocolate.jpg" class="h-80" />
<p class="!mb-4 text-sm font-bold tracking-[0.16em] text-green uppercase">Not your AI slop chocolate</p>

</center>
<!-- They don't want to sell your AI slop chocolate bars with random typos -->

---
layout: section

---

# Image Models are a Black Box

<center>
  <p class="text-green font-semibold w-180 !leading-tight p-0">People don't really understand how they work and have really no idea how to steer them in one direction or another.</p>

  <img src="./assets/dune-pain-box.avif" class="h-90" />
  <sub class="relative -top-6 !text-xs !p-0 text-slate-600">Image: Warner Bros. Pictures</sub>
</center>
---
layout: section

---

<p class="!mb-4 text-sm font-bold tracking-[0.16em] text-violet-300 uppercase">Part 2 · Thinking outside the prompt</p>

# A better interface for image generation

<!--
At stability I worked with a fantastic product manager Max who was constantly vibe coding different prototypes for how users could get better results out of our product.

Obviously, there are so many different ways we could think about solving this problem. I'm going to talk about just a few of the approaches that we took, what worked and didn't work about them, and how we ended up combining them all together into a feature that we call producer mode


Google combined autoregressive language-model behaviour with diffusion and other techniques. We were not Google and did not have many ML researchers, so Max proposed a complicated solution that lived mostly in the product workflow.
-->

---

# AI Prompt Enhancement

<ModelBadge
  v-click
  class="absolute right-7 bottom-6 z-20 w-88"
  :step="1"
  title="Enhance prompt"
  model-type="LLM"
  model="Mistral Small 4"
/>

<video src="./assets/prompt-enhance.mov" class="w-full" autoplay  controls></video>

<!--

 The first thing that we brought in was this AI prompt enhancement feature. Essentially, you type in your bad prompt, you click the little magic button, we send it to an LLM, and you get back a much more detailed prompt, which you can then edit before requesting your image.

It worked okay, but it definitely wasn't enough.
-->

---

# Auto-Model Selection

<ModelBadge
  v-click="2"
  class="absolute right-7 bottom-6 z-20 w-88"
  :step="2"
  title="Auto Model Selection"
  model-type="Decision Model"
  model="Jev, Clef or Decisions API"
  tone="current"
/>

<div v-click="1" class="absolute">

![d](./assets/auto-model.png)

</div>

<div v-click.hide="1" class="flex flex-row gap-4">

![a](./assets/models-1.png)

![b](./assets/models-2.png)

![c](./assets/models-3.png)

</div>

<!--
As you can see from this very long dropdown, there are a tonne of model choices that we offer in Brand Studio. And nobody ever wants to think about the best one for their pick.
-->

---

# CAN WE DO BETTER THAN TEXT?

<!--
Trying to describe an image to a diffusion model is kind of like those police sketches where you have to describe the features that you want them to draw and they do their best to interpret it
-->
<div class="flex flex-row gap-4">
  <figure>
    <img src="./assets/police-sketch.avif" class="h-80" />
    <figcaption><a class="text-xs" href="https://www.independent.co.uk/news/weird-news/worst-police-sketch-ever-police-hunt-oddly-spherical-man-9075276.html">The Independent: Worst police sketch ever? Police hunt oddly spherical man
  </a></figcaption>
  </figure>
  <figure>
  <div v-click.hide="1" class="absolute text-[2.5em] leading-[1.2] text-shadow-navy text-shadow-lg text-yellow">
  If you were looking for someone specific, what would be the quickest way to convey that information?
  </div>
    <img v-click="1" src="./assets/t2-have-you-seen.jpg" />
    <figcaption  v-click="1"  class="text-xs">Waynes World</figcaption>
  </figure>
</div>
---

<center>
<img v-click="1" src="./assets/categories-urban.png" class="absolute h-120"/>
<img src="./assets/board-urban.png" class="h-120"/>
</center>

<ModelBadge
  v-click="2"
  class="absolute right-7 bottom-6 z-20 w-88"
  :step="3"
  title="Describe Images"
  model-type="Vision LM (VLM)"
  model="Gemini 3.1 Flash-Lite"
/>

<!--
 Instead of expecting people to very carefully type the perfect description about what they want, why not just have them show you?

 And then we'll use a vision language model or a multi-modal VLM such as Gemini 3.1-flash lite to provide a description of that image.
-->

---

<div class="absolute inset-10 grid grid-cols-[1fr_28rem] items-center gap-14">
  <div>
    <p class="!mb-3 text-sm font-bold tracking-[0.2em] text-green uppercase">A Partial AI Workflow</p>
    <h1 class="!m-0 text-6xl leading-tight text-blue">Creative<br/>Guidance</h1>
    <p class="mt-6 max-w-md text-xl leading-relaxed text-slate-300">
      Use moodboard images to enhance the users image prompt and then pick
      the best model
    </p>
  </div>

  <div class="flex flex-col gap-4">
    <ModelBadge
      class="w-full"
      :step="1"
      title="Describe Moodboard Images"
      model-type="Vision LM (VLM)"
      model="Gemini 3.1 Flash-Lite"
    />
    <ModelBadge
      class="w-full"
      :step="2"
      title="Enhance prompt"
      model-type="LLM"
      model="Mistral Small 4"
    />
    <ModelBadge
      class="w-full"
      :step="3"
      title="Auto Model Selection"
      model-type="Decision Model"
      model="Jev, Clef or Decisions API"
    />
  </div>
</div>

<!--
So if you combine all those together (prompt enhancement, auto model, and auto model routing with the mood board), you get this feature that we call creative guidance.
-->

---

<div class="mt-4 flex justify-center items-center">
  <img src="./assets/prompt-boots.png" v-click.hide="1" class="absolute" />
  <img src="./assets/boots-urban.png" v-click="1" class="h-100"/>
</div>

<!--
 You can see here, even with a very simple prompt like a boot, you get a really detailed image of a pair of boots in a really dystopian urban scene.
-->

---

<div class="flex justify-evenly items-center">
  <img src="./assets/board-glacier.png" class="h-120" />
</div>

<!--
Here's another example of creative guidance. I put a couple of pictures of a glacier in the moodboard, and I got this really cool picture of boots hanging out by a frozen lake.
-->

---

<p v-click class="!mb-4 text-sm font-bold tracking-[0.16em] uppercase">OUR CUSTOMERS DON'T WANT TO SELL MY AI-GENERATED BOOTS</p>

<div class="mt-4 gap-4 flex justify-evenly items-center">
  <img src="./assets/boots-glacier.png" class="h-100" />
  <img src="./assets/chocolate-glacier.jpeg" v-click class="h-100" />
</div>

<!--
Prompt: Replace the boots in this image with a stack ION chocolate bars

They don'tw want to sell my AI generated boots, they want to sell their own products...
-->

---

<center>
<img src="./assets/board-fishing.png" class="h-120"/>
</center>

<!--
So we added some new capabilities to our moodboard.
-->

---

<center>
  <img src="./assets/board-categories.png" class="absolute h-120 left-24" />
</center>

---

<div v-click.hide="1" class="absolute  right-4 p-4 pl-8 gap-4 flex flex-col text-right w-100">
    <p class="!mb-3text-sm font-bold tracking-[0.2em] text-green uppercase">Product Insertion Loop</p>
    <p class="pl-4 mt-6 max-w-md text-xl leading-relaxed text-slate-300">
      Replace items in your base image with people and products from your moodboard.
    </p>
</div>

<center class="flex flex-row gap-4">
  <img src="./assets/prompt-fishing.png" class="absolute h-100 mt-4" />
  <img v-click="1" src="./assets/base-fishing.jpg" class="left-36 absolute h-120" />  
  <img
    v-click="2"
    v-click.hide="4"
    v-motion
    :initial="{ x: 0, y: 0, scale: 1 }"
    :click-3="{ x: 435, y: 65, scale: .25 }"
    src="./assets/item-me.png"
    class="absolute h70 z-20"
  />
  <img 
    v-click="4" src="./assets/fishing-me.jpg" class="left-36 absolute h-120" />  
  <img v-click="5" 
      v-motion
      :initial="{ x: 0, y: 0, scale: 1 }"
      :click-6="{ x: 330, y: 175, scale: 2 }"
      v-click.hide="7"
      src="./assets/item-canoe.png"
      class="left-36 absolute z-20 h-50" />
  <img v-click="7" src="./assets/fishing-all.png" class="left-36 absolute h-120" />
  <ModelBadge
  v-click="8"
  class="absolute right-7 bottom-6 z-20 w-88"
  :step="4"
  title="Insert Product(s)"
  model-type="Inpaint Model"
  model="Nano Banana Pro"
/>
</center>

<!--
For each image that was added, you could categorise it as either an inspiration image to impact the style of the OUTPUT or a product image to be INSERTED into the ouput.

So here i added a canoe, and I also have a picture of myself.

Along with a INSPIRATION image of loch ness
-->

---
transition: none

---

<div class="flex justify-center">
  <img src="./assets/fishing-fail.jpg" class="h-120 "/>
   <span class="absolute top-60 z-20" style="font-size: 138px;">❌</span>
</div>

<ModelBadge
  v-click
  class="absolute right-7 top-6 z-20 w-88"
  :step="5"
  title="Evaluate Realism"
  model-type="Multi-Modal LLM"
  model="Gemini Flash 3.8"
/>

---

<div class="absolute pr-4 grid grid-cols-[1fr_28rem] items-center gap-14">
  <div>
    <p class="!mb-3 text-sm font-bold tracking-[0.2em] text-green uppercase">Our Full AI Workflow</p>
    <h1 class="!m-0 text-6xl leading-tight text-blue">Producer<br/>Mode</h1>
    <p class="mt-6 max-w-md text-xl leading-relaxed text-slate-300">
      Use moodboard images to enhance the users image prompt and then pick
      the best model
    </p>
  </div>

  <div class="flex flex-col gap-4">
    <ModelBadge
      class="w-full"
      :step="1"
      title="Describe Moodboard Images"
      model-type="Vision LM (VLM)"
      model="Gemini 3.1 Flash-Lite"
    />
    <ModelBadge
      class="w-full"
      :step="2"
      title="Enhance prompt"
      model-type="LLM"
      model="Mistral Small 4"
    />
    <ModelBadge
      class="w-full"
      :step="3"
      title="Auto Model Selection"
      model-type="Decision Model"
      model="Jev, Clef or Decisions API"
    />
    <div class="rounded-xl border-2 border-dashed border-fuchsia p-4 gap-4 flex flex-col">
    <ModelBadge
      class="w-full"
      :step="4"
      title="Insert Product(s)"
      model-type="Inpaint Model"
      model="Nano Banana Pro"
    />
    <ModelBadge
      class="w-full"
      :step="5"
      title="Evaluate Realism"
      model-type="Multi-Modal LLM"
      model="Gemini Flash 3.8"
    />
    </div>
  </div>
</div>

<!--
So if you combine all those together (prompt enhancement, auto model, and auto model routing with the mood board), you get this feature that we call creative guidance.
-->

---

# PUTTING THE USER IN CONTROL

<center class="flex flex-row gap-4">
  <img src="./assets/prompt-fishing.png" class="h80"/>
  <img v-click src="./assets/approve.png" class="h-100"/>
</center>

<!--
And finally to get rid of the black box, the first thing we're going to do is show the
customers the image generation plan before we even start (and potentially allow them to change it) and secondly as the the images are generating,

-->

---
transition: none
---

<center>
  <img v-click  src="./assets/fishing-1.png" class="left-36 absolute h-120"/>  
  <img v-click src="./assets/fishing-2.png" class="left-36 absolute h-120"/>  
  <img v-click src="./assets/fishing-3.png" class="left-36 absolute h-120"/>
  <img src="./assets/placeholder.png" class="h-120"/>
</center>

---
transition: none
---

  <img src="./assets/fishing-bad.png" class="left-36 absolute h-120"/>

<!--

---

### We finally nailed the UX (more or less)

<div class="mt-8 p-4">

<ol>
  <li class="!list-decimal">Create a moodboard (style & product images)</li>
  <li class="!list-decimal">Type a simple prompt</li>
  <li class="!list-decimal">We generate the image for them:
      <p class="pl-4 !m-0">• Building the base image</p>
      <p class="pl-4 !m-0">• Inserting any product images</p>
      <p class="pl-4 !m-0">• Evaluating for realism</p>
  </li>
  <li class="!list-decimal">Users can see and modify steps in real-time</li>
</ol>

</div>
-->

---
layout: section
transition: none
---

<p class="!mb-4 text-sm font-bold tracking-[0.16em] text-violet-300 uppercase">Part 3 · Making the Workflow</p>

# Let's Build The Thing

Building an Agent is Fun (Sometimes)

<!--

Now for the fun bit, how did we build it?
-->

---

<div class="flex gap-20 justify-center">
  <img
    src="./assets/orly-cloudflare.png"
    alt="Brand Studio workflow"
    class="block w-85"
  />
</div>

<!--
Pretty much all the stuff that we're going to talk about today is built on top of Cloudflare, which is the service that we are already using for hosting our web stuff. There are tonnes of other agent frameworks that we could have used to have built this, but in our case this seemed like a good bet.
-->

---

# Cloudflare Concepts

<CloudflareRings />

<!--
This is a conceptual product stack, not a literal inheritance diagram.
Cloudflare Agents build on Durable Objects. Workflows provide durable execution for applications on Workers, and the Agents SDK can be used with Workflows. The CDN represents the global Cloudflare network beneath the application runtime.

Sources:
https://developers.cloudflare.com/agents/runtime/lifecycle/agent-class/
https://developers.cloudflare.com/workflows/
https://developers.cloudflare.com/workflows/get-started/durable-agents/
https://developers.cloudflare.com/reference-architecture/architectures/cdn/

CDN: Worldwide distributed edge network, nice for serving image files and caching requests to your website.

Workers: are very similar to AWS lambdas, except they run at the edge, so they're extremely low latency , and they're built using something called V8 isolates instead of containers so there are almost no cold start costs.

Almost everything that Cloudflare has built is built since then is built on top of Workers.

On of the most unique features of Cloudflare is called durable objects which builds on workers by giving each instance a unique name, a local database and a “hibernating” WebSocket connection that it can use to connect directly with clients.


Whereas Workers are a sort of a generic serverless compute platform, with durable objects you can assign every user or organization their own cute little dedicated stateful computing instance.


Durability means being able to recover after a crash or for example if the instance gets "evicted" because the data is stored in database, not in the workers memory.
-->

---

<div class="absolute inset-10 flex flex-col">
  <p class="!mb-2 text-sm font-bold tracking-[0.2em] text-green uppercase">The architecture</p>
  <h1 class="!m-0 text-5xl text-blue">Our Two Challenges</h1>
  <div class="mt-7 grid h-[19.5rem] grid-cols-2 gap-7">
    <div class="flex flex-col overflow-hidden rounded-3xl border border-teal/60 bg-[#15172a] shadow-xl shadow-black/30">
      <div class="flex min-h-0 flex-1 flex-col p-5">
        <div class="mb-3 flex h-10 w-10 items-center justify-center rounded-xl bg-teal/15 text-base font-bold !text-green">01</div>
        <p class="!m-0 text-xs font-bold tracking-[0.14em] text-slate-400 uppercase">Orchestration</p>
        <h2 class="!m-0 text-[1.6rem] leading-tight text-white">Model and durably execute our AI workflows</h2>
        <p class="!mb-0 !mt-2 text-sm leading-snug text-slate-300">Long-running steps, retries, and observability.</p>
      </div>
      <div v-click class="border-t border-teal/40 py-3 px-6">
        <span class="text-sm font-bold tracking-[0.16em] text-green uppercase">Cloudflare Workflows</span>
      </div>
    </div>
    <div class="flex flex-col overflow-hidden rounded-3xl border border-purple/70 bg-[#17152b] shadow-xl shadow-black/30">
      <div class="flex min-h-0 flex-1 flex-col p-5">
        <div class="mb-3 flex h-10 w-10 items-center justify-center rounded-xl bg-purple/20 text-base font-bold text-purple">02</div>
        <p class="!m-0 text-xs font-bold tracking-[0.14em] text-slate-400 uppercase">Communication</p>
        <h2 class="!m-0 text-[1.6rem] leading-tight text-white">Sync the workflow state with the browser</h2>
        <div class="!mb-0 !mt-2 text-sm leading-snug text-slate-300">The UI should always show the latest progress. Clients needs to be able to direct the workflow.</div>
      </div>
      <div v-click class="border-t border-purple/50 py-3 px-6">
        <span class="text-sm font-bold tracking-[0.16em] text-purple uppercase">Cloudflare Agents SDK</span>
      </div>
    </div>
  </div>
</div>

---
layout: section
transition: none
---

# Cloudflare Workflows

Modeling and Durably Executing our AI Workflows

<!--
Let's define this very quickly, and then we'll turn it into code.
-->

---
transition: none
---

<div class="flex flex-row gap-4">
    <img src="./assets/docs-workflows.png" class="h-120" />
    <div class="flex flex-col gap-4">
    <ModelBadge
        v-click
        class="z-20 w-72"
        :step="1"
        title="Fetch Image"
        model-type="Service"
        model="R2"
        />
    <ModelBadge
        v-click
        class="z-20 w-72"
        :step="2"
        title="Generate Description"
        model-type="VLM"
        model="LLaVA 1.5 7B"
    />
    <ModelBadge
        v-click
        class="z-20 w-72"
        :step="3"
        title="Await Human Approval"
        model-type="Interface"
        model="Dashboard"
    />
    <ModelBadge
        v-click
        class="z-20 w-72"
        :step="4"
        title="Publish Description"
        model-type="Service"
        model="R2"
    />
    </div>
</div>

---
transition: none
---

```ts {1-2,24-25|3-6|8-15|17-19|21-23}
export class ImageProcessingWorkflow extends WorkflowEntrypoint {
  async run(event: WorkflowEvent, step: WorkflowStep) {
    const imageData = await step.do("fetch image", async () => {
      const object = await this.env.BUCKET.get(event.payload.imageKey);
      return await object.arrayBuffer();
    });

    const description = await step.do("generate description", async () => {
      const imageArray = Array.from(new Uint8Array(imageData));
      return await this.env.AI.run("@cf/llava-hf/llava-1.5-7b-hf", {
        image: imageArray,
        prompt: "Describe this image in one sentence",
        max_tokens: 50,
      });
    });

    await step.waitForEvent("await approval", {
      event: "approved",
      timeout: "24 hours",
    });

    await step.do("publish", async () => {
      await this.env.BUCKET.put(`public/${event.payload.imageKey}`, imageData);
    });
  }
}
```

<!--
Sample Workflow Code
-->

---
transition: none
---


```ts{|3-5|6,7|9-13|15,16|18,19|21,22|24-25}
type TaskParams = { moodboardId: string, userPrompt: string; };

export class ImageProcessingWorkflow extends AgentWorkflow<MyAgent, TaskParams> {
  async run(event: AgentWorkflowEvent<TaskParams>, step: AgentWorkflowStep) {
  
    // grab user input
    const { moodboardId, userPrompt } = event.payload;

    // step 1
    const moodboard = await step.do("collect moodboard data", async () => {
      const moodbard = await getMoodboardById(moodboardId);
      return moodboard;
    }

    // step 2
    const newPrompt = await step.do("enhance prompt", () => { /* ... */ });

    // step 3
    const model = await step.do("select optimal model", () => { /* ... */ });

    // step 4
    const image = await step.do("generate image", () => { /* ... */ });

    // step 5
    await step.do("save to image library", () => { /* ... */ })
}
```
---

## Enhance Prompt Step Example

```ts
await step.do("enhance prompt", () => {
    const messages = [{
        role: "system",
        content: "You are an expert at writing prompts for diffusion image generation..." +
            "Be extremely clear and descriptive about the feautres of the image..." +
            "Please extend the users prompt to include details similar to the following examples:"
            moodboard.map(item =>  item.category + ":" + item.description) +
    },
    {
        role: "user",
        content: userPrompt", // <--- send in the users prompt as well
    }];
    const response = await this.env.AI.run("@cf/mistralai/mistral-small-3.1-24b-instruct", { messages });
    return response;
});
```
---
layout: none
---
<div class="absolute inset-0 flex flex-col">
  <div class="relative mt-3 min-h-0 flex-1">
  <div class="absolute inset-0 overflow-hidden">
    <img src="/assets/cf-workflows-dash.png" class="h-full w-full rounded-xl object-contain" />
  </div>
  <div v-click class="absolute inset-0 overflow-hidden">
    <img src="/assets/cf-workflows-dash-2.png" class="h-full w-full rounded-xl object-contain" />  </div>
  <div v-click class="absolute inset-0 overflow-hidden">
    <img src="/assets/cf-worfklows-dash-3.png" class="h-full w-full rounded-xl object-contain" />
  </div>
  </div>
</div>

---
layout: section
transition: none
---

# Cloudflare Agents SDK

Coordinate the work and keep us in sync

<!--
Let's define this very quickly, and then we'll turn it into code.
-->

---
transition: none
---

<center>
   <img src="./assets/cloudflare-agents.png" class="h-120" />
</center>

<!--
Cloudflare has got this entire choose your own adventure series of Agent-related infastructure we're only going taking advantage of a very small part of it.
-->

---
transition: none
---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Agents SDK Basics</h4>

<div class="flex flex-row gap-4 justify-start h-106">

<div class="w-1/2 h-full">
  <span class="text-sm pl-2">Agent Code</span>
  <div class="flex flex-col w-full h-full rounded bg-black p-2"  style="background-color: var(--slidev-code-background)">

```ts{|3,8|4-7}
import { Agent, callable } from "agents";

export class MyAgent extends Agent {
  @callable()
  async greet(name: string): Promise<string> {
    return `Hello, ${name}!`;
  }
}
```
  </div>
</div>

<div class="w-1/2 h-full">
  <span class="text-sm pl-2">Client Code</span>
  <div class="flex flex-col w-full h-full rounded bg-black p-2"  style="background-color: var(--slidev-code-background)">

```ts {|5-7|9-11}
import { useAgent } from "agents/react";
import type { MyAgent } from "./server";

function App() {
  const agent = useAgent<MyAgent, State>({
    agent: "my-agent",
    name: "user-123",
  });

  // Call methods on the agent
  const greeting = agent.stub.greet();
}
```
  </div>
</div>

</div>

<!--
The Agents SDK is used across the frontend and backend.

The hook itself is going to setup a web socket connection to the agent with
this particular name

You can make RPC calls by adding this decorator to your agent methods
-->

---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Managing Agent State</h4>

<div class="flex flex-row gap-4 justify-start h-106">

<div class="w-1/2 h-full">
  <span class="text-sm pl-2">Agent Code</span>
  <div class="flex flex-col w-full h-full rounded bg-black p-2"  style="background-color: var(--slidev-code-background)">


```ts{|3-5,7-10|11-19|13,18|}
import { Agent, callable } from "agents";

type MyState = {
    name: string;
}
export class MyAgent extends Agent {
	initialState: MyState = {
		name: "",
	};

	@callable()
    async setName(name: string): Promise<string> {
        this.setState({ name: name });
    }

	@callable()
	async greet(): Promise<string> {
		return `Hello, ${this.state.name}!`;
	}
}
```

</div>
</div>
<div class="w-1/2 h-full">
  <span class="text-sm pl-2">Client Code</span>
  <div class="flex flex-col w-full h-full rounded bg-black p-2"  style="background-color: var(--slidev-code-background)">

```ts{|10|12}
import { useAgent } from "agents/react";
import type { MyAgent } from "./server";

function App() {
  const agent = useAgent<MyAgent, State>({
    agent: "my-agent",
    name: "user-123",
  });

  agent.stub.setName("Jamund");

  agent.stub.greet(); // "Hello, Jamund"
}
```

</div>
</div>
</div>
<!--
Remembmer, Agents are built on-top of durable, so they have state, but the agents class makes it much easier to work with than durable objects.

-->



---


<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">State automatically flows between client and server</h4>

<div class="flex flex-row gap-4 justify-start h-106">

<div class="w-1/2 h-full">
  <span class="text-sm pl-2">Agent Code</span>
  <div class="flex flex-col w-full h-full rounded bg-black p-2"  style="background-color: var(--slidev-code-background)">


```ts{10-14}
import { Agent, callable } from "agents";

type MyState = {
    name: string;
}
export class MyAgent extends Agent {
	initialState: MyState = {
		name: "",
	};

	@callable()
    async setName(name: string): Promise<string> {
        this.setState({ name: name });
    }

	@callable()
	async greet(): Promise<string> {
		return `Hello, ${this.state.name}!`;
	}
}
```

</div>
</div>
<div class="w-1/2 h-full">
  <span class="text-sm pl-2">Client Code</span>
  <div class="flex flex-col w-full h-full rounded bg-black p-2"  style="background-color: var(--slidev-code-background)">

```ts{|8-10|13|9-10}
import { useAgent } from "agents/react";
import type { MyAgent } from "./server";

function App() {
  const agent = useAgent<MyAgent, State>({
    agent: "my-agent",
    name: "user-123",
    onStateChanged: (state) => {
      console.log("New state:", state);
    }
  });

  agent.stub.setName("Jamund");

  agent.stub.greet(); // "Hello, Jamund"
}
```

</div>
</div>
</div>
<!--
Remembmer, Agents are built on-top of durable, so they have state, but the agents class makes it much easier to work with than durable objects.

-->

---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Firing off a workflow</h4>


```ts{|9-15,18|1,3,8,16-17}
type MyState = {
    name: string;
    activeWorkflows: string[]
}
export class MyAgent extends Agent {
	initialState: MyState = {
		name: "",
        activeWorkflows: []
	};

    @callable()
    async start(moodboardId: string): Promise<string> {
        const id = await this.runWorkflow("IMAGE_GEN_WORKFLOW", {
            moodboardId
        });
        const activeWorkflows = this.state.activeWorkflows.concat(id);
        this.setState({ activeWorkflows })
        return { id };
    }

	// ...
}
```

---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Showing Our Progress</h4>


```ts{|5,9-11|14-16}
import { useAgent } from "agents/react";
import type { MyAgent } from "./server";

function App() {
  const [placeholders, setPlaceholders] = useState(); 
  const agent = useAgent<MyAgent, State>({
    agent: "my-agent",
    name: "user-123",
    onStateChanged: (state) => {
      setPlaceholders(state.placeholders);
    }
  });

  return placeholders.map(workflowId => {
    return <PlaceholderCard workflowId={workflowId} />
  })
}
```

<img src="/assets/placeholder.png" class="absolute top-12 -right-75" >

<!--
In real life we used a zustand store, but the pattern works well.
-->


---

```ts
type TaskParams = { moodboardId: string };

export class ProcessingWorkflow extends AgentWorkflow<ConductorAgent, TaskParams> {
  async run(event: AgentWorkflowEvent<TaskParams>, step: AgentWorkflowStep) {
    const { moodboardId } = event.payload;

    const moodboard = await step.do("collect moodboard data", async () => {
      const moodbard = await getMoodboardById(moodboardId);
      return moodboard
    }
    // await step.do("");
    // await step.do("");
    // await step.do("");
}
```


---

## Human decisions inside the workflow

- Accept
- Skip
- Retry

<!--
Accept and skip were straightforward. Retry required more creative modelling. Add the real code example and expand the retry path when ready.

-->

---

## Deterministic and probabilistic workflows

Does an agent need to choose every model, tool, and argument?

---

## Deterministic where possible

- Cheaper because a model does not decide every turn
- Easier to test because the possible paths are known
- A practical way to encode human taste
- Humans do not need to cede all control yet

---

## What the happy path leaves out

- Frontend state modelling
- Mapping workflow state into the UI
- Creative mistakes and edge cases
- Observability and failure modes

<!--
The happy-path agent code was roughly 100 lines for complex capabilities. Offer to discuss the omitted engineering details offline.

-->

---

# Part 4

## What happened next

---

## The product problem

One-shot prompts could not reliably create the marketing images users needed.

<!--
The team augmented human abilities with agentic ones while keeping final decisions with people.

-->

---

## We built something we were proud of

> **Image placeholder:** Beautiful Producer Mode marketing images

---

## We shipped it

🎉

---

## Two weeks later, the team shut down

New sign-ups stopped. We never learned whether we had solved the UX.

---

## Build something you can be proud of

Even if nobody ever sees it.

Build a feature worth talking about after you leave.

---

# Thank you, Athens 🇬🇷

---

## Appendix: problems and evals

1. The workflow cannot arbitrarily restart at earlier steps
2. Failed-step output is hidden; only the retry is visible
3. Combining image generation and evaluation weakens observability

<!--
These points appeared after the main ending in the original notes. Keep them for Q&A or a deeper technical version.
-->

---

![x](./assets/explainer-noise.png)
![x](./assets/explainer-clear.png)

<center>
<strong>Source:</strong> <a href="https://poloclub.github.io/diffusion-explainer/">https://poloclub.github.io/diffusion-explainer/</a>
</center>

<!--
Diffusion models learn by replacing pixels with static noise and learning to reverse that process while associating the image with its description. Starting with noise and text, the model guesses the missing pixels. The book analogy is crossing out words and characters, then asking someone to reconstruct the text.

-->

---

## Prompt detail matters

![x](./assets/explainer-diff2.png)

<center>
<strong>Source:</strong> <a href="https://poloclub.github.io/diffusion-explainer/">https://poloclub.github.io/diffusion-explainer/</a>
</center>

<!--
Here's another graphic from the same site demonstrating how important it is to layer details into the prompt. Slight changes in the wording or in the text can end up creating drastically different images.
-->

-->
