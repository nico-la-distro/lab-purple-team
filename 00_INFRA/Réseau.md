## Schéma Réseau

![](screenshots/infra_lab_purple_team.png)

---
## Table d'adressage IP

| Machine | Rôle           | IP             | OS             |
| ------- | -------------- | -------------- | -------------- |
| DC01    | AD / DNS       | 192.168.10.100 | Windows Server |
| WS01    | Client domaine | 192.168.10.101 | Windows 10     |
| Kali    | Attaquant      | 192.168.10.200 | Kali Linux     |
| WAZUH   | SIEM           | Public IP      | Ubuntu server  |

> Kali est sur le même réseau que l'AD pour simplifier l'infra. En conditions réelles, l'attaquant serait sur un réseau externe avec une IP publique.

---
## Domaine AD

| Élément       | Valeur            |
| ------------- | ----------------- |
| Domaine AD    | `lab.local`       |
| Subnet        | `192.168.10.0/24` |
| DNS principal | `192.168.10.100`  |
| DC principal  | `DC01`            |
| Réseau VMware | `Host-Only`       |

