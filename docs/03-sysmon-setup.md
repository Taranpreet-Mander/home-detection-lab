# 03 - sysmon setup

default windows logs dont show much. sysmon (its a microsoft tool) adds way more detail like what process started what, network connections etc. this is what makes detections actually possible.

## step 1 - download sysmon

on the windows vm, get it from microsoft:
https://learn.microsoft.com/sysinternals/downloads/sysmon

unzip to like C:\Tools\Sysmon

## step 2 - get a config

sysmon does basically nothing without a config telling it what to log. everyone recommends the SwiftOnSecurity one so i used that:
https://github.com/SwiftOnSecurity/sysmon-config

download sysmonconfig-export.xml into the same folder.

## step 3 - install it

powershell as admin, in the sysmon folder:

```powershell
.\Sysmon64.exe -accepteula -i sysmonconfig-export.xml
```

## step 4 - check its logging

Event Viewer -> Applications and Services Logs -> Microsoft -> Windows -> Sysmon -> Operational
you should see events. Event ID 1 = a process started, thats the main one.

## step 5 - make wazuh read sysmon

edit the agent config:
C:\Program Files (x86)\ossec-agent\ossec.conf

add this inside the config block:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

restart the agent:

```powershell
NET STOP WazuhSvc
NET START WazuhSvc
```

## step 6 - confirm

open notepad or run a command on windows, then look in the wazuh dashboard (Discover / security events). sysmon events should show up.

TODO: i think my config might be logging too much, dashboard is kinda noisy. need to look into tuning it.

next: [04 - attack simulation](04-attack-simulation.md)
