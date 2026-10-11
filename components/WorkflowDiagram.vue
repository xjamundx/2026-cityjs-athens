<script setup lang="ts">
withDefaults(defineProps<{ active: number; moodboardStage?: number; assetStage?: number }>(), {
  moodboardStage: 0,
  assetStage: 0,
})

const steps = [
  ['Enhance prompt', 'LLM', 'Mistral Small 4'],
  ['Auto-route model', 'Decision model', 'Jev'],
  ['Moodboard guidance', 'VLM', 'Gemini 3.8 Flash-Lite', 'Performed at creation time'],
  ['Generate base image', 'Image generation model', 'Selected in Step 2'],
  ['Image Insertion Loop', '', 'Insert moodboard images by category'],
]

const moodboardSteps = [
  'Add reference images',
  'Label their roles',
  'Generate descriptions with a VLM',
  'Save the descriptions',
]

</script>

<template>
  <div class="workflow-shell">
    <div class="workflow-canvas">
      <template v-for="(step, index) in steps" :key="step[0]">
        <div v-if="index" class="connector" :class="{ complete: index < active, newest: index + 1 === active && active < 5 }" />
        <div
          class="workflow-node"
          :class="{ complete: index + 1 < active, current: index + 1 === active, future: index + 1 > active, settled: index === 4 }"
          :data-step="index + 1"
        >
          <div class="node-main">
            <span class="step-number">{{ index + 1 }}</span>
            <span class="step-copy"><small>Step {{ index + 1 }}</small><strong>{{ step[0] }}</strong><i v-if="step[3]">{{ step[3] }}</i></span>
            <em><b v-if="step[1]">{{ step[1] }}:</b> {{ step[2] }}</em><span class="status-dot" />
          </div>
          <div v-if="index === 2 && active >= 3" class="moodboard-subflow">
            <div
              v-for="(substep, subIndex) in moodboardSteps"
              :key="substep"
              class="moodboard-substep"
              :class="{ visible: subIndex + 1 <= Math.min(moodboardStage, moodboardSteps.length), newest: subIndex + 1 === moodboardStage && moodboardStage < moodboardSteps.length }"
            >
              <span>{{ subIndex + 1 }}</span><strong>{{ substep }}</strong>
            </div>
          </div>
          <div
            v-if="index === 2 && active >= 3 && moodboardStage >= 5"
            class="moodboard-handoff"
            :class="{ newest: active === 3 }"
          >
            <span>↰</span><strong>Feeds Step 1 · Prompt enhancement</strong>
          </div>
          <div v-if="index === 4 && active >= 5" class="asset-subflow" :class="{ 'has-evaluation': assetStage >= 3 }">
            <div class="asset-eval-meta">
              <span>Image insertion <strong>Nano Banana 2</strong></span>
              <span>Evaluator <strong>Gemini Flash 3.8</strong></span>
              <span>Quality gate <strong>≥80% passes</strong></span>
            </div>
            <div class="asset-lanes">
              <template v-for="(lane, laneIndex) in ['People', 'Products', 'Landmarks', 'Props']" :key="lane">
                <div
                  class="asset-lane"
                  :class="{
                    visible: assetStage >= 3 || assetStage >= laneIndex + 1,
                    current: assetStage === laneIndex + 1,
                    complete: assetStage >= 3 || assetStage > laneIndex + 1,
                  }"
                >
                  <strong>{{ lane }}</strong>
                  <div class="asset-flow">
                    <span>Insert</span><i>→</i><span>Evaluate</span>
                  </div>
                </div>
              </template>
            </div>
            <div v-if="assetStage >= 3" class="evaluation-flow">
              <strong class="evaluation-question">Evaluate realism</strong>
              <div class="evaluation-branches">
                <div class="evaluation-path pass">
                  <small>80% or above</small>
                  <strong>Continue</strong>
                </div>
                <div class="evaluation-path review">
                  <small>Below 80%</small>
                  <strong>Ask a human</strong>
                  <span>Continue · Retry · Skip</span>
                  <i>Skip reuses the previous image</i>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>

    </div>
  </div>
</template>

<style scoped>
.workflow-shell{height:100%;min-width:0;padding:.65rem .55rem;display:flex;flex-direction:column;border:1px solid rgb(168 139 250/.38);border-radius:1rem;background:radial-gradient(circle at 1px 1px,rgb(196 181 253/.18) 1px,transparent 0) 0 0/16px 16px,linear-gradient(155deg,rgb(20 16 35/.96),rgb(8 8 18/.96));box-shadow:inset 0 0 2rem rgb(124 58 237/.08);overflow:hidden}
.workflow-canvas{position:relative;flex:1;min-height:0;padding-top:.25rem;display:flex;flex-direction:column;align-items:center;justify-content:flex-start}
.workflow-node{position:relative;z-index:2;width:94%;padding:.22rem .38rem;border:1px solid rgb(148 163 184/.25);border-radius:.55rem;background:rgb(22 22 38/.97);color:#f1f5f9}
.node-main{min-height:1.55rem;display:grid;grid-template-columns:1.15rem minmax(0,1fr) .45rem;align-items:center;gap:.3rem}
.workflow-node.complete{border-color:rgb(52 211 153/.42)}
.workflow-node.current{border-color:rgb(196 181 253/.92);background:linear-gradient(100deg,rgb(76 29 149/.68),rgb(30 27 75/.96));box-shadow:0 0 1rem rgb(139 92 246/.26);animation:node-arrive .42s .42s cubic-bezier(.2,.8,.2,1) both}
.workflow-node.current.settled{animation:none}
.workflow-node.future{opacity:0;visibility:hidden}
.step-number{grid-row:1;align-self:center;width:1.15rem;height:1.15rem;padding:0;display:flex;align-items:center;justify-content:center;border-radius:.34rem;background:rgb(139 92 246/.27);color:#ddd6fe;font-size:.55rem;font-weight:800;line-height:1;text-align:center;font-variant-numeric:tabular-nums}
.step-copy{min-width:0;display:flex;flex-direction:column;line-height:1.02}.step-copy small{color:#94a3b8;font-size:.4rem;letter-spacing:.08em;text-transform:uppercase}.step-copy strong{font-size:.62rem;white-space:nowrap}.step-copy i{margin-top:.06rem;color:#94a3b8;font-size:.34rem;font-style:normal;font-weight:500}
.node-main em{grid-column:2/3;color:rgb(196 181 253/.85);font-size:.4rem;font-style:normal;font-weight:600;line-height:1.1}.node-main em b{color:#94a3b8;font-size:.34rem;font-weight:600;text-transform:uppercase;letter-spacing:.03em}.status-dot{grid-column:3;grid-row:1}
.status-dot{width:.43rem;height:.43rem;border-radius:99px;background:rgb(100 116 139/.48)}.complete .status-dot{background:#34d399;box-shadow:0 0 .5rem rgb(52 211 153/.45)}.current .status-dot{background:#d8b4fe;box-shadow:0 0 .6rem rgb(192 132 252/.7)}
.connector{z-index:1;width:1px;height:.42rem;background:transparent}.connector.complete{background:rgb(52 211 153/.65)}.connector.newest{transform-origin:top;animation:connector-grow .3s .34s ease-out both}
.moodboard-subflow{margin:.22rem .04rem .04rem 1.65rem;padding:.22rem .32rem;display:grid;gap:.1rem;border:1px solid rgb(167 139 250/.24);border-radius:.38rem;background:rgb(8 8 18/.55)}
.moodboard-substep{min-height:.72rem;display:grid;grid-template-columns:.75rem 1fr;align-items:center;color:#94a3b8;font-size:.42rem;line-height:1;opacity:0;visibility:hidden}.moodboard-substep.visible{color:#cbd5e1;opacity:1;visibility:visible}.moodboard-substep.newest{color:#ddd6fe;animation:substep-arrive .34s .62s ease-out both}.moodboard-substep span{width:.6rem;height:.6rem;display:grid;place-items:center;border-radius:99px;background:rgb(139 92 246/.3);font-size:.34rem}.moodboard-substep strong{font-size:inherit;font-weight:580}
.moodboard-handoff{margin:.12rem .04rem .04rem 1.65rem;padding:.12rem .22rem;display:grid;grid-template-columns:.7rem 1fr;align-items:center;border:1px dashed rgb(251 191 36/.48);border-radius:.3rem;color:#fcd34d;background:rgb(120 53 15/.13);font-size:.34rem;line-height:1}.moodboard-handoff span{font-size:.55rem}.moodboard-handoff.newest{animation:handoff-arrive .34s .55s ease-out both}
.asset-subflow{margin:.16rem .04rem .02rem 1.35rem;padding:.14rem .18rem;display:grid;gap:.08rem;border:1px solid rgb(167 139 250/.24);border-radius:.38rem;background:rgb(8 8 18/.55)}.asset-subflow.has-evaluation{grid-template-columns:minmax(0,1.18fr) minmax(0,.82fr);column-gap:.16rem;align-items:start}
.asset-eval-meta{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:.14rem;padding-bottom:.08rem}.asset-eval-meta span{display:flex;flex-direction:column;color:#94a3b8;font-size:.27rem;line-height:1.1}.asset-eval-meta strong{margin-top:.04rem;color:#ddd6fe;font-size:.32rem}
.has-evaluation .asset-eval-meta{grid-column:1/-1}.asset-lanes{position:relative;display:grid;gap:.08rem;min-width:0}.has-evaluation .asset-lanes:after{content:"image →";position:absolute;right:-.14rem;top:50%;z-index:2;padding:.02rem .04rem;color:#ddd6fe;background:rgb(30 27 75/.96);font-size:.22rem;font-weight:750;line-height:1;transform:translate(50%,-50%);white-space:nowrap}
.asset-lane{display:none;min-height:0;padding:.05rem .07rem;border:1px solid rgb(100 116 139/.38);border-radius:.2rem;color:#94a3b8;line-height:1}.asset-lane.visible{display:grid;grid-template-columns:2.55rem 1fr;align-items:center;column-gap:.1rem}.asset-lane.current{border-color:rgb(251 191 36/.7);color:#fef3c7}.asset-lane.complete{border-color:rgb(52 211 153/.55);color:#d1fae5}
.asset-lane>strong{grid-column:1;align-self:center;font-size:.34rem;line-height:1}.asset-flow{grid-column:2;display:flex;align-items:center;gap:.05rem;white-space:nowrap}.asset-flow span{padding:.02rem .06rem;border:1px solid rgb(196 181 253/.24);border-radius:.14rem;background:rgb(76 29 149/.18);font-size:.27rem;font-weight:650;line-height:1}.asset-flow i{color:#a78bfa;font-size:.28rem;font-style:normal}
.evaluation-flow{min-width:0;padding:.12rem;border:1px solid rgb(196 181 253/.48);border-radius:.3rem;background:rgb(15 10 30/.42);color:#e2e8f0}.evaluation-question{display:block;margin:0 0 .12rem;color:#ddd6fe;font-size:.3rem;line-height:1;text-align:center}
.evaluation-branches{display:grid;gap:.08rem}.evaluation-path{position:relative;min-width:0;padding:.07rem .08rem;border:1px solid rgb(100 116 139/.38);border-radius:.2rem;background:rgb(15 23 42/.44);text-align:center;line-height:1}.evaluation-path:before{content:"";position:absolute;left:50%;bottom:100%;width:1px;height:.08rem;background:rgb(196 181 253/.4)}.evaluation-path small{display:block;margin-bottom:.03rem;color:#94a3b8;font-size:.21rem}.evaluation-path>strong{display:block;font-size:.27rem}.evaluation-path.pass{border-color:rgb(52 211 153/.5);color:#a7f3d0}.evaluation-path.review{border-color:rgb(251 191 36/.52);color:#fde68a}.evaluation-path span{display:block;margin-top:.05rem;color:#e2e8f0;font-size:.2rem;font-weight:650}.evaluation-path i{display:block;margin-top:.03rem;color:#94a3b8;font-size:.18rem;font-style:normal}
@keyframes node-arrive{from{opacity:0;transform:translateY(-.4rem) scale(.97)}to{opacity:1;transform:none}}@keyframes connector-grow{from{transform:scaleY(0)}to{transform:scaleY(1)}}@keyframes substep-arrive{from{opacity:0;transform:translateX(-.3rem)}to{opacity:1;transform:none}}@keyframes handoff-arrive{from{opacity:0;transform:translateY(-.2rem)}to{opacity:1;transform:none}}
@media(prefers-reduced-motion:reduce){.workflow-node.current,.connector.newest,.moodboard-substep.newest,.moodboard-handoff.newest{animation:none}}
</style>
