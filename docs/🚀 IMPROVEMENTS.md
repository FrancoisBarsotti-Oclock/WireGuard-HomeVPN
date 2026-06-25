# 🚀 Améliorations futures (Roadmap)

## 📖 Objectif

Ce document présente les principales évolutions envisagées pour améliorer l'infrastructure VPN WireGuard mise en place dans ce projet.

Le serveur est aujourd'hui pleinement fonctionnel, mais plusieurs optimisations permettraient d'augmenter sa disponibilité, sa sécurité, sa facilité d'administration et son évolutivité.

---

# 🎯 Priorités d'évolution

| Priorité | Évolution                       | Statut       |
| -------- | ------------------------------- | ------------ |
| ⭐⭐⭐      | Migration vers un Raspberry Pi  | 📋 Prévu     |
| ⭐⭐⭐      | Mise en place d'un service DDNS | 📋 Prévu     |
| ⭐⭐       | Supervision du serveur          | 💡 À l'étude |
| ⭐⭐       | DNS local                       | 💡 À l'étude |
| ⭐        | IPv6                            | 🔬 Recherche |

---

# 🍓 Hébergement sur Raspberry Pi

## Pourquoi ?

Le serveur actuel est hébergé sur un ancien ordinateur portable Linux Mint.

Cette solution est parfaitement adaptée aux tests et à un usage ponctuel, mais elle n'est pas idéale pour un fonctionnement permanent.

Le remplacement par un Raspberry Pi permettrait :

* une consommation électrique réduite (quelques watts seulement) ;
* un fonctionnement 24 h/24 et 7 j/7 ;
* une meilleure fiabilité pour un usage continu ;
* un encombrement minimal.

Le Raspberry Pi deviendrait ainsi un serveur VPN dédié.

---

# 🌍 Mise en place d'un service DDNS

Aujourd'hui, les clients utilisent directement l'adresse IP publique de la connexion Internet.

En cas de changement d'adresse IP par le fournisseur d'accès, les fichiers de configuration des clients doivent être modifiés.

L'utilisation d'un service DDNS permettrait d'utiliser un nom de domaine à la place de l'adresse IP.

Exemple :

```ini id="scvwaw"
Endpoint = monvpn.domaine.fr:51820
```

Les clients continueraient ainsi à fonctionner même après un changement d'adresse IP publique.

---

# 🌐 Mise en place d'un DNS local

Le projet utilise actuellement :

```text id="7qojlk"
DNS = 1.1.1.1
```

Une évolution intéressante consisterait à déployer un résolveur DNS local, par exemple :

* dnsmasq
* Unbound

Cette solution permettrait :

* la résolution locale des noms d'hôtes ;
* une meilleure confidentialité des requêtes DNS ;
* une administration centralisée des clients VPN.

---

# 📊 Supervision

Afin de surveiller la disponibilité du serveur VPN, plusieurs solutions pourraient être intégrées.

Par exemple :

* Zabbix
* Wazuh
* Grafana

Les principaux indicateurs surveillés pourraient être :

* disponibilité du service WireGuard ;
* utilisation CPU ;
* mémoire ;
* espace disque ;
* trafic réseau ;
* nombre de clients connectés.

---

# 💾 Sauvegardes

Une stratégie de sauvegarde permettrait de restaurer rapidement le serveur.

Les fichiers critiques sont notamment :

```text id="4lp9az"
/etc/wireguard/wg0.conf

privatekey

publickey
```

Une sauvegarde automatique chiffrée pourrait être réalisée régulièrement.

---

# 🔒 Renforcement de la sécurité

Parmi les améliorations possibles :

* authentification SSH par clés uniquement ;
* désactivation de l'authentification par mot de passe ;
* limitation des tentatives de connexion SSH (`fail2ban`) ;
* journalisation des connexions WireGuard ;
* contrôle périodique des permissions des fichiers.

---

# 🌐 Compatibilité IPv6

Le projet est actuellement basé sur IPv4.

Une évolution future pourrait consister à prendre en charge IPv6 afin de préparer l'infrastructure aux réseaux modernes.

---

# 📱 Nouveaux clients

L'infrastructure est conçue pour accueillir facilement de nouveaux équipements :

* ordinateur portable supplémentaire ;
* tablette ;
* serveur distant ;
* machine virtuelle ;
* second poste Linux.

Chaque nouvel équipement disposerait :

* d'une paire de clés dédiée ;
* d'une adresse VPN unique ;
* d'un fichier de configuration indépendant.

---

# 🏢 Évolutions vers un environnement professionnel

Cette architecture pourrait être adaptée à une petite infrastructure d'entreprise.

Les évolutions possibles seraient notamment :

* authentification centralisée ;
* supervision complète ;
* sauvegardes automatisées ;
* segmentation réseau (VLAN) ;
* haute disponibilité ;
* documentation d'exploitation.

---

# 🎯 Conclusion

Le projet atteint son objectif principal : fournir un accès distant sécurisé à un réseau local grâce à WireGuard.

Les améliorations proposées visent désormais à renforcer la disponibilité, la sécurité et l'administration de la solution, tout en préparant une migration vers une plateforme dédiée, telle qu'un Raspberry Pi ou un mini-PC, afin de disposer d'un service VPN permanent et facilement maintenable.

---
