# senviskyec.github.io

<style>
:root {
  --bg:#ffffff;--bg2:#f7f7f7;--bg3:#f0f0f0;
  --text:#1a1a1a;--text2:#555;--text3:#999;
  --border:rgba(0,0,0,0.1);--border2:rgba(0,0,0,0.18);
  --rad:8px;--radlg:12px;--orange:#e05a1b;
}
*{box-sizing:border-box;margin:0;padding:0;}
body{font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',sans-serif;font-size:14px;color:var(--text);background:var(--bg3);}
.topbar{background:var(--orange);color:#fff;padding:11px 16px;display:flex;justify-content:space-between;align-items:center;position:sticky;top:0;z-index:50;}
.topbar-title{font-size:11px;font-weight:600;letter-spacing:2px;text-transform:uppercase;}
.tbr{display:flex;gap:8px;}
.btn{padding:7px 14px;border-radius:var(--rad);border:1px solid rgba(255,255,255,0.4);background:rgba(255,255,255,0.15);font-size:12px;font-weight:600;cursor:pointer;color:#fff;font-family:inherit;transition:background 0.15s;}
.btn:hover{background:rgba(255,255,255,0.25);}
.btn.ok{background:#065f46;border-color:#065f46;}
.page{padding:16px;display:flex;flex-direction:column;gap:8px;max-width:760px;margin:0 auto;}
.card{background:var(--bg);border:1px solid var(--border);border-radius:var(--radlg);overflow:hidden;}
.ch{display:flex;align-items:center;justify-content:space-between;padding:13px 16px;cursor:pointer;user-select:none;transition:background 0.1s;}
.ch:hover{background:var(--bg2);}
.ct{font-size:13px;font-weight:600;display:flex;align-items:center;gap:8px;color:var(--text);}
.cb{padding:16px;display:flex;flex-direction:column;gap:12px;border-top:1px solid var(--border);}
.tog{width:38px;height:22px;border-radius:11px;background:#ccc;position:relative;cursor:pointer;transition:background 0.2s;border:none;flex-shrink:0;}
.tog.on{background:var(--orange);}
.tog::after{content:'';position:absolute;width:16px;height:16px;background:#fff;border-radius:50%;top:3px;left:3px;transition:left 0.2s;box-shadow:0 1px 3px rgba(0,0,0,0.2);}
.tog.on::after{left:19px;}
.field{display:flex;flex-direction:column;gap:5px;}
.field label{font-size:10px;font-weight:600;letter-spacing:1px;text-transform:uppercase;color:var(--text2);}
.field input,.field textarea{font-family:inherit;font-size:13px;padding:8px 10px;border:1px solid var(--border2);border-radius:var(--rad);background:var(--bg2);color:var(--text);width:100%;resize:vertical;}
.field textarea{min-height:68px;line-height:1.5;}
.field input:focus,.field textarea:focus{outline:none;border-color:var(--orange);background:#fff;}
.r2{display:grid;grid-template-columns:1fr 1fr;gap:10px;}
.r3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px;}
.li{display:flex;gap:8px;align-items:flex-start;background:var(--bg2);border-radius:var(--rad);padding:10px;border:1px solid var(--border);}
.lif{flex:1;display:flex;flex-direction:column;gap:7px;}
.rm{background:none;border:none;color:var(--text3);cursor:pointer;font-size:18px;line-height:1;padding:2px 4px;flex-shrink:0;border-radius:4px;}
.rm:hover{color:#b91c1c;background:#feeaea;}
.addbtn{background:none;border:1px dashed var(--border2);border-radius:var(--rad);padding:8px;font-size:12px;color:var(--text3);cursor:pointer;width:100%;text-align:center;font-family:inherit;}
.addbtn:hover{border-color:var(--orange);color:var(--orange);background:#fff8f5;}
.badge{font-size:10px;padding:2px 8px;border-radius:999px;font-weight:600;}
.req{background:#feeaea;color:#b91c1c;}
.offtag{background:var(--bg2);color:var(--text3);border:1px solid var(--border);}
.ontag{background:#fff3ee;color:var(--orange);border:1px solid #fbd5c2;}
.hint{font-size:12px;color:var(--text3);padding:10px 12px;background:var(--bg2);border-radius:var(--rad);border:1px solid var(--border);}
.divhr{height:1px;background:var(--border);margin:4px 0;}
.sublabel{font-size:10px;font-weight:600;letter-spacing:1px;text-transform:uppercase;color:var(--text3);margin-top:4px;}
.ov{position:fixed;inset:0;background:rgba(0,0,0,0.65);display:flex;align-items:flex-start;justify-content:center;z-index:999;padding:20px;overflow-y:auto;}
.pvbox{background:#fff;border-radius:var(--radlg);width:100%;max-width:680px;}
.pvhdr{padding:12px 16px;border-bottom:1px solid var(--border);display:flex;justify-content:space-between;align-items:center;background:#fff;border-radius:var(--radlg) var(--radlg) 0 0;position:sticky;top:0;z-index:10;}
.pvhdr span{font-size:13px;font-weight:600;}
.pvacts{display:flex;gap:8px;}
.pvbtn{padding:7px 14px;border-radius:var(--rad);border:1px solid var(--border2);background:var(--bg);font-size:12px;font-weight:600;cursor:pointer;font-family:inherit;color:var(--text);}
.pvbtn:hover{background:var(--bg2);}
.pvbtn.primary{background:var(--orange);color:#fff;border-color:var(--orange);}
.pvbtn.primary:hover{background:#c94e16;}
.pvbtn.ok{background:#065f46;color:#fff;border-color:#065f46;}
</style>
<div class="topbar">
  <span class="topbar-title">LNGVTY Newsletter Editor</span>
  <div class="tbr">
    <button class="btn" id="prevBtn">👁 Preview</button>
    <button class="btn" id="cpBtn">Copy HTML</button>
  </div>
</div>
<div class="page" id="page"></div>
<div class="ov" id="ov" style="display:none">
  <div class="pvbox">
    <div class="pvhdr">
      <span>Newsletter Preview</span>
      <div class="pvacts">
        <button class="pvbtn primary" id="pvCopy">Copy HTML</button>
        <button class="pvbtn" id="pvClose">✕ Close</button>
      </div>
    </div>
    <iframe id="pvFrame" style="width:100%;height:75vh;border:none;display:block;"></iframe>
  </div>
</div>
<script>
const SKEY='lngvty-nl-v4';
const uid=()=>Math.random().toString(36).slice(2,7);
const esc=s=>(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;');

function defS(){return{
  date:'',quote:'Results happen over time, not overnight. Work hard, stay consistent.',
  includeNote:false,noteText:'',
  includeSchedule:true,schedule:[{id:uid(),day:'',name:'',time:''},{id:uid(),day:'',name:'',time:''}],
  includeMembers:false,members:[{id:uid(),name:''}],
  includeRecipe:false,recipeName:'',recipeCategory:'',recipeDesc:'',recipeProtein:'',recipeCalories:'',recipeCost:'',recipePrepTime:'',
  ingredients:[{id:uid(),v:''},{id:uid(),v:''},{id:uid(),v:''},{id:uid(),v:''}],
  includeTip:true,tipCategory:'Mindset',tipTitle:'Consistency Beats Perfection Every Time',
  tipBody:"One hundred average weeks will beat one perfect week every time. The members who see the biggest transformations aren't the ones who go the hardest, but the ones who keep showing up.",
  hydrationTip:'Add a pinch of salt and squeeze of lemon to morning water.',
  proteinTip:'Hit at least 30g protein at breakfast to reduce cravings all day.',
  sleepTip:"Don't eat 2–3 hours before bed — digestion raises core temp.",
  mobilityTip:'5 minutes of hip work before every workout. 90/90 or hip circles.',
  includeSwaps:false,swaps:[{id:uid(),from:'',fromNote:'',to:'',toNote:''}],
  budgetProteins:[{id:uid(),name:'',cost:''},{id:uid(),name:'',cost:''}],
  includeSpecialEvents:false,specialEvents:[{id:uid(),name:'',tag:'',dow:'',dayNum:'',desc:''}],
  include90Day:true,
};}

let S=(()=>{try{const d=localStorage.getItem(SKEY);return d?JSON.parse(d):defS();}catch(e){return defS();}})();
const save=()=>{try{localStorage.setItem(SKEY,JSON.stringify(S));}catch(e){}};

function buildHTML(){
  const e=esc;
  let note='';
  if(S.includeNote){const t=S.noteText||'Another week, another chance to show up for yourself.';note=`<div class="section"><div class="section-label">Welcome</div><div class="section-title">A Note from the Team</div><div class="intro-box"><p>${e(t)}</p></div></div>`;}
  let sched='';
  if(S.includeSchedule){const it=S.schedule.filter(x=>x.name||x.day).map(x=>`<li class="schedule-item"><div class="schedule-day">${e(x.day)}</div><div><div class="schedule-event-name">${e(x.name)}</div><div class="schedule-event-detail">${e(x.time)}</div></div></li>`).join('');if(it)sched=`<div class="section"><div class="section-label">Every Week at LNGVTY</div><div class="section-title">Upcoming Schedule</div><div class="section-sub">Same time, every week — no excuses.</div><div class="schedule-card"><div class="schedule-title">Recurring Weekly Events</div><ul class="schedule-list">${it}</ul></div></div>`;}
  let memb='';
  if(S.includeMembers&&S.members.some(m=>m.name)){const it=S.members.filter(m=>m.name).map(m=>`<li class="new-member-item"><div class="member-name-text">- ${e(m.name)}</div></li>`).join('');memb=`<div class="section"><div class="section-label">New Faces</div><div class="section-title">Welcome New Members</div><div class="section-sub">Give a warm welcome to everyone who joined this week.</div><div class="welcome-card"><p class="welcome-intro">We're so glad you're here. Say hi and make them feel at home — that's what LNGVTY is all about.</p><ul class="new-members-list">${it}</ul></div></div>`;}
  let reci='';
  if(S.includeRecipe&&S.recipeName){const it=S.ingredients.filter(i=>i.v).map(i=>`<li>${e(i.v)}</li>`).join('');reci=`<div class="section"><div class="section-label">Recipe Corner</div><div class="section-title">This Week's Featured Recipe</div><div class="section-sub">${e(S.recipeCategory)||'High protein · Easy prep · Budget friendly'}</div><div class="recipe-card"><div class="recipe-meta"><span class="recipe-badge">${e(S.recipeCategory)||'High Protein'}</span><span class="recipe-badge orange">Prep: ${e(S.recipePrepTime)}</span></div><div class="recipe-name">${e(S.recipeName)}</div><div class="recipe-desc">${e(S.recipeDesc)}</div><div class="recipe-stats"><div class="stat"><div class="stat-num">${e(S.recipeProtein)}</div><div class="stat-label">Protein</div></div><div class="stat"><div class="stat-num">${e(S.recipeCalories)}</div><div class="stat-label">Calories</div></div><div class="stat"><div class="stat-num">$${e(S.recipeCost)}</div><div class="stat-label">Per Serving</div></div><div class="stat"><div class="stat-num">${e(S.recipePrepTime)}</div><div class="stat-label">Prep</div></div></div></div>${it?`<div class="grocery-box"><div class="grocery-title">Grocery List</div><ul class="grocery-list">${it}</ul></div>`:''}</div>`;}
  let tiph='';
  if(S.includeTip&&S.tipTitle){tiph=`<div class="section"><div class="section-label">Tip Tuesday</div><div class="section-title">This Week's Focus</div><div class="tip-graphic"><div class="tip-day">Tip Tuesday</div><div class="tip-category">${e(S.tipCategory)}</div><div class="tip-headline">${e(S.tipTitle)}</div><div class="tip-body">${e(S.tipBody)}</div></div><div class="tip-grid"><div class="mini-tip"><div class="mini-tip-icon">&#x1F4A7;</div><div class="mini-tip-label">Hydration</div><div class="mini-tip-text">${e(S.hydrationTip)}</div></div><div class="mini-tip"><div class="mini-tip-icon">&#x1F969;</div><div class="mini-tip-label">Protein</div><div class="mini-tip-text">${e(S.proteinTip)}</div></div><div class="mini-tip"><div class="mini-tip-icon">&#x1F634;</div><div class="mini-tip-label">Sleep</div><div class="mini-tip-text">${e(S.sleepTip)}</div></div><div class="mini-tip"><div class="mini-tip-icon">&#x1F9D8;</div><div class="mini-tip-label">Mobility</div><div class="mini-tip-text">${e(S.mobilityTip)}</div></div></div></div>`;}
  let swps='';
  if(S.includeSwaps){const rows=S.swaps.filter(sw=>sw.from||sw.to).map(sw=>`<div class="nutrition-swap"><div class="swap-from"><div class="swap-label">Instead of</div><div class="swap-item">${e(sw.from)}</div><div class="swap-note">${e(sw.fromNote)}</div></div><div class="swap-arrow">&#x2192;</div><div class="swap-to"><div class="swap-label">Try this</div><div class="swap-item">${e(sw.to)}</div><div class="swap-note">${e(sw.toNote)}</div></div></div>`).join('');const bp=S.budgetProteins.filter(p=>p.name).map(p=>`<li>${e(p.name)} <span class="budget-price">~${e(p.cost)}</span></li>`).join('');swps=`<div class="section"><div class="section-label">Nutrition Tips</div><div class="section-title">Smarter Swaps</div><div class="section-sub">Eating healthier doesn't have to mean eating boring.</div>${rows}${bp?`<div class="budget-box"><div class="budget-title">&#x1F4B0; Budget-Friendly Proteins This Week</div><ul class="budget-list">${bp}</ul></div>`:''}</div>`;}
  let evts='';
  if(S.includeSpecialEvents&&S.specialEvents.some(x=>x.name)){const rows=S.specialEvents.filter(x=>x.name).map(x=>`<div class="event-row"><div class="event-date-box"><div class="event-month">${e(x.dow)}</div><div class="event-day-num">${e(x.dayNum)}</div></div><div><div class="event-name">${e(x.name)}</div><div class="event-detail">${e(x.desc)}</div><span class="event-tag et-orange">${e(x.tag)}</span></div></div>`).join('');evts=`<div class="section"><div class="section-label">Special Upcoming Events</div><div class="section-title">Mark Your Calendar</div>${rows}</div>`;}
  let r90='';
  if(S.include90Day){r90=`<div class="section"><div class="section-label">Your Progress</div><div class="section-title">Coming Up on 90 Days?</div><div class="section-sub">Don't let the milestone pass without making it count.</div><p class="body-text">If you're approaching your 90-day mark — or you've already hit it — it's time to sit down with a coach. We'll pull up your InBody data, review what's changed, and map out your next 90 days with fresh goals.</p><br/><p class="body-text">Not sure where you're at? Check in with us — we'll look it up. <span class="highlight">Haven't scanned in a while? Come in before your review so your data is fresh.</span></p><br/><a href="https://lngvtyhub.com/schedule/" style="display:inline-block;background:#e05a1b;color:#fff;font-family:Montserrat,sans-serif;font-weight:700;font-size:12px;letter-spacing:2px;text-transform:uppercase;text-decoration:none;padding:14px 32px;border-radius:4px;">&raquo; Book Your 90-Day Review</a></div>`;}

  const css=`*{margin:0;padding:0;box-sizing:border-box}body{background:#f2f2f2;font-family:'Open Sans',sans-serif;color:#1a1a1a}.email-wrapper{max-width:620px;margin:0 auto;background:#fff}.top-bar{background:#e05a1b;padding:10px 24px;text-align:center;font-family:'Montserrat',sans-serif;font-size:11px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:#fff}.hero{background:#1a1a1a;padding:48px 48px 0;position:relative;overflow:hidden}.hero::after{content:'';position:absolute;bottom:0;left:0;right:0;height:4px;background:#e05a1b}.hero-top{display:flex;justify-content:space-between;align-items:flex-start;margin-bottom:32px}.logo-text{font-family:'Montserrat',sans-serif;font-weight:900;font-size:36px;letter-spacing:2px;color:#fff;line-height:1;margin-bottom:2px}.logo-sub{font-family:'Montserrat',sans-serif;font-size:10px;letter-spacing:5px;text-transform:uppercase;color:#e05a1b}.issue-info{text-align:right}.issue-label{font-family:'Montserrat',sans-serif;font-size:10px;letter-spacing:3px;text-transform:uppercase;color:#888}.issue-date{font-family:'Montserrat',sans-serif;font-weight:700;font-size:14px;color:#e05a1b;margin-top:3px}.opener{background:#e05a1b;padding:18px 48px 20px}.opener-quote{font-family:'Montserrat',sans-serif;font-weight:700;font-size:16px;color:#fff;line-height:1.4;letter-spacing:.3px}.section{padding:36px 48px;border-bottom:1px solid #eee}.section-label{font-family:'Montserrat',sans-serif;font-size:10px;letter-spacing:4px;text-transform:uppercase;color:#bbb;margin-bottom:6px}.section-title{font-family:'Montserrat',sans-serif;font-weight:900;font-size:22px;color:#3a3a3a;text-transform:uppercase;margin-bottom:4px;line-height:1.1}.section-sub{font-size:12px;color:#aaa;font-weight:300;margin-bottom:20px}.body-text{font-size:14px;color:#555;line-height:1.85;font-weight:300}.highlight{color:#e05a1b;font-weight:700}.intro-box{background:#fafafa;border-left:3px solid #e05a1b;padding:20px 24px;margin-top:4px}.intro-box p{font-size:14px;color:#555;line-height:1.85;font-weight:300}.schedule-card{background:#fafafa;padding:24px;margin-top:4px}.schedule-title{font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:4px;color:#bbb;text-transform:uppercase;margin-bottom:16px}.schedule-list{list-style:none;display:flex;flex-direction:column;gap:10px}.schedule-item{display:flex;align-items:flex-start;border-left:3px solid #e05a1b;background:#fff;padding:10px 14px}.schedule-day{font-family:'Montserrat',sans-serif;font-weight:700;font-size:9px;letter-spacing:2px;text-transform:uppercase;color:#e05a1b;min-width:76px;padding-top:2px}.schedule-event-name{font-family:'Montserrat',sans-serif;font-weight:900;font-size:13px;color:#3a3a3a;margin-bottom:2px}.schedule-event-detail{font-size:11px;color:#999;font-weight:300;line-height:1.5}.welcome-card{background:#fafafa;padding:24px;margin-top:4px}.welcome-intro{font-size:13px;color:#777;line-height:1.65;font-weight:300;margin-bottom:18px}.new-members-list{list-style:none;display:grid;grid-template-columns:1fr 1fr;gap:10px}.new-member-item{display:flex;align-items:center}.member-name-text{font-family:'Montserrat',sans-serif;font-weight:700;font-size:12px;color:#3a3a3a}.recipe-card{border:1px solid #e8e8e8;border-top:3px solid #e05a1b;padding:20px;margin-bottom:14px}.recipe-meta{display:flex;gap:10px;flex-wrap:wrap;margin-bottom:10px}.recipe-badge{font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:2px;text-transform:uppercase;background:#f0f0f0;color:#3a3a3a;padding:3px 8px}.recipe-badge.orange{background:#fff3ef;color:#e05a1b}.recipe-name{font-family:'Montserrat',sans-serif;font-weight:900;font-size:18px;color:#3a3a3a;text-transform:uppercase;margin-bottom:6px}.recipe-desc{font-size:13px;color:#777;line-height:1.65;font-weight:300;margin-bottom:14px}.recipe-stats{display:flex;gap:24px}.stat{text-align:center}.stat-num{font-family:'Montserrat',sans-serif;font-weight:900;font-size:20px;color:#e05a1b;line-height:1}.stat-label{font-size:9px;color:#aaa;letter-spacing:1px;text-transform:uppercase;margin-top:3px}.grocery-box{background:#3a3a3a;padding:18px 20px;margin-top:14px}.grocery-title{font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:4px;color:#e05a1b;text-transform:uppercase;margin-bottom:12px}.grocery-list{list-style:none;display:grid;grid-template-columns:1fr 1fr;gap:6px}.grocery-list li{font-size:12px;color:#ccc;font-weight:300;padding-left:14px;position:relative;line-height:1.4}.grocery-list li::before{content:'&#x2014;';position:absolute;left:0;color:#e05a1b}.tip-graphic{background:#3a3a3a;padding:28px;margin-top:4px;position:relative;overflow:hidden}.tip-day{font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:4px;text-transform:uppercase;color:#e05a1b;margin-bottom:8px}.tip-category{display:inline-block;font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:2px;text-transform:uppercase;border:1px solid #e05a1b;color:#e05a1b;padding:3px 10px;margin-bottom:12px}.tip-headline{font-family:'Montserrat',sans-serif;font-weight:900;font-size:22px;color:#fff;text-transform:uppercase;line-height:1.2;margin-bottom:10px}.tip-body{font-size:13px;color:#aaa;line-height:1.7;font-weight:300}.tip-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-top:16px}.mini-tip{background:rgba(255,255,255,.05);border-top:2px solid #e05a1b;padding:14px}.mini-tip-icon{font-size:16px;margin-bottom:6px}.mini-tip-label{font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:#e05a1b;margin-bottom:4px}.mini-tip-text{font-size:12px;color:#aaa;line-height:1.5;font-weight:300}.nutrition-swap{display:flex;align-items:center;margin-bottom:10px;border:1px solid #eee}.swap-from{flex:1;padding:14px 16px;background:#fff3ef}.swap-arrow{padding:0 12px;font-size:18px;color:#e05a1b;font-weight:900;background:#fff}.swap-to{flex:1;padding:14px 16px;background:#fafafa}.swap-label{font-family:'Montserrat',sans-serif;font-size:9px;letter-spacing:2px;text-transform:uppercase;color:#bbb;margin-bottom:4px;font-weight:700}.swap-item{font-family:'Montserrat',sans-serif;font-weight:700;font-size:13px;color:#3a3a3a}.swap-note{font-size:11px;color:#999;margin-top:2px;font-weight:300}.budget-box{background:#fafafa;border:1px solid #e8e8e8;border-top:3px solid #e05a1b;padding:18px 20px;margin-top:16px}.budget-title{font-family:'Montserrat',sans-serif;font-weight:700;font-size:11px;letter-spacing:2px;text-transform:uppercase;color:#3a3a3a;margin-bottom:12px}.budget-list{list-style:none}.budget-list li{font-size:13px;color:#555;font-weight:300;padding:7px 0;border-bottom:1px solid #eee;display:flex;justify-content:space-between}.budget-list li:last-child{border-bottom:none}.budget-price{color:#e05a1b;font-weight:700}.event-row{display:flex;gap:16px;padding:16px 0;border-bottom:1px solid #f0f0f0;align-items:flex-start}.event-row:last-child{border-bottom:none}.event-date-box{background:#3a3a3a;min-width:52px;text-align:center;padding:10px 8px;flex-shrink:0}.event-month{font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:#999}.event-day-num{font-family:'Montserrat',sans-serif;font-weight:900;font-size:26px;color:#fff;line-height:1.1}.event-name{font-family:'Montserrat',sans-serif;font-weight:700;font-size:15px;color:#3a3a3a;text-transform:uppercase;margin-bottom:4px}.event-detail{font-size:12px;color:#888;line-height:1.6;font-weight:300}.event-tag{display:inline-block;font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:2px;text-transform:uppercase;padding:3px 8px;margin-top:6px}.et-orange{background:#fff3ef;color:#e05a1b}.cta-section{background:#3a3a3a;padding:44px 48px;text-align:center}.cta-headline{font-family:'Montserrat',sans-serif;font-weight:900;font-size:26px;color:#fff;text-transform:uppercase;margin-bottom:8px}.cta-sub{font-size:13px;color:#888;margin-bottom:28px;font-weight:300}.cta-button{display:inline-block;background:#e05a1b;color:#fff;font-family:'Montserrat',sans-serif;font-weight:700;font-size:14px;letter-spacing:2px;text-transform:uppercase;text-decoration:none;padding:18px 44px;border-radius:4px;margin-bottom:16px}.cta-note{font-size:12px;color:#666;font-weight:300}.footer{padding:28px 48px;text-align:center;background:#f2f2f2}.footer-logo{font-family:'Montserrat',sans-serif;font-weight:900;font-size:16px;letter-spacing:2px;color:#bbb;margin-bottom:10px}.social-row{display:flex;justify-content:center;gap:12px;margin:12px 0}.s-link{font-family:'Montserrat',sans-serif;font-size:9px;font-weight:700;letter-spacing:2px;text-transform:uppercase;color:#999;text-decoration:none;border:1px solid #ddd;padding:5px 12px}.footer-text{font-size:11px;color:#aaa;line-height:1.8}.footer-text a{color:#e05a1b;text-decoration:none}`;

  return `<!DOCTYPE html><html lang="en"><head><meta charset="UTF-8"/><meta name="viewport" content="width=device-width,initial-scale=1.0"/><title>LNGVTY Hub Newsletter</title><link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;900&family=Open+Sans:wght@300;400&display=swap" rel="stylesheet"/><style>${css}</style></head><body><div class="email-wrapper"><div class="top-bar">LNGVTY Hub &#x2014; Weekly Member Newsletter</div><div class="hero"><div class="hero-top"><div><div class="logo-text">LNGVTY</div><div class="logo-sub">Longevity Hub</div></div><div class="issue-info"><div class="issue-label">Weekly Newsletter</div><div class="issue-date">${e(S.date)}</div></div></div></div><div class="opener"><div class="opener-quote">${e(S.quote)}</div></div>${note}${sched}${memb}${reci}${tiph}${swps}${evts}${r90}<div class="cta-section"><div class="cta-headline">Ready for This Week?</div><p class="cta-sub">Show up. Stay consistent. The results take care of themselves.</p><a href="https://lngvtyhub.com/" class="cta-button">&raquo; Book Your Session</a><p class="cta-note">Questions? Reply to this email &#x2014; a real person will respond.</p></div><div class="footer"><div class="footer-logo">LNGVTY HUB &#xB7; CARRBORO</div><div class="social-row"><a href="https://www.facebook.com/lngvtyhub" class="s-link">Facebook</a><a href="https://www.instagram.com/lngvtyhub" class="s-link">Instagram</a><a href="https://lngvtyhub.com/" class="s-link">Website</a></div><p class="footer-text">You're receiving this as a valued LNGVTY Hub member.<br/><a href="{{contact.unsubscribe_url}}">Unsubscribe</a></p></div></div></body></html>`;
}

function doCopy(btn){
  const html=buildHTML();
  const orig=btn.textContent;
  const ok=()=>{btn.textContent='✓ Copied!';btn.classList.add('ok');setTimeout(()=>{btn.textContent=orig;btn.classList.remove('ok');},2500);};
  if(navigator.clipboard&&navigator.clipboard.writeText){
    navigator.clipboard.writeText(html).then(ok).catch(()=>{fbCopy(html);ok();});
  } else { fbCopy(html);ok(); }
}
function fbCopy(text){const ta=document.createElement('textarea');ta.value=text;ta.style.cssText='position:fixed;opacity:0;top:0;left:0;';document.body.append(ta);ta.focus();ta.select();try{document.execCommand('copy');}catch(e){}ta.remove();}

function showPreview(){
  const ov=document.getElementById('ov');
  const frame=document.getElementById('pvFrame');
  ov.style.display='flex';
  // Use doc.write for reliable iframe rendering
  const doc=frame.contentDocument||frame.contentWindow.document;
  doc.open();
  doc.write(buildHTML());
  doc.close();
}

// Wire up topbar buttons (these exist in the static HTML, never re-rendered)
document.getElementById('prevBtn').addEventListener('click',showPreview);
document.getElementById('cpBtn').addEventListener('click',function(){doCopy(this);});
document.getElementById('pvClose').addEventListener('click',()=>{document.getElementById('ov').style.display='none';});
document.getElementById('pvCopy').addEventListener('click',function(){doCopy(this);});
document.getElementById('ov').addEventListener('click',function(e){if(e.target===this)this.style.display='none';});

// EDITOR RENDERING
function el(tag,attrs,...kids){
  const e=document.createElement(tag);
  for(const[k,v]of Object.entries(attrs)){
    if(k==='cls')e.className=v;
    else if(k==='on')Object.entries(v).forEach(([ev,fn])=>e.addEventListener(ev,fn));
    else if(k==='style'&&typeof v==='object')Object.assign(e.style,v);
    else e.setAttribute(k,v);
  }
  kids.forEach(k=>k!=null&&e.append(typeof k==='string'?document.createTextNode(k):k));
  return e;
}
function inp(val,cb,ph='',rows=0){
  const e=rows?el('textarea',{placeholder:ph,rows:String(rows)}):el('input',{type:'text',placeholder:ph});
  e.value=val||'';e.addEventListener('input',()=>cb(e.value));return e;
}
function fld(lbl,val,cb,ph='',rows=0){
  const d=el('div',{cls:'field'});d.append(el('label',{},lbl),inp(val,cb,ph,rows));return d;
}

function render(){
  const page=document.getElementById('page');
  page.innerHTML='';

  // BASICS
  const bc=el('div',{cls:'card'});
  const bh=el('div',{cls:'ch',style:{cursor:'default'}});
  bh.append(el('div',{cls:'ct'},'Newsletter Basics',el('span',{cls:'badge req'},'required')));
  const bb=el('div',{cls:'cb'});
  const br=el('div',{cls:'r2'});
  br.append(fld('Date',S.date,v=>{S.date=v;save();},'e.g. June 9, 2026'));
  bb.append(br,fld('Weekly Quote',S.quote,v=>{S.quote=v;save();},"e.g. Show up even when you don't feel like it."));
  bc.append(bh,bb);page.append(bc);

  function sec(title,key,buildFn){
    const card=el('div',{cls:'card'});
    const hdr=el('div',{cls:'ch'});
    const tog=el('button',{cls:'tog'+(S[key]?' on':''),'aria-label':'Toggle'});
    const badge=el('span',{cls:'badge '+(S[key]?'ontag':'offtag')},S[key]?'on':'off');
    hdr.append(el('div',{cls:'ct'},title,badge),tog);
    const toggle=()=>{S[key]=!S[key];save();render();};
    hdr.addEventListener('click',toggle);
    tog.addEventListener('click',e=>{e.stopPropagation();toggle();});
    card.append(hdr);
    if(S[key]){const body=el('div',{cls:'cb'});buildFn(body);card.append(body);}
    page.append(card);
  }

  sec('A Note from Us','includeNote',body=>{
    body.append(fld('Note text',S.noteText,v=>{S.noteText=v;save();},'Write a message to members...',3));
  });

  sec('Weekly Schedule','includeSchedule',body=>{
    S.schedule.forEach((ev,i)=>{
      const li=el('div',{cls:'li'}),f=el('div',{cls:'lif'});
      const r=el('div',{cls:'r2'});
      const di=inp(ev.day,v=>{S.schedule[i].day=v;save();},'e.g. Monday');
      const ni=inp(ev.name,v=>{S.schedule[i].name=v;save();},'e.g. 6am Class');
      r.append(el('div',{cls:'field'},el('label',{},'Day'),di),el('div',{cls:'field'},el('label',{},'Event'),ni));
      const ti=inp(ev.time,v=>{S.schedule[i].time=v;save();},'e.g. 6:00 AM — Open to all');
      f.append(r,el('div',{cls:'field'},el('label',{},'Time & Details'),ti));
      const rm=el('button',{cls:'rm',on:{click:()=>{S.schedule.splice(i,1);save();render();}}},'×');
      li.append(f,rm);body.append(li);
    });
    if(S.schedule.length<7)body.append(el('button',{cls:'addbtn',on:{click:()=>{S.schedule.push({id:uid(),day:'',name:'',time:''});save();render();}}},'+  Add event'));
  });

  sec('New Members','includeMembers',body=>{
    S.members.forEach((m,i)=>{
      const li=el('div',{cls:'li'});
      const ni=inp(m.name,v=>{S.members[i].name=v;save();},'First Last');ni.style.flex='1';
      const rm=el('button',{cls:'rm',on:{click:()=>{S.members.splice(i,1);save();render();}}},'×');
      li.append(ni,rm);body.append(li);
    });
    body.append(el('button',{cls:'addbtn',on:{click:()=>{S.members.push({id:uid(),name:''});save();render();}}},'+  Add member'));
  });

  sec('Weekly Recipe','includeRecipe',body=>{
    const r1=el('div',{cls:'r2'});
    r1.append(fld('Recipe Name',S.recipeName,v=>{S.recipeName=v;save();},'e.g. Sheet Pan Chicken Thighs'),fld('Category',S.recipeCategory,v=>{S.recipeCategory=v;save();},'e.g. High Protein · Keto'));
    body.append(r1,fld('Description',S.recipeDesc,v=>{S.recipeDesc=v;save();},'2–3 sentence description...',2));
    const r2=el('div',{cls:'r3'});
    r2.append(fld('Protein',S.recipeProtein,v=>{S.recipeProtein=v;save();},'e.g. 42g'),fld('Calories',S.recipeCalories,v=>{S.recipeCalories=v;save();},'e.g. 520'),fld('Cost/serving',S.recipeCost,v=>{S.recipeCost=v;save();},'e.g. 3.50'));
    body.append(r2,fld('Prep Time',S.recipePrepTime,v=>{S.recipePrepTime=v;save();},'e.g. 35 min'));
    body.append(el('p',{cls:'sublabel'},'Ingredients'));
    S.ingredients.forEach((ing,i)=>{
      const li=el('div',{cls:'li',style:{padding:'7px 8px'}});
      const ni=inp(ing.v,v=>{S.ingredients[i].v=v;save();},`Ingredient ${i+1}`);ni.style.flex='1';
      const rm=el('button',{cls:'rm',on:{click:()=>{S.ingredients.splice(i,1);save();render();}}},'×');
      li.append(ni,rm);body.append(li);
    });
    body.append(el('button',{cls:'addbtn',on:{click:()=>{S.ingredients.push({id:uid(),v:''});save();render();}}},'+  Add ingredient'));
  });

  sec('Tip Tuesday','includeTip',body=>{
    const r1=el('div',{cls:'r2'});
    r1.append(fld('Category',S.tipCategory,v=>{S.tipCategory=v;save();},'e.g. Mindset'),fld('Title',S.tipTitle,v=>{S.tipTitle=v;save();},'Tip headline'));
    body.append(r1,fld('Body',S.tipBody,v=>{S.tipBody=v;save();},'3–5 sentence tip...',3));
    const r2=el('div',{cls:'r2'}),r3=el('div',{cls:'r2'});
    r2.append(fld('💧 Hydration',S.hydrationTip,v=>{S.hydrationTip=v;save();},'Hydration tip'),fld('🥩 Protein',S.proteinTip,v=>{S.proteinTip=v;save();},'Protein tip'));
    r3.append(fld('😴 Sleep',S.sleepTip,v=>{S.sleepTip=v;save();},'Sleep tip'),fld('🧘 Mobility',S.mobilityTip,v=>{S.mobilityTip=v;save();},'Mobility tip'));
    body.append(r2,r3);
  });

  sec('Nutrition Swaps','includeSwaps',body=>{
    body.append(el('p',{cls:'sublabel'},'Swaps'));
    S.swaps.forEach((sw,i)=>{
      const li=el('div',{cls:'li'}),f=el('div',{cls:'lif'});
      const r1=el('div',{cls:'r2'}),r2=el('div',{cls:'r2'});
      const mkI=(lbl,k,ph)=>{const d=el('div',{cls:'field'});const ii=inp(sw[k],v=>{S.swaps[i][k]=v;save();},ph);d.append(el('label',{},lbl),ii);return d;};
      r1.append(mkI('Instead of','from','e.g. Frappuccino'),mkI('Try this','to','e.g. Iced espresso'));
      r2.append(mkI('Cal/cost from','fromNote','e.g. 400 cal'),mkI('Cal/cost to','toNote','e.g. 80 cal'));
      f.append(r1,r2);
      const rm=el('button',{cls:'rm',on:{click:()=>{S.swaps.splice(i,1);save();render();}}},'×');
      li.append(f,rm);body.append(li);
    });
    if(S.swaps.length<3)body.append(el('button',{cls:'addbtn',on:{click:()=>{S.swaps.push({id:uid(),from:'',fromNote:'',to:'',toNote:''});save();render();}}},'+  Add swap'));
    body.append(el('div',{cls:'divhr'}),el('p',{cls:'sublabel'},'Budget-Friendly Proteins'));
    S.budgetProteins.forEach((p,i)=>{
      const li=el('div',{cls:'li',style:{padding:'7px 8px'}});
      const ni=inp(p.name,v=>{S.budgetProteins[i].name=v;save();},'e.g. Canned tuna');ni.style.flex='1';
      const ci=inp(p.cost,v=>{S.budgetProteins[i].cost=v;save();},'$1.29');ci.style.cssText='width:90px;flex-shrink:0;';
      const rm=el('button',{cls:'rm',on:{click:()=>{S.budgetProteins.splice(i,1);save();render();}}},'×');
      li.append(ni,ci,rm);body.append(li);
    });
    body.append(el('button',{cls:'addbtn',on:{click:()=>{S.budgetProteins.push({id:uid(),name:'',cost:''});save();render();}}},'+  Add protein'));
  });

  sec('Special Events','includeSpecialEvents',body=>{
    S.specialEvents.forEach((ev,i)=>{
      const li=el('div',{cls:'li'}),f=el('div',{cls:'lif'});
      const r1=el('div',{cls:'r2'}),r2=el('div',{cls:'r2'});
      const mkI=(lbl,k,ph)=>{const d=el('div',{cls:'field'});const ii=inp(ev[k],v=>{S.specialEvents[i][k]=v;save();},ph);d.append(el('label',{},lbl),ii);return d;};
      r1.append(mkI('Event Name','name','e.g. Keto Kickoff'),mkI('Tag','tag','e.g. Members Only'));
      r2.append(mkI('Day of Week','dow','e.g. SAT'),mkI('Day Number','dayNum','e.g. 14'));
      const ta=el('textarea',{placeholder:'Short description...',rows:'2'});ta.value=ev.desc||'';ta.addEventListener('input',()=>{S.specialEvents[i].desc=ta.value;save();});
      f.append(r1,r2,el('div',{cls:'field'},el('label',{},'Description'),ta));
      const rm=el('button',{cls:'rm',on:{click:()=>{S.specialEvents.splice(i,1);save();render();}}},'×');
      li.append(f,rm);body.append(li);
    });
    if(S.specialEvents.length<3)body.append(el('button',{cls:'addbtn',on:{click:()=>{S.specialEvents.push({id:uid(),name:'',tag:'',dow:'',dayNum:'',desc:''});save();render();}}},'+  Add event'));
  });

  sec('90-Day Review CTA','include90Day',body=>{
    body.append(el('div',{cls:'hint'},'Static section — shows the standard 90-day check-in reminder with scheduling link. Toggle to include or exclude.'));
  });
}

render();
</script>
