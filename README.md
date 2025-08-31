<!doctype html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>طريقك يخصنا - القاهرة</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.css" />
  <link rel="stylesheet" href="https://unpkg.com/leaflet.markercluster@1.5.3/dist/MarkerCluster.Default.css" />
  <style>
:root{--primary:#2b59c3;--primary-dark:#1e3e8a;--bg-top:#f0f8ff;--bg-bottom:#d6e6ff;--card:#fff;--muted:#6b7280;--danger:#e63946;--success:#2ecc71;--warning:#f39c12;--shadow:0 6px 18px rgba(0,0,0,0.08);--radius:14px}
*{box-sizing:border-box}body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Arial;background:linear-gradient(180deg,var(--bg-top),var(--bg-bottom));color:#111827;min-height:100vh}
header{background:linear-gradient(135deg,var(--primary),#5a8dee);color:#fff;padding:18px}
.wrap{max-width:1100px;margin:0 auto;display:flex;align-items:center;justify-content:space-between}
.brand h1{margin:0;font-size:20px}
.container{max-width:1100px;margin:18px auto;padding:12px}
.card{background:var(--card);border-radius:12px;box-shadow:var(--shadow);padding:16px;margin-bottom:12px}
.row{display:grid;grid-template-columns:1fr 360px;gap:14px}
@media (max-width:900px){.row{grid-template-columns:1fr}}
.field input,.field select{width:100%;padding:10px;border-radius:10px;border:1px solid #e5e7eb}
.controls{display:flex;gap:8px;align-items:center;flex-wrap:wrap}
.btn{padding:10px 14px;border-radius:10px;border:none;cursor:pointer;font-weight:700;transition:all 0.2s}
.btn-primary{background:var(--primary);color:#fff}
.btn-light{background:#eef2ff;color:#1e3a8a}
.btn-danger{background:var(--danger);color:#fff}
.btn-success{background:var(--success);color:#fff}
.btn-warning{background:var(--warning);color:#fff}
#map{height:520px;border-radius:10px;border:1px solid #e5e7eb}
.list{max-height:520px;overflow:auto;border:1px solid #e5e7eb;border-radius:10px;padding:8px}
.item{padding:8px;border-bottom:1px dashed #eee;display:flex;justify-content:space-between;align-items:center;cursor:pointer}
.item.highlight{background:#eef2ff}
.modal{position:fixed;inset:0;background:rgba(0,0,0,0.45);display:none;align-items:center;justify-content:center;padding:18px;z-index:1000}
.modal .panel{width:100%;max-width:1100px;background:#fff;border-radius:12px;overflow:hidden}
.modal .header{display:flex;justify-content:space-between;align-items:center;padding:12px;border-bottom:1px solid #f0f0f0}
.modal .body{height:640px}
#bigMapInner{width:100%;height:100%;border:0}
.badge{background:#eef2ff;color:#0b3a8a;padding:6px 10px;border-radius:999px}
.small{font-size:13px;color:var(--muted)}
.filter-pill{background:#f7f9ff;padding:8px;border-radius:999px;border:1px solid #e8efff}
.section-title{display:flex;justify-content:space-between;align-items:center}
.kv{font-size:13px;color:#666}
.counter{font-size:14px;color:#333;margin-left:8px;font-weight:bold;}
.center{text-align:center}
footer{text-align:center;padding:12px;color:var(--muted)}
.traffic-legend{display:flex;gap:10px;margin-top:10px;flex-wrap:wrap}
.traffic-item{display:flex;align-items:center;gap:5px;font-size:13px}
.traffic-color{width:16px;height:16px;border-radius:50%}
.traffic-low{background:var(--success)}
.traffic-medium{background:var(--warning)}
.traffic-high{background:var(--danger)}
.tab-controls{display:flex;background:#f0f4ff;border-radius:10px;padding:4px;margin-bottom:12px}
.tab{flex:1;text-align:center;padding:8px;cursor:pointer;border-radius:8px}
.tab.active{background:var(--primary);color:white}
.loading{text-align:center;padding:20px;color:var(--muted)}
.gov-filter{display:flex;gap:8px;margin-top:10px;flex-wrap:wrap}
.gov-filter button{font-size:12px;padding:6px 10px}
.auth-error{color:var(--danger);margin-top:8px;font-size:14px}
  </style>
</head>
<body>
<header>
  <div class="wrap">
    <div class="brand">
      <h1 id="appTitle">طريقك يخصنا - القاهرة</h1>
      <div class="small">لوحة تحكم للخدمات والخريطة — مع بيانات الزحمة والمصالح الحكومية</div>
    </div>
    <div>
      <span class="badge" id="sessionStatus">خارج النظام</span>
    </div>
  </div>
</header>

<!-- Login Card -->
<div id="authCard" class="container">
  <h2>تسجيل الدخول</h2>
  <div class="field"><input id="username" type="text" placeholder="الإيميل" value="mohamed@gmail.com" /></div>
  <div class="field"><input id="password" type="password" placeholder="كلمة السر" value="password123" /></div>
  <button id="loginBtn" class="btn btn-primary">دخول</button>
  <div id="authMsg" class="auth-error"></div>
  <div class="small" style="margin-top: 12px;">
    <p>لتجربة النظام، يمكنك استخدام:</p>
    <p>🔹 الإيميل: mohamed@gmail.com | كلمة السر: password123 (صلاحيات مدير)</p>
    <p>🔹 الإيميل: user@example.com | كلمة السر: user123 (صلاحيات مستخدم عادي)</p>
  </div>
</div>

<!-- Main UI -->
<div id="mainUI" class="container" style="display:none">
    <!-- باقي المحتوى كما هو -->
    <div class="card">
      <div style="display:flex;gap:10px;align-items:center;justify-content:space-between;flex-wrap:wrap">
        <div style="display:flex;gap:8px;flex:1;flex-wrap:wrap">
          <div style="flex:1;min-width:200px" class="field"><input id="searchBox" placeholder="ابحث بالاسم أو الحي…" /></div>
          <div style="width:160px" class="field"><select id="typeFilter"><option value="">كل الأنواع</option><option value="hospital">مستشفى</option><option value="pharmacy">صيدلية</option><option value="gov">مصلحة حكومية</option><option value="clinic">عيادة</option></select></div>
          <div style="width:140px" class="field"><select id="districtFilter"><option value="">كل الاحياء</option></select></div>
        </div>
        <div class="controls">
          <span class="counter" id="placesCount">0</span>
          <button class="btn btn-light" id="locateBtn">📍 موقعي</button>
          <button class="btn btn-primary" id="openBigMapBtn">🗺️ الخريطة الكبيرة</button>
          <button class="btn btn-warning" id="trafficToggleBtn">🚦 تفعيل الزحمة</button>
          <button class="btn btn-danger" id="logoutBtn">تسجيل الخروج</button>
        </div>
      </div>
      
      <!-- Traffic Legend -->
      <div class="traffic-legend">
        <div class="traffic-item"><div class="traffic-color traffic-low"></div> زحمة خفيفة</div>
        <div class="traffic-item"><div class="traffic-color traffic-medium"></div> زحمة متوسطة</div>
        <div class="traffic-item"><div class="traffic-color traffic-high"></div> زحمة شديدة</div>
      </div>
    </div>

    <div class="tab-controls">
      <div class="tab active" data-tab="services">الخدمات العامة</div>
      <div class="tab" data-tab="government">المصالح الحكومية</div>
    </div>

    <div class="row">
      <div class="card no-pad">
        <div id="map"></div>
      </div>

      <div id="servicesTab">
        <div class="card list-card">
          <div class="section-title"><h4 id="nearbyTitle">الخدمات القريبة</h4><div class="small">اضغط على أي مكان للتركيز</div></div>
          <div id="placesList" class="list"></div>
        </div>
        <div class="card">
          <h4>أفضل المستشفيات</h4>
          <div id="bestList" class="list" style="max-height:160px"></div>
        </div>
      </div>

      <div id="governmentTab" style="display:none">
        <div class="card">
          <h4>المصالح الحكومية في القاهرة</h4>
          <div class="gov-filter">
            <button class="btn btn-light active" data-gov-type="all">الكل</button>
            <button class="btn btn-light" data-gov-type="ministry">الوزارات</button>
            <button class="btn btn-light" data-gov-type="municipality">المحليات</button>
            <button class="btn btn-light" data-gov-type="security">الأمن</button>
            <button class="btn btn-light" data-gov-type="education">التعليم</button>
          </div>
          <div id="governmentList" class="list" style="max-height:480px"></div>
        </div>
      </div>
    </div>

    <div class="card" id="adminPanel" style="display:none">
      <h4>لوحة الإدارة</h4>
      <div class="field"><input id="newName" placeholder="اسم المكان" /></div>
      <div class="field"><select id="newType"><option value="hospital">مستشفى</option><option value="pharmacy">صيدلية</option><option value="gov">مصلحة حكومية</option><option value="clinic">عيادة</option></select></div>
      <div class="field"><input id="newDistrict" placeholder="الحي / المنطقة (مثال: الزمالك)" /></div>
      <div style="display:flex;gap:8px;flex-wrap:wrap"><input id="newLat" placeholder="خط العرض" class="field" style="flex:1" /><input id="newLng" placeholder="خط الطول" class="field" style="flex:1" /></div>
      <div class="controls" style="margin-top:8px"><button class="btn btn-primary" id="addPlaceBtn">➕ إضافة</button><button class="btn btn-light" id="savePlacesBtn">💾 حفظ</button><button class="btn btn-danger" id="clearAllBtn">🗑️ مسح الكل</button></div>
      <div id="adminMsg" class="small" style="margin-top:8px"></div>
    </div>

    <div class="small center" style="margin-top:10px"> — القاهرة. التعديلات تحفظ محليًا في المتصفح.</div>
</div>

<footer class="small" style="margin-top:12px">©️  طريقك يخصنا — القاهرة </footer>

<!-- Big Map Modal -->
<div class="modal" id="bigMapModal">
  <div class="panel">
    <div class="header">
      <div style="display:flex;gap:8px;align-items:center;width:70%;flex-wrap:wrap">
        <input id="bigMapSearch" placeholder="ابحث عن العنوان أو المكان…" style="flex:1;padding:10px;border-radius:8px;border:1px solid #eee;min-width:200px" />
        <button class="btn btn-primary" id="bigMapSearchBtn">بحث</button>
        <button class="btn btn-warning" id="bigMapTrafficToggle">تشغيل/إيقاف الزحمة</button>
      </div>
      <div style="display:flex;gap:8px;align-items:center">
        <button class="btn btn-light" id="bigMapClose">إغلاق</button>
      </div>
    </div>
    <div class="body">
      <div id="bigMapInner" style="width:100%;height:100%"></div>
    </div>
  </div>
</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
<script src="https://unpkg.com/leaflet.markercluster@1.5.3/dist/leaflet.markercluster.js"></script>
<script>
// بيانات المستخدمين المسجلين
const users = [
  { email: "mohamed@gmail.com", password: "password123", isAdmin: true },
  { email: "user@example.com", password: "user123", isAdmin: false }
];

const ADMIN_EMAIL = "mohamed@gmail.com";
let places = [
  {name:"مستشفى القصر العيني", type:"hospital", district:"المعادي/وسط البلد", lat:30.0459, lng:31.2336, rank:1},
  {name:"صيدلية التحرير", type:"pharmacy", district:"وسط البلد", lat:30.0444, lng:31.2357},
  {name:"مصلحة الجوازات - القاهرة", type:"gov", district:"وسط البلد", lat:30.0469, lng:31.2351},
  {name:"عيادة الحياة — الزمالك", type:"clinic", district:"الزمالك", lat:30.0610, lng:31.2200}
];

// بيانات المصالح الحكومية في القاهرة
const governmentPlaces = [
  {name:"وزارة الداخلية", type:"ministry", district:"وسط البلد", lat:30.0444, lng:31.2357, details:"الوزارة المسؤولة عن الأمن الداخلي"},
  {name:"محافظة القاهرة", type:"municipality", district:"وسط البلد", lat:30.0440, lng:31.2350, details:"مبنى محافظة القاهرة"},
  {name:"وزارة التربية والتعليم", type:"ministry", district:"الزمالك", lat:30.0620, lng:31.2180, details:"الوزارة المسؤولة عن التعليم في مصر"},
  {name:"مديرية أمن القاهرة", type:"security", district:"العتبة", lat:30.0550, lng:31.2450, details:"مديرية الأمن المسؤولة عن العاصمة"},
  {name:"جامعة القاهرة", type:"education", district:"الجيزة", lat:30.0276, lng:31.2101, details:"أقدم الجامعات المصرية"},
  {name:"وزارة الصحة", type:"ministry", district:"الدقي", lat:30.0370, lng:31.2140, details:"الوزارة المسؤولة عن الصحة في مصر"},
  {name:"مصلحة الضرائب", type:"gov", district:"المعادي", lat:29.9665, lng:31.2498, details:"الهيئة العامة للضرائب"},
  {name:"وزارة العدل", type:"ministry", district:"الزمالك", lat:30.0600, lng:31.2190, details:"الوزارة المسؤولة عن الشؤون القضائية"},
  {name:"محكمة استئناف القاهرة", type:"gov", district:"باب الخلق", lat:30.0460, lng:31.2500, details:"أحد أهم المحاكم الاستئنافية"},
  {name:"جهاز التعبئة العامة والإحصاء", type:"gov", district:"التحرير", lat:30.0470, lng:31.2330, details:"الجهاز المركزي للتعبئة العامة والإحصاء"},
  {name:"وزارة الإسكان", type:"ministry", district:"مدينة نصر", lat:30.0500, lng:31.3300, details:"الوزارة المسؤولة عن الإسكان والمرافق"},
  {name:"الهيئة العامة للاستثمار", type:"gov", district:"مصر الجديدة", lat:30.0900, lng:31.3300, details:"الهيئة العامة للاستثمار والمناطق الحرة"},
  {name:"وزارة السياحة", type:"ministry", district:"الجيزة", lat:30.0200, lng:31.2100, details:"الوزارة المسؤولة عن السياحة والآثار"},
  {name:"جهاز مدينة القاهرة الجديدة", type:"municipality", district:"القاهرة الجديدة", lat:30.0300, lng:31.4700, details:"جهاز التخطيط والعمران لمدينة القاهرة الجديدة"},
  {name:"محكمة شمال القاهرة", type:"gov", district:"شبرا", lat:30.0800, lng:31.2400, details:"محكمة شمال القاهرة الابتدائية"},
  {name:"وزارة النقل", type:"ministry", district:"الفسطاط", lat:30.0100, lng:31.2300, details:"الوزارة المسؤولة عن النقل والمواصلات"},
  {name:"جهاز الخدمة الوطنية", type:"security", district:"الهايكستب", lat:30.0900, lng:31.3400, details:"جهاز مشروعات الخدمة الوطنية للقوات المسلحة"},
  {name:"وزارة التضامن الاجتماعي", type:"ministry", district:"الزمالك", lat:30.0630, lng:31.2170, details:"الوزارة المسؤولة عن الشؤون الاجتماعية"},
  {name:"الهيئة القومية للأنفاق", type:"gov", district:"العباسية", lat:30.0670, lng:31.2770, details:"الهيئة القومية للأنفاق (المترو)"},
  {name:"محافظة الجيزة", type:"municipality", district:"الجيزة", lat:30.0131, lng:31.2089, details:"مبنى محافظة الجيزة"}
];

function savePlaces(){ localStorage.setItem('placesData_v2', JSON.stringify(places)); adminMsg('تم الحفظ محليًا'); updateCounter(); }
function loadPlaces(){ try{ const raw = localStorage.getItem('placesData_v2'); if(raw) return JSON.parse(raw);}catch(e){} return places; }
places = loadPlaces();

let map, markers=[], userMarker=null, bigMap = null, trafficLayer = null, trafficEnabled = false;
let govMarkers = [], markersCluster = null;

window.addEventListener('load', ()=>{
  document.getElementById('loginBtn').onclick = doLogin;
  document.getElementById('logoutBtn').onclick = doLogout;
  document.getElementById('addPlaceBtn').onclick = addPlaceFromAdmin;
  document.getElementById('savePlacesBtn').onclick = savePlaces;
  document.getElementById('clearAllBtn').onclick = ()=>{ if(confirm('مسح كل الأماكن؟')){ places=[]; savePlaces(); renderPlaces(); }};
  document.getElementById('searchBox').oninput = applyFilters;
  document.getElementById('typeFilter').onchange = applyFilters;
  document.getElementById('districtFilter').onchange = applyFilters;
  document.getElementById('locateBtn').onclick = locateMe;
  document.getElementById('openBigMapBtn').onclick = openBigMap;
  document.getElementById('bigMapClose').onclick = closeBigMap;
  document.getElementById('bigMapSearchBtn').onclick = searchBigMap;
  document.getElementById('trafficToggleBtn').onclick = toggleTraffic;

  // أحداث التبويبات
  document.querySelectorAll('.tab').forEach(tab => {
    tab.addEventListener('click', () => {
      const tabName = tab.getAttribute('data-tab');
      switchTab(tabName);
    });
  });

  // أحداث تصفية المصالح الحكومية
  document.querySelectorAll('.gov-filter button').forEach(btn => {
    btn.addEventListener('click', () => {
      document.querySelectorAll('.gov-filter button').forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      filterGovernmentPlaces(btn.getAttribute('data-gov-type'));
    });
  });

  // إدخال البيانات تلقائياً لتسهيل التجربة
  document.getElementById('username').value = 'mohamed@gmail.com';
  document.getElementById('password').value = 'password123';

  populateDistricts();
  renderAuthState();
});

function doLogin(){
  const u=document.getElementById('username').value.trim();
  const p=document.getElementById('password').value.trim();
  
  if(!u || !p){ 
    authMsg('من فضلك ادخل المستخدم وكلمة السر'); 
    return; 
  }
  
  // البحث عن المستخدم في قاعدة البيانات
  const user = users.find(user => user.email === u && user.password === p);
  
  if(user){
    authMsg('تم تسجيل الدخول بنجاح!', 'success');
    localStorage.setItem('ty_session','1');
    localStorage.setItem('ty_user', JSON.stringify(user));
    renderAuthState();
  } else {
    authMsg('بيانات الدخول غير صحيحة. حاول مرة أخرى.');
  }
}

function authMsg(msg, type = 'error') {
  const msgElement = document.getElementById('authMsg');
  msgElement.textContent = msg;
  msgElement.className = type === 'success' ? 'small' : 'auth-error';
  
  setTimeout(() => { 
    msgElement.textContent = ''; 
  }, 3000);
}

function doLogout(){ 
  if(confirm('هل تريد تسجيل الخروج؟')) {
    localStorage.setItem('ty_session','0'); 
    localStorage.removeItem('ty_user');
    renderAuthState();
    authMsg('تم تسجيل الخروج', 'success');
  }
}

function isSessionActive(){ 
  return localStorage.getItem('ty_session') === '1';
}

function getCurrentUser() {
  try {
    const userStr = localStorage.getItem('ty_user');
    return userStr ? JSON.parse(userStr) : null;
  } catch(e) {
    return null;
  }
}

function renderAuthState(){
  if(isSessionActive()){ 
    document.getElementById('authCard').style.display='none';
    document.getElementById('mainUI').style.display='block';
    document.getElementById('sessionStatus').textContent = 'متصل';
    document.getElementById('sessionStatus').style.background = '#e6f7e6';
    document.getElementById('sessionStatus').style.color = '#2e7d32';
    
    const currentUser = getCurrentUser();
    if(currentUser && currentUser.isAdmin) {
      document.getElementById('adminPanel').style.display = 'block';
    } else {
      document.getElementById('adminPanel').style.display = 'none';
    }
    
    initMap(); 
    renderPlaces(places);
    renderGovernmentPlaces();
    updateCounter();
  } else { 
    document.getElementById('authCard').style.display='block';
    document.getElementById('mainUI').style.display='none';
    document.getElementById('sessionStatus').textContent = 'خارج النظام';
    document.getElementById('sessionStatus').style.background = '#eef2ff';
    document.getElementById('sessionStatus').style.color = '#0b3a8a';
    if(map){ map.remove(); map=null; }
  }
}

// باقي الدوال تبقى كما هي بدون تغيير
function initMap(){ 
  if(map) return; 
  map = L.map('map').setView([30.05,31.23],12); 
  L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
    attribution:'&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
  }).addTo(map); 
  
  // إنشاء مجموعة للمجموعات العنقودية
  markersCluster = L.markerClusterGroup();
  map.addLayer(markersCluster);
  
  renderPlaces(places); 
}

function createDivIcon(type){ 
  const emoji = type==='hospital'? '🏥': type==='pharmacy'? '💊': type==='gov'? '🏛️': type==='clinic'? '🩺':'📍'; 
  return L.divIcon({
    className:'custom-divicon', 
    html:`<div style="background:rgba(255,255,255,0.8);border-radius:50%;padding:5px;box-shadow:0 2px 5px rgba(0,0,0,0.2)">${emoji}</div>`, 
    iconSize:[34,34],
    iconAnchor: [17, 17]
  }); 
}

function createGovIcon(type){ 
  const emoji = type==='ministry'? '🏢': type==='municipality'? '🏛️': type==='security'? '👮': type==='education'? '🎓':'🏢'; 
  return L.divIcon({
    className:'custom-divicon', 
    html:`<div style="background:rgba(255,255,255,0.9);border-radius:50%;padding:5px;box-shadow:0 2px 5px rgba(0,0,0,0.3)">${emoji}</div>`, 
    iconSize:[36,36],
    iconAnchor: [18, 18]
  }); 
}

function renderPlaces(list){
  if(!map) return; 
  markersCluster.clearLayers();
  markers = [];
  
  const container=document.getElementById('placesList'); 
  container.innerHTML='';
  
  const best=document.getElementById('bestList'); 
  best.innerHTML='';
  
  list.forEach((p,i)=>{
    const m = L.marker([p.lat,p.lng], {icon:createDivIcon(p.type)});
    m.bindPopup(`<b>${p.name}</b><div class="kv">${p.type} • ${p.district || ''}</div>`);
    m.on('click', ()=>{ m.openPopup(); }); 
    markersCluster.addLayer(m);
    markers.push(m);

    const div = document.createElement('div'); 
    div.className='item'; 
    div.innerHTML = `<div><strong>${p.name}</strong><div class="kv">${p.type} • ${p.district || ''}</div></div>`;
    div.onclick = ()=>{ map.setView([p.lat,p.lng],15); m.openPopup(); };
    container.appendChild(div);

    if(p.rank && p.rank<=3){ 
      const rdiv=document.createElement('div'); 
      rdiv.className='item'; 
      rdiv.innerHTML=`<div><strong>${p.name}</strong><div class="kv">${p.district}</div></div>`;
      rdiv.onclick = ()=>{ map.setView([p.lat,p.lng],15); m.openPopup(); };
      best.appendChild(rdiv); 
    }
  });
  updateCounter();
}

function renderGovernmentPlaces() {
  const container = document.getElementById('governmentList');
  container.innerHTML = '';
  
  governmentPlaces.forEach((place, i) => {
    const div = document.createElement('div');
    div.className = 'item';
    div.innerHTML = `
      <div>
        <strong>${place.name}</strong>
        <div class="kv">${place.district} • ${getGovTypeName(place.type)}</div>
        <div class="kv">${place.details}</div>
      </div>
    `;
    div.onclick = () => {
      map.setView([place.lat, place.lng], 15);
      // فتح popup إذا كان marker موجود
      const marker = govMarkers.find(m => {
        const latLng = m.getLatLng();
        return latLng.lat === place.lat && latLng.lng === place.lng;
      });
      if (marker) {
        marker.openPopup();
      }
    };
    container.appendChild(div);
  });
}

function getGovTypeName(type) {
  const types = {
    'ministry': 'وزارة',
    'municipality': 'محلية',
    'security': 'أمنية',
    'education': 'تعليمية',
    'gov': 'حكومية'
  };
  return types[type] || 'حكومية';
}

function addGovernmentMarkers() {
  // إزالة العلامات القديمة إذا كانت موجودة
  if (govMarkers.length > 0) {
    govMarkers.forEach(marker => map.removeLayer(marker));
    govMarkers = [];
  }
  
  // إضافة علامات جديدة للمصالح الحكومية
  governmentPlaces.forEach(place => {
    const marker = L.marker([place.lat, place.lng], {icon: createGovIcon(place.type)});
    marker.bindPopup(`
      <b>${place.name}</b>
      <div class="kv">${getGovTypeName(place.type)} • ${place.district}</div>
      <div class="kv">${place.details}</div>
    `);
    marker.addTo(map);
    govMarkers.push(marker);
  });
}

function removeGovernmentMarkers() {
  govMarkers.forEach(marker => map.removeLayer(marker));
  govMarkers = [];
}

function filterGovernmentPlaces(type) {
  const container = document.getElementById('governmentList');
  container.innerHTML = '';
  
  const filtered = type === 'all' 
    ? governmentPlaces 
    : governmentPlaces.filter(p => p.type === type);
  
  filtered.forEach((place, i) => {
    const div = document.createElement('div');
    div.className = 'item';
    div.innerHTML = `
      <div>
        <strong>${place.name}</strong>
        <div class="kv">${place.district} • ${getGovTypeName(place.type)}</div>
        <div class="kv">${place.details}</div>
      </div>
    `;
    div.onclick = () => {
      map.setView([place.lat, place.lng], 15);
    };
    container.appendChild(div);
  });
}

function updateCounter(){
  document.getElementById('placesCount').textContent = places.length;
}

function applyFilters(){
  const q=document.getElementById('searchBox').value.trim().toLowerCase();
  const type=document.getElementById('typeFilter').value;
  const district=document.getElementById('districtFilter').value;
  const filtered = places.filter(p=>{
    return (!q || p.name.toLowerCase().includes(q) || (p.district||'').toLowerCase().includes(q)) &&
           (!type || p.type===type) &&
           (!district || p.district===district);
  });
  renderPlaces(filtered);
  highlightFilter();
}

function highlightFilter(){
  const type=document.getElementById('typeFilter').value;
  const district=document.getElementById('districtFilter').value;
  document.querySelectorAll('.item').forEach(el=>el.classList.remove('highlight'));
  Array.from(document.getElementById('placesList').children).forEach(el=>{
    if(type && !el.innerHTML.includes(type)) return;
    if(district && !el.innerHTML.includes(district)) return;
    el.classList.add('highlight');
  });
}

function populateDistricts(){
  const sel=document.getElementById('districtFilter'); 
  // الحفاظ على الخيار الأول (كل الأحياء)
  const firstOption = sel.options[0];
  sel.innerHTML = '';
  sel.appendChild(firstOption);
  
  const set=new Set(); 
  places.forEach(p=>{ if(p.district) set.add(p.district); }); 
  Array.from(set).sort().forEach(d=>{ 
    const opt=document.createElement('option'); 
    opt.value=d; 
    opt.textContent=d; 
    sel.appendChild(opt); 
  }); 
}

function addPlaceFromAdmin(){
  const name=document.getElementById('newName').value.trim();
  const type=document.getElementById('newType').value;
  const district=document.getElementById('newDistrict').value.trim();
  const lat=parseFloat(document.getElementById('newLat').value);
  const lng=parseFloat(document.getElementById('newLng').value);
  if(!name||isNaN(lat)||isNaN(lng)){ adminMsg('ادخل اسم وإحداثيات صحيحة'); return;}
  places.push({name,type,district,lat,lng});
  savePlaces();
  populateDistricts();
  renderPlaces(places);
  document.getElementById('newName').value=''; 
  document.getElementById('newDistrict').value='';
  document.getElementById('newLat').value=''; 
  document.getElementById('newLng').value='';
}

function adminMsg(m){ 
  document.getElementById('adminMsg').textContent = m; 
  setTimeout(()=>document.getElementById('adminMsg').textContent='',2500); 
}

function locateMe(){
  if(!navigator.geolocation) return alert('المتصفح لا يدعم تحديد الموقع');
  navigator.geolocation.getCurrentPosition(pos=>{
    const lat=pos.coords.latitude;
    const lng=pos.coords.longitude;
    if(userMarker) map.removeLayer(userMarker);
    userMarker = L.circleMarker([lat,lng],{radius:8,color:'#ff5722',fillColor:'#ff8a50',fillOpacity:0.9}).addTo(map).bindPopup('أنت هنا');
    map.setView([lat,lng],14);
  },()=>alert('تعذر الحصول على موقعك'));
}

function toggleTraffic() {
  trafficEnabled = !trafficEnabled;
  
  if (trafficEnabled) {
    // تفعيل طبقة الزحمة (محاكاة)
    enableTraffic();
    document.getElementById('trafficToggleBtn').textContent = '🚦 إيقاف الزحمة';
    document.getElementById('trafficToggleBtn').classList.remove('btn-warning');
    document.getElementById('trafficToggleBtn').classList.add('btn-success');
  } else {
    // إيقاف طبقة الزحمة
    disableTraffic();
    document.getElementById('trafficToggleBtn').textContent = '🚦 تفعيل الزحمة';
    document.getElementById('trafficToggleBtn').classList.remove('btn-success');
    document.getElementById('trafficToggleBtn').classList.add('btn-warning');
  }
}

function enableTraffic() {
  // محاكاة بيانات الزحمة (في تطبيق حقيقي، نستخدم API خاص بالزحمة)
  const trafficData = generateSimulatedTraffic();
  
  // إضافة طبقة الزحمة
  trafficLayer = L.layerGroup();
  
  trafficData.forEach(road => {
    const color = getTrafficColor(road.level);
    const polyline = L.polyline(road.coordinates, {color: color, weight: 6}).addTo(trafficLayer);
    
    // إضافة تلميح عند المرور فوق الطريق
    polyline.bindPopup(`<b>${road.name}</b><br>مستوى الزحمة: ${getTrafficLevelText(road.level)}`);
  });
  
  trafficLayer.addTo(map);
}

function disableTraffic() {
  if (trafficLayer) {
    map.removeLayer(trafficLayer);
    trafficLayer = null;
  }
}

function generateSimulatedTraffic() {
  // محاكاة بيانات زحمة لطرق رئيسية في القاهرة
  return [
    {
      name: "طريق صلاح سالم",
      coordinates: [[30.062, 31.245], [30.058, 31.265], [30.055, 31.285], [30.052, 31.305]],
      level: Math.floor(Math.random() * 3) + 1 // مستوى زحمة عشوائي (1-3)
    },
    {
      name: "كورنيش النيل",
      coordinates: [[30.105, 31.215], [30.090, 31.220], [30.075, 31.225], [30.060, 31.230], [30.045, 31.235]],
      level: Math.floor(Math.random() * 3) + 1
    },
    {
      name: "طريق الأوتوستراد",
      coordinates: [[30.130, 31.325], [30.115, 31.315], [30.100, 31.305], [30.085, 31.295]],
      level: Math.floor(Math.random() * 3) + 1
    },
    {
      name: "طريق القاهرة - الإسكندرية الزراعي",
      coordinates: [[30.150, 31.200], [30.140, 31.210], [30.130, 31.220], [30.120, 31.230]],
      level: Math.floor(Math.random() * 3) + 1
    },
    {
      name: "طريق ميدان لبنان",
      coordinates: [[30.070, 31.360], [30.065, 31.350], [30.060, 31.340], [30.055, 31.330]],
      level: Math.floor(Math.random() * 3) + 1
    }
  ];
}

function getTrafficColor(level) {
  switch(level) {
    case 1: return '#2ecc71'; // أخضر - زحمة خفيفة
    case 2: return '#f39c12'; // برتقالي - زحمة متوسطة
    case 3: return '#e74c3c'; // أحمر - زحمة شديدة
    default: return '#2ecc71';
  }
}

function getTrafficLevelText(level) {
  switch(level) {
    case 1: return 'خفيفة';
    case 2: return 'متوسطة';
    case 3: return 'شديدة';
    default: return 'غير معروف';
  }
}

function switchTab(tabName) {
  // تحديث التبويبات النشطة
  document.querySelectorAll('.tab').forEach(tab => {
    tab.classList.remove('active');
    if (tab.getAttribute('data-tab') === tabName) {
      tab.classList.add('active');
    }
  });
  
  // إظهار/إخفاء المحتوى حسب التبويب
  if (tabName === 'services') {
    document.getElementById('servicesTab').style.display = 'block';
    document.getElementById('governmentTab').style.display = 'none';
    removeGovernmentMarkers();
    renderPlaces(places);
  } else if (tabName === 'government') {
    document.getElementById('servicesTab').style.display = 'none';
    document.getElementById('governmentTab').style.display = 'block';
    addGovernmentMarkers();
  }
}

function openBigMap(){
  document.getElementById('bigMapModal').style.display='flex';
  setTimeout(() => {
    if(!bigMap){ 
      bigMap = L.map('bigMapInner').setView(map.getCenter(), map.getZoom()); 
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',{
        attribution:'&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
      }).addTo(bigMap); 
      
      // إضافة أماكن إلى الخريطة الكبيرة
      places.forEach(p => {
        const m = L.marker([p.lat, p.lng], {icon: createDivIcon(p.type)}).addTo(bigMap);
        m.bindPopup(`<b>${p.name}</b><div class="kv">${p.type} • ${p.district || ''}</div>`);
      });
    } else {
      bigMap.setView(map.getCenter(), map.getZoom());
    }
  }, 100);
}

function closeBigMap(){ 
  document.getElementById('bigMapModal').style.display='none'; 
}

function searchBigMap() {
  const query = document.getElementById('bigMapSearch').value.trim();
  if (!query) return;
  
  // هذا مثال بسيط للبحث - يمكن تطويره لاستخدام خدمة geocoding فعلية
  const found = places.filter(p => 
    p.name.includes(query) || (p.district && p.district.includes(query))
  );
  
  if (found.length > 0 && bigMap) {
    const group = new L.featureGroup(found.map(p => L.marker([p.lat, p.lng])));
    bigMap.fitBounds(group.getBounds().pad(0.1));
    
    // فتح popup لأول نتيجة
    const firstMarker = bigMap.getLayers().find(layer => 
      layer instanceof L.Marker && 
      layer.getLatLng().lat === found[0].lat && 
      layer.getLatLng().lng === found[0].lng
    );
    
    if (firstMarker) {
      firstMarker.openPopup();
    }
  } else {
    alert('لم يتم العثور على نتائج لـ: ' + query);
  }
}
</script>
</body>
</html>
