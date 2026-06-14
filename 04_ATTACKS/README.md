## Catalogue des attaques

| #   | Scénario             | Technique MITRE     | Statut |
| --- | -------------------- | ------------------- | ------ |
| 01  | Reverse Shell        | T1204.002           | ✅      |
| 02  | Local Recon          | T1082, T1016, T1069 | ✅      |
| 03  | Privilege Escalation | T1548.002           | ✅      |
| 04  | Credential Dump      | T1003.001           | ✅      |
| 05  | Persistence          | T1053.005           | ✅      |
| 06  | AS-REP Roasting      | T1558.004           | ✅      |
| 07  | Kerberoasting        | T1558.003           | ✅      |
| 08  | DCSync               | T1003.006           | ✅      |
| 09  | Golden Ticket        | T1558.001           | ✅      |

Chaque scénario est documenté en trois parties :
- `attack.md` contexte, technique, exécution
- `detection.md` pipeline de détection, règles Wazuh, analyse
- `patch.md` remédiation et mesures correctives

