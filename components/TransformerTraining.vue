<template>
  <div class="training-illustration">
    <div class="main-flow">
      <div class="input-side">
        <div class="section-label">MODEL INPUT</div>
        <div class="side-title">Shifted action sequence</div>
        <div class="token-row">
          <span class="token start">SOS</span>
          <span class="token">ENI</span>
          <span class="token">D</span>
          <span class="token focus">NO</span>
        </div>
        <div class="side-note">Each position predicts the action that follows it</div>
      </div>

      <div class="entry-arrow">
        <span>token + position<br>embeddings</span>
        <b>&#8594;</b>
      </div>

      <div class="model-shell">
        <div class="model-header">
          <div>
            <span>DECODER-ONLY</span>
            <b>Transformer</b>
          </div>
          <div class="model-depth">&#215; 2 layers</div>
        </div>

        <div class="model-body">
          <div class="attention-stage">
            <div class="stage-title">Masked self-attention</div>
            <div class="attention-visual">
              <div class="mask-grid" aria-label="causal mask">
                <i class="on"></i><i></i><i></i><i></i>
                <i class="on"></i><i class="on"></i><i></i><i></i>
                <i class="on"></i><i class="on"></i><i class="on"></i><i></i>
                <i class="on"></i><i class="on"></i><i class="on"></i><i class="on hot"></i>
              </div>
              <div class="attention-copy">
                <b>4 heads</b>
                <span>Each token sees only itself and earlier actions</span>
              </div>
            </div>
          </div>

          <div class="inside-arrow">&#8594;</div>

          <div class="ffn-stage">
            <div class="stage-title">Feed-forward network</div>
            <div class="ffn-nodes">
              <span>16</span><i></i><span class="wide">128</span><i></i><span>16</span>
            </div>
            <div class="ffn-copy">with residual connections + normalization</div>
          </div>
        </div>
      </div>

      <div class="exit-arrow">
        <span>linear +<br>softmax</span>
        <b>&#8594;</b>
      </div>

      <div class="output-side">
        <div class="section-label">MODEL OUTPUT</div>
        <div class="side-title">Next-action probabilities</div>
        <div class="token-row">
          <span class="token predicted">ENI</span>
          <span class="token predicted">D</span>
          <span class="token predicted">NO</span>
          <span class="token answer">EXI</span>
        </div>
        <div class="distribution">
          <span></span><span></span><span></span><span class="high"></span>
        </div>
        <div class="side-note">One probability distribution at every position</div>
      </div>
    </div>

    <div class="loss-zone">
      <div class="target-group">
        <div class="loss-label">TRUE NEXT TOKENS</div>
        <div class="target-sequence">
          <span>ENI</span><span>D</span><span>NO</span><span class="target-answer">EXI</span>
        </div>
      </div>

      <div class="compare-arrow">
        <span>compare at<br>each position</span>
        <b>&#8594;</b>
      </div>

      <div class="loss-core">
        <span>TRAINING OBJECTIVE</span>
        <b>Cross-entropy loss</b>
        <small>padding positions are ignored</small>
      </div>

      <div class="update-path">
        <span>backpropagate error</span>
        <b>&#8630; update model weights</b>
      </div>
    </div>

  </div>
</template>

<style scoped>
.training-illustration {
  --ink: #27231d;
  --muted: #7d7568;
  --paper: #fbf7ec;
  --line: #d6cdb9;
  --teal: #1f6f78;
  --teal-dark: #164f56;
  --teal-soft: #d9eef0;
  --orange: #d94f30;
  width: 100%;
  color: var(--ink);
  font-size: 12px;
}

.main-flow {
  min-height: 255px;
  display: grid;
  grid-template-columns: 177px 70px 340px 62px 1fr;
  align-items: center;
}

.section-label,
.loss-label {
  color: var(--teal);
  font-size: 9px;
  font-weight: 850;
  letter-spacing: .14em;
}

.side-title {
  margin: 7px 0 13px;
  font-size: 14px;
  font-weight: 750;
}

.token-row {
  display: flex;
  gap: 6px;
}

.token {
  width: 36px;
  height: 34px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #c8bda6;
  border-radius: 7px;
  background: #fffdf7;
  font-family: ui-monospace, SFMono-Regular, Menlo, monospace;
  font-size: 11px;
  font-weight: 800;
  box-shadow: 0 3px 8px rgba(59, 51, 35, .08);
}

.token.start { color: var(--teal); border-color: #8ebbc0; }
.token.focus { color: #fff; border-color: var(--teal-dark); background: var(--teal); }
.token.predicted { background: #f7f0dc; }
.token.answer { color: #fff; border-color: #a43b26; background: var(--orange); }

.side-note {
  max-width: 170px;
  margin-top: 12px;
  color: var(--muted);
  font-size: 9px;
  line-height: 1.4;
}

.entry-arrow,
.exit-arrow {
  text-align: center;
  color: var(--teal);
}

.entry-arrow span,
.exit-arrow span {
  display: block;
  margin-bottom: 3px;
  color: var(--muted);
  font-size: 8px;
  line-height: 1.25;
}

.entry-arrow b,
.exit-arrow b {
  font-size: 28px;
  font-weight: 300;
}

.model-shell {
  overflow: hidden;
  border: 2px solid var(--teal-dark);
  border-radius: 18px;
  background: var(--paper);
  box-shadow: 0 15px 32px rgba(22, 79, 86, .20);
}

.model-header {
  height: 67px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0 18px;
  color: #fff;
  background: linear-gradient(135deg, var(--teal), var(--teal-dark));
}

.model-header > div:first-child {
  display: flex;
  flex-direction: column;
}

.model-header span {
  color: #cbe7e9;
  font-size: 8px;
  font-weight: 800;
  letter-spacing: .15em;
}

.model-header b {
  margin-top: 2px;
  font-size: 20px;
}

.model-depth {
  padding: 5px 9px;
  border: 1px solid rgba(255,255,255,.38);
  border-radius: 20px;
  color: #effafa;
  font-size: 9px;
  font-weight: 750;
}

.model-body {
  min-height: 154px;
  display: grid;
  grid-template-columns: 1.2fr 27px 1fr;
  align-items: center;
  padding: 16px;
  background:
    radial-gradient(circle at 8% 25%, rgba(217,238,240,.72), transparent 35%),
    linear-gradient(145deg, #fffdf7, #f3ead4);
}

.stage-title {
  margin-bottom: 12px;
  color: var(--teal-dark);
  font-size: 11px;
  font-weight: 800;
}

.attention-visual {
  display: grid;
  grid-template-columns: 66px 1fr;
  gap: 11px;
  align-items: center;
}

.mask-grid {
  display: grid;
  grid-template-columns: repeat(4, 12px);
  grid-template-rows: repeat(4, 12px);
  gap: 3px;
}

.mask-grid i {
  border: 1px solid #c9c0ae;
  border-radius: 2px;
  background: rgba(255,255,255,.65);
}

.mask-grid i.on { border-color: #4f8d94; background: var(--teal); }
.mask-grid i.hot { border-color: #a43b26; background: var(--orange); }

.attention-copy {
  display: flex;
  flex-direction: column;
  gap: 5px;
  color: var(--muted);
  font-size: 8px;
  line-height: 1.35;
}

.attention-copy b { color: var(--teal); font-size: 10px; }
.inside-arrow { color: #6d9ba0; font-size: 20px; text-align: center; }

.ffn-nodes {
  display: flex;
  align-items: center;
  gap: 5px;
}

.ffn-nodes span {
  min-width: 26px;
  height: 29px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #7ba8ad;
  border-radius: 50%;
  background: var(--teal-soft);
  color: var(--teal-dark);
  font-size: 9px;
  font-weight: 800;
}

.ffn-nodes span.wide {
  min-width: 40px;
  border-color: #d6a47b;
  border-radius: 14px;
  background: #f6e4d1;
  color: #8a4f26;
}

.ffn-nodes i { width: 8px; height: 1px; background: #9c917e; }
.ffn-copy { margin-top: 13px; color: var(--muted); font-size: 8px; line-height: 1.3; }

.distribution {
  width: 162px;
  height: 18px;
  display: flex;
  align-items: flex-end;
  gap: 6px;
  margin-top: 6px;
}

.distribution span {
  width: 36px;
  height: 5px;
  border-radius: 5px;
  background: #c9d9d8;
}

.distribution span:nth-child(2) { height: 8px; }
.distribution span:nth-child(3) { height: 11px; }
.distribution span.high { height: 17px; background: var(--orange); }

.loss-zone {
  min-height: 118px;
  display: grid;
  grid-template-columns: 250px 92px 250px 1fr;
  align-items: center;
  margin-top: 8px;
  padding: 10px 18px;
  border-top: 1px solid var(--line);
  border-bottom: 1px solid var(--line);
  background: linear-gradient(90deg, rgba(251,247,236,.25), rgba(251,247,236,.9), rgba(251,247,236,.25));
}

.target-sequence {
  display: flex;
  gap: 5px;
  margin-top: 8px;
}

.target-sequence span {
  min-width: 38px;
  padding: 5px 7px;
  border-bottom: 2px solid #a5bfc0;
  color: var(--teal-dark);
  font-family: ui-monospace, SFMono-Regular, Menlo, monospace;
  font-size: 10px;
  font-weight: 750;
  text-align: center;
}

.target-sequence span.target-answer { border-color: var(--orange); color: var(--orange); }

.compare-arrow { text-align: center; color: var(--teal); }
.compare-arrow span { display: block; color: var(--muted); font-size: 8px; line-height: 1.25; }
.compare-arrow b { font-size: 23px; font-weight: 300; }

.loss-core {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 78px;
  border: 2px solid var(--orange);
  border-radius: 40px;
  background: #fff9f5;
  box-shadow: 0 8px 18px rgba(217,79,48,.12);
}

.loss-core span { color: var(--orange); font-size: 8px; font-weight: 850; letter-spacing: .12em; }
.loss-core b { margin: 4px 0; color: #8e321f; font-size: 16px; }
.loss-core small { color: var(--muted); font-size: 8px; }

.update-path {
  display: flex;
  flex-direction: column;
  align-items: center;
  color: var(--teal);
  text-align: center;
}

.update-path span { color: var(--muted); font-size: 8px; }
.update-path b { margin-top: 4px; font-size: 11px; }

.bottom-message {
  margin-top: 13px;
  color: #d0ceba;
  font-size: 11px;
  font-weight: 650;
  text-align: center;
}
</style>
