## Utilisateurs du domaine

| User       | OU           | Description                | Vulnérabilité                          | Scénario                        |
| ---------- | ------------ | -------------------------- | -------------------------------------- | ------------------------------- |
| alice      | LAB/USERS    | Utilisateur standard       | Admin local WS01 + mot de passe faible | Pivot (Reverse Shell → PrivEsc) |
| svc-web    | LAB/SERVICES | Compte de service - Web    | SPN configuré                          | Kerberoasting                   |
| svc-backup | LAB/SERVICES | Compte de service - Backup | No pre-auth Kerberos                   | AS-REP Roasting                 |
| svc-sync   | LAB/SERVICES | Compte de service - Sync   | Droits réplication                     | DCSync                          |

> Mots de passe volontairement faibles et vulnérabilités intentionnelles 
> pour simuler des négligences courantes en entreprise.
> Chaque scénario est documenté dans `04_ATTACKS/`.

## Commandes de configuration

### svc-backup - No pre-auth Kerberos (AS-REP roasting)

```powershell
Set-ADAccountControl -Identity "svc-backup" -DoesNotRequirePreAuth $true
```

### svc-web - SPN (Kerberoasting)

```powershell
Set-ADUser -Identity "svc-web" -ServicePrincipalNames @{Add="HTTP/svc-web.lab.local"}
```

### svc-sync - Droits de réplication (DCSync)

```
Active Directory Users and Computers
→ View → Advanced Features (activer)
→ Clic droit sur lab.local → Properties
→ Security → Advanced → Add
→ Sélectionner svc-sync
→ Cocher "Replicating Directory Changes" et "Replicating Directory Changes All"
→ OK
```

