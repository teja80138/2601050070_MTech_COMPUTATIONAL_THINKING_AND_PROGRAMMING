# Online Shopping Cart System

A beginner-friendly Python OOP-based online shopping cart application.

The program allows a user to:

- View available products
- Add products to the cart
- Remove products from the cart
- Change product quantities
- View the shopping cart
- Apply discounts
- Calculate GST
- Checkout and buy products
- Automatically reduce stock after purchase
- Exit the application

---

## Complete Python Code

```python
class Product:
    def __init__(self, name, price, stock):
        self.name = name
        self.price = price
        self.stock = stock


class ShoppingCart:
    def __init__(self, gst_rate=18):
        self.items = {}          # product name -> [Product, quantity]
        self.discount = 0
        self.gst_rate = gst_rate

    # Add product to cart
    def add_to_cart(self, product, quantity):
        if quantity <= 0:
            print("Quantity must be greater than 0.")
            return

        if quantity > product.stock:
            print(f"Only {product.stock} units of {product.name} are available.")
            return

        if product.name in self.items:
            current_quantity = self.items[product.name][1]

            if current_quantity + quantity > product.stock:
                print(
                    f"Cannot add that many. "
                    f"Only {product.stock - current_quantity} more available."
                )
                return

            self.items[product.name][1] += quantity
        else:
            self.items[product.name] = [product, quantity]

        print(f"{quantity} x {product.name} added to cart.")

    # Remove product completely from cart
    def remove_from_cart(self, product_name):
        if product_name in self.items:
            del self.items[product_name]
            print(f"{product_name} removed from cart.")
        else:
            print("Product not found in cart.")

    # Change quantity
    def change_quantity(self, product_name, quantity):
        if product_name not in self.items:
            print("Product not found in cart.")
            return

        product = self.items[product_name][0]

        if quantity <= 0:
            self.remove_from_cart(product_name)

        elif quantity > product.stock:
            print(f"Only {product.stock} units are available.")

        else:
            self.items[product_name][1] = quantity
            print(f"Quantity updated to {quantity}.")

    # Apply discount
    def apply_discount(self, discount_percent):
        if 0 <= discount_percent <= 100:
            self.discount = discount_percent
            print(f"{discount_percent}% discount applied.")
        else:
            print("Invalid discount percentage.")

    # Calculate subtotal
    def calculate_subtotal(self):
        subtotal = 0

        for product, quantity in self.items.values():
            subtotal += product.price * quantity

        return subtotal

    # Calculate discount
    def calculate_discount(self):
        subtotal = self.calculate_subtotal()
        return subtotal * self.discount / 100

    # Calculate GST
    def calculate_gst(self):
        subtotal = self.calculate_subtotal()
        discount = self.calculate_discount()

        taxable_amount = subtotal - discount

        return taxable_amount * self.gst_rate / 100

    # Calculate final total
    def calculate_total(self):
        subtotal = self.calculate_subtotal()
        discount = self.calculate_discount()
        gst = self.calculate_gst()

        return subtotal - discount + gst

    # Display cart
    def display_cart(self):
        print("\n========== YOUR CART ==========")

        if not self.items:
            print("Your cart is empty.")
            return

        for product, quantity in self.items.values():
            amount = product.price * quantity

            print(
                f"{product.name:<15} "
                f"₹{product.price:<10.2f} "
                f"x {quantity:<3} "
                f"= ₹{amount:.2f}"
            )

        subtotal = self.calculate_subtotal()
        discount = self.calculate_discount()
        gst = self.calculate_gst()
        total = self.calculate_total()

        print("--------------------------------")
        print(f"Subtotal     : ₹{subtotal:.2f}")
        print(f"Discount     : -₹{discount:.2f}")
        print(f"GST ({self.gst_rate}%)     : ₹{gst:.2f}")
        print(f"Final Total  : ₹{total:.2f}")
        print("================================")


# -------------------------------------------------
# Store / Shop
# -------------------------------------------------

products = {
    "Laptop": Product("Laptop", 50000, 5),
    "Mouse": Product("Mouse", 1000, 20),
    "Keyboard": Product("Keyboard", 2000, 10),
    "Headphones": Product("Headphones", 3000, 15),
    "Monitor": Product("Monitor", 15000, 8),
    "Mobile": Product("Mobile", 25000, 10)
}


# Create shopping cart
cart = ShoppingCart(gst_rate=18)


# -------------------------------------------------
# Display Available Products
# -------------------------------------------------

def display_products():
    print("\n========== AVAILABLE PRODUCTS ==========")

    print(
        f"{'Product':<15}"
        f"{'Price':<15}"
        f"{'Stock':<10}"
    )

    print("----------------------------------------")

    for product in products.values():
        print(
            f"{product.name:<15}"
            f"₹{product.price:<14.2f}"
            f"{product.stock:<10}"
        )

    print("========================================")


# -------------------------------------------------
# Buy / Checkout
# -------------------------------------------------

def checkout():
    if not cart.items:
        print("\nYour cart is empty.")
        return

    cart.display_cart()

    confirm = input("\nDo you want to buy these items? (yes/no): ").lower()

    if confirm == "yes":

        # Reduce stock
        for product, quantity in cart.items.values():
            product.stock -= quantity

        print("\n================================")
        print("       ORDER SUCCESSFUL!")
        print("================================")
        print(f"Amount Paid: ₹{cart.calculate_total():.2f}")
        print("Thank you for your purchase!")

        # Empty cart after purchase
        cart.items.clear()
        cart.discount = 0

    else:
        print("Checkout cancelled.")


# -------------------------------------------------
# Main Menu
# -------------------------------------------------

while True:

    print("\n\n========== ONLINE SHOP ==========")
    print("1. View Products")
    print("2. Add Item to Cart")
    print("3. Remove Item from Cart")
    print("4. Change Quantity")
    print("5. View Cart")
    print("6. Apply Discount")
    print("7. Buy / Checkout")
    print("8. Exit")
    print("=================================")

    choice = input("Enter your choice: ")

    # 1. View Products
    if choice == "1":
        display_products()

    # 2. Add Item
    elif choice == "2":

        display_products()

        product_name = input(
            "\nEnter product name: "
        ).strip()

        if product_name in products:

            try:
                quantity = int(
                    input("Enter quantity: ")
                )

                cart.add_to_cart(
                    products[product_name],
                    quantity
                )

            except ValueError:
                print("Please enter a valid number.")

        else:
            print("Product not found.")

    # 3. Remove Item
    elif choice == "3":

        if not cart.items:
            print("Your cart is empty.")
            continue

        cart.display_cart()

        product_name = input(
            "\nEnter product name to remove: "
        ).strip()

        cart.remove_from_cart(product_name)

    # 4. Change Quantity
    elif choice == "4":

        if not cart.items:
            print("Your cart is empty.")
            continue

        cart.display_cart()

        product_name = input(
            "\nEnter product name: "
        ).strip()

        try:
            quantity = int(
                input("Enter new quantity: ")
            )

            cart.change_quantity(
                product_name,
                quantity
            )

        except ValueError:
            print("Please enter a valid number.")

    # 5. View Cart
    elif choice == "5":
        cart.display_cart()

    # 6. Apply Discount
    elif choice == "6":

        try:
            discount = float(
                input("Enter discount percentage: ")
            )

            cart.apply_discount(discount)

        except ValueError:
            print("Please enter a valid number.")

    # 7. Checkout
    elif choice == "7":
        checkout()

    # 8. Exit
    elif choice == "8":

        print("\nThank you for visiting our shop!")
        print("Goodbye!")
        break

    else:
        print("Invalid choice. Please try again.")
```

---

# How the Program Works

## 1. Product Class

The `Product` class represents a product available in the shop.

Each product contains:

- Product name
- Product price
- Available stock

Example:

```python
laptop = Product("Laptop", 50000, 5)
```

This means:

- Product: Laptop
- Price: ₹50,000
- Stock: 5 units

---

## 2. ShoppingCart Class

The `ShoppingCart` class manages everything related to the customer's cart.

It stores:

```python
self.items = {}
self.discount = 0
self.gst_rate = gst_rate
```

The cart can:

- Add products
- Remove products
- Change quantities
- Apply discounts
- Calculate subtotal
- Calculate GST
- Calculate final bill

---

# Main Menu

When the program starts, the following menu is displayed:

```text
========== ONLINE SHOP ==========
1. View Products
2. Add Item to Cart
3. Remove Item from Cart
4. Change Quantity
5. View Cart
6. Apply Discount
7. Buy / Checkout
8. Exit
=================================
Enter your choice:
```

The user can select any option.

---

# 1. View Products

Select:

```text
1
```

Example output:

```text
========== AVAILABLE PRODUCTS ==========
Product        Price          Stock
----------------------------------------
Laptop         ₹50000.00      5
Mouse          ₹1000.00       20
Keyboard       ₹2000.00       10
Headphones     ₹3000.00       15
Monitor        ₹15000.00      8
Mobile         ₹25000.00      10
========================================
```

---

# 2. Add Item to Cart

Select:

```text
2
```

The program asks:

```text
Enter product name: Laptop
Enter quantity: 1
```

Output:

```text
1 x Laptop added to cart.
```

You can add multiple products.

Example:

```text
Laptop x 1
Mouse x 2
Headphones x 1
```

The program also checks whether enough stock is available.

---

# 3. Remove Item from Cart

Select:

```text
3
```

The program displays your cart and asks:

```text
Enter product name to remove: Mouse
```

Output:

```text
Mouse removed from cart.
```

---

# 4. Change Quantity

Select:

```text
4
```

Example:

```text
Enter product name: Mouse
Enter new quantity: 5
```

Output:

```text
Quantity updated to 5.
```

If you enter `0`, the product is removed.

---

# 5. View Cart

Select:

```text
5
```

Example:

```text
========== YOUR CART ==========
Laptop          ₹50000.00   x 1   = ₹50000.00
Mouse           ₹1000.00    x 2   = ₹2000.00
--------------------------------
Subtotal     : ₹52000.00
Discount     : -₹0.00
GST (18%)    : ₹9360.00
Final Total  : ₹61360.00
================================
```

---

# 6. Apply Discount

Select:

```text
6
```

Enter:

```text
Enter discount percentage: 10
```

Output:

```text
10% discount applied.
```

For a ₹52,000 subtotal:

```text
Discount = ₹52,000 × 10 / 100
         = ₹5,200
```

Taxable amount:

```text
₹52,000 - ₹5,200
= ₹46,800
```

GST:

```text
₹46,800 × 18 / 100
= ₹8,424
```

Final amount:

```text
₹52,000 - ₹5,200 + ₹8,424
= ₹55,224
```

---

# 7. Buy / Checkout

Select:

```text
7
```

The program displays the cart and asks:

```text
Do you want to buy these items? (yes/no):
```

Enter:

```text
yes
```

The order is completed.

Example:

```text
================================
       ORDER SUCCESSFUL!
================================
Amount Paid: ₹55224.00
Thank you for your purchase!
```

After the purchase:

- Stock is reduced
- Cart is emptied
- Discount is reset

For example, if there were 20 Mouse units and you bought 2:

```text
Old stock = 20
Purchased = 2
New stock = 18
```

---

# 8. Exit

Select:

```text
8
```

Output:

```text
Thank you for visiting our shop!
Goodbye!
```

The program then stops.

---

# Billing Formula

The program uses the following formulas.

## Subtotal

```text
Subtotal = Product Price × Quantity
```

For multiple products, all amounts are added.

## Discount

```text
Discount = Subtotal × Discount Percentage / 100
```

## Taxable Amount

```text
Taxable Amount = Subtotal - Discount
```

## GST

```text
GST = Taxable Amount × GST Rate / 100
```

## Final Total

```text
Final Total = Subtotal - Discount + GST
```

---

# OOP Concepts Used

This project demonstrates several important Python OOP concepts.

## Class

Two classes are used:

```python
Product
ShoppingCart
```

## Object

Objects are created from the classes:

```python
Product("Laptop", 50000, 5)
ShoppingCart(gst_rate=18)
```

## Constructor

The `__init__()` method initializes objects:

```python
def __init__(self, name, price, stock):
    self.name = name
    self.price = price
    self.stock = stock
```

## Instance Variables

Examples:

```python
self.name
self.price
self.stock
self.items
self.discount
self.gst_rate
```

## Methods

The `ShoppingCart` class contains methods such as:

```python
add_to_cart()
remove_from_cart()
change_quantity()
apply_discount()
calculate_subtotal()
calculate_discount()
calculate_gst()
calculate_total()
display_cart()
```

## Dictionary

Products are stored in a dictionary:

```python
products = {
    "Laptop": Product("Laptop", 50000, 5),
    "Mouse": Product("Mouse", 1000, 20)
}
```

The shopping cart also uses a dictionary to track selected products.

## Loops

A `while` loop keeps the shopping application running until the user selects Exit:

```python
while True:
    ...
```

## Conditional Statements

`if`, `elif`, and `else` are used to process menu choices and validate user input.

---

# Features of the Project

| Feature | Supported |
|---|---|
| View products | Yes |
| Product prices | Yes |
| Product stock | Yes |
| Add to cart | Yes |
| Remove from cart | Yes |
| Change quantity | Yes |
| Stock validation | Yes |
| Discount | Yes |
| GST | Yes |
| View cart | Yes |
| Checkout | Yes |
| Update stock after purchase | Yes |
| Clear cart after purchase | Yes |
| Interactive menu | Yes |

---

# Possible Future Improvements

This project can be expanded into a more advanced e-commerce application by adding:

- User registration and login
- Product search
- Product categories
- Product ratings and reviews
- Multiple payment methods
- Order history
- Customer details
- Delivery address
- Invoice generation
- Admin panel
- Add/remove products by admin
- Database connectivity using MySQL
- GUI using Tkinter
- Web application using Flask or Django
- REST API
