# THEME: business
**Deep navy + gold accent + authoritative serif**

## CSS VARIABLES
```css
:root {
  --bg-primary:    #0f172a;
  --bg-secondary:  #1e293b;
  --color-primary: #3b82f6;
  --color-accent:  #f59e0b;
  --color-accent2: #e2e8f0;
  --text-primary:  #f8fafc;
  --text-secondary:#94a3b8;
  --surface:       rgba(255,255,255,0.06);
  --border:        rgba(59,130,246,0.25);
  --grad-slide: linear-gradient(160deg,#0f172a 0%,#1e3a5f 100%);
  --font-display: 'Playfair Display',serif;
  --font-body:    'Inter',sans-serif;
  --google-fonts: 'Playfair+Display:wght@700;900&family=Inter:wght@400;500;600';
}
```

## SIGNATURE ELEMENTS

### Gold Hairline Divider (use between sections)
```html
<div style="width:100%;height:1px;background:linear-gradient(90deg,transparent,#f59e0b,transparent);margin:28px 0;opacity:0.6;"></div>
```

### Authority Frame (key content box)
```html
<div style="border-left:3px solid #f59e0b;padding:20px 28px;
  background:rgba(245,158,11,0.05);">
  [CONTENT]
</div>
```

### Stat with Gold Label
```html
<div class="animate">
  <div style="font-size:14px;color:#f59e0b;letter-spacing:3px;
    text-transform:uppercase;margin-bottom:8px;">[LABEL]</div>
  <div class="counter data-editable" data-target="[N]"
    style="font-family:var(--font-display);font-size:100px;
    color:var(--color-primary);font-weight:900;line-height:0.9;">0</div>
</div>
```

## AMBIENT ANIMATION
```javascript
function buildAmbientBusiness(layer) {
  // Rising graph line SVG
  const svg=document.createElementNS('http://www.w3.org/2000/svg','svg');
  svg.setAttribute('viewBox','0 0 1920 400');
  svg.style.cssText='position:absolute;bottom:0;left:0;width:100%;height:auto;opacity:0.08;pointer-events:none;';
  const path=document.createElementNS('http://www.w3.org/2000/svg','path');
  path.setAttribute('d','M0,350 C400,300 600,200 900,180 C1200,160 1500,100 1920,80');
  path.setAttribute('stroke','#3b82f6');path.setAttribute('stroke-width','2');
  path.setAttribute('fill','none');
  svg.appendChild(path);
  layer.appendChild(svg);
  const len=path.getTotalLength();
  path.style.strokeDasharray=len;path.style.strokeDashoffset=len;
  gsap.to(path,{strokeDashoffset:0,duration:3,ease:'power2.out',repeat:-1,delay:1});
  // Floating stats
  ['+12%','$2.4M','↑87%'].forEach((t,i)=>{
    const el=document.createElement('div');
    el.textContent=t;
    el.style.cssText=`position:absolute;right:${120+i*200}px;top:${200+i*80}px;font-family:var(--font-display);font-size:22px;color:rgba(245,158,11,0.3);pointer-events:none;`;
    layer.appendChild(el);
    gsap.to(el,{y:-20,opacity:0.5,duration:4+i,yoyo:true,repeat:-1,ease:'sine.inOut',delay:i*1.5});
  });
}
```

## LAYOUT IDENTITY
```
FORBIDDEN: neon colors, glitch, rounded pill buttons, informal layouts, bright colors
MANDATORY: gold (#f59e0b) hairline rules between sections (never as bg),
  Playfair Display serif headlines, thin 1px borders, city/graph ambient,
  authoritative left-aligned layouts, gold accent on key numbers only
SLIDE IDENTITY:
  Cover: serif hero left + diagonal image + gold accent line
  Content: clean split + gold hairline separator + left-aligned text
  Data: gold-labeled stats + blue large numbers
  Chapter: elegant serif word + gold 80px underline
FONT: Playfair Display 700/900 (display), Inter 400/500 (body)
```
