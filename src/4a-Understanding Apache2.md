# Understanding Apache's Architecture

Apache HTTP Server is built using a modular architecture. Instead of including every feature by default, Apache loads modules that provide specific functionality.

Benefits of the modular design include:

- Reduced resource consumption
- Improved security
- Easier maintenance
- Flexible configuration

Apache's configuration files are commonly located in:

```text
/etc/apache2/
```

Important directories include:

| Directory | Purpose |
|------------|------------|
| `/etc/apache2/apache2.conf` | Main configuration file |
| `/etc/apache2/sites-available/` | Available virtual hosts |
| `/etc/apache2/sites-enabled/` | Enabled virtual hosts |
| `/etc/apache2/mods-available/` | Available modules |
| `/etc/apache2/mods-enabled/` | Enabled modules |
| `/var/www/html/` | Default document root |
| `/var/log/apache2/` | Log files |

---

# Apache Modules

Modules extend Apache's capabilities.

To view enabled modules:

```bash
apache2ctl -M
```

or

```bash
sudo apachectl -M
```

To enable a module:

```bash
sudo a2enmod module_name
```

To disable a module:

```bash
sudo a2dismod module_name
```

After enabling or disabling modules:

```bash
sudo systemctl restart apache2
```

---

## Common Apache Modules

### mod_ssl

Provides HTTPS and SSL/TLS support.

Enable:

```bash
sudo a2enmod ssl
```

Features:

- HTTPS
- TLS encryption
- Certificate support

---

### mod_rewrite

Provides URL rewriting capabilities.

Enable:

```bash
sudo a2enmod rewrite
```

Examples:

```text
/page.php?id=10
```

can become:

```text
/products/laptop
```

Commonly used by:

- WordPress
- Laravel
- Drupal
- Joomla

---

### mod_headers

Allows manipulation of HTTP headers.

Enable:

```bash
sudo a2enmod headers
```

Useful for:

- Security headers
- Cache control
- CORS policies

Example:

```apache
Header always set X-Frame-Options SAMEORIGIN
```

---

### mod_deflate

Compresses responses before sending them to clients.

Enable:

```bash
sudo a2enmod deflate
```

Benefits:

- Faster websites
- Reduced bandwidth usage

---

### mod_expires

Controls browser caching.

Enable:

```bash
sudo a2enmod expires
```

Example:

```apache
ExpiresActive On
ExpiresByType text/css "access plus 30 days"
```

---

### mod_proxy

Allows Apache to operate as a reverse proxy.

Enable:

```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
```

Use cases:

- Load balancing
- Reverse proxying
- Backend application servers

---

### mod_proxy_fcgi

Used with PHP-FPM.

Enable:

```bash
sudo a2enmod proxy_fcgi
```

Improves PHP performance compared to traditional mod_php setups.

---

### mod_status

Provides real-time server statistics.

Enable:

```bash
sudo a2enmod status
```

Useful for:

- Monitoring
- Troubleshooting
- Performance analysis

---

### mod_security

A Web Application Firewall (WAF) for Apache.

Installation:

```bash
sudo apt install libapache2-mod-security2
```

Features:

- SQL injection protection
- Cross-site scripting protection
- Request filtering

---

### mod_evasive

Protects against denial-of-service attacks.

Installation:

```bash
sudo apt install libapache2-mod-evasive
```

Features:

- Request rate limiting
- DoS mitigation
- Brute-force protection

---

### PHP Module

Allows Apache to execute PHP code.

Installation:

```bash
sudo apt install php libapache2-mod-php
```

Example PHP file:

```php
<?php
phpinfo();
?>
```

---

# Virtual Hosts

Apache supports hosting multiple websites on a single server.

Example:

```text
www.company.com
www.school.edu
www.blog.net
```

Each site receives its own configuration file.

Example:

```text
/etc/apache2/sites-available/example.conf
```

Enable a site:

```bash
sudo a2ensite example.conf
```

Disable a site:

```bash
sudo a2dissite example.conf
```

Reload Apache:

```bash
sudo systemctl reload apache2
```

---

# Apache Log Files

Logs are essential for monitoring and troubleshooting.

Access log:

```text
/var/log/apache2/access.log
```

Error log:

```text
/var/log/apache2/error.log
```

View logs:

```bash
tail -f /var/log/apache2/access.log
```

```bash
tail -f /var/log/apache2/error.log
```

Common uses:

- Debugging websites
- Monitoring visitors
- Detecting attacks

---

# Apache Web Server Hardening

A default Apache installation works immediately, but production systems require additional security measures.

Hardening reduces the attack surface and improves overall security.

---

## Keep Apache Updated

Always install security updates.

```bash
sudo apt update
sudo apt upgrade
```

Check Apache version:

```bash
apache2 -v
```

---

## Hide Apache Version Information

By default, Apache may reveal version information.

Edit:

```text
/etc/apache2/conf-available/security.conf
```

Set:

```apache
ServerTokens Prod
ServerSignature Off
```

Restart Apache:

```bash
sudo systemctl restart apache2
```

This changes:

```text
Apache/2.4.58 Ubuntu
```

to:

```text
Apache
```

---

## Disable Directory Listing

Without an index file, Apache may display directory contents.

Unsafe:

```text
http://example.com/uploads/
```

may expose files.

Disable by ensuring:

```apache
Options -Indexes
```

appears inside the directory configuration.

---

## Disable Unused Modules

Review enabled modules:

```bash
apache2ctl -M
```

Disable unnecessary modules:

```bash
sudo a2dismod autoindex
sudo a2dismod status
```

Only enable modules you actually need.

---

## Configure a Firewall

Ubuntu uses UFW.

Allow web traffic:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Enable firewall:

```bash
sudo ufw enable
```

Check status:

```bash
sudo ufw status
```

---

## Enable HTTPS

Install SSL support:

```bash
sudo a2enmod ssl
```

Install Certbot:

```bash
sudo apt install certbot python3-certbot-apache
```

Request a certificate:

```bash
sudo certbot --apache
```

HTTPS protects:

- Passwords
- Session cookies
- Personal information
- API traffic

---

## Force HTTPS

Redirect all HTTP traffic to HTTPS.

Example:

```apache
<VirtualHost *:80>
ServerName example.com
Redirect permanent / https://example.com/
</VirtualHost>
```

---

## Configure Security Headers

Enable the headers module:

```bash
sudo a2enmod headers
```

Example:

```apache
Header always set X-Frame-Options SAMEORIGIN
Header always set X-Content-Type-Options nosniff
Header always set Referrer-Policy strict-origin
```

Benefits:

- Clickjacking protection
- MIME sniffing protection
- Improved privacy

---

## Protect Sensitive Files

Prevent access to hidden files.

Example:

```apache
<FilesMatch "^\.">
Require all denied
</FilesMatch>
```

Protects files such as:

```text
.htaccess
.git
.env
```

---

## Limit Request Size

Prevent oversized uploads.

Example:

```apache
LimitRequestBody 10485760
```

This limits uploads to approximately 10 MB.

---

## Disable Unnecessary HTTP Methods

Example:

```apache
<LimitExcept GET POST HEAD>
Require all denied
</LimitExcept>
```

Only allows:

- GET
- POST
- HEAD

---

## Enable mod_security

Install:

```bash
sudo apt install libapache2-mod-security2
```

Provides:

- SQL injection protection
- Cross-site scripting protection
- Request filtering

---

## Enable mod_evasive

Install:

```bash
sudo apt install libapache2-mod-evasive
```

Provides protection against:

- DoS attacks
- Brute-force attacks
- Excessive requests

---

## Secure File Permissions

Recommended ownership:

```bash
sudo chown -R www-data:www-data /var/www/html
```

Recommended permissions:

```bash
find /var/www/html -type d -exec chmod 755 {} \;
```

```bash
find /var/www/html -type f -exec chmod 644 {} \;
```

---

## Monitor Logs Regularly

Watch access logs:

```bash
tail -f /var/log/apache2/access.log
```

Watch error logs:

```bash
tail -f /var/log/apache2/error.log
```

Regular log monitoring helps detect:

- Failed attacks
- Configuration issues
- Application errors
- Unusual traffic patterns

---

# Apache Security Checklist

Before deploying a production server:

- [ ] Keep Ubuntu updated
- [ ] Keep Apache updated
- [ ] Enable HTTPS
- [ ] Install Certbot
- [ ] Hide Apache version information
- [ ] Disable directory listing
- [ ] Disable unused modules
- [ ] Configure firewall rules
- [ ] Enable security headers
- [ ] Install mod_security
- [ ] Install mod_evasive
- [ ] Secure file permissions
- [ ] Monitor logs regularly
- [ ] Create regular backups

Following these hardening practices significantly improves the security, reliability, and maintainability of Apache web servers in production environments.
