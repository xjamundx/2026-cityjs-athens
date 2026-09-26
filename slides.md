---
theme: default
title: Agentic Image Generation
info: |
  Jamund Ferguson, CityJS Athens 2026
  Organized from the preserved source notes in source-notes-verbatim.txt.
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Agentic Image Generation

From prompt to production workflow

Jamund Ferguson · CityJS Athens 2026

<!--
Hi, I'm Jamund Ferguson. I'm currently at Axle Energy, but I spent the last two years at Stability AI leading the front-end team and building interfaces for image generation.

-->

---

## The story

1. Diffusion models
2. Make it Work(flow)
3. Agents
4. What happened next

---

# Part 1
## Diffusion models

---

## Brand Studio starts with a text box

![Brand Studio screenshot](images/brand-studio-placeholder.png)

<!--
Brand Studio helps brands generate marketing images. The interface puts a large text box in front of people and asks them to describe the image they want.

-->

---

## A text box looks like chat

- People expect a conversation
- The model receives a one-way prompt
- Questions and instructions may become parts of the image

<!--
ChatGPT and Claude trained people to treat a big text box as a conversation. The logs showed many people trying to talk to a robot that could not understand them in that way.

-->

---

## How diffusion works

![Noise being added and removed](images/diffusion-placeholder.png)

<!--
Diffusion models learn by replacing pixels with static noise and learning to reverse that process while associating the image with its description. Starting with noise and text, the model guesses the missing pixels. The book analogy is crossing out words and characters, then asking someone to reconstruct the text.

-->

---

## Prompt detail matters

| Prompt | Likely result |
| --- | --- |
| “A cat” | Generic |
| “A shoe” | Generic |
| A detailed visual description | More directed |

---

## Dialogue confuses diffusion models

![Bad dialogue-as-image example](images/dialogue-failure-placeholder.png)

<!--
Diffusion models may try to render the wording of a question or instruction rather than respond to it.

-->

---

## Hybrid models changed the interaction

Nano Banana can reason about a request before rendering the image.

<!--
Google combined autoregressive language-model behaviour with diffusion and other techniques. We were not Google and did not have many ML researchers, so our product manager Max proposed a complicated solution that lived mostly in the product workflow.

-->

---

## A better interface for image generation

This is where the talk gets technical.

---

# Part 2
## Make it Work(flow)

---

## Step 1: Enhance the prompt

```text
[user prompt]
      |
      v
[Mistral Small 4]
      |
      v
[enhanced image prompt]
```

<!--
If someone types “a shoe,” convert it into something more detailed. It might not be the exact shoe they wanted, but it adds flair, teaches better prompting, and converts conversational input into an image description.

-->

---

## Step 2: Choose the best model

```text
[prompt + model options]
           |
           v
         [Jev]
           |
           v
      [best model]
```

<!--
Cartoons and hyperreal images may need different models. Even Stability's own family offered many options. The notes call the routing model Jev, and once call it Jed; verify the final product name before presenting.

-->

---

## People struggle to describe what they see

Police sketches and charades are hard for the same reason.

![Police sketch or charades reference](images/description-problem-placeholder.png)

---

## Showing beats describing

![Visual reference](images/showing-reference-placeholder.png)

---

## Moodboards

Users collect a scene, style, or vibe and label what matters.

![Moodboard examples](images/moodboards-placeholder.png)

---

## Moodboard processing

1. Add images
2. Categorise background, subject, or another role
3. Run each image through a VLM such as Gemini Flash 3.8 Lite
4. Save the descriptions
5. Inject descriptions before prompt enhancement
6. Generate from the prompt and visual guidance

<!--
At generation time, the workflow pulls the descriptions rather than the original images. The result mixes the user's text with the moodboard guidance.

-->

---

## Marketing needs real products and people

Good base images still fail when a company needs its own product, person, prop, or place.

---

## Product insertion

```text
[base image] + [product / person / prop / place] = [result]
```

<!--
The team added product-like categories to moodboards and used an inpainting model to insert every tagged item into the base image.

-->

---

## Two new problems

1. The workflow takes a long time
2. The output risks becoming AI slop

---

## Progress updates

The user needs to see where they are in the multi-step process.

---

## Slop checker

- A VLM produces a slop score
- A high score sends the result to a person
- The person can accept, retry, discard, or skip the insertion

<!--
The human remains the final decision maker. The exact choices in the notes are: accept it, throw it away and start over, or use the previous image and skip the step.

-->

---

## ComfyUI was the obvious image tool

ComfyUI models image manipulation as a graph of nodes, inputs, and outputs stored as JSON.

<!--
Stability's applied AI team relied on ComfyUI. It is excellent, but it felt like a black box for a product that needed constant progress communication with the client.

-->

---

# Part 3
## Agents

---

## Interactivity without chat

Most AI SDKs assume the product is a chatbot. This product needed an interactive workflow instead.

<!--
The wider option set included LangChain, Mastra, Cloudflare Agents SDK, CodeMode, Workflows, Dynamic Workflows, Vercel tools, Vercel AI SDK, TanStack AI, CrewAI, and others. The exact current names should be checked before the talk.

-->

---

## Two architecture problems

1. Arrange and execute a multi-step image workflow
2. Synchronise progress and feedback between the agent and browser

---

## Early attempt: LangChain

- Awkward fit with Cloudflare's serverless environment
- No native client state synchronisation
- Large learning surface and a lot of code
- More capable than we needed, but still a valid choice

<!--
LangChain remained compelling, especially if chat might be added later. The criticism is about fit and complexity, not that the framework is bad.

-->

---

## Cloudflare Workers

Temporary compute close to the request.

<!--
Workers resemble AWS Lambda but run on Cloudflare's edge network. They use V8 isolates rather than containers, which reduces cold-start overhead. Workers can communicate with one another.

-->

---

## Durable Objects

A named, stateful compute instance with local storage and a hibernating WebSocket.

<!--
Each user or organisation can have a dedicated stateful instance. It sleeps when unused and retains its data when it wakes. This became the primitive for Cloudflare's agent framework.

-->

---

## Cloudflare Agents SDK

```ts
this.setState({
  status: "running",
  progress: 0.4,
})
```

<!--
setState is backed by SQL. A change on the server or frontend immediately notifies the other side over WebSocket. A React hook establishes the connection, and decorators expose backend methods to clients. This solves client/server communication with little custom work.

-->

---

## Cloudflare Workflows

```ts
await step.do("enhance prompt", enhancePrompt)
await step.do("route model", routeModel)
await step.do("generate image", generateImage)
```

<!--
Workflows model durable multi-step processes and provide observability for each step. Add a dashboard screenshot, the full happy-path pipeline, and the trickier wait-for-response example later.

-->

---

## Agents plus workflows

An agent workflow communicates progress from the workflow to the agent, then immediately to the browser.

![Agent, workflow, and browser diagram](images/agent-workflow-placeholder.png)

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

![Beautiful Producer Mode marketing images](images/final-results-placeholder.png)

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
