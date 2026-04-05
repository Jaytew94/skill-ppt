# THEME: nature
**Forest dark green + lime accent + organic forms**

## CSS VARIABLES
```css
:root {
  --bg-primary:#1a2e1a;--bg-secondary:#243324;
  --color-primary:#4ade80;--color-accent:#d4a574;--color-accent2:#86efac;
  --text-primary:#f0fdf4;--text-secondary:#86efac;
  --surface:rgba(255,255,255,0.06);--border:rgba(74,222,128,0.25);
  --grad-slide:linear-gradient(135deg,#1a2e1a 0%,#243324 50%,#1a3320 100%);
  --font-display:'Merriweather',serif;--font-body:'Source Sans 3',sans-serif;
  --google-fonts:'Merriweather:wght@700;900&family=Source+Sans+3:wght@400;600';
}
```
## SIGNATURE: Organic curved image panels (border-radius:40%) + leaf SVG motif dividers
## AMBIENT
```javascript
function buildAmbientNature(layer){
  // Falling leaves
  for(let i=0;i<12;i++){
    const leaf=document.createElement('div');
    leaf.innerHTML='🍃';
    leaf.style.cssText=`position:absolute;font-size:${20+Math.random()*16}px;left:${Math.random()*1800}px;top:-40px;opacity:0.4;pointer-events:none;`;
    layer.appendChild(leaf);
    gsap.to(leaf,{y:1150,x:`random(-120,120)`,rotation:720,duration:8+Math.random()*6,delay:Math.random()*8,repeat:-1,ease:'none'});
  }
  // Water ripple SVG
  const svg=document.createElementNS('http://www.w3.org/2000/svg','svg');
  svg.style.cssText='position:absolute;bottom:60px;right:120px;opacity:0.2;pointer-events:none;';
  svg.setAttribute('width','200');svg.setAttribute('height','100');
  for(let i=0;i<3;i++){
    const ellipse=document.createElementNS('http://www.w3.org/2000/svg','ellipse');
    ellipse.setAttribute('cx','100');ellipse.setAttribute('cy','50');
    ellipse.setAttribute('rx','30');ellipse.setAttribute('ry','15');
    ellipse.setAttribute('fill','none');ellipse.setAttribute('stroke','#4ade80');ellipse.setAttribute('stroke-width','1.5');
    svg.appendChild(ellipse);
    gsap.to(ellipse,{attr:{rx:30+i*30,ry:15+i*20},opacity:0,duration:2.5,delay:i*0.8,repeat:-1,ease:'power1.out'});
  }
  layer.appendChild(svg);
}
```
## LAYOUT IDENTITY
```
FORBIDDEN: sharp diagonal cuts, neon colors, city imagery, hard geometric shapes
MANDATORY: organic curves on image panels (border-radius:40%+), falling leaf ambient,
  earth tone accent (#d4a574), vine/leaf SVG motifs, nature imagery only
SLIDE IDENTITY:
  Cover: organic shaped image + botanical leaf accent
  Content: organic card shapes with leaf dividers
  Chapter: nature word + vine SVG growing beneath it
  Data: green numbers + water ripple animation
FONT: Merriweather 700 (display), Source Sans 3 400/600 (body)
```
