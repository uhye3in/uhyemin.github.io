# uhyemin.github.io
from pathlib import Path
from zipfile import ZipFile, ZIP_DEFLATED

root = Path("/mnt/data/campus-market-pro")
(root/"css").mkdir(parents=True, exist_ok=True)
(root/"js").mkdir(parents=True, exist_ok=True)

files = {
"index.html": """<!DOCTYPE html><html lang='ko'><head><meta charset='UTF-8'><meta name='viewport' content='width=device-width,initial-scale=1'><title>캠퍼스 중고거래</title><link rel='stylesheet' href='css/style.css'></head><body><div class='auth'><h1>캠퍼스 중고거래</h1><input id='studentId' placeholder='학번'><input id='password' type='password' placeholder='비밀번호'><button onclick='login()'>로그인</button></div><script src='js/app.js'></script></body></html>""",
"home.html": """<!DOCTYPE html><html lang='ko'><head><meta charset='UTF-8'><meta name='viewport' content='width=device-width,initial-scale=1'><link rel='stylesheet' href='css/style.css'></head><body><nav><a href='home.html'>홈</a><a href='publish.html'>등록</a><a href='profile.html'>마이</a></nav><div class='container'><input id='search' placeholder='상품 검색' oninput='renderProducts()'><div id='products' class='grid'></div></div><script src='js/app.js'></script></body></html>""",
"publish.html": """<!DOCTYPE html><html lang='ko'><head><meta charset='UTF-8'><link rel='stylesheet' href='css/style.css'></head><body><div class='container'><h2>상품 등록</h2><input id='title' placeholder='상품명'><input id='price' placeholder='가격'><select id='category'><option>전자제품</option><option>교재</option><option>생활용품</option></select><textarea id='desc' placeholder='상품 설명'></textarea><input type='file' id='image'><button onclick='addProduct()'>등록</button></div><script src='js/app.js'></script></body></html>""",
"profile.html": """<!DOCTYPE html><html lang='ko'><head><meta charset='UTF-8'><link rel='stylesheet' href='css/style.css'></head><body><div class='container'><h2>내 상품</h2><div id='myProducts'></div><h2>찜한 상품</h2><div id='favProducts'></div></div><script src='js/app.js'></script></body></html>""",
"detail.html": """<!DOCTYPE html><html lang='ko'><head><meta charset='UTF-8'><link rel='stylesheet' href='css/style.css'></head><body><div class='container' id='detail'></div><script src='js/app.js'></script></body></html>"""
}
for n,c in files.items(): (root/n).write_text(c,encoding="utf-8")

(root/"css/style.css").write_text("""
*{box-sizing:border-box}body{margin:0;font-family:sans-serif;background:#f4f6fb}
nav{display:flex;gap:20px;padding:15px;background:#fff;position:sticky;top:0}
a{text-decoration:none;color:#333}
.container,.auth{max-width:1000px;margin:30px auto;background:#fff;padding:20px;border-radius:20px}
input,textarea,select,button{width:100%;padding:12px;margin:8px 0;border:1px solid #ddd;border-radius:12px}
button{background:#4f46e5;color:#fff;border:none}
.grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:15px}
.card{background:#fff;border-radius:16px;overflow:hidden;box-shadow:0 4px 15px rgba(0,0,0,.08);padding:10px}
.card img{width:100%;height:180px;object-fit:cover}
""", encoding="utf-8")

(root/"js/app.js").write_text("""
if(!localStorage.users)localStorage.users=JSON.stringify([{studentId:'20240001',password:'123456'}]);
const get=(k,d=[])=>JSON.parse(localStorage.getItem(k)||JSON.stringify(d));
const set=(k,v)=>localStorage.setItem(k,JSON.stringify(v));

function login(){
const u=get('users').find(x=>x.studentId==studentId.value&&x.password==password.value);
if(u){set('currentUser',u);location='home.html'}else alert('로그인 실패');
}

function addProduct(){
const file=image.files[0]; const r=new FileReader();
r.onload=e=>save(e.target.result); if(file)r.readAsDataURL(file); else save('');
function save(img){
const arr=get('products');
arr.push({id:Date.now(),title:title.value,price:price.value,category:category.value,desc:desc.value,image:img,seller:get('currentUser',{}).studentId,status:'판매중'});
set('products',arr); location='home.html';
}}

function renderProducts(){
const box=document.getElementById('products'); if(!box)return;
const q=(document.getElementById('search').value||'').toLowerCase();
box.innerHTML=get('products').filter(p=>p.title.toLowerCase().includes(q)).map(p=>`
<div class='card'>
${p.image?`<img src="${p.image}">`:''}
<h3>${p.title}</h3><p>${p.price}원</p><small>${p.category}</small><br>
<button onclick="location='detail.html?id=${p.id}'">상세보기</button>
</div>`).join('');
}
renderProducts();

const params=new URLSearchParams(location.search);
if(document.getElementById('detail')){
const p=get('products').find(x=>x.id==params.get('id'));
if(p)detail.innerHTML=`${p.image?`<img src="${p.image}" style="width:100%;max-height:350px;object-fit:cover">`:''}<h2>${p.title}</h2><h3>${p.price}원</h3><p>${p.desc}</p><button onclick="fav(${p.id})">❤️ 찜하기</button><button onclick="alert('판매자 학번: ${p.seller}')">판매자 연락</button>`;
}

function fav(id){
const u=get('currentUser',{}).studentId;
const f=get('favorites');
if(!f.find(x=>x.userId==u&&x.productId==id)){f.push({userId:u,productId:id});set('favorites',f);}
alert('찜 완료');
}

if(document.getElementById('myProducts')){
const u=get('currentUser',{}).studentId;
myProducts.innerHTML=get('products').filter(p=>p.seller==u).map(p=>`<div class='card'>${p.title}</div>`).join('');
const ids=get('favorites').filter(f=>f.userId==u).map(f=>f.productId);
favProducts.innerHTML=get('products').filter(p=>ids.includes(p.id)).map(p=>`<div class='card'>${p.title}</div>`).join('');
}
""", encoding="utf-8")

zip_path="/mnt/data/campus-market-pro.zip"
with ZipFile(zip_path,"w",ZIP_DEFLATED) as z:
    for f in root.rglob("*"):
        z.write(f,f.relative_to(root))
print(zip_path)
