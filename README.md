<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Kalkulator Modal & Keuntungan - Online Shop</title>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;800&display=swap" rel="stylesheet">
  <style>
    :root{--bg:#f6f8fb;--card:#ffffff;--muted:#6b7280;--accent:#0f172a;--accent-2:#0ea5a4}
    *{box-sizing:border-box;font-family:Inter,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial}
    body{margin:0;background:linear-gradient(180deg,#eef2ff 0%,var(--bg) 100%);padding:32px;color:var(--accent)}
    .container{max-width:980px;margin:0 auto}
    header{display:flex;align-items:center;gap:16px;margin-bottom:20px}
    .logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,var(--accent-2),#0ea5f9);display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:20px}
    h1{margin:0;font-size:20px}
    p.lead{margin:4px 0 0;color:var(--muted);font-size:14px}

    .grid{display:grid;grid-template-columns:1fr 380px;gap:20px}
    .card{background:var(--card);border-radius:12px;padding:18px;box-shadow:0 6px 18px rgba(15,23,42,0.06)}

    .field{display:flex;flex-direction:column;margin-bottom:12px}
    label{font-size:13px;margin-bottom:6px;color:var(--muted)}
    input[type="number"],select{padding:10px;border-radius:8px;border:1px solid #e6e9ef;font-size:14px}
    .row{display:flex;gap:10px}
    .row .field{flex:1}

    .smallmuted{font-size:12px;color:var(--muted)}
    .divider{height:1px;background:#f1f5f9;margin:14px 0;border-radius:2px}

    .results{display:flex;flex-direction:column;gap:10px}
    .result-item{display:flex;justify-content:space-between;align-items:center;padding:12px;border-radius:8px;border:1px dashed #eef2ff;background:linear-gradient(180deg,#ffffff,#fbfdff)}
    .result-item.big{font-size:18px;padding:16px;background:linear-gradient(90deg,#eef2ff,#f3f9ff);border:1px solid #e0f2fe}

    .fee-table{width:100%;border-collapse:collapse;margin-top:8px}
    .fee-table th,.fee-table td{padding:8px;border-bottom:1px solid #f1f5f9;font-size:13px;text-align:left}
    .fee-table th{font-weight:600}

    footer{margin-top:14px;font-size:12px;color:var(--muted)}

    @media (max-width:900px){.grid{grid-template-columns:1fr;}.container{padding:12px}}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="logo">KP</div>
      <div>
        <h1>Kalkulator Modal & Keuntungan</h1>
        <p class="lead">Hitung harga jual yang tepat dan rincian keuntungan setelah semua fee (platform, affiliate, event, voucher).</p>
      </div>
    </header>

    <main class="grid">
      <section class="card">
        <h3>Input Biaya & Preferensi</h3>
        <div class="divider"></div>

        <div class="row">
          <div class="field">
            <label>HPP (Harga Pokok Produksi) — Rp</label>
            <input id="hpp" type="number" min="0" value="50000" />
          </div>
          <div class="field">
            <label>Biaya packing & tambahan — Rp</label>
            <input id="packing" type="number" min="0" value="5000" />
          </div>
        </div>

        <div class="row">
          <div class="field">
            <label>Platform</label>
            <select id="platform"></select>
          </div>
          <div class="field">
            <label>Kategori (untuk fee)</label>
            <select id="category">
              <option value="pakaian">Pakaian</option>
              <option value="makanan">Makanan</option>
              <option value="elektronik">Elektronik</option>
              <option value="aksesoris">Aksesoris</option>
            </select>
          </div>
        </div>

        <div class="row">
          <div class="field">
            <label>Estimasi Affiliate (%) — dari harga jual</label>
            <input id="affiliate" type="number" min="0" max="100" value="3" />
          </div>
          <div class="field">
            <label>Fee event (%) — jika ikut campaign (opsional)</label>
            <input id="eventPercent" type="number" min="0" max="100" value="0" />
          </div>
        </div>

        <div class="row">
          <div class="field">
            <label>Biaya event / ongkir / voucher (tetap) — Rp</label>
            <input id="fixedEvent" type="number" min="0" value="0" />
          </div>
          <div class="field">
            <label>Voucher (potongan untuk pembeli) — Rp</label>
            <input id="voucher" type="number" min="0" value="0" />
          </div>
        </div>

        <div class="row">
          <div class="field">
            <label>Keuntungan yang diinginkan</label>
            <div style="display:flex;gap:8px;align-items:center">
              <input id="profitVal" type="number" min="0" value="20000" style="flex:1" />
              <select id="profitMode" style="width:140px">
                <option value="rp">Rp</option>
                <option value="pct">% dari HPP</option>
              </select>
            </div>
            <div class="smallmuted">Pilih 'Rp' untuk keuntungan nomimal atau '%' untuk persentase terhadap HPP.</div>
          </div>
        </div>

        <div class="divider"></div>
        <button id="calcBtn" style="padding:10px 14px;border-radius:10px;border:0;background:linear-gradient(90deg,#0ea5a4,#0ea5f9);color:white;font-weight:600;cursor:pointer">Hitung Harga Jual</button>

        <div style="margin-top:12px;font-size:13px;color:var(--muted)">Hasil akan tampil di panel sebelah kanan dengan ringkasan dan rincian tiap komponen biaya.</div>
      </section>

      <aside class="card">
        <h3>Hasil & Rincian</h3>
        <div class="divider"></div>
        <div class="results">
          <div class="result-item big">
            <div>Harga Jual yang Disarankan</div>
            <div id="recommended">Rp 0</div>
          </div>

          <div class="result-item">
            <div>Total Biaya Tetap (HPP + Packing + Event tetap)</div>
            <div id="totalFixed">Rp 0</div>
          </div>

          <div class="result-item">
            <div>Total Persentase Fee (platform + affiliate + event%)</div>
            <div id="totalPercent">0%</div>
          </div>

          <div class="result-item">
            <div>Estimasi Fee Platform + Affiliate + Event (Rp)</div>
            <div id="feesRp">Rp 0</div>
          </div>

          <div class="result-item">
            <div>Keuntungan Bersih Setelah Semua Fee</div>
            <div id="netProfit">Rp 0</div>
          </div>

          <div class="divider"></div>

          <h4>Daftar Fee (contoh)</h4>
          <table class="fee-table">
            <thead><tr><th>Platform</th><th>Pakaian</th><th>Makanan</th><th>Elektronik</th><th>Aksesoris</th></tr></thead>
            <tbody id="feeBody">
              <!-- diisi secara dinamis -->
            </tbody>
          </table>

          <footer>Catatan: tabel di atas hanya contoh. Sesuaikan nilai fee/platform bila dibutuhkan.</footer>
        </div>
      </aside>
    </main>
  </div>

<script>
// Data platform fee (contoh). Angka adalah persentase fee dari harga jual.
const platformFees = {
  'Shopee': {pakaian: 0.02, makanan: 0.02, elektronik: 0.025, aksesoris: 0.02},
  'Tokopedia': {pakaian: 0.02, makanan: 0.03, elektronik: 0.03, aksesoris: 0.02},
  'TikTok Shop': {pakaian: 0.015, makanan: 0.02, elektronik: 0.025, aksesoris: 0.015},
  'Lazada': {pakaian: 0.025, makanan: 0.03, elektronik: 0.035, aksesoris: 0.02}
};

// init platform select
const platformSelect = document.getElementById('platform');
Object.keys(platformFees).forEach(p => {
  const opt = document.createElement('option'); opt.value = p; opt.textContent = p; platformSelect.appendChild(opt);
});

// render fee table
function renderFeeTable(){
  const feeBody = document.getElementById('feeBody'); feeBody.innerHTML='';
  for(const p of Object.keys(platformFees)){
    const row = document.createElement('tr');
    const th = document.createElement('td'); th.textContent = p; row.appendChild(th);
    ['pakaian','makanan','elektronik','aksesoris'].forEach(cat=>{
      const td = document.createElement('td'); td.textContent = (platformFees[p][cat]*100).toFixed(1)+" %"; row.appendChild(td);
    });
    feeBody.appendChild(row);
  }
}
renderFeeTable();

function formatRp(v){
  return 'Rp ' + Number(v).toLocaleString('id-ID');
}

function calculate(){
  const hpp = Number(document.getElementById('hpp').value) || 0;
  const packing = Number(document.getElementById('packing').value) || 0;
  const platform = document.getElementById('platform').value;
  const category = document.getElementById('category').value;
  const affiliatePct = (Number(document.getElementById('affiliate').value) || 0)/100;
  const eventPct = (Number(document.getElementById('eventPercent').value) || 0)/100;
  const fixedEvent = Number(document.getElementById('fixedEvent').value) || 0;
  const voucher = Number(document.getElementById('voucher').value) || 0;
  const profitMode = document.getElementById('profitMode').value;
  let desired = Number(document.getElementById('profitVal').value) || 0;
  if(profitMode==='pct'){
    // desired % dari HPP
    desired = (desired/100)*hpp;
  }

  const platformPct = platformFees[platform][category] || 0;
  const totalPercent = platformPct + affiliatePct + eventPct; // persentase terhadap harga jual

  const totalFixed = hpp + packing + fixedEvent; // biaya tetap

  // SP * (1 - totalPercent) = totalFixed + desired + voucher
  const denom = (1 - totalPercent);
  if(denom <= 0){
    alert('Total persentase fee >= 100% — tidak bisa menghitung. Kurangi persentase fee atau cek input.');
    return;
  }

  const recommended = (totalFixed + desired + voucher) / denom;

  // estimasi fees in Rp (platform+affiliate+event%) dari recommended price
  const feesRp = recommended * totalPercent;

  const netProfit = recommended - feesRp - totalFixed - voucher - hpp; // double-check: hpp included in totalFixed already, so subtracting again? we must ensure consistent.
  // Adjust netProfit calculation: profit after costs and fees should be recommended - feesRp - totalFixed - voucher
  const netProfitCorrect = recommended - feesRp - totalFixed - voucher;

  document.getElementById('recommended').textContent = formatRp(Math.round(recommended));
  document.getElementById('totalFixed').textContent = formatRp(totalFixed);
  document.getElementById('totalPercent').textContent = (totalPercent*100).toFixed(2) + ' %';
  document.getElementById('feesRp').textContent = formatRp(Math.round(feesRp));
  document.getElementById('netProfit').textContent = formatRp(Math.round(netProfitCorrect));
}

document.getElementById('calcBtn').addEventListener('click', calculate);

// auto calculate on change for convenience
['hpp','packing','platform','category','affiliate','eventPercent','fixedEvent','voucher','profitVal','profitMode'].forEach(id=>{
  document.getElementById(id).addEventListener('change', ()=>{/*no spam*/});
});

// initial calculate
calculate();
</script>
</body>
</html>
