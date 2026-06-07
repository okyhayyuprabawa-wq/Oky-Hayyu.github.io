<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Simulasi pH Hidrolisis Garam — Kualitas Air Bersih</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,300&display=swap');

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0a0e1a;
    --surface: #111827;
    --surface2: #1a2235;
    --surface3: #1f2d45;
    --border: rgba(99,179,237,0.12);
    --border2: rgba(99,179,237,0.22);
    --accent: #38bdf8;
    --accent2: #0ea5e9;
    --teal: #2dd4bf;
    --green: #4ade80;
    --amber: #fbbf24;
    --red: #f87171;
    --purple: #a78bfa;
    --text: #e2e8f0;
    --text2: #94a3b8;
    --text3: #64748b;
    --radius: 12px;
    --radius-sm: 8px;
  }

  html { scroll-behavior: smooth; }

  body {
    font-family: 'DM Sans', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    line-height: 1.6;
  }

  /* ── HEADER ── */
  header {
    background: linear-gradient(135deg, #0c1525 0%, #0f1e35 50%, #0a1628 100%);
    border-bottom: 1px solid var(--border2);
    padding: 0 2rem;
    position: sticky;
    top: 0;
    z-index: 100;
    backdrop-filter: blur(12px);
  }
  .header-inner {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 1rem 0;
    gap: 1rem;
  }
  .logo {
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .logo-icon {
    width: 38px; height: 38px;
    background: linear-gradient(135deg, var(--accent), var(--teal));
    border-radius: 10px;
    display: flex; align-items: center; justify-content: center;
    font-size: 18px;
  }
  .logo-text { font-family: 'Syne', sans-serif; font-size: 15px; font-weight: 700; color: var(--text); line-height: 1.2; }
  .logo-sub { font-size: 11px; color: var(--text2); font-weight: 400; }
  .header-badge {
    font-size: 11px;
    font-weight: 600;
    padding: 4px 12px;
    border-radius: 20px;
    background: rgba(56,189,248,0.1);
    border: 1px solid rgba(56,189,248,0.25);
    color: var(--accent);
    letter-spacing: 0.04em;
  }

  /* ── HERO ── */
  .hero {
    max-width: 1200px;
    margin: 0 auto;
    padding: 3rem 2rem 2rem;
    display: grid;
    grid-template-columns: 1fr auto;
    align-items: center;
    gap: 2rem;
  }
  .hero-eyebrow {
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.12em;
    color: var(--accent);
    font-weight: 600;
    margin-bottom: 10px;
  }
  .hero-title {
    font-family: 'Syne', sans-serif;
    font-size: clamp(26px, 4vw, 40px);
    font-weight: 800;
    line-height: 1.15;
    color: #fff;
    margin-bottom: 12px;
  }
  .hero-title span { color: var(--accent); }
  .hero-desc { font-size: 14px; color: var(--text2); max-width: 500px; line-height: 1.7; }
  .sdg-pill {
    background: rgba(74,222,128,0.1);
    border: 1px solid rgba(74,222,128,0.25);
    color: var(--green);
    border-radius: 20px;
    padding: 6px 14px;
    font-size: 12px;
    font-weight: 600;
    white-space: nowrap;
  }

  /* ── MAIN LAYOUT ── */
  .main {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 2rem 4rem;
    display: grid;
    grid-template-columns: 340px 1fr;
    gap: 1.5rem;
    align-items: start;
  }

  /* ── PANELS ── */
  .panel {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.5rem;
    position: sticky;
    top: 80px;
  }
  .panel-title {
    font-family: 'Syne', sans-serif;
    font-size: 13px;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    color: var(--text2);
    margin-bottom: 1.25rem;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .panel-title::before {
    content: '';
    display: block;
    width: 3px; height: 14px;
    background: var(--accent);
    border-radius: 2px;
  }

  /* ── GARAM SELECTOR ── */
  .garam-list { display: flex; flex-direction: column; gap: 8px; margin-bottom: 1.5rem; }
  .garam-btn {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 12px 14px;
    cursor: pointer;
    transition: all 0.2s;
    text-align: left;
    width: 100%;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  .garam-btn:hover { border-color: var(--border2); background: var(--surface3); }
  .garam-btn.active {
    border-color: var(--accent);
    background: rgba(56,189,248,0.08);
  }
  .garam-formula {
    font-family: 'Syne', sans-serif;
    font-size: 15px;
    font-weight: 700;
    color: #fff;
    min-width: 90px;
  }
  .garam-info { flex: 1; }
  .garam-name { font-size: 12px; color: var(--text2); line-height: 1.3; }
  .garam-type {
    font-size: 10px;
    font-weight: 600;
    padding: 2px 8px;
    border-radius: 10px;
    margin-top: 4px;
    display: inline-block;
  }
  .type-asam { background: rgba(248,113,113,0.15); color: var(--red); }
  .type-basa { background: rgba(56,189,248,0.15); color: var(--accent); }
  .type-netral { background: rgba(74,222,128,0.15); color: var(--green); }
  .type-mix { background: rgba(167,139,250,0.15); color: var(--purple); }

  /* ── SLIDER ── */
  .slider-section { margin-bottom: 1.5rem; }
  .slider-label {
    font-size: 12px;
    color: var(--text2);
    margin-bottom: 8px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .slider-val {
    font-family: 'Syne', sans-serif;
    font-size: 15px;
    font-weight: 700;
    color: var(--accent);
  }
  input[type=range] {
    width: 100%;
    -webkit-appearance: none;
    height: 4px;
    border-radius: 2px;
    background: var(--surface3);
    outline: none;
    cursor: pointer;
  }
  input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 18px; height: 18px;
    border-radius: 50%;
    background: var(--accent);
    border: 2px solid var(--bg);
    box-shadow: 0 0 8px rgba(56,189,248,0.5);
    cursor: pointer;
    transition: box-shadow 0.2s;
  }
  input[type=range]:hover::-webkit-slider-thumb { box-shadow: 0 0 14px rgba(56,189,248,0.7); }

  /* ── PH BIG DISPLAY ── */
  .ph-hero {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.5rem;
    text-align: center;
    margin-bottom: 1.25rem;
    position: relative;
    overflow: hidden;
  }
  .ph-hero::before {
    content: '';
    position: absolute;
    top: -40px; right: -40px;
    width: 120px; height: 120px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(56,189,248,0.08) 0%, transparent 70%);
    pointer-events: none;
  }
  .ph-label-sm { font-size: 11px; color: var(--text3); letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 6px; }
  .ph-big {
    font-family: 'Syne', sans-serif;
    font-size: 64px;
    font-weight: 800;
    line-height: 1;
    margin-bottom: 8px;
    transition: color 0.3s;
  }
  .ph-status {
    font-size: 12px;
    font-weight: 600;
    padding: 5px 16px;
    border-radius: 20px;
    display: inline-block;
    margin-bottom: 1rem;
  }

  /* ── PH BAR ── */
  .ph-bar-wrap { position: relative; margin: 0.5rem 0 0.25rem; }
  .ph-bar {
    height: 10px;
    border-radius: 5px;
    background: linear-gradient(to right, #ef4444, #f97316, #eab308, #22c55e, #3b82f6, #8b5cf6);
  }
  .ph-needle {
    position: absolute;
    top: -4px;
    width: 18px; height: 18px;
    border-radius: 50%;
    background: #fff;
    border: 3px solid var(--bg);
    box-shadow: 0 0 10px rgba(255,255,255,0.4);
    transform: translateX(-50%);
    transition: left 0.4s cubic-bezier(.34,1.56,.64,1);
  }
  .ph-bar-labels {
    display: flex;
    justify-content: space-between;
    font-size: 10px;
    color: var(--text3);
    margin-top: 6px;
  }

  /* ── STATS ROW ── */
  .stats-row {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 8px;
    margin-bottom: 1.25rem;
  }
  .stat-card {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 12px;
  }
  .stat-label { font-size: 10px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 4px; }
  .stat-value { font-family: 'Syne', sans-serif; font-size: 18px; font-weight: 700; color: #fff; }
  .stat-sub { font-size: 10px; color: var(--text3); margin-top: 2px; }

  /* ── REAKSI BOX ── */
  .reaksi-box {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 14px;
    margin-bottom: 1.25rem;
  }
  .reaksi-title { font-size: 11px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.06em; margin-bottom: 8px; }
  .reaksi-eq {
    font-family: 'DM Sans', sans-serif;
    font-size: 13px;
    color: var(--teal);
    font-weight: 500;
    line-height: 1.8;
    white-space: pre-line;
  }
  .reaksi-desc { font-size: 12px; color: var(--text2); margin-top: 8px; line-height: 1.6; }

  /* ── VOLUME SECTION ── */
  .volume-section {
    margin-bottom: 1.5rem;
  }
  .volume-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
    margin-bottom: 10px;
  }
  .volume-input-wrap {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 10px 12px;
  }
  .volume-input-label {
    font-size: 10px;
    color: var(--text3);
    text-transform: uppercase;
    letter-spacing: 0.06em;
    margin-bottom: 6px;
  }
  .volume-input-row {
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .volume-input-row input[type=number] {
    background: transparent;
    border: none;
    outline: none;
    font-family: 'Syne', sans-serif;
    font-size: 18px;
    font-weight: 700;
    color: var(--teal);
    width: 70px;
    -moz-appearance: textfield;
  }
  .volume-input-row input[type=number]::-webkit-inner-spin-button,
  .volume-input-row input[type=number]::-webkit-outer-spin-button { -webkit-appearance: none; }
  .volume-unit {
    font-size: 12px;
    color: var(--text3);
  }
  .volume-slider { margin-top: 6px; }
  .volume-result-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
  }
  .vol-stat {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: var(--radius-sm);
    padding: 10px;
    text-align: center;
  }
  .vol-stat-label { font-size: 9px; color: var(--text3); text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 4px; }
  .vol-stat-val { font-family: 'Syne', sans-serif; font-size: 14px; font-weight: 700; color: var(--teal); }
  .vol-stat-sub { font-size: 9px; color: var(--text3); margin-top: 2px; }

  /* ── STANDAR BOX ── */
  .standar-box {
    background: rgba(74,222,128,0.06);
    border: 1px solid rgba(74,222,128,0.2);
    border-radius: var(--radius-sm);
    padding: 12px;
  }
  .standar-title { font-size: 11px; color: var(--green); font-weight: 600; margin-bottom: 6px; }
  .standar-text { font-size: 12px; color: var(--text2); line-height: 1.5; }

  /* ── RIGHT PANEL ── */
  .right-panel { display: flex; flex-direction: column; gap: 1.5rem; }

  .card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.5rem;
  }
  .card-title {
    font-family: 'Syne', sans-serif;
    font-size: 14px;
    font-weight: 700;
    color: #fff;
    margin-bottom: 1rem;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .card-title-dot {
    width: 8px; height: 8px;
    border-radius: 50%;
    background: var(--accent);
    box-shadow: 0 0 6px var(--accent);
  }

  /* ── CHART ── */
  .chart-wrap { position: relative; height: 280px; }

  /* ── MULTI COMPARE ── */
  .compare-chart-wrap { position: relative; height: 260px; }

  /* ── TABEL LOG ── */
  .tabel-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1rem; }
  .tabel-desc { font-size: 12px; color: var(--text2); }
  table { width: 100%; border-collapse: collapse; font-size: 13px; }
  th {
    text-align: left;
    font-size: 11px;
    text-transform: uppercase;
    letter-spacing: 0.06em;
    color: var(--text3);
    padding: 8px 10px;
    border-bottom: 1px solid var(--border);
    font-weight: 500;
  }
  td {
    padding: 9px 10px;
    border-bottom: 1px solid rgba(99,179,237,0.06);
    color: var(--text);
    font-size: 13px;
  }
  tr:last-child td { border-bottom: none; }
  tr:hover td { background: rgba(255,255,255,0.02); }
  .td-mono { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 14px; }
  .pill {
    font-size: 10px;
    font-weight: 600;
    padding: 3px 10px;
    border-radius: 20px;
    display: inline-block;
  }
  .pill-asam { background: rgba(248,113,113,0.15); color: var(--red); }
  .pill-basa { background: rgba(56,189,248,0.15); color: var(--accent); }
  .pill-netral { background: rgba(74,222,128,0.15); color: var(--green); }
  .pill-layak { background: rgba(74,222,128,0.12); color: var(--green); border: 1px solid rgba(74,222,128,0.3); }
  .pill-tidak { background: rgba(248,113,113,0.12); color: var(--red); border: 1px solid rgba(248,113,113,0.3); }

  /* ── INFO CARDS ROW ── */
  .info-row {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
  }
  .info-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 1.25rem;
  }
  .info-icon { font-size: 24px; margin-bottom: 10px; }
  .info-head { font-family: 'Syne', sans-serif; font-size: 13px; font-weight: 700; color: #fff; margin-bottom: 6px; }
  .info-body { font-size: 12px; color: var(--text2); line-height: 1.6; }
  .info-formula {
    margin-top: 10px;
    background: var(--surface2);
    border-radius: var(--radius-sm);
    padding: 8px 10px;
    font-size: 12px;
    color: var(--teal);
    font-weight: 500;
    font-family: monospace;
  }

  /* ── FOOTER ── */
  footer {
    border-top: 1px solid var(--border);
    padding: 1.5rem 2rem;
    text-align: center;
    font-size: 12px;
    color: var(--text3);
    max-width: 1200px;
    margin: 0 auto;
  }

  /* ── RESPONSIVE ── */
  @media (max-width: 900px) {
    .main { grid-template-columns: 1fr; }
    .panel { position: static; }
    .info-row { grid-template-columns: 1fr; }
    .hero { grid-template-columns: 1fr; }
  }
  @media (max-width: 600px) {
    .hero, .main { padding-left: 1rem; padding-right: 1rem; }
    .stats-row { grid-template-columns: 1fr 1fr; }
    .ph-big { font-size: 48px; }
  }
</style>
</head>
<body>

<header>
  <div class="header-inner">
    <div class="logo">
      <div class="logo-icon">💧</div>
      <div>
        <div class="logo-text">SimHidrolisis</div>
        <div class="logo-sub">Kimia Kelas XI · Fase F · Kurikulum Merdeka</div>
      </div>
    </div>
    <div class="header-badge">LKPD Kegiatan 2</div>
  </div>
</header>

<div class="hero">
  <div>
    <div class="hero-eyebrow">Simulasi Interaktif</div>
    <h1 class="hero-title">pH Hidrolisis Garam &<br><span>Kualitas Air Bersih</span></h1>
    <p class="hero-desc">Eksplorasi bagaimana garam-garam terlarut mempengaruhi pH air melalui reaksi hidrolisis. Amati perubahan pH secara real-time dan pelajari dampaknya terhadap kelayakan air minum.</p>
  </div>
  <div class="sdg-pill">🌍 SDGs Goal 6 — Air Bersih</div>
</div>

<div class="main">

  <!-- LEFT: CONTROL PANEL -->
  <div class="panel">
    <div class="panel-title">Pilih Garam</div>
    <div class="garam-list" id="garamList"></div>

    <div class="slider-section">
      <div class="slider-label">
        <span>Konsentrasi (M)</span>
        <span class="slider-val" id="concLabel">0.10 M</span>
      </div>
      <input type="range" id="concSlider" min="1" max="200" value="10" step="1">
      <div style="display:flex;justify-content:space-between;font-size:10px;color:var(--text3);margin-top:4px;">
        <span>0.01 M</span><span>1.00 M</span><span>2.00 M</span>
      </div>
      <div id="concEff" style="font-size:11px;color:var(--teal);margin-top:6px;min-height:16px;"></div>
    </div>

    <!-- VOLUME SECTION -->
    <div class="volume-section">
      <div class="panel-title" style="margin-bottom:1rem;">Volume</div>
      <div class="volume-grid">
        <div class="volume-input-wrap">
          <div class="volume-input-label">💧 Volume Air</div>
          <div class="volume-input-row">
            <input type="number" id="volAir" min="10" max="5000" value="500" step="10">
            <span class="volume-unit">mL</span>
          </div>
          <input type="range" class="volume-slider" id="volAirSlider" min="10" max="5000" value="500" step="10">
        </div>
        <div class="volume-input-wrap">
          <div class="volume-input-label">🧪 Volume Larutan</div>
          <div class="volume-input-row">
            <input type="number" id="volLarutan" min="10" max="5000" value="500" step="10">
            <span class="volume-unit">mL</span>
          </div>
          <input type="range" class="volume-slider" id="volLarutanSlider" min="10" max="5000" value="500" step="10">
        </div>
      </div>
      <div class="volume-result-row">
        <div class="vol-stat">
          <div class="vol-stat-label">Mol Garam</div>
          <div class="vol-stat-val" id="molGaram">—</div>
          <div class="vol-stat-sub">mol</div>
        </div>
        <div class="vol-stat">
          <div class="vol-stat-label">Mol H⁺/OH⁻</div>
          <div class="vol-stat-val" id="molIon">—</div>
          <div class="vol-stat-sub">mol</div>
        </div>
        <div class="vol-stat">
          <div class="vol-stat-label">Rasio Vol</div>
          <div class="vol-stat-val" id="rasioVol">—</div>
          <div class="vol-stat-sub">air : larutan</div>
        </div>
      </div>
    </div>

    <div class="ph-hero">
      <div class="ph-label-sm">pH Larutan</div>
      <div class="ph-big" id="phBig">7.00</div>
      <div class="ph-status" id="phStatus">Netral</div>
      <div class="ph-bar-wrap">
        <div class="ph-bar"></div>
        <div class="ph-needle" id="phNeedle" style="left:50%"></div>
      </div>
      <div class="ph-bar-labels">
        <span>0 Asam</span><span>7 Netral</span><span>14 Basa</span>
      </div>
    </div>

    <div class="stats-row">
      <div class="stat-card">
        <div class="stat-label">Konsentrasi H⁺</div>
        <div class="stat-value" id="hConc">1.0×10⁻⁷</div>
        <div class="stat-sub">mol/L</div>
      </div>
      <div class="stat-card">
        <div class="stat-label">Jenis Hidrolisis</div>
        <div class="stat-value" id="jenisHidro" style="font-size:13px">—</div>
        <div class="stat-sub" id="sifatLarutan">—</div>
      </div>
    </div>

    <div class="reaksi-box">
      <div class="reaksi-title">Reaksi Hidrolisis</div>
      <div class="reaksi-eq" id="reaksiEq">—</div>
      <div class="reaksi-desc" id="reaksiDesc">—</div>
    </div>

    <div class="standar-box">
      <div class="standar-title">📋 Standar WHO / Permenkes</div>
      <div class="standar-text">Air minum layak: <strong style="color:#fff">pH 6.5 – 8.5</strong><br>Di luar rentang ini, air tidak layak dikonsumsi dan dapat membahayakan kesehatan.</div>
    </div>
  </div>

  <!-- RIGHT: CHARTS & TABLE -->
  <div class="right-panel">

    <!-- pH vs Konsentrasi Chart -->
    <div class="card">
      <div class="card-title"><div class="card-title-dot"></div>Grafik pH vs Konsentrasi</div>
      <div class="chart-wrap">
        <canvas id="phChart"></canvas>
      </div>
      <div style="margin-top:12px;font-size:12px;color:var(--text2);">
        ⬤ <span style="color:var(--accent)">Garis biru</span> = pH garam yang dipilih &nbsp;|&nbsp;
        <span style="color:rgba(74,222,128,0.6)">Zona hijau</span> = rentang layak minum (6.5–8.5) &nbsp;|&nbsp;
        ⬤ Titik kuning = kondisi saat ini
      </div>
    </div>

    <!-- Perbandingan semua garam -->
    <div class="card">
      <div class="card-title"><div class="card-title-dot" style="background:var(--teal);box-shadow:0 0 6px var(--teal)"></div>Perbandingan pH Semua Garam</div>
      <div style="font-size:12px;color:var(--text2);margin-bottom:1rem;">Pada konsentrasi yang sama: <span id="compareConc" style="color:var(--accent);font-weight:600">0.10 M</span></div>
      <div class="compare-chart-wrap">
        <canvas id="compareChart"></canvas>
      </div>
    </div>

    <!-- Tabel Log -->
    <div class="card">
      <div class="tabel-header">
        <div class="card-title" style="margin-bottom:0"><div class="card-title-dot" style="background:var(--purple);box-shadow:0 0 6px var(--purple)"></div>Tabel Data Pengamatan</div>
        <div class="tabel-desc" id="tabelDesc">—</div>
      </div>
      <div style="overflow-x:auto;">
        <table id="logTable">
          <thead>
            <tr>
              <th>Konsentrasi (M)</th>
              <th>Vol. Larutan (mL)</th>
              <th>Mol Garam</th>
              <th>pH</th>
              <th>[H⁺] (M)</th>
              <th>Sifat</th>
              <th>Layak Minum?</th>
            </tr>
          </thead>
          <tbody id="logBody"></tbody>
        </table>
      </div>
    </div>

    <!-- Info boxes -->
    <div class="info-row">
      <div class="info-card">
        <div class="info-icon">⚗️</div>
        <div class="info-head">Hidrolisis Kation</div>
        <div class="info-body">Garam dari asam kuat + basa lemah. Kation bereaksi dengan air menghasilkan ion H⁺. Larutan bersifat <strong style="color:var(--red)">asam</strong>.</div>
        <div class="info-formula">pH = 7 − ½(pKb + log C)</div>
      </div>
      <div class="info-card">
        <div class="info-icon">🧪</div>
        <div class="info-head">Hidrolisis Anion</div>
        <div class="info-body">Garam dari basa kuat + asam lemah. Anion bereaksi dengan air menghasilkan ion OH⁻. Larutan bersifat <strong style="color:var(--accent)">basa</strong>.</div>
        <div class="info-formula">pH = 7 + ½(pKa + log C)</div>
      </div>
      <div class="info-card">
        <div class="info-icon">⚖️</div>
        <div class="info-head">Tidak Terhidrolisis</div>
        <div class="info-body">Garam dari asam kuat + basa kuat. Tidak ada reaksi dengan air. Larutan bersifat <strong style="color:var(--green)">netral</strong>.</div>
        <div class="info-formula">pH = 7 (tetap)</div>
      </div>
    </div>

    <!-- Volume info card row -->
    <div class="card" style="background:rgba(45,212,191,0.04);border-color:rgba(45,212,191,0.2);">
      <div class="card-title"><div class="card-title-dot" style="background:var(--teal);box-shadow:0 0 6px var(--teal)"></div>Konsep Volume dalam Hidrolisis</div>
      <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:1rem;font-size:13px;color:var(--text2);line-height:1.7;">
        <div>
          <strong style="color:var(--teal)">📐 Mol Garam</strong><br>
          Jumlah mol garam terlarut dihitung dari konsentrasi × volume larutan.<br>
          <span style="color:var(--teal);font-family:monospace">n = C × V(L)</span>
        </div>
        <div>
          <strong style="color:var(--teal)">💧 Volume Air vs Larutan</strong><br>
          Volume air adalah pelarut murni. Volume larutan = air + solut. Perbedaan ini mempengaruhi konsentrasi efektif.<br>
          <span style="color:var(--teal);font-family:monospace">C = n / V(larutan)</span>
        </div>
        <div>
          <strong style="color:var(--teal)">🔬 Pengenceran</strong><br>
          Menambah volume air akan mengencerkan larutan. Konsentrasi baru: C₂ = C₁V₁/V₂.<br>
          <span style="color:var(--teal);font-family:monospace">C₁V₁ = C₂V₂</span>
        </div>
      </div>
    </div>

  </div>
</div>

<footer>
  <p>SimHidrolisis · LKPD Kimia Kelas XI Fase F · Kurikulum Merdeka 2024 · Topik: Hidrolisis Garam &amp; Kualitas Air Bersih Berkelanjutan (SDGs Goal 6)</p>
  <p style="margin-top:4px;">Ka CH₃COOH = 1.8×10⁻⁵ &nbsp;|&nbsp; Kb NH₃ = 1.8×10⁻⁵ &nbsp;|&nbsp; Ka H₂CO₃ = 4.7×10⁻¹¹ &nbsp;|&nbsp; Kb Al(OH)₃ = 1.0×10⁻⁹</p>
</footer>

<script>
// ── DATA GARAM ──
const GARAM = [
  {
    id: 'nh4cl',
    formula: 'NH₄Cl',
    name: 'Amonium klorida',
    type: 'kation',
    typeLabel: 'Hidrolisis kation',
    typeClass: 'type-asam',
    typeText: 'Asam',
    K: 1.8e-5,  // Kb NH3
    calcPh: (C) => {
      const pKb = -Math.log10(1.8e-5);
      return Math.max(0, Math.min(14, 7 - 0.5*(pKb + Math.log10(C))));
    },
    reaksiEq: 'NH₄⁺  +  H₂O  ⇌  NH₃  +  H₃O⁺',
    reaksiDesc: 'Ion NH₄⁺ (dari basa lemah NH₃) mengalami hidrolisis parsial. Menghasilkan ion H₃O⁺ → larutan ASAM.',
    color: '#f87171'
  },
  {
    id: 'ch3coona',
    formula: 'CH₃COONa',
    name: 'Natrium asetat',
    type: 'anion',
    typeLabel: 'Hidrolisis anion',
    typeClass: 'type-basa',
    typeText: 'Basa',
    K: 1.8e-5,  // Ka CH3COOH
    calcPh: (C) => {
      const pKa = -Math.log10(1.8e-5);
      return Math.max(0, Math.min(14, 7 + 0.5*(pKa + Math.log10(C))));
    },
    reaksiEq: 'CH₃COO⁻  +  H₂O  ⇌  CH₃COOH  +  OH⁻',
    reaksiDesc: 'Ion CH₃COO⁻ (dari asam lemah CH₃COOH) mengalami hidrolisis anion. Menghasilkan OH⁻ → larutan BASA.',
    color: '#38bdf8'
  },
  {
    id: 'nacl',
    formula: 'NaCl',
    name: 'Natrium klorida',
    type: 'netral',
    typeLabel: 'Tidak terhidrolisis',
    typeClass: 'type-netral',
    typeText: 'Netral',
    K: null,
    calcPh: () => 7,
    reaksiEq: 'Na⁺  +  Cl⁻  →  tidak bereaksi dengan H₂O',
    reaksiDesc: 'Na⁺ (dari NaOH, basa kuat) dan Cl⁻ (dari HCl, asam kuat) tidak mengalami hidrolisis. pH tetap = 7.',
    color: '#4ade80'
  },
  {
    id: 'na2co3',
    formula: 'Na₂CO₃',
    name: 'Natrium karbonat (soda abu)',
    type: 'anion',
    typeLabel: 'Hidrolisis anion kuat',
    typeClass: 'type-basa',
    typeText: 'Basa kuat',
    K: 4.7e-11, // Ka2 H2CO3
    calcPh: (C) => {
      const pKa = -Math.log10(4.7e-11);
      return Math.max(0, Math.min(14, 7 + 0.5*(pKa + Math.log10(C))));
    },
    reaksiEq: 'CO₃²⁻  +  H₂O  ⇌  HCO₃⁻  +  OH⁻',
    reaksiDesc: 'Ion CO₃²⁻ dari asam lemah H₂CO₃ mengalami hidrolisis kuat. Menghasilkan banyak OH⁻ → larutan sangat BASA.',
    color: '#a78bfa'
  },
  {
    id: 'al2so43',
    formula: 'Al₂(SO₄)₃',
    name: 'Aluminium sulfat (tawas)',
    type: 'kation',
    typeLabel: 'Hidrolisis kation kuat',
    typeClass: 'type-asam',
    typeText: 'Asam kuat',
    K: 1.0e-9,  // Kb Al(OH)3
    calcPh: (C) => {
      const pKb = -Math.log10(1.0e-9);
      return Math.max(0, Math.min(14, 7 - 0.5*(pKb + Math.log10(C * 2))));
    },
    reaksiEq: 'Al³⁺  +  3H₂O  ⇌  Al(OH)₃  +  3H⁺',
    reaksiDesc: 'Ion Al³⁺ dari basa lemah Al(OH)₃ mengalami hidrolisis kuat. Menghasilkan banyak H⁺ → larutan sangat ASAM.',
    color: '#fbbf24'
  }
];

let selectedGaram = GARAM[0];
let concValue = 0.10;
let volAirValue = 500;    // mL
let volLarutanValue = 500; // mL
let phChart = null;
let compareChart = null;

// ── BUILD GARAM BUTTONS ──
function buildGaramList() {
  const list = document.getElementById('garamList');
  GARAM.forEach(g => {
    const btn = document.createElement('button');
    btn.className = 'garam-btn' + (g.id === selectedGaram.id ? ' active' : '');
    btn.dataset.id = g.id;
    btn.innerHTML = `
      <div class="garam-formula" style="color:${g.color}">${g.formula}</div>
      <div class="garam-info">
        <div class="garam-name">${g.name}</div>
        <span class="garam-type ${g.typeClass}">${g.typeText}</span>
      </div>`;
    btn.addEventListener('click', () => {
      selectedGaram = g;
      document.querySelectorAll('.garam-btn').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      update();
    });
    list.appendChild(btn);
  });
}

// ── SLIDER ──
document.getElementById('concSlider').addEventListener('input', function() {
  concValue = parseInt(this.value) / 100;
  update();
});

// ── VOLUME SYNC ──
function syncVolume(inputId, sliderId, varSetter) {
  const input = document.getElementById(inputId);
  const slider = document.getElementById(sliderId);
  input.addEventListener('input', function() {
    let v = Math.max(10, Math.min(5000, parseInt(this.value) || 10));
    this.value = v;
    slider.value = v;
    varSetter(v);
    update();
  });
  slider.addEventListener('input', function() {
    let v = parseInt(this.value);
    input.value = v;
    varSetter(v);
    update();
  });
}
syncVolume('volAir', 'volAirSlider', v => { volAirValue = v; });
syncVolume('volLarutan', 'volLarutanSlider', v => { volLarutanValue = v; });

// ── FORMAT H+ ──
function formatHConc(ph) {
  const h = Math.pow(10, -ph);
  if (h < 1e-10) return '< 10⁻¹⁰';
  const exp = Math.floor(Math.log10(h));
  const coeff = (h / Math.pow(10, exp)).toFixed(1);
  const supMap = {'0':'⁰','1':'¹','2':'²','3':'³','4':'⁴','5':'⁵','6':'⁶','7':'⁷','8':'⁸','9':'⁹','-':'⁻'};
  const expStr = String(exp).split('').map(c => supMap[c]||c).join('');
  return `${coeff}×10${expStr}`;
}

// ── MAIN UPDATE ──
function update() {
  // Effective concentration adjusted for volume ratio (air dilutes the solution)
  const C_base = concValue;
  // If volAir differs from volLarutan, treat it as dilution:
  // C_eff = C_base * volLarutan / (volLarutan + volAir - volLarutan) when air added separately
  // More physically: solution of volume volLarutan, then mixed into total = volAir + volLarutan
  const totalVol = volAirValue + volLarutanValue; // mL
  const C = C_base * (volLarutanValue / totalVol);
  const ph = selectedGaram.calcPh(C);

  document.getElementById('concLabel').textContent = C_base.toFixed(2) + ' M';
  // Show effective concentration if diluted
  const effEl = document.getElementById('concEff');
  if (effEl) {
    const diff = Math.abs(C - C_base);
    effEl.textContent = diff > 0.001 ? `C efektif: ${C.toFixed(4)} M (setelah pengenceran)` : '';
  }
  document.getElementById('phBig').textContent = ph.toFixed(2);
  document.getElementById('hConc').textContent = formatHConc(ph);
  document.getElementById('jenisHidro').textContent = selectedGaram.typeLabel;
  document.getElementById('sifatLarutan').textContent = selectedGaram.typeText;
  document.getElementById('reaksiEq').textContent = selectedGaram.reaksiEq;
  document.getElementById('reaksiDesc').textContent = selectedGaram.reaksiDesc;
  document.getElementById('compareConc').textContent = C.toFixed(2) + ' M';

  // Color pH
  let phColor, statusText, statusBg, statusColor;
  if (ph < 6.5) {
    phColor = '#f87171'; statusText = 'Asam — Tidak Layak Minum'; statusBg = 'rgba(248,113,113,0.12)'; statusColor = '#f87171';
  } else if (ph > 8.5) {
    phColor = '#a78bfa'; statusText = 'Basa — Tidak Layak Minum'; statusBg = 'rgba(167,139,250,0.12)'; statusColor = '#a78bfa';
  } else {
    phColor = '#4ade80'; statusText = 'Netral — Layak Minum ✓'; statusBg = 'rgba(74,222,128,0.12)'; statusColor = '#4ade80';
  }
  document.getElementById('phBig').style.color = phColor;
  const st = document.getElementById('phStatus');
  st.textContent = statusText;
  st.style.background = statusBg;
  st.style.color = statusColor;
  st.style.border = '1px solid ' + phColor + '33';

  // Needle
  const pct = (ph / 14) * 100;
  document.getElementById('phNeedle').style.left = pct + '%';

  updatePhChart(ph, C);
  updateCompareChart(C);
  updateTable();
  updateVolumeStats(ph, C);
}

// ── VOLUME STATS ──
function formatMol(mol) {
  if (mol < 0.0001) return mol.toExponential(2);
  if (mol >= 1000) return mol.toFixed(0);
  if (mol >= 10) return mol.toFixed(2);
  return mol.toFixed(4);
}

function updateVolumeStats(ph, C) {
  const VL = volLarutanValue / 1000; // L
  const VA = volAirValue / 1000;     // L
  const molGaram = C * VL;

  // mol H+ or OH- relative to neutral
  const hConc = Math.pow(10, -ph);
  const ohConc = Math.pow(10, -(14 - ph));
  let molIon;
  if (ph < 7) {
    molIon = (hConc - 1e-7) * VL;
  } else if (ph > 7) {
    molIon = (ohConc - 1e-7) * VL;
  } else {
    molIon = 0;
  }

  const gcd = (a, b) => b < 0.001 ? a : gcd(b, a % b);
  let ratioText;
  const ra = Math.round(VA * 1000), rl = Math.round(VL * 1000);
  const g = gcd(ra, rl);
  ratioText = (ra / g) + ':' + (rl / g);

  document.getElementById('molGaram').textContent = formatMol(molGaram);
  document.getElementById('molIon').textContent = ph === 7 ? '≈ 0' : formatMol(Math.abs(molIon));
  document.getElementById('rasioVol').textContent = ratioText;
}

// ── PH vs CONC CHART ──
function buildPhChart() {
  const ctx = document.getElementById('phChart').getContext('2d');
  const concs = [];
  const phs = [];
  for (let i = 1; i <= 200; i++) {
    const c = i / 100;
    concs.push(c.toFixed(2));
    phs.push(parseFloat(selectedGaram.calcPh(c).toFixed(3)));
  }

  phChart = new Chart(ctx, {
    type: 'line',
    data: {
      labels: concs,
      datasets: [
        {
          label: 'Zona aman atas',
          data: Array(200).fill(8.5),
          borderColor: 'rgba(74,222,128,0)',
          backgroundColor: 'rgba(74,222,128,0.08)',
          fill: '+1',
          pointRadius: 0,
          borderDash: [4,4],
          tension: 0,
        },
        {
          label: 'Zona aman bawah',
          data: Array(200).fill(6.5),
          borderColor: 'rgba(74,222,128,0.4)',
          backgroundColor: 'transparent',
          fill: false,
          pointRadius: 0,
          borderDash: [4,4],
          tension: 0,
          borderWidth: 1,
        },
        {
          label: 'pH ' + selectedGaram.formula,
          data: phs,
          borderColor: selectedGaram.color,
          backgroundColor: 'transparent',
          fill: false,
          pointRadius: 0,
          tension: 0.3,
          borderWidth: 2.5,
        },
        {
          label: 'Kondisi saat ini',
          data: [],
          borderColor: '#fbbf24',
          backgroundColor: '#fbbf24',
          pointRadius: 7,
          pointHoverRadius: 9,
          showLine: false,
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      interaction: { mode: 'index', intersect: false },
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#1a2235',
          borderColor: 'rgba(99,179,237,0.2)',
          borderWidth: 1,
          titleColor: '#94a3b8',
          bodyColor: '#e2e8f0',
          callbacks: {
            title: (items) => 'Konsentrasi: ' + items[0].label + ' M',
            label: (item) => item.datasetIndex === 2 ? 'pH: ' + item.raw.toFixed(2) : null,
            filter: (item) => item.datasetIndex === 2,
          }
        }
      },
      scales: {
        x: {
          ticks: { color: '#64748b', maxTicksLimit: 8, font: { size: 11 } },
          grid: { color: 'rgba(99,179,237,0.05)' },
          title: { display: true, text: 'Konsentrasi (M)', color: '#64748b', font: { size: 11 } }
        },
        y: {
          min: 0, max: 14,
          ticks: { color: '#64748b', font: { size: 11 } },
          grid: { color: 'rgba(99,179,237,0.05)' },
          title: { display: true, text: 'pH', color: '#64748b', font: { size: 11 } }
        }
      }
    }
  });
}

function updatePhChart(currentPh, currentConc) {
  if (!phChart) return;

  // Update line data for selected garam
  const phs = [];
  for (let i = 1; i <= 200; i++) {
    const c = i / 100;
    phs.push(parseFloat(selectedGaram.calcPh(c).toFixed(3)));
  }
  phChart.data.datasets[2].data = phs;
  phChart.data.datasets[2].label = 'pH ' + selectedGaram.formula;
  phChart.data.datasets[2].borderColor = selectedGaram.color;

  // Current point
  const idx = Math.round(currentConc * 100) - 1;
  const pointData = Array(200).fill(null);
  pointData[idx] = currentPh;
  phChart.data.datasets[3].data = pointData;

  phChart.update('none');
}

// ── COMPARE CHART ──
function buildCompareChart() {
  const ctx = document.getElementById('compareChart').getContext('2d');

  compareChart = new Chart(ctx, {
    type: 'bar',
    data: {
      labels: GARAM.map(g => g.formula),
      datasets: [
        {
          label: 'pH',
          data: GARAM.map(g => parseFloat(g.calcPh(0.10).toFixed(2))),
          backgroundColor: GARAM.map(g => g.color + 'BB'),
          borderColor: GARAM.map(g => g.color),
          borderWidth: 1.5,
          borderRadius: 6,
        }
      ]
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: {
          backgroundColor: '#1a2235',
          borderColor: 'rgba(99,179,237,0.2)',
          borderWidth: 1,
          titleColor: '#94a3b8',
          bodyColor: '#e2e8f0',
          callbacks: {
            label: (item) => 'pH: ' + item.raw.toFixed(2)
          }
        },
        annotation: {}
      },
      scales: {
        x: {
          ticks: { color: '#94a3b8', font: { size: 12, weight: '600' } },
          grid: { display: false }
        },
        y: {
          min: 0, max: 14,
          ticks: { color: '#64748b', font: { size: 11 } },
          grid: { color: 'rgba(99,179,237,0.05)' },
          title: { display: true, text: 'pH', color: '#64748b', font: { size: 11 } }
        }
      }
    }
  });
}

function updateCompareChart(C) {
  if (!compareChart) return;
  compareChart.data.datasets[0].data = GARAM.map(g => parseFloat(g.calcPh(C).toFixed(2)));
  compareChart.update('none');
}

// ── TABLE ──
function updateTable() {
  const samples = [0.01, 0.05, 0.10, 0.25, 0.50, 1.00, 2.00];
  const tbody = document.getElementById('logBody');
  tbody.innerHTML = '';
  document.getElementById('tabelDesc').textContent = selectedGaram.formula + ' — ' + selectedGaram.name;

  const VL = volLarutanValue; // mL
  samples.forEach(C => {
    const ph = parseFloat(selectedGaram.calcPh(C).toFixed(2));
    const hConc = formatHConc(ph);
    const molGaram = C * (VL / 1000);
    let sifat, sifatClass, layak, layakClass;
    if (ph < 7) { sifat = 'Asam'; sifatClass = 'pill-asam'; }
    else if (ph > 7) { sifat = 'Basa'; sifatClass = 'pill-basa'; }
    else { sifat = 'Netral'; sifatClass = 'pill-netral'; }
    if (ph >= 6.5 && ph <= 8.5) { layak = 'Layak ✓'; layakClass = 'pill-layak'; }
    else { layak = 'Tidak ✗'; layakClass = 'pill-tidak'; }

    const isActive = Math.abs(C - concValue) < 0.001;
    const tr = document.createElement('tr');
    if (isActive) tr.style.background = 'rgba(56,189,248,0.05)';
    tr.innerHTML = `
      <td class="td-mono" style="${isActive ? 'color:var(--accent)' : ''}">${C.toFixed(2)}</td>
      <td style="font-size:12px;color:var(--teal);font-weight:600">${VL}</td>
      <td style="font-size:12px;color:var(--teal)">${formatMol(molGaram)}</td>
      <td class="td-mono" style="${isActive ? 'color:var(--accent)' : ''}">${ph.toFixed(2)}</td>
      <td style="font-size:12px;color:var(--text2)">${hConc}</td>
      <td><span class="pill ${sifatClass}">${sifat}</span></td>
      <td><span class="pill ${layakClass}">${layak}</span></td>`;
    tbody.appendChild(tr);
  });
}

// ── INIT ──
buildGaramList();
buildPhChart();
buildCompareChart();
update();
</script>
</body>
</html>
