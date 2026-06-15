
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Final Score Calculator</title>
  <style>
    :root {
      --cream: #F2F1EF;
      --taupe: #B1A6A4;
      --slate: #697184;
      --mist: #D8CFD0;
      --charcoal: #413F3D;
      --white-glass: rgba(255, 255, 255, 0.54);
      --deep-glass: rgba(65, 63, 61, 0.08);
      --shadow: 0 24px 70px rgba(65, 63, 61, 0.18);
      --soft-shadow: 0 14px 36px rgba(65, 63, 61, 0.12);
      --danger: #8e4d4d;
      --success: #526b5c;
      --radius: 8px;
      --meter: 0;
    }

    * {
      box-sizing: border-box;
    }

    html {
      min-height: 100%;
      scroll-behavior: smooth;
    }

    body {
      min-height: 100vh;
      margin: 0;
      color: var(--charcoal);
      font-family: Inter, "Segoe UI", Roboto, Arial, sans-serif;
      letter-spacing: 0;
      background:
        linear-gradient(135deg, rgba(216, 207, 208, 0.68) 0 25%, transparent 25% 100%),
        linear-gradient(315deg, rgba(105, 113, 132, 0.20) 0 24%, transparent 24% 100%),
        repeating-linear-gradient(90deg, rgba(65, 63, 61, 0.035) 0 1px, transparent 1px 92px),
        var(--cream);
      overflow-x: hidden;
    }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      pointer-events: none;
      background:
        linear-gradient(120deg, transparent 0 54%, rgba(177, 166, 164, 0.22) 54% 56%, transparent 56% 100%),
        linear-gradient(20deg, transparent 0 66%, rgba(65, 63, 61, 0.07) 66% 68%, transparent 68% 100%);
      mask-image: linear-gradient(to bottom, #000 0%, transparent 88%);
    }

    button,
    input {
      font: inherit;
      letter-spacing: 0;
    }

    button {
      border: 0;
      cursor: pointer;
    }

    [hidden] {
      display: none !important;
    }

    .page {
      position: relative;
      display: flex;
      min-height: 100vh;
      flex-direction: column;
      justify-content: center;
      padding: 42px 20px 24px;
    }

    .app {
      width: min(1120px, 100%);
      margin: 0 auto;
    }

    .intro {
      display: grid;
      grid-template-columns: minmax(0, 1fr) auto;
      gap: 24px;
      align-items: end;
      margin-bottom: 22px;
      animation: riseIn 560ms ease both;
    }

    .eyebrow {
      margin: 0 0 10px;
      color: var(--slate);
      font-size: 0.86rem;
      font-weight: 800;
      text-transform: uppercase;
    }

    h1 {
      max-width: 720px;
      margin: 0;
      color: var(--charcoal);
      font-family: "Segoe UI", Inter, Arial, sans-serif;
      font-size: 3.25rem;
      font-weight: 900;
      line-height: 1;
      letter-spacing: 0;
    }

    .intro-copy {
      max-width: 520px;
      margin: 14px 0 0;
      color: rgba(65, 63, 61, 0.76);
      font-size: 1rem;
      line-height: 1.7;
    }

    .badge {
      display: grid;
      width: 134px;
      min-height: 108px;
      place-items: center;
      padding: 16px;
      color: var(--cream);
      background: linear-gradient(145deg, var(--charcoal), var(--slate));
      box-shadow: var(--soft-shadow);
      border: 1px solid rgba(255, 255, 255, 0.45);
      border-radius: var(--radius);
      text-align: center;
    }

    .badge strong {
      display: block;
      font-size: 2rem;
      line-height: 1;
    }

    .badge span {
      display: block;
      margin-top: 8px;
      font-size: 0.78rem;
      font-weight: 750;
    }

    .calculator {
      display: grid;
      grid-template-columns: minmax(0, 1.05fr) minmax(340px, 0.95fr);
      gap: 20px;
      align-items: stretch;
      animation: riseIn 700ms 80ms ease both;
    }

    .panel,
    .result-card {
      border: 1px solid rgba(255, 255, 255, 0.56);
      border-radius: var(--radius);
      background: linear-gradient(145deg, rgba(255, 255, 255, 0.62), rgba(216, 207, 208, 0.34));
      box-shadow: var(--shadow);
      backdrop-filter: blur(22px);
      -webkit-backdrop-filter: blur(22px);
    }

    .panel {
      padding: 26px;
    }

    .form-grid {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 18px;
    }

    .field {
      min-width: 0;
    }

    .field.full {
      grid-column: 1 / -1;
    }

    label {
      display: block;
      margin-bottom: 8px;
      color: var(--charcoal);
      font-size: 0.94rem;
      font-weight: 800;
    }

    .input-wrap {
      position: relative;
    }

    input {
      width: 100%;
      min-height: 54px;
      padding: 15px 48px 15px 16px;
      color: var(--charcoal);
      font-size: 1rem;
      font-weight: 750;
      background: rgba(242, 241, 239, 0.72);
      border: 1px solid rgba(105, 113, 132, 0.24);
      border-radius: var(--radius);
      outline: none;
      box-shadow: inset 0 1px 0 rgba(255, 255, 255, 0.72);
      transition: border-color 180ms ease, box-shadow 180ms ease, transform 180ms ease, background 180ms ease;
    }

    input:focus {
      border-color: rgba(105, 113, 132, 0.76);
      background: rgba(242, 241, 239, 0.92);
      box-shadow: 0 0 0 4px rgba(105, 113, 132, 0.14), inset 0 1px 0 rgba(255, 255, 255, 0.82);
      transform: translateY(-1px);
    }

    input.invalid {
      border-color: rgba(142, 77, 77, 0.75);
      box-shadow: 0 0 0 4px rgba(142, 77, 77, 0.13);
    }

    .suffix {
      position: absolute;
      top: 50%;
      right: 16px;
      color: var(--slate);
      font-size: 0.92rem;
      font-weight: 900;
      transform: translateY(-50%);
      pointer-events: none;
    }

    .hint {
      min-height: 20px;
      margin: 8px 0 0;
      color: rgba(65, 63, 61, 0.62);
      font-size: 0.82rem;
      line-height: 1.45;
    }

    .field-error {
      display: block;
      min-height: 18px;
      margin-top: 6px;
      color: var(--danger);
      font-size: 0.8rem;
      font-weight: 800;
      line-height: 1.35;
    }

    .project-row {
      display: flex;
      min-height: 64px;
      align-items: center;
      justify-content: space-between;
      gap: 18px;
      margin: 24px 0 4px;
      padding: 14px;
      border: 1px solid rgba(105, 113, 132, 0.20);
      border-radius: var(--radius);
      background: rgba(242, 241, 239, 0.44);
    }

    .project-row label {
      margin: 0;
    }

    .toggle-help {
      display: block;
      margin-top: 5px;
      color: rgba(65, 63, 61, 0.60);
      font-size: 0.82rem;
      font-weight: 500;
      line-height: 1.4;
    }

    .toggle {
      display: inline-flex;
      min-width: 190px;
      min-height: 46px;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      padding: 8px 8px 8px 14px;
      color: var(--charcoal);
      background: rgba(255, 255, 255, 0.55);
      border: 1px solid rgba(105, 113, 132, 0.25);
      border-radius: var(--radius);
      box-shadow: 0 10px 24px rgba(65, 63, 61, 0.08);
      transition: transform 180ms ease, border-color 180ms ease, background 180ms ease;
    }

    .toggle > span:first-child {
      white-space: nowrap;
    }

    .toggle:hover,
    .toggle:focus-visible {
      border-color: rgba(105, 113, 132, 0.58);
      background: rgba(255, 255, 255, 0.76);
      transform: translateY(-1px);
    }

    .toggle-track {
      position: relative;
      width: 42px;
      height: 26px;
      flex: 0 0 auto;
      background: var(--taupe);
      border-radius: 6px;
      transition: background 180ms ease;
    }

    .toggle-track::after {
      content: "";
      position: absolute;
      top: 4px;
      left: 4px;
      width: 18px;
      height: 18px;
      background: var(--cream);
      border-radius: 4px;
      box-shadow: 0 4px 10px rgba(65, 63, 61, 0.2);
      transition: transform 180ms ease;
    }

    .toggle[aria-pressed="true"] {
      color: var(--cream);
      background: linear-gradient(145deg, var(--slate), var(--charcoal));
      border-color: rgba(65, 63, 61, 0.28);
    }

    .toggle[aria-pressed="true"] .toggle-track {
      background: rgba(242, 241, 239, 0.24);
    }

    .toggle[aria-pressed="true"] .toggle-track::after {
      transform: translateX(16px);
    }

    .project-fields {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 18px;
      max-height: 0;
      margin-top: 0;
      overflow: hidden;
      opacity: 0;
      transform: translateY(-8px);
      transition: max-height 320ms ease, opacity 220ms ease, transform 220ms ease, margin-top 220ms ease;
    }

    .project-fields[hidden] {
      display: none;
    }

    .project-fields.open {
      max-height: 260px;
      margin-top: 18px;
      opacity: 1;
      transform: translateY(0);
    }

    .actions {
      display: flex;
      flex-wrap: wrap;
      gap: 12px;
      margin-top: 26px;
    }

    .button {
      display: inline-flex;
      min-width: 156px;
      min-height: 52px;
      align-items: center;
      justify-content: center;
      padding: 14px 20px;
      border-radius: var(--radius);
      font-size: 0.96rem;
      font-weight: 900;
      transition: transform 180ms ease, box-shadow 180ms ease, background 180ms ease;
    }

    .button.primary {
      color: var(--cream);
      background: linear-gradient(135deg, var(--charcoal), var(--slate));
      box-shadow: 0 16px 32px rgba(65, 63, 61, 0.24);
    }

    .button.secondary {
      color: var(--charcoal);
      background: rgba(242, 241, 239, 0.72);
      border: 1px solid rgba(105, 113, 132, 0.20);
      box-shadow: 0 10px 24px rgba(65, 63, 61, 0.08);
    }

    .button:hover,
    .button:focus-visible {
      transform: translateY(-2px);
    }

    .button.primary:hover,
    .button.primary:focus-visible {
      box-shadow: 0 20px 40px rgba(65, 63, 61, 0.28);
    }

    .button.secondary:hover,
    .button.secondary:focus-visible {
      background: rgba(255, 255, 255, 0.78);
    }

    .result-card {
      display: grid;
      min-height: 100%;
      align-content: start;
      padding: 26px;
      overflow: hidden;
    }

    .result-top {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 16px;
      margin-bottom: 24px;
    }

    .result-top h2 {
      margin: 0;
      font-size: 1.08rem;
      font-weight: 900;
    }

    .status-pill {
      min-width: 88px;
      padding: 8px 10px;
      color: var(--cream);
      background: var(--slate);
      border-radius: var(--radius);
      text-align: center;
      font-size: 0.78rem;
      font-weight: 900;
    }

    .meter-wrap {
      display: grid;
      place-items: center;
      margin: 4px 0 26px;
    }

    .meter {
      position: relative;
      display: grid;
      width: min(260px, 72vw);
      aspect-ratio: 1;
      place-items: center;
      border-radius: 50%;
      background:
        radial-gradient(circle, rgba(242, 241, 239, 0.94) 0 55%, transparent 56%),
        conic-gradient(var(--slate) calc(var(--meter) * 1%), rgba(177, 166, 164, 0.32) 0);
      box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.55), 0 20px 54px rgba(65, 63, 61, 0.15);
      transition: background 300ms ease;
    }

    .meter::before {
      content: "";
      position: absolute;
      inset: 20px;
      border: 1px solid rgba(105, 113, 132, 0.18);
      border-radius: 50%;
    }

    .meter-content {
      position: relative;
      text-align: center;
      z-index: 1;
    }

    .meter-value {
      display: block;
      color: var(--charcoal);
      font-size: 2.8rem;
      font-weight: 950;
      line-height: 1;
    }

    .meter-label {
      display: block;
      margin-top: 9px;
      color: var(--slate);
      font-size: 0.85rem;
      font-weight: 900;
      text-transform: uppercase;
    }

    .message {
      min-height: 74px;
      margin: 0 0 22px;
      padding: 16px;
      color: rgba(65, 63, 61, 0.82);
      background: rgba(242, 241, 239, 0.58);
      border: 1px solid rgba(105, 113, 132, 0.16);
      border-radius: var(--radius);
      font-size: 1rem;
      font-weight: 750;
      line-height: 1.55;
    }

    .message.error {
      color: var(--danger);
      background: rgba(142, 77, 77, 0.08);
      border-color: rgba(142, 77, 77, 0.22);
    }

    .message.success {
      color: var(--success);
      background: rgba(82, 107, 92, 0.10);
      border-color: rgba(82, 107, 92, 0.22);
    }

    .breakdown {
      display: grid;
      gap: 12px;
      padding-top: 2px;
    }

    .breakdown-row {
      display: grid;
      grid-template-columns: minmax(0, 1fr) auto;
      gap: 14px;
      align-items: center;
      min-height: 42px;
      padding-bottom: 12px;
      border-bottom: 1px solid rgba(105, 113, 132, 0.16);
    }

    .breakdown-row:last-child {
      border-bottom: 0;
      padding-bottom: 0;
    }

    .breakdown-label {
      color: rgba(65, 63, 61, 0.66);
      font-size: 0.9rem;
      font-weight: 750;
      line-height: 1.35;
    }

    .breakdown-value {
      color: var(--charcoal);
      font-size: 0.98rem;
      font-weight: 950;
      text-align: right;
      white-space: nowrap;
    }

    .signature {
      margin: 24px auto 0;
      color: var(--charcoal);
      font-family: Georgia, "Times New Roman", serif;
      font-size: 1.05rem;
      font-weight: 900;
      text-align: center;
      text-transform: uppercase;
    }

    @keyframes riseIn {
      from {
        opacity: 0;
        transform: translateY(16px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    @media (max-width: 900px) {
      .page {
        justify-content: flex-start;
      }

      .intro,
      .calculator {
        grid-template-columns: 1fr;
      }

      .badge {
        width: 100%;
        min-height: 82px;
        grid-template-columns: auto 1fr;
        justify-items: start;
        text-align: left;
      }

      .badge span {
        margin: 0 0 0 12px;
      }

      .result-card {
        min-height: auto;
      }
    }

    @media (max-width: 620px) {
      .page {
        padding: 30px 14px 20px;
      }

      h1 {
        font-size: 2.15rem;
        line-height: 1.05;
      }

      .panel,
      .result-card {
        padding: 18px;
      }

      .form-grid,
      .project-fields {
        grid-template-columns: 1fr;
      }

      .project-fields.open {
        max-height: 460px;
      }

      .project-row {
        align-items: stretch;
        flex-direction: column;
      }

      .toggle {
        width: 100%;
      }

      .actions {
        display: grid;
        grid-template-columns: 1fr;
      }

      .button {
        width: 100%;
      }

      .result-top {
        align-items: stretch;
        flex-direction: column;
      }

      .status-pill {
        width: 100%;
      }
    }

    @media (prefers-reduced-motion: reduce) {
      *,
      *::before,
      *::after {
        animation-duration: 1ms !important;
        animation-iteration-count: 1 !important;
        scroll-behavior: auto !important;
        transition-duration: 1ms !important;
      }
    }
  </style>
</head>
<body>
  <main class="page">
    <div class="app">
      <section class="intro" aria-labelledby="page-title">
        <div>
          <p class="eyebrow">Grade planning studio</p>
          <h1 id="page-title">Final Score Calculator</h1>
          <p class="intro-copy">Plan the final with the midterm locked at 40%, then add a project split when your class has one.</p>
        </div>
        <div class="badge" aria-hidden="true">
          <strong>40</strong>
          <span>Midterm weight</span>
        </div>
      </section>

      <section class="calculator" aria-label="Final score calculator">
        <form class="panel" id="gradeForm" novalidate>
          <div class="form-grid">
            <div class="field">
              <label for="midterm">Midterm score</label>
              <div class="input-wrap">
                <input id="midterm" name="midterm" type="number" inputmode="decimal" min="0" max="100" step="0.01" placeholder="82">
                <span class="suffix">/100</span>
              </div>
              <p class="hint">Your raw midterm grade before the 40% weighting.</p>
              <span class="field-error" data-error-for="midterm" aria-live="polite"></span>
            </div>

            <div class="field">
              <label for="target">Target total grade</label>
              <div class="input-wrap">
                <input id="target" name="target" type="number" inputmode="decimal" min="0" max="100" step="0.01" value="50" placeholder="90">
                <span class="suffix">/100</span>
              </div>
              <p class="hint">The overall course grade you want to reach.</p>
              <span class="field-error" data-error-for="target" aria-live="polite"></span>
            </div>
          </div>

          <div class="project-row">
            <div>
              <label id="projectToggleLabel">Include Project</label>
              <span class="toggle-help">Split the final 60% between a project and the final exam.</span>
            </div>
            <button class="toggle" id="projectToggle" type="button" aria-labelledby="projectToggleLabel" aria-controls="projectFields" aria-pressed="false" aria-expanded="false">
              <span>Include Project</span>
              <span class="toggle-track" aria-hidden="true"></span>
            </button>
          </div>

          <div class="project-fields" id="projectFields" aria-hidden="true" inert hidden>
            <div class="field">
              <label for="projectPercent">Project percent</label>
              <div class="input-wrap">
                <input id="projectPercent" name="projectPercent" type="number" inputmode="decimal" min="0" max="60" step="0.01" placeholder="20" disabled>
                <span class="suffix">%</span>
              </div>
              <p class="hint">How much of the total course grade the project is worth.</p>
              <span class="field-error" data-error-for="projectPercent" aria-live="polite"></span>
            </div>

            <div class="field">
              <label for="projectScore">Project score</label>
              <div class="input-wrap">
                <input id="projectScore" name="projectScore" type="number" inputmode="decimal" min="0" max="100" step="0.01" placeholder="88" disabled>
                <span class="suffix">/100</span>
              </div>
              <p class="hint">Your project grade out of 100.</p>
              <span class="field-error" data-error-for="projectScore" aria-live="polite"></span>
            </div>
          </div>

          <div class="actions">
            <button class="button primary" type="submit">Calculate</button>
            <button class="button secondary" id="resetButton" type="button">Reset</button>
          </div>
        </form>

        <aside class="result-card" aria-live="polite">
          <div class="result-top">
            <h2>Result</h2>
            <span class="status-pill" id="statusPill">Ready</span>
          </div>

          <div class="meter-wrap">
            <div class="meter" id="meter" aria-hidden="true">
              <div class="meter-content">
                <span class="meter-value" id="meterValue">--</span>
                <span class="meter-label" id="meterLabel">Final needed</span>
              </div>
            </div>
          </div>

          <p class="message" id="resultMessage">Enter your scores, choose whether a project counts, then hit calculate.</p>

          <div class="breakdown" aria-label="Score breakdown">
            <div class="breakdown-row">
              <span class="breakdown-label">Current weighted midterm score</span>
              <span class="breakdown-value" id="midtermWeighted">-- / 40</span>
            </div>
            <div class="breakdown-row" id="projectBreakdown" hidden>
              <span class="breakdown-label">Weighted project score</span>
              <span class="breakdown-value" id="projectWeighted">--</span>
            </div>
            <div class="breakdown-row">
              <span class="breakdown-label">Final exam weight</span>
              <span class="breakdown-value" id="finalWeight">60%</span>
            </div>
          </div>
        </aside>
      </section>

      <footer class="signature">MADE BY FINLEY</footer>
    </div>
  </main>

  <script>
    const form = document.getElementById("gradeForm");
    const projectToggle = document.getElementById("projectToggle");
    const projectFields = document.getElementById("projectFields");
    const resetButton = document.getElementById("resetButton");
    const meter = document.getElementById("meter");
    const meterValue = document.getElementById("meterValue");
    const meterLabel = document.getElementById("meterLabel");
    const resultMessage = document.getElementById("resultMessage");
    const statusPill = document.getElementById("statusPill");
    const midtermWeighted = document.getElementById("midtermWeighted");
    const projectWeighted = document.getElementById("projectWeighted");
    const projectBreakdown = document.getElementById("projectBreakdown");
    const finalWeight = document.getElementById("finalWeight");

    const inputs = {
      midterm: document.getElementById("midterm"),
      target: document.getElementById("target"),
      projectPercent: document.getElementById("projectPercent"),
      projectScore: document.getElementById("projectScore")
    };

    const seriousMessage = "are we being serious right now";
    const targetMinimumMessage = "you want to fail at this course??? really???";
    let projectHideTimer;

    function getNumber(input) {
      if (input.value.trim() === "" || input.validity.badInput) {
        return null;
      }

      return Number(input.value);
    }

    function roundTwo(value) {
      return Math.round((value + Number.EPSILON) * 100) / 100;
    }

    function formatTwo(value) {
      return roundTwo(value).toFixed(2);
    }

    function setMeter(value, label) {
      const safeValue = Number.isFinite(value) ? Math.max(0, Math.min(100, value)) : 0;
      meter.style.setProperty("--meter", safeValue);
      meterValue.textContent = Number.isFinite(value) ? formatTwo(value) : "--";
      meterLabel.textContent = label;
    }

    function setMessage(text, type) {
      resultMessage.textContent = text;
      resultMessage.classList.remove("error", "success");

      if (type) {
        resultMessage.classList.add(type);
      }
    }

    function setStatus(text, tone) {
      statusPill.textContent = text;
      statusPill.style.background = tone || "var(--slate)";
    }

    function clearErrors() {
      Object.values(inputs).forEach((input) => {
        input.classList.remove("invalid");
      });

      document.querySelectorAll(".field-error").forEach((error) => {
        error.textContent = "";
      });
    }

    function markInvalid(name, message) {
      const input = inputs[name];
      const error = document.querySelector(`[data-error-for="${name}"]`);

      input.classList.add("invalid");
      if (error) {
        error.textContent = message;
      }
    }

    function showInvalidRange(fields) {
      fields.forEach((field) => markInvalid(field, seriousMessage));
      setMessage(seriousMessage, "error");
      setStatus("Check", "var(--danger)");
      setMeter(NaN, "Final needed");
    }

    function showInvalidTargetMinimum() {
      markInvalid("target", targetMinimumMessage);
      setMessage(targetMinimumMessage, "error");
      setStatus("Check", "var(--danger)");
      setMeter(NaN, "Final needed");
    }

    function resetResult() {
      setMeter(NaN, "Final needed");
      setMessage("Enter your scores, choose whether a project counts, then hit calculate.");
      setStatus("Ready");
      midtermWeighted.textContent = "-- / 40";
      projectWeighted.textContent = "--";
      projectBreakdown.hidden = true;
      finalWeight.textContent = "60%";
      clearErrors();
    }

    function toggleProject(forceState) {
      const isOn = typeof forceState === "boolean" ? forceState : projectToggle.getAttribute("aria-pressed") !== "true";
      window.clearTimeout(projectHideTimer);

      projectToggle.setAttribute("aria-pressed", String(isOn));
      projectToggle.setAttribute("aria-expanded", String(isOn));
      projectFields.setAttribute("aria-hidden", String(!isOn));
      projectFields.inert = !isOn;
      inputs.projectPercent.disabled = !isOn;
      inputs.projectScore.disabled = !isOn;

      if (isOn) {
        projectFields.hidden = false;
        window.requestAnimationFrame(() => {
          projectFields.classList.add("open");
        });
      } else {
        projectFields.classList.remove("open");
        projectHideTimer = window.setTimeout(() => {
          if (projectToggle.getAttribute("aria-pressed") !== "true") {
            projectFields.hidden = true;
          }
        }, 340);
      }

      if (!isOn) {
        inputs.projectPercent.value = "";
        inputs.projectScore.value = "";
        inputs.projectPercent.classList.remove("invalid");
        inputs.projectScore.classList.remove("invalid");
        document.querySelector('[data-error-for="projectPercent"]').textContent = "";
        document.querySelector('[data-error-for="projectScore"]').textContent = "";
      }
    }

    function calculateGrade(event) {
      event.preventDefault();
      clearErrors();

      const includeProject = projectToggle.getAttribute("aria-pressed") === "true";
      const midtermScore = getNumber(inputs.midterm);
      const targetGrade = getNumber(inputs.target);
      const missing = [];
      const outOfRange = [];

      if (midtermScore === null) missing.push("midterm");
      if (targetGrade === null) missing.push("target");

      if (midtermScore !== null && (midtermScore < 0 || midtermScore > 100)) outOfRange.push("midterm");
      if (targetGrade !== null && (targetGrade < 0 || targetGrade > 100)) outOfRange.push("target");

      let projectPercent = 0;
      let projectScore = 0;

      if (includeProject) {
        projectPercent = getNumber(inputs.projectPercent);
        projectScore = getNumber(inputs.projectScore);

        if (projectPercent === null) missing.push("projectPercent");
        if (projectScore === null) missing.push("projectScore");

        if (projectPercent !== null && (projectPercent < 0 || projectPercent > 60)) outOfRange.push("projectPercent");
        if (projectScore !== null && (projectScore < 0 || projectScore > 100)) outOfRange.push("projectScore");
      }

      if (outOfRange.length) {
        showInvalidRange(outOfRange);
        return;
      }

      if (targetGrade !== null && targetGrade < 50) {
        showInvalidTargetMinimum();
        return;
      }

      if (missing.length) {
        missing.forEach((field) => markInvalid(field, "Please enter a number."));
        setMessage("Add the missing numbers first.", "error");
        setStatus("Missing", "var(--danger)");
        setMeter(NaN, "Final needed");
        return;
      }

      const weightedMidterm = midtermScore * 0.4;
      const projectWeight = includeProject ? projectPercent / 100 : 0;
      const finalExamWeight = includeProject ? (60 - projectPercent) / 100 : 0.6;
      const weightedProject = includeProject ? projectScore * projectWeight : 0;
      const earnedBeforeFinal = weightedMidterm + weightedProject;

      midtermWeighted.textContent = `${formatTwo(weightedMidterm)} / 40`;
      finalWeight.textContent = `${formatTwo(finalExamWeight * 100)}%`;
      projectBreakdown.hidden = !includeProject;

      if (includeProject) {
        projectWeighted.textContent = `${formatTwo(weightedProject)} / ${formatTwo(projectPercent)}`;
      }

      if (finalExamWeight <= 0) {
        if (earnedBeforeFinal >= targetGrade) {
          setMeter(0, "Final needed");
          setMessage("You already reached the target with your midterm and project scores.", "success");
          setStatus("Reached", "var(--success)");
        } else {
          setMeter(100, "No final weight");
          setMessage("The target is impossible because the final exam has no weight left.", "error");
          setStatus("Impossible", "var(--danger)");
        }
        return;
      }

      const requiredFinal = (targetGrade - weightedMidterm - weightedProject) / finalExamWeight;
      const roundedFinal = roundTwo(requiredFinal);

      if (requiredFinal > 100) {
        setMeter(100, "Final needed");
        setMessage(`Target impossible. You would need ${formatTwo(requiredFinal)}% on the final exam.`, "error");
        setStatus("Impossible", "var(--danger)");
        return;
      }

      if (requiredFinal <= 0) {
        setMeter(0, "Final needed");
        setMessage("You already reached the target. No final exam score is needed.", "success");
        setStatus("Reached", "var(--success)");
        return;
      }

      setMeter(roundedFinal, "Final needed");
      setMessage(`You need ${formatTwo(requiredFinal)}% on the final exam to reach your target total grade.`, "success");
      setStatus("Solved", "var(--slate)");
    }

    projectToggle.addEventListener("click", () => toggleProject());
    form.addEventListener("submit", calculateGrade);

    resetButton.addEventListener("click", () => {
      form.reset();
      toggleProject(false);
      resetResult();
      inputs.midterm.focus();
    });

    Object.values(inputs).forEach((input) => {
      input.addEventListener("input", () => {
        input.classList.remove("invalid");
        const error = document.querySelector(`[data-error-for="${input.id}"]`);
        if (error) {
          error.textContent = "";
        }
      });
    });
  </script>
</body>
</html>
