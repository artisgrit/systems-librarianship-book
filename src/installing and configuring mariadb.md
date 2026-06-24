# Installing and Configuring MariaDB

## Introduction

In previous sections, we installed and configured the Apache HTTP Server and PHP. Together, these components form part of the classic **LAMP stack**:

- **L**inux
- **A**pache
- **M**ariaDB
- **P**HP

MariaDB is a powerful open-source relational database management system (RDBMS) that serves as a drop-in replacement for MySQL. It is widely used for web applications, content management systems, e-commerce platforms, and enterprise applications.

A relational database stores information in tables consisting of rows and columns. These tables can be linked together through relationships, allowing applications to efficiently store, retrieve, and manage data.

In this chapter, we will learn how to:

- Install MariaDB
- Secure a MariaDB installation
- Create databases and users
- Manage permissions
- Create tables
- Insert, update, and delete records
- Connect PHP applications to MariaDB
- Build a simple database-driven website

---

# Understanding MariaDB's Role in Web Applications

A static website simply serves files such as:

- HTML
- CSS
- JavaScript
- Images

A dynamic website stores and retrieves information from a database.

Examples include:

- WordPress
- Moodle
- Drupal
- Joomla
- Laravel applications
- E-commerce platforms

When a user visits a dynamic website:

1. Browser sends request
2. Apache receives request
3. PHP processes request
4. PHP queries MariaDB
5. MariaDB returns data
6. PHP generates HTML
7. Apache sends response back to browser

Example architecture:

```text
User Browser
      │
      ▼
   Apache
      │
      ▼
     PHP
      │
      ▼
   MariaDB
```

---

# Updating the System

Before installing software, update package repositories and installed packages.

```bash
sudo apt update
sudo apt upgrade
sudo apt autoremove
sudo apt clean
```

Sometimes a reboot may be required after kernel updates.

```bash
sudo systemctl --no-wall reboot
```

Reconnect after the reboot completes.

---

# Installing MariaDB

Install the MariaDB server and client packages.

```bash
sudo apt install mariadb-server mariadb-client
```

The packages provide:

| Package | Purpose |
|----------|----------|
| mariadb-server | Database server |
| mariadb-client | Command-line database client |

Verify installation:

```bash
mariadb --version
```

Example output:

```text
mariadb  Ver 15.1 Distrib 10.11.x
```

---

# Managing the MariaDB Service

Start MariaDB:

```bash
sudo systemctl start mariadb
```

Enable automatic startup:

```bash
sudo systemctl enable mariadb
```

Check status:

```bash
sudo systemctl status mariadb
```

Look for:

```text
Loaded: loaded
Active: active (running)
```

If MariaDB is active and running, the service is functioning correctly.

---

# Securing MariaDB

MariaDB includes a security script that performs several important hardening tasks.

Run:

```bash
sudo mariadb-secure-installation
```

The script can:

- Remove anonymous users
- Disable remote root login
- Remove test databases
- Reload privilege tables
- Configure root authentication

Recommended responses:

```text
Remove anonymous users?           Y
Disallow root login remotely?     Y
Remove test database?             Y
Reload privilege tables?          Y
```

---

# Understanding Root Authentication

Most Ubuntu installations use Unix socket authentication for the MariaDB root account.

Instead of using a password:

```bash
mariadb -u root -p
```

administrators log in using Linux administrative privileges:

```bash
sudo mariadb
```

This improves security by preventing password-based root logins.

---

# Logging Into MariaDB

Access the database server:

```bash
sudo mariadb
```

You should see:

```text
Welcome to the MariaDB monitor.
```

The prompt changes to:

```text
MariaDB [(none)]>
```

---

# Viewing Existing Databases

List available databases:

```sql
SHOW DATABASES;
```

Example output:

```text
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

Exit MariaDB:

```sql
EXIT;
```

or

```sql
\q
```

---

# Creating a Database

Log back in:

```bash
sudo mariadb
```

Create a database:

```sql
CREATE DATABASE opacdb
CHARACTER SET utf8mb4
COLLATE utf8mb4_unicode_ci;
```

View databases:

```sql
SHOW DATABASES;
```

---

# Why UTF-8?

The UTF-8 character set allows storage of:

- International languages
- Emoji characters
- Mathematical symbols
- Special characters

Using `utf8mb4` ensures maximum compatibility.

---

# Creating a Database User

Applications should never use the MariaDB root account.

Create a dedicated application user:

```sql
CREATE USER 'opacuser'@'localhost'
IDENTIFIED BY 'StrongPassword123!';
```

Verify:

```sql
SELECT User FROM mysql.user;
```

---

# Granting Permissions

Grant privileges on the database:

```sql
GRANT ALL PRIVILEGES
ON opacdb.*
TO 'opacuser'@'localhost';
```

Apply changes:

```sql
FLUSH PRIVILEGES;
```

Verify grants:

```sql
SHOW GRANTS FOR 'opacuser'@'localhost';
```

---

# Understanding Database Privileges

Common MariaDB privileges include:

| Privilege | Purpose |
|------------|------------|
| SELECT | Read data |
| INSERT | Add records |
| UPDATE | Modify records |
| DELETE | Remove records |
| CREATE | Create tables |
| DROP | Delete tables |
| ALTER | Modify table structures |

Example:

```sql
GRANT SELECT, INSERT, UPDATE
ON opacdb.*
TO 'opacuser'@'localhost';
```

This follows the Principle of Least Privilege.

---

# Logging In as the Application User

Exit root session:

```sql
EXIT;
```

Login as the application user:

```bash
mariadb -u opacuser -p
```

Enter the password when prompted.

List databases:

```sql
SHOW DATABASES;
```

Switch to the application database:

```sql
USE opacdb;
```

---

# Creating Tables

Tables store data inside a database.

Create a table named `books`:

```sql
CREATE TABLE books (
    id INT UNSIGNED NOT NULL AUTO_INCREMENT,
    author VARCHAR(150) NOT NULL,
    title VARCHAR(150) NOT NULL,
    copyright YEAR NOT NULL,
    PRIMARY KEY (id)
);
```

Verify:

```sql
SHOW TABLES;
```

Describe table structure:

```sql
DESCRIBE books;
```

---

# Understanding Table Fields

| Field | Purpose |
|---------|---------|
| id | Unique identifier |
| author | Book author |
| title | Book title |
| copyright | Publication year |

The `id` field is a primary key and automatically increments.

---

# Inserting Data

Insert records:

```sql
INSERT INTO books (author, title, copyright)
VALUES
('Jennifer Egan', 'The Candy House', '2022'),
('Imbolo Mbue', 'How Beautiful We Were', '2021'),
('Lydia Millet', 'A Children''s Bible', '2020'),
('Julia Phillips', 'Disappearing Earth', '2019');
```

View all records:

```sql
SELECT * FROM books;
```

---

# Querying Data

Retrieve authors:

```sql
SELECT author FROM books;
```

Retrieve titles:

```sql
SELECT title FROM books;
```

Retrieve specific columns:

```sql
SELECT author, title FROM books;
```

Search records:

```sql
SELECT author
FROM books
WHERE author LIKE '%Millet%';
```

---

# Modifying Tables

Add a new column:

```sql
ALTER TABLE books
ADD publisher VARCHAR(75)
AFTER title;
```

Verify:

```sql
DESCRIBE books;
```

---

# Updating Records

Add publisher information:

```sql
UPDATE books
SET publisher='Simon & Schuster'
WHERE id='1';
```

```sql
UPDATE books
SET publisher='Penguin Random House'
WHERE id='2';
```

```sql
UPDATE books
SET publisher='W. W. Norton & Company'
WHERE id='3';
```

```sql
UPDATE books
SET publisher='Knopf'
WHERE id='4';
```

View results:

```sql
SELECT * FROM books;
```

---

# Deleting Records

Delete a record:

```sql
DELETE FROM books
WHERE author='Julia Phillips';
```

Verify:

```sql
SELECT * FROM books;
```

---

# Additional Inserts

Add more books:

```sql
INSERT INTO books
(author, title, publisher, copyright)
VALUES
('Emma Donoghue', 'Room', 'Little, Brown & Company', '2010'),
('Zadie Smith', 'White Teeth', 'Hamish Hamilton', '2000');
```

Verify:

```sql
SELECT * FROM books;
```

---

# Additional Queries

Retrieve books published before 2011:

```sql
SELECT author, publisher
FROM books
WHERE copyright < '2011';
```

Sort by publication year:

```sql
SELECT author
FROM books
ORDER BY copyright;
```

---

# MariaDB Security Hardening

Production systems require additional hardening.

---

## Keep MariaDB Updated

```bash
sudo apt update
sudo apt upgrade
```

---

## Restrict Network Access

Verify MariaDB only listens locally:

```bash
sudo ss -tlnp | grep 3306
```

Expected:

```text
127.0.0.1:3306
```

---

## Use Dedicated Users

Avoid application access through root.

Good:

```sql
CREATE USER 'webapp'@'localhost';
```

Bad:

```sql
GRANT ALL PRIVILEGES TO root;
```

---

## Use Strong Passwords

Example:

```text
CorrectHorseBatteryStaple2025!
```

Avoid:

```text
password123
```

---

## Backup Databases

Create a backup:

```bash
mysqldump -u root -p opacdb > opacdb_backup.sql
```

Restore:

```bash
mariadb -u root -p opacdb < opacdb_backup.sql
```

---

## Monitor Logs

Check MariaDB logs:

```bash
sudo journalctl -u mariadb
```

or

```bash
sudo tail -f /var/log/mysql/error.log
```

---

# Installing PHP Support for MariaDB

PHP requires database drivers.

Install support:

```bash
sudo apt install php-mysql php-mysqli php-common
```

Restart services:

```bash
sudo systemctl restart apache2
sudo systemctl restart mariadb
```

Although the package name contains "mysql", it fully supports MariaDB because MariaDB maintains compatibility with MySQL client libraries.

---

# Creating Database Credentials File

Move to `/var/www`:

```bash
cd /var/www
```

Create credentials file:

```bash
sudo touch login.php
```

Secure permissions:

```bash
sudo chmod 640 login.php
sudo chown :www-data login.php
```

Edit:

```bash
sudo nano login.php
```

Contents:

```php
<?php
$db_hostname = "localhost";
$db_database = "opacdb";
$db_username = "opacuser";
$db_password = "StrongPassword123!";
?>
```

---

# Why Store Credentials Outside the Document Root?

Document root:

```text
/var/www/html
```

Credentials file:

```text
/var/www/login.php
```

This prevents direct web access to sensitive database credentials.

---

# Creating a PHP Database Application

Create:

```bash
cd /var/www/html
sudo nano opac.php
```

Add:

```php
<?php

require_once '/var/www/login.php';

$conn = new mysqli(
    $db_hostname,
    $db_username,
    $db_password,
    $db_database
);

if ($conn->connect_error) {
    die("Connection failed");
}

$result = $conn->query(
    "SELECT author, title, copyright FROM books"
);

echo "<h1>Book List</h1>";

while ($row = $result->fetch_assoc()) {

    echo "<p>";
    echo htmlspecialchars($row['author']);
    echo " - ";
    echo htmlspecialchars($row['title']);
    echo " (";
    echo htmlspecialchars($row['copyright']);
    echo ")";
    echo "</p>";
}

$conn->close();

?>
```

---

# Testing PHP Syntax

Verify syntax:

```bash
sudo php -f /var/www/login.php
```

```bash
sudo php -f /var/www/html/opac.php
```

No errors should be displayed.

---

# Testing the Application

Open:

```text
http://SERVER-IP/opac.php
```

You should see data retrieved directly from MariaDB and rendered through PHP.

This demonstrates the complete flow:

```text
Browser
   │
   ▼
Apache
   │
   ▼
PHP
   │
   ▼
MariaDB
```

---

# Conclusion

In this chapter, we completed the LAMP stack by installing and configuring MariaDB.

We learned how to:

- Install MariaDB
- Manage the database service
- Secure the installation
- Create databases
- Create users
- Assign privileges
- Create tables
- Insert and retrieve data
- Update and delete records
- Secure MariaDB
- Connect PHP to MariaDB
- Build a simple database-driven web application

MariaDB provides the persistent storage layer for dynamic websites and applications. Combined with Linux, Apache, and PHP, it forms a powerful platform capable of supporting modern web services ranging from small personal websites to large enterprise applications.
