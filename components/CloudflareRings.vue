<template>
  <div
    class="cloudflare-rings"
    :class="{
      'has-focus': $slidev.nav.clicks >= 1 && $slidev.nav.clicks <= 4,
      'focus-cdn': $slidev.nav.clicks === 1,
      'focus-workers': $slidev.nav.clicks === 2,
      'focus-durable': $slidev.nav.clicks === 3,
      'focus-agents': $slidev.nav.clicks === 4,
    }"
    aria-label="Cloudflare platform layers"
  >
    <div class="click-stops" aria-hidden="true">
      <span v-click />
      <span v-click />
      <span v-click />
      <span v-click />
    </div>

    <div class="ring ring-agents" />
    <div class="ring ring-durable" />
    <div class="ring ring-workers" />
    <div class="ring ring-cdn" />

    <svg class="ring-labels" viewBox="0 0 1000 463" aria-hidden="true">
      <defs>
        <path id="agents-arc" d="M 72 448 A 428 398 0 0 1 928 448" />
        <path id="durable-arc" d="M 185 448 A 315 283 0 0 1 815 448" />
        <path id="workers-arc" d="M 320 448 A 180 169 0 0 1 680 448" />
        <path id="cdn-arc" d="M 420 442 A 80 46 0 0 1 580 442" />
      </defs>

      <text class="label label-agents" transform="translate(0 -2)">
        <textPath href="#agents-arc" startOffset="50%" text-anchor="middle">
          AGENTS SDK &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WORKFLOWS
        </textPath>
      </text>
      <text class="label label-durable" transform="translate(0 8)">
        <textPath href="#durable-arc" startOffset="50%" text-anchor="middle">
          DURABLE OBJECTS
        </textPath>
      </text>
      <text class="label label-workers" transform="translate(0 14)">
        <textPath href="#workers-arc" startOffset="50%" text-anchor="middle">
          WORKERS
        </textPath>
      </text>
      <text class="label label-cdn" transform="translate(0 17)">
        <textPath href="#cdn-arc" startOffset="50%" text-anchor="middle">
          CDN
        </textPath>
      </text>
    </svg>
  </div>
</template>

<style scoped>
.cloudflare-rings {
  position: relative;
  width: min(54rem, 96%);
  height: 25rem;
  margin: 0 auto;
  overflow: hidden;
  filter: drop-shadow(0 1.1rem 1.5rem rgb(0 0 0 / 0.24));
}

.ring {
  position: absolute;
  left: 50%;
  bottom: -0.15rem;
  transform: translateX(-50%);
  border: 1px solid rgb(255 255 255 / 0.16);
  border-radius: 50% 50% 0 0 / 100% 100% 0 0;
  box-shadow: inset 0 0.8rem 1.5rem rgb(255 255 255 / 0.07);
  transition: opacity 280ms ease, filter 280ms ease;
}

.ring-labels {
  position: absolute;
  inset: 0;
  z-index: 10;
  width: 100%;
  height: 100%;
  overflow: visible;
  pointer-events: none;
}

.label {
  fill: white;
  font-family: inherit;
  font-weight: 800;
  letter-spacing: 0.055em;
  filter: drop-shadow(0 2px 5px rgb(0 0 0 / 0.35));
  transition: opacity 280ms ease, filter 280ms ease;
}

.click-stops {
  position: absolute;
  width: 1px;
  height: 1px;
  overflow: hidden;
  opacity: 0;
  pointer-events: none;
}

.has-focus .ring,
.has-focus .label {
  opacity: 0.2;
  filter: saturate(0.45) brightness(0.75);
}

.focus-cdn .ring-cdn,
.focus-cdn .label-cdn,
.focus-workers .ring-cdn,
.focus-workers .label-cdn,
.focus-workers .ring-workers,
.focus-workers .label-workers,
.focus-durable .ring-cdn,
.focus-durable .label-cdn,
.focus-durable .ring-workers,
.focus-durable .label-workers,
.focus-durable .ring-durable,
.focus-durable .label-durable,
.focus-agents .ring-cdn,
.focus-agents .label-cdn,
.focus-agents .ring-workers,
.focus-agents .label-workers,
.focus-agents .ring-durable,
.focus-agents .label-durable,
.focus-agents .ring-agents,
.focus-agents .label-agents {
  opacity: 1;
  filter: saturate(1.08) brightness(1.12)
    drop-shadow(0 0 12px rgb(255 255 255 / 0.24));
}

.label-agents {
  font-size: 23px;
}

.label-durable {
  font-size: 23px;
}

.label-workers {
  font-size: 22px;
}

.label-cdn {
  font-size: 20px;
}

.ring-cdn {
  width: 13.7rem;
  height: 6.55rem;
  background: linear-gradient(135deg, #ea580c, #fb923c);
}

.ring-workers {
  width: 26.5rem;
  height: 12.7rem;
  background: linear-gradient(135deg, #f59e0b, #fbbf24);
}

.ring-durable {
  width: 39.25rem;
  height: 18.85rem;
  background: linear-gradient(135deg, #7c3aed, #a78bfa);
}

.ring-agents {
  width: 52rem;
  height: 25rem;
  background: linear-gradient(
    90deg,
    #37308a 0%,
    #37308a 50%,
    #4b3296 50%,
    #4b3296 100%
  );
}
</style>
