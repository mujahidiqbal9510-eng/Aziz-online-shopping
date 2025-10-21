<!DOCTYPE html>
<html lang="ur" dir="rtl">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Awan Shopping</title>
<style>
  body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background: linear-gradient(135deg, #74ebd5 0%, #ACB6E5 100%);
    margin: 0;
    color: #333;
    padding: 20px;
  }
  .container {
    max-width: 1100px;
    margin: auto;
    background: white;
    border-radius: 15px;
    padding: 25px 35px;
    box-shadow: 0 8px 24px rgba(0, 0, 50, 0.15);
  }
  h1 {
    text-align: center;
    color: #2c3e50;
    margin-bottom: 25px;
  }
  #products {
    display: grid;
    grid-template-columns: repeat(auto-fill,minmax(250px,1fr));
    gap: 25px;
  }
  .product-card {
    border: 1.8px solid #74a0c7;
    border-radius: 15px;
    padding: 20px;
    text-align: center;
    background: #f0f7ff;
    box-shadow: 0 4px 20px rgba(0, 40, 100, 0.05);
    position: relative;
    transition: transform 0.3s ease;
  }
  .product-card:hover {
    transform: translateY(-8px);
    box-shadow: 0 8px 30px rgba(0, 40, 100, 0.15);
  }
  .product-card img {
    width: 100%;
    height: 160px;
    object-fit: cover;
    border-radius: 10px;
    margin-bottom: 12px;
  }
  .product-name {
    font-weight: bold;
    margin: 10px 0 6px;
    font-size: 1.2rem;
    color: #274690;
  }
  .product-price {
    color: #008dd5;
    font-weight: bold;
    margin: 10px 0 18px;
    font-size: 1.15rem;
  }
  button {
    background: #008dd5;
    color: white;
    border: none;
    padding: 12px 20px;
    border-radius: 28px;
    cursor: pointer;
    font-weight: 700;
    font-size: 1rem;
    transition: background 0.3s ease;
  }
  button:hover {
    background: #005f86;
  }
  .order-form-container {
    display: none;
    background: white;
    border-radius: 14px;
    box-shadow: 0 8px 18px rgba(0,0,0,0.15);
    padding: 18px;
    position: absolute;
    top: 12px; left: 12px; right: 12px;
    z-index: 10;
  }
  .order-form-container input {
    width: 100%;
    margin-bottom: 12px;
    padding: 12px;
    border-radius: 10px;
    border: 1px solid #ccc;
  }
  .order-form-container button.submit-btn {
    background-color: #28a745;
  }
  .order-form-container button.cancel-btn {
    background-color: #dc3545;
    margin-top: 8px;
  }
  .admin-section {
    max-width: 450px; margin: 40px auto 0;
    background: #2c3e50; color: white; padding: 22px;
    border-radius: 18px;
  }
  input, textarea {
    width: 100%; margin: 10px 0; padding: 14px;
    border-radius: 10px; border: none;
    font-size: 1rem;
  }
  input[type="file"] {
    padding: 6px;
  }
  #adminPanel {
    display: none;
  }
  .admin-btn {
    width: 100%;
    background: #008dd5;
    padding: 14px;
    border-radius: 28px;
    font-weight: bold;
    font-size: 1.1rem;
    border: none;
    color: white;
    cursor: pointer;
  }
  .admin-btn:hover {
    background: #005f86;
  }
  .edit-delete-btn {
    margin-top: 10px;
    background: #e74c3c;
    border: none;
    color: white;
    padding: 9px 15px;
    border-radius: 14px;
    cursor: pointer;
    font-size: 0.9rem;
  }
  .edit-btn {
    background: #4caf50;
    margin-left: 10px;
  }
</style>
</head>
<body>
<div class="container">
  <h1>Aziz online 🛒 center</h1>
  <div id="products"></div>

  <div id="adminLogin" class="admin-section">
    <h3>ایڈمن لاگ ان کریں</h3>
    <input type="text" id="user" placeholder="یوزر نیم" />
    <input type="password" id="pass" placeholder="پاس ورڈ" />
    <button onclick="loginAdmin()" class="admin-btn">لاگ ان کریں</button>
  </div>

  <div id="adminPanel" class="admin-section">
    <h3>نیا پروڈکٹ شامل کریں یا ترمیم کریں</h3>
    <input type="text" id="pname" placeholder="پروڈکٹ کا نام" />
    <textarea id="pdetails" placeholder="پروڈکٹ کی تفصیل"></textarea>
    <input type="number" id="pprice" placeholder="قیمت (روپے)" />
    <input type="file" id="pimage" accept="image/*" />
    <button class="admin-btn" onclick="addOrSaveProduct()">پروڈکٹ شامل کریں</button>
  </div>
</div>

<script>
let products = JSON.parse(localStorage.getItem('products')) || [
  {
    name: "سمارٹ فون",
    details: "جدید اینڈرائیڈ فون بہترین کیمرہ کے ساتھ",
    price: 25000,
    image: "https://via.placeholder.com/300x160?text=Smartphone"
  },
  {
    name: "لیپ ٹاپ",
    details: "کاروبار اور گیمنگ کے لیے اعلیٰ کارکردگی",
    price: 45000,
    image: "https://via.placeholder.com/300x160?text=Laptop"
  },
  {
    name: "ہیڈ فون",
    details: "بلوٹوتھ وائرلیس ہیڈ فون شور کو ختم کرنے کے ساتھ",
    price: 3000,
    image: "https://via.placeholder.com/300x160?text=Headphones"
  },
  {
    name: "اسمارٹ واچ",
    details: "فٹنس ٹریکنگ کے ساتھ جدید اسمارٹ واچ",
    price: 8000,
    image: "https://via.placeholder.com/300x160?text=Smart+Watch"
  },
  {
    name: "کیمرا",
    details: "زیادہ میگا پکسلز والا ڈیجیٹل کیمرا",
    price: 15000,
    image: "https://via.placeholder.com/300x160?text=Camera"
  }
];

let editIndex = -1;

function displayProducts() {
  const container = document.getElementById('products');
  if(products.length === 0) {
    container.innerHTML = '<p style="text-align:center;">کوئی پروڈکٹ دستیاب نہیں ہے۔</p>';
    return;
  }
  container.innerHTML = products.map((p,i) => `
    <div class="product-card" id="product-${i}">
      ${p.image ? `<img src="${p.image}" alt="${p.name}">` : ''}
      <div class="product-name">${p.name}</div>
      <div class="product-price">Rs ${p.price}</div>
      <div>${p.details}</div>
      <button onclick="showOrderForm(${i})">کارٹ میں ڈالیں</button>

      <div class="order-form-container" id="order-form-${i}">
        <input type="text" id="order-name-${i}" placeholder="اپنا نام درج کریں" required />
        <input type="tel" id="order-mobile-${i}" placeholder="موبائل نمبر درج کریں" pattern='[0-9]{10,15}' required />
        <input type="text" id="order-city-${i}" placeholder="شہر" required />
        <input type="text" id="order-area-${i}" placeholder="علاقہ" required />
        <input type="text" id="order-landmark-${i}" placeholder="مشہور جگہ کا نام" required />
        <button class="submit-btn" onclick="sendOrder(${i})">آرڈر بھیجیں</button>
        <button class="cancel-btn" onclick="hideOrderForm(${i})">منسوخ کریں</button>
      </div>

      ${document.getElementById('adminPanel').style.display === 'block' ? `
        <button class="edit-delete-btn edit-btn" onclick="editProduct(${i})">ترمیم کریں</button>
        <button class="edit-delete-btn" onclick="deleteProduct(${i})">حذف کریں</button>
      ` : ''}
    </div>
  `).join('');
}

function showOrderForm(i) {
  const form = document.getElementById(`order-form-${i}`);
  form.style.display = 'block';
}
function hideOrderForm(i) {
  const form = document.getElementById(`order-form-${i}`);
  form.style.display = 'none';
}

function sendOrder(i) {
  const name = document.getElementById(`order-name-${i}`).value.trim();
  const mobile = document.getElementById(`order-mobile-${i}`).value.trim();
  const city = document.getElementById(`order-city-${i}`).value.trim();
  const area = document.getElementById(`order-area-${i}`).value.trim();
  const landmark = document.getElementById(`order-landmark-${i}`).value.trim();

  if(!name || !mobile || !city || !area || !landmark) {
    alert('براہ کرم تمام معلومات مکمل کریں۔');
    return;
  }

  const product = products[i];
  const orderMessage = 
    `*نیا آرڈر - اوان شاپنگ*%0A%0A` +
    `کسٹمر: ${encodeURIComponent(name)}%0A` +
    `موبائل: ${encodeURIComponent(mobile)}%0A` +
    `شہر: ${encodeURIComponent(city)}%0A` +
    `علاقہ: ${encodeURIComponent(area)}%0A` +
    `مشہور جگہ: ${encodeURIComponent(landmark)}%0A%0A` +
    `*پروڈکٹ:* ${encodeURIComponent(product.name)}%0A` +
    `قیمت: Rs ${encodeURIComponent(product.price)}`;

  const whatsappNumber = '923424737950'; // اپنا واٹس ایپ نمبر یہاں ڈالیں
  const whatsappURL = `https://wa.me/${whatsappNumber}?text=${orderMessage}`;

  window.open(whatsappURL, '_blank');
  alert(`${name}، شکریہ! آپ کا آرڈر بھیج دیا گیا ہے۔`);
  hideOrderForm(i);
  clearOrderForm(i);
}

function clearOrderForm(i) {
  document.getElementById(`order-name-${i}`).value = '';
  document.getElementById(`order-mobile-${i}`).value = '';
  document.getElementById(`order-city-${i}`).value = '';
  document.getElementById(`order-area-${i}`).value = '';
  document.getElementById(`order-landmark-${i}`).value = '';
}

// ایڈمن لاگ ان اور پروڈکٹ مینجمنٹ

function loginAdmin() {
  const username = document.getElementById('user').value;
  const password = document.getElementById('pass').value;
  if(username === 'Azizkhan' && password === 'Azizkhan123') {
    alert('لاگ ان کامیاب ہوگیا!');
    document.getElementById('adminLogin').style.display = 'none';
    document.getElementById('adminPanel').style.display = 'block';
    clearAdminForm();
    displayProducts();
  } else {
    alert('یوزر نیم یا پاس ورڈ غلط ہے۔');
  }
}

function addOrSaveProduct() {
  const name = document.getElementById('pname').value.trim();
  const details = document.getElementById('pdetails').value.trim();
  const price = +document.getElementById('pprice').value;
  const fileInput = document.getElementById('pimage');

  if(!name || !price) {
    alert('پروڈکٹ کا نام اور قیمت ضروری ہے۔');
    return;
  }

  if(fileInput.files && fileInput.files[0]) {
    const reader = new FileReader();
    reader.onload = function(e) {
      saveProductData(name, details, price, e.target.result);
    };
    reader.readAsDataURL(fileInput.files[0]);
  } else {
    let existingImage = editIndex >=0 ? products[editIndex].image : '';
    saveProductData(name, details, price, existingImage);
  }
}

function saveProductData(name, details, price, image) {
  if(editIndex >= 0) {
    products[editIndex] = {name, details, price, image};
    alert('پروڈکٹ کو اپڈیٹ کر دیا گیا۔');
  } else {
    products.push({name, details, price, image});
    alert('نیا پروڈکٹ شامل کر دیا گیا۔');
  }
  localStorage.setItem('products', JSON.stringify(products));
  editIndex = -1;
  clearAdminForm();
  displayProducts();
  document.querySelector('.admin-btn').textContent = 'پروڈکٹ شامل کریں';
}

function clearAdminForm() {
  document.getElementById('pname').value = '';
  document.getElementById('pdetails').value = '';
  document.getElementById('pprice').value = '';
  document.getElementById('pimage').value = '';
}

function editProduct(i) {
  editIndex = i;
  let product = products[i];
  document.getElementById('pname').value = product.name;
  document.getElementById('pdetails').value = product.details;
  document.getElementById('pprice').value = product.price;
  document.getElementById('pimage').value = '';
  document.querySelector('.admin-btn').textContent = 'تبدیلی محفوظ کریں';
  window.scrollTo(0, document.body.scrollHeight);
}

function deleteProduct(i) {
  if(confirm('کیا آپ واقعی اس پروڈکٹ کو حذف کرنا چاہتے ہیں؟')) {
    products.splice(i, 1);
    localStorage.setItem('products', JSON.stringify(products));
    displayProducts();
  }
}

displayProducts();
</script>
</body>
</html>
