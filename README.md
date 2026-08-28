<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LEWA MAGIL FORUM</title>
<style>
:root{--navy:#0f172a;--indigo:#0f766e;--indigo-dark:#0b5852;--bg:#eef3f4;--card:#fff;--text:#1e293b;--green:#15803d;--red:#dc2626;--orange:#d97706;--muted:#64748b;--whatsapp:#22c55e;--soft:#e6f4f3;--blue:#2563eb}
*{box-sizing:border-box}body{margin:0;font-family:Inter,Arial,sans-serif;background:var(--bg);color:var(--text)}
header{background:linear-gradient(135deg,#0b1526,#0f766e 60%,#14b8a6);color:#fff;padding:20px;position:sticky;top:0;z-index:10;box-shadow:0 4px 18px #0f172a33}
header h1{margin:0;font-size:23px}header p{margin:5px 0 0;opacity:.9}
nav{display:flex;gap:8px;overflow:auto;padding:10px;background:#fff;border-bottom:1px solid #dde5e7;position:sticky;top:82px;z-index:9}
nav button{border:0;background:var(--soft);color:var(--indigo-dark);padding:10px 13px;border-radius:10px;white-space:nowrap;font-weight:700}
nav button.active{background:linear-gradient(135deg,#0f766e,#0b5852);color:#fff}
main{max-width:900px;margin:auto;padding:16px}
.card{background:var(--card);border-radius:15px;padding:16px;box-shadow:0 4px 16px #0f172a12;margin-bottom:14px}
h2{margin-top:4px}.section-title{display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap}
button.primary{background:var(--indigo);color:#fff;border:0;border-radius:9px;padding:10px 14px;font-weight:700;cursor:pointer}
button.secondary{background:var(--soft);color:var(--indigo-dark);border:0;border-radius:9px;padding:8px 12px;font-weight:700;cursor:pointer}
button.danger{background:var(--red);color:#fff;border:0;border-radius:9px;padding:8px 12px;cursor:pointer}
button.whatsapp{background:var(--whatsapp);color:#fff;border:0;border-radius:9px;padding:8px 12px;font-weight:700;cursor:pointer}
a.call,button.call{background:var(--blue);color:#fff;border:0;border-radius:9px;padding:8px 12px;font-weight:700;text-decoration:none;display:inline-block;cursor:pointer}
input,select,textarea{width:100%;padding:10px;border:1px solid #cfd9db;border-radius:9px;background:#fff}
textarea{min-height:100px;resize:vertical}.formgrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:10px}
label{font-size:12px;font-weight:700;color:#3f5254}
.tablewrap{overflow:auto}table{width:100%;border-collapse:collapse;min-width:520px}th,td{padding:10px;border-bottom:1px solid #e5ecec;text-align:left;font-size:13px}th{background:var(--navy);color:#fff;position:sticky;top:0}
.pill{display:inline-block;padding:4px 8px;border-radius:999px;font-size:11px;font-weight:800;margin:2px}.paid{background:#dcfce7;color:#15803d}.pending{background:#fee2e2;color:#dc2626}.partial{background:#fef9c3;color:#a16207}
.small{font-size:12px;color:var(--muted)}
.threerow{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px}
.actions{display:flex;gap:8px;flex-wrap:wrap}
footer{text-align:center;padding:18px;color:var(--muted);font-size:12px}
@media(max-width:600px){main{padding:10px}header{padding:16px}.threerow{grid-template-columns:1fr}}
</style>
</head>
<body>
<header>
<h1>LEWA MAGIL FORUM</h1>
<p>Emergency help fund for friends</p>
</header>
<nav id="nav"></nav>
<main id="app"></main>
<footer>Built by Siloma Kvn</footer>

<script>
const KEY="lewaMagilForumV3";
const accents=["#0f766e","#2563eb","#d97706","#dc2626","#7c3aed","#0891b2"];
const defaultData={
 settings:{status:"Active"},
 paymentTypes:[{id:"T1",name:"Emergency Help",amount:0,dueDate:""},{id:"T2",name:"Augustine's contribution",amount:0,dueDate:""}],
 members:[
  ["M001","Micah lee","","Member"],
  ["M002","Augustine sankale","","Member"],
  ["M003","Lesirnkit","","Member"],
  ["M004","Meitamei Justus","","Member"],
  ["M005","Gideon leperes","","Member"],
  ["M006","Meshack leshao","","Member"],
  ["M007","Kevin leyian","","Member"],
  ["M008","Diof leshamin","","Member"],
  ["M009","Eliakim lekura","","Member"],
  ["M010","Collins lekishon","","Member"],
  ["M011","Morgan","","Member"],
  ["M012","Milton","","Member"],
  ["M013","Emmanuel meritei","","Member"],
  ["M014","Hosea saiguran","","Member"],
  ["M015","Shadrack meitamei","","Member"],
  ["M016","Collins meoli","","Member"],
  ["M017","Brian kosen","","Member"]
 ],
 payments:[
  ["P001","M001","Micah lee",300,"","","","Received","","Augustine's contribution"],
  ["P002","M002","Augustine sankale",300,"","","","Received","","Augustine's contribution"],
  ["P003","M003","Lesirnkit",300,"","","","Received","","Augustine's contribution"],
  ["P004","M004","Meitamei Justus",300,"","","","Received","","Augustine's contribution"],
  ["P005","M005","Gideon leperes",300,"","","","Received","","Augustine's contribution"],
  ["P006","M006","Meshack leshao",300,"","","","Received","","Augustine's contribution"],
  ["P007","M007","Kevin leyian",300,"","","","Received","","Augustine's contribution"],
  ["P008","M008","Diof leshamin",300,"","","","Received","","Augustine's contribution"],
  ["P009","M009","Eliakim lekura",300,"","","","Received","","Augustine's contribution"],
  ["P010","M010","Collins lekishon",300,"","","","Received","","Augustine's contribution"],
  ["P011","M011","Morgan",300,"","","","Received","","Augustine's contribution"],
  ["P012","M012","Milton",300,"","","","Received","","Augustine's contribution"],
  ["P013","M013","Emmanuel meritei",300,"","","","Received","","Augustine's contribution"],
  ["P014","M014","Hosea saiguran",300,"","","","Received","","Augustine's contribution"],
  ["P015","M015","Shadrack meitamei",300,"","","","Pending","","Augustine's contribution"],
  ["P016","M016","Collins meoli",300,"","","","Received","","Augustine's contribution"],
  ["P017","M017","Brian kosen",300,"","","","Pending","","Augustine's contribution"]
 ]
};
let data=JSON.parse(localStorage.getItem(KEY)||JSON.stringify(defaultData));
if(!data.paymentTypes||!data.paymentTypes.length)data.paymentTypes=[{id:"T1",name:"Emergency Help",amount:0,dueDate:""}];
data.paymentTypes.forEach(t=>{if(t.amount===undefined)t.amount=0;if(t.dueDate===undefined)t.dueDate="";if(t.closed===undefined)t.closed=false});
if(!data.settings)data.settings={status:"Active"};
if(!data.settings.activeContribution){data.settings.activeContribution=data.paymentTypes[data.paymentTypes.length-1].name}
data.paymentTypes.forEach(t=>{t.closed=(t.name!==data.settings.activeContribution)});
data.members.forEach(m=>{if(m[2])m[2]=normalizePhone(m[2])});
const tabs=["Home","Contributions","Members","Settings"];
let active="Home", personSearch="";

function save(){localStorage.setItem(KEY,JSON.stringify(data));render()}
function money(n){return "KSh "+Number(n||0).toLocaleString()}
function members(){return data.members}
function memberPayments(name){return data.payments.filter(p=>p[2]===name)}
function normalizePhone(raw){
 if(!raw)return "";
 let d=String(raw).replace(/\D/g,"");
 if(!d)return "";
 if(d.startsWith("254"))return d;
 if(d.startsWith("0"))return "254"+d.slice(1);
 if(d.length===9)return "254"+d;
 return "254"+d.replace(/^0+/,"");
}
function formatPhone(raw){let n=normalizePhone(raw);return n?"+"+n:""}
function livePhonePreview(inputId,hintId){
 let v=document.getElementById(inputId).value;
 document.getElementById(hintId).textContent=v.trim()?"Will be saved as: "+formatPhone(v):"Numbers are automatically formatted to start with +254 when saved.";
}
function openContribution(){return data.paymentTypes.find(t=>!t.closed)||data.paymentTypes[data.paymentTypes.length-1]}
function closedContributions(){return data.paymentTypes.filter(t=>t.closed)}
function memberContribStatus(name,t){
 let recs=data.payments.filter(p=>p[2]===name&&(p[9]||t.name)===t.name);
 let paid=recs.reduce((a,p)=>a+Number(p[3]),0);
 let amt=Number(t.amount||0);
 if(amt<=0){
  if(!recs.length)return {status:"owing",paid:0,amt:0,remaining:0};
  let hasReceived=recs.some(p=>p[7]!=="Pending");
  return {status:hasReceived?"paid":"owing",paid,amt:0,remaining:0};
 }
 if(paid<=0)return {status:"owing",paid,amt,remaining:amt};
 if(paid<amt)return {status:"partial",paid,amt,remaining:amt-paid};
 return {status:"paid",paid,amt,remaining:0};
}
function contributionSummary(t){
 let pays=data.payments.filter(p=>(p[9]||t.name)===t.name);
 let collected=pays.reduce((a,p)=>a+Number(p[3]),0);
 let expectedTotal=Number(t.amount||0)*members().length;
 let paidNames=[],partialNames=[],oweNames=[];
 members().forEach(m=>{
  let st=memberContribStatus(m[1],t);
  if(st.status==="paid")paidNames.push(m[1]);
  else if(st.status==="partial")partialNames.push(m[1]+` (${money(st.remaining)} left)`);
  else oweNames.push(m[1]);
 });
 return {t,name:t.name,amount:Number(t.amount||0),dueDate:t.dueDate||"",collected,expectedTotal,paidNames,partialNames,oweNames,count:pays.length,total:members().length};
}
function nav(){
 document.getElementById("nav").innerHTML=tabs.map(t=>`<button class="${t===active?"active":""}" onclick="go('${t}')">${t}</button>`).join("");
}
function go(t){active=t;render();scrollTo(0,0)}
function render(){
 nav(); let a=document.getElementById("app");
 let fEl=document.activeElement,fId=fEl&&fEl.id,fSel=fId&&typeof fEl.selectionStart==="number"?fEl.selectionStart:null;
 a.innerHTML=views[active]();
 if(fId){let el=document.getElementById(fId);if(el){el.focus();if(fSel!=null&&el.setSelectionRange){try{el.setSelectionRange(fSel,fSel)}catch(e){}}}}
}
const views={
Home:()=>{
 let t=openContribution();
 let s=contributionSummary(t);
 return `<div class="section-title"><h2>Current Contribution</h2><button class="primary" onclick="startNewContribution()">+ Start New Contribution</button></div>
 <div class="card" style="border-left:6px solid var(--indigo)">
 <h3 style="margin:0 0 6px">${t.name}</h3>
 <p class="small">${t.amount?`KSh ${money(t.amount).replace("KSh ","")} per person`:"Custom amount per person"}${t.dueDate?` · due ${t.dueDate}`:""}</p>
 <p><b>${money(s.collected)}</b> collected${s.expectedTotal?` of ${money(s.expectedTotal)} expected`:""} · <b>${s.paidNames.length}/${s.total}</b> cleared</p>
 <div class="actions"><button class="secondary" onclick="contributionDetail('${t.id}')">View Full List</button><button class="whatsapp" onclick="remindAllOwing()">Remind those who haven't cleared</button><button class="secondary" onclick="announceCurrent()">📢 Announce</button><button class="secondary" onclick="bulkPaymentForm()">Paste a list</button></div>
 </div>
 <div class="card"><h3 style="margin-top:0">See Every Contribution</h3><p class="small">Augustine's contribution, Meshack's contribution, this one, and any others — each shown separately with its own color, so nothing gets mixed up.</p><button class="primary" onclick="go('Contributions')">Open Contributions List</button></div>`
},
Members:()=>{
 let q=personSearch.trim().toLowerCase();
 let list=members().filter(m=>!q||m[1].toLowerCase().includes(q));
 return `<div class="section-title"><h2>Members</h2><button class="primary" onclick="memberForm()">+ Add Member</button></div>
 <div class="card"><label>Search by name</label><input id="memberSearch" value="${personSearch}" placeholder="Type a name..." oninput="personSearch=this.value;render()"></div>
 <div class="card"><div class="tablewrap"><table><tr><th>Name</th><th>Number</th><th>Total Contributed (all time)</th><th>Action</th></tr>
 ${list.map(m=>{let paid=memberPayments(m[1]).reduce((a,p)=>a+Number(p[3]),0);
 return `<tr><td>${m[1]}</td><td>${m[2]?formatPhone(m[2]):"—"}</td><td>${money(paid)}</td><td class="actions">
 <button class="primary" onclick="editMember('${m[0]}')">Edit</button>
 ${m[2]?`<button class="whatsapp" onclick="contactMember('${m[0]}')">Contact</button>`:""}
 </td></tr>`}).join("")||'<tr><td colspan="4">No members match.</td></tr>'}</table></div></div>`
},
Contributions:()=>{
 let list=data.paymentTypes.slice().reverse();
 return `<h2>All Contributions</h2>
 <div class="card"><p class="small">Every contribution — Augustine's, Meshack's, this one, and any others — shown as its own card with its own color. Tap one to see everything about it.</p></div>
 ${list.map((t,i)=>{let s=contributionSummary(t);let color=accents[i%accents.length];
 return `<div class="card" style="border-left:6px solid ${color}">
 <div class="section-title"><h3 style="margin:0">${t.name}</h3>${t.closed?'<span class="pill" style="background:#e2e8f0;color:#475569">Closed</span>':'<span class="pill paid">🟢 Open</span>'}</div>
 <p class="small">${t.amount?`KSh ${t.amount} per person`:"Custom amount"}${t.dueDate?` · due ${t.dueDate}`:""}</p>
 <p><b>${money(s.collected)}</b> collected${s.expectedTotal?` of ${money(s.expectedTotal)} expected`:""} · <span style="color:var(--green);font-weight:700">${s.paidNames.length} cleared</span>${s.partialNames.length?` · <span style="color:#a16207;font-weight:700">${s.partialNames.length} partly paid</span>`:""}${s.oweNames.length?` · <span style="color:var(--red);font-weight:700">${s.oweNames.length} not paid</span>`:""}</p>
 <button class="secondary" onclick="contributionDetail('${t.id}')">View Details</button>
 </div>`}).join("")}`
},
Settings:()=>`<h2>Settings</h2>
<div class="card"><div class="formgrid"><div><label>Fund status</label><select id="setStatus"><option ${data.settings.status==="Active"?"selected":""}>Active</option><option ${data.settings.status==="Closed"?"selected":""}>Closed</option></select></div></div><br><button class="primary" onclick="saveSettings()">Save</button></div>
<div class="card"><h3>Data</h3><div class="actions"><button class="primary" onclick="exportData()">Export Backup</button><button class="danger" onclick="resetData()">Reset All Data</button></div></div>`
};
function modal(title,body){
 let old=document.getElementById("modal");if(old)old.remove();
 let d=document.createElement("div");d.id="modal";d.style="position:fixed;inset:0;background:#0008;display:flex;align-items:center;justify-content:center;padding:15px;z-index:100";
 d.innerHTML=`<div class="card" style="width:min(600px,100%);max-height:90vh;overflow:auto"><div class="section-title"><h2>${title}</h2><button class="secondary" onclick="document.getElementById('modal').remove()">✕</button></div>${body}</div>`;document.body.appendChild(d);
}
function startNewContribution(){
 modal("Start New Contribution",`<p class="small">This closes the current contribution and starts a fresh one. Every member starts this new round as owing until logged as paid.</p>
 <div class="formgrid">
 <div><label>Contribution Name</label><input id="scName" placeholder="e.g. Hospital Bill Help"></div>
 <div><label>Amount per person (KSh)</label><input id="scAmount" type="number" value="300"></div>
 <div><label>Due Date (optional)</label><input id="scDue" type="date"></div>
 </div>
 <br><button class="primary" onclick="saveNewContribution()">Start Contribution</button>`);
}
function saveNewContribution(){
 let n=document.getElementById("scName").value.trim();
 let amt=Number(document.getElementById("scAmount").value||0);
 let due=document.getElementById("scDue").value||"";
 if(!n)return alert("Give this contribution a name");
 if(data.paymentTypes.find(x=>x.name.toLowerCase()===n.toLowerCase()))return alert("A contribution with that name already exists — choose a different name");
 let cur=openContribution();if(cur)cur.closed=true;
 data.paymentTypes.push({id:"T"+Date.now(),name:n,amount:amt,dueDate:due,closed:false});
 data.settings.activeContribution=n;
 document.getElementById("modal").remove();
 save();
 modal("Contribution started 🎉",`<p><b>${n}</b> is now open${amt?` — everyone is expected to pay ${money(amt)}`:""}${due?`, due by ${due}`:""}. The previous contribution has been closed — you'll find every contribution, open and closed, in the Contributions tab.</p>
 <div class="actions" style="margin-top:10px"><button class="whatsapp" onclick="document.getElementById('modal').remove();announceCurrent()">📢 Announce to Everyone</button><button class="secondary" onclick="document.getElementById('modal').remove()">Done</button></div>`);
}
function logPayment(id){
 let t=openContribution();
 let m=members().find(x=>x[0]===id);if(!m)return;
 let st=memberContribStatus(m[1],t);
 modal("Log Payment — "+m[1],`<div class="formgrid">
 <div><label>Amount (KSh)</label><input id="lpAmt" type="number" value="${st.remaining||t.amount||0}"></div>
 <div><label>Method</label><select id="lpMethod"><option>M-Pesa</option><option>Cash</option><option>Bank</option><option>Other</option></select></div>
 <div><label>Date</label><input id="lpDate" type="date" value="${new Date().toISOString().slice(0,10)}"></div>
 </div>
 <br><button class="primary" onclick="saveLoggedPayment('${id}')">Save Payment</button>`);
}
function saveLoggedPayment(id){
 let t=openContribution();
 let m=members().find(x=>x[0]===id);if(!m)return;
 let amt=Number(document.getElementById("lpAmt").value||0);
 if(!amt)return alert("Enter an amount");
 let method=document.getElementById("lpMethod").value;
 let date=document.getElementById("lpDate").value||new Date().toISOString().slice(0,10);
 let p=["P"+String(data.payments.length+1).padStart(3,"0"),id,m[1],amt,date,new Date().toTimeString().slice(0,5),method,"Received","",t.name];
 data.payments.push(p);
 document.getElementById("modal").remove();
 save();
}
function reminderMsgFor(name){
 let t=openContribution();
 let st=memberContribStatus(name,t);
 if(st.status==="paid")return `Hello ${name}, you have already cleared your contribution for ${t.name}. Thank you!`;
 if(t.amount>0)return `Hello ${name}, this is a reminder from LEWA MAGIL FORUM. You need ${money(st.remaining)} to complete your contribution for ${t.name}. Thank you!`;
 return `Hello ${name}, this is a reminder from LEWA MAGIL FORUM. Kindly make your contribution for ${t.name} when you're able. Thank you!`;
}
function contactMsgFor(name){return `Hello ${name}, greetings from LEWA MAGIL FORUM.`}
function sendWA(phone,taId){let msg=document.getElementById(taId).value;let n=normalizePhone(phone);window.open("https://wa.me/"+n+"?text="+encodeURIComponent(msg),"_blank")}
function sendSMS(phone,taId){let msg=document.getElementById(taId).value;let n=normalizePhone(phone);window.open("sms:"+n+"?&body="+encodeURIComponent(msg))}
function remindMember(id){
 let m=members().find(x=>x[0]===id);if(!m)return;
 let msg=reminderMsgFor(m[1]);
 modal("Reminder — "+m[1],`<label>Message (edit if needed)</label><textarea id="rMsg">${msg}</textarea><br><br><p>Choose how to reach ${m[1]}:</p><div class="actions">${m[2]?`<a class="call" href="tel:${m[2]}">Call</a><button class="whatsapp" onclick="sendWA('${m[2]}','rMsg')">WhatsApp</button><button class="secondary" onclick="sendSMS('${m[2]}','rMsg')">SMS</button>`:"<p class='small'>No number on file for this member.</p>"}</div>`);
}
function contactMember(id){
 let m=members().find(x=>x[0]===id);if(!m)return;
 let msg=contactMsgFor(m[1]);
 modal("Contact — "+m[1],`<label>Message (edit if needed)</label><textarea id="cMsg">${msg}</textarea><br><br><p>Choose how to reach ${m[1]}:</p><div class="actions">${m[2]?`<a class="call" href="tel:${m[2]}">Call</a><button class="whatsapp" onclick="sendWA('${m[2]}','cMsg')">WhatsApp</button><button class="secondary" onclick="sendSMS('${m[2]}','cMsg')">SMS</button>`:"<p class='small'>No number on file for this member.</p>"}</div>`);
}
function remindAllOwing(){
 let t=openContribution();
 let notPaid=members().map(m=>({m,st:memberContribStatus(m[1],t)})).filter(x=>x.st.status!=="paid");
 if(!notPaid.length)return modal(t.name,`<p>Everyone has cleared <b>${t.name}</b> 🎉</p>`);
 modal("Remind — "+t.name,`<div class="tablewrap"><table><tr><th>Name</th><th>Status</th><th>Number</th><th>Action</th></tr>${notPaid.map(x=>`<tr style="background:${x.st.status==="partial"?"#fffbeb":"#fef2f2"}"><td>${x.m[1]}</td><td><span class="pill ${x.st.status==="partial"?"partial":"pending"}">${x.st.status==="partial"?`Owes ${money(x.st.remaining)}`:(t.amount?`Owes ${money(t.amount)}`:"Not paid")}</span></td><td>${x.m[2]?formatPhone(x.m[2]):"Missing"}</td><td class="actions">${x.m[2]?`<a class="call" href="tel:${x.m[2]}">Call</a><button class="whatsapp" onclick="remindMember('${x.m[0]}')">Message</button>`:"—"}</td></tr>`).join("")}</table></div>`);
}
function announceCurrent(){
 let t=openContribution();
 let msg=`Hello everyone! A new contribution is open: *${t.name}*.${t.amount?` Please contribute ${money(t.amount)} each.`:""}${t.dueDate?` Deadline: ${t.dueDate}.`:""} Thank you!`;
 modal("Announce — "+t.name,`<label>Message (edit if needed)</label><textarea id="announceMsg">${msg}</textarea><br><br><p>Send to everyone at once:</p><div class="actions"><button class="whatsapp" onclick="broadcastSMS('announceMsg')">SMS All (bulk)</button><button class="secondary" onclick="broadcastWAList('announceMsg','announceWAList')">WhatsApp All (one by one)</button></div><div id="announceWAList"></div>`);
}
function broadcastSMS(taId){
 let list=members().filter(m=>m[2]);
 if(!list.length)return alert("No members with a WhatsApp/phone number yet.");
 let numbers=list.map(m=>normalizePhone(m[2])).join(",");
 let body=document.getElementById(taId).value;
 window.open("sms:"+numbers+"?&body="+encodeURIComponent(body));
}
function broadcastWAList(taId,divId){
 let list=members().filter(m=>m[2]);
 document.getElementById(divId).innerHTML=`<div class="tablewrap" style="margin-top:10px"><table><tr><th>Name</th><th>Action</th></tr>${list.map(m=>`<tr><td>${m[1]}</td><td><button class="whatsapp" onclick="sendWA('${m[2]}','${taId}')">Open WhatsApp</button></td></tr>`).join("")||'<tr><td colspan="2">No members with numbers yet.</td></tr>'}</table></div>`;
}
function contributionDetail(id){
 let t=data.paymentTypes.find(x=>x.id===id);if(!t)return;
 let s=contributionSummary(t);
 let isOpen=!t.closed;
 let rows=members().map(m=>{
  let st=memberContribStatus(m[1],t);
  let pillClass=st.status==="paid"?"paid":st.status==="partial"?"partial":"pending";
  let rowBg=st.status==="paid"?"#f0fdf4":st.status==="partial"?"#fffbeb":"#fef2f2";
  let label=st.status==="paid"?"Cleared":st.status==="partial"?`Owes ${money(st.remaining)}`:(t.amount?`Owes ${money(t.amount)}`:"Not paid");
  let actionBtns=isOpen?`<button class="primary" onclick="logPayment('${m[0]}')">Log Payment</button>${st.status!=="paid"&&m[2]?`<button class="whatsapp" onclick="remindMember('${m[0]}')">Remind</button>`:""}`:"";
  return `<tr style="background:${rowBg}"><td>${m[1]}</td><td><span class="pill ${pillClass}">${label}</span></td><td>${m[2]?formatPhone(m[2]):"—"}</td><td class="actions">${actionBtns}</td></tr>`;
 }).join("");
 modal(t.name,`<p class="small">${isOpen?'<span class="pill paid">🟢 Open</span>':'<span class="pill" style="background:#e2e8f0;color:#475569">Closed</span>'} ${t.amount?`· KSh ${t.amount} per person`:"· Custom amount"}${t.dueDate?` · due ${t.dueDate}`:""}</p>
 <p><b>${money(s.collected)}</b> collected${s.expectedTotal?` of ${money(s.expectedTotal)} expected`:""} · ${s.paidNames.length}/${s.total} cleared</p>
 ${isOpen?`<div class="actions" style="margin-bottom:10px"><button class="whatsapp" onclick="remindAllOwing()">Remind all who haven't cleared</button><button class="secondary" onclick="announceCurrent()">📢 Announce</button></div>`:""}
 <div class="tablewrap"><table><tr><th>Name</th><th>Status</th><th>Number</th><th>Action</th></tr>${rows}</table></div>`);
}
function memberForm(existing=null){
 let m=existing||["","","",""];
 modal(existing?"Edit Member":"Add Member",`<div class="formgrid"><div><label>Full Name</label><input id="mn" value="${m[1]}"></div><div><label>WhatsApp Number</label><input id="mp" value="${m[2]}" placeholder="07... or 254..." oninput="livePhonePreview('mp','mpHint')"></div></div><p class="small" id="mpHint">Numbers are automatically formatted to start with +254 when saved.</p><br><button class="primary" onclick="saveMember('${m[0]}')">Save</button>`);
}
function editMember(id){memberForm(members().find(x=>x[0]===id))}
function saveMember(id){
 let n=document.getElementById("mn").value.trim(),p=formatPhone(document.getElementById("mp").value.trim());
 if(!n)return alert("Name is required");
 if(id){let m=members().find(x=>x[0]===id);m[1]=n;m[2]=p}else members().push(["M"+String(members().length+1).padStart(3,"0"),n,p,"Member"]);
 document.getElementById("modal").remove();save();
}
function parseBulkLine(line){
 line=line.replace(/^\s*\d+[\.\)]?\s+(?=[A-Za-z])/,"");
 let nums=[...line.matchAll(/\d+(\.\d+)?/g)];
 if(!nums.length)return null;
 let last=nums[nums.length-1];
 let amount=Number(last[0]);
 let name=line.slice(0,last.index).trim().replace(/[-=:]+$/,"").trim().replace(/\s{2,}/g," ");
 if(!name)return null;
 return {name,amount};
}
function bulkPaymentForm(){
 let t=openContribution();
 modal("Paste a List — "+t.name,`<p class="small">Paste one person per line — this is added to the current open contribution: <b>${t.name}</b>.</p>
 <div class="formgrid">
 <div><label>Date</label><input id="bulkDate" type="date" value="${new Date().toISOString().slice(0,10)}"></div>
 <div><label>Method</label><select id="bulkMethod"><option>M-Pesa</option><option>Cash</option><option>Bank</option><option>Other</option></select></div>
 </div>
 <label>Paste the list — one member per line</label>
 <textarea id="bulkText" rows="10" placeholder="1. Micah lee=300-cleared✅
2. Augustine sankale =300 cleared ✅"></textarea>
 <p class="small">Numbering, "cleared", checkmarks are ignored automatically — the last number on each line is used as that person's amount. New names are added to Members automatically.</p>
 <br><button class="primary" onclick="runBulkImport()">Preview &amp; Import</button>`);
}
function runBulkImport(){
 let t=openContribution();
 let date=document.getElementById("bulkDate").value||new Date().toISOString().slice(0,10);
 let method=document.getElementById("bulkMethod").value;
 let text=document.getElementById("bulkText").value;
 let lines=text.split("\n").map(l=>l.trim()).filter(Boolean);
 let entries=[],statedTotal=null;
 lines.forEach(line=>{
  if(/mpesa|cash|total|treasur/i.test(line)){
   let mnum=line.match(/\d+(\.\d+)?/);
   if(/total/i.test(line)&&mnum)statedTotal=Number(mnum[0]);
   return;
  }
  let parsed=parseBulkLine(line);
  if(parsed)entries.push(parsed);
 });
 if(!entries.length){alert("No member/amount lines were recognized. Check the format and try again.");return}
 entries.forEach(e=>{
  let existing=members().find(m=>m[1].toLowerCase()===e.name.toLowerCase());
  let id;
  if(existing)id=existing[0];
  else{id="M"+String(members().length+1).padStart(3,"0");members().push([id,e.name,"","Member"])}
  let m=members().find(x=>x[0]===id);
  let p=["P"+String(data.payments.length+1).padStart(3,"0"),id,m[1],e.amount,date,new Date().toTimeString().slice(0,5),method,"Received","",t.name];
  data.payments.push(p);
 });
 let sum=entries.reduce((a,e)=>a+e.amount,0);
 save();
 let old=document.getElementById("modal");if(old)old.remove();
 let mismatchNote=(statedTotal!==null&&statedTotal!==sum)?`<p style="color:#b91c1c">⚠️ Heads up: the total in your paste (${money(statedTotal)}) doesn't match the sum of the ${entries.length} lines imported (${money(sum)}). Everything below has already been added — double-check the list and edit in Log Payment if a number was off.</p>`:"";
 modal("Import complete",`<p>Added ${entries.length} payment(s) to <b>${t.name}</b> totalling <b>${money(sum)}</b>.</p>${mismatchNote}<button class="secondary" onclick="document.getElementById('modal').remove()">Close</button>`);
}
function saveSettings(){data.settings.status=document.getElementById("setStatus").value;save()}
function exportData(){let blob=new Blob([JSON.stringify(data,null,2)],{type:"application/json"}),a=document.createElement("a");a.href=URL.createObjectURL(blob);a.download="LEWA_MAGIL_FORUM_backup.json";a.click();URL.revokeObjectURL(a.href)}
function resetData(){if(confirm("Reset all saved data? This clears members and payments.")){localStorage.removeItem(KEY);location.reload()}}
render();
</script>
</body>
</html>
