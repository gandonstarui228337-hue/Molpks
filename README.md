<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>AUTO UPGRADE 5.0</title>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Orbitron:wght@500;700;900&display=swap" rel="stylesheet">
<style>
*{margin:0;padding:0;box-sizing:border-box}
:root{--bg:#070a10;--card:rgba(17,24,39,.7);--card2:rgba(26,35,50,.8);--orange:#ff6b00;--cyan:#00d2ff;--green:#10b981;--red:#ef4444;--gold:#f59e0b;--text:#e5e7eb;--muted:#9ca3af;--border:rgba(31,41,55,.6)}
html,body{height:100%}
body{background:var(--bg);color:var(--text);font-family:'Inter',sans-serif;min-height:100vh;overflow-x:hidden;position:relative}
.mono{font-family:'Orbitron',monospace}
#bgCanvas{position:fixed;inset:0;z-index:0;pointer-events:none}
.app-wrap{position:relative;z-index:1}
#authOverlay{position:fixed;inset:0;z-index:9999;background:radial-gradient(ellipse at center,#0f172a 0%,#070a10 70%);display:flex;align-items:center;justify-content:center;padding:20px;overflow:auto}
#authOverlay.hidden{display:none}
.auth-card{max-width:480px;width:100%;background:rgba(17,24,39,.85);backdrop-filter:blur(20px);border:1px solid rgba(0,210,255,.3);border-radius:24px;padding:40px 32px;box-shadow:0 30px 80px rgba(0,0,0,.6),0 0 60px rgba(255,107,0,.15);animation:authIn .6s cubic-bezier(.2,.8,.2,1)}
@keyframes authIn{from{opacity:0;transform:translateY(30px) scale(.95)}to{opacity:1;transform:none}}
.auth-logo{font-family:'Orbitron';font-weight:900;font-size:32px;text-align:center;background:linear-gradient(90deg,var(--orange),var(--cyan));-webkit-background-clip:text;background-clip:text;color:transparent;letter-spacing:1px;margin-bottom:6px}
.auth-sub{text-align:center;color:var(--muted);font-size:13px;margin-bottom:28px}
.auth-tabs{display:flex;gap:6px;margin-bottom:20px;background:var(--card2);padding:4px;border-radius:10px}
.auth-tab{flex:1;padding:10px;border:none;background:transparent;color:var(--muted);cursor:pointer;border-radius:8px;font-weight:600;font-family:'Inter';transition:all .2s}
.auth-tab.active{background:linear-gradient(135deg,var(--orange),#e85900);color:#fff;box-shadow:0 4px 12px rgba(255,107,0,.3)}
.auth-form{display:none}
.auth-form.active{display:block;animation:fadeIn .3s}
.auth-note{font-size:11px;color:var(--muted);text-align:center;margin-top:14px;line-height:1.5}
.gift-box{background:linear-gradient(135deg,rgba(245,158,11,.15),rgba(255,107,0,.05));border:1px solid rgba(245,158,11,.3);border-radius:12px;padding:14px;margin-bottom:20px;text-align:center}
.gift-box .ico{font-size:32px;margin-bottom:6px}
.gift-box .t{font-weight:700;color:var(--gold);font-size:14px}
.gift-box .d{font-size:12px;color:var(--muted);margin-top:4px}
header{background:rgba(15,23,42,.75);backdrop-filter:blur(14px);border-bottom:1px solid var(--border);padding:14px 20px;position:sticky;top:0;z-index:100}
.header-top{display:flex;align-items:center;justify-content:space-between;flex-wrap:wrap;gap:12px}
.logo{font-family:'Orbitron';font-weight:900;font-size:22px;background:linear-gradient(90deg,var(--orange),var(--cyan));-webkit-background-clip:text;background-clip:text;color:transparent;letter-spacing:1px}
.header-actions{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.btn{background:var(--card2);color:var(--text);border:1px solid var(--border);padding:8px 14px;border-radius:8px;cursor:pointer;font-family:'Inter';font-weight:600;font-size:13px;transition:all .2s;display:inline-flex;align-items:center;gap:6px;position:relative;overflow:hidden}
.btn:hover{background:rgba(39,51,71,.9);border-color:var(--cyan);box-shadow:0 0 12px rgba(0,210,255,.3);transform:translateY(-1px)}
.btn-primary{background:linear-gradient(135deg,var(--orange),#e85900);border:none;color:#fff}
.btn-primary:hover{box-shadow:0 0 20px rgba(255,107,0,.6);transform:translateY(-2px)}
.btn-green{background:linear-gradient(135deg,var(--green),#059669);border:none;color:#fff}
.btn-red{background:linear-gradient(135deg,var(--red),#dc2626);border:none;color:#fff}
.btn-cyan{background:linear-gradient(135deg,var(--cyan),#0284c7);border:none;color:#fff}
.btn-gold{background:linear-gradient(135deg,var(--gold),#d97706);border:none;color:#fff}
.stat{background:var(--card2);padding:8px 14px;border-radius:8px;border:1px solid var(--border);font-family:'Orbitron';font-weight:700;font-size:14px;cursor:pointer;transition:all .2s}
.stat.balance{color:var(--gold);border-color:rgba(245,158,11,.4)}
.stat.balance:hover{background:rgba(245,158,11,.1)}
.stat.cars{color:var(--cyan)}
nav.tabs{display:flex;gap:4px;margin-top:12px;overflow-x:auto;scrollbar-width:none}
nav.tabs::-webkit-scrollbar{display:none}
.tab{flex:0 0 auto;padding:10px 16px;background:transparent;border:none;color:var(--muted);cursor:pointer;font-family:'Inter';font-weight:600;font-size:13px;border-radius:8px;white-space:nowrap;transition:all .2s;display:flex;align-items:center;gap:6px;position:relative;overflow:hidden}
.tab:hover{color:var(--text);background:var(--card2)}
.tab.active{background:linear-gradient(135deg,var(--orange),#e85900);color:#fff;box-shadow:0 0 15px rgba(255,107,0,.4)}
.ticker{background:rgba(10,15,28,.8);border-top:1px solid var(--border);padding:8px 0;overflow:hidden;white-space:nowrap;font-family:'Orbitron';font-size:12px}
.ticker-inner{display:inline-block;animation:tick 60s linear infinite;padding-left:100%}
.ticker-live{color:var(--green);font-weight:700;margin-right:12px;text-shadow:0 0 8px var(--green)}
.ticker-item{color:var(--muted);margin-right:28px}
.ticker-item b{color:var(--gold)}
.ticker-item .w{color:var(--green)}
.ticker-item .l{color:var(--red)}
@keyframes tick{0%{transform:translateX(0)}100%{transform:translateX(-100%)}}
main{padding:20px;max-width:1600px;margin:0 auto}
.section{display:none}
.section.active{display:block;animation:sectionIn .45s cubic-bezier(.2,.8,.2,1)}
@keyframes sectionIn{from{opacity:0;transform:translateY(20px) scale(.98);filter:blur(4px)}to{opacity:1;transform:translateY(0) scale(1);filter:blur(0)}}
@keyframes fadeIn{from{opacity:0}to{opacity:1}}
h2{font-family:'Orbitron';font-size:24px;margin-bottom:16px;color:var(--cyan);text-shadow:0 0 12px rgba(0,210,255,.3)}
h3,h4{color:var(--cyan)}
.grid{display:grid;gap:14px;grid-template-columns:repeat(auto-fill,minmax(260px,1fr))}
.card{background:var(--card);backdrop-filter:blur(10px);border:1px solid var(--border);border-radius:12px;padding:14px;transition:all .3s cubic-bezier(.2,.8,.2,1);position:relative;overflow:hidden;transform-style:preserve-3d}
.card::before{content:'';position:absolute;inset:0;border-radius:12px;background:linear-gradient(135deg,rgba(255,107,0,.12),transparent 50%,rgba(0,210,255,.12));opacity:0;transition:opacity .3s;pointer-events:none}
.card:hover{transform:translateY(-4px);box-shadow:0 20px 50px rgba(0,210,255,.25),0 0 30px rgba(255,107,0,.15);border-color:var(--cyan)}
.card:hover::before{opacity:1}
.card-img{width:100%;height:140px;background:linear-gradient(135deg,#1e293b,#0f172a);border-radius:8px;overflow:hidden;position:relative;display:flex;align-items:center;justify-content:center;margin-bottom:10px}
.card-img img{width:100%;height:100%;object-fit:cover}
.card-img svg{width:100%;height:100%}
.card-title{font-weight:700;font-size:14px;margin-bottom:4px}
.card-price{font-family:'Orbitron';font-weight:700;color:var(--gold);font-size:15px}
.badge{position:absolute;top:8px;right:8px;padding:3px 8px;border-radius:6px;font-size:10px;font-weight:700;font-family:'Orbitron'}
.badge-by{background:var(--red);color:#fff}
.badge-stage{background:var(--orange);color:#fff;margin-top:4px}
.badge-cond{background:var(--card2);color:var(--cyan);border:1px solid var(--cyan);margin-top:4px}
.badge-boss{background:linear-gradient(135deg,var(--gold),#b45309);color:#000;font-size:11px;padding:5px 10px}
.upgrade-layout{display:grid;grid-template-columns:300px 1fr 300px;gap:20px;align-items:start}
@media(max-width:1100px){.upgrade-layout{grid-template-columns:1fr}}
.upgrade-wheel{background:var(--card);backdrop-filter:blur(10px);border:1px solid var(--border);border-radius:16px;padding:20px;display:flex;flex-direction:column;align-items:center}
.wheel-svg{width:100%;max-width:380px}
.wheel-info{display:flex;gap:10px;align-items:center;margin-top:14px;font-size:13px}
.wheel-info img,.wheel-info svg{width:60px;height:40px;object-fit:cover;border-radius:6px;background:#1e293b}
.pct{font-family:'Orbitron';font-weight:900;font-size:32px;color:var(--green);text-shadow:0 0 15px rgba(16,185,129,.6)}
.pct.loss{color:var(--red);text-shadow:0 0 15px rgba(239,68,68,.6)}
.target-list{background:var(--card);backdrop-filter:blur(10px);border:1px solid var(--border);border-radius:12px;padding:16px;max-height:600px;overflow-y:auto}
.stake-area{background:var(--card);backdrop-filter:blur(10px);border:1px solid var(--border);border-radius:12px;padding:16px}
.stake-cars{min-height:80px;border:2px dashed var(--border);border-radius:10px;padding:10px;display:flex;gap:6px;flex-wrap:wrap;margin-bottom:12px}
.stake-chip{background:var(--card2);padding:6px 10px;border-radius:8px;font-size:11px;display:flex;align-items:center;gap:6px;border:1px solid var(--cyan)}
.stake-chip .x{color:var(--red);cursor:pointer;font-weight:900;margin-left:4px}
.stake-chip img,.stake-chip svg{width:30px;height:20px;border-radius:4px;background:#1e293b;object-fit:cover}
.switch-row{display:flex;align-items:center;justify-content:space-between;padding:10px 0;border-bottom:1px solid var(--border);font-size:13px}
.switch{position:relative;width:40px;height:22px;background:#374151;border-radius:11px;cursor:pointer;transition:all .2s;flex-shrink:0}
.switch.on{background:var(--green)}
.switch::after{content:'';position:absolute;top:2px;left:2px;width:18px;height:18px;background:#fff;border-radius:50%;transition:all .2s}
.switch.on::after{left:20px}
.quick-btns{display:flex;gap:6px;flex-wrap:wrap;margin:12px 0}
.quick-btns button{flex:1;padding:8px;background:var(--card2);border:1px solid var(--border);color:var(--text);border-radius:6px;cursor:pointer;font-family:'Orbitron';font-weight:700;position:relative;overflow:hidden}
.quick-btns button:hover{background:var(--cyan);color:#000}
.sto-wrap{text-align:center;padding:40px 20px}
.sto-btn{width:240px;height:240px;border-radius:50%;background:radial-gradient(circle at 30% 30%,var(--orange),#991b1b);border:6px solid var(--gold);color:#fff;font-family:'Orbitron';font-weight:900;font-size:22px;cursor:pointer;box-shadow:0 0 50px rgba(255,107,0,.5);transition:all .1s;margin:20px auto;display:flex;flex-direction:column;align-items:center;justify-content:center;gap:8px;position:relative;overflow:hidden}
.sto-btn:active{transform:scale(0.95);box-shadow:0 0 80px rgba(255,107,0,.8)}
.sto-progress{max-width:500px;margin:20px auto;height:24px;background:var(--card);border-radius:12px;overflow:hidden;border:2px solid var(--border)}
.sto-progress-bar{height:100%;background:linear-gradient(90deg,var(--orange),var(--gold));transition:width .3s;position:relative}
.sto-progress-bar::after{content:'';position:absolute;inset:0;background:linear-gradient(90deg,transparent,rgba(255,255,255,.3),transparent);animation:shine 1.5s infinite}
@keyframes shine{0%{transform:translateX(-100%)}100%{transform:translateX(100%)}}
.sto-cars{display:flex;gap:12px;justify-content:center;margin-top:20px;flex-wrap:wrap}
.sto-car{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:16px;min-width:200px;cursor:pointer;transition:all .2s}
.sto-car.affordable{border-color:var(--green);box-shadow:0 0 20px rgba(16,185,129,.3)}
.sto-car.affordable:hover{transform:translateY(-3px)}
.modal{position:fixed;inset:0;background:rgba(0,0,0,.85);backdrop-filter:blur(8px);z-index:1000;display:none;align-items:center;justify-content:center;padding:20px}
.modal.active{display:flex;animation:fadeIn .2s}
.modal-content{background:rgba(17,24,39,.95);backdrop-filter:blur(20px);border:1px solid var(--border);border-radius:16px;padding:24px;max-width:900px;width:100%;max-height:90vh;overflow-y:auto;position:relative;box-shadow:0 30px 80px rgba(0,0,0,.7);animation:modalIn .3s cubic-bezier(.2,.8,.2,1)}
@keyframes modalIn{from{opacity:0;transform:scale(.95)}to{opacity:1;transform:scale(1)}}
.modal-close{position:absolute;top:12px;right:12px;background:var(--red);color:#fff;border:none;width:32px;height:32px;border-radius:50%;cursor:pointer;font-size:16px;font-weight:900}
.input{width:100%;padding:10px 14px;background:rgba(7,10,16,.8);border:1px solid var(--border);border-radius:8px;color:var(--text);font-family:'Inter';font-size:14px;margin-bottom:10px;transition:all .2s}
.input:focus{outline:none;border-color:var(--cyan);box-shadow:0 0 0 3px rgba(0,210,255,.15)}
.label{font-size:12px;color:var(--muted);margin-bottom:4px;display:block}
.db-table{width:100%;border-collapse:collapse;margin-top:10px;font-size:13px}
.db-table th,.db-table td{padding:8px;border:1px solid var(--border);text-align:left}
.db-table th{background:var(--card2);color:var(--cyan);font-family:'Orbitron'}
.history-item{background:var(--card);border:1px solid var(--border);border-radius:10px;padding:12px;margin-bottom:8px;display:flex;justify-content:space-between;align-items:center;gap:10px;font-size:13px}
.history-item.win{border-left:4px solid var(--green)}
.history-item.loss{border-left:4px solid var(--red)}
.history-item small{color:var(--muted)}
.stat-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:14px;margin-bottom:20px}
.stat-card{background:var(--card);border:1px solid var(--border);border-radius:12px;padding:18px;text-align:center;backdrop-filter:blur(10px)}
.stat-card .val{font-family:'Orbitron';font-size:26px;font-weight:900;margin:8px 0}
.stat-card .lbl{color:var(--muted);font-size:12px}
.inspection{display:grid;grid-template-columns:1fr 1fr;gap:20px;align-items:start}
@media(max-width:768px){.inspection{grid-template-columns:1fr}}
.insp-img{width:100%;height:300px;background:linear-gradient(135deg,#1e293b,#0f172a);border-radius:12px;position:relative;overflow:hidden;display:flex;align-items:center;justify-content:center}
.insp-img img,.insp-img svg{width:100%;height:100%;object-fit:cover}
.headlights{position:absolute;inset:0;background:radial-gradient(ellipse at center,rgba(255,255,200,.6),transparent 60%);opacity:0;transition:opacity .3s;pointer-events:none}
.headlights.on{opacity:1}
.insp-controls{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:12px}
.garage-stats{background:var(--card2);padding:14px;border-radius:10px;margin-bottom:14px}
.garage-stat-row{display:flex;justify-content:space-between;padding:4px 0;font-size:13px}
.garage-stat-row b{font-family:'Orbitron';color:var(--gold)}
.plates-section{display:grid;grid-template-columns:repeat(auto-fit,minmax(200px,1fr));gap:12px;margin-top:16px}
.plate{background:linear-gradient(180deg,#fff,#f0f0f0);color:#000;padding:12px;border-radius:8px;text-align:center;font-family:'Orbitron';font-weight:900;font-size:18px;letter-spacing:2px;border:3px solid #000;position:relative;cursor:pointer;transition:all .2s}
.plate:hover{transform:scale(1.05)}
.plate .by{background:#d52b1e;color:#fff;font-size:10px;padding:2px 6px;border-radius:3px;margin-top:4px;display:inline-block}
.plate.active{box-shadow:0 0 20px var(--gold);border-color:var(--gold)}
.notification{position:fixed;top:20px;right:20px;background:rgba(17,24,39,.95);backdrop-filter:blur(14px);border-left:4px solid var(--green);padding:14px 18px;border-radius:8px;box-shadow:0 10px 40px rgba(0,0,0,.5);z-index:2000;animation:slideIn .3s;max-width:320px;font-size:13px}
.notification.error{border-color:var(--red)}
.notification.warn{border-color:var(--gold)}
@keyframes slideIn{from{transform:translateX(400px)}to{transform:translateX(0)}}
.car-option{background:var(--card2);padding:10px;border-radius:8px;margin-bottom:6px;cursor:pointer;border:2px solid transparent;transition:all .2s;display:flex;gap:8px;align-items:center}
.car-option:hover{border-color:var(--cyan)}
.car-option.selected{border-color:var(--green);background:rgba(13,58,46,.6)}
.car-option img,.car-option svg{width:50px;height:35px;border-radius:6px;background:#1e293b;object-fit:cover;flex-shrink:0}
.car-option-info{flex:1;min-width:0}
.car-option-info b{display:block;font-size:13px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.car-option-info small{color:var(--muted);font-size:11px}
.arrow{transition:transform 3.5s cubic-bezier(0.25,0.1,0.25,1)}
.empty{text-align:center;padding:40px;color:var(--muted);font-size:14px}
.filter-bar{display:flex;gap:8px;margin-bottom:16px;flex-wrap:wrap}
.filter-bar select,.filter-bar input{padding:8px 12px;background:var(--card);border:1px solid var(--border);border-radius:8px;color:var(--text);font-family:'Inter'}
.cases-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:16px}
.case-card{background:var(--card);border:1px solid var(--border);border-radius:14px;padding:20px;text-align:center;cursor:pointer;transition:all .2s;position:relative;overflow:hidden;backdrop-filter:blur(10px)}
.case-card:hover{border-color:var(--gold);transform:translateY(-4px);box-shadow:0 15px 40px rgba(245,158,11,.2)}
.case-icon{font-size:64px;margin-bottom:10px;display:block}
.case-name{font-family:'Orbitron';font-weight:700;margin-bottom:6px}
.case-price{font-family:'Orbitron';color:var(--gold);font-weight:900;font-size:18px}
.case-contents{font-size:11px;color:var(--muted);margin-top:8px}
.case-opening{position:fixed;inset:0;background:rgba(0,0,0,.95);z-index:3000;display:none;align-items:center;justify-content:center;flex-direction:column;padding:20px}
.case-opening.active{display:flex}
.case-roulette{width:100%;max-width:800px;height:200px;background:var(--card);border:2px solid var(--gold);border-radius:12px;position:relative;overflow:hidden;margin-bottom:20px}
.case-roulette-track{display:flex;gap:10px;padding:20px;transition:transform 4s cubic-bezier(0.25,0.1,0.25,1)}
.case-roulette-item{min-width:150px;height:160px;background:var(--card2);border-radius:10px;display:flex;flex-direction:column;align-items:center;justify-content:center;padding:10px;flex-shrink:0}
.case-roulette-item img,.case-roulette-item svg{width:100%;height:100px;object-fit:cover;border-radius:6px}
.case-indicator{position:absolute;left:50%;top:0;bottom:0;width:3px;background:var(--red);box-shadow:0 0 20px var(--red);transform:translateX(-50%);z-index:10}
.win-popup{font-family:'Orbitron';font-size:32px;font-weight:900;text-align:center;animation:pulse 1s infinite;color:var(--gold);text-shadow:0 0 30px var(--gold)}
@keyframes pulse{0%,100%{transform:scale(1)}50%{transform:scale(1.1)}}
.ripple-effect{position:absolute;border-radius:50%;background:rgba(255,255,255,.4);transform:scale(0);animation:rippleAnim .6s linear;pointer-events:none}
@keyframes rippleAnim{to{transform:scale(4);opacity:0}}
.confetti-piece{position:fixed;z-index:9999;pointer-events:none;animation:confettiFall linear forwards}
@keyframes confettiFall{0%{transform:translateY(-20px) rotate(0) scale(1);opacity:1}100%{transform:translateY(100vh) rotate(720deg) scale(.6);opacity:0}}
#achievementToast{position:fixed;bottom:20px;left:50%;transform:translateX(-50%) translateY(200px);background:linear-gradient(135deg,#f59e0b,#d97706);color:#000;padding:16px 24px;border-radius:12px;font-family:'Orbitron';font-weight:900;z-index:5000;box-shadow:0 0 40px rgba(245,158,11,.6);transition:transform .5s cubic-bezier(.2,.8,.2,1);display:flex;align-items:center;gap:12px;border:2px solid #fff;pointer-events:none}
#achievementToast.show{transform:translateX(-50%) translateY(0)}
.xp-wrap{display:flex;flex-direction:column;gap:2px;min-width:140px}
.xp-label{font-size:10px;color:var(--muted);font-family:'Orbitron';display:flex;justify-content:space-between}
.xp-bar{height:6px;background:var(--card2);border-radius:3px;overflow:hidden;border:1px solid var(--border)}
.xp-bar-fill{height:100%;background:linear-gradient(90deg,var(--cyan),var(--green));transition:width .5s;box-shadow:0 0 8px var(--cyan)}
.pm-box{background:rgba(7,10,16,.5);border:1px solid var(--border);border-radius:10px;padding:12px;margin-top:8px}
@media(max-width:768px){.auth-card{padding:28px 20px}.auth-logo{font-size:24px}h2{font-size:20px}}
</style>
</head>
<body>
<canvas id="bgCanvas"></canvas>

<div id="authOverlay">
  <div class="auth-card">
    <div class="auth-logo">⚡ AUTO UPGRADE 5.0</div>
    <div class="auth-sub">Симулятор апгрейда автомобилей · Беларусь Edition</div>
    <div class="gift-box"><div class="ico">🎁</div><div class="t">Стартовый подарок</div><div class="d">ЗАЗ-968М «Запорожец» + 100 BYN при создании аккаунта</div></div>
    <div class="auth-tabs">
      <button class="auth-tab active" data-mode="reg">Регистрация</button>
      <button class="auth-tab" data-mode="log">Вход</button>
    </div>
    <div class="auth-form active" id="formReg">
      <label class="label">Придумайте ник</label>
      <input class="input" id="regUser" placeholder="Например: TurboDriver_BY" maxlength="20">
      <label class="label">Пароль</label>
      <input class="input" id="regPass" type="password" placeholder="Минимум 4 символа">
      <label class="label">Повторите пароль</label>
      <input class="input" id="regPass2" type="password" placeholder="Повторите пароль">
      <button class="btn btn-primary" style="width:100%;padding:12px;font-size:15px" onclick="register()">🚀 Создать аккаунт</button>
    </div>
    <div class="auth-form" id="formLog">
      <label class="label">Ваш ник</label>
      <input class="input" id="loginUser" placeholder="Ник">
      <label class="label">Пароль</label>
      <input class="input" id="loginPass" type="password" placeholder="Пароль">
      <button class="btn btn-primary" style="width:100%;padding:12px;font-size:15px" onclick="login()">🔓 Войти</button>
    </div>
    <div class="auth-note">Нажимая «Создать аккаунт», вы соглашаетесь с правилами игры.<br>Данные сохраняются локально в вашем браузере.</div>
  </div>
</div>

<div class="app-wrap">
<header>
  <div class="header-top">
    <div class="logo">⚡ AUTO UPGRADE 5.0</div>
    <div class="header-actions">
      <button class="btn" onclick="openAuth()">🔑 Аккаунт</button>
      <button class="btn btn-red" id="adminBtn" style="display:none" onclick="openAdmin()">🛡️ Админ</button>
      <div class="xp-wrap"><div class="xp-label"><span id="xpLevel">LVL 1</span><span id="xpCur">0/100</span></div><div class="xp-bar"><div class="xp-bar-fill" id="xpFill" style="width:0%"></div></div></div>
      <div class="stat balance mono" onclick="switchTab('sto')">💰 <span id="balance">0</span></div>
      <div class="stat cars mono" onclick="switchTab('garage')">🏠 <span id="carsCount">0</span></div>
      <button class="btn" id="soundBtn" onclick="toggleSound()">🔊</button>
    </div>
  </div>
  <nav class="tabs" id="tabs">
    <button class="tab active" data-tab="upgrade">⚡ Апгрейд</button>
    <button class="tab" data-tab="sto">🔧 СТО Кликер</button>
    <button class="tab" data-tab="cases">📦 Кейсы</button>
    <button class="tab" data-tab="catalog">🚘 Каталог</button>
    <button class="tab" data-tab="garage">🏠 Гараж</button>
    <button class="tab" data-tab="plates">🇧 Номера РБ</button>
    <button class="tab" data-tab="history">🕒 История</button>
    <button class="tab" data-tab="profile">🏆 Профиль</button>
  </nav>
</header>
<div class="ticker"><div class="ticker-inner" id="ticker"></div></div>
<main>
  <section class="section active" id="sec-upgrade">
    <h2>⚡ АПГРЕЙД МАШИНЫ</h2>
    <div class="upgrade-layout">
      <div class="stake-area">
        <h3 style="font-family:'Orbitron';margin-bottom:12px;color:var(--orange)">🎯 Ставка (до 4 авто)</h3>
        <div class="stake-cars" id="stakeCars"><span class="empty" style="padding:20px">Выберите машины из гаража</span></div>
        <button class="btn btn-cyan" style="width:100%" onclick="openStakePicker()">➕ Выбрать авто</button>
        <div style="margin-top:12px;padding:10px;background:var(--card2);border-radius:8px;font-size:13px"><div style="display:flex;justify-content:space-between"><span>Сумма ставки:</span><b class="mono" id="totalStake">0 BYN</b></div></div>
        <div class="switch-row"><span>🛡️ Страховка 25% (-10% шанса)</span><div class="switch" id="insuranceSw" onclick="toggleInsurance()"></div></div>
        <div class="switch-row"><span>⇄ Сектор: Конец/Начало</span><div class="switch" id="invertSw" onclick="toggleInvert()"></div></div>
      </div>
      <div class="upgrade-wheel">
        <svg class="wheel-svg" viewBox="0 0 200 200" id="wheelSvg">
          <defs>
            <radialGradient id="wheelGrad"><stop offset="0%" stop-color="#1e293b"/><stop offset="60%" stop-color="#0f172a"/><stop offset="100%" stop-color="#070a10"/></radialGradient>
            <radialGradient id="glowGrad"><stop offset="0%" stop-color="#ff6b00" stop-opacity=".3"/><stop offset="100%" stop-color="#ff6b00" stop-opacity="0"/></radialGradient>
            <filter id="glow"><feGaussianBlur stdDeviation="2"/></filter>
          </defs>
          <circle cx="100" cy="100" r="95" fill="url(#wheelGrad)" stroke="#1f2937" stroke-width="2"/>
          <circle cx="100" cy="100" r="95" fill="url(#glowGrad)"/>
          <circle cx="100" cy="100" r="85" fill="none" stroke="#1f2937" stroke-width="1"/>
          <path id="winArc" fill="none" stroke="var(--green)" stroke-width="20" opacity="0.7" filter="url(#glow)"/>
          <path id="loseArc" fill="none" stroke="var(--red)" stroke-width="20" opacity="0.4"/>
          <line id="arrow" class="arrow" x1="100" y1="100" x2="100" y2="15" stroke="#fff" stroke-width="4" stroke-linecap="round"/>
          <circle cx="100" cy="100" r="8" fill="#fff" stroke="#000" stroke-width="2"/>
        </svg>
        <div class="pct mono" id="pctDisplay">0.00%</div>
        <div class="wheel-info">
          <div id="stakeImg" style="width:60px;height:40px;background:#1e293b;border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:10px;color:var(--muted)">Ставка</div>
          <span style="font-size:20px">➔</span>
          <div id="targetImg" style="width:60px;height:40px;background:#1e293b;border-radius:6px;display:flex;align-items:center;justify-content:center;font-size:10px;color:var(--muted)">Цель</div>
        </div>
        <div style="margin-top:14px;font-size:12px;color:var(--muted)" id="wheelStatus">Выберите ставку и цель</div>
      </div>
      <div class="target-list">
        <h3 style="font-family:'Orbitron';margin-bottom:12px;color:var(--gold)">🎯 Цель апгрейда</h3>
        <div id="selectedTarget" style="padding:14px;background:var(--card2);border-radius:10px;margin-bottom:12px;text-align:center"><span style="color:var(--muted);font-size:13px">Цель не выбрана</span></div>
        <button class="btn btn-primary" style="width:100%;margin-bottom:10px" onclick="openTargetPicker()">🎯 Выбрать цель</button>
        <div class="quick-btns"><button onclick="quickTarget(2)">x2</button><button onclick="quickTarget(5)">x5</button><button onclick="quickTarget(10)">x10</button><button onclick="quickTarget('auto')">AUTO</button></div>
        <button class="btn btn-green" style="width:100%;margin-top:12px;font-size:16px;padding:14px" onclick="doUpgrade()">🚀 АПГРЕЙДНУТЬ</button>
      </div>
    </div>
  </section>
  <section class="section" id="sec-sto">
    <h2>🔧 СТО КЛИКЕР (+10 BYN)</h2>
    <div class="sto-wrap">
      <p style="color:var(--muted);margin-bottom:20px;font-size:14px">Работайте на СТО, чтобы накопить на первую машину</p>
      <button class="sto-btn" onclick="stoClick()"><div style="font-size:50px">🔧</div><div>РАБОТАТЬ</div><div style="font-size:16px;color:var(--gold)">+10 BYN</div></button>
      <div class="sto-progress"><div class="sto-progress-bar" id="stoBar" style="width:0%"></div></div>
      <p class="mono" id="stoStatus">0 / 1 900 BYN (ЗАЗ-968)</p>
      <div class="sto-cars">
        <div class="sto-car" onclick="buyStoCar(1)"><div style="font-size:40px">🚗</div><div style="font-weight:700;margin:6px 0">ЗАЗ-968М</div><div class="mono" style="color:var(--gold)">1 900 BYN</div></div>
        <div class="sto-car" onclick="buyStoCar(2)"><div style="font-size:40px">🚙</div><div style="font-weight:700;margin:6px 0">ВАЗ-2101</div><div class="mono" style="color:var(--gold)">2 200 BYN</div></div>
      </div>
    </div>
  </section>
  <section class="section" id="sec-cases"><h2>📦 АВТОКОНТЕЙНЕРЫ</h2><div class="cases-grid" id="casesGrid"></div></section>
  <section class="section" id="sec-catalog">
    <h2>🚘 КАТАЛОГ (83 АВТО)</h2>
    <div class="filter-bar">
      <input type="text" id="catSearch" placeholder="🔍 Поиск..." oninput="renderCatalog()">
      <select id="catCategory" onchange="renderCatalog()"><option value="">Все категории</option><option value="budget">Бюджетные (25)</option><option value="mid">Средний (25)</option><option value="premium">Дорогие (30)</option><option value="boss">Эксклюзивы (3)</option></select>
      <select id="catSort" onchange="renderCatalog()"><option value="price-asc">Цена ↑</option><option value="price-desc">Цена ↓</option><option value="name">По имени</option></select>
    </div>
    <div class="grid" id="catalogGrid"></div>
  </section>
  <section class="section" id="sec-garage"><h2>🏠 МОЙ ГАРАЖ</h2><div id="garageStats" class="garage-stats"></div><div class="grid" id="garageGrid"></div></section>
  <section class="section" id="sec-plates">
    <h2>🇧 БЛАТНЫЕ НОМЕРА ГАИ РБ</h2>
    <p style="color:var(--muted);margin-bottom:16px">Выберите машину в гараже и выбейте блатной номер за 3 500 BYN (+18%..+35% к цене)</p>
    <div><label class="label">Выберите машину:</label><select class="input" id="plateCar" onchange="updatePlateUI()"></select></div>
    <h3 style="font-family:'Orbitron';margin-top:20px;margin-bottom:10px">Доступные номера:</h3>
    <div class="plates-section" id="platesSection"></div>
  </section>
  <section class="section" id="sec-history"><h2>🕒 ИСТОРИЯ</h2><div id="historyList"></div></section>
  <section class="section" id="sec-profile"><h2>🏆 ПРОФИЛЬ</h2><div id="profileInfo"></div></section>
</main>
</div>

<div class="modal" id="authModal">
  <div class="modal-content">
    <button class="modal-close" onclick="closeModal('authModal')">×</button>
    <h2>🔑 Аккаунт</h2>
    <p style="margin:10px 0 16px;font-size:14px">Вы вошли как: <b class="mono" style="color:var(--cyan)" id="authWho">—</b></p>
    <div style="display:flex;gap:8px;flex-wrap:wrap"><button class="btn btn-red" onclick="logout()">🚪 Выйти из аккаунта</button></div>
    <p style="margin-top:16px;font-size:12px;color:var(--muted)">Прогресс сохраняется автоматически в этом браузере.</p>
  </div>
</div>

<div class="modal" id="adminModal">
  <div class="modal-content">
    <button class="modal-close" onclick="closeModal('adminModal')">×</button>
    <h2>🛡️ АДМИН-ПАНЕЛЬ</h2>
    <p style="font-size:12px;color:var(--green);margin-bottom:10px">● Режим администратора активен (Largus)</p>
    <h3 style="color:var(--gold);margin:14px 0 10px">💰 Мой баланс</h3>
    <div style="display:flex;gap:8px;flex-wrap:wrap">
      <button class="btn btn-green" onclick="adminAdd(50000)">+50 000 BYN</button>
      <button class="btn btn-green" onclick="adminAdd(1000000)">+1 000 000 BYN</button>
      <button class="btn btn-red" onclick="adminAdd(-state.balance)">Обнулить</button>
    </div>
    <h3 style="color:var(--cyan);margin:14px 0 10px">🚗 Выдать себе авто</h3>
    <select class="input" id="adminCar"></select>
    <button class="btn btn-gold" style="width:100%" onclick="adminGiveCar()">🎁 Выдать (Капсула + Stage 3 + 7777)</button>
    <h3 style="color:var(--orange);margin:14px 0 10px">🔥 Читы</h3>
    <div class="switch-row"><span>100% Победа в апгрейде (God Mode)</span><div class="switch" id="godSw" onclick="toggleGod()"></div></div>
    <h3 style="color:var(--cyan);margin:14px 0 10px">👥 УПРАВЛЕНИЕ ИГРОКАМИ</h3>
    <select class="input" id="pmUser" onchange="pmRender()"></select>
    <div id="pmPanel" class="pm-box"></div>
    <h3 style="color:var(--gold);margin:14px 0 10px">📊 База данных игроков</h3>
    <div id="dbTable"></div>
    <button class="btn btn-gold" style="margin-top:10px" onclick="exportDB()">💾 Скачать БД (.JSON)</button>
  </div>
</div>

<div class="modal" id="pickerModal">
  <div class="modal-content">
    <button class="modal-close" onclick="closeModal('pickerModal')">×</button>
    <h2 id="pickerTitle">Выберите</h2>
    <input class="input" id="pickerSearch" placeholder="🔍 Поиск..." oninput="renderPickerList()">
    <div id="pickerList" style="max-height:500px;overflow-y:auto"></div>
    <button class="btn btn-green" style="width:100%;margin-top:10px" onclick="confirmPicker()">✓ Подтвердить</button>
  </div>
</div>
<div class="modal" id="inspModal">
  <div class="modal-content">
    <button class="modal-close" onclick="closeModal('inspModal')">×</button>
    <h2 id="inspTitle">🔍 Осмотр</h2>
    <div id="inspContent"></div>
  </div>
</div>
<div class="modal" id="tuneModal">
  <div class="modal-content">
    <button class="modal-close" onclick="closeModal('tuneModal')">×</button>
    <h2>🔧 ТЮНИНГ-АТЕЛЬЕ</h2>
    <div id="tuneContent"></div>
  </div>
</div>
<div class="case-opening" id="caseOpening">
  <h2 style="color:var(--gold);font-family:'Orbitron'">🎰 ОТКРЫТИЕ КОНТЕЙНЕРА</h2>
  <div class="case-roulette"><div class="case-indicator"></div><div class="case-roulette-track" id="rouletteTrack"></div></div>
  <div id="caseResult"></div>
</div>
<div id="achievementToast"></div>

<script>
const ADMIN_LOGIN='Largus', ADMIN_PASS='Boss777';
const CARS_DB=[
{id:1,name:'ЗАЗ-968М Запорожец',price:1900,wiki:'ZAZ-968',cat:'budget',engine:'V4_air',cyl:4,year:1979},
{id:2,name:'ВАЗ-2101 Копейка',price:2200,wiki:'VAZ-2101',cat:'budget',engine:'I4_soviet',cyl:4,year:1970},
{id:3,name:'ВАЗ-2102',price:2300,wiki:'VAZ-2102',cat:'budget',engine:'I4_soviet',cyl:4,year:1971},
{id:4,name:'ВАЗ-2103',price:2500,wiki:'VAZ-2103',cat:'budget',engine:'I4_soviet',cyl:4,year:1972},
{id:5,name:'ВАЗ-2106',price:2800,wiki:'VAZ-2106',cat:'budget',engine:'I4_soviet',cyl:4,year:1976},
{id:6,name:'ВАЗ-2107',price:3000,wiki:'VAZ-2107',cat:'budget',engine:'I4_soviet',cyl:4,year:1982},
{id:7,name:'ВАЗ-2108 Самара',price:3200,wiki:'VAZ-2108',cat:'budget',engine:'I4_soviet',cyl:4,year:1984},
{id:8,name:'ВАЗ-2109',price:3400,wiki:'VAZ-2109',cat:'budget',engine:'I4_soviet',cyl:4,year:1987},
{id:9,name:'ВАЗ-2110',price:4000,wiki:'VAZ-2110',cat:'budget',engine:'I4_soviet',cyl:4,year:1995},
{id:10,name:'ГАЗ-24 Волга',price:5500,wiki:'GAZ-24',cat:'budget',engine:'I4_volga',cyl:4,year:1970},
{id:11,name:'ГАЗ-3110',price:4500,wiki:'GAZ-3110',cat:'budget',engine:'I4_volga',cyl:4,year:1997},
{id:12,name:'Москвич-2141',price:3800,wiki:'Moskvich-2141',cat:'budget',engine:'I4_soviet',cyl:4,year:1986},
{id:13,name:'Daewoo Nexia',price:5500,wiki:'Daewoo_Nexia',cat:'budget',engine:'I4',cyl:4,year:1995},
{id:14,name:'Daewoo Lanos',price:5000,wiki:'Daewoo_Lanos',cat:'budget',engine:'I4',cyl:4,year:1997},
{id:15,name:'Opel Astra G',price:6500,wiki:'Opel_Astra#Second_generation_(G)',cat:'budget',engine:'I4',cyl:4,year:1998},
{id:16,name:'VW Passat B3',price:6000,wiki:'Volkswagen_Passat_B3',cat:'budget',engine:'I4',cyl:4,year:1988},
{id:17,name:'VW Golf 3',price:5500,wiki:'Volkswagen_Golf_Mk3',cat:'budget',engine:'I4',cyl:4,year:1991},
{id:18,name:'Audi 80 B4',price:7500,wiki:'Audi_80#B4_(1991–1996)',cat:'budget',engine:'I4',cyl:4,year:1991},
{id:19,name:'BMW E36',price:9000,wiki:'BMW_3_Series_(E36)',cat:'budget',engine:'I6',cyl:6,year:1990},
{id:20,name:'Mercedes W124',price:8500,wiki:'Mercedes-Benz_W124',cat:'budget',engine:'I6',cyl:6,year:1984},
{id:21,name:'Ford Focus II',price:8000,wiki:'Ford_Focus_(second_generation)',cat:'budget',engine:'I4',cyl:4,year:2004},
{id:22,name:'Renault Logan',price:7500,wiki:'Renault_Logan',cat:'budget',engine:'I4',cyl:4,year:2004},
{id:23,name:'Lada Niva',price:9500,wiki:'Lada_Niva',cat:'budget',engine:'I4_soviet',cyl:4,year:1977},
{id:24,name:'Lada Kalina',price:6000,wiki:'Lada_Kalina',cat:'budget',engine:'I4_soviet',cyl:4,year:2004},
{id:25,name:'Lada Priora',price:7500,wiki:'Lada_Priora',cat:'budget',engine:'I4_soviet',cyl:4,year:2007},
{id:26,name:'Lada Granta',price:10000,wiki:'Lada_Granta',cat:'mid',engine:'I4',cyl:4,year:2011},
{id:27,name:'Lada Vesta',price:15000,wiki:'Lada_Vesta',cat:'mid',engine:'I4',cyl:4,year:2015},
{id:28,name:'Lada Largus',price:14000,wiki:'Lada_Largus',cat:'mid',engine:'I4',cyl:4,year:2012},
{id:29,name:'Hyundai Solaris',price:13000,wiki:'Hyundai_Accent#Fourth_generation_(RB;_2010)',cat:'mid',engine:'I4',cyl:4,year:2010},
{id:30,name:'Kia Rio',price:14000,wiki:'Kia_Rio#Third_generation_(QB;_2011)',cat:'mid',engine:'I4',cyl:4,year:2011},
{id:31,name:'VW Polo',price:15000,wiki:'Volkswagen_Polo#Fifth_generation_(6R/6C;_2009)',cat:'mid',engine:'I4',cyl:4,year:2009},
{id:32,name:'Skoda Octavia A7',price:18000,wiki:'Škoda_Octavia#Third_generation_(5E;_2013)',cat:'mid',engine:'I4',cyl:4,year:2013},
{id:33,name:'VW Passat B8',price:28000,wiki:'Volkswagen_Passat_(B8)',cat:'mid',engine:'I4',cyl:4,year:2014},
{id:34,name:'Toyota Corolla',price:20000,wiki:'Toyota_Corolla_(E160)',cat:'mid',engine:'I4',cyl:4,year:2013},
{id:35,name:'Toyota Camry XV70',price:45000,wiki:'Toyota_Camry_(XV70)',cat:'mid',engine:'V6',cyl:6,year:2017},
{id:36,name:'Honda Accord',price:25000,wiki:'Honda_Accord_(ninth_generation)',cat:'mid',engine:'I4',cyl:4,year:2013},
{id:37,name:'Mazda 6',price:24000,wiki:'Mazda6#Third_generation_(GJ;_2012)',cat:'mid',engine:'I4',cyl:4,year:2012},
{id:38,name:'Hyundai Sonata',price:22000,wiki:'Hyundai_Sonata#Seventh_generation_(LF;_2014)',cat:'mid',engine:'I4',cyl:4,year:2014},
{id:39,name:'Kia K5',price:30000,wiki:'Kia_K5',cat:'mid',engine:'I4',cyl:4,year:2019},
{id:40,name:'Renault Duster',price:18000,wiki:'Dacia_Duster#First_generation_(H79;_2010)',cat:'mid',engine:'I4',cyl:4,year:2010},
{id:41,name:'Nissan Qashqai',price:25000,wiki:'Nissan_Qashqai',cat:'mid',engine:'I4',cyl:4,year:2006},
{id:42,name:'Kia Sportage',price:28000,wiki:'Kia_Sportage#Fourth_generation_(QL;_2015)',cat:'mid',engine:'I4',cyl:4,year:2015},
{id:43,name:'Hyundai Tucson',price:26000,wiki:'Hyundai_Tucson#Third_generation_(TL;_2015)',cat:'mid',engine:'I4',cyl:4,year:2015},
{id:44,name:'Toyota RAV4',price:32000,wiki:'Toyota_RAV4_(XA40)',cat:'mid',engine:'I4',cyl:4,year:2012},
{id:45,name:'Mitsubishi Outlander',price:27000,wiki:'Mitsubishi_Outlander#Third_generation_(GF)',cat:'mid',engine:'I4',cyl:4,year:2012},
{id:46,name:'BMW E46',price:15000,wiki:'BMW_3_Series_(E46)',cat:'mid',engine:'I6',cyl:6,year:1998},
{id:47,name:'BMW E39',price:20000,wiki:'BMW_5_Series_(E39)',cat:'mid',engine:'I6',cyl:6,year:1995},
{id:48,name:'BMW E60',price:28000,wiki:'BMW_5_Series_(E60)',cat:'mid',engine:'I6',cyl:6,year:2003},
{id:49,name:'Mercedes W210',price:14000,wiki:'Mercedes-Benz_E-Class_(W210)',cat:'mid',engine:'V6',cyl:6,year:1995},
{id:50,name:'Mercedes W211',price:22000,wiki:'Mercedes-Benz_E-Class_(W211)',cat:'mid',engine:'V6',cyl:6,year:2002},
{id:51,name:'Mercedes W212',price:35000,wiki:'Mercedes-Benz_E-Class_(W212)',cat:'premium',engine:'V6',cyl:6,year:2009},
{id:52,name:'Audi A4 B8',price:25000,wiki:'Audi_A4#B8_(Typ_8K;_2008)',cat:'premium',engine:'I4',cyl:4,year:2008},
{id:53,name:'Audi A6 C7',price:35000,wiki:'Audi_A6#C7_(Typ_4G;_2011)',cat:'premium',engine:'V6',cyl:6,year:2011},
{id:54,name:'BMW G20',price:55000,wiki:'BMW_3_Series_(G20)',cat:'premium',engine:'I4_turbo',cyl:4,year:2018},
{id:55,name:'BMW G30',price:75000,wiki:'BMW_5_Series_(G30)',cat:'premium',engine:'I6_turbo',cyl:6,year:2017},
{id:56,name:'BMW X5 G05',price:95000,wiki:'BMW_X5_(G05)',cat:'premium',engine:'I6_turbo',cyl:6,year:2018},
{id:57,name:'BMW M3 G80',price:120000,wiki:'BMW_M3#G80_(2021–present)',cat:'premium',engine:'I6_twin',cyl:6,year:2021},
{id:58,name:'BMW M5 F90',price:150000,wiki:'BMW_5_Series_(G30)#M5',cat:'premium',engine:'V8_twinturbo',cyl:8,year:2018},
{id:59,name:'Mercedes W213',price:80000,wiki:'Mercedes-Benz_E-Class_(W213)',cat:'premium',engine:'I6_turbo',cyl:6,year:2016},
{id:60,name:'Mercedes W223',price:150000,wiki:'Mercedes-Benz_S-Class_(W223)',cat:'premium',engine:'I6_turbo',cyl:6,year:2020},
{id:61,name:'Mercedes G63 AMG',price:250000,wiki:'Mercedes-Benz_G-Class#Second_generation_(W463;_2018)',cat:'premium',engine:'V8_amg',cyl:8,year:2018},
{id:62,name:'Mercedes AMG C63',price:120000,wiki:'Mercedes-Benz_C-Class_(W205)#C63_AMG',cat:'premium',engine:'V8_amg',cyl:8,year:2015},
{id:63,name:'Mercedes AMG GT',price:220000,wiki:'Mercedes-AMG_GT',cat:'premium',engine:'V8_amg',cyl:8,year:2014},
{id:64,name:'Audi A6 C8',price:80000,wiki:'Audi_A6#C8_(Typ_4K;_2018)',cat:'premium',engine:'V6_turbo',cyl:6,year:2018},
{id:65,name:'Audi Q7 4M',price:90000,wiki:'Audi_Q7#Second_generation_(4M;_2015)',cat:'premium',engine:'V6_turbo',cyl:6,year:2015},
{id:66,name:'Audi RS6 Avant',price:180000,wiki:'Audi_RS_6#C8_(2019–present)',cat:'premium',engine:'V8_twinturbo',cyl:8,year:2019},
{id:67,name:'Porsche Cayenne',price:120000,wiki:'Porsche_Cayenne#Third_generation_(2018)',cat:'premium',engine:'V6_turbo',cyl:6,year:2018},
{id:68,name:'Porsche Panamera',price:140000,wiki:'Porsche_Panamera#Second_generation_(2016)',cat:'premium',engine:'V8_twinturbo',cyl:8,year:2016},
{id:69,name:'Porsche 911 (992)',price:200000,wiki:'Porsche_911_(992)',cat:'premium',engine:'Boxer6',cyl:6,year:2019},
{id:70,name:'Porsche 911 Turbo S',price:350000,wiki:'Porsche_911_(992)#911_Turbo_and_Turbo_S',cat:'premium',engine:'Boxer6_twin',cyl:6,year:2020},
{id:71,name:'Tesla Model 3',price:60000,wiki:'Tesla_Model_3',cat:'premium',engine:'Tesla',cyl:0,year:2017},
{id:72,name:'Tesla Model Y',price:75000,wiki:'Tesla_Model_Y',cat:'premium',engine:'Tesla',cyl:0,year:2020},
{id:73,name:'Tesla Model S Plaid',price:180000,wiki:'Tesla_Model_S#Plaid_(2021–present)',cat:'premium',engine:'Tesla_Plaid',cyl:0,year:2021},
{id:74,name:'Toyota Land Cruiser 300',price:140000,wiki:'Toyota_Land_Cruiser_(J300)',cat:'premium',engine:'V6_twin',cyl:6,year:2021},
{id:75,name:'Lexus LX',price:180000,wiki:'Lexus_LX#Fourth_generation_(J310;_2022)',cat:'premium',engine:'V6_twin',cyl:6,year:2022},
{id:76,name:'Range Rover L460',price:200000,wiki:'Range_Rover_(L460)',cat:'premium',engine:'V8_twin',cyl:8,year:2022},
{id:77,name:'VW Touareg',price:90000,wiki:'Volkswagen_Touareg#Third_generation_(CR;_2018)',cat:'premium',engine:'V6_tdi',cyl:6,year:2018},
{id:78,name:'Lamborghini Huracan',price:450000,wiki:'Lamborghini_Huracán',cat:'premium',engine:'V10',cyl:10,year:2014},
{id:79,name:'Ferrari F8 Tributo',price:500000,wiki:'Ferrari_F8',cat:'premium',engine:'V8_twin',cyl:8,year:2019},
{id:80,name:'McLaren 720S',price:480000,wiki:'McLaren_720S',cat:'premium',engine:'V8_twin',cyl:8,year:2017},
{id:81,name:'Rolls-Royce Cullinan Black Badge',price:2900000,wiki:'Rolls-Royce_Cullinan',cat:'boss',engine:'V12',cyl:12,year:2018,boss:true},
{id:82,name:'Bugatti Chiron Super Sport',price:4800000,wiki:'Bugatti_Chiron',cat:'boss',engine:'W16',cyl:16,year:2016,boss:true},
{id:83,name:'LADA Largus VIP [БОСС]',price:10000000,wiki:'Lada_Largus',cat:'boss',engine:'Boss',cyl:4,year:2024,boss:true}];
const CONDITIONS=[{id:1,name:'После ДТП (Битая)',mult:0.55,emoji:'💥'},{id:2,name:'Перекуп-эдишн',mult:0.75,emoji:'🔄'},{id:3,name:'На бодром ходу',mult:1.00,emoji:'✅'},{id:4,name:'После деда (Идеал)',mult:1.25,emoji:'👴'},{id:5,name:'Капсула времени',mult:1.50,emoji:'⏳'}];
const PLATES=[{id:'7777 AA-7',mult:0.35},{id:'0001 MI-7',mult:0.32},{id:'8888 OO-7',mult:0.25},{id:'0770 BP-7',mult:0.18}];
const CASES=[{id:1,name:'Бомж-кейс',icon:'📦',price:5000,min:1,max:10},{id:2,name:'Средний контейнер',icon:'🎁',price:25000,min:10,max:50},{id:3,name:'Премиум кейс',icon:'💎',price:100000,min:50,max:80},{id:4,name:'VIP Контейнер',icon:'👑',price:500000,min:60,max:83},{id:5,name:'БОСС-контейнер',icon:'🔥',price:3000000,min:81,max:83}];
const ACHIEVEMENTS=[{id:'first_win',name:'Первая победа',icon:'🏆',check:s=>s.history.some(h=>h.win)},{id:'millionaire',name:'Миллионер',icon:'💎',check:s=>s.balance>=1000000},{id:'collector10',name:'Коллекционер 10',icon:'🚗',check:s=>s.garage.length>=10},{id:'collector25',name:'Гараж мечты',icon:'🏁',check:s=>s.garage.length>=25},{id:'boss_hunter',name:'Охотник на боссов',icon:'🔥',check:s=>s.garage.some(c=>c.boss)},{id:'tuner',name:'Мастер тюнинга',icon:'🔧',check:s=>s.garage.some(c=>c.stage>=3)},{id:'lucky',name:'Везунчик',icon:'🍀',check:s=>s.history.filter(h=>h.win).length>=10},{id:'highroller',name:'Игрок по-крупному',icon:'💰',check:s=>s.history.some(h=>h.stake>=1000000)}];

function freshState(user){return{balance:0,garage:[],history:[],stakeCars:[],target:null,currentUser:user,soundOn:true,insurance:false,invertSector:false,godMode:false,xp:0,level:1,achievements:[],isAdmin:false,chanceMod:1};}
let state=freshState(null);
const SAVE_KEY='auto_upgrade_5_save_';
const DB_KEY='users_db';

let audioCtx;
function getCtx(){if(!audioCtx)audioCtx=new (window.AudioContext||window.webkitAudioContext)();return audioCtx;}
function playUI(s){if(!state.soundOn)return;try{const ctx=getCtx();const P={hover:{f:1200,d:.03,v:.05,t:'sine'},click:{f:800,d:.05,v:.1,t:'square'},tab:{f:600,d:.08,v:.08,t:'triangle'},modal:{f:400,d:.15,v:.1,t:'sine'}};const p=P[s]||P.click;const o=ctx.createOscillator(),g=ctx.createGain();o.type=p.t;o.frequency.value=p.f;g.gain.setValueAtTime(p.v,ctx.currentTime);g.gain.exponentialRampToValueAtTime(.001,ctx.currentTime+p.d);o.connect(g);g.connect(ctx.destination);o.start();o.stop(ctx.currentTime+p.d);}catch(e){}}
function playClick(){if(!state.soundOn)return;try{const ctx=getCtx();const o=ctx.createOscillator(),g=ctx.createGain();o.type='square';o.frequency.value=800;g.gain.setValueAtTime(.1,ctx.currentTime);g.gain.exponentialRampToValueAtTime(.001,ctx.currentTime+.05);o.connect(g);g.connect(ctx.destination);o.start();o.stop(ctx.currentTime+.05);}catch(e){}}
function playWin(){if(!state.soundOn)return;try{const ctx=getCtx();[523.25,659.25,783.99].forEach((f,i)=>{const o=ctx.createOscillator(),g=ctx.createGain();o.type='triangle';o.frequency.value=f;g.gain.setValueAtTime(.2,ctx.currentTime+i*.1);g.gain.exponentialRampToValueAtTime(.001,ctx.currentTime+i*.1+.3);o.connect(g);g.connect(ctx.destination);o.start(ctx.currentTime+i*.1);o.stop(ctx.currentTime+i*.1+.3);});}catch(e){}}
function playLoss(){if(!state.soundOn)return;try{const ctx=getCtx();const o=ctx.createOscillator(),g=ctx.createGain();o.type='sawtooth';o.frequency.setValueAtTime(300,ctx.currentTime);o.frequency.exponentialRampToValueAtTime(80,ctx.currentTime+.5);g.gain.setValueAtTime(.2,ctx.currentTime);g.gain.exponentialRampToValueAtTime(.001,ctx.currentTime+.5);o.connect(g);g.connect(ctx.destination);o.start();o.stop(ctx.currentTime+.5);}catch(e){}}
function playAlarm(){if(!state.soundOn)return;try{const ctx=getCtx();for(let i=0;i<4;i++){const o=ctx.createOscillator(),g=ctx.createGain();o.type='square';o.frequency.value=i%2===0?880:660;g.gain.setValueAtTime(.15,ctx.currentTime+i*.3);g.gain.exponentialRampToValueAtTime(.001,ctx.currentTime+i*.3+.25);o.connect(g);g.connect(ctx.destination);o.start(ctx.currentTime+i*.3);o.stop(ctx.currentTime+i*.3+.25);}}catch(e){}}
function playEngine(type){if(!state.soundOn)return;try{
const ctx=getCtx();const master=ctx.createGain();master.gain.setValueAtTime(.4,ctx.currentTime);master.gain.exponentialRampToValueAtTime(.001,ctx.currentTime+4);master.connect(ctx.destination);
const profiles={V4_air:{freq:180,type:'square',h:[2,3,4],sweep:1.3,dur:3},I4_soviet:{freq:150,type:'sawtooth',h:[2,3],sweep:1.4,dur:3.5},I4_volga:{freq:140,type:'sawtooth',h:[2,3,5],sweep:1.3,dur:3},I4:{freq:200,type:'sawtooth',h:[2,3],sweep:1.5,dur:2.5},I4_turbo:{freq:220,type:'sawtooth',h:[2,3,5],sweep:1.6,dur:2.5,turbo:1},I6:{freq:160,type:'sawtooth',h:[2,3,4,5],sweep:1.5,dur:3},I6_turbo:{freq:180,type:'sawtooth',h:[2,3,5,6],sweep:1.7,dur:2.5,turbo:1},I6_twin:{freq:220,type:'sawtooth',h:[2,3,4,5,6],sweep:2,dur:2.5,turbo:1},V6:{freq:130,type:'triangle',h:[2,3,5],sweep:1.5,dur:3},V6_turbo:{freq:150,type:'triangle',h:[2,3,5],sweep:1.7,dur:2.8,turbo:1},V6_twin:{freq:180,type:'triangle',h:[2,3,5,7],sweep:1.8,dur:2.8,turbo:1},V6_tdi:{freq:90,type:'square',h:[2,3,5],sweep:1.4,dur:3.5,turbo:1,diesel:1},V8_twinturbo:{freq:90,type:'sawtooth',h:[2,3,4,5,7],sweep:1.8,dur:3,popcorn:1,turbo:1},V8_amg:{freq:80,type:'sawtooth',h:[2,3,5,7],sweep:1.9,dur:3.5,popcorn:1,turbo:1},V8_twin:{freq:100,type:'sawtooth',h:[2,3,5,7],sweep:1.8,dur:2.8,popcorn:1,turbo:1},Boxer6:{freq:120,type:'sawtooth',h:[2,3,5,7],sweep:2,dur:2.5},Boxer6_twin:{freq:160,type:'sawtooth',h:[2,3,5,7,9],sweep:2.2,dur:2.8,turbo:1},V10:{freq:110,type:'sawtooth',h:[2,3,5,7,9,11],sweep:2.5,dur:3},V12:{freq:100,type:'sawtooth',h:[2,3,5,7,9,11,13],sweep:2.2,dur:3.5},W16:{freq:120,type:'sawtooth',h:[2,3,5,7,9,11,13,15],sweep:2.5,dur:3,turbo:1,popcorn:1},Tesla:{freq:2000,type:'sine',h:[3,5,7],sweep:.8,dur:2,inv:1},Tesla_Plaid:{freq:2500,type:'sine',h:[3,5,7,9],sweep:.7,dur:2.5,inv:1},Boss:{freq:140,type:'sawtooth',h:[2,3,5,7,11],sweep:2.2,dur:3.5,turbo:1,blow:1}};
const p=profiles[type]||profiles.I4;
p.h.forEach((h,i)=>{const o=ctx.createOscillator(),g=ctx.createGain();o.type=p.type;o.frequency.setValueAtTime(p.freq*h,ctx.currentTime);o.frequency.exponentialRampToValueAtTime(p.freq*h*p.sweep,ctx.currentTime+.3);o.frequency.exponentialRampToValueAtTime(p.freq*h*p.sweep*.85,ctx.currentTime+p.dur);g.gain.setValueAtTime(.15/(i+1),ctx.currentTime);g.gain.linearRampToValueAtTime(.2/(i+1),ctx.currentTime+.5);g.gain.linearRampToValueAtTime(0,ctx.currentTime+p.dur);o.connect(g);g.connect(master);o.start();o.stop(ctx.currentTime+p.dur);});
if(p.turbo){const n=ctx.createBufferSource();const buf=ctx.createBuffer(1,ctx.sampleRate*p.dur,ctx.sampleRate);const d=buf.getChannelData(0);for(let i=0;i<d.length;i++)d[i]=(Math.random()*2-1)*.05;n.buffer=buf;const f=ctx.createBiquadFilter();f.type='bandpass';f.frequency.setValueAtTime(3000,ctx.currentTime);f.frequency.exponentialRampToValueAtTime(8000,ctx.currentTime+p.dur*.8);const ng=ctx.createGain();ng.gain.setValueAtTime(0,ctx.currentTime+.5);ng.gain.linearRampToValueAtTime(.15,ctx.currentTime+1);ng.gain.linearRampToValueAtTime(0,ctx.currentTime+p.dur);n.connect(f);f.connect(ng);ng.connect(master);n.start(ctx.currentTime+.5);n.stop(ctx.currentTime+p.dur);}
if(p.popcorn){for(let i=0;i<8;i++){const t=ctx.currentTime+1+Math.random()*1.5;const o=ctx.createOscillator(),g=ctx.createGain();o.type='sawtooth';o.frequency.value=60+Math.random()*40;g.gain.setValueAtTime(0,t);g.gain.linearRampToValueAtTime(.3,t+.01);g.gain.exponentialRampToValueAtTime(.001,t+.15);o.connect(g);g.connect(master);o.start(t);o.stop(t+.15);}}
if(p.inv){const o=ctx.createOscillator(),g=ctx.createGain();o.type='sawtooth';o.frequency.setValueAtTime(8000,ctx.currentTime);o.frequency.exponentialRampToValueAtTime(12000,ctx.currentTime+p.dur);g.gain.setValueAtTime(0,ctx.currentTime);g.gain.linearRampToValueAtTime(.08,ctx.currentTime+.5);g.gain.linearRampToValueAtTime(0,ctx.currentTime+p.dur);o.connect(g);g.connect(master);o.start();o.stop(ctx.currentTime+p.dur);}
if(p.blow){const t=ctx.currentTime+2;const o=ctx.createOscillator(),g=ctx.createGain();o.type='sawtooth';o.frequency.setValueAtTime(5000,t);o.frequency.exponentialRampToValueAtTime(500,t+.5);g.gain.setValueAtTime(0,t);g.gain.linearRampToValueAtTime(.4,t+.05);g.gain.exponentialRampToValueAtTime(.001,t+.5);o.connect(g);g.connect(master);o.start(t);o.stop(t+.5);}
}catch(e){}}

(function(){const cv=document.getElementById('bgCanvas');const ctx=cv.getContext('2d');let W,H,ps=[];function rz(){W=cv.width=innerWidth;H=cv.height=innerHeight}rz();addEventListener('resize',rz);const N=innerWidth<768?40:70;for(let i=0;i<N;i++)ps.push({x:Math.random()*W,y:Math.random()*H,vx:(Math.random()-.5)*.4,vy:(Math.random()-.5)*.4,r:Math.random()*1.5+.5,c:Math.random()>.5?'#ff6b00':'#00d2ff'});function draw(){ctx.fillStyle='rgba(7,10,16,0.18)';ctx.fillRect(0,0,W,H);ps.forEach(p=>{p.x+=p.vx;p.y+=p.vy;if(p.x<0||p.x>W)p.vx*=-1;if(p.y<0||p.y>H)p.vy*=-1;ctx.beginPath();ctx.arc(p.x,p.y,p.r,0,7);ctx.fillStyle=p.c;ctx.shadowBlur=10;ctx.shadowColor=p.c;ctx.fill();});for(let i=0;i<ps.length;i++)for(let j=i+1;j<ps.length;j++){const dx=ps[i].x-ps[j].x,dy=ps[i].y-ps[j].y;const d=Math.sqrt(dx*dx+dy*dy);if(d<120){ctx.strokeStyle=`rgba(0,210,255,${.15*(1-d/120)})`;ctx.lineWidth=.5;ctx.shadowBlur=0;ctx.beginPath();ctx.moveTo(ps[i].x,ps[i].y);ctx.lineTo(ps[j].x,ps[j].y);ctx.stroke();}}requestAnimationFrame(draw);}draw();})();

document.addEventListener('click',e=>{const b=e.target.closest('.btn,.tab,.sto-btn,.case-card,.quick-btns button');if(!b)return;const r=document.createElement('span');r.className='ripple-effect';const d=Math.max(b.clientWidth,b.clientHeight);r.style.width=r.style.height=d+'px';r.style.left=(e.clientX-b.getBoundingClientRect().left-d/2)+'px';r.style.top=(e.clientY-b.getBoundingClientRect().top-d/2)+'px';b.appendChild(r);setTimeout(()=>r.remove(),600);});
function spawnConfetti(){const colors=['#ff6b00','#00d2ff','#10b981','#f59e0b','#ef4444','#fff'];for(let i=0;i<80;i++){const c=document.createElement('div');c.className='confetti-piece';c.style.left=Math.random()*100+'vw';c.style.top='-20px';c.style.background=colors[i%colors.length];c.style.width=(6+Math.random()*8)+'px';c.style.height=(6+Math.random()*8)+'px';c.style.borderRadius=Math.random()>.5?'50%':'2px';c.style.animationDuration=(2.5+Math.random()*2)+'s';document.body.appendChild(c);setTimeout(()=>c.remove(),4500);}}

const imgCache={};
function getCarSVG(car,w=300,h=200,showName=true){const grad=car.boss?'#f59e0b,#b45309':(car.cat==='premium'?'#ef4444,#7f1d1d':(car.cat==='mid'?'#00d2ff,#0284c7':'#10b981,#065f46'));const[c1,c2]=grad.split(',');return `<svg viewBox="0 0 ${w} ${h}" xmlns="http://www.w3.org/2000/svg"><defs><linearGradient id="g${car.id}_${w}" x1="0" y1="0" x2="1" y2="1"><stop offset="0%" stop-color="${c1}"/><stop offset="100%" stop-color="${c2}"/></linearGradient></defs><rect width="${w}" height="${h}" fill="url(#g${car.id}_${w})" opacity="0.3"/><rect width="${w}" height="${h}" fill="#0f172a" opacity="0.5"/><g transform="translate(${w/2},${h/2-10})"><path d="M-60,-20 L-50,-30 L-20,-30 L-10,-20 L30,-20 L40,-10 L50,-10 L55,0 L-65,0 Z" fill="${c1}" opacity="0.9"/><circle cx="-40" cy="5" r="10" fill="#000" stroke="#444" stroke-width="2"/><circle cx="40" cy="5" r="10" fill="#000" stroke="#444" stroke-width="2"/><rect x="-15" y="-25" width="18" height="8" fill="#7dd3fc" opacity="0.7"/></g>${showName?`<text x="${w/2}" y="${h-30}" text-anchor="middle" fill="#fff" font-family="Orbitron,sans-serif" font-size="14" font-weight="700">${car.name}</text><text x="${w/2}" y="${h-12}" text-anchor="middle" fill="#f59e0b" font-family="Orbitron,sans-serif" font-size="13" font-weight="700">${car.price.toLocaleString('ru')} BYN</text>`:''}</svg>`;}
async function getCarImg(car){if(imgCache[car.id])return imgCache[car.id];try{const r=await fetch(`https://en.wikipedia.org/api/rest_v1/page/summary/${encodeURIComponent(car.wiki)}`);if(r.ok){const d=await r.json();if(d.thumbnail&&d.thumbnail.source){imgCache[car.id]=d.thumbnail.source;localStorage.setItem('img_'+car.id,d.thumbnail.source);return d.thumbnail.source;}}}catch(e){}const c=localStorage.getItem('img_'+car.id);if(c){imgCache[car.id]=c;return c;}return null;}
async function loadImgInto(elId,car){const url=await getCarImg(car);const el=document.getElementById(elId);if(el&&url){const svg=getCarSVG(car,300,140,true).replace(/`/g,'\\`');el.innerHTML=`<img src="${url}" alt="${car.name}" onerror="this.parentNode.innerHTML=\`${svg}\`">`;}}

function save(){if(!state.currentUser)return;localStorage.setItem(SAVE_KEY+state.currentUser,JSON.stringify(state));saveUsersDB();}
function loadSave(user){const raw=localStorage.getItem(SAVE_KEY+user);if(!raw)return false;try{state={...freshState(user),...JSON.parse(raw)};state.currentUser=user;return true;}catch(e){return false;}}
function getUsersDB(){try{return JSON.parse(localStorage.getItem(DB_KEY)||'{}')}catch(e){return{}}}
function saveUsersDB(){if(!state.currentUser||state.currentUser===ADMIN_LOGIN)return;const db=getUsersDB();db[state.currentUser]={pass:(db[state.currentUser]||{}).pass,balance:state.balance,cars:state.garage.length,capital:state.balance+state.garage.reduce((s,c)=>s+calcCarPrice(c),0),xp:state.xp||0,level:state.level||1,achievements:state.achievements||[],chanceMod:state.chanceMod||1};localStorage.setItem(DB_KEY,JSON.stringify(db));}

// ===== Сейвы игроков (для админа) =====
function getPlayerSave(u){try{return JSON.parse(localStorage.getItem(SAVE_KEY+u))||null}catch(e){return null}}
function setPlayerSave(u,d){localStorage.setItem(SAVE_KEY+u,JSON.stringify(d));const db=getUsersDB();if(db[u]){db[u].balance=d.balance;db[u].cars=(d.garage||[]).length;db[u].capital=(d.balance||0)+(d.garage||[]).reduce((s,c)=>s+calcCarPrice(c),0);db[u].chanceMod=d.chanceMod||1;db[u].level=d.level||1;localStorage.setItem(DB_KEY,JSON.stringify(db));}}

function renderHeader(){document.getElementById('balance').textContent=state.balance.toLocaleString('ru');document.getElementById('carsCount').textContent=state.garage.length;document.getElementById('adminBtn').style.display=state.isAdmin?'inline-flex':'none';updateXP();}
function updateXP(){const lvl=state.level||1;const need=lvl*100;const cur=(state.xp||0)%need;document.getElementById('xpLevel').textContent='LVL '+lvl;document.getElementById('xpCur').textContent=cur+'/'+need;document.getElementById('xpFill').style.width=(cur/need*100)+'%';}
function addXP(a){state.xp=(state.xp||0)+a;let need=(state.level||1)*100;while(state.xp>=need){state.xp-=need;state.level=(state.level||1)+1;notify(`🎉 Уровень ${state.level}!`);spawnConfetti();need=state.level*100;}updateXP();}
function renderTicker(){const items=[];const bots=['Alex_91','TurboKing','BY_Driver','SpeedMax','Garazh_777','MinskRacer'];state.history.slice(-5).forEach(h=>{items.push(`<span class="ticker-item"><b>${state.currentUser}</b> ${h.win?'<span class="w">✓ выиграл</span>':'<span class="l">✗ проиграл</span>'} <b>${h.targetName}</b> (${h.pct.toFixed(1)}%)</span>`);});for(let i=0;i<8;i++){const c=CARS_DB[Math.floor(Math.random()*CARS_DB.length)];const win=Math.random()>.3;const b=bots[Math.floor(Math.random()*bots.length)];items.push(`<span class="ticker-item"><b>${b}</b> ${win?'<span class="w">✓ выиграл</span>':'<span class="l">✗ проиграл</span>'} <b>${c.name}</b></span>`);}document.getElementById('ticker').innerHTML=`<span class="ticker-live">● LIVE</span>`+items.join('');}
function renderCatalog(){const q=(document.getElementById('catSearch').value||'').toLowerCase();const cat=document.getElementById('catCategory').value;const sort=document.getElementById('catSort').value;let list=CARS_DB.filter(c=>{if(q&&!c.name.toLowerCase().includes(q))return false;if(cat&&c.cat!==cat)return false;return true;});if(sort==='price-asc')list.sort((a,b)=>a.price-b.price);else if(sort==='price-desc')list.sort((a,b)=>b.price-a.price);else list.sort((a,b)=>a.name.localeCompare(b.name));document.getElementById('catalogGrid').innerHTML=list.map(c=>`<div class="card"><div class="card-img" id="catimg-${c.id}">${getCarSVG(c)}</div><div class="card-title">${c.name}</div><div style="font-size:11px;color:var(--muted);margin-bottom:6px">${c.year} · ${c.engine}</div><div class="card-price mono">${c.price.toLocaleString('ru')} BYN</div><button class="btn btn-primary" style="width:100%;margin-top:8px" onclick="buyFromCatalog(${c.id})">Купить</button></div>`).join('');list.forEach(c=>loadImgInto('catimg-'+c.id,c));}
function renderGarage(){const total=state.garage.reduce((s,c)=>s+calcCarPrice(c),0);document.getElementById('garageStats').innerHTML=`<div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:14px"><div><span style="color:var(--muted)">Машин:</span> <b class="mono" style="color:var(--cyan);font-size:20px">${state.garage.length}</b></div><div><span style="color:var(--muted)">Стоимость:</span> <b class="mono" style="color:var(--gold);font-size:20px">${total.toLocaleString('ru')} BYN</b></div></div>`;const grid=document.getElementById('garageGrid');if(!state.garage.length){grid.innerHTML=`<div class="empty" style="grid-column:1/-1">🚗 Гараж пуст. Пора на СТО Кликер!</div>`;return;}grid.innerHTML=state.garage.map((c,i)=>{const cond=CONDITIONS.find(x=>x.id===c.condition);const stage=c.stage||0;return `<div class="card">${c.plate?`<div class="badge badge-by">BY ${c.plate}</div>`:''}<div class="card-img" id="grimg-${i}">${getCarSVG(c,300,140,false)}</div><div class="card-title">${c.name}</div><div style="display:flex;gap:4px;flex-wrap:wrap;margin:6px 0"><span class="badge badge-cond">${cond.emoji} ${cond.name}</span>${stage>0?`<span class="badge badge-stage">Stage ${stage}</span>`:''}${c.boss?'<span class="badge badge-boss">🔥 БОСС</span>':''}</div><div class="card-price mono">${calcCarPrice(c).toLocaleString('ru')} BYN</div><div style="display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-top:8px"><button class="btn btn-cyan" onclick="openInspection(${i})">🔍 Осмотр</button><button class="btn btn-gold" onclick="openTuning(${i})">🔧 Тюнинг</button><button class="btn btn-green" onclick="sellCar(${i})">💰 Продать</button><button class="btn btn-red" onclick="stakeFromGarage(${i})">⚡ Ставка</button></div></div>`;}).join('');state.garage.forEach((c,i)=>loadImgInto('grimg-'+i,c));}
function renderHistory(){const list=document.getElementById('historyList');if(!state.history.length){list.innerHTML=`<div class="empty">📜 История пуста</div>`;return;}list.innerHTML=state.history.slice().reverse().map(h=>`<div class="history-item ${h.win?'win':'loss'}"><div><div style="font-weight:700">${h.win?'✓ ПОБЕДА':'✗ ПРОИГРЫШ'}</div><div><small>${h.stakeCars.map(s=>s.name).join(', ')} ➔ ${h.targetName}</small></div><div><small>Шанс: <b class="mono">${h.pct.toFixed(2)}%</b> · Ставка: <b class="mono">${h.stake} BYN</b></small></div></div><div class="mono" style="color:${h.win?'var(--green)':'var(--red)'};font-size:14px">${h.win?'+':''}${(h.win?(h.targetPrice-h.stake):-h.stake).toLocaleString('ru')} BYN</div></div>`).join('');}
function renderCases(){document.getElementById('casesGrid').innerHTML=CASES.map(c=>`<div class="case-card" onclick="buyCase(${c.id})"><span class="case-icon">${c.icon}</span><div class="case-name">${c.name}</div><div class="case-price mono">${c.price.toLocaleString('ru')} BYN</div><div class="case-contents">Авто #${c.min} - #${c.max}</div></div>`).join('');}
function renderProfile(){const total=state.garage.reduce((s,c)=>s+calcCarPrice(c),0);const wins=state.history.filter(h=>h.win).length;const losses=state.history.filter(h=>!h.win).length;const mod=state.chanceMod||1;document.getElementById('profileInfo').innerHTML=`<div class="stat-grid"><div class="stat-card"><div class="lbl">Пользователь</div><div class="val" style="font-size:18px">${state.currentUser}</div></div><div class="stat-card"><div class="lbl">Уровень</div><div class="val" style="color:var(--cyan)">LVL ${state.level||1}</div></div><div class="stat-card"><div class="lbl">Баланс</div><div class="val" style="color:var(--gold)">${state.balance.toLocaleString('ru')}</div></div><div class="stat-card"><div class="lbl">Машин</div><div class="val" style="color:var(--cyan)">${state.garage.length}</div></div><div class="stat-card"><div class="lbl">Капитал</div><div class="val" style="color:var(--gold)">${(state.balance+total).toLocaleString('ru')}</div></div><div class="stat-card"><div class="lbl">Побед</div><div class="val" style="color:var(--green)">${wins}</div></div><div class="stat-card"><div class="lbl">Проигрышей</div><div class="val" style="color:var(--red)">${losses}</div></div><div class="stat-card"><div class="lbl">Винрейт</div><div class="val" style="color:var(--cyan)">${wins+losses>0?((wins/(wins+losses))*100).toFixed(1):0}%</div></div><div class="stat-card"><div class="lbl">Модификатор удачи</div><div class="val" style="color:${mod>1?'var(--green)':mod<1?'var(--red)':'var(--cyan)'}">×${mod}</div></div></div><h3 style="margin-top:20px">🏆 Достижения (${(state.achievements||[]).length}/${ACHIEVEMENTS.length})</h3><div class="stat-grid" style="margin-top:10px">${ACHIEVEMENTS.map(a=>{const u=(state.achievements||[]).includes(a.id);return `<div class="stat-card" style="opacity:${u?1:.4}"><div style="font-size:32px">${a.icon}</div><div style="font-weight:700;margin-top:6px">${a.name}</div><div style="font-size:11px;color:${u?'var(--green)':'var(--muted)'}">${u?'✓ Получено':'Заблокировано'}</div></div>`;}).join('')}</div>`;}
function renderPlates(){const sel=document.getElementById('plateCar');sel.innerHTML=state.garage.length?state.garage.map((c,i)=>`<option value="${i}">${c.name}${c.plate?' (с номером)':''}</option>`).join(''):'<option>Нет машин</option>';updatePlateUI();}
function updatePlateUI(){const idx=parseInt(document.getElementById('plateCar').value);const car=state.garage[idx];const sec=document.getElementById('platesSection');if(!car){sec.innerHTML='<div class="empty">Выберите машину</div>';return;}sec.innerHTML=PLATES.map(p=>`<div class="plate ${car.plate===p.id?'active':''}" onclick="buyPlate(${idx},'${p.id}',${p.mult})">${p.id}<div class="by">BY</div><div style="font-size:10px;color:#000;margin-top:6px;font-weight:400">3 500 BYN · +${Math.round(p.mult*100)}%</div></div>`).join('');}

function calcCarPrice(car){const cm=CONDITIONS.find(c=>c.id===car.condition)?.mult||1;const sm=1+(car.stage||0)*.20;const pm=1+(car.plateMult||0);return Math.round(car.price*cm*sm*pm);}
function calcStakePrice(cars){return cars.reduce((s,c)=>s+calcCarPrice(c),0);}
function calcChance(total,targetPrice){let K=.85;if(state.insurance)K*=.9;let pct=Math.min(95,(total/targetPrice)*100*K);pct=Math.min(95,pct*(state.chanceMod||1));return pct;}

function switchTab(t){playUI('tab');document.querySelectorAll('.tab').forEach(x=>x.classList.toggle('active',x.dataset.tab===t));document.querySelectorAll('.section').forEach(x=>x.classList.toggle('active',x.id==='sec-'+t));if(t==='catalog')renderCatalog();if(t==='garage')renderGarage();if(t==='history')renderHistory();if(t==='profile')renderProfile();if(t==='plates')renderPlates();if(t==='cases')renderCases();}
function toggleSound(){state.soundOn=!state.soundOn;document.getElementById('soundBtn').textContent=state.soundOn?'🔊':'🔇';save();}
function buyFromCatalog(id){const car=CARS_DB.find(c=>c.id===id);if(state.balance<car.price){notify('❌ Недостаточно BYN','error');return;}state.balance-=car.price;state.garage.push({id:car.id,name:car.name,price:car.price,wiki:car.wiki,cat:car.cat,engine:car.engine,cyl:car.cyl,year:car.year,boss:car.boss,condition:3,stage:0});addXP(5);notify(`✓ Куплено: ${car.name}`);save();renderHeader();checkAchievements();}
function sellCar(idx){const c=state.garage[idx];const price=calcCarPrice(c);if(!confirm(`Продать ${c.name} за ${price.toLocaleString('ru')} BYN?`))return;state.balance+=price;state.garage.splice(idx,1);addXP(2);notify(`💰 Продано за ${price.toLocaleString('ru')} BYN`);save();renderHeader();renderGarage();updateStakeDisplay();}
function stakeFromGarage(idx){const c=state.garage[idx];if(state.stakeCars.length>=4){notify('⚠️ Максимум 4 авто','warn');return;}if(state.stakeCars.find(s=>s.garageIdx===idx)){notify('⚠️ Уже в ставке','warn');return;}state.stakeCars.push({...c,garageIdx:idx});updateStakeDisplay();switchTab('upgrade');}
let pickerMode='stake',pickerSelected=[];
function openStakePicker(){pickerMode='stake';pickerSelected=state.stakeCars.map(s=>s.garageIdx);document.getElementById('pickerTitle').textContent='Выберите в ставку (макс 4)';renderPickerList();openModal('pickerModal');playUI('modal');}
function openTargetPicker(){pickerMode='target';pickerSelected=state.target?[state.target.id]:[];document.getElementById('pickerTitle').textContent='Выберите цель';renderPickerList();openModal('pickerModal');playUI('modal');}
function renderPickerList(){const q=(document.getElementById('pickerSearch').value||'').toLowerCase();const list=pickerMode==='stake'?state.garage.filter(c=>c.name.toLowerCase().includes(q)):CARS_DB.filter(c=>c.name.toLowerCase().includes(q));document.getElementById('pickerList').innerHTML=list.map((c,i)=>{const sel=pickerMode==='stake'?pickerSelected.includes(i):pickerSelected.includes(c.id);const price=pickerMode==='stake'?calcCarPrice(c):c.price;return `<div class="car-option ${sel?'selected':''}" onclick="togglePickerItem(${pickerMode==='stake'?i:c.id})"><div style="width:50px;height:35px;background:#1e293b;border-radius:6px;flex-shrink:0">${getCarSVG(c,50,35,false)}</div><div class="car-option-info"><b>${c.name}</b><small>${price.toLocaleString('ru')} BYN · ${c.year||''}</small></div></div>`;}).join('')||'<div class="empty">Ничего не найдено</div>';}
function togglePickerItem(id){if(pickerMode==='stake'){const idx=pickerSelected.indexOf(id);if(idx>=0)pickerSelected.splice(idx,1);else{if(pickerSelected.length>=4){notify('⚠️ Максимум 4','warn');return;}pickerSelected.push(id);}}else{pickerSelected=[id];}renderPickerList();}
function confirmPicker(){if(pickerMode==='stake'){state.stakeCars=pickerSelected.map(idx=>({...state.garage[idx],garageIdx:idx}));}else{state.target={...CARS_DB.find(c=>c.id===pickerSelected[0])};}updateStakeDisplay();closeModal('pickerModal');}
function updateStakeDisplay(){const el=document.getElementById('stakeCars');if(!state.stakeCars.length){el.innerHTML='<span class="empty" style="padding:20px">Выберите машины</span>';}else{el.innerHTML=state.stakeCars.map((c,i)=>`<div class="stake-chip"><div style="width:30px;height:20px;background:#1e293b;border-radius:4px">${getCarSVG(c,30,20,false)}</div><span>${c.name}</span><span class="x" onclick="removeStake(${i})">×</span></div>`).join('');}const total=calcStakePrice(state.stakeCars);document.getElementById('totalStake').textContent=total.toLocaleString('ru')+' BYN';document.getElementById('stakeImg').innerHTML=state.stakeCars.length?`<div style="font-size:11px;padding:2px">${state.stakeCars.length} авто</div>`:'<span style="font-size:10px;color:var(--muted)">Ставка</span>';document.getElementById('targetImg').innerHTML=state.target?`<div style="font-size:10px;padding:2px;text-align:center">${state.target.name.substring(0,20)}</div>`:'<span style="font-size:10px;color:var(--muted)">Цель</span>';updateWheel();}
function removeStake(i){state.stakeCars.splice(i,1);updateStakeDisplay();}
function quickTarget(m){if(!state.stakeCars.length){notify('⚠️ Сначала выберите ставку','warn');return;}const total=calcStakePrice(state.stakeCars);const tp=m==='auto'?total*3:total*m;const cand=CARS_DB.filter(c=>c.price>=tp*.9&&c.price<=tp*1.1);if(!cand.length){notify('⚠️ Нет подходящих целей','warn');return;}state.target={...cand[Math.floor(Math.random()*cand.length)]};updateStakeDisplay();}
function toggleInsurance(){state.insurance=!state.insurance;document.getElementById('insuranceSw').classList.toggle('on',state.insurance);updateWheel();save();}
function toggleInvert(){state.invertSector=!state.invertSector;document.getElementById('invertSw').classList.toggle('on',state.invertSector);updateWheel();save();}
function updateWheel(){const total=calcStakePrice(state.stakeCars);const tp=state.target?.price||0;const pct=tp>0&&total>0?calcChance(total,tp):0;document.getElementById('pctDisplay').textContent=pct.toFixed(2)+'%';document.getElementById('pctDisplay').classList.toggle('loss',pct<20);const deg=pct*3.6,sA=-90,eA=sA+deg,cx=100,cy=100,r=85;function arc(a1,a2){const r1=a1*Math.PI/180,r2=a2*Math.PI/180;const x1=cx+r*Math.cos(r1),y1=cy+r*Math.sin(r1);const x2=cx+r*Math.cos(r2),y2=cy+r*Math.sin(r2);const lg=(a2-a1)>180?1:0;return `M${cx},${cy} L${x1},${y1} A${r},${r} 0 ${lg} 1 ${x2},${y2} Z`;}const w=document.getElementById('winArc');w.setAttribute('d',arc(sA,eA));w.setAttribute('fill','var(--green)');w.setAttribute('stroke','none');w.setAttribute('opacity','0.5');const l=document.getElementById('loseArc');l.setAttribute('d',arc(eA,sA+360));l.setAttribute('fill','var(--red)');l.setAttribute('opacity','0.15');l.setAttribute('stroke','none');const mod=state.chanceMod||1;document.getElementById('wheelStatus').textContent=(state.target?`Цель: ${state.target.name} (${state.target.price.toLocaleString('ru')} BYN)`:'Выберите цель')+(mod!==1?` · 🎲 модификатор ×${mod}`:'');}

function doUpgrade(){if(!state.stakeCars.length){notify('❌ Выберите ставку','error');return;}if(!state.target){notify('❌ Выберите цель','error');return;}if(state.target.boss){notify('🔥 БОСС-авто нельзя апгрейдить','warn');return;}const total=calcStakePrice(state.stakeCars);const pct=calcChance(total,state.target.price);const ra=new Uint32Array(1);crypto.getRandomValues(ra);const rand=ra[0]/0xFFFFFFFF;const win=state.godMode||(rand*100<pct);const stakeSnap=state.stakeCars.map(c=>({...c}));const targetSnap={...state.target};const gi=state.stakeCars.map(c=>c.garageIdx);gi.slice().sort((a,b)=>b-a).forEach(i=>state.garage.splice(i,1));animateWheel(pct,win,()=>{if(win){state.garage.push({id:targetSnap.id,name:targetSnap.name,price:targetSnap.price,wiki:targetSnap.wiki,cat:targetSnap.cat,engine:targetSnap.engine,cyl:targetSnap.cyl,year:targetSnap.year,boss:targetSnap.boss,condition:3,stage:0});notify(`🎉 ПОБЕДА! Получено: ${targetSnap.name}`);playWin();spawnConfetti();addXP(Math.floor(Math.log10(targetSnap.price+1)*20));}else{let refund=0;if(state.insurance){refund=Math.floor(total*.25);state.balance+=refund;}notify(`💥 Проигрыш!${refund?' Возврат '+refund.toLocaleString('ru')+' BYN':''}`,'error');playLoss();addXP(3);}state.history.push({win,targetName:targetSnap.name,targetPrice:targetSnap.price,stake:total,pct,stakeCars:stakeSnap,date:Date.now()});state.stakeCars=[];state.target=null;save();renderHeader();updateStakeDisplay();renderTicker();checkAchievements();});}
function animateWheel(pct,win,cb){const arrow=document.getElementById('arrow');const ws=pct*3.6;let fd;if(win){const mn=state.invertSector?(360-ws/2+5):(-ws/2+5);const mx=state.invertSector?(360-5):(ws/2-5);fd=mn+Math.random()*(mx-mn);}else{const mn=state.invertSector?(ws/2+5):(-ws/2+5+ws);const mx=state.invertSector?(ws-5):(360-ws/2-5);fd=mn+Math.random()*(mx-mn);}const total=5*360+fd;arrow.style.transition='transform 3.5s cubic-bezier(0.25,0.1,0.25,1)';arrow.style.transform=`rotate(${total}deg)`;setTimeout(()=>{arrow.style.transition='none';arrow.style.transform=`rotate(${fd}deg)`;setTimeout(()=>{arrow.style.transition='transform 3.5s cubic-bezier(0.25,0.1,0.25,1)';},50);cb();},3550);}

let stoTarget=1900;
function stoClick(){playClick();state.balance+=10;addXP(1);renderHeader();updateStoUI();save();}
function updateStoUI(){const pct=Math.min(100,(state.balance/stoTarget)*100);document.getElementById('stoBar').style.width=pct+'%';document.getElementById('stoStatus').textContent=`${state.balance.toLocaleString('ru')} / ${stoTarget.toLocaleString('ru')} BYN`;document.querySelectorAll('.sto-car').forEach((el,i)=>{el.classList.toggle('affordable',state.balance>=(i===0?1900:2200));});}
function buyStoCar(t){const price=t===1?1900:2200;const name=t===1?'ЗАЗ-968М':'ВАЗ-2101';const id=t===1?1:2;if(state.balance<price){notify('❌ Недостаточно BYN','error');return;}state.balance-=price;state.garage.push({id,name,price,wiki:CARS_DB.find(c=>c.id===id).wiki,cat:'budget',engine:t===1?'V4_air':'I4_soviet',cyl:4,year:1979,condition:3,stage:0});notify(`🎉 Куплено: ${name}`);playWin();addXP(10);save();renderHeader();updateStoUI();checkAchievements();}

function buyCase(id){const c=CASES.find(x=>x.id===id);if(state.balance<c.price){notify('❌ Недостаточно BYN','error');return;}state.balance-=c.price;const cand=CARS_DB.filter(car=>car.id>=c.min&&car.id<=c.max&&!car.boss);if(!cand.length){notify('⚠️ Пустой контейнер','warn');return;}const won=cand[Math.floor(Math.random()*cand.length)];openCaseAnimation(won);save();renderHeader();}
function openCaseAnimation(won){const track=document.getElementById('rouletteTrack');const items=[];for(let i=0;i<50;i++){items.push(Math.random()<.1?won:CARS_DB[Math.floor(Math.random()*CARS_DB.length)]);}items[45]=won;track.innerHTML=items.map(c=>`<div class="case-roulette-item"><div style="width:100%;height:100px;background:#1e293b;border-radius:6px">${getCarSVG(c,150,100,false)}</div><div style="font-size:10px;margin-top:4px;text-align:center">${c.name}</div></div>`).join('');document.getElementById('caseResult').innerHTML='';document.getElementById('caseOpening').classList.add('active');track.style.transition='none';track.style.transform='translateX(0)';setTimeout(()=>{track.style.transition='transform 4s cubic-bezier(0.25,0.1,0.25,1)';track.style.transform='translateX(-6500px)';},100);setTimeout(()=>{state.garage.push({id:won.id,name:won.name,price:won.price,wiki:won.wiki,cat:won.cat,engine:won.engine,cyl:won.cyl,year:won.year,boss:won.boss,condition:3,stage:0});document.getElementById('caseResult').innerHTML=`<div class="win-popup">🎉 ВЫИГРАНО: ${won.name}<br><span style="font-size:18px;color:var(--gold)">${won.price.toLocaleString('ru')} BYN</span></div>`;playWin();spawnConfetti();addXP(15);save();renderHeader();checkAchievements();setTimeout(()=>{document.getElementById('caseOpening').classList.remove('active');},3000);},4100);}

function openInspection(idx){const c=state.garage[idx];const cond=CONDITIONS.find(x=>x.id===c.condition);document.getElementById('inspTitle').textContent=`🔍 ${c.name}`;document.getElementById('inspContent').innerHTML=`<div class="inspection"><div><div class="insp-img" id="inspImg">${getCarSVG(c,400,300,false)}<div class="headlights" id="headlights"></div></div><div class="insp-controls"><button class="btn btn-gold" onclick="toggleHeadlights()">🔦 Фары</button><button class="btn btn-red" onclick="playAlarm()">🚨 Сигналка</button><button class="btn btn-green" style="grid-column:1/-1" onclick="playEngine('${c.engine}')">🔑 Завести / Выхлоп</button></div></div><div><div class="garage-stat-row"><span>Год:</span><b>${c.year}</b></div><div class="garage-stat-row"><span>Двигатель:</span><b>${c.engine}</b></div><div class="garage-stat-row"><span>Цилиндров:</span><b>${c.cyl||'-'}</b></div><div class="garage-stat-row"><span>Состояние:</span><b style="color:var(--cyan)">${cond.emoji} ${cond.name}</b></div><div class="garage-stat-row"><span>Stage:</span><b style="color:var(--orange)">${c.stage||0}</b></div><div class="garage-stat-row"><span>Номер:</span><b style="color:var(--red)">${c.plate||'нет'}</b></div><div class="garage-stat-row"><span>База:</span><b class="mono" style="color:var(--gold)">${c.price.toLocaleString('ru')}</b></div><div class="garage-stat-row"><span>Текущая:</span><b class="mono" style="color:var(--gold);font-size:18px">${calcCarPrice(c).toLocaleString('ru')} BYN</b></div></div></div>`;loadImgInto('inspImg',c);openModal('inspModal');playUI('modal');}
function toggleHeadlights(){document.getElementById('headlights').classList.toggle('on');}
function openTuning(idx){const c=state.garage[idx];const cond=c.condition;const stage=c.stage||0;const cp={2:2000,3:5000,4:15000,5:50000};const sp={1:Math.floor(c.price*.15),2:Math.floor(c.price*.25),3:Math.floor(c.price*.4)};document.getElementById('tuneContent').innerHTML=`<h3 style="color:var(--cyan);margin:12px 0">🚗 ${c.name}</h3><h4 style="margin:14px 0 8px">Кузов:</h4><div style="display:grid;gap:6px">${[2,3,4,5].map(cid=>{const cn=CONDITIONS.find(x=>x.id===cid);if(cid<=cond)return `<div class="car-option" style="opacity:.5;cursor:not-allowed"><span>✓ ${cn.emoji} ${cn.name}</span></div>`;return `<div class="car-option" onclick="tuneCondition(${idx},${cid},${cp[cid]})"><div style="flex:1"><b>${cn.emoji} ${cn.name}</b><br><small>×${cn.mult}</small></div><b class="mono" style="color:var(--gold)">${cp[cid].toLocaleString('ru')} BYN</b></div>`;}).join('')}</div><h4 style="margin:14px 0 8px">🔥 ДВС:</h4><div style="display:grid;gap:6px">${[1,2,3].map(s=>{if(s<=stage)return `<div class="car-option" style="opacity:.5;cursor:not-allowed"><span>✓ Stage ${s}</span></div>`;return `<div class="car-option" onclick="tuneStage(${idx},${s},${sp[s]})"><div style="flex:1"><b>Stage ${s}</b><br><small>+${s*20}%</small></div><b class="mono" style="color:var(--gold)">${sp[s].toLocaleString('ru')} BYN</b></div>`;}).join('')}</div>`;openModal('tuneModal');playUI('modal');}
function tuneCondition(i,nc,p){if(state.balance<p){notify('❌ Недостаточно BYN','error');return;}state.balance-=p;state.garage[i].condition=nc;notify('✓ Состояние улучшено');save();renderHeader();renderGarage();closeModal('tuneModal');checkAchievements();}
function tuneStage(i,ns,p){if(state.balance<p){notify('❌ Недостаточно BYN','error');return;}state.balance-=p;state.garage[i].stage=ns;addXP(5);notify(`✓ Stage ${ns}`);save();renderHeader();renderGarage();closeModal('tuneModal');checkAchievements();}
function buyPlate(idx,pid,m){const c=state.garage[idx];if(!c){notify('⚠️ Выберите машину','warn');return;}if(c.plate){notify('⚠️ Уже есть номер','warn');return;}if(state.balance<3500){notify('❌ Нужно 3 500 BYN','error');return;}state.balance-=3500;c.plate=pid;c.plateMult=m;addXP(8);notify(`🇧 Номер ${pid}!`);save();renderHeader();updatePlateUI();renderGarage();checkAchievements();}

function checkAchievements(){if(!state.achievements)state.achievements=[];ACHIEVEMENTS.forEach(a=>{if(!state.achievements.includes(a.id)&&a.check(state)){state.achievements.push(a.id);showAchievement(a);addXP(25);}});}
function showAchievement(a){const t=document.getElementById('achievementToast');t.innerHTML=`<span style="font-size:32px">${a.icon}</span><div><div style="font-size:11px;opacity:.7">ДОСТИЖЕНИЕ</div><div>${a.name}</div></div>`;t.classList.add('show');setTimeout(()=>t.classList.remove('show'),4000);}

function openModal(id){document.getElementById(id).classList.add('active');}
function closeModal(id){document.getElementById(id).classList.remove('active');}
function notify(msg,type='success'){const n=document.createElement('div');n.className='notification '+type;n.textContent=msg;document.body.appendChild(n);setTimeout(()=>{n.style.animation='slideIn .3s reverse';setTimeout(()=>n.remove(),300);},2500);}

document.querySelectorAll('.auth-tab').forEach(t=>{t.addEventListener('click',()=>{document.querySelectorAll('.auth-tab').forEach(x=>x.classList.remove('active'));document.querySelectorAll('.auth-form').forEach(x=>x.classList.remove('active'));t.classList.add('active');document.getElementById(t.dataset.mode==='reg'?'formReg':'formLog').classList.add('active');playUI('click');});});
function afterLogin(){document.getElementById('authOverlay').classList.add('hidden');renderHeader();renderTicker();renderCases();updateStoUI();updateStakeDisplay();}
function register(){const u=document.getElementById('regUser').value.trim();const p=document.getElementById('regPass').value;const p2=document.getElementById('regPass2').value;if(!u||u.length<2){notify('⚠️ Ник минимум 2 символа','warn');return;}if(u.toLowerCase()===ADMIN_LOGIN.toLowerCase()){notify('⚠️ Этот ник зарезервирован','warn');return;}if(p.length<4){notify('⚠️ Пароль минимум 4 символа','warn');return;}if(p!==p2){notify('⚠️ Пароли не совпадают','warn');return;}if(localStorage.getItem(SAVE_KEY+u)){notify('⚠️ Такой ник уже есть','warn');return;}state=freshState(u);state.balance=100;state.garage=[{id:1,name:'ЗАЗ-968М Запорожец',price:1900,wiki:'ZAZ-968',cat:'budget',engine:'V4_air',cyl:4,year:1979,condition:3,stage:0}];save();const db=getUsersDB();db[u]={pass:p,balance:100,cars:1,capital:2000,xp:0,level:1,achievements:[],chanceMod:1};localStorage.setItem(DB_KEY,JSON.stringify(db));afterLogin();notify(`🎁 Добро пожаловать, ${u}! Получен ЗАЗ-968М + 100 BYN`);setTimeout(()=>showAchievement({icon:'🎉',name:'Первые шаги!'}),500);}
function login(){const u=document.getElementById('loginUser').value.trim();const p=document.getElementById('loginPass').value;if(u===ADMIN_LOGIN&&p===ADMIN_PASS){if(!loadSave(ADMIN_LOGIN)){state=freshState(ADMIN_LOGIN);}state.isAdmin=true;state.currentUser=ADMIN_LOGIN;afterLogin();notify('🛡️ Режим администратора активен');return;}const db=getUsersDB();if(!db[u]||db[u].pass!==p){notify('❌ Неверный логин или пароль','error');return;}if(!loadSave(u)){notify('❌ Сохранение не найдено','error');return;}state.isAdmin=false;afterLogin();notify(`✓ С возвращением, ${u}!`);}
function logout(){if(!confirm('Выйти из аккаунта?'))return;state=freshState(null);document.getElementById('authOverlay').classList.remove('hidden');closeModal('authModal');renderHeader();}
function openAuth(){if(!state.currentUser)return;document.getElementById('authWho').textContent=state.currentUser+(state.isAdmin?' (админ)':'');openModal('authModal');playUI('modal');}

// ===== АДМИН =====
function openAdmin(){if(!state.isAdmin){notify('❌ Доступ запрещён','error');return;}document.getElementById('adminCar').innerHTML=CARS_DB.map(c=>`<option value="${c.id}">${c.name} (${c.price.toLocaleString('ru')})</option>`).join('');document.getElementById('godSw').classList.toggle('on',state.godMode);renderDBTable();pmPopulate();openModal('adminModal');playUI('modal');}
function renderDBTable(){const db=getUsersDB();const users=Object.entries(db);document.getElementById('dbTable').innerHTML=users.length?`<table class="db-table"><tr><th>Ник</th><th>Машин</th><th>Капитал</th><th>LVL</th><th>Шанс</th></tr>${users.map(([u,d])=>`<tr><td>${u}</td><td>${d.cars||0}</td><td class="mono">${(d.capital||0).toLocaleString('ru')}</td><td>${d.level||1}</td><td class="mono" style="color:${(d.chanceMod||1)>1?'var(--green)':(d.chanceMod||1)<1?'var(--red)':'var(--cyan)'}">×${d.chanceMod||1}</td></tr>`).join('')}</table>`:'<p style="color:var(--muted)">Пока нет игроков</p>';}
function exportDB(){const data={users:getUsersDB(),currentSave:state};const blob=new Blob([JSON.stringify(data,null,2)],{type:'application/json'});const a=document.createElement('a');a.href=URL.createObjectURL(blob);a.download='auto_upgrade_db_'+Date.now()+'.json';a.click();}
function adminAdd(v){state.balance+=v;notify(`💰 Баланс: ${state.balance.toLocaleString('ru')} BYN`);save();renderHeader();}
function adminGiveCar(){const id=parseInt(document.getElementById('adminCar').value);const c=CARS_DB.find(x=>x.id===id);state.garage.push({id:c.id,name:c.name,price:c.price,wiki:c.wiki,cat:c.cat,engine:c.engine,cyl:c.cyl,year:c.year,boss:c.boss,condition:5,stage:3,plate:'7777 AA-7',plateMult:.35});notify(`🎁 Выдано: ${c.name}`);save();renderGarage();renderHeader();}
function toggleGod(){state.godMode=!state.godMode;document.getElementById('godSw').classList.toggle('on',state.godMode);notify(state.godMode?'🔥 God Mode ON':'God Mode OFF');save();}

// ===== УПРАВЛЕНИЕ ИГРОКАМИ =====
function pmList(){return Object.keys(getUsersDB()).filter(u=>u!==ADMIN_LOGIN);}
function pmPopulate(){const sel=document.getElementById('pmUser');const users=pmList();const cur=sel.value;sel.innerHTML=users.length?users.map(u=>`<option value="${u}">${u}</option>`).join(''):'<option value="">— нет игроков —</option>';if(users.includes(cur))sel.value=cur;pmRender();}
function pmRender(){const u=document.getElementById('pmUser').value;const panel=document.getElementById('pmPanel');if(!u){panel.innerHTML='<p style="color:var(--muted)">Нет зарегистрированных игроков</p>';return;}const d=getPlayerSave(u);if(!d){panel.innerHTML='<p style="color:var(--muted)">Сейв игрока не найден</p>';return;}const mod=d.chanceMod||1;
panel.innerHTML=`<div class="garage-stat-row"><span>Баланс:</span><b class="mono">${(d.balance||0).toLocaleString('ru')} BYN</b></div>
<div class="garage-stat-row"><span>Машин:</span><b class="mono">${(d.garage||[]).length}</b></div>
<div class="garage-stat-row"><span>Уровень:</span><b class="mono">LVL ${d.level||1}</b></div>
<div class="garage-stat-row"><span>Шанс победы:</span><b class="mono" style="color:${mod>1?'var(--green)':mod<1?'var(--red)':'var(--cyan)'}">×${mod}</b></div>
<h4 style="margin:12px 0 6px">💰 Деньги</h4>
<div style="display:flex;gap:6px;flex-wrap:wrap"><button class="btn btn-green" onclick="pmMoney('${u}',10000)">+10k</button><button class="btn btn-green" onclick="pmMoney('${u}',100000)">+100k</button><button class="btn btn-green" onclick="pmMoney('${u}',1000000)">+1M</button><button class="btn btn-red" onclick="pmZero('${u}')">Обнулить</button></div>
<input class="input" id="pmMoneyVal" type="number" placeholder="Своя сумма (можно отрицательную)" style="margin-top:8px">
<button class="btn btn-cyan" onclick="pmMoney('${u}',parseInt(document.getElementById('pmMoneyVal').value)||0)">Применить сумму</button>
<h4 style="margin:12px 0 6px">🎲 Шанс победы (множитель 0.1–5)</h4>
<div style="display:flex;gap:6px;flex-wrap:wrap;align-items:center"><input class="input" id="pmChanceVal" type="number" step="0.1" min="0.1" max="5" value="${mod}" style="width:90px;margin:0"><button class="btn btn-cyan" onclick="pmChance('${u}',parseFloat(document.getElementById('pmChanceVal').value))">Установить</button><button class="btn" onclick="pmChance('${u}',0.5)">×0.5</button><button class="btn" onclick="pmChance('${u}',1)">×1</button><button class="btn" onclick="pmChance('${u}',1.5)">×1.5</button><button class="btn" onclick="pmChance('${u}',2)">×2</button></div>
<h4 style="margin:12px 0 6px">🚗 Выдать машину</h4>
<select class="input" id="pmCar">${CARS_DB.map(c=>`<option value="${c.id}">${c.name}</option>`).join('')}</select>
<div style="display:flex;gap:6px;flex-wrap:wrap"><button class="btn btn-gold" onclick="pmGiveCar('${u}',3)">🎁 Выдать (на ходу)</button><button class="btn btn-gold" onclick="pmGiveCar('${u}',5)">👑 Выдать (капсула)</button></div>
<h4 style="margin:12px 0 6px">🚘 Гараж игрока (${(d.garage||[]).length})</h4>
<div style="max-height:160px;overflow-y:auto">${(d.garage||[]).map((c,i)=>`<div class="car-option"><div class="car-option-info"><b>${c.name}</b><small>${calcCarPrice(c).toLocaleString('ru')} BYN</small></div><button class="btn btn-red" onclick="pmRemoveCar('${u}',${i})">×</button></div>`).join('')||'<p style="color:var(--muted)">Пусто</p>'}</div>
<h4 style="margin:12px 0 6px">⚠️ Опасная зона</h4>
<div style="display:flex;gap:6px;flex-wrap:wrap"><button class="btn btn-red" onclick="pmReset('${u}')">♻️ Сброс прогресса</button><button class="btn btn-red" onclick="pmDelete('${u}')">🗑 Удалить игрока</button></div>`;}
function pmMoney(u,v){const d=getPlayerSave(u);if(!d)return;d.balance=Math.max(0,(d.balance||0)+v);setPlayerSave(u,d);notify(`💰 ${u}: баланс ${d.balance.toLocaleString('ru')} BYN`);pmRender();renderDBTable();}
function pmZero(u){const d=getPlayerSave(u);if(!d)return;pmMoney(u,-(d.balance||0));}
function pmChance(u,m){if(!(m>0))m=1;m=Math.min(5,Math.max(0.1,m));const d=getPlayerSave(u);if(!d)return;d.chanceMod=m;setPlayerSave(u,d);notify(`🎲 ${u}: модификатор шанса ×${m}`);pmRender();renderDBTable();}
function pmGiveCar(u,cond){const id=parseInt(document.getElementById('pmCar').value);const c=CARS_DB.find(x=>x.id===id);const d=getPlayerSave(u);if(!d)return;d.garage=d.garage||[];d.garage.push({id:c.id,name:c.name,price:c.price,wiki:c.wiki,cat:c.cat,engine:c.engine,cyl:c.cyl,year:c.year,boss:c.boss,condition:cond||3,stage:0});setPlayerSave(u,d);notify(`🎁 ${u} выдано: ${c.name}`);pmRender();renderDBTable();}
function pmRemoveCar(u,i){const d=getPlayerSave(u);if(!d)return;(d.garage||[]).splice(i,1);setPlayerSave(u,d);notify(`🗑 У ${u} изъята машина`);pmRender();renderDBTable();}
function pmReset(u){if(!confirm(`Сбросить прогресс игрока ${u}?`))return;const d=freshState(u);d.balance=100;d.garage=[{id:1,name:'ЗАЗ-968М Запорожец',price:1900,wiki:'ZAZ-968',cat:'budget',engine:'V4_air',cyl:4,year:1979,condition:3,stage:0}];const db=getUsersDB();d.chanceMod=(db[u]||{}).chanceMod||1;setPlayerSave(u,d);notify(`♻️ Прогресс ${u} сброшен`);pmRender();renderDBTable();}
function pmDelete(u){if(!confirm(`Удалить игрока ${u} полностью?`))return;localStorage.removeItem(SAVE_KEY+u);const db=getUsersDB();delete db[u];localStorage.setItem(DB_KEY,JSON.stringify(db));pmPopulate();renderDBTable();notify(`🗑 Игрок ${u} удалён`);}

document.querySelectorAll('.tab').forEach(t=>t.addEventListener('click',()=>switchTab(t.dataset.tab)));
(function init(){const db=getUsersDB();const users=Object.keys(db);if(users.length===1&&loadSave(users[0])){state.isAdmin=false;document.getElementById('authOverlay').classList.add('hidden');}renderHeader();renderTicker();renderCases();updateStoUI();updateStakeDisplay();updateWheel();setInterval(renderTicker,60000);})();
</script>
</body>
</html>
