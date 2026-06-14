## Remédiation

### Contexte

Mimikatz accède à LSASS en mémoire pour extraire les hashes NTLM des sessions actives.
La prévention repose sur la protection de LSASS et la limitation des privilèges.

### Prévention

| Mesure                    | Description                                                                                                                      |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| LSA Protection (RunAsPPL) | Protège LSASS en le faisant tourner comme Protected Process Light. Mimikatz ne peut plus accéder à sa mémoire sans driver signé. |
| Credential Guard          | Isole les credentials dans un environnement virtualisé séparé. Rend sekurlsa::logonpasswords inefficace.                         |
| Least Privilege           | Sans droits SYSTEM, kiwi ne peut pas être chargé. Retirer alice des admins locaux casse la chaîne dès le scénario 03.            |

### Remédiation post-incident

1. Considérer tous les hashes dumpés comme compromis
2. Réinitialiser les mots de passe des comptes affectés
3. Activer LSA Protection sur tous les endpoints
4. Retracer les actions effectuées avec le hash avant détection