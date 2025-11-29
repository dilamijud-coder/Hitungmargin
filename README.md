<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Kalkulator Toko Online</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      padding: 20px;
    }
    .container {
      max-width: 500px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }
    input, select {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
    button {
      width: 100%;
      padding: 12px;
      background: #4CAF50;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }
    button:hover {
      background: #45a049;
    }
    .result {
      margin-top: 20px;
      padding: 15px;
      background: #e8ffe8;
      border-left: 5px solid #4CAF50;
      border-radius: 5px;
    }
  </style>
</head>
<body>

<div class="container">
  <h2>Kalkulator Toko Online</h2>

  <label>Harga Modal (Rp)</label>
  <input type="number" id="modal" />

  <label>Harga Jual (Rp)</label>
  <input type="number" id="jual" />

  <label>Platform / Aplikasi</label>
  <select id="platform">
    <option value="0.02">Shopee (2%)</option>
    <option value="0.04">Tokopedia (4%)</option>
    <option value="0.05">Lazada (5%)</option>
  </select>

  <label>Biaya Event / Promo (%)</label>
  <input type="number" id="event" placeholder="contoh: 10 untuk 10%" />

  <button onclick="hitung()">Hitung</button>

  <div class="result" id="hasil"></div>

  <h3>Hitung Harga Jual Berdasarkan Keuntungan</h3>
  <label>Keuntungan yang Diinginkan (Rp)</label>
  <input type="number" id="keuntunganDiinginkan" />
  <button onclick="hitungHargaJual()">Hitung Harga Jual</button>
  <div class="result" id="hasilJual"></div>
</div>

<script>
function hitung() {
  let modal = parseFloat(document.getElementById('modal').value);
  let jual = parseFloat(document.getElementById('jual').value);
  let platform = parseFloat(document.getElementById('platform').value);
  let event = parseFloat(document.getElementById('event').value) / 100;

  let feePlatform = jual * platform;
  let feeEvent = jual * event;
  let totalFee = feePlatform + feeEvent;
  let keuntungan = jual - modal - totalFee;

  document.getElementById('hasil').innerHTML = `
    <b>Rincian:</b><br>
    Fee Platform: Rp ${feePlatform.toLocaleString()}<br>
    Fee Event: Rp ${feeEvent.toLocaleString()}<br>
    Total Fee: Rp ${totalFee.toLocaleString()}<br>
    <b>Keuntungan Bersih: Rp ${keuntungan.toLocaleString()}</b>
  `;
}

function hitungHargaJual() {
  let modal = parseFloat(document.getElementById('modal').value);
  let keuntunganDiinginkan = parseFloat(document.getElementById('keuntunganDiinginkan').value);
  let platform = parseFloat(document.getElementById('platform').value);
  let event = parseFloat(document.getElementById('event').value) / 100;

  let totalPersen = 1 - (platform + event);
  let hargaJual = (modal + keuntunganDiinginkan) / totalPersen;

  document.getElementById('hasilJual').innerHTML = `
    <b>Harga Jual yang Disarankan:</b><br>
    Rp ${hargaJual.toLocaleString()}
  `;
}
</script>

</body>
</html>
