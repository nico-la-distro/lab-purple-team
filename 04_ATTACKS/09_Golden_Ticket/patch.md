## Remédiation

### Contexte

Le Golden Ticket exploite le hash de `krbtgt`. Tant que ce hash n'est pas
changé, tous les tickets forgés restent valides. C'est la persistance ultime :
même si l'attaquant perd son accès, ses tickets fonctionnent encore.

### Prévention

| Mesure                         | Description                                                                                                               |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| Protéger le DCSync             | La vraie prévention est en amont : empêcher le DCSync (scénario 08) qui donne le hash krbtgt.                             |
| Réinitialiser krbtgt deux fois | Changer le mot de passe de krbtgt invalide tous les tickets forgés. Deux fois car Windows garde l'ancien hash en mémoire. |
| Tiering model                  | Isoler les comptes admin de domaine sur des postes dédiés. Limite la surface d'attaque.                                   |

### Remédiation post-incident

1. Identifier l'activité via Wazuh (règle 100012)
2. Réinitialiser le mot de passe de `krbtgt` deux fois à 10 heures d'intervalle
3. Forcer la déconnexion de toutes les sessions actives
4. Auditer tous les accès à DC01 pendant la période de compromission

