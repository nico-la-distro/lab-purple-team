## SIEM - Wazuh

### Accès

| Élément   | Valeur           |
| --------- | ---------------- |
| Dashboard | https://<IP_VPS> |

### Chemins utiles (VPS)

| Fichier | Chemin |
|---------|--------|
| Règles custom | `/var/ossec/etc/rules/local_rules.xml` |
| Config manager | `/var/ossec/etc/ossec.conf` |
| Logs manager | `/var/ossec/logs/ossec.log` |

### Commandes utiles

| Action | Commande |
|--------|----------|
| Redémarrer le manager | `sudo systemctl restart wazuh-manager` |
| Vérifier la syntaxe des règles | `sudo /var/ossec/bin/wazuh-logtest` |
| Lister les agents | `sudo /var/ossec/bin/agent_control -l` |
