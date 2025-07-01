<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>DigitallyFame WhatsApp Store</title>
  <style>
    body { font-family: Arial, sans-serif; padding: 10px; margin: 0; background: #f4f4f4; }
    h1 { text-align: center; }
    #search-bar { width: 100%; padding: 10px; margin-bottom: 10px; }
    .category-buttons { text-align: center; margin-bottom: 10px; }
    .category-buttons button { margin: 5px; padding: 5px 10px; border: none; background: #ddd; border-radius: 5px; cursor: pointer; }
    .product-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(160px, 1fr)); gap: 15px; }
    .product { background: white; padding: 10px; border-radius: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); text-align: center; }
    .product img { max-width: 100%; height: 100px; object-fit: contain; }
    .product h3 { font-size: 16px; margin: 10px 0 5px; }
    .product button { background: green; color: white; padding: 5px 10px; border: none; border-radius: 5px; cursor: pointer; }
    .cart { position: fixed; bottom: 10px; left: 0; right: 0; text-align: center; }
    .cart button { background: #25d366; color: white; font-size: 18px; padding: 10px 20px; border: none; border-radius: 10px; box-shadow: 0 2px 8px rgba(0,0,0,0.2); }
    #backToTop { position: fixed; bottom: 70px; right: 10px; display: none; background: #333; color: white; border: none; border-radius: 50%; padding: 10px; cursor: pointer; }
  </style>
</head>
<body>
  <h1>DigitallyFame WhatsApp Store</h1>

  <!-- 🔎 Search Bar -->
  <input type="text" id="search-bar" placeholder="Search products...">

  <!-- 🗂️ Category Filters -->
  <div class="category-buttons">
    <button onclick="filterCategory('All')">All</button>
    <button onclick="filterCategory('Games')">Games</button>
    <button onclick="filterCategory('Software')">Software</button>
    <button onclick="filterCategory('OTP')">OTP</button>
  </div>

  <!-- 🛒 Products -->
  <div class="product-grid" id="product-grid"></div>

  <!-- 🛒 Cart -->
  <div class="cart">
    <button onclick="checkout()">🛒 Place Order on WhatsApp (<span id="cart-count">0</span>)</button>
  </div>

  <!-- 🔝 Back to Top Button -->
  <button id="backToTop" onclick="topFunction()">↑</button>

  <script>
    const products = [
      {name: "GTA V", description: "Original game for PC", price: 500, old_price: 800, image: "https://linktoimage.jpg", category: "Games"},
      {name: "Windows 10 Pro Key", description: "Original license key", price: 200, old_price: 500, image: "https://linktoimage.jpg", category: "Software"},
      {name: "Virtual Number OTP", description: "India/US numbers", price: 50, old_price: 100, image: "https://linktoimage.jpg", category: "OTP"}
    ];

    const cart = {};

    function renderProducts(filter="", category="All") {
      const grid = document.getElementById('product-grid');
      grid.innerHTML = "";
      products.forEach((p, idx) => {
        if ((p.name.toLowerCase().includes(filter.toLowerCase()) || p.description.toLowerCase().includes(filter.toLowerCase())) && (category==="All" || p.category===category)) {
          const div = document.createElement('div');
          div.className = 'product';
          div.innerHTML = `
            <img src="${p.image}" alt="${p.name}" />
            <h3>${p.name}</h3>
            <p><del>₹${p.old_price}</del> <strong>₹${p.price}</strong></p>
            <p>${p.description}</p>
            <button onclick="addToCart(${idx})">Add to Cart</button>
          `;
          grid.appendChild(div);
        }
      });
    }

    function addToCart(index) {
      if (!cart[index]) cart[index] = 0;
      cart[index]++;
      updateCartCount();
      alert(`${products[index].name} added to cart!`);
    }

    function updateCartCount() {
      const count = Object.values(cart).reduce((a,b)=>a+b,0);
      document.getElementById('cart-count').innerText = count;
    }

    function checkout() {
      const items = Object.keys(cart).map(i => {
        const p = products[i];
        return `- ${p.name} x${cart[i]} – ₹${p.price * cart[i]}`;
      });
      const total = Object.keys(cart).reduce((sum, i) => sum + products[i].price * cart[i], 0);
      const message = `Hi, I want to order:\n${items.join('\n')}\nTotal: ₹${total}\nPincode: 364710`;
      const url = `https://wa.me/917623090169?text=${encodeURIComponent(message)}`;
      window.open(url, '_blank');
    }

    document.getElementById('search-bar').addEventListener('input', (e) => {
      renderProducts(e.target.value);
    });

    function filterCategory(cat) {
      renderProducts(document.getElementById('search-bar').value, cat);
    }

    // 🔝 Back to Top Function
    const backToTop = document.getElementById("backToTop");
    window.onscroll = function() {
      if (document.body.scrollTop > 200 || document.documentElement.scrollTop > 200) {
        backToTop.style.display = "block";
      } else {
        backToTop.style.display = "none";
      }
    };
    function topFunction() {
      document.body.scrollTop = 0;
      document.documentElement.scrollTop = 0;
    }

    renderProducts();
  </script>
</body>
</html>
