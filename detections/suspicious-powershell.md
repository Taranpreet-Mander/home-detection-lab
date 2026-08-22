# suspicious powershell

mitre: T1059.001 (powershell) - https://attack.mitre.org/techniques/T1059/001/
log source: sysmon
event id: 1 (process creation)

## what it looks like

attackers use powershell to download and run code without saving a file. things to look for in the command line:
- DownloadString / DownloadFile (grabbing code off the internet)
- IEX / Invoke-Expression (running text as code)
- -enc / -EncodedCommand (base64 to hide what its doing)
- -w hidden / -nop (running quietly)

## how i detected it

sysmon event 1 has the full command line. search in wazuh:

```
data.win.eventdata.image: *powershell.exe* and data.win.eventdata.commandLine: *DownloadString*
```

### my custom rule (WIP - see note below)

in /var/ossec/etc/rules/local_rules.xml:

```xml
<group name="local,sysmon,">
  <rule id="100200" level="12">
    <if_group>sysmon_event1</if_group>
    <field name="win.eventdata.commandLine" type="pcre2">(?i)(downloadstring|invoke-expression|\biex\b|-enc\b|encodedcommand)</field>
    <description>suspicious powershell command line</description>
    <mitre>
      <id>T1059.001</id>
    </mitre>
  </rule>
</group>
```

restart: sudo systemctl restart wazuh-manager

**note to self:** this rule fires sometimes but not always and i cant figure out why yet. i think the if_group might be wrong, or the field name. the plain search above always finds it though so the logging is fine, its just my rule thats broken. TODO fix this.

## why it works

admins use these sometimes, but the combo - hidden window + encoded + downloading something - all at once is a strong sign. encoding especially, honest scripts dont usually need to hide.

## false positives

- some legit admin scripts / installers use encoded commands
- fix by allowlisting known scripts or only alerting when a few of these show up together
