## Catalogue des attaques

| #   | Scénario             | Technique MITRE     | Tactique             |
| --- | -------------------- | ------------------- | -------------------- |
| 01  | [Reverse Shell](01_Reverse_Shell/attack.md)        | T1204.002           | Execution            |
| 02  | [Local Recon](02_Local_Recon/attack.md)          | T1082, T1016, T1069 | Discovery            |
| 03  | [Privilege Escalation](03_Privilege_Escalation/attack.md) | T1548.002           | Privilege Escalation |
| 04  | [Credential Dump](04_Credential_Dump/attack.md)      | T1003.001           | Credential Access    |
| 05  | [Persistence](05_Persistence/attack.md)          | T1053.005           | Persistence          |
| 06  | [AS-REP Roasting](06_AS-REP_Roasting/attack.md)      | T1558.004           | Credential Access    |
| 07  | [Kerberoasting](07_Kerberoasting/attack.md)        | T1558.003           | Credential Access    |
| 08  | [DCSync](08_DCSync/attack.md)               | T1003.006           | Credential Access    |
| 09  | [Golden Ticket](09_Golden_Ticket/attack.md)        | T1558.001           | Persistence          |


Chaque scénario est documenté en trois parties :
- `attack.md` contexte, technique, exécution
- `detection.md` pipeline de détection, règles Wazuh, analyse
- `patch.md` remédiation et mesures correctives

