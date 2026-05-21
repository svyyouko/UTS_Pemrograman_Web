<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Script Presentasi — Bug Form Mahasiswa</title>
  <link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;700&family=Syne:wght@700;800&family=DM+Sans:wght@400;500;600&display=swap" rel="stylesheet">
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --bg:       #0a0a0f;
      --surface:  #13131a;
      --border:   #1e1e2e;
      --red:      #ff3b5c;
      --amber:    #ffb347;
      --green:    #00e5a0;
      --blue:     #4db8ff;
      --purple:   #b57aff;
      --text:     #e8e8f0;
      --muted:    #6b6b88;
      --mono:     'JetBrains Mono', monospace;
      --display:  'Syne', sans-serif;
      --body:     'DM Sans', sans-serif;
    }

    html { scroll-behavior: smooth; }

    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--body);
      min-height: 100vh;
      overflow-x: hidden;
    }

    /* ── PRINT / VIDEO EXPORT HINT ── */
    .top-bar {
      background: var(--surface);
      border-bottom: 1px solid var(--border);
      padding: 10px 24px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      position: sticky;
      top: 0;
      z-index: 100;
    }
    .top-bar-left { display: flex; align-items: center; gap: 12px; }
    .top-bar span { font-size: .78rem; color: var(--muted); font-family: var(--mono); }
    .dot { width: 10px; height: 10px; border-radius: 50%; }
    .dot-r { background: var(--red); }
    .dot-a { background: var(--amber); }
    .dot-g { background: var(--green); }
    .progress-bar {
      height: 2px;
      background: linear-gradient(90deg, var(--red), var(--purple));
      width: 0%;
      transition: width .2s;
      position: fixed;
      top: 0; left: 0;
      z-index: 200;
    }

    /* ── SCENE WRAPPER ── */
    .scene {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      padding: 60px 48px;
      position: relative;
      border-bottom: 1px solid var(--border);
      overflow: hidden;
    }
    .scene::before {
      content: attr(data-scene);
      position: absolute;
      top: 28px; right: 40px;
      font-family: var(--mono);
      font-size: .72rem;
      color: var(--muted);
      letter-spacing: .12em;
    }

    /* scene number glow dot */
    .scene-num {
      font-family: var(--mono);
      font-size: .72rem;
      color: var(--muted);
      margin-bottom: 16px;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    .scene-num::before {
      content: '';
      display: block;
      width: 6px; height: 6px;
      border-radius: 50%;
      background: var(--red);
      box-shadow: 0 0 8px var(--red);
      animation: blink 1.4s infinite;
    }
    @keyframes blink { 0%,100%{opacity:1} 50%{opacity:.2} }

    /* ── SCENE: OPENING ── */
    .scene-opening {
      background: radial-gradient(ellipse at 30% 50%, #1a0a1e 0%, var(--bg) 60%);
      align-items: center;
      text-align: center;
    }
    .scene-opening .big-title {
      font-family: var(--display);
      font-size: clamp(2.8rem, 6vw, 5rem);
      font-weight: 800;
      line-height: 1.05;
      background: linear-gradient(135deg, var(--red) 0%, var(--purple) 50%, var(--blue) 100%);
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin-bottom: 20px;
    }
    .scene-opening .subtitle {
      font-size: 1.1rem;
      color: var(--muted);
      max-width: 520px;
      line-height: 1.7;
    }
    .badge-row {
      display: flex;
      gap: 10px;
      justify-content: center;
      flex-wrap: wrap;
      margin-top: 28px;
    }
    .badge {
      padding: 6px 16px;
      border-radius: 20px;
      font-size: .8rem;
      font-weight: 600;
      font-family: var(--mono);
      border: 1px solid;
    }
    .badge-red    { color: var(--red);    border-color: var(--red);    background: rgba(255,59,92,.08); }
    .badge-amber  { color: var(--amber);  border-color: var(--amber);  background: rgba(255,179,71,.08); }
    .badge-green  { color: var(--green);  border-color: var(--green);  background: rgba(0,229,160,.08); }

    /* ── SCENE: BUG ── */
    .scene-bug { background: var(--bg); }

    .bug-header {
      display: flex;
      align-items: flex-start;
      gap: 20px;
      margin-bottom: 32px;
    }
    .bug-index {
      font-family: var(--mono);
      font-size: 3rem;
      font-weight: 700;
      line-height: 1;
      min-width: 64px;
    }
    .bug-index.fatal  { color: var(--red); }
    .bug-index.medium { color: var(--amber); }
    .bug-index.minor  { color: var(--green); }

    .bug-title-wrap {}
    .severity-pill {
      display: inline-block;
      padding: 3px 12px;
      border-radius: 4px;
      font-size: .72rem;
      font-family: var(--mono);
      font-weight: 700;
      letter-spacing: .08em;
      margin-bottom: 8px;
    }
    .pill-fatal  { background: rgba(255,59,92,.18);  color: var(--red); }
    .pill-medium { background: rgba(255,179,71,.18); color: var(--amber); }
    .pill-minor  { background: rgba(0,229,160,.18);  color: var(--green); }

    .bug-name {
      font-family: var(--display);
      font-size: clamp(1.6rem, 3.5vw, 2.6rem);
      font-weight: 800;
      line-height: 1.1;
      color: var(--text);
    }

    /* two-col layout */
    .bug-body {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 24px;
    }
    .bug-body.full { grid-template-columns: 1fr; }

    /* narration box */
    .narration {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 20px 24px;
    }
    .narration-label {
      font-family: var(--mono);
      font-size: .68rem;
      color: var(--muted);
      letter-spacing: .1em;
      margin-bottom: 12px;
    }
    .narration p {
      font-size: .95rem;
      line-height: 1.75;
      color: #c8c8e0;
    }
    .narration p + p { margin-top: 10px; }

    /* visual cue */
    .visual-cue {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 20px 24px;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .vc-label {
      font-family: var(--mono);
      font-size: .68rem;
      color: var(--muted);
      letter-spacing: .1em;
    }

    /* code snippet */
    .code-block {
      background: #080810;
      border: 1px solid var(--border);
      border-radius: 8px;
      padding: 14px 16px;
      font-family: var(--mono);
      font-size: .8rem;
      line-height: 1.8;
      overflow-x: auto;
      flex: 1;
    }
    .code-block .ln { color: #333355; margin-right: 16px; user-select: none; }
    .code-ok   { color: #6b6b88; }
    .code-bad  { color: var(--red); background: rgba(255,59,92,.08); display: block; border-radius: 3px; }
    .code-fix  { color: var(--green); }
    .code-tag  { color: var(--blue); }
    .code-attr { color: var(--amber); }
    .code-val  { color: var(--green); }
    .code-comment { color: #444466; font-style: italic; }

    /* action items */
    .action-list {
      list-style: none;
      display: flex;
      flex-direction: column;
      gap: 8px;
    }
    .action-list li {
      display: flex;
      align-items: flex-start;
      gap: 10px;
      font-size: .88rem;
      line-height: 1.5;
      color: #c8c8e0;
    }
    .action-list li::before {
      content: '→';
      color: var(--purple);
      font-family: var(--mono);
      font-weight: 700;
      flex-shrink: 0;
      margin-top: 1px;
    }

    /* timeline bar */
    .timeline-bar {
      display: flex;
      align-items: center;
      gap: 6px;
      font-family: var(--mono);
      font-size: .72rem;
      color: var(--muted);
      margin-top: 28px;
      padding-top: 20px;
      border-top: 1px solid var(--border);
    }
    .tl-item {
      padding: 4px 10px;
      border-radius: 4px;
      border: 1px solid var(--border);
      white-space: nowrap;
    }
    .tl-active { border-color: var(--purple); color: var(--purple); background: rgba(181,122,255,.08); }
    .tl-sep { color: var(--border); }

    /* ── SCENE: SUMMARY ── */
    .scene-summary { background: radial-gradient(ellipse at 70% 50%, #0d0a1e 0%, var(--bg) 60%); }

    .summary-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 14px;
      margin: 28px 0;
    }
    .summary-card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 16px;
      display: flex;
      flex-direction: column;
      gap: 6px;
    }
    .sc-num { font-family: var(--mono); font-size: .72rem; color: var(--muted); }
    .sc-name { font-size: .88rem; font-weight: 600; color: var(--text); }
    .sc-sev { font-family: var(--mono); font-size: .72rem; font-weight: 700; }
    .sc-sev.fatal  { color: var(--red); }
    .sc-sev.medium { color: var(--amber); }
    .sc-sev.minor  { color: var(--green); }

    .closing-title {
      font-family: var(--display);
      font-size: clamp(1.6rem, 3vw, 2.4rem);
      font-weight: 800;
      margin-bottom: 12px;
    }

    /* ── SCENE: CLOSING ── */
    .scene-closing {
      background: radial-gradient(ellipse at 50% 60%, #1a0a0e 0%, var(--bg) 60%);
      align-items: center;
      text-align: center;
    }
    .final-title {
      font-family: var(--display);
      font-size: clamp(3rem, 7vw, 6rem);
      font-weight: 800;
      background: linear-gradient(135deg, var(--red), var(--purple));
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      background-clip: text;
      margin-bottom: 16px;
    }
    .final-sub { font-size: 1rem; color: var(--muted); max-width: 420px; line-height: 1.7; }

    /* ── DECORATIVE GRID ── */
    .grid-overlay {
      position: absolute;
      inset: 0;
      background-image:
        linear-gradient(rgba(255,255,255,.015) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,.015) 1px, transparent 1px);
      background-size: 48px 48px;
      pointer-events: none;
    }

    @media (max-width: 700px) {
      .scene { padding: 40px 20px; }
      .bug-body { grid-template-columns: 1fr; }
      .bug-header { flex-direction: column; gap: 10px; }
    }
  </style>
</head>
<body>

<div class="progress-bar" id="progress"></div>

<!-- TOP BAR -->
<div class="top-bar">
  <div class="top-bar-left">
    <div class="dot dot-r"></div>
    <div class="dot dot-a"></div>
    <div class="dot dot-g"></div>
    <span>script-presentasi-bug.html</span>
  </div>
  <span>11 bugs · Form Data Mahasiswa</span>
</div>


<!-- ═══════════════════════════════════════════
     SCENE 00 — OPENING
══════════════════════════════════════════════ -->
<section class="scene scene-opening" data-scene="SCENE 00 · OPENING">
  <div class="grid-overlay"></div>
  <div class="scene-num">SCENE 00 · OPENING · DURASI: 0:00 – 0:30</div>
  <div class="big-title">11 Bug<br>Dalam 1 File HTML</div>
  <p class="subtitle">Analisis mendalam terhadap kode Form Data Mahasiswa — menemukan dan memahami setiap kesalahan dari yang paling fatal hingga minor.</p>
  <div class="badge-row">
    <span class="badge badge-red">🔴 5 Fatal</span>
    <span class="badge badge-amber">🟡 4 Sedang</span>
    <span class="badge badge-green">🟢 2 Minor</span>
  </div>

  <div class="narration" style="max-width:600px;margin-top:36px">
    <div class="narration-label">🎙 NARASI PEMBUKA</div>
    <p>"Halo semua! Hari ini kita akan membedah sebuah file HTML — Form Data Mahasiswa — dan menemukan tidak kurang dari <strong style="color:var(--red)">11 bug</strong> di dalamnya. Mulai dari bug yang membuat halaman sama sekali tidak berfungsi, hingga kebiasaan buruk yang perlu diperbaiki. Yuk kita mulai!"</p>
  </div>

  <!-- VISUAL CUE -->
  <div style="margin-top:24px;max-width:600px">
    <div class="vc-label">🎬 VISUAL — tampilkan file HTML di editor, sorot judul file</div>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 01 — BUG #1
══════════════════════════════════════════════ -->
<section class="scene scene-bug" data-scene="SCENE 01 · BUG #1">
  <div class="scene-num">SCENE 01 · DURASI: 0:30 – 1:15</div>

  <div class="bug-header">
    <div class="bug-index fatal">#1</div>
    <div class="bug-title-wrap">
      <div class="severity-pill pill-fatal">🔴 FATAL</div>
      <div class="bug-name">&lt;style&gt; di Luar &lt;head&gt;</div>
    </div>
  </div>

  <div class="bug-body">
    <div class="narration">
      <div class="narration-label">🎙 NARASI</div>
      <p>"Bug pertama langsung terlihat di struktur HTML-nya. Perhatikan — tag <code style="color:var(--blue)">&lt;head&gt;</code> sudah ditutup, tapi <code style="color:var(--blue)">&lt;style&gt;</code> ditulis di bawahnya. Ini invalid HTML!"</p>
      <p>"Browser mungkin masih me-render-nya karena browser sangat toleran, tapi ini melanggar standar HTML dan bisa menyebabkan masalah di browser tertentu atau tools validasi."</p>
      <ul class="action-list" style="margin-top:14px">
        <li>Pindahkan seluruh blok <code>&lt;style&gt;</code> ke dalam <code>&lt;head&gt;</code></li>
        <li>Pastikan urutan: meta → title → style → link</li>
      </ul>
    </div>
    <div class="visual-cue">
      <div class="vc-label">🎬 VISUAL — sorot baris kode</div>
      <div class="code-block">
<span class="code-ok"><span class="ln">1</span><span class="code-tag">&lt;head&gt;</span></span>
<span class="code-ok"><span class="ln">2</span></span>
<span class="code-bad"><span class="ln">3</span><span class="code-tag">&lt;/head&gt;</span>  <span class="code-comment">← head ditutup</span></span>
<span class="code-bad"><span class="ln">4</span><span class="code-tag">&lt;style&gt;</span>  <span class="code-comment">← ❌ style di luar!</span></span>
<span class="code-ok"><span class="ln">5</span>  body { ... }</span>
<span class="code-bad"><span class="ln">6</span><span class="code-tag">&lt;/style&gt;</span></span>
      </div>
      <div class="vc-label" style="margin-top:8px">✅ SOLUSI</div>
      <div class="code-block">
<span class="code-fix"><span class="ln">1</span><span class="code-tag">&lt;head&gt;</span></span>
<span class="code-fix"><span class="ln">2</span>  <span class="code-tag">&lt;style&gt;</span></span>
<span class="code-fix"><span class="ln">3</span>    body { ... }</span>
<span class="code-fix"><span class="ln">4</span>  <span class="code-tag">&lt;/style&gt;</span></span>
<span class="code-fix"><span class="ln">5</span><span class="code-tag">&lt;/head&gt;</span></span>
      </div>
    </div>
  </div>

  <div class="timeline-bar">
    <span class="tl-item tl-active">#1 Style</span><span class="tl-sep">›</span>
    <span class="tl-item">#2 Head</span><span class="tl-sep">›</span>
    <span class="tl-item">#3 Dropdown</span><span class="tl-sep">›</span>
    <span class="tl-item">#4 JS</span><span class="tl-sep">›</span>
    <span class="tl-item">...</span>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 02 — BUG #2
══════════════════════════════════════════════ -->
<section class="scene scene-bug" data-scene="SCENE 02 · BUG #2">
  <div class="scene-num">SCENE 02 · DURASI: 1:15 – 2:00</div>

  <div class="bug-header">
    <div class="bug-index fatal">#2</div>
    <div class="bug-title-wrap">
      <div class="severity-pill pill-fatal">🔴 FATAL</div>
      <div class="bug-name">&lt;head&gt; Kosong Total</div>
    </div>
  </div>

  <div class="bug-body">
    <div class="narration">
      <div class="narration-label">🎙 NARASI</div>
      <p>"Bug kedua: isi <code>&lt;head&gt;</code> sama sekali kosong. Tidak ada <code>charset</code>, tidak ada <code>title</code>, tidak ada <code>viewport</code>."</p>
      <p>"Tanpa <code>charset UTF-8</code>, teks Indonesia seperti 'Januari', 'Februari' bisa muncul sebagai karakter aneh — ini disebut <strong style="color:var(--red)">mojibake</strong>. Tanpa viewport, tampilan di HP berantakan."</p>
      <ul class="action-list" style="margin-top:14px">
        <li>Tambahkan <code>&lt;meta charset="UTF-8"&gt;</code></li>
        <li>Tambahkan <code>&lt;meta name="viewport" ...&gt;</code> untuk mobile</li>
        <li>Tambahkan <code>&lt;title&gt;</code> untuk tab browser</li>
      </ul>
    </div>
    <div class="visual-cue">
      <div class="vc-label">🎬 VISUAL</div>
      <div class="code-block">
<span class="code-bad"><span class="ln">1</span><span class="code-tag">&lt;head&gt;</span></span>
<span class="code-bad"><span class="ln">2</span>  <span class="code-comment">← kosong!</span></span>
<span class="code-bad"><span class="ln">3</span><span class="code-tag">&lt;/head&gt;</span></span>
      </div>
      <div class="vc-label" style="margin-top:8px">✅ SOLUSI</div>
      <div class="code-block">
<span class="code-fix"><span class="ln">1</span><span class="code-tag">&lt;head&gt;</span></span>
<span class="code-fix"><span class="ln">2</span>  <span class="code-tag">&lt;meta</span> <span class="code-attr">charset</span>=<span class="code-val">"UTF-8"</span><span class="code-tag">&gt;</span></span>
<span class="code-fix"><span class="ln">3</span>  <span class="code-tag">&lt;meta</span> <span class="code-attr">name</span>=<span class="code-val">"viewport"</span></span>
<span class="code-fix"><span class="ln">4</span>    <span class="code-attr">content</span>=<span class="code-val">"width=device-width"</span><span class="code-tag">&gt;</span></span>
<span class="code-fix"><span class="ln">5</span>  <span class="code-tag">&lt;title&gt;</span>Form Mahasiswa<span class="code-tag">&lt;/title&gt;</span></span>
<span class="code-fix"><span class="ln">6</span><span class="code-tag">&lt;/head&gt;</span></span>
      </div>
    </div>
  </div>

  <div class="timeline-bar">
    <span class="tl-item">#1 Style</span><span class="tl-sep">›</span>
    <span class="tl-item tl-active">#2 Head</span><span class="tl-sep">›</span>
    <span class="tl-item">#3 Dropdown</span><span class="tl-sep">›</span>
    <span class="tl-item">#4 JS</span><span class="tl-sep">›</span>
    <span class="tl-item">...</span>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 03 — BUG #3
══════════════════════════════════════════════ -->
<section class="scene scene-bug" data-scene="SCENE 03 · BUG #3">
  <div class="scene-num">SCENE 03 · DURASI: 2:00 – 2:50</div>

  <div class="bug-header">
    <div class="bug-index fatal">#3</div>
    <div class="bug-title-wrap">
      <div class="severity-pill pill-fatal">🔴 FATAL</div>
      <div class="bug-name">Dropdown Tanggal &amp; Tahun Kosong</div>
    </div>
  </div>

  <div class="bug-body">
    <div class="narration">
      <div class="narration-label">🎙 NARASI</div>
      <p>"Bug ketiga cukup memalukan — dropdown tanggal dan tahun tidak punya isi sama sekali. Tidak ada <code>&lt;option&gt;</code> di dalamnya, dan tidak ada JavaScript yang mengisinya."</p>
      <p>"Kalau dibuka di browser, dropdown-nya muncul tapi isinya kosong. User tidak bisa memilih tanggal atau tahun apapun."</p>
      <ul class="action-list" style="margin-top:14px">
        <li>Isi tanggal (1–31) dengan loop JavaScript</li>
        <li>Isi tahun (misal 2026–1970) dengan loop JavaScript</li>
      </ul>
    </div>
    <div class="visual-cue">
      <div class="vc-label">🎬 VISUAL</div>
      <div class="code-block">
<span class="code-bad"><span class="ln">1</span><span class="code-comment">/* ❌ KOSONG! */</span></span>
<span class="code-bad"><span class="ln">2</span><span class="code-tag">&lt;select</span> <span class="code-attr">id</span>=<span class="code-val">"tanggal"</span><span class="code-tag">&gt;&lt;/select&gt;</span></span>
<span class="code-bad"><span class="ln">3</span><span class="code-tag">&lt;select</span> <span class="code-attr">id</span>=<span class="code-val">"tahun"</span><span class="code-tag">&gt;&lt;/select&gt;</span></span>
      </div>
      <div class="vc-label" style="margin-top:8px">✅ SOLUSI (JavaScript)</div>
      <div class="code-block">
<span class="code-fix"><span class="ln">1</span><span class="code-comment">// Isi tanggal 1–31</span></span>
<span class="code-fix"><span class="ln">2</span>for (let i = 1; i &lt;= 31; i++) {</span>
<span class="code-fix"><span class="ln">3</span>  const o = document.createElement('option');</span>
<span class="code-fix"><span class="ln">4</span>  o.value = String(i).padStart(2,'0');</span>
<span class="code-fix"><span class="ln">5</span>  o.textContent = i;</span>
<span class="code-fix"><span class="ln">6</span>  selTgl.appendChild(o);</span>
<span class="code-fix"><span class="ln">7</span>}</span>
      </div>
    </div>
  </div>

  <div class="timeline-bar">
    <span class="tl-item">#1</span><span class="tl-sep">›</span>
    <span class="tl-item">#2</span><span class="tl-sep">›</span>
    <span class="tl-item tl-active">#3 Dropdown</span><span class="tl-sep">›</span>
    <span class="tl-item">#4 JS</span><span class="tl-sep">›</span>
    <span class="tl-item">...</span>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 04 — BUG #4
══════════════════════════════════════════════ -->
<section class="scene scene-bug" data-scene="SCENE 04 · BUG #4">
  <div class="scene-num">SCENE 04 · DURASI: 2:50 – 4:00</div>

  <div class="bug-header">
    <div class="bug-index fatal">#4</div>
    <div class="bug-title-wrap">
      <div class="severity-pill pill-fatal">🔴 FATAL</div>
      <div class="bug-name">Tidak Ada JavaScript Sama Sekali</div>
    </div>
  </div>

  <div class="bug-body bug-body full">
    <div class="narration">
      <div class="narration-label">🎙 NARASI</div>
      <p>"Ini bug terbesar dan paling fatal. Tidak ada satu baris JavaScript pun di seluruh file. Akibatnya — form ini tidak bisa melakukan <strong style="color:var(--red)">apapun</strong>."</p>
      <p>"Klik Submit? Data tidak masuk ke tabel. Tabel? Selalu kosong selamanya. Tombol Edit dan Hapus? Tidak ada. Ini seperti membuat mobil tapi lupa pasang mesin."</p>
    </div>

    <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px;margin-top:4px">
      <div class="visual-cue">
        <div class="vc-label">❌ YANG HILANG</div>
        <ul class="action-list">
          <li>Submit handler</li>
          <li>Render tabel</li>
          <li>Tombol Edit</li>
          <li>Tombol Hapus</li>
          <li>Isi dropdown</li>
          <li>Validasi form</li>
        </ul>
      </div>
      <div class="visual-cue" style="grid-column:span 2">
        <div class="vc-label">✅ SOLUSI MINIMAL</div>
        <div class="code-block" style="font-size:.73rem">
<span class="code-fix"><span class="ln"> 1</span>document.getElementById('formMahasiswa')</span>
<span class="code-fix"><span class="ln"> 2</span>  .addEventListener('submit', function(e) {</span>
<span class="code-fix"><span class="ln"> 3</span>    e.preventDefault();</span>
<span class="code-fix"><span class="ln"> 4</span>    const nim  = document.getElementById('nim').value;</span>
<span class="code-fix"><span class="ln"> 5</span>    const nama = document.getElementById('nama').value;</span>
<span class="code-fix"><span class="ln"> 6</span>    <span class="code-comment">// validasi, simpan, render...</span></span>
<span class="code-fix"><span class="ln"> 7</span>  });</span>
        </div>
      </div>
    </div>
  </div>

  <div class="timeline-bar">
    <span class="tl-item">#1</span><span class="tl-sep">›</span>
    <span class="tl-item">#2</span><span class="tl-sep">›</span>
    <span class="tl-item">#3</span><span class="tl-sep">›</span>
    <span class="tl-item tl-active">#4 JavaScript</span><span class="tl-sep">›</span>
    <span class="tl-item">#5 #6 #7 #8</span><span class="tl-sep">›</span>
    <span class="tl-item">...</span>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 05 — BUG #5 + #6 + #7 + #8 (GROUP)
══════════════════════════════════════════════ -->
<section class="scene scene-bug" data-scene="SCENE 05 · BUG #5–#8">
  <div class="scene-num">SCENE 05 · DURASI: 4:00 – 5:30 · GROUP: Bug Logika &amp; Fungsional</div>

  <div class="bug-header">
    <div class="bug-index medium" style="font-size:2rem">#5–8</div>
    <div class="bug-title-wrap">
      <div class="severity-pill pill-medium">🟡 SEDANG</div>
      <div class="bug-name">4 Bug Logika &amp; Fungsional</div>
    </div>
  </div>

  <div style="display:grid;grid-template-columns:1fr 1fr;gap:14px">

    <div class="narration">
      <div class="narration-label">🔴 BUG #5 — Reset Tidak Lengkap</div>
      <p><code>type="reset"</code> hanya mereset field HTML bawaan. State JavaScript seperti mode edit, index baris yang diedit, dan highlight tabel <strong>tidak ikut ter-reset</strong>.</p>
      <ul class="action-list" style="margin-top:10px">
        <li>Buat fungsi <code>resetForm()</code> di JS yang reset semua state</li>
      </ul>
    </div>

    <div class="narration">
      <div class="narration-label">🟡 BUG #6 — Validasi Radio Tidak Ada</div>
      <p>Radio button Jenis Kelamin tidak divalidasi. User bisa klik Submit tanpa memilih Pria atau Wanita, dan data yang tersimpan akan <strong>kosong/undefined</strong>.</p>
      <ul class="action-list" style="margin-top:10px">
        <li>Cek <code>querySelector('input[name="jk"]:checked')</code></li>
        <li>Tampilkan pesan error jika belum dipilih</li>
      </ul>
    </div>

    <div class="narration">
      <div class="narration-label">🟡 BUG #7 — Tidak Ada Cek NIM Duplikat</div>
      <p>NIM yang sama bisa diinput berkali-kali. Tidak ada logika untuk mendeteksi duplikat, sehingga tabel bisa penuh dengan data ganda yang menyesatkan.</p>
      <ul class="action-list" style="margin-top:10px">
        <li>Gunakan <code>findIndex(m =&gt; m.nim === nim)</code> sebelum simpan</li>
      </ul>
    </div>

    <div class="narration">
      <div class="narration-label">🔴 BUG #8 — Kolom Aksi Kosong</div>
      <p>Header tabel punya kolom "Aksi", tapi di dalam <code>&lt;tbody&gt;</code> tidak ada satu pun tombol Edit atau Hapus. Kolom ini hanya pajangan.</p>
      <ul class="action-list" style="margin-top:10px">
        <li>Tambahkan tombol Edit + Hapus saat render tiap baris tabel</li>
      </ul>
    </div>

  </div>

  <div class="timeline-bar">
    <span class="tl-item">#1–4</span><span class="tl-sep">›</span>
    <span class="tl-item tl-active">#5 Reset · #6 Radio · #7 Duplikat · #8 Aksi</span><span class="tl-sep">›</span>
    <span class="tl-item">#9–11</span>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 06 — BUG #9 + #10 + #11 (GROUP)
══════════════════════════════════════════════ -->
<section class="scene scene-bug" data-scene="SCENE 06 · BUG #9–#11">
  <div class="scene-num">SCENE 06 · DURASI: 5:30 – 6:30 · GROUP: Bug CSS &amp; Aksesibilitas</div>

  <div class="bug-header">
    <div class="bug-index minor" style="font-size:2rem">#9–11</div>
    <div class="bug-title-wrap">
      <div class="severity-pill pill-minor">🟢 MINOR</div>
      <div class="bug-name">3 Bug CSS &amp; Aksesibilitas</div>
    </div>
  </div>

  <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:14px">

    <div class="narration">
      <div class="narration-label">🟢 BUG #9 — CSS img Tak Berguna</div>
      <div class="code-block" style="margin:10px 0;font-size:.78rem">
<span class="code-bad">img {</span>
<span class="code-bad">  cursor: pointer;</span>
<span class="code-bad">  <span class="code-comment">/* tidak ada &lt;img&gt; di halaman! */</span></span>
<span class="code-bad">}</span>
      </div>
      <p style="font-size:.85rem;color:#c8c8e0;line-height:1.6">Rule ini tidak pernah ter-apply karena tidak ada elemen gambar di halaman. Kode mati yang membingungkan.</p>
    </div>

    <div class="narration">
      <div class="narration-label">🟡 BUG #10 — Label Tanpa Atribut for</div>
      <div class="code-block" style="margin:10px 0;font-size:.78rem">
<span class="code-bad"><span class="code-tag">&lt;label&gt;</span>NIM<span class="code-tag">&lt;/label&gt;</span></span>
<span class="code-bad"><span class="code-tag">&lt;input</span> <span class="code-attr">id</span>=<span class="code-val">"nim"</span><span class="code-tag">&gt;</span></span>
<span class="code-comment"> </span>
<span class="code-fix"><span class="code-tag">&lt;label</span> <span class="code-attr">for</span>=<span class="code-val">"nim"</span><span class="code-tag">&gt;</span>NIM<span class="code-tag">&lt;/label&gt;</span></span>
<span class="code-fix"><span class="code-tag">&lt;input</span> <span class="code-attr">id</span>=<span class="code-val">"nim"</span><span class="code-tag">&gt;</span></span>
      </div>
      <p style="font-size:.85rem;color:#c8c8e0;line-height:1.6">Klik label tidak fokus ke input. Buruk untuk aksesibilitas dan user experience.</p>
    </div>

    <div class="narration">
      <div class="narration-label">🟢 BUG #11 — Layout Pakai &lt;br&gt;</div>
      <div class="code-block" style="margin:10px 0;font-size:.78rem">
<span class="code-bad"><span class="code-tag">&lt;label&gt;</span>Jenis Kelamin<span class="code-tag">&lt;/label&gt;&lt;br&gt;</span></span>
<span class="code-bad">...radio...</span>
<span class="code-bad"><span class="code-tag">&lt;br&gt;&lt;br&gt;</span></span>
<span class="code-comment"> </span>
<span class="code-fix"><span class="code-comment">/* gunakan CSS flexbox/margin */</span></span>
<span class="code-fix">.form-group { margin-bottom: 16px; }</span>
      </div>
      <p style="font-size:.85rem;color:#c8c8e0;line-height:1.6">Penggunaan <code>&lt;br&gt;</code> untuk spasi adalah praktik lama yang tidak fleksibel.</p>
    </div>

  </div>

  <div class="timeline-bar">
    <span class="tl-item">#1–8</span><span class="tl-sep">›</span>
    <span class="tl-item tl-active">#9 CSS · #10 Label · #11 Layout</span><span class="tl-sep">›</span>
    <span class="tl-item">Summary</span>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 07 — SUMMARY
══════════════════════════════════════════════ -->
<section class="scene scene-summary" data-scene="SCENE 07 · SUMMARY">
  <div class="grid-overlay"></div>
  <div class="scene-num">SCENE 07 · DURASI: 6:30 – 7:15 · RINGKASAN</div>

  <div class="closing-title">Rekap 11 Bug</div>

  <div class="summary-grid">
    <div class="summary-card">
      <div class="sc-num">BUG #1</div>
      <div class="sc-name">&lt;style&gt; di Luar &lt;head&gt;</div>
      <div class="sc-sev fatal">🔴 FATAL</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #2</div>
      <div class="sc-name">&lt;head&gt; Kosong</div>
      <div class="sc-sev fatal">🔴 FATAL</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #3</div>
      <div class="sc-name">Dropdown Kosong</div>
      <div class="sc-sev fatal">🔴 FATAL</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #4</div>
      <div class="sc-name">Tidak Ada JavaScript</div>
      <div class="sc-sev fatal">🔴 FATAL</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #5</div>
      <div class="sc-name">Reset Tidak Lengkap</div>
      <div class="sc-sev medium">🟡 SEDANG</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #6</div>
      <div class="sc-name">Validasi Radio Kosong</div>
      <div class="sc-sev medium">🟡 SEDANG</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #7</div>
      <div class="sc-name">NIM Duplikat</div>
      <div class="sc-sev medium">🟡 SEDANG</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #8</div>
      <div class="sc-name">Kolom Aksi Kosong</div>
      <div class="sc-sev fatal">🔴 FATAL</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #9</div>
      <div class="sc-name">CSS img Tak Berguna</div>
      <div class="sc-sev minor">🟢 MINOR</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #10</div>
      <div class="sc-name">Label Tanpa for</div>
      <div class="sc-sev medium">🟡 SEDANG</div>
    </div>
    <div class="summary-card">
      <div class="sc-num">BUG #11</div>
      <div class="sc-name">Layout Pakai &lt;br&gt;</div>
      <div class="sc-sev minor">🟢 MINOR</div>
    </div>
  </div>

  <div class="narration" style="max-width:700px">
    <div class="narration-label">🎙 NARASI RINGKASAN</div>
    <p>"Dari 11 bug ini, <strong style="color:var(--red)">5 di antaranya Fatal</strong> — artinya halaman benar-benar tidak bisa digunakan. Yang paling kritis adalah tidak adanya JavaScript, karena itu membuat form ini hanya tampilan saja tanpa fungsi. Kabar baiknya — semua bug ini sudah diperbaiki di versi yang telah dilengkapi!"</p>
  </div>
</section>


<!-- ═══════════════════════════════════════════
     SCENE 08 — CLOSING
══════════════════════════════════════════════ -->
<section class="scene scene-closing" data-scene="SCENE 08 · CLOSING">
  <div class="grid-overlay"></div>
  <div class="scene-num">SCENE 08 · DURASI: 7:15 – 7:45 · CLOSING</div>

  <div class="final-title">Bug Fixed. ✓</div>
  <p class="final-sub">"Setiap bug adalah kesempatan belajar. Sekarang kamu tahu apa yang harus diperhatikan saat menulis HTML — dari struktur, fungsionalitas, validasi, hingga aksesibilitas."</p>

  <div class="badge-row" style="margin-top:28px">
    <span class="badge badge-red">5 Fatal → Diperbaiki</span>
    <span class="badge badge-amber">4 Sedang → Diperbaiki</span>
    <span class="badge badge-green">2 Minor → Diperbaiki</span>
  </div>

  <div class="narration" style="max-width:540px;margin-top:32px">
    <div class="narration-label">🎙 NARASI PENUTUP</div>
    <p>"Terima kasih sudah menonton! Jangan lupa selalu validasi HTML kamu, tambahkan JavaScript yang dibutuhkan, dan perhatikan aksesibilitas. Sampai jumpa di video berikutnya!"</p>
  </div>
</section>


<script>
  // Progress bar scroll
  window.addEventListener('scroll', () => {
    const scrollTop = window.scrollY;
    const docHeight = document.body.scrollHeight - window.innerHeight;
    const pct = docHeight > 0 ? (scrollTop / docHeight) * 100 : 0;
    document.getElementById('progress').style.width = pct + '%';
  });
</script>
</body>
</html>
