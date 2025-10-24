// Dummy product data
const products = [
    { id: 1, name: 'Rose Elegance', description: 'A floral masterpiece.', notes: 'Rose, Jasmine', ingredients: 'Natural oils', price: 120, image: 'https://source.unsplash.com/featured/?rose,perfume', category: 'floral' },
    { id: 2, name: 'Woody Mystique', description: 'Earthy and deep.', notes: 'Sandalwood, Cedar', ingredients: 'Essential extracts', price: 150, image: 'https://source.unsplash.com/featured/?wood,perfume', category: 'woody' },
    { id: 3, name: 'Oriental Bliss', description: 'Warm and exotic.', notes: 'Vanilla, Amber', ingredients: 'Spice blends', price: 140, image: 'https://source.unsplash.com/featured/?oriental,perfume', category: 'oriental' },
    { id: 4, name: 'Fresh Breeze', description: 'Light and invigorating.', notes: 'Citrus, Mint', ingredients: 'Fruit essences', price: 100, image: 'https://source.unsplash.com/featured/?fresh,perfume', category: 'fresh' },
    // Add more as needed
];

// Cart functionality
let cart = JSON.parse(localStorage.getItem('cart')) || [];
function updateCartCount() {
    document.getElementById('cart-count').textContent = cart.length;
}
updateCartCount();

// Populate featured products (first 3)
const featuredContainer = document.getElementById('featured-products');
products.slice(0, 3).forEach(product => {
    const card = createProductCard(product);
    featuredContainer.appendChild(card);
});

// Populate shop products
const shopContainer = document.getElementById('shop-products');
function displayProducts(filter = 'all') {
    shopContainer.innerHTML = '';
    const filtered = filter === 'all' ? products : products.filter(p => p.category === filter);
    filtered.forEach(product => {
        const card = createProductCard(product);
        shopContainer.appendChild(card);
    });
}
displayProducts();

// Create product card
function createProductCard(product) {
    const card = document.createElement('div');
    card.className = 'product-card';
    card.innerHTML = `
        <img src="${product.image}" alt="${product.name}" loading="lazy">
        <h3>${product.name}</h3>
        <p>${product.description}</p>
        <p>$${product.price}</p>
        <button class="add-to-cart" data-id="${product.id}">Add to Cart</button>
    `;
    card.querySelector('.add-to-cart').addEventListener('click', () => addToCart(product.id));
    card.addEventListener('click', () => showProductDetail(product));
    return card;
}

// Add to cart
function addToCart(id) {
    const product = products.find(p => p.id === id);
    cart.push(product);
    localStorage.setItem('cart', JSON.stringify(cart));
    updateCartCount();
    alert(`${product.name} added to cart!`);
}

// Show product detail
function showProductDetail(product) {
    document.getElementById('detail-image').src = product.image;
    document.getElementById('detail-name').textContent = product.name;
    document.getElementById('detail-description').textContent = product.description;
    document.getElementById('detail-notes').textContent = product.notes;
    document.getElementById('detail-ingredients').textContent = product.ingredients;
    document.getElementById('detail-price').textContent = product.price;
    document.getElementById('product-detail').classList.remove('hidden');
    document.getElementById('shop').classList.add('hidden');
}

// Back to shop
document.getElementById('back-to-shop').addEventListener('click', () => {
    document.getElementById('product-detail').classList.add('hidden');
   
