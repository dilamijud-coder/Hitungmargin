<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kalkulator Seller — Hitung HPP & Harga Jual</title>
  <style>
    :root{
      --bg:#f5f7fb; --card:#ffffff; --accent:#2563eb; --muted:#6b7280;
      --success:#10b981; --danger:#ef4444; --glass: rgba(255,255,255,0.6);
    }
    *{box-sizing:border-box;font-family:Inter,system-ui,-apple-system,"Segoe UI",Roboto,"Helvetica Neue",Arial;}
    body{margin:0;background:linear-gradient(180deg,#eef2ff 0%,var(--bg) 100%);color:#0f172a;}
    .container{max-width:1100px;margin:28px auto;padding:20px;}
    header{display:flex;align-items:center;gap:16px;margin-bottom:18px;}
    .logo{width:56px;height:56px;border-radius:10px;background:linear-gradient(135deg,var(--accent),#7c3aed);display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:20px;box-shadow:0 6px 18px rgba(37,99,235,0.18);}
    h1{margin:0;font-size:20px;}
    p.lead{margin:4px 0 0;color:var(--muted);font-size:13px}

    .grid{display:grid;grid-template-columns:1fr 420px;gap:18px;align-items:start;}
    .card{background:var(--card);border-radius:12px;padding:18px;box-shadow:0 6px 18px rgba(15,23,42,0.06);}

    form .row{display:flex;gap:10px;margin-bottom:12px;}
    .col{flex:1;display:flex;flex-direction:column;}
    label{font-size:13px;color:var(--muted);margin-bottom:6px;}
    input[type="number"], input[type="text"], select{padding:10px;border-radius:8px;border:1px solid #e6e9ef;font-size:14px;}
    .small{font-size:12px;color:var(--muted);margin-top:6px}
    .actions{display:flex;gap:10px;margin-top:12px;}
    button{background:var(--accent);color:white;padding:10px 12px;border-radius:8px;border:0;cursor:pointer;font-weight:600;}
    button.ghost{background:transparent;color:var(--accent);border:1px solid rgba(37,99,235,0.12);}
    .result{padding:14px;border-radius:10px;background:linear-gradient(180deg,#ffffff,#fbfdff);border:1px solid #eef2ff;margin-top:12px;}
    .result .row{display:flex;justify-content:space-between;align-items:center;margin:6px 0;}
    .big{font-size:18px;font-weight:700;}
    .muted{color:var(--muted);font-size:13px}
    table{width:100%;border-collapse:collapse;margin-top:12px;font-size:13px;}
    table th, table td{padding:8px;border-bottom:1px solid #f1f5f9;text-align:left;}
    table th{background:#fafafa;font-weight:600}
    .fees-list{max-height:340px;overflow:auto;padding-right:4px}
    footer{margin-top:18px;color:var(--muted);font-size:13px;text-align:center}

    /* responsive */
    @media (max-width:980px){
      .grid{grid-template-columns:1fr; }
      .card{padding:14px;}
    }
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="logo">KS</div>
      <div>
        <h1>Kalkulator Seller — Hitung HPP & Harga Jual</h1>
        <p class="lead">Masukkan data produk dan pilih platform untuk melihat estimasi biaya, potongan, dan dana yang Anda terima.</p>
      </div>
    </header>

    <div class="grid">
      <!-- LEFT: Calculator -->
      <div class="card">
        <h3>Input Data Produk</h3>
        <form id="calcForm" onsubmit="return false">
          <div class="row">
            <div class="col">
              <label>HPP (Harga Pokok Produksi / HPP) — Rp</label>
              <input id="hpp" type="number" min="0" step="1" value="50000">
            </div>
            <div class="col">
              <label>Biaya Packing & Tambahan — Rp</label>
              <input id="packing" type="number" min="0" step="1" value="2000">
            </div>
          </div>

          <div class="row">
            <div class="col">
              <label>Jumlah per Transaksi (qty)</label>
              <input id="qty" type="number" min="1" step="1" value="1">
            </div>
            <div class="col">
              <label>Voucher / Diskon (Rp)</label>
              <input id="voucher" type="number" min="0" step="1" value="0">
            </div>
          </div>

          <div class="row">
            <div class="col">
              <label>Platform</label>
              <select id="platform" onchange="updatePlatform()">
                <option value="shopee">Shopee</option>
                <option value="tokopedia">Tokopedia</option>
                <option value="lazada">Lazada</option>
                <option value="tiktok">TikTok Shop</option>
              </select>
            </div>
            <div class="col">
              <label>Kategori (untuk fee platform)</label>
              <select id="category" onchange="updateCategory()">
                <option value="umum">Umum</option>
                <option value="pakaian">Pakaian</option>
                <option value="makanan">Makanan</option>
                <option value="elektronik">Elektronik</option>
                <option value="aksesoris">Aksesoris</option>
              </select>
            </div>
          </div>

          <div class="row">
            <div class="col">
              <label>Fee Platform (persen) — otomatis berdasarkan platform & kategori</label>
              <input id="feePlatform" type="number" min="0" step="0.01" value="6.0">
            </div>
            <div class="col">
              <label>Estimasi Biaya Affiliate (%)</label>
              <input id="feeAffiliatePct" type="number" min="0" step="0.01" value="0">
              <div class="small">Jika affiliate dihitung persen (opsional). Jika pakai Rp, masukkan di bawah.</div>
            </div>
          </div>

          <div class="row">
            <div class="col">
              <label>Biaya Affiliate (Rp per unit)</label>
              <input id="feeAffiliateRp" type="number" min="0" step="1" value="0">
            </div>
            <div class="col">
              <label>Fee Event / Campaign (%)</label>
              <input id="feeEventPct" type="number" min="0" step="0.01" value="0">
            </div>
          </div>

          <div class="row">
            <div class="col">
              <label>Biaya Event (Rp per unit)</label>
              <input id="feeEventRp" type="number" min="0" step="1" value="0">
            </div>
            <div class="col">
              <label>Keuntungan yang Diinginkan (Rp per unit)</label>
              <input id="profitRp" type="number" min="0" step="1" value="20000">
            </div>
          </div>

          <div style="margin-top:8px">
            <label>Mode Perhitungan</label>
            <div class="row">
              <div style="display:flex;gap:8px;align-items:center;">
                <input id="modePrice" type="radio" name="mode" checked onchange="showMode()"> <label class="muted" style="margin:0">Hitung dari Harga Jual</label>
                <input id="modeTarget" type="radio" name="mode" style="margin-left:12px" onchange="showMode()"> <label class="muted" style="margin:0">Hitung dari Target Dana Diterima</label>
              </div>
            </div>
          </div>

          <div id="priceModeInputs" style="margin-top:10px">
            <div class="row">
              <div class="col">
                <label>Harga Jual (Sebelum diskon) — Rp</label>
                <input id="price" type="number" min="0" step="1" value="100000">
              </div>
              <div class="col">
                <label>Diskon (%)</label>
                <input id="discountPct" type="number" min="0" max="100" step="0.01" value="0">
              </div>
            </div>
          </div>

          <div id="targetModeInputs" style="display:none;margin-top:10px">
            <div class="row">
              <div class="col">
                <label>Target Dana Diterima (bersih) — Rp</label>
                <input id="targetReceived" type="number" min="0" step="1" value="80000">
              </div>
              <div class="col">
                <label>Catatan</label>
                <div class="small">Sistem akan menyarankan harga jual agar setelah potongan Anda menerima nominal ini.</div>
              </div>
            </div>
          </div>

          <div class="actions">
            <button type="button" onclick="calculateFromPrice()">Hitung</button>
            <button type="button" class="ghost" onclick="resetForm()">Reset</button>
          </div>
        </form>

        <div id="resultBox" class="result" aria-live="polite" style="display:block">
          <div class="row"><div class="muted">Ringkasan Perhitungan</div><div></div></div>
          <div class="row"><div>Harga Jual (after diskon)</div><div id="displayPrice" class="big">Rp 0</div></div>
          <div class="row"><div>Jumlah potongan total</div><div id="displayTotalCuts">Rp 0</div></div>
          <div class="row"><div>Dana diterima penjual</div><div id="displayReceived" class="big">Rp 0</div></div>
          <div class="row"><div>Estimasi keuntungan bersih (setelah semua biaya)</div><div id="displayNetProfit" class="big">Rp 0</div></div>

          <div style="margin-top:10px">
            <div class="muted">Rincian Potongan</div>
            <div style="margin-top:8px">
              <div class="row"><div>Fee platform (%)</div><div id="showFeePlatform">0%</div></div>
              <div class="row"><div>Fee platform (Rp)</div><div id="showFeePlatformRp">Rp 0</div></div>
              <div class="row"><div>Fee event (%)</div><div id="showFeeEventPct">0%</div></div>
              <div class="row"><div>Fee event (Rp)</div><div id="showFeeEventRp">Rp 0</div></div>
              <div class="row"><div>Fee affiliate (%)</div><div id="showFeeAffPct">0%</div></div>
              <div class="row"><div>Fee affiliate (Rp)</div><div id="showFeeAffRp">Rp 0</div></div>
              <div class="row"><div>Voucher / diskon (Rp)</div><div id="showVoucherRp">Rp 0</div></div>
            </div>
          </div>
        </div>
      </div>

      <!-- RIGHT: Fee tables -->
      <div>
        <div class="card">
          <h3>Tabel Fee Platform (referensi)</h3>
          <p class="small">Pilih platform dan kategori untuk menyesuaikan persentase fee platform. Tabel di bawah contoh standar — sesuaikan saat berubah.</p>
          <div class="fees-list">
            <table>
              <thead>
                <tr><th>Platform</th><th>Kategori</th><th>Fee Platform (%)</th><th>Catatan</th></tr>
              </thead>
              <tbody id="feesTableBody">
                <!-- rows inserted by JS -->
              </tbody>
            </table>
          </div>
        </div>

        <div class="card" style="margin-top:14px">
          <h3>Petunjuk Singkat</h3>
          <ol style="padding-left:18px;color:var(--muted);font-size:13px">
            <li>Isi HPP dan biaya tambahan. Pilih platform & kategori untuk fee otomatis.</li>
            <li>Pilih mode: hitung dari Harga Jual, atau masukkan Target Dana Diterima.</li>
            <li>Hasil menampilkan potongan per-item dan dana bersih.</li>
            <li>Tabel fee bersifat referensi — update bila platform mengubah kebijakan.</li>
          </ol>
        </div>
      </div>
    </div>

    <footer>
      &copy; <span id="year"></span> Kalkulator Seller — dibuat cepat. Sesuaikan data fee sesuai kebijakan platform.
    </footer>
  </div>

  <script>
    // sample fee data (bisa disesuaikan lebih detail)
    const feeData = {
      shopee: {
        name: 'Shopee',
        fees: {
          umum: 6.0,
          pakaian: 8.0,
          makanan: 5.0,
          elektronik: 6.5,
          aksesoris: 6.5
        },
        note: 'Fee admin + layanan, contoh'
      },
      tokopedia: {
        name: 'Tokopedia',
        fees: {
          umum: 5.5,
          pakaian: 7.0,
          makanan: 4.5,
          elektronik: 6.0,
          aksesoris: 5.5
        },
        note: 'Contoh fee platform'
      },
      lazada: {
        name: 'Lazada',
        fees: {
          umum: 6.5,
          pakaian: 7.5,
          makanan: 5.5,
          elektronik: 7.0,
          aksesoris: 6.0
        },
        note: 'Fee dapat berbeda per kampanye'
      },
      tiktok: {
        name: 'TikTok Shop',
        fees: {
          umum: 7.0,
          pakaian: 8.0,
          makanan: 6.0,
          elektronik: 7.5,
          aksesoris: 7.0
        },
        note: 'Contoh saja'
      }
    };

    // fill fees table
    function populateFeesTable(){
      const tbody = document.getElementById('feesTableBody');
      tbody.innerHTML = '';
      Object.keys(feeData).forEach(platformKey=>{
        const plat = feeData[platformKey];
        Object.keys(plat.fees).forEach((cat, idx)=>{
          const tr = document.createElement('tr');
          tr.innerHTML = '<td>' + plat.name + '</td><td>' + capitalize(cat) + '</td><td>' + plat.fees[cat].toFixed(2) + '%</td><td>' + plat.note + '</td>';
          tbody.appendChild(tr);
        });
      });
    }

    function capitalize(s){ return s.charAt(0).toUpperCase()+s.slice(1); }

    // initial population
    populateFeesTable();
    document.getElementById('year').innerText = new Date().getFullYear();

    // update platform/category default fee
    function updatePlatform(){
      const platform = document.getElementById('platform').value;
      const category = document.getElementById('category').value;
      const fee = (feeData[platform]?.fees?.[category] ?? 6.0);
      document.getElementById('feePlatform').value = fee.toFixed(2);
    }
    function updateCategory(){ updatePlatform(); }

    // show mode toggles
    function showMode(){
      const isPrice = document.getElementById('modePrice').checked;
      document.getElementById('priceModeInputs').style.display = isPrice ? '' : 'none';
      document.getElementById('targetModeInputs').style.display = isPrice ? 'none' : '';
    }

    // reset
    function resetForm(){
      document.getElementById('calcForm').reset();
      updatePlatform();
      showMode();
      calculateFromPrice();
    }

    // core calculations
    function calculateFromPrice(){
      // read inputs
      const hpp = Number(document.getElementById('hpp').value) || 0;
      const packing = Number(document.getElementById('packing').value) || 0;
      const qty = Number(document.getElementById('qty').value) || 1;
      const voucher = Number(document.getElementById('voucher').value) || 0;
      const platformPct = Number(document.getElementById('feePlatform').value) || 0;
      const affiliatePct = Number(document.getElementById('feeAffiliatePct').value) || 0;
      const affiliateRp = Number(document.getElementById('feeAffiliateRp').value) || 0;
      const eventPct = Number(document.getElementById('feeEventPct').value) || 0;
      const eventRp = Number(document.getElementById('feeEventRp').value) || 0;
      const profitWanted = Number(document.getElementById('profitRp').value) || 0;
      const discountPct = Number(document.getElementById('discountPct').value) || 0;
      const inputPrice = Number(document.getElementById('price').value) || 0;

      // price after discount
      const priceAfterDiscount = inputPrice * (1 - discountPct/100);

      // per unit fixed costs
      const costPerUnit = hpp + packing;

      // percentage based cuts (applied on priceAfterDiscount)
      const feePlatformRp = priceAfterDiscount * (platformPct/100);
      const feeEventRp = priceAfterDiscount * (eventPct/100);
      const feeAffiliateFromPctRp = priceAfterDiscount * (affiliatePct/100);
      const feeAffiliateTotalRp = feeAffiliateFromPctRp + affiliateRp;
      const totalCuts = feePlatformRp + feeEventRp + feeAffiliateTotalRp + voucher + eventRp;

      const received = priceAfterDiscount - totalCuts;
      const netProfit = received - costPerUnit;

      // display
      document.getElementById('displayPrice').innerText = formatRp(priceAfterDiscount);
      document.getElementById('displayTotalCuts').innerText = formatRp(totalCuts);
      document.getElementById('displayReceived').innerText = formatRp(received);
      document.getElementById('displayNetProfit').innerText = formatRp(netProfit);

      document.getElementById('showFeePlatform').innerText = platformPct.toFixed(2) + '%';
      document.getElementById('showFeePlatformRp').innerText = formatRp(feePlatformRp);
      document.getElementById('showFeeEventPct').innerText = eventPct.toFixed(2) + '%';
      document.getElementById('showFeeEventRp').innerText = formatRp(eventRp + feeEventRp);
      document.getElementById('showFeeAffPct').innerText = affiliatePct.toFixed(2) + '%';
      document.getElementById('showFeeAffRp').innerText = formatRp(feeAffiliateTotalRp);
      document.getElementById('showVoucherRp').innerText = formatRp(voucher);
    }

    // calculate recommended price when user chooses target received
    function calculateFromTarget(){
      // We'll solve for price P such that:
      // received = P*(1 - discount%) - P*(platform%/100) - P*(event%/100) - P*(affiliate%/100) - fixedCosts = target
      // => P * (1 - discount - platform% - event% - affiliate%) - fixedCuts = target
      const discountPct = Number(document.getElementById('discountPct').value) || 0;
      const platformPct = Number(document.getElementById('feePlatform').value) || 0;
      const eventPct = Number(document.getElementById('feeEventPct').value) || 0;
      const affiliatePct = Number(document.getElementById('feeAffiliatePct').value) || 0;
      const affiliateRp = Number(document.getElementById('feeAffiliateRp').value) || 0;
      const eventRp = Number(document.getElementById('feeEventRp').value) || 0;
      const voucher = Number(document.getElementById('voucher').value) || 0;
      const target = Number(document.getElementById('targetReceived').value) || 0;
      const hpp = Number(document.getElementById('hpp').value) || 0;
      const packing = Number(document.getElementById('packing').value) || 0;

      const fixedCosts = affiliateRp + eventRp + voucher;
      const combinedPct = (discountPct/100) + (platformPct/100) + (eventPct/100) + (affiliatePct/100);
      const denom = 1 - combinedPct;
      let suggestedPrice = 0;
      if(denom <= 0){
        suggestedPrice = NaN;
      } else {
        suggestedPrice = (target + fixedCosts + hpp + packing) / denom;
      }

      // set price input and calculate
      if(!isNaN(suggestedPrice) && isFinite(suggestedPrice)){
        document.getElementById('price').value = Math.round(suggestedPrice);
      } else {
        document.getElementById('price').value = 0;
      }
      calculateFromPrice();
    }

    // decide action on Hitung button
    function detectModeAndCalculate(){
      if(document.getElementById('modePrice').checked){
        calculateFromPrice();
      } else {
        calculateFromTarget();
      }
    }

    // wire buttons to correct behaviour
    document.querySelector('button[onclick="calculateFromPrice()"]').addEventListener('click', function(){
      if(document.getElementById('modePrice').checked){
        calculateFromPrice();
      } else {
        calculateFromTarget();
      }
    });

    // small helper
    function formatRp(n){
      const sign = n < 0 ? '-' : '';
      n = Math.abs(n);
      const s = Math.round(n).toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
      return sign + 'Rp ' + s;
    }

    // initialize defaults
    updatePlatform();
    calculateFromPrice();

    // expose updatePlatform to window for onchange
    window.updatePlatform = updatePlatform;
    window.updateCategory = updateCategory;
    window.showMode = showMode;
    window.calculateFromPrice = calculateFromPrice;
    window.calculateFromTarget = calculateFromTarget;
    window.resetForm = resetForm;
  </script>
</body>
</html>
