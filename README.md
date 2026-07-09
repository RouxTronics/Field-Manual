# Overview

A reference deck for live engagements — HTB, THM, PicoCTF, and CPTS practice. Not a writeup archive (those live in the Quartz site's `Labs/` trees) and not a course-notes dump (HTB Academy content stays private in `Vault-Obsidian` per platform ToS). This is the stuff you want open in a pane while you're actually working a box.

Format per page: a **cheatsheet table** up top for copy-paste, then a short **why/when** underneath. Inspired in part by BRM's field-manual methodology (cheatsheet-first, explanation-second) — see Resources below.

## Phases

| #  | Phase                                                             | Covers                                                           |
| -- | ----------------------------------------------------------------- | ---------------------------------------------------------------- |
| 00 | [Pre-Engagement](/broken/pages/Ln1HGi7qDieGnTIRxZgC)              | Scope, ROE, environment setup, tmux                              |
| 01 | [Reconnaissance](/broken/pages/BgnZPwjFANvKFv5L0458)              | Passive OSINT, active scanning, web/vhost/param enum             |
| 02 | [Exploitation](/broken/pages/q0PjxfxwdrXSLxHW1rkE)                | Web app attacks, auth attacks, service-specific exploits         |
| 03 | [Post-Exploitation — Linux](/broken/pages/VwqjTpfUFRAlzpyFKfTK)   | Shell stabilization, situational awareness, privesc              |
| 04 | [Post-Exploitation — Windows](/broken/pages/rKKUJZOJeN34aVm287kF) | Situational awareness, privesc, AD attacks                       |
| 05 | [Lateral Movement & Shells](/broken/pages/98uwuZHvCLR78jvmW18Z)   | Pivoting, file transfer, reverse shells, payloads                |
| 06 | [Password Cracking](/broken/pages/wOIM8qj4EuCzU7TKtgmy)           | Hash ID, hashcat, John                                           |
| 07 | [Remediation & Reporting](/broken/pages/3pLiPFMrMoR37LDXeLyp)     | Finding structure, severity, report checklist, Obsidian template |

## Conventions

* Tool references link back to this vault's own [`Tools/`](https://github.com/RouxTronics/TryHackMe-FreeRoadMap/blob/main/Tools/README.md) directory where a STEMSecure page exists; otherwise an external link is given.
* Commands assume `$TARGET` is exported (`export TARGET=10.10.11.X`).
* Anything gated behind a platform's academic-integrity rules (module-specific answers, exam content) does not belong here — general technique and tooling only.

## Resources

| Resource                                                                        | Use                                         |
| ------------------------------------------------------------------------------- | ------------------------------------------- |
| [GTFOBins](https://gtfobins.github.io)                                          | SUID/sudo/capability shell escapes          |
| [LOLBAS](https://lolbas-project.github.io)                                      | Windows living-off-the-land binaries        |
| [HackTricks](https://book.hacktricks.xyz)                                       | Deep-dive technique reference               |
| [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)     | Payload library                             |
| [RevShells](https://www.revshells.com)                                          | Reverse shell generator                     |
| [BRM Field Manual](https://field-manual.brunorochamoura.com/)                   | Structural reference for this manual        |
| [Hacking Bible](https://www.hackthebox.com/blog/learn-to-hack-beginners-bible)  | HTB Blog of Hacking                         |
| [Pentest Tools ](https://www.hackthebox.com/blog/pentesting-tools-hackers-need) | HTB Blog of the 7 powerful pentesting tools |
