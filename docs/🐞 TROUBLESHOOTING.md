# 🐞 Dépannage (Troubleshooting)

## 📖 Objectif

Ce document recense les principaux incidents rencontrés lors de la mise en place de l'infrastructure WireGuard ainsi que les méthodes utilisées pour les diagnostiquer et les corriger.

Les exemples présentés sont issus de la mise en œuvre réelle du projet.

---

# 📋 Méthodologie de diagnostic

Avant toute modification, la démarche suivante est recommandée :

1. Vérifier l'état de l'interface WireGuard.
2. Contrôler les handshakes.
3. Vérifier le routage IPv4.
4. Contrôler les règles NAT et iptables.
5. Tester la connectivité réseau.
6. Vérifier les journaux système si nécessaire.

Les commandes les plus utilisées sont :

```bash
sudo wg

ip addr show wg0

ip route

cat /proc/sys/net/ipv4/ip_forward

sudo iptables -L

sudo iptables -t nat -L

sudo systemctl status wg-quick@wg0
```

---

# ❌ Aucun accès Internet malgré un handshake réussi

## Symptôme

* Le client établit un handshake.
* Aucun site Internet ne s'ouvre.

## Cause

Une faute de frappe dans la configuration :

```text
-i w0
```

au lieu de :

```text
-i wg0
```

## Solution

Corriger les règles `PostUp` et `PostDown` dans :

```text
/etc/wireguard/wg0.conf
```

Puis recharger WireGuard :

```bash
sudo wg-quick down wg0
sudo wg-quick up wg0
```

---

# ❌ Erreur "Key is not the correct length or format"

## Symptôme

Au démarrage :

```text
Key is not the correct length or format
```

## Cause

La clé privée avait été copiée sans son caractère final :

```text
=
```

## Solution

Copier la totalité de la clé privée générée par WireGuard.

---

# ❌ Erreur "Configuration parsing error"

## Symptôme

Au lancement :

```text
Line unrecognized
```

## Cause

Erreur de syntaxe dans :

```ini
Address
```

par exemple :

```text
Adress
```

## Solution

Corriger la syntaxe du fichier `wg0.conf`.

---

# ❌ Impossible d'importer le tunnel Android

## Symptôme

L'application Android affiche :

```text
Impossible d'analyser le point de terminaison.
```

## Cause

Erreur de syntaxe dans :

```ini
Endpoint =
```

Adresse IP incomplète ou format incorrect.

## Solution

Vérifier :

```ini
Endpoint = IP_PUBLIQUE:51820
```

---

# ❌ Tunnel actif mais aucun handshake

## Symptôme

Le client semble connecté mais :

```bash
sudo wg
```

n'affiche aucun handshake.

## Causes possibles

* Redirection NAT/PAT absente.
* Mauvaise adresse IP publique.
* Port UDP 51820 non accessible.
* Clé publique incorrecte.
* Pair (Peer) non déclaré.

## Vérifications

```bash
sudo wg

ip route

sudo iptables -t nat -L
```

---

# ❌ Le DNS privé Android ne fonctionne plus

## Symptôme

Android indique :

```text
Impossible d'accéder au serveur DNS privé.
```

## Cause

Le serveur DNS configuré n'était plus joignable via le tunnel.

## Solution

Tester temporairement :

```text
DNS = 1.1.1.1
```

Une fois le VPN opérationnel, le DNS privé peut être réactivé.

---

# ❌ Permissions insuffisantes sur wg0.conf

## Symptôme

Le fichier de configuration est lisible par d'autres utilisateurs.

## Risque

Exposition de la clé privée du serveur.

## Solution

Limiter les permissions :

```bash
sudo chmod 600 /etc/wireguard/wg0.conf
```

---

# 🔎 Commandes utiles

## Vérifier WireGuard

```bash
sudo wg
```

---

## Vérifier l'interface

```bash
ip addr show wg0
```

---

## Vérifier le routage

```bash
ip route
```

---

## Vérifier le NAT

```bash
sudo iptables -t nat -L
```

---

## Vérifier le Forwarding

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Résultat attendu :

```text
1
```

---

## Vérifier le service

```bash
sudo systemctl status wg-quick@wg0
```

---

# 💡 Bonnes pratiques de dépannage

* Modifier un seul paramètre à la fois.
* Recharger WireGuard après chaque modification.
* Vérifier les handshakes avant de poursuivre.
* Tester depuis un réseau externe (4G/5G).
* Sauvegarder une copie fonctionnelle de `wg0.conf`.

---

# ✅ Conclusion

La majorité des incidents rencontrés lors du déploiement d'un serveur WireGuard proviennent de problèmes de configuration plutôt que du protocole lui-même.

Une démarche de diagnostic méthodique, associée aux outils intégrés de Linux (`wg`, `ip`, `iptables`, `systemctl`), permet d'identifier rapidement l'origine des dysfonctionnements et de rétablir le service.

---
