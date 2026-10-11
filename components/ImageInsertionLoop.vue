<template>
  <div class="insertion-loop" :class="`stage-${Math.min($slidev.nav.clicks, 4)}`">
    <span v-click class="click-stop" />
    <span v-click class="click-stop" />
    <span v-click class="click-stop" />
    <span v-click class="click-stop" />

    <header class="loop-header">
      <span class="step-number">5</span>
      <div>
        <small>Step 5 of 5</small>
        <h2>Image Insertion Loop</h2>
      </div>
    </header>

    <section class="loop-context reveal context">
      <div class="categories">
        <span class="context-label">Categories</span>
        <template v-for="(category, index) in categories" :key="category">
          <strong>{{ category }}</strong>
          <i v-if="index < categories.length - 1">→</i>
        </template>
      </div>
      <div class="model-row">
        <span><small>Insertion</small><strong>Nano Banana 2</strong></span>
        <span><small>Evaluator</small><strong>Gemini Flash 3.8</strong></span>
      </div>
      <p><span>Active image</span> Starts with the base image · updates only when accepted</p>
    </section>

    <section class="loop-body">
      <p class="eyebrow"><span>For each category</span><span>After Props → Done</span></p>

      <div class="primary-flow reveal flow">
        <div class="action-card">
          <strong>Insert category images</strong>
          <span>Into the active image</span>
        </div>
        <i class="arrow">→</i>
        <div class="action-card">
          <strong>Evaluate</strong>
          <span>Realism</span>
        </div>
      </div>

      <div class="pass-flow reveal pass">
        <span class="branch-label">≥80%</span>
        <i class="arrow">→</i>
        <div class="action-card accepted">
          <strong>Update active image</strong>
          <span>→ Next category</span>
        </div>
      </div>

      <div class="human-gate reveal review">
        <div class="gate-heading"><span>Below 80%</span><strong>Ask a human</strong></div>
        <div class="choices reveal choices">
          <article>
            <strong>Continue</strong>
            <p>Accept output · update active image</p>
            <span>→ Next category</span>
          </article>
          <article>
            <strong>Retry</strong>
            <p>Discard output · keep active image</p>
            <span>↶ Back to insert</span>
          </article>
          <article>
            <strong>Skip</strong>
            <p>Discard output · keep active image</p>
            <span>→ Next category</span>
          </article>
        </div>
      </div>

    </section>
  </div>
</template>

<script setup lang="ts">
const categories = ['People', 'Products', 'Landmarks', 'Props']
</script>

<style scoped>
.insertion-loop{position:absolute;inset:1.75rem;display:grid;grid-template-rows:auto auto minmax(0,1fr);gap:.48rem;padding:.82rem 1rem .75rem;border:2px solid rgb(196 181 253/.88);border-radius:1.45rem;background:radial-gradient(circle at 1px 1px,rgb(196 181 253/.16) 1px,transparent 0) 0 0/18px 18px,linear-gradient(145deg,rgb(71 35 130/.95),rgb(40 22 82/.98));box-shadow:inset 0 0 3rem rgb(124 58 237/.22),0 1rem 3rem rgb(0 0 0/.18);color:#f8fafc;overflow:hidden}
.click-stop{position:absolute;width:0;height:0;overflow:hidden}.loop-header{display:flex;align-items:center;gap:.65rem}.step-number{width:2.55rem;height:2.55rem;display:grid;place-items:center;border-radius:.78rem;background:rgb(139 92 246/.44);color:#ede9fe;font-size:1.3rem;font-weight:850;line-height:1}.loop-header small,.eyebrow{color:#c4b5fd;font-size:.59rem;font-weight:700;letter-spacing:.16em;text-transform:uppercase}.loop-header h2{margin:.02rem 0 0!important;color:#fff;font-size:1.62rem;line-height:1.02}
.loop-context{display:grid;grid-template-columns:minmax(0,1.65fr) minmax(0,.75fr);align-items:center;gap:.3rem .7rem}.categories{display:flex;align-items:center;gap:.3rem}.context-label{margin-right:.08rem;color:#c4b5fd;font-size:.62rem}.categories strong{padding:.25rem .43rem;border:1px solid rgb(196 181 253/.45);border-radius:.45rem;background:rgb(15 10 35/.22);font-size:.6rem;white-space:nowrap}.categories i{color:#c4b5fd;font-size:.68rem;font-style:normal}.model-row{display:grid;grid-template-columns:1fr 1fr;gap:.32rem}.model-row>span{padding:.24rem .34rem;border:1px solid rgb(196 181 253/.3);border-radius:.4rem;background:rgb(15 10 35/.22)}.model-row small,.model-row strong{display:block}.model-row small{color:#c4b5fd;font-size:.4rem;text-transform:uppercase;letter-spacing:.08em}.model-row strong{margin-top:.03rem;font-size:.53rem}.loop-context p{grid-column:1/-1;margin:0!important;color:#f5f3ff;font-size:.62rem}.loop-context p span{margin-right:.45rem;color:#c4b5fd}
.loop-body{position:relative;min-height:0;padding:.55rem .72rem .48rem;border:1px solid rgb(196 181 253/.38);border-radius:.85rem;background:rgb(10 8 25/.7);display:grid;grid-template-columns:minmax(0,1.25fr) minmax(0,.88fr);grid-template-rows:auto auto auto;gap:.32rem .62rem}.eyebrow{grid-column:1/-1;margin:0!important;display:flex;align-items:center;justify-content:space-between}.primary-flow{grid-column:1;grid-row:2;display:grid;grid-template-columns:1fr auto 1fr;align-items:center;gap:.32rem}.pass-flow{grid-column:2;grid-row:2;display:grid;grid-template-columns:auto auto 1fr;align-items:center;gap:.28rem}.action-card{min-height:2.6rem;padding:.36rem .48rem;display:flex;flex-direction:column;align-items:center;justify-content:center;border:1px solid rgb(196 181 253/.46);border-radius:.6rem;background:rgb(30 23 55/.85);text-align:center}.action-card strong{font-size:.7rem}.action-card span{margin-top:.1rem;color:#c4b5fd;font-size:.54rem}.action-card.accepted{border-color:rgb(52 211 153/.72);background:rgb(6 78 59/.32)}.action-card.accepted strong{color:#a7f3d0}.arrow{color:#c4b5fd;font-size:.92rem;font-style:normal}.branch-label{color:#a7f3d0;font-size:.61rem;font-weight:750}
.human-gate{grid-column:1/-1;grid-row:3;padding:.28rem .58rem .34rem;border:1px solid rgb(251 191 36/.68);border-radius:.68rem;background:rgb(52 38 29/.3);display:grid;grid-template-rows:auto auto;gap:.18rem}.gate-heading{display:flex;align-items:center;justify-content:center;gap:.55rem}.gate-heading span{color:#fde68a;font-size:.55rem}.gate-heading strong{color:#fde68a;font-size:.7rem}.choices{display:grid;grid-template-columns:repeat(3,1fr);gap:.48rem}.choices article{padding:.2rem .42rem 0;border-top:1px solid rgb(253 230 138/.35)}.choices article strong{font-size:.62rem}.choices article p{margin:.06rem 0!important;color:#ddd6fe;font-size:.47rem;line-height:1.08}.choices article span{color:#c4b5fd;font-size:.46rem}
.reveal{opacity:0;transform:translateY(.35rem);transition:opacity .25s ease,transform .25s ease}.stage-1 .context,.stage-2 .context,.stage-3 .context,.stage-4 .context,.stage-2 .flow,.stage-3 .flow,.stage-4 .flow,.stage-3 .pass,.stage-4 .pass,.stage-4 .review,.stage-4 .choices{opacity:1;transform:none}@media(prefers-reduced-motion:reduce){.reveal{transition:none}}
</style>
