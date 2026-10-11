<script setup lang="ts">
withDefaults(defineProps<{
  step?: number
  title: string
  modelType: string
  model: string
  tone?: 'complete' | 'current'
}>(), {
  step: 1,
  tone: 'complete',
})
</script>

<template>
  <div class="model-badge" :class="`model-badge-${tone}`">
    <span class="model-badge-number">{{ step }}</span>
    <span class="model-badge-copy">
      <small>Step {{ step }}</small>
      <strong>{{ title }}</strong>
      <em><b>{{ modelType }}:</b> {{ model }}</em>
    </span>
    <span class="model-badge-status" />
  </div>
</template>

<style scoped>
.model-badge {
  min-height: 4.9rem;
  padding: 0.65rem 0.85rem;
  display: grid;
  grid-template-columns: 2.7rem minmax(0, 1fr) 0.7rem;
  align-items: center;
  gap: 0.65rem;
  border: 2px solid rgb(52 211 153 / 0.48);
  border-radius: 1rem;
  background: rgb(22 22 38 / 0.98);
  box-shadow: 0 0 1.25rem rgb(15 23 42 / 0.24);
  color: #f1f5f9;
}

.model-badge-current {
  border-color: rgb(196 181 253 / 0.9);
  background: linear-gradient(100deg, rgb(76 29 149 / 0.84), rgb(30 27 75 / 0.98));
  box-shadow: 0 0 1.1rem rgb(139 92 246 / 0.3);
}

.model-badge-current .model-badge-status {
  background: #d8b4fe;
  box-shadow: 0 0 0.65rem rgb(192 132 252 / 0.7);
}

.model-badge-number {
  width: 2.7rem;
  height: 2.7rem;
  display: grid;
  place-items: center;
  border-radius: 0.7rem;
  background: rgb(139 92 246 / 0.32);
  color: #ddd6fe;
  font-size: 1.2rem;
  font-weight: 800;
  line-height: 1;
}

.model-badge-copy {
  min-width: 0;
  display: flex;
  flex-direction: column;
  line-height: 1.05;
}

.model-badge-copy small {
  color: #94a3b8;
  font-size: 0.57rem;
  letter-spacing: 0.09em;
  text-transform: uppercase;
}

.model-badge-copy strong {
  font-size: 1rem;
  white-space: nowrap;
}

.model-badge-copy em {
  margin-top: 0.42rem;
  color: #c4b5fd;
  font-size: 0.7rem;
  font-style: normal;
  font-weight: 650;
}

.model-badge-copy em b {
  color: #94a3b8;
  font-size: 0.62rem;
  letter-spacing: 0.03em;
  text-transform: uppercase;
}

.model-badge-status {
  width: 0.68rem;
  height: 0.68rem;
  align-self: start;
  margin-top: 0.15rem;
  border-radius: 999px;
  background: #34d399;
  box-shadow: 0 0 0.65rem rgb(52 211 153 / 0.55);
}
</style>
