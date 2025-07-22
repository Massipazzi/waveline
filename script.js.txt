const products = [
  {
    id: 1,
    name: "Leash Surf 6mm",
    description: "Resistente e flessibile. Ideale per onde medie.",
    price: 24,
    img: "images/leash.jpg"
  },
  {
    id: 2,
    name: "Poncho da Surf",
    description: "Morbido e assorbente. Ideale dopo ogni sessione.",
    price: 38,
    img: "images/poncho.jpg"
  },
  {
    id: 3,
    name: "Mutandina Surf",
    description: "Comfort e sicurezza sotto la muta.",
    price: 16,
    img: "images/mutandina.jpg"
  },
  {
    id: 4,
    name: "Muta Surf 3/2mm",
    description: "Muta in neoprene per acque fresche, ottima elasticità.",
    price: 120,
    img: "images/muta.jpg"
  },
  {
    id: 5,
    name: "Wax Surf Board",
    description: "Cera per tavola, ottima aderenza in acqua.",
    price: 8,
    img: "images/wax.jpg"
  },
  {
    id: 6,
    name: "Sacca Tavola Surf",
    description: "Proteggi la tua tavola durante i trasporti.",
    price: 45,
    img: "images/sacca.jpg"
  }
];

let cart = [];

function showPage(pageId) {
  document.querySelectorAll('main section').forEach(s => s.style.display = 'none');
  document.getElementById(pageId).style.display = 'block';
}

function renderProducts() {
  const container = document.getElementById('products-list');
  container.innerHTML = '';
  products.forEach(p => {
    const div = document.createElement('div');
    div.className = 'product-card';
    div.innerHTML = `
      <img src="${p.img}" alt="${p.name}" />
      <h3>${p.name}</h3>
      <p>${p.description}</p>
      <div class="price">€${p.price.toFixed(2)}</div>
      <button onclick="addToCart(${p.id})">Aggiungi al carrello</button>
    `;
    container.appendChild(div);
  });
}

function addToCart(id) {
  const prod = products.find(p => p.id === id);
  if (!prod) return;
  cart.push(prod);
  updateCartCount();
  alert(`${prod.name} aggiunto al carrello!`);
}

function updateCartCount() {
  document.getElementById('cart-count').textContent = cart.length;
}

function showCart() {
  if (cart.length === 0) {
    alert("Il carrello è vuoto!");
    return;
  }
  showPage('cart');
  renderCart();
}

function renderCart() {
  const container = document.getElementById('cart-items');
  container.innerHTML = '';
  cart.forEach((item, i) => {
    const div = document.createElement('div');
    div.className = 'cart-item';
    div.innerHTML = `
      <span>${item.name} - €${item.price.toFixed(2)}</span>
      <button onclick="removeFromCart(${i})">X</button>
    `;
    container.appendChild(div);
  });
  const total = cart.reduce((sum, item) => sum + item.price, 0);
  document.getElementById('cart-total').textContent = `Totale: €${total.toFixed(2)}`;
}

function removeFromCart(index) {
  cart.splice(index, 1);
  updateCartCount();
  renderCart();
}

function closeCart() {
  showPage('home');
}

document.getElementById('checkout-btn').addEventListener('click', () => {
  if (cart.length === 0) {
    alert("Il carrello è vuoto!");
    return;
  }
  const total = cart.reduce((sum, item) => sum + item.price, 0);
  const paypalEmail = "massipazzi93@hotmail.it";
  // Link PayPal.Me con importo in Euro (arrotondato)
  const url = `https://www.paypal.me/${paypalEmail.split('@')[0]}/${Math.ceil(total)}`;
  window.open(url, "_blank");
});

// inizializza prodotti e pagina home
renderProducts();
showPage('home');
updateCartCount();
