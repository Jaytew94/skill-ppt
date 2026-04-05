# THEME: tech
**Deep space dark + neon cyan/purple + particle network**

## CSS VARIABLES
```css
:root {
  --bg-primary:    #0a0e1a;
  --bg-secondary:  #0f1629;
  --color-primary: #00d4ff;
  --color-accent:  #7c3aed;
  --color-accent2: #06ffa5;
  --text-primary:  #ffffff;
  --text-secondary:#94a3b8;
  --surface:       rgba(255,255,255,0.05);
  --border:        rgba(0,212,255,0.2);
  --grad-slide: linear-gradient(135deg,#0a0e1a 0%,#0f1629 50%,#0a0e1a 100%);
  --font-display: 'Orbitron',sans-serif;
  --font-body:    'DM Sans',sans-serif;
  --google-fonts: 'Orbitron:wght@400;700&family=DM+Sans:wght@400;500;700';
}
```

## SIGNATURE ELEMENTS

### Gradient Text on Hero
```html
<h1 class="hero-title animate" style="font-family:var(--font-display);
  font-size:130px;font-weight:700;line-height:0.88;
  background:linear-gradient(135deg,#00d4ff,#7c3aed);
  -webkit-background-clip:text;-webkit-text-fill-color:transparent;
  background-clip:text;">
  [HEADLINE]
</h1>
```

### Glowing Border Card
```html
<div style="border:1px solid rgba(0,212,255,0.2);padding:28px 32px;
  background:rgba(255,255,255,0.04);
  box-shadow:0 0 30px rgba(0,212,255,0.1);">
  [CONTENT]
</div>
```

## AMBIENT ANIMATION
```javascript
function buildAmbientTech(layer) {
  const canvas=document.createElement('canvas');
  canvas.width=1920;canvas.height=1080;
  canvas.style.cssText='position:absolute;inset:0;opacity:0.6;pointer-events:none;';
  layer.appendChild(canvas);
  const ctx=canvas.getContext('2d');
  const dots=Array.from({length:55},()=>({
    x:Math.random()*1920,y:Math.random()*1080,
    vx:(Math.random()-0.5)*0.4,vy:(Math.random()-0.5)*0.4
  }));
  function draw(){
    ctx.clearRect(0,0,1920,1080);
    dots.forEach(d=>{
      d.x+=d.vx;d.y+=d.vy;
      if(d.x<0||d.x>1920)d.vx*=-1;
      if(d.y<0||d.y>1080)d.vy*=-1;
      ctx.beginPath();ctx.arc(d.x,d.y,2,0,Math.PI*2);
      ctx.fillStyle='rgba(0,212,255,0.6)';ctx.fill();
    });
    dots.forEach((a,i)=>dots.slice(i+1).forEach(b=>{
      const dist=Math.hypot(a.x-b.x,a.y-b.y);
      if(dist<180){
        ctx.beginPath();ctx.moveTo(a.x,a.y);ctx.lineTo(b.x,b.y);
        ctx.strokeStyle=`rgba(0,212,255,${(1-dist/180)*0.2})`;
        ctx.lineWidth=0.8;ctx.stroke();
      }
    }));
    requestAnimationFrame(draw);
  }
  draw();
  // Code fragments
  ['AI','ML','</>', 'λ','∑','API'].forEach((t,i)=>{
    const el=document.createElement('div');
    el.textContent=t;
    el.style.cssText=`position:absolute;left:${150+i*300}px;top:${100+Math.random()*700}px;font-family:monospace;font-size:16px;color:rgba(0,212,255,0.2);pointer-events:none;`;
    layer.appendChild(el);
    gsap.to(el,{y:-30,opacity:0.4,duration:3+Math.random()*2,yoyo:true,repeat:-1,delay:Math.random()*3,ease:'sine.inOut'});
  });
}
```

## LAYOUT IDENTITY
```
FORBIDDEN: rounded cards>8px, warm colors, organic shapes, soft orbs only
MANDATORY: particle network canvas always active, hex/grid overlay (opacity 0.03),
  gradient text on hero title (-webkit-background-clip), cyan glow on borders
SLIDE IDENTITY:
  Cover: gradient text hero + particle bg + glowing border subtitle
  Content: dark surface cards + glowing cyan border + code fragments ambient
  Data: glowing numbers + gradient fill bars
  Chapter: single word letter-by-letter stagger + gradient color
FONT: Orbitron 700 (display), DM Sans 400 (body)
```
