```xml
<!-- Local rules -->
<!-- Modify it at your will. -->
<!-- Copyright (C) 2015, Wazuh Inc. -->
<group name="local,sysmon,">

  <rule id="100002" level="10">
    <if_sid>61605</if_sid>
    <field name="win.eventdata.destinationPort">4444</field>
    <description>Suspicious outbound connection on port 4444 - possible reverse shell</description>
    <mitre>
      <id>T1571</id>
    </mitre>
  </rule>

  <rule id="100003" level="12">
    <if_sid>61605</if_sid>
    <field name="win.eventdata.image" type="pcre2">(?i)\\Users\\.+\\(Downloads|AppData|Temp)\\.+\.exe</field>
    <description>Outbound network connection from user directory - possible C2</description>
    <mitre>
      <id>T1095</id>
    </mitre>
  </rule>

  <rule id="100004" level="14">
    <if_sid>61603</if_sid>
    <field name="win.eventdata.parentImage" type="pcre2">(?i)\\fodhelper\.exe</field>
    <description>UAC bypass - fodhelper.exe spawned a child process (T1548.002)</description>
    <mitre>
      <id>T1548.002</id>
    </mitre>
  </rule>

  <rule id="100006" level="0">
    <if_sid>92900</if_sid>
    <field name="win.eventdata.sourceImage" type="pcre2">(?i)\\MsMpEng\.exe</field>
    <description>False positive - Defender MsMpEng.exe accessing LSASS</description>
  </rule>

  <rule id="100007" level="0">
    <if_sid>92307</if_sid>
    <field name="win.eventdata.image" type="pcre2">(?i)\\svchost\.exe</field>
    <description>False positive - svchost.exe service creation at boot, expected behavior</description>
  </rule>

</group>

<group name="local,windows">

  <rule id="100008" level="12">
    <if_sid>60103</if_sid>
    <field name="win.system.eventID">4698</field>
    <description>Scheduled task created - possible persistence (T1053.005)</description>
    <mitre>
      <id>T1053.005</id>
    </mitre>
  </rule>
  
  <rule id="100009" level="12">
    <if_sid>60103</if_sid>
    <field name="win.system.eventID">4768</field>
    <field name="win.eventdata.preAuthType">0</field>
    <description>AS-REP Roasting - TGT requested without pre-authentication (T1558.004)</description>
    <mitre>
      <id>T1558.004</id>
    </mitre>
  </rule>
  
  <rule id="100010" level="12">
    <if_sid>60103</if_sid>
    <field name="win.system.eventID">4769</field>
    <field name="win.eventdata.ticketEncryptionType">0x17</field>
    <description>Kerberoasting - TGS requested with RC4 encryption (T1558.003)</description>
    <mitre>
      <id>T1558.003</id>
    </mitre>
  </rule>

  <rule id="100011" level="15">
    <if_sid>60103</if_sid>
    <field name="win.system.eventID">4662</field>
    <field name="win.eventdata.properties" type="pcre2">1131f6aa|1131f6ad</field>
    <description>DCSync - Replication rights accessed on domain object (T1003.006)</description>
    <mitre>
      <id>T1003.006</id>
    </mitre>
  </rule>

  <rule id="100012" level="15">
    <if_sid>60103</if_sid>
    <field name="win.system.eventID">4769</field>
    <field name="win.eventdata.targetUserName" type="pcre2">(?i)^administrator@</field>
    <field name="win.eventdata.ipAddress" type="pcre2">(?!::1$)</field>
    <description>Golden Ticket - TGS requested for Administrator from external host (T1558.001)</description>
    <mitre>
      <id>T1558.001</id>
    </mitre>
  </rule>

</group>
```

