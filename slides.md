---
theme: "@ktym4a/slidev-theme-ktym4a"
title: Agentic Image Generation
info: |
  Jamund Ferguson, CityJS Athens 2026
  Organized from the preserved source notes in source-notes-verbatim.txt.
drawings:
  persist: false
transition: none
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
  src="./assets/prompt-shoe-2.png"
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

<div class="flex flex-row justify-center">
  <img src="./assets/dont-make-me-think.jpg" class="h-120" />
</div>

<!--
We're breaking the golden rule of UX
-->

---

<div class="flex flex-col items-center">
  <h2 class="uppercase text-shadow-lg"><span class="text-violet-200">People Want To Sell Their Own Stuff</span></h2>
  <img src="./assets/ai-slop-chocolate.jpg" class="h-80" />
  <p class="!mb-4 text-sm font-bold tracking-[0.16em] text-green uppercase">Not your AI slop chocolate</p>    
</div>
<!-- They don't want to sell your AI slop chocolate bars with random typos -->

---
layout: section

---

# AI MODELS ARE A BLACK BOX

<div class="flex flex-col items-center">
  <p class="text-green font-light w-180 !leading-tight p-0 text-shadow-sm text-shadow-black">People generally don't understand what they're doing or how to improve their output</p>

  <img src="./assets/dune-pain-box.avif" class="h-90" />
  <sub class="relative -top-6 !text-xs !p-0 text-slate-600">Image: Warner Bros. Pictures</sub>
</div>

---
layout: section

---

## Ask the Audience
How do we fix this?

<!--
Repeat it back to them, thank them and move on...
-->

---

### Some of our ideas

- Train a better model
- Build a ComfyUI workflow
- Add tutorials and onboarding instructions
- Add an MCP server and let Claude figure it out
- What about MCP UI?
- Or a chat-based UI?


<p v-click class="text-violet relative top-8">What was it!?</p>


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

# DON'T MAKE ME PROMPT

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
  If you were describing someone specific, what would be the quickest way to convey that information?
  </div>
    <img v-click="1" src="./assets/t2-have-you-seen.jpg" />
    <figcaption  v-click="1"  class="text-xs">Waynes World</figcaption>
  </figure>
</div>
---

<div class="flex flex-row justify-center">
    <img src="./assets/board-urban.png" class="h-120"/>
</div>

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

<div class="absolute inset-10 flex flex-row justify-between items-center">
  
  <div>
    <p class="!mb-3 text-sm font-bold tracking-[0.2em] text-green uppercase">A Partial AI Workflow</p>
    <h1 class="!m-0 text-6xl leading-tight text-blue">Creative<br/>Guidance</h1>
    <p class="mt-6 max-w-md text-xl leading-relaxed text-slate-300">
      Use moodboard images to enhance the users image prompt and then pick
      the best model
    </p>
  </div>

  <div class="flex flex-col gap-4 w-86">
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
  <img src="./assets/prompt-boots.png" v-click.hide="1" class="absolute top-30 w-200" />
  <img src="./assets/board-urban.png" v-click v-click.hide="2" class=" h-100" />
  <img src="./assets/boots-urban.png" v-click="2" class="absolute h-100"/>
</div>

<!--
 You can see here, even with a very simple prompt like a boot, you get a really detailed image of a pair of boots in a really dystopian urban scene.
-->

---
transition: none
---

<div class="mt-4 flex justify-center items-center gap-4">
  <img src="./assets/board-glacier.png" class=" h-100"/>
  <img src="./assets/boots-glacier.png" v-click class=" h-100" />
</div>


---

<p class="!mb-4 text-sm font-bold tracking-[0.16em] uppercase">MOODDBOARDS SOLVE A LOT OF PROBLEMS <span class="text-right text-violet" v-click="1">BUT THEY DONT SHOW US THE USERS PRODUCTS</span>
</p>

<div class="mt-4 gap-4 flex justify-evenly items-center">
  <div>
    <img src="./assets/boots-glacier.png" class="h-100" />
  </div>
  <div>
    <img src="./assets/chocolate-glacier.jpeg" v-click="1" class="h-100" />
  </div>
</div>

<!--
Prompt: Replace the boots in this image with a stack ION chocolate bars

They don'tw want to sell my AI generated boots, they want to sell their own products...

YET
-->

---

<div class="flex flex-row justify-center">
  <img src="./assets/board-products.png" class="absolute h-125"/>
  <img src="./assets/board-guidance.png" v-click class="absolute z-20 top-25 h-80"/>
</div>


<!--
So we added some new capabilities to our moodboard.
-->

<!--
<center>
  <img src="./assets/board-categories.png" class="absolute h-120 left-24" />
</center>

-->

---
  
<div class="absolute right-4 p-4 pl-8 gap-4 flex flex-col text-right w-100">
    <p class="!mb-3text-sm font-bold tracking-[0.2em] text-green uppercase">Product Insertion Loop</p>
    <p class="pl-4 mt-6 max-w-md text-xl leading-relaxed text-slate-300">
      Replace items in your base image with people and products from your moodboard.
    </p>
</div>

<img src="./assets/prompt-fishing.png" class="absolute h-100 mt-4" />


---

<div class="relative -top-20">
<img src="./assets/canoe-me-1.png" class="absolute" />
<img src="./assets/item-me.png" v-click class="absolute h-40 top-50 z-10" />
<img src="./assets/canoe-me-2.png" v-click class="absolute"/>
<img src="./assets/canoe-me-3.png" v-click class="absolute"/>
<img src="./assets/item-canoe.png" v-click class="absolute h-40 top-50 z-20" />
<img src="./assets/canoe-me-4-fail.png" v-click class="absolute" />
<img src="./assets/canoe-me-4-ok.png" v-click class="absolute z-20" />
<ModelBadge
  v-click
  class="absolute right-7 top-20 z-50 w-88"
  :step="4"
  title="Insert Product(s)"
  model-type="Inpaint Model"
  model="Nano Banana Pro"
/>
<ModelBadge
  v-click
  class="absolute right-7 top-42 z-50 w-88"
  :step="5"
  title="Evaluate Realism"
  model-type="Multi-Modal LLM"
  model="Gemini Flash 3.8"
/>


</div>

---

<div class="absolute pr-4 grid grid-cols-[1fr_28rem] items-center gap-14">
  <div>
    <p class="!mb-3 text-sm font-bold tracking-[0.2em] text-green uppercase">Our Full AI Workflow</p>
    <h1 class="!m-0 text-6xl leading-tight text-blue">Producer<br/>Mode</h1>
    <p class="mt-6 max-w-md text-xl leading-relaxed text-slate-300">
      Use moodboard images to enhance the users prompt. Pick
      the best model. Loop through product images and replace
      AI versions with the real thing. Evaluate for realism.
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



<div v-click.hide class="absolute -top-4">
  <h3>So basically took mooboards, which everyone knows well...</h3>
</div>

<div v-click="1" class="absolute -top-4">
  
  ### And combined it with this plan and execute UI pattern

<div class="grid grid-cols-2 grid-rows-2 gap-3">
  <img src="./assets/ui-plan-claude.png"  class="w-80" />
  <img src="./assets/ui-plan-copilot-kit.png"  class="w-80 let-30" />
  <img src="./assets/ui-plan-generic.png" class="w-60 relative -top-26" />
  <img src="./assets/ui-plan-cursor.png"  class="w-80 -top-40 left-60" />
</div>

</div>

<img src="./assets/board-urban.png" class="h-100 mt-12" v-click.hide="1" />

<!--

READ WORDs thEN:
that is becoming more common with agentic experiences


WHAT WE DIDNT bUILt WAS CHAT
-->

---

## Featuring:

<div class="flex flex-row justify-between">
  <div class="text-green text-sm">An Approval Screen
  <img src="./assets/approve.png" class="h-75" /></div>
  <div class="text-green text-sm">A Progress Screen
  <img src="./assets/plan-image.png" class="h-75" /></div>
  <div class="text-green text-sm">A Built-in Eval
  <img src="./assets/planner.jpg" class="h-75" /></div>
</div>

---
layout: section
transition: none
---

<p class="!mb-4 text-sm font-bold tracking-[0.16em] text-violet-300 uppercase">Part 3 · Making the Workflow</p>

# Let's Build The Thing

How hard can it be?

<!--

Now for the fun bit, how did we build it?
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
        <p class="!m-0 text-xs font-bold tracking-[0.14em] text-slate-400 uppercase">Coordination</p>
        <h2 class="!m-0 text-[1.6rem] leading-tight text-white">Execute AI workflows and keep the users in sync</h2>
        <div class="!mb-0 !mt-2 text-sm leading-snug text-slate-300">Help us pick the right AI tools to use. Run them and keep the client aware of progress and problems.</div>
      </div>
      <div v-click class="border-t border-purple/50 py-3 px-6">
        <span class="text-sm font-bold tracking-[0.16em] text-purple uppercase">Cloudflare Agents SDK</span>
      </div>
    </div>
  </div>
</div>

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

<div class="flex flex-row gap-4 items-end">
<div>
  <h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Workflows let you specify pre-determined <br/>places your AI can go.</h4>
  <img src="./assets/athens-metro-cropped.png" class="h-100">
</div>
<div  v-click="1" class="w-1/2">
    <h4 class="text-violet !text-md mb-3 w-full text-shadow-lg " v-click="1">Agents are tour guides</h4>
    <img src="./assets/athens-tour-guide-cf.jpg" class="h-100">
</div>
</div>

<!--
  <li>Know about all the routes you could take</li>
  <li>Recommend correct route for each person</li>
  <li>Constantly in communication with everyone</li>            
-->

---

<h2><span class="!text-violet">WE CAN BUILD THIS WITH JUST A FEW FILES</span></h2>

<div class="flex flex-row w-full gap-8">

  <div class="flex-1 bg-black/10 h-full rounded-lg p-2">

  ### Client Code
  App.tsx

  </div>

  <div class="flex-1 bg-black/10 h-full rounded-lg p-2">

  ### Agent Code
  producer-agent.ts

  </div>

  <div class="flex-1 bg-black/10 h-full rounded-lg p-2">

  ### Workflow Code
  creative-guidance.ts
  producer-mode.ts

  </div>

  <div class="flex-1 bg-black/10 h-full rounded-lg p-2">

  ### Config
  wrangler.toml

  </div>

</div>

---
transition: none
---

<SystemArchitecture />


---

<h3 class="relative -top-9">The Full Agent Experience</h3>

<div class="flex flex-row justify-between relative -top-9">
  <div  class="flex flex-col gap-3">
    <div>
      <code class="text-green">agent.stub.createPlan()</code>
      <img src="./assets/prompt-fishing.png" class="h-50" />
    </div>
    <div>
      <code class="text-green">agent.stub.approvePlan()</code>
      <img src="./assets/approve.png" class="h-75" />
    </div>
  </div>
  <div>
     <code class="text-green">onStateUpdate()</code>
    <img src="./assets/plan-image.png" class="h-75" />
  </div>
  <div>
    <code class="text-green absolute right-0 -top-1">agent.stub.approveStep()</code>
    <img src="./assets/planner.jpg" class="h-75 mt-8" />
  </div>
</div>

---
transition: none
---

```ts {1-2,24-25|3-6|8-15|17-19|21-23}
export class ImageProcessingWorkflow extends AgentWorkflow {
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

Transition: So how do you define a workflow? It's basically infrastructure as code.

Here's a sample from the cloudflare docs that relates pretty heavily to what we're trying to do with producer.

Simple. Series of steps.
-->

---


<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Step-By-Step Progress Updates (Workflow)</h4>

```ts{|2-5|2-5,12-15|22-23}
await step.do("enhance prompt", async ({ step: { name }}) => {
    await this.reportProgress({
        step: name,
        status: "active"
    });
   
    const response = await this.env.AI.run(
        "@cf/mistralai/mistral-small-3.1-24b-instruct",
        { messages }
    );
    
    await this.reportProgress({
        step: name,
        status: "completed"
    });

    return response;

 });


// only runs once
await step.reportComplete(image);

```

<!--

If you want to add progress updates, that's easy too.

-->


---
transition: none
---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Product Insertion Loop Example</h4>

```ts{|3,23|5,13|15,22}
let baseImage = image;
// loop over each category in the moodboard
for (const category in moodboard.categories) {
    // generate an image by replacing an item in the image with the moodboard image
    const activeImage = await step.do(`product insertion (${category.name})`, () => {
        const prompt = `We are in the process of making an image with the following description:
            ${enhancedPrompt}
            Replace the attached picture (2nd attachment) into the base image (1st attachment),
            replacing the ai generated version of this ${category.name} with the provided image.`
        const imageInput = [baseImage, category.image];
        const image = await this.env.AI.run("@google/nano-banana-2", { prompt, image_input });
        return image;
    });
    // evaluate for realism and override the base image if it passes
    const thing = await step.do(`evaluate image (${category.name})`, () => {
        const messages = ["Evalate this picture for realism on a scale of 0-1"];
        const result = await this.env.AI.run("@google/gemini-3.1-lite", { messages, attachments: [activeImage] });
        if (result.quality > .8) {
            baseImage = activeImage;
        } // else skip, log, ask for human input, etc...
        return result;
    });
}

```

<!--

Setting up loops is easy as well.
-->

---
layout: none
---

<div class="absolute inset-0 flex flex-col">
  <div class="relative mt-3 min-h-0 flex-1">
  <div  class="absolute inset-0 overflow-hidden">
    <img src="/assets/cf-workflows-dash-2.png" class="h-full w-full rounded-xl object-contain" />  </div>
  <div v-click class="absolute inset-0 overflow-hidden">
    <img src="/assets/cf-worfklows-dash-3.png" class="h-full w-full rounded-xl object-contain" />
  </div>
  </div>
</div>

---

### Critical Things About Agents on Cloudflare

<div class="flex flex-row">

<div class="w-1/2">
    <img src="./assets/athens-tour-guide-cf.jpg" class="h-100">
</div>
  <div class="w-1/2 rounded-xl p-4 bg-black/20">
    <ul class="text-white/80">
      <li>Clients can connect to them directly via websocket</li>
      <li>They help run AI workflows, workers, sub-agents, codemode, chat, etc.</li>
      <li>They have persistent state backed by SQL storage</li>
      <li>Any state changes made in the agent are also sent to the client</li>
    </ul>
</div>
</div>
<!--
Remembmer, Agents are built on-top of durable, so they have state, but the agents SDK makes it a lot easier to work with than durable objects.

-->

---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Firing off a workflow and keeping it in state</h4>


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
    async start(moodboardId: string, prompt: string): Promise<string> {
        const id = await this.runWorkflow("CREATIVE_GUIDANCE", {
            moodboardId, prompt
        });
        const activeWorkflows = this.state.activeWorkflows.concat(id);
        this.setState({ activeWorkflows })
        return { id };
    }

	// ...
}
```
<!--

creative guidance workflow (As defined in our config file)
And then we can use the setState function to send the in progress workflows down to the client
-->

---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Showing Active Workflows</h4>


```ts{|6-8,12|14,17|5,9-11|5,9-11,18-20}
import { useAgent } from "agents/react";
import type { MyAgent } from "./server";

function App() {
  const [placeholders, setPlaceholders] = useState(); 
  const agent = useAgent<MyAgent, State>({
    agent: "my-agent", // /agents/my-agent
    name: "user-123", // channel name
    onStateChanged: (state) => {
      setPlaceholders(state.activeWorkflows);
    }
  });

  const start = async () => await agent.stub.start();

  return (<>
    <button onClick={start}>Start Generation</button>
    {placeholders.map(workflowId => (
        <PlaceholderCard workflowId={workflowId} />
    ))}
  </>);
}
```

<img src="/assets/placeholder.png" class="absolute top-12 -right-75" v-click >

<!--
So first we setup a persistent weboscket connection here with the useAgent() hook and then secondly we listen for
any state chang events on teh server...
-->



---

<h4 class="text-violet !text-md mb-3 w-full text-shadow-lg">Step-By-Step Progress Updates (Agent)</h4>

```ts{|1-3,6|9,18-19,23|10-13|14-17|20-22}
type StepStatus = Record<StepName, "pending" | "active" | "completed">;
// { "enhance prompt": "completed", "generating image": "pending" }
type MyState = { activeWorkflows: Record<WorkflowId, StepStatus> }
export class MyAgent extends Agent {
	initialState: MyState = {
        activeWorkflows: {} 
	};

  async onWorkflowProgress(workflowName: string, instanceId: string, progress: MyState["activeWorkflows"]) {
    const steps = {
        ...this.state.activeWorkflows[instanceId],
        [progress.step]: progress.status,
    };
    this.setState({
        ...this.state.activeWorkflows,
        [instanceId]: steps
    });
  }
  async onWorkflowComplete(workflowName: string, instanceId: string, result?: GeneratedImage) {
    const activeWorkflows = activeWorkflows
      .filter(id => id === instanceId);
    this.setState({ activeWorkflows })
  }
}
```



---
layout: center
---

<h1 class="uppercase bg-white"><span class="text-black font-bold text-shadow-red text-shadow-lg">🕷️ Building agents does not have to be scary 👻</span></h1>

---
layout: center

---

<h1><span class="text-green">PICK A TOOL</span></h1>
<h1 v-click>BE CREATIVE</h1>
<h1 v-click>HAVE FUN</h1>
<h1 v-click>INVENT THE FUTURE</h1>

<h3 v-click><span class="!text-violet">THE END</span></h3>

<!--

We didn't realy solve agentic UX for image generation here. But we took a stab at it. And I had a lot of fun.

The web is changing. Alright.

 The last big era we entered was the mobile web. almost 20 years ago.

We don't know yet what this new era is going to look like.

But it's up to US as developers to figure it out!

So my advice to you for building agents:
1. Be creative
2. Have fun
3. Invent the future

-->


---



---

EXTRAS

---

Rant about evals


---
<div class="flex gap-20 justify-evenly">
  <img
    src="./assets/orly-cloudflare.png"
    alt="Brand Studio workflow"
    class="block w-85"
  />
  <div class="flex-1">
    <h2>Cloudflare is a Great Place to your Build Agents</h2>
    <ul>
      <li>Durable objects are a great primitive<br/>for building agents</li>
      <li>The Agents SDK cleans a lot of the rough edges (scheduling, state management, routing, etc.)</li>
      <li>Cloudflare also provides frameworks for building chat-based app with tool-calling, code-mode nad MCP</li>
      <li>Other providers and frameworks exist (Vercel, Mastra, Flue, Eve, etc.) that may suit your use case</li>
      <li class="text-violet">The main thing is learn one and build something</li>
    </ul>
  </div>
</div>

<!--
If you're already using cloudflare these tools make it really easy to build and experiment with agentic experiences such as these, but otherwise there are a ton of other tool and frameworks to choose from.
-->
