# 01 - lab setup

setting up the 3 vms. this took me a while because i kept messing up the network part.

## what you need

- a pc with enough RAM. i have 16gb and running all 3 at once is fine-ish (kind of laggy tbh)
- like 100gb free space
- VirtualBox (free)

## the vms

| vm | os | ram i gave it | what its for |
|----|----|----|----|
| wazuh | ubuntu server 22.04 | 4gb | the SIEM |
| victim | windows 10 | 4gb | machine i defend |
| attacker | kali | 2gb | where attacks come from |

## step 1 - virtualbox

download from virtualbox.org, install, next next next.

## step 2 - get the OS isos

- ubuntu server: https://ubuntu.com/download/server
- kali (get the prebuilt virtualbox one, way easier): https://www.kali.org/get-kali/#kali-virtual-machines
- windows 10 eval from microsoft

## step 3 - the network (this is the important bit)

took me forever to understand this. you want a **host-only network** so the vms can talk to each other but NOT to the internet or your real wifi. keeps everything safe.

in virtualbox:
- File -> Tools -> Network Manager
- Host-only Networks -> Create
- mine came out as 192.168.56.0/24

then for each vm: Settings -> Network -> Adapter 1 -> "Host-only Adapter" -> pick the one you made.

> note: kali needs internet to install some tools. i temporarily added a second NAT adapter for that, then removed it after. dont leave it on.

## step 4 - ips

i gave them fixed ips so i can remember them:

- wazuh = 192.168.56.10
- victim = 192.168.56.20
- kali = 192.168.56.30

## step 5 - test

from kali:

```bash
ping 192.168.56.10
ping 192.168.56.20
```

if they reply you're good.

btw at first mine didnt ping and i spent an hour on it - turned out windows firewall blocks ping by default. had to allow ICMP. lol.

next: [02 - install wazuh](02-wazuh-install.md)
