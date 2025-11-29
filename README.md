<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Kalkulator Keuntungan & Harga Jual — Toko Online</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--muted:#94a3b8;--accent:#7c3aed;--glass: rgba(255,255,255,0.03)}
    *{box-sizing:border-box;font-family:Inter,ui-sans-serif,system-ui,-apple-system,'Segoe UI',Roboto,'Helvetica Neue',Arial}
    body{margin:0;background:linear-gradient(180deg,#071024 0%, #071933 60%);color:#e6eef8;min-height:100vh;padding:36px}
    .container{max-width:1100px;margin:0 auto}
    header{display:flex;align-items:center;gap:16px;margin-bottom:20px}
    h1{font-size:20px;margin:0}
    p.lead{margin:0;color:var(--muted)}
    .grid{display:grid;gap:18px}
    .cols-3{grid-template-columns:repeat(3,1fr)}
    .cols-2{grid-template-columns:repeat(2,1fr)}
    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));padding:18px;border-radius:12px;box-shadow:0 6px 18px rgba(2,6,23,0.6);border:1px solid rgba(255,255,255,0.03)}
    label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
    input[type="number"], select{width:100%;padding:10px;border-radius:8px;border:1px solid rgba(255,255,255,0.04);background:transparent;color:inherit;font-size:14px}
    .row{display:flex;gap:12px}
    .btn{display:inline-block;padding:10px 14px;border-radius:10px;background:var(--accent);border:none;color:white;cursor:pointer}
    .small{font-size:13px;color:var(--muted)}
    table{width:100%;border-collapse:collapse;background:transparent}
    th,td{padding:8px 10px;border-bottom:1px dashed rgba(255,255,255,0.03);text-align:left;font-size:13px}
    th{color:var(--muted);font-weight:600}
    .muted{color:var(--muted)}
    .result{font-weight:700;font-size:16px}
    .flex-between{display:flex;justify-content:space-between;align-items:center}
    footer{margin-top:18px;color:var(--muted);font-size:13px}
    .tables-wrap{display:grid;grid-template-columns:1fr 1fr;gap:18px}
    .note{background:var(--glass);padding:10px;border-radius:8px;color:var(--muted);font-size:13px}
    @media(max-width:900px){.cols-3,.cols-2{grid-template-columns:1fr}.tables-wrap{grid-template-columns:1fr}}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="card" style="padding:12px;border-radius:10px">
        <svg width="32" height="32" viewBox="0 0 24 24" fill="none" aria-hidden>
          <path d="M12 2v20M2 12h20" stroke="white" stroke-opacity="0.9" stroke-width="1.2" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </div>
      <div>
        <h1>Kalkulator Keuntungan & Harga Jual</h1>
        <p class="lead">Hitung harga jual, estimasi fee platform, efek event, dan ROAS iklan — cepat & rapi.</p>
      </div>
    </header>

    <section class="grid card" style="gap:20px">
      <div>
        <h2 style="margin:0 0 8px 0">Kalkulator Harga Jual & Keuntungan</h2>
        <p class="small">Masukkan HPP, packing, pilih platform & kategori. Tabel fee di bawah adalah permanen (tidak bisa diedit).</p>
      </div>

      <div class="grid cols-3">
        <div>
          <label>HPP produk (Rp)</label>
          <input id="hpp" type="number" min="0" value="100000" />
        </div>
        <div>
          <label>Biaya packing & tambahan (Rp)</label>
          <input id="packing" type="number" min="0" value="5000" />
        </div>
        <div>
          <label>Platform</label>
          <select id="platform">
            <option value="shopee">Shopee</option>
            <option value="tiktok">Tiktok Shop</option>
            <option value="tokped">Tokopedia</option>
            <option value="lazada">Lazada</option>
          </select>
        </div>
      </div>

      <div class="grid cols-3">
        <div>
          <label>Kategori</label>
          <select id="category">
            <option value="pakaian">Pakaian</option>
            <option value="aksesoris">Aksesoris</option>
            <option value="makanan">Makanan</option>
            <option value="elektronik">Elektronik</option>
            <option value="kecantikan">Kecantikan</option>
            <option value="lainnya">Lainnya</option>
          </select>
        </div>
        <div>
          <label>Event (opsional)</label>
          <select id="event">
            <option value="none" selected>Tidak ikut event</option>
            <option value="gratis_ongkir">Gratis Ongkir (seller tanggung ongkir)</option>
            <option value="tanggal_kembar">Tanggal Kembar (diskon campaign)</option>
            <option value="event_gajian">Event Gajian (diskon & biaya campaign)</option>
          </select>
        </div>
        <div>
          <label>Keuntungan yang diinginkan (%)</label>
          <input id="profit_percent" type="number" min="0" value="30" />
        </div>
      </div>

      <div class="row" style="justify-content:flex-end">
        <button class="btn" onclick="calculatePrice()">Hitung Harga Jual</button>
      </div>

      <div class="card">
        <div class="flex-between">
          <div>
            <div class="small">Rincian</div>
            <div id="rincian" class="result">Belum dihitung</div>
          </div>
          <div style="text-align:right">
            <div class="small">Harga jual minimal (Rp)</div>
            <div id="harga_jual" class="result">-</div>
          </div>
        </div>
      </div>
    </section>

    <section style="margin-top:18px" class="grid">
      <div class="card">
        <h3 style="margin-top:0">Tabel Fee Platform & Kategori (PERMANEN)</h3>
        <p class="small">Tabel ini bersifat tetap sebagai referensi untuk perhitungan. Nilai persentase adalah contoh yang realistis — sesuaikan jika ada update platform.</p>
        <div class="tables-wrap" style="margin-top:12px">
          <div>
            <table id="platform-fees">
              <thead><tr><th>Platform</th><th>Kategori</th><th>Fee (%)</th></tr></thead>
              <tbody>
                <tr><td>Shopee</td><td>Pakaian</td><td>2.5</td></tr>
                <tr><td>Shopee</td><td>Aksesoris</td><td>2.5</td></tr>
                <tr><td>Shopee</td><td>Makanan</td><td>2.0</td></tr>
                <tr><td>Shopee</td><td>Elektronik</td><td>2.5</td></tr>
                <tr><td>Tiktok Shop</td><td>Pakaian</td><td>3.5</td></tr>
                <tr><td>Tiktok Shop</td><td>Aksesoris</td><td>3.5</td></tr>
                <tr><td>Tiktok Shop</td><td>Makanan</td><td>3.0</td></tr>
                <tr><td>Tokopedia</td><td>Pakaian</td><td>2.0</td></tr>
                <tr><td>Tokopedia</td><td>Aksesoris</td><td>2.0</td></tr>
                <tr><td>Tokopedia</td><td>Makanan</td><td>2.0</td></tr>
                <tr><td>Lazada</td><td>Pakaian</td><td>2.8</td></tr>
                <tr><td>Lazada</td><td>Aksesoris</td><td>2.8</td></tr>
                <tr><td>Lazada</td><td>Makanan</td><td>2.5</td></tr>
              </tbody>
            </table>
          </div>

          <div>
            <table id="event-fees">
              <thead><tr><th>Event</th><th>Tipe</th><th>Nilai</th></tr></thead>
              <tbody>
                <tr><td>Gratis Ongkir</td><td>Flat (Rp)</td><td>15000</td></tr>
                <tr><td>Tanggal Kembar</td><td>% Tambahan biaya campaign</td><td>5</td></tr>
                <tr><td>Event Gajian</td><td>% Tambahan biaya campaign</td><td>4</td></tr>
                <tr><td>Diskon Campaign</td><td>% Potongan harga (jika seller ikut potong harga)</td><td>10</td></tr>
              </tbody>
            </table>
          </div>
        </div>

        <p class="note" style="margin-top:10px">Catatan: Nilai di tabel bersifat permanen di halaman ini. Jika platform mengubah kebijakan, Anda perlu update tabel di file HTML.</p>
      </div>

      <div class="card">
        <h3 style="margin-top:0">Kalkulator ROAS Iklan</h3>
        <div class="grid cols-2">
          <div>
            <label>HPP produk (Rp)</label>
            <input id="hpp_roas" type="number" min="0" value="100000" />
          </div>
          <div>
            <label>Modal iklan per hari (Rp)</label>
            <input id="ad_spend" type="number" min="0" value="50000" />
          </div>
        </div>
        <div class="grid cols-2" style="margin-top:10px">
          <div>
            <label>Target keuntungan per hari dari iklan (Rp)</label>
            <input id="target_profit" type="number" min="0" value="150000" />
          </div>
          <div>
            <label>Rata-rata margin per produk (%)</label>
            <input id="margin_pct" type="number" min="0" value="30" />
          </div>
        </div>
        <div style="margin-top:12px;text-align:right">
          <button class="btn" onclick="calculateROAS()">Hitung ROAS</button>
        </div>

        <div style="margin-top:12px">
          <div class="small">Hasil ROAS</div>
          <div id="roas_result" class="result">Belum dihitung</div>
          <div class="small muted" style="margin-top:8px">Penjelasan singkat: ROAS = Pendapatan dari iklan / Modal iklan. Jika target keuntungan tercapai setelah biaya HPP dan margin, sistem menghitung berapa pendapatan yang dibutuhkan dari iklan.</div>
        </div>
      </div>
    </section>

    <footer class="card" style="margin-top:18px">
      <div class="small">File: kalkulator_keuntungan_harga_jual.html — Buat tampilan rapi & elegan. Tabel fee bersifat permanen pada halaman ini. Jika ingin versi ZIP atau tambahan fitur (export, simpan), beritahu saya.</div>
    </footer>
  </div>

  <script>
    // Data fees (digunakan di perhitungan). Jika ingin update, ubah nilai di sini.
    const platformFees = {
      shopee: {pakaian:2.5, aksesoris:2.5, makanan:2.0, elektronik:2.5, kecantikan:2.5, lainnya:2.5},
      tiktok: {pakaian:3.5, aksesoris:3.5, makanan:3.0, elektronik:3.5, kecantikan:3.5, lainnya:3.5},
      tokped: {pakaian:2.0, aksesoris:2.0, makanan:2.0, elektronik:2.0, kecantikan:2.0, lainnya:2.0},
      lazada: {pakaian:2.8, aksesoris:2.8, makanan:2.5, elektronik:2.8, kecantikan:2.8, lainnya:2.8}
    };

    const eventFees = {
      none: {type:'none', value:0},
      gratis_ongkir: {type:'flat', value:15000}, // seller menanggung biaya ongkir per transaksi
      tanggal_kembar: {type:'percent', value:5}, // tambahan biaya campaign sebagai persen dari harga jual
      event_gajian: {type:'percent', value:4}
    };

    function formatRupiah(x){
      if(isNaN(x) || x===Infinity) return '-';
      return 'Rp ' + Number(x).toLocaleString('id-ID');
    }

    function calculatePrice(){
      const hpp = Number(document.getElementById('hpp').value) || 0;
      const packing = Number(document.getElementById('packing').value) || 0;
      const platform = document.getElementById('platform').value;
      const category = document.getElementById('category').value;
      const event = document.getElementById('event').value;
      const profitPct = Number(document.getElementById('profit_percent').value) || 0;

      const platformFeePct = platformFees[platform][category] || 0;
      const eventInfo = eventFees[event] || {type:'none', value:0};

      // biaya dasar
      const biayaDasar = hpp + packing;

      // jika event gratis ongkir -> seller tanggung flat ongkir
      let biayaEventFlat = 0;
      let biayaEventPct = 0;
      if(eventInfo.type === 'flat') biayaEventFlat = Number(eventInfo.value);
      if(eventInfo.type === 'percent') biayaEventPct = Number(eventInfo.value);

      // kita asumsikan platform fee dihitung dari harga jual (persentase)
      // rumus: harga_jual minimal such that: (harga_jual - platform_fee - biayaEventFlat) - biayaDasar >= profitPct% dari (biayaDasar)
      // platform_fee = harga_jual * platformFeePct/100
      // event pct biaya campaign (jika ada) dianggap sebagai tambahan persentase biaya dari harga_jual

      // Let H = harga_jual
      // profitWanted = profitPct% * (biayaDasar)
      const profitWanted = (profitPct/100) * biayaDasar;

      // total percentage fees on price
      const totalPctOnPrice = platformFeePct/100 + (biayaEventPct/100);

      // Solve H such that: (H - H*totalPctOnPrice - biayaEventFlat) - biayaDasar >= profitWanted
      // => H*(1 - totalPctOnPrice) - biayaEventFlat - biayaDasar = profitWanted
      // => H = (profitWanted + biayaEventFlat + biayaDasar) / (1 - totalPctOnPrice)

      let denom = (1 - totalPctOnPrice);
      let hargaJual = denom <= 0 ? Infinity : (profitWanted + biayaEventFlat + biayaDasar) / denom;

      // round up to nearest 100
      if(hargaJual !== Infinity) hargaJual = Math.ceil(hargaJual/100)*100;

      const platformFeeRp = hargaJual === Infinity ? Infinity : hargaJual * (platformFeePct/100);
      const eventPctFeeRp = hargaJual === Infinity ? Infinity : hargaJual * (biayaEventPct/100);

      // rincian text
      let rincian = `Biaya dasar: ${formatRupiah(biayaDasar)} — Platform fee: ${platformFeePct}% (${formatRupiah(platformFeeRp)})`;
      if(biayaEventFlat) rincian += ` — Biaya event (flat): ${formatRupiah(biayaEventFlat)}`;
      if(biayaEventPct) rincian += ` — Biaya event (pct): ${biayaEventPct}% (${formatRupiah(eventPctFeeRp)})`;
      rincian += ` — Target profit: ${profitPct}% dari biaya dasar (${formatRupiah(profitWanted)})`;

      document.getElementById('rincian').innerText = rincian;
      document.getElementById('harga_jual').innerText = hargaJual===Infinity? 'Tidak bisa dihitung (fee% >= 100%)' : formatRupiah(hargaJual);
    }

    function calculateROAS(){
      const hpp = Number(document.getElementById('hpp_roas').value) || 0;
      const adSpend = Number(document.getElementById('ad_spend').value) || 0;
      const targetProfit = Number(document.getElementById('target_profit').value) || 0;
      const marginPct = Number(document.getElementById('margin_pct').value) || 0;

      // Kita anggap margin% adalah persentase keuntungan kotor per penjualan terhadap harga jual.
      // Untuk sederhana, kita hitung revenue yang dibutuhkan agar margin * revenue - adSpend >= targetProfit
      // atau: revenue * (marginPct/100) - adSpend >= targetProfit  => revenue >= (targetProfit + adSpend) / (marginPct/100)

      const marginRatio = marginPct/100 || 0.001; // hindari div0
      const revenueNeeded = (targetProfit + adSpend) / marginRatio;
      const roasNeeded = adSpend === 0 ? Infinity : revenueNeeded / adSpend;

      document.getElementById('roas_result').innerText = adSpend === 0 ? 'Modal iklan = 0, ROAS tak terdefinisi' :
        `Pendapatan yg dibutuhkan: ${formatRupiah(Math.ceil(revenueNeeded))} — ROAS minimal: ${roasNeeded.toFixed(2)}x`;
    }

    // inisialisasi default
    calculatePrice();
    calculateROAS();
  </script>
</body>
</html>
