<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DashPay Ruiru - Live Price</title>
<link rel="manifest" href="./manifest.json">
<style>
*{box-sizing:border-box}body{font-family:system-ui;margin:0;background:#fff8f0;display:flex;justify-content:center;padding:15px}
.card{width:100%;max-width:400px;background:white;padding:22px;border-radius:20px;box-shadow:0 8px 30px rgba(0,0,0,.1);border-top:6px solid #f7931e}
h2{margin:0}.live{font-size:12px;color:green;background:#e6f9e6;padding:5px 10px;border-radius:20px;display:inline-block;margin:5px 0}
input,button{width:100%;padding:15px;margin-top:8px;border-radius:12px;border:1.5px solid #ddd;font-size:16px}
button{background:#f7931e;color:white;font-weight:800;border:none}
.box{background:#f9f9f9;padding:14px;border-radius:12px;margin:15px 0;display:flex;justify-content:space-between}
#status{margin-top:15px;padding:12px;border-radius:10px;font-size:14px;display:none}
</style>
</head>
<body>
<div class="card">
<h2>⚡ DashPay Ruiru</h2>
<div id="price" class="live">Loading live Dash price...</div>
<p style="font-size:13px;color:#666">Business: 0142929374 • Fee 5%</p>

<label>M-Pesa Account Number</label>
<input id="mpesa" type="tel" placeholder="07XXXXXXXX">

<label>Dash Amount</label>
<input id="dash" type="number" placeholder="e.g. 0.5" oninput="calc()" step="0.0001">

<div class="box">
<div><small>Fee 5%</small><br><b id="fee">0 KES</b></div>
<div style="text-align:right"><small>You Send</small><br><b id="kes" style="color:#0a8a0a">0 KES</b></div>
</div>

<button onclick="send()">SEND TO M-PESA ACCOUNT</button>
<div id="status"></div>
<a href="https://wa.me/2541142929374" style="display:block;text-align:center;margin-top:15px;color:#25D366;font-weight:600;text-decoration:none">WhatsApp Support</a>
</div>

<script>
let DASH_RATE = 2500; // fallback

// LIVE PRICE FROM YOUR LINK
async function loadPrice(){
 try{
  const res = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=dash&vs_currencies=kes');
  const data = await res.json();
  DASH_RATE = data.dash.kes;
  document.getElementById('price').innerText = `● LIVE: 1 DASH = ${DASH_RATE.toLocaleString()} KES`;
  calc();
 }catch(e){
  document.getElementById('price').innerText = `Using default: 1 DASH = ${DASH_RATE} KES (offline)`;
 }
}
loadPrice();

function calc(){
 let d = parseFloat(document.getElementById('dash').value)||0;
 let total = d * DASH_RATE;
 let fee = total*0.05;
 document.getElementById('fee').innerText = fee.toFixed(2)+' KES';
 document.getElementById('kes').innerText = (total-fee).toFixed(2)+' KES';
}
function send(){
 let m=document.getElementById('mpesa').value;
 let d=document.getElementById('dash').value;
 let box=document.getElementById('status');
 if(m.length<10||!d){box.style.display='block';box.style.background='#ffe0e0';box.innerText='Enter M-Pesa and Dash';return;}
 box.style.display='block';box.style.background='#e6f9e6';box.style.border='1px solid #b5e6b5';
 box.innerHTML=`✅ LIVE Order<br>M-Pesa: ${m}<br>Amount: ${document.getElementById('kes').innerText}<br><br>Customer sends ${d} DASH to:<br><code>YOUR_DASH_ADDRESS</code><br><br>Then you send M-Pesa from 0142929374`;
}
</script>
</body>
</html>
