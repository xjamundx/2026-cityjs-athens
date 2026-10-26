<template>
  <div
    class="system-architecture"
    aria-label="Browser, Agent, and Workflow system architecture"
  >
    <div class="actor actor-browser">
      <strong>Browser</strong>
      <span class="actor-kicker">React</span>
    </div>

    <div class="actor actor-agent">
      <strong>Agent</strong>
      <span class="actor-kicker">Agents SDK</span>
    </div>

    <div class="actor actor-workflow">
      <strong>Workflow</strong>
      <span class="actor-kicker">Workflow</span>
    </div>

    <div class="lifeline lifeline-browser" />
    <div class="lifeline lifeline-agent" />
    <div class="lifeline lifeline-workflow" />

    <div class="message message-use-agent bidirectional" v-click>
      <span class="message-label"><code>useAgent()</code></span>
      <span class="message-note">live state + RPC</span>
    </div>

    <div class="message message-generate bidirectional" v-click>
      <span class="message-label"
        ><code>agent.stub.createPlan(moodboard, prompt)</code></span
      >
      <span class="message-note">generation request, returns a plan</span>
    </div>

    <div class="message message-approve forward" v-click>
      <span class="message-label"><code>agent.stub.approve(planId)</code></span>
      <span class="message-note">human decision</span>
    </div>

    <div class="message message-run forward" v-click>
      <span class="message-label"><code>runWorkflow()</code></span>
      <span class="message-note">starts AI workflow</span>
    </div>

    <div
      class="absolute right-4 rounded bg-teal/10 shadow-black shadow-sm top-68 text-xs text-orange p-2"
      v-click
    >
      Runs each step...
    </div>

    <div class="message message-progress backward" v-click>
      <span class="message-label"><code>reportProgress()</code></span>
      <span class="message-note">update agent state</span>
    </div>

      <div class="message message-step forward" v-click>
      <span class="message-label"><code>agent.stub.makeDecision(workflowId, decision)</code></span>
      <span class="message-note">human decision</span>
    </div>


    <div class="message message-complete backward" v-click>
      <span class="message-label"><code>reportComplete()</code></span>
      <span class="message-note">finish + final result</span>
    </div>

    <div class="message message-broadcast backward" v-click>
      <span class="message-label"><code>broadcast()</code></span>
      <span class="message-note"
        >🎉 Tell the customer their image is ready</span
      >
    </div>

  </div>
</template>

<style scoped>
.actor {
  position: absolute;
  top: 0.35rem;
  z-index: 3;
  display: flex;
  width: 13.5rem;
  height: 2.75rem;
  flex-direction: column;
  justify-content: center;
  border: 1px solid currentColor;
  border-radius: 1.05rem;
  padding: 0.7rem 1rem;
  background: #151628;
  box-shadow: 0 0.7rem 1.6rem rgba(0, 0, 0, 0.28);
  transform: translateX(-50%);
}

.actor-browser {
  left: 16%;
  color: var(--browser);
}
.actor-agent {
  left: 50%;
  color: var(--agent);
}
.actor-workflow {
  left: 84%;
  color: var(--workflow);
}

.actor-kicker {
  margin-bottom: 0.1rem;
  font-size: 0.63rem;
  font-weight: 800;
  letter-spacing: 0.17em;
  text-transform: uppercase;
  opacity: 0.75;
}

.actor strong {
  color: #f8fafc;
  font-size: 1.52rem;
  line-height: 1.05;
  top: 8px;
  position: relative;
  display: block;
}

.actor-detail {
  margin-top: 0.18rem;
  color: #aeb8cc;
  font-size: 0.78rem;
}

.lifeline {
  position: absolute;
  top: 3.45rem;
  bottom: 0.8rem;
  width: 0;
  border-left: 1px dashed rgba(226, 232, 240, 0.22);
}

.lifeline-browser {
  left: 16%;
}
.lifeline-agent {
  left: 50%;
}
.lifeline-workflow {
  left: 84%;
}

.message {
  position: absolute;
  z-index: 2;
  height: 0;
  border-top: 2px solid currentColor;
  color: var(--agent);
}

.message::after,
.message::before {
  position: absolute;
  top: -0.39rem;
  width: 0;
  height: 0;
  border-top: 0.34rem solid transparent;
  border-bottom: 0.34rem solid transparent;
  content: "";
}

.message.forward::after,
.message.bidirectional::after {
  right: -0.08rem;
  border-left: 0.62rem solid currentColor;
}

.message.backward::before,
.message.bidirectional::before {
  left: -0.08rem;
  border-right: 0.62rem solid currentColor;
}

.message-label,
.message-note {
  position: absolute;
  left: 50%;
  white-space: nowrap;
  transform: translateX(-50%);
}

.message-label {
  bottom: 0.34rem;
  border: 1px solid color-mix(in srgb, currentColor 46%, transparent);
  border-radius: 999px;
  padding: 0.18rem 0.55rem;
  background: #151628;
  color: #f8fafc;
  font-size: 0.78rem;
  font-weight: 700;
}

.message-label code {
  color: inherit;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
}

.message-note {
  top: 0.38rem;
  color: #9aa8bf;
  font-size: 0.64rem;
  font-weight: 650;
  letter-spacing: 0.02em;
}

.message-use-agent {
  top: 5.5rem;
  left: 16%;
  width: 34%;
  color: violet;
}

.message-run {
  top: 15.45rem;
  left: 50%;
  width: 34%;
  color: var(--agent);
}

.message-progress {
  top: 20.4rem;
  left: 50%;
  width: 34%;
  color: var(--workflow);
}

.message-approve {
  top: 14.9rem;
  left: 16%;
  width: 34%;
  color: #fbbf24;
}
.message-step {
  top: 23rem;
  left: 16%;
  width: 34%;
  color: #fbbf24;
}

.message-generate {
  top: 10rem;
  left: 16%;
  width: 34%;
  color: #fbbf24;
}

.message-broadcast {
  top: 28rem;
  left: 16%;
  width: 34%;
  color: violet;
}

.message-complete {
  top: 25.8rem;
  left: 50%;
  width: 34%;
  color: var(--workflow);
}
</style>
