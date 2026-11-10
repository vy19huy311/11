# 11
<!DOCTYPE html>
<html lang="vi">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Money Manager</title>
<style>
:root {
  --primary:#6558e8;
  --bg:#f5f6fc;
  --dark:#20243a;
  --muted:#858ba3;
  --green:#16b887;
  --red:#ed6478;
}
*{box-sizing:border-box}
body{
 margin:0;background:var(--bg);color:var(--dark);
 font-family:Inter,Arial,sans-serif;
}
.app{display:flex;min-height:100vh}
.sidebar{
 width:245px;background:#fff;padding:28px 20px;
 border-right:1px solid #eee;flex-shrink:0;
}
.logo{font-size:23px;font-weight:800;color:var(--primary);margin-bottom:42px}
.logo span{color:#252941}
.nav-title{font-size:11px;color:#aaa;margin:24px 12px 12px}
.nav button{
 display:block;width:100%;border:0;background:none;
 padding:13px 14px;text-align:left;border-radius:10px;
 color:#777e96;font-size:14px;cursor:pointer;margin:5px 0;
}
.nav button.active,.nav button:hover{
 background:#efedff;color:var(--primary);font-weight:700;
}
main{flex:1;padding:30px 35px;min-width:0}
header{display:flex;justify-content:space-between;align-items:center;margin-bottom:30px}
h1{font-size:25px;margin:0}
.subtitle{color:var(--muted);font-size:13px;margin-top:8px}
.user{background:white;padding:10px 15px;border-radius:12px;font-size:13px}
.cards{display:grid;grid-template-columns:repeat(3,1fr);gap:18px}
.card{
 background:white;border-radius:16px;padding:22px;
 box-shadow:0 4px 20px #25294a08;
}
.card-label{font-size:13px;color:var(--muted)}
.card-value{font-size:25px;font-weight:800;margin:14px 0 8px}
.green{color:var(--green)}
.red{color:var(--red)}
.purple{color:var(--primary)}
.section{
 background:white;padding:23px;border-radius:16px;
 margin-top:22px;box-shadow:0 4px 20px #25294a08;
}
.section-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:20px}
h2{font-size:17px;margin:0}
button,.btn{
 cursor:pointer;border:0;border-radius:9px;padding:10px 15px;
 font-weight:600;font-size:13px;
}
.primary{background:var(--primary);color:white}
.secondary{background:#f0efff;color:var(--primary)}
input,select{
 padding:11px;border:1px solid #e5e6ef;border-radius:9px;
 outline:none;width:100%;background:white;font-size:13px;
}
input:focus,select:focus{border-color:var(--primary)}
.form-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:12px}
.form-grid .full{grid-column:1/-1}
table{width:100%;border-collapse:collapse;font-size:13px}
th{text-align:left;color:var(--muted);font-weight:500;padding:13px 8px}
td{padding:15px 8px;border-top:1px solid #f0f0f5}
.tag{padding:6px 9px;border-radius:20px;background:#f1efff;color:var(--primary);font-size:11px}
.progress{height:9px;background:#eeeefa;border-radius:20px;overflow:hidden;margin:12px 0}
.progress div{height:100%;border-radius:20px;background:var(--primary)}
.budget-item{margin:20px 0}
.budget-head{display:flex;justify-content:space-between;font-size:13px}
.pages{display:none}
.pages.active{display:block}
.empty{text-align:center;padding:30px;color:var(--muted)}
.delete{background:#fff0f1;color:var(--red);padding:7px}
@media(max-width:850px){
 .sidebar{width:190px}
 main{padding:20px}
 .cards{grid-template-columns:1fr}
}
@media(max-width:600px){
 .app{display:block}
 .sidebar{width:100%;padding:15px}
 .logo{margin:0 0 12px}
 .nav{display:flex;overflow-x:auto;gap:5px}
 .nav-title{display:none}
 .nav button{white-space:nowrap;width:auto}
 main{padding:15px}
 header{align-items:flex-start;gap:10px}
 h1{font-size:21px}
 .form-grid{grid-template-columns:1fr}
 .form-grid .full{grid-column:auto}
 .section{overflow-x:auto}
 table{min-width:520px}
}
</style>
</head>
<body>
<div class="app">
<aside class="sidebar">
 <div class="logo">✦ <span>Money</span> Manager</div>
 <div class="nav-title">MENU CHÍNH</div>
 <div class="nav">
  <button class="active" onclick="showPage('dashboard',this)">▦ Tổng quan</button>
  <button onclick="showPage('transactions',this)">↕ Thu chi</button>
  <button onclick="showPage('categories',this)">▤ Danh mục</button>
  <button onclick="showPage('budgets',this)">◉ Ngân sách</button>
 </div>
 <div class="nav-title">TÀI KHOẢN</div>
 <div class="nav">
  <button onclick="resetData()">↻ Làm mới dữ liệu</button>
 </div>
</aside>

<main>
<header>
 <div>
  <h1 id="pageTitle">Tổng quan tài chính</h1>
  <div class="subtitle">Quản lý tài chính cá nhân của bạn</div>
 </div>
 <div class="user">👤 Tài khoản cá nhân</div>
</header>

<!-- DASHBOARD -->
<div id="dashboard" class="pages active">
 <div class="cards">
  <div class="card">
   <div class="card-label">💰 Tổng số dư</div>
   <div class="card-value purple" id="balance">0 ₫</div>
   <div class="subtitle">Thu nhập - Chi tiêu</div>
  </div>
  <div class="card">
   <div class="card-label">↗ Tổng thu nhập</div>
   <div class="card-value green" id="income">0 ₫</div>
   <div class="subtitle">Tổng các khoản thu</div>
  </div>
  <div class="card">
   <div class="card-label">↘ Tổng chi tiêu</div>
   <div class="card-value red" id="expense">0 ₫</div>
   <div class="subtitle">Tổng các khoản chi</div>
  </div>
 </div>
 <div class="section">
  <div class="section-head">
   <h2>Giao dịch gần đây</h2>
   <button class="secondary" onclick="showPage('transactions')">Xem tất cả</button>
  </div>
  <div id="recent"></div>
 </div>
 <div class="section">
  <h2>Tình hình ngân sách</h2>
  <div id="dashboardBudgets"></div>
 </div>
</div>

<!-- TRANSACTIONS -->
<div id="transactions" class="pages">
 <div class="section">
  <div class="section-head"><h2>Thêm giao dịch mới</h2></div>
  <div class="form-grid">
   <select id="type" onchange="updateCategoryOptions()">
    <option value="expense">Chi tiêu</option>
    <option value="income">Thu nhập</option>
   </select>
   <input id="amount" type="number" min="1" placeholder="Số tiền (VNĐ)">
   <select id="category"></select>
   <input id="date" type="date">
   <input id="note" class="full" placeholder="Ghi chú giao dịch">
   <button class="primary full" onclick="addTransaction()">+ Thêm giao dịch</button>
  </div>
 </div>
 <div class="section">
  <div class="section-head">
   <h2>Lịch sử giao dịch</h2>
   <select id="filter" style="width:150px" onchange="render()">
    <option value="all">Tất cả</option>
    <option value="income">Thu nhập</option>
    <option value="expense">Chi tiêu</option>
   </select>
  </div>
  <div id="transactionList"></div>
 </div>
</div>

<!-- CATEGORIES -->
<div id="categories" class="pages">
 <div class="section">
  <h2>Thêm danh mục mới</h2>
  <div class="form-grid" style="margin-top:18px">
   <input id="newCategory" placeholder="Tên danh mục">
   <select id="newType">
    <option value="expense">Danh mục chi tiêu</option>
    <option value="income">Danh mục thu nhập</option>
   </select>
   <button class="primary full" onclick="addCategory()">+ Tạo danh mục</button>
  </div>
 </div>
 <div class="section">
  <h2>Danh sách danh mục</h2>
  <div id="categoryList"></div>
 </div>
</div>

<!-- BUDGETS -->
<div id="budgets" class="pages">
 <div class="section">
  <h2>Thiết lập ngân sách</h2>
  <div class="form-grid" style="margin-top:18px">
   <select id="budgetCategory"></select>
   <input id="budgetAmount" type="number" min="1" placeholder="Hạn mức (VNĐ)">
   <button class="primary full" onclick="addBudget()">+ Thiết lập ngân sách</button>
  </div>
 </div>
 <div class="section">
  <h2>Ngân sách của tôi</h2>
  <div id="budgetList"></div>
 </div>
</div>
</main>
</div>

<script>
const defaultCategories = [
 {name:"Ăn uống",type:"expense"},
 {name:"Mua sắm",type:"expense"},
 {name:"Học tập",type:"expense"},
 {name:"Đi lại",type:"expense"},
 {name:"Hóa đơn",type:"expense"},
 {name:"Giải trí",type:"expense"},
 {name:"Sức khỏe",type:"expense"},
 {name:"Khác",type:"expense"},
 {name:"Lương",type:"income"},
 {name:"Thưởng",type:"income"},
 {name:"Thu nhập khác",type:"income"}
];

let transactions = JSON.parse(localStorage.getItem("mm_transactions") || "[]");
let categories = JSON.parse(localStorage.getItem("mm_categories") || "null") || defaultCategories;
let budgets = JSON.parse(localStorage.getItem("mm_budgets") || "[]");

const money = n => new Intl.NumberFormat("vi-VN").format(n)+" ₫";
const today = new Date().toISOString().slice(0,10);
document.getElementById("date").value=today;

function save(){
 localStorage.setItem("mm_transactions",JSON.stringify(transactions));
 localStorage.setItem("mm_categories",JSON.stringify(categories));
 localStorage.setItem("mm_budgets",JSON.stringify(budgets));
}
function showPage(id,btn){
 document.querySelectorAll(".pages").forEach(p=>p.classList.remove("active"));
 document.getElementById(id).classList.add("active");
 document.querySelectorAll(".nav button").forEach(b=>b.classList.remove("active"));
 if(btn)btn.classList.add("active");
 document.getElementById("pageTitle").textContent={
 dashboard:"Tổng quan tài chính",
 transactions:"Quản lý thu chi",
 categories:"Danh mục thanh toán",
 budgets:"Quản lý ngân sách"
 }[id];
 render();
}
function updateCategoryOptions(){
 let type=document.getElementById("type").value;
 let options=categories.filter(c=>c.type===type);
 document.getElementById("category").innerHTML=options.map(c=>
 `<option value="${safe(c.name)}">${safe(c.name)}</option>`).join("");
}
function safe(s){
 return String(s).replace(/[&<>"']/g,c=>({
 "&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"
 })[c]);
}
function addTransaction(){
 let type=document.getElementById("type").value;
 let amount=Number(document.getElementById("amount").value);
 let category=document.getElementById("category").value;
 let date=document.getElementById("date").value;
 let note=document.getElementById("note").value.trim();
 if(!amount||amount<=0||!category||!date)return alert("Vui lòng nhập đầy đủ thông tin hợp lệ!");
 transactions.unshift({id:Date.now(),type,amount,category,date,note});
 document.getElementById("amount").value="";
 document.getElementById("note").value="";
 save();render();alert("Đã thêm giao dịch!");
}
function addCategory(){
 let name=document.getElementById("newCategory").value.trim();
 let type=document.getElementById("newType").value;
 if(!name)return alert("Nhập tên danh mục!");
 if(categories.some(c=>c.name.toLowerCase()===name.toLowerCase()&&c.type===type))
 return alert("Danh mục đã tồn tại!");
 categories.push({name,type});
 document.getElementById("newCategory").value="";
 save();render();
}
function deleteCategory(name,type){
 if(transactions.some(t=>t.category===name)||budgets.some(b=>b.category===name))
 return alert("Danh mục đang được sử dụng, không thể xóa!");
 if(confirm("Xóa danh mục "+name+"?")){
 categories=categories.filter(c=>!(c.name===name&&c.type===type));
 save();render();
 }
}
function addBudget(){
 let category=document.getElementById("budgetCategory").value;
 let amount=Number(document.getElementById("budgetAmount").value);
 if(!category||amount<=0)return alert("Nhập danh mục và hạn mức hợp lệ!");
 let old=budgets.find(b=>b.category===category);
 if(old)old.amount=amount;
 else budgets.push({category,amount});
 document.getElementById("budgetAmount").value="";
 save();render();
}
function deleteTransaction(id){
 if(confirm("Xóa giao dịch này?")){
 transactions=transactions.filter(t=>t.id!==id);
 save();render();
 }
}
function deleteBudget(category){
 budgets=budgets.filter(b=>b.category!==category);
 save();render();
}
function totals(){
 let income=transactions.filter(t=>t.type==="income").reduce((s,t)=>s+t.amount,0);
 let expense=transactions.filter(t=>t.type==="expense").reduce((s,t)=>s+t.amount,0);
 return {income,expense,balance:income-expense};
}
function budgetHTML(b){
 let spent=transactions.filter(t=>t.type==="expense"&&t.category===b.category)
 .reduce((s,t)=>s+t.amount,0);
 let percent=Math.round(spent/b.amount*100);
 let width=Math.min(percent,100);
 return `<div class="budget-item">
 <div class="budget-head"><strong>${safe(b.category)}</strong>
 <span>${money(spent)} / ${money(b.amount)}</span></div>
 <div class="progress"><div style="width:${width}%;background:${percent>=100?'#ed6478':percent>=80?'#edaa45':'#6558e8'}"></div></div>
 <div class="budget-head"><span>${percent>=100?'⚠ Đã vượt ngân sách':percent>=80?'Sắp đạt giới hạn':'Đang sử dụng'}</span><b>${percent}%</b></div>
 </div>`;
}
function render(){
 let t=totals();
 document.getElementById("income").textContent=money(t.income);
 document.getElementById("expense").textContent=money(t.expense);
 document.getElementById("balance").textContent=money(t.balance);

 let rows=(arr)=>arr.length?`<table><thead><tr>
 <th>Danh mục</th><th>Ngày</th><th>Ghi chú</th><th>Số tiền</th><th></th>
 </tr></thead><tbody>${arr.map(x=>`<tr>
 <td><span class="tag">${safe(x.category)}</span></td>
 <td>${safe(x.date)}</td><td>${safe(x.note||"—")}</td>
 <td class="${x.type==='income'?'green':'red'}">${x.type==='income'?'+':'-'}${money(x.amount)}</td>
 <td><button class="delete" onclick="deleteTransaction(${x.id})">Xóa</button></td>
 </tr>`).join("")}</tbody></table>`:
 `<div class="empty">Chưa có giao dịch nào.</div>`;

 document.getElementById("recent").innerHTML=rows(transactions.slice(0,5));
 let filter=document.getElementById("filter").value;
 document.getElementById("transactionList").innerHTML=rows(
 transactions.filter(x=>filter==="all"||x.type===filter)
 );
 document.getElementById("category").innerHTML=categories
 .filter(c=>c.type===document.getElementById("type").value)
 .map(c=>`<option value="${safe(c.name)}">${safe(c.name)}</option>`).join("");

 document.getElementById("categoryList").innerHTML=categories.map(c=>
 `<div class="budget-item budget-head">
 <span><strong>${safe(c.name)}</strong> · ${c.type==="income"?"Thu nhập":"Chi tiêu"}</span>
 <button class="delete" onclick="deleteCategory('${safe(c.name)}','${c.type}')">Xóa</button>
 </div>`).join("");

 let opts=categories.filter(c=>c.type==="expense");
 document.getElementById("budgetCategory").innerHTML=opts.map(c=>
 `<option value="${safe(c.name)}">${safe(c.name)}</option>`).join("");

 document.getElementById("budgetList").innerHTML=budgets.length?
 budgets.map(b=>budgetHTML(b)+
 `<button class="delete" onclick="deleteBudget('${safe(b.category)}')">Xóa ngân sách</button>`
 ).join(""):'<div class="empty">Chưa thiết lập ngân sách.</div>';

 document.getElementById("dashboardBudgets").innerHTML=budgets.length?
 budgets.map(budgetHTML).join(""):'<div class="empty">Hãy thiết lập ngân sách để theo dõi chi tiêu.</div>';
}
function resetData(){
 if(confirm("Xóa toàn bộ dữ liệu quản lý tiền trên trình duyệt?")){
 transactions=[];categories=defaultCategories;budgets=[];
 save();render();
 }
}
updateCategoryOptions();
render();
</script>
</body>
</html>
