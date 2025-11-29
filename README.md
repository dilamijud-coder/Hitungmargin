<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kalkulator Keuntungan & ROAS - Toko Online</title>
  <style>
    :root{--bg:#f7fafc;--card:#ffffff;--muted:#6b7280;--accent:#2563eb}
    body{font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background:var(--bg); color:#111827; margin:0; padding:24px}
    .wrap{max-width:1100px;margin:0 auto;display:grid;grid-template-columns:1fr 420px;gap:20px}
    header{grid-column:1/-1;margin-bottom:8px}
    h1{font-size:20px;margin:0 0 6px}
    p.lead{margin:0;color:var(--muted);font-size:13px}
    .card{background:var(--card);padding:18px;border-radius:12px;box-shadow:0 4px 18px rgba(2,6,23,0.06)}
    label{display:block;font-size:13px;margin-bottom:6px;color:#374151}
    input[type="number"], select, input[type="text"]{width:100%;padding:10px;border-radius:8px;border:1px solid #e5e7eb;font-size:14px}
    .row{display:flex;gap:12px}
    .col{flex:1}
    .muted{color:var(--muted);font-size:13px}
    table{width:100%;border-collapse:collapse;font-size:13px}
    th,td{padding:8px;text-align:left;border-bottom:1px solid #f3f4f6}
    .small{font-size:12px}
    button{background:var(--accent);color:white;padding:10px 12px;border-radius:8px;border:0;cursor:pointer}
    .result{background:#f1f5f9;padding:12px;border-radius:8px;margin-top:10px}
    .flex-center{display:flex;align-items:center;gap:10px}
    footer{grid-column:1/-1;margin-top:12px;font-size:12px;color:var(--muted)}
    .editable{background:#f8fafc;padding:8px;border-radius:8px}
    @media(max-width:980px){.wrap{grid-template-columns:1fr;}.card{margin-bottom:12px}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Kalkulator Harga Jual & Keuntungan + ROAS Iklan</h1>
      <p class="lead">Masukkan HPP, biaya packing, dan pilih platform/campaign untuk menghitung fee, keuntungan target, serta harga jual yang direkomendasikan. Juga ada kalkulator ROAS sederhana untuk perhitungan iklan harian.</p>
    </header>

    <section class="card">
      <h2 style="margin-top:0;font-size:16px">Kalkulator Harga Jual & Keuntungan</h2>

      <div style="margin-bottom:12px">
        <label>Nama Produk (opsional)</label>
        <input id="productName" type="text" placeholder="Contoh: Kaos Polos Lengan Pendek" />
      </div>

      <div class="row" style="margin-bottom:8px">
        <div class="col">
          <label>HPP produk (Rp)</label>
          <input id="hpp" type="number" value="20000" />
        </div>
        <div class="col">
          <label>Biaya packing & tambahan per produk (Rp)</label>
          <input id="packing" type="number" value="3000" />
        </div>
      </div>

      <div class="row" style="margin-bottom:8px">
        <div class="col">
          <label>Platform</label>
          <select id="platform">
            <option value="shopee">Shopee</option>
            <option value="tiktok">TikTok Shop</option>
            <option value="tokopedia">Tokopedia</option>
            <option value="lazada">Lazada</option>
          </select>
        </div>
        <div class="col">
          <label>Kategori Produk</label>
          <select id="kategori">
            <option value="pakaian">Pakaian</option>
            <option value="makanan">Makanan & Minuman</option>
            <option value="elektronik">Elektronik</option>
            <option value="kosmetik">Kosmetik</option>
            <option value="aksesoris">Aksesoris</option>
          </select>
        </div>
      </div>

      <div class="row" style="margin-top:8px">
        <div class="col">
          <label>Fee event (mis. subsidi ongkir / campaign) — pilih jenis</label>
          <select id="eventType">
            <option value="none">Tidak ada</option>
            <option value="free_ship">Subsidi Gratis Ongkir (persen dari jual)</option>
            <option value="campaign">Campaign Fee (persen dari jual)</option>
            <option value="fixed">Biaya Event Tetap (Rp)</option>
          </select>
        </div>
        <div class="col">
          <label>Nilai event (isi sesuai tipe; persen tanpa simbol atau Rp untuk fixed)</label>
          <input id="eventValue" type="number" value="0" />
        </div>
      </div>

      <div style="margin-top:10px">
        <label>Keuntungan yang diinginkan (Rp atau persen) — pilih tipe</label>
        <div class="row">
          <select id="profitType" style="max-width:180px">
            <option value="percent">Persentase (%) dari modal total</option>
            <option value="fixed">Nominal (Rp) per produk</option>
          </select>
          <input id="profitValue" type="number" value="30" />
        </div>
      </div>

      <div style="margin-top:12px;display:flex;gap:10px;align-items:center">
        <button id="calcPrice">Hitung Harga Jual</button>
        <div class="muted small">Hasil akan menampilkan rincian biaya, fee platform/event, dan harga jual rekomendasi.</div>
      </div>

      <div id="priceResult" class="result" style="display:none"></div>

      <hr style="margin:16px 0" />

      <h3 style="margin:6px 0">Daftar Fee Per Kategori (editable)</h3>
      <div class="muted small">Ubah nilai fee jika ada perubahan kebijakan platform — nilai default hanya contoh umum.</div>

      <div style="margin-top:8px" class="editable">
        <table id="feeTable">
          <thead>
            <tr><th>Platform</th><th>Kategori</th><th>Fee (%)</th><th>Fee tetap (Rp)</th></tr>
          </thead>
          <tbody>
            <!-- default rows filled by JS -->
          </tbody>
        </table>
      </div>

    </section>

    <aside class="card">
      <h2 style="margin-top:0;font-size:16px">Kalkulator ROAS Iklan</h2>
      <div style="margin-bottom:8px">
        <label>HPP produk (Rp)</label>
        <input id="hppAd" type="number" value="20000" />
      </div>
      <div style="margin-bottom:8px">
        <label>Modal iklan per hari (Rp)</label>
        <input id="adBudget" type="number" value="50000" />
      </div>
      <div style="margin-bottom:8px">
        <label>Target keuntungan per hari dari iklan (Rp)</label>
        <input id="targetProfitDay" type="number" value="30000" />
      </div>
      <div style="display:flex;gap:8px;align-items:center">
        <button id="calcRoas">Hitung ROAS & Target Konversi</button>
        <div class="muted small">Hasil: target ROAS minimal & estimasi orders/hari.</div>
      </div>
      <div id="roasResult" class="result" style="display:none"></div>

      <hr style="margin:12px 0" />
      <h3 style="margin:6px 0">Petunjuk singkat</h3>
      <ul style="padding-left:18px;margin:6px 0;color:var(--muted)">
        <li>ROAS = (Pendapatan dari iklan) / (Biaya iklan). Contoh ROAS 3 berarti setiap Rp1 iklan kembali Rp3.</li>
        <li>Perhitungan di sini menghitung ROAS minimal yang diperlukan agar target keuntungan harian tercapai, serta estimasi konversi/orders yang dibutuhkan.</li>
      </ul>
    </aside>

    <footer class="muted small">Catatan: Nilai fee platform di bawah ini adalah nilai default contoh. Silakan sesuaikan di tabel jika platform memperbarui kebijakan fee mereka.</footer>
  </div>

  <script>
    // Default fee data (contoh) — pengguna bisa edit di tabel
    const defaultFees = [
      {platform:'shopee', kategori:'pakaian', pct:3.0, fixed:0},
      {platform:'shopee', kategori:'makanan', pct:5.0, fixed:0},
      {platform:'shopee', kategori:'elektronik', pct:2.5, fixed:0},
      {platform:'shopee', kategori:'kosmetik', pct:4.0, fixed:0},
      {platform:'shopee', kategori:'aksesoris', pct:3.5, fixed:0},

      {platform:'tiktok', kategori:'pakaian', pct:2.5, fixed:0},
      {platform:'tiktok', kategori:'makanan', pct:5.5, fixed:0},
      {platform:'tiktok', kategori:'elektronik', pct:3.0, fixed:0},
      {platform:'tiktok', kategori:'kosmetik', pct:4.5, fixed:0},
      {platform:'tiktok', kategori:'aksesoris', pct:3.0, fixed:0},

      {platform:'tokopedia', kategori:'pakaian', pct:2.9, fixed:0},
      {platform:'tokopedia', kategori:'makanan', pct:4.8, fixed:0},
      {platform:'tokopedia', kategori:'elektronik', pct:2.2, fixed:0},
      {platform:'tokopedia', kategori:'kosmetik', pct:3.8, fixed:0},
      {platform:'tokopedia', kategori:'aksesoris', pct:3.2, fixed:0},

      {platform:'lazada', kategori:'pakaian', pct:3.2, fixed:0},
      {platform:'lazada', kategori:'makanan', pct:5.2, fixed:0},
      {platform:'lazada', kategori:'elektronik', pct:2.7, fixed:0},
      {platform:'lazada', kategori:'kosmetik', pct:4.0, fixed:0},
      {platform:'lazada', kategori:'aksesoris', pct:3.4, fixed:0},
    ];

    // Render fee table
    function renderFeeTable(){
      const tbody = document.querySelector('#feeTable tbody');
      tbody.innerHTML = '';
      defaultFees.forEach((r, i)=>{
        const tr = document.createElement('tr');
        tr.innerHTML = `<td>${capitalize(r.platform)}</td>
                        <td>${capitalize(r.kategori)}</td>
                        <td><input data-i="${i}" class="feePct" type="number" value="${r.pct}" step="0.1"/></td>
                        <td><input data-i="${i}" class="feeFixed" type="number" value="${r.fixed}"/></td>`;
        tbody.appendChild(tr);
      });

      // add listeners
      document.querySelectorAll('.feePct').forEach(inp=>inp.oninput = (e)=>{
        const i = +e.target.dataset.i; defaultFees[i].pct = parseFloat(e.target.value || 0);
      });
      document.querySelectorAll('.feeFixed').forEach(inp=>inp.oninput = (e)=>{
        const i = +e.target.dataset.i; defaultFees[i].fixed = parseFloat(e.target.value || 0);
      });
    }

    function capitalize(s){return s.charAt(0).toUpperCase()+s.slice(1)}

    renderFeeTable();

    document.getElementById('calcPrice').addEventListener('click', ()=>{
      const hpp = Number(document.getElementById('hpp').value) || 0;
      const packing = Number(document.getElementById('packing').value) || 0;
      const platform = document.getElementById('platform').value;
      const kategori = document.getElementById('kategori').value;
      const eventType = document.getElementById('eventType').value;
      const eventValue = Number(document.getElementById('eventValue').value) || 0;
      const profitType = document.getElementById('profitType').value;
      const profitValue = Number(document.getElementById('profitValue').value) || 0;

      // find platform fee
      const feeRow = defaultFees.find(f=>f.platform===platform && f.kategori===kategori) || {pct:0,fixed:0};
      const platformPct = feeRow.pct; const platformFixed = feeRow.fixed;

      // total modal per produk
      const modal = hpp + packing;

      // event fee calculation placeholder
      let eventFeeRp = 0; // absolute Rp
      if(eventType==='free_ship'){
        // interpret eventValue as percent dari jual — but we don't know jual yet; approximate: treat as percent of modal? better compute algebraically below
        // We'll do algebraic solution for selling price S using fees that are percent of S
      } else if(eventType==='campaign'){
        // percent of jual
      } else if(eventType==='fixed'){
        eventFeeRp = eventValue;
      }

      // We need to solve for selling price S where:
      // platform fee = S * platformPct/100 + platformFixed
      // event percent fees (free_ship/campaign) = S * eventPct/100
      // profit desired: either fixed (Rp) or percent of modal (or percent of S?) We defined percent as percent dari modal total (user option). If profitType percent, interpret as percent dari modal.

      // We'll compute two approaches: target profit as fixed Rp profitTargetRp
      let profitTargetRp = profitType==='fixed' ? profitValue : Math.round(modal * (profitValue/100));

      // Let S = selling price. Fees that depend on S: platformPct% and eventPct% (if eventType is percent type)
      let eventPct = 0;
      if(eventType==='free_ship' || eventType==='campaign') eventPct = eventValue;

      // Equation: S - platformPct/100*S - eventPct/100*S - platformFixed - eventFixed - modal = profitTargetRp
      // => S*(1 - (platformPct+eventPct)/100) = modal + platformFixed + eventFixed + profitTargetRp
      const denom = 1 - ((platformPct + eventPct)/100);
      let platformFixedRp = platformFixed;
      let eventFixedRp = (eventType==='fixed') ? eventFeeRp : 0;
      let S = denom>0 ? (modal + platformFixedRp + eventFixedRp + profitTargetRp)/denom : NaN;
      if(!isFinite(S)) S = NaN;

      const platformFeeRp = (S * platformPct/100) + platformFixedRp;
      const eventFeeRpCalculated = (eventType==='fixed') ? eventFixedRp : (S * eventPct/100);
      const totalFees = platformFeeRp + eventFeeRpCalculated;
      const revenue = S - totalFees; // received after platform+event fees
      const netProfit = revenue - modal;

      const resultEl = document.getElementById('priceResult');
      resultEl.style.display = 'block';
      if(isNaN(S)){
        resultEl.innerHTML = `<strong>Perhitungan gagal:</strong> persentase fee terlalu besar sehingga tidak memungkinkan menentukan harga jual. Periksa nilai fee/platform/event.`;
        return;
      }

      resultEl.innerHTML = `
        <strong>Rincian perhitungan</strong>
        <div class="muted small">Modal (HPP + packing): Rp ${numberFormat(modal)}</div>
        <table style="margin-top:8px">
          <tr><td>Harga jual direkomendasikan (S)</td><td>Rp ${numberFormat(Math.ceil(S))}</td></tr>
          <tr><td>Platform fee (${platformPct}% + Rp ${numberFormat(platformFixedRp)})</td><td>Rp ${numberFormat(Math.round(platformFeeRp))}</td></tr>
          <tr><td>Event fee (${eventType}${eventPct?(' '+eventPct+'%'):''}${eventFixedRp?(' Rp '+numberFormat(eventFixedRp)):''})</td><td>Rp ${numberFormat(Math.round(eventFeeRpCalculated))}</td></tr>
          <tr><td>Total fee</td><td>Rp ${numberFormat(Math.round(totalFees))}</td></tr>
          <tr><td>Pendapatan bersih setelah fee</td><td>Rp ${numberFormat(Math.round(revenue))}</td></tr>
          <tr><td>Keuntungan bersih (setelah modal)</td><td>Rp ${numberFormat(Math.round(netProfit))}</td></tr>
        </table>
        <div style="margin-top:8px" class="muted small">Catatan: jika Anda memilih keuntungan persen maka persentase tersebut dihitung dari modal (HPP + packing).</div>
        <div style="margin-top:8px">Jika ingin target margin tertentu (misal margin dari harga jual), ubah nilai di bagian keuntungan ke tipe <em>Nominal (Rp)</em> dan masukkan nilai yang diinginkan.</div>
      `;

    });

    document.getElementById('calcRoas').addEventListener('click', ()=>{
      const hppAd = Number(document.getElementById('hppAd').value) || 0;
      const adBudget = Number(document.getElementById('adBudget').value) || 0;
      const targetProfitDay = Number(document.getElementById('targetProfitDay').value) || 0;

      // assume tiap order menghasilkan margin = (harga jual - fee - modal). But we don't have harga jual. We'll compute minimal revenue per order that yields profit.
      // Simplify: targetProfitDay must come from (revenue_per_order - modal) * orders - adBudget? Wait adBudget is cost, profit from sales should exceed targetProfitDay + adBudget to be profitable.
      // We'll compute minimal revenue_per_day needed: revenue_needed = adBudget + targetProfitDay + totalModalOfOrders
      // For simpler UX, assume user sets price jual per product equal to HPP + desired profit per order; but user did not.
      // Instead compute minimal number of orders per day and minimal ROAS assuming average order value = 2 * HPP (heuristic). But better: ask user for AOV — but per instructions avoid clarification. We'll compute ROAS minimal under two scenarios: AOV = 2*HPP and AOV = 3*HPP.

      const aov1 = hppAd * 2; // average order value scenario 1
      const aov2 = hppAd * 3; // scenario 2

      // For required revenue per day = adBudget + targetProfitDay + cost_of_goods_sold_for_orders
      // Let orders = x. revenue = AOV * x. COGS = hppAd * x. Profit from sales before ads = (AOV - hppAd) * x. After ads, profit = (AOV - hppAd) * x - adBudget
      // We want (AOV - hppAd)*x - adBudget >= targetProfitDay -> solve x

      function solveOrders(aov){
        const marginPerOrder = aov - hppAd; // contribution to profit before ads
        if(marginPerOrder<=0) return {orders:Infinity};
        const x = Math.ceil((targetProfitDay + adBudget) / marginPerOrder);
        const revenueNeeded = aov * x;
        const roas = revenueNeeded / adBudget || Infinity;
        return {orders:x, revenueNeeded:revenueNeeded, roas:roas.toFixed(2)};
      }

      const s1 = solveOrders(aov1);
      const s2 = solveOrders(aov2);

      const el = document.getElementById('roasResult'); el.style.display='block';
      el.innerHTML = `<strong>Hasil Perhitungan ROAS & Target Konversi</strong>
        <div class="muted small">Asumsi: tidak memasukkan fee platform/event. Gunakan hasil ini sebagai panduan kasar.</div>
        <table style="margin-top:8px">
          <tr><td>Skema A: AOV = 2 × HPP (Rp ${numberFormat(aov1)})</td><td></td></tr>
          <tr><td>Target orders per hari</td><td>${isFinite(s1.orders)?s1.orders:'Tidak mungkin'}</td></tr>
          <tr><td>Total pendapatan dari iklan (estimasi)</td><td>Rp ${isFinite(s1.revenueNeeded)?numberFormat(s1.revenueNeeded):'-'}</td></tr>
          <tr><td>ROAS minimal (pendapatan/biaya iklan)</td><td>${isFinite(s1.roas)?s1.roas:'-'}</td></tr>
        </table>
        <hr />
        <table>
          <tr><td>Skema B: AOV = 3 × HPP (Rp ${numberFormat(aov2)})</td><td></td></tr>
          <tr><td>Target orders per hari</td><td>${isFinite(s2.orders)?s2.orders:'Tidak mungkin'}</td></tr>
          <tr><td>Total pendapatan dari iklan (estimasi)</td><td>Rp ${isFinite(s2.revenueNeeded)?numberFormat(s2.revenueNeeded):'-'}</td></tr>
          <tr><td>ROAS minimal (pendapatan/biaya iklan)</td><td>${isFinite(s2.roas)?s2.roas:'-'}</td></tr>
        </table>
        <div style="margin-top:8px" class="muted small">Interpretasi: jika ROAS yang Anda peroleh dari platform lebih besar atau sama dengan angka di atas, target keuntungan harian bisa tercapai dengan estimasi orders yang tercantum.</div>
      `;

    });

    function numberFormat(n){ return new Intl.NumberFormat('id-ID').format(n); }

  </script>
</body>
</html>
