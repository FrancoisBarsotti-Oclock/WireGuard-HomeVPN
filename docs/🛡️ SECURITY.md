# 🛡️ Sécurité

## 📖 Objectif

Ce document présente les principales mesures de sécurité mises en œuvre lors du déploiement de l'infrastructure WireGuard ainsi que les bonnes pratiques recommandées pour limiter les risques liés à l'exploitation d'un serveur VPN exposé sur Internet.

L'objectif n'est pas uniquement d'assurer le fonctionnement du VPN, mais également de protéger le serveur, les clients et les données qui transitent dans le tunnel.

---

# 🔑 Authentification par clés cryptographiques

WireGuard repose exclusivement sur la cryptographie asymétrique.

Chaque appareil possède :

* une clé privée ;
* une clé publique.

La clé privée ne quitte jamais son propriétaire.

Le serveur autorise uniquement les clients dont la clé publique est explicitement déclarée dans le fichier `wg0.conf`.

Cette approche élimine les mots de passe et réduit fortement les risques liés aux attaques par force brute.

---

# 🔒 Protection des clés privées

Les fichiers contenant des clés privées constituent les éléments les plus sensibles de l'infrastructure.

Les bonnes pratiques sont les suivantes :

* ne jamais publier une clé privée ;
* ne jamais envoyer une clé privée par messagerie ;
* conserver une sauvegarde dans un emplacement sécurisé ;
* limiter les permissions des fichiers.

Exemple :

```bash id="xhkpuo"
sudo chmod 600 /etc/wireguard/wg0.conf
```

|Chiffre|Propriétaire|Groupe|Autres|
|---|---|---|---|
|**6**|Lecture + Écriture (`rw-`)|||
|**0**||Aucun droit (`---`)||
|**0**|||Aucun droit (`---`)|

Cette configuration applique le **principe du moindre privilège** en réservant la lecture et l'écriture au seul utilisateur `root`.

---

# 🌐 Exposition minimale des services

Le serveur n'expose qu'un seul service sur Internet :

| Service   |  Port | Protocole |
| --------- | ----: | --------- |
| WireGuard | 51820 | UDP       |

La Livebox redirige uniquement ce port vers le serveur Linux Mint.

Réduire le nombre de services accessibles depuis Internet diminue la surface d'attaque.

---

# 🔀 Sécurisation du routage

Le routage IPv4 est activé uniquement pour permettre aux clients VPN de communiquer avec le réseau local et d'accéder à Internet via la connexion domestique.

Le NAT est assuré par :

```bash id="v8iypq"
iptables -t nat -A POSTROUTING -o wlo1 -j MASQUERADE
```

Cette règle permet de masquer les adresses privées des clients VPN lors de leur sortie vers Internet.

---

# 👥 Gestion des clients

Chaque client possède :

* une paire de clés unique ;
* une adresse VPN dédiée ;
* une entrée indépendante dans `wg0.conf`.

Cette organisation permet :

* d'identifier facilement les appareils ;
* de supprimer rapidement un accès en cas de perte ou de vol ;
* de limiter les erreurs de configuration.

---

# 🖥️ Sécurisation du serveur

Les recommandations suivantes sont appliquées :

* maintenir Linux Mint à jour ;
* utiliser uniquement les paquets officiels ;
* protéger l'accès SSH ;
* utiliser une adresse IP fixe via réservation DHCP ;
* démarrer automatiquement WireGuard au démarrage.

---

# 📱 Sécurisation des clients

Les clients VPN doivent également être protégés.

Bonnes pratiques :

* verrouillage de l'appareil ;
* mises à jour régulières ;
* suppression immédiate du tunnel en cas de perte ou de vol ;
* conservation sécurisée des fichiers de configuration.

---

# 🧰 Bonnes pratiques d'administration

Pour maintenir une infrastructure fiable :

* sauvegarder régulièrement `wg0.conf` ;
* documenter chaque nouveau client ajouté ;
* vérifier régulièrement les handshakes avec :

```bash id="0w8tvt"
sudo wg
```

* contrôler les règles de pare-feu et de NAT ;
* tester périodiquement les connexions depuis un réseau externe.

---

# 🚀 Évolutions recommandées

Cette infrastructure peut être renforcée par plusieurs améliorations :

* remplacement du PC par un Raspberry Pi ou un mini-PC dédié ;
* utilisation d'un service DDNS afin d'éviter les changements d'adresse IP publique ;
* mise en place d'un DNS local (par exemple `dnsmasq` ou `Unbound`) ;
* journalisation des connexions ;
* supervision du serveur (Zabbix, Wazuh, etc.) ;
* sauvegarde automatisée de la configuration.

---

# 📋 Résumé des mesures mises en œuvre

| Mesure                   | Mise en œuvre                       |
| ------------------------ | ----------------------------------- |
| Authentification         | Clés publiques / privées            |
| Chiffrement              | WireGuard                           |
| Permissions des fichiers | `chmod 600`                         |
| Routage                  | IPv4 (`ip_forward`)                 |
| NAT                      | `iptables` (MASQUERADE)             |
| Port exposé              | UDP 51820 uniquement                |
| Attribution des clients  | Une adresse VPN dédiée par appareil |
| Persistance              | `wg-quick@wg0` activé au démarrage  |

---

# ✅ Conclusion

Le niveau de sécurité d'une infrastructure VPN ne dépend pas uniquement du protocole utilisé, mais également de la qualité de son administration.

L'utilisation de WireGuard, combinée à une gestion rigoureuse des clés, à une exposition minimale des services, à des permissions adaptées et à une documentation complète, permet de disposer d'une solution simple, performante et adaptée à un usage personnel ou à un petit environnement professionnel.

---
