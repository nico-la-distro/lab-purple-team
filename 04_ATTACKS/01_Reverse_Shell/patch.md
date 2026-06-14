## Remédiation
### Contexte

Scénario post-phishing : alice exécute un payload reçu par mail.
Defender actif avec exclusions de dossiers dans le lab.

### Prévention

**Sensibilisation utilisateur**

Premier vecteur de défense contre le phishing : former les utilisateurs à ne pas exécuter de pièces jointes non sollicitées.

**AppLocker**

Bloquer l'exécution de tout `.exe` depuis `Downloads`, `AppData`, `Temp` via GPO :

`Computer Configuration → Windows Settings → Security Settings → Application Control Policies → AppLocker → Executable Rules`

### Confinement

| Mesure              | Description                                                                               |
| ------------------- | ----------------------------------------------------------------------------------------- |
| Least Privilege     | alice ne dispose que des droits strictement nécessaires, pas d'accès admin local          |
| Segmentation réseau | Les endpoints ne doivent pas être joignables directement depuis une machine non autorisée |
| Zero Trust          | Toute connexion sortante inhabituelle est bloquée par défaut                              |

### Remédiation post-incident

1. Isoler WS01 du réseau
2. Terminer le processus malveillant
3. Supprimer le payload
4. Analyser les logs Wazuh pour retracer les actions effectuées
5. Réinitialiser les credentials de Alice