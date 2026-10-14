# NEXOR-bot


<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>پنل مدیریت ربات روبیکا</title>
<style>
* { margin: 0; padding: 0; box-sizing: border-box; font-family: Tahoma, sans-serif; }
body { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); min-height: 100vh; padding: 20px; display: flex; justify-content: center; align-items: flex-start; }
.container { background: #fff; border-radius: 20px; padding: 30px; max-width: 700px; width: 100%; box-shadow: 0 20px 60px rgba(0,0,0,0.3); margin-top: 20px; }
h1 { text-align: center; color: #333; margin-bottom: 25px; font-size: 24px; }
h1 span { font-size: 30px; }
.section { background: #f8f9fa; border-radius: 12px; padding: 20px; margin-bottom: 20px; border: 1px solid #e9ecef; }
.section h3 { color: #555; margin-bottom: 15px; font-size: 16px; display: flex; align-items: center; gap: 8px; }
label { display: block; font-weight: bold; color: #444; margin-bottom: 6px; font-size: 14px; }
input[type="text"], input[type="password"], textarea, select { width: 100%; padding: 12px; border: 2px solid #dee2e6; border-radius: 10px; font-size: 14px; font-family: Tahoma, sans-serif; transition: border-color 0.3s; margin-bottom: 12px; }
input:focus, textarea:focus, select:focus { border-color: #667eea; outline: none; }
textarea { resize: vertical; min-height: 80px; direction: ltr; text-align: left; }
button { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: #fff; border: none; padding: 14px 20px; border-radius: 10px; font-size: 15px; font-weight: bold; cursor: pointer; width: 100%; transition: transform 0.2s, box-shadow 0.2s; margin-bottom: 10px; }
button:hover { transform: translateY(-2px); box-shadow: 0 8px 20px rgba(102,126,234,0.4); }
button:active { transform: translateY(0); }
button.secondary { background: #6c757d; }
button.success { background: linear-gradient(135deg, #28a745 0%, #20c997 100%); }
button.danger { background: linear-gradient(135deg, #dc3545 0%, #e83e8c 100%); }
.btn-row { display: flex; gap: 10px; }
.btn-row button { flex: 1; }
pre { background: #1e1e2e; color: #a6e3a1; padding: 15px; border-radius: 10px; overflow-x: auto; font-size: 12px; direction: ltr; text-align: left; max-height: 300px; overflow-y: auto; white-space: pre-wrap; word-break: break-all; }
.status { display: inline-block; padding: 6px 14px; border-radius: 20px; font-size: 13px; font-weight: bold; }
.status.ok { background: #d4edda; color: #155724; }
.status.err { background: #f8d7da; color: #721c24; }
.status.wait { background: #fff3cd; color: #856404; }
.toast { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); background: #333; color: #fff; padding: 12px 24px; border-radius: 10px; font-size: 14px; z-index: 999; opacity: 0; transition: opacity 0.3s; }
.toast.show { opacity: 1; }
.toggle-row { display: flex; justify-content: space-between; align-items: center; padding: 10px 0; border-bottom: 1px solid #e9ecef; }
.toggle-row:last-child { border-bottom: none; }
.toggle { position: relative; width: 50px; height: 26px; }
.toggle input { opacity: 0; width: 0; height: 0; }
.slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background: #ccc; border-radius: 26px; transition: 0.3s; }
.slider:before { position: absolute; content: ""; height: 20px; width: 20px; left: 3px; bottom: 3px; background: #fff; border-radius: 50%; transition: 0.3s; }
input:checked + .slider { background: #667eea; }
input:checked + .slider:before { transform: translateX(24px); }
@media (max-width: 600px) { .container { padding: 20px; } .btn-row { flex-direction: column; } }
</style>
</head>
<body>
<div class="container">
  <h1><span>🤖</span> پنل مدیریت ربات روبیکا</h1>

  <!-- وضعیت -->
  <div style="text-align:center; margin-bottom:20px;">
    <span id="statusBadge" class="status wait">⏳ در انتظار توکن</span>
  </div>

  <!-- بخش ۱: توکن -->
  <div class="section">
    <h3>🔑 توکن ربات</h3>
    <label>توکن دریافتی از @BotFather در روبیکا:</label>
    <input type="password" id="tokenInput" placeholder="توکن را اینجا بچسبانید (مثال: abc123...)" dir="ltr" style="text-align:left;">
    <div class="btn-row">
      <button onclick="saveToken()" class="success">💾 ذخیره و تست اتصال</button>
      <button onclick="getMe()" class="secondary">📋 دریافت اطلاعات ربات</button>
    </div>
    <pre id="getMeResult">اطلاعات ربات اینجا نمایش داده می‌شود...</pre>
  </div>

  <!-- بخش ۲: تنظیم Webhook -->
  <div class="section">
    <h3>🌐 تنظیم آدرس دریافت پیام‌ها (Webhook)</h3>
    <p style="font-size:13px; color:#888; margin-bottom:12px;">آدرسی که روبیکا پیام‌های کاربران را به آن بفرستد:</p>
    <input type="text" id="webhookUrl" placeholder="https://username.github.io/repo-name/receiveUpdate" dir="ltr" style="text-align:left;">
    <button onclick="setWebhook()">🔗 فعال‌سازی Webhook</button>
    <pre id="webhookResult">نتیجه تنظیم Webhook اینجا نمایش داده می‌شود...</pre>
  </div>

  <!-- بخش ۳: تنظیمات ربات -->
  <div class="section">
    <h3>⚙️ تنظیمات ربات</h3>
    <div class="toggle-row">
      <span>😄 بخش سرگرمی (جوک، فال، معما، بازی)</span>
      <label class="toggle"><input type="checkbox" id="entertainmentToggle" checked><span class="slider"></span></label>
    </div>
    <div class="toggle-row">
      <span>🎲 بازی سنگ کاغذ قیچی</span>
      <label class="toggle"><input type="checkbox" id="rpsToggle" checked><span class="slider"></span></label>
    </div>
    <div class="toggle-row">
      <span>🔮 فال حافظ</span>
      <label class="toggle"><input type="checkbox" id="fortuneToggle" checked><span class="slider"></span></label>
    </div>
    <div class="toggle-row">
      <span>🧩 معما و چیستان</span>
      <label class="toggle"><input type="checkbox" id="riddleToggle" checked><span class="slider"></span></label>
    </div>
    <div class="toggle-row">
      <span>🤔 حقیقت یا جرأت</span>
      <label class="toggle"><input type="checkbox" id="truthToggle" checked><span class="slider"></span></label>
    </div>
    <div class="toggle-row">
      <span>✨ جمله انگیزشی</span>
      <label class="toggle"><input type="checkbox" id="quoteToggle" checked><span class="slider"></span></label>
    </div>
    <br>
    <label>📝 پیام خوش‌آمدگویی (با /start ارسال می‌شود):</label>
    <textarea id="welcomeMsg" rows="3">سلام! به ربات سرگرمی خوش اومدی 🎉
من می‌تونم برات جوک بگم، فال بگیرم، معما بپرسم و کلی بازی کنم!

از دستورات زیر استفاده کن:
/start - شروع
/help - راهنما
/joke - جوک 😂
/fortune - فال 🔮
/riddle - معما 🧩
/rps - سنگ کاغذ قیچی ✊
/truth - حقیقت 🤔
/dare - جرأت 😈
/quote - جمله انگیزشی ✨
/dice - تاس 🎲</textarea>
    <button onclick="saveSettings()" class="success">💾 ذخیره تنظیمات</button>
  </div>

  <!-- بخش ۴: آزمایش دستی -->
  <div class="section">
    <h3>🧪 ارسال پیام آزمایشی</h3>
    <label>شناسه چت (chat_id):</label>
    <input type="text" id="testChatId" placeholder="u0..." dir="ltr" style="text-align:left;">
    <label>متن پیام:</label>
    <input type="text" id="testText" placeholder="سلام از پنل مدیریت!" value="سلام! ربات با موفقیت وصل شد ✅">
    <button onclick="sendTestMessage()">📤 ارسال پیام تست</button>
    <pre id="testResult">نتیجه ارسال اینجا نمایش داده می‌شود...</pre>
  </div>

  <!-- بخش ۵: راهنما -->
  <div class="section" style="background:#fff3cd; border-color:#ffc107;">
    <h3>📖 راهنمای سریع</h3>
    <ol style="font-size:13px; color:#555; line-height:2; padding-right:20px;">
      <li>توکن ربات رو از <b>@BotFather</b> توی روبیکا بگیر</li>
      <li>توکن رو تو کادر بالا وارد کن و <b>ذخیره</b> بزن</li>
      <li>آدرس GitHub Pages خودت رو تو کادر Webhook بذار (مثال: <code dir="ltr">https://username.github.io/repo-name/</code>)</li>
      <li>دکمه <b>فعال‌سازی Webhook</b> رو بزن</li>
      <li>ربات رو به یه گپ اضافه کن و <code dir="ltr">/start</code> بزن</li>
      <li>تمام! ربات سرگرمی‌ات آماده‌ست 🎉</li>
    </ol>
  </div>
</div>

<!-- Toast -->
<div class="toast" id="toast"></div>

<script>
// ==================== تنظیمات ====================
const API_BASE = 'https://botapi.rubika.ir/v3';
let token = localStorage.getItem('rubika_token') || '';

// ==================== توابع کمکی ====================
function showToast(msg, duration = 3000) {
  const t = document.getElementById('toast');
  t.textContent = msg;
  t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), duration);
}

function setStatus(type, text) {
  const b = document.getElementById('statusBadge');
  b.className = 'status ' + type;
  b.textContent = text;
}

function showResult(id, data) {
  document.getElementById(id).textContent = typeof data === 'string' ? data : JSON.stringify(data, null, 2);
}

// ==================== توکن ====================
function saveToken() {
  const t = document.getElementById('tokenInput').value.trim();
  if (!t) { showToast('❌ لطفاً توکن را وارد کنید'); return; }
  token = t;
  localStorage.setItem('rubika_token', token);
  showToast('✅ توکن ذخیره شد');
  setStatus('ok', '✅ آماده به کار');
  getMe();
}

async function apiCall(method, params = {}) {
  if (!token) { showToast('❌ ابتدا توکن را ذخیره کنید'); return null; }
  try {
    const res = await fetch(`${API_BASE}/${token}/${method}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(params)
    });
    const data = await res.json();
    return data;
  } catch (e) {
    return { error: e.message };
  }
}

async function getMe() {
  showResult('getMeResult', '⏳ در حال دریافت...');
  const data = await apiCall('getMe');
  showResult('getMeResult', data);
  if (data && data.status === 'OK') {
    setStatus('ok', '✅ ربات متصل است');
  } else {
    setStatus('err', '❌ خطا در اتصال');
  }
}

// ==================== Webhook ====================
async function setWebhook() {
  let url = document.getElementById('webhookUrl').value.trim();
  if (!url) { showToast('❌ آدرس Webhook را وارد کنید'); return; }
  if (!url.startsWith('https://')) { showToast('❌ آدرس باید با https شروع شود'); return; }

  showResult('webhookResult', '⏳ در حال تنظیم...');
  const data = await apiCall('updateBotEndpoint', {
    endpoint: url,
    type: 'Webhook'
  });
  showResult('webhookResult', data);
  if (data && data.status === 'OK') {
    showToast('✅ Webhook با موفقیت فعال شد');
  } else {
    showToast('❌ خطا در تنظیم Webhook');
  }
}

// ==================== تنظیمات ====================
function saveSettings() {
  const settings = {
    entertainment: document.getElementById('entertainmentToggle').checked,
    rps: document.getElementById('rpsToggle').checked,
    fortune: document.getElementById('fortuneToggle').checked,
    riddle: document.getElementById('riddleToggle').checked,
    truth: document.getElementById('truthToggle').checked,
    quote: document.getElementById('quoteToggle').checked,
    welcome: document.getElementById('welcomeMsg').value
  };
  localStorage.setItem('rubika_settings', JSON.stringify(settings));
  showToast('✅ تنظیمات ذخیره شد');
}

function loadSettings() {
  const s = JSON.parse(localStorage.getItem('rubika_settings') || '{}');
  if (s.entertainment !== undefined) document.getElementById('entertainmentToggle').checked = s.entertainment;
  if (s.rps !== undefined) document.getElementById('rpsToggle').checked = s.rps;
  if (s.fortune !== undefined) document.getElementById('fortuneToggle').checked = s.fortune;
  if (s.riddle !== undefined) document.getElementById('riddleToggle').checked = s.riddle;
  if (s.truth !== undefined) document.getElementById('truthToggle').checked = s.truth;
  if (s.quote !== undefined) document.getElementById('quoteToggle').checked = s.quote;
  if (s.welcome) document.getElementById('welcomeMsg').value = s.welcome;
}

// ==================== ارسال تست ====================
async function sendTestMessage() {
  const chatId = document.getElementById('testChatId').value.trim();
  const text = document.getElementById('testText').value.trim();
  if (!chatId) { showToast('❌ شناسه چت را وارد کنید'); return; }

  showResult('testResult', '⏳ در حال ارسال...');
  const data = await apiCall('sendMessage', { chat_id: chatId, text: text });
  showResult('testResult', data);
  if (data && data.status === 'OK') {
    showToast('✅ پیام ارسال شد');
  } else {
    showToast('❌ خطا در ارسال');
  }
}

// ==================== بارگذاری اولیه ====================
window.onload = function() {
  if (token) {
    document.getElementById('tokenInput').value = token;
    setStatus('ok', '✅ توکن ذخیره شده');
    getMe();
  }
  loadSettings();

  // پر کردن خودکار آدرس Webhook
  const currentUrl = window.location.href.split('?')[0];
  document.getElementById('webhookUrl').value = currentUrl;
};
</script>
</body>
</html>
