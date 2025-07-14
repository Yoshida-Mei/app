<h1>冷蔵庫の中管理</h1>

<input type="text" id="itemName" placeholder="食材の名前">
<input type="date" id="expiryDate">
<button onclick="addItem()">追加</button>

<ul id="itemList"></ul>
body {
  font-family: sans-serif;
  background-color: #f6f9fc;
  padding: 20px;
  max-width: 600px;
  margin: auto;
}
h1 {
  text-align: center;
  color: #333;
}
input, button {
  padding: 10px;
  margin: 5px 0;
  font-size: 16px;
  width: 100%;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  background: #fff;
  margin: 8px 0;
  padding: 10px;
  border-left: 6px solid #4CAF50;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
.danger {
  border-left-color: #e53935;
  background: #ffeaea;
}
.delete-btn {
  float: right;
  background: #e53935;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}
let items = JSON.parse(localStorage.getItem("fridgeItems")) || [];

function saveItems() {
  localStorage.setItem("fridgeItems", JSON.stringify(items));
}

function renderItems() {
  const list = document.getElementById("itemList");
  list.innerHTML = "";
  const today = new Date();

  items.forEach((item, index) => {
    const li = document.createElement("li");
    const expiry = new Date(item.expiry);
    const daysLeft = (expiry - today) / (1000 * 60 * 60 * 24);

    if (daysLeft < 3) {
      li.classList.add("danger");
    }

    li.innerHTML = `
      ${item.name}（賞味期限: ${item.expiry}）
      <button class="delete-btn" onclick="deleteItem(${index})">削除</button>
    `;
    list.appendChild(li);
  });
}

function addItem() {
  const name = document.getElementById("itemName").value.trim();
  const expiry = document.getElementById("expiryDate").value;

  if (!name || !expiry) {
    alert("名前と賞味期限を入力してください");
    return;
  }

  items.push({ name, expiry });
  saveItems();
  renderItems();

  document.getElementById("itemName").value = "";
  document.getElementById("expiryDate").value = "";
}

function deleteItem(index) {
  items.splice(index, 1);
  saveItems();
  renderItems();
}

renderItems();
