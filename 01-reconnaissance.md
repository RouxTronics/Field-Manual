# 01 - Reconnaissance

## Passive OSINT

| Tool         | Command                               | Purpose                    |
| ------------ | ------------------------------------- | -------------------------- |
| whois        | `whois target.com`                    | Registrar, dates, contacts |
| dig          | `dig any target.com @8.8.8.8`         | DNS records                |
| subfinder    | `subfinder -d target.com -o subs.txt` | Subdomain enum             |
| amass        | `amass enum -passive -d target.com`   | Passive subdomain enum     |
| theHarvester | `theHarvester -d target.com -b all`   | Emails, hosts, employees   |
| shodan       | `shodan search hostname:target.com`   | Exposed services           |

## Active Scanning — Nmap

```
nmap -sC -sV -oA nmap/initial $TARGET
nmap -p- --min-rate 5000 -oA nmap/allports $TARGET
nmap -p <ports> -sC -sV -oA nmap/targeted $TARGET
nmap -sU --top-ports 20 $TARGET          # UDP
nmap --script vuln -p <ports> $TARGET    # vuln scan
```

Standard flow: Rustscan for a fast full-port sweep → pull the open-port list into `$PORTS` → targeted Nmap deep scan against just those ports.

## Web Port Discovery

```
httpx -l hosts.txt -status-code -title -tech-detect
whatweb $TARGET
curl -v http://$TARGET/
```

## Web Enumeration

| Tool        | Command                                                                                                  |
| ----------- | -------------------------------------------------------------------------------------------------------- |
| feroxbuster | `feroxbuster -u http://$TARGET -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt` |
| gobuster    | `gobuster dir -u http://$TARGET -w /opt/SecLists/Discovery/Web-Content/big.txt -x php,html,txt`          |
| ffuf        | `ffuf -w wordlist.txt -u http://$TARGET/FUZZ -mc 200,301,302,403`                                        |

**Vhost enum**

```
ffuf -w subdomains.txt -u http://$TARGET -H "Host: FUZZ.target.htb" -fs <size>
gobuster vhost -u http://target.htb -w subdomains.txt --append-domain
```

Add anything discovered to `/etc/hosts` before continuing.

**Parameter discovery**

```
arjun -u http://$TARGET/endpoint -m GET
ffuf -w params.txt -u http://$TARGET/page?FUZZ=test
```

## Common Ports Quick Index

| Port    | Service    | Notes                                    |
| ------- | ---------- | ---------------------------------------- |
| 21      | FTP        | Try anonymous login                      |
| 22      | SSH        | Brute force; check for stale algos       |
| 23      | Telnet     | Plaintext — sniff or brute               |
| 25      | SMTP       | User enum, open relay                    |
| 53      | DNS        | Zone transfer: `dig axfr @target domain` |
| 80/443  | HTTP/S     | Main attack surface                      |
| 110     | POP3       | Email creds                              |
| 139/445 | SMB        | Shares, relay attacks, EternalBlue       |
| 389     | LDAP       | AD enumeration                           |
| 1433    | MSSQL      | `xp_cmdshell` RCE path                   |
| 3306    | MySQL      | UDF privesc                              |
| 3389    | RDP        | BlueKeep, brute force                    |
| 5432    | PostgreSQL | `COPY TO/FROM` RCE                       |
| 5985    | WinRM      | Evil-WinRM if creds available            |
| 6379    | Redis      | Unauthenticated RCE via module load      |
| 8080    | Alt HTTP   | Admin panels, Tomcat, Jenkins            |
| 27017   | MongoDB    | Often unauthenticated                    |
