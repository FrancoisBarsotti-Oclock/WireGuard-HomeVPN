# 🔐 Déploiement d'une infrastructure VPN WireGuard auto-hébergée pour sécuriser les accès distants à un réseau local

> Mise en place d'un serveur VPN WireGuard auto-hébergé derrière une Livebox 6 afin de sécuriser les connexions distantes, protéger les communications sur les réseaux publics et accéder au réseau local depuis n'importe où.

![Linux Mint](https://img.shields.io/badge/Linux_Mint-22.x-87CF3E?logo=linuxmint&logoColor=white)
![WireGuard](https://img.shields.io/badge/WireGuard-VPN-88171A?logo=wireguard&logoColor=white)
![Windows](https://img.shields.io/badge/Windows-11-0078D6?logo=windows11&logoColor=white)
![Android](https://img.shields.io/badge/Android-Compatible-3DDC84?logo=android&logoColor=white)
![IPv4](https://img.shields.io/badge/IPv4-Routing-blue)
![iptables](https://img.shields.io/badge/iptables-NAT-orange)
![Status](https://img.shields.io/badge/Status-Completed-success)

> 💡 **Objectif du projet :** déployer une infrastructure VPN moderne, légère et sécurisée afin de permettre un accès distant chiffré au réseau local depuis n'importe quel appareil compatible WireGuard.

## 📖 Présentation

Ce projet présente la mise en place d'une infrastructure VPN WireGuard auto-hébergée derrière une Livebox 6 afin de fournir un accès distant sécurisé à un réseau local.

L'objectif est de permettre à plusieurs appareils (Android, Windows et Linux) d'accéder au réseau domestique depuis n'importe où tout en chiffrant les communications et en utilisant la connexion Internet du domicile.

## 📚 Sommaire

- [📖 Présentation](#-présentation)
- [🚧 État du projet](#-état-du-projet)
- [✨ Fonctionnalités](#-fonctionnalités)
- [🎯 Objectifs](#-objectifs)
- [❓ Pourquoi WireGuard ?](#-pourquoi-wireguard-)
- [💼 Compétences techniques](#-compétences-techniques)
- [🛠️ Technologies](#️-technologies)
- [🏗️ Architecture](#️-architecture)
- [📂 Documentation](#-documentation)
- [✅ Résultat](#-résultat)
- [📷 Captures](#-captures)
- [🧪 Tests](#-tests)
- [🛡️ Sécurité](#️-sécurité)
- [🐞 Dépannage](#-dépannage)
- [🚀 Améliorations futures](#-améliorations-futures)

## 🚧 État du projet

✅ Fonctionnel

🔄 Documentation en cours de rédaction

📚 Améliorations prévues

## ✨ Fonctionnalités

- 🔒 Chiffrement des communications avec WireGuard
- 🌍 Accès distant sécurisé au réseau local
- 📡 Redirection NAT/PAT via Livebox 6
- 📱 Support Android
- 💻 Support Windows 11
- 🐧 Serveur Linux Mint
- 🔑 Authentification par clés publiques/privées
- 🚀 Routage IPv4 et NAT via iptables

## 🎯 Objectifs

- Sécuriser les connexions sur les réseaux Wi-Fi publics
- Accéder au réseau domestique depuis Internet
- Faire transiter tout le trafic Internet via la connexion de la Livebox.
- Découvrir WireGuard
- Comprendre le routage IPv4
- Comprendre le NAT

## ❓ Pourquoi WireGuard ?

Parmi les différentes solutions VPN disponibles, WireGuard a été retenu pour ce projet en raison de sa simplicité de déploiement, de ses excellentes performances et de son architecture moderne. Son code source réduit facilite les audits de sécurité et limite la surface d'attaque par rapport à des solutions plus anciennes.

| Critère | WireGuard | OpenVPN |
|----------|:---------:|:-------:|
| Performance | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| Simplicité de configuration | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Facilité d'audit | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| Taille du code source | ≈ 4 000 lignes | > 100 000 lignes |
> **Note :** comparaison qualitative basée sur la simplicité de déploiement, les performances généralement observées et la taille approximative du code source.

## 💼 Compétences techniques

- Administration Linux
- WireGuard VPN
- Routage IPv4
- NAT (MASQUERADE)
- iptables
- UFW
- NAT/PAT Livebox
- DHCP statique
- SSH
- SCP
- QRCode
- Cryptographie asymétrique
- Gestion des clés publiques/privées

## 🛠️ Technologies

| **Domaine** | **Technologie** |
|---------|-------------|
| Système | Linux Mint |
| VPN | WireGuard |
| Pare-feu | iptables / UFW |
| Routeur | Livebox 6 |
| Administration | OpenSSH |
| Transfert | SCP |
| Client | Windows 11 |
| Client | Android |

## 🏗️ Architecture


## 🏗️ Architecture

Cette infrastructure repose sur un serveur WireGuard auto-hébergé sous Linux Mint, placé derrière une Livebox 6. Les clients VPN (Android et Windows) établissent un tunnel chiffré afin d'accéder au réseau local et de faire transiter leur trafic Internet via la connexion du domicile.

![Architecture générale](https://github.com/FrancoisBarsotti-Oclock/WireGuard-HomeVPN/blob/main/images/Architecture.png)

> **Figure 1 – Vue d'ensemble de l'architecture du projet.**

## 📂 Documentation

| **Document** | **Description** | **État** |
|----------|-------------|------|
| INSTALLATION.md | Installation complète | 🚧 |
| CONFIGURATION.md | Configuration détaillée | 🚧 |
| CLIENTS.md | Android et Windows | 🚧 |
| SECURITY.md | Bonnes pratiques | 🚧 |
| TROUBLESHOOTING.md | Résolution des incidents | 🚧 |

## ✅ Résultat

- ✔ Infrastructure VPN opérationnelle
- ✔ Accès sécurisé au réseau local
- ✔ Android et Windows connectés
- ✔ Fonctionnement validé en 4G/5G
- ✔ Routage IPv4 fonctionnel
- ✔ NAT via iptables
- ✔ Accès Internet via la connexion Orange
- ✔ Déploiement derrière une Livebox 6

---
