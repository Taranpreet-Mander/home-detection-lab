# failed logins / brute force

mitre: T1110 (brute force) - https://attack.mitre.org/techniques/T1110/
log source: windows security log
event ids: 4625 (failed logon), 4624 (successful)

## what it looks like

attacker tries loads of passwords fast. shows up as tons of 4625 failed logons in a short time, usually from one ip.

## how i detected it

good news, wazuh catches this out of the box. rules to look for:
- 60122 = windows logon failure
- 60204 = multiple failures (this is the one that means "hey this is probably an attack")

in the dashboard, security events, filter:

```
rule.id: 60204
```

or search the raw event:

```
data.win.system.eventID: 4625
```

### my own rule (i wanted to try writing one)

put this in /var/ossec/etc/rules/local_rules.xml on the manager:

```xml
<group name="local,authentication_failures,">
  <rule id="100100" level="10" frequency="8" timeframe="60">
    <if_matched_sid>60122</if_matched_sid>
    <description>possible brute force: 8+ failed logons in 60s</description>
    <mitre>
      <id>T1110</id>
    </mitre>
  </rule>
</group>
```

then restart: sudo systemctl restart wazuh-manager

## why it works

normal people mistype their password once or twice. 8 fails in a minute from one ip is a bot, not a person. the frequency + timeframe is what turns "a failure" into "an attack".

## false positives

- someone who genuinely forgot their password (but thats like 1-3 tries not 8)
- a service account with an old password retrying in a loop
- can fix by raising the threshold or ignoring known accounts
