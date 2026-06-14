## Troubleshooting - Détection Wazuh

### Workflow quand une alerte ne remonte pas

#### Étape 1 - Vérifier que l'event existe en local sur la machine

Sur Windows (PowerShell admin) :

```powershell
Get-WinEvent -LogName Security -MaxEvents 100 | Where-Object {$_.Id -eq <EventID>} | Select-Object -First 3
```

Pour Sysmon :

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 100 | Where-Object {$_.Id -eq <EventID>} | Select-Object -First 3
```

Si rien ne remonte : l'audit ou la config Sysmon n'est pas activé.

#### Étape 2 - Vérifier que Wazuh reçoit l'event

Dans Wazuh Discover :

```
agent.name: <MACHINE> AND data.win.system.eventID: <EventID>
```

Si rien ne remonte mais l'étape 1 fonctionne : Wazuh reçoit l'event mais aucune règle ne l'affiche dans Security Alerts. Passer à l'étape 3.

#### Étape 3 - Créer une règle custom

Si l'event remonte dans Discover mais pas dans Security Alerts, créer une règle dans `/var/ossec/etc/rules/local_rules.xml` :

```xml
<rule id="10xxxx" level="12">
  <if_sid>60103</if_sid>
  <field name="win.system.eventID"><EventID></field>
  <description>Description de l'alerte</description>
  <mitre>
    <id>TXXXX</id>
  </mitre>
</rule>
```

Puis redémarrer le manager :

```bash
sudo systemctl restart wazuh-manager
```

### Audits Windows à activer selon le scénario

| Audit | Chemin GPO | Events générés |
|-------|-----------|----------------|
| Other Object Access Events | Object Access | 4698 (tâches planifiées) |
| Kerberos Authentication Service | Account Logon | 4768 (TGT) |
| Kerberos Service Ticket Operations | Account Logon | 4769 (TGS) |
| Directory Service Access | DS Access | 4662 (accès objets AD) |
