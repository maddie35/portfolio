<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Target Size (Minimum) — Decision Chart</title>
<style>
  :root {
    --ink: #1c2431;
    --ink-soft: #47536a;
    --paper: #f7f8fa;
    --card: #ffffff;
    --line: #c9d1de;
    --accent: #1d4ed8;      /* cobalt — questions */
    --pass-bg: #e7f4ec;     /* outcomes: pass / exception */
    --pass-ink: #135c31;
    --pass-line: #1b7a42;
    --fail-bg: #fbeaea;
    --fail-ink: #8c1d24;
    --fail-line: #b3282f;
    --focus: #7c3aed;
  }

  * { box-sizing: border-box; margin: 0; }

  body {
    font-family: "Segoe UI", "Avenir Next", Avenir, -apple-system, "Helvetica Neue", Arial, sans-serif;
    background: var(--paper);
    color: var(--ink);
    line-height: 1.55;
    padding: 2.5rem 1rem 4rem;
  }

  main { max-width: 46rem; margin: 0 auto; }

  header { margin-bottom: 2.25rem; }

  .eyebrow {
    font-size: 0.8rem;
    letter-spacing: 0.12em;
    text-transform: uppercase;
    font-weight: 600;
    color: var(--accent);
    margin-bottom: 0.5rem;
  }

  h1 {
    font-size: clamp(1.6rem, 4vw, 2.2rem);
    line-height: 1.2;
    margin-bottom: 0.75rem;
  }

  .intro { color: var(--ink-soft); max-width: 40rem; }

  /* True-size reference swatch — rendered at actual 24×24 CSS px */
  .swatch-row {
    display: flex;
    align-items: center;
    gap: 0.85rem;
    margin-top: 1.25rem;
    padding: 0.9rem 1rem;
    background: var(--card);
    border: 1px solid var(--line);
    border-radius: 8px;
    max-width: 40rem;
  }
  .swatch {
    width: 24px;
    height: 24px;
    flex: none;
    background: var(--accent);
    border-radius: 4px;
  }
  .swatch-circle {
    width: 24px;
    height: 24px;
    flex: none;
    border: 2px solid var(--accent);
    border-radius: 50%;
  }
  .swatch-row p { font-size: 0.9rem; color: var(--ink-soft); }
  .swatch-row strong { color: var(--ink); }

  /* Flowchart */
  ol.flow {
    list-style: none;
    padding: 0;
    counter-reset: step;
  }

  .step {
    counter-increment: step;
    position: relative;
    padding-left: 3.4rem;
    margin-bottom: 0;
  }

  /* Step number badge */
  .step::before {
    content: counter(step);
    position: absolute;
    left: 0;
    top: 0;
    width: 2.4rem;
    height: 2.4rem;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--accent);
    color: #fff;
    font-weight: 700;
    font-size: 1.05rem;
    border-radius: 50%;
  }

  /* Vertical connector between steps */
  .step:not(:last-child)::after {
    content: "";
    position: absolute;
    left: calc(1.2rem - 1.5px);
    top: 2.4rem;
    bottom: -0.25rem;
    width: 3px;
    background: var(--line);
  }

  .question {
    background: var(--card);
    border: 1.5px solid var(--accent);
    border-radius: 10px;
    padding: 1rem 1.15rem;
  }

  .question h2 {
    font-size: 1.05rem;
    line-height: 1.4;
    font-weight: 650;
  }

  .question .note {
    font-size: 0.88rem;
    color: var(--ink-soft);
    margin-top: 0.35rem;
  }

  .branches {
    display: grid;
    gap: 0.6rem;
    padding: 0.9rem 0 1.6rem;
  }
  @media (min-width: 560px) {
    .branches { grid-template-columns: 1fr 1fr; }
  }

  .branch {
    border-radius: 8px;
    padding: 0.7rem 0.85rem;
    font-size: 0.94rem;
    display: flex;
    gap: 0.6rem;
    align-items: flex-start;
    border: 1px solid;
  }

  .branch .tag {
    flex: none;
    font-weight: 700;
    font-size: 0.78rem;
    letter-spacing: 0.06em;
    text-transform: uppercase;
    padding: 0.1rem 0.5rem;
    border-radius: 999px;
    border: 1.5px solid currentColor;
    margin-top: 0.1rem;
  }

  .branch.pass  { background: var(--pass-bg); color: var(--pass-ink); border-color: var(--pass-line); }
  .branch.next  { background: #eef2fb; color: #23387a; border-color: #8ea3d8; }
  .branch.fail  { background: var(--fail-bg); color: var(--fail-ink); border-color: var(--fail-line); }

  .branch strong { font-weight: 700; }

  /* Final outcome card */
  .final {
    margin-top: 0.2rem;
    background: var(--fail-bg);
    border: 1.5px solid var(--fail-line);
    border-radius: 10px;
    padding: 1rem 1.15rem;
    color: var(--fail-ink);
  }
  .final h2 { font-size: 1.05rem; }
  .final p { font-size: 0.94rem; margin-top: 0.35rem; }

  footer {
    margin-top: 2.5rem;
    font-size: 0.85rem;
    color: var(--ink-soft);
    border-top: 1px solid var(--line);
    padding-top: 1rem;
  }
  footer a { color: var(--accent); }

  a:focus-visible,
  :focus-visible {
    outline: 3px solid var(--focus);
    outline-offset: 2px;
    border-radius: 3px;
  }

  @media (prefers-reduced-motion: no-preference) {
    html { scroll-behavior: smooth; }
  }

  @media print {
    body { background: #fff; padding: 0.5rem; }
    .question, .branch, .final { break-inside: avoid; }
  }
</style>
</head>
<body>
<main>
  <header>
    <p class="eyebrow">WCAG 2.2 · Success Criterion 2.5.8 (Level AA)</p>
    <h1>Target Size (Minimum) decision chart</h1>
    <p class="intro">Work through each question in order for the interactive target you are evaluating. Stop as soon as you reach a green outcome (meets the criterion or qualifies for an exception) or the red outcome at the end (fails the criterion).</p>
    <div class="swatch-row" aria-hidden="false">
      <span class="swatch" role="img" aria-label="A square rendered at the actual size of 24 by 24 CSS pixels"></span>
      <span class="swatch-circle" role="img" aria-label="A circle rendered at the actual diameter of 24 CSS pixels"></span>
      <p><strong>True-size reference:</strong> the square is exactly 24 × 24 CSS pixels; the circle has a 24 CSS pixel diameter. Compare your target against these when viewed at 100% zoom.</p>
    </div>
  </header>

  <ol class="flow">
    <li class="step">
      <div class="question">
        <h2>Is the target at least 24 × 24 CSS pixels?</h2>
        <p class="note">Measure the full clickable/tappable area, not just the visible icon or text.</p>
      </div>
      <div class="branches">
        <p class="branch pass"><span class="tag">Yes</span><span><strong>Meets the criterion.</strong> No further checks needed for this target.</span></p>
        <p class="branch next"><span class="tag">No</span><span>Continue to step 2 to check whether an exception applies.</span></p>
      </div>
    </li>

    <li class="step">
      <div class="question">
        <h2>Can the target be centered in a 24 CSS pixel diameter circle that doesn't intersect any other target or another target's circle?</h2>
        <p class="note">This is the <em>spacing</em> exception: a small target is acceptable if it has enough clear space around it.</p>
      </div>
      <div class="branches">
        <p class="branch pass"><span class="tag">Yes</span><span><strong>Allowed exception (Spacing).</strong> The target passes this criterion.</span></p>
        <p class="branch next"><span class="tag">No</span><span>Continue to step 3.</span></p>
      </div>
    </li>

    <li class="step">
      <div class="question">
        <h2>Does another control on the same page perform the exact same function, and does that control meet the size requirement?</h2>
        <p class="note">This is the <em>equivalent</em> exception: an adequately sized alternative achieves the same outcome.</p>
      </div>
      <div class="branches">
        <p class="branch pass"><span class="tag">Yes</span><span><strong>Allowed exception (Equivalent).</strong> The target passes this criterion.</span></p>
        <p class="branch next"><span class="tag">No</span><span>Continue to step 4.</span></p>
      </div>
    </li>

    <li class="step">
      <div class="question">
        <h2>Is the target inline within a sentence or block of text?</h2>
        <p class="note">Example: a text hyperlink inside a paragraph, whose size is constrained by the line height of the surrounding text.</p>
      </div>
      <div class="branches">
        <p class="branch pass"><span class="tag">Yes</span><span><strong>Allowed exception (Inline).</strong> The target passes this criterion.</span></p>
        <p class="branch next"><span class="tag">No</span><span>Continue to step 5.</span></p>
      </div>
    </li>

    <li class="step">
      <div class="question">
        <h2>Is the target's size determined by the user agent and not modified by the author?</h2>
        <p class="note">Example: a default browser control or plug-in element whose size the page author has not changed.</p>
      </div>
      <div class="branches">
        <p class="branch pass"><span class="tag">Yes</span><span><strong>Allowed exception (User agent control).</strong> The target passes this criterion.</span></p>
        <p class="branch next"><span class="tag">No</span><span>Continue to step 6.</span></p>
      </div>
    </li>

    <li class="step">
      <div class="question">
        <h2>Is a specific target size legally required, or essential to conveying the information?</h2>
        <p class="note">Example: pins on a map where precise placement is essential, or a size mandated by regulation.</p>
      </div>
      <div class="branches">
        <p class="branch pass"><span class="tag">Yes</span><span><strong>Allowed exception (Essential / legal).</strong> The target passes this criterion.</span></p>
        <p class="branch fail"><span class="tag">No</span><span>Continue to the outcome below.</span></p>
      </div>
      <div class="final" role="note">
        <h2>Does not meet the criterion</h2>
        <p>The target is smaller than 24 × 24 CSS pixels and no exception applies. <strong>Fix:</strong> increase the target to at least 24 × 24 CSS pixels, or add enough spacing so that it can be centered in a non-overlapping 24 CSS pixel circle.</p>
      </div>
    </li>
  </ol>

  <footer>
    <p>Based on <a href="https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html">Understanding SC 2.5.8: Target Size (Minimum)</a>. Outcomes are conveyed with text labels as well as color.</p>
  </footer>
</main>
</body>
</html>
