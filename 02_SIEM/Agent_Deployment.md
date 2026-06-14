## Objectif

Déployer les agents Wazuh sur les endpoints Windows pour centraliser 
la collecte de logs vers le SIEM.

## Hosts

- DC01 -> 192.168.10.100
- WS01 -> 192.168.10.101

## Installation

Sur chaque endpoint (PowerShell admin) :

1. Télécharger l'agent depuis le dashboard Wazuh

```powershell
Invoke-WebRequest -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.7.5-1.msi -OutFile ${env.tmp}\wazuh-agent; msiexec.exe /i ${env.tmp}\wazuh-agent /q WAZUH_MANAGER='<IP_WAZUH>' WAZUH_REGISTRATION_SERVER='<IP_WAZUH>'
```

2. Démarrer le service :

```powershell
NET START WazuhSvc
```

## Validation

```powershell
Get-Service WazuhSvc
```

- Agent visible dans le dashboard Wazuh ✅
- Logs reçus sur le manager ✅
- Statut : Active ✅