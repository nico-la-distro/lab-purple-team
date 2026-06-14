## Remédiation

### Contexte

La création d'une tâche planifiée depuis une session SYSTEM est une technique
de persistance courante. Elle survit aux redémarrages et s'exécute automatiquement
au login de l'utilisateur ciblé.

### Prévention

| Mesure | Description |
|--------|-------------|
| Audit Other Object Access Events | Activer via GPO pour détecter toute création de tâche (Event ID 4698). Non activé par défaut. |
| Least Privilege | Sans session SYSTEM, la création de tâches planifiées pour d'autres utilisateurs est bloquée. Retirer alice des admins locaux casse la chaîne dès le scénario 03. |
| AppLocker | Bloquer l'exécution de binaires depuis AppData. Empêche le payload de s'exécuter même si la tâche est créée. |

### Remédiation post-incident

1. Identifier la tâche malveillante via Wazuh (règle 100008) ou `schtasks /query`
2. Supprimer la tâche : `schtasks /delete /tn "OneDriveUpdate" /f`
3. Supprimer le payload : `del "C:\Users\alice.LAB\AppData\Local\Microsoft\OneDrive\OneDriveUpdater.exe"`
4. Retracer les actions effectuées depuis la session persistante
5. Auditer toutes les tâches planifiées sur les endpoints du domaine