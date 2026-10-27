
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>DashPay M-Pesa - Ruiru Exchange</title>
<link rel="manifest" href="manifest.json">
<style>
body{font-family:system-ui;background:#0d1117;color:white;padding:15px;text-align:center}
.card{background:#161b22;padding:18px;border-radius:16px;margin:12px 0;text-align:left}
input{width:92%;padding:14px;border-radius:10px;border:1px solid #30363d;background:#0d1117;color:white;font-size:16px;margin:8px 0}
button{background:#00a651;color:white;padding:15px;border:none;border-radius:10px;font-size:17px;width:100%;font-weight:bold}
.fee{color:#f0883e;font-size:13px}
.seller{background:#0d253f;border:1px solid #1f6feb;border-radius:12px;padding:12px;text-align:center}
.kes{font-size:26px;color:#58a6ff;font-weight:bold}
</style>
</head>
<body>
<img src="icon.png" width="85" style="border-radius:18px">
<h2>Dash → M-Pesa</h2>
<small>Ruiru Fast Exchange | 0142929374</small>

<div class="card">
<label>Customer M-Pesa Number</label>
<input id="phone" placeholder="07xx or 01xx">
<label>Dash Amount Customer Sends</label>
<input id="dash" type="number" step="0.01" placeholder="e.g. 1.5" oninput="calc()">

<div class="kes" id="totalKes">0 KES</div>
<div class="fee" id="feeInfo"></div>
<div style="margin-top:8px">Customer will RECEIVE: <b id="receive" style="color:#3fb950">0 KES</b></div>

<button onclick="sendMpesa()" style="margin-top:12px">SEND TO M-PESA ACCOUNT</button>
<p id="status" style="font-size:13px;color:#8b949e"></p>
</div>

<div class="seller">
<b>Exchange Owner: 0142929374</b><br>
<small>Fee: 5% | Powered by DashPay</small><br>
<a id="waLink" href="https://wa.me/254142929374"><button style="background:#25D366;margin-top:10px">Contact Seller on WhatsApp</button></a>
</div>

<script>
const SELLER_PHONE = "0142929374";
const COMMISSION = 0.05; // 5%
const KOTANI_KEY = "PASTE_YOUR_KEY_HERE"; // Get from kotanipay.com/dashboard
let rate = 3500;

fetch('https://api.coingecko.com/api/v3/simple/price?ids=dash&vs_currencies=kes')
.then(r=>r.json()).then(j=>{ if(j.dash?.kes) rate=j.dash.kes; calc(); });

function calc(){
 let d = parseFloat(document.getElementById('dash').value)||0;
 let total = d * rate;
 let fee = total * COMMISSION;
 let receive = total - fee;
 document.getElementById
