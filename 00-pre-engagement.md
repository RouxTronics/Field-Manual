# Page 1

## Scope & Rules of Engagement

| Item                | Notes                                              |
| ------------------- | -------------------------------------------------- |
| Target IPs / ranges | Get written authorization before scanning anything |
| Excluded systems    | List explicitly in the ROE doc                     |
| Testing window      | Hours you're permitted to operate                  |
| Emergency contact   | Client SOC/NOC number for critical findings        |
| Data handling       | What can actually be exfiltrated for PoC           |
| Reporting deadline  | Confirm format up front (PDF, Word, GitBook)       |

For HTB/THM/PicoCTF labs the ROE is the platform's ToS — the main things that matter are not sharing active-machine solutions publicly and not touching out-of-scope hosts on a shared VPN.

## Environment Checklist

* [ ] Parrot/Kali on bare metal or dedicated VM
* [ ] VPN connected to the target range
* [ ] Obsidian engagement note created from the [template](/broken/pages/3pLiPFMrMoR37LDXeLyp#obsidian-note-template)
* [ ] tmux session up: recon / shell / notes / proxychains panes
* [ ] `export TARGET=10.10.11.X`
* [ ] Burp running, browser proxied
* [ ] Screenshot tool ready (Flameshot)

## tmux Quick Reference

| Keybind        | Action                    |
| -------------- | ------------------------- |
| `Ctrl+B %`     | Split pane vertical       |
| `Ctrl+B "`     | Split pane horizontal     |
| `Ctrl+B` arrow | Navigate panes            |
| `Ctrl+B z`     | Zoom/unzoom pane          |
| `Ctrl+B [`     | Scroll mode (`q` to exit) |
| `Ctrl+B d`     | Detach session            |
