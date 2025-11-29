<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kalkulator Keuntungan & ROAS — Simple</title>
  <style>
    :root{--bg:#f6f7fb;--card:#ffffff;--accent:#0f172a;--muted:#6b7280;--glass:rgba(15,23,42,0.04)}
    *{box-sizing:border-box}
    body{margin:0;font-family:Inter,ui-sans-serif,system-ui, -apple-system, 'Segoe UI', Roboto; background:var(--bg); color:var(--accent); -webkit-font-smoothing:antialiased}
    .wrap{max-width:980px;margin:18px auto;padding:18px}
    header{display:flex;gap:12px;align-items:center;margin-bottom:14px}
    .logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,#7c3aed,#06b6d4);display:flex;align-items:center;justify-content:center;color:white;font-weight:700}
    h1{font-size:1.15rem;margin:0}
    p.lead{margin:4px 0 0;color:var(--muted);font-size:0.95rem}
    .grid{display:grid;grid-template-columns:1fr;gap:14px}
    @media(min-width:880px){.grid{grid-template-columns:1fr 420px}}
    .card{background:var(--card);border-radius:12px;padding:14px;box-shadow:0 6px 18px rgba(15,23,42,0.06)}
    label{display:block;font-size:0.85rem;margin-bottom:6px;color:var(--muted)}
    input[type="number"], input[type="text"], select{width:100%;padding:10px;border-radius:8px;border:1px solid #e6e9f2;font-size:0.95rem}
    .row{display:flex;gap:10px}
    .row > *{flex:1}
    button.btn{background:#0ea5a4;color:white;padding:10px 12px;border-radius:8px;border:0;font-weight:600;cursor:pointer}
    .muted{color:var(--muted);font-size:0.9rem}
    table{width:100%;border-collapse:collapse;font-size:0.9rem}
    th,td{padding:8px;border-bottom:1px solid #f1f5f9;text-align:left}
    th{background:var(--glass);font-weight:700}
    .small{font-size:0.82rem}
    .result{background:linear-gradient(90deg,#ecfeff,#f0f9ff);padding:12px;border-radius:10px;margin-top:8px;font-weight:700}
    .download{background:#111827;color:white;padding:8px 10px;border-radius:8px;border:0}
    .section-title{display:flex;justify-content:space-between;align-items:center}
    .chips{display:flex;gap:8px;flex-wrap:wrap}
    .chip{background:#f1f5f9;padding:6px 8px;border-radius:999px;font-size:0.85rem}
    footer{margin-top:14px;color:var(--muted);font-size:0.85rem;text-align:left}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="logo">KK</div>
      <div>
        <h1>Kalkulator Keuntungan & ROAS</h1>
        <p class="lead">Mobile-friendly, rapi, dan berisi tabel permanen fee platform & event.</p>
      </div>
    </header>

    <div class="grid">
      <div class="card">
        <div class="section-title">
          <div>
            <h3 style="margin:0">Kalkulator Harga Jual & Keuntungan</h3>
            <div class="muted small">Hitung harga jual yang harus ditetapkan untuk mencapai keuntungan yang diinginkan.</div>
          </div>
          <div class="chips">
            <div class="chip">Mobile-first</div>
            <div class="chip">Non-edit tabel</div>
          </div>
        </div>

        <div style="margin-top:12px">
          <label>HPP produk (Rp)</label>
          <input id="hpp" type="number" value="20000" min="0">
        </div>

        <div style="margin-top:12px">
          <label>Biaya packing & tambahan (Rp)</label>
          <input id="packing" type="number" value="2000" min="0">
        </div>

        <div style="margin-top:12px" class="row">
          <div>
            <label>Pilih platform</label>
            <select id="platform"></select>
          </div>
          <div>
            <label>Kategori</label>
            <select id="kategori"></select>
          </div>
        </div>

        <div style="margin-top:12px" class="row">
          <div>
            <label>Estimasi biaya affiliate (Rp)</label>
            <input id="affiliateRp" type="number" value="0" min="0">
          </div>
          <div>
            <label>atau affiliate (%)</label>
            <input id="affiliatePct" type="number" value="0" min="0" max="100">
          </div>
        </div>

        <div style="margin-top:12px">
          <label>Fee event tambahan (pilih, bisa lebih dari 1)</label>
          <div id="events" style="display:flex;gap:8px;flex-wrap:wrap;margin-top:8px"></div>
        </div>

        <div style="margin-top:12px">
          <label>Keuntungan yang diinginkan</label>
          <div class="row">
            <input id="profitRp" type="number" placeholder="Rp (contoh 10000)" value="10000">
            <input id="profitPct" type="number" placeholder="% dari modal" value="20">
          </div>
          <div class="muted small" style="margin-top:6px">Isi salah satu: jika mengisi % maka Rp akan dihitung dari (HPP+packing+fees).</div>
        </div>

        <div style="margin-top:12px;display:flex;gap:8px">
          <button class="btn" onclick="calcPrice()">Hitung Harga Jual</button>
          <button class="download" onclick="downloadHTML()">Download HTML</button>
        </div>

        <div id="priceResult" class="result" style="display:none;margin-top:12px"></div>
      </div>

      <div class="card">
        <h3 style="margin:0">Kalkulator ROAS / Iklan</h3>
        <div class="muted small">Hitung berapa banyak penjualan / ROAS yang diperlukan untuk mencapai target keuntungan harian.</div>

        <div style="margin-top:10px">
          <label>Harga jual produk (Rp)</label>
          <input id="sellPrice" type="number" value="50000">
        </div>

        <div style="margin-top:10px" class="row">
          <div>
            <label>Biaya komisi affiliate (%)</label>
            <input id="affPctROAS" type="number" value="5">
          </div>
          <div>
            <label>Biaya potongan admin per produk (%)</label>
            <input id="adminPctROAS" type="number" value="1">
          </div>
        </div>

        <div style="margin-top:10px" class="row">
          <div>
            <label>Modal iklan per hari (Rp)</label>
            <input id="adPerDay" type="number" value="100000">
          </div>
          <div>
            <label>HPP produk (Rp)</label>
            <input id="hppROAS" type="number" value="20000">
          </div>
        </div>

        <div style="margin-top:10px">
          <label>Target keuntungan per hari (Rp)</label>
          <input id="targetProfitDay" type="number" value="200000">
        </div>

        <div style="margin-top:10px;display:flex;gap:8px">
          <button class="btn" onclick="calcROAS()">Hitung ROAS</button>
        </div>

        <div id="roasResult" class="result" style="display:none;margin-top:12px"></div>

        <hr style="margin:12px 0;border:none;border-top:1px solid #f1f5f9">
        <h4 style="margin:0 0 8px 0">Tabel Fee Permanen — Platform x Kategori</h4>
        <div style="max-height:220px;overflow:auto">
          <table id="feeTable"></table>
        </div>

        <h4 style="margin-top:12px;margin-bottom:6px">Tabel Event (Permanen)</h4>
        <div style="max-height:160px;overflow:auto">
          <table id="eventTable"></table>
        </div>
      </div>
    </div>

    <footer>
      Tip: isi satu-satu, gunakan tombol "Download HTML" untuk menyimpan file ini di HP Anda.
    </footer>
  </div>

<script>
// --- Data: platform x kategori fees (persen) ---
const data = {
  platforms: ["Shopee","Tokopedia","TikTok Shop","Lazada"],
  categories: ["Pakaian","Makanan","Elektronik","Aksesoris","Kecantikan"],
  fees: {
    "Shopee": {"Pakaian": 3, "Makanan": 5, "Elektronik": 4, "Aksesoris":3.5, "Kecantikan":3},
    "Tokopedia": {"Pakaian": 4, "Makanan": 6, "Elektronik": 4.5, "Aksesoris":4, "Kecantikan":3.8},
    "TikTok Shop": {"Pakaian": 2.5, "Makanan": 5, "Elektronik": 5, "Aksesoris":3, "Kecantikan":2.5},
    "Lazada": {"Pakaian": 4, "Makanan": 6, "Elektronik": 4.5, "Aksesoris":4, "Kecantikan":3.5}
  },
  events: [
    {id:'free_ongkir', name:'Gratis Ongkir', type:'fixed', value:5000, desc:'Subsidi ongkir (nilai estimasi Rp) — tambahkan jika mengikuti'},
    {id:'tanggal_kembar', name:'Tanggal Kembar', type:'pct', value:5, desc:'Promo event besar, tambahan fee % dari harga jual'},
    {id:'event_gajian', name:'Event Gajian', type:'pct', value:3, desc:'Event gajian, tambahan fee %'},
    {id:'flash_sale', name:'Flash Sale', type:'pct', value:4, desc:'Potongan/biaya event dalam %'}
  ]
};

// Populate selects and tables
const platSel = document.getElementById('platform');
const catSel = document.getElementById('kategori');
for(const p of data.platforms){ let o = document.createElement('option'); o.value=p; o.textContent=p; platSel.appendChild(o)}
for(const c of data.categories){ let o = document.createElement('option'); o.value=c; o.textContent=c; catSel.appendChild(o)}

// events chips
const evWrap = document.getElementById('events');
for(const e of data.events){
  const btn = document.createElement('label');
  btn.style.display='inline-flex'; btn.style.alignItems='center'; btn.style.gap='8px'; btn.style.padding='6px 8px'; btn.style.borderRadius='8px'; btn.style.background='#f8fafc';
  const input = document.createElement('input'); input.type='checkbox'; input.value=e.id; input.dataset.type=e.type; input.dataset.val=e.value; input.style.width='16px';
  const span = document.createElement('span'); span.textContent = e.name; span.className='small';
  btn.appendChild(input); btn.appendChild(span);
  evWrap.appendChild(btn);
}

function buildFeeTable(){
  const t = document.getElementById('feeTable');
  let html = '<thead><tr><th>Platform \ Kategori</th>' + data.categories.map(c=>`<th>${c}</th>`).join('') + '</tr></thead><tbody>';
  for(const p of data.platforms){
    html += `<tr><th>${p}</th>` + data.categories.map(c=>`<td>${data.fees[p][c]}%</td>`).join('') + '</tr>`;
  }
  html += '</tbody>';
  t.innerHTML = html;
}

function buildEventTable(){
  const t = document.getElementById('eventTable');
  let html = '<thead><tr><th>Event</th><th>Tipe</th><th>Nilai</th><th>Deskripsi</th></tr></thead><tbody>';
  for(const e of data.events){
    html += `<tr><td>${e.name}</td><td>${e.type==='pct'?'%':'Rp'}</td><td>${e.value}${e.type==='pct'?'%':''}</td><td class="small">${e.desc}</td></tr>`;
  }
  html += '</tbody>';
  t.innerHTML = html;
}

buildFeeTable(); buildEventTable();

function calcPrice(){
  // Read inputs
  const hpp = Number(document.getElementById('hpp').value)||0;
  const packing = Number(document.getElementById('packing').value)||0;
  const platform = document.getElementById('platform').value;
  const kategori = document.getElementById('kategori').value;
  const affiliateRp = Number(document.getElementById('affiliateRp').value)||0;
  const affiliatePct = Number(document.getElementById('affiliatePct').value)||0;
  const profitRpInput = Number(document.getElementById('profitRp').value)||0;
  const profitPct = Number(document.getElementById('profitPct').value)||0;

  // Platform fee percent
  const platFeePct = Number(data.fees[platform][kategori])||0;

  // Events
  const checked = Array.from(document.querySelectorAll('#events input:checked'));
  // Summarize events: total fixed and total pct
  let eventFixedTotal = 0; let eventPctTotal = 0;
  for(const cb of checked){ if(cb.dataset.type==='fixed'){ eventFixedTotal += Number(cb.dataset.val) } else { eventPctTotal += Number(cb.dataset.val) } }

  // Base cost (modal)
  const baseCost = hpp + packing;

  // Affiliate: if percentage provided, use percentage of selling price (unknown). We'll treat affiliatePct as % of selling price; affiliateRp is flat per-product.
  // To solve for selling price when some fees are percentage-of-selling-price, we must solve equation:
  // SellingPrice = baseCost + affiliateRp + eventFixed + desiredProfit + (platformPct+eventPct+affiliatePct+adminPct)*SellingPrice
  // Let totalPct = (platFeePct + eventPctTotal + affiliatePct + adminPct)
  // But adminPct not specified in this calculator; we'll not include admin here. Rearranged: SellingPrice*(1 - totalPct/100) = baseCost + affiliateRp + eventFixed + desiredProfit
  // We'll compute desiredProfit either as fixed Rp or as percent on top of cost (we'll compute two variations). For profit% we interpret as percent of baseCost+fees (pre-fee) — common approach.

  const totalPct = platFeePct + eventPctTotal + (affiliatePct||0); // percent of selling price

  // Determine desiredProfitRp
  let desiredProfitRp = 0;
  if(profitRpInput>0){ desiredProfitRp = profitRpInput; }
  else if(profitPct>0){
    // Interpret profit% as percent of (baseCost + affiliateRp + eventFixed) BEFORE platform/event percent fees.
    // This is an approximation because some fees are on selling price; this is common business practice to target markup on cost.
    const costForProfit = baseCost + affiliateRp + eventFixedTotal;
    desiredProfitRp = Math.round(costForProfit * (profitPct/100));
  }

  // Now solve for selling price
  // SP * (1 - totalPct/100) = baseCost + affiliateRp + eventFixedTotal + desiredProfitRp
  const denominator = (1 - totalPct/100);
  let sellingPrice = 0;
  if(denominator <= 0){ sellingPrice = NaN; }
  else{ sellingPrice = Math.ceil( (baseCost + affiliateRp + eventFixedTotal + desiredProfitRp) / denominator ); }

  // Calculate breakdown
  const platformFeeRp = Math.round(sellingPrice * (platFeePct/100));
  const eventPctFeeRp = Math.round(sellingPrice * (eventPctTotal/100));
  const affiliatePctRp = Math.round(sellingPrice * (affiliatePct/100));
  const totalFeesRp = platformFeeRp + eventPctFeeRp + affiliatePctRp + eventFixedTotal;
  const netToSeller = sellingPrice - totalFeesRp;
  const grossProfitRp = netToSeller - baseCost; // should be approx desiredProfitRp

  // Show result
  const out = document.getElementById('priceResult');
  out.style.display='block';
  out.innerHTML = `Harga jual yang disarankan: <div style="font-size:1.1rem;margin-top:6px">Rp ${sellingPrice.toLocaleString('id-ID')}</div>
  <div class="small" style="margin-top:6px">Rincian:</div>
  <div class="small">Modal (HPP+packing): Rp ${baseCost.toLocaleString('id-ID')}</div>
  <div class="small">Platform fee (${platFeePct}%): Rp ${platformFeeRp.toLocaleString('id-ID')}</div>
  <div class="small">Event pct fee (${eventPctTotal}%): Rp ${eventPctFeeRp.toLocaleString('id-ID')}</div>
  <div class="small">Event fixed fee: Rp ${eventFixedTotal.toLocaleString('id-ID')}</div>
  <div class="small">Affiliate flat: Rp ${affiliateRp.toLocaleString('id-ID')}, affiliate%: Rp ${affiliatePctRp.toLocaleString('id-ID')}</div>
  <div class="small">Total fees: Rp ${totalFeesRp.toLocaleString('id-ID')}</div>
  <div class="small">Net ke penjual: Rp ${netToSeller.toLocaleString('id-ID')}</div>
  <div class="small">Perkiraan keuntungan (setelah fees): Rp ${grossProfitRp.toLocaleString('id-ID')}</div>
  `;
}

function calcROAS(){
  const sell = Number(document.getElementById('sellPrice').value)||0;
  const affPct = Number(document.getElementById('affPctROAS').value)||0;
  const adminPct = Number(document.getElementById('adminPctROAS').value)||0;
  const adPerDay = Number(document.getElementById('adPerDay').value)||0;
  const hpp = Number(document.getElementById('hppROAS').value)||0;
  const targetProfit = Number(document.getElementById('targetProfitDay').value)||0;

  // Per-product fees
  const affRp = Math.round(sell * (affPct/100));
  const adminRp = Math.round(sell * (adminPct/100));

  // Net revenue per sale after affiliate & admin
  const revenueAfterCuts = sell - affRp - adminRp;
  // Profit per sale after cost of goods
  const profitPerSale = revenueAfterCuts - hpp;

  let needSales = 0;
  if(profitPerSale <= 0){ needSales = Infinity; }
  else{ needSales = Math.ceil(targetProfit / profitPerSale); }

  const requiredRevenueFromAds = needSales * sell;
  // ROAS = revenue from ads / ad spend
  const roas = adPerDay>0 ? (requiredRevenueFromAds / adPerDay).toFixed(2) : '—';

  const out = document.getElementById('roasResult');
  out.style.display='block';
  out.innerHTML = `Per sale: keuntungan bersih ≈ Rp ${profitPerSale.toLocaleString('id-ID')}<br>
  Jika target keuntungan Rp ${targetProfit.toLocaleString('id-ID')}/hari -> perlu ≈ <strong>${isFinite(needSales)?needSales:'(tidak mungkin)'} penjualan/hari</strong><br>
  Pendapatan dari iklan yang diperlukan: Rp ${requiredRevenueFromAds.toLocaleString('id-ID')}<br>
  ROAS minimal (revenue/ad spend): ${roas}
  `;
}

// Download HTML helper — will download the entire page HTML
function downloadHTML(){
  const html = '<!doctype html>\n' + document.documentElement.outerHTML;
  const blob = new Blob([html], {type: 'text/html'});
  const url = URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url; a.download = 'kalkulator-keuntungan.html';
  document.body.appendChild(a); a.click(); a.remove();
  URL.revokeObjectURL(url);
}

// Initialize with default calc
calcPrice(); calcROAS();
</script>
</body>
</html>
