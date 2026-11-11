
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Mahalaxmi Embroidery</title>

<style>
*{box-sizing:border-box}
body{
 margin:0;
 font-family:Arial,sans-serif;
 background:#fffaf5;
 color:#333;
}
button{
 border:0;
 padding:12px 18px;
 background:#85002f;
 color:white;
 border-radius:6px;
 cursor:pointer;
}
input{
 padding:12px;
 width:100%;
 margin:8px 0;
 border:1px solid #ddd;
 border-radius:6px;
}
.auth{
 min-height:100vh;
 display:flex;
 justify-content:center;
 align-items:center;
 background:linear-gradient(135deg,#50001d,#a00043);
 padding:20px;
}
.auth-box{
 background:white;
 padding:30px;
 border-radius:12px;
 width:100%;
 max-width:370px;
 text-align:center;
}
.auth-box h2{color:#85002f}
.auth-box button{width:100%;margin-top:10px}
.auth-box a{color:#900039;cursor:pointer}
.hidden{display:none!important}

/* WEBSITE */

header{
 background:#650024;
 color:white;
 padding:18px;
 display:flex;
 justify-content:space-between;
 align-items:center;
 flex-wrap:wrap;
}
header h2{margin:0;color:#e9bd55}
header button{background:#d9ad42;color:#350015}
nav{
 background:#d8ae50;
 padding:14px;
 text-align:center;
}
nav a{
 color:#45001b;
 text-decoration:none;
 margin:10px;
 font-weight:bold;
}
.hero{
 padding:65px 20px;
 text-align:center;
 background:#fff0df;
}
.hero h1{
 color:#79002b;
 font-size:34px;
}
.section{
 padding:30px 18px;
 text-align:center;
}
.section h2{color:#80002e}
.controls{
 max-width:500px;
 margin:20px auto;
}
.gallery{
 display:grid;
 grid-template-columns:repeat(auto-fit,minmax(160px,1fr));
 gap:18px;
 max-width:1100px;
 margin:20px auto;
}
.design{
 background:white;
 padding:10px;
 border-radius:10px;
 box-shadow:0 2px 10px #0002;
}
.design img{
 width:100%;
 height:180px;
 object-fit:cover;
 border-radius:7px;
 cursor:pointer;
}
.design h3{color:#80002e}
.design button{margin:4px}
.upload-box{
 max-width:450px;
 margin:20px auto;
 padding:20px;
 background:#fff0df;
 border-radius:10px;
}
footer{
 background:#650024;
 color:white;
 text-align:center;
 padding:20px;
}
#preview{
 position:fixed;
 inset:0;
 background:#000d;
 display:none;
 justify-content:center;
 align-items:center;
 padding:20px;
 z-index:10;
}
#preview img{
 max-width:95%;
 max-height:80vh;
 object-fit:contain;
}
</style>
</head>

<body>

<!-- LOGIN PAGE -->

<div class="auth" id="authPage">
<div class="auth-box">

<div id="loginForm">
<h2>MAHALAXMI EMBROIDERY</h2>
<p>Welcome Back!</p>
<input id="loginEmail" type="email" placeholder="Email address">
<input id="loginPass" type="password" placeholder="Password">
<button onclick="login()">LOGIN</button>
<p>New user? <a onclick="showRegister()">Create Account</a></p>
</div>

<div id="registerForm" class="hidden">
<h2>Create Account</h2>
<input id="regName" placeholder="Your name">
<input id="regEmail" type="email" placeholder="Email address">
<input id="regPass" type="password" placeholder="Create password">
<button onclick="register()">REGISTER</button>
<p>Already registered? <a onclick="showLogin()">Sign In</a></p>
</div>

</div>
</div>

<!-- MAIN WEBSITE -->

<div id="website" class="hidden">

<header>
<h2>MAHALAXMI EMBROIDERY</h2>
<button onclick="logout()">LOGOUT</button>
</header>

<nav>
<a href="#home">Home</a>
<a href="#gallery">Gallery</a>
<a href="#about">About</a>
<a href="#contact">Contact</a>
</nav>

<section class="hero" id="home">
<h1>Mahalaxmi Embroidery</h1>
<h3>Beautiful Embroidery Designs</h3>
<p>Explore our collection of creative embroidery designs.</p>
</section>

<section class="section" id="gallery">
<h2>Our Embroidery Gallery</h2>
<p>Discover our latest embroidery collections.</p>

<div class="upload-box hidden" id="ownerPanel">
<h3>Owner Dashboard</h3>
<p>Upload a new embroidery design photo.</p>
<input type="text" id="designName" placeholder="Design name">
<input type="file" id="designPhoto" accept="image/*">
<button onclick="uploadDesign()">UPLOAD DESIGN</button>
</div>

<div class="controls">
<input id="search" placeholder="Search embroidery designs..."
oninput="displayGallery()">
</div>

<div class="gallery" id="galleryList"></div>
</section>

<section class="section" id="about">
<h2>About Us</h2>
<p>Welcome to Mahalaxmi Embroidery.
Explore beautiful embroidery design photos in our gallery.</p>
</section>

<section class="section" id="contact">
<h2>Contact Us</h2>
<p>For design enquiries, contact us.</p>
</section>

<footer>
<p>© 2026 Mahalaxmi Embroidery</p>
<p>Maroon & Gold Collection</p>
</footer>

</div>

<!-- IMAGE PREVIEW -->

<div id="preview" onclick="closePreview()">
<img id="previewImage" alt="Embroidery design preview">
</div>

<script>

// DEMO SETTINGS
// Change this to the owner's email address.
// This browser-only role check is NOT secure for a public website.

const OWNER_EMAIL = "owner@example.com";

function getUsers(){
 return JSON.parse(localStorage.getItem("meUsers") || "[]");
}

function showRegister(){
 loginForm.classList.add("hidden");
 registerForm.classList.remove("hidden");
}

function showLogin(){
 registerForm.classList.add("hidden");
 loginForm.classList.remove("hidden");
}

function register(){
 let name=regName.value.trim();
 let email=regEmail.value.trim().toLowerCase();
 let password=regPass.value;

 if(!name || !email || !password){
  alert("Please fill all fields");
  return;
 }
 if(password.length<6){
  alert("Password must have at least 6 characters");
  return;
 }

 let users=getUsers();

 if(users.some(u=>u.email===email)){
  alert("Account already exists");
  return;
 }

 users.push({name,email,password});
 localStorage.setItem("meUsers",JSON.stringify(users));

 alert("Account created! Please login.");
 showLogin();
}

function login(){
 let email=loginEmail.value.trim().toLowerCase();
 let password=loginPass.value;

 let user=getUsers().find(
  u=>u.email===email && u.password===password
 );

 if(!user){
  alert("Invalid email or password");
  return;
 }

 sessionStorage.setItem("meCurrent",user.email);
 sessionStorage.setItem("meName",user.name);
 openWebsite();
}

function openWebsite(){
 let email=sessionStorage.getItem("meCurrent");
 if(!email)return;

 authPage.classList.add("hidden");
 website.classList.remove("hidden");

 ownerPanel.classList.toggle(
  "hidden",email!==OWNER_EMAIL.toLowerCase()
 );

 displayGallery();
}

function logout(){
 sessionStorage.clear();
 website.classList.add("hidden");
 authPage.classList.remove("hidden");
 showLogin();
}

function getDesigns(){
 return JSON.parse(localStorage.getItem("meDesigns") || "[]");
}

function uploadDesign(){
 let name=designName.value.trim();
 let file=designPhoto.files[0];

 if(!name || !file){
  alert("Enter a design name and select a photo");
  return;
 }

 if(!file.type.startsWith("image/")){
  alert("Please select an image");
  return;
 }

 if(file.size>500000){
  alert("Please select an image below 500 KB for this demo.");
  return;
 }

 let reader=new FileReader();

 reader.onload=function(){
  let designs=getDesigns();
  designs.push({
   id:Date.now(),
   name:name,
   image:reader.result
  });

  try{
   localStorage.setItem("meDesigns",JSON.stringify(designs));
   designName.value="";
   designPhoto.value="";
   displayGallery();
   alert("Design uploaded successfully!");
  }catch(e){
   alert("Browser storage is full. Use online image storage for more photos.");
  }
 };

 reader.readAsDataURL(file);
}

function displayGallery(){
 let gallery=document.getElementById("galleryList");
 let search=document.getElementById("search").value.toLowerCase();

 let designs=getDesigns().filter(
  d=>d.name.toLowerCase().includes(search)
 );

 gallery.innerHTML="";

 if(designs.length===0){
  gallery.innerHTML="<p>No designs found. Upload your first design!</p>";
  return;
 }

 designs.forEach(d=>{
  let card=document.createElement("div");
  card.className="design";

  let img=document.createElement("img");
  img.src=d.image;
  img.alt=d.name;
  img.onclick=()=>showPreview(d.image);

  let title=document.createElement("h3");
  title.textContent=d.name;

  card.append(img,title);

  if(sessionStorage.getItem("meCurrent")===OWNER_EMAIL.toLowerCase()){
   let del=document.createElement("button");
   del.textContent="DELETE";
   del.onclick=()=>deleteDesign(d.id);
   card.append(del);
  }

  gallery.append(card);
 });
}

function deleteDesign(id){
 if(!confirm("Delete this design?"))return;

 let designs=getDesigns().filter(d=>d.id!==id);
 localStorage.setItem("meDesigns",JSON.stringify(designs));
 displayGallery();
}

function showPreview(image){
 previewImage.src=image;
 preview.style.display="flex";
}

function closePreview(){
 preview.style.display="none";
}

openWebsite();

</script>
</body>
</html>

