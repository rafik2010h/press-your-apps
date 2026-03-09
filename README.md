<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Press Your App</title>
<style>
body{
    font-family: Arial;
    margin: 0;
    background: url('a_digital_illustration_features_a_promotional_bann.png') no-repeat center center fixed;
    background-size: cover;
    color: white;
    text-align: center;
}
header{
    padding: 25px;
    font-size: 30px;
    font-weight: bold;
    background: rgba(0,0,0,0.25);
}
.container{
    padding: 30px;
    background: rgba(0,0,0,0.3);
    border-radius: 20px;
    max-width: 1100px;
    margin: 20px auto;
    display:none; /* نخفيها حتى يسجل الدخول */
}
.search{
    padding: 12px;
    border: none;
    border-radius: 20px;
    width: 260px;
    margin-bottom: 20px;
}
.addbox input{
    padding: 10px;
    border: none;
    border-radius: 10px;
    margin: 5px;
}
.addbox button{
    padding: 10px 20px;
    border: none;
    border-radius: 10px;
    background: #00e676;
    cursor: pointer;
}
.grid{
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
    gap: 20px;
    max-width: 1000px;
    margin: auto;
}
.app{
    background: rgba(255,255,255,0.2);
    padding: 25px;
    border-radius: 20px;
    transition: 0.3s;
    position: relative;
}
.app:hover{
    transform: scale(1.08);
    background: rgba(255,255,255,0.3);
}
.app a{
    color: white;
    font-size: 18px;
    text-decoration: none;
}
.delete{
    position: absolute;
    top: 8px;
    right: 10px;
    background: red;
    border: none;
    color: white;
    border-radius: 50%;
    width: 22px;
    height: 22px;
    cursor: pointer;
}
.fav{
    position: absolute;
    top: 8px;
    left: 10px;
    background: gold;
    border: none;
    border-radius: 50%;
    width: 22px;
    height: 22px;
    cursor: pointer;
}
footer{
    margin-top: 40px;
    opacity: 0.7;
}
</style>

<script src="https://accounts.google.com/gsi/client" async defer></script>
</head>
<body>

<header>🚀 Press Your App</header>

<!-- هنا نبدل Get Started بزر تسجيل الدخول Google -->
<div id="google-signin" style="margin:30px auto;"></div>

<!-- Main Container -->
<div class="container">
<input class="search" id="search" placeholder="Search..." onkeyup="searchApp()">
<div class="addbox">
    <input id="name" placeholder="App name">
    <input id="link" placeholder="App link">
    <button onclick="addApp()">Add</button>
</div>
<div class="grid" id="apps"></div>
</div>

<footer>Press Your App © 2026</footer>

<script>
// ================= Apps Array =================
let apps = [
{name:"YouTube",link:"https://youtube.com",color:"#FF0000"},
{name:"Instagram",link:"https://instagram.com",color:"#C13584"},
{name:"TikTok",link:"https://tiktok.com",color:"#69C9D0"},
{name:"Discord",link:"https://discord.com",color:"#7289DA"},
{name:"Facebook",link:"https://facebook.com",color:"#1877F2"},
{name:"Twitter / X",link:"https://x.com",color:"#1DA1F2"},
{name:"Netflix",link:"https://netflix.com",color:"#E50914"},
{name:"Spotify",link:"https://spotify.com",color:"#1DB954"},
{name:"Twitch",link:"https://twitch.tv",color:"#9146FF"},
{name:"Google",link:"https://google.com",color:"#4285F4"},
{name:"Gmail",link:"https://mail.google.com",color:"#D93025"}
];

// ============== Functions ====================
function load(){
    let grid=document.getElementById("apps");
    grid.innerHTML="";
    apps.forEach((app,i)=>{
        grid.innerHTML+=`
        <div class="app" style="background:${app.color}">
            <button class="delete" onclick="del(${i})">x</button>
            <button class="fav" onclick="fav(${i})">★</button>
            <a href="${app.link}" target="_blank">${app.name}</a>
        </div>
        `;
    });
}

function addApp(){
    let name=document.getElementById("name").value.trim();
    let link=document.getElementById("link").value.trim();
    if(name && link){
        apps.push({name,link,color:"#888"});
        load();
        document.getElementById("name").value="";
        document.getElementById("link").value="";
    }
}

function del(i){
    apps.splice(i,1);
    load();
}

function fav(i){
    let item=apps.splice(i,1)[0];
    apps.unshift(item);
    load();
}

function searchApp(){
    let s=document.getElementById("search").value.toLowerCase();
    document.querySelectorAll(".app").forEach(app=>{
        app.style.display = app.innerText.toLowerCase().includes(s) ? "block" : "none";
    });
}

// ============== Google Login =================
window.onload = function() {
    google.accounts.id.initialize({
        client_id: "YOUR_GOOGLE_CLIENT_ID",
        callback: handleCredentialResponse,
        auto_select: true
    });
    google.accounts.id.renderButton(
        document.getElementById("google-signin"),
        { theme: "outline", size: "large", width: 250 }  // حجم الزر
    );
    google.accounts.id.prompt(); // يظهر تسجيل الدخول مباشرة
}

function handleCredentialResponse(response){
    console.log("JWT Token:", response.credential);
    document.getElementById("google-signin").style.display = "none"; // نخفي الزر
    document.querySelector(".container").style.display = "block"; // نوري التطبيقات
    load();
}
</script>
</body>
</html>
