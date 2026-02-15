# Projet : Simulation d'une Architecture Réseau d'Entreprise sous GNS3 (Windows x86)

## Objectif
Créer une infrastructure réseau d'entreprise virtuelle sous **GNS3** reproduisant les différents départements d'une organisation (IT, Finance, RH, Marketing), avec routage, firewall, Active Directory, serveurs internes et segmentation VLAN.

---

## 1. Prérequis Matériels
- **PC Windows  (x86_64)**
  - CPU : 4 cœurs minimum
  - RAM : 16 Go (32 Go idéal)
  - Disque : 100 Go libres (SSD conseillé)
- **Connexion Internet** stable
- **Mac Routeur Image** : https://github.com/GNS3/gns3-gui/releases
---

## 2. Logiciels à installer

### 2.1. GNS3
- Téléchargement : [https://www.gns3.com/software/download](https://www.gns3.com/software/download)
- Installe **GNS3 + GNS3 VM (VMware Workstation Player)**
  - VMware Player (gratuit) : [https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)

### 2.2. Images systèmes à importer dans GNS3

| Rôle | Système | Format | Lien officiel |
|--------|------------|---------|----------------|
| Firewall / Routeur | **pfSense** | `.iso` | [https://www.pfsense.org/download/](https://www.pfsense.org/download/) |
| Routeur secondaire | **VyOS** | `.iso` | [https://vyos.net/get/](https://vyos.net/get/) |
| Serveur AD / DNS | **Windows Server 2019/2022 (Evaluation)** | `.iso` | [https://www.microsoft.com/evalcenter/](https://www.microsoft.com/evalcenter/) |
| Postes clients | **Windows 10 / 11 (Evaluation)** | `.iso` | [https://www.microsoft.com/software-download/](https://www.microsoft.com/software-download/) |
| Serveur Linux / Web | **Ubuntu Server 22.04 LTS** | `.iso` | [https://ubuntu.com/download/server](https://ubuntu.com/download/server) |
| Attaquant / Pentest | **Kali Linux** | `.iso` | [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/) |
| SIEM / IDS | **Wazuh / Suricata / ELK** | Docker / `.ova` | [https://wazuh.com/downloads/](https://wazuh.com/downloads/) |

---

## 3. Architecture du Réseau

### 3.1. Schéma logique
```
                   [Internet]
                        |
                  +---------------+
                  |   pfSense FW  |
                  +---------------+
                    |     |     |
        VLAN10-IT   |     |    VLAN30-Marketing
                  VLAN20-Finance  VLAN40-RH
                    |     |     |
         [Srv-IT] [Srv-Fin] [Srv-Mark] [Srv-RH]
                    |     |     |
               [PCs par département]
```

### 3.2. Plan d'adressage (exemple)
| VLAN | Département | Adresse | Plage IP |
|------|---------------|----------|-----------|
| 10 | IT | 10.10.10.1/24 | 10.10.10.0 - 10.10.10.255 |
| 20 | Finance | 10.10.20.1/24 | 10.10.20.0 - 10.10.20.255 |
| 30 | Marketing | 10.10.30.1/24 | 10.10.30.0 - 10.10.30.255 |
| 40 | RH | 10.10.40.1/24 | 10.10.40.0 - 10.10.40.255 |
| 99 | Management (Admin/SIEM) | 10.10.99.1/24 | 10.10.99.0 - 10.10.99.255 |

---

## 4. Déploiement dans GNS3

### 4.1. Importation des images
1. Ouvrir **GNS3** > *New Template* > *QEMU VM* > *Use existing image*.
2. Choisir l'image `.iso` ou `.qcow2` correspondante.
3. Définir la RAM :
   - pfSense : 1 Go
   - Windows Server : 4 Go
   - Windows 10 : 2 Go
   - Ubuntu : 1 Go
   - Wazuh : 2 Go

### 4.2. Configuration du pare-feu pfSense
1. Installer pfSense via ISO.
2. Attribuer les interfaces :
   - `WAN` : Internet (Cloud GNS3 ou NAT)
   - `LAN` : 10.10.10.1/24 (interne)
3. Créer des VLANs (10, 20, 30, 40, 99).
4. Activer le DHCP par VLAN.
5. Configurer le NAT pour la sortie Internet.

### 4.3. Routeurs secondaires (VyOS)
- Permet d'interconnecter des sous-réseaux ou de simuler un backbone.
- Exemple :
```bash
configure
set interfaces ethernet eth0 address 10.10.99.2/24
set protocols static route 0.0.0.0/0 next-hop 10.10.99.1
commit
save
```

### 4.4. Active Directory (Windows Server)
1. Installer le rôle **AD DS**.
2. Promouvoir le serveur en **contrôleur de domaine**.
3. Créer des OU : IT, RH, Finance, Marketing.
4. Créer les utilisateurs par service.
5. GPO : blocage de ports, politiques de sécurité, partage de fichiers.

### 4.5. Postes Clients
- Connecter chaque poste à son VLAN.
- Joindre au domaine Windows.
- Tester la communication inter-départements (ping, RDP, partage).

### 4.6. Serveurs applicatifs
- **IT** : Serveur DNS secondaire, Wazuh agent.
- **Finance** : Serveur ERP / SQL (Ubuntu ou Windows).
- **Marketing** : Serveur web (Apache/Nginx).
- **RH** : Serveur fichiers / Intranet.

---

## 5. Supervision et Sécurité

### 5.1. Wazuh SIEM
- Télécharger l'appliance : [https://wazuh.com/downloads/](https://wazuh.com/downloads/)
- Importer dans GNS3.
- Ajouter les agents sur les serveurs (Windows & Linux).

### 5.2. Suricata IDS
- Installer sur une VM Ubuntu :
```bash
sudo apt install suricata
sudo suricata-update
```
- Rediriger le trafic LAN pour inspection.

---

## 6. Tests Fonctionnels
- Ping entre départements : doit passer via pfSense.
- Accès Internet depuis tous les VLANs.
- Résolution DNS via serveur AD.
- Logs collectés par Wazuh.
- GPO appliquées par AD.

---

## 7. Extensions possibles
- Créer un VPN site à site (pfSense).
- Ajouter un proxy Squid / IDS Snort.
- Intégrer Grafana pour la visualisation SIEM.
- Simuler des attaques avec Kali (brute force, scan, phishing).

---

## 8. Livrables du projet
- Diagramme réseau (VLANs, IP plan, VMs).
- Screenshots de configuration (pfSense, AD, Wazuh).
- Dossier technique (.docx ou .pdf) avec :
  - Objectifs, topologie, paramètres IP.
  - Table de routage, VLANs, firewall rules.
  - Screenshots et résultats des tests.
  - Analyse de risques et recommandations RSSI.

---

## 9. Conseils de Performance
- Ne pas démarrer toutes les VMs simultanément.
- Utiliser des snapshots dans GNS3.
- Allouer plus de RAM à la GNS3 VM (8-12 Go si possible).

---

## 10. Objectif Pédagogique (Portfolio RSSI)
Ce projet permet de prouver vos compétences :
- Architecture réseau & sécurité.
- Gestion Active Directory & GPO.
- Routage inter-VLAN & pare-feu.
- SIEM, IDS, supervision.
- Démonstration de détection et réponse à incident.

---

**Auteur :** Hicham BAHOUTI 
**Version :** 1.0  
**Date :** 15/02/26

