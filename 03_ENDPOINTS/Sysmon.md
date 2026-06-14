## Objectif

Enrichir la télémétrie des endpoints Windows en déployant Sysmon avec une 
configuration de référence. Les logs natifs Windows ne capturent pas suffisamment 
de détails pour une détection efficace : Sysmon ajoute la visibilité sur les 
créations de processus, connexions réseau, modifications du registre, etc.

## Config utilisée

SwiftOnSecurity - sysmonconfig-export.xml
https://github.com/SwiftOnSecurity/sysmon-config

## Déploiement

Hosts : DC01, WS01 (PowerShell admin)

```powershell
mkdir C:\sysmon

cd C:\sysmon

Invoke-WebRequest -Uri https://download.sysinternals.com/files/Sysmon.zip -OutFile Sysmon.zip

Expand-Archive .\Sysmon.zip -DestinationPath .

Invoke-WebRequest -Uri "https://raw.githubusercontent.com/SwiftOnSecurity/sysmon-config/master/sysmonconfig-export.xml" -OutFile C:\sysmon\sysmonconfig.xml

.\Sysmon64.exe -accepteula -i C:\sysmon\sysmonconfig.xml
```

## Intégration Wazuh

Ajout dans `ossec.conf` sur chaque agent :

C:\Program Files (x86)\ossec-agent\ossec.conf

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

```powershell
Restart-Service WazuhSvc
```

## Validation

```powershell
Get-Service Sysmon64
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 5
```

- Service Sysmon64 : Running ✅
- Logs Sysmon visibles dans Wazuh dashboard ✅