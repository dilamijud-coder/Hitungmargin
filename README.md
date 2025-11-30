<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Kalkulator Seller - Modal & Keuntungan</title>
  <style>
    :root{--bg:#f5f7fb;--card:#ffffff;--muted:#6b7280;--accent:#2563eb;--success:#10b981}
    *{box-sizing:border-box}
    body{font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background:var(--bg); color:#0f172a; margin:0; padding:32px}
    .container{max-width:1100px;margin:0 auto}
    header{display:flex;align-items:center;gap:16px;margin-bottom:20px}
    header h1{margin:0;font-size:20px}
    .grid{display:grid;grid-template-columns:1fr 360px;gap:20px}
    .card{background:var(--card);border-radius:14px;padding:20px;box-shadow:0 6px 18px rgba(9,30,66,.06)}
    label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
    input[type=text], input[type=number], select{width:100%;padding:10px;border-radius:8px;border:1px solid #e6e9ef;font-size:15px}
    .row{display:flex;gap:12px}
    .col{flex:1}
    .actions{display:flex;gap:8px;align-items:center;margin-top:12px}
    button{background:var(--accent);color:white;padding:10px 14px;border-radius:10px;border:0;cursor:pointer}
    button.secondary{background:transparent;color:var(--accent);border:1px solid rgba(37,99,235,.12)}
    .result{margin-top:14px;padding:14px;border-radius:10px;background:#f8fafc}
    .result h3{margin:0;font-size:16px}
    .small{font-size:13px;color:var(--muted)}
    table{width:100%;border-collapse:collapse;margin-top:12px}
    th,td{padding:8px;border-bottom:1px solid #f1f5f9;text-align:left;font-size:14px}
    th{font-size:13px;color:var(--muted);}
    .badge{display:inline-block;padding:6px 8px;border-radius:999px;background:#eef2ff;color:#3730a3;font-weight:600;font-size:12px}
    .flex-between{display:flex;justify-content:space-between;align-items:center}
    .note{font-size:13px;color:var(--muted);margin-top:8px}
    footer{margin-top:18px;font-size:13px;color:var(--muted)}
    @media (max-width:900px){.grid{grid-template-columns:1fr}.container{padding:16px}}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="badge">Kalkulator Seller</div>
      <div>
        <h1>Hitung Modal & Harga Jual — Lengkap dan Rapi</h1>
        <div class="small">Masukkan data produk dan pilihan platform, kami akan tampilkan estimasi biaya & harga jual minimal.</div>
      </div>
    </header>

    <div class="grid">
      <main class="card">
        <form id="calcForm" onsubmit="return false;">
          <div class="row" style="margin-bottom:12px">
            <div class="col">
              <label for="hpp">HPP Produk (Rp)</label>
              <input id="hpp" type="number" min="0" step="100" value="50000" />
            </div>
            <div class="col">
              <label for="pack">Biaya packing & tambahan (Rp)</label>
              <input id="pack" type="number" min="0" step="100" value="5000" />
            </div>
          </div>

          <div class="row" style="margin-bottom:12px">
            <div class="col">
              <label for="platform">Platform</label>
              <select id="platform">
                <option value="shopee">Shopee</option>
                <option value="tokopedia">Tokopedia</option>
                <option value="tiktok">TikTok Shop</option>
                <option value="lazada">Lazada</option>
              </select>
            </div>
            <div class="col">
              <label for="category">Kategori</label>
              <select id="category">
                <option value="pakaian">Pakaian</option>
                <option value="makanan">Makanan & Minuman</option>
                <option value="elektronik">Elektronik</option>
                <option value="aksesoris">Aksesoris</option>
                <option value="kecantikan">Kecantikan</option>
                <option value="lainnya">Lainnya</option>
              </select>
            </div>
          </div>

          <div style="margin-bottom:12px">
            <label for="affiliate">Estimasi biaya affiliate (%)</label>
            <input id="affiliate" type="number" min="0" max="100" step="0.1" value="5" />
          </div>

          <div style="margin-bottom:12px">
            <label>Fee event tambahan</label>
            <div class="small" style="margin-bottom:6px">Centang jika mengikuti event/biaya tambahan</div>
            <label><input type="checkbox" id="gratisOngkir" /> Gratis Ongkir (estimasi potongan Rp) </label><br/>
            <label><input type="checkbox" id="campaign" /> Campaign/Event (persentase tambahan)</label>
          </div>

          <div style="margin-bottom:12px">
            <label for="voucher">Estimasi biaya voucher (Rp)</label>
            <input id="voucher" type="number" min="0" step="100" value="0" />
          </div>

          <div style="margin-bottom:12px">
            <label for="profitPct">Keuntungan yang diinginkan (%)</label>
            <input id="profitPct" type="number" min="0" max="500" step="0.1" value="20" />
          </div>

          <div class="actions">
            <button id="calculate">Hitung Harga Jual</button>
            <button class="secondary" id="reset">Reset</button>
            <div style="margin-left:auto" class="small">Hasil akan muncul di panel hasil.</div>
          </div>

        </form>

        <div id="results" class="result" aria-live="polite" hidden>
          <div class="flex-between"><h3>Ringkasan Perhitungan</h3><div class="small" id="stamp">--</div></div>
          <div style="display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-top:10px">
            <div class="small">Total Modal (HPP + Packing)</div>
            <div id="totalModal" class="small">Rp 0</div>

            <div class="small">Estimasi Fee Platform</div>
            <div id="feePlatform" class="small">Rp 0</div>

            <div class="small">Estimasi Affiliate</div>
            <div id="feeAffiliate" class="small">Rp 0</div>

            <div class="small">Fee Event & Voucher</div>
            <div id="feeEventVoucher" class="small">Rp 0</div>

            <div class="small">Total Biaya (termasuk fee)</div>
            <div id="totalCost" style="font-weight:700">Rp 0</div>

            <div class="small">Keuntungan yang diinginkan</div>
            <div id="desiredProfit" class="small">Rp 0</div>

            <div class="small">Harga Jual Minimal</div>
            <div id="suggestedPrice" style="font-weight:800;font-size:18px">Rp 0</div>

            <div class="small">Margin (Rp)</div>
            <div id="marginRp" class="small">Rp 0</div>

            <div class="small">Margin (%)</div>
            <div id="marginPct" class="small">0%</div>
          </div>

          <div style="margin-top:12px;display:flex;gap:8px">
            <button id="copyPrice" class="secondary">Copy Harga Jual</button>
            <button id="exportCSV">Export CSV</button>
          </div>
        </div>

      </main>

      <aside class="card">
        <div class="flex-between"><h3>Daftar Fee Platform (contoh)</h3></div>
        <div class="small">Tabel ini bersifat tetap / tidak dapat diedit — gunakan untuk referensi.</div>

        <table aria-describedby="fee-note">
          <thead>
            <tr><th>Platform</th><th>Kategori</th><th>Fee (%)</th></tr>
          </thead>
          <tbody>
            <tr><td>Shopee</td><td>Pakaian</td><td>2.5%</td></tr>
            <tr><td>Shopee</td><td>Makanan & Minuman</td><td>3.0%</td></tr>
            <tr><td>Tokopedia</td><td>Umum</td><td>2.0%</td></tr>
            <tr><td>TikTok Shop</td><td>Umum</td><td>2.5%</td></tr>
            <tr><td>Lazada</td><td>Elektronik</td><td>2.8%</td></tr>
          </tbody>
        </table>

        <h4 style="margin-top:14px">Contoh Biaya Event</h4>
        <table>
          <thead><tr><th>Event</th><th>Biaya</th></tr></thead>
          <tbody>
            <tr><td>Gratis Ongkir</td><td>Rp 5.000 (estimasi per item)</td></tr>
            <tr><td>Campaign</td><td>3% dari harga jual</td></tr>
            <tr><td>Voucher</td><td>Nominal input pengguna</td></tr>
          </tbody>
        </table>

        <div id="fee-note" class="note">Catatan: Angka di tabel adalah contoh umum. Untuk akurasi, sesuaikan dengan syarat & ketentuan masing-masing platform.</div>
      </aside>
    </div>

    <footer class="small">Dibuat otomatis — modifikasi fee platform pada variabel di skrip bila perlu.</footer>
  </div>

  <script>
    // Fee matrix contoh (persentase dari harga jual)
    const feeMatrix = {
      shopee: { pakaian: 2.5, makanan: 3.0, elektronik:2.8, aksesoris:2.5, kecantikan:2.5, lainnya:2.5 },
      tokopedia: { pakaian:2.0, makanan:2.5, elektronik:2.5, aksesoris:2.0, kecantikan:2.0, lainnya:2.0 },
      tiktok: { pakaian:2.5, makanan:3.5, elektronik:2.7, aksesoris:2.6, kecantikan:2.8, lainnya:2.8 },
      lazada: { pakaian:2.6, makanan:3.2, elektronik:3.0, aksesoris:2.6, kecantikan:2.7, lainnya:2.6 }
    };

    const el = id => document.getElementById(id);
    const format = n => {
      const x = Number(n);
      if (!isFinite(x)) return 'Rp 0';
      return 'Rp ' + x.toLocaleString('id-ID');
    }

    function calculate(){
      const hpp = Number(el('hpp').value) || 0;
      const pack = Number(el('pack').value) || 0;
      const platform = el('platform').value;
      const category = el('category').value;
      const affiliatePct = Number(el('affiliate').value) || 0;
      const voucher = Number(el('voucher').value) || 0;
      const profitPct = Number(el('profitPct').value) || 0;
      const gratisOngkir = el('gratisOngkir').checked;
      const campaign = el('campaign').checked;

      const totalModal = hpp + pack;

      // platform fee calculated as percentage of price (we approximate using price variable later)
      const platformFeePct = (feeMatrix[platform] && feeMatrix[platform][category]) ? feeMatrix[platform][category] : 2.5;

      // We'll solve for price using algebra: Price = TotalCost + desiredProfit, but platform & affiliate are percent of Price ->
      // Let P = price. Total variable absolute costs = totalModal + voucher + (gratisOngkir ? 5000 : 0)
      // Fees that are percentage of P: platformFeePct% and affiliatePct% and campaign ? 3% : 0
      const campaignPct = campaign ? 3.0 : 0.0;
      const percentFees = (platformFeePct + affiliatePct + campaignPct) / 100.0; // fraction of price

      const absoluteCosts = totalModal + voucher + (gratisOngkir ? 5000 : 0);

      // Desired profit is profitPct% of price or of cost? We'll interpret as margin based on price: desiredProfit = profitPct% of price.
      // Alternatively user wants 'keuntungan yang diinginkan' on top of cost. We'll implement as percentage of price (common in marketplace sellers).
      const desiredProfitFraction = profitPct / 100.0;

      // We want P such that: P = absoluteCosts + percentFees * P + desiredProfitFraction * P
      // => P * (1 - percentFees - desiredProfitFraction) = absoluteCosts
      const denom = 1 - percentFees - desiredProfitFraction;
      let priceSuggested;
      if (denom <= 0) {
        priceSuggested = NaN; // impossible with given percentages
      } else {
        priceSuggested = absoluteCosts / denom;
      }

      // compute breakdown based on P
      const feePlatformRp = isFinite(priceSuggested) ? priceSuggested * (platformFeePct/100) : 0;
      const feeAffiliateRp = isFinite(priceSuggested) ? priceSuggested * (affiliatePct/100) : 0;
      const feeCampaignRp = isFinite(priceSuggested) ? (campaign ? priceSuggested * (campaignPct/100) : 0) : 0;
      const feeEventVoucher = voucher + (gratisOngkir ? 5000 : 0) + feeCampaignRp;
      const totalCost = totalModal + feePlatformRp + feeAffiliateRp + feeEventVoucher;
      const desiredProfitRp = isFinite(priceSuggested) ? priceSuggested * desiredProfitFraction : 0;
      const marginRp = isFinite(priceSuggested) ? priceSuggested - totalCost : 0;
      const marginPct = isFinite(priceSuggested) ? (marginRp / priceSuggested * 100) : 0;

      el('results').hidden = false;
      el('stamp').textContent = new Date().toLocaleString('id-ID');
      el('totalModal').textContent = format(totalModal);
      el('feePlatform').textContent = format(feePlatformRp);
      el('feeAffiliate').textContent = format(feeAffiliateRp);
      el('feeEventVoucher').textContent = format(feeEventVoucher);
      el('totalCost').textContent = format(totalCost);
      el('desiredProfit').textContent = format(desiredProfitRp);
      el('suggestedPrice').textContent = isFinite(priceSuggested) ? format(Math.ceil(priceSuggested/100)*100) : 'Tidak dapat dihitung — cek persentase';
      el('marginRp').textContent = format(marginRp);
      el('marginPct').textContent = isFinite(marginPct) ? marginPct.toFixed(1) + '%' : '-';

      // store last computed for other actions
      window._lastResult = {
        priceSuggested: priceSuggested,
        roundedPrice: Math.ceil(priceSuggested/100)*100,
        totalCost, feePlatformRp, feeAffiliateRp, feeEventVoucher, desiredProfitRp, marginRp, marginPct
      };
    }

    document.getElementById('calculate').addEventListener('click', calculate);
    document.getElementById('reset').addEventListener('click', function(){
      document.getElementById('calcForm').reset();
      el('hpp').value = 50000; el('pack').value = 5000; el('affiliate').value = 5; el('voucher').value = 0; el('profitPct').value = 20;
      el('results').hidden = true;
    });

    document.getElementById('copyPrice').addEventListener('click', function(){
      if (!window._lastResult || !isFinite(window._lastResult.roundedPrice)) return alert('Silakan hitung terlebih dahulu.');
      navigator.clipboard && navigator.clipboard.writeText(String(window._lastResult.roundedPrice));
      alert('Harga jual disalin: Rp ' + window._lastResult.roundedPrice.toLocaleString('id-ID'));
    });

    document.getElementById('exportCSV').addEventListener('click', function(){
      if (!window._lastResult || !isFinite(window._lastResult.priceSuggested)) return alert('Silakan hitung terlebih dahulu.');
      const lines = [
        ['Waktu','HPP+Packing','FeePlatform','FeeAffiliate','FeeEventVoucher','TotalCost','DesiredProfit','SuggestedPrice','MarginRp','MarginPct'].join(','),
        [new Date().toISOString(), el('hpp').value+'+'+el('pack').value, window._lastResult.feePlatformRp, window._lastResult.feeAffiliateRp, window._lastResult.feeEventVoucher, window._lastResult.totalCost, window._lastResult.desiredProfitRp, window._lastResult.roundedPrice, window._lastResult.marginRp, window._lastResult.marginPct].join(',')
      ].join('\n');

      const blob = new Blob([lines],{type:'text/csv'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a'); a.href = url; a.download = 'kalkulasi_seller.csv'; a.click(); URL.revokeObjectURL(url);
    });

    // small enhancement: recalc when user changes key fields
    ['hpp','pack','platform','category','affiliate','voucher','profitPct'].forEach(id => document.getElementById(id).addEventListener('change', ()=>{/*no auto*/}));
  </script>
</body>
</html>
