<!doctype html>
<html lang="id">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Kalkulator Keuntungan & ROAS - Toko Online (Bahasa Indonesia)</title>
  <style>
    :root{--bg:#f6f8fb;--card:#fff;--accent:#2563eb;--muted:#6b7280}
    body{font-family:Inter, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial; background:var(--bg); margin:0; padding:24px}
    .wrap{max-width:1100px;margin:0 auto}
    header{display:flex;gap:16px;align-items:center;margin-bottom:18px}
    h1{margin:0;font-size:20px}
    p.lead{margin:0;color:var(--muted);font-size:13px}
    .grid{display:grid;grid-template-columns:repeat(2,1fr);gap:18px}
    .card{background:var(--card);padding:16px;border-radius:12px;box-shadow:0 6px 18px rgba(16,24,40,0.06)}
    label{display:block;font-size:13px;color:#111827;margin-bottom:6px}
    input[type=text], input[type=number], select, textarea{width:100%;padding:8px;border:1px solid #e6eef8;border-radius:8px;font-size:14px}
    .row{display:flex;gap:8px}
    .muted{color:var(--muted);font-size:13px}
    .table{width:100%;border-collapse:collapse;margin-top:12px}
    .table th, .table td{padding:8px;border-bottom:1px solid #f1f5f9;text-align:left;font-size:13px}
    button{background:var(--accent);color:#fff;border:0;padding:8px 12px;border-radius:10px;cursor:pointer}
    .small{font-size:13px;padding:6px 8px}
    .flex-between{display:flex;justify-content:space-between;align-items:center}
    .full{grid-column:1/-1}
    .result{background:#f3f9ff;padding:12px;border-radius:10px;margin-top:10px}
    .badge{background:#eef2ff;color:#1e3a8a;padding:4px 8px;border-radius:999px;font-weight:600;font-size:12px}
    footer{margin-top:18px;color:var(--muted);font-size:13px}
    @media(max-width:900px){.grid{grid-template-columns:1fr}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div>
        <h1>Kalkulator Harga Jual & Keuntungan</h1>
        <p class="lead">Hitung harga jual, estimasi fee platform, fee event, dan ROAS iklan. Data fee default bisa diubah sesuai kebutuhan.</p>
      </div>
      <div style="margin-left:auto"><span class="badge">Versi: 1.0 (Editable)</span></div>
    </header>

    <div class="grid">
      <!-- Kalkulator utama -->
      <div class="card">
        <h3>1. Hitung Harga Jual & Keuntungan</h3>
        <label>HPP Produk (Rp)</label>
        <input id="hpp" type="number" value="50000" />

        <label>Biaya Packing & Tambahan (Rp)</label>
        <input id="packing" type="number" value="3000" />

        <label>Platform</label>
        <select id="platform"></select>

        <label>Kategori Produk</label>
        <select id="kategori"></select>

        <label>Fee Event (pilih yang berlaku)</label>
        <select id="event"></select>

        <label>Keuntungan yang diinginkan (%)</label>
        <input id="profitPct" type="number" value="20" />

        <div style="margin-top:10px" class="row">
          <button id="calcPrice">Hitung Harga Jual</button>
          <button id="resetFees" class="small" style="background:#ef4444">Reset ke default</button>
        </div>

        <div id="priceResult" class="result" style="display:none"></div>
      </div>

      <!-- Tabel Fee per Platform & Kategori -->
      <div class="card">
        <h3>2. Tabel Fee Platform x Kategori (Editable)</h3>
        <p class="muted">Ubah persentase fee platform per kategori jika diperlukan. Simpan perubahan akan mempengaruhi perhitungan.</p>
        <div id="feeTables"></div>

        <div style="margin-top:10px">
          <button id="applyFees">Simpan perubahan fee</button>
        </div>
      </div>

      <!-- Event Fee -->
      <div class="card">
        <h3>3. Daftar Fee Event (contoh)</h3>
        <p class="muted">Jenis event: bisa berupa potongan gratis ongkir (rp atau %), biaya campaign (flat atau %), potongan harga event, dll. Nilai default bisa diedit.</p>
        <div id="eventTable"></div>
        <div style="margin-top:8px" class="row">
          <button id="addEvent" class="small">Tambah event custom</button>
        </div>
      </div>

      <!-- ROAS Calculator -->
      <div class="card">
        <h3>4. Kalkulator ROAS Iklan</h3>
        <label>HPP Produk (Rp)</label>
        <input id="hppRoas" type="number" value="50000" />
        <label>Modal Iklan per hari (Rp)</label>
        <input id="adSpend" type="number" value="100000" />
        <label>Target Omset per hari dari iklan (Rp)</label>
        <input id="targetRevenue" type="number" value="500000" />
        <div style="margin-top:8px" class="row">
          <button id="calcRoas">Hitung ROAS</button>
        </div>
        <div id="roasResult" class="result" style="display:none"></div>
      </div>

      <!-- Penjelasan & Tips -->
      <div class="card full">
        <h3>Penjelasan singkat perhitungan</h3>
        <ul>
          <li><strong>Harga jual rekomendasi</strong> dihitung dari: (HPP + packing) + fee platform + fee event + keuntungan yang diinginkan.</li>
          <li><strong>Fee platform</strong> ditentukan sebagai persentase dari harga jual.</li>
          <li><strong>Event</strong> bisa berupa potongan (persentase), subsidi ongkir (nominal), atau biaya campaign (persentase/nominal) — sistem memungkinkan pengaturan.</li>
          <li><strong>ROAS</strong> = Pendapatan dari iklan / Biaya iklan. Jika target omset diketahui, ROAS yang dibutuhkan = targetOmset / modalIklan.</li>
        </ul>
      </div>

    </div>

    <footer>
      <div>Catatan: Semua nilai fee pada file ini adalah contoh. Sesuaikan dengan peraturan platform yang berlaku. Program ini memungkinkan pengeditan tabel fee agar cocok dengan update masing-masing marketplace.</div>
    </footer>
  </div>

  <script>
    // Default data (editable)
    const defaultData = {
      platforms: [
        {id:'shopee', name:'Shopee'},
        {id:'tiktok', name:'TikTok Shop'},
        {id:'tokopedia', name:'Tokopedia'},
        {id:'lazada', name:'Lazada'}
      ],
      categories: ['pakaian','aksesoris','makanan','elektronik','kesehatan','perlengkapan rumah','lainnya'],
      // platformFees[platformId][category] = percentage (as number)
      platformFees: {
        shopee: {pakaian:12, aksesoris:12, makanan:10, elektronik:12, kesehatan:12, 'perlengkapan rumah':12, lainnya:12},
        tiktok: {pakaian:15, aksesoris:15, makanan:12, elektronik:15, kesehatan:15, 'perlengkapan rumah':15, lainnya:15},
        tokopedia: {pakaian:10, aksesoris:10, makanan:8, elektronik:10, kesehatan:10, 'perlengkapan rumah':10, lainnya:10},
        lazada: {pakaian:11, aksesoris:11, makanan:10, elektronik:11, kesehatan:11, 'perlengkapan rumah':11, lainnya:11}
      },
      // eventFees: array of events with type: 'percent'|'nominal' and target: 'seller'|'buyer' etc.
      eventFees: [
        {id:'free_ongkir', name:'Gratis Ongkir Extra (subsidi marketplace)', type:'nominal', value:15000, note:'Subsidi ongkir (nilai potongan per transaksi)'},
        {id:'diskon_event', name:'Diskon Event (potongan harga)', type:'percent', value:10, note:'Persentase potongan harga yang berlaku pada produk'},
        {id:'campaign_fee', name:'Biaya Campaign / Promosi', type:'percent', value:3, note:'Biaya tambahan sebagai persentase dari harga jual jika mengikuti campaign berbayar'},
        {id:'tanggal_kembar', name:'Event Tanggal Kembar', type:'percent', value:5, note:'Biaya atau potongan spesial saat event besar (contoh)'}
      ]
    };

    // State (mutable)
    let state = JSON.parse(JSON.stringify(defaultData));

    // Init selects and tables
    const platformEl = document.getElementById('platform');
    const kategoriEl = document.getElementById('kategori');
    const eventEl = document.getElementById('event');

    function populatePlatformKategori(){
      platformEl.innerHTML = '';
      state.platforms.forEach(p=>{
        const opt = document.createElement('option'); opt.value = p.id; opt.textContent = p.name; platformEl.appendChild(opt);
      });
      kategoriEl.innerHTML = '';
      state.categories.forEach(cat=>{ const o = document.createElement('option'); o.value=cat; o.textContent = cat; kategoriEl.appendChild(o); });

      eventEl.innerHTML = '';
      state.eventFees.forEach(ev=>{ const o = document.createElement('option'); o.value = ev.id; o.textContent = ev.name + (ev.type==='percent'?` (${ev.value}%)`:` (Rp ${numberFormat(ev.value)})`); eventEl.appendChild(o); });
    }

    function renderFeeTables(){
      const container = document.getElementById('feeTables'); container.innerHTML='';
      state.platforms.forEach(p=>{
        const tbl = document.createElement('div'); tbl.className='card'; tbl.style.marginTop='10px';
        tbl.innerHTML = `<h4>${p.name}</h4><table class='table'><thead><tr><th>Kategori</th><th>Fee platform (%)</th></tr></thead><tbody></tbody></table>`;
        const tbody = tbl.querySelector('tbody');
        state.categories.forEach(cat=>{
          const tr = document.createElement('tr');
          tr.innerHTML = `<td>${cat}</td><td><input data-platform='${p.id}' data-cat='${cat}' class='feeInp' type='number' value='${state.platformFees[p.id][cat] ?? 0}' style='width:90px' /></td>`;
          tbody.appendChild(tr);
        });
        container.appendChild(tbl);
      });
    }

    function renderEventTable(){
      const container = document.getElementById('eventTable'); container.innerHTML='';
      const table = document.createElement('table'); table.className='table';
      table.innerHTML = `<thead><tr><th>Event</th><th>Jenis</th><th>Nilai</th><th>Catatan</th><th>Aksi</th></tr></thead><tbody></tbody>`;
      const tbody = table.querySelector('tbody');
      state.eventFees.forEach(ev=>{
        const tr = document.createElement('tr');
        tr.innerHTML = `<td><input data-id='name-${ev.id}' class='smallInp' value='${ev.name}' /></td>
                        <td>${ev.type}</td>
                        <td><input data-id='val-${ev.id}' type='number' value='${ev.value}' style='width:120px' /></td>
                        <td><input data-id='note-${ev.id}' value='${ev.note}' /></td>
                        <td><button data-del='${ev.id}' class='small'>Hapus</button></td>`;
        tbody.appendChild(tr);
      });
      container.appendChild(table);

      // attach handlers for delete and change
      container.querySelectorAll('[data-del]').forEach(btn=>{
        btn.addEventListener('click',()=>{
          const id = btn.getAttribute('data-del');
          state.eventFees = state.eventFees.filter(e=>e.id!==id);
          populatePlatformKategori(); renderEventTable();
        });
      });

      // update values on change
      container.querySelectorAll('[data-id]').forEach(inp=>{
        inp.addEventListener('change',()=>{
          const key = inp.getAttribute('data-id');
          const parts = key.split('-');
          const field = parts[0];
          const id = parts[1];
          const ev = state.eventFees.find(x=>x.id===id);
          if(!ev) return;
          if(field==='val') ev.value = Number(inp.value);
          if(field==='note') ev.note = inp.value;
          if(field==='name') ev.name = inp.value;
          populatePlatformKategori();
        });
      });
    }

    // Utilities
    function numberFormat(v){ return Number(v).toLocaleString('id-ID'); }

    // Calculation logic
    function calculatePrice(){
      const hpp = Number(document.getElementById('hpp').value) || 0;
      const packing = Number(document.getElementById('packing').value) || 0;
      const platform = platformEl.value;
      const kategori = kategoriEl.value;
      const eventId = eventEl.value;
      const profitPct = Number(document.getElementById('profitPct').value) || 0;

      // platform fee percentage
      const platformFeePct = (state.platformFees[platform] && state.platformFees[platform][kategori]) ? Number(state.platformFees[platform][kategori]) : 0;

      // event fee effects (may be percent or nominal)
      const ev = state.eventFees.find(e=>e.id===eventId) || null;
      const eventPercent = ev && ev.type==='percent' ? Number(ev.value) : 0;
      const eventNominal = ev && ev.type==='nominal' ? Number(ev.value) : 0;

      // Let price be X. Fees that are % are calculated on price. Nominal are fixed.
      // We solve: X = HPP + packing + (platformFeePct% of X) + (eventPercent% of X) + eventNominal + desiredProfit
      // desiredProfit = profitPct% of X
      // So X = HPP + packing + eventNominal + (platformFeePct/100)*X + (eventPercent/100)*X + (profitPct/100)*X
      // Bring percent terms to left: X * (1 - (platformFeePct+eventPercent+profitPct)/100) = HPP + packing + eventNominal
      const totalPercent = platformFeePct + eventPercent + profitPct;
      const numerator = hpp + packing + eventNominal;
      const denom = 1 - (totalPercent/100);
      let recommendedPrice = denom > 0 ? numerator / denom : NaN;

      // Also compute breakdown
      const platformFeeRp = recommendedPrice * (platformFeePct/100);
      const eventFeeRp = recommendedPrice * (eventPercent/100) + eventNominal;
      const profitRp = recommendedPrice * (profitPct/100);
      const totalCost = hpp + packing + platformFeeRp + eventFeeRp;

      const out = {
        recommendedPrice, platformFeePct, platformFeeRp, eventFeeRp, profitRp, totalCost, hpp, packing, totalPercent
      };
      return out;
    }

    // ROAS
    function calculateRoas(){
      const hpp = Number(document.getElementById('hppRoas').value) || 0;
      const adSpend = Number(document.getElementById('adSpend').value) || 0;
      const targetRevenue = Number(document.getElementById('targetRevenue').value) || 0;

      const requiredROAS = adSpend > 0 ? (targetRevenue / adSpend) : Infinity;
      // Estimasi profit jika terjual dengan margin sederhana: revenue - (hpp * qty) - adSpend.
      // Kita tidak punya qty; as estimate use single-unit margin: margin% = (revenue - hpp)/revenue
      const estMargin = targetRevenue > 0 ? ((targetRevenue - hpp) / targetRevenue) : 0;
      const estProfit = targetRevenue - hpp - adSpend;
      return {requiredROAS, estMargin, estProfit};
    }

    // Event handlers
    document.getElementById('calcPrice').addEventListener('click', ()=>{
      const res = calculatePrice();
      const el = document.getElementById('priceResult'); el.style.display='block';
      if(!isFinite(res.recommendedPrice) || isNaN(res.recommendedPrice)){
        el.innerHTML = `<strong>Error:</strong> Kombinasi persen terlalu besar (jumlah percent = ${res.totalPercent}%). Coba kurangi persen atau ubah nilai.`;
        return;
      }
      el.innerHTML = `
        <div class='flex-between'><strong>Harga Jual Rekomendasi:</strong><div><strong>Rp ${numberFormat(Math.ceil(res.recommendedPrice))}</strong></div></div>
        <div style='margin-top:8px'>
          <div class='muted'>Rincian:</div>
          <ul>
            <li>HPP Produk: Rp ${numberFormat(res.hpp)}</li>
            <li>Biaya Packing & Tambahan: Rp ${numberFormat(res.packing)}</li>
            <li>Fee Platform (${res.platformFeePct}%): Rp ${numberFormat(Math.round(res.platformFeeRp))}</li>
            <li>Fee Event (total): Rp ${numberFormat(Math.round(res.eventFeeRp))}</li>
            <li>Keuntungan (${document.getElementById('profitPct').value}%): Rp ${numberFormat(Math.round(res.profitRp))}</li>
          </ul>
        </div>
        <div style='margin-top:6px' class='muted'>Total biaya (HPP+packing+fees): Rp ${numberFormat(Math.round(res.totalCost))}</div>
      `;
    });

    document.getElementById('applyFees').addEventListener('click', ()=>{
      // read fee inputs
      document.querySelectorAll('.feeInp').forEach(inp=>{
        const pid = inp.getAttribute('data-platform');
        const cat = inp.getAttribute('data-cat');
        const val = Number(inp.value) || 0;
        if(!state.platformFees[pid]) state.platformFees[pid] = {};
        state.platformFees[pid][cat] = val;
      });
      alert('Perubahan fee tersimpan (di memori lokal session).');
    });

    document.getElementById('resetFees').addEventListener('click', ()=>{
      if(confirm('Kembalikan semua fee ke nilai default?')){ state = JSON.parse(JSON.stringify(defaultData)); populatePlatformKategori(); renderFeeTables(); renderEventTable(); }
    });

    document.getElementById('addEvent').addEventListener('click', ()=>{
      const id = 'ev_' + Math.random().toString(36).slice(2,8);
      state.eventFees.push({id, name:'Event baru', type:'percent', value:5, note:'Catatan'});
      populatePlatformKategori(); renderEventTable();
    });

    document.getElementById('calcRoas').addEventListener('click', ()=>{
      const out = calculateRoas();
      const el = document.getElementById('roasResult'); el.style.display='block';
      el.innerHTML = `
        <div class='flex-between'><strong>ROAS dibutuhkan:</strong><div><strong>${Number(out.requiredROAS).toFixed(2)}x</strong></div></div>
        <div style='margin-top:8px'>Estimasi profit jika tercapai target: Rp ${numberFormat(Math.round(out.estProfit))} <span class='muted' style='display:block'>Estimasi margin dari omset: ${(out.estMargin*100).toFixed(2)}%</span></div>
      `;
    });

    // Initialize UI
    populatePlatformKategori(); renderFeeTables(); renderEventTable();

    // allow inline fee input changes to update state on change
    document.addEventListener('change', (e)=>{
      if(e.target && e.target.classList.contains('feeInp')){
        // update state immediately
        const pid = e.target.getAttribute('data-platform');
        const cat = e.target.getAttribute('data-cat');
        state.platformFees[pid][cat] = Number(e.target.value) || 0;
      }
    });

  </script>
</body>
</html>
