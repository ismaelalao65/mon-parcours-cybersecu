# 🧪 Test de connectivité entre VMs

Mise en place d'un labo de cybersécurité personnel avec 4 machines virtuelles, dans le but de m'entraîner au pentest dans un environnement isolé et sécurisé.

## 🎯 Objectif

Construire un environnement de test réseau contrôlé permettant de :
- Pratiquer la reconnaissance réseau (scan, découverte d'hôtes)
- S'entraîner sur des cibles volontairement vulnérables, sans risque pour un réseau réel
- Comprendre la configuration réseau d'un hyperviseur (modes NAT, Host-only, Bridged)

## 🖥️ Environnement technique

- **Hyperviseur** : VMware Workstation Pro 17
- **Machine hôte** : Windows
- **Réseau virtuel** : NAT isolé (VMnet8) — aucune exposition sur le réseau domestique ni sur internet

## 🗂️ Machines virtuelles du labo

| Machine | Rôle | Adresse IP |
|---|---|---|
| Kali Linux | Machine d'attaque / outils de pentest | 192.168.142.137 |
| Metasploitable2 | Cible volontairement vulnérable | 192.168.142.140 |
| Ubuntu Server | Cible de test (services Linux) | 192.168.142.136 |
| Windows Server 2019 | Cible de test (environnement Windows) | 192.168.142.139 |

## 🏗️ Architecture réseau

Toutes les machines sont isolées du réseau domestique via un réseau **NAT dédié (VMnet8)** — seule la machine hôte peut communiquer avec elles ; aucun appareil externe ne peut les atteindre.

*(voir `screenshots/architecture-reseau.png`)*

## ⚙️ Étapes réalisées

1. **Installation des 4 VM** dans VMware Workstation Pro
2. **Configuration réseau** : harmonisation de toutes les VM sur le même réseau virtuel (NAT / VMnet8) pour garantir qu'elles soient sur le même sous-réseau
3. **Résolution d'un problème de configuration réseau** : Metasploitable2 était initialement isolée sur un réseau Host-only différent des autres VM (sous-réseaux distincts), empêchant toute communication. Diagnostic via `ifconfig` et correction du mode réseau.
4. **Attribution des adresses IP** via DHCP (Kali, Ubuntu, Windows Server) et configuration réseau sur Metasploitable2
5. **Tests de connectivité** entre toutes les machines (ping)
6. **Scan de reconnaissance réseau** avec Nmap depuis Kali Linux

## 📸 Captures d'écran

- `screenshots/network-editor.png` — configuration des réseaux virtuels VMware
- `screenshots/ping-results.png` — tests de connectivité réussis entre les 4 VM
- `screenshots/nmap-discovery.png` — scan de découverte du réseau (`nmap -sn`)
- `screenshots/nmap-metasploitable.png` — scan de services sur la cible Metasploitable2

## 🔍 Résultats obtenus

- ✅ Connectivité réseau fonctionnelle entre les 4 machines
- ✅ Résolution d'un problème réel de segmentation réseau (sous-réseaux différents)
- ✅ Premier scan Nmap réalisé sur Metasploitable2, révélant une vingtaine de services ouverts (FTP, SSH, SMB, MySQL, etc.) — base pour de futurs travaux d'analyse de vulnérabilités

## 🧠 Compétences mises en pratique

- Configuration réseau sous VMware (NAT, Host-only, segmentation)
- Diagnostic réseau (`ifconfig`, `ip a`, `ping`)
- Reconnaissance réseau avec Nmap
- Méthodologie de dépannage (isolement de la cause racine d'un problème de connectivité)

## 🔒 Note sur la sécurité

Ce labo est volontairement isolé de tout réseau externe. Aucune information sensible, identifiant réel ou donnée personnelle n'est utilisée dans cet environnement. Les résultats partagés ici sont strictement pédagogiques et ne concernent que des machines de test locales m'appartenant.

## 📌 Prochaines étapes

- Approfondissement de l'analyse des services découverts sur Metasploitable2
- Découverte des bases de Metasploit Framework
- Mise en place d'un environnement Active Directory avec Windows Server

---
*Projet réalisé dans le cadre de mon apprentissage autonome en sécurité réseau, suite à ma Licence en Sécurité des Systèmes et Réseaux Informatiques.*