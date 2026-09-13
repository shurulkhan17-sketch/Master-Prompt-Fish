<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>BLACKWINS AI VIDEO STUDIO</title>
<style>
  :root{
    --hull:#0d1117;
    --plate:#141b22;
    --plate-2:#1a232c;
    --line:#2a3742;
    --steel:#7fa8bf;
    --steel-dim:#4d6b7d;
    --amber:#e8a33d;
    --amber-dim:#7a5a26;
    --white:#eef3f6;
    --dim:#8fa1ac;
    --danger:#d9634a;
    --ok:#5fae7c;
    --mono: 'IBM Plex Mono', 'SFMono-Regular', Consolas, monospace;
    --sans: 'Inter', -apple-system, 'Segoe UI', sans-serif;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(1200px 600px at 15% -10%, rgba(127,168,191,0.08), transparent 60%),
      var(--hull);
    color:var(--white);
    font-family:var(--sans);
    min-height:100vh;
    line-height:1.5;
  }
  .rivet-bar{
    height:6px;
    background:repeating-linear-gradient(90deg, var(--amber) 0 18px, transparent 18px 40px);
    opacity:.55;
  }
  header.top{
    display:flex; align-items:center; justify-content:space-between;
    padding:20px 28px 16px 28px;
    border-bottom:1px solid var(--line);
  }
  .brand{
    display:flex; align-items:baseline; gap:12px;
  }
  .brand .mark{
    font-family:var(--mono);
    font-size:12px;
    letter-spacing:.16em;
    color:var(--amber);
    border:1px solid var(--amber-dim);
    padding:3px 8px;
    border-radius:3px;
  }
  .brand h1{
    font-size:19px;
    margin:0;
    font-weight:700;
    letter-spacing:.01em;
  }
  .brand h1 small{
    display:block;
    font-family:var(--mono);
    font-size:10.5px;
    color:var(--dim);
    font-weight:400;
    letter-spacing:.08em;
    margin-top:2px;
  }
  .content-counter{
    font-family:var(--mono);
    font-size:12px;
    color:var(--dim);
    text-align:right;
  }
  .content-counter b{ color:var(--steel); font-size:15px; }

  main{
    max-width:1040px;
    margin:0 auto;
    padding:28px 20px 80px 20px;
  }

  /* ---------- Mode select ---------- */
  .mode-grid{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:18px;
    margin-top:28px;
  }
  @media(max-width:720px){ .mode-grid{grid-template-columns:1fr;} }
  .mode-card{
    background:linear-gradient(180deg, var(--plate-2), var(--plate));
    border:1px solid var(--line);
    border-radius:6px;
    padding:26px 22px;
    cursor:pointer;
    transition:border-color .15s ease, transform .15s ease;
    position:relative;
    overflow:hidden;
  }
  .mode-card:hover{ border-color:var(--steel-dim); transform:translateY(-2px); }
  .mode-card .tag{
    font-family:var(--mono); font-size:11px; letter-spacing:.12em;
    color:var(--amber); margin-bottom:10px; display:block;
  }
  .mode-card h2{ margin:0 0 8px 0; font-size:22px; }
  .mode-card p{ margin:0; color:var(--dim); font-size:14.5px; }
  .mode-card .steps-preview{
    margin-top:16px; font-family:var(--mono); font-size:11px; color:var(--steel-dim);
  }

  /* ---------- Wizard shell ---------- */
  .wizard{
    display:grid;
    grid-template-columns:220px 1fr;
    gap:22px;
    margin-top:8px;
  }
  @media(max-width:800px){ .wizard{grid-template-columns:1fr;} }
  .railnav{
    border-right:1px solid var(--line);
    padding-right:16px;
  }
  @media(max-width:800px){ .railnav{border-right:none; border-bottom:1px solid var(--line); padding-right:0; padding-bottom:12px; display:flex; gap:8px; overflow-x:auto;} }
  .railnav .rstep{
    font-family:var(--mono);
    font-size:11.5px;
    color:var(--steel-dim);
    padding:8px 6px;
    border-left:2px solid transparent;
    white-space:nowrap;
  }
  .railnav .rstep.active{ color:var(--white); border-left-color:var(--amber); }
  .railnav .rstep.done{ color:var(--steel); }
  .railnav .rstep .n{ color:var(--dim); margin-right:6px; }

  .panel{
    background:var(--plate);
    border:1px solid var(--line);
    border-radius:6px;
    padding:22px;
    min-height:340px;
  }
  .panel h3{
    margin:0 0 4px 0;
    font-size:12px;
    font-family:var(--mono);
    letter-spacing:.12em;
    color:var(--amber);
  }
  .panel h2{
    margin:0 0 18px 0;
    font-size:20px;
  }

  .option-grid{
    display:grid;
    grid-template-columns:1fr 1fr;
    gap:10px;
  }
  @media(max-width:560px){ .option-grid{grid-template-columns:1fr;} }
  .opt{
    text-align:left;
    background:var(--plate-2);
    border:1px solid var(--line);
    border-radius:5px;
    padding:12px 13px;
    cursor:pointer;
    color:var(--white);
    font-family:var(--sans);
    font-size:14px;
    display:flex; flex-direction:column; gap:4px;
    position:relative;
  }
  .opt:hover{ border-color:var(--steel-dim); }
  .opt.selected{ border-color:var(--steel); background:rgba(127,168,191,0.08); }
  .opt .code{ font-family:var(--mono); font-size:10.5px; color:var(--steel-dim); }
  .opt .badge{
    position:absolute; top:10px; right:10px;
    font-family:var(--mono); font-size:9.5px; letter-spacing:.06em;
    padding:2px 6px; border-radius:3px;
  }
  .badge.recommended{ background:rgba(95,174,124,0.15); color:var(--ok); border:1px solid rgba(95,174,124,.4); }
  .badge.warning{ background:rgba(217,99,74,0.15); color:var(--danger); border:1px solid rgba(217,99,74,.4); }
  .badge.compatible{ background:rgba(127,168,191,0.12); color:var(--steel); border:1px solid rgba(127,168,191,.35); }

  .subgrid-label{
    font-family:var(--mono); font-size:11px; color:var(--dim); margin:18px 0 8px 0;
  }

  .warn-banner{
    background:rgba(217,99,74,0.1); border:1px solid rgba(217,99,74,.4);
    color:var(--danger); font-size:13px; padding:10px 12px; border-radius:5px; margin-top:12px;
  }
  .info-banner{
    background:rgba(127,168,191,0.08); border:1px solid var(--line);
    color:var(--dim); font-size:12.5px; padding:10px 12px; border-radius:5px; margin-top:12px;
  }

  form.custom-form{ display:grid; grid-template-columns:1fr 1fr; gap:10px 14px; }
  @media(max-width:560px){ form.custom-form{grid-template-columns:1fr;} }
  form.custom-form label{
    font-family:var(--mono); font-size:10.5px; color:var(--dim); letter-spacing:.05em;
    display:flex; flex-direction:column; gap:5px;
  }
  form.custom-form input, form.custom-form select, form.custom-form textarea{
    background:var(--hull); border:1px solid var(--line); color:var(--white);
    border-radius:4px; padding:8px 9px; font-family:var(--sans); font-size:13.5px;
  }
  form.custom-form textarea{ min-height:52px; resize:vertical; font-family:var(--sans);}
  form.custom-form .full{ grid-column:1/-1; }

  .row-inline{ display:flex; gap:10px; }
  .row-inline > div{ flex:1; }

  .actions{
    display:flex; justify-content:space-between; align-items:center;
    margin-top:22px; padding-top:16px; border-top:1px solid var(--line);
  }
  button{
    font-family:var(--sans); font-size:13.5px; font-weight:600;
    border-radius:5px; padding:10px 18px; cursor:pointer; border:1px solid transparent;
  }
  button.primary{ background:var(--amber); color:#231a08; border-color:var(--amber); }
  button.primary:disabled{ background:var(--amber-dim); color:#5b4a26; cursor:not-allowed; }
  button.ghost{ background:transparent; color:var(--dim); border-color:var(--line); }
  button.ghost:hover{ color:var(--white); border-color:var(--steel-dim); }
  button.small{ padding:6px 11px; font-size:12px; }

  /* ---------- Output ---------- */
  .output-wrap{ margin-top:10px; }
  .out-block{
    background:var(--plate); border:1px solid var(--line); border-radius:6px;
    margin-bottom:14px; overflow:hidden;
  }
  .out-block .out-head{
    display:flex; justify-content:space-between; align-items:center;
    padding:12px 16px; border-bottom:1px solid var(--line); background:var(--plate-2);
  }
  .out-head h4{ margin:0; font-family:var(--mono); font-size:12px; letter-spacing:.1em; color:var(--steel); }
  .out-body{ padding:15px 16px; white-space:pre-wrap; font-size:13.5px; color:var(--white); }
  .out-body.mono{ font-family:var(--mono); font-size:12.5px; line-height:1.65; }
  .copy-btn{
    font-family:var(--mono); font-size:10.5px; letter-spacing:.05em;
    background:transparent; border:1px solid var(--line); color:var(--dim);
    padding:5px 10px; border-radius:4px; cursor:pointer;
  }
  .copy-btn:hover{ color:var(--white); border-color:var(--steel-dim); }
  .copy-btn.copied{ color:var(--ok); border-color:var(--ok); }

  .setup-table{ width:100%; border-collapse:collapse; font-size:13px;}
  .setup-table td{ padding:6px 4px; border-bottom:1px solid var(--line); vertical-align:top; }
  .setup-table td:first-child{ color:var(--dim); font-family:var(--mono); font-size:11px; width:150px; }

  .hashtag-row{ margin-bottom:8px; }
  .hashtag-row b{ font-family:var(--mono); font-size:11px; color:var(--steel); display:block; margin-bottom:3px;}

  .qc-list{ list-style:none; padding:0; margin:0; font-size:13px; }
  .qc-list li{ padding:4px 0; display:flex; gap:8px; align-items:flex-start;}
  .qc-list li.fail{ color:var(--danger); }
  .qc-list li.pass{ color:var(--dim); }
  .qc-list li .mk{ font-family:var(--mono); }

  .history-item{
    display:flex; justify-content:space-between; align-items:center;
    padding:9px 12px; border-bottom:1px solid var(--line); font-size:13px;
  }
  .history-item:last-child{ border-bottom:none; }
  .history-item .hid{ font-family:var(--mono); color:var(--amber); margin-right:8px; }
  .history-item .hmeta{ color:var(--dim); font-size:11.5px; }

  a.linklike{ color:var(--steel); cursor:pointer; text-decoration:underline; }
</style>
</head>
<body>

<div class="rivet-bar"></div>
<header class="top">
  <div class="brand">
    <span class="mark">BWS</span>
    <h1>BLACKWINS AI VIDEO STUDIO<small>PROMPT ENGINE &middot; REALISM FIRST &middot; SAFETY ALWAYS &middot; DRAMA SECOND</small></h1>
  </div>
  <div class="content-counter">NEXT CONTENT<br><b id="counterDisplay">#1</b></div>
</header>

<main id="app"></main>

<script>
/* =========================================================
   DATA LIBRARIES
   ========================================================= */

const FISH_LIBRARY = {
  SHARK: ["Great White","Bull Shark","Tiger Shark","Hammerhead","Mako","Megalodon"],
  TUNA: ["Bluefin","Yellowfin","Bigeye"],
  MARLIN: ["Blue Marlin","Black Marlin","Striped Marlin"],
  ORCA: ["Giant Orca"],
  DOLPHIN: ["Giant Dolphin"],
  CATFISH: ["Giant Catfish"],
  STURGEON: ["Giant Sturgeon"],
  GROUPER: ["Giant Grouper"],
  SQUID: ["Giant Squid"],
  OTHER: ["Custom Category"]
};

const TOOLS = [
  {id:"TOOL-01", name:"Long Industrial Blade"},
  {id:"TOOL-02", name:"Long Industrial Axe"},
  {id:"TOOL-03", name:"Huge Industrial Cleaver"},
  {id:"TOOL-04", name:"Large Industrial Fish Saw"},
  {id:"TOOL-05", name:"Super-Large Fish Knife"},
  {id:"TOOL-06", name:"Heavy Industrial Butcher Blade"},
  {id:"TOOL-07", name:"Heavy Industrial Splitting Tool"},
  {id:"TOOL-08", name:"Custom Monster Tool", custom:true},
  {id:"TOOL-09", name:"Industrial Saw + Knife Combination"},
  {id:"TOOL-10", name:"Custom Tool", custom:true}
];

const WORKERS = [
  {id:"WORKER-01", name:"Alaska Seafood Worker"},
  {id:"WORKER-02", name:"Pacific Northwest Fish Cutter"},
  {id:"WORKER-03", name:"Veteran Fish Processor"},
  {id:"WORKER-04", name:"Young Professional Processor"},
  {id:"WORKER-05", name:"Female Seafood Processor"},
  {id:"WORKER-06", name:"Heavy-Build Industrial Worker"},
  {id:"WORKER-07", name:"Rugged American Fisherman"},
  {id:"WORKER-08", name:"Modern Factory Technician"},
  {id:"WORKER-09", name:"Old-School Cannery Worker"},
  {id:"WORKER-10", name:"Custom Worker", custom:true},
  {id:"WORKER-11", name:"Muscular Bald Seafood Processor"}
];

const INDOOR_LOCATIONS = [
  {id:"IND-01", name:"American Fish Processing Plant"},
  {id:"IND-02", name:"Alaska Seafood Facility"},
  {id:"IND-03", name:"Pacific Northwest Fish Cannery"},
  {id:"IND-04", name:"Dedicated Fish-Cutting Room"},
  {id:"IND-05", name:"Cold-Storage Processing Facility"},
  {id:"IND-06", name:"Large Tuna/Fish Processing Facility"},
  {id:"IND-07", name:"Shark Processing Facility"},
  {id:"IND-08", name:"Modern American Seafood Factory"},
  {id:"IND-09", name:"Old-School American Cannery"},
  {id:"IND-10", name:"Giant Industrial Seafood Warehouse"},
  {id:"IND-CUSTOM", name:"Custom Indoor Location", custom:true}
];

const OUTDOOR_LOCATIONS = [
  {id:"OUT-01", name:"Alaska Commercial Fishing Dock"},
  {id:"OUT-02", name:"Pacific Northwest Fishing Harbor"},
  {id:"OUT-03", name:"Industrial Seafood Pier"},
  {id:"OUT-04", name:"Commercial Fishing Vessel Deck"},
  {id:"OUT-05", name:"Remote Coastal Fish Landing Area"},
  {id:"OUT-06", name:"Outdoor Seafood Processing Yard"},
  {id:"OUT-07", name:"Large Harbor Loading Area"},
  {id:"OUT-08", name:"Cold Coastal Dock"},
  {id:"OUT-09", name:"Remote Fishing Camp"},
  {id:"OUT-CUSTOM", name:"Custom Outdoor Seafood Processing Area", custom:true}
];

const CUTTING_STYLES = [
  {id:"CUT-01", name:"Overhead Full Cross-Body Tebasan", desc:"Raise tool high, brief pause, one powerful downward strike, full cross-body cutting line, follow-through, result reveal."},
  {id:"CUT-02", name:"Heavy Industrial Swing", desc:"Wide lateral swing with full body torque and heavy follow-through."},
  {id:"CUT-03", name:"Heavy Vertical Strike", desc:"Straight vertical downward strike with braced stance."},
  {id:"CUT-04", name:"One Main Overhead Slash + Short Finish", desc:"Single overhead slash followed by a short controlled finishing motion."},
  {id:"CUT-05", name:"Close Heavy Cut", desc:"Tight-framed heavy cut emphasizing tool contact and immediate result."},
  {id:"CUT-06", name:"Worker-Centered Cut", desc:"Camera stays centered on the worker's stance and mechanics throughout the action."},
  {id:"CUT-07", name:"Long Tool Swing", desc:"Extended swing arc using a long-handled tool for leverage."},
  {id:"CUT-08", name:"Heavy Vertical Strike — Extreme", desc:"Maximum-force vertical strike with pronounced wind-up."},
  {id:"CUT-09", name:"Industrial Saw", desc:"Continuous back-and-forth sawing motion through the target area."},
  {id:"CUT-10", name:"Professional Processing", desc:"Calm, methodical, multi-motion professional breakdown sequence."},
  {id:"CUT-11", name:"Wide → Action → Close Result", desc:"Wide establishing framing into the action, ending on a close result reveal."},
  {id:"CUT-12", name:"Close Tool → Reveal Full Fish", desc:"Starts on the tool in close-up, then pulls back to reveal the full animal."},
  {id:"CUT-13", name:"Scale → Preparation → Action", desc:"Opens on a scale comparison, then preparation, then the main action."},
  {id:"CUT-14", name:"One-Take Spontaneous", desc:"Single continuous handheld take with a spontaneous, unrehearsed feel."},
  {id:"CUT-15", name:"Custom Mix", desc:"User-defined combination of movement beats.", custom:true}
];

const TOOL_COMPATIBILITY = {
  "CUT-01": ["TOOL-02","TOOL-03","TOOL-07","TOOL-01"],
  "CUT-02": ["TOOL-02","TOOL-03","TOOL-07","TOOL-06"],
  "CUT-03": ["TOOL-02","TOOL-03","TOOL-07"],
  "CUT-09": ["TOOL-04","TOOL-09"],
  "CUT-10": ["TOOL-05","TOOL-06","TOOL-04"]
};

const FLOWS = [
  {id:"FLOW-01", name:"Zoom Out → Worker Arrives → Preparation → Action → Finishing → Close-up"},
  {id:"FLOW-02", name:"Wide Establishing → Take Tool → Prepare → Action → Reveal"},
  {id:"FLOW-03", name:"Fish First → Person Enters → Tool → Action"},
  {id:"FLOW-04", name:"Person + Fish → Take Tool → Raise → Swing"},
  {id:"FLOW-05", name:"Tool Hook → Fish Reveal → Process"},
  {id:"FLOW-06", name:"From Behind Worker → Giant Animal → Process → Close-up"},
  {id:"FLOW-07", name:"Scale Comparison → Prepare → Full Action → Reveal"},
  {id:"FLOW-08", name:"Camera Approaches → Prepare → Action → Follow Result"},
  {id:"FLOW-09", name:"One-Take Viral Moment"},
  {id:"FLOW-10", name:"Dramatic Reveal → Build-up → Action → Detail Reveal"}
];

const DURATIONS = [
  {id:"DUR-10", seconds:10, label:"10 seconds"},
  {id:"DUR-15", seconds:15, label:"15 seconds"},
  {id:"DUR-30", seconds:30, label:"30 seconds"},
  {id:"DUR-CUSTOM", seconds:null, label:"Custom", custom:true}
];

const PHOTO_RATIOS = [
  {id:"9:16", label:"9:16 — Vertical"},
  {id:"4:5", label:"4:5 — Portrait"},
  {id:"1:1", label:"1:1 — Square"},
  {id:"16:9", label:"16:9 — Landscape"},
  {id:"CUSTOM", label:"Custom Ratio", custom:true}
];

const VIDEO_FORMATS = [
  {id:"FMT-9:16", label:"9:16 — Vertical (Reels/TikTok/Shorts)"},
  {id:"FMT-16:9", label:"16:9 — Landscape"},
  {id:"FMT-1:1", label:"1:1 — Square"},
  {id:"FMT-CUSTOM", label:"Custom Format", custom:true}
];

const CUSTOM_FISH_FIELDS = [
  ["category","Category"],["species","Species / Name"],["weight","Weight"],
  ["weightUnit","Weight Unit"],["length","Length"],["lengthUnit","Length Unit"],
  ["bodyShape","Body Shape"],["bodyThickness","Body Thickness"],["bodyColor","Body Color"],
  ["skinTexture","Skin Texture"],["headShape","Head Shape"],["mouthShape","Mouth Shape"],
  ["finCharacteristics","Fin Characteristics"],["tailCharacteristics","Tail Characteristics"],
  ["eyeCharacteristics","Eye Characteristics"],["markings","Markings"],
  ["overallAppearance","Overall Appearance"],["specialCharacteristics","Special Characteristics"],
  ["notes","Additional Notes"]
];

const CUSTOM_WORKER_FIELDS = [
  ["gender","Gender"],["age","Age"],["height","Height"],["bodyType","Body Type"],
  ["skinTone","Skin Tone"],["hairLength","Hair Length"],["hairColor","Hair Color"],
  ["facialAppearance","Facial Appearance"],["facialHair","Beard / Mustache"],
  ["clothing","Clothing"],["ppe","PPE"],["accessories","Accessories"],
  ["expression","Expression"],["workingBehavior","Working Behavior"],
  ["bodyLanguage","Body Language"],["pose","Pose"],["build","Build / Physical Strength"],
  ["notes","Additional Details"]
];

const CUSTOM_INDOOR_FIELDS = [
  ["name","Location Name"],["buildingType","Building Type"],["room","Room / Area"],
  ["floor","Floor"],["walls","Walls"],["ceiling","Ceiling"],["lighting","Lighting"],
  ["equipment","Processing Equipment"],["tables","Work Tables"],["background","Background Elements"],
  ["atmosphere","Temperature / Atmosphere"],["cleanliness","Cleanliness"],
  ["cameraAccess","Camera Access"],["notes","Additional Notes"]
];

const CUSTOM_OUTDOOR_FIELDS = [
  ["name","Location Name"],["envType","Environment Type"],["geoFeel","Geographic Feel"],
  ["ground","Ground / Surface"],["weather","Weather"],["timeOfDay","Time of Day"],
  ["lighting","Lighting"],["background","Background"],["structures","Structures"],
  ["equipment","Equipment"],["processingArea","Processing Area"],
  ["envConditions","Environmental Conditions"],["cameraAccess","Camera Access"],["notes","Additional Notes"]
];

/* =========================================================
   STATE
   ========================================================= */

let contentNumber = 1;
const sessionHistory = [];

function freshState(mode){
  return {
    mode: mode,
    step: 1,
    format: null,
    fish: null,           // {category, species, isCustom, custom:{...}}
    tool: null,           // {id, custom:{name,notes}}
    toolOverride: false,
    worker: null,         // {id, custom:{...}}
    locationType: null,   // INDOOR | OUTDOOR
    location: null,       // {id, custom:{...}}
    cuttingStyle: null,
    flow: null,
    duration: null,       // {id, seconds}
    ratio: null,
    photoRatio: null
  };
}

let state = freshState('video');
let screen = 'mode-select'; // 'mode-select' | 'wizard' | 'output'
let lastOutput = null;

/* =========================================================
   HELPERS
   ========================================================= */

function el(html){
  const t = document.createElement('template');
  t.innerHTML = html.trim();
  return t.content.firstChild;
}
function esc(s){
  return (s===undefined||s===null||s==='') ? '' : String(s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
}
function findTool(id){ return TOOLS.find(t=>t.id===id); }
function findWorker(id){ return WORKERS.find(w=>w.id===id); }
function findLocation(id, type){
  const lib = type === 'INDOOR' ? INDOOR_LOCATIONS : OUTDOOR_LOCATIONS;
  return lib.find(l=>l.id===id);
}
function findCut(id){ return CUTTING_STYLES.find(c=>c.id===id); }
function findFlow(id){ return FLOWS.find(f=>f.id===id); }

function fishLabel(fish){
  if(!fish) return '';
  if(fish.isCustom){
    const c = fish.custom;
    return `${c.species || 'Custom Fish'} (${c.category || 'Custom Category'})`;
  }
  return `${fish.species} (${fish.category})`;
}
function fishWeightLength(fish){
  if(!fish) return {weight:'', length:''};
  if(fish.isCustom){
    return {
      weight: `${fish.custom.weight || '?'} ${fish.custom.weightUnit || ''}`.trim(),
      length: `${fish.custom.length || '?'} ${fish.custom.lengthUnit || ''}`.trim()
    };
  }
  return { weight: `${fish.weight} ${fish.weightUnit}`, length: `${fish.length} ${fish.lengthUnit}` };
}
function toolLabel(tool){
  if(!tool) return '';
  const base = findTool(tool.id);
  if(base && base.custom && tool.custom && tool.custom.name){
    return `${tool.custom.name} (${tool.id})`;
  }
  return base ? `${base.name} (${base.id})` : tool.id;
}
function workerLabel(worker){
  if(!worker) return '';
  const base = findWorker(worker.id);
  if(base && base.custom && worker.custom){
    const c = worker.custom;
    return `Custom Worker — ${[c.gender,c.age,c.bodyType].filter(Boolean).join(', ')}`;
  }
  return base ? base.name : worker.id;
}
function locationLabel(loc, type){
  if(!loc) return '';
  const base = findLocation(loc.id, type);
  if(base && base.custom && loc.custom){
    return loc.custom.name || 'Custom Location';
  }
  return base ? base.name : loc.id;
}

/* =========================================================
   RENDER: ROOT
   ========================================================= */

function render(){
  const app = document.getElementById('app');
  app.innerHTML = '';
  document.getElementById('counterDisplay').textContent = '#' + contentNumber;
  if(screen === 'mode-select'){
    app.appendChild(renderModeSelect());
  } else if(screen === 'wizard'){
    app.appendChild(renderWizard());
  } else if(screen === 'output'){
    app.appendChild(renderOutputScreen());
  }
}

function renderModeSelect(){
  const wrap = el(`<div>
    <p style="color:var(--dim); font-size:14px; max-width:640px;">Select a studio mode to begin. Each mode walks through one configuration step at a time, then produces a complete, structured prompt package ready to paste into your generation tool.</p>
    <div class="mode-grid">
      <div class="mode-card" id="pickVideo">
        <span class="tag">MODE A</span>
        <h2>Video Studio</h2>
        <p>Full 11-step workflow: format, animal, cutting style, tools, worker, location, flow, duration, ratio.</p>
        <div class="steps-preview">FORMAT → FISH → CUT → TOOLS → WORKER → LOCATION → FLOW → DURATION → RATIO</div>
      </div>
      <div class="mode-card" id="pickImage">
        <span class="tag">MODE B</span>
        <h2>Image Studio</h2>
        <p>Simplified 4-step workflow for a single realistic documentary-style photograph.</p>
        <div class="steps-preview">FISH → TOOLS &amp; MACHINES → LOCATION → PHOTO RATIO</div>
      </div>
    </div>
    ${sessionHistory.length ? renderHistoryPreview() : ''}
  </div>`);
  wrap.querySelector('#pickVideo').onclick = () => { state = freshState('video'); screen='wizard'; render(); };
  wrap.querySelector('#pickImage').onclick = () => { state = freshState('image'); screen='wizard'; render(); };
  return wrap;
}

function renderHistoryPreview(){
  const items = sessionHistory.slice().reverse().slice(0,6).map(h => `
    <div class="history-item">
      <div><span class="hid">#${h.number}</span>${esc(h.subject)}</div>
      <div class="hmeta">${h.mode.toUpperCase()}</div>
    </div>`).join('');
  return `<div class="out-block" style="margin-top:26px;">
    <div class="out-head"><h4>SESSION HISTORY</h4></div>
    <div>${items}</div>
  </div>`;
}

/* =========================================================
   WIZARD STEP DEFINITIONS
   ========================================================= */

const VIDEO_STEPS = [
  "FORMAT","FISH / ANIMAL","CUTTING STYLE","TOOLS & MACHINES","WORKER",
  "LOCATION TYPE","LOCATION","FLOW","DURATION","RATIO","GENERATE"
];
const IMAGE_STEPS = ["FISH","TOOLS & MACHINES","LOCATION","PHOTO RATIO"];

function currentSteps(){ return state.mode === 'video' ? VIDEO_STEPS : IMAGE_STEPS; }
function totalMainSteps(){ return state.mode === 'video' ? 11 : 4; }

function renderWizard(){
  const wrap = el(`<div>
    <div class="wizard">
      <div class="railnav" id="railnav"></div>
      <div class="panel" id="panelContent"></div>
    </div>
  </div>`);

  const rail = wrap.querySelector('#railnav');
  currentSteps().forEach((label, idx) => {
    const n = idx+1;
    const cls = n === state.step ? 'active' : (n < state.step ? 'done' : '');
    rail.appendChild(el(`<div class="rstep ${cls}"><span class="n">${String(n).padStart(2,'0')}</span>${label}</div>`));
  });

  const panel = wrap.querySelector('#panelContent');
  panel.appendChild(renderStepBody());
  return wrap;
}

function stepFrame(kicker, title, bodyNode, opts){
  opts = opts || {};
  const frame = el(`<div>
    <h3>${kicker}</h3>
    <h2>${title}</h2>
    <div id="stepBody"></div>
    <div class="actions">
      <button class="ghost" id="btnBack">${state.step===1 ? 'Change Mode' : '← Back'}</button>
      <button class="primary" id="btnNext" ${opts.disableNext ? 'disabled' : ''}>${opts.nextLabel || 'Continue →'}</button>
    </div>
  </div>`);
  frame.querySelector('#stepBody').appendChild(bodyNode);
  frame.querySelector('#btnBack').onclick = () => {
    if(state.step === 1){ screen = 'mode-select'; }
    else { state.step -= 1; }
    render();
  };
  if(!opts.disableNext){
    frame.querySelector('#btnNext').onclick = opts.onNext || (() => { state.step += 1; render(); });
  }
  return frame;
}

function renderStepBody(){
  if(state.mode === 'video'){
    switch(state.step){
      case 1: return stepFormat();
      case 2: return stepFish();
      case 3: return stepCuttingStyle();
      case 4: return stepTools();
      case 5: return stepWorker();
      case 6: return stepLocationType();
      case 7: return stepLocation();
      case 8: return stepFlow();
      case 9: return stepDuration();
      case 10: return stepRatio();
      case 11: return stepGenerate();
    }
  } else {
    switch(state.step){
      case 1: return stepFish();
      case 2: return stepTools();
      case 3: return stepLocation();
      case 4: return stepPhotoRatio();
    }
  }
}

/* -------- STEP: FORMAT (video only) -------- */
function stepFormat(){
  const body = el(`<div><div class="option-grid" id="fmtGrid"></div></div>`);
  const grid = body.querySelector('#fmtGrid');
  VIDEO_FORMATS.forEach(f => {
    const selected = state.format && state.format.id === f.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}"><div>${esc(f.label)}</div></button>`);
    card.onclick = () => {
      if(f.custom){
        const custom = prompt("Describe the custom format (e.g. '2.35:1 cinematic'):", state.format?.customLabel || '');
        if(custom === null) return;
        state.format = {id:f.id, customLabel: custom};
      } else {
        state.format = {id:f.id, label:f.label};
      }
      render();
    };
    grid.appendChild(card);
  });
  return stepFrame('STEP 01', 'Format', body, { disableNext: !state.format, onNext: ()=>{state.step++; render();} });
}

/* -------- STEP: FISH (shared) -------- */
function stepFish(){
  const kicker = state.mode==='video' ? 'STEP 02' : 'STEP 01';
  const body = el(`<div></div>`);

  if(!state.fish || state.fish.pickingCategory){
    const grid = el(`<div class="option-grid"></div>`);
    Object.keys(FISH_LIBRARY).forEach(cat => {
      const card = el(`<button type="button" class="opt"><div>${esc(cat)}</div><div class="code">${FISH_LIBRARY[cat].length} species</div></button>`);
      card.onclick = () => { state.fish = {category:cat, pickingSpecies:true}; render(); };
      grid.appendChild(card);
    });
    body.appendChild(el(`<div class="subgrid-label">SELECT FISH CATEGORY</div>`));
    body.appendChild(grid);
  } else if(state.fish.pickingSpecies){
    const cat = state.fish.category;
    body.appendChild(el(`<div class="subgrid-label">CATEGORY: ${esc(cat)} — SELECT SPECIES</div>`));
    const grid = el(`<div class="option-grid"></div>`);
    FISH_LIBRARY[cat].forEach(sp => {
      const card = el(`<button type="button" class="opt"><div>${esc(sp)}</div></button>`);
      card.onclick = () => { openFishDimensions(cat, sp); };
      grid.appendChild(card);
    });
    const customCard = el(`<button type="button" class="opt"><div>Custom ${esc(cat)}</div><div class="code">Full custom fish form</div></button>`);
    customCard.onclick = () => { state.fish = {isCustom:true, custom:{category:cat}, editingCustom:true}; render(); };
    grid.appendChild(customCard);
    body.appendChild(grid);
    const back = el(`<button class="ghost small" style="margin-top:14px;">← Choose different category</button>`);
    back.onclick = () => { state.fish = {pickingCategory:true}; render(); };
    body.appendChild(back);
  } else if(state.fish.editingCustom){
    body.appendChild(renderCustomFishForm());
  } else {
    // fish fully selected — summary + edit option
    body.appendChild(renderFishSummary());
  }

  return stepFrame(kicker, 'Fish / Animal', body, {
    disableNext: !(state.fish && !state.fish.pickingCategory && !state.fish.pickingSpecies && !state.fish.editingCustom),
    onNext: () => { state.step++; render(); }
  });
}

function openFishDimensions(category, species){
  const weight = prompt(`Weight for ${species} (number):`, "800");
  if(weight === null) return;
  const weightUnit = prompt("Weight unit (lbs / kg):", "lbs") || "lbs";
  const length = prompt(`Length for ${species} (number):`, "18");
  if(length === null) return;
  const lengthUnit = prompt("Length unit (ft / m):", "ft") || "ft";
  state.fish = { category, species, weight, weightUnit, length, lengthUnit };
  render();
}

function renderFishSummary(){
  const f = state.fish;
  const wl = fishWeightLength(f);
  const box = el(`<div>
    <div class="info-banner">
      <b>${esc(fishLabel(f))}</b><br>
      Weight: ${esc(wl.weight)} &middot; Length: ${esc(wl.length)}
    </div>
    <button class="ghost small" style="margin-top:12px;">Edit fish selection</button>
  </div>`);
  box.querySelector('button').onclick = () => { state.fish = {pickingCategory:true}; render(); };
  return box;
}

function renderCustomFishForm(){
  const c = state.fish.custom || {};
  const form = el(`<form class="custom-form"></form>`);
  CUSTOM_FISH_FIELDS.forEach(([key,label]) => {
    const isArea = key==='notes' || key==='overallAppearance' || key==='specialCharacteristics';
    const field = el(`<label>${esc(label)}${isArea
      ? `<textarea data-key="${key}">${esc(c[key]||'')}</textarea>`
      : `<input data-key="${key}" value="${esc(c[key]||'')}" ${key==='category'?'':''}>`
    }</label>`);
    if(isArea) field.classList.add('full');
    form.appendChild(field);
  });
  const wrap = el(`<div></div>`);
  wrap.appendChild(el(`<div class="subgrid-label">CUSTOM FISH — ALL FIELDS FEED THE FINAL PROMPT</div>`));
  wrap.appendChild(form);
  const saveRow = el(`<div style="margin-top:14px; display:flex; gap:10px;">
    <button type="button" class="primary small" id="saveCustomFish">Save Custom Fish</button>
    <button type="button" class="ghost small" id="cancelCustomFish">Cancel</button>
  </div>`);
  wrap.appendChild(saveRow);
  saveRow.querySelector('#saveCustomFish').onclick = () => {
    const data = {};
    form.querySelectorAll('[data-key]').forEach(inp => { data[inp.dataset.key] = inp.value.trim(); });
    if(!data.category) data.category = c.category || 'Custom Category';
    state.fish = { isCustom:true, custom:data };
    render();
  };
  saveRow.querySelector('#cancelCustomFish').onclick = () => { state.fish = {pickingCategory:true}; render(); };
  return wrap;
}

/* -------- STEP: CUTTING STYLE (video only) -------- */
function stepCuttingStyle(){
  const body = el(`<div class="option-grid" id="cutGrid"></div>`);
  const grid = body.querySelector('#cutGrid') ? body : body; // body itself is the grid
  CUTTING_STYLES.forEach(c => {
    const selected = state.cuttingStyle === c.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}">
      <div class="code">${c.id}</div>
      <div>${esc(c.name)}</div>
    </button>`);
    card.title = c.desc;
    card.onclick = () => {
      state.cuttingStyle = c.id;
      // If tool already chosen and incompatible, don't silently clear — leave as is (warning shown later)
      render();
    };
    body.appendChild(card);
  });
  return stepFrame('STEP 03', 'Cutting Style', body, { disableNext: !state.cuttingStyle });
}

/* -------- STEP: TOOLS & MACHINES (shared) -------- */
function stepTools(){
  const kicker = state.mode==='video' ? 'STEP 04' : 'STEP 02';
  const wrap = el(`<div></div>`);
  const compat = state.mode==='video' && state.cuttingStyle ? TOOL_COMPATIBILITY[state.cuttingStyle] : null;

  const grid = el(`<div class="option-grid"></div>`);
  TOOLS.forEach(t => {
    const selected = state.tool && state.tool.id === t.id;
    let badge = '';
    if(compat){
      if(compat.includes(t.id)) badge = '<span class="badge recommended">RECOMMENDED</span>';
      else if(!t.custom) badge = '<span class="badge warning">WARNING</span>';
    }
    const card = el(`<button type="button" class="opt ${selected?'selected':''}">
      ${badge}<div class="code">${t.id}</div><div>${esc(t.name)}</div>
    </button>`);
    card.onclick = () => {
      if(t.custom){
        const name = prompt("Custom tool name / description:", state.tool?.custom?.name || '');
        if(name === null) return;
        state.tool = {id:t.id, custom:{name}};
      } else {
        state.tool = {id:t.id};
      }
      state.toolOverride = false;
      render();
    };
    grid.appendChild(card);
  });
  wrap.appendChild(el(`<div class="subgrid-label">${state.mode==='video' ? 'TOOL IS INDEPENDENT FROM CUTTING STYLE — RECOMMENDATIONS SHOWN BASED ON SELECTED STYLE' : 'TOOLS ARE VISUAL PROPS — SHOWN AT REST, NOT MID-ACTION'}</div>`));
  wrap.appendChild(grid);

  if(compat && state.tool && !compat.includes(state.tool.id) && !findTool(state.tool.id).custom){
    const warn = el(`<div class="warn-banner">
      WARNING: "${esc(toolLabel(state.tool))}" is not normally compatible with cutting style ${state.cuttingStyle} (${findCut(state.cuttingStyle).name}).
      <label style="display:block; margin-top:8px; font-family:var(--sans); font-size:12.5px; cursor:pointer;">
        <input type="checkbox" id="overrideBox" ${state.toolOverride?'checked':''}> I understand — keep this tool anyway
      </label>
    </div>`);
    warn.querySelector('#overrideBox').onchange = (e) => { state.toolOverride = e.target.checked; render(); };
    wrap.appendChild(warn);
  }

  const blocked = compat && state.tool && !compat.includes(state.tool.id) && !findTool(state.tool.id).custom && !state.toolOverride;

  return stepFrame(kicker, 'Tools & Machines', wrap, {
    disableNext: !state.tool || blocked,
    onNext: () => { state.step++; render(); }
  });
}

/* -------- STEP: WORKER (video only) -------- */
function stepWorker(){
  const wrap = el(`<div></div>`);
  if(state.worker && state.worker.editingCustom){
    wrap.appendChild(renderCustomWorkerForm());
    return stepFrame('STEP 05','Worker', wrap, {disableNext:true});
  }
  const grid = el(`<div class="option-grid"></div>`);
  WORKERS.forEach(w => {
    const selected = state.worker && state.worker.id === w.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}">
      <div class="code">${w.id}</div><div>${esc(w.name)}</div>
    </button>`);
    card.onclick = () => {
      if(w.custom){ state.worker = {id:w.id, custom:{}, editingCustom:true}; }
      else { state.worker = {id:w.id}; }
      render();
    };
    grid.appendChild(card);
  });
  wrap.appendChild(grid);
  if(state.worker && !state.worker.editingCustom && findWorker(state.worker.id).custom){
    const editBtn = el(`<button class="ghost small" style="margin-top:12px;">Edit custom worker details</button>`);
    editBtn.onclick = () => { state.worker.editingCustom = true; render(); };
    wrap.appendChild(editBtn);
  }
  return stepFrame('STEP 05', 'Worker', wrap, { disableNext: !state.worker || state.worker.editingCustom });
}

function renderCustomWorkerForm(){
  const c = state.worker.custom || {};
  const form = el(`<form class="custom-form"></form>`);
  CUSTOM_WORKER_FIELDS.forEach(([key,label]) => {
    const isArea = key==='workingBehavior' || key==='notes';
    const field = el(`<label>${esc(label)}${isArea
      ? `<textarea data-key="${key}">${esc(c[key]||'')}</textarea>`
      : `<input data-key="${key}" value="${esc(c[key]||'')}">`
    }</label>`);
    if(isArea) field.classList.add('full');
    form.appendChild(field);
  });
  const wrap = el(`<div></div>`);
  wrap.appendChild(el(`<div class="subgrid-label">CUSTOM WORKER — WORKER MUST BE AN ADULT, IN SAFE STANCE, WITH REALISTIC PPE. BLANK FIELDS ARE NOT INVENTED.</div>`));
  wrap.appendChild(form);
  const row = el(`<div style="margin-top:14px; display:flex; gap:10px;">
    <button type="button" class="primary small" id="saveWorker">Save Worker</button>
    <button type="button" class="ghost small" id="cancelWorker">Cancel</button>
  </div>`);
  wrap.appendChild(row);
  row.querySelector('#saveWorker').onclick = () => {
    const data = {};
    form.querySelectorAll('[data-key]').forEach(inp => { data[inp.dataset.key] = inp.value.trim(); });
    state.worker = { id:'WORKER-10', custom:data };
    render();
  };
  row.querySelector('#cancelWorker').onclick = () => {
    if(Object.keys(c).length){ state.worker = {id:'WORKER-10', custom:c}; }
    else { state.worker = null; }
    render();
  };
  return wrap;
}

/* -------- STEP: LOCATION TYPE (video only) -------- */
function stepLocationType(){
  const body = el(`<div class="option-grid">
    <button type="button" class="opt ${state.locationType==='INDOOR'?'selected':''}"><div>Indoor</div><div class="code">Processing plant, factory, cannery</div></button>
    <button type="button" class="opt ${state.locationType==='OUTDOOR'?'selected':''}"><div>Outdoor</div><div class="code">Dock, harbor, deck, coastal yard</div></button>
  </div>`);
  const [indBtn, outBtn] = body.querySelectorAll('.opt');
  indBtn.onclick = () => { state.locationType='INDOOR'; state.location=null; render(); };
  outBtn.onclick = () => { state.locationType='OUTDOOR'; state.location=null; render(); };
  return stepFrame('STEP 06', 'Location Type', body, { disableNext: !state.locationType });
}

/* -------- STEP: LOCATION (shared) -------- */
function stepLocation(){
  const kicker = state.mode==='video' ? 'STEP 07' : 'STEP 03';
  const wrap = el(`<div></div>`);

  if(state.mode === 'image' && !state.locationType){
    const pick = el(`<div class="option-grid">
      <button type="button" class="opt"><div>Indoor</div></button>
      <button type="button" class="opt"><div>Outdoor</div></button>
    </div>`);
    const [i,o] = pick.querySelectorAll('.opt');
    i.onclick = () => { state.locationType='INDOOR'; render(); };
    o.onclick = () => { state.locationType='OUTDOOR'; render(); };
    wrap.appendChild(el(`<div class="subgrid-label">CHOOSE LOCATION TYPE</div>`));
    wrap.appendChild(pick);
    return stepFrame(kicker, 'Location', wrap, {disableNext:true});
  }

  if(state.location && state.location.editingCustom){
    wrap.appendChild(renderCustomLocationForm());
    return stepFrame(kicker, 'Location', wrap, {disableNext:true});
  }

  const lib = state.locationType === 'INDOOR' ? INDOOR_LOCATIONS : OUTDOOR_LOCATIONS;
  wrap.appendChild(el(`<div class="subgrid-label">${state.locationType} LOCATIONS</div>`));
  const grid = el(`<div class="option-grid"></div>`);
  lib.forEach(loc => {
    const selected = state.location && state.location.id === loc.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}"><div class="code">${loc.id}</div><div>${esc(loc.name)}</div></button>`);
    card.onclick = () => {
      if(loc.custom){ state.location = {id:loc.id, custom:{}, editingCustom:true}; }
      else { state.location = {id:loc.id}; }
      render();
    };
    grid.appendChild(card);
  });
  wrap.appendChild(grid);
  const changeType = el(`<button class="ghost small" style="margin-top:12px;">← Change location type</button>`);
  changeType.onclick = () => { state.locationType=null; state.location=null; render(); };
  wrap.appendChild(changeType);

  return stepFrame(kicker, 'Location', wrap, { disableNext: !state.location });
}

function renderCustomLocationForm(){
  const isIndoor = state.locationType === 'INDOOR';
  const fields = isIndoor ? CUSTOM_INDOOR_FIELDS : CUSTOM_OUTDOOR_FIELDS;
  const c = state.location.custom || {};
  const form = el(`<form class="custom-form"></form>`);
  fields.forEach(([key,label]) => {
    const isArea = key==='notes' || key==='background';
    const field = el(`<label>${esc(label)}${isArea
      ? `<textarea data-key="${key}">${esc(c[key]||'')}</textarea>`
      : `<input data-key="${key}" value="${esc(c[key]||'')}">`
    }</label>`);
    if(isArea) field.classList.add('full');
    form.appendChild(field);
  });
  const wrap = el(`<div></div>`);
  wrap.appendChild(el(`<div class="subgrid-label">CUSTOM ${isIndoor?'INDOOR':'OUTDOOR'} LOCATION</div>`));
  wrap.appendChild(form);
  const row = el(`<div style="margin-top:14px; display:flex; gap:10px;">
    <button type="button" class="primary small" id="saveLoc">Save Location</button>
    <button type="button" class="ghost small" id="cancelLoc">Cancel</button>
  </div>`);
  wrap.appendChild(row);
  row.querySelector('#saveLoc').onclick = () => {
    const data = {};
    form.querySelectorAll('[data-key]').forEach(inp => { data[inp.dataset.key] = inp.value.trim(); });
    state.location = { id: isIndoor?'IND-CUSTOM':'OUT-CUSTOM', custom:data };
    render();
  };
  row.querySelector('#cancelLoc').onclick = () => {
    if(Object.keys(c).length){ state.location = {id:isIndoor?'IND-CUSTOM':'OUT-CUSTOM', custom:c}; }
    else { state.location = null; }
    render();
  };
  return wrap;
}

/* -------- STEP: FLOW (video only) -------- */
function stepFlow(){
  const body = el(`<div class="option-grid"></div>`);
  FLOWS.forEach(f => {
    const selected = state.flow === f.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}"><div class="code">${f.id}</div><div>${esc(f.name)}</div></button>`);
    card.onclick = () => { state.flow = f.id; render(); };
    body.appendChild(card);
  });
  return stepFrame('STEP 08', 'Flow', body, { disableNext: !state.flow });
}

/* -------- STEP: DURATION (video only) -------- */
function stepDuration(){
  const wrap = el(`<div></div>`);
  const grid = el(`<div class="option-grid"></div>`);
  DURATIONS.forEach(d => {
    const selected = state.duration && state.duration.id === d.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}"><div>${esc(d.label)}</div></button>`);
    card.onclick = () => {
      if(d.custom){
        const secs = prompt("Custom duration in seconds:", state.duration?.seconds || '20');
        if(secs===null) return;
        state.duration = {id:d.id, seconds:parseInt(secs,10)||20, label:`${secs} seconds (custom)`};
      } else {
        state.duration = {id:d.id, seconds:d.seconds, label:d.label};
      }
      render();
    };
    grid.appendChild(card);
  });
  wrap.appendChild(grid);
  return stepFrame('STEP 09', 'Duration', wrap, { disableNext: !state.duration });
}

/* -------- STEP: RATIO (video only) -------- */
function stepRatio(){
  const body = el(`<div class="option-grid"></div>`);
  const options = [{id:'9:16',label:'9:16 — Vertical'},{id:'4:5',label:'4:5 — Portrait'},{id:'1:1',label:'1:1 — Square'},{id:'16:9',label:'16:9 — Landscape'},{id:'CUSTOM',label:'Custom Ratio',custom:true}];
  options.forEach(r => {
    const selected = state.ratio && state.ratio.id === r.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}"><div>${esc(r.label)}</div></button>`);
    card.onclick = () => {
      if(r.custom){
        const custom = prompt("Custom ratio (e.g. 2:3):", state.ratio?.custom || '');
        if(custom===null) return;
        state.ratio = {id:'CUSTOM', label:`${custom} (custom)`};
      } else {
        state.ratio = {id:r.id, label:r.label};
      }
      render();
    };
    body.appendChild(card);
  });
  return stepFrame('STEP 10', 'Ratio', body, { disableNext: !state.ratio });
}

/* -------- STEP: PHOTO RATIO (image only) -------- */
function stepPhotoRatio(){
  const body = el(`<div class="option-grid"></div>`);
  PHOTO_RATIOS.forEach(r => {
    const selected = state.photoRatio && state.photoRatio.id === r.id;
    const card = el(`<button type="button" class="opt ${selected?'selected':''}"><div>${esc(r.label)}</div></button>`);
    card.onclick = () => {
      if(r.custom){
        const custom = prompt("Custom photo ratio:", state.photoRatio?.custom || '');
        if(custom===null) return;
        state.photoRatio = {id:'CUSTOM', label:`${custom} (custom)`};
      } else {
        state.photoRatio = {id:r.id, label:r.label};
      }
      render();
    };
    body.appendChild(card);
  });
  return stepFrame('STEP 04', 'Photo Ratio', body, {
    disableNext: !state.photoRatio,
    nextLabel: 'Review & Generate →',
    onNext: () => { screen='output'; lastOutput = buildOutput(); render(); }
  });
}

/* -------- STEP: GENERATE (video final step) -------- */
function stepGenerate(){
  const qc = runQualityControl();
  const failed = qc.filter(q => !q.pass);
  const body = el(`<div>
    <div class="subgrid-label">PREFLIGHT QUALITY CONTROL</div>
    <ul class="qc-list">
      ${qc.map(q => `<li class="${q.pass?'pass':'fail'}"><span class="mk">${q.pass?'[OK]':'[MISSING]'}</span> ${esc(q.label)}</li>`).join('')}
    </ul>
    ${failed.length ? `<div class="warn-banner">Resolve the missing fields above before generating.</div>` : `<div class="info-banner">All checks passed. Ready to generate the final prompt package.</div>`}
  </div>`);
  return stepFrame('STEP 11', 'Generate', body, {
    disableNext: failed.length > 0,
    nextLabel: 'Generate Content →',
    onNext: () => { screen='output'; lastOutput = buildOutput(); render(); }
  });
}

/* =========================================================
   QUALITY CONTROL
   ========================================================= */
function runQualityControl(){
  const checks = [];
  const push = (pass, label) => checks.push({pass:!!pass, label});
  push(!!state.fish, "Fish selected");
  push(state.fish && (state.fish.isCustom ? state.fish.custom.species : state.fish.species), "Species specified");
  const wl = fishWeightLength(state.fish);
  push(wl.weight && wl.weight.trim() && !wl.weight.includes('?'), "Weight specified");
  push(wl.length && wl.length.trim() && !wl.length.includes('?'), "Length specified");
  push(!!state.tool, "Tool selected");
  push(!!state.location, "Location selected");
  if(state.mode === 'video'){
    push(!!state.worker, "Worker selected");
    push(!!state.cuttingStyle, "Cutting style selected");
    push(!!state.flow, "Flow selected");
    push(!!state.duration, "Duration selected");
    push(!!state.ratio, "Ratio selected");
  } else {
    push(!!state.photoRatio, "Photo ratio selected");
  }
  push(true, "Camera rules present (auto-included)");
  push(true, "Physical continuity rules present (auto-included)");
  push(true, "Safety rules present (auto-included)");
  if(state.mode === 'video') push(true, "Maximum Result Reveal present (auto-included for processing video)");
  push(true, "Negative prompt will be generated");
  return checks;
}

/* =========================================================
   PROMPT BUILDERS
   ========================================================= */

function fishPromptDescriptor(fish){
  const wl = fishWeightLength(fish);
  if(fish.isCustom){
    const c = fish.custom;
    const traits = [c.bodyShape,c.bodyThickness,c.bodyColor,c.skinTexture,c.headShape,c.mouthShape,
      c.finCharacteristics,c.tailCharacteristics,c.eyeCharacteristics,c.markings,c.overallAppearance,
      c.specialCharacteristics].filter(Boolean);
    return `an enormous ${c.species || 'custom sea creature'} (${c.category || 'custom category'}), weighing ${wl.weight}, measuring ${wl.length} in length${traits.length ? ', with ' + traits.join(', ') : ''}${c.notes ? '. Additional notes: ' + c.notes : ''}`;
  }
  return `an enormous ${fish.species} ${fish.category.toLowerCase()}, weighing ${wl.weight}, measuring ${wl.length} in length`;
}

function toolPromptDescriptor(tool, forVideo){
  const base = findTool(tool.id);
  const name = (base.custom && tool.custom && tool.custom.name) ? tool.custom.name : base.name;
  return forVideo
    ? `a ${name.toLowerCase()} (${tool.id}), sized appropriately for heavy industrial processing work`
    : `a ${name.toLowerCase()} (${tool.id}), resting or held naturally beside the work surface as a realistic industrial prop, scaled appropriately to the animal`;
}

function workerPromptDescriptor(worker){
  const base = findWorker(worker.id);
  if(base.custom && worker.custom){
    const c = worker.custom;
    const parts = [
      c.gender, c.age ? `around ${c.age} years old` : '', c.height, c.bodyType, c.skinTone,
      c.hairLength && c.hairColor ? `${c.hairLength} ${c.hairColor} hair` : (c.hairColor||c.hairLength),
      c.facialAppearance, c.facialHair, c.clothing ? `wearing ${c.clothing}` : '',
      c.ppe ? `with ${c.ppe} PPE` : '', c.accessories, c.expression ? `${c.expression} expression` : '',
      c.workingBehavior, c.bodyLanguage, c.pose, c.build
    ].filter(Boolean);
    return `an adult worker — ${parts.join(', ')}${c.notes ? '. ' + c.notes : ''}. The worker maintains a safe stance, outside any tool's trajectory`;
  }
  return `an adult worker matching the "${base.name}" profile, wearing realistic PPE and maintaining a safe working stance`;
}

function locationPromptDescriptor(loc, type){
  const base = findLocation(loc.id, type);
  if(base.custom && loc.custom){
    const c = loc.custom;
    const vals = Object.values(c).filter(Boolean);
    return `${c.name || 'a custom ' + (type==='INDOOR'?'indoor facility':'outdoor site')} — ${vals.join(', ')}`;
  }
  return `${base.name}, a realistic ${type === 'INDOOR' ? 'industrial indoor seafood processing environment' : 'outdoor commercial fishing environment'}`;
}

function buildVideoPrompt(){
  const s = state;
  const fishDesc = fishPromptDescriptor(s.fish);
  const toolDesc = toolPromptDescriptor(s.tool, true);
  const workerDesc = workerPromptDescriptor(s.worker);
  const locDesc = locationPromptDescriptor(s.location, s.locationType);
  const cut = findCut(s.cuttingStyle);
  const flow = findFlow(s.flow);
  const durLabel = s.duration.label;

  return `Realistic iPhone handheld documentary footage, ${s.duration.seconds || 'custom-length'} seconds, shot in ${s.format.customLabel || s.format.label} format.

SUBJECT: ${fishDesc}, lying passive and completely still (no breathing, no blinking, no living movement), positioned on a realistic work surface at ${locDesc}.

WORKER: ${workerDesc}.

TOOL: ${toolDesc}.

SEQUENCE (${flow.name}): ${cut.desc}

CUTTING STYLE — ${cut.name}: describe only the worker's movement, direction, body mechanics, force, sequence and follow-through. The tool itself is a separate, already-established prop; do not redesign it here.

MAXIMUM RESULT REVEAL: after the main action, the worker completes the follow-through, stabilizes the tool, and physically presents the result. The camera physically repositions (no digital zoom) so the actual physical result — traceable from the original subject through the target area, the tool, the action, to the physical outcome — fills a meaningful portion of the frame.

CAMERA: realistic handheld iPhone documentary style operated by a human — natural framing, realistic autofocus and exposure, subtle handheld motion, physical repositioning only. No drone, no gimbal, no tripod, no digital zoom.

CONTINUITY: the same animal, worker, tool, clothing, location, lighting, cutting point, anatomy and dimensions remain fully consistent throughout — no morphing, teleportation, object spawning/disappearing, identity changes, anatomy resets, or scale changes.

DURATION STRUCTURE: this is a genuine ${durLabel} structure (not a stretched or slowed version of a shorter cut) — pacing, number of beats and shot holds are built specifically for this length.

SAFETY: the worker is an adult wearing realistic PPE, in a safe stance, fully outside the tool's trajectory at all times; the tool is never directed toward any person. No human injury, blood, or accidents of any kind are depicted. The animal, if shown post-action, remains entirely passive with no living behavior. Gratuitous graphic gore is avoided — the emphasis stays on scale, industrial realism, structural result and cut geometry rather than extreme gore.

RATIO: ${s.ratio.label}.`;
}

function buildImagePrompt(){
  const s = state;
  const fishDesc = fishPromptDescriptor(s.fish);
  const toolDesc = toolPromptDescriptor(s.tool, false);
  const locDesc = locationPromptDescriptor(s.location, s.locationType);

  return `Realistic iPhone documentary photograph (not studio photography, not CGI, not a 3D render, not concept art) — a single still frame captured by a real person on location.

SUBJECT: ${fishDesc}. The animal is the primary subject, physically massive, with realistic anatomy, skin, proportions, texture and consistent scale. If shown post-processing, the animal is completely passive with no living behavior (no breathing, no blinking).

ENVIRONMENT: ${locDesc}. Believable industrial architecture or coastal environment, realistic surfaces, realistic wet textures where applicable, natural believable lighting, coherent scale between all elements.

TOOL / PROP: ${toolDesc}.

SCALE REFERENCE: if a worker or familiar object appears in frame, it is used purely as a scale reference and does not obscure the animal's important features. No additional workers unless explicitly requested.

CAMERA: realistic smartphone camera perspective, natural lens behavior, realistic exposure and autofocus, subtle handheld framing, natural depth of field, natural imperfections — authentic, not overly polished or commercial.

SAFETY: no human injuries, blood, or accidents; no distressed living animals; no excessive blood spray or graphic organs. Restrained, realistic, documentary-style result — the emphasis is on scale, industrial atmosphere, and physical realism.

RATIO: ${s.photoRatio.label}.`;
}

function buildNegativePrompt(){
  const common = [
    "human injury","human blood","worker accident","human amputation","tool striking a person",
    "person inside cutting path","gratuitous gore","excessive blood spray","graphic exposed organs",
    "distressed living animal behavior on a passive subject","breathing or blinking on a dead animal",
    "CGI look","3D render look","video game render","studio lighting","fantasy poster style",
    "overly polished commercial advertising look","digital zoom","drone shot","gimbal shot","tripod-locked shot",
    "morphing anatomy","object teleporting","object spawning or disappearing","inconsistent scale",
    "identity changes mid-shot","extra unrequested workers","low quality","blurry","distorted anatomy",
    "watermark","text overlay","logo"
  ];
  return common.join(', ') + '.';
}

function buildTitle(subject){
  const templates = [
    `This ${subject} Was Bigger Than Anyone Expected`,
    `They Weren't Ready For This ${subject}`,
    `The Biggest ${subject} Processed This Year`,
    `Watch This Massive ${subject} Get Processed`,
    `Nobody Believed This ${subject} Was Real`
  ];
  return templates[contentNumber % templates.length];
}
function buildCaption(subject){
  return `This might be one of the biggest ${subject.toLowerCase()} ever processed on camera — full scale, full detail, watch till the end.`;
}
function buildHashtags(kind){
  const base = ["#GiantFish","#SeafoodProcessing","#BigCatch","#FishingLife","#OceanGiants","#CommercialFishing"];
  const counts = { tiktok:6, instagram:5, youtube:5, facebook:3 };
  let tags = base.slice(0, counts[kind] || 5);
  if(kind === 'youtube' && !tags.includes('#Shorts')) tags[tags.length-1] = '#Shorts';
  return tags.join(' ');
}

function buildOutput(){
  const s = state;
  const subjectLabel = s.fish.isCustom ? (s.fish.custom.species || 'Custom Fish') : s.fish.species;
  const number = contentNumber;
  contentNumber += 1;

  let result = { mode: s.mode, number, subject: subjectLabel };

  if(s.mode === 'video'){
    result.prompt = buildVideoPrompt();
    result.negative = buildNegativePrompt();
    result.title = buildTitle(subjectLabel);
    result.caption = buildCaption(subjectLabel);
    result.hashtags = {
      tiktok: buildHashtags('tiktok'),
      instagram: buildHashtags('instagram'),
      youtube: buildHashtags('youtube'),
      facebook: buildHashtags('facebook')
    };
    result.setup = [
      ['Format', s.format.customLabel || s.format.label],
      ['Fish', fishLabel(s.fish)],
      ['Weight', fishWeightLength(s.fish).weight],
      ['Length', fishWeightLength(s.fish).length],
      ['Cutting Style', `${s.cuttingStyle} — ${findCut(s.cuttingStyle).name}`],
      ['Tool', toolLabel(s.tool)],
      ['Worker', workerLabel(s.worker)],
      ['Location', `${s.locationType} — ${locationLabel(s.location, s.locationType)}`],
      ['Flow', `${s.flow} — ${findFlow(s.flow).name}`],
      ['Duration', s.duration.label],
      ['Ratio', s.ratio.label],
      ['Camera', 'Realistic iPhone handheld documentary footage']
    ];
  } else {
    result.prompt = buildImagePrompt();
    result.negative = buildNegativePrompt();
    result.title = buildTitle(subjectLabel);
    result.caption = buildCaption(subjectLabel);
    result.hashtags = {
      tiktok: buildHashtags('tiktok'),
      instagram: buildHashtags('instagram'),
      facebook: buildHashtags('facebook')
    };
    result.setup = [
      ['Fish', fishLabel(s.fish)],
      ['Weight', fishWeightLength(s.fish).weight],
      ['Length', fishWeightLength(s.fish).length],
      ['Tool', toolLabel(s.tool)],
      ['Location', `${s.locationType} — ${locationLabel(s.location, s.locationType)}`],
      ['Photo Ratio', s.photoRatio.label],
      ['Camera', 'Realistic iPhone documentary photography']
    ];
  }

  sessionHistory.push(result);
  return result;
}

/* =========================================================
   OUTPUT SCREEN
   ========================================================= */

function renderOutputScreen(){
  const o = lastOutput;
  const wrap = el(`<div class="output-wrap"></div>`);
  const header = el(`<div style="display:flex; justify-content:space-between; align-items:center; margin-bottom:16px;">
    <h2 style="margin:0; font-size:20px;">${o.mode === 'video' ? 'CONTENT' : 'IMAGE CONTENT'} #${o.number} — ${esc(o.subject.toUpperCase())}</h2>
    <div style="display:flex; gap:8px;">
      <button class="ghost small" id="editBtn">Edit One Field</button>
      <button class="ghost small" id="newBtn">New Content</button>
    </div>
  </div>`);
  wrap.appendChild(header);
  header.querySelector('#newBtn').onclick = () => { state = freshState(state.mode); screen='mode-select'; render(); };
  header.querySelector('#editBtn').onclick = () => { screen='wizard'; state.step = 1; render(); };

  wrap.appendChild(outBlock('FINAL SETUP', renderSetupTable(o.setup)));
  wrap.appendChild(outBlock(o.mode==='video' ? 'SEEDANCE 2.0 PROMPT — FINAL' : 'IMAGE PROMPT — FINAL', `<div class="out-body mono">${esc(o.prompt)}</div>`, o.prompt));
  wrap.appendChild(outBlock('NEGATIVE PROMPT', `<div class="out-body mono">${esc(o.negative)}</div>`, o.negative));
  wrap.appendChild(outBlock('TITLE', `<div class="out-body">${esc(o.title)}</div>`, o.title));
  wrap.appendChild(outBlock('CAPTION', `<div class="out-body">${esc(o.caption)}</div>`, o.caption));

  const hashtagLines = Object.entries(o.hashtags).map(([k,v]) => `<div class="hashtag-row"><b>${k.toUpperCase()}</b>${esc(v)}</div>`).join('');
  const hashtagFull = Object.entries(o.hashtags).map(([k,v]) => `${k.toUpperCase()}: ${v}`).join('\n');
  wrap.appendChild(outBlock('HASHTAGS', `<div class="out-body">${hashtagLines}</div>`, hashtagFull));

  return wrap;
}

function renderSetupTable(rows){
  return `<table class="setup-table">${rows.map(([k,v]) => `<tr><td>${esc(k)}</td><td>${esc(v)}</td></tr>`).join('')}</table>`;
}

function outBlock(title, bodyHtml, copyText){
  const block = el(`<div class="out-block">
    <div class="out-head"><h4>${esc(title)}</h4>${copyText ? `<button class="copy-btn">COPY</button>` : ''}</div>
    <div>${bodyHtml}</div>
  </div>`);
  if(copyText){
    const btn = block.querySelector('.copy-btn');
    btn.onclick = () => {
      navigator.clipboard.writeText(copyText).then(() => {
        btn.textContent = 'COPIED';
        btn.classList.add('copied');
        setTimeout(() => { btn.textContent='COPY'; btn.classList.remove('copied'); }, 1400);
      });
    };
  }
  return block;
}

/* =========================================================
   INIT
   ========================================================= */
render();
</script>
</body>
</html>
