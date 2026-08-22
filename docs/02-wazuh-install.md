# 02 - install wazuh

wazuh is the SIEM. two parts:
- manager + dashboard -> runs on ubuntu, collects logs and shows alerts
- agent -> runs on the windows victim, sends the logs over

## part A - install manager on ubuntu

theres an all-in-one installer which is nice. on the ubuntu vm:

```bash
curl -sO https://packages.wazuh.com/4.9/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

the -a means all-in-one. took like 10 min on my machine.

at the end it prints the admin password. SAVE IT. i didnt save it the first time and had to reinstall lol.

## part B - login

open https://192.168.56.10 in a browser on my host.
it gives a cert warning, thats normal (self signed), just continue.
login is admin + the password from before.

dashboard is empty until you add an agent.

## part C - install agent on windows

- in the dashboard: Agents -> Deploy new agent
- pick Windows, put in the manager ip (192.168.56.10), name it something like victim-win
- it gives you a command, copy it
- on windows, open powershell as admin, paste it
- then start the service:

```powershell
NET START WazuhSvc
```

## part D - check its connected

go back to Agents in the dashboard. after a minute victim-win should turn green / Active.

## problems i hit

- agent said "never connected" for ages -> was windows firewall again. also make sure the ip is right.
- couldnt open the dashboard -> i forgot the https, kept typing http.

next: [03 - sysmon setup](03-sysmon-setup.md)
