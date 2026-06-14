## Détection

### Pipeline de détection

| Composant | Rôle |
|-----------|------|
| Windows Security Log (DC01) | Génère Event ID 4769 à chaque demande de TGS Kerberos |
| Wazuh agent | Ingestion du canal Security de DC01 |
| Règle custom Wazuh | Aucune règle native pour T1558.003, règle custom nécessaire |

> L'Event ID 4769 nécessite l'activation de l'audit Kerberos via GPO.
> Ajouté dans la GPO `DOMAIN-AUDIT-KERBEROS` existante.

### Règle custom 100010

```xml
<rule id="100010" level="12">
  <if_sid>60103</if_sid>
  <field name="win.system.eventID">4769</field>
  <field name="win.eventdata.ticketEncryptionType">0x17</field>
  <description>Kerberoasting - TGS requested with RC4 encryption (T1558.003)</description>
  <mitre>
    <id>T1558.003</id>
  </mitre>
</rule>
```

| Champ | Signification |
|-------|--------------|
| `if_sid 60103` | Règle parent Wazuh pour les events Windows Security |
| `eventID 4769` | A Kerberos service ticket was requested |
| `ticketEncryptionType 0x17` | RC4 - anormal sur un environnement moderne, signal caractéristique du Kerberoasting |
| `level 12` | Criticité haute |

> RC4 (0x17) est anormal sur un environnement moderne qui utilise AES. C'est le signal caractéristique du Kerberoasting.

### Champs clés (Event ID 4769)

| Champ | Valeur |
|-------|--------|
| targetUserName | alice@LAB.LOCAL |
| serviceName | svc-web |
| ipAddress | ::ffff:192.168.10.200 (Kali) |
| ticketEncryptionType | 0x17 (RC4, crackable offline) |

### Alertes Wazuh

![[wazuh_kerberoasting_detection.png]]