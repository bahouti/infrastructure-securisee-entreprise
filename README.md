# 🏢 Infrastructure Réseau Sécurisée d'Entreprise

## 🎯 Objectif

Concevoir et déployer une infrastructure réseau complète pour une entreprise fictive afin de reproduire un environnement professionnel réaliste et appliquer les bonnes pratiques d’administration système et de cybersécurité.

---

## 🧠 Compétences mises en œuvre

* Segmentation réseau (VLAN)
* Routage inter-VLAN
* Pare-feu et filtrage inter-zones
* Active Directory (gestion des identités et des accès)
* VPN d’accès distant sécurisé
* Supervision et centralisation des logs
* Détection d’intrusion réseau et système
* Durcissement des systèmes (hardening)

---

## 🏗️ Architecture

L’infrastructure est organisée en plusieurs zones de sécurité :

| Zone             | Rôle              |
| ---------------- | ----------------- |
| LAN Utilisateurs | Postes employés   |
| Serveurs         | AD, DNS, DHCP     |
| DMZ              | Services exposés  |
| Administration   | Accès IT sécurisé |
| WAN              | Accès Internet    |

---

## 🖥️ Services déployés

* Contrôleur de domaine (Active Directory)
* DNS / DHCP
* Pare-feu
* VPN
* Plateforme XDR (Wazuh)
* IDS/IPS réseau (Suricata)
* Centralisation des logs

---

## 🔐 Sécurité implémentée

* Isolation par VLAN
* Filtrage strict entre zones réseau
* Accès administrateur dédié
* Authentification centralisée
* Détection d’activités suspectes
* Journalisation et corrélation des événements

---

## 📊 Objectifs pédagogiques

Ce projet simule le travail d’un administrateur système et réseau : conception, déploiement, sécurisation et documentation d’une infrastructure d’entreprise complète.

---

## 📎 Résultats et documents

La documentation détaillée du projet (schémas réseau, configurations et rapport complet) est disponible sur demande.

---

## Licence

Projet distribué sous licence MIT.
Voir le fichier `LICENSE` pour plus d’informations.
