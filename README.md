

# 🔍 MySQL Essential Commands Cheat Sheet

Here are the fundamental MySQL commands you'll need for your e-commerce project, with explanations of what each one does:

## Database Operations

```sql
-- Show all databases on the server
SHOW DATABASES;

-- Create a new database
CREATE DATABASE ecommerce_db;

-- Select/use a specific database
USE ecommerce_db;

-- Delete a database (careful!)
DROP DATABASE ecommerce_db;
```

## Table Operations

```sql
-- Show all tables in current database
SHOW TABLES;

-- Create users table (example)
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(255) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  full_name VARCHAR(100),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Show table structure (columns, types, etc.)
DESCRIBE users;
-- or
SHOW COLUMNS FROM users;

-- Show complete table creation command
SHOW CREATE TABLE users;

-- Delete a table
DROP TABLE users;
```

## Data Operations

```sql
-- Insert data
INSERT INTO users (email, password, full_name) 
VALUES ('test@example.com', 'hashed_password', 'John Doe');

-- View all data in table
SELECT * FROM users;

-- View specific columns
SELECT id, email, created_at FROM users;

-- View with conditions
SELECT * FROM users WHERE email = 'test@example.com';

-- Update data
UPDATE users 
SET full_name = 'John Smith' 
WHERE id = 1;

-- Delete data
DELETE FROM users WHERE id = 1;
```

## Advanced Queries

```sql
-- Count rows
SELECT COUNT(*) FROM users;

-- Join tables (example with products)
SELECT users.email, orders.total_amount
FROM users
JOIN orders ON users.id = orders.user_id;

-- Add index to improve performance
CREATE INDEX idx_email ON users(email);

-- Show all indexes on a table
SHOW INDEX FROM users;
```

## User Management

```sql
-- Create user
CREATE USER 'ecommerce_user'@'localhost' IDENTIFIED BY 'password';

-- Grant privileges
GRANT ALL PRIVILEGES ON ecommerce_db.* TO 'ecommerce_user'@'localhost';

-- Show user privileges
SHOW GRANTS FOR 'ecommerce_user'@'localhost';

-- Remove user
DROP USER 'ecommerce_user'@'localhost';
```

## Troubleshooting Commands

```sql
-- Check server status
STATUS;

-- Show running processes
SHOW PROCESSLIST;

-- Show server variables
SHOW VARIABLES;

-- Show server version
SELECT VERSION();
```

## Practical Example for Your Project

```sql
-- Create products table
CREATE TABLE products (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  price DECIMAL(10,2) NOT NULL,
  description TEXT,
  category_id INT,
  stock_quantity INT DEFAULT 0,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (category_id) REFERENCES categories(id)
);

-- Create categories table
CREATE TABLE categories (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  slug VARCHAR(100) UNIQUE
);

-- Create orders table
CREATE TABLE orders (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  total_amount DECIMAL(10,2) NOT NULL,
  status ENUM('pending', 'completed', 'cancelled') DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

Remember to:
1. Always back up your database before making structural changes
2. Use `WHERE` clauses with `UPDATE` and `DELETE` to avoid modifying all rows
3. Test queries with `SELECT` before `UPDATE` or `DELETE`
4. Use transactions for critical operations:

```sql
START TRANSACTION;
-- Your SQL commands here
COMMIT; -- or ROLLBACK if something goes wrong
```
