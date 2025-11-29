<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Kalkulator Keuntungan & ROAS</title>
  <style>
    :root{--bg:#0f1724;--card:#0b1220;--muted:#9aa4b2;--accent:#7c3aed;--glass:rgba(255,255,255,0.03)}
    *{box-sizing:border-box}
    body{font-family:Inter,ui-sans-serif,system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;margin:0;background:linear-gradient(180deg,#071023 0%, #071a2b 100%);color:#e6eef6;-webkit-font-smoothing:antialiased}
    .container{max-width:980px;margin:20px auto;padding:18px}
    header{display:flex;gap:12px;align-items:center;margin-bottom:12px}
    .logo{width:56px;height:56px;border-radius:12px;background:linear-gradient(135deg,#6ee7b7,#60a5fa);display:flex;align-items:center;justify-content:center;font-weight:700;color:#032;box-shadow:0 6px 20px rgba(2,6,23,0.6)}
    h1{font-size:18px;margin:0}
    p.lead{margin:0;color:var(--muted);font-size:13px}

    .grid{display:grid;grid-template-columns:1fr;gap:12px}
    @media(min-width:880px){.grid{grid-template-columns:1fr 420px}}

    .card{background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01));border-radius:14px;padding:14px;box-shadow:0 6px 30px rgba(2,6,23,0.6);border:1px solid rgba(255,255,255,0.03)}
    .section-title{font-weight:600;margin-bottom:8px}

    label{display:block;font-size:13px;color:var(--muted);margin-bottom:6px}
    input[type="text"], input[type="number"], select{width:100%;padding:10px;border-radius:10px;border:1px solid rgba(255,255,255,0.04);background:var(--glass);color:inherit;font-size:14px}
    .row{display:grid;grid-template-columns:1fr 1fr;gap:8px}
    .row-3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:8px}

    .btn{display:inline-block;padding:10px 12px;border-radius:10px;background:linear-gradient(90deg,var(--accent),#4f46e5);color:white;font-weight:600;border:none;cursor:pointer}
    .muted{color:var(--muted);font-size:13px}

    table{width:100%;border-collapse:collapse;margin-top:8px;font-size:13px}
    th,td{padding:8px;border-bottom:1px solid rgba(255,255,255,0.03);text-align:left}
    th{font-weight:600;color:#cfe7ff;background:rgba(255,255,255,0.01)}

    .result{padding:10px;border-radius:10px;background:rgba(255,255,255,0.02);margin-top:10px}
    .kpi{display:flex;gap:8px;flex-wrap:wrap}
    .chip{background:rgba(255,255,255,0.03);padding:10px;border-radius:10px;min-width:120px}
    .small{font-size:12px;color:var(--muted)}

    footer{color:var(--muted);font-size:12px;margin-top:12px;text-align:center}

    /* make tables scroll on mobile */
    .table-wrap{overflow:auto}

    .events-list{display:flex;gap:8px;flex-wrap:wrap}
    .event-chip{padding:8px 10px;border-radius:10px;background:rgba(255,255,255,0.02);border:1px solid rgba(255,255,255,0.03);font-size:13px}

    .note{font-size:12px;color:var(--muted);margin-top:6px}
  </style>
</head>
<body>
  <div class="container">
    <header>
      <div class="logo">KK</div>
      <div>
        <h1>Kalkulator Keuntungan & ROAS</h1>
        <p class="lead">Hitung harga jual, fee platform, potongan event, voucher, estimasi affiliate, dan ROAS iklan — tampilan mobile friendly.</p>
      </div>
    </header>

    <div class="grid">
      <!-- Left: Form -->
      <div class="card">
        <div class="section">
          <div class="section-title">Kalkulator Harga Jual & Keuntungan</div>
          <div class="small">Masukkan komponen biaya di bawah. Semua nilai dalam Rupiah kecuali disebutkan %.</div>

          <div style="margin-top:10px">
            <label>Modal produk (HPP)</label>
            <input id="hpp" type="number" value="20000" min="0">
          </div>

          <div style="margin-top:10px">
            <label>Biaya packing & tambahan (nominal)</label>
            <input id="packing" type="number" value="2000" min="0">
          </div>

          <div style="margin-top:10px">
            <label>Platform</label>
            <select id="platform">
              <option value="shopee">Shopee</option>
              <option value="tokopedia">Tokopedia</option>
              <option value="tiktok">TikTok Shop</option>
              <option value="lazada">Lazada</option>
            </select>
          </div>

          <div style="margin-top:10px">
            <label>Kategori produk</label>
            <select id="category">
              <option value="pakaian">Pakaian</option>
              <option value="aksesori">Aksesoris</option>
              <option value="makanan">Makanan</option>
              <option value="elektronik">Elektronik</option>
              <option value="kecantikan">Kecantikan</option>
              <option value="rumah">Perlengkapan Rumah</option>
            </select>
          </div>

          <div style="margin-top:10px" class="row">
            <div>
              <label>Estimasi biaya affiliate (%)</label>
              <input id="affiliate" type="number" value="5" min="0">
            </div>
            <div>
              <label>Fee event tambahan (%)</label>
              <input id="eventFee" type="number" value="0" min="0">
            </div>
          </div>

          <div style="margin-top:10px" class="row">
            <div>
              <label>Biaya voucher (pilih tipe)</label>
              <select id="voucherType">
                <option value="nominal">Nominal (Rp)</option>
                <option value="percent">Persen (%)</option>
              </select>
            </div>
            <div>
              <label>Nilai voucher</label>
              <input id="voucherValue" type="number" value="0" min="0">
            </div>
          </div>

          <div style="margin-top:10px">
            <label>Keuntungan yang diinginkan (%)</label>
            <input id="profitTarget" type="number" value="20" min="0">
          </div>

          <div style="margin-top:12px;display:flex;gap:8px;align-items:center">
            <button class="btn" id="calcBtn">Hitung Harga Jual</button>
            <div class="muted">Hasil dan rincian muncul di sebelah kanan.</div>
          </div>

          <div class="note">Catatan: Tabel fee platform dan event tersedia di panel kanan (tidak bisa diedit) untuk referensi.</div>
        </div>

        <hr style="margin:14px 0;border:none;border-top:1px solid rgba(255,255,255,0.03)">

        <div>
          <div class="section-title">Kalkulator ROAS Iklan</div>
          <div class="small">Hitung ROAS / pengaruh iklan terhadap margin</div>

          <div style="margin-top:10px">
            <label>HPP produk</label>
            <input id="hpp_roas" type="number" value="20000" min="0">
          </div>
          <div style="margin-top:10px" class="row">
            <div>
              <label>Modal iklan per hari (Rp)</label>
              <input id="adSpend" type="number" value="50000" min="0">
            </div>
            <div>
              <label>Estimasi keuntungan (Rp) per unit</label>
              <input id="estProfit" type="number" value="5000" min="0">
            </div>
          </div>

          <div style="margin-top:10px" class="row">
            <div>
              <label>Potongan admin platform (%)</label>
              <input id="adminCut" type="number" value="2" min="0">
            </div>
            <div>
              <label>Biaya potongan affiliate (%)</label>
              <input id="aff_roas" type="number" value="5" min="0">
            </div>
          </div>

          <div style="margin-top:12px;display:flex;gap:8px;align-items:center">
            <button class="btn" id="calcRoasBtn">Hitung ROAS</button>
            <div class="muted">Hasil ROAS dan profit per hari muncul di sebelah kanan.</div>
          </div>
        </div>
      </div>

      <!-- Right: Results & Tables -->
      <div>
        <div class="card">
          <div class="section-title">Hasil Perhitungan</div>

          <div id="resultArea">
            <div class="result">
              <div class="kpi">
                <div class="chip"><div class="small">Harga jual disarankan</div><div id="suggestedPrice">-</div></div>
                <div class="chip"><div class="small">Margin Bersih</div><div id="netMargin">-</div></div>
                <div class="chip"><div class="small">Total Biaya</div><div id="totalFees">-</div></div>
              </div>

              <div style="margin-top:10px" id="breakdown">Rincian biaya akan tampil di sini.</div>
            </div>

            <div style="margin-top:12px" class="result">
              <div class="section-title">Hasil ROAS</div>
              <div class="kpi">
                <div class="chip"><div class="small">ROAS</div><div id="roas">-</div></div>
                <div class="chip"><div class="small">Profit/hari (estimasi)</div><div id="profitDay">-</div></div>
                <div class="chip"><div class="small">Revenue/hari</div><div id="revenueDay">-</div></div>
              </div>
              <div style="margin-top:10px" id="roasBreakdown">Rincian ROAS akan tampil di sini.</div>
            </div>
          </div>
        </div>

        <div style="height:12px"></div>

        <div class="card">
          <div class="section-title">Tabel Fee Platform per Kategori (Permanen)</div>
          <div class="table-wrap">
            <table aria-label="tabel-fee-platform">
              <thead>
                <tr><th>Platform</th><th>Kategori</th><th>Fee (%)</th><th>Catatan</th></tr>
              </thead>
              <tbody id="feeTableBody">
                <!-- populated by JS -->
              </tbody>
            </table>
          </div>
          <div class="note">Tabel ini bersifat tetap — sebagai referensi untuk perhitungan otomatis.</div>
        </div>

        <div style="height:12px"></div>

        <div class="card">
          <div class="section-title">Daftar Event & Fee Tambahan (Permanen)</div>
          <div class="events-list" id="eventList">
            <!-- populated by JS -->
          </div>
          <div class="note">Centang event pada form untuk menambahkan potongan biaya (jika perlu). Untuk kemudahan, masukkan % di kolom Fee event jika tidak ada dalam daftar.</div>
        </div>

      </div>
    </div>

    <footer>
      Dibuat otomatis — ubah nilai input untuk menyesuaikan. Jika mau versi ZIP / Github-ready, beri tahu saya.
    </footer>
  </div>

  <script>
    // Data fee (permanent)
    const platformFees = {
      shopee:{pakaian:6, aksesori:6, makanan:8, elektronik:5, kecantikan:6, rumah:6},
      tokopedia:{pakaian:5, aksesori:5, makanan:7, elektronik:4, kecantikan:5, rumah:5},
      tiktok:{pakaian:8, aksesori:8, makanan:10, elektronik:6, kecantikan:8, rumah:7},
      lazada:{pakaian:6, aksesori:6, makanan:9, elektronik:5, kecantikan:6, rumah:6}
    };

    const events = [
      {id:'gratisOngkir', name:'Gratis Ongkir', type:'nominal', value:12000, note:'Subsidi ongkir (contoh) — bisa nominal per unit'},
      {id:'tanggalKembar', name:'Tanggal Kembar (11.11/12.12)', type:'percent', value:5, note:'Komisi/event ekstra sebagai persentase dari penjualan'},
      {id:'eventGajian', name:'Event Gajian', type:'percent', value:3, note:'Diskon/fee tambahan saat event gajian'},
      {id:'campaign', name:'Campaign Platform', type:'percent', value:2.5, note:'Biaya campaign/platform takeover'}
    ];

    // render tables
    const feeTableBody = document.getElementById('feeTableBody');
    const mapCat = {pakaian:'Pakaian', aksesori:'Aksesoris', makanan:'Makanan', elektronik:'Elektronik', kecantikan:'Kecantikan', rumah:'Perlengkapan Rumah'};
    for(const p of Object.keys(platformFees)){
      for(const cat of Object.keys(platformFees[p])){
        const tr = document.createElement('tr');
        tr.innerHTML = `<td>${capitalize(p)}</td><td>${mapCat[cat]}</td><td>${platformFees[p][cat]}%</td><td>-</td>`;
        feeTableBody.appendChild(tr);
      }
    }

    const eventList = document.getElementById('eventList');
    for(const ev of events){
      const btn = document.createElement('div');
      btn.className = 'event-chip';
      btn.innerHTML = `<label style="display:flex;gap:8px;align-items:center"><input type=checkbox data-id="${ev.id}" data-type="${ev.type}" data-value="${ev.value}"> <div style="font-weight:600">${ev.name}</div> <div style="margin-left:8px;color:var(--muted)">${ev.type==='percent'?ev.value+'%':'Rp '+format(ev.value)}</div></label>`;
      eventList.appendChild(btn);
    }

    function capitalize(s){return s.charAt(0).toUpperCase()+s.slice(1)}
    function format(n){return new Intl.NumberFormat('id-ID').format(Math.round(n))}

    // Calculation: Price suggestion
    document.getElementById('calcBtn').addEventListener('click', ()=>{
      const hpp = Number(document.getElementById('hpp').value)||0;
      const packing = Number(document.getElementById('packing').value)||0;
      const platform = document.getElementById('platform').value;
      const category = document.getElementById('category').value;
      const affiliate = Number(document.getElementById('affiliate').value)||0;
      const eventFeeInput = Number(document.getElementById('eventFee').value)||0; // user extra
      const voucherType = document.getElementById('voucherType').value;
      const voucherValue = Number(document.getElementById('voucherValue').value)||0;
      const profitTarget = Number(document.getElementById('profitTarget').value)||0;

      const platformFeePercent = platformFees[platform][category] || 0;

      // calculate base costs
      const baseCost = hpp + packing; // rp
      // platform fee amount = percent of selling price (we need to solve iteratively because fees are % of selling price)

      // We'll compute suggested price by solving for price P where:
      // Net after fees = P - platformFee%*P - affiliate%*P - event%*P - voucher - baseCost
      // We want net after fees = profitTarget% * P  ??? User likely wants desired profit as % of selling price or of cost? We'll interpret as % of selling price.
      // Alternative: profitTarget% of cost (markup). To keep intuitive, we'll compute price such that profit equals profitTarget% of price (i.e., margin%).

      // Equation: P - (pf+af+ef)*P - voucherAmount - baseCost = profitTarget% * P
      // => P*(1 - (pf+af+ef) - profitTarget/100) - voucherAmount - baseCost = 0
      // => P = (baseCost + voucherAmount) / (1 - totalPercent - profitTarget/100)

      const pf = platformFeePercent/100;
      const af = affiliate/100;
      const ef_user = eventFeeInput/100;

      // event checkboxes from list
      let extraEventPercent = 0; let extraEventNominal = 0;
      document.querySelectorAll('#eventList input[type=checkbox]').forEach(ch => {
        if(ch.checked){
          const t = ch.dataset.type; const v = Number(ch.dataset.value);
          if(t==='percent') extraEventPercent += v/100; else extraEventNominal += v;
        }
      });

      // combine event percent
      const ef_total = ef_user + extraEventPercent;

      // voucher amount depends on type
      let voucherAmount = 0;
      if(voucherType==='nominal'){ voucherAmount = voucherValue; }
      else { voucherAmount = voucherValue/100 * (/* approximate of P unknown, use base cost + margin guess -> iterate */0); }
      // Because voucher percent depends on P, we must solve iteratively. We'll do a simple numeric iteration.

      function solvePrice(){
        let P = Math.max( (baseCost+1) * 1.2, 10000 );
        for(let i=0;i<60;i++){
          // voucher percent type
          if(voucherType==='percent') voucherAmount = (voucherValue/100) * P;
          const totalPercent = pf + af + ef_total;
          const denom = 1 - totalPercent - (profitTarget/100);
          if(denom<=0){ P = Infinity; break; }
          const newP = (baseCost + packing*0 + voucherAmount) / denom; // packing already in baseCost
          // small relaxation
          P = P*0.3 + newP*0.7;
        }
        return P;
      }

      const suggested = solvePrice();
      const voucherFinal = voucherType==='percent' ? (voucherValue/100*suggested) : voucherValue;
      const totalPercent = pf + af + ef_total;
      const platformFeeAmt = suggested * pf;
      const affiliateAmt = suggested * af;
      const eventAmt = suggested * ef_total;
      const totalFeesAmt = platformFeeAmt + affiliateAmt + eventAmt + voucherFinal;
      const net = suggested - totalFeesAmt - baseCost;
      const netMarginPerc = suggested>0 ? (net/suggested*100) : 0;

      // Render
      document.getElementById('suggestedPrice').innerText = suggested===Infinity? 'Tidak dapat dihitung (persentase terlalu besar)' : 'Rp '+format(suggested.toFixed(0));
      document.getElementById('totalFees').innerText = suggested===Infinity? '-' : 'Rp '+format(totalFeesAmt.toFixed(0));
      document.getElementById('netMargin').innerText = suggested===Infinity? '-' : netMarginPerc.toFixed(2)+'%';

      const breakdown = `
        <div class="small">Rincian:</div>
        <table>
          <tr><td>Modal (HPP)</td><td>Rp ${format(hpp)}</td></tr>
          <tr><td>Biaya packing</td><td>Rp ${format(packing)}</td></tr>
          <tr><td>Platform fee (${platform} - ${category})</td><td>${platformFeePercent}% &rarr; Rp ${format(platformFeeAmt.toFixed(0))}</td></tr>
          <tr><td>Estimasi affiliate</td><td>${affiliate}% &rarr; Rp ${format(affiliateAmt.toFixed(0))}</td></tr>
          <tr><td>Estimasi event fee</td><td>${(ef_total*100).toFixed(2)}% &rarr; Rp ${format(eventAmt.toFixed(0))}</td></tr>
          <tr><td>Biaya voucher</td><td>${voucherType==='percent'?voucherValue+'%':'Rp '+format(voucherValue)} &rarr; Rp ${format(voucherFinal.toFixed(0))}</td></tr>
          <tr><td><strong>Total biaya & potongan</strong></td><td><strong>Rp ${format(totalFeesAmt.toFixed(0))}</strong></td></tr>
          <tr><td><strong>Keuntungan bersih</strong></td><td><strong>Rp ${format(net.toFixed(0))} (${netMarginPerc.toFixed(2)}%)</strong></td></tr>
        </table>
      `;

      document.getElementById('breakdown').innerHTML = breakdown;
    });

    // ROAS calc
    document.getElementById('calcRoasBtn').addEventListener('click', ()=>{
      const hpp = Number(document.getElementById('hpp_roas').value)||0;
      const adSpend = Number(document.getElementById('adSpend').value)||0;
      const estProfit = Number(document.getElementById('estProfit').value)||0; // profit per unit estimated
      const adminCut = Number(document.getElementById('adminCut').value)||0;
      const aff = Number(document.getElementById('aff_roas').value)||0;

      // Assume sales per day = adSpend / (cost-per-order). We don't have cpo; instead calculate ROAS given revenue unknown.
      // We'll compute ROAS if user sells 1 unit: revenue = hpp + estProfit + fees? Better: ask user for price? But user didn't provide price — we will estimate using HPP+estProfit as selling price.
      const assumedPrice = hpp + estProfit;
      const revenue = assumedPrice; // per sale
      // ROAS = revenue / adSpend. But adSpend is per day; need sales per day to be consistent. We'll assume adSpend generates sales = Math.max(1, Math.round(adSpend / Math.max(10000, adSpend/2))) -> that is messy.
      // Simpler: compute ROAS per 1 sale and also compute required sales to break even adSpend.

      const platformCut = adminCut/100 * assumedPrice;
      const affCut = aff/100 * assumedPrice;
      const profitPerSaleNet = assumedPrice - platformCut - affCut - hpp;

      // Sales needed to cover adSpend
      const salesToCoverAds = profitPerSaleNet>0 ? Math.ceil(adSpend / profitPerSaleNet) : Infinity;
      const roas = adSpend>0 ? (assumedPrice / adSpend) : 0; // revenue per Rp ad spend

      document.getElementById('roas').innerText = roas===Infinity?'-':roas.toFixed(3)+' (Rp revenue per Rp iklan)';
      document.getElementById('revenueDay').innerText = 'Jika 1 penjualan: Rp '+format(assumedPrice);
      document.getElementById('profitDay').innerText = profitPerSaleNet>0 ? 'Per unit: Rp '+format(profitPerSaleNet) + ' • Butuh '+(salesToCoverAds===Infinity?'-':salesToCoverAds)+' penjualan untuk tutup biaya iklan' : 'Tidak ada margin positif setelah potongan';

      const rd = `
        <div class="small">Asumsi: Harga jual = HPP + Estimasi keuntungan (Rp). Jika Anda tahu harga jual sebenarnya, gunakan nilai Estimasi keuntungan yang sesuai.</div>
        <table>
          <tr><td>Harga jual (asumsi)</td><td>Rp ${format(assumedPrice)}</td></tr>
          <tr><td>Potongan admin platform (${adminCut}%)</td><td>Rp ${format(platformCut.toFixed(0))}</td></tr>
          <tr><td>Potongan affiliate (${aff}%)</td><td>Rp ${format(affCut.toFixed(0))}</td></tr>
          <tr><td>Profit bersih per penjualan</td><td>Rp ${profitPerSaleNet>0?format(profitPerSaleNet.toFixed(0)):'-'} </td></tr>
          <tr><td>Biaya iklan/hari</td><td>Rp ${format(adSpend)}</td></tr>
          <tr><td>Sales perlu untuk tutup iklan</td><td>${salesToCoverAds===Infinity?'-':salesToCoverAds+' penjualan'}</td></tr>
        </table>
      `;
      document.getElementById('roasBreakdown').innerHTML = rd;
    });
  </script>
</body>
</html>
