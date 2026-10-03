<script setup lang="ts">
import { nextTick, onBeforeUnmount, onMounted, ref, watch } from 'vue'

const props = withDefaults(defineProps<{ active: number; moodboardStage?: number }>(), { moodboardStage: 0 })

const steps = [
  ['Enhance prompt', 'Mistral Small 4'],
  ['Auto-route model', 'JEV → Stable Image Ultra'],
  ['Moodboard guidance', 'Nested subprocess'],
  ['Generate base image', 'Stable Image Ultra'],
  ['Insert branded assets', 'Inpainting model'],
  ['Evaluate and review', 'Gemini Flash 3.8'],
]

const moodboardSteps = [
  'Add reference images',
  'Label their roles',
  'Describe with Gemini Flash 3.8',
  'Save the descriptions',
  'Inject before prompt enhancement',
]

const canvas = ref<HTMLElement | null>(null)
const size = ref({ width: 400, height: 520 })
const routes = ref({ accept: '', retry: '', skip: '' })
let observer: ResizeObserver | undefined

function anchor(selector: string, edge: 'top' | 'bottom' | 'right') {
  const host = canvas.value
  const element = host?.querySelector<HTMLElement>(selector)
  if (!host || !element) return { x: 0, y: 0 }
  const h = host.getBoundingClientRect()
  const r = element.getBoundingClientRect()
  const left = r.left - h.left
  const top = r.top - h.top
  if (edge === 'right') return { x: left + r.width, y: top + r.height / 2 }
  if (edge === 'bottom') return { x: left + r.width / 2, y: top + r.height }
  return { x: left + r.width / 2, y: top }
}

function updateRoutes() {
  if (!canvas.value || props.active < 6) return
  const rect = canvas.value.getBoundingClientRect()
  size.value = { width: rect.width, height: rect.height }
  const accept = anchor('[data-route="accept"]', 'bottom')
  const output = anchor('[data-route="output"]', 'top')
  const retry = anchor('[data-route="retry"]', 'right')
  const retryTarget = anchor('[data-step="5"]', 'right')
  const skip = anchor('[data-route="skip"]', 'right')
  const skipTarget = anchor('[data-step="4"]', 'right')
  const outer = Math.max(retry.x, skip.x) + 14
  routes.value = {
    accept: `M ${accept.x} ${accept.y} L ${output.x} ${output.y}`,
    retry: `M ${retry.x} ${retry.y} C ${outer} ${retry.y}, ${outer} ${retryTarget.y}, ${retryTarget.x} ${retryTarget.y}`,
    skip: `M ${skip.x} ${skip.y} C ${outer + 10} ${skip.y}, ${outer + 10} ${skipTarget.y}, ${skipTarget.x} ${skipTarget.y}`,
  }
}

async function scheduleRoutes() {
  await nextTick()
  requestAnimationFrame(updateRoutes)
}

watch(() => [props.active, props.moodboardStage], scheduleRoutes)
onMounted(() => {
  scheduleRoutes()
  if (canvas.value) {
    observer = new ResizeObserver(updateRoutes)
    observer.observe(canvas.value)
  }
})
onBeforeUnmount(() => observer?.disconnect())
</script>

<template>
  <div class="workflow-shell">
    <div class="workflow-label">IMAGE WORKFLOW</div>
    <div ref="canvas" class="workflow-canvas">
      <template v-for="(step, index) in steps" :key="step[0]">
        <div v-if="index" class="connector" :class="{ complete: index < active, newest: index + 1 === active }" />
        <div
          class="workflow-node"
          :class="{ complete: index + 1 < active, current: index + 1 === active, future: index + 1 > active }"
          :data-step="index + 1"
        >
          <div class="node-main">
            <span class="step-number">{{ index + 1 }}</span>
            <span class="step-copy"><small>Step {{ index + 1 }}</small><strong>{{ step[0] }}</strong></span>
            <em>{{ step[1] }}</em><span class="status-dot" />
          </div>
          <div v-if="index === 2 && active >= 3" class="moodboard-subflow">
            <div
              v-for="(substep, subIndex) in moodboardSteps"
              :key="substep"
              class="moodboard-substep"
              :class="{ visible: subIndex + 1 <= moodboardStage, newest: subIndex + 1 === moodboardStage }"
            >
              <span>{{ subIndex + 1 }}</span><strong>{{ substep }}</strong>
            </div>
          </div>
        </div>
      </template>

      <div v-if="active >= 6" class="review-routing">
        <div class="review-options">
          <span data-route="accept">Accept</span><span data-route="retry">Retry</span><span data-route="skip">Skip</span>
        </div>
        <div data-route="output" class="final-output">Final image</div>
      </div>

      <svg v-if="active >= 6" class="route-arrows" :viewBox="`0 0 ${size.width} ${size.height}`" aria-hidden="true">
        <defs><marker id="route-arrowhead" markerWidth="7" markerHeight="7" refX="6" refY="3.5" orient="auto"><path d="M0 0L7 3.5L0 7Z" /></marker></defs>
        <path :d="routes.accept" class="accept-route" marker-end="url(#route-arrowhead)" />
        <path :d="routes.retry" class="retry-route" marker-end="url(#route-arrowhead)" />
        <path :d="routes.skip" class="skip-route" marker-end="url(#route-arrowhead)" />
      </svg>
    </div>
  </div>
</template>

<style scoped>
.workflow-shell{height:100%;min-width:0;padding:.65rem 1rem;display:flex;flex-direction:column;border:1px solid rgb(168 139 250/.38);border-radius:1rem;background:radial-gradient(circle at 1px 1px,rgb(196 181 253/.18) 1px,transparent 0) 0 0/16px 16px,linear-gradient(155deg,rgb(20 16 35/.96),rgb(8 8 18/.96));box-shadow:inset 0 0 2rem rgb(124 58 237/.08);overflow:hidden}
.workflow-label{margin-bottom:.3rem;color:rgb(196 181 253/.78);font-size:.56rem;font-weight:700;letter-spacing:.16em}
.workflow-canvas{position:relative;flex:1;min-height:0;display:flex;flex-direction:column;align-items:center;justify-content:center}
.workflow-node{position:relative;z-index:2;width:min(86%,20rem);padding:.22rem .48rem;border:1px solid rgb(148 163 184/.25);border-radius:.55rem;background:rgb(22 22 38/.97);color:#f1f5f9}
.node-main{min-height:1.55rem;display:grid;grid-template-columns:1.3rem 1fr auto .55rem;align-items:center;gap:.4rem}
.workflow-node.complete{border-color:rgb(52 211 153/.42)}
.workflow-node.current{border-color:rgb(196 181 253/.92);background:linear-gradient(100deg,rgb(76 29 149/.68),rgb(30 27 75/.96));box-shadow:0 0 1rem rgb(139 92 246/.26);animation:node-arrive .42s cubic-bezier(.2,.8,.2,1) both}
.workflow-node.future{opacity:0;visibility:hidden}
.step-number{width:1.3rem;height:1.3rem;display:grid;place-items:center;border-radius:.38rem;background:rgb(139 92 246/.27);color:#ddd6fe;font-size:.6rem;font-weight:800}
.step-copy{min-width:0;display:flex;flex-direction:column;line-height:1.02}.step-copy small{color:#94a3b8;font-size:.4rem;letter-spacing:.08em;text-transform:uppercase}.step-copy strong{font-size:.62rem;white-space:nowrap}
.node-main em{max-width:7rem;color:rgb(196 181 253/.85);font-size:.44rem;font-style:normal;font-weight:600;text-align:right;line-height:1.12}
.status-dot{width:.43rem;height:.43rem;border-radius:99px;background:rgb(100 116 139/.48)}.complete .status-dot{background:#34d399;box-shadow:0 0 .5rem rgb(52 211 153/.45)}.current .status-dot{background:#d8b4fe;box-shadow:0 0 .6rem rgb(192 132 252/.7)}
.connector{z-index:1;width:1px;height:.2rem;background:transparent}.connector.complete{background:rgb(52 211 153/.65)}.connector.newest{transform-origin:top;animation:connector-grow .3s ease-out both}
.moodboard-subflow{margin:.22rem .04rem .04rem 1.65rem;padding:.22rem .32rem;display:grid;gap:.1rem;border:1px solid rgb(167 139 250/.24);border-radius:.38rem;background:rgb(8 8 18/.55)}
.moodboard-substep{min-height:.72rem;display:grid;grid-template-columns:.75rem 1fr;align-items:center;color:#94a3b8;font-size:.42rem;line-height:1;opacity:0;visibility:hidden}.moodboard-substep.visible{color:#cbd5e1;opacity:1;visibility:visible}.moodboard-substep.newest{color:#ddd6fe;animation:substep-arrive .34s ease-out both}.moodboard-substep span{width:.6rem;height:.6rem;display:grid;place-items:center;border-radius:99px;background:rgb(139 92 246/.3);font-size:.34rem}.moodboard-substep strong{font-size:inherit;font-weight:580}
.review-routing{position:relative;z-index:2;width:min(82%,19rem);margin-top:.22rem}.review-options{display:grid;grid-template-columns:repeat(3,1fr);gap:.28rem;animation:options-arrive .42s .18s ease-out both}.review-options span,.final-output{padding:.18rem .3rem;border:1px solid rgb(196 181 253/.38);border-radius:99px;color:#ddd6fe;background:rgb(76 29 149/.28);font-size:.45rem;font-weight:650;text-align:center}.final-output{width:31%;margin-top:.48rem;border-color:rgb(52 211 153/.5);color:#a7f3d0;background:rgb(6 78 59/.32)}
.route-arrows{position:absolute;z-index:3;inset:0;width:100%;height:100%;overflow:visible;pointer-events:none}.route-arrows path{fill:none;stroke-width:1.2;stroke-linecap:round;stroke-dasharray:4 3;animation:route-arrive .55s .28s ease-out both}.route-arrows marker path{fill:#c4b5fd;stroke:none}.accept-route{stroke:rgb(52 211 153/.9)}.retry-route{stroke:rgb(251 191 36/.9)}.skip-route{stroke:rgb(196 181 253/.88)}
@keyframes node-arrive{from{opacity:0;transform:translateY(-.4rem) scale(.97)}to{opacity:1;transform:none}}@keyframes connector-grow{from{transform:scaleY(0)}to{transform:scaleY(1)}}@keyframes substep-arrive{from{opacity:0;transform:translateX(-.3rem)}to{opacity:1;transform:none}}@keyframes options-arrive{from{opacity:0;transform:translateY(-.2rem)}to{opacity:1;transform:none}}@keyframes route-arrive{from{opacity:0;stroke-dashoffset:24}to{opacity:1;stroke-dashoffset:0}}
@media(prefers-reduced-motion:reduce){.workflow-node.current,.connector.newest,.moodboard-substep.newest,.review-options,.route-arrows path{animation:none}}
</style>
