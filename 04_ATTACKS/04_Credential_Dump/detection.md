## Détection

### Pipeline de détection

| Composant                | Rôle                                                                     |
| ------------------------ | ------------------------------------------------------------------------ |
| Sysmon (SwiftOnSecurity) | Capture Event ID 10 (ProcessAccess) après ajout manuel de la règle lsass |
| Wazuh agent              | Ingestion des logs Sysmon                                                |

> La config SwiftOnSecurity ne capture pas les Event ID 10 par défaut.
> Le bloc ProcessAccess est vide (onmatch="include" sans règle).
> Ajout manuel requis dans `sysmonconfig.xml` :

```xml
<ProcessAccess onmatch="include">
   <TargetImage condition="is">C:\Windows\System32\lsass.exe</TargetImage>
</ProcessAccess>
```

### Règle native déclenchée

| Rule ID | Level | Technique | Description |
|---------|-------|-----------|-------------|
| 92900 | 12 | T1003.001 | Lsass process was accessed by spoolsv.exe with read permissions, possible credential dump |

> spoolsv.exe accédant à LSASS est anormal.

### Alertes Wazuh

![[wazuh_lsass_detection.png]]

