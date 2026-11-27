# my-earning-app
my Earning app visibility public 

<!DOCTYPE html>
<html lang="hi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>My Earning App</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial;background:#f1f4f9;color:#222}
header{background:#5426c9;color:white;padding:20px;font-size:23px;font-weight:bold;text-align:center}
.container{padding:16px}
.card{background:white;padding:18px;border-radius:14px;margin-bottom:15px;box-shadow:0 3px 12px #ddd}
.balance{background:linear-gradient(120deg,#5426c9,#8b45ed);color:white}
.amount{font-size:32px;font-weight:bold;margin:12px 0}
button{border:0;padding:12px;border-radius:9px;background:#5426c9;color:white;font-size:15px;margin:5px;cursor:pointer}
input{width:100%;padding:13px;margin:8px 0;border:1px solid #ccc;border-radius:8px}
h3{margin-top:0}
nav{display:flex;flex-wrap:wrap;justify-content:center;background:white;padding:8px}
nav button{font-size:13px}
.hidden{display:none}
.green{color:green}
</style>
</head>
<body>
<header>💰 MY EARNING APP</header>

<div class="container">

<div id="login" class="card">
<h3>Login / Signup</h3>
<input id="username" placeholder="अपना नाम लिखें">
<input id="password" type="password" placeholder="Password">
<button onclick="signup()">Signup</button>
<button onclick="loginUser()">Login</button>
<p id="loginMsg"></p>
</div>

<div id="app" class="hidden">
<nav>
<button onclick="show('home')">Home</button>
<button onclick="show('earn')">Earn</button>
<button onclick="show('wallet')">Wallet</button>
<button onclick="show('history')">History</button>
<button onclick="show('profile')">Profile</button>
</nav>

<section id="home">
<div class="card balance">
<h3>My Total Balance</h3>
<div class="amount">₹<span id="balance">0</span></div>
Welcome to My Earning App
</div>
<div class="card">
<h3>🎁 Commission Earning</h3>
<p>अपने रेफरल लिंक से नए यूजर जोड़ें।</p>
<button onclick="copyReferral()">Referral Link Copy</button>
</div>
</section>

<section id="earn" class="hidden">
<div class="card">
<h3>Commission Task</h3>
<p>अभी कोई सत्यापित कमाई टास्क उपलब्ध नहीं है।</p>
</div>
</section>

<section id="wallet" class="hidden">
<div class="card">
<h3>My Wallet</h3>
<h2>₹<span id="walletBalance">0</span></h2>
<input id="withdrawAmount" type="number" placeholder="Withdrawal Amount">
<input id="upi" placeholder="UPI ID">
<button onclick="withdraw()">Withdrawal Request</button>
<p id="withdrawMsg"></p>
</div>
</section>

<section id="history" class="hidden">
<div class="card">
<h3>Transaction History</h3>
<div id="transactions">No transactions yet.</div>
</div>
</section>

<section id="profile" class="hidden">
<div class="card">
<h3>My Profile</h3>
<p id="profileName"></p>
<button onclick="logout()">Logout</button>
</div>
</section>

</div>

<div id="admin" class="hidden card">
<h3>Admin Panel</h3>
<p>Admin Login Successful</p>
<p>यह डेमो एडमिन पैनल है।</p>
<button onclick="logout()">Logout</button>
</div>
</div>

<script>
let currentUser="";
let users=JSON.parse(localStorage.getItem("users")||"{}");

function signup(){
let u=username.value.trim(),p=password.value;
if(!u||!p){loginMsg.innerText="सभी जानकारी भरें";return;}
if(users[u]){loginMsg.innerText="User पहले से मौजूद है";return;}
users[u]={password:p,balance:0,history:[]};
localStorage.setItem("users",JSON.stringify(users));
loginMsg.innerText="Signup सफल! अब Login करें।";
}
function loginUser(){
let u=username.value.trim(),p=password.value;
if(u==="milan" && p==="Ram@123"){
login.classList.add("hidden");
admin.classList.remove("hidden");
return;
}
if(users[u]&&users[u].password===p){
currentUser=u;
login.classList.add("hidden");
app.classList.remove("hidden");
update();
}else loginMsg.innerText="गलत Login जानकारी";
}
function show(id){
["home","earn","wallet","history","profile"].forEach(x=>document.getElementById(x).classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}
function update(){
let b=users[currentUser].balance;
balance.innerText=b;
walletBalance.innerText=b;
profileName.innerText="Name: "+currentUser;
transactions.innerHTML=users[currentUser].history.map(x=>"<p>"+x+"</p>").join("")||"No transactions yet.";
}
function withdraw(){
let a=Number(withdrawAmount.value),v=upi.value.trim();
if(a<=0||!v){withdrawMsg.innerText="सही Amount और UPI ID डालें";return;}
if(a>users[currentUser].balance){withdrawMsg.innerText="Insufficient Balance";return;}
withdrawMsg.innerText="यह डेमो है। वास्तविक भुगतान के लिए सर्वर जरूरी है।";
}
function copyReferral(){
let link=location.href+"?ref="+encodeURIComponent(currentUser);
if(navigator.clipboard)navigator.clipboard.writeText(link).then(()=>alert("Referral Link Copied"));
else alert(link);
}
function logout(){
currentUser="";
app.classList.add("hidden");
admin.classList.add("hidden");
login.classList.remove("hidden");
}
</script>
</body>
</html>
