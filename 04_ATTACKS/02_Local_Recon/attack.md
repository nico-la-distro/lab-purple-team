## Attaque
### Contexte

Après avoir obtenu un accès initial via Reverse Shell, l'attaquant effectue 
une reconnaissance locale pour cartographier l'environnement avant de 
continuer la chaîne d'attaque.

### Technique MITRE

| ID    | Technique                              | Tactique  |
| ----- | -------------------------------------- | --------- |
| T1082 | System Information Discovery           | Discovery |
| T1069 | Permission Groups Discovery            | Discovery |
| T1016 | System Network Configuration Discovery | Discovery |

### Prérequis

| Élément | Valeur                                 |
| ------- | -------------------------------------- |
| Accès   | Session Meterpreter active (LAB\alice) |
| Cible   | WS01 - 192.168.10.101                  |

### Exécution

Depuis la session Meterpreter :

```meterpreter
sysinfo
getuid
```

![[recon_basics.png]]

Puis depuis un shell Windows :

```meterpreter
shell
```

```cmd
net user alice /domain
```

![[net_user_alice_domain.png]]


```cmd
net group "Domain Admins" /domain
```

![[net_group_domain_admins_domain.png]]


```cmd
net localgroup Administrators
```

![[net_localgroup_administrators.png]]


```cmd
arp -a
```

```cmd
ipconfig /all
```

### Résultats

| Info             | Valeur                                      |
| ---------------- | ------------------------------------------- |
| Machine          | WS01                                        |
| Utilisateur      | LAB\alice                                   |
| Privilèges       | Domain Users -> pas admin                   |
| Domain Admins    | Administrator                               |
| DC               | 192.168.10.100                              |
| Admin local WS01 | Administrator, LAB\alice, LAB\Domain Admins |
| Domain           | lab.local                                   |
### Conclusion

alice est un compte standard sans privilèges de domaine, mais dispose des droits administrateur local sur WS01. L'objectif suivant : exploiter cette configuration pour élever les privilèges vers SYSTEM.