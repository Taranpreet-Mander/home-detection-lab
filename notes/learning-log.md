# learning log

just notes to myself as i go. newest on top.

---

## suspicious powershell + stuck on my rule

got the powershell test working - sysmon logs the full command line which is cool. the plain search finds it fine.

BUT my custom rule for it (100200) only fires sometimes and i cant work out why. spent way too long on this. gonna move on and come back. the built in wazuh stuff catches some of it anyway.

learned: sysmon event id 1 = process creation, and it has the whole command line which is gold.

---

## first detection working!!

did the hydra brute force thing and wazuh actually alerted on it (rule 60204). first real detection, pretty hyped.

also tried writing my OWN rule for it and it worked. starting to get how the frequency/timeframe thing works.

---

## sysmon

installed sysmon with the swiftonsecurity config. default windows logs are useless, sysmon shows way more. dashboard got noisy though, might be logging too much.

---

## got wazuh running

the all-in-one installer worked. forgot to save the admin password the first time and had to redo the whole install lol. saved it this time.

agent took a while to connect - windows firewall again.

---

## day 1 - setting up

figured out what a detection lab even is. attacker + victim + SIEM on an isolated network. blue team = defenders (thats what im learning).

hardest part so far was the virtualbox host-only network. also windows firewall blocking ping wasted an hour.

still confused about:
- how much of the wazuh rule language i actually need to learn
- difference between sysmon logs and normal windows logs (getting clearer now)
