# Page 1

## Hash Identification

```
hash-identifier <hash>
hashid <hash>
```

| Hash type        | Signature                           |
| ---------------- | ----------------------------------- |
| MD5              | 32 chars                            |
| SHA1             | 40 chars                            |
| SHA256           | 64 chars                            |
| NTLM             | 32 chars, unsalted (looks like MD5) |
| bcrypt           | starts `$2y$` or `$2a$`             |
| SHA512crypt      | starts `$6$`                        |
| Kerberos 5 (TGS) | starts `$krb5tgs$`                  |

## Hashcat

| Mode  | Type        | Command                                               |
| ----- | ----------- | ----------------------------------------------------- |
| 1000  | NTLM        | `hashcat -m 1000 hash.txt rockyou.txt -r best64.rule` |
| 0     | MD5         | `hashcat -m 0 hash.txt rockyou.txt`                   |
| 1800  | SHA512crypt | `hashcat -m 1800 hash.txt rockyou.txt --force`        |
| 13100 | Kerberoast  | `hashcat -m 13100 tgs.txt rockyou.txt`                |
| 18200 | AS-REP      | `hashcat -m 18200 asrep.txt rockyou.txt`              |
| 16500 | JWT         | `hashcat -m 16500 jwt.txt rockyou.txt`                |
| 3200  | bcrypt      | `hashcat -m 3200 hash.txt rockyou.txt` (slow)         |

## John the Ripper

```
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
john hash.txt --format=NT --wordlist=rockyou.txt
john --show hash.txt
ssh2john id_rsa > id_rsa.hash && john id_rsa.hash --wordlist=rockyou.txt
zip2john protected.zip > zip.hash && john zip.hash --wordlist=rockyou.txt
```
