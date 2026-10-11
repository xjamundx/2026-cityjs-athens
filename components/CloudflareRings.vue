<template>
  <div class="cloudflare-rings" aria-label="Cloudflare platform layers">
    <div class="ring ring-agents" />
    <div class="ring ring-durable" />
    <div class="ring ring-workers" />
    <div class="ring ring-cdn" />

    <svg class="ring-labels" viewBox="0 0 1000 463" aria-hidden="true">
      <defs>
        <path id="agents-arc" d="M 72 448 A 428 414 0 0 1 928 448" />
        <path id="durable-arc" d="M 170 448 A 330 301 0 0 1 830 448" />
        <path id="workers-arc" d="M 278 448 A 222 203 0 0 1 722 448" />
        <path id="cdn-arc" d="M 376 430 A 124 96 0 0 1 624 430" />
      </defs>

      <text class="label label-agents" transform="translate(0 22)">
        <textPath href="#agents-arc" startOffset="50%" text-anchor="middle">
          AGENTS SDK &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; WORKFLOWS
        </textPath>
      </text>
      <text class="label label-durable" transform="translate(0 12)">
        <textPath href="#durable-arc" startOffset="50%" text-anchor="middle">
          DURABLE OBJECTS
        </textPath>
      </text>
      <text class="label label-workers" transform="translate(0 18)">
        <textPath href="#workers-arc" startOffset="50%" text-anchor="middle">
          WORKERS
        </textPath>
      </text>
      <text class="label label-cdn" transform="translate(0 61)">
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
  width: 19rem;
  height: 9.1rem;
  background: linear-gradient(135deg, #ea580c, #fb923c);
}

.ring-workers {
  width: 30rem;
  height: 14.4rem;
  background: linear-gradient(135deg, #f59e0b, #fbbf24);
}

.ring-durable {
  width: 41rem;
  height: 19.7rem;
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
