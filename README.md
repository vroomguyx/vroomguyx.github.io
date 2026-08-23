<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Rockport Bounty Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Oswald:wght@400;600;700&family=Roboto+Mono:wght@400;600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#0b0d10;
    --bg-alt:#15181d;
    --bg-card:#1b1f26;
    --line:#2a2f37;
    --heat:#ff4e2a;
    --heat-dim:#3a2019;
    --radar:#36a6ff;
    --radar-dim:#152634;
    --text:#eef1f2;
    --muted:#8a93a1;
    --gold:#ffcf5c;
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background:
      radial-gradient(1200px 600px at 50% -10%, #1a1e25 0%, var(--bg) 55%),
      var(--bg);
    color:var(--text);
    font-family:'Inter',system-ui,sans-serif;
    min-height:100vh;
    padding-bottom:60px;
  }
  h1,h2,h3,.display{font-family:'Oswald',sans-serif;text-transform:uppercase;}
  .mono{font-family:'Roboto Mono',monospace;}

  header{
    padding:28px 20px 18px;
    text-align:center;
    border-bottom:1px solid var(--line);
    background:linear-gradient(180deg, rgba(255,78,42,0.08), transparent);
  }
  header h1{
    font-size:clamp(28px,5vw,42px);
    letter-spacing:2px;
    margin:0;
    color:var(--text);
  }
  header h1 span{color:var(--heat);}
  header p{
    margin:6px 0 0;
    color:var(--muted);
    font-size:13px;
    letter-spacing:1.5px;
    text-transform:uppercase;
  }
  #mode-tag{
    display:inline-block;
    margin-top:10px;
    font-family:'Roboto Mono',monospace;
    font-size:11px;
    letter-spacing:1px;
    padding:3px 10px;
    border-radius:20px;
    border:1px solid var(--line);
    color:var(--muted);
  }

  .wrap{max-width:720px;margin:0 auto;padding:0 16px;}

  /* --- Shared centered-card screens --- */
  .center-screen{
    display:flex;
    align-items:center;
    justify-content:center;
    min-height:60vh;
    padding:20px;
  }
  .card{
    background:var(--bg-card);
    border:1px solid var(--line);
    border-radius:10px;
    padding:32px 28px;
    max-width:400px;
    width:100%;
    text-align:center;
    box-shadow:0 20px 60px rgba(0,0,0,0.4);
  }
  .card h2{
    font-size:20px;
    letter-spacing:1px;
    margin:0 0 6px;
  }
  .card .sub{
    color:var(--muted);
    font-size:13px;
    margin-bottom:22px;
  }

  /* --- Screen 0: category select --- */
  #category-screen{display:flex;}
  .category-btn{
    display:block;
    width:100%;
    background:var(--bg-alt);
    border:2px solid var(--line);
    border-radius:10px;
    padding:20px 18px;
    margin-top:14px;
    text-align:left;
    cursor:pointer;
    transition:all .15s ease;
    color:var(--text);
  }
  .category-btn:first-of-type{margin-top:0;}
  .category-btn:hover{transform:translateY(-1px);}
  .category-btn:active{transform:scale(0.98);}
  .category-btn .cat-title{
    font-family:'Oswald',sans-serif;
    font-size:26px;
    letter-spacing:1px;
  }
  .category-btn .cat-sub{
    font-size:12px;
    color:var(--muted);
    margin-top:4px;
    text-transform:none;
    letter-spacing:0;
  }
  .category-btn.any .cat-title{color:var(--heat);}
  .category-btn.any:hover{border-color:var(--heat);}
  .category-btn.nmg .cat-title{color:var(--radar);}
  .category-btn.nmg:hover{border-color:var(--radar);}

  /* --- Screen 1: bounty entry --- */
  #bounty-input{
    width:100%;
    background:var(--bg-alt);
    border:1px solid var(--line);
    color:var(--gold);
    font-family:'Roboto Mono',monospace;
    font-size:22px;
    text-align:center;
    padding:14px 10px;
    border-radius:6px;
    outline:none;
  }
  #bounty-input:focus{border-color:var(--heat);}
  .error{font-size:12px;color:var(--heat);margin-top:8px;min-height:16px;}
  .text-link{
    display:inline-block;
    margin-top:16px;
    font-size:12px;
    color:var(--muted);
    background:none;
    border:none;
    text-decoration:underline;
    cursor:pointer;
    text-transform:none;
    letter-spacing:0;
    font-family:'Inter',sans-serif;
  }
  .text-link:hover{color:var(--text);}

  button{
    font-family:'Oswald',sans-serif;
    cursor:pointer;
    border:none;
    letter-spacing:1px;
    text-transform:uppercase;
  }
  .btn-primary{
    margin-top:18px;
    width:100%;
    background:var(--heat);
    color:#1a0a05;
    font-weight:600;
    font-size:15px;
    padding:14px;
    border-radius:6px;
    transition:transform .1s ease, filter .15s ease;
  }
  .btn-primary:hover{filter:brightness(1.08);}
  .btn-primary:active{transform:scale(0.98);}

  /* --- Screen 2: tracker --- */
  #tracker-screen{display:none;}

  .total-bar{
    position:sticky;
    top:0;
    z-index:10;
    background:rgba(11,13,16,0.92);
    backdrop-filter:blur(6px);
    border-bottom:1px solid var(--line);
    padding:14px 16px;
    display:flex;
    align-items:center;
    justify-content:space-between;
    gap:12px;
  }
  .total-label{
    font-size:11px;
    color:var(--muted);
    letter-spacing:1.5px;
    margin-bottom:2px;
  }
  .total-value{
    font-family:'Roboto Mono',monospace;
    font-size:26px;
    font-weight:600;
    color:var(--gold);
  }
  .reset-btn{
    background:transparent;
    border:1px solid var(--line);
    color:var(--muted);
    font-size:11px;
    padding:8px 12px;
    border-radius:6px;
  }
  .reset-btn:hover{border-color:var(--heat);color:var(--heat);}

  .boss-list{padding:18px 0 6px;display:flex;flex-direction:column;gap:14px;}

  .boss-card{
    background:var(--bg-card);
    border:1px solid var(--line);
    border-radius:10px;
    overflow:hidden;
    animation:rise .35s ease both;
  }
  @keyframes rise{
    from{opacity:0;transform:translateY(10px);}
    to{opacity:1;transform:translateY(0);}
  }

  .boss-top{
    display:flex;
    align-items:center;
    gap:14px;
    padding:14px 16px;
    border-bottom:1px solid var(--line);
  }
  .rank-badge{
    font-family:'Oswald',sans-serif;
    font-size:13px;
    color:var(--heat);
    border:1.5px solid var(--heat);
    border-radius:4px;
    padding:4px 8px;
    letter-spacing:0.5px;
    flex-shrink:0;
  }
  .boss-id{flex:1;min-width:0;}
  .boss-name{font-size:19px;letter-spacing:0.5px;line-height:1.1;}
  .boss-car{font-size:12px;color:var(--muted);margin-top:2px;}
  .boss-req{
    text-align:right;
    flex-shrink:0;
  }
  .boss-req .req-label{font-size:10px;color:var(--muted);letter-spacing:1px;}
  .boss-req .req-value{font-family:'Roboto Mono',monospace;font-size:14px;}
  .status-tag{
    display:inline-block;
    margin-top:4px;
    font-size:10px;
    letter-spacing:0.5px;
    padding:2px 7px;
    border-radius:20px;
    font-family:'Inter',sans-serif;
    text-transform:uppercase;
  }
  .status-unlocked{background:rgba(70,200,120,0.15);color:#5fd68a;}
  .status-locked{background:rgba(255,78,42,0.12);color:var(--heat);}

  .boss-body{padding:14px 16px 16px;}
  .group-label{
    font-size:11px;
    color:var(--muted);
    letter-spacing:1.5px;
    margin-bottom:8px;
    display:flex;
    align-items:center;
    gap:6px;
  }
  .group-label:not(:first-child){margin-top:14px;}
  .dot{width:7px;height:7px;border-radius:50%;display:inline-block;flex-shrink:0;}
  .dot-heat{background:var(--heat);}
  .dot-radar{background:var(--radar);}
  .dot-chase{background:var(--gold);}

  .chip-row{display:flex;flex-wrap:wrap;gap:8px;}
  .chip{
    background:var(--bg-alt);
    border:1px solid var(--line);
    border-radius:7px;
    padding:9px 12px;
    font-family:'Roboto Mono',monospace;
    font-size:12.5px;
    color:var(--text);
    text-align:left;
    line-height:1.3;
    min-width:96px;
    transition:all .12s ease;
  }
  .chip .chip-title{
    display:block;
    font-family:'Inter',sans-serif;
    font-size:10px;
    letter-spacing:0.5px;
    color:var(--muted);
    text-transform:uppercase;
    margin-bottom:2px;
  }
  .chip.milestone:hover{border-color:var(--heat);}
  .chip.camera:hover{border-color:var(--radar);}
  .chip.milestone.active{
    background:var(--heat-dim);
    border-color:var(--heat);
    color:#ffc9b8;
  }
  .chip.camera.active{
    background:var(--radar-dim);
    border-color:var(--radar);
    color:#bfe4ff;
  }
  .chip.active .chip-title::after{content:" ✓";}

  .load-more-wrap{
    display:flex;
    justify-content:center;
    padding:18px 0 8px;
  }
  #load-more-btn{
    background:var(--bg-card);
    border:1px solid var(--line);
    color:var(--text);
    font-size:13px;
    padding:12px 22px;
    border-radius:8px;
  }
  #load-more-btn:hover{border-color:var(--heat);color:var(--heat);}

  /* Route Math — calculated minimum Chase Bounty targets, per scenario */
  .scenario-panel{
    background:rgba(255,207,92,0.05);
    border:1px solid rgba(255,207,92,0.25);
    border-radius:8px;
    padding:12px 14px;
    margin-top:14px;
  }
  .scenario-title{
    font-size:10.5px;
    color:var(--gold);
    letter-spacing:1px;
    text-transform:uppercase;
    margin-bottom:6px;
  }
  .scenario-title .note{
    font-size:10px;
    color:var(--muted);
    text-transform:none;
    letter-spacing:0;
    font-weight:400;
  }
  .scenario-row{
    display:flex;
    justify-content:space-between;
    align-items:baseline;
    gap:10px;
    padding:6px 0;
    border-top:1px solid rgba(255,207,92,0.12);
    font-size:12px;
  }
  .scenario-row:first-of-type{border-top:none;}
  .scenario-row .s-label{color:var(--text);}
  .scenario-row .s-value{font-family:'Roboto Mono',monospace;color:var(--gold);font-weight:600;white-space:nowrap;}
  .scenario-hint{font-size:10.5px;color:var(--muted);margin-top:6px;line-height:1.4;}

  /* Chase Bounty bar — sized for a quick, confident tap mid-race */
  .chase-bar{
    background:var(--bg-card);
    border:1.5px dashed #4a4020;
    border-radius:10px;
    padding:16px;
    animation:rise .35s ease both;
  }
  .chase-hint{
    font-size:10px;
    color:var(--muted);
    text-transform:none;
    letter-spacing:0;
    margin-left:2px;
  }
  .chase-input-row{
    display:flex;
    align-items:center;
    background:var(--bg-alt);
    border:1.5px solid var(--line);
    border-radius:10px;
    padding:0 16px;
    width:100%;
    min-height:58px;
  }
  .chase-input-row:focus-within{border-color:var(--gold);}
  .chase-prefix{font-family:'Roboto Mono',monospace;color:var(--gold);font-size:24px;}
  .chase-input{
    flex:1;
    background:transparent;
    border:none;
    outline:none;
    color:var(--gold);
    font-family:'Roboto Mono',monospace;
    font-weight:600;
    font-size:24px;
    padding:14px 12px;
    width:100%;
  }

  .done-msg{
    text-align:center;
    color:var(--muted);
    font-size:12px;
    letter-spacing:1px;
    padding:16px 0 4px;
    text-transform:uppercase;
  }
</style>
</head>
<body>

<header>
  <h1><span>ROCKPORT</span> BOUNTY TRACKER</h1>
  <p>Need for Speed · Most Wanted (2005) — Blacklist Milestones</p>
  <p style="margin-top:4px;font-size:10.5px;letter-spacing:1px;color:var(--muted);">v1.0 · built by VROOMGUYx &amp; Claude</p>
  <div id="mode-tag" style="display:none;"></div>
</header>

<!-- SCREEN 0: category select -->
<div id="category-screen" class="center-screen">
  <div class="card">
    <h2>Choose Your Route</h2>
    <div class="sub">Which blacklist are you running?</div>
    <button class="category-btn any" id="pick-any">
      <div class="cat-title">ANY%</div>
      <div class="cat-sub">Vic → Razor · 13 rivals</div>
    </button>
    <button class="category-btn nmg" id="pick-nmg">
      <div class="cat-title">NMG</div>
      <div class="cat-sub">Sonny → Razor · all 15 rivals</div>
    </button>
  </div>
</div>

<!-- SCREEN 1: bounty entry -->
<div id="entry-screen" class="center-screen" style="display:none;">
  <div class="card">
    <h2>Enter Your Current Bounty</h2>
    <div class="sub">This is your starting total. Minimum 50,000.</div>
    <input type="number" inputmode="numeric" id="bounty-input" placeholder="50000" min="50000" step="1000" value="50000">
    <div class="error" id="entry-error"></div>
    <button class="btn-primary" id="start-btn">Start Tracking</button>
    <br>
    <button class="text-link" id="change-category-btn">← change route</button>
  </div>
</div>

<!-- SCREEN 2: tracker -->
<div id="tracker-screen">
  <div class="total-bar">
    <div>
      <div class="total-label">TOTAL BOUNTY</div>
      <div class="total-value mono">$<span id="total-value">0</span></div>
    </div>
    <button class="reset-btn" id="reset-btn">Reset</button>
  </div>

  <div class="wrap">
    <div class="boss-list" id="boss-list"></div>
    <div class="load-more-wrap">
      <button id="load-more-btn">▼ Show Next 2 Rivals</button>
    </div>
    <div class="done-msg" id="done-msg" style="display:none;">— Top of the Blacklist reached —</div>
  </div>
</div>

<script>
  // Full Blacklist, #15 down to #1 — bounty required to challenge, and each
  // milestone / milestone-camera's short requirement + payout, from
  // Need for Speed: Most Wanted (2005) career mode.
  //   anyPercent      → included in the Any% route (Vic through Razor)
  //   chaseInAnyPercent → gets a Chase Bounty bar when running Any%
  //                        (NMG always shows a Chase Bounty bar on every rival)
  const allBosses = [
    { num:15, name:"Sonny", car:"VW Golf GTI", required:20000, anyPercent:false, chaseInAnyPercent:false,
      milestones:[ {label:"2 tags",amount:5000}, {label:"under 4m chase",amount:5000,quick:true}, {label:"2m chase",amount:5000} ],
      cameras:[ {label:"80 mph cam",amount:4000} ] },
    { num:14, name:"Taz", car:"Lexus IS 300", required:50000, anyPercent:false, chaseInAnyPercent:false,
      milestones:[ {label:"1k bounty",amount:6000}, {label:"15k cost",amount:6000}, {label:"3 infractions",amount:6000} ],
      cameras:[ {label:"93 mph cam",amount:4500}, {label:"90 mph cam",amount:4500} ] },
    { num:13, name:"Vic", car:"Toyota Supra", required:100000, anyPercent:true, chaseInAnyPercent:false,
      milestones:[ {label:"4 tags",amount:11000,defaultOn:true,key:"vic-tags"}, {label:"5k bounty",amount:11000,defaultOn:true}, {label:"under 4m chase",amount:11000,defaultOn:true,quick:true,key:"vic-quick"} ],
      cameras:[ {label:"108 mph cam",amount:5000,key:"vic-cam1",defaultOn:true}, {label:"105 mph cam",amount:5000,key:"vic-cam2"} ] },
    { num:12, name:"Izzy", car:"Mazda RX-8", required:180000, anyPercent:true, chaseInAnyPercent:true, scenarioType:"izzy",
      milestones:[ {label:"8 tags",amount:19000,defaultOn:true,key:"izzy-tags"}, {label:"under 3m chase",amount:19000,defaultOn:true,quick:true,key:"izzy-quick"}, {label:"4m chase",amount:19000,defaultOn:true} ],
      cameras:[ {label:"114 mph cam",amount:5500,key:"izzy-cam1",defaultOn:true}, {label:"121 mph cam",amount:5500,key:"izzy-cam2"} ] },
    { num:11, name:"Big Lou", car:"Mitsubishi Eclipse", required:300000, anyPercent:true, chaseInAnyPercent:false,
      milestones:[ {label:"20k cost",amount:30000,defaultOn:true}, {label:"2 roadblocks",amount:30000,defaultOn:true,key:"biglou-roadblock"}, {label:"4 infractions",amount:30000,defaultOn:true,key:"biglou-infractions"} ],
      cameras:[ {label:"118 mph cam",amount:6000}, {label:"118 mph cam",amount:6000} ] },
    { num:10, name:"Baron", car:"Porsche Cayman S", required:500000, anyPercent:true, chaseInAnyPercent:true, scenarioType:"baron",
      milestones:[ {label:"10k bounty",amount:35000,defaultOn:true}, {label:"30k cost",amount:35000,defaultOn:true}, {label:"5m chase",amount:35000,defaultOn:true,key:"baron-longchase"}, {label:"4 roadblocks",amount:35000,key:"baron-roadblock"} ],
      cameras:[ {label:"108 mph cam",amount:8000}, {label:"114 mph cam",amount:8000}, {label:"139 mph cam",amount:8000} ] },
    { num:9, name:"Earl", car:"Mitsubishi Lancer Evo VIII", required:790000, anyPercent:true, chaseInAnyPercent:false,
      milestones:[ {label:"12 tags",amount:46000,defaultOn:true,key:"earl-tags"}, {label:"30k bounty",amount:46000,defaultOn:true}, {label:"under 3m chase",amount:46000,quick:true,key:"earl-quick"}, {label:"6 roadblocks",amount:46000,key:"earl-roadblock"} ],
      cameras:[ {label:"149 mph cam",amount:16000,defaultOn:true}, {label:"136 mph cam",amount:16000}, {label:"139 mph cam",amount:16000} ] },
    { num:8, name:"Jewels", car:"Ford Mustang GT", required:1180000, anyPercent:true, chaseInAnyPercent:true, scenarioType:"jewels",
      milestones:[ {label:"18 tags",amount:60000,defaultOn:true,key:"jewels-tags"}, {label:"6m chase",amount:60000,key:"jewels-chase"}, {label:"2 spike strips",amount:60000,key:"jewels-spikes"}, {label:"5 infractions",amount:60000,defaultOn:true,key:"jewels-infractions"} ],
      cameras:[ {label:"147 mph cam",amount:24000,defaultOn:true}, {label:"147 mph cam",amount:24000}, {label:"132 mph cam",amount:24000} ] },
    { num:7, name:"Kaze", car:"Mercedes-Benz CLK500", required:1680000, anyPercent:true, chaseInAnyPercent:false,
      milestones:[ {label:"75k bounty",amount:70000,defaultOn:true}, {label:"50k cost",amount:70000,defaultOn:true}, {label:"under 3m chase",amount:70000,key:"kaze-quick",quick:true}, {label:"8 roadblocks",amount:70000,key:"kaze-roadblock"} ],
      cameras:[ {label:"158 mph cam",amount:34000}, {label:"142 mph cam",amount:34000}, {label:"111 mph cam",amount:34000} ] },
    { num:6, name:"Ming", car:"Lamborghini Gallardo", required:2300000, anyPercent:true, chaseInAnyPercent:true, scenarioType:"ming",
      milestones:[ {label:"22 tags",amount:80000,key:"ming-m1"}, {label:"7m chase",amount:80000,key:"ming-m2"}, {label:"10 roadblocks",amount:80000,key:"ming-m3"}, {label:"4 spike strips",amount:80000,key:"ming-m4"} ],
      cameras:[ {label:"111 mph cam",amount:46000,defaultOn:true}, {label:"107 mph cam",amount:46000}, {label:"158 mph cam",amount:46000} ] },
    { num:5, name:"Webster", car:"Corvette C6", required:3050000, anyPercent:true, chaseInAnyPercent:false,
      milestones:[ {label:"200k bounty",amount:90000,key:"webster-m1",defaultOn:true}, {label:"100k cost",amount:90000,key:"webster-m2",defaultOn:true}, {label:"under 2m chase",amount:90000,key:"webster-quick",quick:true}, {label:"6 spike strips",amount:90000,key:"webster-spikes"} ],
      cameras:[ {label:"180 mph cam",amount:60000}, {label:"120 mph cam",amount:60000}, {label:"186 mph cam",amount:60000} ] },
    { num:4, name:"JV", car:"Dodge Viper SRT10", required:4050000, anyPercent:true, chaseInAnyPercent:true, scenarioType:"jv",
      milestones:[ {label:"28 tags",amount:100000,key:"jv-tags"}, {label:"325k bounty",amount:100000,key:"jv-m-325k",defaultOn:true}, {label:"150k cost",amount:100000,key:"jv-m-150k",defaultOn:true}, {label:"6 infractions",amount:100000,key:"jv-m-infractions",defaultOn:true} ],
      cameras:[ {label:"180 mph cam",amount:76000,key:"jv-cam1",defaultOn:true}, {label:"164 mph cam",amount:76000,key:"jv-cam2",defaultOn:true}, {label:"152 mph cam",amount:76000} ] },
    { num:3, name:"Ronnie", car:"Aston Martin DB9", required:5550000, anyPercent:true, chaseInAnyPercent:false,
      milestones:[ {label:"32 tags",amount:140000,key:"ronnie-tags"}, {label:"600k bounty",amount:140000,key:"ronnie-m-600k",defaultOn:true}, {label:"under 2m chase",amount:140000,quick:true,key:"ronnie-quick"}, {label:"8m chase",amount:140000,key:"ronnie-longchase"} ],
      cameras:[ {label:"139 mph cam",amount:94000,defaultOn:true}, {label:"158 mph cam",amount:94000}, {label:"177 mph cam",amount:94000} ] },
    { num:2, name:"Bull", car:"Mercedes-Benz SLR McLaren", required:7550000, anyPercent:true, chaseInAnyPercent:true, scenarioType:"bull",
      milestones:[ {label:"200k cost",amount:180000,defaultOn:true}, {label:"9m chase",amount:180000,key:"bull-longchase"}, {label:"12 roadblocks",amount:180000,key:"bull-roadblock"}, {label:"8 spike strips",amount:180000,defaultOn:true,key:"bull-spikes"} ],
      cameras:[ {label:"194 mph cam",amount:114000,defaultOn:true}, {label:"136 mph cam",amount:114000,defaultOn:true}, {label:"165 mph cam",amount:114000,defaultOn:true} ] },
    { num:1, name:"Razor", car:"BMW M3 GTR", required:10000000, anyPercent:true, chaseInAnyPercent:true, scenarioType:"razor",
      milestones:[ {label:"35 tags",amount:220000,key:"razor-tags"}, {label:"850k bounty",amount:220000,key:"razor-m-850k",defaultOn:true}, {label:"under 2m chase",amount:220000,defaultOn:true,quick:true,key:"razor-quick"}, {label:"13m chase",amount:220000,key:"razor-longchase"} ],
      cameras:[ {label:"149 mph cam",amount:136000,key:"razor-cam1",defaultOn:true}, {label:"139 mph cam",amount:136000,key:"razor-cam2",defaultOn:true}, {label:"198 mph cam",amount:136000,key:"razor-cam3",defaultOn:true} ] },
  ];

  let mode = null;            // 'any' | 'nmg'
  let activeList = [];        // filtered boss list for the chosen mode
  let baseBounty = 0;
  let chipBounty = 0;         // running total from toggled milestone/camera chips
  let chaseBountyTotal = 0;   // running total from all Chase Bounty inputs
  let shownCount = 0;

  // Abbreviated so big totals stay readable at a glance mid-race:
  // 1,180,000 -> "1.18M", 800,000 -> "800K", 11,000 -> "11K".
  const fmt = n => {
    n = Math.round(n);
    const sign = n < 0 ? '-' : '';
    const abs = Math.abs(n);
    if(abs >= 1000000) return sign + (Math.round(abs/10000)/100) + 'M';
    if(abs >= 1000) return sign + (Math.round(abs/100)/10) + 'K';
    return sign + abs.toLocaleString('en-US');
  };
  const totalBounty = () => baseBounty + chipBounty + chaseBountyTotal;

  function bossGetsChaseBar(boss){
    return mode === 'nmg' ? true : !!boss.chaseInAnyPercent;
  }

  function updateTotalDisplay(){
    document.getElementById('total-value').textContent = fmt(totalBounty());
    document.querySelectorAll('.boss-card').forEach(card=>{
      const req = Number(card.dataset.required);
      const tag = card.querySelector('.status-tag');
      if(totalBounty() >= req){
        tag.textContent = 'Unlocked';
        tag.className = 'status-tag status-unlocked';
      } else {
        tag.textContent = 'Need $' + fmt(req - totalBounty()) + ' more';
        tag.className = 'status-tag status-locked';
      }
    });
    refreshScenarios();
  }

  function chip(kind, label, amount, key, defaultOn, quick, bossName){
    const el = document.createElement('button');
    el.className = 'chip ' + kind;
    el.dataset.amount = amount;
    if(key) el.dataset.key = key;
    if(quick) el.dataset.quick = '1';
    if(bossName) el.dataset.boss = bossName;
    el.innerHTML = `<span class="chip-title">${label}</span>+$${fmt(amount)}`;
    el.addEventListener('click', () => {
      const active = el.classList.toggle('active');
      chipBounty += active ? amount : -amount;
      updateTotalDisplay();
    });
    if(defaultOn){
      el.classList.add('active');
      chipBounty += amount;
    }
    return el;
  }

  // At JV's reveal, catch up any quick escape (an "under Xm chase" milestone)
  // that hasn't been picked up yet on an earlier, already-rendered rival.
  // Returns the list of boss names that actually got auto-enabled.
  function enableRemainingQuickEscapes(){
    const enabled = [];
    document.querySelectorAll('.chip.milestone[data-quick="1"]').forEach(el => {
      if(!el.classList.contains('active')){
        el.classList.add('active');
        chipBounty += Number(el.dataset.amount);
        if(el.dataset.boss) enabled.push(el.dataset.boss);
      }
    });
    return enabled;
  }

  // At Ming's reveal, Jewels' spike-strip milestone is assumed done too
  // (carried over from an earlier, still-open pursuit).
  function enableJewelsCarryovers(){
    const el = document.querySelector('.chip[data-key="jewels-spikes"]');
    if(el && !el.classList.contains('active')){
      el.classList.add('active');
      chipBounty += Number(el.dataset.amount);
    }
  }

  // Completing a bigger milestone on a later rival auto-completes the
  // smaller version of the same milestone on any earlier rival that hasn't
  // done it yet — true for roadblocks, spike strips, tags, infractions, and
  // long ("at least Xm") chases. Each array is in ascending order.
  const SWEEP_CHAINS = [
    ['biglou-roadblock','baron-roadblock','earl-roadblock','kaze-roadblock','ming-m3','bull-roadblock'],
    ['jewels-spikes','ming-m4','webster-spikes','bull-spikes'],
    ['vic-tags','izzy-tags','earl-tags','jewels-tags','ming-m1','jv-tags','ronnie-tags','razor-tags'],
    ['biglou-infractions','jewels-infractions','jv-m-infractions'],
    ['baron-longchase','jewels-chase','ming-m2','ronnie-longchase','bull-longchase','razor-longchase'],
    ['vic-quick','izzy-quick','earl-quick','kaze-quick','webster-quick','ronnie-quick','razor-quick'],
  ];
  function sweepChain(chain, fromKey){
    const idx = chain.indexOf(fromKey);
    if(idx < 0) return;
    let changed = false;
    for(let i=0;i<idx;i++){
      const el = document.querySelector(`.chip[data-key="${chain[i]}"]`);
      if(el && !el.classList.contains('active')){
        el.classList.add('active');
        chipBounty += Number(el.dataset.amount);
        changed = true;
      }
    }
    if(changed) updateTotalDisplay();
  }
  function wireSweeps(mRow){
    SWEEP_CHAINS.forEach(chain => {
      chain.forEach(key => {
        const el = mRow.querySelector(`[data-key="${key}"]`);
        if(!el) return;
        el.addEventListener('click', () => {
          if(el.classList.contains('active')) sweepChain(chain, key);
        });
        // Chips that start pre-toggled (defaultOn) never fire a click event,
        // so they need the same sweep run once, immediately, at creation.
        if(el.classList.contains('active')) sweepChain(chain, key);
      });
    });
  }

  // --- Route-math helpers: read whether a specific keyed chip is toggled ---
  function chipActive(key){
    const el = document.querySelector(`.chip[data-key="${key}"]`);
    return !!(el && el.classList.contains('active'));
  }
  function chipAmount(key){
    const el = document.querySelector(`.chip[data-key="${key}"]`);
    return el ? Number(el.dataset.amount) : 0;
  }
  // Live total, with a specific set of keyed chips' current contribution
  // pulled back out — so a scenario can substitute its own hypothetical.
  function baseExcluding(keys){
    const live = keys.reduce((sum,k)=> sum + (chipActive(k) ? chipAmount(k) : 0), 0);
    return totalBounty() - live;
  }

  function izzyScenarios(){
    const keys = ['vic-cam1','vic-cam2','izzy-cam1','izzy-cam2'];
    const base = baseExcluding(keys);
    const skip2ndVic = 5000 + 5500 + 5500;              // vic-cam1 + both izzy cams
    const skip2ndVicAnd2ndIzzy = 5000 + 5500;           // vic-cam1 + izzy-cam1 only
    return {
      skip2ndVic: Math.max(0, 180000 - base - skip2ndVic),
      skip2ndVicAnd2ndIzzy: Math.max(0, 180000 - base - skip2ndVicAnd2ndIzzy),
    };
  }

  function baronScenarios(){
    const base = baseExcluding(['baron-roadblock']);
    return {
      with4: Math.max(0, 500000 - base - 35000),
      only2: Math.max(0, 500000 - base - 0),
    };
  }

  function jewelsScenarios(){
    const baronDone = chipActive('baron-roadblock');
    const req = 1180000;
    if(baronDone){
      // Baron's roadblock is real and already banked — it stays in the live
      // total normally. Only Earl's roadblock and Jewels' own chase are
      // still hypothetical here, so only those two get excluded/replayed.
      const base = baseExcluding(['earl-roadblock','jewels-chase']);
      return {
        mode:'baronDone',
        both:    Math.max(0, req - base - (46000+60000)),
        either:  Math.max(0, req - base - Math.min(46000,60000)),
        neither: Math.max(0, req - base),
      };
    }
    // Baron's roadblock isn't banked yet, so it's part of the hypothetical too.
    const base = baseExcluding(['baron-roadblock','earl-roadblock','jewels-chase']);
    return {
      mode:'baronPending',
      g4g6: Math.max(0, req - base - (35000+60000)),
      g4n6: Math.max(0, req - base - 35000),
      l4n6: Math.max(0, req - base),
      l4g6: Math.max(0, req - base - 60000),
      g6g6: Math.max(0, req - base - (35000+46000+60000)),
      g6or6:Math.max(0, req - base - Math.min(35000+46000, 60000)),
    };
  }

  // Ming and JV are coupled: Kaze's 75k+50k+quick, and Webster's 200k+100k+quick
  // are always-assumed defaults, and JV is assumed to land its 1 camera, the
  // 325k/150k/infractions milestones, AND the raw 325,000 chase bounty from
  // that one pursuit — all fixed constants, so Ming's math doesn't depend on
  // JV's card actually being on screen yet, and excluding their keys below
  // means it's never double-counted once those cards do render.
  const MING_JV_FIXED_KEYS = ['kaze-quick','webster-m1','webster-m2','webster-quick','jv-cam1','jv-cam2','jv-m-325k','jv-m-150k','jv-m-infractions'];
  const KAZE_QUICK = 70000;
  const WEBSTER_FIXED = 90000 + 90000 + 90000;                // 200k + 100k milestones + quick escape
  const JV_FIXED = 76000*2 + 100000 + 100000 + 100000 + 325000; // 2 cams + 325k + 150k + infractions + the raw 325k itself

  function mingScenarios(){
    const liveThroughKaze = baseExcluding(['ming-m1','ming-m2','ming-m3','ming-m4', ...MING_JV_FIXED_KEYS]);
    const passMing = Math.max(0, 2300000 - totalBounty());
    const rows = [2,3,4].map(n => ({
      n,
      need: Math.max(0, 4050000 - (liveThroughKaze + n*80000 + KAZE_QUICK + WEBSTER_FIXED + JV_FIXED)),
    }));
    return { passMing, rows };
  }

  let jvCatchupNames = [];

  function jvShortfall(){
    // Live total already includes JV's own defaulted cam + milestones
    // (once rendered) — only the raw 325,000 from that pursuit is added here.
    return Math.max(0, 4050000 - (totalBounty() + 325000));
  }

  // Bull -> Razor: Razor's 3 cameras + the 850k milestone bonus + the raw
  // 850,000 from that one pursuit are the fixed, always-assumed pieces —
  // excluded from base and replayed here so nothing double-counts once
  // Razor's card actually renders.
  const RAZOR_EXCLUDE_KEYS = ['razor-cam1','razor-cam2','razor-cam3','razor-m-850k'];
  const RAZOR_FIXED = 136000*3 + 220000 + 850000; // 1,478,000
  function bullScenario(){
    const base = baseExcluding(RAZOR_EXCLUDE_KEYS);
    return {
      need: Math.max(0, 10000000 - base - RAZOR_FIXED),
      target: 10000000 - RAZOR_FIXED, // 8,522,000 — total needed before the 850k pursuit
    };
  }

  function renderIzzyScenarios(el){
    const s = izzyScenarios();
    el.innerHTML = `
      <div class="scenario-title">Route Math — min. Chase Bounty needed</div>
      <div class="scenario-row"><span class="s-label">Skip 2nd Vic cam</span><span class="s-value">$${fmt(s.skip2ndVic)}</span></div>
      <div class="scenario-row"><span class="s-label">Skip 2nd Vic + 2nd Izzy cam</span><span class="s-value">$${fmt(s.skip2ndVicAnd2ndIzzy)}</span></div>
    `;
  }

  function renderBaronScenarios(el){
    const s = baronScenarios();
    const warn = chipActive('baron-roadblock') ? '' :
      `<div class="scenario-hint">↳ without the roadblock milestone, plan on grabbing one of Baron's cameras too</div>`;
    el.innerHTML = `
      <div class="scenario-title">Route Math — min. Chase Bounty needed</div>
      <div class="scenario-row"><span class="s-label">4 roadblocks (milestone done)</span><span class="s-value">$${fmt(s.with4)}</span></div>
      <div class="scenario-row"><span class="s-label">Only 2 (milestone missed)</span><span class="s-value">$${fmt(s.only2)}</span></div>
      ${warn}
    `;
  }

  function renderJewelsScenarios(el){
    const s = jewelsScenarios();
    if(s.mode === 'baronDone'){
      el.innerHTML = `
        <div class="scenario-title">Route Math — min. Chase Bounty needed <span class="note">(Baron's roadblocks already banked)</span></div>
        <div class="scenario-row"><span class="s-label">Earl's 6 roadblocks + Jewels' 6m chase</span><span class="s-value">$${fmt(s.both)}</span></div>
        <div class="scenario-row"><span class="s-label">Only one of the two</span><span class="s-value">$${fmt(s.either)}</span></div>
        <div class="scenario-row"><span class="s-label">Neither</span><span class="s-value">$${fmt(s.neither)}</span></div>
        <div class="scenario-hint">↳ landing on neither? a camera here would help close the gap</div>
      `;
    } else {
      el.innerHTML = `
        <div class="scenario-title">Route Math — min. Chase Bounty needed <span class="note">(Baron's roadblocks still pending)</span></div>
        <div class="scenario-row"><span class="s-label">4+ blocks &amp; 6m chase</span><span class="s-value">$${fmt(s.g4g6)}</span></div>
        <div class="scenario-row"><span class="s-label">4+ blocks, no 6m chase</span><span class="s-value">$${fmt(s.g4n6)}</span></div>
        <div class="scenario-row"><span class="s-label">Under 4 blocks, no 6m chase</span><span class="s-value">$${fmt(s.l4n6)}</span></div>
        <div class="scenario-row"><span class="s-label">Under 4 blocks, 6m chase done</span><span class="s-value">$${fmt(s.l4g6)}</span></div>
        <div class="scenario-row"><span class="s-label">6+ blocks &amp; 6m chase</span><span class="s-value">$${fmt(s.g6g6)}</span></div>
        <div class="scenario-row"><span class="s-label">6+ blocks or 6m chase</span><span class="s-value">$${fmt(s.g6or6)}</span></div>
      `;
    }
  }

  function renderMingScenario(el){
    const s = mingScenarios();
    el.innerHTML = `
      <div class="scenario-title">Route Math — min. Chase Bounty needed</div>
      <div class="scenario-row"><span class="s-label">To pass Ming (2.3M total)</span><span class="s-value">$${fmt(s.passMing)}</span></div>
      <div class="scenario-hint">Looking ahead to JV's 325k pursuit (2 cams + 325k/150k/infraction milestones + the 325k itself, plus Kaze's &amp; Webster's quick escapes — always assumed):</div>
      ${s.rows.map(r => `<div class="scenario-row"><span class="s-label">${r.n} Ming milestones done</span><span class="s-value">$${fmt(r.need)}</span></div>`).join('')}
    `;
  }

  function renderJVScenario(el){
    const shortfall = jvShortfall();
    const catchupNote = jvCatchupNames.length
      ? `<div class="scenario-hint">↳ Including ${jvCatchupNames.join(', ')} quick escapes</div>`
      : '';
    if(shortfall <= 0 && !jvCatchupNames.length){
      el.style.display = 'none';
      el.innerHTML = '';
      return;
    }
    el.style.display = 'block';
    const row = shortfall > 0
      ? `<div class="scenario-row"><span class="s-label">Still short</span><span class="s-value">$${fmt(shortfall)}</span></div>`
      : `<div class="scenario-row"><span class="s-label">You're covered</span><span class="s-value">$0</span></div>`;
    el.innerHTML = `
      <div class="scenario-title">Route Math — min. Chase Bounty needed <span class="note">(even after the 325k pursuit)</span></div>
      ${row}
      ${catchupNote}
    `;
  }

  let bullCatchupNames = [];

  function renderBullScenario(el){
    const s = bullScenario();
    const catchupNote = bullCatchupNames.length
      ? `<div class="scenario-hint">↳ Including ${bullCatchupNames.join(', ')} quick escapes</div>`
      : '';
    el.innerHTML = `
      <div class="scenario-title">Route Math — min. Chase Bounty needed</div>
      <div class="scenario-row"><span class="s-label">Before Razor's 850k pursuit (target $${fmt(s.target)})</span><span class="s-value">$${fmt(s.need)}</span></div>
      <div class="scenario-hint">Assumes Razor's 3 cameras + the 850k milestone + the raw 850k itself.</div>
      ${catchupNote}
    `;
  }

  function renderRazorScenario(el){
    const projected = totalBounty() + 850000; // includes the raw 850k from that one pursuit
    // How much more Chase Bounty needs to land specifically in Razor's own
    // chase bar — accounts for everything already banked everywhere else,
    // unlike a from-scratch total across the whole run.
    const neededHere = Math.max(0, 10000000 - projected);
    el.style.display = 'block';
    el.innerHTML = `
      <div class="scenario-title">Route Math <span class="note">(after the 850k pursuit)</span></div>
      <div class="scenario-row"><span class="s-label">Projected total</span><span class="s-value">$${fmt(projected)}</span></div>
      <div class="scenario-row"><span class="s-label">Chase Bounty needed here</span><span class="s-value">$${fmt(neededHere)}</span></div>
    `;
  }

  function refreshScenarios(){
    const izzyEl = document.getElementById('izzy-scenario');
    if(izzyEl) renderIzzyScenarios(izzyEl);
    const baronEl = document.getElementById('baron-scenario');
    if(baronEl) renderBaronScenarios(baronEl);
    const jewelsEl = document.getElementById('jewels-scenario');
    if(jewelsEl) renderJewelsScenarios(jewelsEl);
    const mingEl = document.getElementById('ming-scenario');
    if(mingEl) renderMingScenario(mingEl);
    const jvEl = document.getElementById('jv-scenario');
    if(jvEl) renderJVScenario(jvEl);
    const bullEl = document.getElementById('bull-scenario');
    if(bullEl) renderBullScenario(bullEl);
    const razorEl = document.getElementById('razor-scenario');
    if(razorEl) renderRazorScenario(razorEl);
  }

  function renderBoss(boss){
    const card = document.createElement('div');
    card.className = 'boss-card';
    card.dataset.required = boss.required;

    const top = document.createElement('div');
    top.className = 'boss-top';
    top.innerHTML = `
      <div class="rank-badge">BL-${boss.num}</div>
      <div class="boss-id">
        <div class="boss-name">${boss.name}</div>
        <div class="boss-car">${boss.car}</div>
      </div>
      <div class="boss-req">
        <div class="req-label">REQUIRES</div>
        <div class="req-value mono">$${fmt(boss.required)}</div>
        <div class="status-tag status-locked">—</div>
      </div>
    `;
    card.appendChild(top);

    const body = document.createElement('div');
    body.className = 'boss-body';

    const mLabel = document.createElement('div');
    mLabel.className = 'group-label';
    mLabel.innerHTML = `<span class="dot dot-heat"></span> Milestones`;
    body.appendChild(mLabel);

    const mRow = document.createElement('div');
    mRow.className = 'chip-row';
    boss.milestones.forEach(m => mRow.appendChild(chip('milestone', m.label, m.amount, m.key, m.defaultOn, m.quick, boss.name)));
    body.appendChild(mRow);
    wireSweeps(mRow);

    const cLabel = document.createElement('div');
    cLabel.className = 'group-label';
    cLabel.innerHTML = `<span class="dot dot-radar"></span> Milestone Cameras`;
    body.appendChild(cLabel);

    const cRow = document.createElement('div');
    cRow.className = 'chip-row';
    boss.cameras.forEach(c => cRow.appendChild(chip('camera', c.label, c.amount, c.key, c.defaultOn, false, boss.name)));
    body.appendChild(cRow);

    // Route-math panel — a live, calculated minimum Chase Bounty target
    // for rivals with a known scenario twist (Izzy, Baron, Jewels).
    if(boss.scenarioType){
      const sPanel = document.createElement('div');
      sPanel.className = 'scenario-panel';
      sPanel.id = boss.scenarioType + '-scenario';
      body.appendChild(sPanel);
      if(boss.scenarioType === 'izzy') renderIzzyScenarios(sPanel);
      else if(boss.scenarioType === 'baron') renderBaronScenarios(sPanel);
      else if(boss.scenarioType === 'jewels') renderJewelsScenarios(sPanel);
      else if(boss.scenarioType === 'ming'){
        enableJewelsCarryovers();
        renderMingScenario(sPanel);
      } else if(boss.scenarioType === 'jv'){
        jvCatchupNames = enableRemainingQuickEscapes();
        renderJVScenario(sPanel);
      } else if(boss.scenarioType === 'bull'){
        bullCatchupNames = enableRemainingQuickEscapes();
        renderBullScenario(sPanel);
      } else if(boss.scenarioType === 'razor'){
        renderRazorScenario(sPanel);
      }
    }

    // Chase Bounty now lives inside the rival's own panel — sized big and
    // full-width on purpose, since it gets tapped mid-race, one-handed.
    if(bossGetsChaseBar(boss)){
      body.appendChild(makeChaseBar());
    }

    card.appendChild(body);
    return card;
  }

  function makeChaseBar(){
    const wrap = document.createElement('div');
    wrap.className = 'chase-bar';
    wrap.innerHTML = `
      <div class="group-label"><span class="dot dot-chase"></span> Chase Bounty <span class="chase-hint">— bounty picked up during a pursuit, type it in</span></div>
      <div class="chase-input-row">
        <span class="chase-prefix">$</span>
        <input type="number" inputmode="numeric" class="chase-input" placeholder="0" min="0" step="500">
      </div>
    `;
    const input = wrap.querySelector('.chase-input');
    let prevVal = 0;
    input.addEventListener('input', () => {
      const val = Number(input.value) || 0;
      chaseBountyTotal += (val - prevVal);
      prevVal = val;
      updateTotalDisplay();
    });
    return wrap;
  }

  function appendBosses(list, bosses){
    bosses.forEach(b => list.appendChild(renderBoss(b)));
  }

  function updateLoadMoreUI(){
    const btn = document.getElementById('load-more-btn');
    const done = document.getElementById('done-msg');
    if(shownCount >= activeList.length){
      btn.style.display = 'none';
      done.style.display = 'block';
    } else {
      btn.style.display = 'inline-block';
      done.style.display = 'none';
      const remaining = activeList.length - shownCount;
      btn.textContent = remaining === 1 ? '▼ Show Last Rival' : '▼ Show Next 2 Rivals';
    }
  }

  function startTracking(){
    const list = document.getElementById('boss-list');
    list.innerHTML = '';
    shownCount = Math.min(2, activeList.length);
    appendBosses(list, activeList.slice(0, shownCount));
    updateTotalDisplay();
    updateLoadMoreUI();
  }

  document.getElementById('load-more-btn').addEventListener('click', () => {
    const list = document.getElementById('boss-list');
    const nextEnd = Math.min(shownCount + 2, activeList.length);
    appendBosses(list, activeList.slice(shownCount, nextEnd));
    shownCount = nextEnd;
    updateTotalDisplay();
    updateLoadMoreUI();
  });

  function chooseMode(chosen){
    mode = chosen;
    activeList = mode === 'nmg' ? allBosses : allBosses.filter(b => b.anyPercent);
    const tag = document.getElementById('mode-tag');
    tag.textContent = mode === 'nmg' ? 'NMG · ALL 15 RIVALS' : 'ANY% · 13 RIVALS';
    tag.style.display = 'inline-block';
    document.getElementById('category-screen').style.display = 'none';

    if(mode === 'nmg'){
      // NMG starts from 0 bounty — no entry screen, straight to the list.
      baseBounty = 0;
      chipBounty = 0;
      chaseBountyTotal = 0;
      document.getElementById('entry-screen').style.display = 'none';
      document.getElementById('tracker-screen').style.display = 'block';
      startTracking();
    } else {
      document.getElementById('entry-screen').style.display = 'flex';
    }
  }

  document.getElementById('pick-any').addEventListener('click', () => chooseMode('any'));
  document.getElementById('pick-nmg').addEventListener('click', () => chooseMode('nmg'));

  document.getElementById('change-category-btn').addEventListener('click', () => {
    document.getElementById('entry-screen').style.display = 'none';
    document.getElementById('category-screen').style.display = 'flex';
    document.getElementById('mode-tag').style.display = 'none';
  });

  document.getElementById('start-btn').addEventListener('click', () => {
    const input = document.getElementById('bounty-input');
    const val = Number(input.value);
    const err = document.getElementById('entry-error');
    if(!val || val < 50000){
      err.textContent = 'Enter a bounty of at least $50,000.';
      return;
    }
    err.textContent = '';
    baseBounty = val;
    chipBounty = 0;
    chaseBountyTotal = 0;
    document.getElementById('entry-screen').style.display = 'none';
    document.getElementById('tracker-screen').style.display = 'block';
    startTracking();
  });

  document.getElementById('reset-btn').addEventListener('click', () => {
    document.getElementById('tracker-screen').style.display = 'none';
    document.getElementById('entry-error').textContent = '';
    if(mode === 'nmg'){
      document.getElementById('category-screen').style.display = 'flex';
      document.getElementById('mode-tag').style.display = 'none';
    } else {
      document.getElementById('entry-screen').style.display = 'flex';
    }
  });

  document.getElementById('bounty-input').addEventListener('keydown', (e) => {
    if(e.key === 'Enter') document.getElementById('start-btn').click();
  });
</script>

</body>
</html>
