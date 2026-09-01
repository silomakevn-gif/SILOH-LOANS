
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>SILOH LOANS — SiloMoney Emergency</title>
<link rel="icon" type="image/png" href="data:image/png;base64,__LOGO_BASE64_DATA_HERE__">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;700&family=Inter:wght@400;500;600&family=IBM+Plex+Mono:wght@500;600&display=swap" rel="stylesheet">
<style>
  :root{
    --bg:#12141c;
    --surface:#1a1e2a;
    --surface-2:#212739;
    --border:#2b3145;
    --gold:#d9b84c;
    --gold-soft:rgba(217,184,76,.13);
    --mint:#33d6a6;
    --mint-soft:rgba(51,214,166,.13);
    --coral:#ff6b5e;
    --coral-soft:rgba(255,107,94,.14);
    --mpesa:#3fae49;
    --mpesa-dark:#1c5e26;
    --airtel:#e4192c;
    --airtel-dark:#7c0f1c;
    --text:#f2efe6;
    --dim:#9aa0b4;
    --faint:#5c6280;
    --radius:16px;
  }
  *{box-sizing:border-box; margin:0; padding:0;}
  html,body{height:100%;}
  body{
    background:var(--bg);
    color:var(--text);
    font-family:'Inter',sans-serif;
    -webkit-font-smoothing:antialiased;
    display:flex;
    justify-content:center;
    min-height:100vh;
  }
  body::before{
    content:"";
    position:fixed; inset:0;
    background:
      radial-gradient(ellipse 700px 500px at 50% -10%, rgba(217,184,76,.07), transparent 60%),
      radial-gradient(ellipse 600px 400px at 100% 100%, rgba(51,214,166,.05), transparent 60%);
    pointer-events:none;
  }
  .frame{
    width:100%; max-width:460px;
    min-height:100vh;
    background:var(--bg);
    display:flex; flex-direction:column;
    position:relative;
    border-left:1px solid var(--border);
    border-right:1px solid var(--border);
  }
  h1,h2,h3,.display{font-family:'Space Grotesk',sans-serif; letter-spacing:-.01em;}
  .mono{font-family:'IBM Plex Mono',monospace; font-variant-numeric:tabular-nums;}

  header{
    padding:22px 20px 16px;
    display:flex; align-items:center; justify-content:space-between;
    position:sticky; top:0; z-index:20;
    background:linear-gradient(var(--bg) 80%, transparent);
  }
  .brand{display:flex; align-items:center; gap:10px;}
  .brand-mark{
    width:38px; height:38px; border-radius:50%;
    object-fit:cover; border:1.5px solid var(--gold);
    box-shadow:0 0 0 1px rgba(217,184,76,.2);
  }
  .brand-name{font-weight:700; font-size:16px; line-height:1.1;}
  .brand-sub{font-size:10.5px; color:var(--dim); letter-spacing:.04em; text-transform:uppercase;}
  .mode-pill{
    font-size:10.5px; color:var(--mint); background:var(--mint-soft);
    border:1px solid rgba(51,214,166,.3); padding:5px 10px; border-radius:99px;
    display:flex; align-items:center; gap:5px; font-weight:600;
  }
  .mode-pill::before{content:""; width:6px; height:6px; border-radius:50%; background:var(--mint); box-shadow:0 0 6px var(--mint);}

  main{flex:1; padding:4px 20px 100px; overflow-x:hidden;}
  .view{display:none; animation:rise .35s ease both;}
  .view.active{display:block;}
  @keyframes rise{from{opacity:0; transform:translateY(10px);} to{opacity:1; transform:translateY(0);}}

  .hero{padding:10px 0 26px;}
  .eyebrow{font-size:11px; color:var(--gold); letter-spacing:.08em; text-transform:uppercase; font-weight:600; margin-bottom:10px;}
  .hero h1{font-size:29px; line-height:1.15; font-weight:700; margin-bottom:10px;}
  .hero p{color:var(--dim); font-size:14px; line-height:1.55; max-width:34ch;}

  .calc-card{
    background:var(--surface); border:1px solid var(--border); border-radius:var(--radius);
    padding:20px; margin-top:22px;
  }
  .calc-row{display:flex; justify-content:space-between; align-items:baseline; margin-bottom:6px;}
  .calc-row span:first-child{font-size:12.5px; color:var(--dim);}
  .calc-amount{font-size:34px; font-weight:600; color:var(--gold);}
  input[type=range]{
    width:100%; -webkit-appearance:none; height:4px; border-radius:2px;
    background:var(--border); margin:16px 0 6px; accent-color:var(--gold);
  }
  input[type=range]::-webkit-slider-thumb{
    -webkit-appearance:none; width:20px; height:20px; border-radius:50%;
    background:var(--gold); border:3px solid var(--bg); box-shadow:0 0 0 1px var(--gold);
    cursor:pointer; margin-top:-8px;
  }
  .range-labels{display:flex; justify-content:space-between; font-size:11px; color:var(--faint); margin-bottom:16px;}
  .tier-badge{
    display:inline-flex; align-items:center; gap:6px; font-size:11.5px; font-weight:600;
    padding:5px 10px; border-radius:99px; background:var(--gold-soft); color:var(--gold);
    margin-bottom:14px;
  }
  .calc-grid{display:grid; grid-template-columns:1fr 1fr; gap:10px; margin-top:6px;}
  .calc-cell{background:var(--surface-2); border-radius:12px; padding:12px 14px;}
  .calc-cell .label{font-size:10.5px; color:var(--dim); text-transform:uppercase; letter-spacing:.04em; margin-bottom:4px;}
  .calc-cell .value{font-size:16.5px; font-weight:600;}
  .calc-cell.warn .value{color:var(--coral);}
  .calc-cell.good .value{color:var(--mint);}

  .btn{
    display:block; width:100%; text-align:center; text-decoration:none;
    border:none; border-radius:12px; padding:15px; font-family:'Inter'; font-weight:600;
    font-size:14.5px; cursor:pointer; transition:.2s;
  }
  .btn-primary{background:var(--gold); color:#1a1305; margin-top:18px;}
  .btn-primary:active{transform:scale(.98);}
  .btn-ghost{background:transparent; color:var(--text); border:1px solid var(--border); margin-top:10px;}
  .btn-mint{background:var(--mint); color:#04261c;}

  .section-title{font-size:12px; letter-spacing:.06em; text-transform:uppercase; color:var(--dim); font-weight:600; margin:28px 0 12px;}
  .steps{display:flex; flex-direction:column; gap:10px;}
  .step{display:flex; gap:12px; align-items:flex-start; background:var(--surface); border:1px solid var(--border); border-radius:12px; padding:13px 14px;}
  .step-num{
    width:24px; height:24px; border-radius:7px; background:var(--surface-2); color:var(--gold);
    font-family:'IBM Plex Mono'; font-size:12px; font-weight:600; display:flex; align-items:center; justify-content:center; flex-shrink:0;
  }
  .step div p{font-size:13px; color:var(--dim); line-height:1.4;}
  .step div strong{font-size:13.5px; display:block; margin-bottom:2px;}

  .field{margin-bottom:14px;}
  .field label{font-size:12px; color:var(--dim); display:block; margin-bottom:7px; font-weight:500;}
  .field input, .field select{
    width:100%; background:var(--surface); border:1px solid var(--border); color:var(--text);
    border-radius:11px; padding:13px 14px; font-family:'Inter'; font-size:14.5px; outline:none;
  }
  .field input:focus, .field select:focus{border-color:var(--gold);}
  .field input::placeholder{color:var(--faint);}
  .helptext{font-size:11px; color:var(--faint); margin-top:6px;}

  .summary-card{background:var(--surface); border:1px dashed var(--border); border-radius:12px; padding:16px; margin:18px 0;}
  .summary-row{display:flex; justify-content:space-between; font-size:13px; padding:6px 0; color:var(--dim);}
  .summary-row strong{color:var(--text); font-weight:600;}
  .summary-row.total{border-top:1px solid var(--border); margin-top:4px; padding-top:12px;}
  .summary-row.total strong{color:var(--gold); font-size:15px;}

  .tc-check{display:flex; align-items:flex-start; gap:10px; margin:18px 0 6px;}
  .tc-check input{margin-top:3px; accent-color:var(--gold); width:16px; height:16px; flex-shrink:0;}
  .tc-check label{font-size:12px; color:var(--dim); line-height:1.5;}
  .tc-check a{color:var(--gold); text-decoration:underline;}

  .tc-block{margin-top:8px;}
  .tc-block h3{font-size:13px; color:var(--gold); font-weight:600; margin:20px 0 6px; font-family:'Inter';}
  .tc-block h3:first-child{margin-top:0;}
  .tc-block p{font-size:12.5px; color:var(--dim); line-height:1.6;}

  .status-card{background:var(--surface); border:1px solid var(--border); border-radius:var(--radius); padding:20px; margin-top:6px;}
  .status-head{display:flex; justify-content:space-between; align-items:flex-start; margin-bottom:16px;}
  .status-tag{font-size:10.5px; font-weight:700; padding:5px 10px; border-radius:99px; text-transform:uppercase; letter-spacing:.03em;}
  .status-tag.ontrack{background:var(--mint-soft); color:var(--mint);}
  .status-tag.late{background:var(--coral-soft); color:var(--coral);}
  .status-tag.paid{background:var(--gold-soft); color:var(--gold);}

  .ledger{margin-top:16px;}
  .ledger-bar{
    height:34px; border-radius:9px; overflow:hidden; display:flex; background:var(--surface-2);
    border:1px solid var(--border);
  }
  .ledger-seg{height:100%; display:flex; align-items:center; justify-content:center; font-size:10px; font-weight:700; font-family:'IBM Plex Mono';}
  .seg-principal{background:var(--gold-soft); color:var(--gold);}
  .seg-interest{background:rgba(154,160,180,.15); color:var(--dim);}
  .seg-late{background:var(--coral); color:#2a0704; animation:pulse 1.6s infinite;}
  @keyframes pulse{0%,100%{filter:brightness(1);} 50%{filter:brightness(1.3);}}
  .ledger-legend{display:flex; gap:14px; margin-top:10px; flex-wrap:wrap;}
  .legend-item{display:flex; align-items:center; gap:6px; font-size:11px; color:var(--dim);}
  .legend-dot{width:8px; height:8px; border-radius:2px;}

  .day-counter{font-size:12.5px; color:var(--coral); margin-top:10px; font-weight:600;}

  .pay-name-field{margin-bottom:16px;}
  .pay-methods{display:flex; flex-direction:column; gap:14px;}
  .pay-method{
    border-radius:16px; overflow:hidden; position:relative;
    border:1px solid rgba(255,255,255,.08);
  }
  .pay-method.mpesa{background:linear-gradient(155deg, var(--mpesa) 0%, var(--mpesa-dark) 100%);}
  .pay-method.airtel{background:linear-gradient(155deg, var(--airtel) 0%, var(--airtel-dark) 100%);}
  .pay-method-top{display:flex; align-items:center; gap:12px; padding:16px 16px 14px;}
  .pay-method-icon{
    width:42px; height:42px; border-radius:11px; flex-shrink:0;
    display:flex; align-items:center; justify-content:center;
    background:rgba(255,255,255,.16);
    font-family:'Space Grotesk'; font-weight:700; font-size:17px; color:#fff;
  }
  .pay-method-info{flex:1; min-width:0;}
  .pay-method-info strong{font-size:14px; display:block; color:#fff;}
  .pay-method-info span{font-size:11px; color:rgba(255,255,255,.75);}
  .pay-method-verified{
    font-size:9.5px; font-weight:700; color:#fff; background:rgba(255,255,255,.18);
    padding:4px 8px; border-radius:99px; display:flex; align-items:center; gap:4px; white-space:nowrap;
  }
  .pay-method-num{
    margin:0 16px 14px; background:rgba(0,0,0,.22); border-radius:11px;
    padding:12px 14px; font-size:19px; font-weight:700; letter-spacing:.03em; color:#fff;
    display:flex; align-items:center; justify-content:space-between;
  }
  .pay-method-num .copy-hint{font-size:9.5px; font-weight:600; color:rgba(255,255,255,.7); letter-spacing:.04em; text-transform:uppercase;}
  .pay-steps{display:flex; justify-content:space-between; padding:0 14px 16px; gap:4px;}
  .pay-step{display:flex; flex-direction:column; align-items:center; gap:5px; flex:1;}
  .pay-step-dot{
    width:24px; height:24px; border-radius:50%; background:rgba(255,255,255,.2);
    color:#fff; font-family:'IBM Plex Mono'; font-size:10px; font-weight:600;
    display:flex; align-items:center; justify-content:center;
  }
  .pay-step-label{font-size:8px; color:rgba(255,255,255,.85); text-align:center; line-height:1.25; font-weight:600; text-transform:uppercase; letter-spacing:.02em;}

  .faq{border-bottom:1px solid var(--border);}
  .faq summary{
    list-style:none; cursor:pointer; padding:16px 2px; display:flex; justify-content:space-between;
    align-items:center; font-size:13.5px; font-weight:600;
  }
  .faq summary::-webkit-details-marker{display:none;}
  .faq summary::after{content:"+"; color:var(--gold); font-size:18px; font-weight:400; transition:.2s;}
  .faq[open] summary::after{content:"–";}
  .faq p{padding:0 2px 16px; color:var(--dim); font-size:13px; line-height:1.55;}

  .contact-row{
    display:flex; align-items:center; gap:12px; background:var(--surface); border:1px solid var(--border);
    border-radius:12px; padding:14px; margin-top:20px;
  }
  .contact-icon{width:38px; height:38px; border-radius:10px; background:var(--mint-soft); display:flex; align-items:center; justify-content:center; font-size:17px; flex-shrink:0;}
  .contact-row div strong{font-size:13.5px; display:block;}
  .contact-row div span{font-size:11.5px; color:var(--dim);}

  nav{
    position:fixed; bottom:0; left:50%; transform:translateX(-50%);
    width:100%; max-width:460px;
    background:rgba(18,20,28,.92); backdrop-filter:blur(14px);
    border-top:1px solid var(--border);
    display:flex; padding:8px 6px calc(10px + env(safe-area-inset-bottom));
    z-index:30;
  }
  nav button{
    flex:1; background:none; border:none; color:var(--faint); font-family:'Inter';
    display:flex; flex-direction:column; align-items:center; gap:4px; padding:8px 0;
    font-size:10.5px; font-weight:600; cursor:pointer; border-radius:10px; transition:.2s;
  }
  nav button .ic{font-size:19px; line-height:1;}
  nav button.active{color:var(--gold);}
  nav button.active .ic{transform:translateY(-1px);}

  ::-webkit-scrollbar{display:none;}
</style>
</head>
<body>
<div class="frame">

  <header>
    <div class="brand">
      <img class="brand-mark" src="data:image/png;base64,__LOGO_BASE64_DATA_HERE__" alt="SILOH LOANS logo">
      <div>
        <div class="brand-name">SILOH LOANS</div>
        <div class="brand-sub">SiloMoney Emergency</div>
      </div>
    </div>
    <div class="mode-pill">M-Pesa ready</div>
  </header>

  <main>

    <!-- ================= HOME ================= -->
    <section class="view active" id="view-home">

      <div class="hero" style="padding-top:10px;">
        <div class="eyebrow">Emergency cash, same day</div>
        <h1>Borrow KSh 100–5,000 on your M-Pesa.</h1>
        <p>Flat 27.89% interest, no hidden charges. Apply in under a minute, straight from WhatsApp.</p>
      </div>

      <div class="calc-card">
        <div class="calc-row"><span>You want to borrow</span></div>
        <div class="calc-amount mono" id="calcAmountLabel">KSh 1,500</div>
        <input type="range" id="calcSlider" min="100" max="5000" step="50" value="1500" oninput="updateCalc()">
        <div class="range-labels"><span>KSh 100</span><span>KSh 5,000</span></div>
        <div class="tier-badge" id="tierBadge">Basic tier</div>
        <div class="calc-grid">
          <div class="calc-cell"><div class="label">Interest (27.89%)</div><div class="value mono" id="calcInterest">KSh 418</div></div>
          <div class="calc-cell"><div class="label">Repay in</div><div class="value mono" id="calcDays">18 days</div></div>
          <div class="calc-cell good"><div class="label">Total repayment</div><div class="value mono" id="calcTotal">KSh 1,918</div></div>
          <div class="calc-cell warn"><div class="label">If late, per day</div><div class="value mono" id="calcLate">+KSh 150</div></div>
        </div>
        <button class="btn btn-primary" onclick="goTo('apply')">Apply for this amount</button>
      </div>

      <div class="section-title">How it works</div>
      <div class="steps">
        <div class="step"><div class="step-num">01</div><div><strong>Send your details</strong><p>Fill the form, we open WhatsApp with everything pre-filled for you.</p></div></div>
        <div class="step"><div class="step-num">02</div><div><strong>We confirm on WhatsApp</strong><p>Our team verifies your ID and M-Pesa number, usually within minutes.</p></div></div>
        <div class="step"><div class="step-num">03</div><div><strong>Cash lands on M-Pesa</strong><p>Disbursed directly to your registered number, ready to use.</p></div></div>
      </div>

    </section>

    <!-- ================= APPLY ================= -->
    <section class="view" id="view-apply">
      <div class="hero" style="padding-bottom:16px;">
        <div class="eyebrow">Apply</div>
        <h1 style="font-size:24px;">Tell us who you are</h1>
        <p>We'll open WhatsApp with your application ready to send. Nothing is sent until you hit send there.</p>
      </div>

      <div class="field"><label>Full name</label><input id="apName" type="text" placeholder="As on your ID"></div>
      <div class="field"><label>Phone number</label><input id="apPhone" type="tel" placeholder="07XX XXX XXX"></div>
      <div class="field"><label>National ID number</label><input id="apId" type="text" placeholder="e.g. 30112233"></div>
      <div class="field"><label>M-Pesa number for disbursement</label><input id="apMpesa" type="tel" placeholder="Leave blank if same as phone above"></div>
      <div class="field">
        <label>Loan amount (KSh)</label>
        <input id="apAmount" type="number" min="100" max="5000" step="50" value="1500" oninput="syncApplySummary()">
        <div class="helptext">Between KSh 100 and KSh 5,000</div>
      </div>

      <div class="summary-card">
        <div class="summary-row"><span>Tier</span><strong id="apTier">Basic</strong></div>
        <div class="summary-row"><span>Interest at 27.89%</span><strong id="apInterest">KSh 418</strong></div>
        <div class="summary-row"><span>Repayment period</span><strong id="apDays">18 days</strong></div>
        <div class="summary-row total"><span>Total to repay</span><strong id="apTotal">KSh 1,918</strong></div>
      </div>

      <div class="tc-check">
        <input type="checkbox" id="apAgree">
        <label for="apAgree">I agree to the <a href="#" onclick="goTo('terms'); return false;">Terms &amp; Conditions</a> of SILOH LOANS.</label>
      </div>

      <button class="btn btn-mint" onclick="sendApplication()">Continue on WhatsApp</button>
      <div class="helptext" style="text-align:center; margin-top:10px;">Late repayments accrue 10% of the loan amount per day.</div>
    </section>

    <!-- ================= PAY LOAN ================= -->
    <section class="view" id="view-pay">
      <div class="hero" style="padding-bottom:10px;">
        <div class="eyebrow">Pay loan</div>
        <h1 style="font-size:24px;">How to pay</h1>
        <p>Pick the number you're paying from, then send to either option below.</p>
      </div>

      <div class="field pay-name-field">
        <label>Number you're paying from</label>
        <select id="payerNumberSelect" onchange="onPayerNumberChange()">
          <option value="">Select a number…</option>
        </select>
        <input id="payerNumberCustom" type="tel" placeholder="Or type a number, e.g. 07XX XXX XXX" style="margin-top:8px;" oninput="syncCustomNumber()">
        <div class="helptext">We'll use this number on your payment confirmation to admin.</div>
      </div>

      <div class="field pay-name-field">
        <label>Name to reflect on payment</label>
        <input id="payerName" type="text" placeholder="e.g. Kelvin Sirinkit">
      </div>

      <div class="pay-methods">
        <div class="pay-method mpesa">
          <div class="pay-method-top">
            <div class="pay-method-icon">M</div>
            <div class="pay-method-info"><strong>Lipa na M-Pesa</strong><span>Pochi la Biashara · SILOH LOANS</span></div>
            <div class="pay-method-verified">✓ Verified</div>
          </div>
          <div class="pay-method-num mono"><span>0702 994 132</span><span class="copy-hint">Till</span></div>
          <div class="pay-steps">
            <div class="pay-step"><div class="pay-step-dot">1</div><div class="pay-step-label">Dial<br>*334#</div></div>
            <div class="pay-step"><div class="pay-step-dot">2</div><div class="pay-step-label">Select<br>Send Money</div></div>
            <div class="pay-step"><div class="pay-step-dot">3</div><div class="pay-step-label">Enter<br>0702994132</div></div>
            <div class="pay-step"><div class="pay-step-dot">4</div><div class="pay-step-label">Enter<br>KSh Amount</div></div>
            <div class="pay-step"><div class="pay-step-dot">5</div><div class="pay-step-label">Enter PIN<br>to Confirm</div></div>
          </div>
        </div>

        <div class="pay-method airtel">
          <div class="pay-method-top">
            <div class="pay-method-icon">A</div>
            <div class="pay-method-info"><strong>Lipa na Airtel Money</strong><span>Send Money · SILOH LOANS</span></div>
            <div class="pay-method-verified">✓ Verified</div>
          </div>
          <div class="pay-method-num mono"><span>0784 693 195</span><span class="copy-hint">Number</span></div>
          <div class="pay-steps">
            <div class="pay-step"><div class="pay-step-dot">1</div><div class="pay-step-label">Dial<br>*334#</div></div>
            <div class="pay-step"><div class="pay-step-dot">2</div><div class="pay-step-label">Select<br>Send Money</div></div>
            <div class="pay-step"><div class="pay-step-dot">3</div><div class="pay-step-label">Enter<br>0784693195</div></div>
            <div class="pay-step"><div class="pay-step-dot">4</div><div class="pay-step-label">Enter<br>KSh Amount</div></div>
            <div class="pay-step"><div class="pay-step-dot">5</div><div class="pay-step-label">Enter PIN<br>to Confirm</div></div>
          </div>
        </div>
      </div>

      <div class="section-title" style="margin:26px 0 10px;">Check your balance</div>
      <div class="field">
        <label>Phone or National ID</label>
        <input id="payLookup" type="text" placeholder="07XX XXX XXX or ID number" oninput="onLookupInput()">
      </div>
      <button class="btn btn-ghost" onclick="checkStatus()">Check my loan</button>

      <div class="status-card" id="noLoanCard" style="display:none; margin-top:20px; text-align:center;">
        <div style="font-size:28px; margin-bottom:8px;">🔍</div>
        <div style="font-weight:600; font-size:14.5px; margin-bottom:4px;">No loan found</div>
        <div style="font-size:12.5px; color:var(--dim);">Nothing on this device matches that phone or ID yet. Apply first, then check back here.</div>
        <button class="btn btn-ghost" style="margin-top:14px;" onclick="goTo('apply')">Apply for a loan</button>
      </div>

      <div class="status-card" id="statusCard" style="display:none; margin-top:20px;">
        <div style="font-size:10.5px; color:var(--dim); background:var(--surface-2); border-radius:8px; padding:8px 10px; margin-bottom:14px;">
          Stored on this device only — not yet synced with SILOH LOANS records.
        </div>
        <div class="status-head">
          <div><div style="font-size:11px; color:var(--dim);">Loan ref</div><div style="font-weight:700; font-size:15px;" id="stRef">—</div></div>
          <div class="status-tag ontrack" id="stTag">—</div>
        </div>

        <div class="ledger">
          <div class="ledger-bar">
            <div class="ledger-seg seg-principal" id="segPrincipal" style="flex:1;">Principal</div>
            <div class="ledger-seg seg-interest" id="segInterest" style="flex:1;">Interest</div>
            <div class="ledger-seg seg-late" id="segLate" style="flex:0; display:none;">Late</div>
          </div>
          <div class="ledger-legend">
            <div class="legend-item"><span class="legend-dot" style="background:var(--gold);"></span><span id="legPrincipal">Principal · KSh 0</span></div>
            <div class="legend-item"><span class="legend-dot" style="background:var(--faint);"></span><span id="legInterest">Interest · KSh 0</span></div>
            <div class="legend-item" id="legLateWrap" style="display:none;"><span class="legend-dot" style="background:var(--coral);"></span><span id="legLate">Late fee · KSh 0</span></div>
          </div>
          <div class="day-counter" id="dayCounter" style="display:none;"></div>
        </div>

        <div class="summary-card" style="margin-bottom:0;">
          <div class="summary-row total"><span>Amount due now</span><strong id="stTotal">KSh 0</strong></div>
        </div>

        <button class="btn btn-primary" style="margin-top:16px;" id="paidBtn" onclick="confirmPayment()">I've paid — confirm on WhatsApp</button>
      </div>
    </section>

    <!-- ================= HELP ================= -->
    <section class="view" id="view-help">
      <div class="hero" style="padding-bottom:6px;">
        <div class="eyebrow">Help</div>
        <h1 style="font-size:24px;">Questions, answered</h1>
      </div>

      <details class="faq" open>
        <summary>How fast do I get the money?</summary>
        <p>Most applications are confirmed on WhatsApp within minutes during business hours, and cash lands on your M-Pesa right after confirmation.</p>
      </details>
      <details class="faq">
        <summary>What happens if I pay late?</summary>
        <p>Late loans accrue an extra 10% of the loan amount for every day past the due date, on top of your original interest. Pay as soon as you can to keep it small.</p>
      </details>
      <details class="faq">
        <summary>Can I have two loans at once?</summary>
        <p>No. You need to fully repay your current loan before you qualify for another one.</p>
      </details>
      <details class="faq">
        <summary>How do I repay?</summary>
        <p>Pay the total shown on your loan status via our M-Pesa Till (0702 994 132) or Airtel Money (0784 693 195), then confirm your payment on WhatsApp so we can mark it received.</p>
      </details>

      <div class="section-title">Still stuck?</div>
      <div class="contact-row">
        <div class="contact-icon">💬</div>
        <div><strong>Chat with us on WhatsApp</strong><span>Usually replies within minutes</span></div>
      </div>
      <button class="btn btn-mint" style="margin-top:12px;" onclick="openHelpWhatsapp()">Open WhatsApp chat</button>

      <div class="contact-row" style="cursor:pointer;" onclick="goTo('terms')">
        <div class="contact-icon">📄</div>
        <div><strong>Terms &amp; Conditions</strong><span>Loan rules, repayment and late fees</span></div>
      </div>
    </section>

    <!-- ================= TERMS & CONDITIONS ================= -->
    <section class="view" id="view-terms">
      <div class="hero" style="padding-bottom:6px;">
        <div class="eyebrow">Legal</div>
        <h1 style="font-size:24px;">Terms &amp; Conditions</h1>
        <p>Please read before applying. By applying for a loan you agree to these terms.</p>
      </div>

      <div class="tc-block">
        <h3>1. Eligibility</h3>
        <p>You must be 18 years or older, hold a valid Kenyan National ID, and provide an active M-Pesa registered number in your name to be considered for a loan.</p>

        <h3>2. Loan amounts &amp; interest</h3>
        <p>Loans range from KSh 100 to KSh 5,000. A flat interest rate of 27.89% applies to the principal for the full loan term, regardless of when you repay within that term.</p>

        <h3>3. Repayment periods</h3>
        <p>Starter (KSh 100–500) and Basic (KSh 501–1,500) loans: 18 days. Standard (KSh 1,501–3,000): 21 days. Premium (KSh 3,001–5,000): 30 days.</p>

        <h3>4. Late payments</h3>
        <p>Loans not repaid by the due date accrue a late fee of 10% of the original loan amount for every day the balance remains unpaid, in addition to the interest already due. This continues until the loan is repaid in full.</p>

        <h3>5. One loan at a time</h3>
        <p>Only one active loan is permitted per customer. A new application will not be accepted until your current loan is fully repaid.</p>

        <h3>6. Disbursement</h3>
        <p>Approved loans are disbursed via M-Pesa to the number provided at application, after verification of your details on WhatsApp. SILOH LOANS is not liable for delays caused by incorrect number details supplied by you.</p>

        <h3>7. Repayment</h3>
        <p>Repayments are accepted via the M-Pesa Till or Airtel Money number shown in the app. You are responsible for confirming your payment with SILOH LOANS on WhatsApp; a payment is not considered received until confirmed.</p>

        <h3>8. Approval &amp; refusal</h3>
        <p>Submitting an application does not guarantee approval. SILOH LOANS may decline any application at its discretion, including where details cannot be verified.</p>

        <h3>9. Your information</h3>
        <p>Details you submit (name, phone, ID, M-Pesa number) are used solely to process and verify your loan and are shared with SILOH LOANS via WhatsApp for that purpose.</p>

        <h3>10. Changes to these terms</h3>
        <p>SILOH LOANS may update these terms from time to time. Continued use of the app after changes are posted constitutes acceptance of the updated terms.</p>
      </div>

      <button class="btn btn-ghost" style="margin-top:8px;" onclick="goTo('help')">Back to Help</button>
    </section>

  </main>

  <nav>
    <button class="active" data-view="home" onclick="goTo('home')"><span class="ic">⌂</span>Home</button>
    <button data-view="apply" onclick="goTo('apply')"><span class="ic">＋</span>Apply</button>
    <button data-view="pay" onclick="goTo('pay')"><span class="ic">▣</span>Pay Loan</button>
    <button data-view="help" onclick="goTo('help')"><span class="ic">?</span>Help</button>
  </nav>

</div>

<script>
  const ADMIN_WHATSAPP = "254784693195";
  const TILL_NUMBER = "0702994132";
  const AIRTEL_NUMBER = "0784693195";
  const INTEREST_RATE = 0.2789;
  const LATE_FEE_RATE_PER_DAY = 0.10;

  function tierFor(amount){
    if(amount <= 500) return {name:"Starter", days:18};
    if(amount <= 1500) return {name:"Basic", days:18};
    if(amount <= 3000) return {name:"Standard", days:21};
    return {name:"Premium", days:30};
  }
  function fmt(n){ return "KSh " + Math.round(n).toLocaleString(); }

  function goTo(view){
    document.querySelectorAll(".view").forEach(v=>v.classList.remove("active"));
    document.getElementById("view-"+view).classList.add("active");
    document.querySelectorAll("nav button").forEach(b=>b.classList.toggle("active", b.dataset.view===view));
    window.scrollTo(0,0);
    if(view==="apply") syncApplySummary();
  }

  function updateCalc(){
    const amt = parseInt(document.getElementById("calcSlider").value,10);
    const tier = tierFor(amt);
    const interest = amt * INTEREST_RATE;
    const total = amt + interest;
    const lateDaily = amt * LATE_FEE_RATE_PER_DAY;

    document.getElementById("calcAmountLabel").textContent = fmt(amt);
    document.getElementById("tierBadge").textContent = tier.name + " tier";
    document.getElementById("calcInterest").textContent = fmt(interest);
    document.getElementById("calcDays").textContent = tier.days + " days";
    document.getElementById("calcTotal").textContent = fmt(total);
    document.getElementById("calcLate").textContent = "+" + fmt(lateDaily);

    document.getElementById("apAmount").value = amt;
    syncApplySummary();
  }

  function syncApplySummary(){
    let amt = parseInt(document.getElementById("apAmount").value,10);
    if(isNaN(amt)) amt = 0;
    amt = Math.max(100, Math.min(5000, amt));
    const tier = tierFor(amt);
    const interest = amt * INTEREST_RATE;
    const total = amt + interest;

    document.getElementById("apTier").textContent = tier.name;
    document.getElementById("apInterest").textContent = fmt(interest);
    document.getElementById("apDays").textContent = tier.days + " days";
    document.getElementById("apTotal").textContent = fmt(total);
  }

  function sendApplication(){
    const name = document.getElementById("apName").value.trim();
    const phone = document.getElementById("apPhone").value.trim();
    const id = document.getElementById("apId").value.trim();
    let mpesa = document.getElementById("apMpesa").value.trim();
    let amt = parseInt(document.getElementById("apAmount").value,10) || 0;
    amt = Math.max(100, Math.min(5000, amt));

    if(!name || !phone || !id){
      alert("Please fill in your name, phone and ID before continuing.");
      return;
    }

    if(!document.getElementById("apAgree").checked){
      alert("Please agree to the Terms & Conditions before applying.");
      return;
    }

    const existing = findActiveLoan(phone, id);
    if(existing){
      alert("You already have an active loan (" + existing.ref + "). It needs to be repaid before applying again — check Pay Loan for the balance.");
      goTo('pay');
      return;
    }

    if(!mpesa) mpesa = "Same as above";

    saveKnownNumber(phone);
    if(mpesa !== "Same as above") saveKnownNumber(mpesa);
    if(name) localStorage.setItem("siloh_last_name", name);

    const tier = tierFor(amt);
    const total = amt + amt*INTEREST_RATE;
    const ref = "SL-" + Math.floor(1000 + Math.random()*9000);
    const appliedAt = new Date();
    const dueDate = new Date();
    dueDate.setDate(dueDate.getDate() + tier.days);
    const dueDateStr = dueDate.toLocaleDateString("en-GB", {day:"2-digit", month:"short", year:"numeric"});

    saveLoanRecord({
      ref: ref, name: name, phone: phone, id: id, mpesa: mpesa,
      amount: amt, days: tier.days,
      appliedAt: appliedAt.toISOString(), dueAt: dueDate.toISOString(),
      status: "active"
    });

    const msg = [
      "SILOH LOANS — LOAN APPLICATION",
      "",
      "Name: " + name,
      "Number: " + phone,
      "ID: " + id,
      "Amount: " + fmt(amt),
      "Amount Repay: " + fmt(total),
      "Date to Repay: " + dueDateStr,
      "SL: " + ref,
      "",
      "M-Pesa (disbursement): " + mpesa,
      "Pay via M-Pesa Till: " + TILL_NUMBER + " or Airtel Money: " + AIRTEL_NUMBER
    ].join("\n");

    const url = "https://wa.me/" + ADMIN_WHATSAPP + "?text=" + encodeURIComponent(msg);
    window.open(url, "_blank");
  }

  function getLoans(){ return JSON.parse(localStorage.getItem("siloh_loans") || "[]"); }
  function saveLoanRecord(record){
    const loans = getLoans();
    loans.push(record);
    localStorage.setItem("siloh_loans", JSON.stringify(loans));
  }
  function normalize(v){ return (v||"").toString().trim().toLowerCase().replace(/\s+/g,""); }
  function findActiveLoan(phone, id){
    const loans = getLoans();
    const p = normalize(phone), i = normalize(id);
    return loans.slice().reverse().find(l =>
      l.status === "active" && (normalize(l.phone) === p || (i && normalize(l.id) === i))
    );
  }
  function findAnyLoan(query){
    const loans = getLoans();
    const q = normalize(query);
    return loans.slice().reverse().find(l => normalize(l.phone) === q || normalize(l.id) === q);
  }

  function saveKnownNumber(num){
    if(!num) return;
    let list = JSON.parse(localStorage.getItem("siloh_known_numbers") || "[]");
    if(!list.includes(num)) list.push(num);
    localStorage.setItem("siloh_known_numbers", JSON.stringify(list));
  }

  function populatePayerNumbers(){
    const select = document.getElementById("payerNumberSelect");
    const list = JSON.parse(localStorage.getItem("siloh_known_numbers") || "[]");
    select.innerHTML = "";
    if(list.length === 0){
      select.innerHTML = '<option value="">No saved numbers yet — type yours below</option>';
      return;
    }
    select.innerHTML = '<option value="">Select a number…</option>' +
      list.map(n => '<option value="'+n+'">'+n+'</option>').join("") +
      '<option value="__other">Use a different number</option>';
    const lastName = localStorage.getItem("siloh_last_name");
    if(lastName && !document.getElementById("payerName").value){
      document.getElementById("payerName").value = lastName;
    }
  }

  function onPayerNumberChange(){
    const select = document.getElementById("payerNumberSelect");
    const customField = document.getElementById("payerNumberCustom");
    if(select.value === "__other" || select.value === ""){
      customField.style.display = "block";
      if(select.value === "__other") customField.focus();
    } else {
      customField.value = select.value;
      customField.style.display = "block";
    }
  }

  function syncCustomNumber(){
    document.getElementById("payerNumberSelect").value = "__other";
  }

  function getPayerNumber(){
    const custom = document.getElementById("payerNumberCustom").value.trim();
    if(custom) return custom;
    const select = document.getElementById("payerNumberSelect").value;
    return (select && select !== "__other") ? select : "Not provided";
  }

  function onLookupInput(){
    const q = document.getElementById("payLookup").value.trim();
    const customField = document.getElementById("payerNumberCustom");
    if(/^0?7\d{8}$/.test(q.replace(/\s/g,"")) && !customField.value){
      customField.value = q;
      document.getElementById("payerNumberSelect").value = "__other";
    }
  }

  let currentLoanRef = null;

  function checkStatus(){
    const q = document.getElementById("payLookup").value.trim();
    if(!q){ alert("Enter the phone or ID you applied with."); return; }

    const loan = findAnyLoan(q);
    const statusCard = document.getElementById("statusCard");
    const noLoanCard = document.getElementById("noLoanCard");

    if(!loan){
      statusCard.style.display = "none";
      noLoanCard.style.display = "block";
      noLoanCard.scrollIntoView({behavior:"smooth", block:"nearest"});
      currentLoanRef = null;
      return;
    }

    noLoanCard.style.display = "none";
    renderLoanStatus(loan);
    statusCard.style.display = "block";
    statusCard.scrollIntoView({behavior:"smooth", block:"nearest"});
  }

  function renderLoanStatus(loan){
    currentLoanRef = loan.ref;
    const amount = loan.amount;
    const interest = amount * INTEREST_RATE;
    const dueAt = new Date(loan.dueAt);
    const today = new Date();
    const msPerDay = 86400000;
    const daysLate = loan.status === "paid" ? 0 : Math.max(0, Math.floor((today - dueAt) / msPerDay));
    const lateFee = daysLate * amount * LATE_FEE_RATE_PER_DAY;
    const total = amount + interest + lateFee;

    document.getElementById("stRef").textContent = loan.ref;

    const tag = document.getElementById("stTag");
    if(loan.status === "paid"){
      tag.className = "status-tag paid";
      tag.textContent = "Paid";
    } else if(daysLate > 0){
      tag.className = "status-tag late";
      tag.textContent = daysLate + " day" + (daysLate === 1 ? "" : "s") + " late";
    } else {
      const daysLeft = Math.max(0, Math.ceil((dueAt - today) / msPerDay));
      tag.className = "status-tag ontrack";
      tag.textContent = daysLeft + " day" + (daysLeft === 1 ? "" : "s") + " left";
    }

    document.getElementById("segPrincipal").style.flex = amount;
    document.getElementById("segInterest").style.flex = interest;
    document.getElementById("legPrincipal").textContent = "Principal · " + fmt(amount);
    document.getElementById("legInterest").textContent = "Interest · " + fmt(interest);

    const segLate = document.getElementById("segLate");
    const legLateWrap = document.getElementById("legLateWrap");
    const dayCounter = document.getElementById("dayCounter");
    if(lateFee > 0){
      segLate.style.display = "flex";
      segLate.style.flex = lateFee;
      legLateWrap.style.display = "flex";
      document.getElementById("legLate").textContent = "Late fee · " + fmt(lateFee);
      dayCounter.style.display = "block";
      dayCounter.textContent = "Growing by " + fmt(amount * LATE_FEE_RATE_PER_DAY) + "/day while unpaid — pay today to stop it.";
    } else {
      segLate.style.display = "none";
      legLateWrap.style.display = "none";
      dayCounter.style.display = "none";
    }

    document.getElementById("stTotal").textContent = loan.status === "paid" ? fmt(0) : fmt(total);
    document.getElementById("paidBtn").style.display = loan.status === "paid" ? "none" : "block";
  }

  function confirmPayment(){
    const payerName = document.getElementById("payerName").value.trim() || "Not provided";
    const payerNumber = getPayerNumber();
    const ref = document.getElementById("stRef").textContent;

    const msg = [
      "SILOH LOANS — PAYMENT CONFIRMATION",
      "",
      "Ref: " + ref,
      "Name on payment: " + payerName,
      "Paid from number: " + payerNumber,
      "Paid via M-Pesa Till: " + TILL_NUMBER + " or Airtel Money: " + AIRTEL_NUMBER,
      "",
      "Please confirm receipt."
    ].join("\n");
    const url = "https://wa.me/" + ADMIN_WHATSAPP + "?text=" + encodeURIComponent(msg);
    window.open(url, "_blank");

    if(currentLoanRef){
      const loans = getLoans();
      const idx = loans.findIndex(l => l.ref === currentLoanRef);
      if(idx > -1){
        loans[idx].status = "paid";
        localStorage.setItem("siloh_loans", JSON.stringify(loans));
        renderLoanStatus(loans[idx]);
      }
    }
  }

  function openHelpWhatsapp(){
    const msg = "Hello SILOH LOANS, I have a question.";
    const url = "https://wa.me/" + ADMIN_WHATSAPP + "?text=" + encodeURIComponent(msg);
    window.open(url, "_blank");
  }

  updateCalc();
  populatePayerNumbers();
</script>
</body>
</html>
