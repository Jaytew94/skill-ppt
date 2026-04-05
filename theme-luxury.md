# THEME: luxury
**Pure black + gold + ultra-light Cormorant Garamond**

## CSS VARIABLES
```css
:root {
  --bg-primary:#0a0a0a;--bg-secondary:#111111;
  --color-primary:#c9a84c;--color-accent:#e8d5a3;--color-accent2:#a0a0a0;
  --text-primary:#fafafa;--text-secondary:#a0a0a0;
  --surface:rgba(255,255,255,0.04);--border:rgba(201,168,76,0.25);
  --grad-slide:linear-gradient(135deg,#0a0a0a 0%,#111111 100%);
  --font-display:'Cormorant Garamond',serif;--font-body:'Jost',sans-serif;
  --google-fonts:'Cormorant+Garamond:wght@600;700&family=Jost:ital,wght@0,300;0,400;1,300';
}
```
## SIGNATURE: Gold hairlines + ornamental corners + extreme restraint
## AMBIENT: Slow gold particle drift (opacity 0.06) — very subtle
```javascript
function buildAmbientLuxury(layer){
  for(let i=0;i<20;i++){
    const p=document.createElement('div');
    p.style.cssText=`position:absolute;width:2px;height:2px;border-radius:50%;background:#c9a84c;left:${Math.random()*1920}px;top:${Math.random()*1080}px;opacity:0;pointer-events:none;`;
    layer.appendChild(p);
    gsap.to(p,{y:-80,opacity:0.15,duration:6+Math.random()*6,delay:Math.random()*10,repeat:-1,yoyo:true,ease:'sine.inOut'});
  }
}
```
## LAYOUT IDENTITY
```
FORBIDDEN: bold heavy fonts, bright colors, playful elements, crowded layouts, glows
MANDATORY: Cormorant Garamond 600 weight ONLY (never 900 — restraint is everything),
  gold (#c9a84c) for dividers/accents ONLY (never as background),
  extreme whitespace, thin 1px gold borders, slow fade animations only
SLIDE IDENTITY:
  Cover: elegant serif title + thin gold rule + minimal image
  Content: sparse text, gold hairline separators, nothing extra
  Chapter: single refined word, very slow opacity fade in only
  Data: elegant large numbers + gold label lines, nothing else
FONT: Cormorant Garamond 600 (display — 600 NOT 900), Jost 300/400 (body)
```
