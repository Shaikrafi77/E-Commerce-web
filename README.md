<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>My E-Commerce Store</title>
  <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
  <header>
    <h1>My E-Commerce Store</h1>
    <nav>
      <a href="/">Home</a>
      <a href="/cart">Cart</a>
    </nav>
  </header>

  <main>
    <div class="products">
      {% for product in products %}
        <div class="product-card">
          <img src="{{ product.image }}" alt="{{ product.name }}">
          <h2>{{ product.name }}</h2>
          <p>${{ product.price }}</p>
          <form action="/add_to_cart" method="post">
            <input type="hidden" name="product_id" value="{{ product.id }}">
            <button type="submit">Add to Cart</button>
          </form>
        </div>
      {% endfor %}
    </div>
  </main>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

header {
  background-color: #333;
  color: white;
  padding: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

nav a {
  color: white;
  margin-left: 15px;
  text-decoration: none;
}

.products {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 20px;
  padding: 20px;
}

.product-card {
  border: 1px solid #ddd;
  padding: 10px;
  text-align: center;
  border-radius: 10px;
  background-color: #f9f9f9;
}

.product-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}
from flask import Flask, render_template, request, redirect, url_for

app = Flask(__name__)

# Sample products (can be stored in DB later)
products = [
    {"id": 1, "name": "Laptop", "price": 800, "image": "/static/laptop.jpg"},
    {"id": 2, "name": "Headphones", "price": 150, "image": "/static/headphones.jpg"},
    {"id": 3, "name": "Camera", "price": 500, "image": "/static/camera.jpg"},
]

cart = []

@app.route("/")
def home():
    return render_template("index.html", products=products)

@app.route("/add_to_cart", methods=["POST"])
def add_to_cart():
    product_id = int(request.form.get("product_id"))
    for product in products:
        if product["id"] == product_id:
            cart.append(product)
            break
    return redirect(url_for("home"))

@app.route("/cart")
def view_cart():
    return render_template("cart.html", cart=cart)

if __name__ == "__main__":
    app.run(debug=True)
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Your Cart</title>
  <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
</head>
<body>
  <header>
    <h1>Your Cart</h1>
    <nav>
      <a href="/">Home</a>
    </nav>
  </header>

  <main>
    <ul>
      {% for item in cart %}
        <li>{{ item.name }} - ${{ item.price }}</li>
      {% else %}
        <li>Your cart is empty.</li>
      {% endfor %}
    </ul>
  </main>
</body>
</html>
