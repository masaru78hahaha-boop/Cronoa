# Cronoa
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>Cronoa</title>
<style>
  * {
    box-sizing: border-box;
    user-select: none;
    -webkit-user-select: none;
    -webkit-tap-highlight-color: transparent;
    margin: 0;
    padding: 0;
  }

  body {
    background-color: #030303;
    color: #c5a059;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    padding: 15px 12px 40px 12px;
    background-image: radial-gradient(circle at 50% -10%, rgba(212, 175, 55, 0.3) 0%, transparent 60%);
    overflow-x: hidden;
  }

  .main-header {
    position: relative;
    width: 100%;
    max-width: 350px;
    height: 44px;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 6px;
  }

  .main-title {
    font-size: 18px;
    font-weight: 500;
    color: #f3e5c8;
    letter-spacing: 1px;
    text-shadow: 0 0 10px rgba(212, 175, 55, 0.4);
  }

  .datetime-widget {
    position: absolute;
    left: 0;
    top: 50%;
    transform: translateY(-50%);
    background: rgba(12, 10, 8, 0.9);
    border: 1px solid #7a633a;
    border-radius: 8px;
    padding: 3px 6px;
    font-size: 9px;
    color: #e6c875;
    line-height: 1.2;
    box-shadow: inset 0 0 5px rgba(0,0,0,0.5);
    white-space: nowrap;
  }
  .datetime-widget .time-val {
    font-size: 11px;
    font-weight: bold;
    color: #ffd700;
  }

  .settings-btn {
    position: absolute;
    right: 0;
    top: 50%;
    transform: translateY(-50%);
    width: 36px;
    height: 36px;
    border-radius: 50%;
    background: linear-gradient(135deg, #ffd700 0%, #b8860b 50%, #8a6d29 100%);
    border: 1.5px solid #ffe89c;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #0d0b09;
    font-size: 18px;
    cursor: pointer;
    box-shadow: 0 0 12px rgba(212, 175, 55, 0.6), inset 0 0 8px rgba(255, 255, 255, 0.3);
    transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }
  .settings-btn.spinning {
    transform: translateY(-50%) rotate(180deg);
  }

  .ornament {
    display: flex;
    justify-content: center;
    align-items: center;
    color: #7a633a;
    font-size: 8px;
    margin-bottom: 10px;
  }
  .ornament::before, .ornament::after {
    content: '';
    width: 40px;
    height: 1px;
    background: linear-gradient(90deg, transparent, #7a633a, transparent);
    margin: 0 8px;
  }

  .main-content {
    width: 100%;
    max-width: 350px;
    display: flex;
    flex-direction: column;
    align-items: center;
    transition: opacity 0.3s ease, transform 0.3s ease;
  }
  
  .main-content.hidden {
    opacity: 0;
    transform: scale(0.95);
    pointer-events: none;
    position: absolute;
  }

  .section-box {
    width: 100%;
    background: rgba(12, 10, 8, 0.9);
    border: 1px solid #4a3b22;
    border-radius: 12px;
    padding: 12px 14px;
    margin-bottom: 10px;
    box-shadow: inset 0 0 10px rgba(0, 0, 0, 0.8);
    transition: border-color 0.3s, box-shadow 0.3s;
  }

  .section-box.active-mode {
    border: 1px solid #ffd700;
    box-shadow: 0 0 12px rgba(212, 175, 55, 0.25), inset 0 0 8px rgba(212, 175, 55, 0.15);
  }

  .section-title {
    font-size: 14px;
    color: #d4af37;
    margin-bottom: 8px;
    font-weight: 500;
  }

  .input-row { display: flex; align-items: center; gap: 6px; margin-bottom: 8px; }
  .icon-btn { width: 36px; height: 36px; flex-shrink: 0; border: 1px solid #5a4729; border-radius: 8px; background: #080705; display: flex; justify-content: center; align-items: center; font-size: 16px; }
  .input-box { flex: 1; height: 36px; border: 1px solid #5a4729; border-radius: 8px; background: #080705; display: flex; align-items: center; justify-content: center; padding: 0 6px; color: #f3e5c8; font-size: 13px; white-space: nowrap; overflow: hidden; }
  .unit-btn { width: 75px; height: 36px; flex-shrink: 0; border: 1px solid #7a633a; border-radius: 8px; background: #2a2115; color: #c5a059; font-size: 11px; display: flex; justify-content: center; align-items: center; cursor: pointer; }
  .textarea-box { width: 100%; height: 48px; border: 1px solid #5a4729; border-radius: 8px; background: #080705; padding: 6px 8px; color: #ffffff; font-size: 12px; margin-bottom: 8px; outline: none; resize: none; user-select: text; -webkit-user-select: text; }
  
  .btn-interactive { cursor: pointer; }

  .glow-btn { width: 100%; height: 38px; border-radius: 8px; background: linear-gradient(180deg, #e6c875 0%, #9e7b2f 100%); border: 1px solid #ffe89c; color: #0d0b09; font-size: 13px; font-weight: bold; display: flex; justify-content: center; align-items: center; }
  .btn-group { display: flex; gap: 8px; }
  .gold-btn-half { flex: 1; height: 38px; border-radius: 8px; background: linear-gradient(180deg, #e6c875 0%, #9e7b2f 100%); border: 1px solid #ffe89c; color: #0d0b09; font-size: 12px; font-weight: bold; display: flex; justify-content: center; align-items: center; gap: 4px; }

  .picker-container { position: relative; width: 100%; height: 150px; background: #080705; border: 2px solid #ffd700; border-radius: 10px; display: flex; justify-content: space-around; margin-top: 6px; overflow: hidden; transition: box-shadow 0.2s ease, border-color 0.2s ease; }
  .picker-container.wheel-glowing { border-color: #ffffff !important; box-shadow: 0 0 25px rgba(255, 215, 0, 0.95), inset 0 0 15px rgba(255, 215, 0, 0.6) !important; }
  .selection-overlay { position: absolute; top: 59px; left: 4px; right: 4px; height: 32px; border: 1.5px solid #ffe89c; border-radius: 16px; pointer-events: none; z-index: 10; box-shadow: 0 0 8px rgba(212, 175, 55, 0.3); }
  .column-wrapper { height: 100%; overflow-y: scroll; scroll-snap-type: y mandatory; -webkit-overflow-scrolling: touch; scrollbar-width: none; scroll-behavior: smooth; }
  .column-wrapper::-webkit-scrollbar { display: none; }
  .col-date { width: 45%; } .col-hour { width: 25%; } .col-min  { width: 25%; }
  .wheel-padding { height: 59px; }
  .item { height: 32px; line-height: 32px; font-size: 13px; color: #5c4d36; text-align: center; scroll-snap-align: center; transition: color 0.1s; }
  .col-date .item { text-align: left; padding-left: 10px; }
  .header-sub-row { display: flex; justify-content: space-between; align-items: center; gap: 6px; }
  .btn-done-small { width: 55px; height: 26px; background: linear-gradient(135deg, #ffd700 0%, #8a6d29 100%); border: 1px solid #ffe89c; border-radius: 13px; color: #0d0b09; font-size: 11px; font-weight: bold; display: flex; justify-content: center; align-items: center; flex-shrink: 0; }

  .settings-content {
    width: 100%;
    max-width: 350px;
    background: linear-gradient(180deg, #1a150e 0%, #0d0b09 100%);
    border: 2px solid #b8860b;
    border-radius: 16px;
    padding: 14px 12px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.8), 0 0 15px rgba(212, 175, 55, 0.3), inset 0 0 15px rgba(255, 215, 0, 0.1);
    display: none;
    flex-direction: column;
    gap: 10px;
    opacity: 0;
    transform: translateY(-20px);
    transition: opacity 0.4s ease, transform 0.4s cubic-bezier(0.175, 0.885, 0.32, 1.275);
  }
  
  .settings-content.show {
    display: flex;
    opacity: 1;
    transform: translateY(0);
  }

  .settings-header {
    text-align: center;
    font-size: 14px;
    color: #ffd700;
    font-weight: bold;
    letter-spacing: 2px;
    text-shadow: 0 0 8px rgba(255, 215, 0, 0.5);
  }

  .setting-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 9px 12px;
    background: rgba(20, 16, 12, 0.8);
    border: 1px solid #5a4729;
    border-radius: 10px;
    box-shadow: inset 0 0 5px rgba(0,0,0,0.5);
  }

  .setting-label {
    font-size: 12px;
    color: #f3e5c8;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  
  .setting-icon {
    font-size: 14px;
  }

  .summary-panel {
    background: rgba(10, 8, 6, 0.95);
    border: 1px solid #ffd700;
    border-radius: 10px;
    padding: 10px 12px;
    display: flex;
    flex-direction: column;
    gap: 8px;
    box-shadow: 0 0 10px rgba(212, 175, 55, 0.25), inset 0 0 8px rgba(0,0,0,0.9);
  }

  .summary-title {
    font-size: 11px;
    color: #ffd700;
    font-weight: bold;
    border-bottom: 1px solid #4a3b22;
    padding-bottom: 4px;
    display: flex;
    align-items: center;
    gap: 6px;
  }

  .summary-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 11px;
    color: #d1c7b7;
    border-bottom: 1px dashed rgba(90, 71, 41, 0.4);
    padding-bottom: 6px;
  }
  .summary-row:last-child {
    border-bottom: none;
    padding-bottom: 0;
  }

  .summary-label-col {
    color: #9e8b6c;
    width: 65px;
    flex-shrink: 0;
  }

  .summary-val-col {
    color: #f3e5c8;
    flex: 1;
    text-align: left;
    padding: 0 6px;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }

  .del-row-btn {
    background: rgba(138, 43, 43, 0.3);
    border: 1px solid #b22222;
    color: #ff9999;
    font-size: 10px;
    padding: 3px 6px;
    border-radius: 6px;
    cursor: pointer;
    flex-shrink: 0;
    transition: background 0.2s;
  }
  .del-row-btn:active {
    background: rgba(138, 43, 43, 0.7);
  }

  .link-btn {
    width: 100%;
    height: 34px;
    border-radius: 8px;
    background: rgba(25, 20, 14, 0.9);
    border: 1px solid #7a633a;
    color: #e6c875;
    font-size: 12px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 12px;
    cursor: pointer;
    box-shadow: inset 0 0 5px rgba(0,0,0,0.5);
  }

  .toggle-switch {
    width: 42px;
    height: 22px;
    background: #000;
    border: 1.5px solid #5a4729;
    border-radius: 11px;
    position: relative;
    cursor: pointer;
    transition: border-color 0.3s;
  }
  .toggle-switch.on {
    border-color: #ffd700;
    box-shadow: 0 0 8px rgba(212, 175, 55, 0.4);
  }
  .toggle-knob {
    position: absolute;
    top: 1.5px; left: 1.5px;
    width: 15px; height: 15px;
    background: #5a4729;
    border-radius: 50%;
    transition: transform 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.275), background 0.3s;
  }
  .toggle-switch.on .toggle-knob {
    transform: translateX(20px);
    background: linear-gradient(135deg, #ffd700 0%, #b8860b 100%);
    box-shadow: 0 0 5px #ffd700;
  }
</style>
</head>
<body>

  <div class="main-header">
    <div class="datetime-widget" id="live-datetime">
      <div id="widget-date">---</div>
      <div class="time-val" id="widget-time">--:--:--</div>
    </div>
    <div class="main-title" id="top-title">Cronoa</div>
    <div class="settings-btn btn-interactive" id="settings-btn">⚙</div>
  </div>
  <div class="ornament">◆</div>

  <div class="main-content" id="main-content">
    <div class="section-box active-mode" id="sec-datetime">
      <div class="section-title">時間指定</div>
      <div class="input-row">
        <div class="icon-btn btn-interactive" id="cal-btn">📅</div>
        <div class="input-box btn-interactive" id="display-datetime-1">---</div>
        <div class="unit-btn btn-interactive" id="unit-btn-1">1分単位</div>
      </div>
      <textarea class="textarea-box" id="comment-input-1" placeholder="メッセージを入力..."></textarea>
      <div class="glow-btn btn-interactive" id="set-time-btn">時間指定で通知を設定</div>
    </div>

    <div class="section-box" id="sec-app-add">
      <div class="section-title">リマインダー日時指定</div>
      <div class="input-row">
        <div class="input-box btn-interactive" id="display-datetime-2">---</div>
        <div class="unit-btn btn-interactive" id="unit-btn-2">1分単位</div>
      </div>
      <div class="section-title" style="margin-top:6px;">件名・内容</div>
      <textarea class="textarea-box" id="comment-input-2" placeholder="件名を入力..."></textarea>
      
      <div class="section-title" style="margin-top:6px;">登録先を選択</div>
      <div class="btn-group">
        <div class="gold-btn-half btn-interactive" id="add-cal-btn">📅 カレンダー</div>
        <div class="gold-btn-half btn-interactive" id="add-rem-btn">📌 リマインダー</div>
      </div>
    </div>

    <div class="section-box" id="sec-recurring">
      <div class="header-sub-row">
        <div class="section-title" style="margin:0; flex-shrink:0;">定期</div>
        <div class="input-box btn-interactive" id="display-recurring" style="height:26px; font-size:11px;">時分・曜日指定</div>
        <div class="btn-done-small btn-interactive" id="done-btn">完了</div>
      </div>

      <div class="picker-container" id="picker-container">
        <div class="selection-overlay"></div>
        <div class="column-wrapper col-date" id="wrap-date"><div class="wheel-padding"></div><div id="list-date"></div><div class="wheel-padding"></div></div>
        <div class="column-wrapper col-hour" id="wrap-hour"><div class="wheel-padding"></div><div id="list-hour"></div><div class="wheel-padding"></div></div>
        <div class="column-wrapper col-min" id="wrap-min"><div class="wheel-padding"></div><div id="list-min"></div><div class="wheel-padding"></div></div>
      </div>
    </div>
  </div>

  <div class="settings-content" id="settings-content">
    <div class="settings-header">SETTINGS</div>
    <div class="setting-item">
      <div class="setting-label"><span class="setting-icon">🎵</span> 効果音（SE）</div>
      <div class="toggle-switch on" id="toggle-se"><div class="toggle-knob"></div></div>
    </div>
    
    <div class="summary-panel">
      <div class="summary-title"><span class="setting-icon">📋</span> 現在の設定・予約状況</div>
      
      <div class="summary-row">
        <span class="summary-label-col">⏰ 時間指定:</span>
        <span class="summary-val-col" id="sum-datetime">未設定</span>
        <div class="del-row-btn" id="del-datetime" style="display:none;">削除</div>
      </div>
      
      <div class="summary-row">
        <span class="summary-label-col">🔄 定期:</span>
        <span class="summary-val-col" id="sum-recurring">未設定</span>
        <div class="del-row-btn" id="del-recurring" style="display:none;">削除</div>
      </div>

      <div class="summary-row">
        <span class="summary-label-col">📌 リマイン:</span>
        <span class="summary-val-col" id="sum-reminder">未設定</span>
        <div class="del-row-btn" id="del-reminder" style="display:none;">削除</div>
      </div>

      <div class="summary-row">
        <span class="summary-label-col">📅 カレンダ:</span>
        <span class="summary-val-col" id="sum-calendar">未設定</span>
        <div class="del-row-btn" id="del-calendar" style="display:none;">削除</div>
      </div>
    </div>

    <div class="link-btn btn-interactive" id="privacy-btn">
      <span class="setting-label" style="color:#e6c875;"><span class="setting-icon">🔒</span> プライバシーポリシー</span>
      <span style="font-size:10px; color:#9e8b6c;">＞</span>
    </div>
    <div class="link-btn btn-interactive" id="feedback-btn">
      <span class="setting-label" style="color:#e6c875;"><span class="setting-icon">📝</span> ご要望・フィードバック</span>
      <span style="font-size:10px; color:#9e8b6c;">＞</span>
    </div>
  </div>

<script>
  function updateLiveClock() {
    const now = new Date();
    const days = ['日', '月', '火', '水', '木', '金', '土'];
    const y = now.getFullYear();
    const m = now.getMonth() + 1;
    const d = now.getDate();
    const day = days[now.getDay()];
    
    document.getElementById('widget-date').textContent = y + '年 ' + m + '月' + d + '日(' + day + ')';
    
    const h = String(now.getHours()).padStart(2, '0');
    const min = String(now.getMinutes()).padStart(2, '0');
    const s = String(now.getSeconds()).padStart(2, '0');
    document.getElementById('widget-time').textContent = h + ':' + min + ':' + s;
  }
  setInterval(updateLiveClock, 1000);
  updateLiveClock();

  let isSoundEnabled = true;
  const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
  function playTickSound() {
    if (!isSoundEnabled) return;
    try {
      if (audioCtx.state === 'suspended') audioCtx.resume();
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = 'triangle';
      osc.frequency.setValueAtTime(2800, audioCtx.currentTime);
      osc.frequency.exponentialRampToValueAtTime(400, audioCtx.currentTime + 0.015);
      gain.gain.setValueAtTime(0.12, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + 0.015);
      osc.connect(gain); gain.connect(audioCtx.destination);
      osc.start(); osc.stop(audioCtx.currentTime + 0.015);
    } catch(e){}
  }

  const settingsBtn = document.getElementById('settings-btn');
  const mainContent = document.getElementById('main-content');
  const settingsContent = document.getElementById('settings-content');
  const topTitle = document.getElementById('top-title');
  let isSettingsOpen = false;

  settingsBtn.addEventListener('click', function() {
    isSettingsOpen = !isSettingsOpen;
    settingsBtn.classList.toggle('spinning');
    if (isSettingsOpen) {
      settingsBtn.textContent = '↩';
      topTitle.textContent = '設定';
      mainContent.classList.add('hidden');
      setTimeout(function() { mainContent.style.display = 'none'; settingsContent.style.display = 'flex'; setTimeout(function() { settingsContent.classList.add('show'); }, 10); }, 300);
    } else {
      settingsBtn.innerHTML = '⚙';
      topTitle.textContent = 'Cronoa';
      settingsContent.classList.remove('show');
      setTimeout(function() { settingsContent.style.display = 'none'; mainContent.style.display = 'flex'; setTimeout(function() { mainContent.classList.remove('hidden'); }, 10); }, 400);
    }
  });

  document.getElementById('toggle-se').addEventListener('click', function() {
    this.classList.toggle('on'); isSoundEnabled = this.classList.contains('on');
    if (isSoundEnabled) playTickSound();
  });

  // Web標準用：プライバシーポリシーとフィードバック
  document.getElementById('privacy-btn').addEventListener('click', function() {
    alert("🔒 プライバシーポリシー\n\n当アプリはユーザーのデバイス内（ローカル）で処理を行います。\n外部サーバーへの個人情報の送信や収集は一切行っていませんので安心してご利用ください。");
  });
  document.getElementById('feedback-btn').addEventListener('click', function() {
    if (confirm("📝 ご要望・フィードバック\n\nアプリに対するご要望や不具合の報告は、Googleフォームからお寄せください。\nフォームを開きますか？")) {
      window.open("https://forms.gle/sDNGHM46H9iUnjuE9", "_blank");
    }
  });

  // localStorage 連携と表示初期化
  function updateRow(type, val) {
    const el = document.getElementById('sum-' + type);
    const btn = document.getElementById('del-' + type);
    if (val && val !== "未設定") {
      el.textContent = val;
      btn.style.display = 'block';
    } else {
      el.textContent = "未設定";
      btn.style.display = 'none';
    }
  }

  function loadSavedData() {
    updateRow('datetime', localStorage.getItem("cronoa_sum_datetime") || "未設定");
    updateRow('recurring', localStorage.getItem("cronoa_sum_recurring") || "未設定");
    updateRow('reminder', localStorage.getItem("cronoa_sum_reminder") || "未設定");
    updateRow('calendar', localStorage.getItem("cronoa_sum_calendar") || "未設定");
  }
  loadSavedData();

  function setupDeleteBtn(type) {
    document.getElementById('del-' + type).addEventListener('click', function(e) {
      e.stopPropagation();
      localStorage.removeItem("cronoa_sum_" + type);
      updateRow(type, "未設定");
      alert("🗑 削除しました\n指定された設定を解除しました。");
    });
  }
  setupDeleteBtn('datetime');
  setupDeleteBtn('recurring');
  setupDeleteBtn('reminder');
  setupDeleteBtn('calendar');

  const pickerBox = document.getElementById('picker-container');
  let glowTimer = null;
  function triggerWheelGlow() {
    pickerBox.classList.add('wheel-glowing');
    if (glowTimer) clearTimeout(glowTimer);
    glowTimer = setTimeout(function() { pickerBox.classList.remove('wheel-glowing'); }, 220);
  }

  let currentMode = 'datetime';
  let isProgrammaticScroll = false;
  
  const modeStep = { datetime: 1, appAdd: 1, recurring: 1 };
  const modeState = {
    datetime: { dateIdx: 0, hourIdx: new Date().getHours(), minIdx: new Date().getMinutes() },
    appAdd:   { dateIdx: 0, hourIdx: new Date().getHours(), minIdx: new Date().getMinutes() },
    recurring:{ dateIdx: 0, hourIdx: new Date().getHours(), minIdx: new Date().getMinutes() }
  };

  const daysOfWeek = ['日', '月', '火', '水', '木', '金', '土'];
  const today = new Date();
  const dateObjects = [];
  const dates = [];

  for (let i = 0; i < 30; i++) {
    const d = new Date(); d.setDate(today.getDate() + i);
    dateObjects.push(d);
    if (i === 0) dates.push('今日');
    else dates.push((d.getMonth() + 1) + '月' + d.getDate() + '日 ' + daysOfWeek[d.getDay()]);
  }

  const hours = Array.from({length: 24}, function(_, i) { return ('0' + i).slice(-2) + '時'; });
  
  function getMinsArray(step) {
    let arr = [];
    for (let i = 0; i < 60; i += step) {
      arr.push(('0' + i).slice(-2) + '分');
    }
    return arr;
  }

  let currentMins = getMinsArray(1);

  function populateList(id, arr) {
    const container = document.getElementById(id);
    container.innerHTML = '';
    arr.forEach(function(text) {
      const div = document.createElement('div');
      div.className = 'item'; div.textContent = text;
      container.appendChild(div);
    });
  }
  populateList('list-date', dates);
  populateList('list-hour', hours);
  populateList('list-min', currentMins);

  function WheelController(wrapId, getCountFunc, onScrollCallback) {
    this.wrap = document.getElementById(wrapId);
    this.getCount = getCountFunc;
    this.selectedIndex = 0;
    this.onScrollCB = onScrollCallback;
    var self = this;
    this.wrap.addEventListener('scroll', function() { self.onScroll(); }, {passive: true});
  }
  WheelController.prototype.onScroll = function() {
    const count = this.getCount();
    const index = Math.min(Math.max(0, Math.round(this.wrap.scrollTop / 32)), count - 1);
    if (index !== this.selectedIndex) {
      this.selectedIndex = index;
      if (!isProgrammaticScroll) { playTickSound(); triggerWheelGlow(); }
      if (this.onScrollCB) this.onScrollCB(index);
      updateDisplay();
    }
  };
  WheelController.prototype.setIndex = function(index) {
    const count = this.getCount();
    if (index >= count) index = count - 1;
    this.selectedIndex = index;
    this.wrap.scrollTop = index * 32;
  };

  const wDate = new WheelController('wrap-date', function() { return dates.length; }, function(idx) { modeState[currentMode].dateIdx = idx; });
  const wHour = new WheelController('wrap-hour', function() { return hours.length; }, function(idx) { modeState[currentMode].hourIdx = idx; });
  const wMin  = new WheelController('wrap-min', function() { return currentMins.length; }, function(idx) { modeState[currentMode].minIdx = idx; });

  function updateMinsListForCurrentMode() {
    const step = modeStep[currentMode];
    currentMins = getMinsArray(step);
    populateList('list-min', currentMins);
    
    let st = modeState[currentMode];
    let maxIdx = currentMins.length - 1;
    if (st.minIdx > maxIdx) st.minIdx = maxIdx;
  }

  function switchMode(newMode) {
    currentMode = newMode;
    document.getElementById('sec-datetime').classList.toggle('active-mode', currentMode === 'datetime');
    document.getElementById('sec-app-add').classList.toggle('active-mode', currentMode === 'appAdd');
    document.getElementById('sec-recurring').classList.toggle('active-mode', currentMode === 'recurring');
    
    updateMinsListForCurrentMode();
    const st = modeState[currentMode];
    isProgrammaticScroll = true;
    wDate.setIndex(st.dateIdx); 
    wHour.setIndex(st.hourIdx); 
    wMin.setIndex(st.minIdx);
    updateDisplay();
    setTimeout(function() { isProgrammaticScroll = false; }, 100);
  }

  function updateDisplay() {
    ['datetime', 'appAdd', 'recurring'].forEach(function(m) {
      const st = modeState[m];
      const d = dateObjects[st.dateIdx];
      const h = hours[st.hourIdx] ? hours[st.hourIdx].replace('時', '') : '00';
      
      const minsArr = getMinsArray(modeStep[m]);
      const minStrItem = minsArr[st.minIdx] || minsArr[0];
      const mVal = minStrItem.replace('分', '');
      
      const dayStr = daysOfWeek[d.getDay()];
      const fullStr = d.getFullYear() + '年' + (d.getMonth() + 1) + '月' + d.getDate() + '日 (' + dayStr + ') ' + h + ':' + mVal;

      if (m === 'datetime') {
        document.getElementById('display-datetime-1').textContent = fullStr;
      } else if (m === 'appAdd') {
        document.getElementById('display-datetime-2').textContent = fullStr;
      } else if (m === 'recurring') {
        let recStr = st.dateIdx === 0 ? '毎日 ' + h + ':' + mVal : '毎週' + dayStr + '曜 ' + h + ':' + mVal;
        document.getElementById('display-recurring').textContent = recStr;
      }
    });
  }

  setTimeout(function() { updateDisplay(); switchMode('datetime'); }, 100);

  function setupUnitToggle(btnId, modeName) {
    const btn = document.getElementById(btnId);
    btn.addEventListener('click', function(e) {
      e.stopPropagation();
      if (modeStep[modeName] === 1) {
        modeStep[modeName] = 5;
        btn.textContent = '5分単位';
      } else {
        modeStep[modeName] = 1;
        btn.textContent = '1分単位';
      }
      if (currentMode === modeName) {
        updateMinsListForCurrentMode();
        wMin.setIndex(modeState[modeName].minIdx);
        updateDisplay();
      }
      playTickSound();
    });
  }

  setupUnitToggle('unit-btn-1', 'datetime');
  setupUnitToggle('unit-btn-2', 'appAdd');

  document.getElementById('sec-datetime').addEventListener('click', function() { switchMode('datetime'); });
  document.getElementById('sec-app-add').addEventListener('click', function() { switchMode('appAdd'); });
  document.getElementById('sec-recurring').addEventListener('click', function() { switchMode('recurring'); });

  // Web環境用データ送信・保存処理
  function submitData(targetType) {
    let modeKey = 'datetime';
    if (targetType === 'calendar' || targetType === 'reminder') modeKey = 'appAdd';
    else if (targetType === 'recurring') modeKey = 'recurring';

    const st = modeState[modeKey];
    const selectedDate = dateObjects[st.dateIdx];
    const h = parseInt(hours[st.hourIdx]);
    
    const minsArr = getMinsArray(modeStep[modeKey]);
    const m = parseInt(minsArr[st.minIdx].replace('分', ''));
    
    let target = new Date(selectedDate);
    target.setHours(h, m, 0, 0);

    let comment = "";
    if (targetType === 'datetime') comment = document.getElementById('comment-input-1').value;
    else if (targetType === 'calendar' || targetType === 'reminder') comment = document.getElementById('comment-input-2').value;
    else if (targetType === 'recurring') comment = document.getElementById('comment-input-1').value || document.getElementById('comment-input-2').value;

    let titleStr = comment && comment.trim() !== "" ? comment : "無題の予定";
    let timeStr = document.getElementById(modeKey === 'datetime' ? 'display-datetime-1' : 'display-datetime-2').textContent;
    let recurringStr = document.getElementById('display-recurring').textContent;

    if (targetType === 'datetime') {
      if (target <= new Date()) {
        alert("⚠️ エラー\n未来の時間を指定してください。");
        return;
      }
      let summaryText = timeStr + (comment ? " (" + comment + ")" : "");
      localStorage.setItem("cronoa_sum_datetime", summaryText);
      updateRow('datetime', summaryText);
      alert("✨ 登録されました\n【設定内容】\n" + timeStr + "\n\n【コメント】\n" + (comment || "(なし)"));

    } else if (targetType === 'calendar') {
      let summaryText = timeStr + " " + titleStr;
      localStorage.setItem("cronoa_sum_calendar", summaryText);
      updateRow('calendar', summaryText);
      alert("📅 カレンダーに登録しました\n【日時】\n" + timeStr + "\n\n【件名】\n" + titleStr);

    } else if (targetType === 'reminder') {
      let summaryText = timeStr + " " + titleStr;
      localStorage.setItem("cronoa_sum_reminder", summaryText);
      updateRow('reminder', summaryText);
      alert("📌 リマインダーに登録しました\n【日時】\n" + timeStr + "\n\n【件名】\n" + titleStr);

    } else if (targetType === 'recurring') {
      let now = new Date();
      let firstTarget = new Date();
      firstTarget.setHours(h, m, 0, 0);

      if (st.dateIdx === 0) {
        if (firstTarget <= now) firstTarget.setDate(firstTarget.getDate() + 1);
      } else {
        let targetDay = selectedDate.getDay();
        let currentDay = now.getDay();
        let dayDiff = targetDay - currentDay;
        if (dayDiff < 0 || (dayDiff === 0 && firstTarget <= now)) dayDiff += 7;
        firstTarget.setDate(firstTarget.getDate() + dayDiff);
      }

      let recSummary = recurringStr + (comment ? " (" + comment + ")" : "");
      localStorage.setItem("cronoa_sum_recurring", recSummary);
      updateRow('recurring', recSummary);

      let firstTargetStr = (firstTarget.getMonth() + 1) + "月" + firstTarget.getDate() + "日 " +
                           ("0" + firstTarget.getHours()).slice(-2) + ":" + ("0" + firstTarget.getMinutes()).slice(-2);

      alert("✨ 登録されました\n【設定内容】\n" + recurringStr + "\n初回発火予定:\n" + firstTargetStr + "\n\n【コメント】\n" + (comment || "(なし)"));
    }
  }

  document.getElementById('set-time-btn').addEventListener('click', function(e) { e.stopPropagation(); submitData('datetime'); });
  document.getElementById('add-cal-btn').addEventListener('click', function(e) { e.stopPropagation(); submitData('calendar'); });
  document.getElementById('add-rem-btn').addEventListener('click', function(e) { e.stopPropagation(); submitData('reminder'); });
  document.getElementById('done-btn').addEventListener('click', function(e) { e.stopPropagation(); submitData('recurring'); });
</script>
</body>
</html>
