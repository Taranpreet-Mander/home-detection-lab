# Home Detection Lab

my home lab for learning blue team / detection stuff. basically i attack a windows vm from kali and try to catch it with wazuh (a free SIEM).

im still learning so this is very much a work in progress. some of it works, some of it i'm still stuck on. writing it down so i don't forget.

## the idea

3 virtual machines on an isolated network:
- **kali** = the attacker (me pretending to be a hacker)
- **windows** = the victim, i put sysmon + wazuh agent on it for logging
- **ubuntu** = runs wazuh manager (the SIEM that collects logs and makes alerts)

```
kali (attacker)  --->  windows (victim, has sysmon + wazuh agent)
                              |
                              v
                       ubuntu (wazuh manager + dashboard)
```

all on a host-only network so it can't touch the internet or my real home network.

## tools

- VirtualBox - to run the VMs
- Kali Linux - attacker
- Windows 10 - victim
- Sysmon - better windows logging
- Wazuh - the SIEM (free!)

## setup guides

i'm writing these as i go:

- [01 - lab setup (vms + network)](docs/01-lab-setup.md)
- [02 - install wazuh](docs/02-wazuh-install.md)
- [03 - sysmon setup](docs/03-sysmon-setup.md)
- [04 - attack simulation](docs/04-attack-simulation.md)

## detections

when i catch an attack i write down how i did it in [detections/](detections/).
so far:
- failed logins / brute force - working
- suspicious powershell - kind of working, still tuning it

## stuff i still want to do

- [ ] get more sysmon detections working
- [ ] try atomic red team
- [ ] map everything to MITRE att&ck properly
- [ ] figure out why my custom rule sometimes doesn't fire (TODO)

## important - only do this in a lab

everything here is on my own isolated VMs. dont run attack tools against stuff you dont own, thats illegal. this is just for learning to defend.

## license

MIT, do whatever
