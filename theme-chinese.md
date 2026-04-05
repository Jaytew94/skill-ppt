# THEME: chinese
**Deep red-brown + vermillion + gold + ink wash**

## CSS VARIABLES
```css
:root {
  --bg-primary:#1a0a00;--bg-secondary:#2d1200;
  --color-primary:#e8472a;--color-accent:#f5c842;--color-accent2:#c4955a;
  --text-primary:#fdf6e3;--text-secondary:#d4b896;
  --surface:rgba(255,255,255,0.06);--border:rgba(232,71,42,0.3);
  --grad-slide:linear-gradient(135deg,#1a0a00 0%,#2d1800 100%);
  --font-display:'ZCOOL XiaoWei',serif;--font-body:'Noto Sans SC',sans-serif;
  --google-fonts:'ZCOOL+XiaoWei&family=Noto+Sans+SC:wght@400;700';
}
```
## SIGNATURE ELEMENTS

### Circular Seal Motif
```html
<div class="animate" style="position:absolute;width:180px;height:180px;border-radius:50%;
  border:2px solid var(--color-primary);opacity:0.3;
  display:flex;align-items:center;justify-content:center;
  right:120px;bottom:80px;">
  <div style="width:140px;height:140px;border-radius:50%;
    border:1px solid var(--color-accent);opacity:0.6;
    display:flex;align-items:center;justify-content:center;
    font-family:var(--font-display);font-size:24px;color:var(--color-primary);">
    [BRAND]
  </div>
</div>
```

### Brush Stroke SVG Divider
```html
<svg viewBox="0 0 300 20" style="width:300px;height:20px;margin:20px 0;">
  <path d="M0,10 C50,2 100,18 150,10 C200,2 250,18 300,10"
    stroke="#e8472a" stroke-width="2" fill="none" opacity="0.6"/>
</svg>
```

## AMBIENT
```javascript
function buildAmbientChinese(layer){
  // Ink wash spread
  [{cx:200,cy:800},{cx:1700,cy:200}].forEach(pos=>{
    const div=document.createElement('div');
    div.style.cssText=`position:absolute;width:200px;height:200px;border-radius:50%;background:radial-gradient(circle,rgba(232,71,42,0.15),transparent);left:${pos.cx-100}px;top:${pos.cy-100}px;pointer-events:none;`;
    layer.appendChild(div);
    gsap.to(div,{scale:1.5,opacity:0,duration:4,repeat:-1,delay:Math.random()*3,ease:'power1.out'});
    gsap.set(div,{scale:0.5,opacity:1});
  });
  // Floating Chinese characters
  ['福','运','金','盛'].forEach((char,i)=>{
    const el=document.createElement('div');
    el.textContent=char;
    el.style.cssText=`position:absolute;font-family:var(--font-display);font-size:60px;color:rgba(232,71,42,0.06);left:${200+i*450}px;top:${100+i*150}px;pointer-events:none;`;
    layer.appendChild(el);
    gsap.to(el,{y:-20,opacity:0.12,duration:5+i,yoyo:true,repeat:-1,ease:'sine.inOut',delay:i*1.5});
  });
}
```

## LAYOUT IDENTITY
```
FORBIDDEN: western geometric shapes, neon colors, rounded pill buttons, octagon shapes
MANDATORY: circular seal/stamp motif on at least 1 slide, brush stroke SVG divider,
  ZCOOL XiaoWei for ALL headlines (letter-spacing:0 ALWAYS — Chinese fonts only),
  vermillion+gold color combination, ink wash bg elements
SLIDE IDENTITY:
  Cover: large Chinese characters + circular seal decoration + ink wash
  Content: vertical-inspired layout with brush stroke accent
  Chapter: single Chinese character large + ink wash circle bg
  Data: red numbers + gold label, circular frame around key stat
FONT: ZCOOL XiaoWei (display), Noto Sans SC 400/700 (body)
CRITICAL: letter-spacing:0 on ALL text — never add letter-spacing to Chinese
```
