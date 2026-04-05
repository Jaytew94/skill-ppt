# SKILL: ppt-build
**Step 4 of 4 — HTML Generation**
**Reads: ppt-brief + ppt-design-core + themes/theme-[X] → outputs complete .html file**

---

## ⛔ GEMINI / ANTIGRAVITY CRITICAL RULES

```
These rules exist because Gemini Pro generates code that breaks in Antigravity's iframe viewer.
Violating any of these = blank screen or broken layout.

RULE 1 — scaleToFit() MUST use parentElement, not window:
  WRONG:  const availW = window.innerWidth;
  CORRECT: const availW = container && container !== document.body
             ? container.clientWidth : window.innerWidth;
  Reason: Antigravity embeds PPT in iframe. window.innerWidth = outer page width.
  parentElement.clientWidth = actual iframe width. Using wrong value = content invisible.

RULE 2 — scaleToFit() MUST run with setTimeout(100) on first call:
  Reason: iframe dimensions are not ready at DOMContentLoaded. Need 100ms delay.

RULE 3 — EVERY element that should appear MUST have class="animate":
  Without it: opacity:0 CSS hides element permanently (GSAP never runs on it)
  No exceptions. Every h1, h2, h3, p, div with content = must have class="animate"

RULE 4 — Images: ONLY picsum.photos. Never empty src. Never unsplash.
  CORRECT: https://picsum.photos/seed/technology1/1200/1080
  WRONG:   src="" or source.unsplash.com/...

RULE 5 — Font sizes: never below these minimums on 1920×1080 canvas:
  body/p: 24px min | h3: 48px min | h2: 80px min | h1: 120px min

RULE 6 — Ambient must have GSAP animation. Never static decorations.
  Every ambient element must call gsap.to() or run a canvas loop.

RULE 7 — PDF export uses html2canvas, NOT window.print():
  window.print() = broken layout, missing images, wrong fonts
  html2canvas = pixel-perfect screenshot → one-click PDF download
```

---

## MODULE 1 — HTML BOILERPLATE

```html
<!DOCTYPE html>
<html lang="[zh/en]">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>[TITLE]</title>
<link href="https://fonts.googleapis.com/css2?family=[GOOGLE_FONTS]&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/animejs/3.2.1/anime.min.js"></script>
<style>

*,*::before,*::after{margin:0;padding:0;box-sizing:border-box;}

:root{
  /* INSERT FULL :root BLOCK FROM THEME FILE */
}

/* ── SCALE WRAPPER ──
   CRITICAL: transform-origin top left. margin-based centering.
   Works in standalone browser AND Antigravity iframe. */
html,body{width:100%;height:100%;overflow:hidden;background:#000;margin:0;padding:0;}
body{display:flex;align-items:flex-start;justify-content:flex-start;
  font-family:var(--font-body);}
.scaler{
  width:1920px;height:1080px;transform-origin:top left;
  position:relative;overflow:hidden;
  background:var(--bg-primary);color:var(--text-primary);flex-shrink:0;
}

/* ── ANIMATE DEFAULTS — prevents 1-frame flash ── */
.animate{opacity:0;}
.hero-title{opacity:0;}
.closing-title{opacity:0;}
.chapter-word{opacity:0;}

/* ── SLIDES ── */
.slide{position:absolute;inset:0;width:1920px;height:1080px;
  overflow:hidden;display:flex;opacity:0;pointer-events:none;z-index:0;}
.slide.active{opacity:1;pointer-events:all;z-index:10;}

/* ── LAYERS ── */
.ambient-layer{position:absolute;inset:0;z-index:1;pointer-events:none;overflow:hidden;}
.slide-content{position:relative;z-index:10;width:100%;height:100%;}

/* ── TYPOGRAPHY ── */
.eyebrow{font-size:16px;text-transform:uppercase;letter-spacing:3px;
  color:var(--color-accent);display:block;margin-bottom:20px;}
h1,.hero-title{font-family:var(--font-display);font-size:140px;line-height:0.88;
  letter-spacing:0;word-break:keep-all;}
h2{font-family:var(--font-display);font-size:80px;line-height:1.05;
  letter-spacing:0;word-break:keep-all;}
h3{font-family:var(--font-display);font-size:52px;line-height:1.1;
  letter-spacing:0;word-break:keep-all;}
p{font-size:28px;line-height:1.55;color:var(--text-secondary);}
.divider{width:80px;height:3px;background:var(--color-primary);
  border-radius:2px;margin:28px 0;}
.ghost-text{position:absolute;font-size:280px;font-family:var(--font-display);
  font-weight:900;opacity:0.04;pointer-events:none;user-select:none;
  color:var(--text-primary);line-height:1;}

/* ── BULLET LIST ── */
.bullet-list{list-style:none;margin-top:28px;}
.bullet-list li{font-size:24px;color:var(--text-secondary);padding-left:36px;
  margin-bottom:20px;position:relative;line-height:1.4;word-break:keep-all;}
.bullet-list li::before{content:'';position:absolute;left:0;top:10px;
  width:12px;height:12px;border-radius:50%;background:var(--color-primary);}

/* ── SPLIT ── */
.l-split{flex-direction:row;align-items:stretch;}
.l-split .content-pane{flex:0 0 46%;padding:100px 80px;
  display:flex;flex-direction:column;justify-content:center;}
.l-split .visual-pane{flex:1;position:relative;overflow:hidden;}
.l-split .visual-pane img{width:100%;height:100%;object-fit:cover;}
.l-split .content-pane h2{font-size:58px;}
.l-split .content-pane h3{font-size:36px;}
.l-split .content-pane p,.l-split .content-pane li{font-size:24px;}

/* ── CHAPTER ── */
.l-chapter{flex-direction:column;justify-content:center;align-items:center;text-align:center;}
.chapter-word{font-family:var(--font-display);font-size:220px;line-height:0.85;
  color:var(--color-primary);letter-spacing:0;word-break:keep-all;}
.chapter-line{height:4px;background:var(--color-accent);margin:28px auto 0;
  border-radius:2px;width:0;}

/* ── DATA ── */
.l-data{flex-direction:column;justify-content:center;align-items:center;
  padding:60px 100px;text-align:center;}
.data-grid{display:flex;gap:80px;margin-top:60px;align-items:flex-end;}
.data-number{font-size:160px;font-family:var(--font-display);
  line-height:0.9;color:var(--color-primary);}
.data-label{font-size:20px;color:var(--text-secondary);margin-top:12px;letter-spacing:2px;}

/* ── COMPARISON ── */
.l-compare{display:grid;grid-template-columns:1fr 4px 1fr;
  width:100%;height:100%;align-items:center;padding:80px 100px;gap:50px;}
.compare-divider{height:65%;background:linear-gradient(180deg,transparent,
  var(--color-primary),transparent);border-radius:4px;margin:auto;}
.compare-col{display:flex;flex-direction:column;justify-content:center;}
.compare-col h3{font-family:var(--font-display);font-size:52px;
  margin-bottom:28px;word-break:keep-all;}
.compare-col li{font-size:24px;color:var(--text-secondary);padding:12px 0;
  border-bottom:1px solid rgba(255,255,255,0.07);list-style:none;}

/* ── STATEMENT ── */
.l-statement{flex-direction:column;justify-content:center;align-items:center;
  text-align:center;padding:0 180px;}
.statement-text{font-family:var(--font-display);font-size:90px;
  line-height:1.05;word-break:keep-all;}

/* ── CLOSING ── */
.l-closing{flex-direction:column;justify-content:center;padding:0 160px;}
.closing-title{font-family:var(--font-display);font-size:140px;
  line-height:0.85;word-break:keep-all;}

/* ── THREE COLUMN ── */
.l-three-col{flex-direction:column;padding:70px 100px;overflow:hidden;}
.three-col-grid{display:grid;grid-template-columns:1fr 1fr 1fr;gap:28px;
  margin-top:36px;flex:1;width:100%;box-sizing:border-box;overflow:hidden;}
.col-card{display:flex;flex-direction:column;background:var(--surface);
  border-radius:8px;overflow:hidden;min-width:0;}
.col-card-img{height:220px;overflow:hidden;flex-shrink:0;position:relative;}
.col-card-img img{width:100%;height:100%;object-fit:cover;display:block;}
.col-card-img::after{content:'';position:absolute;inset:0;
  background:linear-gradient(to bottom,transparent 40%,var(--bg-primary) 100%);}
.col-card-body{padding:20px 24px 24px;flex:1;border-left:2px solid var(--color-primary);}
.col-num{font-family:var(--font-display);font-size:40px;
  color:var(--color-primary);margin-bottom:8px;line-height:1;}
.col-card-body h3{font-size:30px;margin-bottom:10px;word-break:keep-all;}
.col-card-body p{font-size:20px;color:var(--text-secondary);line-height:1.4;
  display:-webkit-box;-webkit-line-clamp:4;-webkit-box-orient:vertical;overflow:hidden;}

/* ── 2×2 GRID ── */
.grid22{display:grid;grid-template-columns:1fr 1fr;gap:22px;
  margin-top:32px;flex:1;width:100%;box-sizing:border-box;}
.grid-card{background:var(--surface);border-radius:8px;overflow:hidden;
  display:flex;flex-direction:column;}
.grid-card-img{height:160px;overflow:hidden;flex-shrink:0;}
.grid-card-img img{width:100%;height:100%;object-fit:cover;}
.grid-card-body{padding:20px 24px;flex:1;border-left:3px solid var(--color-primary);}
.grid-card-body h3{font-size:30px;margin-bottom:10px;word-break:keep-all;}
.grid-card-body p{font-size:20px;color:var(--text-secondary);line-height:1.4;}

/* ── FULL BLEED ── */
.l-fullbleed{position:relative;}
.l-fullbleed .fb-img{position:absolute;inset:0;}
.l-fullbleed .fb-img img{width:100%;height:100%;object-fit:cover;display:block;}
.l-fullbleed .fb-overlay{position:absolute;inset:0;
  background:linear-gradient(to top,rgba(0,0,0,0.88) 0%,transparent 60%);}
.l-fullbleed .fb-content{position:absolute;bottom:80px;left:120px;right:120px;}

/* ── EDITABLE ── */
.data-editable{cursor:default;}
.edit-mode .data-editable{cursor:pointer;
  outline:2px dashed var(--color-primary);outline-offset:8px;}
.edit-mode img{cursor:pointer !important;
  outline:2px dashed rgba(255,255,255,0.3) !important;transition:filter 0.2s;}
.edit-mode img:hover{outline:2px solid var(--color-primary) !important;filter:brightness(1.1);}

/* ── PROGRESS BAR ── */
.progress-track{width:100%;height:12px;background:rgba(255,255,255,0.1);
  border-radius:6px;overflow:hidden;}
.edit-mode .progress-track{cursor:pointer;outline:1px dashed var(--color-primary);}
.progress-fill{height:100%;background:var(--color-primary);border-radius:6px;}

/* ── IMG ZONE ── */
.img-zone{width:100%;height:100%;position:relative;overflow:hidden;cursor:pointer;}
.img-zone img{position:absolute;inset:0;width:100%;height:100%;object-fit:cover;}
.img-zone input[type=file]{position:absolute;inset:0;opacity:0;cursor:pointer;z-index:5;}
.img-zone .img-delete{position:absolute;top:10px;right:10px;width:32px;height:32px;
  border-radius:50%;background:rgba(0,0,0,0.7);border:none;color:#fff;
  font-size:16px;cursor:pointer;display:none;align-items:center;justify-content:center;}
.img-zone:hover .img-delete{display:flex;}

/* ── IP CHARACTER ── */
.ip-character{position:fixed;bottom:0;right:80px;width:200px;height:280px;
  z-index:50;pointer-events:none;transform-origin:bottom center;}

/* ── PRESENTER BAR ── */
.presenter-bar{position:fixed;bottom:20px;left:40px;display:flex;align-items:center;
  gap:14px;z-index:200;background:rgba(0,0,0,0.4);backdrop-filter:blur(10px);
  border:1px solid rgba(255,255,255,0.1);border-radius:40px;padding:8px 16px 8px 8px;}
.presenter-avatar{width:44px;height:44px;border-radius:50%;
  border:2px solid var(--color-primary);overflow:hidden;background:var(--surface);
  display:flex;align-items:center;justify-content:center;
  font-size:18px;color:var(--color-primary);flex-shrink:0;}
.presenter-name{font-size:15px;color:var(--text-primary);font-weight:600;}
.presenter-title{font-size:12px;color:var(--text-secondary);}
.presenter-brand{font-size:11px;color:var(--color-primary);
  letter-spacing:1px;text-transform:uppercase;}

/* ── NAV / CHROME ── */
.progress-bar-ui{position:fixed;bottom:0;left:0;height:3px;
  background:var(--color-primary);z-index:400;transition:width .4s ease;}
.slide-counter{position:fixed;bottom:22px;right:40px;font-size:18px;
  color:rgba(255,255,255,0.4);letter-spacing:2px;z-index:400;}
.nav-dots{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);
  display:flex;gap:8px;z-index:400;}
.nav-dot{width:7px;height:7px;border-radius:50%;
  background:rgba(255,255,255,0.3);cursor:pointer;transition:all .3s;}
.nav-dot.active{background:var(--color-primary);width:22px;border-radius:4px;}
.ui-chrome{position:fixed;top:22px;right:22px;display:flex;gap:10px;z-index:500;}
.ui-btn{padding:10px 20px;border-radius:8px;border:none;font-size:16px;
  cursor:pointer;transition:all .2s;backdrop-filter:blur(12px);}
.btn-edit{background:rgba(255,255,255,0.12);color:#fff;
  border:1px solid rgba(255,255,255,0.18);}
.btn-pdf{background:rgba(255,255,255,0.10);color:#fff;
  border:1px solid rgba(255,255,255,0.18);}
.btn-present{background:var(--color-primary);color:#fff;font-weight:700;}

/* ── EDIT MODE ── */
.edit-mode h1,.edit-mode h2,.edit-mode h3,.edit-mode p,.edit-mode li,
.edit-mode .eyebrow,.edit-mode .chapter-word,.edit-mode .statement-text,
.edit-mode .closing-title{outline:1px dashed rgba(255,255,255,0.2);
  cursor:text;border-radius:2px;}

/* ── PRESENT MODE ── */
.present-mode .ui-chrome,.present-mode .nav-dots,.present-mode .slide-counter,
.present-mode .progress-bar-ui,.present-mode .presenter-bar{display:none!important;}

/* ── PDF PROGRESS OVERLAY ── */
#pdfOverlay{display:none;position:fixed;inset:0;background:rgba(0,0,0,0.85);
  z-index:9999;align-items:center;justify-content:center;flex-direction:column;gap:20px;}
#pdfOverlay.show{display:flex;}
#pdfProgress{width:400px;height:8px;background:rgba(255,255,255,0.2);border-radius:4px;overflow:hidden;}
#pdfProgressFill{height:100%;background:var(--color-primary);border-radius:4px;
  width:0%;transition:width 0.3s ease;}
#pdfStatusText{color:white;font-size:22px;font-family:sans-serif;}

</style>
</head>
<body>

<!-- PDF Progress Overlay -->
<div id="pdfOverlay">
  <div id="pdfStatusText">正在生成PDF...</div>
  <div id="pdfProgress"><div id="pdfProgressFill"></div></div>
  <div style="color:rgba(255,255,255,0.5);font-size:16px;font-family:sans-serif;">
    请勿关闭此窗口
  </div>
</div>

<!-- UI Chrome -->
<div class="progress-bar-ui" id="progressBar"></div>
<div class="slide-counter" id="slideCounter"></div>
<div class="nav-dots" id="navDots"></div>
<div class="ui-chrome">
  <button class="ui-btn btn-edit" onclick="toggleEdit()">✏️ 编辑</button>
  <button class="ui-btn btn-pdf" id="pdfBtn" onclick="exportPDF()">📄 导出PDF</button>
  <button class="ui-btn btn-present" onclick="startPresent()">▶ 演示</button>
</div>

<!-- IP Character (remove div if not needed) -->
<div id="ipCharacter" class="ip-character" style="display:none;cursor:pointer;"
  title="点击上传吉祥物图片"></div>

<!-- Presenter Bar (remove div if not needed) -->
<div class="presenter-bar" id="presenterBar" style="display:none;">
  <div class="presenter-avatar" id="presenterAvatar">[AB]</div>
  <div>
    <div class="presenter-name">[NAME]</div>
    <div class="presenter-title">[TITLE]</div>
    <div class="presenter-brand">[BRAND]</div>
  </div>
</div>

<div class="scaler" id="scaler">
  <div class="ambient-layer" id="ambientLayer"></div>
  <!-- ═══ SLIDES START ═══ -->
  <!-- Generate all <section class="slide"> here -->
  <!-- Follow personality rhythm from ppt-brief + theme identity from theme file -->
  <!-- ═══ SLIDES END ═══ -->
</div>

<script>
// ══════════════════════════════════════════════════
// SCALE TO FIT — CRITICAL: uses parentElement for Antigravity iframe compatibility
// DO NOT change window.innerWidth to parentElement without the fallback below
// ══════════════════════════════════════════════════
function scaleToFit(){
  const el=document.getElementById('scaler');
  const container=el.parentElement;
  const availW=(container&&container!==document.body)?container.clientWidth:window.innerWidth;
  const availH=(container&&container!==document.body)?container.clientHeight:window.innerHeight;
  if(!availW||!availH)return; // guard against 0 dimensions
  const scale=Math.min(availW/1920,availH/1080);
  el.style.transform=`scale(${scale})`;
  el.style.marginLeft=Math.max(0,(availW-1920*scale)/2)+'px';
  el.style.marginTop=Math.max(0,(availH-1080*scale)/2)+'px';
}
window.addEventListener('resize',scaleToFit);
window.addEventListener('orientationchange',scaleToFit);
document.fonts.ready.then(()=>setTimeout(scaleToFit,50));
setTimeout(scaleToFit,100); // iframe delay fix for Antigravity
scaleToFit();

// ══════════════════════════════════════════════════
// ENGINE
// ══════════════════════════════════════════════════
const slides=document.querySelectorAll('.slide');
const total=slides.length;
let cur=0,animating=false,editMode=false,presentMode=false;

// Nav dots
const dotsEl=document.getElementById('navDots');
slides.forEach((_,i)=>{
  const d=document.createElement('div');
  d.className='nav-dot'+(i===0?' active':'');
  d.onclick=()=>goTo(i);
  dotsEl.appendChild(d);
});

function goTo(i){
  if(animating||i===cur||i<0||i>=total)return;
  animating=true;
  const old=slides[cur],nw=slides[i],dir=i>cur?1:-1;
  resetAnims(old);
  gsap.set(nw,{zIndex:15,x:dir*80,opacity:0});
  nw.classList.add('active');
  gsap.to(old,{opacity:0,x:dir*-60,duration:.4,ease:'power2.in',
    onComplete:()=>{old.classList.remove('active');gsap.set(old,{x:0,zIndex:0});}});
  gsap.to(nw,{opacity:1,x:0,duration:.5,ease:'power3.out',
    onComplete:()=>{animating=false;gsap.set(nw,{zIndex:10});}});
  setTimeout(()=>triggerAnim(nw),180);
  cur=i;updateUI();
}
function next(){goTo(cur+1);}
function prev(){goTo(cur-1);}

function resetAnims(s){
  gsap.set(s.querySelectorAll('.animate'),{opacity:0,y:55,x:0,skewY:0,scale:1});
  gsap.set(s.querySelectorAll('.hero-title,.closing-title'),{opacity:0,y:100,skewY:0});
  gsap.set(s.querySelectorAll('.chapter-word'),{opacity:0,scale:0.82});
  gsap.set(s.querySelectorAll('.chapter-line'),{width:0});
  gsap.set(s.querySelectorAll('.timeline-line-h'),{width:0});
}

function updateUI(){
  document.getElementById('progressBar').style.width=((cur+1)/total*100)+'%';
  document.getElementById('slideCounter').textContent=`${cur+1} / ${total}`;
  document.querySelectorAll('.nav-dot').forEach((d,i)=>{
    const a=i===cur;
    d.classList.toggle('active',a);
    d.style.width=a?'22px':'7px';
    d.style.borderRadius=a?'4px':'50%';
    d.style.background=a?'var(--color-primary)':'rgba(255,255,255,0.3)';
  });
}

function triggerAnim(slide){
  const type=slide.dataset.animation||'stagger';
  if(type==='stagger')
    gsap.fromTo(slide.querySelectorAll('.animate'),
      {opacity:0,y:55},{opacity:1,y:0,duration:.75,stagger:.13,ease:'power3.out'});
  if(type==='cinematic'){
    const t=slide.querySelector('.hero-title,.closing-title');
    if(t)gsap.fromTo(t,{opacity:0,y:100,skewY:4},
      {opacity:1,y:0,skewY:0,duration:1.1,ease:'expo.out'});
    gsap.fromTo(slide.querySelectorAll('.animate:not(.hero-title):not(.closing-title)'),
      {opacity:0,y:28},{opacity:1,y:0,duration:.8,stagger:.1,delay:.45,ease:'power3.out'});
  }
  if(type==='chapter'){
    gsap.fromTo(slide.querySelector('.chapter-word'),
      {opacity:0,scale:.82},{opacity:1,scale:1,duration:1,ease:'back.out(1.7)'});
    gsap.to(slide.querySelector('.chapter-line'),
      {width:140,duration:.9,delay:.55,ease:'power4.out'});
    gsap.fromTo(slide.querySelectorAll('.animate'),
      {opacity:0,y:-20},{opacity:1,y:0,duration:.6,stagger:.1,delay:.2,ease:'power2.out'});
  }
  if(type==='counter'){
    gsap.fromTo(slide.querySelectorAll('.animate'),
      {opacity:0,y:40},{opacity:1,y:0,duration:.75,stagger:.1,ease:'power3.out'});
    slide.querySelectorAll('.counter').forEach(el=>
      anime({targets:el,innerHTML:[0,+el.dataset.target],round:1,
        duration:2200,easing:'easeOutExpo',delay:350}));
  }
  if(type==='timeline'){
    gsap.to(slide.querySelector('.timeline-line-h'),
      {width:'100%',duration:1.2,ease:'power3.out'});
    gsap.fromTo(slide.querySelectorAll('.t-node'),
      {opacity:0,y:30},{opacity:1,y:0,stagger:.18,delay:.8,duration:.6,ease:'power3.out'});
  }
  if(type==='process')
    gsap.fromTo(slide.querySelectorAll('.process-step'),
      {opacity:0,x:40},{opacity:1,x:0,stagger:.2,duration:.7,ease:'power3.out'});
  if(type==='wipe')
    slide.querySelectorAll('.animate').forEach((el,i)=>
      gsap.fromTo(el,{clipPath:'inset(0 100% 0 0)'},
        {clipPath:'inset(0 0% 0 0)',duration:.9,delay:i*.15,ease:'power4.inOut'}));
}

// ══════════════════════════════════════════════════
// KEYBOARD + REMOTE CONTROL
// Supports: arrow keys, space, PageUp/Down, remote control media keys
// Focus fix: body gets tabindex so it receives keys without click first
// ══════════════════════════════════════════════════
document.body.setAttribute('tabindex','0');
document.body.focus();

document.addEventListener('keydown',e=>{
  // Standard navigation
  if(['ArrowRight','ArrowDown',' ','PageDown'].includes(e.key)){e.preventDefault();next();}
  if(['ArrowLeft','ArrowUp','PageUp'].includes(e.key)){e.preventDefault();prev();}
  // Present mode
  if(e.key==='F5'||e.key==='f'||e.key==='F'){e.preventDefault();startPresent();}
  if(e.key==='Escape')exitPresent();
  // Remote control media keys (Logitech, Kensington etc.)
  if(e.key==='MediaNextTrack'||e.key==='MediaTrackNext')next();
  if(e.key==='MediaPreviousTrack'||e.key==='MediaTrackPrevious')prev();
  // B key = black screen toggle (common presenter remote feature)
  if(e.key==='b'||e.key==='B'){
    const scaler=document.getElementById('scaler');
    scaler.style.visibility=scaler.style.visibility==='hidden'?'visible':'hidden';
  }
});

// Re-focus after click anywhere (ensures remote works after UI interaction)
document.body.addEventListener('click',()=>{
  if(!editMode)document.body.focus();
});

// Touch swipe
let tx=0;
document.addEventListener('touchstart',e=>{tx=e.touches[0].clientX;},{passive:true});
document.addEventListener('touchend',e=>{
  const dx=tx-e.changedTouches[0].clientX;
  if(Math.abs(dx)>50){dx>0?next():prev();}
},{passive:true});

// ══════════════════════════════════════════════════
// THREE MODES: Edit / Present / Default
// ══════════════════════════════════════════════════
function toggleEdit(){
  if(presentMode)return;
  editMode=!editMode;
  document.body.classList.toggle('edit-mode',editMode);
  document.querySelector('.btn-edit').textContent=editMode?'✅ 完成':'✏️ 编辑';
  document.querySelectorAll('h1,h2,h3,p,li,.eyebrow,.chapter-word,.statement-text,.closing-title')
    .forEach(el=>el.contentEditable=editMode?'true':'false');
  if(editMode){
    document.querySelectorAll('.counter').forEach(el=>
      el.title='点击编辑数值（动画同步更新）');
  }
}

function startPresent(){
  if(editMode)toggleEdit();
  presentMode=true;
  document.body.classList.add('present-mode');
  document.body.focus(); // ensure keyboard focus for remote
  const el=document.documentElement;
  if(el.requestFullscreen)el.requestFullscreen().catch(()=>{});
  else if(el.webkitRequestFullscreen)el.webkitRequestFullscreen();
}
function exitPresent(){
  presentMode=false;
  document.body.classList.remove('present-mode');
  if(document.exitFullscreen)document.exitFullscreen().catch(()=>{});
  else if(document.webkitExitFullscreen)document.webkitExitFullscreen();
}
document.addEventListener('fullscreenchange',()=>{
  if(!document.fullscreenElement)exitPresent();
});

// ══════════════════════════════════════════════════
// PDF EXPORT — html2canvas screenshot method
// One-click → auto screenshots all slides → downloads PDF
// No broken layouts, no missing images, 100% visual fidelity
// ══════════════════════════════════════════════════
function loadScript(src){
  return new Promise((resolve,reject)=>{
    if(document.querySelector(`script[src="${src}"]`)){resolve();return;}
    const s=document.createElement('script');
    s.src=src;s.onload=resolve;s.onerror=reject;
    document.head.appendChild(s);
  });
}

async function exportPDF(){
  const btn=document.getElementById('pdfBtn');
  const overlay=document.getElementById('pdfOverlay');
  const statusText=document.getElementById('pdfStatusText');
  const progressFill=document.getElementById('pdfProgressFill');

  try{
    // Load libraries
    btn.textContent='⏳ 加载中...';
    await loadScript('https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js');
    await loadScript('https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js');

    // Show progress overlay
    overlay.classList.add('show');
    document.body.classList.add('present-mode'); // hide UI chrome

    const {jsPDF}=window.jspdf;
    const pdf=new jsPDF({orientation:'landscape',unit:'px',format:[1920,1080]});
    const allSlides=document.querySelectorAll('.slide');
    const savedCur=cur;

    for(let i=0;i<allSlides.length;i++){
      // Update progress
      const pct=Math.round((i/allSlides.length)*100);
      progressFill.style.width=pct+'%';
      statusText.textContent=`正在截图第 ${i+1} / ${allSlides.length} 页...`;

      // Show this slide temporarily
      allSlides.forEach(s=>{
        s.style.opacity='0';s.style.zIndex='0';
        s.style.position='absolute';
      });
      allSlides[i].style.opacity='1';
      allSlides[i].style.zIndex='10';

      // Wait for render
      await new Promise(r=>setTimeout(r,250));

      // Screenshot
      const canvas=await html2canvas(allSlides[i],{
        width:1920,height:1080,
        scale:1,
        useCORS:true,
        allowTaint:true,
        backgroundColor:null,
        logging:false
      });

      const imgData=canvas.toDataURL('image/jpeg',0.92);
      if(i>0)pdf.addPage([1920,1080],'landscape');
      pdf.addImage(imgData,'JPEG',0,0,1920,1080);
    }

    // Final progress
    progressFill.style.width='100%';
    statusText.textContent='✅ 正在生成文件...';
    await new Promise(r=>setTimeout(r,300));

    // Download
    const title=document.title||'presentation';
    pdf.save(`${title}.pdf`);

    // Restore
    overlay.classList.remove('show');
    document.body.classList.remove('present-mode');
    btn.textContent='📄 导出PDF';
    allSlides.forEach((s,i)=>{
      s.style.opacity=i===savedCur?'1':'0';
      s.style.zIndex=i===savedCur?'10':'0';
      s.style.position='absolute';
    });
    cur=savedCur;
    updateUI();

  }catch(err){
    console.error('PDF export error:',err);
    overlay.classList.remove('show');
    document.body.classList.remove('present-mode');
    btn.textContent='📄 导出PDF';
    alert('PDF生成失败，请检查网络连接后重试。\n错误：'+err.message);
    goTo(cur);
  }
}

// ══════════════════════════════════════════════════
// GLOBAL IMAGE REPLACEMENT — click any image in edit mode to replace
// Right-click to restore original
// ══════════════════════════════════════════════════
function initEditableImages(){
  let globalImgInput=document.getElementById('_globalImgInput');
  if(!globalImgInput){
    globalImgInput=document.createElement('input');
    globalImgInput.type='file';globalImgInput.accept='image/*';
    globalImgInput.id='_globalImgInput';globalImgInput.style.display='none';
    document.body.appendChild(globalImgInput);
  }
  let targetImg=null;

  document.querySelectorAll('.scaler img').forEach(img=>{
    img.addEventListener('click',e=>{
      if(!editMode)return;
      e.stopPropagation();
      targetImg=img;
      globalImgInput.value='';
      globalImgInput.click();
    });
    img.addEventListener('contextmenu',e=>{
      if(!editMode)return;
      e.preventDefault();
      if(img.dataset.originalSrc&&img.dataset.replaced==='true'){
        if(confirm('恢复原始图片？')){
          gsap.to(img,{opacity:0,duration:.2,onComplete:()=>{
            img.src=img.dataset.originalSrc;
            img.dataset.replaced='false';
            gsap.to(img,{opacity:1,duration:.3});
          }});
        }
      }
    });
  });

  globalImgInput.addEventListener('change',e=>{
    const file=e.target.files[0];
    if(!file||!targetImg)return;
    const reader=new FileReader();
    reader.onload=ev=>{
      if(!targetImg.dataset.originalSrc)
        targetImg.dataset.originalSrc=targetImg.src;
      gsap.to(targetImg,{opacity:0,scale:.95,duration:.25,ease:'power2.in',
        onComplete:()=>{
          targetImg.src=ev.target.result;
          targetImg.dataset.replaced='true';
          gsap.fromTo(targetImg,{opacity:0,scale:.95},
            {opacity:1,scale:1,duration:.35,ease:'power3.out'});
        }});
    };
    reader.readAsDataURL(file);
  });
}

// ══════════════════════════════════════════════════
// EDITABLE DATA — counters, progress bars
// ══════════════════════════════════════════════════
function initEditableCounters(){
  document.querySelectorAll('.counter').forEach(el=>{
    el.classList.add('data-editable');
    el.style.position='relative';
    el.addEventListener('click',()=>{
      if(!editMode)return;
      const newVal=prompt('输入新数值：',el.dataset.target||el.textContent);
      if(newVal===null)return;
      const val=parseFloat(newVal.replace(/[^0-9.]/g,''));
      if(isNaN(val))return;
      el.dataset.target=val;
      anime.remove(el);
      anime({targets:el,innerHTML:[0,val],round:1,duration:1800,easing:'easeOutExpo'});
    });
  });
}

function initEditableProgressBars(){
  document.querySelectorAll('.progress-track').forEach(track=>{
    const fill=track.querySelector('.progress-fill');
    const label=track.parentElement?.querySelector('.progress-label');
    if(!fill)return;
    track.addEventListener('click',()=>{
      if(!editMode)return;
      const newPct=prompt('输入百分比（0-100）：',fill.dataset.value||'50');
      if(newPct===null)return;
      const val=Math.min(100,Math.max(0,parseFloat(newPct)));
      if(isNaN(val))return;
      fill.dataset.value=val;
      gsap.to(fill,{width:val+'%',duration:.9,ease:'power3.out'});
      if(label)anime({targets:{v:parseFloat(label.textContent)||0},v:val,round:1,
        duration:900,easing:'easeOutExpo',
        update:a=>label.textContent=Math.round(a.animations[0].currentValue)+'%'});
    });
  });
}

// ══════════════════════════════════════════════════
// IMG ZONE UPLOAD (dedicated upload zones)
// ══════════════════════════════════════════════════
document.querySelectorAll('.img-zone').forEach(zone=>{
  let inp=zone.querySelector('input[type=file]');
  if(!inp){inp=document.createElement('input');inp.type='file';
    inp.accept='image/*';zone.appendChild(inp);}
  let del=zone.querySelector('.img-delete');
  if(!del){del=document.createElement('button');del.className='img-delete';
    del.innerHTML='✕';zone.appendChild(del);}
  inp.addEventListener('change',e=>{
    const file=e.target.files[0];if(!file)return;
    const r=new FileReader();
    r.onload=ev=>{
      let img=zone.querySelector('img');
      if(!img){img=document.createElement('img');zone.insertBefore(img,zone.firstChild);}
      img.src=ev.target.result;
    };
    r.readAsDataURL(file);
  });
  del.addEventListener('click',e=>{
    e.stopPropagation();
    const img=zone.querySelector('img');
    const def=zone.dataset.defaultSrc;
    if(img&&def)img.src=def;else if(img)img.remove();
    inp.value='';
  });
});

// ══════════════════════════════════════════════════
// IP CHARACTER UPLOAD
// ══════════════════════════════════════════════════
(function(){
  const char=document.getElementById('ipCharacter');
  if(!char)return;
  const inp=document.createElement('input');
  inp.type='file';inp.accept='image/*';inp.style.display='none';
  document.body.appendChild(inp);
  char.addEventListener('click',()=>inp.click());
  inp.addEventListener('change',e=>{
    const file=e.target.files[0];if(!file)return;
    const r=new FileReader();
    r.onload=ev=>{
      char.style.display='block';
      let img=char.querySelector('img');
      if(!img){img=document.createElement('img');
        img.style.cssText='width:100%;height:100%;object-fit:contain;';
        char.appendChild(img);}
      img.src=ev.target.result;
      gsap.to(char,{y:-12,duration:2.5,yoyo:true,repeat:-1,ease:'sine.inOut'});
    };
    r.readAsDataURL(file);
  });
})();

// ══════════════════════════════════════════════════
// AMBIENT
// ══════════════════════════════════════════════════
function buildAmbient(){
  const layer=document.getElementById('ambientLayer');
  if(total>40){buildAmbientCSS(layer);return;}
  // PASTE AMBIENT FUNCTION FROM THEME FILE HERE — must use GSAP
  buildAmbientDefault(layer);
}
function buildAmbientDefault(layer){
  [{col:'var(--color-primary)',size:400,pos:'top:-80px;left:-80px'},
   {col:'var(--color-accent)',size:350,pos:'bottom:-70px;right:-70px'}
  ].forEach((o,i)=>{
    const orb=document.createElement('div');
    orb.style.cssText=`position:absolute;border-radius:50%;width:${o.size}px;height:${o.size}px;background:${o.col};filter:blur(100px);opacity:.13;pointer-events:none;${o.pos}`;
    layer.appendChild(orb);
    gsap.to(orb,{x:'random(-80,80)',y:'random(-80,80)',
      duration:'random(10,16)',repeat:-1,yoyo:true,ease:'sine.inOut',delay:i*3});
  });
  const grid=document.createElement('div');
  grid.style.cssText=`position:absolute;inset:0;opacity:.04;pointer-events:none;
    background-image:linear-gradient(rgba(255,255,255,.5) 1px,transparent 1px),
    linear-gradient(90deg,rgba(255,255,255,.5) 1px,transparent 1px);
    background-size:80px 80px;`;
  layer.appendChild(grid);
  gsap.to(grid,{opacity:.08,duration:4,yoyo:true,repeat:-1,ease:'sine.inOut'});
}
function buildAmbientCSS(layer){
  layer.innerHTML=`
    <div style="position:absolute;width:500px;height:500px;border-radius:50%;background:var(--color-primary);filter:blur(120px);opacity:.12;top:-80px;left:-80px;animation:_af1 14s ease-in-out infinite alternate;pointer-events:none;"></div>
    <div style="position:absolute;width:400px;height:400px;border-radius:50%;background:var(--color-accent);filter:blur(100px);opacity:.10;bottom:-60px;right:-60px;animation:_af2 18s ease-in-out infinite alternate;pointer-events:none;"></div>
    <style>@keyframes _af1{from{transform:translate(0,0)}to{transform:translate(100px,70px)}}@keyframes _af2{from{transform:translate(0,0)}to{transform:translate(-80px,-60px)}}</style>`;
}

// ══════════════════════════════════════════════════
// EMPTY SLIDE GUARD
// ══════════════════════════════════════════════════
function validateSlides(){
  document.querySelectorAll('.slide').forEach((s,i)=>{
    if(s.innerText.trim().length<5)
      console.warn(`⚠️ Slide ${i+1} appears empty — check content`);
  });
}

// ══════════════════════════════════════════════════
// INIT
// ══════════════════════════════════════════════════
buildAmbient();
updateUI();
validateSlides();
initEditableImages();
initEditableCounters();
initEditableProgressBars();
document.fonts.ready.then(()=>setTimeout(()=>triggerAnim(slides[0]),100));
</script>
</body>
</html>
```

---

## MODULE 2 — LONGFORM STRATEGY (25+ slides)

```
Step 1: Output chapter plan, wait for confirmation
Step 2: Generate slides 1-12 as full HTML
Step 3: Ask "继续生成第三章？"
Step 4: Output ONLY <section> blocks for next chapters
Step 5: User pastes before <!-- SLIDES END --> comment
Step 6: Repeat until complete
```

---

## MODULE 3 — ADDING NEW THEMES

```
User sends PDF/screenshots → Claude analyzes → creates themes/theme-[name].md
Include: CSS variables, decorative elements, Layout Identity, ambient function, fonts
Add to ppt-brief.md theme mapping
No other files need changes.
```
