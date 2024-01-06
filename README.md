<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shopping Website</title>
    <link rel="stylesheet" href="styles.css">
    <style>
        /* Add your custom styles here */
        body {
            font-family: 'Helvetica Neue', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f4;
            color: #333;
        }

        header {
            background-color: #232f3e;
            color: white;
            text-align: center;
            padding: 15px;
        }

        nav {
            background-color: #333;
            overflow: hidden;
        }

        nav a {
            float: left;
            display: block;
            color: white;
            text-align: center;
            padding: 14px 16px;
            text-decoration: none;
        }

        .search-bar {
            width: 100%;
            padding: 10px;
            margin: 10px 0;
            box-sizing: border-box;
            display: flex;
            justify-content: space-between;
        }

        .search-bar input {
            flex: 1;
            padding: 8px;
            margin-right: 10px;
            border: 1px solid #ccc;
        }

        .search-bar button {
            background-color: #f0c14b;
            border: 1px solid #a88734;
            padding: 8px 15px;
            color: #111;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .search-bar button:hover {
            background-color: #d4ac2b;
        }

        section {
            padding: 20px;
            display: flex;
            flex-wrap: wrap;
            justify-content: space-around;
        }

        .product {
            border: 1px solid #ddd;
            padding: 15px;
            margin: 10px;
            background-color: white;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            width: 300px;
            transition: transform 0.2s;
            cursor: pointer;
        }

        .product:hover {
            transform: scale(1.05);
        }

        .product img {
            width: 100%;
            height: 200px;
            object-fit: cover;
            margin-bottom: 10px;
        }

        .product h2 {
            font-size: 1.5em;
            margin-bottom: 10px;
        }

        .product p {
            margin-bottom: 10px;
            color: #777;
        }

        .product button {
            background-color: #f0c14b;
            border: 1px solid #a88734;
            padding: 8px 15px;
            color: #111;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .product button:hover {
            background-color: #d4ac2b;
        }

        .product-details {
            padding: 20px;
            max-width: 800px;
            margin: 0 auto;
        }

        .modal {
            display: none;
            position: fixed;
            z-index: 1;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            overflow: auto;
            background-color: rgba(0, 0, 0, 0.8);
        }

        .modal-content {
            position: relative;
            background-color: #fefefe;
            margin: 5% auto;
            padding: 20px;
            border: 1px solid #888;
            width: 80%;
        }

        .close {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 20px;
            cursor: pointer;
        }

        .shopping-cart {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background-color: #232f3e;
            color: white;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
        }

        footer {
            text-align: center;
            padding: 10px;
            background-color: #232f3e;
            color: white;
        }

        @media (max-width: 768px) {
            .product {
                width: calc(50% - 20px);
            }
        }

        @media (max-width: 480px) {
            .product {
                width: 100%;
            }
        }
    </style>
</head>

<body>
    <header>
        <h1>Shopping Website</h1>
    </header>

    <nav>
        <a href="#">Home</a>
        <a href="#">Products</a>
        <a href="#">Contact</a>
    </nav>

    <div class="search-bar">
        <input type="text" placeholder="Search for products...">
        <button>Search</button>
    </div>

    <section>
        <div class="product" onclick="showProductDetails('Product 1', 'Description of Product 1. Lorem ipsum dolor sit amet, consectetur adipiscing elit.', 19.99)">
            <img src="product1.jpg" alt="Product 1 Image">
            <h2>Product 1</h2>
            <p>Description of Product 1. Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
            <p>Price: $19.99</p>
            <button onclick="addToCart('Product 1', 19.99)">Add to Cart</button>
        </div>

        <div class="product" onclick="showProductDetails('Product 2', 'Description of Product 2. Lorem ipsum dolor sit amet, consectetur adipiscing elit.', 29.99)">
            <img src="product2.jpg" alt="Product 2 Image">
            <h2>Product 2</h2>
            <p>Description of Product 2. Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
            <p>Price: $29.99</p>
            <button onclick="addToCart('Product 2', 29.99)">Add to Cart</button>
        </div>

        <!-- Add more product sections as needed -->

    </section>

    <div class="shopping-cart" onclick="showShoppingCart()">
        🛒 View Cart
    </div>

    <div id="productDetailsModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <div class="product-details" id="productDetails">
                <!-- Product details will be filled here dynamically -->
            </div>
        </div>
    </div>

    <div id="shoppingCartModal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeModal()">&times;</span>
            <h2>Shopping Cart</h2>
            <div id="cartItems">
                <!-- Cart items will be filled here dynamically -->
            </div>
            <button onclick="checkout()">Checkout</button>
        </div>
    </div>

    <footer>
        <p>&copy; 2024 Shopping Website. All rights reserved.</p>
    </footer>

    <script>
        // Mock product data
        const products = {
            'Product 1': {
                description: 'Description of Product 1. Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
                price: 19.99
            },
            'Product 2': {
                description: 'Description of Product 2. Lorem ipsum dolor sit amet, consectetur adipiscing elit.',
                price: 29.99
            }
            // Add more products as needed
        };

        let cart = [];

        function showProductDetails(productName, description, price) {
            const productDetailsModal = document.getElementById('productDetailsModal');
            const productDetails = document.getElementById('productDetails');
            productDetails.innerHTML = `
                <h2>${productName}</h2>
                <p>${description}</p>
                <p>Price: $${price.toFixed(2)}</p>
                <button onclick="addToCart('${productName}', ${price})">Add to Cart</button>
            `;
            productDetailsModal.style.display = 'block';
        }

        function showShoppingCart() {
            const shoppingCartModal = document.getElementById('shoppingCartModal');
            const cartItems = document.getElementById('cartItems');
            cartItems.innerHTML = '';
            if (cart.length === 0) {
                cartItems.innerHTML = '<p>Your shopping cart is empty.</p>';
            } else {
                cart.forEach(item => {
                    cartItems.innerHTML += `
                        <div>${item.name} - $${item.price.toFixed(2)}</div>
                    `;
                });
            }
            shoppingCartModal.style.display = 'block';
        }

        function closeModal() {
            const modals = document.getElementsByClassName('modal');
            Array.from(modals).forEach(modal => {
                modal.style.display = 'none';
            });
        }

        function addToCart(productName, price) {
            cart.push({ name: productName, price: price });
            alert(`Added ${productName} to cart! Price: $${price.toFixed(2)}`);
        }

        function checkout() {
            alert('Checkout functionality will be implemented in the real website.');
        }

        window.onclick = function (event) {
            const modals = document.getElementsByClassName('modal');
            Array.from(modals).forEach(modal => {
                if (event.target === modal) {
                    modal.style.display = 'none';
                }
            });
        };
    </script>
</body>

</html>
