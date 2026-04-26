# Build-a-Storefront-Backend

## Project Overview

This project is a backend API for an online storefront application. It allows users to browse products, view product details, manage orders, and add products to their cart. The frontend consumes this API to display the data.

---

## Technologies

* Node.js & Express.js
* PostgreSQL
* dotenv
* db-migrate
* jsonwebtoken (JWT)
* bcrypt
* Jasmine (testing)

---

## Setup and Installation

### 1. Clone the repository

```bash
git clone https://github.com/AsmaaSalah1/Build-a-Storefront-Backend.git
cd Build-a-Storefront-Backend
```

### 2. Install dependencies

```bash
npm install
```

---

## Environment Variables

Create a `.env` file in the root directory:

```
POSTGRES_HOST=127.0.0.1
POSTGRES_DB=store_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres

BCRYPT_PASSWORD=your_secret_salt
SALT_ROUNDS=10
TOKEN_SECRET=your_jwt_secret
PORT=3000
```

---

## Database Setup

### Option 1 (Recommended): Using Docker

If you don’t have PostgreSQL installed, you can run it using Docker:

```bash
docker-compose up -d
```

Then run migrations:

```bash
npx db-migrate up
```

---

### Option 2: Local PostgreSQL

1. Create the database:

```sql
CREATE DATABASE store_db;
```

2. Run migrations:

```bash
npx db-migrate up
```

---

## Important Note (For Reviewers)

If migrations do not run correctly, reset the database:

```bash
docker-compose down -v
docker-compose up -d
npx db-migrate up
```

---

## Seeding Initial Data (Optional)

### Users

```sql
INSERT INTO users (id, firstname, lastname, password_digest) VALUES
(1, 'John', 'Doe', '$2b$10$u9914pVh.5wrNhMIXZwCIurk42GRx7wyYX004ofW9QY1lXp2vFQ1.');
```

### Products

```sql
INSERT INTO products (id, name, price, category) VALUES
(1, 'Laptop', 1200, 'Electronics'),
(2, 'Smartphone', 800, 'Electronics'),
(3, 'Shoes', 100, 'Fashion'),
(4, 'Book', 20, 'Education');
```

### Orders

```sql
INSERT INTO orders (id, user_id, status) VALUES
(1, 1, 'active');
```

---

## API Endpoints

### Products

| Route                     | Method | Description            | Auth |
| ------------------------- | ------ | ---------------------- | ---- |
| `/products`               | GET    | Get all products       | No   |
| `/products/:id`           | GET    | Get product by id      | No   |
| `/products`               | POST   | Create product         | Yes  |
| `/products/top`           | GET    | Top 5 popular products | No   |
| `/products/category/:cat` | GET    | Products by category   | No   |

---

### Users

| Route        | Method | Description    | Auth |
| ------------ | ------ | -------------- | ---- |
| `/users`     | GET    | Get all users  | Yes  |
| `/users/:id` | GET    | Get user by id | Yes  |
| `/users`     | POST   | Create user    | Yes  |

---

### Orders

| Route                        | Method | Description                 | Auth |
| ---------------------------- | ------ | --------------------------- | ---- |
| `/orders/current/:user_id`   | GET    | Current order for user      | Yes  |
| `/orders/:id/products`       | POST   | Add product to order        | Yes  |
| `/orders/completed/:user_id` | GET    | Completed orders (optional) | Yes  |

---

## Running the Server

```bash
npm run start
```

Server runs on:

```
http://localhost:3000
```

---

## Running Tests

```bash
npm run test
```

Make sure the database is migrated before running tests.

---

## Project Structure

```
/src
  /handlers
  /models
  /tests
  /utils
  server.ts
```

---

## Notes

* Passwords are hashed using bcrypt.
* JWT authentication is used for protected routes.
* Database relationships:

  * users → orders (1:M)
  * orders → products (M:N via order_products)
* RESTful API design is followed.
