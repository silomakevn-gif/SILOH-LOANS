<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>SILOH LOANS GOLD EDITION</title>
  <meta name="description" content="Fast reliable loan application platform" />
  <style>
    :root{
      --bg:#071a12;
      --card:#0d1f15;
      --card2:#132a1d;
      --line:#243f2f;
      --text:#e8f5e9;
      --muted:#b7d9be;
      --gold:#d4af37;
      --gold2:#facc15;
      --green:#22c55e;
      --danger:#ef4444;
      --shadow:0 20px 60px rgba(0,0,0,.35);
      --radius:24px;
      --radius2:16px;
      --max:1160px;
      --font: Inter, system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif;
    }
    *{box-sizing:border-box}
    html,body{margin:0;padding:0;background:
      radial-gradient(900px 600px at 12% 8%, rgba(212,175,55,.18), transparent 60%),
      radial-gradient(700px 500px at 85% 90%, rgba(34,197,94,.12), transparent 60%),
      var(--bg);
      color:var(--text);font-family:var(--font);line-height:1.5}
    a{color:inherit;text-decoration:none}
    .wrap{max-width:var(--max);margin:0 auto;padding:18px}
    .topbar{position:sticky;top:0;z-index:10;background:rgba(7,26,18,.8);backdrop-filter:blur(14px);border-bottom:1px solid rgba(255,255,255,.08)}
    .topbar .wrap{display:flex;align-items:center;justify-content:space-between;gap:12px;padding-block:14px}
    .brand{display:flex;align-items:center;gap:12px}
    .logo{width:46px;height:46px;border-radius:999px;background:
      linear-gradient(135deg, rgba(250,204,21,.95), rgba(212,175,55,.55));
      box-shadow:0 10px 30px rgba(212,175,55,.25)}
    .brand h1{margin:0;font-size:16px;letter-spacing:.18em}
    .brand p{margin:2px 0 0;color:var(--muted);font-size:12px;letter-spacing:.12em}
    .nav{display:flex;gap:8px;flex-wrap:wrap}
    .btn{
      border:1px solid rgba(212,175,55,.25);
      background:rgba(255,255,255,.03);
      color:var(--text);
      padding:10px 14px;border-radius:999px;
      cursor:pointer;transition:.2s ease;font-weight:600
    }
    .btn:hover{transform:translateY(-1px);border-color:rgba(250,204,21,.5)}
    .btn.gold{background:linear-gradient(90deg,var(--gold2),var(--gold));color:#111;border:none}
    .hero{padding:28px 0 14px}
    .grid{display:grid;grid-template-columns:1.15fr .85fr;gap:18px}
    .card{
      background:linear-gradient(180deg,var(--card2),var(--card));
      border:1px solid rgba(255,255,255,.08);
      border-radius:var(--radius);
      box-shadow:var(--shadow);
      overflow:hidden
    }
    .card .inner{padding:24px}
    .badge{display:inline-flex;align-items:center;gap:8px;padding:8px 12px;border-radius:999px;background:rgba(34,197,94,.08);border:1px solid rgba(34,197,94,.16);color:#bff5cf;font-size:12px}
    .badge .dot{width:8px;height:8px;border-radius:50%;background:var(--green);box-shadow:0 0 0 4px rgba(34,197,94,.15)}
    .title{font-size:clamp(30px,5vw,54px);line-height:1.03;margin:14px 0 12px;font-weight:800}
    .title .gold{background:linear-gradient(90deg,#fde68a,#facc15,#d4af37);-webkit-background-clip:text;background-clip:text;color:transparent}
    .sub{color:var(--muted);max-width:58ch;margin:0 0 18px}
    .stats{display:flex;gap:10px;flex-wrap:wrap;margin:18px 0 0}
    .pill{padding:10px 12px;border-radius:999px;background:rgba(255,255,255,.04);border:1px solid rgba(255,255,255,.08);font-size:13px;color:#dff3e5}
    .section{padding:18px 0}
    .form{display:grid;grid-template-columns:1fr 1fr;gap:14px}
    .field{display:flex;flex-direction:column;gap:7px}
    .field label{font-size:13px;color:#d7eadc}
    .field input,.field select,.field textarea{
      width:100%;border-radius:14px;border:1px solid rgba(255,255,255,.08);
      background:rgba(255,255,255,.04);color:var(--text);
      padding:13px 14px;font:inherit;outline:none
    }
    .field input:focus,.field select:focus,.field textarea:focus{border-color:rgba(250,204,21,.55);box-shadow:0 0 0 3px rgba(250,204,21,.12)}
    .full{grid-column:1/-1}
    .actions{display:flex;gap:10px;flex-wrap:wrap;margin-top:6px}
    .msg{margin-top:14px;padding:12px 14px;border-radius:14px;display:none}
    .msg.ok{display:block;background:rgba(34,197,94,.1);border:1px solid rgba(34,197,94,.2);color:#d7ffe3}
    .msg.err{display:block;background:rgba(239,68,68,.1);border:1px solid rgba(239,68,68,.2);color:#ffd6d6}
    .list{display:grid;gap:12px}
    .item{
      padding:14px;border-radius:16px;background:rgba(255,255,255,.03);
      border:1px solid rgba(255,255,255,.07)
    }
    .item strong{display:block;margin-bottom:4px}
    .item span{color:var(--muted);font-size:13px}
    .footer{padding:18px 0 30px;color:var(--muted);font-size:13px}
    @media (max-width:900px){.grid,.form{grid-template-columns:1fr}}
  </style>
</head>
<body>
  <header class="topbar">
    <div class="wrap">
      <div class="brand">
        <div class="logo"></div>
        <div>
          <h1>SILOH LOANS</h1>
          <p>GOLD EDITION</p>
        </div>
      </div>
      <nav class="nav">
        <a class="btn" href="#apply">Apply</a>
        <a class="btn" href="#support">Support</a>
        <a class="btn gold" href="#admin">Admin</a>
      </nav>
    </div>
  </header>

  <main class="wrap">
    <section class="hero grid">
      <div class="card">
        <div class="inner">
          <div class="badge"><span class="dot"></span> Fast, secure, and trusted</div>
          <h2 class="title">Fast Reliable <span class="gold">Loans</span> in Minutes</h2>
          <p class="sub">
            Apply online, get reviewed quickly, and stay informed with a clear repayment plan.
            Contact support anytime on 0702994132.
          </p>
          <div class="stats">
            <div class="pill">Support: 0702994132</div>
            <div class="pill">Payment Day: 0702994132</div>
            <div class="pill">Clear repayment terms</div>
            <div class="pill">Mobile-friendly</div>
          </div>
        </div>
      </div>

      <div class="card" id="support">
        <div class="inner">
          <h3 style="margin:0 0 10px">Contact & Payments</h3>
          <div class="list">
            <div class="item">
              <strong>Assistance Number</strong>
              <span>0702994132</span>
            </div>
            <div class="item">
              <strong>Payment Day</strong>
              <span>0702994132</span>
            </div>
            <div class="item">
              <strong>Note</strong>
              <span>Use the official admin dashboard for all submission reviews.</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="section grid" id="apply">
      <div class="card">
        <div class="inner">
          <h3 style="margin:0 0 16px">Loan Application</h3>
          <form id="loanForm" class="form">
            <div class="field">
              <label for="fullName">Full name</label>
              <input id="fullName" name="fullName" placeholder="Enter full name" required />
            </div>
            <div class="field">
              <label for="phone">Phone number</label>
              <input id="phone" name="phone" placeholder="07XXXXXXXX" required />
            </div>
            <div class="field">
              <label for="idNumber">ID number</label>
              <input id="idNumber" name="idNumber" placeholder="ID number" required />
            </div>
            <div class="field">
              <label for="mpesaName">M-Pesa name</label>
              <input id="mpesaName" name="mpesaName" placeholder="Name on M-Pesa" required />
            </div>
            <div class="field">
              <label for="amount">Amount</label>
              <select id="amount" name="amount" required>
                <option value="">Select amount</option>
                <option>1000</option>
                <option>1500</option>
                <option>2000</option>
                <option>3500</option>
                <option>4999</option>
              </select>
            </div>
            <div class="field">
              <label for="dueDay">Repayment day</label>
              <input id="dueDay" name="dueDay" placeholder="e.g. 30" required />
            </div>
            <div class="field full">
              <label for="notes">Notes</label>
              <textarea id="notes" name="notes" rows="4" placeholder="Any extra details"></textarea>
            </div>
            <div class="field full">
              <label style="display:flex;gap:10px;align-items:center">
                <input type="checkbox" id="terms" required />
                I confirm the details are correct and I agree to the terms.
              </label>
            </div>
            <div class="full actions">
              <button class="btn gold" type="submit">Submit Application</button>
              <button class="btn" type="reset">Clear</button>
            </div>
            <div id="formMsg" class="msg"></div>
          </form>
        </div>
      </div>

      <div class="card" id="admin">
        <div class="inner">
          <h3 style="margin:0 0 16px">Admin Dashboard</h3>
          <div class="list">
            <div class="item">
              <strong>Secure Access</strong>
              <span>Connect this to a backend login before going live.</span>
            </div>
            <div class="item">
              <strong>Submissions</strong>
              <span>Store applications in Firebase, Supabase, or MySQL.</span>
            </div>
            <div class="item">
              <strong>Client View</strong>
              <span>Show clients only their own loan status, not other users’ data.</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <div class="footer">
      © 2026 SILOH LOANS GOLD EDITION. Support: 0702994132.
    </div>
  </main>

  <script>
    const form = document.getElementById('loanForm');
    const msg = document.getElementById('formMsg');

    function setMsg(type, text){
      msg.className = 'msg ' + type;
      msg.textContent = text;
    }

    form.addEventListener('submit', async (e) => {
      e.preventDefault();

      const fullName = form.fullName.value.trim();
      const phone = form.phone.value.trim();
      const idNumber = form.idNumber.value.trim();
      const mpesaName = form.mpesaName.value.trim();
      const amount = form.amount.value.trim();
      const dueDay = form.dueDay.value.trim();
      const notes = form.notes.value.trim();

      if (fullName.length < 3) return setMsg('err', 'Enter a valid full name.');
      if (!/^(07|01)d{8}$/.test(phone)) return setMsg('err', 'Use a valid Kenya phone number.');
      if (idNumber.length < 6) return setMsg('err', 'Enter a valid ID number.');
      if (!amount) return setMsg('err', 'Select an amount.');
      if (!dueDay) return setMsg('err', 'Enter a repayment day.');
      if (!document.getElementById('terms').checked) return setMsg('err', 'Accept the terms first.');

      const payload = { fullName, phone, idNumber, mpesaName, amount, dueDay, notes, support: '0702994132' };

      localStorage.setItem('siloh_last_application', JSON.stringify(payload));
      setMsg('ok', 'Application saved locally. Connect a backend next for real submissions.');
      form.reset();
    });
  </script>
</body>
</html># SILOH-LOANS