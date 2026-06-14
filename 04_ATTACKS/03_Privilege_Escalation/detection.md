## Détection

### Comment fonctionne bypassuac_fodhelper

Le module opère en deux temps :
1. Écriture d'une clé de registre sous `HKCU\Software\Classes\ms-settings\Shell\Open\command` avec le chemin du payload
2. Lancement de `fodhelper.exe`, un binaire Windows qui s'auto-élève sans prompt UAC, et exécute ce qu'il trouve dans cette clé

### Pipeline de détection

| Composant                | Rôle                                                                                                     |
| ------------------------ | -------------------------------------------------------------------------------------------------------- |
| Sysmon (SwiftOnSecurity) | Event ID 1 (Process Create)                                                                              |
| Wazuh agent              | Ingestion des logs Sysmon                                                                                |
| Règles custom Wazuh      | Wazuh dispose de règles natives pour cette technique. La règle custom complète avec un level plus élevé. |
### Règles natives déclenchées

| Rule ID | Level | Technique | Description |
|---------|-------|-----------|-------------|
| 92055 | 12 | T1548.002 | Known auto-elevated utility FodHelper.EXE may have been used to bypass UAC |
| 92304 | 6 | T1548.002, T1112 | Modified registry key associated to UAC bypass by auto-elevated processes |
### Règle 100004 - fodhelper spawn

```xml
<rule id="100004" level="14">
  <if_sid>61603</if_sid>
  <field name="win.eventdata.parentImage" type="pcre2">(?i)\\fodhelper\.exe</field>
  <description>UAC bypass - fodhelper.exe spawned a child process (T1548.002)</description>
  <mitre>
   <id>T1548.002</id>
  </mitre>
</rule>
```

| Champ            | Signification                                                 |
| ---------------- | ------------------------------------------------------------- |
| `if_sid 61603`   | Sysmon Event ID 1 : Process Create                            |
| `parentImage`    | Processus parent de la création                               |
| `fodhelper\.exe` | Aucune raison légitime pour fodhelper de spawner un processus |
| `level 14`       | Criticité très haute                                          |

> Pyramid of Pain : niveau **TTP** - fodhelper qui spawn un enfant est anormal quel que soit le payload.

**Limite** : ne couvre pas les autres binaires auto-élevés.

### Alertes Wazuh

![[wazuh_priv_esc_detection.png]]