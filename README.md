# financehub
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<title>FinanceHub</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;1,9..40,300&display=swap" rel="stylesheet">
<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
<script src="https://cdn.sheetjs.com/xlsx-0.20.0/package/dist/xlsx.full.min.js"></script>

<!-- Firebase SDKs -->
<script type="module">
import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
import { getAuth, GoogleAuthProvider, signInWithPopup, signOut, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js";
import { getFirestore, collection, doc, setDoc, getDoc, getDocs, addDoc, updateDoc, deleteDoc, query, where, orderBy, onSnapshot } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";

const firebaseConfig = {
  apiKey: "AIzaSyB7t_4FXVpeL2Ln5r2Loc8lY6syEKnvA7g",
  authDomain: "financeiro-pessoal-cartoes.firebaseapp.com",
  projectId: "financeiro-pessoal-cartoes",
  storageBucket: "financeiro-pessoal-cartoes.firebasestorage.app",
  messagingSenderId: "794267782688",
  appId: "1:794267782688:web:7d9f6b792fbd1ffc2d3645",
  measurementId: "G-C2Q4BG6FYV"
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);
window._db = db;
window._auth = auth;
window._GoogleAuthProvider = GoogleAuthProvider;
window._signInWithPopup = signInWithPopup;
window._signOut = signOut;
window._onAuthStateChanged = onAuthStateChanged;
window._firestore = { collection, doc, setDoc, getDoc, getDocs, addDoc, updateDoc, deleteDoc, query, where, orderBy, onSnapshot };
window._firebaseReady = true;
document.dispatchEvent(new Event('firebaseReady'));
</script>

<style>
:root {
  --bg: #0a0d14;
  --bg2: #111520;
  --bg3: #181d2e;
  --card: #1a2035;
  --card2: #1f2640;
  --border: #2a3350;
  --accent: #4f8cff;
  --accent2: #7c3aed;
  --green: #22c55e;
  --red: #ef4444;
  --yellow: #f59e0b;
  --orange: #f97316;
  --text: #e8ecf4;
  --text2: #8b95b0;
  --text3: #4a5270;
  --eur: #a78bfa;
  --brl: #34d399;
  --radius: 16px;
  --radius-sm: 10px;
  --shadow: 0 4px 24px rgba(0,0,0,0.4);
  --shadow-lg: 0 8px 48px rgba(0,0,0,0.6);
}

* { margin:0; padding:0; box-sizing:border-box; }
html { scroll-behavior: smooth; }

body {
  font-family: 'DM Sans', sans-serif;
  background: var(--bg);
  color: var(--text);
  min-height: 100vh;
  overflow-x: hidden;
}

h1,h2,h3,h4 { font-family: 'Syne', sans-serif; }

/* ─── SCROLLBAR ─── */
::-webkit-scrollbar { width: 6px; height: 6px; }
::-webkit-scrollbar-track { background: var(--bg2); }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 3px; }

/* ─── LOGIN SCREEN ─── */
#loginScreen {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  background: radial-gradient(ellipse at 20% 50%, rgba(79,140,255,0.12) 0%, transparent 60%),
              radial-gradient(ellipse at 80% 20%, rgba(124,58,237,0.1) 0%, transparent 50%),
              var(--bg);
  padding: 2rem;
}

.login-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 24px;
  padding: 3rem 2.5rem;
  width: 100%;
  max-width: 420px;
  text-align: center;
  box-shadow: var(--shadow-lg);
  animation: fadeUp 0.6s ease;
}

.login-logo {
  width: 72px; height: 72px;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  border-radius: 20px;
  display: flex; align-items: center; justify-content: center;
  margin: 0 auto 1.5rem;
  font-size: 2rem;
  box-shadow: 0 8px 32px rgba(79,140,255,0.3);
}

.login-card h1 { font-size: 2rem; font-weight: 800; margin-bottom: 0.5rem; }
.login-card p { color: var(--text2); margin-bottom: 2rem; line-height: 1.6; }

.btn-google {
  display: flex; align-items: center; justify-content: center; gap: 0.75rem;
  width: 100%; padding: 0.9rem 1.5rem;
  background: white; color: #333;
  border: none; border-radius: var(--radius-sm);
  font-size: 1rem; font-weight: 500; cursor: pointer;
  transition: all 0.2s;
  font-family: 'DM Sans', sans-serif;
}
.btn-google:hover { transform: translateY(-1px); box-shadow: 0 4px 20px rgba(255,255,255,0.15); }
.btn-google img { width: 20px; }

/* ─── APP LAYOUT ─── */
#app { display: none; min-height: 100vh; }

.sidebar {
  position: fixed; left: 0; top: 0; bottom: 0;
  width: 240px;
  background: var(--bg2);
  border-right: 1px solid var(--border);
  display: flex; flex-direction: column;
  z-index: 100;
  transition: transform 0.3s ease;
}

.sidebar-logo {
  padding: 1.5rem;
  display: flex; align-items: center; gap: 0.75rem;
  border-bottom: 1px solid var(--border);
}
.sidebar-logo .logo-icon {
  width: 40px; height: 40px;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  font-size: 1.1rem;
}
.sidebar-logo span { font-family: 'Syne', sans-serif; font-weight: 800; font-size: 1.1rem; }

.sidebar-nav { flex: 1; padding: 1rem 0.75rem; overflow-y: auto; }
.nav-section { margin-bottom: 1.5rem; }
.nav-label { font-size: 0.7rem; font-weight: 600; letter-spacing: 0.1em; color: var(--text3); text-transform: uppercase; padding: 0 0.75rem; margin-bottom: 0.5rem; }

.nav-item {
  display: flex; align-items: center; gap: 0.75rem;
  padding: 0.65rem 0.75rem;
  border-radius: var(--radius-sm);
  cursor: pointer; color: var(--text2);
  font-size: 0.9rem; font-weight: 500;
  transition: all 0.15s;
  margin-bottom: 2px;
}
.nav-item:hover { background: var(--card); color: var(--text); }
.nav-item.active { background: linear-gradient(135deg, rgba(79,140,255,0.15), rgba(124,58,237,0.1)); color: var(--accent); }
.nav-item i { width: 18px; text-align: center; font-size: 0.9rem; }

.sidebar-footer {
  padding: 1rem;
  border-top: 1px solid var(--border);
}
.user-info {
  display: flex; align-items: center; gap: 0.75rem;
  padding: 0.75rem;
  background: var(--card);
  border-radius: var(--radius-sm);
  margin-bottom: 0.75rem;
}
.user-avatar {
  width: 36px; height: 36px; border-radius: 50%;
  background: linear-gradient(135deg, var(--accent), var(--accent2));
  display: flex; align-items: center; justify-content: center;
  font-size: 0.9rem; font-weight: 700;
  flex-shrink: 0; overflow: hidden;
}
.user-avatar img { width: 100%; height: 100%; object-fit: cover; }
.user-name { font-size: 0.85rem; font-weight: 600; }
.user-role { font-size: 0.72rem; color: var(--text2); }

.btn-logout {
  display: flex; align-items: center; justify-content: center; gap: 0.5rem;
  width: 100%; padding: 0.6rem;
  background: transparent; border: 1px solid var(--border);
  color: var(--text2); border-radius: var(--radius-sm);
  cursor: pointer; font-size: 0.85rem;
  transition: all 0.2s;
  font-family: 'DM Sans', sans-serif;
}
.btn-logout:hover { background: rgba(239,68,68,0.1); border-color: var(--red); color: var(--red); }

/* ─── MAIN CONTENT ─── */
.main {
  margin-left: 240px;
  min-height: 100vh;
  display: flex; flex-direction: column;
}

.topbar {
  position: sticky; top: 0; z-index: 50;
  background: rgba(10,13,20,0.85);
  backdrop-filter: blur(12px);
  border-bottom: 1px solid var(--border);
  padding: 0 1.5rem;
  height: 60px;
  display: flex; align-items: center; justify-content: space-between;
}

.topbar-title { font-family: 'Syne', sans-serif; font-size: 1.1rem; font-weight: 700; }

.topbar-actions { display: flex; align-items: center; gap: 0.75rem; }

.btn { 
  display: inline-flex; align-items: center; gap: 0.5rem;
  padding: 0.55rem 1.1rem;
  border-radius: var(--radius-sm);
  font-size: 0.875rem; font-weight: 500; cursor: pointer;
  border: none; transition: all 0.15s;
  font-family: 'DM Sans', sans-serif;
  white-space: nowrap;
}
.btn-primary { background: var(--accent); color: white; }
.btn-primary:hover { background: #3d7aee; transform: translateY(-1px); }
.btn-secondary { background: var(--card2); color: var(--text); border: 1px solid var(--border); }
.btn-secondary:hover { background: var(--card); }
.btn-danger { background: rgba(239,68,68,0.15); color: var(--red); border: 1px solid rgba(239,68,68,0.3); }
.btn-danger:hover { background: rgba(239,68,68,0.25); }
.btn-success { background: rgba(34,197,94,0.15); color: var(--green); border: 1px solid rgba(34,197,94,0.3); }
.btn-success:hover { background: rgba(34,197,94,0.25); }
.btn-whatsapp { background: #25D366; color: white; }
.btn-whatsapp:hover { background: #1ebe5d; transform: translateY(-1px); }
.btn-sm { padding: 0.35rem 0.75rem; font-size: 0.8rem; }
.btn-icon { padding: 0.55rem; width: 36px; height: 36px; justify-content: center; }

/* ─── PAGE CONTENT ─── */
.page { display: none; padding: 1.5rem; }
.page.active { display: block; }

/* ─── CARDS ─── */
.card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.25rem;
}

.card-header {
  display: flex; align-items: center; justify-content: space-between;
  margin-bottom: 1.25rem;
}
.card-title { font-family: 'Syne', sans-serif; font-size: 1rem; font-weight: 700; }

/* ─── STATS GRID ─── */
.stats-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-bottom: 1.5rem;
}

.stat-card {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 1.25rem;
  position: relative;
  overflow: hidden;
  transition: transform 0.2s;
}
.stat-card:hover { transform: translateY(-2px); }
.stat-card::before {
  content: '';
  position: absolute; top: 0; left: 0; right: 0; height: 3px;
}
.stat-card.blue::before { background: linear-gradient(90deg, var(--accent), var(--accent2)); }
.stat-card.green::before { background: linear-gradient(90deg, var(--green), #16a34a); }
.stat-card.red::before { background: linear-gradient(90deg, var(--red), #dc2626); }
.stat-card.yellow::before { background: linear-gradient(90deg, var(--yellow), var(--orange)); }
.stat-card.purple::before { background: linear-gradient(90deg, var(--eur), var(--accent2)); }

.stat-label { font-size: 0.78rem; color: var(--text2); font-weight: 500; margin-bottom: 0.5rem; text-transform: uppercase; letter-spacing: 0.05em; }
.stat-value { font-family: 'Syne', sans-serif; font-size: 1.6rem; font-weight: 800; line-height: 1; }
.stat-value.brl { color: var(--brl); }
.stat-value.eur { color: var(--eur); }
.stat-sub { font-size: 0.78rem; color: var(--text2); margin-top: 0.4rem; }

/* ─── GRID LAYOUTS ─── */
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
.grid-3 { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 1rem; }

/* ─── TABLE ─── */
.table-wrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; font-size: 0.875rem; }
th {
  text-align: left; padding: 0.75rem 1rem;
  font-size: 0.72rem; font-weight: 600; color: var(--text2);
  text-transform: uppercase; letter-spacing: 0.06em;
  border-bottom: 1px solid var(--border);
}
td { padding: 0.8rem 1rem; border-bottom: 1px solid rgba(42,51,80,0.5); vertical-align: middle; }
tr:last-child td { border-bottom: none; }
tr:hover td { background: rgba(255,255,255,0.02); }

/* ─── BADGES ─── */
.badge {
  display: inline-flex; align-items: center; gap: 0.3rem;
  padding: 0.2rem 0.6rem;
  border-radius: 20px;
  font-size: 0.72rem; font-weight: 600;
}
.badge-green { background: rgba(34,197,94,0.15); color: var(--green); }
.badge-red { background: rgba(239,68,68,0.15); color: var(--red); }
.badge-yellow { background: rgba(245,158,11,0.15); color: var(--yellow); }
.badge-blue { background: rgba(79,140,255,0.15); color: var(--accent); }
.badge-purple { background: rgba(167,139,250,0.15); color: var(--eur); }

/* ─── CURRENCY TAGS ─── */
.curr-brl { color: var(--brl); font-weight: 600; }
.curr-eur { color: var(--eur); font-weight: 600; }

/* ─── MODAL ─── */
.modal-overlay {
  display: none; position: fixed; inset: 0; z-index: 1000;
  background: rgba(0,0,0,0.7);
  backdrop-filter: blur(4px);
  align-items: center; justify-content: center;
  padding: 1rem;
}
.modal-overlay.open { display: flex; }

.modal {
  background: var(--card);
  border: 1px solid var(--border);
  border-radius: 20px;
  width: 100%; max-width: 560px;
  max-height: 90vh; overflow-y: auto;
  box-shadow: var(--shadow-lg);
  animation: fadeUp 0.25s ease;
}
.modal-header {
  padding: 1.25rem 1.5rem;
  border-bottom: 1px solid var(--border);
  display: flex; align-items: center; justify-content: space-between;
}
.modal-header h3 { font-family: 'Syne', sans-serif; font-size: 1.05rem; font-weight: 700; }
.modal-body { padding: 1.5rem; }
.modal-footer { padding: 1rem 1.5rem; border-top: 1px solid var(--border); display: flex; gap: 0.75rem; justify-content: flex-end; }

/* ─── FORM ─── */
.form-group { margin-bottom: 1rem; }
.form-row { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; }
label { display: block; font-size: 0.8rem; font-weight: 500; color: var(--text2); margin-bottom: 0.4rem; }
input, select, textarea {
  width: 100%; padding: 0.7rem 0.9rem;
  background: var(--bg2); border: 1px solid var(--border);
  border-radius: var(--radius-sm); color: var(--text);
  font-size: 0.875rem; font-family: 'DM Sans', sans-serif;
  transition: border-color 0.15s;
  appearance: none;
}
input:focus, select:focus, textarea:focus { outline: none; border-color: var(--accent); }
select option { background: var(--bg2); }
textarea { resize: vertical; min-height: 80px; }

/* ─── ALERTS ─── */
.alert-bar {
  display: flex; align-items: center; gap: 0.75rem;
  padding: 0.9rem 1.1rem;
  border-radius: var(--radius-sm);
  font-size: 0.875rem; margin-bottom: 0.75rem;
}
.alert-bar.warning { background: rgba(245,158,11,0.1); border: 1px solid rgba(245,158,11,0.25); color: var(--yellow); }
.alert-bar.danger { background: rgba(239,68,68,0.1); border: 1px solid rgba(239,68,68,0.25); color: var(--red); }
.alert-bar.info { background: rgba(79,140,255,0.1); border: 1px solid rgba(79,140,255,0.25); color: var(--accent); }
.alert-bar.success { background: rgba(34,197,94,0.1); border: 1px solid rgba(34,197,94,0.25); color: var(--green); }

/* ─── CREDIT CARD VISUAL ─── */
.credit-card-visual {
  border-radius: 16px;
  padding: 1.25rem;
  aspect-ratio: 1.586;
  position: relative;
  overflow: hidden;
  display: flex; flex-direction: column; justify-content: space-between;
  cursor: pointer; transition: transform 0.2s;
  max-width: 300px;
}
.credit-card-visual:hover { transform: scale(1.02); }
.credit-card-visual.brl { background: linear-gradient(135deg, #1a4a2e, #22c55e30, #0f2d1c); border: 1px solid rgba(34,197,94,0.3); }
.credit-card-visual.eur { background: linear-gradient(135deg, #1e1b4b, #7c3aed30, #0f0a2d); border: 1px solid rgba(167,139,250,0.3); }
.card-bank { font-size: 0.8rem; color: rgba(255,255,255,0.6); }
.card-number { font-family: 'Syne', sans-serif; font-size: 1.1rem; letter-spacing: 0.15em; }
.card-bottom { display: flex; justify-content: space-between; align-items: flex-end; }
.card-holder { font-size: 0.8rem; color: rgba(255,255,255,0.7); }
.card-limit { text-align: right; }
.card-limit .limit-val { font-family: 'Syne', sans-serif; font-size: 0.9rem; font-weight: 700; }
.card-limit .limit-label { font-size: 0.65rem; color: rgba(255,255,255,0.5); }

/* ─── PROGRESS BAR ─── */
.progress-wrap { margin-top: 0.5rem; }
.progress-bar { height: 4px; background: rgba(255,255,255,0.1); border-radius: 2px; overflow: hidden; }
.progress-fill { height: 100%; border-radius: 2px; transition: width 0.4s ease; }
.progress-fill.safe { background: var(--green); }
.progress-fill.warn { background: var(--yellow); }
.progress-fill.danger { background: var(--red); }
.progress-label { display: flex; justify-content: space-between; font-size: 0.7rem; color: rgba(255,255,255,0.5); margin-top: 0.25rem; }

/* ─── CHARTS ─── */
.chart-container { position: relative; height: 260px; }

/* ─── MOBILE HAMBURGER ─── */
.hamburger {
  display: none;
  background: none; border: none; color: var(--text); cursor: pointer;
  font-size: 1.2rem; padding: 0.5rem;
}
.sidebar-overlay { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 99; }

/* ─── EMPTY STATE ─── */
.empty-state {
  text-align: center; padding: 3rem;
  color: var(--text2);
}
.empty-state i { font-size: 2.5rem; margin-bottom: 1rem; opacity: 0.4; display: block; }
.empty-state p { font-size: 0.9rem; }

/* ─── TABS ─── */
.tabs { display: flex; gap: 0.25rem; background: var(--bg2); border-radius: var(--radius-sm); padding: 4px; margin-bottom: 1.25rem; }
.tab { flex: 1; padding: 0.55rem 0.75rem; border-radius: 7px; cursor: pointer; font-size: 0.85rem; font-weight: 500; text-align: center; color: var(--text2); transition: all 0.15s; border: none; background: none; font-family: 'DM Sans', sans-serif; }
.tab.active { background: var(--card2); color: var(--text); box-shadow: 0 2px 8px rgba(0,0,0,0.3); }

/* ─── MONTH SELECTOR ─── */
.month-nav { display: flex; align-items: center; gap: 0.75rem; }
.month-nav button { background: var(--card2); border: 1px solid var(--border); color: var(--text); border-radius: 8px; padding: 0.4rem 0.75rem; cursor: pointer; font-size: 0.85rem; transition: all 0.15s; }
.month-nav button:hover { background: var(--card); }
.month-nav .month-label { font-family: 'Syne', sans-serif; font-weight: 700; font-size: 1rem; min-width: 140px; text-align: center; }

/* ─── ANIMATIONS ─── */
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
@keyframes spin { to { transform: rotate(360deg); } }
.spin { animation: spin 0.8s linear infinite; }

/* ─── LOADING ─── */
.loading-overlay {
  position: fixed; inset: 0; z-index: 9999;
  background: var(--bg);
  display: flex; align-items: center; justify-content: center;
  flex-direction: column; gap: 1rem;
}
.loading-overlay.hidden { display: none; }
.loader { width: 40px; height: 40px; border: 3px solid var(--border); border-top-color: var(--accent); border-radius: 50%; animation: spin 0.7s linear infinite; }

/* ─── RESPONSIVE ─── */
@media (max-width: 768px) {
  .sidebar { transform: translateX(-100%); }
  .sidebar.open { transform: translateX(0); }
  .sidebar-overlay.open { display: block; }
  .main { margin-left: 0; }
  .hamburger { display: block; }
  .grid-2, .grid-3 { grid-template-columns: 1fr; }
  .form-row { grid-template-columns: 1fr; }
  .stats-grid { grid-template-columns: 1fr 1fr; }
  .page { padding: 1rem; }
  .topbar { padding: 0 1rem; }
}
@media (max-width: 480px) {
  .stats-grid { grid-template-columns: 1fr; }
}

/* ─── NOTIFICATION TOAST ─── */
.toast-container { position: fixed; top: 1rem; right: 1rem; z-index: 9000; display: flex; flex-direction: column; gap: 0.5rem; }
.toast {
  background: var(--card2); border: 1px solid var(--border);
  border-radius: var(--radius-sm); padding: 0.9rem 1.1rem;
  display: flex; align-items: center; gap: 0.75rem;
  font-size: 0.875rem; max-width: 320px;
  animation: fadeUp 0.3s ease;
  box-shadow: var(--shadow);
}
.toast.success { border-color: rgba(34,197,94,0.4); }
.toast.error { border-color: rgba(239,68,68,0.4); }
.toast i { font-size: 1rem; }
.toast.success i { color: var(--green); }
.toast.error i { color: var(--red); }

/* ─── SECTION HEADER ─── */
.section-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 1.25rem; flex-wrap: gap; gap: 0.75rem; }
.section-header h2 { font-size: 1.3rem; font-weight: 800; }

/* Installment indicator */
.installment-badge { font-size: 0.7rem; background: rgba(79,140,255,0.15); color: var(--accent); padding: 0.1rem 0.4rem; border-radius: 4px; margin-left: 0.4rem; }

/* ─── WHATSAPP PREVIEW ─── */
.wa-preview {
  background: #111b21;
  border-radius: 12px;
  padding: 1rem;
  font-family: monospace;
  font-size: 0.8rem;
  white-space: pre-wrap;
  color: #e9edef;
  max-height: 300px;
  overflow-y: auto;
  line-height: 1.6;
}

/* Financing progress */
.fin-progress { margin-top: 0.75rem; }

/* ─── SAVINGS TIPS ─── */
.tip-card {
  background: var(--bg3);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 1rem;
  display: flex; gap: 0.75rem; align-items: flex-start;
  margin-bottom: 0.75rem;
}
.tip-icon { font-size: 1.3rem; flex-shrink: 0; }
.tip-text h4 { font-size: 0.875rem; font-weight: 600; margin-bottom: 0.25rem; }
.tip-text p { font-size: 0.8rem; color: var(--text2); line-height: 1.5; }
</style>
</head>
<body>

<!-- Loading -->
<div class="loading-overlay" id="loadingOverlay">
  <div class="logo-icon" style="width:56px;height:56px;background:linear-gradient(135deg,#4f8cff,#7c3aed);border-radius:16px;display:flex;align-items:center;justify-content:center;font-size:1.6rem;margin-bottom:0.5rem;">💎</div>
  <div class="loader"></div>
  <p style="color:var(--text2);font-size:0.85rem;">Carregando FinanceHub...</p>
</div>

<!-- Toast container -->
<div class="toast-container" id="toastContainer"></div>

<!-- ═══════════════════════════════════════════
     LOGIN SCREEN
═══════════════════════════════════════════ -->
<div id="loginScreen" style="display:none;">
  <div class="login-card">
    <div class="login-logo">💎</div>
    <h1>FinanceHub</h1>
    <p>Controle total das suas finanças pessoais e familiares — cartões, contas, financiamentos e muito mais.</p>
    <button class="btn-google" id="btnGoogleLogin">
      <img src="https://www.google.com/favicon.ico" alt="Google">
      Entrar com Google (Gmail)
    </button>
    <p style="margin-top:1.5rem;font-size:0.75rem;color:var(--text3);">Seus dados ficam seguros na nuvem Google (Firebase)</p>
  </div>
</div>

<!-- ═══════════════════════════════════════════
     MAIN APP
═══════════════════════════════════════════ -->
<div id="app">
  <!-- Sidebar overlay (mobile) -->
  <div class="sidebar-overlay" id="sidebarOverlay" onclick="closeSidebar()"></div>

  <!-- Sidebar -->
  <nav class="sidebar" id="sidebar">
    <div class="sidebar-logo">
      <div class="logo-icon">💎</div>
      <span>FinanceHub</span>
    </div>
    <div class="sidebar-nav">
      <div class="nav-section">
        <div class="nav-label">Principal</div>
        <div class="nav-item active" onclick="showPage('dashboard')"><i class="fas fa-chart-pie"></i> Dashboard</div>
        <div class="nav-item" onclick="showPage('transactions')"><i class="fas fa-list"></i> Transações</div>
      </div>
      <div class="nav-section">
        <div class="nav-label">Financeiro</div>
        <div class="nav-item" onclick="showPage('cards')"><i class="fas fa-credit-card"></i> Cartões</div>
        <div class="nav-item" onclick="showPage('accounts')"><i class="fas fa-university"></i> Contas Bancárias</div>
        <div class="nav-item" onclick="showPage('financing')"><i class="fas fa-file-invoice-dollar"></i> Financiamentos</div>
      </div>
      <div class="nav-section">
        <div class="nav-label">Relatórios</div>
        <div class="nav-item" onclick="showPage('reports')"><i class="fas fa-chart-bar"></i> Relatórios</div>
        <div class="nav-item" onclick="showPage('whatsapp')"><i class="fab fa-whatsapp"></i> WhatsApp</div>
        <div class="nav-item" onclick="showPage('savings')"><i class="fas fa-piggy-bank"></i> Economizar</div>
      </div>
      <div class="nav-section" id="adminNav" style="display:none;">
        <div class="nav-label">Admin</div>
        <div class="nav-item" onclick="showPage('users')"><i class="fas fa-users"></i> Usuários</div>
        <div class="nav-item" onclick="showPage('settings')"><i class="fas fa-cog"></i> Configurações</div>
      </div>
    </div>
    <div class="sidebar-footer">
      <div class="user-info">
        <div class="user-avatar" id="sidebarAvatar">?</div>
        <div>
          <div class="user-name" id="sidebarName">Usuário</div>
          <div class="user-role" id="sidebarRole">Carregando...</div>
        </div>
      </div>
      <button class="btn-logout" onclick="handleLogout()"><i class="fas fa-sign-out-alt"></i> Sair</button>
    </div>
  </nav>

  <!-- Main content -->
  <main class="main">
    <div class="topbar">
      <div style="display:flex;align-items:center;gap:0.75rem;">
        <button class="hamburger" onclick="toggleSidebar()"><i class="fas fa-bars"></i></button>
        <span class="topbar-title" id="topbarTitle">Dashboard</span>
      </div>
      <div class="topbar-actions">
        <div class="month-nav">
          <button onclick="changeMonth(-1)"><i class="fas fa-chevron-left"></i></button>
          <span class="month-label" id="monthLabel">—</span>
          <button onclick="changeMonth(1)"><i class="fas fa-chevron-right"></i></button>
        </div>
        <button class="btn btn-primary btn-sm" onclick="openAddTransaction()"><i class="fas fa-plus"></i> Nova</button>
      </div>
    </div>

    <!-- ─── DASHBOARD ─── -->
    <div class="page active" id="page-dashboard">
      <div id="alertsContainer" style="margin-bottom:1rem;"></div>

      <div class="stats-grid" id="statsGrid">
        <div class="stat-card blue">
          <div class="stat-label">Total Cartões BRL</div>
          <div class="stat-value brl" id="statTotalBRL">R$ 0,00</div>
          <div class="stat-sub" id="statTotalBRLsub">0 transações</div>
        </div>
        <div class="stat-card purple">
          <div class="stat-label">Total Cartões EUR</div>
          <div class="stat-value eur" id="statTotalEUR">€ 0,00</div>
          <div class="stat-sub" id="statTotalEURsub">0 transações</div>
        </div>
        <div class="stat-card green">
          <div class="stat-label">Saldo Contas</div>
          <div class="stat-value" id="statBalance" style="color:var(--green)">R$ 0,00</div>
          <div class="stat-sub">Todas as contas</div>
        </div>
        <div class="stat-card yellow">
          <div class="stat-label">Financiamentos</div>
          <div class="stat-value" id="statFinancing" style="color:var(--yellow)">R$ 0,00</div>
          <div class="stat-sub">Parcelas em aberto</div>
        </div>
      </div>

      <div class="grid-2" style="margin-bottom:1rem;">
        <div class="card">
          <div class="card-header">
            <span class="card-title">Gastos por Categoria</span>
            <div style="display:flex;gap:0.5rem;">
              <button class="btn btn-secondary btn-sm" onclick="toggleChartCurrency('category')" id="btnCatCurr">BRL</button>
            </div>
          </div>
          <div class="chart-container"><canvas id="chartCategory"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header">
            <span class="card-title">Gastos por Pessoa</span>
            <button class="btn btn-secondary btn-sm" onclick="toggleChartCurrency('person')" id="btnPersonCurr">BRL</button>
          </div>
          <div class="chart-container"><canvas id="chartPerson"></canvas></div>
        </div>
      </div>

      <div class="grid-2">
        <div class="card">
          <div class="card-header">
            <span class="card-title">Evolução Mensal</span>
          </div>
          <div class="chart-container"><canvas id="chartMonthly"></canvas></div>
        </div>
        <div class="card">
          <div class="card-header">
            <span class="card-title">Limite dos Cartões</span>
          </div>
          <div id="cardLimitsContainer">
            <div class="empty-state"><i class="fas fa-credit-card"></i><p>Nenhum cartão cadastrado</p></div>
          </div>
        </div>
      </div>
    </div>

    <!-- ─── TRANSACTIONS ─── -->
    <div class="page" id="page-transactions">
      <div class="section-header">
        <h2>Transações</h2>
        <div style="display:flex;gap:0.5rem;flex-wrap:wrap;">
          <button class="btn btn-secondary btn-sm" onclick="importCSV()"><i class="fas fa-upload"></i> Importar CSV</button>
          <button class="btn btn-secondary btn-sm" onclick="exportExcel()"><i class="fas fa-file-excel"></i> Exportar</button>
          <button class="btn btn-primary btn-sm" onclick="openAddTransaction()"><i class="fas fa-plus"></i> Nova</button>
        </div>
      </div>
      <div class="card">
        <div style="display:flex;gap:0.75rem;margin-bottom:1rem;flex-wrap:wrap;">
          <select id="filterPerson" style="flex:1;min-width:120px;" onchange="renderTransactions()">
            <option value="">Todas as pessoas</option>
          </select>
          <select id="filterCard" style="flex:1;min-width:120px;" onchange="renderTransactions()">
            <option value="">Todos os cartões</option>
          </select>
          <select id="filterCat" style="flex:1;min-width:120px;" onchange="renderTransactions()">
            <option value="">Todas as categorias</option>
          </select>
          <select id="filterStatus" style="flex:1;min-width:120px;" onchange="renderTransactions()">
            <option value="">Todos os status</option>
            <option value="pending">Pendente</option>
            <option value="paid">Pago</option>
            <option value="late">Atrasado</option>
          </select>
        </div>
        <div class="table-wrap">
          <table>
            <thead><tr>
              <th>Data</th><th>Descrição</th><th>Categoria</th>
              <th>Cartão</th><th>Pessoa</th><th>Valor</th>
              <th>Parcelas</th><th>Status</th><th>Ações</th>
            </tr></thead>
            <tbody id="transactionsBody"></tbody>
          </table>
        </div>
      </div>
      <input type="file" id="csvInput" accept=".csv,.ofx" style="display:none;" onchange="handleCSVImport(event)">
    </div>

    <!-- ─── CARDS PAGE ─── -->
    <div class="page" id="page-cards">
      <div class="section-header">
        <h2>Cartões de Crédito</h2>
        <button class="btn btn-primary" onclick="openAddCard()"><i class="fas fa-plus"></i> Novo Cartão</button>
      </div>
      <div id="cardsGrid" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:1rem;"></div>
    </div>

    <!-- ─── ACCOUNTS PAGE ─── -->
    <div class="page" id="page-accounts">
      <div class="section-header">
        <h2>Contas Bancárias</h2>
        <button class="btn btn-primary" onclick="openAddAccount()"><i class="fas fa-plus"></i> Nova Conta</button>
      </div>
      <div id="accountsGrid" style="display:grid;grid-template-columns:repeat(auto-fill,minmax(280px,1fr));gap:1rem;margin-bottom:1.5rem;"></div>
      <div class="card">
        <div class="card-header"><span class="card-title">Movimentações Recentes</span></div>
        <div class="table-wrap">
          <table>
            <thead><tr><th>Data</th><th>Descrição</th><th>Conta</th><th>Tipo</th><th>Valor</th><th>Status</th><th>Ações</th></tr></thead>
            <tbody id="accountMovBody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- ─── FINANCING PAGE ─── -->
    <div class="page" id="page-financing">
      <div class="section-header">
        <h2>Financiamentos & Empréstimos</h2>
        <button class="btn btn-primary" onclick="openAddFinancing()"><i class="fas fa-plus"></i> Novo</button>
      </div>
      <div id="financingList"></div>
    </div>

    <!-- ─── REPORTS PAGE ─── -->
    <div class="page" id="page-reports">
      <div class="section-header"><h2>Relatórios</h2></div>
      <div class="tabs">
        <button class="tab active" onclick="showReportTab('byPerson')">Por Pessoa</button>
        <button class="tab" onclick="showReportTab('byCard')">Por Cartão</button>
        <button class="tab" onclick="showReportTab('byBank')">Por Banco</button>
        <button class="tab" onclick="showReportTab('byCategory')">Por Categoria</button>
      </div>
      <div id="reportContent"></div>
    </div>

    <!-- ─── WHATSAPP PAGE ─── -->
    <div class="page" id="page-whatsapp">
      <div class="section-header"><h2>Enviar Fatura por WhatsApp</h2></div>
      <div class="grid-2">
        <div class="card">
          <div class="card-header"><span class="card-title">Configurar Envio</span></div>
          <div class="form-group">
            <label>Pessoa</label>
            <select id="waPerson" onchange="updateWAPreview()">
              <option value="">Selecione...</option>
            </select>
          </div>
          <div class="form-group">
            <label>Cartões a incluir</label>
            <div id="waCardsCheckboxes"></div>
          </div>
          <div class="form-group">
            <label>Forma de Pagamento BRL</label>
            <select id="waPayMethodBRL" onchange="updateWAPreview()">
              <option value="PIX">PIX</option>
              <option value="TED">TED</option>
              <option value="Dinheiro">Dinheiro</option>
              <option value="Débito">Débito</option>
            </select>
          </div>
          <div class="form-group">
            <label>Conta de Destino BRL</label>
            <select id="waAccountBRL" onchange="updateWAPreview()"></select>
          </div>
          <div class="form-group">
            <label>Forma de Pagamento EUR</label>
            <select id="waPayMethodEUR" onchange="updateWAPreview()">
              <option value="Transferência Bancária">Transferência Bancária</option>
              <option value="SEPA">SEPA</option>
              <option value="Multibanco">Multibanco</option>
              <option value="MBWay">MBWay</option>
            </select>
          </div>
          <div class="form-group">
            <label>Conta de Destino EUR</label>
            <select id="waAccountEUR" onchange="updateWAPreview()"></select>
          </div>
          <div class="form-group">
            <label>Data de Vencimento</label>
            <input type="date" id="waDueDate" onchange="updateWAPreview()">
          </div>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-title">Prévia da Mensagem</span></div>
          <div class="wa-preview" id="waPreview">Selecione uma pessoa para ver a prévia...</div>
          <div style="margin-top:1rem;">
            <button class="btn btn-whatsapp" onclick="sendWhatsApp()" style="width:100%;">
              <i class="fab fa-whatsapp"></i> Abrir WhatsApp e Enviar
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- ─── SAVINGS PAGE ─── -->
    <div class="page" id="page-savings">
      <div class="section-header"><h2>Dicas de Economia</h2></div>
      <div class="grid-2">
        <div>
          <div class="card" style="margin-bottom:1rem;">
            <div class="card-header"><span class="card-title">Metas Mensais</span>
              <button class="btn btn-primary btn-sm" onclick="openAddGoal()"><i class="fas fa-plus"></i> Meta</button>
            </div>
            <div id="goalsList"></div>
          </div>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-title">💡 Sugestões Inteligentes</span></div>
          <div id="savingsTips"></div>
        </div>
      </div>
    </div>

    <!-- ─── USERS PAGE (ADMIN) ─── -->
    <div class="page" id="page-users">
      <div class="section-header">
        <h2>Gestão de Usuários</h2>
        <button class="btn btn-primary" onclick="openAddUser()"><i class="fas fa-plus"></i> Adicionar Usuário</button>
      </div>
      <div class="card">
        <div class="table-wrap">
          <table>
            <thead><tr><th>Nome</th><th>Email</th><th>Perfil</th><th>WhatsApp</th><th>Cartões</th><th>Ações</th></tr></thead>
            <tbody id="usersBody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- ─── SETTINGS PAGE (ADMIN) ─── -->
    <div class="page" id="page-settings">
      <div class="section-header"><h2>Configurações</h2></div>
      <div class="grid-2">
        <div class="card">
          <div class="card-header"><span class="card-title">Taxa de Câmbio</span></div>
          <div class="form-group">
            <label>1 EUR = ? BRL</label>
            <input type="number" id="exchangeRate" placeholder="5.50" step="0.01">
          </div>
          <button class="btn btn-primary" onclick="saveExchangeRate()">Salvar Taxa</button>
        </div>
        <div class="card">
          <div class="card-header"><span class="card-title">Categorias</span></div>
          <div id="categoriesList" style="margin-bottom:1rem;"></div>
          <div style="display:flex;gap:0.5rem;">
            <input type="text" id="newCategory" placeholder="Nova categoria...">
            <button class="btn btn-primary" onclick="addCategory()"><i class="fas fa-plus"></i></button>
          </div>
        </div>
      </div>
    </div>

  </main>
</div>

<!-- ═══════════════════════════════════════════
     MODALS
═══════════════════════════════════════════ -->

<!-- Add Transaction Modal -->
<div class="modal-overlay" id="modalTransaction">
  <div class="modal">
    <div class="modal-header">
      <h3 id="modalTransactionTitle">Nova Transação</h3>
      <button class="btn btn-icon btn-secondary" onclick="closeModal('modalTransaction')"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-body">
      <input type="hidden" id="txId">
      <div class="form-row">
        <div class="form-group">
          <label>Descrição *</label>
          <input type="text" id="txDesc" placeholder="Ex: Supermercado">
        </div>
        <div class="form-group">
          <label>Data *</label>
          <input type="date" id="txDate">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Valor *</label>
          <input type="number" id="txAmount" placeholder="0,00" step="0.01">
        </div>
        <div class="form-group">
          <label>Moeda</label>
          <select id="txCurrency">
            <option value="BRL">BRL (R$)</option>
            <option value="EUR">EUR (€)</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Categoria</label>
          <select id="txCategory"></select>
        </div>
        <div class="form-group">
          <label>Cartão</label>
          <select id="txCard">
            <option value="">Sem cartão (débito/dinheiro)</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Pessoa</label>
          <select id="txPerson"></select>
        </div>
        <div class="form-group">
          <label>Status</label>
          <select id="txStatus">
            <option value="pending">Pendente</option>
            <option value="paid">Pago</option>
            <option value="late">Atrasado</option>
          </select>
        </div>
      </div>
      <div class="form-group">
        <label>Tipo</label>
        <select id="txType" onchange="toggleInstallments()">
          <option value="expense">Despesa</option>
          <option value="income">Receita</option>
          <option value="installment">Parcelado</option>
        </select>
      </div>
      <div id="installmentFields" style="display:none;">
        <div class="form-row">
          <div class="form-group">
            <label>Número de Parcelas</label>
            <input type="number" id="txInstallments" placeholder="12" min="2" max="72">
          </div>
          <div class="form-group">
            <label>Mês de Início da 1ª Parcela</label>
            <input type="month" id="txInstallmentStart">
          </div>
        </div>
      </div>
      <div class="form-group">
        <label>Observação</label>
        <textarea id="txNotes" placeholder="Opcional..."></textarea>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeModal('modalTransaction')">Cancelar</button>
      <button class="btn btn-primary" onclick="saveTransaction()"><i class="fas fa-save"></i> Salvar</button>
    </div>
  </div>
</div>

<!-- Add Card Modal -->
<div class="modal-overlay" id="modalCard">
  <div class="modal">
    <div class="modal-header">
      <h3 id="modalCardTitle">Novo Cartão</h3>
      <button class="btn btn-icon btn-secondary" onclick="closeModal('modalCard')"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-body">
      <input type="hidden" id="cardId">
      <div class="form-row">
        <div class="form-group">
          <label>Nome do Cartão *</label>
          <input type="text" id="cardName" placeholder="Ex: Nubank Principal">
        </div>
        <div class="form-group">
          <label>Banco/Instituição *</label>
          <input type="text" id="cardBank" placeholder="Ex: Nubank">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Últimos 4 Dígitos *</label>
          <input type="text" id="cardDigits" placeholder="1234" maxlength="4">
        </div>
        <div class="form-group">
          <label>Moeda</label>
          <select id="cardCurrency">
            <option value="BRL">BRL (R$)</option>
            <option value="EUR">EUR (€)</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Limite Total</label>
          <input type="number" id="cardLimit" placeholder="5000" step="0.01">
        </div>
        <div class="form-group">
          <label>Tipo</label>
          <select id="cardType">
            <option value="principal">Principal</option>
            <option value="additional">Adicional</option>
          </select>
        </div>
      </div>
      <div class="form-group" id="cardParentGroup">
        <label>Cartão Principal (para adicional)</label>
        <select id="cardParent">
          <option value="">Selecione o principal...</option>
        </select>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Dia de Fechamento</label>
          <input type="number" id="cardClosingDay" placeholder="1-31" min="1" max="31">
        </div>
        <div class="form-group">
          <label>Dia de Vencimento</label>
          <input type="number" id="cardDueDay" placeholder="1-31" min="1" max="31">
        </div>
      </div>
      <div class="form-group">
        <label>Titular do Cartão</label>
        <select id="cardOwner"></select>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeModal('modalCard')">Cancelar</button>
      <button class="btn btn-primary" onclick="saveCard()"><i class="fas fa-save"></i> Salvar</button>
    </div>
  </div>
</div>

<!-- Add Account Modal -->
<div class="modal-overlay" id="modalAccount">
  <div class="modal">
    <div class="modal-header">
      <h3 id="modalAccountTitle">Nova Conta Bancária</h3>
      <button class="btn btn-icon btn-secondary" onclick="closeModal('modalAccount')"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-body">
      <input type="hidden" id="accId">
      <div class="form-row">
        <div class="form-group">
          <label>Nome da Conta *</label>
          <input type="text" id="accName" placeholder="Ex: Conta Corrente Nubank">
        </div>
        <div class="form-group">
          <label>Banco *</label>
          <input type="text" id="accBank" placeholder="Ex: Nubank">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Tipo</label>
          <select id="accType">
            <option value="checking">Conta Corrente</option>
            <option value="savings">Poupança</option>
            <option value="investment">Investimento</option>
          </select>
        </div>
        <div class="form-group">
          <label>Moeda</label>
          <select id="accCurrency">
            <option value="BRL">BRL (R$)</option>
            <option value="EUR">EUR (€)</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Saldo Inicial</label>
          <input type="number" id="accBalance" placeholder="0,00" step="0.01">
        </div>
        <div class="form-group">
          <label>Titular</label>
          <select id="accOwner"></select>
        </div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeModal('modalAccount')">Cancelar</button>
      <button class="btn btn-primary" onclick="saveAccount()"><i class="fas fa-save"></i> Salvar</button>
    </div>
  </div>
</div>

<!-- Add Financing Modal -->
<div class="modal-overlay" id="modalFinancing">
  <div class="modal">
    <div class="modal-header">
      <h3 id="modalFinancingTitle">Novo Financiamento</h3>
      <button class="btn btn-icon btn-secondary" onclick="closeModal('modalFinancing')"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-body">
      <input type="hidden" id="finId">
      <div class="form-row">
        <div class="form-group">
          <label>Descrição *</label>
          <input type="text" id="finDesc" placeholder="Ex: Financiamento Carro">
        </div>
        <div class="form-group">
          <label>Instituição</label>
          <input type="text" id="finBank" placeholder="Ex: Caixa Econômica">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Valor Total *</label>
          <input type="number" id="finTotal" placeholder="50000" step="0.01">
        </div>
        <div class="form-group">
          <label>Moeda</label>
          <select id="finCurrency">
            <option value="BRL">BRL (R$)</option>
            <option value="EUR">EUR (€)</option>
          </select>
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Valor da Parcela *</label>
          <input type="number" id="finInstallment" placeholder="1500" step="0.01">
        </div>
        <div class="form-group">
          <label>Total de Parcelas *</label>
          <input type="number" id="finTotalInstallments" placeholder="60">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Parcelas Pagas</label>
          <input type="number" id="finPaidInstallments" placeholder="0">
        </div>
        <div class="form-group">
          <label>Taxa de Juros (% a.m.)</label>
          <input type="number" id="finRate" placeholder="1.5" step="0.01">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Data de Vencimento (dia)</label>
          <input type="number" id="finDueDay" placeholder="15" min="1" max="31">
        </div>
        <div class="form-group">
          <label>Titular</label>
          <select id="finOwner"></select>
        </div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeModal('modalFinancing')">Cancelar</button>
      <button class="btn btn-primary" onclick="saveFinancing()"><i class="fas fa-save"></i> Salvar</button>
    </div>
  </div>
</div>

<!-- Add User Modal -->
<div class="modal-overlay" id="modalUser">
  <div class="modal">
    <div class="modal-header">
      <h3 id="modalUserTitle">Adicionar Usuário</h3>
      <button class="btn btn-icon btn-secondary" onclick="closeModal('modalUser')"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-body">
      <input type="hidden" id="userId">
      <div class="form-group">
        <label>Nome *</label>
        <input type="text" id="userName" placeholder="Nome completo">
      </div>
      <div class="form-group">
        <label>Email Gmail *</label>
        <input type="email" id="userEmail" placeholder="usuario@gmail.com">
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Perfil</label>
          <select id="userRole">
            <option value="user">Usuário</option>
            <option value="admin">Admin</option>
          </select>
        </div>
        <div class="form-group">
          <label>WhatsApp (com código do país)</label>
          <input type="text" id="userWhatsApp" placeholder="351912345678">
        </div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeModal('modalUser')">Cancelar</button>
      <button class="btn btn-primary" onclick="saveUser()"><i class="fas fa-save"></i> Salvar</button>
    </div>
  </div>
</div>

<!-- Add Goal Modal -->
<div class="modal-overlay" id="modalGoal">
  <div class="modal">
    <div class="modal-header">
      <h3>Nova Meta de Gasto</h3>
      <button class="btn btn-icon btn-secondary" onclick="closeModal('modalGoal')"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-body">
      <div class="form-group">
        <label>Categoria</label>
        <select id="goalCategory"></select>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Limite Mensal BRL (R$)</label>
          <input type="number" id="goalLimitBRL" placeholder="500" step="0.01">
        </div>
        <div class="form-group">
          <label>Limite Mensal EUR (€)</label>
          <input type="number" id="goalLimitEUR" placeholder="100" step="0.01">
        </div>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeModal('modalGoal')">Cancelar</button>
      <button class="btn btn-primary" onclick="saveGoal()"><i class="fas fa-save"></i> Salvar</button>
    </div>
  </div>
</div>

<!-- Account Movement Modal -->
<div class="modal-overlay" id="modalMovement">
  <div class="modal">
    <div class="modal-header">
      <h3>Nova Movimentação</h3>
      <button class="btn btn-icon btn-secondary" onclick="closeModal('modalMovement')"><i class="fas fa-times"></i></button>
    </div>
    <div class="modal-body">
      <div class="form-row">
        <div class="form-group">
          <label>Descrição *</label>
          <input type="text" id="movDesc" placeholder="Ex: Salário">
        </div>
        <div class="form-group">
          <label>Data *</label>
          <input type="date" id="movDate">
        </div>
      </div>
      <div class="form-row">
        <div class="form-group">
          <label>Valor *</label>
          <input type="number" id="movAmount" placeholder="0,00" step="0.01">
        </div>
        <div class="form-group">
          <label>Tipo</label>
          <select id="movType">
            <option value="credit">Crédito (entrada)</option>
            <option value="debit">Débito (saída)</option>
          </select>
        </div>
      </div>
      <div class="form-group">
        <label>Conta *</label>
        <select id="movAccount"></select>
      </div>
      <div class="form-group">
        <label>Status</label>
        <select id="movStatus">
          <option value="pending">Pendente</option>
          <option value="paid">Confirmado</option>
        </select>
      </div>
    </div>
    <div class="modal-footer">
      <button class="btn btn-secondary" onclick="closeModal('modalMovement')">Cancelar</button>
      <button class="btn btn-primary" onclick="saveMovement()"><i class="fas fa-save"></i> Salvar</button>
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════════
     JAVASCRIPT
═══════════════════════════════════════════ -->
<script>
// ─── STATE ───
let currentUser = null;
let currentUserProfile = null;
let isAdmin = false;
let currentMonth = new Date().getMonth();
let currentYear = new Date().getFullYear();
let chartCurrencyCategory = 'BRL';
let chartCurrencyPerson = 'BRL';

// Data stores
let users = {};
let cards = [];
let accounts = [];
let transactions = [];
let financing = [];
let movements = [];
let goals = [];
let settings = { exchangeRate: 5.50, categories: ['Alimentação','Transporte','Saúde','Educação','Lazer','Moradia','Vestuário','Tecnologia','Viagem','Serviços','Outros'] };

// Chart instances
let chartCat, chartPerson, chartMonthly;

// ─── INIT ───
document.addEventListener('firebaseReady', initApp);
document.addEventListener('DOMContentLoaded', () => {
  if (window._firebaseReady) initApp();
});

function initApp() {
  window._onAuthStateChanged(window._auth, async (user) => {
    document.getElementById('loadingOverlay').classList.add('hidden');
    if (user) {
      currentUser = user;
      await loadUserProfile(user);
      showApp();
    } else {
      showLogin();
    }
  });
}

// ─── AUTH ───
async function handleGoogleLogin() {
  try {
    const provider = new window._GoogleAuthProvider();
    await window._signInWithPopup(window._auth, provider);
  } catch(e) {
    showToast('Erro ao fazer login: ' + e.message, 'error');
  }
}

async function handleLogout() {
  await window._signOut(window._auth);
  currentUser = null; currentUserProfile = null; isAdmin = false;
  showLogin();
}

document.getElementById('btnGoogleLogin').addEventListener('click', handleGoogleLogin);

// ─── USER PROFILE ───
async function loadUserProfile(user) {
  const db = window._db;
  const { doc, getDoc, setDoc } = window._firestore;
  const ref = doc(db, 'users', user.uid);
  let snap = await getDoc(ref);
  if (!snap.exists()) {
    // Check if any users exist; if not, make this user admin
    const { collection, getDocs } = window._firestore;
    const usersSnap = await getDocs(collection(db, 'users'));
    const role = usersSnap.empty ? 'admin' : 'user';
    const profile = { name: user.displayName || user.email, email: user.email, role, photo: user.photoURL || '', whatsapp: '', createdAt: new Date().toISOString() };
    await setDoc(ref, profile);
    currentUserProfile = { id: user.uid, ...profile };
  } else {
    currentUserProfile = { id: user.uid, ...snap.data() };
  }
  isAdmin = currentUserProfile.role === 'admin';
}

// ─── SHOW LOGIN/APP ───
function showLogin() {
  document.getElementById('loginScreen').style.display = 'flex';
  document.getElementById('app').style.display = 'none';
}

async function showApp() {
  document.getElementById('loginScreen').style.display = 'none';
  document.getElementById('app').style.display = 'flex';
  // Update sidebar user info
  const av = document.getElementById('sidebarAvatar');
  if (currentUser.photoURL) {
    av.innerHTML = `<img src="${currentUser.photoURL}" alt="avatar">`;
  } else {
    av.textContent = (currentUserProfile.name || 'U')[0].toUpperCase();
  }
  document.getElementById('sidebarName').textContent = currentUserProfile.name || currentUser.email;
  document.getElementById('sidebarRole').textContent = isAdmin ? '👑 Administrador' : '👤 Usuário';
  if (isAdmin) document.getElementById('adminNav').style.display = 'block';
  
  updateMonthLabel();
  await loadAllData();
  renderDashboard();
}

// ─── DATA LOADING ───
async function loadAllData() {
  const db = window._db;
  const { collection, getDocs, query, where } = window._firestore;

  // Load users (admin sees all)
  const usersSnap = await getDocs(collection(db, 'users'));
  users = {};
  usersSnap.forEach(d => users[d.id] = { id: d.id, ...d.data() });

  // Load settings
  const settSnap = await getDocs(collection(db, 'settings'));
  settSnap.forEach(d => { if(d.id === 'global') settings = { ...settings, ...d.data() }; });

  // Load cards
  let cardsQ = isAdmin ? collection(db, 'cards') : query(collection(db, 'cards'), where('ownerId', '==', currentUser.uid));
  const cardsSnap = await getDocs(cardsQ);
  cards = []; cardsSnap.forEach(d => cards.push({ id: d.id, ...d.data() }));

  // Load accounts
  let accQ = isAdmin ? collection(db, 'accounts') : query(collection(db, 'accounts'), where('ownerId', '==', currentUser.uid));
  const accSnap = await getDocs(accQ);
  accounts = []; accSnap.forEach(d => accounts.push({ id: d.id, ...d.data() }));

  // Load transactions
  let txQ = isAdmin ? collection(db, 'transactions') : query(collection(db, 'transactions'), where('ownerId', '==', currentUser.uid));
  const txSnap = await getDocs(txQ);
  transactions = []; txSnap.forEach(d => transactions.push({ id: d.id, ...d.data() }));

  // Load financing
  let finQ = isAdmin ? collection(db, 'financing') : query(collection(db, 'financing'), where('ownerId', '==', currentUser.uid));
  const finSnap = await getDocs(finQ);
  financing = []; finSnap.forEach(d => financing.push({ id: d.id, ...d.data() }));

  // Load movements
  let movQ = isAdmin ? collection(db, 'movements') : query(collection(db, 'movements'), where('ownerId', '==', currentUser.uid));
  const movSnap = await getDocs(movQ);
  movements = []; movSnap.forEach(d => movements.push({ id: d.id, ...d.data() }));

  // Load goals
  const goalsSnap = await getDocs(collection(db, 'goals'));
  goals = []; goalsSnap.forEach(d => goals.push({ id: d.id, ...d.data() }));

  populateSelects();
}

// ─── POPULATE SELECTS ───
function populateSelects() {
  const personOpts = Object.values(users).map(u => `<option value="${u.id}">${u.name}</option>`).join('');
  const cardOpts = cards.map(c => `<option value="${c.id}">${c.name} **** ${c.digits} (${c.currency})</option>`).join('');
  const catOpts = settings.categories.map(c => `<option value="${c}">${c}</option>`).join('');
  const accOpts = accounts.map(a => `<option value="${a.id}">${a.name} (${a.currency})</option>`).join('');

  // Transaction modal
  setHTML('txPerson', `<option value="${currentUser.uid}">${currentUserProfile.name}</option>` + (isAdmin ? personOpts : ''));
  setHTML('txCard', `<option value="">Sem cartão</option>` + cardOpts);
  setHTML('txCategory', catOpts);
  
  // Card modal
  setHTML('cardOwner', personOpts);
  setHTML('cardParent', `<option value="">Selecione...</option>` + cards.filter(c=>c.type==='principal').map(c=>`<option value="${c.id}">${c.name} **** ${c.digits}</option>`).join(''));
  
  // Account modal
  setHTML('accOwner', personOpts);
  
  // Financing modal
  setHTML('finOwner', personOpts);
  
  // Filters
  setHTML('filterPerson', `<option value="">Todas as pessoas</option>` + Object.values(users).map(u=>`<option value="${u.id}">${u.name}</option>`).join(''));
  setHTML('filterCard', `<option value="">Todos os cartões</option>` + cardOpts);
  setHTML('filterCat', `<option value="">Todas as categorias</option>` + catOpts);
  
  // Goal modal
  setHTML('goalCategory', catOpts);
  
  // WhatsApp
  setHTML('waPerson', `<option value="">Selecione...</option>` + personOpts);
  setHTML('waAccountBRL', `<option value="">Selecione conta BRL...</option>` + accounts.filter(a=>a.currency==='BRL').map(a=>`<option value="${a.id}">${a.name}</option>`).join(''));
  setHTML('waAccountEUR', `<option value="">Selecione conta EUR...</option>` + accounts.filter(a=>a.currency==='EUR').map(a=>`<option value="${a.id}">${a.name}</option>`).join(''));
  
  // Movement modal
  setHTML('movAccount', accOpts);
  
  // Settings
  document.getElementById('exchangeRate').value = settings.exchangeRate;
  renderCategories();
}

// ─── MONTH NAVIGATION ───
function updateMonthLabel() {
  const months = ['Janeiro','Fevereiro','Março','Abril','Maio','Junho','Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'];
  document.getElementById('monthLabel').textContent = `${months[currentMonth]} ${currentYear}`;
}

function changeMonth(dir) {
  currentMonth += dir;
  if (currentMonth > 11) { currentMonth = 0; currentYear++; }
  if (currentMonth < 0) { currentMonth = 11; currentYear--; }
  updateMonthLabel();
  renderDashboard();
  renderTransactions();
}

// ─── GET MONTH TRANSACTIONS ───
function getMonthTransactions(month = currentMonth, year = currentYear) {
  const key = `${year}-${String(month+1).padStart(2,'0')}`;
  return transactions.filter(tx => {
    const txDate = tx.installmentMonth || tx.date?.substring(0,7);
    return txDate === key;
  });
}

// ─── DASHBOARD ───
function renderDashboard() {
  const txs = getMonthTransactions();
  
  // Stats
  const brlTxs = txs.filter(t => t.currency === 'BRL' && t.type !== 'income');
  const eurTxs = txs.filter(t => t.currency === 'EUR' && t.type !== 'income');
  const totalBRL = brlTxs.reduce((s,t) => s + (t.amount||0), 0);
  const totalEUR = eurTxs.reduce((s,t) => s + (t.amount||0), 0);
  
  document.getElementById('statTotalBRL').textContent = formatBRL(totalBRL);
  document.getElementById('statTotalEUR').textContent = formatEUR(totalEUR);
  document.getElementById('statTotalBRLsub').textContent = `${brlTxs.length} transações`;
  document.getElementById('statTotalEURsub').textContent = `${eurTxs.length} transações`;
  
  // Account balance
  const balBRL = accounts.filter(a=>a.currency==='BRL').reduce((s,a)=>s+(a.balance||0),0);
  document.getElementById('statBalance').textContent = formatBRL(balBRL);
  
  // Financing
  const finTotal = financing.reduce((s,f) => {
    const rem = (f.totalInstallments - f.paidInstallments) * f.installmentAmount;
    return s + (f.currency==='BRL' ? rem : rem * settings.exchangeRate);
  }, 0);
  document.getElementById('statFinancing').textContent = formatBRL(finTotal);

  renderAlerts(txs);
  renderCharts(txs);
  renderCardLimits();
}

// ─── ALERTS ───
function renderAlerts(txs) {
  const alerts = [];
  
  // Card limit alerts
  cards.forEach(card => {
    const spent = txs.filter(t => t.cardId === card.id).reduce((s,t) => s + (t.amount||0), 0);
    const pct = card.limit ? (spent / card.limit) * 100 : 0;
    if (pct >= 90) alerts.push({ type:'danger', icon:'fa-exclamation-circle', text:`Cartão ${card.name} **** ${card.digits}: ${pct.toFixed(0)}% do limite usado!` });
    else if (pct >= 75) alerts.push({ type:'warning', icon:'fa-exclamation-triangle', text:`Cartão ${card.name} **** ${card.digits}: ${pct.toFixed(0)}% do limite usado` });
  });
  
  // Due date alerts
  const today = new Date();
  cards.forEach(card => {
    if (card.dueDay) {
      const dueDate = new Date(today.getFullYear(), today.getMonth(), card.dueDay);
      const diff = Math.ceil((dueDate - today) / (1000*60*60*24));
      if (diff >= 0 && diff <= 5) alerts.push({ type:'info', icon:'fa-calendar-alt', text:`Fatura do ${card.name} **** ${card.digits} vence em ${diff} dia(s)` });
    }
  });

  // Goal alerts
  goals.forEach(g => {
    const spent = txs.filter(t => t.category === g.category && t.currency === 'BRL' && t.type !== 'income').reduce((s,t)=>s+(t.amount||0),0);
    if (g.limitBRL && spent > g.limitBRL) alerts.push({ type:'danger', icon:'fa-chart-bar', text:`Meta ultrapassada em "${g.category}": ${formatBRL(spent)} / ${formatBRL(g.limitBRL)}` });
  });
  
  const container = document.getElementById('alertsContainer');
  container.innerHTML = alerts.map(a => `<div class="alert-bar ${a.type}"><i class="fas ${a.icon}"></i>${a.text}</div>`).join('');
}

// ─── CHARTS ───
function renderCharts(txs) {
  renderCategoryChart(txs);
  renderPersonChart(txs);
  renderMonthlyChart();
}

function renderCategoryChart(txs) {
  const curr = chartCurrencyCategory;
  const symbol = curr === 'BRL' ? 'R$' : '€';
  const data = {};
  txs.filter(t => t.currency === curr && t.type !== 'income').forEach(t => {
    data[t.category||'Outros'] = (data[t.category||'Outros']||0) + (t.amount||0);
  });
  const labels = Object.keys(data);
  const values = Object.values(data);
  const colors = ['#4f8cff','#7c3aed','#22c55e','#f59e0b','#ef4444','#06b6d4','#f97316','#8b5cf6','#ec4899','#14b8a6','#84cc16'];
  
  if (chartCat) chartCat.destroy();
  const ctx = document.getElementById('chartCategory').getContext('2d');
  chartCat = new Chart(ctx, {
    type: 'doughnut',
    data: { labels, datasets:[{ data: values, backgroundColor: colors.slice(0,labels.length), borderWidth: 2, borderColor: '#1a2035' }] },
    options: { responsive: true, maintainAspectRatio: false, plugins: { legend:{ position:'right', labels:{ color:'#8b95b0', font:{size:11} }}, tooltip:{ callbacks:{ label: c => ` ${symbol} ${c.parsed.toFixed(2)}` }}}}
  });
}

function renderPersonChart(txs) {
  const curr = chartCurrencyPerson;
  const symbol = curr === 'BRL' ? 'R$' : '€';
  const data = {};
  txs.filter(t => t.currency === curr && t.type !== 'income').forEach(t => {
    const name = users[t.ownerId]?.name || 'Desconhecido';
    data[name] = (data[name]||0) + (t.amount||0);
  });
  const labels = Object.keys(data);
  const values = Object.values(data);
  
  if (chartPerson) chartPerson.destroy();
  const ctx = document.getElementById('chartPerson').getContext('2d');
  chartPerson = new Chart(ctx, {
    type: 'bar',
    data: { labels, datasets:[{ data: values, backgroundColor: curr==='BRL' ? 'rgba(52,211,153,0.7)' : 'rgba(167,139,250,0.7)', borderColor: curr==='BRL' ? '#34d399' : '#a78bfa', borderWidth: 2, borderRadius: 8 }] },
    options: { responsive: true, maintainAspectRatio: false, plugins:{ legend:{display:false}, tooltip:{callbacks:{label: c=>`${symbol} ${c.parsed.y.toFixed(2)}`}}}, scales:{ x:{ticks:{color:'#8b95b0'}}, y:{ticks:{color:'#8b95b0',callback:v=>`${symbol}${v}`},grid:{color:'rgba(42,51,80,0.5)'}}}}
  });
}

function renderMonthlyChart() {
  const labels = [];
  const dataBRL = [], dataEUR = [];
  for (let i = 5; i >= 0; i--) {
    let m = currentMonth - i, y = currentYear;
    if (m < 0) { m += 12; y--; }
    const months = ['Jan','Fev','Mar','Abr','Mai','Jun','Jul','Ago','Set','Out','Nov','Dez'];
    labels.push(`${months[m]}/${y}`);
    const txs = getMonthTransactions(m, y);
    dataBRL.push(txs.filter(t=>t.currency==='BRL'&&t.type!=='income').reduce((s,t)=>s+(t.amount||0),0));
    dataEUR.push(txs.filter(t=>t.currency==='EUR'&&t.type!=='income').reduce((s,t)=>s+(t.amount||0),0));
  }
  
  if (chartMonthly) chartMonthly.destroy();
  const ctx = document.getElementById('chartMonthly').getContext('2d');
  chartMonthly = new Chart(ctx, {
    type: 'line',
    data: { labels, datasets:[
      { label:'BRL', data:dataBRL, borderColor:'#34d399', backgroundColor:'rgba(52,211,153,0.1)', tension:0.4, fill:true, pointBackgroundColor:'#34d399' },
      { label:'EUR', data:dataEUR, borderColor:'#a78bfa', backgroundColor:'rgba(167,139,250,0.1)', tension:0.4, fill:true, pointBackgroundColor:'#a78bfa' }
    ]},
    options:{ responsive:true, maintainAspectRatio:false, plugins:{legend:{labels:{color:'#8b95b0'}}}, scales:{ x:{ticks:{color:'#8b95b0'}}, y:{ticks:{color:'#8b95b0'},grid:{color:'rgba(42,51,80,0.5)'}}}}
  });
}

function renderCardLimits() {
  const txs = getMonthTransactions();
  const container = document.getElementById('cardLimitsContainer');
  if (!cards.length) { container.innerHTML = '<div class="empty-state"><i class="fas fa-credit-card"></i><p>Nenhum cartão cadastrado</p></div>'; return; }
  container.innerHTML = cards.map(card => {
    const spent = txs.filter(t=>t.cardId===card.id).reduce((s,t)=>s+(t.amount||0),0);
    const pct = card.limit ? Math.min((spent/card.limit)*100,100) : 0;
    const cls = pct >= 90 ? 'danger' : pct >= 75 ? 'warn' : 'safe';
    const sym = card.currency === 'BRL' ? 'R$' : '€';
    return `<div style="margin-bottom:1rem;">
      <div style="display:flex;justify-content:space-between;margin-bottom:0.3rem;">
        <span style="font-size:0.85rem;font-weight:600;">${card.name} <span style="font-size:0.75rem;color:var(--text2);">**** ${card.digits}</span></span>
        <span style="font-size:0.8rem;color:var(--text2);">${sym}${spent.toFixed(2)} / ${sym}${(card.limit||0).toFixed(2)}</span>
      </div>
      <div class="progress-bar"><div class="progress-fill ${cls}" style="width:${pct}%"></div></div>
      <div class="progress-label"><span>${pct.toFixed(0)}% usado</span><span>${sym}${((card.limit||0)-spent).toFixed(2)} disponível</span></div>
    </div>`;
  }).join('');
}

function toggleChartCurrency(type) {
  if (type === 'category') {
    chartCurrencyCategory = chartCurrencyCategory === 'BRL' ? 'EUR' : 'BRL';
    document.getElementById('btnCatCurr').textContent = chartCurrencyCategory;
    renderCategoryChart(getMonthTransactions());
  } else {
    chartCurrencyPerson = chartCurrencyPerson === 'BRL' ? 'EUR' : 'BRL';
    document.getElementById('btnPersonCurr').textContent = chartCurrencyPerson;
    renderPersonChart(getMonthTransactions());
  }
}

// ─── TRANSACTIONS ───
function renderTransactions() {
  const filterPerson = document.getElementById('filterPerson').value;
  const filterCard = document.getElementById('filterCard').value;
  const filterCat = document.getElementById('filterCat').value;
  const filterStatus = document.getElementById('filterStatus').value;
  
  let txs = getMonthTransactions();
  if (filterPerson) txs = txs.filter(t=>t.ownerId===filterPerson);
  if (filterCard) txs = txs.filter(t=>t.cardId===filterCard);
  if (filterCat) txs = txs.filter(t=>t.category===filterCat);
  if (filterStatus) txs = txs.filter(t=>t.status===filterStatus);
  
  txs.sort((a,b) => (b.date||'').localeCompare(a.date||''));
  
  const body = document.getElementById('transactionsBody');
  if (!txs.length) { body.innerHTML = `<tr><td colspan="9"><div class="empty-state"><i class="fas fa-receipt"></i><p>Nenhuma transação encontrada</p></div></td></tr>`; return; }
  
  body.innerHTML = txs.map(tx => {
    const card = cards.find(c=>c.id===tx.cardId);
    const owner = users[tx.ownerId];
    const statusBadge = { paid:'<span class="badge badge-green">✓ Pago</span>', pending:'<span class="badge badge-yellow">⏳ Pendente</span>', late:'<span class="badge badge-red">⚠ Atrasado</span>' }[tx.status] || '';
    const inst = tx.installmentOf ? `<span class="installment-badge">${tx.installmentIndex}/${tx.installmentOf}</span>` : '';
    const sym = tx.currency === 'BRL' ? 'R$' : '€';
    const valClass = tx.type === 'income' ? 'curr-brl' : tx.currency === 'EUR' ? 'curr-eur' : 'curr-brl';
    return `<tr>
      <td>${formatDate(tx.date)}</td>
      <td>${tx.description||'—'}${inst}</td>
      <td>${tx.category||'—'}</td>
      <td>${card ? `**** ${card.digits}` : '—'}</td>
      <td>${owner?.name||'—'}</td>
      <td class="${valClass}">${tx.type==='income'?'+':'−'} ${sym} ${(tx.amount||0).toFixed(2)}</td>
      <td>${tx.installmentOf ? `${tx.installmentIndex}/${tx.installmentOf}` : '—'}</td>
      <td>${statusBadge}</td>
      <td>
        <div style="display:flex;gap:0.25rem;">
          ${tx.status!=='paid'?`<button class="btn btn-success btn-sm btn-icon" title="Marcar Pago" onclick="markPaid('${tx.id}')"><i class="fas fa-check"></i></button>`:''}
          <button class="btn btn-secondary btn-sm btn-icon" title="Editar" onclick="editTransaction('${tx.id}')"><i class="fas fa-edit"></i></button>
          <button class="btn btn-danger btn-sm btn-icon" title="Excluir" onclick="deleteTransaction('${tx.id}')"><i class="fas fa-trash"></i></button>
        </div>
      </td>
    </tr>`;
  }).join('');
}

// ─── SAVE TRANSACTION ───
async function saveTransaction() {
  const desc = document.getElementById('txDesc').value.trim();
  const date = document.getElementById('txDate').value;
  const amount = parseFloat(document.getElementById('txAmount').value);
  const currency = document.getElementById('txCurrency').value;
  const category = document.getElementById('txCategory').value;
  const cardId = document.getElementById('txCard').value;
  const ownerId = document.getElementById('txPerson').value;
  const status = document.getElementById('txStatus').value;
  const type = document.getElementById('txType').value;
  const notes = document.getElementById('txNotes').value;
  const txId = document.getElementById('txId').value;
  
  if (!desc || !date || isNaN(amount) || !ownerId) { showToast('Preencha os campos obrigatórios', 'error'); return; }
  
  const db = window._db;
  const { collection, doc, addDoc, updateDoc, setDoc } = window._firestore;
  
  if (type === 'installment') {
    const nInst = parseInt(document.getElementById('txInstallments').value);
    const startMonth = document.getElementById('txInstallmentStart').value;
    if (!nInst || !startMonth) { showToast('Preencha parcelas e mês de início', 'error'); return; }
    const [sy, sm] = startMonth.split('-').map(Number);
    const instAmount = amount / nInst;
    
    for (let i = 0; i < nInst; i++) {
      let m = (sm - 1 + i) % 12;
      let y = sy + Math.floor((sm - 1 + i) / 12);
      const instMonth = `${y}-${String(m+1).padStart(2,'0')}`;
      const txData = { description:desc, date, amount:instAmount, currency, category, cardId, ownerId, status:'pending', type:'installment', installmentOf:nInst, installmentIndex:i+1, installmentMonth:instMonth, notes, createdAt:new Date().toISOString() };
      await addDoc(collection(db, 'transactions'), txData);
    }
    showToast(`${nInst} parcelas criadas com sucesso!`);
  } else {
    const txData = { description:desc, date, amount, currency, category, cardId, ownerId, status, type, notes, installmentMonth:date.substring(0,7), createdAt:new Date().toISOString() };
    if (txId) {
      await updateDoc(doc(db,'transactions',txId), txData);
      showToast('Transação atualizada!');
    } else {
      await addDoc(collection(db,'transactions'), txData);
      showToast('Transação adicionada!');
    }
  }
  
  closeModal('modalTransaction');
  await loadAllData();
  renderDashboard();
  renderTransactions();
}

async function markPaid(id) {
  const { doc, updateDoc } = window._firestore;
  await updateDoc(doc(window._db,'transactions',id), { status:'paid' });
  const tx = transactions.find(t=>t.id===id);
  if (tx) tx.status = 'paid';
  showToast('Marcado como pago! ✓', 'success');
  renderTransactions();
  renderDashboard();
}

async function deleteTransaction(id) {
  if (!confirm('Excluir esta transação?')) return;
  const { doc, deleteDoc } = window._firestore;
  await deleteDoc(doc(window._db,'transactions',id));
  transactions = transactions.filter(t=>t.id!==id);
  showToast('Transação excluída');
  renderTransactions();
  renderDashboard();
}

function editTransaction(id) {
  const tx = transactions.find(t=>t.id===id);
  if (!tx) return;
  document.getElementById('txId').value = tx.id;
  document.getElementById('txDesc').value = tx.description;
  document.getElementById('txDate').value = tx.date;
  document.getElementById('txAmount').value = tx.amount;
  document.getElementById('txCurrency').value = tx.currency;
  document.getElementById('txCategory').value = tx.category;
  document.getElementById('txCard').value = tx.cardId||'';
  document.getElementById('txPerson').value = tx.ownerId;
  document.getElementById('txStatus').value = tx.status;
  document.getElementById('txType').value = tx.type;
  document.getElementById('txNotes').value = tx.notes||'';
  document.getElementById('modalTransactionTitle').textContent = 'Editar Transação';
  openModal('modalTransaction');
}

// ─── CARDS ───
function renderCards() {
  const grid = document.getElementById('cardsGrid');
  if (!cards.length) { grid.innerHTML = '<div class="empty-state"><i class="fas fa-credit-card"></i><p>Nenhum cartão cadastrado</p></div>'; return; }
  const txs = getMonthTransactions();
  grid.innerHTML = cards.map(card => {
    const owner = users[card.ownerId];
    const spent = txs.filter(t=>t.cardId===card.id).reduce((s,t)=>s+(t.amount||0),0);
    const pct = card.limit ? Math.min((spent/card.limit)*100,100) : 0;
    const cls = pct >= 90 ? 'danger' : pct >= 75 ? 'warn' : 'safe';
    const sym = card.currency === 'BRL' ? 'R$' : '€';
    const isAdditional = card.type === 'additional';
    const parentCard = isAdditional ? cards.find(c=>c.id===card.parentId) : null;
    return `<div>
      <div class="credit-card-visual ${card.currency.toLowerCase()}" onclick="editCard('${card.id}')">
        <div>
          <div class="card-bank">${card.bank} ${isAdditional?'<span style="font-size:0.65rem;opacity:0.7;">Adicional</span>':''}</div>
          <div style="font-size:0.75rem;color:rgba(255,255,255,0.5);">${card.name}</div>
        </div>
        <div class="card-number">**** **** **** ${card.digits}</div>
        <div class="card-bottom">
          <div class="card-holder">${owner?.name||'—'}</div>
          <div class="card-limit">
            <div class="limit-val">${sym} ${(card.limit||0).toFixed(2)}</div>
            <div class="limit-label">LIMITE</div>
          </div>
        </div>
        <div class="progress-wrap">
          <div class="progress-bar"><div class="progress-fill ${cls}" style="width:${pct}%"></div></div>
          <div class="progress-label"><span>${sym} ${spent.toFixed(2)} usado</span><span>${pct.toFixed(0)}%</span></div>
        </div>
      </div>
      ${parentCard?`<div style="font-size:0.75rem;color:var(--text3);margin-top:0.4rem;text-align:center;">↑ Vinculado a ${parentCard.name}</div>`:''}
      <div style="display:flex;gap:0.5rem;margin-top:0.5rem;">
        <button class="btn btn-secondary btn-sm" style="flex:1;" onclick="editCard('${card.id}')"><i class="fas fa-edit"></i> Editar</button>
        <button class="btn btn-danger btn-sm btn-icon" onclick="deleteCard('${card.id}')"><i class="fas fa-trash"></i></button>
      </div>
    </div>`;
  }).join('');
}

async function saveCard() {
  const name = document.getElementById('cardName').value.trim();
  const bank = document.getElementById('cardBank').value.trim();
  const digits = document.getElementById('cardDigits').value.trim();
  const currency = document.getElementById('cardCurrency').value;
  const limit = parseFloat(document.getElementById('cardLimit').value)||0;
  const type = document.getElementById('cardType').value;
  const parentId = document.getElementById('cardParent').value;
  const closingDay = parseInt(document.getElementById('cardClosingDay').value)||1;
  const dueDay = parseInt(document.getElementById('cardDueDay').value)||10;
  const ownerId = document.getElementById('cardOwner').value;
  const cardId = document.getElementById('cardId').value;
  
  if (!name||!bank||!digits) { showToast('Preencha os campos obrigatórios','error'); return; }
  
  const db = window._db;
  const { collection, doc, addDoc, updateDoc } = window._firestore;
  const data = { name, bank, digits, currency, limit, type, parentId, closingDay, dueDay, ownerId, updatedAt:new Date().toISOString() };
  
  if (cardId) {
    await updateDoc(doc(db,'cards',cardId), data);
    showToast('Cartão atualizado!');
  } else {
    await addDoc(collection(db,'cards'), data);
    showToast('Cartão adicionado!');
  }
  
  closeModal('modalCard');
  await loadAllData();
  renderCards();
  renderDashboard();
}

function editCard(id) {
  const card = cards.find(c=>c.id===id);
  if (!card) return;
  document.getElementById('cardId').value = card.id;
  document.getElementById('cardName').value = card.name;
  document.getElementById('cardBank').value = card.bank;
  document.getElementById('cardDigits').value = card.digits;
  document.getElementById('cardCurrency').value = card.currency;
  document.getElementById('cardLimit').value = card.limit;
  document.getElementById('cardType').value = card.type;
  document.getElementById('cardParent').value = card.parentId||'';
  document.getElementById('cardClosingDay').value = card.closingDay;
  document.getElementById('cardDueDay').value = card.dueDay;
  document.getElementById('cardOwner').value = card.ownerId;
  document.getElementById('modalCardTitle').textContent = 'Editar Cartão';
  openModal('modalCard');
}

async function deleteCard(id) {
  if (!confirm('Excluir este cartão?')) return;
  const { doc, deleteDoc } = window._firestore;
  await deleteDoc(doc(window._db,'cards',id));
  cards = cards.filter(c=>c.id!==id);
  showToast('Cartão excluído');
  renderCards();
}

// ─── ACCOUNTS ───
function renderAccounts() {
  const grid = document.getElementById('accountsGrid');
  if (!accounts.length) { grid.innerHTML = '<div class="empty-state"><i class="fas fa-university"></i><p>Nenhuma conta cadastrada</p></div>'; return; }
  const typeLabels = { checking:'Conta Corrente', savings:'Poupança', investment:'Investimento' };
  const typeIcons = { checking:'fa-university', savings:'fa-piggy-bank', investment:'fa-chart-line' };
  grid.innerHTML = accounts.map(acc => {
    const owner = users[acc.ownerId];
    const sym = acc.currency === 'BRL' ? 'R$' : '€';
    const balClass = (acc.balance||0) >= 0 ? 'curr-brl' : 'curr-eur';
    return `<div class="card">
      <div style="display:flex;align-items:center;gap:0.75rem;margin-bottom:1rem;">
        <div style="width:44px;height:44px;background:linear-gradient(135deg,var(--accent),var(--accent2));border-radius:12px;display:flex;align-items:center;justify-content:center;font-size:1.1rem;flex-shrink:0;">
          <i class="fas ${typeIcons[acc.type]||'fa-university'}" style="color:white;"></i>
        </div>
        <div>
          <div style="font-weight:700;font-size:0.95rem;">${acc.name}</div>
          <div style="font-size:0.78rem;color:var(--text2);">${acc.bank} · ${typeLabels[acc.type]||acc.type}</div>
        </div>
      </div>
      <div style="font-size:0.78rem;color:var(--text2);margin-bottom:0.25rem;">Saldo Atual</div>
      <div class="stat-value ${balClass}" style="font-size:1.4rem;">${sym} ${(acc.balance||0).toFixed(2)}</div>
      <div style="font-size:0.78rem;color:var(--text2);margin-top:0.5rem;">Titular: ${owner?.name||'—'}</div>
      <div style="display:flex;gap:0.5rem;margin-top:1rem;">
        <button class="btn btn-primary btn-sm" style="flex:1;" onclick="openAddMovement('${acc.id}')"><i class="fas fa-plus"></i> Movimentação</button>
        <button class="btn btn-secondary btn-sm btn-icon" onclick="editAccount('${acc.id}')"><i class="fas fa-edit"></i></button>
        <button class="btn btn-danger btn-sm btn-icon" onclick="deleteAccount('${acc.id}')"><i class="fas fa-trash"></i></button>
      </div>
    </div>`;
  }).join('');
  renderMovements();
}

function renderMovements() {
  const body = document.getElementById('accountMovBody');
  const sorted = [...movements].sort((a,b)=>(b.date||'').localeCompare(a.date||'')).slice(0,50);
  if (!sorted.length) { body.innerHTML = `<tr><td colspan="7"><div class="empty-state"><i class="fas fa-exchange-alt"></i><p>Nenhuma movimentação</p></div></td></tr>`; return; }
  body.innerHTML = sorted.map(mov => {
    const acc = accounts.find(a=>a.id===mov.accountId);
    const sym = acc?.currency==='EUR'?'€':'R$';
    const typeLabel = mov.type==='credit'?'<span class="badge badge-green">↑ Entrada</span>':'<span class="badge badge-red">↓ Saída</span>';
    const statusBadge = mov.status==='paid'?'<span class="badge badge-green">✓ Conf.</span>':'<span class="badge badge-yellow">⏳ Pend.</span>';
    return `<tr>
      <td>${formatDate(mov.date)}</td>
      <td>${mov.description||'—'}</td>
      <td>${acc?.name||'—'}</td>
      <td>${typeLabel}</td>
      <td class="${mov.type==='credit'?'curr-brl':'curr-eur'}">${mov.type==='credit'?'+':'−'} ${sym} ${(mov.amount||0).toFixed(2)}</td>
      <td>${statusBadge}</td>
      <td>
        ${mov.status!=='paid'?`<button class="btn btn-success btn-sm btn-icon" onclick="confirmMovement('${mov.id}')"><i class="fas fa-check"></i></button>`:''}
        <button class="btn btn-danger btn-sm btn-icon" onclick="deleteMovement('${mov.id}')"><i class="fas fa-trash"></i></button>
      </td>
    </tr>`;
  }).join('');
}

async function saveAccount() {
  const name = document.getElementById('accName').value.trim();
  const bank = document.getElementById('accBank').value.trim();
  const type = document.getElementById('accType').value;
  const currency = document.getElementById('accCurrency').value;
  const balance = parseFloat(document.getElementById('accBalance').value)||0;
  const ownerId = document.getElementById('accOwner').value;
  const accId = document.getElementById('accId').value;
  
  if (!name||!bank) { showToast('Preencha os campos obrigatórios','error'); return; }
  
  const db = window._db;
  const { collection, doc, addDoc, updateDoc } = window._firestore;
  const data = { name, bank, type, currency, balance, ownerId, updatedAt:new Date().toISOString() };
  
  if (accId) {
    await updateDoc(doc(db,'accounts',accId), data);
    showToast('Conta atualizada!');
  } else {
    await addDoc(collection(db,'accounts'), data);
    showToast('Conta adicionada!');
  }
  
  closeModal('modalAccount');
  await loadAllData();
  renderAccounts();
}

function editAccount(id) {
  const acc = accounts.find(a=>a.id===id);
  if (!acc) return;
  document.getElementById('accId').value = acc.id;
  document.getElementById('accName').value = acc.name;
  document.getElementById('accBank').value = acc.bank;
  document.getElementById('accType').value = acc.type;
  document.getElementById('accCurrency').value = acc.currency;
  document.getElementById('accBalance').value = acc.balance;
  document.getElementById('accOwner').value = acc.ownerId;
  document.getElementById('modalAccountTitle').textContent = 'Editar Conta';
  openModal('modalAccount');
}

async function deleteAccount(id) {
  if (!confirm('Excluir esta conta?')) return;
  const { doc, deleteDoc } = window._firestore;
  await deleteDoc(doc(window._db,'accounts',id));
  accounts = accounts.filter(a=>a.id!==id);
  showToast('Conta excluída');
  renderAccounts();
}

async function saveMovement() {
  const desc = document.getElementById('movDesc').value.trim();
  const date = document.getElementById('movDate').value;
  const amount = parseFloat(document.getElementById('movAmount').value);
  const type = document.getElementById('movType').value;
  const accountId = document.getElementById('movAccount').value;
  const status = document.getElementById('movStatus').value;
  
  if (!desc||!date||isNaN(amount)||!accountId) { showToast('Preencha todos os campos','error'); return; }
  
  const db = window._db;
  const { collection, doc, addDoc, updateDoc } = window._firestore;
  const data = { description:desc, date, amount, type, accountId, status, ownerId:currentUser.uid, createdAt:new Date().toISOString() };
  await addDoc(collection(db,'movements'), data);
  
  if (status === 'paid') {
    const acc = accounts.find(a=>a.id===accountId);
    if (acc) {
      const newBal = (acc.balance||0) + (type==='credit' ? amount : -amount);
      await updateDoc(doc(db,'accounts',accountId), { balance:newBal });
    }
  }
  
  showToast('Movimentação registrada!');
  closeModal('modalMovement');
  await loadAllData();
  renderAccounts();
}

async function confirmMovement(id) {
  const mov = movements.find(m=>m.id===id);
  if (!mov) return;
  const db = window._db;
  const { doc, updateDoc } = window._firestore;
  await updateDoc(doc(db,'movements',id), { status:'paid' });
  const acc = accounts.find(a=>a.id===mov.accountId);
  if (acc) {
    const newBal = (acc.balance||0) + (mov.type==='credit' ? mov.amount : -mov.amount);
    await updateDoc(doc(db,'accounts',mov.accountId), { balance:newBal });
  }
  showToast('Movimentação confirmada!');
  await loadAllData();
  renderAccounts();
}

async function deleteMovement(id) {
  if (!confirm('Excluir esta movimentação?')) return;
  const { doc, deleteDoc } = window._firestore;
  await deleteDoc(doc(window._db,'movements',id));
  movements = movements.filter(m=>m.id!==id);
  showToast('Movimentação excluída');
  renderMovements();
}

// ─── FINANCING ───
function renderFinancing() {
  const list = document.getElementById('financingList');
  if (!financing.length) { list.innerHTML = '<div class="empty-state"><i class="fas fa-file-invoice-dollar"></i><p>Nenhum financiamento cadastrado</p></div>'; return; }
  list.innerHTML = financing.map(fin => {
    const owner = users[fin.ownerId];
    const sym = fin.currency==='EUR'?'€':'R$';
    const remaining = fin.totalInstallments - fin.paidInstallments;
    const pct = (fin.paidInstallments / fin.totalInstallments) * 100;
    const remainingTotal = remaining * fin.installmentAmount;
    return `<div class="card" style="margin-bottom:1rem;">
      <div style="display:flex;align-items:flex-start;justify-content:space-between;flex-wrap:wrap;gap:0.75rem;">
        <div>
          <div style="font-family:'Syne',sans-serif;font-weight:800;font-size:1.05rem;">${fin.description}</div>
          <div style="font-size:0.8rem;color:var(--text2);">${fin.bank} · Titular: ${owner?.name||'—'}</div>
        </div>
        <div style="display:flex;gap:0.5rem;">
          <button class="btn btn-success btn-sm" onclick="payFinancingInstallment('${fin.id}')"><i class="fas fa-check"></i> Pagar Parcela</button>
          <button class="btn btn-secondary btn-sm btn-icon" onclick="editFinancing('${fin.id}')"><i class="fas fa-edit"></i></button>
          <button class="btn btn-danger btn-sm btn-icon" onclick="deleteFinancing('${fin.id}')"><i class="fas fa-trash"></i></button>
        </div>
      </div>
      <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(140px,1fr));gap:1rem;margin:1rem 0;">
        <div><div style="font-size:0.72rem;color:var(--text2);text-transform:uppercase;">Parcela</div><div style="font-weight:700;">${sym} ${(fin.installmentAmount||0).toFixed(2)}</div></div>
        <div><div style="font-size:0.72rem;color:var(--text2);text-transform:uppercase;">Pagas</div><div style="font-weight:700;">${fin.paidInstallments}/${fin.totalInstallments}</div></div>
        <div><div style="font-size:0.72rem;color:var(--text2);text-transform:uppercase;">Restante</div><div style="font-weight:700;color:var(--yellow);">${sym} ${remainingTotal.toFixed(2)}</div></div>
        <div><div style="font-size:0.72rem;color:var(--text2);text-transform:uppercase;">Juros</div><div style="font-weight:700;">${fin.rate||0}% a.m.</div></div>
      </div>
      <div class="progress-bar"><div class="progress-fill safe" style="width:${pct}%"></div></div>
      <div class="progress-label"><span>${pct.toFixed(0)}% pago</span><span>${remaining} parcelas restantes</span></div>
    </div>`;
  }).join('');
}

async function saveFinancing() {
  const desc = document.getElementById('finDesc').value.trim();
  const bank = document.getElementById('finBank').value.trim();
  const total = parseFloat(document.getElementById('finTotal').value)||0;
  const currency = document.getElementById('finCurrency').value;
  const installmentAmount = parseFloat(document.getElementById('finInstallment').value)||0;
  const totalInstallments = parseInt(document.getElementById('finTotalInstallments').value)||0;
  const paidInstallments = parseInt(document.getElementById('finPaidInstallments').value)||0;
  const rate = parseFloat(document.getElementById('finRate').value)||0;
  const dueDay = parseInt(document.getElementById('finDueDay').value)||15;
  const ownerId = document.getElementById('finOwner').value;
  const finId = document.getElementById('finId').value;
  
  if (!desc||!totalInstallments||!installmentAmount) { showToast('Preencha os campos obrigatórios','error'); return; }
  
  const db = window._db;
  const { collection, doc, addDoc, updateDoc } = window._firestore;
  const data = { description:desc, bank, total, currency, installmentAmount, totalInstallments, paidInstallments, rate, dueDay, ownerId, updatedAt:new Date().toISOString() };
  
  if (finId) {
    await updateDoc(doc(db,'financing',finId), data);
    showToast('Financiamento atualizado!');
  } else {
    await addDoc(collection(db,'financing'), data);
    showToast('Financiamento adicionado!');
  }
  
  closeModal('modalFinancing');
  await loadAllData();
  renderFinancing();
  renderDashboard();
}

async function payFinancingInstallment(id) {
  const fin = financing.find(f=>f.id===id);
  if (!fin) return;
  if (fin.paidInstallments >= fin.totalInstallments) { showToast('Todas as parcelas já foram pagas!', 'error'); return; }
  const { doc, updateDoc } = window._firestore;
  await updateDoc(doc(window._db,'financing',id), { paidInstallments: fin.paidInstallments + 1 });
  showToast('Parcela registrada como paga!');
  await loadAllData();
  renderFinancing();
  renderDashboard();
}

function editFinancing(id) {
  const fin = financing.find(f=>f.id===id);
  if (!fin) return;
  document.getElementById('finId').value = fin.id;
  document.getElementById('finDesc').value = fin.description;
  document.getElementById('finBank').value = fin.bank;
  document.getElementById('finTotal').value = fin.total;
  document.getElementById('finCurrency').value = fin.currency;
  document.getElementById('finInstallment').value = fin.installmentAmount;
  document.getElementById('finTotalInstallments').value = fin.totalInstallments;
  document.getElementById('finPaidInstallments').value = fin.paidInstallments;
  document.getElementById('finRate').value = fin.rate;
  document.getElementById('finDueDay').value = fin.dueDay;
  document.getElementById('finOwner').value = fin.ownerId;
  document.getElementById('modalFinancingTitle').textContent = 'Editar Financiamento';
  openModal('modalFinancing');
}

async function deleteFinancing(id) {
  if (!confirm('Excluir este financiamento?')) return;
  const { doc, deleteDoc } = window._firestore;
  await deleteDoc(doc(window._db,'financing',id));
  financing = financing.filter(f=>f.id!==id);
  showToast('Financiamento excluído');
  renderFinancing();
}

// ─── REPORTS ───
let currentReportTab = 'byPerson';
function showReportTab(tab) {
  currentReportTab = tab;
  document.querySelectorAll('.tab').forEach(t=>t.classList.remove('active'));
  event.target.classList.add('active');
  renderReport();
}

function renderReport() {
  const txs = getMonthTransactions();
  const content = document.getElementById('reportContent');
  
  if (currentReportTab === 'byPerson') {
    const data = {};
    Object.values(users).forEach(u => { data[u.id] = { name:u.name, BRL:0, EUR:0 }; });
    txs.filter(t=>t.type!=='income').forEach(t => {
      if (!data[t.ownerId]) data[t.ownerId] = { name:users[t.ownerId]?.name||'Desconhecido', BRL:0, EUR:0 };
      data[t.ownerId][t.currency] += (t.amount||0);
    });
    content.innerHTML = `<div class="card"><div class="table-wrap"><table>
      <thead><tr><th>Pessoa</th><th>Total BRL</th><th>Total EUR</th></tr></thead>
      <tbody>${Object.values(data).map(d=>`<tr>
        <td><strong>${d.name}</strong></td>
        <td class="curr-brl">${formatBRL(d.BRL)}</td>
        <td class="curr-eur">${formatEUR(d.EUR)}</td>
      </tr>`).join('')}</tbody>
    </table></div></div>`;
  } else if (currentReportTab === 'byCard') {
    content.innerHTML = `<div class="card"><div class="table-wrap"><table>
      <thead><tr><th>Cartão</th><th>Banco</th><th>Dígitos</th><th>Titular</th><th>Total</th></tr></thead>
      <tbody>${cards.map(card=>{
        const spent = txs.filter(t=>t.cardId===card.id).reduce((s,t)=>s+(t.amount||0),0);
        const sym = card.currency==='EUR'?'€':'R$';
        return `<tr><td>${card.name}</td><td>${card.bank}</td><td>**** ${card.digits}</td><td>${users[card.ownerId]?.name||'—'}</td><td class="${card.currency==='EUR'?'curr-eur':'curr-brl'}">${sym} ${spent.toFixed(2)}</td></tr>`;
      }).join('')}</tbody>
    </table></div></div>`;
  } else if (currentReportTab === 'byBank') {
    const data = {};
    txs.filter(t=>t.cardId).forEach(t => {
      const card = cards.find(c=>c.id===t.cardId);
      if (!card) return;
      if (!data[card.bank]) data[card.bank] = { BRL:0, EUR:0 };
      data[card.bank][t.currency] += (t.amount||0);
    });
    content.innerHTML = `<div class="card"><div class="table-wrap"><table>
      <thead><tr><th>Banco</th><th>Total BRL</th><th>Total EUR</th></tr></thead>
      <tbody>${Object.entries(data).map(([bank,d])=>`<tr><td><strong>${bank}</strong></td><td class="curr-brl">${formatBRL(d.BRL)}</td><td class="curr-eur">${formatEUR(d.EUR)}</td></tr>`).join('')}</tbody>
    </table></div></div>`;
  } else if (currentReportTab === 'byCategory') {
    const data = {};
    txs.filter(t=>t.type!=='income').forEach(t => {
      const cat = t.category||'Outros';
      if (!data[cat]) data[cat] = { BRL:0, EUR:0 };
      data[cat][t.currency] += (t.amount||0);
    });
    content.innerHTML = `<div class="card"><div class="table-wrap"><table>
      <thead><tr><th>Categoria</th><th>Total BRL</th><th>Total EUR</th></tr></thead>
      <tbody>${Object.entries(data).sort((a,b)=>(b[1].BRL+b[1].EUR*settings.exchangeRate)-(a[1].BRL+a[1].EUR*settings.exchangeRate)).map(([cat,d])=>`<tr><td><strong>${cat}</strong></td><td class="curr-brl">${formatBRL(d.BRL)}</td><td class="curr-eur">${formatEUR(d.EUR)}</td></tr>`).join('')}</tbody>
    </table></div></div>`;
  }
}

// ─── WHATSAPP ───
function updateWAPreview() {
  const personId = document.getElementById('waPerson').value;
  if (!personId) { document.getElementById('waPreview').textContent = 'Selecione uma pessoa para ver a prévia...'; return; }
  
  const person = users[personId];
  const txs = getMonthTransactions().filter(t => t.ownerId === personId && t.type !== 'income');
  const months = ['Janeiro','Fevereiro','Março','Abril','Maio','Junho','Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'];
  const monthName = `${months[currentMonth]}/${currentYear}`;
  const dueDate = document.getElementById('waDueDate').value;
  const dueDateFormatted = dueDate ? dueDate.split('-').reverse().join('/') : 'A definir';
  const payMethodBRL = document.getElementById('waPayMethodBRL').value;
  const payMethodEUR = document.getElementById('waPayMethodEUR').value;
  const accBRLId = document.getElementById('waAccountBRL').value;
  const accEURId = document.getElementById('waAccountEUR').value;
  const accBRL = accounts.find(a=>a.id===accBRLId);
  const accEUR = accounts.find(a=>a.id===accEURId);
  
  // Get selected cards
  const selectedCards = Array.from(document.querySelectorAll('#waCardsCheckboxes input:checked')).map(cb => cb.value);
  
  let msg = `💎 *Fechamento de Fatura — ${monthName}*\n\nOlá, *${person.name}*! 👋\n\nSegue o resumo dos seus gastos:\n`;
  
  // BRL cards
  const brlCards = cards.filter(c => c.ownerId === personId && c.currency === 'BRL' && (selectedCards.length === 0 || selectedCards.includes(c.id)));
  brlCards.forEach(card => {
    const cardTxs = txs.filter(t => t.cardId === card.id);
    if (!cardTxs.length) return;
    const total = cardTxs.reduce((s,t)=>s+(t.amount||0),0);
    msg += `\n*Cartão ${card.bank} **** ${card.digits} (BRL)*\n`;
    cardTxs.forEach(t => {
      const inst = t.installmentOf ? ` (${t.installmentIndex}/${t.installmentOf})` : '';
      msg += `• ${t.description}${inst}    R$ ${(t.amount||0).toFixed(2)}\n`;
    });
    msg += `${'─'.repeat(25)}\n`;
    msg += `💰 *Total: R$ ${total.toFixed(2)}*\n`;
    msg += `💳 Pagamento: ${payMethodBRL}\n`;
    if (accBRL) msg += `🏦 Conta: ${accBRL.name}\n`;
    msg += `📅 Vencimento: ${dueDateFormatted}\n`;
  });
  
  // EUR cards
  const eurCards = cards.filter(c => c.ownerId === personId && c.currency === 'EUR' && (selectedCards.length === 0 || selectedCards.includes(c.id)));
  if (eurCards.length) msg += '\n' + '━'.repeat(20) + '\n';
  eurCards.forEach(card => {
    const cardTxs = txs.filter(t => t.cardId === card.id);
    if (!cardTxs.length) return;
    const total = cardTxs.reduce((s,t)=>s+(t.amount||0),0);
    msg += `\n*Cartão ${card.bank} **** ${card.digits} (EUR)*\n`;
    cardTxs.forEach(t => {
      const inst = t.installmentOf ? ` (${t.installmentIndex}/${t.installmentOf})` : '';
      msg += `• ${t.description}${inst}    € ${(t.amount||0).toFixed(2)}\n`;
    });
    msg += `${'─'.repeat(25)}\n`;
    msg += `💰 *Total: € ${total.toFixed(2)}*\n`;
    msg += `💳 Pagamento: ${payMethodEUR}\n`;
    if (accEUR) msg += `🏦 Conta: ${accEUR.name}\n`;
    msg += `📅 Vencimento: ${dueDateFormatted}\n`;
  });
  
  msg += `\n${'━'.repeat(20)}\n✅ Qualquer dúvida, entre em contato!`;
  document.getElementById('waPreview').textContent = msg;
}

function updateWACards() {
  const personId = document.getElementById('waPerson').value;
  const container = document.getElementById('waCardsCheckboxes');
  if (!personId) { container.innerHTML = ''; return; }
  const personCards = cards.filter(c=>c.ownerId===personId);
  container.innerHTML = personCards.map(c=>`
    <label style="display:flex;align-items:center;gap:0.5rem;margin-bottom:0.4rem;font-size:0.85rem;cursor:pointer;">
      <input type="checkbox" value="${c.id}" checked onchange="updateWAPreview()" style="width:auto;">
      ${c.name} **** ${c.digits} (${c.currency})
    </label>`).join('');
  updateWAPreview();
}

document.getElementById('waPerson').addEventListener('change', updateWACards);

function sendWhatsApp() {
  const personId = document.getElementById('waPerson').value;
  if (!personId) { showToast('Selecione uma pessoa','error'); return; }
  const person = users[personId];
  if (!person.whatsapp) { showToast(`Nenhum WhatsApp cadastrado para ${person.name}. Configure em Usuários.`,'error'); return; }
  const msg = document.getElementById('waPreview').textContent;
  const url = `https://wa.me/${person.whatsapp}?text=${encodeURIComponent(msg)}`;
  window.open(url, '_blank');
}

// ─── SAVINGS ───
function renderSavings() {
  renderGoals();
  renderSavingsTips();
}

function renderGoals() {
  const list = document.getElementById('goalsList');
  const txs = getMonthTransactions();
  if (!goals.length) { list.innerHTML = '<div class="empty-state"><i class="fas fa-bullseye"></i><p>Nenhuma meta definida</p></div>'; return; }
  list.innerHTML = goals.map(g => {
    const spentBRL = txs.filter(t=>t.category===g.category&&t.currency==='BRL'&&t.type!=='income').reduce((s,t)=>s+(t.amount||0),0);
    const spentEUR = txs.filter(t=>t.category===g.category&&t.currency==='EUR'&&t.type!=='income').reduce((s,t)=>s+(t.amount||0),0);
    const pctBRL = g.limitBRL ? Math.min((spentBRL/g.limitBRL)*100,100) : 0;
    const pctEUR = g.limitEUR ? Math.min((spentEUR/g.limitEUR)*100,100) : 0;
    const clsBRL = pctBRL>=100?'danger':pctBRL>=80?'warn':'safe';
    const clsEUR = pctEUR>=100?'danger':pctEUR>=80?'warn':'safe';
    return `<div style="margin-bottom:1rem;padding-bottom:1rem;border-bottom:1px solid var(--border);">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:0.5rem;">
        <strong style="font-size:0.9rem;">${g.category}</strong>
        <button class="btn btn-danger btn-sm btn-icon" onclick="deleteGoal('${g.id}')"><i class="fas fa-trash"></i></button>
      </div>
      ${g.limitBRL?`<div style="font-size:0.78rem;color:var(--text2);margin-bottom:0.25rem;">BRL: R$ ${spentBRL.toFixed(2)} / R$ ${g.limitBRL.toFixed(2)}</div>
      <div class="progress-bar"><div class="progress-fill ${clsBRL}" style="width:${pctBRL}%"></div></div>`:''}
      ${g.limitEUR?`<div style="font-size:0.78rem;color:var(--text2);margin:0.4rem 0 0.25rem;">EUR: € ${spentEUR.toFixed(2)} / € ${g.limitEUR.toFixed(2)}</div>
      <div class="progress-bar"><div class="progress-fill ${clsEUR}" style="width:${pctEUR}%"></div></div>`:''}
    </div>`;
  }).join('');
}

function renderSavingsTips() {
  const txs = getMonthTransactions();
  const tips = [];
  const totalBRL = txs.filter(t=>t.currency==='BRL'&&t.type!=='income').reduce((s,t)=>s+(t.amount||0),0);
  const totalEUR = txs.filter(t=>t.currency==='EUR'&&t.type!=='income').reduce((s,t)=>s+(t.amount||0),0);
  
  // Category analysis
  const catData = {};
  txs.filter(t=>t.type!=='income').forEach(t => { catData[t.category||'Outros'] = (catData[t.category||'Outros']||0) + t.amount * (t.currency==='EUR'?settings.exchangeRate:1); });
  const topCat = Object.entries(catData).sort((a,b)=>b[1]-a[1])[0];
  if (topCat) tips.push({ icon:'📊', title:`Maior gasto: ${topCat[0]}`, text:`Esta categoria representou R$ ${topCat[1].toFixed(2)} dos seus gastos este mês. Considere estabelecer um limite mensal para ela.` });
  
  if (totalBRL > 0) tips.push({ icon:'💡', title:'Regra dos 50/30/20', text:`Tente destinar 50% da renda para necessidades, 30% para desejos e 20% para poupança e investimentos.` });
  
  const pendingTxs = txs.filter(t=>t.status==='pending').length;
  if (pendingTxs > 0) tips.push({ icon:'⚠️', title:`${pendingTxs} pagamentos pendentes`, text:`Você tem transações pendentes este mês. Evite atrasos para não pagar juros desnecessários.` });
  
  const installmentTotal = txs.filter(t=>t.installmentOf).reduce((s,t)=>s+(t.amount||0)*(t.currency==='EUR'?settings.exchangeRate:1),0);
  if (installmentTotal > 0) tips.push({ icon:'🔄', title:`Parcelamentos: R$ ${installmentTotal.toFixed(2)}`, text:`Você tem compras parceladas comprometendo a renda futura. Avalie se é possível quitá-las antecipadamente.` });
  
  tips.push({ icon:'🎯', title:'Defina metas de economia', text:`Use o botão "+ Meta" para definir limites de gasto por categoria e receber alertas quando se aproximar do limite.` });
  
  const container = document.getElementById('savingsTips');
  container.innerHTML = tips.map(t=>`<div class="tip-card"><span class="tip-icon">${t.icon}</span><div class="tip-text"><h4>${t.title}</h4><p>${t.text}</p></div></div>`).join('');
}

async function saveGoal() {
  const category = document.getElementById('goalCategory').value;
  const limitBRL = parseFloat(document.getElementById('goalLimitBRL').value)||0;
  const limitEUR = parseFloat(document.getElementById('goalLimitEUR').value)||0;
  
  const db = window._db;
  const { collection, addDoc } = window._firestore;
  await addDoc(collection(db,'goals'), { category, limitBRL, limitEUR, createdAt:new Date().toISOString() });
  showToast('Meta adicionada!');
  closeModal('modalGoal');
  await loadAllData();
  renderSavings();
}

async function deleteGoal(id) {
  const { doc, deleteDoc } = window._firestore;
  await deleteDoc(doc(window._db,'goals',id));
  goals = goals.filter(g=>g.id!==id);
  showToast('Meta excluída');
  renderSavings();
}

// ─── USERS ───
function renderUsers() {
  const body = document.getElementById('usersBody');
  body.innerHTML = Object.values(users).map(u=>`<tr>
    <td><strong>${u.name}</strong></td>
    <td>${u.email}</td>
    <td>${u.role==='admin'?'<span class="badge badge-purple">👑 Admin</span>':'<span class="badge badge-blue">👤 Usuário</span>'}</td>
    <td>${u.whatsapp||'<span style="color:var(--text3)">Não cadastrado</span>'}</td>
    <td>${cards.filter(c=>c.ownerId===u.id).length} cartão(ões)</td>
    <td>
      <button class="btn btn-secondary btn-sm btn-icon" onclick="editUser('${u.id}')"><i class="fas fa-edit"></i></button>
    </td>
  </tr>`).join('');
}

async function saveUser() {
  const name = document.getElementById('userName').value.trim();
  const email = document.getElementById('userEmail').value.trim();
  const role = document.getElementById('userRole').value;
  const whatsapp = document.getElementById('userWhatsApp').value.trim();
  const userId = document.getElementById('userId').value;
  
  if (!name||!email) { showToast('Preencha nome e email','error'); return; }
  
  const db = window._db;
  const { collection, doc, addDoc, setDoc } = window._firestore;
  const data = { name, email, role, whatsapp, updatedAt:new Date().toISOString() };
  
  if (userId) {
    await setDoc(doc(db,'users',userId), data, { merge:true });
    showToast('Usuário atualizado!');
  } else {
    await addDoc(collection(db,'users'), { ...data, createdAt:new Date().toISOString() });
    showToast('Usuário adicionado!');
  }
  
  closeModal('modalUser');
  await loadAllData();
  renderUsers();
}

function editUser(id) {
  const u = users[id];
  if (!u) return;
  document.getElementById('userId').value = id;
  document.getElementById('userName').value = u.name;
  document.getElementById('userEmail').value = u.email;
  document.getElementById('userRole').value = u.role;
  document.getElementById('userWhatsApp').value = u.whatsapp||'';
  document.getElementById('modalUserTitle').textContent = 'Editar Usuário';
  openModal('modalUser');
}

// ─── SETTINGS ───
async function saveExchangeRate() {
  const rate = parseFloat(document.getElementById('exchangeRate').value);
  if (!rate) { showToast('Taxa inválida','error'); return; }
  const db = window._db;
  const { doc, setDoc } = window._firestore;
  await setDoc(doc(db,'settings','global'), { exchangeRate: rate }, { merge:true });
  settings.exchangeRate = rate;
  showToast('Taxa de câmbio atualizada!');
}

function renderCategories() {
  const list = document.getElementById('categoriesList');
  list.innerHTML = settings.categories.map((cat,i) => `
    <div style="display:flex;align-items:center;justify-content:space-between;padding:0.4rem 0;border-bottom:1px solid var(--border);">
      <span style="font-size:0.875rem;">${cat}</span>
      <button class="btn btn-danger btn-sm btn-icon" onclick="removeCategory(${i})"><i class="fas fa-times"></i></button>
    </div>`).join('');
}

async function addCategory() {
  const val = document.getElementById('newCategory').value.trim();
  if (!val) return;
  settings.categories.push(val);
  document.getElementById('newCategory').value = '';
  const db = window._db;
  const { doc, setDoc } = window._firestore;
  await setDoc(doc(db,'settings','global'), { categories: settings.categories }, { merge:true });
  showToast('Categoria adicionada!');
  renderCategories();
  populateSelects();
}

async function removeCategory(index) {
  settings.categories.splice(index, 1);
  const db = window._db;
  const { doc, setDoc } = window._firestore;
  await setDoc(doc(db,'settings','global'), { categories: settings.categories }, { merge:true });
  showToast('Categoria removida');
  renderCategories();
  populateSelects();
}

// ─── CSV IMPORT ───
function importCSV() { document.getElementById('csvInput').click(); }
function handleCSVImport(event) {
  const file = event.target.files[0];
  if (!file) return;
  const reader = new FileReader();
  reader.onload = async e => {
    const text = e.target.result;
    const lines = text.split('\n').filter(l=>l.trim());
    const headers = lines[0].split(',').map(h=>h.trim().toLowerCase());
    let count = 0;
    const db = window._db;
    const { collection, addDoc } = window._firestore;
    for (let i = 1; i < lines.length; i++) {
      const cols = lines[i].split(',');
      const row = {};
      headers.forEach((h,j) => row[h] = cols[j]?.trim().replace(/"/g,''));
      const amount = parseFloat(row.valor||row.amount||row.value||0);
      const date = row.data||row.date||new Date().toISOString().substring(0,10);
      if (!amount) continue;
      await addDoc(collection(db,'transactions'), {
        description: row.descricao||row.description||row.historico||'Importado',
        date: date.substring(0,10),
        amount: Math.abs(amount),
        currency: 'BRL',
        category: 'Outros',
        type: amount < 0 ? 'expense' : 'income',
        status: 'paid',
        ownerId: currentUser.uid,
        installmentMonth: date.substring(0,7),
        createdAt: new Date().toISOString()
      });
      count++;
    }
    showToast(`${count} transações importadas!`);
    await loadAllData();
    renderTransactions();
    renderDashboard();
  };
  reader.readAsText(file);
}

// ─── EXCEL EXPORT ───
function exportExcel() {
  const txs = getMonthTransactions();
  const data = txs.map(t => ({
    'Data': t.date,
    'Descrição': t.description,
    'Categoria': t.category,
    'Cartão': cards.find(c=>c.id===t.cardId)?.name||'—',
    'Dígitos': cards.find(c=>c.id===t.cardId)?.digits||'—',
    'Pessoa': users[t.ownerId]?.name||'—',
    'Moeda': t.currency,
    'Valor': t.amount,
    'Parcela': t.installmentOf ? `${t.installmentIndex}/${t.installmentOf}` : '—',
    'Status': t.status==='paid'?'Pago':t.status==='late'?'Atrasado':'Pendente',
    'Tipo': t.type==='income'?'Receita':'Despesa'
  }));
  const ws = XLSX.utils.json_to_sheet(data);
  const wb = XLSX.utils.book_new();
  XLSX.utils.book_append_sheet(wb, ws, 'Transações');
  const months = ['Jan','Fev','Mar','Abr','Mai','Jun','Jul','Ago','Set','Out','Nov','Dez'];
  XLSX.writeFile(wb, `FinanceHub_${months[currentMonth]}_${currentYear}.xlsx`);
  showToast('Excel exportado!');
}

// ─── PAGE NAVIGATION ───
const pageTitles = { dashboard:'Dashboard', transactions:'Transações', cards:'Cartões', accounts:'Contas Bancárias', financing:'Financiamentos', reports:'Relatórios', whatsapp:'WhatsApp', savings:'Economizar', users:'Usuários', settings:'Configurações' };

function showPage(page) {
  document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n=>n.classList.remove('active'));
  document.getElementById(`page-${page}`).classList.add('active');
  document.querySelector(`.nav-item[onclick="showPage('${page}')"]`)?.classList.add('active');
  document.getElementById('topbarTitle').textContent = pageTitles[page]||page;
  closeSidebar();
  
  switch(page) {
    case 'dashboard': renderDashboard(); break;
    case 'transactions': renderTransactions(); break;
    case 'cards': renderCards(); break;
    case 'accounts': renderAccounts(); break;
    case 'financing': renderFinancing(); break;
    case 'reports': renderReport(); break;
    case 'whatsapp': break;
    case 'savings': renderSavings(); break;
    case 'users': if(isAdmin) renderUsers(); break;
    case 'settings': if(isAdmin) renderCategories(); break;
  }
}

// ─── MODAL HELPERS ───
function openModal(id) { document.getElementById(id).classList.add('open'); }
function closeModal(id) { document.getElementById(id).classList.remove('open'); }

function openAddTransaction() {
  document.getElementById('txId').value = '';
  document.getElementById('txDesc').value = '';
  document.getElementById('txDate').value = new Date().toISOString().substring(0,10);
  document.getElementById('txAmount').value = '';
  document.getElementById('txCurrency').value = 'BRL';
  document.getElementById('txCategory').value = settings.categories[0]||'';
  document.getElementById('txCard').value = '';
  document.getElementById('txPerson').value = currentUser.uid;
  document.getElementById('txStatus').value = 'pending';
  document.getElementById('txType').value = 'expense';
  document.getElementById('txNotes').value = '';
  document.getElementById('txInstallmentStart').value = `${currentYear}-${String(currentMonth+1).padStart(2,'0')}`;
  document.getElementById('installmentFields').style.display = 'none';
  document.getElementById('modalTransactionTitle').textContent = 'Nova Transação';
  openModal('modalTransaction');
}

function openAddCard() {
  document.getElementById('cardId').value = '';
  document.getElementById('cardName').value = '';
  document.getElementById('cardBank').value = '';
  document.getElementById('cardDigits').value = '';
  document.getElementById('cardLimit').value = '';
  document.getElementById('cardType').value = 'principal';
  document.getElementById('cardClosingDay').value = '';
  document.getElementById('cardDueDay').value = '';
  document.getElementById('modalCardTitle').textContent = 'Novo Cartão';
  openModal('modalCard');
}

function openAddAccount() {
  document.getElementById('accId').value = '';
  document.getElementById('accName').value = '';
  document.getElementById('accBank').value = '';
  document.getElementById('accBalance').value = '0';
  document.getElementById('modalAccountTitle').textContent = 'Nova Conta Bancária';
  openModal('modalAccount');
}

function openAddFinancing() {
  document.getElementById('finId').value = '';
  document.getElementById('finDesc').value = '';
  document.getElementById('finBank').value = '';
  document.getElementById('finTotal').value = '';
  document.getElementById('finInstallment').value = '';
  document.getElementById('finTotalInstallments').value = '';
  document.getElementById('finPaidInstallments').value = '0';
  document.getElementById('finRate').value = '';
  document.getElementById('modalFinancingTitle').textContent = 'Novo Financiamento';
  openModal('modalFinancing');
}

function openAddUser() {
  document.getElementById('userId').value = '';
  document.getElementById('userName').value = '';
  document.getElementById('userEmail').value = '';
  document.getElementById('userRole').value = 'user';
  document.getElementById('userWhatsApp').value = '';
  document.getElementById('modalUserTitle').textContent = 'Adicionar Usuário';
  openModal('modalUser');
}

function openAddGoal() {
  setHTML('goalCategory', settings.categories.map(c=>`<option value="${c}">${c}</option>`).join(''));
  document.getElementById('goalLimitBRL').value = '';
  document.getElementById('goalLimitEUR').value = '';
  openModal('modalGoal');
}

function openAddMovement(accountId) {
  document.getElementById('movDesc').value = '';
  document.getElementById('movDate').value = new Date().toISOString().substring(0,10);
  document.getElementById('movAmount').value = '';
  document.getElementById('movType').value = 'credit';
  document.getElementById('movStatus').value = 'paid';
  if (accountId) document.getElementById('movAccount').value = accountId;
  openModal('modalMovement');
}

function toggleInstallments() {
  const type = document.getElementById('txType').value;
  document.getElementById('installmentFields').style.display = type === 'installment' ? 'block' : 'none';
}

// ─── SIDEBAR MOBILE ───
function toggleSidebar() {
  document.getElementById('sidebar').classList.toggle('open');
  document.getElementById('sidebarOverlay').classList.toggle('open');
}
function closeSidebar() {
  document.getElementById('sidebar').classList.remove('open');
  document.getElementById('sidebarOverlay').classList.remove('open');
}

// ─── TOAST ───
function showToast(msg, type='success') {
  const container = document.getElementById('toastContainer');
  const icon = type==='success' ? 'fa-check-circle' : 'fa-exclamation-circle';
  const toast = document.createElement('div');
  toast.className = `toast ${type}`;
  toast.innerHTML = `<i class="fas ${icon}"></i><span>${msg}</span>`;
  container.appendChild(toast);
  setTimeout(() => toast.remove(), 3500);
}

// ─── HELPERS ───
function setHTML(id, html) { const el = document.getElementById(id); if(el) el.innerHTML = html; }
function formatBRL(v) { return `R$ ${(v||0).toLocaleString('pt-BR',{minimumFractionDigits:2,maximumFractionDigits:2})}`; }
function formatEUR(v) { return `€ ${(v||0).toLocaleString('pt-PT',{minimumFractionDigits:2,maximumFractionDigits:2})}`; }
function formatDate(d) { if(!d) return '—'; const [y,m,day]=d.split('-'); return `${day}/${m}/${y}`; }

// Close modals clicking overlay
document.querySelectorAll('.modal-overlay').forEach(overlay => {
  overlay.addEventListener('click', e => { if(e.target===overlay) overlay.classList.remove('open'); });
});
</script>
</body>
</html>
