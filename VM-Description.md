VM1 : pfSense → Firewall + VLAN + VPN
2 vCPU, 2–3 Go RAM, 20 Go

VM2 : AD-DC01 → Active Directory + DNS
2 vCPU, 4 Go RAM, 40 Go

VM3 : Keycloak → SSO + MFA + LDAP vers AD
2 vCPU, 4 Go RAM, 20 Go

VM4 : Splunk → SIEM (logs, détection, MITRE)
4 vCPU, 8–12 Go RAM, 100 Go

VM5 : Zabbix → Monitoring du SI
2 vCPU, 3–4 Go RAM, 20–30 Go

VM6 : Grafana → Dashboards & visualisation
1–2 vCPU, 2 Go RAM, 10 Go

VM7 : GLPI → Helpdesk / Gestion des tickets IT
2 vCPU, 2–3 Go RAM, 20 Go

VM8 : Mailcow → Serveur mail complet (SMTP/IMAP)
2 vCPU, 6 Go RAM, 40–80 Go

VM9 : Web interne (Intranet) → Site interne
1–2 vCPU, 1–2 Go RAM, 10–20 Go

VM10 : Reverse Proxy (Nginx) → Point d’entrée HTTP/HTTPS
1 vCPU, 1 Go RAM, 10 Go

VM11 : WAF (ModSecurity) → Sécurité applicative / DMZ
1–2 vCPU, 2 Go RAM, 10–20 Go

VM12 : Web public → Site web exposé en DMZ
1 vCPU, 1–2 Go RAM, 10–20 Go

VM13 : PC Utilisateur (Windows 10/11) → Poste standard
2 vCPU, 4 Go RAM, 40–60 Go

VM14 : PC Technicien → Support informatique
2 vCPU, 4–6 Go RAM, 40–60 Go

VM15 : PC RSSI/Admin → Administration & sécurité
2 vCPU, 6–8 Go RAM, 40–60 Go
