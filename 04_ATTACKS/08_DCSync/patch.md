## Remédiation

### Contexte

`svc-sync` possède des droits de réplication sur le domaine, ce qui lui permet
de demander tous les hashes NTLM via le protocole DRSUAPI sans toucher
à la mémoire d'une machine.

### Prévention

| Mesure | Description |
|--------|-------------|
| Least Privilege | Retirer les droits de réplication à tout compte qui n'est pas un DC. Seuls les contrôleurs de domaine en ont besoin. |
| Audit Directory Service Access | Activer via GPO pour détecter toute opération de réplication suspecte (Event ID 4662). |
| Mot de passe fort | Un compte avec droits de réplication doit avoir un mot de passe long et complexe. Le password spraying n'aurait pas fonctionné avec un mot de passe fort unique. |

### Remédiation post-incident

1. Identifier le compte via Wazuh (règle 100011)
2. Retirer immédiatement les droits de réplication de svc-sync
3. Considérer tous les hashes dumpés comme compromis
4. Réinitialiser les mots de passe de tous les comptes du domaine
5. Réinitialiser le mot de passe du compte `krbtgt` deux fois (invalidation des Golden Tickets)

