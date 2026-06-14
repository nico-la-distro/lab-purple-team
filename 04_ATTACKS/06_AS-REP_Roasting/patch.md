## Remédiation

### Contexte

Le compte `svc-backup` a la pré-authentification Kerberos désactivée.
N'importe qui peut demander un TGT pour ce compte sans fournir de credentials,
et cracker le hash offline.

### Prévention

| Mesure | Description |
|--------|-------------|
| Activer la pré-authentification | Retirer le flag DoesNotRequirePreAuth sur svc-backup. C'est la correction directe. |
| Mot de passe fort | Un mot de passe long et complexe rend le crack offline infaisable même si le hash est obtenu. |
| Audit Kerberos | Activer l'Event ID 4768 via GPO pour détecter toute demande de TGT anormale. |

### Remédiation post-incident

1. Identifier le compte via Wazuh (règle 100009) ou les logs DC01
2. Réinitialiser le mot de passe de svc-backup
3. Activer la pré-authentification sur svc-backup :

```powershell
Set-ADUser -Identity svc-backup -DoesNotRequirePreAuth $false
```

4. Auditer tous les comptes du domaine avec DoesNotRequirePreAuth activé :

```powershell
Get-ADUser -Filter {DoesNotRequirePreAuth -eq $true} -Properties DoesNotRequirePreAuth | Select-Object Name, SamAccountName
```

