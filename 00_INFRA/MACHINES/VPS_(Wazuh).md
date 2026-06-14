## Specs

- 2 vCPU
- 4 Go RAM
- 80 Go disk
## Network

**Ethernet0**
- IP: Public IPv4
## OS

- Ubuntu Server 22.04.5 LTS
## Installation

1. Install Ubuntu server
2. Création d'un user avec droit **sudo**
3. Passer en authentification **SSH key** uniquement
4. Désactiver mot de passe `PasswordAuthentification no` & `PermitRootLogin no`
5. Install & configuration firewall **ufw**
6. Install & configuration **Fail2ban**
7. Install **Wazuh**
8. Vérifier que le **dashboard** Wazuh est accessible

## Security Hardening

- `PasswordAuthentication no`
- `PermitRootLogin no`
- SSH key authentication only
- UFW enabled
- Fail2ban enabled

## Role

- SIEM / Security Monitoring
- Centralized log collection
- Detection & alerting platform

