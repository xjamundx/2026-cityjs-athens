<script setup lang="ts">
defineProps<{
  active: number
}>()

const steps = [
  'Enhance prompt',
  'Choose model',
  'Add moodboard',
  'Process references',
  'Generate base image',
  'Insert branded assets',
  'Evaluate and review',
]
</script>

<template>
  <div class="workflow-shell">
    <div class="workflow-label">IMAGE WORKFLOW</div>

    <div class="workflow-canvas">
      <template v-for="(step, index) in steps" :key="step">
        <div
          v-if="index > 0"
          class="connector"
          :class="{
            complete: index < active,
            'current-link': index + 1 === active,
          }"
        />

        <div
          class="workflow-node"
          :class="{
            complete: index + 1 < active,
            current: index + 1 === active,
            future: index + 1 > active,
          }"
        >
          <span class="step-number">{{ index + 1 }}</span>
          <span class="step-copy">
            <small>Step {{ index + 1 }}</small>
            <strong>{{ step }}</strong>
          </span>
          <span class="status-dot" />
        </div>
      </template>

      <div v-if="active >= 7" class="review-options">
        <span>Accept</span>
        <span>Retry</span>
        <span>Skip</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.workflow-shell {
  height: 100%;
  min-width: 0;
  padding: 0.75rem 1rem;
  display: flex;
  flex-direction: column;
  border: 1px solid rgb(168 139 250 / 38%);
  border-radius: 1rem;
  background:
    radial-gradient(circle at 1px 1px, rgb(196 181 253 / 18%) 1px, transparent 0) 0 0 / 16px 16px,
    linear-gradient(155deg, rgb(20 16 35 / 96%), rgb(8 8 18 / 96%));
  box-shadow: inset 0 0 2rem rgb(124 58 237 / 8%);
  overflow: hidden;
}

.workflow-label {
  margin-bottom: 0.45rem;
  color: rgb(196 181 253 / 78%);
  font-size: 0.62rem;
  font-weight: 700;
  letter-spacing: 0.16em;
}

.workflow-canvas {
  flex: 1;
  min-height: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.workflow-node {
  width: min(100%, 20rem);
  min-height: 2.15rem;
  padding: 0.32rem 0.58rem;
  display: grid;
  grid-template-columns: 1.5rem 1fr 0.7rem;
  align-items: center;
  gap: 0.48rem;
  border: 1px solid rgb(148 163 184 / 25%);
  border-radius: 0.58rem;
  background: rgb(22 22 38 / 94%);
  color: rgb(241 245 249);
  transition: opacity 200ms ease, border-color 200ms ease, box-shadow 200ms ease;
}

.workflow-node.complete {
  border-color: rgb(52 211 153 / 42%);
}

.workflow-node.current {
  border-color: rgb(196 181 253 / 92%);
  background: linear-gradient(100deg, rgb(76 29 149 / 62%), rgb(30 27 75 / 92%));
  box-shadow: 0 0 1.25rem rgb(139 92 246 / 28%);
  animation: node-arrive 420ms cubic-bezier(0.2, 0.8, 0.2, 1) both;
}

.workflow-node.future {
  opacity: 0;
  visibility: hidden;
}

.step-number {
  width: 1.5rem;
  height: 1.5rem;
  display: grid;
  place-items: center;
  border-radius: 0.42rem;
  background: rgb(139 92 246 / 24%);
  color: rgb(221 214 254);
  font-size: 0.7rem;
  font-weight: 800;
}

.step-copy {
  min-width: 0;
  display: flex;
  flex-direction: column;
  line-height: 1.05;
}

.step-copy small {
  color: rgb(148 163 184);
  font-size: 0.5rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.step-copy strong {
  margin-top: 0.08rem;
  font-size: 0.7rem;
  font-weight: 650;
  white-space: nowrap;
}

.status-dot {
  width: 0.48rem;
  height: 0.48rem;
  justify-self: end;
  border-radius: 999px;
  background: rgb(100 116 139 / 48%);
}

.complete .status-dot {
  background: rgb(52 211 153);
  box-shadow: 0 0 0.6rem rgb(52 211 153 / 45%);
}

.current .status-dot {
  background: rgb(216 180 254);
  box-shadow: 0 0 0.7rem rgb(192 132 252 / 70%);
}

.connector {
  width: 1px;
  height: 0.32rem;
  background: transparent;
}

.connector.complete {
  background: rgb(52 211 153 / 65%);
}

.connector.current-link {
  transform-origin: top;
  animation: connector-grow 300ms ease-out both;
}

.review-options {
  width: min(100%, 19rem);
  margin-top: 0.35rem;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 0.32rem;
  animation: options-arrive 420ms 180ms ease-out both;
}

.review-options span {
  padding: 0.22rem 0.35rem;
  border: 1px solid rgb(196 181 253 / 34%);
  border-radius: 999px;
  color: rgb(221 214 254);
  background: rgb(76 29 149 / 22%);
  font-size: 0.52rem;
  font-weight: 650;
  text-align: center;
}

@keyframes node-arrive {
  from {
    opacity: 0;
    transform: translateY(-0.55rem) scale(0.97);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}

@keyframes connector-grow {
  from { transform: scaleY(0); }
  to { transform: scaleY(1); }
}

@keyframes options-arrive {
  from {
    opacity: 0;
    transform: translateY(-0.3rem);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@media (prefers-reduced-motion: reduce) {
  .workflow-node.current,
  .connector.current-link,
  .review-options {
    animation: none;
  }
}
</style>
