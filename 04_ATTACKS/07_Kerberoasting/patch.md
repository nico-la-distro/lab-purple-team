## Remédiation

### Contexte

Le compte `svc-web` a un SPN configuré et un mot de passe faible.
Tout utilisateur de domaine peut demander un TGS pour ce compte et le cracker offline.

### Prévention

| Mesure | Description |
|--------|-------------|
| Mot de passe fort | Un mot de passe long (+25 caractères) rend le crack offline infaisable même si le hash est obtenu. |
| AES uniquement | Configurer les comptes de service pour n'accepter que AES. Empêche GetUserSPNs de forcer RC4. |
| Managed Service Accounts (MSA) | Windows gère automatiquement les mots de passe des comptes de service. Mots de passe complexes renouvelés automatiquement. |

### Remédiation post-incident

1. Identifier le compte via Wazuh (règle 100010)
2. Réinitialiser le mot de passe de svc-web avec un mot de passe fort
3. Auditer tous les comptes avec SPN configuré

