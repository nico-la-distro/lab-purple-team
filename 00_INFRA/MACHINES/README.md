
| Machine       | Rôle                   | OS             | vCPU         | RAM                 | Stockage      |
| ------------- | ---------------------- | -------------- | ------------ | ------------------- | ------------- |
| HOST-WIN11    | Hôte VMware            | Windows 11     | CPU physique | **4–5 Go réservés** | SSD principal |
| DC01          | Active Directory + DNS | Windows Server | 2 vCPU       | **3 Go**            | 40–60 Go      |
| WS01          | Client domaine         | Windows        | 2 vCPU       | **2 Go**            | 40 Go         |
| Kali          | Machine attaquante     | Kali Linux     | 2 vCPU       | **3–4 Go**          | 30–40 Go      |
| WAZUH-VPS     | SIEM / SOC             | Wazuh          | VPS          | **RAM cloud**       | VPS storage   |
| **Total RAM** |                        |                |              | **~13-14Go / 16Go** |               |

