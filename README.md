<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Kalkulator Keuntungan & ROAS</title>
<style>
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        background: #f2f4f7;
        padding: 20px;
        color: #333;
    }
    .container {
        max-width: 600px;
        margin: auto;
        background: #fff;
        padding: 20px;
        border-radius: 15px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }
    h2 {
        text-align: center;
        margin-bottom: 20px;
        color: #2c3e50;
    }
    label {
        font-weight: bold;
        margin-top: 10px;
        display: block;
    }
    input, select {
        width: 100%;
        padding: 10px;
        margin-top: 5px;
        border: 1px solid #ccc;
        border-radius: 10px;
    }
    button {
        width: 100%;
        padding: 12px;
        border: 0;
        background: #3498db;
        color: #fff;
        border-radius: 10px;
        margin-top: 15px;
        font-size: 16px;
        cursor: pointer;
    }
    button:hover {
        background: #217dbb;
    }
    .result {
        margin-top: 15px;
        background: #ecf0f1;
        padding: 12px;
        border-radius: 10px;
        font-weight: bold;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
        background: white;
    }
    th, td {
        padding: 10px;
        border: 1px solid #ddd;
        text-align: left;
    }
    th {
        background: #3498db;
        color: white;
    }
</style>
</head>
<body>

<div class="container">
    <h2>Kalkulator Harga Jual & Keuntungan</h2>

    <label>HPP Produk (Rp)</label>
    <input type="number" id="hpp" />

    <label>Biaya Packing & Tambahan (Rp)</label>
    <input type="number" id="packing" />

    <label>Pilih Platform</label>
    <select id="platform">
        <option value="shopee">Shopee</option>
        <option value="tiktok">TikTok Shop</option>
        <option value="tokped">Tokopedia</option>
        <option value="lazada">Lazada</option>
    </select>

    <label>Pilih Kategori Barang</label>
    <select id="kategori">
        <option value="pakaian">Pakaian</option>
        <option value="makanan">Makanan</option>
        <option value="aksesoris">Aksesoris</option>
        <option value="elektronik">Elektronik</option>
    </select>

    <label>Fee Event Tambahan (%)</label>
    <input type="number" id="eventFee" placeholder="Contoh: 5" />

    <label>Keuntungan yang Diinginkan (Rp)</label>
    <input type="number" id="profit" />

    <button onclick="hitungHargaJual()">Hitung Harga Jual</button>

    <div class="result" id="hasilHarga"></div>
</div>

<div class="container" style="margin-top: 30px;">
    <h2>Kalkulator ROAS Iklan</h2>

    <label>HPP Produk (Rp)</label>
    <input type="number" id="roas_hpp" />

    <label>Modal Iklan Perhari (Rp)</label>
    <input type="number" id="modal_iklan" />

    <label>Target Keuntungan Perhari (Rp)</label>
    <input type="number" id="target_profit" />

    <button onclick="hitungROAS()">Hitung ROAS</button>

    <div class="result" id="hasilROAS"></div>
</div>

<div class="container">
    <h2>Tabel Fee Platform per Kategori</h2>
    <table>
        <tr><th>Platform</th><th>Pakaian</th><th>Makanan</th><th>Aksesoris</th><th>Elektronik</th></tr>
        <tr><td>Shopee</td><td>5%</td><td>4%</td><td>6%</td><td>3%</td></tr>
        <tr><td>TikTok</td><td>6%</td><td>5%</td><td>7%</td><td>4%</td></tr>
        <tr><td>Tokopedia</td><td>5%</td><td>4%</td><td>6%</td><td>3%</td></tr>
        <tr><td>Lazada</td><td>6%</td><td>5%</td><td>6%</td><td>4%</td></tr>
    </table>
</div>

<div class="container">
    <h2>Tabel Fee Event</h2>
    <table>
        <tr><th>Event</th><th>Fee Tambahan</th></tr>
        <tr><td>Gratis Ongkir</td><td>3%</td></tr>
        <tr><td>Tanggal Kembar (11.11, 12.12, dll)</td><td>5%</td></tr>
        <tr><td>Event Gajian</td><td>4%</td></tr>
        <tr><td>Brand Mega Sale</td><td>6%</td></tr>
    </table>
</div>

<script>
const feePlatform = {
    shopee: { pakaian: 0.05, makanan: 0.04, aksesoris: 0.06, elektronik: 0.03 },
    tiktok: { pakaian: 0.06, makanan: 0.05, aksesoris: 0.07, elektronik: 0.04 },
    tokped: { pakaian: 0.05, makanan: 0.04, aksesoris: 0.06, elektronik: 0.03 },
    lazada: { pakaian: 0.06, makanan: 0.05, aksesoris: 0.06, elektronik: 0.04 }
};

function hitungHargaJual() {
    let hpp = Number(document.getElementById('hpp').value);
    let packing = Number(document.getElementById('packing').value);
    let platform = document.getElementById('platform').value;
    let kategori = document.getElementById('kategori').value;
    let eventFee = Number(document.getElementById('eventFee').value) / 100;
    let profit = Number(document.getElementById('profit').value);

    let fee = feePlatform[platform][kategori];

    let totalBiaya = hpp + packing;
    let hargaJual = (totalBiaya + profit) / (1 - (fee + eventFee));

    document.getElementById('hasilHarga').innerHTML = "Harga jual ideal: Rp " + hargaJual.toFixed(0);
}

function hitungROAS() {
    let hpp = Number(document.getElementById('roas_hpp').value);
    let modal = Number(document.getElementById('modal_iklan').value);
    let target = Number(document.getElementById('target_profit').value);

    let revenue = hpp + modal + target;
    let roas = revenue / modal;

    document.getElementById('hasilROAS').innerHTML = "ROAS minimal: " + roas.toFixed(2) + "x";
}
</script>

</body>
</html>
