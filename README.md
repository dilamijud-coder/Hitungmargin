<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kalkulator Bisnis Online - Harga Jual</title>
  <style>
    body{font-family:system-ui,Segoe UI,Roboto,Arial;background:#f7fafc;color:#102a43;padding:20px}
    .card{background:white;border-radius:12px;box-shadow:0 6px 18px rgba(16,42,67,0.08);padding:18px;margin-bottom:16px}
    label{display:block;margin:8px 0 4px;font-weight:600}
    input,select,textarea{width:100%;padding:10px;border:1px solid #dfe6ee;border-radius:8px}
    .grid{display:grid;grid-template-columns:1fr 1fr;gap:12px}
    .full{grid-column:1/-1}
    button{background:#0b69ff;color:white;border:none;padding:10px 14px;border-radius:8px;cursor:pointer}
    table{width:100%;border-collapse:collapse;margin-top:8px}
    th,td{padding:8px;border:1px solid #eef3f8;text-align:left}
    small{color:#5b7088}
  </style>
</head>
<body>
  <h1>Kalkulator Harga Jual & Estimasi Keuntungan</h1>
  <p class="small">Isi data HPP (Harga Pokok Produksi), pilih platform atau masukkan fee sendiri, tambahkan biaya event jika ada, lalu tentukan persentase keuntungan yang diinginkan.</p>

  <div class="card">
    <div class="grid">
      <div>
        <label>HPP (Rp)</label>
        <input id="hpp" type="number" value="50000" />
      </div>
      <div>
        <label>Biaya packing / ongkos tambahan (Rp)</label>
        <input id="extra_cost" type="number" value="2000" />
      </div>

      <div>
        <label>Platform</label>
        <select id="platform">
          <option value="custom">Masukkan manual / Lainnya</option>
          <option value="shopee">Shopee</option>
          <option value="tokopedia">Tokopedia</option>
          <option value="lazada">Lazada</option>
          <option value="tiktok">TikTok Shop</option>
        </select>
      </div>
      <div>
        <label>Kategori / Fee (%) — jika tidak tahu pilih "Default"</label>
        <select id="category">
          <option value="default">Gunakan default / rata-rata</option>
          <option value="1">Kategori A (contoh)</option>
          <option value="2">Kategori B (contoh)</option>
          <option value="3">Kategori C (contoh)</option>
        </select>
      </div>

      <div class="full">
        <label>Atau masukkan Fee Platform (%) secara manual</label>
        <input id="platform_fee_manual" type="number" step="0.01" placeholder="Contoh: 5" />
      </div>

      <div>
        <label>Biaya event (flat Rp atau %). Pilih tipe:</label>
        <select id="event_type">
          <option value="none">Tidak ada</option>
          <option value="flat">Flat (Rp)</option>
          <option value="percent">Persentase (%)</option>
        </select>
      </div>
      <div>
        <label>Nilai Biaya Event</label>
        <input id="event_value" type="number" value="0" />
      </div>

      <div>
        <label>Keuntungan yang diinginkan (%)</label>
        <input id="profit_pct" type="number" value="20" />
      </div>
      <div>
        <label>Pajak / Lain-lain (%)</label>
        <input id="tax_pct" type="number" value="0" />
      </div>

      <div class="full">
        <button id="calc">Hitung Harga Jual</button>
      </div>
    </div>
  </div>

  <div class="card">
    <h3>Hasil Perhitungan</h3>
    <table>
      <tr><th>Item</th><th>Nilai</th></tr>
      <tr><td>HPP + ekstra</td><td id="cell_cost">-</td></tr>
      <tr><td>Biaya platform (Rp)</td><td id="cell_platform_fee">-</td></tr>
      <tr><td>Biaya event (Rp)</td><td id="cell_event_fee">-</td></tr>
      <tr><td>Pajak (Rp)</td><td id="cell_tax">-</td></tr>
      <tr><td><strong>Harga pokok total sebelum keuntungan (Rp)</strong></td><td id="cell_total_cost">-</td></tr>
      <tr><td>Keuntungan yang diinginkan (Rp)</td><td id="cell_profit_rp">-</td></tr>
      <tr><td><strong>Harga jual yang disarankan (Rp)</strong></td><td id="cell_selling_price">-</td></tr>
    </table>

    <p><small>Catatan: referensi fee platform bervariasi tiap kategori dan dapat berubah. Gunakan input manual jika tidak yakin.</small></p>
  </div>

  <div class="card">
    <h3>Data Fee Platform (default/range contoh)</h3>
    <p><small>Nilai di bawah ini adalah contoh default/rata-rata berdasarkan sumber publik (harap cek sumber resmi di web untuk daftar kategori lengkap).</small></p>
    <table>
      <tr><th>Platform</th><th>Rentang Fee (%)</th></tr>
      <tr><td>Shopee</td><td>~2.9% – 9.2% (bergantung kategori & promo)</td></tr>
      <tr><td>Tokopedia</td><td>Official Store: ~1% – 5%; Power Merchant: ~1.5% – 3.5% (kategori berdasar dokumen)</td></tr>
      <tr><td>Lazada</td><td>~4% – 18% (tergantung kategori + biaya proses Rp per order)</td></tr>
      <tr><td>TikTok Shop</td><td>Komisi dinamis ~4% – 6% (peraturan 2025, batas per item berlaku)</td></tr>
    </table>
  </div>

  <script>
    // Default fee database (contoh). User bisa override lewat input manual.
    const defaults = {
      shopee: {default:5.5},
      tokopedia: {default:3.0},
      lazada: {default:9.0},
      tiktok: {default:5.0}
    };

    function fmt(n){return typeof n==='number' ? n.toLocaleString('id-ID') : n}

    document.getElementById('calc').addEventListener('click', ()=>{
      const hpp = Number(document.getElementById('hpp').value)||0;
      const extra = Number(document.getElementById('extra_cost').value)||0;
      const platform = document.getElementById('platform').value;
      const manualFee = document.getElementById('platform_fee_manual').value;
      let feePct = manualFee ? Number(manualFee) : (defaults[platform]?.default || 0);
      const eventType = document.getElementById('event_type').value;
      const eventValue = Number(document.getElementById('event_value').value)||0;
      const profitPct = Number(document.getElementById('profit_pct').value)||0;
      const taxPct = Number(document.getElementById('tax_pct').value)||0;

      const baseCost = hpp + extra;

      // platform fee Rp (applied to harga jual later or to base? common approach: commission = fee% * selling price)
      // We'll compute iteratively because commission depends on selling price if fee is % of selling price.

      // First compute target price solving: selling_price = total_cost + profit
      // But commission is % of selling_price: commission = feePct/100 * selling_price
      // total_cost = baseCost + eventFee(Rp) + tax
      // eventFee can be percent of selling price or flat.

      // We'll solve for selling_price: selling = baseCost + eventFee + tax + profit
      // profit = profitPct/100 * selling
      // tax = taxPct/100 * selling
      // eventFee if percent: ev = eventPct/100 * selling, else flat.

      let selling = 0;
      // if event is percent, it's part of variable fees; else it's flat.
      if(eventType==='percent'){
        const evPct = eventValue/100;
        const feePctDec = feePct/100;
        const profitDec = profitPct/100;
        const taxDec = taxPct/100;
        // commission and event percent and tax and profit are all % of selling
        // selling = baseCost + flat(0) + commission + eventPct*selling + tax + profit
        // commission = feePctDec * selling
        // thus selling = baseCost + (feePctDec + evPct + taxDec + profitDec) * selling
        const denom = 1 - (feePctDec + evPct + taxDec + profitDec);
        if(denom<=0){ alert('Persentase terlalu besar sehingga tidak bisa dihitung (jumlah % >=100).'); return; }
        selling = baseCost / denom;
      } else {
        const feePctDec = feePct/100;
        const profitDec = profitPct/100;
        const taxDec = taxPct/100;
        const eventFlat = eventType==='flat' ? eventValue : 0;
        // commission depends on selling: commission = feePctDec * selling
        // selling = baseCost + eventFlat + feePctDec*selling + taxDec*selling + profitDec*selling
        const denom = 1 - (feePctDec + taxDec + profitDec);
        if(denom<=0){ alert('Persentase terlalu besar sehingga tidak bisa dihitung (jumlah % >=100).'); return; }
        selling = (baseCost + eventFlat) / denom;
      }

      const commissionRp = selling * (feePct/100);
      const taxRp = selling * (taxPct/100);
      const eventRp = eventType==='percent' ? selling * (eventValue/100) : (eventType==='flat' ? eventValue : 0);
      const profitRp = selling * (profitPct/100);
      const totalCost = baseCost + commissionRp + eventRp + taxRp;

      document.getElementById('cell_cost').innerText = 'Rp ' + fmt(baseCost);
      document.getElementById('cell_platform_fee').innerText = 'Rp ' + fmt(Math.round(commissionRp)) + ' (' + feePct + '%)';
      document.getElementById('cell_event_fee').innerText = 'Rp ' + fmt(Math.round(eventRp));
      document.getElementById('cell_tax').innerText = 'Rp ' + fmt(Math.round(taxRp));
      document.getElementById('cell_total_cost').innerText = 'Rp ' + fmt(Math.round(totalCost));
      document.getElementById('cell_profit_rp').innerText = 'Rp ' + fmt(Math.round(profitRp));
      document.getElementById('cell_selling_price').innerText = 'Rp ' + fmt(Math.round(selling));
    });

  </script>
</body>
</html>
