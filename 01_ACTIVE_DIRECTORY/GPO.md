## GPO du domaine

| Nom                           | OU               | Type     | Objectif                                                 |
| ----------------------------- | ---------------- | -------- | -------------------------------------------------------- |
| USERS-WALLPAPER               | LAB/USERS        | User     | Fond d'écran domaine (test GPO)                          |
| WORKSTATIONS-FIREWALL-ICMP    | LAB/WORKSTATIONS | Computer | ICMP entrant autorisé                                    |
| WORKSTATIONS-DISABLE-LLMNR    | LAB/WORKSTATIONS | Computer | Désactiver LLMNR (réduction surface d'attaque)           |
| WORKSTATION-SMB-SIGNING       | LAB/WORKSTATIONS | Computer | Forcer la signature SMB                                  |
| DOMAIN-AUDIT-SCHTASK          | lab.local        | Computer | Audit création tâches planifiées (Event ID 4698)         |
| DOMAIN-AUDIT-KERBEROS         | lab.local        | Computer | Audit demandes TGT & TGS Kerberos (Event ID 4768 & 4769) |
| DOMAIN-AUDIT-DCSYNC           | lab.local        | Computer | Audit accès objets AD - DS Access (Event ID 4662)        |
