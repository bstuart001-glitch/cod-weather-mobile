const COD_HOME = 'https://weather.cod.edu/forecast/';
const models = [
  { name: 'HRRR', note: 'Short range / severe' },
  { name: 'RAP', note: 'Short range' },
  { name: 'NAM', note: 'Medium range' },
  { name: 'NAM Nest', note: 'High-res NAM' },
  { name: 'GFS', note: 'Long range' },
  { name: 'ECMWF', note: 'Long range' },
  { name: 'GEFS', note: 'Ensemble' },
  { name: 'COD Home', note: 'All models' }
];

const grid = document.getElementById('modelGrid');
const viewer = document.getElementById('viewer');
const notice = document.getElementById('viewerNotice');
const openExternal = document.getElementById('openExternal');
const favoritesList = document.getElementById('favoritesList');
const form = document.getElementById('favoriteForm');
const nameInput = document.getElementById('favoriteName');
const urlInput = document.getElementById('favoriteUrl');
const clearBtn = document.getElementById('clearFavorites');

function openUrl(url) {
  openExternal.href = url;
  notice.textContent = 'Loading COD. If the frame stays blank, tap “Open full site.”';
  viewer.style.display = 'block';
  viewer.src = url;
}

models.forEach(m => {
  const btn = document.createElement('button');
  btn.className = 'model';
  btn.innerHTML = `${m.name}<span>${m.note}</span>`;
  btn.addEventListener('click', () => openUrl(COD_HOME));
  grid.appendChild(btn);
});
openExternal.href = COD_HOME;

function getFavorites(){
  try { return JSON.parse(localStorage.getItem('codFavorites') || '[]'); }
  catch { return []; }
}
function setFavorites(items){ localStorage.setItem('codFavorites', JSON.stringify(items)); renderFavorites(); }
function renderFavorites(){
  const items = getFavorites();
  favoritesList.innerHTML = '';
  if (!items.length) {
    favoritesList.innerHTML = '<p class="small">No favorites saved yet.</p>';
    return;
  }
  items.forEach((item, index) => {
    const row = document.createElement('div'); row.className='fav';
    row.innerHTML = `<div><b>${escapeHtml(item.name)}</b><small>${escapeHtml(item.url)}</small></div>`;
    const actions = document.createElement('div'); actions.className='fav-actions';
    const open = document.createElement('button'); open.textContent='Open'; open.onclick=()=>openUrl(item.url);
    const del = document.createElement('button'); del.textContent='Del'; del.className='delete'; del.onclick=()=>{ const next=getFavorites(); next.splice(index,1); setFavorites(next); };
    actions.append(open,del); row.appendChild(actions); favoritesList.appendChild(row);
  });
}
function escapeHtml(s){ return s.replace(/[&<>'"]/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;',"'":'&#39;','"':'&quot;'}[c])); }
form.addEventListener('submit', e => {
  e.preventDefault();
  const name = nameInput.value.trim(); const url = urlInput.value.trim();
  if (!name || !url) return;
  if (!/^https:\/\/weather\.cod\.edu\//i.test(url)) { alert('Please use a weather.cod.edu URL.'); return; }
  setFavorites([{name,url}, ...getFavorites()].slice(0,25));
  nameInput.value=''; urlInput.value='';
});
clearBtn.addEventListener('click', () => { if(confirm('Clear saved favorites?')) setFavorites([]); });
renderFavorites();

document.getElementById('installHelp').onclick = () => document.getElementById('installDialog').showModal();
document.getElementById('closeDialog').onclick = () => document.getElementById('installDialog').close();

if ('serviceWorker' in navigator) navigator.serviceWorker.register('./service-worker.js');
