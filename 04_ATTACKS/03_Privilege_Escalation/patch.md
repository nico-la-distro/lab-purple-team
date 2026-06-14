## Remédiation

### Contexte

bypassuac_fodhelper nécessite qu'alice soit déjà membre du groupe local Administrators.
Sans cette condition, le module échoue. La vulnérabilité réelle est la présence d'alice
dans ce groupe, pas le contournement UAC en lui-même.

### Prévention

| Mesure          | Description                                                                                                                                           |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Least Privilege | Retirer alice du groupe local Administrators sur WS01. C'est la seule mesure qui empêche structurellement cette attaque.                              |

### Remédiation post-incident

1. Identifier la session SYSTEM ouverte via les alertes Wazuh (règle 100004)
2. Terminer le processus élevé sur WS01
3. Supprimer la clé de registre `HKCU\Software\Classes\ms-settings` sur WS01
4. Retracer les actions effectuées depuis la session SYSTEM avant détection
5. Revoir les membres du groupe local Administrators sur tous les postes du domaine