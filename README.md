<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1.0,user-scalable=no">
<title>KRGBY + K2 + White String Art Generator</title>
<style>
:root{
  --bg:#0f172a;
  --panel:#111827;
  --text:#f8fafc;
  --muted:#cbd5e1;
  --accent:#2563eb;
  --accent2:#1d4ed8;
}
*{box-sizing:border-box}
body{
  margin:0;
  background:radial-gradient(circle at top,#1e3a8a 0,#0f172a 45%,#020617 100%);
  color:var(--text);
  font-family:Arial,sans-serif;
  text-align:center;
  overscroll-behavior:none;
}
h1{font-size:22px;margin:12px 8px 4px}
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
label{color:#a5b4fc;font-size:11px}
input,select,button{
  border-radius:8px;
  border:1px solid #334155;
  padding:7px;
  font-size:14px;
}
input[type="number"]{width:78px;text-align:center}
input[type="file"]{
  max-width:94vw;
  background:#020617;
  color:var(--text);
}
select{background:#f8fafc;color:#020617}
button{
  background:var(--accent);
  color:white;
  border:none;
  cursor:pointer;
  font-weight:bold;
}
button:hover{background:var(--accent2)}
button.danger{background:#b91c1c}
button.danger:hover{background:#991b1b}
button.ok{background:#16a34a}
button.ok:hover{background:#15803d}
#status{
  font-weight:bold;
  margin:8px auto;
  color:#facc15;
}
.stageWrap{
  width:96vw;
  max-width:760px;
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

.stage.squareStage{
  border-radius:0;
}
canvas{
  position:absolute;
  inset:0;
  width:100%;
  height:100%;
}
#targetCanvas{z-index:1;opacity:1}
#artCanvas{z-index:2;pointer-events:none}
#pinCanvas{z-index:3;pointer-events:none}
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
  Upload an image, crop it on the canvas, then generate a color string-art plan using Black, Red, Green, Blue, Yellow, a second Black detail pass, and a restrained White highlight pass.
</div>

<div class="controls">
  <div class="row">
    <input type="file" id="fileInput" accept="image/*">
  </div>

  <div class="row">
    <div class="block">
      <input id="pinCount" type="number" min="12" max="1200" value="260">
      <label>Pins</label>
    </div>

    <div class="block">
      <input id="maxLines" type="number" min="1" value="4200">
      <label>Total Lines</label>
    </div>

    <div class="block">
      <input id="candidates" type="number" min="2" value="190">
      <label>Candidates</label>
    </div>

    <div class="block">
      <input id="res" type="number" min="80" max="900" value="360">
      <label>Solver Res</label>
    </div>

    <div class="block">
      <input id="subtractOpacity" type="number" min="0.005" max="1" step="0.005" value="0.075">
      <label>Subtract</label>
    </div>

    <div class="block">
      <input id="renderOpacity" type="number" min="0.01" max="1" step="0.01" value="0.18">
      <label>Render Opacity</label>
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
      <label>Canvas</label>
    </div>

    <div class="block">
      <select id="canvasShape">
        <option value="circle">Circle</option>
        <option value="square">Square</option>
      </select>
      <label>Canvas Shape</label>
    </div>

    <div class="block">
      <select id="candidateMode">
        <option value="wide">Wide/chord-biased candidates</option>
        <option value="random">Repo-style random candidates</option>
        <option value="all">Scan all pins, slower</option>
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
    <canvas id="targetCanvas" width="720" height="720"></canvas>
    <canvas id="artCanvas" width="720" height="720"></canvas>
    <canvas id="pinCanvas" width="720" height="720"></canvas>
  </div>
  <div class="note">Upload image should appear immediately. Drag to move. Pinch or mouse wheel to zoom. Generate uses the exact crop shown.</div>
</div>

<div id="output">
<textarea id="instructions" readonly placeholder="Instructions will appear here..."></textarea>
</div>

<script>
const SIZE=720;

const targetCanvas=document.getElementById('targetCanvas');
const artCanvas=document.getElementById('artCanvas');
const pinCanvas=document.getElementById('pinCanvas');

const tctx=targetCanvas.getContext('2d',{willReadFrequently:true});
const actx=artCanvas.getContext('2d');
const pctx=pinCanvas.getContext('2d');

let img=null;
let preview={x:0,y:0,scale:1};
let pins=[];
let lineCache=new Map();
let running=false;
let sequence=[];
let svgLines=[];

function clamp(v,a,b){return Math.max(a,Math.min(b,v));}
function val(id){return document.getElementById(id).value;}
function num(id,fallback){
  const n=parseFloat(document.getElementById(id).value);
  return Number.isFinite(n)?n:fallback;
}
function canvasBg(){return val('canvasMode')==='black'?'#000000':'#ffffff';}

function isSquareMode(){
  return val('canvasShape')==='square';
}

function updateStageShape(){
  if(isSquareMode()){
    stage.classList.add('squareStage');
  }else{
    stage.classList.remove('squareStage');
  }
}

function updateStatus(extra=''){
  document.getElementById('status').textContent='Lines generated: '+sequence.length+(extra?' — '+extra:'');
}
function updateInstructions(full=false){
  updateStatus();
  const data=full?sequence:sequence.slice(-500);
  instructions.value=data.map(s=>`${s.line}) ${s.color}: ${s.from} → ${s.to}`).join('\n');
}
function clearArtTransparent(){actx.clearRect(0,0,SIZE,SIZE);}
function fillArtBackground(){
  actx.clearRect(0,0,SIZE,SIZE);
  actx.fillStyle=canvasBg();
  actx.fillRect(0,0,SIZE,SIZE);
}
function hexToRgb(hex){
  hex=hex.replace('#','');
  const n=parseInt(hex,16);
  return [(n>>16)&255,(n>>8)&255,n&255];
}
function rgbToHex(c){
  return '#'+c.map(v=>Math.round(clamp(v,0,255)).toString(16).padStart(2,'0')).join('');
}
function luminance(r,g,b){return 0.299*r+0.587*g+0.114*b;}

const THREADS={
  K:{name:'K',hex:'#000000',rgb:[0,0,0]},
  R:{name:'R',hex:'#e11d48',rgb:[225,29,72]},
  G:{name:'G',hex:'#16a34a',rgb:[22,163,74]},
  B:{name:'B',hex:'#2563eb',rgb:[37,99,235]},
  Y:{name:'Y',hex:'#facc15',rgb:[250,204,21]},
  K2:{name:'K2',hex:'#111111',rgb:[17,17,17]},
  WHITE:{name:'WHITE',hex:'#ffffff',rgb:[255,255,255]}
};

function drawTarget(){
  updateStageShape();
  targetCanvas.style.display='block';
  tctx.clearRect(0,0,SIZE,SIZE);
  tctx.fillStyle=canvasBg();
  tctx.fillRect(0,0,SIZE,SIZE);

  tctx.save();

  if(!isSquareMode()){
    tctx.beginPath();
    tctx.arc(SIZE/2,SIZE/2,SIZE*0.49,0,Math.PI*2);
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
}

function makePins(){
  const n=Math.max(12,Math.floor(num('pinCount',260)));
  pins=[];
  if(isSquareMode()){

    const side=SIZE*0.94;
    const left=(SIZE-side)/2;
    const top=(SIZE-side)/2;
    const perimeter=side*4;

    for(let i=0;i<n;i++){

      const t=(i/n)*perimeter;

      let x,y;

      if(t<side){
        x=left+t;
        y=top;
      }else if(t<side*2){
        x=left+side;
        y=top+(t-side);
      }else if(t<side*3){
        x=left+side-(t-side*2);
        y=top+side;
      }else{
        x=left;
        y=top+side-(t-side*3);
      }

      pins.push({i,x,y});
    }

  }else{

    const cx=SIZE/2;
    const cy=SIZE/2;
    const r=SIZE*0.485;

    for(let i=0;i<n;i++){
      const theta=(Math.PI*2*i/n)-Math.PI/2;
      pins.push({i,x:cx+Math.cos(theta)*r,y:cy+Math.sin(theta)*r});
    }

  }

  lineCache.clear();
  drawPins();
}

function drawPins(){
  pctx.clearRect(0,0,SIZE,SIZE);
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
  updateStageShape();
  running=false;
  sequence=[];
  svgLines=[];
  lineCache.clear();
  clearArtTransparent();
  drawTarget();
  makePins();
  updateInstructions(true);
}

function prepareTarget(res){
  const off=document.createElement('canvas');
  off.width=res;
  off.height=res;
  const o=off.getContext('2d',{willReadFrequently:true});

  o.fillStyle=canvasBg();
  o.fillRect(0,0,res,res);

  o.save();

  if(!isSquareMode()){
    o.beginPath();
    o.arc(res/2,res/2,res*0.49,0,Math.PI*2);
    o.clip();
  }

  if(img){
    o.drawImage(
      img,
      preview.x*res/SIZE,
      preview.y*res/SIZE,
      img.width*preview.scale*res/SIZE,
      img.height*preview.scale*res/SIZE
    );
  }

  o.restore();

  const raw=o.getImageData(0,0,res,res).data;
  const target=new Float32Array(res*res*3);
  const work=new Float32Array(res*res*3);
  const mask=new Uint8Array(res*res);

  const base=val('canvasMode')==='black'?0:255;

  for(let i=0,j=0;i<raw.length;i+=4,j++){
    target[j*3]=raw[i];
    target[j*3+1]=raw[i+1];
    target[j*3+2]=raw[i+2];

    work[j*3]=base;
    work[j*3+1]=base;
    work[j*3+2]=base;
  }

  if(isSquareMode()){

    for(let y=0;y<res;y++){
      for(let x=0;x<res;x++){
        const j=y*res+x;
        mask[j]=1;
      }
    }

  }else{

    const cx=res/2;
    const cy=res/2;
    const rad=res*0.49;

    for(let y=0;y<res;y++){
      for(let x=0;x<res;x++){
        const j=y*res+x;
        const d=Math.hypot(x-cx,y-cy);
        mask[j]=d<=rad?1:0;
      }
    }

  }

  return{target,work,mask,res};
}

function bresenham(x0,y0,x1,y1,w,h){
  x0=Math.round(x0); y0=Math.round(y0);
  x1=Math.round(x1); y1=Math.round(y1);
  const pts=[];
  let dx=Math.abs(x1-x0);
  let dy=Math.abs(y1-y0);
  let sx=x0<x1?1:-1;
  let sy=y0<y1?1:-1;
  let err=dx-dy;

  while(true){
    if(x0>=0&&x0<w&&y0>=0&&y0<h)pts.push(y0*w+x0);
    if(x0===x1&&y0===y1)break;
    const e2=2*err;
    if(e2>-dy){err-=dy;x0+=sx;}
    if(e2<dx){err+=dx;y0+=sy;}
  }
  return pts;
}

function linePixels(a,b,res){
  const key=a<b?`${a}-${b}-${res}`:`${b}-${a}-${res}`;
  if(lineCache.has(key))return lineCache.get(key);
  if(lineCache.size>50000)lineCache.clear();

  const p=pins[a];
  const q=pins[b];
  const pts=bresenham(
    p.x/SIZE*res,
    p.y/SIZE*res,
    q.x/SIZE*res,
    q.y/SIZE*res,
    res,
    res
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
  const count=Math.max(2,Math.floor(num('candidates',190)));
  const minSkip=Math.max(0,Math.floor(num('minSkip',8)));
  const mode=val('candidateMode');
  const out=new Set();

  function ok(i){
    return i!==current && circularDistance(i,current,n)>=minSkip;
  }

  if(mode==='all'){
    for(let i=0;i<n;i++)if(ok(i))out.add(i);
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
    if(ok(to))out.add(to);
  }

  if(out.size===0){
    for(let i=0;i<n;i++)if(i!==current)out.add(i);
  }

  return [...out];
}

function pixelError(target,work,idx){
  const r=idx*3;
  const dr=target[r]-work[r];
  const dg=target[r+1]-work[r+1];
  const db=target[r+2]-work[r+2];
  return dr*dr+dg*dg+db*db;
}

function whiteHighlightWeight(target,work,idx){
  const r=idx*3;
  const tl=luminance(target[r],target[r+1],target[r+2]);
  const wl=luminance(work[r],work[r+1],work[r+2]);
  if(tl<160)return 0.15;
  if(tl>wl+12)return 1.35;
  return 0.55;
}

function darkDetailWeight(target,work,idx){
  const r=idx*3;
  const tl=luminance(target[r],target[r+1],target[r+2]);
  const wl=luminance(work[r],work[r+1],work[r+2]);
  if(tl<wl-8)return 1.25;
  return 0.55;
}

function scoreLine(a,b,thread,prepared,amount){
  const pts=linePixels(a,b,prepared.res);
  if(!pts.length)return -Infinity;

  const target=prepared.target;
  const work=prepared.work;
  const mask=prepared.mask;
  const c=thread.rgb;
  let gain=0;
  let seen=0;

  for(let i=0;i<pts.length;i++){
    const pix=pts[i];
    if(!mask[pix])continue;

    const idx=pix*3;
    const e0=pixelError(target,work,pix);

    const nr=work[idx]*(1-amount)+c[0]*amount;
    const ng=work[idx+1]*(1-amount)+c[1]*amount;
    const nb=work[idx+2]*(1-amount)+c[2]*amount;

    const dr=target[idx]-nr;
    const dg=target[idx+1]-ng;
    const db=target[idx+2]-nb;
    const e1=dr*dr+dg*dg+db*db;

    let improvement=e0-e1;

    if(thread.name==='WHITE'){
      improvement*=whiteHighlightWeight(target,work,pix);
    }

    if(thread.name==='K2' || thread.name==='K'){
      improvement*=darkDetailWeight(target,work,pix);
    }

    gain+=improvement;
    seen++;
  }

  if(!seen)return -Infinity;
  return gain/seen;
}

function applyLine(a,b,thread,prepared,amount){
  const pts=linePixels(a,b,prepared.res);
  const work=prepared.work;
  const mask=prepared.mask;
  const c=thread.rgb;

  for(let i=0;i<pts.length;i++){
    const pix=pts[i];
    if(!mask[pix])continue;
    const idx=pix*3;
    work[idx]=work[idx]*(1-amount)+c[0]*amount;
    work[idx+1]=work[idx+1]*(1-amount)+c[1]*amount;
    work[idx+2]=work[idx+2]*(1-amount)+c[2]*amount;
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

  svgLines.push(`<line x1="${p.x.toFixed(2)}" y1="${p.y.toFixed(2)}" x2="${q.x.toFixed(2)}" y2="${q.y.toFixed(2)}" stroke="${thread.hex}" stroke-opacity="${op}" stroke-width="${thickness}" data-color="${thread.name}" />`);
}

function buildPasses(total){
  const mode=val('colorMode');

  if(mode==='mono'){
    return [
      {thread:val('canvasMode')==='black'?THREADS.WHITE:THREADS.K, count:total}
    ];
  }

  let k=Math.max(0,num('kShare',0.24));
  let rgb=Math.max(0,num('rgbShare',0.46));
  let k2=Math.max(0,num('k2Share',0.18));
  let white=mode==='krgbyNoWhite'?0:Math.max(0,num('whiteShare',0.12));
  const sum=Math.max(0.0001,k+rgb+k2+white);

  k/=sum; rgb/=sum; k2/=sum; white/=sum;

  const kCount=Math.floor(total*k);
  const rgbTotal=Math.floor(total*rgb);
  const k2Count=Math.floor(total*k2);
  const whiteCount=Math.max(0,total-kCount-rgbTotal-k2Count);

  const colorEach=Math.max(0,Math.floor(rgbTotal/4));
  const passes=[];

  if(kCount>0)passes.push({thread:THREADS.K,count:kCount});
  if(colorEach>0){
    passes.push({thread:THREADS.R,count:colorEach});
    passes.push({thread:THREADS.G,count:colorEach});
    passes.push({thread:THREADS.B,count:colorEach});
    passes.push({thread:THREADS.Y,count:Math.max(0,rgbTotal-colorEach*3)});
  }
  if(k2Count>0)passes.push({thread:THREADS.K2,count:k2Count});
  if(whiteCount>0)passes.push({thread:THREADS.WHITE,count:whiteCount});

  return passes.filter(p=>p.count>0);
}

async function generate(){
  if(!img || running)return;

  running=true;
  sequence=[];
  svgLines=[];
  lineCache.clear();

  const n=Math.max(12,Math.floor(num('pinCount',260)));
  if(n!==pins.length)makePins();

  fillArtBackground();
  targetCanvas.style.display='none';

  const solverRes=clamp(Math.floor(num('res',360)),80,900);
  const totalLines=Math.max(1,Math.floor(num('maxLines',4200)));
  const amountBase=clamp(num('subtractOpacity',0.075),0.001,1);
  const prepared=prepareTarget(solverRes);
  const passes=buildPasses(totalLines);

  let globalLine=0;

  for(let pi=0;pi<passes.length && running;pi++){
    const pass=passes[pi];
    const thread=pass.thread;
    let amount=amountBase;

    if(thread.name==='WHITE')amount*=0.50;
    if(thread.name==='K2')amount*=0.70;
    if(thread.name==='Y')amount*=0.78;

    let current=Math.floor((pins.length/passes.length)*pi)%pins.length;
    const recent=[];
    const recentLimit=Math.max(20,Math.floor(pins.length*0.22));

    for(let local=0;local<pass.count && running;local++){
      let bestTo=-1;
      let bestScore=0;
      const candidateList=getCandidates(current);

      for(let i=0;i<candidateList.length;i++){
        const to=candidateList[i];
        const key=current<to?`${thread.name}-${current}-${to}`:`${thread.name}-${to}-${current}`;

        if(recent.includes(key))continue;

        const sc=scoreLine(current,to,thread,prepared,amount);

        if(sc>bestScore){
          bestScore=sc;
          bestTo=to;
        }
      }

      if(bestTo<0 || bestScore<=0.05){
        break;
      }

      const from=current;
      const to=bestTo;
      const key=from<to?`${thread.name}-${from}-${to}`:`${thread.name}-${to}-${from}`;

      applyLine(from,to,thread,prepared,amount);
      drawLine(from,to,thread);

      recent.push(key);
      while(recent.length>recentLimit)recent.shift();

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
        if((globalLine&63)===0)updateInstructions(false);
        await new Promise(r=>requestAnimationFrame(r));
      }
    }
  }

  running=false;
  updateInstructions(true);
  drawPins();
}

function makeTXT(){
  if(sequence.length===0)return 'No string-art instructions generated yet.';
  return sequence.map(s=>`${s.line}) ${s.color}: ${s.from} -> ${s.to}`).join('\n');
}

function makeCSV(){
  const rows=['Line,Color,From Pin,To Pin'];
  sequence.forEach(s=>rows.push(`${s.line},"${s.color}",${s.from},${s.to}`));
  return rows.join('\n');
}

function makeSVG(){
  return `<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" width="${SIZE}" height="${SIZE}" viewBox="0 0 ${SIZE} ${SIZE}">
<rect x="0" y="0" width="${SIZE}" height="${SIZE}" fill="${canvasBg()}"/>
${isSquareMode()
? `<rect x="${SIZE*0.03}" y="${SIZE*0.03}" width="${SIZE*0.94}" height="${SIZE*0.94}" fill="none" stroke="${val('canvasMode')==='black'?'#fff':'#000'}" stroke-opacity="0.25" stroke-width="1"/>`
: `<circle cx="${SIZE/2}" cy="${SIZE/2}" r="${SIZE*0.485}" fill="none" stroke="${val('canvasMode')==='black'?'#fff':'#000'}" stroke-opacity="0.25" stroke-width="1"/>`
}
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
  if(!f)return;

  const url=URL.createObjectURL(f);
  const nextImg=new Image();

  nextImg.onload=()=>{
    URL.revokeObjectURL(url);
    img=nextImg;

    preview.scale=Math.max(SIZE/img.width,SIZE/img.height);
    preview.x=(SIZE-img.width*preview.scale)/2;
    preview.y=(SIZE-img.height*preview.scale)/2;

    sequence=[];
    svgLines=[];
    lineCache.clear();

    clearArtTransparent();
    drawTarget();
    makePins();
    updateInstructions(true);
    targetCanvas.style.display='block';
    updateStatus('image loaded');
  };

  nextImg.onerror=()=>{
    URL.revokeObjectURL(url);
    updateStatus('image failed to load');
  };

  nextImg.src=url;
});

generateBtn.onclick=generate;
stopBtn.onclick=()=>{running=false;updateStatus('stopping');};
resetBtn.onclick=()=>{resetArt();};

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

txtBtn.onclick=()=>download('krgby-k2-white-string-art-instructions.txt',makeTXT(),'text/plain');
csvBtn.onclick=()=>download('krgby-k2-white-string-art-instructions.csv',makeCSV(),'text/csv');
svgBtn.onclick=()=>download('krgby-k2-white-string-art-preview.svg',makeSVG(),'image/svg+xml');

pinCount.onchange=()=>{if(!running)makePins();};
canvasMode.onchange=()=>{if(!running)resetArt();};
canvasShape.onchange=()=>{if(!running){updateStageShape();resetArt();}};

let pointers=new Map();
let startScale=1;
let startX=0;
let startY=0;
let lastDist=0;
let startCenter={x:0,y:0};

function pointerPos(e){
  const r=stage.getBoundingClientRect();
  return{
    x:(e.clientX-r.left)*SIZE/r.width,
    y:(e.clientY-r.top)*SIZE/r.height
  };
}
function distance(a,b){return Math.hypot(a.x-b.x,a.y-b.y);}
function midpoint(a,b){return{x:(a.x+b.x)/2,y:(a.y+b.y)/2};}

stage.addEventListener('pointerdown',e=>{
  if(!img || running)return;
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
  if(!img || running || !pointers.has(e.pointerId))return;
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

function endPointer(e){pointers.delete(e.pointerId);}
stage.addEventListener('pointerup',endPointer);
stage.addEventListener('pointercancel',endPointer);
stage.addEventListener('pointerleave',endPointer);

stage.addEventListener('wheel',e=>{
  if(!img || running)return;
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

updateStageShape();
resetArt();
</script>
</body>
</html>
