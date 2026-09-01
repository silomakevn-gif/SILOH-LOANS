<!DOCTYPE html>
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
            <div class="pay-step"><div class="pay-step-dot">4</div><div class="pay-step-label">Enter<br>KS
