## Endpoints

### Chemins utiles (Windows)

| Fichier | Chemin |
|---------|--------|
| Config agent Wazuh | `C:\Program Files (x86)\ossec-agent\ossec.conf` |
| Logs agent Wazuh | `C:\Program Files (x86)\ossec-agent\ossec.log` |
| Config Sysmon | `C:\sysmon\sysmonconfig.xml` |

### Commandes utiles

| Action | Commande |
|--------|----------|
| Redémarrer l'agent Wazuh | `Restart-Service Wazuh` |
| Recharger config Sysmon | `C:\sysmon\Sysmon64.exe -c C:\sysmon\sysmonconfig.xml` |
| Vérifier les GPO appliquées | `gpresult /r` |
| Forcer la mise à jour GPO | `gpupdate /force` |
| Vérifier l'audit | `auditpol /get /subcategory:"<nom>"` |