<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kalkulator Keuntungan & Harga Jual Toko Online</title>
<style>
    body { font-family: Arial, sans-serif; background:#f5f5f5; padding:20px; }
    h2 { background:#4CAF50; color:white; padding:10px; border-radius:6px; }
    .box { background:white; padding:15px; margin-top:15px; border-radius:8px; box-shadow:0 2px 5px rgba(0,0,0,0.1); }
    label { font-weight:bold; display:block; margin-top:10px; }
    select, input { width:100%; padding:8px; margin-top:5px; border:1px solid #ccc; border-radius:5px; }
    button { margin-top:15px; padding:10px; width:100%; font-size:16px; background:#4CAF50; color:white; border:none; border-radius:5px; cursor:pointer; }
    button:hover { background:#45a049; }
    .result { background:#e8ffe8; padding:10px; margin-top:10px; border-radius:6px; }
    table { width:100%; border-collapse: collapse; margin-top:10px; }
    table, th, td { border:1px solid #ccc; }
    th, td { padding:8px; text-align:left; }
</style>
</head>
<body>

<h2>Kalkulator Harga Jual & Keuntungan</h2>
<div class="box">
    <label>HPP Produk (Modal)</label>
    <input type="number" id="hpp" placeholder="Contoh: 30000">

    <label>Biaya Packing & Tambahan</label>
    <input type="number" id="packing" placeholder="Contoh: 5000">

    <label>Pilih Platform</label>
    <select id="platform">
        <option value="shopee">Shopee</option>
        <option value="tiktok">TikTok Shop</option>
        <option value="tokped">Tokopedia</option>
        <option value="lazada">Lazada</option>
    </select>

    <label>Pilih Kategori Produk</label>
    <select id="kategori"></select>

    <label>Fee Event (%)</label>
    <input type="number" id="feeEvent" placeholder="Contoh: 5">

    <label>Target Keuntungan (Rp)</label>
    <input type="number" id="profitTarget" placeholder="Contoh: 10000">

    <button onclick="hitungHargaJual()">Hitung Harga Jual</button>

    <div id="hasil" class="result"></div>
</div>

<h2>Daftar Fee Platform per Kategori</h2>
<div class="box">
    <table id="feeTable"></table>
</div>

<h2>Kalkulator ROAS</h2>
<div class="box">
    <label>Total Belanja Iklan (Rp)</label>
    <input type="number" id="adsSpend" placeholder="Contoh: 1000000">

    <label>Total Omset dari Iklan (Rp)</label>
    <input type="number" id="adsRevenue" placeholder="Contoh: 3000000">

    <button onclick="hitungROAS()">Hitung ROAS</button>

    <div id="hasilROAS" class="result"></div>
</div>

<script>
// Data fee per platform & kategori
const feeData = {
    shopee: {
        "Pakaian": 5,
        "Makanan": 4,
        "Elektronik": 4,
        "Kecantikan": 5,
        "Aksesoris": 6
    },
    tiktok: {
        "Pakaian": 4,
        "Makanan": 3,
        "Elektronik": 4,
        "Kecantikan": 5,
        "Aksesoris": 6
    },
    tokped: {
        "Pakaian": 5,
        "Makanan": 4,
        "Elektronik": 3,
        "Kecantikan": 5,
        "Aksesoris": 6
    },
    lazada: {
        "Pakaian": 6,
        "Makanan": 5,
        "Elektronik": 4,
        "Kecantikan": 6,
        "Aksesoris": 6
    }
};

// Isi dropdown kategori
function updateKategori() {
    const platform = document.getElementById("platform").value;
    const kategoriSelect = document.getElementById("kategori");
    kategoriSelect.innerHTML = "";
    Object.keys(feeData[platform]).forEach(k => {
        const option = document.createElement("option");
        option.value = k;
        option.textContent = k;
        kategoriSelect.appendChild(option);
    });
}
updateKategori();
document.getElementById("platform").addEventListener("change", updateKategori);

// Tampilkan tabel fee
function renderFeeTable() {
    const table = document.getElementById("feeTable");
    let html = "<tr><th>Platform</th><th>Kategori</th><th>Fee (%)</th></tr>";
    for (let p in feeData) {
        for (let k in feeData[p]) {
            html += `<tr><td>${p}</td><td>${k}</td><td>${feeData[p][k]}%</td></tr>`;
        }
    }
    table.innerHTML = html;
}
renderFeeTable();

// Hitung harga jual
function hitungHargaJual() {
    const hpp = Number(document.getElementById("hpp").value);
    const packing = Number(document.getElementById("packing").value);
    const platform = document.getElementById("platform").value;
    const kategori = document.getElementById("kategori").value;
    const feePlatform = feeData[platform][kategori] / 100;
    const feeEvent = Number(document.getElementById("feeEvent").value) / 100;
    const profitTarget = Number(document.getElementById("profitTarget").value);

    const totalModal = hpp + packing;
    const totalFee = feePlatform + feeEvent;

    const hargaJual = (totalModal + profitTarget) / (1 - totalFee);

    document.getElementById("hasil").innerHTML = `
        <b>Total Modal:</b> Rp ${totalModal.toLocaleString()}<br>
        <b>Total Fee (%):</b> ${(totalFee * 100).toFixed(2)}%<br>
        <b>Harga Jual Minimal:</b> <span style='color:green'>Rp ${Math.ceil(hargaJual).toLocaleString()}</span>
    `;
}

// Hitung ROAS
function hitungROAS() {
    const spend = Number(document.getElementById("adsSpend").value);
    const revenue = Number(document.getElementById("adsRevenue").value);
    if (spend <= 0) return;
    const roas = revenue / spend;
    document.getElementById("hasilROAS").innerHTML = `<b>ROAS:</b> ${roas.toFixed(2)}x`;
}
</script>

</body>
</html>
