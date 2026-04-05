# THEME: tech-hud
**Dark blue + electric blue + octagon frames + HUD L-brackets**
**Style: Military HUD, sci-fi interface, precision tech**

---

## CSS VARIABLES
```css
:root {
  --bg-primary:    #020b18;
  --bg-secondary:  #061428;
  --bg-card:       #0a1f3d;
  --color-primary: #1a6fdf;
  --color-bright:  #4d9fff;
  --color-accent:  #2952cc;
  --text-primary:  #ffffff;
  --text-secondary:#a8c4e0;
  --text-muted:    #4a7098;
  --surface:       rgba(10,31,61,0.8);
  --border:        rgba(26,111,223,0.3);
  --border-bright: rgba(77,159,255,0.6);
  --grad-slide: linear-gradient(135deg,#020b18 0%,#061428 40%,#020b18 100%);
  --font-display: 'Barlow',sans-serif;
  --font-body:    'Inter',sans-serif;
  --google-fonts: 'Barlow:wght@400;600;700;800;900&family=Inter:wght@400;500;600';
}
```

---

## SIGNATURE DECORATIVE ELEMENTS

### 1. OCTAGON (use on ALL image frames — never rectangles)
```css
.octagon {
  clip-path: polygon(15% 0%,85% 0%,100% 15%,100% 85%,85% 100%,15% 100%,0% 85%,0% 15%);
}
.octagon-sm {
  clip-path: polygon(20% 0%,80% 0%,100% 20%,100% 80%,80% 100%,20% 100%,0% 80%,0% 20%);
}
```

### 2. L-BRACKET CORNERS (use on every content container)
```html
<!-- Wrap any container with this -->
<div style="position:relative;padding:24px;">
  <!-- TL corner -->
  <div style="position:absolute;top:0;left:0;width:20px;height:20px;
    border-top:2px solid #4d9fff;border-left:2px solid #4d9fff;"></div>
  <!-- BR corner -->
  <div style="position:absolute;bottom:0;right:0;width:20px;height:20px;
    border-bottom:2px solid #4d9fff;border-right:2px solid #4d9fff;"></div>
  [CONTENT]
</div>
```

### 3. CHEVRON BUTTON/LABEL
```html
<div style="background:var(--color-primary);color:white;padding:10px 28px;
  clip-path:polygon(16px 0%,100% 0%,calc(100% - 16px) 100%,0% 100%);
  font-size:18px;font-weight:600;letter-spacing:1px;display:inline-block;">
  [LABEL]
</div>
```

### 4. CIRCUIT NODE
```html
<div style="width:7px;height:7px;background:white;position:absolute;"></div>
```

### 5. DIAGONAL ACCENT BLOCK
```html
<div style="position:absolute;top:0;right:0;width:350px;height:350px;
  background:var(--color-primary);opacity:0.18;pointer-events:none;
  clip-path:polygon(100% 0%,0% 0%,100% 100%);"></div>
```

### 6. SCAN LINE OVERLAY
```html
<div style="position:absolute;inset:0;background:repeating-linear-gradient(
  0deg,transparent,transparent 3px,rgba(26,111,223,0.015) 3px,rgba(26,111,223,0.015) 4px);
  pointer-events:none;z-index:2;"></div>
```

---

## AMBIENT ANIMATION
```javascript
function buildAmbientTechHUD(layer) {
  // Circuit grid
  const canvas=document.createElement('canvas');
  canvas.width=1920;canvas.height=1080;
  canvas.style.cssText='position:absolute;inset:0;opacity:0.15;pointer-events:none;';
  layer.appendChild(canvas);
  const ctx=canvas.getContext('2d');
  ctx.strokeStyle='#1a6fdf';ctx.lineWidth=0.5;ctx.globalAlpha=0.3;
  for(let x=0;x<=1920;x+=80){ctx.beginPath();ctx.moveTo(x,0);ctx.lineTo(x,1080);ctx.stroke();}
  for(let y=0;y<=1080;y+=80){ctx.beginPath();ctx.moveTo(0,y);ctx.lineTo(1920,y);ctx.stroke();}
  ctx.globalAlpha=0.5;ctx.fillStyle='#1a6fdf';
  for(let x=0;x<=1920;x+=80)for(let y=0;y<=1080;y+=80){
    ctx.beginPath();ctx.arc(x,y,1.5,0,Math.PI*2);ctx.fill();
  }
  // Scan line
  const scan=document.createElement('div');
  scan.style.cssText='position:absolute;left:0;right:0;height:2px;background:linear-gradient(90deg,transparent,#1a6fdf,transparent);opacity:0.4;pointer-events:none;top:0;';
  layer.appendChild(scan);
  gsap.to(scan,{top:'1080px',duration:6,ease:'none',repeat:-1,delay:1});
  // HUD text fragments
  ['SYS:OK','NET:ON','[SYNC]','v2.4.1','■■□□'].forEach((t,i)=>{
    const el=document.createElement('div');
    el.textContent=t;
    el.style.cssText=`position:absolute;left:${120+i*380}px;top:${80+i*160}px;font-family:monospace;font-size:13px;color:#1a6fdf;opacity:0.2;pointer-events:none;letter-spacing:2px;`;
    layer.appendChild(el);
    gsap.to(el,{opacity:0.4,duration:2+Math.random()*2,yoyo:true,repeat:-1,delay:Math.random()*4,ease:'steps(4)'});
  });
  // Corner brackets
  [[30,30,'2px 0 0 2px'],[30,'calc(100% - 30px)','0 2px 0 0'],[['calc(100% - 30px)'],30,'0 0 0 2px'],[['calc(100% - 30px)'],['calc(100% - 30px)'],'0 0 2px 0']].forEach(([top,left,bw])=>{
    const c=document.createElement('div');
    c.style.cssText=`position:absolute;top:${top}px;left:${left}px;width:40px;height:40px;border:${bw} solid #4d9fff;opacity:0.35;pointer-events:none;`;
    layer.appendChild(c);
    gsap.to(c,{opacity:0.6,duration:2,yoyo:true,repeat:-1,delay:Math.random()*2,ease:'sine.inOut'});
  });
}
```

---

## LAYOUT IDENTITY

```
FORBIDDEN in this theme:
  ❌ Rounded cards (border-radius > 4px on cards/containers)
  ❌ Soft glowing orbs as decoration
  ❌ Pill buttons
  ❌ Rectangular image frames (always use octagon clip-path)
  ❌ Gradient mesh backgrounds

MANDATORY every slide:
  ✅ L-bracket corners on at least one container per slide
  ✅ Octagon clip-path on ALL image frames
  ✅ Circuit node squares at content zone corners
  ✅ Chevron/arrow-cut labels for eyebrow text
  ✅ Sharp corners everywhere (border-radius: 0-4px max)
  ✅ Scan line or circuit grid in ambient layer

SLIDE IDENTITY:
  Cover:    Massive hero title left + octagon image right + diagonal accent block
  Content:  HUD card containers + L-brackets + circuit nodes at corners
  Chapter:  Single word + animated underline + ghost number bg
  Data:     HUD brackets framing each metric + glowing number
  Timeline: Octagon numbered nodes + animated circuit line
  Closing:  Full-width diagonal accent + hero CTA + circuit decoration

FONT:
  Headlines: Barlow 900, letter-spacing:-1px
  Eyebrows:  UPPERCASE, letter-spacing:4px, in chevron label
  Body:      Inter 400, letter-spacing:0
```
