# THEME: cyberpunk
**Near-black purple + neon pink + cyan + glitch effects**

---

## CSS VARIABLES
```css
:root {
  --bg-primary:    #0d0015;
  --bg-secondary:  #130020;
  --color-primary: #ff2d78;
  --color-accent:  #00f5ff;
  --color-accent2: #fcee09;
  --text-primary:  #ffffff;
  --text-secondary:#ff2d78;
  --surface:       rgba(255,255,255,0.05);
  --border:        rgba(255,45,120,0.3);
  --grad-slide: linear-gradient(135deg,#0d0015 0%,#130020 50%,#000d1a 100%);
  --font-display: 'Syncopate',sans-serif;
  --font-body:    'Outfit',sans-serif;
  --google-fonts: 'Syncopate:wght@400;700&family=Outfit:wght@300;400;700';
}
```

---

## SIGNATURE ELEMENTS

### 1. RGB GLITCH TITLE (mandatory on hero headlines)
```html
<h1 class="hero-title animate"
  style="font-family:var(--font-display);font-size:130px;font-weight:700;
  text-shadow:3px 0 #ff2d78,-3px 0 #00f5ff;
  animation:glitch 4s infinite steps(1);">
  [HEADLINE]
</h1>
<style>
@keyframes glitch {
  0%,95%,100%{text-shadow:3px 0 #ff2d78,-3px 0 #00f5ff;}
  96%{text-shadow:-4px 0 #ff2d78,4px 0 #00f5ff;transform:translateX(-3px);}
  97%{text-shadow:4px 0 #00f5ff,-4px 0 #ff2d78;transform:translateX(3px);}
  98%{text-shadow:none;transform:translateX(0);}
}
</style>
```

### 2. SCANLINE OVERLAY (every slide)
```html
<div style="position:absolute;inset:0;background:repeating-linear-gradient(
  0deg,transparent,transparent 3px,rgba(255,45,120,0.015) 3px,
  rgba(255,45,120,0.015) 4px);pointer-events:none;z-index:3;"></div>
```

### 3. NEON BORDER CARD
```html
<div style="border:1px solid var(--color-primary);padding:28px 32px;
  box-shadow:0 0 20px rgba(255,45,120,0.2),inset 0 0 20px rgba(255,45,120,0.05);
  position:relative;">
  [CONTENT]
</div>
```

### 4. NEON CTA BUTTON
```html
<button style="background:var(--color-primary);color:white;font-weight:700;
  padding:14px 36px;border:none;font-size:20px;cursor:pointer;
  box-shadow:0 0 30px rgba(255,45,120,0.5);letter-spacing:2px;
  font-family:var(--font-display);">[CTA]</button>
```

### 5. GLITCH ANIMATION (chapter breaks)
```css
@keyframes glitch-enter {
  0%,100%{transform:translateX(0);opacity:1;}
  20%{transform:translateX(-4px);color:#ff2d78;}
  40%{transform:translateX(4px);color:#00f5ff;}
  60%{transform:translateX(-2px);}
  80%{transform:translateX(2px);}
}
```

---

## AMBIENT ANIMATION
```javascript
function buildAmbientCyberpunk(layer) {
  // Equalizer bars
  const eq=document.createElement('div');
  eq.style.cssText='position:absolute;bottom:0;left:0;right:0;height:120px;display:flex;align-items:flex-end;gap:4px;padding:0 40px;opacity:0.15;pointer-events:none;';
  for(let i=0;i<60;i++){
    const bar=document.createElement('div');
    const h=20+Math.random()*80;
    bar.style.cssText=`flex:1;height:${h}px;background:${Math.random()>0.5?'#ff2d78':'#00f5ff'};border-radius:2px 2px 0 0;`;
    eq.appendChild(bar);
    gsap.to(bar,{height:`random(10,100)`,duration:`random(0.3,0.8)`,repeat:-1,yoyo:true,ease:'power2.inOut',delay:Math.random()*0.5});
  }
  layer.appendChild(eq);
  // Neon orbs
  [{col:'#ff2d78',x:100,y:100},{col:'#00f5ff',x:1700,y:800}].forEach(orb=>{
    const el=document.createElement('div');
    el.style.cssText=`position:absolute;width:300px;height:300px;border-radius:50%;background:${orb.col};filter:blur(100px);opacity:0.12;left:${orb.x}px;top:${orb.y}px;pointer-events:none;`;
    layer.appendChild(el);
    gsap.to(el,{x:'random(-60,60)',y:'random(-60,60)',duration:'random(8,14)',repeat:-1,yoyo:true,ease:'sine.inOut'});
  });
  // Scanlines (global)
  const scan=document.createElement('div');
  scan.style.cssText='position:absolute;inset:0;background:repeating-linear-gradient(0deg,transparent,transparent 3px,rgba(255,45,120,0.012) 3px,rgba(255,45,120,0.012) 4px);pointer-events:none;';
  layer.appendChild(scan);
}
```

---

## LAYOUT IDENTITY

```
FORBIDDEN:
  ❌ Octagon/hexagon shapes (HUD only)
  ❌ L-bracket corners (HUD only)
  ❌ Rounded soft cards (>8px radius)
  ❌ Clean minimal layouts
  ❌ Single accent color only

MANDATORY every slide:
  ✅ Scanline overlay (repeating-linear-gradient) on every slide bg
  ✅ RGB glitch text-shadow on hero/chapter headlines
  ✅ Neon glow box-shadow on at least one element per slide
  ✅ Sharp 90-degree corners on all containers
  ✅ Both pink (#ff2d78) AND cyan (#00f5ff) used together
  ✅ Equalizer bars in ambient layer

SLIDE IDENTITY:
  Cover:    Glitch hero title + scanlines + neon grid bg + pink/cyan contrast
  Content:  Sharp neon-border cards + both accent colors
  Chapter:  Massive glitch word + color aberration animation + flash
  Data:     Neon glowing numbers + horizontal bar graph
  Timeline: Neon line + blinking node dots
  Closing:  Full glitch effect + neon CTA button

FONT: Syncopate 700 for display (uppercase always), Outfit 400 body.
```
