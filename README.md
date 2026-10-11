# Agentic Image Generation

**[Watch the slides →](https://xjamundx.github.io/2026-cityjs-athens/)**

This talk by Jamund Ferguson explores what image-generation products look like when we move beyond a single prompt box and treat creation as a multi-step, agentic workflow. Drawing on Jamund’s experience building Brand Studio at Stability AI, it follows a campaign image from prompt enhancement and model routing through moodboard guidance, generation, image insertion, evaluation, and human quality gates.

Along the way, the talk examines where conversational interfaces help, where purpose-built interfaces remain essential, and how durable workflows can make AI systems understandable and controllable. The examples use Cloudflare Workflows and Workers AI-style APIs to show how the individual model calls and human decisions can fit together in a real application.

The presentation was created for **CityJS Athens 2026** and is built with [Slidev](https://sli.dev/).

## Run locally

You’ll need a recent version of Node.js and npm.

```sh
git clone https://github.com/xjamundx/2026-cityjs-athens.git
cd 2026-cityjs-athens
npm ci
npm run dev
```

Slidev will open the presentation in your browser. Edit `slides.md` to update the deck; changes appear in the preview as you save.

## Build or export

Create the static production build:

```sh
npm run build
```

Export the deck using Slidev’s export tools:

```sh
npm run export
```

The original talk notes are preserved verbatim in `source-notes-verbatim.txt`.
