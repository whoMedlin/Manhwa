<!DOCTYPE html>

<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
<meta name="apple-mobile-web-app-title" content="만화 Trans">
<meta name="theme-color" content="#080810">
<title>만화 Translator</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;400;700;900&family=Bebas+Neue&family=IBM+Plex+Mono:wght@400;700&display=swap" rel="stylesheet">
<style>
*,*::before,*::after{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent}
:root{--ink:#080810;--ink2:#0f0f1a;--ink3:#181828;--ink4:#222235;--line:#2e2e48;--glow:#e63c6e;--glow2:#7b3fe4;--glow3:#3ce6b8;--text:#f0eef8;--text2:#9090b8;--text3:#5a5a80;--safe-top:env(safe-area-inset-top,0px);--safe-bot:env(safe-area-inset-bottom,0px)}
html,body{height:100%;background:var(--ink);color:var(--text);font-family:'IBM Plex Mono',monospace;overflow:hidden;overscroll-behavior:none}
.screen{position:fixed;inset:0;display:flex;flex-direction:column;opacity:0;pointer-events:none;transition:opacity .3s ease}
.screen.active{opacity:1;pointer-events:all}

/* SETUP */
#screen-setup{align-items:center;justify-content:center;padding:40px 28px;padding-top:calc(var(–safe-top) + 40px);background:radial-gradient(ellipse 80% 60% at 50% 0%,rgba(123,63,228,.18) 0%,transparent 70%),radial-gradient(ellipse 60% 40% at 80% 100%,rgba(230,60,110,.12) 0%,transparent 60%),var(–ink);overflow-y:auto}
.logo-wrap{text-align:center;margin-bottom:36px}
.logo-title{font-family:‘Bebas Neue’,sans-serif;font-size:52px;letter-spacing:.06em;line-height:1;background:linear-gradient(135deg,#fff 0%,var(–glow) 60%,var(–glow2) 100%);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;filter:drop-shadow(0 0 24px rgba(230,60,110,.35))}
.logo-sub{font-family:‘Noto Sans KR’,sans-serif;font-size:12px;color:var(–text2);letter-spacing:.15em;margin-top:6px}
.setup-card{width:100%;max-width:420px;background:var(–ink2);border:1px solid var(–line);border-radius:20px;padding:24px 20px;position:relative;overflow:hidden}
.setup-card::before{content:’’;position:absolute;top:0;left:0;right:0;height:2px;background:linear-gradient(90deg,transparent,var(–glow2),var(–glow),transparent)}
.field-label{font-size:10px;letter-spacing:.14em;text-transform:uppercase;color:var(–text3);margin-bottom:8px;display:flex;align-items:center;gap:6px}
.field-label .dot{width:5px;height:5px;border-radius:50%;background:var(–glow)}
.input-row{position:relative;margin-bottom:14px}
input[type=text],input[type=password]{width:100%;background:var(–ink3);border:1px solid var(–line);border-radius:12px;color:var(–text);font-family:‘IBM Plex Mono’,monospace;font-size:13px;padding:13px 44px 13px 14px;outline:none;transition:border-color .2s,box-shadow .2s;-webkit-appearance:none}
input:focus{border-color:var(–glow2);box-shadow:0 0 0 3px rgba(123,63,228,.15)}
.input-eye{position:absolute;right:12px;top:50%;transform:translateY(-50%);background:none;border:none;color:var(–text3);font-size:18px;cursor:pointer;padding:4px}
.lang-grid{display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:14px}
.lang-select-wrap label{display:block;font-size:9px;letter-spacing:.12em;text-transform:uppercase;color:var(–text3);margin-bottom:5px}
select{width:100%;background:var(–ink3);border:1px solid var(–line);border-radius:10px;color:var(–text);font-family:‘Noto Sans KR’,sans-serif;font-size:13px;padding:10px 28px 10px 12px;outline:none;-webkit-appearance:none;appearance:none;cursor:pointer;background-image:url(“data:image/svg+xml,%3Csvg xmlns=‘http://www.w3.org/2000/svg’ width=‘10’ height=‘7’ viewBox=‘0 0 10 7’%3E%3Cpath d=‘M1 1l4 4 4-4’ stroke=’%235a5a80’ stroke-width=‘1.5’ fill=‘none’ stroke-linecap=‘round’/%3E%3C/svg%3E”);background-repeat:no-repeat;background-position:right 10px center}
select:focus{border-color:var(–glow2)}
.btn-start{width:100%;padding:15px;background:linear-gradient(135deg,var(–glow2) 0%,var(–glow) 100%);border:none;border-radius:14px;color:#fff;font-family:‘Bebas Neue’,sans-serif;font-size:20px;letter-spacing:.12em;cursor:pointer;margin-top:6px;transition:transform .15s,box-shadow .2s;box-shadow:0 4px 24px rgba(230,60,110,.3)}
.btn-start:active{transform:scale(.97)}
.hint-text{font-size:11px;color:var(–text3);text-align:center;margin-top:14px;line-height:1.6;font-family:‘Noto Sans KR’,sans-serif}
.hint-text a{color:var(–glow2);text-decoration:none}

/* MAIN */
#screen-main{flex-direction:column;background:var(–ink)}
.topbar{flex-shrink:0;display:flex;align-items:center;gap:10px;padding:0 16px 12px;padding-top:calc(var(–safe-top) + 12px);background:linear-gradient(to bottom,rgba(8,8,16,.98) 80%,transparent);position:relative;z-index:10}
.topbar-logo{font-family:‘Bebas Neue’,sans-serif;font-size:26px;letter-spacing:.08em;background:linear-gradient(135deg,#fff,var(–glow));-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;flex-shrink:0}
.topbar-lang-switcher{display:flex;align-items:center;background:var(–ink3);border:1px solid var(–line);border-radius:20px;padding:3px;gap:2px;flex-shrink:0}
.lang-pill{padding:5px 10px;border-radius:16px;font-size:10px;font-weight:700;letter-spacing:.06em;cursor:pointer;border:none;background:transparent;color:var(–text2);transition:all .2s;white-space:nowrap}
.lang-pill.active{background:linear-gradient(135deg,var(–glow2),var(–glow));color:white}
.topbar-spacer{flex:1}
.topbar-settings{background:var(–ink3);border:1px solid var(–line);border-radius:10px;color:var(–text2);padding:8px 10px;font-size:16px;cursor:pointer;border:none;flex-shrink:0}
.drop-zone{flex:1;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:18px;padding:20px;position:relative;overflow:hidden}
.drop-zone-bg{position:absolute;inset:0;background:radial-gradient(ellipse 50% 40% at 50% 50%,rgba(123,63,228,.08) 0%,transparent 70%);pointer-events:none}
.drop-zone-bg::after{content:’’;position:absolute;inset:0;background-image:linear-gradient(var(–line) 1px,transparent 1px),linear-gradient(90deg,var(–line) 1px,transparent 1px);background-size:40px 40px;opacity:.3;mask-image:radial-gradient(ellipse 70% 70% at 50% 50%,black,transparent);-webkit-mask-image:radial-gradient(ellipse 70% 70% at 50% 50%,black,transparent)}
.drop-icon{font-size:60px;filter:drop-shadow(0 0 20px rgba(123,63,228,.4));animation:float 3s ease-in-out infinite}
@keyframes float{0%,100%{transform:translateY(0)}50%{transform:translateY(-8px)}}
.drop-title{font-family:‘Bebas Neue’,sans-serif;font-size:28px;letter-spacing:.08em;color:var(–text);text-align:center}
.drop-sub{font-family:‘Noto Sans KR’,sans-serif;font-size:13px;color:var(–text2);text-align:center;line-height:1.7;max-width:280px}
.upload-btns{display:flex;gap:10px;flex-wrap:wrap;justify-content:center}
.upload-btn{display:flex;align-items:center;gap:8px;padding:13px 20px;border-radius:14px;border:1px solid var(–line);background:var(–ink2);color:var(–text);font-family:‘IBM Plex Mono’,monospace;font-size:13px;cursor:pointer;transition:all .2s}
.upload-btn:active{transform:scale(.96)}
.upload-btn.primary{background:linear-gradient(135deg,var(–glow2),var(–glow));border-color:transparent;box-shadow:0 4px 20px rgba(230,60,110,.25)}
.upload-btn .icon{font-size:20px}
#fileInput{display:none}
.url-row{display:flex;gap:8px;width:100%;max-width:480px}
.url-input{flex:1;background:var(–ink3);border:1px solid var(–line);border-radius:12px;color:var(–text);font-family:‘IBM Plex Mono’,monospace;font-size:12px;padding:12px 14px;outline:none;-webkit-appearance:none;transition:border-color .2s}
.url-input:focus{border-color:var(–glow2)}
.url-btn{padding:12px 16px;background:var(–glow2);border:none;border-radius:12px;color:white;font-size:18px;cursor:pointer;flex-shrink:0}

/* VIEWER */
#screen-viewer{flex-direction:column;background:var(–ink)}
.viewer-bar{flex-shrink:0;display:flex;align-items:center;gap:8px;padding:10px 14px;padding-top:calc(var(–safe-top) + 10px);background:rgba(8,8,16,.95);border-bottom:1px solid var(–line);position:relative;z-index:20;backdrop-filter:blur(10px);-webkit-backdrop-filter:blur(10px)}
.viewer-back{background:var(–ink3);border:none;border-radius:10px;color:var(–text);padding:8px 12px;font-family:‘IBM Plex Mono’,monospace;font-size:12px;cursor:pointer;white-space:nowrap}
.viewer-filename{flex:1;font-size:11px;color:var(–text2);overflow:hidden;text-overflow:ellipsis;white-space:nowrap}
.viewer-lang-switch{display:flex;background:var(–ink3);border-radius:18px;padding:2px;gap:1px;flex-shrink:0}
.vlang-btn{padding:5px 9px;border-radius:16px;font-size:9px;font-weight:700;letter-spacing:.05em;cursor:pointer;border:none;background:transparent;color:var(–text3);transition:all .2s}
.vlang-btn.active{background:linear-gradient(135deg,var(–glow2),var(–glow));color:white}
.btn-translate-now{padding:9px 14px;background:linear-gradient(135deg,var(–glow2),var(–glow));border:none;border-radius:12px;color:white;font-family:‘Bebas Neue’,sans-serif;font-size:16px;letter-spacing:.08em;cursor:pointer;flex-shrink:0;display:flex;align-items:center;gap:6px;transition:transform .15s;box-shadow:0 2px 12px rgba(230,60,110,.3)}
.btn-translate-now:active{transform:scale(.95)}
.btn-translate-now:disabled{opacity:.5}
.btn-translate-now .spinner{width:14px;height:14px;border:2px solid rgba(255,255,255,.3);border-top-color:white;border-radius:50%;animation:spin .6s linear infinite}
@keyframes spin{to{transform:rotate(360deg)}}
.viewer-body{flex:1;overflow:auto;-webkit-overflow-scrolling:touch;position:relative;display:flex;align-items:flex-start;justify-content:center}
.img-stage{position:relative;display:inline-block;min-width:100%}
.img-stage img{display:block;width:100%;height:auto;user-select:none;-webkit-user-select:none}
.tr-bubble{position:absolute;background:rgba(8,8,16,.92);border-radius:10px;padding:7px 11px;font-family:‘Noto Sans KR’,sans-serif;font-size:clamp(11px,2.2vw,14px);line-height:1.5;color:var(–text);max-width:min(200px,42%);text-align:center;pointer-events:none;animation:bubble-in .25s ease;backdrop-filter:blur(4px);-webkit-backdrop-filter:blur(4px)}
@keyframes bubble-in{from{opacity:0;transform:scale(.88) translateY(4px)}to{opacity:1;transform:scale(1) translateY(0)}}
.tr-bubble[data-type=“speech”]{border:1px solid rgba(123,63,228,.5);box-shadow:0 0 12px rgba(123,63,228,.2)}
.tr-bubble[data-type=“thought”]{border:1px dashed rgba(60,230,184,.45);color:#a0fce0}
.tr-bubble[data-type=“narration”]{border:1px solid rgba(255,255,255,.15);background:rgba(20,12,36,.95);font-style:italic;font-size:clamp(10px,2vw,12px)}
.tr-bubble[data-type=“sfx”]{border:1px solid rgba(230,60,110,.5);color:#ff8ab0;font-weight:700;letter-spacing:.04em}
.tr-dual-line{display:block}
.tr-dual-line.lang2{color:var(–text2);font-size:.88em;border-top:1px solid rgba(255,255,255,.08);margin-top:4px;padding-top:4px}
.status-overlay{position:absolute;bottom:20px;left:50%;transform:translateX(-50%);background:rgba(8,8,16,.9);border:1px solid var(–line);border-radius:24px;padding:10px 20px;font-size:12px;color:var(–text2);font-family:‘Noto Sans KR’,sans-serif;white-space:nowrap;backdrop-filter:blur(8px);-webkit-backdrop-filter:blur(8px);transition:opacity .4s;z-index:30;display:flex;align-items:center;gap:8px}
.status-overlay.hidden{opacity:0;pointer-events:none}

/* SETTINGS */
.settings-panel{position:fixed;inset:0;z-index:100;display:flex;align-items:flex-end;background:rgba(0,0,0,.6);opacity:0;pointer-events:none;transition:opacity .25s;backdrop-filter:blur(4px);-webkit-backdrop-filter:blur(4px)}
.settings-panel.open{opacity:1;pointer-events:all}
.settings-sheet{width:100%;background:var(–ink2);border-radius:24px 24px 0 0;padding:20px 24px;padding-bottom:calc(var(–safe-bot) + 20px);border-top:1px solid var(–line);transform:translateY(100%);transition:transform .3s cubic-bezier(.22,.68,0,1.2)}
.settings-panel.open .settings-sheet{transform:translateY(0)}
.sheet-handle{width:36px;height:4px;background:var(–line);border-radius:2px;margin:0 auto 20px}
.sheet-title{font-family:‘Bebas Neue’,sans-serif;font-size:22px;letter-spacing:.08em;margin-bottom:20px;color:var(–text)}
.setting-row{display:flex;align-items:center;justify-content:space-between;padding:14px 0;border-bottom:1px solid var(–ink4)}
.setting-row:last-child{border-bottom:none}
.setting-info{flex:1}
.setting-name{font-size:14px;color:var(–text);font-family:‘Noto Sans KR’,sans-serif}
.setting-desc{font-size:11px;color:var(–text3);margin-top:2px;font-family:‘Noto Sans KR’,sans-serif}
.toggle{position:relative;width:46px;height:26px;flex-shrink:0}
.toggle input{opacity:0;width:0;height:0}
.toggle-track{position:absolute;inset:0;background:var(–ink4);border-radius:26px;cursor:pointer;transition:background .25s}
.toggle-track::after{content:’’;position:absolute;width:20px;height:20px;top:3px;left:3px;background:white;border-radius:50%;transition:transform .25s;box-shadow:0 1px 4px rgba(0,0,0,.4)}
.toggle input:checked+.toggle-track{background:linear-gradient(135deg,var(–glow2),var(–glow))}
.toggle input:checked+.toggle-track::after{transform:translateX(20px)}
.api-row{margin-top:16px}
.sheet-btn{width:100%;padding:14px;margin-top:10px;background:var(–ink3);border:1px solid var(–line);border-radius:14px;color:var(–text);font-family:‘IBM Plex Mono’,monospace;font-size:13px;cursor:pointer}
.sheet-close-area{position:absolute;inset:0;bottom:auto;height:40%;cursor:pointer}

/* TOAST */
.toast{position:fixed;top:calc(var(–safe-top) + 60px);left:50%;transform:translateX(-50%) translateY(-80px);background:var(–ink3);border:1px solid var(–line);border-radius:20px;padding:10px 20px;font-size:12px;color:var(–text);font-family:‘Noto Sans KR’,sans-serif;white-space:nowrap;z-index:9000;transition:transform .3s cubic-bezier(.22,.68,0,1.2);backdrop-filter:blur(8px);-webkit-backdrop-filter:blur(8px)}
.toast.show{transform:translateX(-50%) translateY(0)}
.toast.error{border-color:var(–glow);color:#ff8ab0}
.toast.success{border-color:var(–glow3);color:#a0fce0}
</style>

</head>
<body>

<!-- SETUP -->

<div class="screen active" id="screen-setup">
  <div class="logo-wrap">
    <div class="logo-title">만화 TRANSLATOR</div>
    <div class="logo-sub">MANHWA · MANGA · MANHUA</div>
  </div>
  <div class="setup-card">
    <div class="field-label"><span class="dot"></span> Claude API Ключ</div>
    <div class="input-row">
      <input type="password" id="setup-key" placeholder="sk-ant-api03-..." autocomplete="off" spellcheck="false">
      <button class="input-eye" id="toggle-eye">👁</button>
    </div>
    <div class="lang-grid">
      <div class="lang-select-wrap">
        <label>Основной язык</label>
        <select id="setup-lang1">
          <option value="Russian">🇷🇺 Русский</option>
          <option value="English">🇺🇸 English</option>
          <option value="Ukrainian">🇺🇦 Українська</option>
          <option value="German">🇩🇪 Deutsch</option>
          <option value="French">🇫🇷 Français</option>
          <option value="Spanish">🇪🇸 Español</option>
        </select>
      </div>
      <div class="lang-select-wrap">
        <label>Второй язык</label>
        <select id="setup-lang2">
          <option value="English">🇺🇸 English</option>
          <option value="Russian">🇷🇺 Русский</option>
          <option value="Ukrainian">🇺🇦 Українська</option>
          <option value="German">🇩🇪 Deutsch</option>
          <option value="French">🇫🇷 Français</option>
          <option value="Spanish">🇪🇸 Español</option>
        </select>
      </div>
    </div>
    <button class="btn-start" id="btn-start">НАЧАТЬ ЧИТАТЬ →</button>
    <p class="hint-text">
      Ключ хранится только на устройстве.<br>
      Получить: <a href="https://console.anthropic.com" target="_blank">console.anthropic.com</a>
    </p>
  </div>
</div>

<!-- MAIN -->

<div class="screen" id="screen-main">
  <div class="topbar">
    <div class="topbar-logo">만화</div>
    <div class="topbar-lang-switcher" id="main-lang-switcher">
      <button class="lang-pill active" data-lang="primary" id="pill-primary">RU</button>
      <button class="lang-pill" data-lang="both" id="pill-both">2×</button>
      <button class="lang-pill" data-lang="secondary" id="pill-secondary">EN</button>
    </div>
    <div class="topbar-spacer"></div>
    <button class="topbar-settings" id="btn-settings">⚙️</button>
  </div>
  <div class="drop-zone" id="drop-zone">
    <div class="drop-zone-bg"></div>
    <div class="drop-icon">🌸</div>
    <div class="drop-title">Загрузи манхву</div>
    <div class="drop-sub">Скриншот панели или страницы —<br>Claude переведёт все пузыри</div>
    <div class="upload-btns">
      <button class="upload-btn primary" id="btn-camera"><span class="icon">📷</span> Фото / Галерея</button>
      <button class="upload-btn" id="btn-file"><span class="icon">🖼️</span> Из файлов</button>
    </div>
    <div class="url-row">
      <input class="url-input" id="url-input" type="url" placeholder="Вставь URL картинки..." inputmode="url" autocomplete="off">
      <button class="url-btn" id="btn-url">→</button>
    </div>
  </div>
  <input type="file" id="fileInput" accept="image/*">
</div>

<!-- VIEWER -->

<div class="screen" id="screen-viewer">
  <div class="viewer-bar">
    <button class="viewer-back" id="btn-back">← Назад</button>
    <div class="viewer-filename" id="viewer-filename">image.jpg</div>
    <div class="viewer-lang-switch" id="viewer-lang-switch">
      <button class="vlang-btn active" data-lang="primary" id="vpill-primary">RU</button>
      <button class="vlang-btn" data-lang="both" id="vpill-both">2×</button>
      <button class="vlang-btn" data-lang="secondary" id="vpill-secondary">EN</button>
    </div>
    <button class="btn-translate-now" id="btn-translate"><span>🔤 ПЕРЕВЕСТИ</span></button>
  </div>
  <div class="viewer-body">
    <div class="img-stage" id="img-stage">
      <img id="viewer-img" src="" alt="">
    </div>
  </div>
  <div class="status-overlay hidden" id="status-overlay">
    <span id="status-text"></span>
  </div>
</div>

<!-- SETTINGS -->

<div class="settings-panel" id="settings-panel">
  <div class="sheet-close-area" id="sheet-close-area"></div>
  <div class="settings-sheet">
    <div class="sheet-handle"></div>
    <div class="sheet-title">Настройки</div>
    <div class="setting-row">
      <div class="setting-info">
        <div class="setting-name">Показывать оригинал</div>
        <div class="setting-desc">Тап на пузырь — корейский текст</div>
      </div>
      <label class="toggle"><input type="checkbox" id="tog-original" checked><span class="toggle-track"></span></label>
    </div>
    <div class="setting-row">
      <div class="setting-info">
        <div class="setting-name">Кэш переводов</div>
        <div class="setting-desc">Не тратить API на одни картинки дважды</div>
      </div>
      <label class="toggle"><input type="checkbox" id="tog-cache" checked><span class="toggle-track"></span></label>
    </div>
    <div class="api-row">
      <div class="field-label" style="margin-bottom:8px"><span class="dot"></span> API Ключ</div>
      <div class="input-row" style="margin-bottom:0">
        <input type="password" id="settings-key" placeholder="sk-ant-api03-..." autocomplete="off">
        <button class="input-eye" id="settings-eye">👁</button>
      </div>
    </div>
    <button class="sheet-btn" id="btn-clear-cache">🗑️ Очистить кэш</button>
    <button class="sheet-btn" id="btn-close-settings" style="color:var(--text2)">Закрыть</button>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
const state={apiKey:'',lang1:'Russian',lang2:'English',displayLang:'primary',showOriginal:true,useCache:true,currentFile:null,currentSrc:null,translations:null};
const LANG_LABELS={Russian:'RU',English:'EN',Ukrainian:'UA',German:'DE',French:'FR',Spanish:'ES',Japanese:'JP',Chinese:'ZH'};
const $=id=>document.getElementById(id);

function saveSettings(){localStorage.setItem('mtr_s',JSON.stringify({apiKey:state.apiKey,lang1:state.lang1,lang2:state.lang2,showOriginal:state.showOriginal,useCache:state.useCache}))}
function loadSettings(){try{const d=JSON.parse(localStorage.getItem('mtr_s')||'{}');if(d.apiKey)state.apiKey=d.apiKey;if(d.lang1)state.lang1=d.lang1;if(d.lang2)state.lang2=d.lang2;state.showOriginal=d.showOriginal!==false;state.useCache=d.useCache!==false}catch(e){}}
function getCache(k){if(!state.useCache)return null;try{return JSON.parse(localStorage.getItem('c_'+k))}catch(e){return null}}
function setCache(k,v){if(!state.useCache)return;try{localStorage.setItem('c_'+k,JSON.stringify(v))}catch(e){}}
function hashSrc(s){let h=0;for(let i=0;i<Math.min(s.length,200);i++)h=Math.imul(31,h)+s.charCodeAt(i)|0;return Math.abs(h).toString(36)+'_'+state.lang1+'_'+state.lang2}
function showScreen(id){document.querySelectorAll('.screen').forEach(s=>s.classList.remove('active'));$(id).classList.add('active')}

let toastTimer;
function toast(msg,type=''){const el=$('toast');el.textContent=msg;el.className='toast show '+type;clearTimeout(toastTimer);toastTimer=setTimeout(()=>el.classList.remove('show'),3000)}

function updateLangPills(){
  const l1=LANG_LABELS[state.lang1]||state.lang1.slice(0,2).toUpperCase();
  const l2=LANG_LABELS[state.lang2]||state.lang2.slice(0,2).toUpperCase();
  ['pill-primary','vpill-primary'].forEach(id=>{const el=$(id);if(el)el.textContent=l1});
  ['pill-secondary','vpill-secondary'].forEach(id=>{const el=$(id);if(el)el.textContent=l2});
  ['','v'].forEach(p=>['primary','both','secondary'].forEach(l=>{const el=$(p+'pill-'+l);if(el)el.classList.toggle('active',l===state.displayLang)}));
}

function setDisplayLang(lang){state.displayLang=lang;updateLangPills();if(state.translations)renderBubbles(state.translations)}

async function callClaude(base64,mediaType){
  const r=await fetch('https://api.anthropic.com/v1/messages',{method:'POST',headers:{'Content-Type':'application/json','x-api-key':state.apiKey,'anthropic-version':'2023-06-01'},body:JSON.stringify({model:'claude-opus-4-5',max_tokens:2000,messages:[{role:'user',content:[{type:'image',source:{type:'base64',media_type:mediaType,data:base64}},{type:'text',text:`You are a professional manhwa/manga translator. Find ALL text in this image: speech bubbles, thought bubbles, sound effects, narration boxes.\n\nTranslate everything to BOTH ${state.lang1} AND ${state.lang2}.\n\nRespond ONLY with a valid JSON array, no markdown:\n[\n  {\n    "original": "원본",\n    "translation": "в ${state.lang1}",\n    "translation2": "in ${state.lang2}",\n    "posX": 50,\n    "posY": 15,\n    "type": "speech"\n  }\n]\nposX/posY = % coordinates (0-100) on the image.\ntype = speech | thought | narration | sfx\nIf no text found return: []\nTranslate naturally, preserve tone.`}]}]})});
  if(!r.ok){const e=await r.json().catch(()=>({}));throw new Error(e.error?.message||'API Error '+r.status)}
  const d=await r.json();
  const raw=d.content?.[0]?.text||'[]';
  return JSON.parse(raw.replace(/```json\n?|\n?```/g,'').trim());
}

function fileToBase64(file){return new Promise((res,rej)=>{const r=new FileReader();r.onload=e=>res({base64:e.target.result.split(',')[1],mediaType:file.type||'image/jpeg'});r.onerror=rej;r.readAsDataURL(file)})}

const POS_MAP={'top-left':{x:5,y:5},'top-center':{x:50,y:5},'top-right':{x:95,y:5},'center-left':{x:5,y:50},'center':{x:50,y:50},'center-right':{x:95,y:50},'bottom-left':{x:5,y:85},'bottom-center':{x:50,y:85},'bottom-right':{x:95,y:85}};

function renderBubbles(translations){
  const stage=$('img-stage');
  stage.querySelectorAll('.tr-bubble').forEach(b=>b.remove());
  if(!translations||!translations.length)return;
  translations.forEach(t=>{
    const b=document.createElement('div');
    b.className='tr-bubble';
    b.dataset.type=t.type||'speech';
    const fb=POS_MAP[t.position]||{x:50,y:50};
    b.style.left=(t.posX??fb.x)+'%';
    b.style.top=(t.posY??fb.y)+'%';
    b.style.transform='translate(-50%,-50%)';
    const t1=t.translation||'?';
    const t2=t.translation2||'';
    if(state.displayLang==='both'&&t2){
      b.innerHTML=`<span class="tr-dual-line lang1">${t1}</span><span class="tr-dual-line lang2">${t2}</span>`;
    }else if(state.displayLang==='secondary'&&t2){
      b.textContent=t2;
    }else{
      b.textContent=t1;
    }
    if(state.showOriginal&&t.original){
      b.style.pointerEvents='all';
      b.addEventListener('click',e=>{e.stopPropagation();toast('원본: '+t.original)});
    }
    stage.appendChild(b);
  });
}

function openImage(src,name){
  state.currentSrc=src;state.translations=null;
  $('viewer-img').src=src;
  $('viewer-filename').textContent=name||'image';
  $('img-stage').querySelectorAll('.tr-bubble').forEach(b=>b.remove());
  showScreen('screen-viewer');
  const ck=hashSrc(src.slice(0,200));
  const cached=getCache(ck);
  if(cached){state.translations=cached;renderBubbles(cached);setStatus('✓ Из кэша — '+cached.length+' пузырей',3000)}
}

async function doTranslate(){
  if(!state.apiKey){toast('Введи API ключ в настройках ⚙️','error');return}
  const btn=$('btn-translate');
  btn.disabled=true;btn.innerHTML='<span class="spinner"></span>';
  setStatus('⏳ Анализируем...',0);
  try{
    let base64,mediaType;
    if(state.currentFile){({base64,mediaType}=await fileToBase64(state.currentFile))}
    else if(state.currentSrc){
      if(state.currentSrc.startsWith('data:')){const p=state.currentSrc.split(',');base64=p[1];mediaType=p[0].match(/:(.*?);/)?.[1]||'image/jpeg'}
      else{const r=await fetch(state.currentSrc);const bl=await r.blob();({base64,mediaType}=await fileToBase64(bl))}
    }
    setStatus('🤖 Claude переводит...',0);
    const translations=await callClaude(base64,mediaType);
    state.translations=translations;
    setCache(hashSrc((state.currentSrc||'').slice(0,200)),translations);
    renderBubbles(translations);
    if(!translations.length){setStatus('🔍 Текст не найден',3000)}
    else{setStatus('✓ '+translations.length+' пузырей переведено',3000);toast('🌸 '+translations.length+' пузырей!','success')}
  }catch(err){
    console.error(err);setStatus('',0);
    toast('Ошибка: '+err.message,'error');
  }finally{btn.disabled=false;btn.innerHTML='<span>🔤 ПЕРЕВЕСТИ</span>'}
}

function setStatus(msg,hideAfter){
  const el=$('status-overlay');$('status-text').textContent=msg;
  el.classList.toggle('hidden',!msg);
  if(hideAfter>0)setTimeout(()=>el.classList.add('hidden'),hideAfter);
}

// Events
$('toggle-eye').addEventListener('click',()=>{const i=$('setup-key');i.type=i.type==='password'?'text':'password'});
$('btn-start').addEventListener('click',()=>{
  const key=$('setup-key').value.trim();
  if(!key){toast('Введи API ключ!','error');return}
  if(!key.startsWith('sk-ant-')){toast('Неверный формат ключа','error');return}
  state.apiKey=key;state.lang1=$('setup-lang1').value;state.lang2=$('setup-lang2').value;
  saveSettings();updateLangPills();showScreen('screen-main');toast('🌸 Готово!','success');
});
$('btn-camera').addEventListener('click',()=>{const i=$('fileInput');i.removeAttribute('capture');i.click()});
$('btn-file').addEventListener('click',()=>{const i=$('fileInput');i.removeAttribute('capture');i.click()});
$('fileInput').addEventListener('change',e=>{const f=e.target.files?.[0];if(!f)return;state.currentFile=f;openImage(URL.createObjectURL(f),f.name);e.target.value=''});
$('btn-url').addEventListener('click',()=>{const u=$('url-input').value.trim();if(!u)return;state.currentFile=null;openImage(u,u.split('/').pop()||'image');$('url-input').value=''});
$('url-input').addEventListener('keydown',e=>{if(e.key==='Enter')$('btn-url').click()});
$('btn-back').addEventListener('click',()=>showScreen('screen-main'));
$('btn-translate').addEventListener('click',doTranslate);
$('main-lang-switcher').addEventListener('click',e=>{const b=e.target.closest('.lang-pill');if(b)setDisplayLang(b.dataset.lang)});
$('viewer-lang-switch').addEventListener('click',e=>{const b=e.target.closest('.vlang-btn');if(b)setDisplayLang(b.dataset.lang)});
$('btn-settings').addEventListener('click',()=>{$('settings-key').value=state.apiKey;$('tog-original').checked=state.showOriginal;$('tog-cache').checked=state.useCache;$('settings-panel').classList.add('open')});
const closeSettings=()=>$('settings-panel').classList.remove('open');
$('sheet-close-area').addEventListener('click',closeSettings);
$('btn-close-settings').addEventListener('click',closeSettings);
$('settings-eye').addEventListener('click',()=>{const i=$('settings-key');i.type=i.type==='password'?'text':'password'});
$('settings-key').addEventListener('change',()=>{const k=$('settings-key').value.trim();if(k){state.apiKey=k;saveSettings();toast('✓ Ключ сохранён','success')}});
$('tog-original').addEventListener('change',e=>{state.showOriginal=e.target.checked;saveSettings()});
$('tog-cache').addEventListener('change',e=>{state.useCache=e.target.checked;saveSettings()});
$('btn-clear-cache').addEventListener('click',()=>{const keys=Object.keys(localStorage).filter(k=>k.startsWith('c_'));keys.forEach(k=>localStorage.removeItem(k));toast('🗑️ Кэш очищен')});

// Init
loadSettings();updateLangPills();
if(state.apiKey){$('setup-key').value=state.apiKey;$('setup-lang1').value=state.lang1;$('setup-lang2').value=state.lang2;showScreen('screen-main')}
</script>

</body>
</html>
