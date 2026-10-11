<script setup lang="ts">
withDefaults(defineProps<{ active: number; assetStage?: number }>(), {
  assetStage: 0,
})

const steps = [
  ['Collect moodboard data', '', 'Retrieve from DB'],
  ['Enhance prompt', 'LLM', 'Mistral Small 4'],
  ['Auto-route model', 'Decision model', 'Jev, Clef, or Decisions API'],
  ['Generate base image', 'Image generation model', 'Selected in Step 3'],
  ['Image Insertion Loop', '', 'Insert moodboard images by category'],
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
          <div v-if="index === 4 && active >= 5" class="asset-subflow">
            <div class="asset-eval-meta">
              <span>Insertion <strong>Nano Banana 2</strong></span>
              <span>Evaluator <strong>Gemini Flash 3.8</strong></span>
            </div>
            <div class="mini-categories">
              <template v-for="(lane, laneIndex) in ['People', 'Products', 'Landmarks', 'Props']" :key="lane">
                <strong>{{ lane }}</strong><i v-if="laneIndex < 3">→</i>
              </template>
            </div>
            <div class="mini-flow"><span>Insert category</span><i>→</i><span>Evaluate realism</span></div>
            <div class="mini-branches">
              <span><small>≥80%</small><strong>Continue</strong></span>
              <span><small>Below 80%</small><strong>Ask human</strong><i>Continue · Retry · Skip</i></span>
            </div>
          </div>
        </div>
      </template>

    </div>
  </div>
</template>

<style scoped>
.workflow-shell{height:100%;min-width:0;padding:.65rem .55rem;display:flex;flex-direction:column;border:1px solid rgb(168 139 250/.38);border-radius:1rem;background:radial-gradient(circle at 1px 1px,rgb(196 181 253/.18) 1px,transparent 0) 0 0/16px 16px,linear-gradient(155deg,rgb(20 16 35/.96),rgb(8 8 18/.96));box-shadow:inset 0 0 2rem rgb(124 58 237/.08);overflow:hidden}
.workflow-canvas{position:relative;flex:1;min-height:0;padding:.25rem 0;display:flex;flex-direction:column;align-items:center;justify-content:flex-start}
.workflow-node{position:relative;z-index:2;width:94%;padding:.22rem .38rem;border:1px solid rgb(148 163 184/.25);border-radius:.55rem;background:rgb(22 22 38/.97);color:#f1f5f9}
.workflow-node[data-step="5"]{min-height:8.3rem}
.node-main{min-height:1.55rem;display:grid;grid-template-columns:1.15rem minmax(0,1fr) .45rem;align-items:center;gap:.3rem}
.workflow-node.complete{border-color:rgb(52 211 153/.42)}
.workflow-node.current{border-color:rgb(196 181 253/.92);background:linear-gradient(100deg,rgb(76 29 149/.68),rgb(30 27 75/.96));box-shadow:0 0 1rem rgb(139 92 246/.26)}
.workflow-node.current.settled{animation:none}
.workflow-node.future{opacity:0;visibility:hidden}
.step-number{grid-row:1;align-self:center;width:1.15rem;height:1.15rem;padding:0;display:flex;align-items:center;justify-content:center;border-radius:.34rem;background:rgb(139 92 246/.27);color:#ddd6fe;font-size:.55rem;font-weight:800;line-height:1;text-align:center;font-variant-numeric:tabular-nums}
.step-copy{min-width:0;display:flex;flex-direction:column;line-height:1.02}.step-copy small{color:#94a3b8;font-size:.4rem;letter-spacing:.08em;text-transform:uppercase}.step-copy strong{font-size:.62rem;white-space:nowrap}.step-copy i{margin-top:.06rem;color:#94a3b8;font-size:.34rem;font-style:normal;font-weight:500}
.node-main em{grid-column:2/3;color:rgb(196 181 253/.85);font-size:.4rem;font-style:normal;font-weight:600;line-height:1.1}.node-main em b{color:#94a3b8;font-size:.34rem;font-weight:600;text-transform:uppercase;letter-spacing:.03em}.status-dot{grid-column:3;grid-row:1}
.status-dot{width:.43rem;height:.43rem;border-radius:99px;background:rgb(100 116 139/.48)}.complete .status-dot{background:#34d399;box-shadow:0 0 .5rem rgb(52 211 153/.45)}.current .status-dot{background:#d8b4fe;box-shadow:0 0 .6rem rgb(192 132 252/.7)}
.connector{z-index:1;width:1px;min-height:.36rem;flex:1 1 .58rem;background:transparent}.connector.complete{background:rgb(52 211 153/.65)}
.asset-subflow{margin:.14rem .04rem .02rem 1.35rem;padding:.15rem .18rem;display:grid;gap:.11rem;border:1px solid rgb(167 139 250/.3);border-radius:.38rem;background:rgb(8 8 18/.58)}
.asset-eval-meta{display:grid;grid-template-columns:1fr 1fr;gap:.18rem}.asset-eval-meta span{display:flex;flex-direction:column;color:#94a3b8;font-size:.25rem;line-height:1.08}.asset-eval-meta strong{margin-top:.03rem;color:#ddd6fe;font-size:.3rem}
.mini-categories{display:flex;align-items:center;justify-content:space-between;gap:.06rem;padding:.09rem .1rem;border:1px solid rgb(196 181 253/.22);border-radius:.22rem}.mini-categories strong{font-size:.25rem}.mini-categories i{color:#a78bfa;font-size:.25rem;font-style:normal}
.mini-flow{display:flex;align-items:center;justify-content:center;gap:.12rem}.mini-flow span{padding:.07rem .14rem;border:1px solid rgb(196 181 253/.35);border-radius:.22rem;background:rgb(76 29 149/.2);font-size:.27rem;font-weight:700}.mini-flow i{color:#a78bfa;font-size:.3rem;font-style:normal}
.mini-branches{display:grid;grid-template-columns:.78fr 1.22fr;gap:.1rem}.mini-branches>span{padding:.08rem;border:1px solid rgb(52 211 153/.48);border-radius:.22rem;text-align:center;line-height:1}.mini-branches>span+span{border-color:rgb(251 191 36/.5)}.mini-branches small,.mini-branches strong,.mini-branches i{display:block}.mini-branches small{color:#94a3b8;font-size:.2rem}.mini-branches strong{margin-top:.03rem;color:#a7f3d0;font-size:.27rem}.mini-branches>span+span strong{color:#fde68a}.mini-branches i{margin-top:.04rem;color:#cbd5e1;font-size:.18rem;font-style:normal}
@keyframes node-arrive{from{opacity:0;transform:translateY(-.4rem) scale(.97)}to{opacity:1;transform:none}}@keyframes connector-grow{from{transform:scaleY(0)}to{transform:scaleY(1)}}
@media(prefers-reduced-motion:reduce){.workflow-node.current,.connector.newest{animation:none}}
</style>
