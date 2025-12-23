<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>BrasilAds - Plataforma de Anúncios</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
body {margin:0;font-family:Arial;background:#f4f6f9}
header {background:#1a73e8;color:#fff;padding:15px;text-align:center}
section {padding:20px}
.box {background:#fff;padding:20px;border-radius:8px;max-width:900px;margin:auto;margin-bottom:20px}
input,button {width:100%;padding:12px;margin-top:10px}
button {background:#1a73e8;color:#fff;border:none;border-radius:5px;font-size:16px}
button:hover {background:#155ab6}
.ad {border-bottom:1px solid #ddd;padding:10px 0}
small {color:#555}
.hidden {display:none}
nav button {width:auto;margin:5px}
</style>
</head>

<body>

<header>
<h1>BrasilAds</h1>
<p>Divulgue, alcance clientes e aumente suas vendas</p>
</header>

<section id="home" class="box">
<h2>Anuncie como no Google Ads, porém mais simples</h2>
<p>Milhares de visualizações para seu site, produto ou serviço.</p>
<button onclick="show('login')">Entrar</button>
<button onclick="show('register')">Criar Conta</button>
</section>

<section id="login" class="box hidden">
<h2>Entrar</h2>
<input id="loginEmail" placeholder="Email">
<input id="loginPass" type="password" placeholder="Senha">
<button onclick="login()">Entrar</button>
</section>

<section id="register" class="box hidden">
<h2>Criar Conta</h2>
<input id="regEmail" placeholder="Email">
<input id="regPass" type="password" placeholder="Senha">
<button onclick="register()">Cadastrar</button>
</section>

<section id="dashboard" class="box hidden">
<h2>Painel do Anunciante</h2>
<nav>
<button onclick="show('newAd')">Criar Anúncio</button>
<button onclick="show('ads')">Meus Anúncios</button>
<button onclick="logout()">Sair</button>
</nav>
</section>

<section id="newAd" class="box hidden">
<h2>Novo Anúncio</h2>
<input id="adTitle" placeholder="Título do anúncio">
<input id="adDesc" placeholder="Descrição">
<input id="adLink" placeholder="Link">
<button onclick="createAd()">Publicar</button>
</section>

<section id="ads" class="box hidden">
<h2>Anúncios Ativos</h2>
<div id="adsList"></div>
</section>

<section class="box">
<h2>Anúncios em Destaque</h2>
<div id="publicAds"></div>
</section>

<script>
let user = null;
let ads = JSON.parse(localStorage.getItem("ads")) || [];

function show(id){
document.querySelectorAll("section").forEach(s=>s.classList.add("hidden"));
document.getElementById(id).classList.remove("hidden");
}

function register(){
localStorage.setItem("user",JSON.stringify({
email:regEmail.value,
pass:regPass.value
}));
alert("Conta criada!");
show("login");
}

function login(){
let u = JSON.parse(localStorage.getItem("user"));
if(u && u.email==loginEmail.value && u.pass==loginPass.value){
user=u;
show("dashboard");
}else alert("Dados inválidos");
}

function logout(){
user=null;
show("home");
}

function createAd(){
ads.push({
title:adTitle.value,
desc:adDesc.value,
link:adLink.value,
views:0
});
localStorage.setItem("ads",JSON.stringify(ads));
alert("Anúncio publicado!");
show("ads");
loadAds();
}

function loadAds(){
adsList.innerHTML="";
publicAds.innerHTML="";
ads.forEach((a,i)=>{
adsList.innerHTML+=`
<div class="ad">
<b>${a.title}</b><br>${a.desc}<br>
<small>Views: ${a.views}</small>
</div>`;
publicAds.innerHTML+=`
<div class="ad">
<a href="${a.link}" target="_blank" onclick="addView(${i})">
<b>${a.title}</b></a>
<p>${a.desc}</p>
<small>Visualizações: ${a.views}</small>
</div>`;
});
}

function addView(i){
ads[i].views++;
localStorage.setItem("ads",JSON.stringify(ads));
}

setInterval(()=>{
ads.forEach(a=>a.views+=Math.floor(Math.random()*3));
localStorage.setItem("ads",JSON.stringify(ads));
loadAds();
},3000);

loadAds();
</script>

</body>
</html>
  Webads
