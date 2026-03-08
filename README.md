# pro4
<!DOCTYPE html>
<html>
<head>
<title>Smart Product Recommendation System</title>

<style>

body{
font-family:Arial;
margin:0;
background:#eef2f7;
}

header{
background:#2563eb;
color:white;
padding:18px;
text-align:center;
font-size:24px;
}

.container{
padding:25px;
text-align:center;
}

.page{
display:none;
animation:fade 0.7s ease;
}

@keyframes fade{
from{opacity:0;transform:translateY(10px);}
to{opacity:1;transform:translateY(0);}
}

input,select{
padding:10px;
margin:6px;
border-radius:6px;
border:1px solid #ccc;
width:220px;
}

button{
padding:10px 15px;
border:none;
background:#2563eb;
color:white;
border-radius:6px;
cursor:pointer;
margin:6px;
}

button:hover{
background:#1e40af;
}

.products{
display:grid;
grid-template-columns:repeat(auto-fill,minmax(250px,1fr));
gap:20px;
margin-top:25px;
}

.card{
background:white;
border-radius:12px;
padding:15px;
box-shadow:0 4px 15px rgba(0,0,0,0.1);
}

.card img{
width:100%;
height:170px;
object-fit:cover;
border-radius:8px;
}

.pricebox{
background:#eef2ff;
padding:6px;
margin-top:5px;
border-radius:5px;
}

.rating{color:#f59e0b;}

.best{
background:#16a34a;
color:white;
padding:4px;
border-radius:5px;
display:inline-block;
margin-top:5px;
}

.filterbox{
margin-top:15px;
}

</style>
</head>

<body>

<header>
Smart Product Recommendation System
</header>

<!-- ROLE PAGE -->

<div id="rolePage" class="container page" style="display:block">

<h2>Select Login</h2>

<button onclick="openUser()">User</button>
<button onclick="openAdmin()">Admin</button>

</div>

<!-- USER LOGIN -->

<div id="userLoginPage" class="container page">

<h2>User Login</h2>

<input id="mobile" placeholder="Mobile Number"><br>

<select id="location">
<option value="">Select Location</option>
<option>Chennai</option>
<option>Madurai</option>
<option>Trichy</option>
<option>Coimbatore</option>
</select><br>

<button onclick="login()">Enter Website</button>

</div>

<!-- ADMIN LOGIN -->

<div id="adminLoginPage" class="container page">

<h2>Admin Login</h2>

<input id="adminUser" placeholder="Admin Username"><br>
<input id="adminPass" type="password" placeholder="Password"><br>

<button onclick="adminLogin()">Login</button>

</div>

<!-- USER MAIN PAGE -->

<div id="mainPage" class="container page">

<h3>Search Product</h3>

<input id="searchInput" placeholder="Search phone, sofa, hair oil, shirt">
<button onclick="searchProduct()">Search</button>
<button onclick="resetSearch()">Reset</button>

<div id="dynamicFilters" class="filterbox"></div>

<div class="products" id="productList"></div>

</div>

<!-- ADMIN PAGE -->

<div id="adminPage" class="container page">

<h3>Admin Product Panel</h3>

<input id="pname" placeholder="Product Name"><br>
<input id="ptype" placeholder="Product Type"><br>
<input id="pimage" placeholder="Image URL"><br>

<button onclick="addProduct()">Add Product</button>

</div>

<script>

let products=[

{
name:"iPhone 14",
type:"mobile",
brand:"Apple",
image:"https://images.unsplash.com/photo-1511707171634",
rating:"4.8",
review:"Best camera phone",
online:{amazon:78999,flipkart:78499},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Reliance Digital",price:79000,map:"https://maps.google.com"}]
},

{
name:"Samsung Galaxy S23",
type:"mobile",
brand:"Samsung",
image:"https://images.unsplash.com/photo-1580910051074",
rating:"4.7",
review:"Flagship performance",
online:{amazon:70999,flipkart:69999},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Mobile World",price:70500,map:"https://maps.google.com"}]
},

{
name:"LG Smart TV",
type:"tv",
brand:"LG",
image:"https://images.unsplash.com/photo-1593784991095",
rating:"4.6",
review:"4K Smart TV",
online:{amazon:45999,flipkart:44999},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Reliance Digital",price:44500,map:"https://maps.google.com"}]
},

{
name:"Wooden Sofa",
type:"sofa",
brand:"Ikea",
image:"https://images.unsplash.com/photo-1505693416388",
rating:"4.5",
review:"Comfortable sofa",
online:{amazon:25000,flipkart:24000},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Furniture World",price:23500,map:"https://maps.google.com"}]
},

{
name:"Parachute Hair Oil",
type:"hair",
hair:"Normal",
image:"https://images.unsplash.com/photo-1620912189868",
rating:"4.4",
review:"Healthy hair oil",
online:{amazon:180,flipkart:175},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Super Market",price:170,map:"https://maps.google.com"}]
},

{
name:"Indulekha Hair Oil",
type:"hair",
hair:"Curly",
image:"https://images.unsplash.com/photo-1600180758895",
rating:"4.3",
review:"Hair growth oil",
online:{amazon:420,flipkart:399},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Medical Store",price:390,map:"https://maps.google.com"}]
},

{
name:"Nivea Face Wash",
type:"face",
skin:"Oily",
image:"https://images.unsplash.com/photo-1596755389378",
rating:"4.4",
review:"Best for oily skin",
online:{amazon:249,flipkart:239},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Apollo Pharmacy",price:240,map:"https://maps.google.com"}]
},

{
name:"Himalaya Face Wash",
type:"face",
skin:"Dry",
image:"https://images.unsplash.com/photo-1608248597279",
rating:"4.3",
review:"Herbal face wash",
online:{amazon:199,flipkart:189},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Medical Shop",price:185,map:"https://maps.google.com"}]
},

{
name:"Cricket Bat",
type:"sports",
color:"Red",
image:"https://images.unsplash.com/photo-1599058917765",
rating:"4.5",
review:"Professional bat",
online:{amazon:1500,flipkart:1450},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Sports Hub",price:1400,map:"https://maps.google.com"}]
},

{
name:"Sports T Shirt",
type:"sports",
color:"Yellow",
image:"https://images.unsplash.com/photo-1520975922284",
rating:"4.2",
review:"Sports wear",
online:{amazon:699,flipkart:649},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Sports Shop",price:620,map:"https://maps.google.com"}]
},

{
name:"Men Shirt",
type:"clothing",
size:"L",
image:"https://images.unsplash.com/photo-1520975698519",
rating:"4.4",
review:"Stylish shirt",
online:{amazon:999,flipkart:899},
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
shops:[{name:"Fashion Store",price:850,map:"https://maps.google.com"}]
}

]

function openUser(){
document.getElementById("rolePage").style.display="none"
document.getElementById("userLoginPage").style.display="block"
}

function openAdmin(){
document.getElementById("rolePage").style.display="none"
document.getElementById("adminLoginPage").style.display="block"
}

function login(){

let mobile=document.getElementById("mobile").value
let location=document.getElementById("location").value

if(mobile.length>=10 && location!=""){

document.getElementById("userLoginPage").style.display="none"
document.getElementById("mainPage").style.display="block"

displayProducts(products)

}else{
alert("Enter valid details")
}

}

function adminLogin(){

let user=document.getElementById("adminUser").value
let pass=document.getElementById("adminPass").value

if(user=="admin" && pass=="1234"){

document.getElementById("adminLoginPage").style.display="none"
document.getElementById("adminPage").style.display="block"

}else{
alert("Invalid login")
}

}

function displayProducts(list){

let html=""

list.forEach(p=>{

let best=999999
let bestShop=""

p.shops.forEach(s=>{
if(s.price<best){
best=s.price
bestShop=s.name
}
})

html+=`

<div class="card">

<img src="${p.image}">

<h4>${p.name}</h4>

<p class="rating">⭐ ${p.rating}</p>

<p>${p.review}</p>

<div class="best">
Best Price ₹${best} at ${bestShop}
</div>

<div class="pricebox">
Amazon ₹${p.online.amazon}
<br>
<a href="${p.amazon}" target="_blank">Buy Amazon</a>
</div>

<div class="pricebox">
Flipkart ₹${p.online.flipkart}
<br>
<a href="${p.flipkart}" target="_blank">Buy Flipkart</a>
</div>

</div>
`

})

document.getElementById("productList").innerHTML=html

}

function searchProduct(){

let q=document.getElementById("searchInput").value.toLowerCase()

let result=products.filter(p=>JSON.stringify(p).toLowerCase().includes(q))

displayProducts(result)

showFilters(q)

}

function resetSearch(){
document.getElementById("searchInput").value=""
displayProducts(products)
document.getElementById("dynamicFilters").innerHTML=""
}

function showFilters(q){

let html=""

if(q.includes("tv")||q.includes("sofa")||q.includes("phone")||q.includes("fan")){
html=`
<h4>Select Brand</h4>
<button onclick="filterBrand('Apple')">Apple</button>
<button onclick="filterBrand('Samsung')">Samsung</button>
<button onclick="filterBrand('LG')">LG</button>
<button onclick="filterBrand('Ikea')">Ikea</button>
`
}

else if(q.includes("hair")){
html=`
<h4>Select Hair Type</h4>
<button onclick="filterHair('Normal')">Normal</button>
<button onclick="filterHair('Curly')">Curly</button>
`
}

else if(q.includes("face")){
html=`
<h4>Select Skin Type</h4>
<button onclick="filterSkin('Oily')">Oily</button>
<button onclick="filterSkin('Dry')">Dry</button>
`
}

else if(q.includes("bat")||q.includes("ball")||q.includes("sports")){
html=`
<h4>Select Color</h4>
<button onclick="filterColor('Red')">Red</button>
<button onclick="filterColor('Yellow')">Yellow</button>
`
}

else if(q.includes("shirt")||q.includes("pant")){
html=`
<h4>Select Size</h4>
<button onclick="filterSize('S')">S</button>
<button onclick="filterSize('M')">M</button>
<button onclick="filterSize('L')">L</button>
<button onclick="filterSize('XL')">XL</button>
`
}

document.getElementById("dynamicFilters").innerHTML=html

}

function filterBrand(b){
displayProducts(products.filter(p=>p.brand==b))
}

function filterHair(h){
displayProducts(products.filter(p=>p.hair==h))
}

function filterSkin(s){
displayProducts(products.filter(p=>p.skin==s))
}

function filterColor(c){
displayProducts(products.filter(p=>p.color==c))
}

function filterSize(s){
displayProducts(products.filter(p=>p.size==s))
}

function addProduct(){

let name=document.getElementById("pname").value
let type=document.getElementById("ptype").value
let image=document.getElementById("pimage").value

products.push({
name:name,
type:type,
image:image,
rating:"4",
review:"New product",
amazon:"https://amazon.in",
flipkart:"https://flipkart.com",
online:{amazon:100,flipkart:95},
shops:[{name:"Local Shop",price:90,map:"https://maps.google.com"}]
})

alert("Product Added")

}

</script>

</body>
</html>
