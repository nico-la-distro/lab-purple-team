## Remédiation
### Contexte

L'énumération AD avec les commandes `net` est difficilement bloquable sans 
impacter le fonctionnement normal du domaine. La détection et la réponse 
rapide restent la mesure prioritaire.

### Remédiation

| Mesure              | Description                                                                                                                                                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Alerting            | Activer les notifications sur les règles 92039/92031 dans Wazuh                                                                                                                                                                             |
| Behavioral baseline | Une rafale de commandes `net` sur une courte période depuis `C:\Users\<user>\Downloads\` est un signal d'énumération. Créer une règle custom Wazuh qui corrèle plusieurs déclenchements de 92039 en moins de 60 secondes sur le même agent. |

**Behavioral baseline custom rule**

```xml
<rule id="100100" level="12" frequency="3" timeframe="60">
  <if_matched_sid>92039</if_matched_sid>
  <same_field>agent.name</same_field>
  <description>AD enumeration burst - multiple net.exe discovery commands within 60s</description>
  <mitre>
    <id>T1087</id>
  </mitre>
</rule>
```

### Remédiation post-incident

1. Identifier la source de l'énumération via les logs Wazuh
2. Isoler WS01
3. Investiguer comment l'attaquant a obtenu le foothold initial

