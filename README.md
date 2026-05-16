
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
<title>KRGBY + K2 + White String Art — Full Complete App</title>

<style>
:root{
  --bg:#0f172a;
  --text:#f8fafc;
  --muted:#cbd5e1;
  --accent:#2563eb;
  --accent2:#1d4ed8;
  --danger:#b91c1c;
  --ok:#16a34a;
}

*{
  box-sizing:border-box;
}

body{
  margin:0;
  background:radial-gradient(circle at top,#1e3a8a 0,#0f172a 45%,#020617 100%);
  color:var(--text);
  font-family:Arial,sans-serif;
  text-align:center;
  overscroll-behavior:none;
}

h1{
  font-size:22px;
  margin:12px 8px 4px;
}

.small{
  max-width:980px;
  margin:4px auto 10px;
  padding:0 10px;
  color:var(--muted);
  font-size:13px;
  line-height:1.35;
}

.controls{
  background:rgba(15,23,42,.96);
  border-top:1px solid rgba(255,255,255,.08);
  border-bottom:1px solid rgba(255,255,255,.08);
  padding:10px;
  box-shadow:0 8px 24px rgba(0,0,0,.25);
}

.row{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  align-items:flex-end;
  gap:8px;
  margin:8px auto;
}

.block{
  display:flex;
  flex-direction:column;
  align-items:center;
  gap:2px;
}

label{
  color:#a5b4fc;
  font-size:11px;
}

input,
select,
button{
  border-radius:8px;
  border:1px solid #334155;
  padding:7px;
  font-size:14px;
}

input[type="number"]{
  width:82px;
  text-align:center;
}

input[type="file"]{
  max-width:94vw;
  background:#020617;
  color:var(--text);
}

select{
  background:#f8fafc;
  color:#020617;
}

button{
  background:var(--accent);
  color:white;
  border:none;
  cursor:pointer;
  font-weight:bold;
}

button:hover{
  background:var(--accent2);
}

button.danger{
  background:var(--danger);
}

button.danger:hover{
  background:#991b1b;
}

button.ok{
  background:var(--ok);
}

button.ok:hover{
  background:#15803d;
}

#status{
  font-weight:bold;
  margin:8px auto;
  color:#facc15;
}

.stageWrap{
  width:96vw;
  max-width:960px;
  margin:14px auto;
}

.stage{
  width:100%;
  aspect-ratio:1/1;
  background:#fff;
  border-radius:50%;
  overflow:hidden;
  position:relative;
  box-shadow:0 16px 38px rgba(0,0,0,.35);
  touch-action:none;
}

.stage.squareStage,
.stage.rectStage{
  border-radius:0;
}

canvas{
  position:absolute;
  inset:0;
  width:100%;
  height:100%;
}

#targetCanvas{
  z-index:1;
  opacity:1;
}

#artCanvas{
  z-index:2;
  pointer-events:none;
}

#pinCanvas{
  z-index:3;
  pointer-events:none;
}

#output{
  width:94vw;
  max-width:900px;
  margin:12px auto 24px;
}

textarea{
  width:100%;
  height:210px;
  background:#020617;
  color:#e5e7eb;
  border:1px solid #334155;
  border-radius:10px;
  padding:10px;
  font-family:monospace;
  resize:vertical;
}

.note{
  font-size:12px;
  color:#cbd5e1;
  margin-top:6px;
}

.swatches{
  display:flex;
  flex-wrap:wrap;
  justify-content:center;
  gap:8px;
  margin:8px auto;
}

.swatch{
  display:flex;
  flex-direction:column;
  align-items:center;
  font-size:10px;
  color:#cbd5e1;
}

.chip{
  width:26px;
  height:20px;
  border:1px solid rgba(255,255,255,.8);
  border-radius:5px;
}
</style>
</head>

<body>

<h1>KRGBY + K2 + White String Art Generator</h1>

<div class="small">
  Full complete single-file app with image upload/crop, circle canvas, square canvas, manually sized rectangle canvas,
  KRGBY color passes, K2 detail pass, restrained white highlight pass, TXT/CSV/SVG exports, opacity control, and line-thickness control.
</div>

<div class="controls">

  <div class="row">
    <input type="file" id="fileInput" accept="image/*">
  </div>

  <div class="row">
    <div class="block">
      <input id="pinCount" type="number" min="12" max="1400" value="300">
      <label>Pins</label>
    </div>

    <div class="block">
      <input id="maxLines" type="number" min="1" value="4600">
      <label>Total Lines</label>
    </div>

    <div class="block">
      <input id="candidates" type="number" min="2" value="200">
      <label>Candidates</label>
    </div>

    <div class="block">
      <input id="res" type="number" min="80" max="900" value="380">
      <label>Solver Width</label>
    </div>

    <div class="block">
      <input id="subtractOpacity" type="number" min="0.005" max="1" step="0.005" value="0.075">
      <label>Subtract</label>
    </div>

    <div class="block">
      <input id="renderOpacity" type="number" min="0.01" max="1" step="0.01" value="0.18">
      <label>Line Opacity</label>
    </div>

    <div class="block">
      <input id="lineThickness" type="number" min="0.05" max="4" step="0.01" value="0.55">
      <label>Line Thickness</label>
    </div>

    <div class="block">
      <input id="whiteThickness" type="number" min="0.05" max="4" step="0.01" value="0.42">
      <label>White Thickness</label>
    </div>

    <div class="block">
      <input id="minSkip" type="number" min="0" value="8">
      <label>Min Skip</label>
    </div>
  </div>

  <div class="row">
    <div class="block">
      <select id="canvasMode">
        <option value="white">White canvas</option>
        <option value="black">Black canvas</option>
      </select>
      <label>Canvas Color</label>
    </div>

    <div class="block">
      <select id="canvasShape">
        <option value="circle">Circle</option>
        <option value="square">Square</option>
        <option value="rect">Rectangle</option>
      </select>
      <label>Canvas Shape</label>
    </div>

    <div class="block">
      <input id="canvasWide" type="number" min="200" max="1600" value="900">
      <label>Rect Width</label>
    </div>

    <div class="block">
      <input id="canvasHigh" type="number" min="200" max="1600" value="600">
      <label>Rect Height</label>
    </div>

    <div class="block">
      <select id="candidateMode">
        <option value="wide">Wide/chord-biased</option>
        <option value="random">Repo-style random</option>
        <option value="all">Scan all pins</option>
      </select>
      <label>Candidate Mode</label>
    </div>

    <div class="block">
      <select id="colorMode">
        <option value="krgby">KRGBY + K2 + White</option>
        <option value="krgbyNoWhite">KRGBY + K2, no White</option>
        <option value="mono">Mono only</option>
      </select>
      <label>Thread Set</label>
    </div>
  </div>

  <div class="row">
    <div class="block">
      <input id="kShare" type="number" min="0" max="1" step="0.01" value="0.24">
      <label>K Share</label>
    </div>

    <div class="block">
      <input id="rgbShare" type="number" min="0" max="1" step="0.01" value="0.46">
      <label>RGBY Share</label>
    </div>

    <div class="block">
      <input id="k2Share" type="number" min="0" max="1" step="0.01" value="0.18">
      <label>K2 Share</label>
    </div>

    <div class="block">
      <input id="whiteShare" type="number" min="0" max="1" step="0.01" value="0.12">
      <label>White Share</label>
    </div>
  </div>

  <div class="swatches">
    <div class="swatch"><div class="chip" style="background:#000"></div>K</div>
    <div class="swatch"><div class="chip" style="background:#e11d48"></div>R</div>
    <div class="swatch"><div class="chip" style="background:#16a34a"></div>G</div>
    <div class="swatch"><div class="chip" style="background:#2563eb"></div>B</div>
    <div class="swatch"><div class="chip" style="background:#facc15"></div>Y</div>
    <div class="swatch"><div class="chip" style="background:#111"></div>K2</div>
    <div class="swatch"><div class="chip" style="background:#fff"></div>WHITE</div>
  </div>

  <div class="row">
    <button id="generateBtn" class="ok">Generate</button>
    <button id="stopBtn" class="danger">Stop</button>
    <button id="resetBtn">Reset</button>
    <button id="copyBtn">Copy Instructions</button>
    <button id="txtBtn">Download TXT</button>
    <button id="csvBtn">Download CSV</button>
    <button id="svgBtn">Download SVG</button>
  </div>

  <div id="status">Lines generated: 0</div>

</div>

<div class="stageWrap">
  <div class="stage" id="stage">
    <canvas id="targetCanvas"></canvas>
    <canvas id="artCanvas"></canvas>
    <canvas id="pinCanvas"></canvas>
  </div>
  <div class="note">
    Upload an image. Drag to move it. Pinch or mouse wheel to zoom. Choose Circle, Square, or Rectangle before generating.
  </div>
</div>

<div id="output">
  <textarea id="instructions" readonly placeholder="Instructions will appear here..."></textarea>
</div>

<script>
let CW=720;
let CH=720;

const targetCanvas=document.getElementById('targetCanvas');
const artCanvas=document.getElementById('artCanvas');
const pinCanvas=document.getElementById('pinCanvas');

const tctx=targetCanvas.getContext('2d',{willReadFrequently:true});
const actx=artCanvas.getContext('2d');
const pctx=pinCanvas.getContext('2d');

let img=null;
let preview={
  x:0,
  y:0,
  scale:1
};

let pins=[];
let lineCache=new Map();
let running=false;
let sequence=[];
let svgLines=[];

function clamp(v,a,b){
  return Math.max(a,Math.min(b,v));
}

function val(id){
  return document.getElementById(id).value;
}

function num(id,fallback){
  const n=parseFloat(document.getElementById(id).value);
  return Number.isFinite(n)?n:fallback;
}

function canvasBg(){
  return val('canvasMode')==='black'?'#000000':'#ffffff';
}

function isCircle(){
  return val('canvasShape')==='circle';
}

function isSquare(){
  return val('canvasShape')==='square';
}

function isRect(){
  return val('canvasShape')==='rect';
}

function updateStatus(extra=''){
  document.getElementById('status').textContent='Lines generated: '+sequence.length+(extra?' — '+extra:'');
}

function updateInstructions(full=false){
  updateStatus();

  const data=full?sequence:sequence.slice(-500);

  instructions.value=data.map(s=>{
    return `${s.line}) ${s.color}: ${s.from} → ${s.to}`;
  }).join('\n');
}

const THREADS={
  K:{
    name:'K',
    hex:'#000000',
    rgb:[0,0,0]
  },
  R:{
    name:'R',
    hex:'#e11d48',
    rgb:[225,29,72]
  },
  G:{
    name:'G',
    hex:'#16a34a',
    rgb:[22,163,74]
  },
  B:{
    name:'B',
    hex:'#2563eb',
    rgb:[37,99,235]
  },
  Y:{
    name:'Y',
    hex:'#facc15',
    rgb:[250,204,21]
  },
  K2:{
    name:'K2',
    hex:'#111111',
    rgb:[17,17,17]
  },
  WHITE:{
    name:'WHITE',
    hex:'#ffffff',
    rgb:[255,255,255]
  }
};

function luminance(r,g,b){
  return 0.299*r+0.587*g+0.114*b;
}

function updateCanvasDimensions(){
  if(isRect()){
    CW=clamp(Math.floor(num('canvasWide',900)),200,1600);
    CH=clamp(Math.floor(num('canvasHigh',600)),200,1600);
  }else{
    CW=720;
    CH=720;
  }

  targetCanvas.width=CW;
  targetCanvas.height=CH;
  artCanvas.width=CW;
  artCanvas.height=CH;
  pinCanvas.width=CW;
  pinCanvas.height=CH;

  stage.style.aspectRatio=CW+'/'+CH;
  stage.style.maxWidth=Math.min(960,CW)+'px';

  stage.classList.toggle('squareStage',isSquare());
  stage.classList.toggle('rectStage',isRect());
}

function clearArtTransparent(){
  actx.clearRect(0,0,CW,CH);
}

function fillArtBackground(){
  actx.clearRect(0,0,CW,CH);
  actx.fillStyle=canvasBg();
  actx.fillRect(0,0,CW,CH);
}

function drawTarget(){
  tctx.clearRect(0,0,CW,CH);
  tctx.fillStyle=canvasBg();
  tctx.fillRect(0,0,CW,CH);

  tctx.save();

  if(isCircle()){
    tctx.beginPath();
    tctx.arc(CW/2,CH/2,Math.min(CW,CH)*0.49,0,Math.PI*2);
    tctx.clip();
  }

  if(img){
    tctx.drawImage(
      img,
      preview.x,
      preview.y,
      img.width*preview.scale,
      img.height*preview.scale
    );
  }

  tctx.restore();

  targetCanvas.style.display='block';
}

function makePins(){
  updateCanvasDimensions();

  const n=Math.max(12,Math.floor(num('pinCount',300)));

  pins=[];

  if(isCircle()){
    const cx=CW/2;
    const cy=CH/2;
    const r=Math.min(CW,CH)*0.485;

    for(let i=0;i<n;i++){
      const th=Math.PI*2*i/n-Math.PI/2;

      pins.push({
        i,
        x:cx+Math.cos(th)*r,
        y:cy+Math.sin(th)*r
      });
    }
  }else{
    const margin=0.03;
    const left=CW*margin;
    const top=CH*margin;
    const boxW=CW*(1-margin*2);
    const boxH=CH*(1-margin*2);
    const perimeter=2*boxW+2*boxH;

    for(let i=0;i<n;i++){
      const t=i/n*perimeter;

      let x;
      let y;

      if(t<boxW){
        x=left+t;
        y=top;
      }else if(t<boxW+boxH){
        x=left+boxW;
        y=top+t-boxW;
      }else if(t<2*boxW+boxH){
        x=left+boxW-(t-boxW-boxH);
        y=top+boxH;
      }else{
        x=left;
        y=top+boxH-(t-2*boxW-boxH);
      }

      pins.push({
        i,
        x,
        y
      });
    }
  }

  lineCache.clear();
  drawPins();
}

function drawPins(){
  pctx.clearRect(0,0,CW,CH);
  pctx.save();

  pctx.fillStyle=val('canvasMode')==='black'?'#e5e7eb':'#111827';
  pctx.strokeStyle=val('canvasMode')==='black'?'#111827':'#ffffff';
  pctx.lineWidth=1;

  pins.forEach((p,i)=>{
    pctx.beginPath();
    pctx.arc(p.x,p.y,i===0?4:2.1,0,Math.PI*2);
    pctx.fill();
    pctx.stroke();
  });

  if(pins[0]){
    pctx.fillStyle='red';
    pctx.font='13px Arial';
    pctx.fillText('0',pins[0].x+6,pins[0].y+14);
  }

  pctx.restore();
}

function resetArt(){
  running=false;
  sequence=[];
  svgLines=[];
  lineCache.clear();

  updateCanvasDimensions();
  clearArtTransparent();
  drawTarget();
  makePins();
  updateInstructions(true);
}

function prepareTarget(solverW){
  const rw=solverW;
  const rh=Math.max(1,Math.round(solverW*CH/CW));

  const off=document.createElement('canvas');
  off.width=rw;
  off.height=rh;

  const o=off.getContext('2d',{willReadFrequently:true});

  o.fillStyle=canvasBg();
  o.fillRect(0,0,rw,rh);

  o.save();

  if(isCircle()){
    o.beginPath();
    o.arc(rw/2,rh/2,Math.min(rw,rh)*0.49,0,Math.PI*2);
    o.clip();
  }

  if(img){
    o.drawImage(
      img,
      preview.x*rw/CW,
      preview.y*rh/CH,
      img.width*preview.scale*rw/CW,
      img.height*preview.scale*rh/CH
    );
  }

  o.restore();

  const raw=o.getImageData(0,0,rw,rh).data;

  const target=new Float32Array(rw*rh*3);
  const work=new Float32Array(rw*rh*3);
  const mask=new Uint8Array(rw*rh);

  const base=val('canvasMode')==='black'?0:255;

  for(let i=0,j=0;i<raw.length;i+=4,j++){
    target[j*3]=raw[i];
    target[j*3+1]=raw[i+1];
    target[j*3+2]=raw[i+2];

    work[j*3]=base;
    work[j*3+1]=base;
    work[j*3+2]=base;
  }

  if(isCircle()){
    const cx=rw/2;
    const cy=rh/2;
    const rad=Math.min(rw,rh)*0.49;

    for(let y=0;y<rh;y++){
      for(let x=0;x<rw;x++){
        const j=y*rw+x;
        mask[j]=Math.hypot(x-cx,y-cy)<=rad?1:0;
      }
    }
  }else{
    mask.fill(1);
  }

  return{
    target,
    work,
    mask,
    rw,
    rh
  };
}

function bresenham(x0,y0,x1,y1,w,h){
  x0=Math.round(x0);
  y0=Math.round(y0);
  x1=Math.round(x1);
  y1=Math.round(y1);

  const pts=[];

  let dx=Math.abs(x1-x0);
  let dy=Math.abs(y1-y0);
  let sx=x0<x1?1:-1;
  let sy=y0<y1?1:-1;
  let err=dx-dy;

  while(true){
    if(x0>=0&&x0<w&&y0>=0&&y0<h){
      pts.push(y0*w+x0);
    }

    if(x0===x1&&y0===y1){
      break;
    }

    const e2=2*err;

    if(e2>-dy){
      err-=dy;
      x0+=sx;
    }

    if(e2<dx){
      err+=dx;
      y0+=sy;
    }
  }

  return pts;
}

function linePixels(a,b,rw,rh){
  const key=a<b
    ? `${a}-${b}-${rw}-${rh}`
    : `${b}-${a}-${rw}-${rh}`;

  if(lineCache.has(key)){
    return lineCache.get(key);
  }

  if(lineCache.size>50000){
    lineCache.clear();
  }

  const p=pins[a];
  const q=pins[b];

  const pts=bresenham(
    p.x/CW*rw,
    p.y/CH*rh,
    q.x/CW*rw,
    q.y/CH*rh,
    rw,
    rh
  );

  lineCache.set(key,pts);

  return pts;
}

function circularDistance(a,b,n){
  const d=Math.abs(a-b);
  return Math.min(d,n-d);
}

function getCandidates(current){
  const n=pins.length;
  const count=Math.max(2,Math.floor(num('candidates',200)));
  const minSkip=Math.max(0,Math.floor(num('minSkip',8)));
  const mode=val('candidateMode');
  const out=new Set();

  function ok(i){
    return i!==current && circularDistance(i,current,n)>=minSkip;
  }

  if(mode==='all'){
    for(let i=0;i<n;i++){
      if(ok(i)){
        out.add(i);
      }
    }

    return [...out];
  }

  let guard=0;

  while(out.size<Math.min(count,n-1) && guard<count*24){
    guard++;

    let to;

    if(mode==='wide'){
      const half=Math.floor(n/2);
      const spread=Math.floor(n*(0.10+Math.random()*0.40));

      to=(current+half+Math.floor((Math.random()*2-1)*spread)+n*4)%n;
    }else{
      to=Math.floor(Math.random()*n);
    }

    to=Math.floor(to);

    if(ok(to)){
      out.add(to);
    }
  }

  if(out.size===0){
    for(let i=0;i<n;i++){
      if(i!==current){
        out.add(i);
      }
    }
  }

  return [...out];
}

function pixelError(target,work,pix){
  const i=pix*3;

  const dr=target[i]-work[i];
  const dg=target[i+1]-work[i+1];
  const db=target[i+2]-work[i+2];

  return dr*dr+dg*dg+db*db;
}

function whiteWeight(target,work,pix){
  const i=pix*3;

  const tl=luminance(target[i],target[i+1],target[i+2]);
  const wl=luminance(work[i],work[i+1],work[i+2]);

  if(tl<160){
    return 0.15;
  }

  if(tl>wl+12){
    return 1.35;
  }

  return 0.55;
}

function darkWeight(target,work,pix){
  const i=pix*3;

  const tl=luminance(target[i],target[i+1],target[i+2]);
  const wl=luminance(work[i],work[i+1],work[i+2]);

  return tl<wl-8?1.25:0.55;
}

function scoreLine(a,b,thread,prepared,amount){
  const pts=linePixels(a,b,prepared.rw,prepared.rh);

  const target=prepared.target;
  const work=prepared.work;
  const mask=prepared.mask;
  const c=thread.rgb;

  let gain=0;
  let seen=0;

  for(const pix of pts){
    if(!mask[pix]){
      continue;
    }

    const i=pix*3;

    const e0=pixelError(target,work,pix);

    const nr=work[i]*(1-amount)+c[0]*amount;
    const ng=work[i+1]*(1-amount)+c[1]*amount;
    const nb=work[i+2]*(1-amount)+c[2]*amount;

    const dr=target[i]-nr;
    const dg=target[i+1]-ng;
    const db=target[i+2]-nb;

    let improvement=e0-(dr*dr+dg*dg+db*db);

    if(thread.name==='WHITE'){
      improvement*=whiteWeight(target,work,pix);
    }

    if(thread.name==='K' || thread.name==='K2'){
      improvement*=darkWeight(target,work,pix);
    }

    gain+=improvement;
    seen++;
  }

  return seen?gain/seen:-Infinity;
}

function applyLine(a,b,thread,prepared,amount){
  const pts=linePixels(a,b,prepared.rw,prepared.rh);

  const work=prepared.work;
  const mask=prepared.mask;
  const c=thread.rgb;

  for(const pix of pts){
    if(!mask[pix]){
      continue;
    }

    const i=pix*3;

    work[i]=work[i]*(1-amount)+c[0]*amount;
    work[i+1]=work[i+1]*(1-amount)+c[1]*amount;
    work[i+2]=work[i+2]*(1-amount)+c[2]*amount;
  }
}

function drawLine(a,b,thread){
  const p=pins[a];
  const q=pins[b];

  let op=clamp(num('renderOpacity',0.18),0.01,1);
  let thickness=clamp(num('lineThickness',0.55),0.05,4);

  if(thread.name==='WHITE'){
    thickness=clamp(num('whiteThickness',0.42),0.05,4);
    op*=0.9;
  }

  if(thread.name==='K2'){
    op=Math.min(0.34,op*1.15);
  }

  actx.strokeStyle=`rgba(${thread.rgb[0]},${thread.rgb[1]},${thread.rgb[2]},${op})`;
  actx.lineWidth=thickness;
  actx.lineCap='round';

  actx.beginPath();
  actx.moveTo(p.x,p.y);
  actx.lineTo(q.x,q.y);
  actx.stroke();

  svgLines.push(
    `<line x1="${p.x.toFixed(2)}" y1="${p.y.toFixed(2)}" x2="${q.x.toFixed(2)}" y2="${q.y.toFixed(2)}" stroke="${thread.hex}" stroke-opacity="${op}" stroke-width="${thickness}" data-color="${thread.name}" />`
  );
}

function buildPasses(total){
  if(val('colorMode')==='mono'){
    return[
      {
        thread:val('canvasMode')==='black'?THREADS.WHITE:THREADS.K,
        count:total
      }
    ];
  }

  let k=Math.max(0,num('kShare',0.24));
  let rgb=Math.max(0,num('rgbShare',0.46));
  let k2=Math.max(0,num('k2Share',0.18));
  let white=val('colorMode')==='krgbyNoWhite'
    ? 0
    : Math.max(0,num('whiteShare',0.12));

  const sum=Math.max(0.0001,k+rgb+k2+white);

  k/=sum;
  rgb/=sum;
  k2/=sum;
  white/=sum;

  const kCount=Math.floor(total*k);
  const rgbTotal=Math.floor(total*rgb);
  const k2Count=Math.floor(total*k2);
  const whiteCount=Math.max(0,total-kCount-rgbTotal-k2Count);
  const each=Math.max(0,Math.floor(rgbTotal/4));

  const passes=[];

  if(kCount){
    passes.push({
      thread:THREADS.K,
      count:kCount
    });
  }

  if(each){
    passes.push({
      thread:THREADS.R,
      count:each
    });

    passes.push({
      thread:THREADS.G,
      count:each
    });

    passes.push({
      thread:THREADS.B,
      count:each
    });

    passes.push({
      thread:THREADS.Y,
      count:Math.max(0,rgbTotal-each*3)
    });
  }

  if(k2Count){
    passes.push({
      thread:THREADS.K2,
      count:k2Count
    });
  }

  if(whiteCount){
    passes.push({
      thread:THREADS.WHITE,
      count:whiteCount
    });
  }

  return passes.filter(p=>p.count>0);
}

async function generate(){
  if(!img || running){
    return;
  }

  running=true;

  sequence=[];
  svgLines=[];
  lineCache.clear();

  makePins();
  fillArtBackground();

  targetCanvas.style.display='none';

  const solverWidth=clamp(Math.floor(num('res',380)),80,900);
  const totalLines=Math.max(1,Math.floor(num('maxLines',4600)));
  const baseAmount=clamp(num('subtractOpacity',0.075),0.001,1);
  const prepared=prepareTarget(solverWidth);
  const passes=buildPasses(totalLines);

  let globalLine=0;

  for(let pi=0;pi<passes.length && running;pi++){
    const pass=passes[pi];
    const thread=pass.thread;

    let amount=baseAmount;

    if(thread.name==='WHITE'){
      amount*=0.5;
    }

    if(thread.name==='K2'){
      amount*=0.7;
    }

    if(thread.name==='Y'){
      amount*=0.78;
    }

    let current=Math.floor((pins.length/passes.length)*pi)%pins.length;

    const recent=[];
    const recentLimit=Math.max(20,Math.floor(pins.length*0.22));

    for(let local=0;local<pass.count && running;local++){
      let bestTo=-1;
      let bestScore=0;

      const candidateList=getCandidates(current);

      for(const to of candidateList){
        const key=current<to
          ? `${thread.name}-${current}-${to}`
          : `${thread.name}-${to}-${current}`;

        if(recent.includes(key)){
          continue;
        }

        const score=scoreLine(
          current,
          to,
          thread,
          prepared,
          amount
        );

        if(score>bestScore){
          bestScore=score;
          bestTo=to;
        }
      }

      if(bestTo<0 || bestScore<=0.05){
        break;
      }

      const from=current;
      const to=bestTo;

      const key=from<to
        ? `${thread.name}-${from}-${to}`
        : `${thread.name}-${to}-${from}`;

      applyLine(
        from,
        to,
        thread,
        prepared,
        amount
      );

      drawLine(
        from,
        to,
        thread
      );

      recent.push(key);

      while(recent.length>recentLimit){
        recent.shift();
      }

      current=to;
      globalLine++;

      sequence.push({
        line:sequence.length+1,
        color:thread.name,
        from,
        to
      });

      if((globalLine&15)===0){
        updateStatus('working '+thread.name);

        if((globalLine&63)===0){
          updateInstructions(false);
        }

        await new Promise(r=>requestAnimationFrame(r));
      }
    }
  }

  running=false;
  updateInstructions(true);
  drawPins();
}

function makeTXT(){
  if(sequence.length===0){
    return 'No string-art instructions generated yet.';
  }

  return sequence.map(s=>{
    return `${s.line}) ${s.color}: ${s.from} -> ${s.to}`;
  }).join('\n');
}

function makeCSV(){
  const rows=['Line,Color,From Pin,To Pin'];

  sequence.forEach(s=>{
    rows.push(`${s.line},"${s.color}",${s.from},${s.to}`);
  });

  return rows.join('\n');
}

function makeSVG(){
  const border=isCircle()
    ? `<circle cx="${CW/2}" cy="${CH/2}" r="${Math.min(CW,CH)*0.485}" fill="none" stroke="${val('canvasMode')==='black'?'#fff':'#000'}" stroke-opacity="0.25" stroke-width="1"/>`
    : `<rect x="${CW*0.03}" y="${CH*0.03}" width="${CW*0.94}" height="${CH*0.94}" fill="none" stroke="${val('canvasMode')==='black'?'#fff':'#000'}" stroke-opacity="0.25" stroke-width="1"/>`;

  return `<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" width="${CW}" height="${CH}" viewBox="0 0 ${CW} ${CH}">
<rect x="0" y="0" width="${CW}" height="${CH}" fill="${canvasBg()}"/>
${border}
${svgLines.join('\n')}
</svg>`;
}

function download(name,text,type){
  const blob=new Blob([text],{type:type+';charset=utf-8'});
  const url=URL.createObjectURL(blob);

  const a=document.createElement('a');
  a.href=url;
  a.download=name;
  a.style.display='none';

  document.body.appendChild(a);
  a.click();

  setTimeout(()=>{
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
  },500);
}

fileInput.addEventListener('change',e=>{
  const f=e.target.files && e.target.files[0];

  if(!f){
    updateStatus('no image selected');
    return;
  }

  const reader=new FileReader();

  reader.onload=ev=>{
    const nextImg=new Image();

    nextImg.onload=()=>{
      img=nextImg;

      updateCanvasDimensions();

      preview.scale=Math.max(CW/img.width,CH/img.height);
      preview.x=(CW-img.width*preview.scale)/2;
      preview.y=(CH-img.height*preview.scale)/2;

      sequence=[];
      svgLines=[];
      lineCache.clear();

      clearArtTransparent();
      drawTarget();
      makePins();
      updateInstructions(true);
      updateStatus('image loaded');
    };

    nextImg.onerror=()=>{
      updateStatus('image failed to load');
    };

    nextImg.src=ev.target.result;
  };

  reader.onerror=()=>{
    updateStatus('file read failed');
  };

  reader.readAsDataURL(f);
});

generateBtn.onclick=generate;

stopBtn.onclick=()=>{
  running=false;
  updateStatus('stopping');
};

resetBtn.onclick=()=>{
  resetArt();
};

copyBtn.onclick=async()=>{
  const text=makeTXT();

  try{
    await navigator.clipboard.writeText(text);
  }catch(e){
    instructions.value=text;
    instructions.focus();
    instructions.select();
    document.execCommand('copy');
  }
};

txtBtn.onclick=()=>{
  download(
    'krgby-k2-white-string-art-instructions.txt',
    makeTXT(),
    'text/plain'
  );
};

csvBtn.onclick=()=>{
  download(
    'krgby-k2-white-string-art-instructions.csv',
    makeCSV(),
    'text/csv'
  );
};

svgBtn.onclick=()=>{
  download(
    'krgby-k2-white-string-art-preview.svg',
    makeSVG(),
    'image/svg+xml'
  );
};

pinCount.onchange=()=>{
  if(!running){
    makePins();
  }
};

canvasMode.onchange=()=>{
  if(!running){
    resetArt();
  }
};

canvasShape.onchange=()=>{
  if(!running){
    resetArt();
  }
};

canvasWide.onchange=()=>{
  if(!running && isRect()){
    resetArt();
  }
};

canvasHigh.onchange=()=>{
  if(!running && isRect()){
    resetArt();
  }
};

let pointers=new Map();
let startScale=1;
let startX=0;
let startY=0;
let lastDist=0;
let startCenter={
  x:0,
  y:0
};

function pointerPos(e){
  const r=stage.getBoundingClientRect();

  return{
    x:(e.clientX-r.left)*CW/r.width,
    y:(e.clientY-r.top)*CH/r.height
  };
}

function distance(a,b){
  return Math.hypot(a.x-b.x,a.y-b.y);
}

function midpoint(a,b){
  return{
    x:(a.x+b.x)/2,
    y:(a.y+b.y)/2
  };
}

stage.addEventListener('pointerdown',e=>{
  if(!img || running){
    return;
  }

  e.preventDefault();
  stage.setPointerCapture(e.pointerId);

  pointers.set(e.pointerId,pointerPos(e));

  if(pointers.size===2){
    const pts=[...pointers.values()];

    lastDist=distance(pts[0],pts[1]);
    startScale=preview.scale;
    startX=preview.x;
    startY=preview.y;
    startCenter=midpoint(pts[0],pts[1]);
  }
},{passive:false});

stage.addEventListener('pointermove',e=>{
  if(!img || running || !pointers.has(e.pointerId)){
    return;
  }

  e.preventDefault();

  const old=pointers.get(e.pointerId);
  const now=pointerPos(e);

  pointers.set(e.pointerId,now);

  if(pointers.size===1){
    preview.x+=now.x-old.x;
    preview.y+=now.y-old.y;
    drawTarget();
  }

  if(pointers.size===2){
    const pts=[...pointers.values()];
    const d=distance(pts[0],pts[1]);
    const c=midpoint(pts[0],pts[1]);

    const newScale=clamp(startScale*(d/lastDist),0.05,20);
    const ratio=newScale/startScale;

    preview.scale=newScale;

    preview.x=startX+(c.x-startCenter.x)+(startCenter.x-startX)*(1-ratio);
    preview.y=startY+(c.y-startCenter.y)+(startCenter.y-startY)*(1-ratio);

    drawTarget();
  }
},{passive:false});

function endPointer(e){
  pointers.delete(e.pointerId);
}

stage.addEventListener('pointerup',endPointer);
stage.addEventListener('pointercancel',endPointer);
stage.addEventListener('pointerleave',endPointer);

stage.addEventListener('wheel',e=>{
  if(!img || running){
    return;
  }

  e.preventDefault();

  const p=pointerPos(e);
  const oldScale=preview.scale;
  const factor=e.deltaY<0?1.1:0.9;
  const newScale=clamp(oldScale*factor,0.05,20);
  const ratio=newScale/oldScale;

  preview.x=p.x-(p.x-preview.x)*ratio;
  preview.y=p.y-(p.y-preview.y)*ratio;
  preview.scale=newScale;

  drawTarget();
},{passive:false});

resetArt();
</script>
</body>
</html>
