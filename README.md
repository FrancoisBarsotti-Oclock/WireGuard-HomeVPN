# 🔐 Déploiement d'une infrastructure VPN WireGuard auto-hébergée pour sécuriser les accès distants à un réseau local

> Mise en place d'un serveur VPN WireGuard auto-hébergé derrière une Livebox 6 afin de sécuriser les connexions distantes, protéger les communications sur les réseaux publics et accéder au réseau local depuis n'importe où.

## 📖 Présentation

Ce projet présente la mise en place d'une infrastructure VPN WireGuard auto-hébergée derrière une Livebox 6 afin de fournir un accès distant sécurisé à un réseau local.

L'objectif est de permettre à plusieurs appareils (Android, Windows et Linux) d'accéder au réseau domestique depuis n'importe où tout en chiffrant les communications et en utilisant la connexion Internet du domicile.

![Linux](...)
![WireGuard](...)
![Windows](...)
![Android](...)
![IPv4](...)
![iptables](...)

## 🎯 Compétences mises en œuvre

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

## 📚 Sommaire

- [📖 Contexte](#-contexte)
- [🎯 Objectifs](#-objectifs)
- [🏗️ Architecture](#️-architecture)
- [🛠️ Technologies](#️-technologies)
- [📂 Documentation](#-documentation)
- [📷 Captures](#-captures)
- [🧪 Tests](#-tests)
- [🛡️ Sécurité](#️-sécurité)
- [🐞 Dépannage](#-dépannage)
- [🚀 Améliorations futures](#-améliorations-futures)

## 🎯 Objectifs

- Sécuriser les connexions sur les réseaux Wi-Fi publics
- Accéder au réseau domestique depuis Internet
- Faire transiter tout le trafic Internet via la connexion de la Livebox.
- Découvrir WireGuard
- Comprendre le routage IPv4
- Comprendre le NAT

## ✅ Résultat

✔ VPN opérationnel

✔ Android

✔ Windows 11

✔ Linux Mint

✔ Routage IPv4

✔ NAT fonctionnel

✔ Connexion via réseau 4G/5G

✔ Accès Internet via l'adresse IP publique de la Livebox

✔ Accès sécurisé au réseau local

✔ Plusieurs clients VPN (Android et Windows)

✔ Fonctionnement validé sur réseau mobile 4G/5G

✔ Fonctionnement derrière une Livebox 6


## 🛠️ Technologies

- Linux Mint
- WireGuard
- iptables
- UFW
- OpenSSH
- SCP
- Livebox 6
- QRencode
- Android
- Windows 11

## 🏗️ Architecture

![Architecture](images/architecture.png)

## 📂 Documentation

| Document | Description |
|----------|-------------|
| INSTALLATION.md | Installation complète du serveur |
| CONFIGURATION.md | Configuration détaillée |
| CLIENTS.md | Android, Windows et Linux |
| SECURITY.md | Bonnes pratiques |
| TROUBLESHOOTING.md | Erreurs rencontrées |