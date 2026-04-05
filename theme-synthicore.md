# THEME: synthicore
**Deep navy + single cyan + large rounded cards + floating products**
**Style: Modern SaaS, clean tech, approachable AI**

---

## CSS VARIABLES
```css
:root {
  --bg-primary:    #080e1f;
  --bg-secondary:  #0d1935;
  --bg-card:       #0f2040;
  --color-primary: #00e5ff;
  --color-accent:  #00b8cc;
  --color-cta:     #00d4e8;
  --text-primary:  #ffffff;
  --text-secondary:#94b8d0;
  --text-muted:    #4a7090;
  --surface:       rgba(15,32,64,0.8);
  --border:        rgba(0,229,255,0.15);
  --border-card:   rgba(255,255,255,0.07);
  --radius-card:   20px;
  --radius-icon:   14px;
  --radius-btn:    50px;
  --grad-slide: linear-gradient(160deg,#080e1f 0%,#0d1935 40%,#0a1628 100%);
  --font-display: 'Plus Jakarta Sans',sans-serif;
  --font-body:    'Plus Jakarta Sans',sans-serif;
  --google-fonts: 'Plus+Jakarta+Sans:wght@300;400;500;600;700;800';
}
```

---

## SIGNATURE ELEMENTS

### 1. TWO-TONE HEADLINE (mandatory every slide)
```html
<h2 class="animate" style="font-family:var(--font-display);font-size:80px;
  line-height:1.05;letter-spacing:0;word-break:keep-all;">
  How It's Changing<br>
  <strong style="color:var(--color-primary);font-weight:800;">the World</strong>
</h2>
<!-- Rule: colored portion = 1-3 key words max (≤40% of headline) -->
```

### 2. ROUNDED GLASS CARD (main container)
```html
<div style="background:linear-gradient(135deg,#0f2040 0%,#0d1935 100%);
  border:1px solid rgba(255,255,255,0.07);border-radius:20px;padding:32px 36px;">
  [CONTENT]
</div>
```

### 3. CYAN ICON SQUARE
```html
<div style="width:64px;height:64px;background:var(--color-primary);
  border-radius:14px;display:flex;align-items:center;
  justify-content:center;flex-shrink:0;font-size:28px;">[EMOJI]</div>
```

### 4. PILL BUTTON
```html
<button style="background:var(--color-cta);color:#000;font-weight:700;
  border-radius:50px;padding:14px 36px;font-size:20px;border:none;cursor:pointer;">
  [CTA TEXT]
</button>
```

### 5. STACKED IMAGE + INFO CARD
```html
<div style="position:relative;width:100%;">
  <div style="border-radius:20px;overflow:hidden;height:260px;">
    <img src="https://picsum.photos/seed/[KW]/600/520"
         style="width:100%;height:100%;object-fit:cover;" alt="">
  </div>
  <div style="background:linear-gradient(135deg,#0f2040,#0d1935);
    border:1px solid rgba(255,255,255,0.07);border-radius:16px;
    padding:28px 32px;margin-top:-50px;margin-left:24px;position:relative;z-index:2;"
    class="animate">
    <h3 style="font-size:28px;margin-bottom:10px;">[TITLE]</h3>
    <p style="font-size:20px;color:var(--text-secondary);">[DESC]</p>
  </div>
</div>
```

### 6. GLOWING ORB (ambient element)
```html
<div style="position:absolute;width:400px;height:400px;border-radius:50%;
  background:var(--color-primary);filter:blur(140px);opacity:0.12;
  top:-100px;right:200px;pointer-events:none;"></div>
```

---

## AMBIENT ANIMATION
```javascript
function buildAmbientSynthicore(layer) {
  [{size:500,x:-100,y:-100,blur:160,op:0.10},
   {size:300,x:'calc(100% - 200px)',y:'calc(100% - 200px)',blur:100,op:0.13},
   {size:200,x:'55%',y:'15%',blur:80,op:0.12}
  ].forEach((orb,i)=>{
    const el=document.createElement('div');
    el.style.cssText=`position:absolute;width:${orb.size}px;height:${orb.size}px;
      border-radius:50%;background:var(--color-primary);filter:blur(${orb.blur}px);
      opacity:${orb.op};pointer-events:none;left:${orb.x}px;top:${orb.y}px;`;
    layer.appendChild(el);
    gsap.to(el,{x:'random(-80,80)',y:'random(-80,80)',
      duration:`random(12,20)`,repeat:-1,yoyo:true,ease:'sine.inOut',delay:i*3});
    gsap.to(el,{opacity:orb.op*1.6,duration:4+i*2,repeat:-1,yoyo:true,ease:'sine.inOut'});
  });
  const grid=document.createElement('div');
  grid.style.cssText=`position:absolute;inset:0;opacity:0.025;pointer-events:none;
    background-image:radial-gradient(circle,rgba(0,229,255,0.8) 1px,transparent 1px);
    background-size:60px 60px;`;
  layer.appendChild(grid);
}
```

---

## LAYOUT IDENTITY

```
FORBIDDEN:
  ❌ Sharp corners (border-radius < 12px on cards/images)
  ❌ L-bracket decorations (HUD theme only)
  ❌ Octagon shapes (HUD theme only)
  ❌ Glitch effects / scanlines
  ❌ Military/industrial aesthetic
  ❌ Multiple accent colors

MANDATORY every slide:
  ✅ border-radius 16-24px on ALL cards and image frames
  ✅ Two-tone headline (white + cyan bold keyword)
  ✅ Soft glowing orbs as ambient (no hard shapes)
  ✅ Cyan square icon boxes (64px, radius 14px)
  ✅ Glass cards (backdrop-filter:blur or gradient bg)
  ✅ Single accent color: cyan only

SLIDE IDENTITY:
  Cover:    Portrait image right (rounded) + hero title left + pill CTA
  Content:  Glass rounded cards + cyan icon squares + two-tone headline
  Chapter:  Centered word + soft glow behind + rounded underline
  Data:     Rounded metric cards + cyan counter numbers
  Team:     Portrait photos in rounded squares + cyan name label
  Pricing:  3 rounded tiers, center in solid cyan (inverted colors)
  Closing:  Left hero title + right rounded contact grid

UNIQUE LAYOUTS for this theme:
  L29 Stacked Card — always for feature highlights
  L25 Pricing Table — rounded, center inverted
  L26 Contact Grid — cyan icon squares + rounded card

FONT: Plus Jakarta Sans only. 800 bold headlines, 400 body.
```
