---
layout: post
title: Earth Beneath — Cross Section to Scale
date: 2026-06-15
tags: [Geology, Interactive, Visualization]
description: A to-scale scrollable cross-section from your doorstep down 35 km to the Moho discontinuity.
---

Scroll to descend through the continental crust at true scale — **10 pixels = 1 meter** (1 pixel ≈ 10 cm). Depths shown are representative; real geology varies enormously by location.

<style>
.prose.anim-4:has(.earth-viz) { opacity: 1; animation: none; }

.earth-viz {
  width: 100vw;
  max-width: 100vw;
  margin-left: calc(50% - 50vw);
  margin-right: calc(50% - 50vw);
  position: relative;
  font-family: Georgia, 'Times New Roman', serif;
  background: #87CEEB;
  color: #2c2416;
  overflow-x: hidden;
  margin-top: 24px;
  margin-bottom: 48px;
  --px-per-m: 10;
  --crust-depth-m: 35000;
  --house-height-m: 4.5;
  --house-width-m: 10;
  --ground-width-m: 120;
  --parallax-y: 0px;
}

.earth-viz, .earth-viz * { box-sizing: border-box; }
.earth-viz * { margin: 0; padding: 0; }

.earth-viz-caption {
  font-family: system-ui, -apple-system, sans-serif;
  font-size: 13px;
  line-height: 1.65;
  color: #e2e8f0;
  background: rgba(15, 23, 42, 0.94);
  padding: 14px 20px;
  margin: 0;
  border-bottom: 1px solid rgba(255,255,255,0.1);
}

.earth-viz-caption .fact-note {
  display: block;
  margin-top: 8px;
  font-size: 11px;
  opacity: 0.72;
  line-height: 1.5;
}

.earth-viz .depth-hud,
.earth-viz .ruler,
.earth-viz .ruler-dot,
.earth-viz .scroll-hint {
  z-index: 400;
}

.depth-hud {
  position: fixed;
  right: 16px;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(15, 23, 42, 0.92);
  color: #f1f5f9;
  padding: 12px 14px;
  border-radius: 8px;
  font-family: 'Courier New', monospace;
  font-size: 12px;
  line-height: 1.55;
  min-width: 148px;
  border: 1px solid rgba(255,255,255,0.12);
  pointer-events: none;
  backdrop-filter: blur(6px);
}

.depth-hud .label { opacity: 0.6; font-size: 10px; text-transform: uppercase; letter-spacing: 0.08em; }
.depth-hud .value { font-size: 18px; font-weight: bold; color: #fbbf24; margin: 2px 0; }
.depth-hud .meta { opacity: 0.72; font-size: 11px; }

.scene {
  margin: 0 auto;
  width: calc(var(--ground-width-m) * var(--px-per-m) * 1px);
  padding-top: 24px;
  position: relative;
}

.sky {
  height: calc(80 * var(--px-per-m) * 1px);
  background: linear-gradient(180deg, #1e3a5f 0%, #3b82c4 35%, #7ec8e3 70%, #b8e0f0 100%);
  position: relative;
  overflow: hidden;
}

.sky::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 40%;
  background: linear-gradient(180deg, transparent, rgba(255,255,255,0.15));
}

.sun {
  position: absolute;
  top: 18%; right: 18%;
  width: 36px; height: 36px;
  background: radial-gradient(circle, #fff9c4, #fde047 60%, #f59e0b);
  border-radius: 50%;
  box-shadow: 0 0 40px 12px rgba(253, 224, 71, 0.5);
}

.cloud { position: absolute; background: rgba(255,255,255,0.85); border-radius: 50px; }
.cloud::before, .cloud::after { content: ''; position: absolute; background: inherit; border-radius: 50%; }
.c1 { width: 70px; height: 22px; top: 22%; left: 12%; }
.c1::before { width: 32px; height: 32px; top: -14px; left: 10px; }
.c1::after { width: 40px; height: 28px; top: -10px; left: 30px; }
.c2 { width: 55px; height: 18px; top: 38%; left: 55%; opacity: 0.7; }
.c2::before { width: 26px; height: 26px; top: -12px; left: 8px; }
.c2::after { width: 32px; height: 22px; top: -8px; left: 22px; }

.surface-row {
  display: flex;
  align-items: flex-end;
  height: calc(var(--house-height-m) * var(--px-per-m) * 1px + 6px);
  position: relative;
  background: linear-gradient(180deg, #6b8f3c 0%, #4a7c31 40%, #3d6b28 100%);
  border-bottom: 3px solid #2d5016;
}

.grass-blade {
  position: absolute;
  bottom: 100%;
  width: 2px;
  background: #5a9e3a;
  border-radius: 1px 1px 0 0;
}

.house-wrap {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  bottom: 0;
  z-index: 5;
}

.house {
  width: calc(var(--house-width-m) * var(--px-per-m) * 1px);
  height: calc(var(--house-height-m) * var(--px-per-m) * 1px);
  position: relative;
}

.house-body {
  position: absolute;
  bottom: 0; left: 10%;
  width: 80%; height: 62%;
  background: #e8dcc8;
  border: 1px solid #a89880;
}

.house-roof {
  position: absolute;
  bottom: 58%; left: 2%;
  width: 96%; height: 0;
  border-left: calc(var(--house-width-m) * var(--px-per-m) * 0.47 * 1px) solid transparent;
  border-right: calc(var(--house-width-m) * var(--px-per-m) * 0.47 * 1px) solid transparent;
  border-bottom: calc(var(--house-height-m) * var(--px-per-m) * 0.38 * 1px) solid #8b3a2a;
}

.house-door { position: absolute; bottom: 0; left: 42%; width: 14%; height: 38%; background: #5c3d2e; }
.house-window { position: absolute; background: #a8d8ea; border: 1px solid #5a7a8a; }
.w1 { bottom: 28%; left: 22%; width: 14%; height: 18%; }
.w2 { bottom: 28%; right: 22%; width: 14%; height: 18%; }

.person {
  position: absolute;
  left: calc(50% + var(--house-width-m) * var(--px-per-m) * 0.65 * 1px);
  bottom: 0;
  width: calc(1.7 * var(--px-per-m) * 1px);
  height: calc(1.7 * var(--px-per-m) * 1px);
}
.person-head { position: absolute; top: 0; left: 25%; width: 50%; height: 28%; background: #d4a574; border-radius: 50%; }
.person-body { position: absolute; top: 26%; left: 20%; width: 60%; height: 45%; background: #3b5998; border-radius: 2px 2px 0 0; }
.person-legs { position: absolute; bottom: 0; left: 22%; width: 56%; height: 32%; background: #2d3748; }

.scale-pin {
  position: absolute;
  left: 8px;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  color: #fff;
  background: rgba(0,0,0,0.55);
  padding: 4px 8px;
  border-radius: 4px;
  white-space: nowrap;
  z-index: 10;
}

.earth {
  position: relative;
  height: calc(var(--crust-depth-m) * var(--px-per-m) * 1px);
  width: 100%;
  overflow: hidden;
  background: linear-gradient(180deg,
    #9a7b4f 0%,
    #8a6d45 0.05%,
    #7a6040 0.15%,
    #6e5842 0.4%,
    #62524a 1%,
    #564c4a 2.5%,
    #4c4648 5%,
    #44404a 10%,
    #3c3a48 18%,
    #343446 30%,
    #2c2e42 45%,
    #26283c 60%,
    #202236 75%,
    #1a1c30 88%,
    #161828 96%,
    #121420 100%
  );
}

.strata-texture {
  position: absolute;
  inset: 0;
  pointer-events: none;
  opacity: 0.55;
  background-image:
    repeating-linear-gradient(0deg,
      rgba(255,255,255,0.09) 0px,
      rgba(255,255,255,0.09) 1px,
      transparent 1px,
      transparent 28px
    ),
    repeating-linear-gradient(0deg,
      rgba(0,0,0,0.14) 0px,
      rgba(0,0,0,0.14) 2px,
      transparent 2px,
      transparent 112px
    ),
    repeating-linear-gradient(90deg,
      transparent 0px,
      transparent 14px,
      rgba(0,0,0,0.05) 14px,
      rgba(0,0,0,0.05) 15px
    );
  background-size: 100% 280px, 100% 1120px, 60px 100%;
  background-position: 0 var(--parallax-y), 0 calc(var(--parallax-y) * 0.6), 0 0;
  mix-blend-mode: overlay;
}

.zone-tint {
  position: absolute;
  left: 0; right: 0;
  pointer-events: none;
  mix-blend-mode: multiply;
}

.stratum-band {
  position: absolute;
  left: 0; right: 0;
  pointer-events: none;
  border-top: 1px solid rgba(255,255,255,0.08);
  border-bottom: 1px solid rgba(0,0,0,0.18);
}

.grain {
  position: absolute;
  border-radius: 50%;
  pointer-events: none;
  opacity: 0.35;
}

.layer-label {
  position: absolute;
  left: 12px;
  font-family: system-ui, sans-serif;
  font-size: 11px;
  color: rgba(255,255,255,0.88);
  background: rgba(0,0,0,0.55);
  padding: 4px 9px;
  border-radius: 4px;
  white-space: nowrap;
  border-left: 3px solid rgba(251, 191, 36, 0.8);
  backdrop-filter: blur(4px);
  box-shadow: 0 2px 8px rgba(0,0,0,0.25);
}

.layer-label.right {
  left: auto; right: 12px;
  border-left: none;
  border-right: 3px solid rgba(251, 191, 36, 0.8);
}

.depth-tick {
  position: absolute;
  left: 0; right: 0;
  height: 0;
  border-top: 1px dashed rgba(255,255,255,0.14);
  pointer-events: none;
}

.depth-tick span {
  position: absolute;
  right: 8px; top: -9px;
  font-family: 'Courier New', monospace;
  font-size: 9px;
  color: rgba(255,255,255,0.4);
  background: rgba(0,0,0,0.35);
  padding: 1px 5px;
  border-radius: 2px;
}

.depth-tick.major { border-top-color: rgba(251, 191, 36, 0.35); }
.depth-tick.major span { color: rgba(251, 191, 36, 0.9); font-size: 10px; font-weight: bold; }

.feature {
  position: absolute;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  color: rgba(220, 235, 255, 0.92);
  padding: 3px 8px;
  background: rgba(20, 45, 80, 0.72);
  border-radius: 4px;
  white-space: nowrap;
  border: 1px solid rgba(100, 160, 255, 0.25);
  box-shadow: 0 2px 6px rgba(0,0,0,0.2);
}

.feature::before { content: '▸ '; opacity: 0.55; color: #fbbf24; }

.milestone {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  width: 3px;
  background: linear-gradient(180deg, rgba(251,191,36,0.9), rgba(251,191,36,0.2));
  box-shadow: 0 0 8px rgba(251,191,36,0.5);
  z-index: 3;
}

.milestone-tag {
  position: absolute;
  left: 10px;
  top: 0;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  font-weight: 600;
  color: #fef3c7;
  background: rgba(120, 53, 15, 0.85);
  padding: 4px 10px;
  border-radius: 4px;
  white-space: nowrap;
  border: 1px solid rgba(251, 191, 36, 0.5);
}

.water-table {
  position: absolute;
  left: 0; right: 0;
  height: calc(12 * var(--px-per-m) * 1px);
  background: linear-gradient(180deg,
    rgba(56, 130, 200, 0.12),
    rgba(56, 130, 200, 0.42),
    rgba(56, 130, 200, 0.12)
  );
  border-top: 1px solid rgba(100, 180, 255, 0.5);
  border-bottom: 1px solid rgba(100, 180, 255, 0.3);
}

.aquifer-pocket {
  position: absolute;
  border-radius: 50%;
  background: radial-gradient(ellipse, rgba(80, 160, 230, 0.55), rgba(40, 90, 160, 0.15));
  border: 1px solid rgba(100, 180, 255, 0.35);
}

.rock-vein {
  position: absolute;
  background: linear-gradient(135deg, rgba(200,175,120,0.45), rgba(140,110,70,0.2));
  transform: skewX(-12deg);
  border-radius: 2px;
}

.fault-line {
  position: absolute;
  width: 4px;
  background: linear-gradient(180deg, rgba(255,100,80,0.75), rgba(255,60,40,0.25));
  transform-origin: top center;
  box-shadow: 0 0 6px rgba(255,80,60,0.3);
}

.inclusion {
  position: absolute;
  border-radius: 2px;
  background: rgba(180, 140, 220, 0.35);
  border: 1px solid rgba(200, 160, 255, 0.25);
}

.moho {
  position: absolute;
  bottom: 0;
  left: -10%; right: -10%;
  height: calc(200 * var(--px-per-m) * 1px);
  background: linear-gradient(180deg,
    rgba(80, 20, 10, 0) 0%,
    rgba(120, 30, 15, 0.45) 30%,
    rgba(180, 50, 20, 0.75) 60%,
    rgba(220, 70, 25, 0.92) 85%,
    rgba(255, 90, 30, 1) 100%
  );
  display: flex;
  align-items: flex-end;
  justify-content: center;
  padding-bottom: calc(20 * var(--px-per-m) * 1px);
}

.moho-label {
  font-family: system-ui, sans-serif;
  text-align: center;
  color: #fff;
  background: rgba(0,0,0,0.78);
  padding: 18px 28px;
  border-radius: 8px;
  border: 2px solid rgba(255, 120, 50, 0.6);
  max-width: 92%;
}

.moho-label h2 { font-size: 18px; margin-bottom: 8px; color: #fdba74; }
.moho-label p { font-size: 13px; line-height: 1.65; opacity: 0.92; }

.moho-line {
  position: absolute;
  top: 0; left: 0; right: 0;
  height: 4px;
  background: linear-gradient(90deg, #ff6b35, #ff4500, #ff6b35);
  box-shadow: 0 0 24px 6px rgba(255, 80, 30, 0.55);
}

.ruler {
  position: fixed;
  left: 8px;
  top: 88px;
  bottom: 20px;
  width: 6px;
  background: linear-gradient(180deg, #4ade80, #fbbf24 25%, #f97316 65%, #ef4444);
  border-radius: 3px;
  opacity: 0.75;
  pointer-events: none;
}

.ruler-dot {
  position: fixed;
  left: 4px;
  width: 14px; height: 14px;
  background: #fbbf24;
  border: 2px solid #fff;
  border-radius: 50%;
  z-index: 401;
  pointer-events: none;
  transform: translateY(-50%);
  box-shadow: 0 2px 8px rgba(0,0,0,0.4);
  transition: top 0.05s linear;
}

.scroll-hint {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(15, 23, 42, 0.92);
  color: #94a3b8;
  font-family: system-ui, sans-serif;
  font-size: 12px;
  padding: 10px 20px;
  border-radius: 20px;
  animation: earth-pulse 2s ease infinite;
  pointer-events: none;
  transition: opacity 0.5s;
}

@keyframes earth-pulse {
  0%, 100% { opacity: 0.7; transform: translateX(-50%) translateY(0); }
  50% { opacity: 1; transform: translateX(-50%) translateY(4px); }
}

.zone-legend {
  position: fixed;
  left: 24px;
  bottom: 24px;
  z-index: 400;
  background: rgba(15, 23, 42, 0.88);
  color: #cbd5e1;
  font-family: system-ui, sans-serif;
  font-size: 10px;
  padding: 10px 12px;
  border-radius: 8px;
  border: 1px solid rgba(255,255,255,0.1);
  pointer-events: none;
  line-height: 1.55;
  max-width: 200px;
  transition: opacity 0.4s;
}

.zone-legend strong { color: #fbbf24; display: block; margin-bottom: 4px; font-size: 11px; }

@media (max-width: 700px) {
  .layer-label, .layer-label.right { font-size: 9px; max-width: 42vw; white-space: normal; }
  .feature { font-size: 9px; max-width: 46vw; white-space: normal; }
  .zone-legend { display: none; }
  .depth-hud { right: 8px; min-width: 120px; font-size: 11px; }
}
</style>

<div class="earth-viz" id="earthViz">
<p class="earth-viz-caption">
  Scale: <strong>10 px = 1 m</strong> &nbsp;·&nbsp; House ≈ 4.5 m &nbsp;·&nbsp; Representative continental crust ≈ 35 km to the Moho
  &nbsp;·&nbsp; <strong>Scroll to descend</strong>
  <span class="fact-note">Fact-checked anchors: Mponeng mine 4.0 km · Kola borehole 12.262 km · Moho depth varies ~25–70 km by region (35 km used here as a typical shield value). Temperatures are rough estimates using a ~25 °C/km geothermal gradient.</span>
</p>

<div class="ruler" id="ruler"></div>
<div class="ruler-dot" id="rulerDot"></div>

<div class="depth-hud" id="depthHud">
  <div class="label">Depth below surface</div>
  <div class="value" id="depthValue">0 m</div>
  <div class="meta" id="depthZone">surface</div>
  <div class="meta" id="depthTemp">~15 °C</div>
  <div class="meta" id="depthPressure">~0.1 MPa</div>
</div>

<div class="zone-legend" id="zoneLegend">
  <strong id="legendTitle">Surface</strong>
  <span id="legendBody">Soil, roots, and the frost line live in the first few metres.</span>
</div>

<div class="scroll-hint" id="scrollHint">↓ Scroll to descend ↓</div>

<div class="scene" id="scene">
  <div class="sky">
    <div class="sun"></div>
    <div class="cloud c1"></div>
    <div class="cloud c2"></div>
    <div class="scale-pin" style="top: 60%;">★ Surface — two-storey house at true scale (1.7 m person beside it)</div>
  </div>

  <div class="surface-row" id="surface">
    <div class="house-wrap">
      <div class="house">
        <div class="house-roof"></div>
        <div class="house-body">
          <div class="house-window w1"></div>
          <div class="house-window w2"></div>
          <div class="house-door"></div>
        </div>
      </div>
      <div class="person">
        <div class="person-head"></div>
        <div class="person-body"></div>
        <div class="person-legs"></div>
      </div>
    </div>
  </div>

  <div class="earth" id="earth">
    <div class="strata-texture" id="strataTexture"></div>
    <div class="moho">
      <div class="moho-line"></div>
      <div class="moho-label">
        <h2>Mohorovičić Discontinuity (Moho)</h2>
        <p>
          Base of the continental crust at ~35 km in this model.<br>
          Below lies the upper mantle — solid silicate rock under extreme pressure, not a magma ocean.<br>
          <em>You scrolled through 35 km of crust. The mantle continues another 2,800+ km to the core.</em>
        </p>
      </div>
    </div>
  </div>
</div>

<script>
(function() {
  const PX_PER_M = 10;
  const CRUST_M = 35000;
  const KOLA_M = 12262;
  const MPONENG_M = 4000;
  const SURFACE_TEMP = 15;
  const GRADIENT = 25; // °C per km — typical continental average

  const earth = document.getElementById('earth');
  const earthViz = document.getElementById('earthViz');
  const surface = document.getElementById('surface');
  const depthValue = document.getElementById('depthValue');
  const depthZone = document.getElementById('depthZone');
  const depthTemp = document.getElementById('depthTemp');
  const depthPressure = document.getElementById('depthPressure');
  const legendTitle = document.getElementById('legendTitle');
  const legendBody = document.getElementById('legendBody');
  const rulerDot = document.getElementById('rulerDot');
  const scrollHint = document.getElementById('scrollHint');

  const zones = [
    { max: 50,    name: 'Soil & regolith',      color: 'rgba(139,105,60,0.25)',  legend: 'Soil & regolith',      desc: 'Topsoil, subsoil, and weathered rock — rarely deeper than tens of metres.' },
    { max: 1000,  name: 'Sedimentary basin',    color: 'rgba(160,130,90,0.22)',  legend: 'Sedimentary basin',    desc: 'Layered sandstone, shale, limestone. Groundwater and aquifers common.' },
    { max: 5000,  name: 'Upper crystalline crust', color: 'rgba(110,100,95,0.2)', legend: 'Upper crust',          desc: 'Granite and metamorphic basement. Deepest mines (~4 km) reach this zone.' },
    { max: 15000, name: 'Mid-crust',            color: 'rgba(80,70,100,0.22)',   legend: 'Mid-crust',            desc: 'Ductile metamorphic rock under high pressure. No human excavation comes close.' },
    { max: 30000, name: 'Lower crust',          color: 'rgba(50,40,55,0.25)',    legend: 'Lower crust',          desc: 'Hot, dense granulites. Most earthquakes nucleate above ~30 km.' },
    { max: CRUST_M, name: 'Moho transition',    color: 'rgba(90,30,20,0.2)',     legend: 'Approaching Moho',     desc: 'Crust thins into mantle. Kola borehole reached only ~⅓ of the way here.' },
  ];

  const layers = [
    { top: 0,     label: 'O horizon — topsoil (0–0.3 m typical)', side: 'left' },
    { top: 2,     label: 'Subsoil & regolith', side: 'right' },
    { top: 15,    label: 'Frost line (~0.8–3 m in mid-latitudes)', side: 'left' },
    { top: 50,    label: 'Weathered bedrock', side: 'right' },
    { top: 200,   label: 'Sedimentary layers', side: 'left' },
    { top: 500,   label: 'Limestone / sandstone', side: 'right' },
    { top: 1000,  label: '1 km — sedimentary basin floor', side: 'left' },
    { top: 2000,  label: 'Crystalline basement', side: 'right' },
    { top: 3500,  label: '3.5 km — deepest natural caves (~2.2 km)', side: 'left' },
    { top: MPONENG_M, label: '4.0 km — Mponeng gold mine (deepest)', side: 'right' },
    { top: 7000,  label: '7 km — expected basaltic layer absent at Kola', side: 'left' },
    { top: 10000, label: '10 km — 2.5× deeper than any mine', side: 'right' },
    { top: KOLA_M, label: '12.262 km — Kola Superdeep SG-3 (deepest hole)', side: 'left' },
    { top: 15000, label: '15 km — mid-crust metamorphic zone', side: 'right' },
    { top: 20000, label: '20 km — ~500 °C, ~600 MPa', side: 'left' },
    { top: 25000, label: '25 km — lower crust begins', side: 'right' },
    { top: 30000, label: '30 km — most earthquakes stop above here', side: 'left' },
    { top: 33000, label: '33 km — crust thinning toward Moho', side: 'right' },
  ];

  const features = [
    { depth: 0.3,   text: 'Plant roots (~0.3–2 m)', left: '28%' },
    { depth: 1.5,   text: 'Burrowing organisms', left: '62%' },
    { depth: 30,    text: 'Shallow groundwater (varies 5–100+ m)', left: '18%' },
    { depth: 300,   text: 'Coal / oil-bearing strata (region-specific)', left: '68%' },
    { depth: 1200,  text: 'Typical deep mine workings (< 3.5 km)', left: '42%' },
    { depth: 3000,  text: 'Kola: free water found 3–6 km down', left: '72%' },
    { depth: MPONENG_M, text: 'Mponeng mine — 4.0 km, ~60 °C ambient', left: '22%' },
    { depth: 9000,  text: 'Kola: 180 °C at 11.9 km (drilling stopped)', left: '58%' },
    { depth: KOLA_M, text: 'Kola SG-3 final depth — 12,262 m (1989)', left: '30%' },
    { depth: 18000, text: 'Rare deep earthquakes (< 1% below 30 km)', left: '65%' },
  ];

  const milestones = [
    { depth: MPONENG_M, label: 'Mponeng 4.0 km', height: 180 },
    { depth: KOLA_M, label: 'Kola 12.262 km', height: 320 },
  ];

  // Grass
  const groundW = 120 * PX_PER_M;
  for (let i = 0; i < 100; i++) {
    const blade = document.createElement('div');
    blade.className = 'grass-blade';
    blade.style.left = (Math.random() * groundW) + 'px';
    blade.style.height = (4 + Math.random() * 12) + 'px';
    blade.style.background = `hsl(${100 + Math.random()*30}, ${45 + Math.random()*20}%, ${35 + Math.random()*15}%)`;
    surface.appendChild(blade);
  }

  // Zone tints
  let prev = 0;
  zones.forEach(z => {
    const el = document.createElement('div');
    el.className = 'zone-tint';
    el.style.top = (prev * PX_PER_M) + 'px';
    el.style.height = ((z.max - prev) * PX_PER_M) + 'px';
    el.style.background = z.color;
    earth.insertBefore(el, earth.firstChild.nextSibling);
    prev = z.max;
  });

  // Strata bands — denser near surface for visible motion
  function addStrata(interval, maxDepth, palette) {
    for (let d = interval; d <= maxDepth; d += interval) {
      const band = document.createElement('div');
      band.className = 'stratum-band';
      band.style.top = (d * PX_PER_M) + 'px';
      band.style.height = Math.max(3, interval * PX_PER_M * 0.35) + 'px';
      const c = palette[Math.floor(d / interval) % palette.length];
      band.style.background = `linear-gradient(180deg, ${c}55, ${c}22)`;
      earth.appendChild(band);
    }
  }
  addStrata(25, 500, ['#a08050', '#8b7048', '#7a6240', '#6e5a42', '#9a8868']);
  addStrata(80, 3000, ['#7a6e62', '#6a5e54', '#8a7a6a', '#5e5248']);
  addStrata(250, 12000, ['#5a5458', '#4a4448', '#6a5a62', '#3e3840']);
  addStrata(600, CRUST_M, ['#3a3238', '#2e2830', '#4a3a40', '#262228']);

  // Mineral grains for texture
  for (let i = 0; i < 500; i++) {
    const g = document.createElement('div');
    g.className = 'grain';
    const depth = Math.pow(Math.random(), 1.4) * CRUST_M;
    g.style.top = (depth * PX_PER_M) + 'px';
    g.style.left = (5 + Math.random() * 90) + '%';
    const size = 2 + Math.random() * 5;
    g.style.width = size + 'px';
    g.style.height = size + 'px';
    g.style.background = Math.random() > 0.5 ? 'rgba(255,255,255,0.2)' : 'rgba(0,0,0,0.25)';
    earth.appendChild(g);
  }

  layers.forEach(l => {
    const el = document.createElement('div');
    el.className = 'layer-label' + (l.side === 'right' ? ' right' : '');
    el.style.top = (l.top * PX_PER_M) + 'px';
    el.textContent = l.label;
    earth.appendChild(el);
  });

  const tickIntervals = [
    { every: 10, max: 100 },
    { every: 50, max: 500 },
    { every: 100, max: 2000 },
    { every: 500, max: 10000 },
    { every: 1000, max: CRUST_M },
  ];
  const ticks = new Set();
  tickIntervals.forEach(({ every, max }) => {
    for (let d = every; d <= max; d += every) ticks.add(d);
  });
  ticks.add(KOLA_M);
  ticks.add(MPONENG_M);
  ticks.add(CRUST_M);

  [...ticks].sort((a, b) => a - b).forEach(d => {
    const tick = document.createElement('div');
    const isMajor = d % 1000 === 0 || d === CRUST_M || d === KOLA_M || d === MPONENG_M;
    tick.className = 'depth-tick' + (isMajor ? ' major' : '');
    tick.style.top = (d * PX_PER_M) + 'px';
    const span = document.createElement('span');
    span.textContent = d >= 1000 ? (d / 1000).toFixed(d === KOLA_M ? 3 : (d % 1000 === 0 ? 0 : 1)) + ' km' : d + ' m';
    tick.appendChild(span);
    earth.appendChild(tick);
  });

  features.forEach(f => {
    const el = document.createElement('div');
    el.className = 'feature';
    el.style.top = (f.depth * PX_PER_M) + 'px';
    el.style.left = f.left;
    el.textContent = f.text;
    earth.appendChild(el);
  });

  milestones.forEach(m => {
    const col = document.createElement('div');
    col.className = 'milestone';
    col.style.top = (m.depth * PX_PER_M) + 'px';
    col.style.height = m.height + 'px';
    const tag = document.createElement('div');
    tag.className = 'milestone-tag';
    tag.textContent = m.label;
    col.appendChild(tag);
    earth.appendChild(col);
  });

  const wt = document.createElement('div');
  wt.className = 'water-table';
  wt.style.top = (40 * PX_PER_M) + 'px';
  earth.appendChild(wt);

  [[80, 25, 50, 28], [220, 58, 65, 38], [420, 18, 55, 32], [780, 42, 48, 26], [1500, 65, 70, 40]].forEach(([d, l, w, h]) => {
    const a = document.createElement('div');
    a.className = 'aquifer-pocket';
    a.style.top = (d * PX_PER_M) + 'px';
    a.style.left = l + '%';
    a.style.width = w + 'px';
    a.style.height = h + 'px';
    earth.appendChild(a);
  });

  [[40, 12, 10, 140], [150, 38, 7, 220], [650, 22, 6, 380], [2200, 48, 5, 520], [7500, 32, 4, 900], [18000, 55, 3, 1400]].forEach(([d, l, w, h]) => {
    const v = document.createElement('div');
    v.className = 'rock-vein';
    v.style.top = (d * PX_PER_M) + 'px';
    v.style.left = l + '%';
    v.style.width = w + 'px';
    v.style.height = h + 'px';
    earth.appendChild(v);
  });

  [[450, 42, 520], [2800, 58, 1600], [8500, 36, 2400], [16000, 62, 3000]].forEach(([d, l, h]) => {
    const f = document.createElement('div');
    f.className = 'fault-line';
    f.style.top = (d * PX_PER_M) + 'px';
    f.style.left = l + '%';
    f.style.height = h + 'px';
    f.style.transform = `rotate(${6 + Math.random() * 10}deg)`;
    earth.appendChild(f);
  });

  for (let i = 0; i < 60; i++) {
    const inc = document.createElement('div');
    inc.className = 'inclusion';
    const depth = 4000 + Math.random() * 26000;
    inc.style.top = (depth * PX_PER_M) + 'px';
    inc.style.left = (8 + Math.random() * 84) + '%';
    inc.style.width = (4 + Math.random() * 14) + 'px';
    inc.style.height = (3 + Math.random() * 10) + 'px';
    inc.style.opacity = 0.2 + Math.random() * 0.35;
    earth.appendChild(inc);
  }

  let surfaceOffset = 0;

  function updateSurfaceOffset() {
    const rect = surface.getBoundingClientRect();
    surfaceOffset = window.scrollY + rect.bottom;
  }

  function formatDepth(m) {
    if (m < 1000) return Math.round(m) + ' m';
    if (m < 10000) return (m / 1000).toFixed(m === KOLA_M ? 3 : 1) + ' km';
    return Math.round(m / 1000) + ' km';
  }

  function estimateTempC(depthM) {
    return Math.round(SURFACE_TEMP + (depthM / 1000) * GRADIENT);
  }

  function estimatePressureMPa(depthM) {
    // ~30 MPa per km in rock
    return (0.1 + (depthM / 1000) * 30).toFixed(depthM < 1000 ? 1 : 0);
  }

  function getZone(depthM) {
    return zones.find(z => depthM < z.max) || zones[zones.length - 1];
  }

  function onScroll() {
    const scrollY = window.scrollY;
    const depthM = Math.max(0, (scrollY + 200 - (surfaceOffset - window.innerHeight * 0.3)) / PX_PER_M);
    const clamped = Math.min(depthM, CRUST_M);

    depthValue.textContent = formatDepth(clamped);

    const zone = getZone(clamped);
    depthZone.textContent = zone.name;
    legendTitle.textContent = zone.legend;
    legendBody.textContent = zone.desc;
    depthTemp.textContent = '~' + estimateTempC(clamped) + ' °C';
    depthPressure.textContent = '~' + estimatePressureMPa(clamped) + ' MPa';

    // Parallax strata — stronger at shallow depths
    const parallaxStrength = clamped < 2000 ? 0.35 : clamped < 10000 ? 0.18 : 0.08;
    earthViz.style.setProperty('--parallax-y', (scrollY * parallaxStrength) + 'px');

    const introH = 88;
    const rulerH = window.innerHeight - introH - 20;
    const progress = Math.min(scrollY / (document.body.scrollHeight - window.innerHeight), 1);
    rulerDot.style.top = (introH + progress * rulerH) + 'px';

    if (scrollY > 300) scrollHint.style.opacity = '0';
    if (clamped > 500) document.getElementById('zoneLegend').style.opacity = '0.95';
  }

  updateSurfaceOffset();
  window.addEventListener('scroll', onScroll, { passive: true });
  window.addEventListener('resize', () => { updateSurfaceOffset(); onScroll(); });
  onScroll();
})();
</script>
</div>
