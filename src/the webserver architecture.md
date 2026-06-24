# Complete Web Server Architecture

A modern web server is more than just software that serves HTML files. It is part of a larger ecosystem involving networking, security, application processing, databases, and user interaction.

The following diagram illustrates a typical web architecture:

```text
+----------------------+
|      Web Browser     |
| (Chrome, Firefox)    |
+----------+-----------+
           |
           | HTTPS Request
           |
           v
+----------------------+
|         DNS          |
| Resolves Domain Name |
+----------+-----------+
           |
           v
+----------------------+
|      Firewall        |
| Ports 80 and 443     |
+----------+-----------+
           |
           v
+----------------------+
|     Web Server       |
| Apache / Nginx       |
+----------+-----------+
           |
     +-----+-----+
     |           |
     v           v
+---------+  +---------+
| Static  |  |Dynamic  |
| Content |  |Content  |
+---------+  +---------+
                 |
                 v
          +-------------+
          | Application |
          | PHP, Python |
          | Node.js     |
          +------+------+
                 |
                 v
          +-------------+
          |  Database   |
          | MySQL       |
          | PostgreSQL  |
          +-------------+
```

---

# Components of a Web Server Architecture

## 1. Client

The client is the software used by the user.

Examples:

- Chrome
- Firefox
- Edge
- Safari
- curl
- w3m

The client sends requests and displays responses.

---

## 2. DNS Server

Humans use names such as:

```text
www.example.com
```

Computers communicate using IP addresses:

```text
142.250.190.78
```

DNS translates domain names into IP addresses.

Without DNS, users would need to remember numerical IP addresses.

---

## 3. Firewall

A firewall controls incoming and outgoing network traffic.

Common web ports:

| Port | Protocol | Purpose |
|--------|----------|----------|
| 80 | HTTP | Unencrypted web traffic |
| 443 | HTTPS | Encrypted web traffic |
| 22 | SSH | Remote administration |

Example:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

---

## 4. Web Server

The web server receives HTTP requests and returns responses.

Examples:

- Apache
- Nginx
- LiteSpeed
- IIS
- Caddy

Responsibilities include:

- Serving files
- Managing virtual hosts
- Processing requests
- Logging traffic
- SSL termination

---

## 5. Application Layer

Modern websites frequently execute server-side code.

Common technologies:

### PHP

```php
<?php
echo "Hello World";
?>
```

### Python

Frameworks:

- Django
- Flask
- FastAPI

### JavaScript

Frameworks:

- Express.js
- NestJS

The application layer generates dynamic content.

---

## 6. Database Layer

Databases store application data.

Examples:

- MySQL
- MariaDB
- PostgreSQL
- SQLite

Common data stored:

- Users
- Products
- Orders
- Blog posts
- Messages

---

## 7. Storage Layer

Files uploaded by users are stored separately from application code.

Examples:

- Images
- Videos
- PDFs
- Backups

Storage locations:

- Local disks
- NAS systems
- Cloud storage

---

# HTTP and HTTPS

## What is HTTP?

HTTP stands for:

```text
HyperText Transfer Protocol
```

It is the protocol used by browsers and web servers to communicate.

Example:

```text
http://example.com
```

HTTP traffic is transmitted in plain text.

This means attackers could potentially intercept data.

---

## What is HTTPS?

HTTPS stands for:

```text
HyperText Transfer Protocol Secure
```

Example:

```text
https://example.com
```

HTTPS protects communication between browsers and servers using encryption.

Benefits:

- Confidentiality
- Integrity
- Authentication

---

# SSL and TLS

## What is SSL?

SSL stands for:

```text
Secure Sockets Layer
```

SSL was the original technology used to secure web traffic.

Today SSL is considered obsolete.

---

## What is TLS?

TLS stands for:

```text
Transport Layer Security
```

TLS is the modern replacement for SSL.

Although people commonly say:

```text
SSL Certificate
```

they are usually referring to TLS certificates.

---

# How HTTPS Works

When a user visits:

```text
https://example.com
```

The browser and server perform a TLS handshake.

```text
Browser
    |
    | Request
    v
Web Server
    |
    | Sends Certificate
    v
Browser Verifies Certificate
    |
    | Generates Session Keys
    v
Encrypted Connection Established
```

After the handshake:

- Data is encrypted
- Attackers cannot easily read traffic
- Users can verify the site's identity

---

# SSL/TLS Certificates

A certificate proves the identity of a website.

Example:

```text
example.com
```

A certificate contains:

- Domain name
- Public key
- Expiration date
- Issuing Certificate Authority

---

# Certificate Authorities (CAs)

Certificate Authorities issue trusted certificates.

Examples:

- Let's Encrypt
- DigiCert
- GlobalSign
- Sectigo

Browsers trust certificates issued by recognized authorities.

---

# Let's Encrypt

Let's Encrypt is a free Certificate Authority.

Benefits:

- Free certificates
- Automated issuance
- Automated renewal
- Widely trusted

It has become the standard solution for securing websites.

---

# What is Certbot?

Certbot is a tool used to automatically obtain and manage SSL/TLS certificates from Let's Encrypt.

Functions:

- Request certificates
- Configure Apache
- Configure Nginx
- Renew certificates automatically

---

# Installing Certbot on Ubuntu

For Apache:

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache
```

For Nginx:

```bash
sudo apt install certbot python3-certbot-nginx
```

---

# Obtaining a Certificate

For Apache:

```bash
sudo certbot --apache
```

Certbot will:

1. Verify domain ownership
2. Request a certificate
3. Configure Apache
4. Redirect HTTP to HTTPS

---

# Viewing Installed Certificates

```bash
sudo certbot certificates
```

Example output:

```text
Certificate Name: example.com
Expiry Date: 2026-12-01
Domains: example.com www.example.com
```

---

# Renewing Certificates

Let's Encrypt certificates expire every 90 days.

Manual renewal:

```bash
sudo certbot renew
```

Test renewal:

```bash
sudo certbot renew --dry-run
```

Most systems configure automatic renewal through systemd timers.

---

# Redirecting HTTP to HTTPS

A secure website should automatically redirect users.

Example:

```text
http://example.com
```

becomes

```text
https://example.com
```

Benefits:

- Better security
- Improved SEO
- Better user trust

---

# Security Best Practices for Web Servers

## Keep Software Updated

```bash
sudo apt update
sudo apt upgrade
```

---

## Use HTTPS Everywhere

Always secure websites with TLS certificates.

---

## Enable Firewall Rules

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

---

## Disable Unnecessary Services

Reduce attack surfaces by removing unused software.

---

## Use Strong File Permissions

Example:

```bash
chmod 644 index.html
```

---

## Monitor Logs

Apache logs:

```text
/var/log/apache2/access.log
```

```text
/var/log/apache2/error.log
```

Logs help identify:

- Attacks
- Errors
- Performance issues

---

## Regular Backups

Back up:

- Website files
- Databases
- Configuration files
- SSL certificates

---

# Reverse Proxies

A reverse proxy sits between users and backend applications.

Example:

```text
User
  |
  v
Nginx
  |
  v
Node.js App
```

Benefits:

- Load balancing
- Security
- SSL termination
- Caching

---

# Virtual Hosts

A single web server can host multiple websites.

Example:

```text
www.site1.com
www.site2.com
www.site3.com
```

Apache determines which site to serve based on the requested hostname.

---

# Load Balancing

Large websites often use multiple servers.

```text
          Load Balancer
           /    |    \
          /     |     \
         v      v      v
     Server1 Server2 Server3
```

Benefits:

- High availability
- Scalability
- Fault tolerance

---

# Web Server Security Architecture

A production environment often looks like:

```text
Internet
    |
Firewall
    |
Load Balancer
    |
Reverse Proxy
    |
Web Server
    |
Application Server
    |
Database Server
```

Each layer adds security and performance improvements.

---

# Key Terms Summary

| Term | Description |
|--------|-------------|
| HTTP | Unencrypted web protocol |
| HTTPS | Encrypted web protocol |
| SSL | Legacy encryption technology |
| TLS | Modern encryption technology |
| Certificate | Proof of website identity |
| Certbot | Tool for managing certificates |
| Let's Encrypt | Free Certificate Authority |
| Reverse Proxy | Intermediary server |
| Virtual Host | Multiple websites on one server |
| Load Balancer | Distributes traffic |
| Firewall | Controls network access |
| DNS | Resolves domain names |
| Apache | Popular web server |
| Nginx | High-performance web server |
| MySQL | Relational database |
