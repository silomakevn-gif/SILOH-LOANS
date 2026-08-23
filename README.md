<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LEWA MAGIL FORUM</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<style>
:root{--navy:#1c130d;--indigo:#6b3410;--violet:#8b4513;--teal:#5c3317;--bg:#f6f0e8;--card:#fff;--text:#241a14;--green:#15803d;--red:#b91c1c;--orange:#a0522d;--muted:#7a6a5c;--whatsapp:#20b858}
*{box-sizing:border-box}body{margin:0;font-family:Inter,Arial,sans-serif;background:var(--bg);color:var(--text)}
header{background:linear-gradient(135deg,#140d09,#6b3410 55%,#b91c1c);color:#fff;padding:20px;position:sticky;top:0;z-index:10;box-shadow:0 4px 18px #14090433}
header h1{margin:0;font-size:23px}header p{margin:5px 0 0;opacity:.9}
nav{display:flex;gap:8px;overflow:auto;padding:10px;background:#fff;border-bottom:1px solid #ddd;position:sticky;top:82px;z-index:9}
nav button{border:0;background:#f1e7db;color:var(--navy);padding:10px 13px;border-radius:10px;white-space:nowrap;font-weight:700}
nav button.active{background:linear-gradient(135deg,#6b3410,#b91c1c);color:#fff}
main{max-width:1250px;margin:auto;padding:16px}.tab{display:none}.tab.active{display:block}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(190px,1fr));gap:12px}
.card{background:var(--card);border-radius:15px;padding:16px;box-shadow:0 4px 16px #1c130d12;margin-bottom:14px}
.stat{font-size:25px;font-weight:800;margin-top:8px}.label{color:var(--muted);font-size:13px;font-weight:600}
h2{margin-top:4px}.section-title{display:flex;justify-content:space-between;align-items:center;gap:10px;flex-wrap:wrap}
button.primary{background:var(--indigo);color:#fff;border:0;border-radius:9px;padding:10px 14px;font-weight:700;cursor:pointer}
button.secondary{background:#ece0d3;color:var(--navy);border:0;border-radius:9px;padding:8px 12px;font-weight:700;cursor:pointer}
button.danger{background:var(--red);color:#fff;border:0;border-radius:9px;padding:8px 12px;cursor:pointer}
button.whatsapp{background:var(--whatsapp);color:#fff;border:0;border-radius:9px;padding:8px 12px;font-weight:700;cursor:pointer}
a.call,button.call{background:var(--teal);color:#fff;border:0;border-radius:9px;padding:8px 12px;font-weight:700;text-decoration:none;display:inline-block;cursor:pointer}
input,select,textarea{width:100%;padding:10px;border:1px solid #d8c9b8;border-radius:9px;background:#fff}
textarea{min-height:100px;resize:vertical}.formgrid{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:10px}
label{font-size:12px;font-weight:700;color:#5c4a3a}
.tablewrap{overflow:auto}table{width:100%;border-collapse:collapse;min-width:680px}th,td{padding:10px;border-bottom:1px solid #e8ddd0;text-align:left;font-size:13px}th{background:var(--navy);color:#fff;position:sticky;top:0}.pill{display:inline-block;padding:4px 8px;border-radius:999px;font-size:11px;font-weight:800}.paid{background:#dcfce7;color:#15803d}.pending{background:#fde2e2;color:#b91c1c}
.small{font-size:12px;color:var(--muted)}.notice{background:#fff8e8;border-left:4px solid var(--orange);padding:12px;border-radius:8px;margin-bottom:12px}
canvas{max-height:330px}.actions{display:flex;gap:8px;flex-wrap:wrap}
footer{text-align:center;padding:18px;color:var(--muted);font-size:12px}
@media(max-width:600px){main{padding:10px}header{padding:16px}.stat{font-size:22px}}
</style>
</head>
<body>
<header>
<h1>LEWA MAGIL FORUM</h1>
<p>Emergency help fund for friends • Balance • Payments • WhatsApp &amp; SMS</p>
</header>
<nav id="nav"></nav>
<main id="app"></main>
<footer>Built by Siloma Kvn</footer>

<script>
const KEY="lewaMagilForumV3";
const accents=["#6b3410","#b91c1c","#3e2417","#8b4513","#a0522d","#1c130d"];
const defaultData={
 settings:{status:"Active"},
 paymentTypes:[{id:"T1",name:"Emergency Help",amount:0,dueDate:""}],
 rules:[
  "This fund supports emergency help between friends — contributions can happen any time, not on a fixed monthly schedule.",
  "Anyone can contribute whenever they are able; there is no fixed required amount unless agreed for a specific need.",
  "Record the giver's name, amount, date and time whenever possible.",
  "The current balance should be shared with the group after a contribution so everyone stays informed.",
  "WhatsApp and phone numbers are used for official communication about the fund.",
  "Keep contact numbers up to date so notifications reach everyone."
 ],
 members:[],
 payments:[]
};
let data=JSON.parse(localStorage.getItem(KEY)||JSON.stringify(defaultData));
if(!data.paymentTypes||!data.paymentTypes.length)data.paymentTypes=[{id:"T1",name:"Emergency Help",amount:0,dueDate:""}];
data.paymentTypes.forEach(t=>{if(t.amount===undefined)t.amount=0;if(t.dueDate===undefined)t.dueDate=""});
if(!data.settings)data.settings={status:"Active"};
if(data.settings.lateFineAmount===undefined)data.settings.lateFineAmount=0;
if(data.settings.lateFineType===undefined)data.settings.lateFineType="fixed";
data.members.forEach(m=>{if(m[2])m[2]=normalizePhone(m[2])});
const tabs=["Dashboard","Members","Payments","Reminders","Rules","WhatsApp","Reports","Settings"];
let active="Dashboard", chart1, chart2, paymentsFilter="All";
let waTemplate="Hello {NAME}, quick update from LEWA MAGIL FORUM: current balance is {BALANCE}. Thank you for being part of the circle.";
let reminderTemplate="Hello {NAME}, this is a reminder from LEWA MAGIL FORUM. You currently owe {OWED} for {CATEGORY} (includes {FINE} in late fines if applicable). Status: {STATUS}. Kindly make your contribution when you're able. Current fund balance: {BALANCE}. Thank you!";

function save(){localStorage.setItem(KEY,JSON.stringify(data));render()}
function money(n){return "KSh "+Number(n||0).toLocaleString()}
function members(){return data.members}
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
function memberPayments(name){return data.payments.filter(p=>p[2]===name)}
function defaultTypeName(){return data.paymentTypes[0]?data.paymentTypes[0].name:"Emergency Help"}
function typeAmount(name){let t=data.paymentTypes.find(x=>x.name===name);return t?Number(t.amount||0):0}
function typeDue(name){let t=data.paymentTypes.find(x=>x.name===name);return t?(t.dueDate||""):""}
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
function categoryTotals(){
 return data.paymentTypes.map(t=>({name:t.name,total:data.payments.filter(p=>(p[9]||defaultTypeName())===t.name).reduce((a,p)=>a+Number(p[3]),0),count:data.payments.filter(p=>(p[9]||defaultTypeName())===t.name).length}));
}
function lateFineFor(daysLate){
 if(!daysLate||daysLate<=0)return 0;
 let amt=Number(data.settings.lateFineAmount||0);
 if(!amt)return 0;
 return data.settings.lateFineType==="perday"?amt*daysLate:amt;
}
function memberCategoryStats(name){
 return data.paymentTypes.filter(t=>Number(t.amount||0)>0).map(t=>{
  let paid=data.payments.filter(p=>p[2]===name&&(p[9]||defaultTypeName())===t.name).reduce((a,p)=>a+Number(p[3]),0);
  let expected=Number(t.amount||0);
  let balance=Math.max(0,expected-paid);
  let overdue=false,daysLate=0;
  if(balance>0&&t.dueDate){let due=new Date(t.dueDate+"T00:00:00"),now=new Date();if(now>due){overdue=true;daysLate=Math.floor((now-due)/86400000)}}
  let fine=overdue?lateFineFor(daysLate):0;
  return {type:t.name,expected,paid,balance,overdue,daysLate,fine,dueDate:t.dueDate||""};
 });
}
function memberTotalDue(name){return memberCategoryStats(name).reduce((a,c)=>a+c.balance+c.fine,0)}
function memberIsLate(name){return memberCategoryStats(name).some(c=>c.overdue)}
function buildReminderMsg(name){
 let cs=memberCategoryStats(name).filter(c=>c.balance>0);
 let owed=cs.reduce((a,c)=>a+c.balance+c.fine,0);
 let fineTotal=cs.reduce((a,c)=>a+c.fine,0);
 let catList=cs.map(c=>`${c.type}: ${money(c.balance)}${c.overdue?` + ${money(c.fine)} late fine (${c.daysLate}d late)`:""}`).join(", ")||"—";
 let late=cs.some(c=>c.overdue);
 return reminderTemplate.replaceAll("{NAME}",name).replaceAll("{OWED}",money(owed)).replaceAll("{FINE}",money(fineTotal)).replaceAll("{CATEGORY}",catList).replaceAll("{STATUS}",late?"LATE":"Pending").replaceAll("{BALANCE}",money(stats().balance));
}
function stats(){
 let balance=data.payments.reduce((a,p)=>a+Number(p[3]),0);
 let received=data.payments.filter(p=>p[7]==="Received").length;
 let pending=data.payments.filter(p=>p[7]==="Pending").length;
 let arrears=members().filter(m=>memberTotalDue(m[1])>0).length;
 return {balance,received,pending,arrears};
}
function nav(){
 document.getElementById("nav").innerHTML=tabs.map(t=>`<button class="${t===active?"active":""}" onclick="go('${t}')">${t}</button>`).join("");
}
function go(t){active=t;render();scrollTo(0,0)}
function render(){
 nav(); let a=document.getElementById("app");
 a.innerHTML=views[active](); bindCharts();
}
const views={
Dashboard:()=>{let s=stats();let sorted=[...data.payments].sort((a,b)=>(b[4]+" "+b[5]).localeCompare(a[4]+" "+a[5]));let cats=categoryTotals();
 return `<div class="section-title"><h2>Fund Dashboard</h2><button class="primary" onclick="exportData()">Export Backup</button></div>
 <div class="grid">
 ${[['Members',members().length],['Current Balance',money(s.balance)],['Received',s.received],['Pending',s.pending],['Members in Arrears',s.arrears],['Contributions Logged',data.payments.length]].map((x,i)=>`<div class="card" style="border-left:6px solid ${accents[i%accents.length]}"><div class="label">${x[0]}</div><div class="stat">${x[1]}</div></div>`).join("")}
 </div>
 <div class="card"><h3>Totals by Category</h3><div class="tablewrap"><table><tr><th>Category</th><th>Total Collected</th><th>Contributions</th></tr>
 ${cats.map(c=>`<tr><td>${c.name}</td><td>${money(c.total)}</td><td>${c.count}</td></tr>`).join("")}</table></div></div>
 <div class="grid"><div class="card"><h3>Payment Status</h3><canvas id="statusChart"></canvas></div><div class="card"><h3>Contributions by Member</h3><canvas id="memberChart"></canvas></div></div>
 <div class="card"><h3>Recent Contributions</h3><div class="tablewrap"><table><tr><th>Name</th><th>Category</th><th>Date</th><th>Time</th><th>Amount</th></tr>
 ${sorted.slice(0,10).map(p=>`<tr><td>${p[2]}</td><td>${p[9]||defaultTypeName()}</td><td>${p[4]}</td><td>${p[5]}</td><td>${money(p[3])}</td></tr>`).join("")||'<tr><td colspan="5">No contributions logged yet.</td></tr>'}</table></div></div>`},
Members:()=>`<div class="section-title"><h2>Members</h2><div class="actions"><button class="whatsapp" onclick="contactAll()">Contact All</button><button class="primary" onclick="memberForm()">+ Add Member</button></div></div>
 <div class="card"><div class="tablewrap"><table><tr><th>ID</th><th>Name</th><th>WhatsApp</th><th>Total Contributed</th><th>Times</th><th>Status</th><th>Action</th></tr>
 ${members().map(m=>{let ps=memberPayments(m[1]), paid=ps.reduce((a,p)=>a+p[3],0);let due=memberTotalDue(m[1]);let late=memberIsLate(m[1]);
 return `<tr><td>${m[0]}</td><td>${m[1]}</td><td>${m[2]||"—"}</td><td>${money(paid)}</td><td>${ps.length}</td><td>${due>0?`<span class="pill pending">${late?"Late":"Owes"} ${money(due)}</span>`:'<span class="pill paid">Up to date</span>'}</td><td class="actions">
 <button class="primary" onclick="editMember('${m[0]}')">Edit</button>
 <button class="secondary" onclick="paymentForm('${m[0]}')">+ Payment</button>
 ${m[2]?`<button class="whatsapp" onclick="messageOne('${m[0]}')">Contact</button>`:""}
 </td></tr>`}).join("")||'<tr><td colspan="7">No members added yet.</td></tr>'}</table></div></div>`,
Payments:()=>`<div class="section-title"><h2>Payment History</h2><div class="actions"><button class="secondary" onclick="bulkPaymentForm()">+ Bulk Add (paste a list)</button><button class="primary" onclick="paymentForm()">+ Add Payment</button></div></div>
 <div class="card"><label>Filter by payment type</label><select onchange="paymentsFilter=this.value;render()"><option ${paymentsFilter==="All"?"selected":""}>All</option>${data.paymentTypes.map(t=>`<option ${paymentsFilter===t.name?"selected":""}>${t.name}</option>`).join("")}</select></div>
 <div class="card"><div class="tablewrap"><table><tr><th>ID</th><th>Name</th><th>Type</th><th>Amount</th><th>Date</th><th>Time</th><th>Method</th><th>Status</th></tr>
 ${data.payments.filter(p=>paymentsFilter==="All"||(p[9]||defaultTypeName())===paymentsFilter).map(p=>`<tr><td>${p[0]}</td><td>${p[2]}</td><td>${p[9]||defaultTypeName()}</td><td>${money(p[3])}</td><td>${p[4]}</td><td>${p[5]}</td><td>${p[6]}</td><td><span class="pill ${p[7]==="Pending"?"pending":"paid"}">${p[7]}</span></td></tr>`).join("")||'<tr><td colspan="8">No payments logged yet.</td></tr>'}</table></div></div>`,
Reminders:()=>{
 let list=members().map(m=>({m,cs:memberCategoryStats(m[1])})).map(x=>({...x,owed:x.cs.reduce((a,c)=>a+c.balance+c.fine,0),fineTotal:x.cs.reduce((a,c)=>a+c.fine,0),late:x.cs.some(c=>c.overdue)})).filter(x=>x.owed>0).sort((a,b)=>b.owed-a.owed);
 return `<div class="section-title"><h2>Payment Reminders</h2></div>
 <div class="notice">Owed amounts are calculated automatically: (Expected Amount set per Category in Settings) − (total paid), plus any Late Payment Fine set in Settings once the Due Date has passed. A category is only tracked here if it has an Expected Amount &gt; 0.</div>
 <div class="card"><label>Reminder message template</label><textarea id="remTemplate" oninput="reminderTemplate=this.value">${reminderTemplate}</textarea>
 <p class="small">Placeholders: {NAME} {OWED} {FINE} {CATEGORY} {STATUS} {BALANCE} — all filled in automatically per member when you tap Remind.</p></div>
 <div class="card"><div class="tablewrap"><table><tr><th>Name</th><th>Number</th><th>Owed (incl. fines)</th><th>Breakdown</th><th>Status</th><th>Action</th></tr>
 ${list.map(x=>`<tr><td>${x.m[1]}</td><td>${x.m[2]||"Missing"}</td><td>${money(x.owed)}</td><td>${x.cs.filter(c=>c.balance>0).map(c=>`${c.type}: ${money(c.balance)}${c.overdue?` + ${money(c.fine)} fine <span class="pill pending">Late ${c.daysLate}d</span>`:""}`).join("<br>")}</td><td>${x.late?'<span class="pill pending">Late</span>':'<span class="pill paid">Pending</span>'}</td><td>${x.m[2]?`<button class="whatsapp" onclick="messageOneReminder('${x.m[0]}')">Remind</button>`:"—"}</td></tr>`).join("")||'<tr><td colspan="6">No outstanding balances right now 🎉 — set an Expected Amount on a Payment Type in Settings to start tracking dues and late payments.</td></tr>'}
 </table></div></div>`},
Rules:()=>`<div class="section-title"><h2>Fund Guidelines</h2><button class="primary" onclick="addRule()">+ Add Rule</button></div><div class="card"><ol>${data.rules.map((r,i)=>`<li style="padding:8px">${r}</li>`).join("")}</ol></div>`,
WhatsApp:()=>{let s=stats();return `<h2>WhatsApp &amp; SMS Message Center</h2><div class="card">
<p>Current balance: <b>${money(s.balance)}</b></p>
<label>Message template (use {NAME} and {BALANCE})</label>
<textarea id="waMsg" oninput="waTemplate=this.value">${waTemplate}</textarea>
<p class="small">Buttons open WhatsApp or your SMS app with the message ready to send. You still confirm/send on your phone.</p>
<div class="actions" style="margin-top:10px"><button class="whatsapp" onclick="contactAll()">Contact All</button></div>
<div class="tablewrap"><table><tr><th>Name</th><th>Number</th><th>Action</th></tr>
${members().map(m=>`<tr><td>${m[1]}</td><td>${m[2]||"Missing"}</td><td class="actions">${m[2]?`<button class="whatsapp" onclick="messageOne('${m[0]}')">Contact</button>`:"—"}</td></tr>`).join("")||'<tr><td colspan="3">No members added yet.</td></tr>'}
</table></div></div>`},
Reports:()=>{let rows=members().map(m=>{let ps=memberPayments(m[1]),paid=ps.reduce((a,p)=>a+p[3],0),last=ps.length?[...ps].sort((a,b)=>(b[4]+" "+b[5]).localeCompare(a[4]+" "+a[5]))[0]:null;return [m[1],paid,ps.length,last?last[4]:"—"]}).sort((a,b)=>b[1]-a[1]);
return `<div class="section-title"><h2>Contribution Summary</h2><button class="primary" onclick="window.print()">Print</button></div><div class="card"><div class="tablewrap"><table><tr><th>Member</th><th>Total Contributed</th><th>Times Contributed</th><th>Last Contribution</th></tr>${rows.map(r=>`<tr><td>${r[0]}</td><td>${money(r[1])}</td><td>${r[2]}</td><td>${r[3]}</td></tr>`).join("")||'<tr><td colspan="4">No members added yet.</td></tr>'}</table></div></div>`},
Settings:()=>`<h2>System Settings</h2>
<div class="card"><div class="section-title"><h3>Payment Types</h3><button class="primary" onclick="paymentTypeForm()">+ Add Payment Type</button></div>
<p class="small">Each payment type has its own heading. Contribution amounts per person are entered freely when logging a payment — but you can optionally set an "Expected Amount" and "Due Date" here so LEWA MAGIL FORUM can automatically calculate who is behind and flag late payments in the Reminders tab.</p>
<div class="tablewrap"><table><tr><th>Heading</th><th>Expected Amount</th><th>Due Date</th><th>Action</th></tr>${data.paymentTypes.map(t=>`<tr><td>${t.name}</td><td>${t.amount?money(t.amount):"—"}</td><td>${t.dueDate||"—"}</td><td class="actions"><button class="primary" onclick="editPaymentType('${t.id}')">Edit</button><button class="danger" onclick="deletePaymentType('${t.id}')">Delete</button></td></tr>`).join("")}</table></div></div>
<div class="card"><div class="formgrid">
 <div><label>Fund status</label><select id="setStatus"><option ${data.settings.status==="Active"?"selected":""}>Active</option><option ${data.settings.status==="Closed"?"selected":""}>Closed</option></select></div>
 <div><label>Late Payment Fine (KSh)</label><input id="setFineAmount" type="number" value="${data.settings.lateFineAmount||0}"></div>
 <div><label>Fine Type</label><select id="setFineType"><option value="fixed" ${data.settings.lateFineType==="fixed"?"selected":""}>One-time flat fee</option><option value="perday" ${data.settings.lateFineType==="perday"?"selected":""}>Per day late</option></select></div>
 </div><p class="small">When Active, you'll be prompted to notify members of the new balance right after logging a payment. The fine is applied automatically once a category's Due Date (set under Payment Types) has passed, and shows up in the Reminders tab and reminder messages.</p><br><button class="primary" onclick="saveSettings()">Save Settings</button></div>
 <div class="card"><h3>Data management</h3><div class="actions"><button class="primary" onclick="exportData()">Export JSON Backup</button><button class="danger" onclick="resetData()">Reset All Data</button></div></div>`
};
function bindCharts(){
 if(active!=="Dashboard")return;
 let s=stats();
 if(chart1)chart1.destroy();if(chart2)chart2.destroy();
 chart1=new Chart(document.getElementById("statusChart"),{type:"doughnut",data:{labels:["Received","Pending"],datasets:[{data:[s.received,s.pending],backgroundColor:["#15803d","#b91c1c"]}]},options:{responsive:true}});
 let vals=members().map(m=>memberPayments(m[1]).reduce((a,p)=>a+p[3],0));
 chart2=new Chart(document.getElementById("memberChart"),{type:"bar",data:{labels:members().map(m=>m[1]),datasets:[{label:"KSh Contributed",data:vals,backgroundColor:"#6b3410"}]},options:{responsive:true,plugins:{legend:{display:false}}}});
}
function modal(title,body){
 let old=document.getElementById("modal");if(old)old.remove();
 let d=document.createElement("div");d.id="modal";d.style="position:fixed;inset:0;background:#0008;display:flex;align-items:center;justify-content:center;padding:15px;z-index:100";
 d.innerHTML=`<div class="card" style="width:min(600px,100%);max-height:90vh;overflow:auto"><div class="section-title"><h2>${title}</h2><button class="secondary" onclick="document.getElementById('modal').remove()">✕</button></div>${body}</div>`;document.body.appendChild(d);
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
function paymentForm(prefillMemberId){
 let mid=prefillMemberId||"";
 modal("Add Payment",`<div class="formgrid">
 <div><label>Client / Friend</label><select id="pn" onchange="document.getElementById('pnNewWrap').style.display=this.value==='__new__'?'block':'none';document.getElementById('pnNewPhoneHint').style.display=this.value==='__new__'?'block':'none'"><option value="">-- choose --</option>${members().map(m=>`<option value="${m[0]}" ${m[0]===mid?"selected":""}>${m[1]}</option>`).join("")}<option value="__new__">+ New person...</option></select></div>
 <div><label>Payment Type</label><select id="ptp">${data.paymentTypes.map(t=>`<option value="${t.name}">${t.name}</option>`).join("")}</select></div>
 <div><label>Amount</label><input id="pa" type="number" value="0"></div>
 <div><label>Date</label><input id="pd" type="date" value="${new Date().toISOString().slice(0,10)}"></div>
 <div><label>Time</label><input id="pt" type="time" value="${new Date().toTimeString().slice(0,5)}"></div>
 <div><label>Method</label><select id="pm"><option>M-Pesa</option><option>Cash</option><option>Bank</option><option>Other</option></select></div>
 <div><label>Status</label><select id="ps"><option>Received</option><option>Pending</option></select></div>
 </div>
 <div id="pnNewWrap" style="display:none" class="formgrid"><div><label>New person's name</label><input id="pnNewName" placeholder="Full name"></div><div><label>WhatsApp (optional)</label><input id="pnNewPhone" placeholder="07..." oninput="livePhonePreview('pnNewPhone','pnNewPhoneHint')"></div></div><p class="small" id="pnNewPhoneHint" style="display:none">Numbers are automatically formatted to start with +254 when saved.</p>
 <br><div class="actions"><button class="primary" onclick="savePayment(false)">Save Payment</button><button class="secondary" onclick="savePayment(true)">Save &amp; Add Another Payment</button></div>`);
}
function resolveMemberId(){
 let sel=document.getElementById("pn").value;
 if(sel==="__new__"){
  let n=document.getElementById("pnNewName").value.trim(),ph=formatPhone(document.getElementById("pnNewPhone").value.trim());
  if(!n){alert("Enter the new person's name");return null}
  let id="M"+String(members().length+1).padStart(3,"0");
  members().push([id,n,ph,"Member"]);
  return id;
 }
 if(!sel){alert("Choose a member or add a new person");return null}
 return sel;
}
function savePayment(andAnother){
 let id=resolveMemberId(); if(!id)return;
 let m=members().find(x=>x[0]===id),type=document.getElementById("ptp").value;
 let p=["P"+String(data.payments.length+1).padStart(3,"0"),id,m[1],Number(document.getElementById("pa").value),document.getElementById("pd").value,document.getElementById("pt").value,document.getElementById("pm").value,document.getElementById("ps").value,"",type];
 data.payments.push(p);
 if(andAnother){save();paymentForm(id);return}
 save();
 if(data.settings.status==="Active"){promptNotifyBalance()}else{let old=document.getElementById("modal");if(old)old.remove()}
}
function bulkPaymentForm(){
 modal("Bulk Add Payments — Paste a List",`<div class="formgrid">
 <div><label>Category</label><select id="bulkCat" onchange="document.getElementById('bulkNewCatWrap').style.display=this.value==='__new__'?'block':'none'">${data.paymentTypes.map(t=>`<option value="${t.name}">${t.name}</option>`).join("")}<option value="__new__">+ New category...</option></select></div>
 <div><label>Date</label><input id="bulkDate" type="date" value="${new Date().toISOString().slice(0,10)}"></div>
 <div><label>Method</label><select id="bulkMethod"><option>M-Pesa</option><option>Cash</option><option>Bank</option><option>Other</option></select></div>
 </div>
 <div id="bulkNewCatWrap" style="display:none" class="formgrid"><div><label>New category name</label><input id="bulkNewCatName"></div><div><label>Expected Amount per person (optional)</label><input id="bulkNewCatAmount" type="number" value="300"></div></div>
 <label>Paste the list — one member per line</label>
 <textarea id="bulkText" rows="10" placeholder="1. Micah lee=300-cleared✅
2. Augustine sankale =300 cleared ✅"></textarea>
 <p class="small">Numbering, "cleared", checkmarks, and "=" are ignored automatically — the last number on each line is used as that person's amount. Names not already in Members will be added automatically (with no number, editable later). Lines like "Mpesa=", "Cash=", "Total=" or "Treasurer no." are skipped, but the stated total (if any) is checked against what's imported.</p>
 <br><button class="primary" onclick="runBulkImport()">Preview &amp; Import</button>`);
}
function runBulkImport(){
 let catSel=document.getElementById("bulkCat").value;
 let date=document.getElementById("bulkDate").value||new Date().toISOString().slice(0,10);
 let method=document.getElementById("bulkMethod").value;
 let catName=catSel;
 if(catSel==="__new__"){
  let n=document.getElementById("bulkNewCatName").value.trim();
  let amt=Number(document.getElementById("bulkNewCatAmount").value||0);
  if(!n)return alert("Enter a name for the new category");
  if(!data.paymentTypes.find(t=>t.name===n))data.paymentTypes.push({id:"T"+(data.paymentTypes.length+1)+"_"+Date.now(),name:n,amount:amt,dueDate:""});
  catName=n;
 }
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
  let p=["P"+String(data.payments.length+1).padStart(3,"0"),id,m[1],e.amount,date,new Date().toTimeString().slice(0,5),method,"Received","",catName];
  data.payments.push(p);
 });
 let sum=entries.reduce((a,e)=>a+e.amount,0);
 save();
 let old=document.getElementById("modal");if(old)old.remove();
 let mismatchNote=(statedTotal!==null&&statedTotal!==sum)?`<p style="color:#b91c1c">⚠️ Heads up: the total in your paste (${money(statedTotal)}) doesn't match the sum of the ${entries.length} lines imported (${money(sum)}). Everything below has already been added — double-check the list and edit any line in the Payments tab if a number was off.</p>`:"";
 modal("Import complete",`<p>Added ${entries.length} payment(s) under <b>${catName}</b> totalling <b>${money(sum)}</b>.</p>${mismatchNote}<button class="secondary" onclick="document.getElementById('modal').remove()">Close</button>`);
}
function promptNotifyBalance(){
 let bal=stats().balance;
 modal("Payment added — Notify friends?",`<p>Current balance: <b>${money(bal)}</b></p><label>Message</label><textarea id="balMsg">${waTemplate.replaceAll("{NAME}","everyone").replaceAll("{BALANCE}",money(bal))}</textarea><div class="actions" style="margin-top:10px"><button class="whatsapp" onclick="notifyAllSMS()">SMS All (bulk)</button><button class="primary" onclick="paymentForm()">+ Add Another Payment</button><button class="secondary" onclick="document.getElementById('modal').remove()">Skip</button></div><p class="small">Opens your SMS app with all members' numbers pre-filled. For WhatsApp, use the WhatsApp &amp; SMS tab to message people one by one.</p>`);
}
function paymentTypeForm(existing){
 let t=existing||{id:"",name:"",amount:0,dueDate:""};
 modal(existing?"Edit Payment Type":"Add Payment Type",`<div class="formgrid">
 <div><label>Heading / Name</label><input id="ptn" value="${t.name}"></div>
 <div><label>Expected Amount (optional)</label><input id="pta" type="number" value="${t.amount||0}"></div>
 <div><label>Due Date (optional)</label><input id="ptd" type="date" value="${t.dueDate||""}"></div>
 </div><p class="small">Leave Expected Amount at 0 to skip due/late tracking for this category.</p><br><button class="primary" onclick="savePaymentType('${t.id}')">Save</button>`);
}
function editPaymentType(id){paymentTypeForm(data.paymentTypes.find(x=>x.id===id))}
function savePaymentType(id){
 let n=document.getElementById("ptn").value.trim();
 let amt=Number(document.getElementById("pta").value||0);
 let due=document.getElementById("ptd").value||"";
 if(!n)return alert("Name is required");
 if(id){let t=data.paymentTypes.find(x=>x.id===id);t.name=n;t.amount=amt;t.dueDate=due}
 else data.paymentTypes.push({id:"T"+(data.paymentTypes.length+1)+"_"+Date.now(),name:n,amount:amt,dueDate:due});
 document.getElementById("modal").remove();save();
}
function deletePaymentType(id){
 if(data.paymentTypes.length<=1)return alert("At least one payment type is required.");
 if(confirm("Delete this payment type? Existing payment records keep their recorded heading.")){data.paymentTypes=data.paymentTypes.filter(x=>x.id!==id);save()}
}
function addRule(){modal("Add Guideline",`<textarea id="rule"></textarea><br><button class="primary" onclick="data.rules.push(document.getElementById('rule').value);document.getElementById('modal').remove();save()">Add Guideline</button>`)}
function openWA(name,phone){let bal=stats().balance;let msg=waTemplate.replaceAll("{NAME}",name).replaceAll("{BALANCE}",money(bal));let n=normalizePhone(phone);window.open("https://wa.me/"+n+"?text="+encodeURIComponent(msg),"_blank")}
function openSMS(name,phone){let bal=stats().balance;let msg=waTemplate.replaceAll("{NAME}",name).replaceAll("{BALANCE}",money(bal));let n=normalizePhone(phone);window.open("sms:"+n+"?&body="+encodeURIComponent(msg))}
function notifyAllSMS(){
 let list=members().filter(m=>m[2]);
 if(!list.length)return alert("No members with a WhatsApp/phone number yet.");
 let numbers=list.map(m=>normalizePhone(m[2])).join(",");
 let bal=stats().balance;
 let bodyEl=document.getElementById("balMsg")||document.getElementById("allMsg");
 let body=bodyEl?bodyEl.value:waTemplate.replaceAll("{NAME}","everyone").replaceAll("{BALANCE}",money(bal));
 window.open("sms:"+numbers+"?&body="+encodeURIComponent(body));
}
function messageOne(id){
 let m=members().find(x=>x[0]===id);if(!m)return;
 let bal=stats().balance;
 let msg=waTemplate.replaceAll("{NAME}",m[1]).replaceAll("{BALANCE}",money(bal));
 modal("Contact "+m[1],`<label>Message (edit if needed)</label><textarea id="oneMsg">${msg}</textarea><br><br><p>Choose how to reach ${m[1]}:</p><div class="actions">${m[2]?`<a class="call" href="tel:${m[2]}">Call</a><button class="whatsapp" onclick="sendOneWA('${m[2]}')">WhatsApp</button><button class="secondary" onclick="sendOneSMS('${m[2]}')">SMS</button>`:"<p class='small'>No number on file for this member.</p>"}</div>`);
}
function sendOneWA(phone){let msg=document.getElementById("oneMsg").value;let n=normalizePhone(phone);window.open("https://wa.me/"+n+"?text="+encodeURIComponent(msg),"_blank")}
function sendOneSMS(phone){let msg=document.getElementById("oneMsg").value;let n=normalizePhone(phone);window.open("sms:"+n+"?&body="+encodeURIComponent(msg))}
function messageOneReminder(id){
 let m=members().find(x=>x[0]===id);if(!m)return;
 let msg=buildReminderMsg(m[1]);
 modal("Payment Reminder — "+m[1],`<label>Message (calculated automatically — edit if needed)</label><textarea id="remMsg">${msg}</textarea><br><br><p>Choose how to reach ${m[1]}:</p><div class="actions">${m[2]?`<a class="call" href="tel:${m[2]}">Call</a><button class="whatsapp" onclick="sendOneWAField('${m[2]}','remMsg')">WhatsApp</button><button class="secondary" onclick="sendOneSMSField('${m[2]}','remMsg')">SMS</button>`:"<p class='small'>No number on file for this member.</p>"}</div>`);
}
function sendOneWAField(phone,taId){let msg=document.getElementById(taId).value;let n=normalizePhone(phone);window.open("https://wa.me/"+n+"?text="+encodeURIComponent(msg),"_blank")}
function sendOneSMSField(phone,taId){let msg=document.getElementById(taId).value;let n=normalizePhone(phone);window.open("sms:"+n+"?&body="+encodeURIComponent(msg))}
function contactAll(){
 let bal=stats().balance;
 let msg=waTemplate.replaceAll("{NAME}","everyone").replaceAll("{BALANCE}",money(bal));
 modal("Contact All Members",`<label>Message (edit if needed)</label><textarea id="allMsg">${msg}</textarea><br><br><p>Choose how to reach everyone:</p><div class="actions"><button class="whatsapp" onclick="notifyAllSMS()">SMS All (bulk)</button><button class="secondary" onclick="showWAAllList()">WhatsApp All (one by one)</button></div><div id="waAllList"></div>`);
}
function showWAAllList(){
 let list=members().filter(m=>m[2]);
 document.getElementById("waAllList").innerHTML=`<div class="tablewrap" style="margin-top:10px"><table><tr><th>Name</th><th>Action</th></tr>${list.map(m=>`<tr><td>${m[1]}</td><td><button class="whatsapp" onclick="waOpenMsg('${m[2]}','allMsg')">Open WhatsApp</button></td></tr>`).join("")||'<tr><td colspan="2">No members with numbers yet.</td></tr>'}</table></div>`;
}
function waOpenMsg(phone,taId){let ta=document.getElementById(taId);let msg=ta?ta.value:waTemplate;let n=normalizePhone(phone);window.open("https://wa.me/"+n+"?text="+encodeURIComponent(msg),"_blank")}
function saveSettings(){data.settings.status=document.getElementById("setStatus").value;data.settings.lateFineAmount=Number(document.getElementById("setFineAmount").value||0);data.settings.lateFineType=document.getElementById("setFineType").value;save()}
function exportData(){let blob=new Blob([JSON.stringify(data,null,2)],{type:"application/json"}),a=document.createElement("a");a.href=URL.createObjectURL(blob);a.download="LEWA_MAGIL_FORUM_backup.json";a.click();URL.revokeObjectURL(a.href)}
function resetData(){if(confirm("Reset all saved data? This clears members and payments.")){localStorage.removeItem(KEY);location.reload()}}
render();
</script>
</body>
</html>
