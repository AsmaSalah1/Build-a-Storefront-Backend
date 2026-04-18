# Build-a-Storefront-Backend

## Project Overview
This project is a backend API for an online storefront application. It allows users to browse products, view product details, manage orders, and add products to their cart. The frontend consumes this API to display the data to users.

---

## Table of Contents
1. [Technologies](#technologies)
2. [Setup and Installation](#setup-and-installation)
3. [Database Setup](#database-setup)
4. [API Endpoints](#api-endpoints)
5. [Running the Tests](#running-the-tests)
6. [Environment Variables](#environment-variables)
7. [Project Structure](#project-structure)

---

## Technologies
- Node.js & Express.js
- PostgreSQL
- dotenv
- db-migrate
- jsonwebtoken (JWT)
- bcrypt
- Jasmine (testing)

---

## Setup and Installation

1. **Clone the repository:**
```bash
git clone https://github.com/AsmaaSalah1/Build-a-Storefront-Backend.git
cd Build-a-Storefront-Backend
```

2. **Install dependencies:**
```bash
npm install
```

3. **Configure environment variables:**
Create a `.env` file in the root:
```
POSTGRES_HOST=localhost
POSTGRES_DB=store_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=your_password
BCRYPT_PASSWORD=your_secret_salt
SALT_ROUNDS=10
TOKEN_SECRET=your_jwt_secret
PORT=3000
```

---

## Database Setup

1. **Create the database:**
```sql
CREATE DATABASE store_db;
```

2. **Run migrations:**  
If using `db-migrate`:
```bash
npx db-migrate up
```

3. **Seed initial data:**  

**Users**
```sql
INSERT INTO users (id, firstname, lastname, password_digest) VALUES
(1, 'John', 'Doe', '$2b$10$u9914pVh.5wrNhMIXZwCIurk42GRx7wyYX004ofW9QY1lXp2vFQ1.');
```

**Products**
```sql
INSERT INTO products (id, name, price) VALUES
(1, 'Laptop', 1200),
(2, 'Smartphone', 800),
(3, 'Shoes', 100),
(4, 'Book', 20);
```

**Orders**
```sql
INSERT INTO orders (id, user_id, status) VALUES
(1, 1, 'active');
```

**Order_Products**  
- Usually created by tests or API calls. Optional seed:
```sql
INSERT INTO order_products (order_id, product_id, quantity) VALUES
(1, 1, 1);
```

**Default Ports**
- Backend API: `3000`
- PostgreSQL: `5432`

---

## API Endpoints

### Products
| Route                    | Method | Description                      | Auth Required |
|---------------------------|--------|----------------------------------|---------------|
| `/products`               | GET    | Get all products                 | No            |
| `/products/:id`           | GET    | Get product details by id        | No            |
| `/products`               | POST   | Create a new product             | Yes           |
| `/products/top`           | GET    | Top 5 most popular products      | No            |
| `/products/category/:cat` | GET    | Products by category             | No            |

### Users
| Route           | Method | Description               | Auth Required |
|-----------------|--------|---------------------------|---------------|
| `/users`        | GET    | List all users            | Yes           |
| `/users/:id`    | GET    | Get user details          | Yes           |
| `/users`        | POST   | Create a new user         | Yes           |

### Orders
| Route                       | Method | Description                            | Auth Required |
|------------------------------|--------|----------------------------------------|---------------|
| `/orders/current/:user_id`   | GET    | Get current order by user              | Yes           |
| `/orders/:id/products`       | POST   | Add product to order                   | Yes           |
| `/orders/completed/:user_id` | GET    | [Optional] Completed orders by user   | Yes           |

---

## Running the Tests
```bash
npm run test
```
- Ensure the database is seeded with initial data before running tests.
- All models and endpoints have corresponding Jasmine test suites.

---

## Project Structure

```
/src
  /handlers      - Express route handlers
  /models        - Database models (User, Product, Order, OrderProduct)
  /tests         - Jasmine test specs
  /utils         - JWT, bcrypt helpers, etc.
  server.ts      - Entry point for backend
```

---

## Notes
- Passwords must be hashed using bcrypt.
- JWT authentication is required for protected routes.
- The database schema must reflect the relationships:
  - `users` 1:M `orders`
  - `orders` M:N `products` via `order_products`
- Follow RESTful conventions for endpoints and HTTP verbs.
