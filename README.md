<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="My Dashboard">
<title>My Dashboard</title>
<link href="https://fonts.googleapis.com/css2?family=Lora:ital,wght@0,500;0,600;1,400&family=Noto+Sans+Thai:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
:root{--cream:#F5F0E8;--warm:#EDE5D8;--sand:#D4C5A9;--brown:#8B6F4E;--dark:#4A3728;--terra:#C17F4A;--rust:#A85C38;--sage:#7A9170;--moss:#5C7A52;--clay:#C4875A;--blush:#C49A8A;--muted:#9A8878;--border:#DDD0BC;--surface:#FDFAF5;--text:#2E221A;--safe-top:env(safe-area-inset-top,0px);--safe-bot:env(safe-area-inset-bottom,0px);}
*{box-sizing:border-box;margin:0;padding:0;-webkit-tap-highlight-color:transparent;}
html,body{height:100%;width:100%;overflow:hidden;position:fixed;}
body{font-family:'Noto Sans Thai',sans-serif;background:var(--cream);color:var(--text);}
body::before{content:'';position:fixed;inset:0;background:radial-gradient(ellipse at 20% 10%,rgba(193,127,74,.08) 0%,transparent 50%),radial-gradient(ellipse at 80% 90%,rgba(122,145,112,.07) 0%,transparent 50%);pointer-events:none;z-index:0;}
.hdr{padding:calc(var(--safe-top) + 12px) 18px 10px;background:var(--surface);border-bottom:1px solid var(--border);position:fixed;top:0;left:0;right:0;z-index:10;}
.hdr-top{display:flex;align-items:center;justify-content:space-between;margin-bottom:2px;}
.hdr-title{font-family:'Lora',serif;font-size:21px;font-weight:600;color:var(--dark);}
.hdr-date{font-size:11px;color:var(--muted);background:var(--warm);padding:3px 10px;border-radius:12px;font-weight:500;}
.hdr-sub{font-size:11px;color:var(--muted);}
.tab-bar{display:flex;background:var(--surface);border-top:1px solid var(--border);padding-bottom:var(--safe-bot);position:fixed;bottom:0;left:0;right:0;z-index:10;}
.tab-item{flex:1;display:flex;flex-direction:column;align-items:center;padding:9px 4px 7px;cursor:pointer;gap:2px;}
.tab-icon{font-size:21px;line-height:1;transition:transform .2s;}
.tab-label{font-size:10px;color:var(--muted);font-weight:500;}
.tab-item.active .tab-label{color:var(--terra);font-weight:600;}
.tab-item.active .tab-icon{transform:scale(1.15);}
.main{position:fixed;top:0;left:0;right:0;bottom:0;overflow-y:scroll;overflow-x:hidden;-webkit-overflow-scrolling:touch;z-index:1;padding-top:calc(var(--safe-top) + 68px);padding-bottom:calc(var(--safe-bot) + 60px);}
.view{display:none;padding:14px 14px 32px;width:100%;}
.view.active{display:block;}
/* CALENDAR */
.month-block{margin-bottom:28px;}
.month-hdr{display:flex;align-items:baseline;gap:8px;margin-bottom:10px;}
.month-name{font-family:'Lora',serif;font-size:20px;font-weight:600;color:var(--dark);}
.month-yr{font-size:12px;color:var(--muted);}
.dow-row{display:grid;grid-template-columns:repeat(7,1fr);gap:3px;margin-bottom:3px;}
.dow-cell{text-align:center;font-size:10px;font-weight:600;color:var(--muted);padding:2px 0;}
.dow-cell.wk{color:var(--rust);}
.cal-grid{display:grid;grid-template-columns:repeat(7,1fr);gap:3px;}
.cc{background:var(--surface);border-radius:10px;border:1px solid var(--border);padding:6px 5px 5px;min-height:68px;cursor:pointer;transition:all .15s;overflow:hidden;}
.cc:active{transform:scale(.96);}
.cc.emp{background:transparent;border-color:transparent;cursor:default;pointer-events:none;}
.cc.bty{background:#F8EEE8;border-color:var(--blush);}
.cc.wkd{background:#FAF7F2;}
.cc.tod{border-color:var(--terra)!important;background:#FDF4EC;}
.cc.fad{opacity:.5;}
.cc .cn{font-family:'Lora',serif;font-size:15px;font-weight:600;color:var(--dark);line-height:1;margin-bottom:4px;}
.cc.wkd .cn{color:var(--rust);}
.cc.bty .cn{color:var(--blush);}
.cc.tod .cn{color:var(--terra);}
.cc .ctags{display:flex;flex-direction:column;gap:2px;}
.ctag{font-size:8.5px;font-weight:600;padding:1px 5px;border-radius:4px;overflow:hidden;text-overflow:ellipsis;white-space:nowrap;line-height:1.6;}
.ct-coral{background:#F5E4DE;color:#A85C38;}
.ct-amber{background:#F5EDDE;color:#8B6F4E;}
.ct-sage{background:#E4EDE0;color:#5C7A52;}
.ct-blue{background:#E0E8F0;color:#4A6A8A;}
.ct-pink{background:#F0E4EC;color:#8A4A6A;}
/* TODAY */
.day-pills{display:flex;gap:6px;overflow-x:auto;overflow-y:hidden;-webkit-overflow-scrolling:touch;padding-bottom:4px;margin-bottom:14px;scrollbar-width:none;flex-shrink:0;}
.day-pills::-webkit-scrollbar{display:none;}
.dp{display:flex;flex-direction:column;align-items:center;min-width:50px;padding:7px 5px;border-radius:13px;border:1.5px solid var(--border);background:var(--surface);cursor:pointer;transition:all .2s;flex-shrink:0;}
.dp .dd{font-size:9px;font-weight:600;color:var(--muted);letter-spacing:.5px;}
.dp .dn{font-family:'Lora',serif;font-size:19px;font-weight:600;color:var(--dark);line-height:1.1;}
.dp .dot{width:5px;height:5px;border-radius:50%;margin-top:3px;background:transparent;}
.dp.act{border-color:var(--terra);background:var(--terra);}
.dp.act .dd,.dp.act .dn{color:white;}
.dp.bty.act{background:var(--blush);border-color:var(--blush);}
.dp.tod .dot{background:var(--terra);}
.dp.act .dot{background:rgba(255,255,255,.6);}
.card{background:var(--surface);border-radius:16px;border:1px solid var(--border);overflow:hidden;margin-bottom:12px;box-shadow:0 2px 10px rgba(74,55,40,.05);}
.card-hdr{display:flex;align-items:center;gap:10px;padding:13px 15px 10px;border-bottom:1px solid var(--border);}
.c-icon{width:32px;height:32px;border-radius:9px;display:flex;align-items:center;justify-content:center;font-size:16px;flex-shrink:0;}
.c-title{font-family:'Lora',serif;font-size:14px;font-weight:600;color:var(--dark);flex:1;line-height:1.3;}
.c-prog{font-size:11px;font-weight:600;padding:2px 9px;border-radius:10px;background:var(--warm);color:var(--brown);flex-shrink:0;transition:all .3s;}
.c-prog.done{background:#D4E8C8;color:var(--moss);}
.pb-wrap{height:3px;background:var(--warm);}
.pb{height:100%;background:linear-gradient(90deg,var(--terra),var(--clay));transition:width .4s;}
.trow{display:flex;align-items:center;gap:11px;padding:11px 15px;border-bottom:1px solid var(--warm);cursor:pointer;-webkit-user-select:none;user-select:none;transition:background .12s;}
.trow:last-of-type{border-bottom:none;}
.trow:active{background:var(--warm);}
.trow.done .ttext{text-decoration:line-through;color:var(--sand);}
.trow.done .ttime{color:var(--sand);}
.tchk{width:23px;height:23px;border-radius:7px;border:2px solid var(--border);display:flex;align-items:center;justify-content:center;flex-shrink:0;font-size:12px;background:white;transition:all .22s cubic-bezier(.34,1.56,.64,1);}
.trow.done .tchk{transform:scale(1.08);}
.ttime{font-size:11px;color:var(--muted);min-width:40px;font-weight:500;font-variant-numeric:tabular-nums;}
.ttext{font-size:13.5px;flex:1;line-height:1.4;}
.tedit{font-size:15px;color:var(--sand);padding:4px;flex-shrink:0;}
@keyframes pop{0%{transform:scale(1)}40%{transform:scale(1.4)}70%{transform:scale(.9)}100%{transform:scale(1.08)}}
.jc .tchk{animation:pop .35s ease forwards;}
.add-btn{display:flex;align-items:center;gap:8px;padding:11px 15px;color:var(--terra);font-size:13px;font-weight:500;cursor:pointer;border-top:1px dashed var(--border);}
.add-btn:active{background:var(--warm);}
/* BEAUTY */
.bty-card{background:linear-gradient(135deg,#F5EAE0,#EDE0D4);border:1.5px solid var(--blush);border-radius:16px;padding:22px 18px;text-align:center;margin-bottom:12px;}
.bty-card h2{font-family:'Lora',serif;font-style:italic;font-size:22px;color:var(--rust);margin-bottom:4px;}
.bty-card p{font-size:12px;color:var(--muted);margin-bottom:14px;}
.bty-pills{display:flex;flex-wrap:wrap;gap:7px;justify-content:center;}
.bty-pill{background:white;border:1.5px solid var(--blush);color:var(--rust);font-size:13px;font-weight:500;padding:7px 13px;border-radius:20px;cursor:pointer;transition:all .2s;}
.bty-pill.on{background:var(--blush);color:white;border-color:var(--blush);}
/* HABITS */
.sec-title{font-family:'Lora',serif;font-size:17px;font-weight:600;color:var(--dark);margin-bottom:12px;display:flex;align-items:center;gap:8px;}
.sec-title::after{content:'';flex:1;height:1px;background:var(--border);}
.sm-btn{background:var(--warm);border:1px solid var(--border);color:var(--brown);font-family:'Noto Sans Thai',sans-serif;font-size:11px;font-weight:500;padding:5px 12px;border-radius:16px;cursor:pointer;white-space:nowrap;}
.hab-hdr{display:flex;align-items:center;padding:7px 14px 4px;gap:8px;}
.hab-hdr-lbl{font-size:10px;font-weight:600;color:var(--muted);flex:1;}
.hab-hdr-days{display:flex;gap:5px;}
.hab-hdr-d{width:30px;text-align:center;font-size:9px;font-weight:600;color:var(--muted);}
.hab-row{display:flex;align-items:center;padding:9px 14px;gap:8px;border-bottom:1px solid var(--warm);}
.hab-row:last-child{border-bottom:none;}
.hab-lbl{font-size:13px;font-weight:500;flex:1;min-width:0;}
.hab-days{display:flex;gap:5px;}
.hdot{width:30px;height:30px;border-radius:50%;border:2px solid var(--border);background:var(--warm);display:flex;align-items:center;justify-content:center;font-size:10px;font-weight:600;color:var(--muted);cursor:pointer;transition:all .2s cubic-bezier(.34,1.56,.64,1);flex-shrink:0;}
.hdot:active{transform:scale(.88);}
.hdot.on{border-color:transparent;color:white;transform:scale(1.1);}
.edit-row-btn{font-size:14px;color:var(--sand);padding:4px 2px;cursor:pointer;flex-shrink:0;}
/* GOALS */
.goal-item{border-radius:14px;padding:14px;display:flex;gap:12px;align-items:flex-start;border:1px solid transparent;margin-bottom:10px;}
.g-emoji{font-size:26px;flex-shrink:0;line-height:1;}
.g-title{font-size:11px;font-weight:600;letter-spacing:.8px;margin-bottom:4px;opacity:.7;}
.g-text{font-size:13px;line-height:1.6;font-weight:500;}
.g-edit{font-size:14px;color:var(--sand);margin-left:auto;align-self:flex-start;padding:2px;cursor:pointer;}
/* MODAL */
.overlay{display:none;position:fixed;inset:0;background:rgba(46,34,26,.5);z-index:200;align-items:flex-end;backdrop-filter:blur(4px);}
.overlay.open{display:flex;}
.sheet{background:var(--surface);border-radius:22px 22px 0 0;padding:18px 18px calc(18px + var(--safe-bot));width:100%;max-height:85svh;max-height:85vh;overflow-y:auto;-webkit-overflow-scrolling:touch;animation:su .26s cubic-bezier(.34,1.2,.64,1);}
@keyframes su{from{transform:translateY(100%)}to{transform:translateY(0)}}
.sheet-handle{width:34px;height:4px;border-radius:2px;background:var(--border);margin:0 auto 14px;}
.sheet-title{font-family:'Lora',serif;font-size:16px;font-weight:600;color:var(--dark);margin-bottom:12px;}
.fl{font-size:11px;font-weight:600;color:var(--muted);margin-bottom:5px;letter-spacing:.5px;display:block;}
.fi{width:100%;padding:11px 13px;border:1.5px solid var(--border);border-radius:11px;font-family:'Noto Sans Thai',sans-serif;font-size:14px;color:var(--text);background:var(--cream);outline:none;margin-bottom:10px;}
.fi:focus{border-color:var(--terra);}
.ft{width:100px;padding:10px 12px;border:1.5px solid var(--border);border-radius:11px;font-family:'Noto Sans Thai',sans-serif;font-size:14px;color:var(--text);background:var(--cream);outline:none;margin-bottom:10px;}
.ft:focus{border-color:var(--terra);}
.cr{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:10px;}
.co{width:28px;height:28px;border-radius:50%;cursor:pointer;border:2px solid transparent;transition:all .18s;}
.co.sel{border-color:var(--dark);transform:scale(1.18);}
.er{display:flex;gap:8px;flex-wrap:wrap;margin-bottom:10px;}
.eo{font-size:22px;padding:4px;cursor:pointer;border-radius:8px;border:2px solid transparent;}
.eo.sel{border-color:var(--terra);background:var(--warm);}
.btn-row{display:flex;gap:8px;margin-top:12px;}
.btn{flex:1;padding:12px;border-radius:13px;border:none;font-family:'Noto Sans Thai',sans-serif;font-size:14px;font-weight:600;cursor:pointer;transition:all .18s;}
.btn:active{transform:scale(.97);}
.btn-c{background:var(--warm);color:var(--brown);}
.btn-s{background:var(--terra);color:white;}
.btn-d{background:#F0E0DA;color:var(--rust);flex:0;min-width:46px;}
::-webkit-scrollbar{width:0;}
</style>
</head>
<body>
<div class="hdr">
  <div class="hdr-top">
    <div class="hdr-title">✦ My Dashboard</div>
    <div class="hdr-date" id="hdrDate"></div>
  </div>
  <div class="hdr-sub" id="hdrSub"></div>
</div>
<div class="main">
  <div class="view active" id="v-today">
    <div class="day-pills" id="dayPills"></div>
    <div id="todayContent"></div>
  </div>
  <div class="view" id="v-cal"><div id="calContent"></div></div>
  <div class="view" id="v-hab">
    <div class="sec-title">💪 Habit Tracker
      <button class="sm-btn" onclick="resetHab()">Reset</button>
      <button class="sm-btn" onclick="openAddHabit()">+ เพิ่ม</button>
    </div>
    <div class="card" id="habGrid"></div>
  </div>
  <div class="view" id="v-goals">
    <div class="sec-title">🎯 Goals เดือนนี้
      <button class="sm-btn" onclick="openAddGoal()">+ เพิ่ม</button>
    </div>
    <div id="goalsList"></div>
  </div>
</div>
<div class="tab-bar">
  <div class="tab-item active" onclick="sv('today',this)"><div class="tab-icon">☀️</div><div class="tab-label">วันนี้</div></div>
  <div class="tab-item" onclick="sv('cal',this)"><div class="tab-icon">📅</div><div class="tab-label">ปฏิทิน</div></div>
  <div class="tab-item" onclick="sv('hab',this)"><div class="tab-icon">💪</div><div class="tab-label">Habits</div></div>
  <div class="tab-item" onclick="sv('goals',this)"><div class="tab-icon">🎯</div><div class="tab-label">Goals</div></div>
</div>
<div class="overlay" id="overlay" onclick="closeBg(event)">
  <div class="sheet" id="sheet"></div>
</div>
<script>
const DOW_S=['จ','อ','พ','พฤ','ศ','ส','อา'];
const DOW_F=['จันทร์','อังคาร','พุธ','พฤหัส','ศุกร์','เสาร์','อาทิตย์'];
const MTH=['ม.ค.','ก.พ.','มี.ค.','เม.ย.','พ.ค.','มิ.ย.','ก.ค.','ส.ค.','ก.ย.','ต.ค.','พ.ย.','ธ.ค.'];
const MFULL=['มกราคม','กุมภาพันธ์','มีนาคม','เมษายน','พฤษภาคม','มิถุนายน','กรกฎาคม','สิงหาคม','กันยายน','ตุลาคม','พฤศจิกายน','ธันวาคม'];
const DTHS=['อาทิตย์','จันทร์','อังคาร','พุธ','พฤหัส','ศุกร์','เสาร์'];
const COL=[{icon:'🎬',ibg:'#F0E4D0',a:'#C17F4A'},{icon:'💉',ibg:'#EDD8CC',a:'#A85C38'},{icon:'🕶️',ibg:'#D8E8CC',a:'#5C7A52'},{icon:'🏠',ibg:'#DCD8EC',a:'#6A5A88'},{icon:'🔴',ibg:'#E4D0CC',a:'#A85C38'},{icon:'❤️',ibg:'#E8E0D4',a:'#8B6F4E'},{icon:'🌿',ibg:'#D4E4D4',a:'#5C7A52'}];
const DR={
  0:[{t:'08:00',x:'ตื่น + Morning routine'},{t:'08:30',x:'🏋️ ออกกำลังกาย 1.5 ชม.'},{t:'10:00',x:'📝 Plan + Script คลิปทั้งสัปดาห์'},{t:'11:00',x:'📖 เรียนภาษา 1 ชม.'},{t:'12:00',x:'🍽️ พักกินข้าว'},{t:'13:00',x:'🎬 ถ่าย Batch 3–5 คลิป'},{t:'15:00',x:'📘 คอร์สธุรกิจ 1 ชม.'},{t:'16:00',x:'✂️ ตัดต่อ + Schedule โพสต์'},{t:'17:30',x:'🌙 กินข้าวเย็น / พัก'},{t:'19:00',x:'📖 เรียนภาษา 1 ชม.'},{t:'20:30',x:'📚 อ่านหนังสือ 1 ชม.'},{t:'23:00',x:'😴 นอน'}],
  1:[{t:'08:00',x:'ตื่น + Morning routine'},{t:'08:30',x:'🏋️ ออกกำลังกาย 1–1.5 ชม.'},{t:'10:00',x:'📚 อ่านหนังสือเบาๆ'},{t:'11:00',x:'💉 สักคิ้ว ลูกค้า (11–17)'},{t:'17:00',x:'🖼️ แต่งรูป + โพสต์ IG'},{t:'18:30',x:'⚡ เตรียม Live 30 นาที'},{t:'19:00',x:'🔴 Live IG/TikTok 1 ชม.'},{t:'20:30',x:'📊 เช็ค Engage'},{t:'23:00',x:'😴 นอน'}],
  2:[{t:'08:00',x:'ตื่น + Morning routine'},{t:'08:30',x:'🏋️ ออกกำลังกาย 1.5 ชม.'},{t:'10:00',x:'📚 อ่านหนังสือเบาๆ'},{t:'11:00',x:'💉 สักคิ้ว ลูกค้า (11–17)'},{t:'17:00',x:'🖼️ แต่งรูป Babebrow + โพสต์'},{t:'18:00',x:'🕶️ Gales Haus: ถ่ายแว่น + Ads'},{t:'19:00',x:'🏠 อสังหา: หาทรัพย์'},{t:'20:30',x:'📘 คอร์สธุรกิจ 1 ชม.'},{t:'23:00',x:'😴 นอน'}],
  3:[{t:'08:00',x:'ตื่น + Morning routine'},{t:'08:30',x:'🏋️ ออกกำลังกาย 1.5 ชม.'},{t:'10:00',x:'🏠 อสังหา: หาทรัพย์'},{t:'11:00',x:'📖 เรียนภาษา 1 ชม.'},{t:'12:00',x:'🍽️ พักกินข้าว'},{t:'13:00',x:'📘 คอร์สธุรกิจ 1 ชม.'},{t:'14:00',x:'🏠 อสังหา: อัพเพจ / ตอบ DM'},{t:'17:30',x:'🌙 กินข้าวเย็น / พัก'},{t:'19:00',x:'🏠 อสังหา: Engage'},{t:'20:30',x:'📚 อ่านหนังสือ'},{t:'23:00',x:'😴 นอน'}],
  4:[{t:'08:00',x:'ตื่น + Morning routine'},{t:'08:30',x:'🏋️ ออกกำลังกาย 1.5 ชม.'},{t:'10:00',x:'📚 อ่านหนังสือเบาๆ'},{t:'11:00',x:'💉 สักคิ้ว ลูกค้า (11–17)'},{t:'17:00',x:'🖼️ แต่งรูป Babebrow + โพสต์'},{t:'17:30',x:'🕶️ Gales Haus: Ads + Insight'},{t:'18:30',x:'⚡ เตรียม Live 30 นาที'},{t:'19:00',x:'🔴 Live IG/TikTok 1 ชม.'},{t:'20:30',x:'📝 Plan Content สัปดาห์หน้า'},{t:'23:00',x:'😴 นอน'}],
  5:[{t:'08:00',x:'ตื่น + Morning routine'},{t:'08:30',x:'🏋️ ออกกำลังกาย (เบา)'},{t:'09:30',x:'📚 อ่านหนังสือเบาๆ'},{t:'11:00',x:'💉 สักคิ้ว ลูกค้า (11–17)'},{t:'17:00',x:'😌 พักผ่อน'},{t:'19:00',x:'❤️ เวลาแฟน / ออกไปกิน'},{t:'21:00',x:'📚 อ่านหนังสือ / Netflix'},{t:'23:00',x:'😴 นอน'}],
  6:[{t:'08:00',x:'ตื่น + Morning routine'},{t:'08:30',x:'🏋️ ออกกำลังกาย (เบา)'},{t:'09:30',x:'📚 อ่านหนังสือเบาๆ'},{t:'11:00',x:'💉 สักคิ้ว ลูกค้า (11–17)'},{t:'17:00',x:'🌙 กินข้าวเย็น'},{t:'18:00',x:'✂️ Content: ตัด + Schedule'},{t:'19:00',x:'📖 เรียนภาษา 1 ชม.'},{t:'20:00',x:'❤️ เวลาแฟน'},{t:'23:00',x:'😴 นอน'}],
  b:['💆 นวด / สปา','🌸 ทำผม','💅 ทำเล็บ','☕ Cafe','🛍️ Shopping','🎬 Netflix','😴 นอนเร็ว']
};
const DEF_HAB=[{e:'🏋️',l:'ออกกำลังกาย',c:'#7A9170'},{e:'📖',l:'เรียนภาษา',c:'#8A8A5C'},{e:'📚',l:'อ่านหนังสือ',c:'#8B6F4E'},{e:'🎬',l:'โพสต์ Content',c:'#C17F4A'},{e:'💉',l:'สักคิ้ว/งาน',c:'#A85C38'},{e:'😴',l:'นอนก่อน 23:30',c:'#9E8070'}];
const DEF_GOL=[{e:'💉',title:'BABEBROW',text:'ลูกค้าใหม่ 20 คน\nIG Followers +500\nLive 2x/สัปดาห์',bg:'#F5EAE0',ac:'#A85C38'},{e:'🎬',title:'CONTENT',text:'คลิปลงทุกวัน\nเริ่ม channel ใหม่',bg:'#F5F0E0',ac:'#8B6F4E'},{e:'🕶️',title:'GALES HAUS',text:'ยอดขายเพิ่มขึ้น\nPost 3x/สัปดาห์',bg:'#E8EDE0',ac:'#5C7A52'},{e:'🏠',title:'อสังหา',text:'ออกดูทรัพย์ 4 ครั้ง\nลูกค้าใหม่ 2 คน',bg:'#EDE8F0',ac:'#6A5A88'},{e:'📖',title:'ภาษา',text:'เรียน 12 ครั้ง/เดือน\nอังกฤษ + จีน',bg:'#E8F0E8',ac:'#5C7A52'},{e:'💪',title:'สุขภาพ',text:'ออกกำลัง 4–5x/สัปดาห์\nนอน 7–8 ชม.',bg:'#F5EDE8',ac:'#A85C38'}];
const HCOLS=['#7A9170','#8A8A5C','#8B6F4E','#C17F4A','#A85C38','#9E8070','#6A5A88','#4A6A8A'];
const GBGS=['#F5EAE0','#F5F0E0','#E8EDE0','#EDE8F0','#E8F0E8','#F5EDE8'];
const GACS=['#A85C38','#8B6F4E','#5C7A52','#6A5A88','#4A6A8A','#C17F4A'];
const EMOH=['🏋️','📖','📚','🎬','💉','😴','🧘','💊','🥗','💧','🧹','✍️','🎯','🌿'];
const EMOG=['💉','🎬','🕶️','🏠','📖','💪','🎯','💰','✈️','🌱','⭐','🔥'];
const S={get:k=>{try{return JSON.parse(localStorage.getItem('d_'+k))??null}catch{return null}},set:(k,v)=>{try{localStorage.setItem('d_'+k,JSON.stringify(v))}catch{}}};
function getH(){return S.get('hd')||JSON.parse(JSON.stringify(DEF_HAB));}
function setH(h){S.set('hd',h);}
function getG(){return S.get('gd')||JSON.parse(JSON.stringify(DEF_GOL));}
function setG(g){S.set('gd',g);}
// getR replaced below
// setR replaced below
const NOW=new Date();
const TDOW=(NOW.getDay()+6)%7;
let sDOW=TDOW,sDK=dk(NOW);
function dk(d){return d.getFullYear()+'-'+(d.getMonth()+1)+'-'+d.getDate();}
function ws(){const d=new Date(NOW);d.setDate(NOW.getDate()-TDOW);d.setHours(0,0,0,0);return d;}
function isB(date){const d=(date.getDay()+6)%7;if(d!==3)return false;let c=0;for(let i=1;i<=date.getDate();i++)if((new Date(date.getFullYear(),date.getMonth(),i).getDay()+6)%7===3)c++;return c===2||c===4;}
// HEADER
function initHdr(){document.getElementById('hdrDate').textContent=NOW.getDate()+' '+MTH[NOW.getMonth()]+' '+(NOW.getFullYear()+543);document.getElementById('hdrSub').textContent='วัน'+DTHS[NOW.getDay()]+' · สวัสดีวันใหม่ 🌿';}
// PILLS
function getWeekStartForDate(date){
  const d=new Date(date);
  const dow=(d.getDay()+6)%7;
  d.setDate(d.getDate()-dow);
  d.setHours(0,0,0,0);
  return d;
}
function renderPills(){renderPillsForDate(new Date(NOW));}
function renderPillsForDate(anchorDate){
  const el=document.getElementById('dayPills');el.innerHTML='';
  const w=getWeekStartForDate(anchorDate);
  for(let i=0;i<7;i++){
    const d=new Date(w);d.setDate(w.getDate()+i);
    const b=isB(d),t=dk(d)===dk(NOW);
    const isSelected=dk(d)===sDK;
    const p=document.createElement('div');
    p.className='dp'+(b?' bty':'')+(t?' tod':'')+(isSelected?' act':'');
    p.innerHTML='<span class="dd">'+DOW_S[i]+'</span><span class="dn">'+d.getDate()+'</span><span class="dot"></span>';
    p.onclick=()=>{
      sDOW=i;sDK=dk(d);
      document.querySelectorAll('.dp').forEach(x=>x.classList.remove('act'));
      p.classList.add('act');renderToday();
    };
    el.appendChild(p);
  }
}
// TODAY
function renderToday(){const el=document.getElementById('todayContent');el.innerHTML='';const w=ws();const sd=new Date(w);sd.setDate(w.getDate()+sDOW);if(isB(sd)){renderBty(el);return;}const col=COL[sDOW],r=getRbyDate(sDK),st=S.get(sDK)||{};const tot=r.length,don=r.filter((_,i)=>st[i]).length;const card=document.createElement('div');card.className='card';const hdr=document.createElement('div');hdr.className='card-hdr';hdr.innerHTML='<div class="c-icon" style="background:'+col.ibg+'">'+col.icon+'</div><div class="c-title">'+DOW_F[sDOW]+'</div><div class="c-prog '+(don===tot?'done':'')+'" id="cP">'+don+'/'+tot+'</div>';card.appendChild(hdr);const pw=document.createElement('div');pw.className='pb-wrap';const pb=document.createElement('div');pb.className='pb';pb.id='cB';pb.style.width=tot>0?(don/tot*100)+'%':'0%';pw.appendChild(pb);card.appendChild(pw);r.forEach((item,idx)=>card.appendChild(mkRow(item,idx,!!st[idx],sDOW,sDK,tot,col)));const ab=document.createElement('div');ab.className='add-btn';ab.innerHTML='＋ เพิ่ม task';ab.onclick=()=>openAddTask(sDOW);card.appendChild(ab);el.appendChild(card);}
function mkRow(item,idx,isDone,dow,dateKey,tot,col){const row=document.createElement('div');row.className='trow'+(isDone?' done':'');row.innerHTML='<div class="tchk" style="'+(isDone?'background:'+col.a+';border-color:'+col.a+';color:white':'')+'">'+(isDone?'✓':'')+'</div><span class="ttime">'+item.t+'</span><span class="ttext">'+item.x+'</span><span class="tedit" onclick="openEditTask(event,'+dow+','+idx+')">✎</span>';let pt;row.addEventListener('touchstart',()=>{pt=setTimeout(()=>openEditTask(null,dow,idx),500);},{passive:true});row.addEventListener('touchend',()=>clearTimeout(pt));row.addEventListener('touchmove',()=>clearTimeout(pt));row.addEventListener('click',e=>{if(e.target.classList.contains('tedit'))return;const s=S.get(dateKey)||{};s[idx]=!s[idx];S.set(dateKey,s);const on=s[idx];row.classList.toggle('done',on);row.classList.add('jc');setTimeout(()=>row.classList.remove('jc'),350);const ch=row.querySelector('.tchk');ch.style.background=on?col.a:'';ch.style.borderColor=on?col.a:'';ch.style.color=on?'white':'';ch.textContent=on?'✓':'';const nd=Object.values(S.get(dateKey)||{}).filter(Boolean).length;const pp=document.getElementById('cP'),bb=document.getElementById('cB');if(pp){pp.textContent=nd+'/'+tot;pp.className='c-prog'+(nd===tot?' done':'');}if(bb)bb.style.width=(nd/tot*100)+'%';});return row;}
function renderBty(el){const st=S.get(sDK+'_b')||{};const card=document.createElement('div');card.className='bty-card';card.innerHTML='<h2>⭐ Beauty Day</h2><p>ไม่มีงานแม้แต่ชิ้นเดียว · กดติ๊กสิ่งที่ทำ!</p><div class="bty-pills" id="bp"></div>';el.appendChild(card);DR.b.forEach((item,i)=>{const p=document.createElement('div');p.className='bty-pill'+(st[i]?' on':'');p.textContent=item;p.onclick=()=>{const s=S.get(sDK+'_b')||{};s[i]=!s[i];S.set(sDK+'_b',s);p.classList.toggle('on',s[i]);};card.querySelector('#bp').appendChild(p);});}
// CALENDAR
function renderCal(){const el=document.getElementById('calContent');el.innerHTML='';mkMonth(el,2026,4,23,30,true);mkMonth(el,2026,5,1,31,false);}
function calTags(dow,beauty){if(beauty)return [{c:'ct-pink',t:'⭐ Beauty Day'}];const m={0:[{c:'ct-amber',t:'🎬 Content'},{c:'ct-sage',t:'📖 เรียน'}],1:[{c:'ct-coral',t:'💉 สักคิ้ว'},{c:'ct-coral',t:'🔴 Live'}],2:[{c:'ct-coral',t:'💉 สักคิ้ว'},{c:'ct-sage',t:'🕶️ แว่น'}],3:[{c:'ct-blue',t:'🏠 อสังหา'},{c:'ct-sage',t:'📖 เรียน'}],4:[{c:'ct-coral',t:'💉 สักคิ้ว'},{c:'ct-coral',t:'🔴 Live'}],5:[{c:'ct-coral',t:'💉 สักคิ้ว'},{c:'ct-pink',t:'❤️ แฟน'}],6:[{c:'ct-coral',t:'💉 สักคิ้ว'},{c:'ct-amber',t:'🎬 Content'}]};return m[dow]||[];}
function mkMonth(container,yr,mo,sd,ed,faded){const blk=document.createElement('div');blk.className='month-block';blk.innerHTML='<div class="month-hdr"><span class="month-name">'+MFULL[mo-1]+'</span><span class="month-yr">พ.ศ. '+(yr+543)+'</span></div>';const dr=document.createElement('div');dr.className='dow-row';DOW_S.forEach((d,i)=>{const c=document.createElement('div');c.className='dow-cell'+(i>=5?' wk':'');c.textContent=d;dr.appendChild(c);});blk.appendChild(dr);const grid=document.createElement('div');grid.className='cal-grid';const fd=new Date(yr,mo-1,sd);const fdow=(fd.getDay()+6)%7;for(let i=0;i<fdow;i++){const e=document.createElement('div');e.className='cc emp';grid.appendChild(e);}for(let d=sd;d<=ed;d++){const date=new Date(yr,mo-1,d);const dow=(date.getDay()+6)%7;const beauty=isB(date);const wkd=dow>=5;const tod=dk(date)===dk(NOW);const cell=document.createElement('div');let cls='cc';if(beauty)cls+=' bty';else if(wkd)cls+=' wkd';if(tod)cls+=' tod';if(faded)cls+=' fad';cell.className=cls;const tags=calTags(dow,beauty);cell.innerHTML='<div class="cn">'+d+'</div><div class="ctags">'+tags.slice(0,2).map(t=>'<div class="ctag '+t.c+'">'+t.t+'</div>').join('')+'</div>';cell.onclick=()=>{
  sDOW=dow; sDK=dk(date);
  // Rebuild pills centered on the week containing clicked date
  renderPillsForDate(date);
  sv('today',document.querySelectorAll('.tab-item')[0]);
  renderToday();
};grid.appendChild(cell);}const tot=fdow+(ed-sd+1);const rem=tot%7;if(rem>0)for(let i=0;i<7-rem;i++){const e=document.createElement('div');e.className='cc emp';grid.appendChild(e);}blk.appendChild(grid);container.appendChild(blk);}
// HABITS
function renderHab(){const grid=document.getElementById('habGrid');grid.innerHTML='';const habits=getH();const stored=S.get('hab')||{};const hdr=document.createElement('div');hdr.className='hab-hdr';hdr.innerHTML='<div class="hab-hdr-lbl"></div><div class="hab-hdr-days">'+DOW_S.map(d=>'<div class="hab-hdr-d">'+d+'</div>').join('')+'</div>';grid.appendChild(hdr);habits.forEach((h,hi)=>{const row=document.createElement('div');row.className='hab-row';row.innerHTML='<div class="hab-lbl">'+h.e+' '+h.l+'</div><div class="hab-days" id="hd'+hi+'"></div><span class="edit-row-btn" onclick="openEditHabit('+hi+')">✎</span>';grid.appendChild(row);const days=row.querySelector('#hd'+hi);for(let di=0;di<7;di++){const key=hi+'_'+di,on=!!stored[key];const dot=document.createElement('div');dot.className='hdot'+(on?' on':'');dot.style.background=on?h.c:'';dot.textContent=on?'✓':'';dot.onclick=()=>{const s=S.get('hab')||{};s[key]=!s[key];S.set('hab',s);dot.classList.toggle('on',s[key]);dot.style.background=s[key]?h.c:'';dot.textContent=s[key]?'✓':'';};days.appendChild(dot);}});const ab=document.createElement('div');ab.className='add-btn';ab.innerHTML='＋ เพิ่ม habit';ab.onclick=openAddHabit;grid.appendChild(ab);}
function resetHab(){if(confirm('Reset Habit Tracker?')){S.set('hab',{});renderHab();}}
// GOALS
function renderGoals(){const el=document.getElementById('goalsList');el.innerHTML='';getG().forEach((g,i)=>{const card=document.createElement('div');card.className='goal-item';card.style.background=g.bg;card.style.borderColor=g.ac+'33';card.innerHTML='<div class="g-emoji">'+g.e+'</div><div style="flex:1"><div class="g-title" style="color:'+g.ac+'">'+g.title+'</div><div class="g-text">'+g.text.replace(/\n/g,'<br>')+'</div></div><span class="g-edit" onclick="openEditGoal('+i+')">✎</span>';el.appendChild(card);});}
// MODAL
let MS=null;
function openSheet(html){document.getElementById('sheet').innerHTML=html;document.getElementById('overlay').classList.add('open');}
function closeSheet(){document.getElementById('overlay').classList.remove('open');MS=null;}
function closeBg(e){if(e.target===document.getElementById('overlay'))closeSheet();}
function cpicker(cols,sel,id){return '<div class="cr" id="'+id+'">'+cols.map(c=>'<div class="co '+(c===sel?'sel':'')+'" style="background:'+c+'" onclick="pickC(this,\''+id+'\')"></div>').join('')+'</div>';}
function epicker(emjs,sel,id){return '<div class="er" id="'+id+'">'+emjs.map(e=>'<div class="eo '+(e===sel?'sel':'')+'" onclick="pickE(this,\''+id+'\')">'+e+'</div>').join('')+'</div>';}
function pickC(el,id){document.getElementById(id).querySelectorAll('.co').forEach(x=>x.classList.remove('sel'));el.classList.add('sel');}
function pickE(el,id){document.getElementById(id).querySelectorAll('.eo').forEach(x=>x.classList.remove('sel'));el.classList.add('sel');}
// TASK
function openEditTask(e,dow,idx){if(e)e.stopPropagation();const r=getRbyDate(sDK);MS={type:'task',dow,idx,mode:'edit',dk:sDK};openSheet('<div class="sheet-handle"></div><div class="sheet-title">แก้ไข Task</div><label class="fl">เวลา</label><input class="ft" id="eT" type="time" value="'+r[idx].t+'"><label class="fl">งาน</label><input class="fi" id="eX" value="'+r[idx].x+'"><div class="btn-row"><button class="btn btn-d" onclick="delTask()">🗑️</button><button class="btn btn-c" onclick="closeSheet()">ยกเลิก</button><button class="btn btn-s" onclick="saveTask()">บันทึก ✓</button></div>');setTimeout(()=>document.getElementById('eX').focus(),300);}
function openAddTask(dow){MS={type:'task',dow,idx:-1,mode:'add',dk:sDK};openSheet('<div class="sheet-handle"></div><div class="sheet-title">เพิ่ม Task ใหม่</div><label class="fl">เวลา</label><input class="ft" id="eT" type="time" value="08:00"><label class="fl">งาน</label><input class="fi" id="eX" placeholder="ชื่อ task..."><div class="btn-row"><button class="btn btn-c" onclick="closeSheet()">ยกเลิก</button><button class="btn btn-s" onclick="saveTask()">เพิ่ม ✓</button></div>');setTimeout(()=>document.getElementById('eX').focus(),300);}
function saveTask(){const t=document.getElementById('eT').value,x=document.getElementById('eX').value.trim();if(!x)return;const r=getRbyDate(MS.dk||sDK);if(MS.mode==='edit')r[MS.idx]={t,x};else{r.push({t,x});r.sort((a,b)=>a.t.localeCompare(b.t));}setRbyDate(MS.dk||sDK,r);closeSheet();renderToday();}
function delTask(){if(!confirm('ลบ task นี้?'))return;const r=getRbyDate(MS.dk||sDK);r.splice(MS.idx,1);setRbyDate(MS.dk||sDK,r);closeSheet();renderToday();}
// HABIT
function openAddHabit(){MS={type:'habit',idx:-1,mode:'add'};openSheet('<div class="sheet-handle"></div><div class="sheet-title">เพิ่ม Habit</div><label class="fl">ชื่อ</label><input class="fi" id="hL" placeholder="เช่น ดื่มน้ำ 8 แก้ว"><label class="fl">Emoji</label>'+epicker(EMOH,'🎯','ep')+'<label class="fl">สี</label>'+cpicker(HCOLS,HCOLS[0],'cp')+'<div class="btn-row"><button class="btn btn-c" onclick="closeSheet()">ยกเลิก</button><button class="btn btn-s" onclick="saveHabit()">เพิ่ม ✓</button></div>');}
function openEditHabit(idx){const h=getH()[idx];MS={type:'habit',idx,mode:'edit'};openSheet('<div class="sheet-handle"></div><div class="sheet-title">แก้ไข Habit</div><label class="fl">ชื่อ</label><input class="fi" id="hL" value="'+h.l+'"><label class="fl">Emoji</label>'+epicker(EMOH,h.e,'ep')+'<label class="fl">สี</label>'+cpicker(HCOLS,h.c,'cp')+'<div class="btn-row"><button class="btn btn-d" onclick="delHabit()">🗑️</button><button class="btn btn-c" onclick="closeSheet()">ยกเลิก</button><button class="btn btn-s" onclick="saveHabit()">บันทึก ✓</button></div>');}
function saveHabit(){const l=document.getElementById('hL').value.trim();if(!l)return;const e=document.querySelector('#ep .eo.sel')?.textContent||'🎯';const c=document.querySelector('#cp .co.sel')?.style.background||HCOLS[0];const h=getH();if(MS.mode==='edit')h[MS.idx]={e,l,c};else h.push({e,l,c});setH(h);closeSheet();renderHab();}
function delHabit(){if(!confirm('ลบ habit นี้?'))return;const h=getH();h.splice(MS.idx,1);setH(h);closeSheet();renderHab();}
// GOAL
function openAddGoal(){MS={type:'goal',idx:-1,mode:'add'};openSheet('<div class="sheet-handle"></div><div class="sheet-title">เพิ่ม Goal</div><label class="fl">ชื่อ</label><input class="fi" id="gT" placeholder="เช่น FITNESS"><label class="fl">รายละเอียด</label><textarea class="fi" id="gX" rows="3" placeholder="เป้าหมาย..."></textarea><label class="fl">Emoji</label>'+epicker(EMOG,'⭐','gep')+'<label class="fl">สีพื้นหลัง</label>'+cpicker(GBGS,GBGS[0],'gbg')+'<label class="fl">สีข้อความ</label>'+cpicker(GACS,GACS[0],'gac')+'<div class="btn-row"><button class="btn btn-c" onclick="closeSheet()">ยกเลิก</button><button class="btn btn-s" onclick="saveGoal()">เพิ่ม ✓</button></div>');}
function openEditGoal(idx){const g=getG()[idx];MS={type:'goal',idx,mode:'edit'};openSheet('<div class="sheet-handle"></div><div class="sheet-title">แก้ไข Goal</div><label class="fl">ชื่อ</label><input class="fi" id="gT" value="'+g.title+'"><label class="fl">รายละเอียด</label><textarea class="fi" id="gX" rows="3">'+g.text+'</textarea><label class="fl">Emoji</label>'+epicker(EMOG,g.e,'gep')+'<label class="fl">สีพื้นหลัง</label>'+cpicker(GBGS,g.bg,'gbg')+'<label class="fl">สีข้อความ</label>'+cpicker(GACS,g.ac,'gac')+'<div class="btn-row"><button class="btn btn-d" onclick="delGoal()">🗑️</button><button class="btn btn-c" onclick="closeSheet()">ยกเลิก</button><button class="btn btn-s" onclick="saveGoal()">บันทึก ✓</button></div>');}
function saveGoal(){const title=document.getElementById('gT').value.trim();if(!title)return;const text=document.getElementById('gX').value.trim();const e=document.querySelector('#gep .eo.sel')?.textContent||'⭐';const bg=document.querySelector('#gbg .co.sel')?.style.background||GBGS[0];const ac=document.querySelector('#gac .co.sel')?.style.background||GACS[0];const g=getG();if(MS.mode==='edit')g[MS.idx]={e,title,text,bg,ac};else g.push({e,title,text,bg,ac});setG(g);closeSheet();renderGoals();}
function delGoal(){if(!confirm('ลบ goal นี้?'))return;const g=getG();g.splice(MS.idx,1);setG(g);closeSheet();renderGoals();}
// VIEW
function sv(name,tabEl){document.querySelectorAll('.view').forEach(v=>v.classList.remove('active'));document.querySelectorAll('.tab-item').forEach(t=>t.classList.remove('active'));document.getElementById('v-'+name).classList.add('active');if(tabEl)tabEl.classList.add('active');if(name==='cal')renderCal();if(name==='hab')renderHab();if(name==='goals')renderGoals();}

// ===================== PRELOADED DAILY TASKS =====================
const PRELOAD_TASKS = {
  "2026-4-22": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "18:00", "x": "🕶️ Gales Haus: ถ่ายแว่น + Ads"}, {"t": "19:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "20:30", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-4-23": [{"t": "09:00", "x": "ตื่นสาย ไม่ต้องรีบ"}, {"t": "10:30", "x": "💆 นวด / สปา / ทำผม / ทำเล็บ"}, {"t": "13:00", "x": "☕ กินของอร่อย / Cafe"}, {"t": "15:00", "x": "🛍️ Shopping / เดินเล่น / Netflix"}, {"t": "18:00", "x": "🌙 กินข้าวเย็นตามใจ"}, {"t": "20:00", "x": "😌 พักผ่อน / Wind down"}, {"t": "22:30", "x": "😴 นอน (นอนเร็วได้เลย)"}],
  "2026-4-24": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "17:30", "x": "🕶️ Gales Haus: Ads + Insight"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📝 Plan Content สัปดาห์หน้า"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-4-25": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "😌 พักผ่อน"}, {"t": "19:00", "x": "❤️ เวลาแฟน / ออกไปกิน"}, {"t": "21:00", "x": "📚 อ่านหนังสือ / Netflix"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-4-26": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🌙 กินข้าวเย็น"}, {"t": "18:00", "x": "✂️ Content: ตัด + Schedule"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:00", "x": "❤️ เวลาแฟน"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-4-27": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📝 Plan + Script คลิปทั้งสัปดาห์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "🎬 ถ่าย Batch 3–5 คลิป"}, {"t": "15:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "16:00", "x": "✂️ ตัดต่อ + Schedule โพสต์"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:30", "x": "📚 อ่านหนังสือ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-4-28": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1–1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป + โพสต์ IG"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📊 เช็ค Engage"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-4-29": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "18:00", "x": "🕶️ Gales Haus: ถ่ายแว่น + Ads"}, {"t": "19:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "20:30", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-4-30": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "14:00", "x": "🏠 อสังหา: อัพเพจ / ตอบ DM"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "🏠 อสังหา: Engage"}, {"t": "20:30", "x": "📚 อ่านหนังสือ"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-1": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "17:30", "x": "🕶️ Gales Haus: Ads + Insight"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📝 Plan Content สัปดาห์หน้า"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-2": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "😌 พักผ่อน"}, {"t": "19:00", "x": "❤️ เวลาแฟน / ออกไปกิน"}, {"t": "21:00", "x": "📚 อ่านหนังสือ / Netflix"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-3": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🌙 กินข้าวเย็น"}, {"t": "18:00", "x": "✂️ Content: ตัด + Schedule"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:00", "x": "❤️ เวลาแฟน"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-4": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📝 Plan + Script คลิปทั้งสัปดาห์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "🎬 ถ่าย Batch 3–5 คลิป"}, {"t": "15:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "16:00", "x": "✂️ ตัดต่อ + Schedule โพสต์"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:30", "x": "📚 อ่านหนังสือ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-5": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1–1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป + โพสต์ IG"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📊 เช็ค Engage"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-6": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "18:00", "x": "🕶️ Gales Haus: ถ่ายแว่น + Ads"}, {"t": "19:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "20:30", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-7": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "14:00", "x": "🏠 อสังหา: อัพเพจ / ตอบ DM"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "🏠 อสังหา: Engage"}, {"t": "20:30", "x": "📚 อ่านหนังสือ"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-8": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "17:30", "x": "🕶️ Gales Haus: Ads + Insight"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📝 Plan Content สัปดาห์หน้า"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-9": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "😌 พักผ่อน"}, {"t": "19:00", "x": "🏠 กลับบ้าน / กินข้าวกับครอบครัว"}, {"t": "21:00", "x": "📚 อ่านหนังสือ / Netflix"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-10": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🌙 กินข้าวเย็น"}, {"t": "18:00", "x": "✂️ Content: ตัด + Schedule"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:00", "x": "❤️ เวลาแฟน"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-11": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📝 Plan + Script คลิปทั้งสัปดาห์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "🎬 ถ่าย Batch 3–5 คลิป"}, {"t": "15:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "16:00", "x": "✂️ ตัดต่อ + Schedule โพสต์"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:30", "x": "📚 อ่านหนังสือ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-12": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1–1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป + โพสต์ IG"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📊 เช็ค Engage"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-13": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "18:00", "x": "🕶️ Gales Haus: ถ่ายแว่น + Ads"}, {"t": "19:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "20:30", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-14": [{"t": "09:00", "x": "ตื่นสาย ไม่ต้องรีบ"}, {"t": "10:30", "x": "💆 นวด / สปา / ทำผม / ทำเล็บ"}, {"t": "13:00", "x": "☕ กินของอร่อย / Cafe"}, {"t": "15:00", "x": "🛍️ Shopping / เดินเล่น / Netflix"}, {"t": "18:00", "x": "🌙 กินข้าวเย็นตามใจ"}, {"t": "20:00", "x": "😌 พักผ่อน / Wind down"}, {"t": "22:30", "x": "😴 นอน (นอนเร็วได้เลย)"}],
  "2026-5-15": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "17:30", "x": "🕶️ Gales Haus: Ads + Insight"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📝 Plan Content สัปดาห์หน้า"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-16": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "😌 พักผ่อน"}, {"t": "19:00", "x": "❤️ เวลาแฟน / ออกไปกิน"}, {"t": "21:00", "x": "📚 อ่านหนังสือ / Netflix"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-17": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🌙 กินข้าวเย็น"}, {"t": "18:00", "x": "✂️ Content: ตัด + Schedule"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:00", "x": "❤️ เวลาแฟน"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-18": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📝 Plan + Script คลิปทั้งสัปดาห์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "🎬 ถ่าย Batch 3–5 คลิป"}, {"t": "15:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "16:00", "x": "✂️ ตัดต่อ + Schedule โพสต์"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:30", "x": "📚 อ่านหนังสือ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-19": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1–1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป + โพสต์ IG"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📊 เช็ค Engage"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-20": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "18:00", "x": "🕶️ Gales Haus: ถ่ายแว่น + Ads"}, {"t": "19:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "20:30", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-21": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "14:00", "x": "🏠 อสังหา: อัพเพจ / ตอบ DM"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "🏠 อสังหา: Engage"}, {"t": "20:30", "x": "📚 อ่านหนังสือ"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-22": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "17:30", "x": "🕶️ Gales Haus: Ads + Insight"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📝 Plan Content สัปดาห์หน้า"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-23": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "😌 พักผ่อน"}, {"t": "19:00", "x": "🏠 กลับบ้าน / กินข้าวกับครอบครัว"}, {"t": "21:00", "x": "📚 อ่านหนังสือ / Netflix"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-24": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🌙 กินข้าวเย็น"}, {"t": "18:00", "x": "✂️ Content: ตัด + Schedule"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:00", "x": "❤️ เวลาแฟน"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-25": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📝 Plan + Script คลิปทั้งสัปดาห์"}, {"t": "11:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "12:00", "x": "🍽️ พักกินข้าว"}, {"t": "13:00", "x": "🎬 ถ่าย Batch 3–5 คลิป"}, {"t": "15:00", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "16:00", "x": "✂️ ตัดต่อ + Schedule โพสต์"}, {"t": "17:30", "x": "🌙 กินข้าวเย็น / พัก"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:30", "x": "📚 อ่านหนังสือ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-26": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1–1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป + โพสต์ IG"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📊 เช็ค Engage"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-27": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "18:00", "x": "🕶️ Gales Haus: ถ่ายแว่น + Ads"}, {"t": "19:00", "x": "🏠 อสังหา: หาทรัพย์"}, {"t": "20:30", "x": "📘 คอร์สธุรกิจ 1 ชม."}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-28": [{"t": "09:00", "x": "ตื่นสาย ไม่ต้องรีบ"}, {"t": "10:30", "x": "💆 นวด / สปา / ทำผม / ทำเล็บ"}, {"t": "13:00", "x": "☕ กินของอร่อย / Cafe"}, {"t": "15:00", "x": "🛍️ Shopping / เดินเล่น / Netflix"}, {"t": "18:00", "x": "🌙 กินข้าวเย็นตามใจ"}, {"t": "20:00", "x": "😌 พักผ่อน / Wind down"}, {"t": "22:30", "x": "😴 นอน (นอนเร็วได้เลย)"}],
  "2026-5-29": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย 1.5 ชม."}, {"t": "10:00", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🖼️ แต่งรูป Babebrow + โพสต์"}, {"t": "17:30", "x": "🕶️ Gales Haus: Ads + Insight"}, {"t": "18:30", "x": "⚡ เตรียม Live 30 นาที"}, {"t": "19:00", "x": "🔴 Live IG/TikTok 1 ชม."}, {"t": "20:30", "x": "📝 Plan Content สัปดาห์หน้า"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-30": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "😌 พักผ่อน"}, {"t": "19:00", "x": "❤️ เวลาแฟน / ออกไปกิน"}, {"t": "21:00", "x": "📚 อ่านหนังสือ / Netflix"}, {"t": "23:00", "x": "😴 นอน"}],
  "2026-5-31": [{"t": "08:00", "x": "ตื่น + Morning routine"}, {"t": "08:30", "x": "🏋️ ออกกำลังกาย (เบา)"}, {"t": "09:30", "x": "📚 อ่านหนังสือเบาๆ"}, {"t": "11:00", "x": "💉 สักคิ้ว ลูกค้า (11–17)"}, {"t": "17:00", "x": "🌙 กินข้าวเย็น"}, {"t": "18:00", "x": "✂️ Content: ตัด + Schedule"}, {"t": "19:00", "x": "📖 เรียนภาษา 1 ชม."}, {"t": "20:00", "x": "❤️ เวลาแฟน"}, {"t": "23:00", "x": "😴 นอน"}],
};

function getR(d) {
  const custom = S.get('r' + d);
  if (custom) return custom;
  // Find date key for this weekday in current week
  const w = ws();
  const dateObj = new Date(w);
  dateObj.setDate(w.getDate() + d);
  const key = dk(dateObj);
  if (PRELOAD_TASKS[key]) return JSON.parse(JSON.stringify(PRELOAD_TASKS[key]));
  return JSON.parse(JSON.stringify(DR[d]));
}

function getRbyDate(dateKey) {
  const custom = S.get('rd_' + dateKey);
  if (custom) return custom;
  if (PRELOAD_TASKS[dateKey]) return JSON.parse(JSON.stringify(PRELOAD_TASKS[dateKey]));
  // fallback by weekday
  const parts = dateKey.split('-');
  const d = new Date(+parts[0], +parts[1]-1, +parts[2]);
  return JSON.parse(JSON.stringify(DR[d.getDay() === 0 ? 6 : d.getDay()-1]));
}

function setRbyDate(dateKey, arr) {
  S.set('rd_' + dateKey, arr);
}


// Fix scroll padding dynamically for iOS
function fixScrollPadding(){
  const hdr = document.querySelector('.hdr');
  const tab = document.querySelector('.tab-bar');
  const main = document.querySelector('.main');
  if(hdr && tab && main){
    const hh = hdr.getBoundingClientRect().height;
    const th = tab.getBoundingClientRect().height;
    main.style.paddingTop = hh + 'px';
    main.style.paddingBottom = th + 'px';
  }
}
window.addEventListener('load', fixScrollPadding);
window.addEventListener('resize', fixScrollPadding);

// INIT
initHdr();renderPills();renderToday();
</script>
</body>
</html>
