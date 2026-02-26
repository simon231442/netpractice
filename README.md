# NetPractice – Module Réseau 42

Guide de référence rapide pour le module NetPractice de l'école 42.

---

## Table des matières

1. [Adresse IP – Bases](#1-adresse-ip--bases)
2. [Classes d'adresses IP](#2-classes-dadresses-ip)
3. [Plages privées (RFC 1918)](#3-plages-privées-rfc-1918)
4. [Plages spéciales et réservées](#4-plages-spéciales-et-réservées)
5. [Masque de sous-réseau et notation CIDR](#5-masque-de-sous-réseau-et-notation-cidr)
6. [Tableau des masques CIDR (/8 à /30)](#6-tableau-des-masques-cidr-8-à-30)
7. [Calculs essentiels](#7-calculs-essentiels)
8. [Routage – Bases](#8-routage--bases)
9. [Règles à retenir](#9-règles-à-retenir)

---

## 1. Adresse IP – Bases

Une adresse **IPv4** est composée de **4 octets** (32 bits) séparés par des points.

```
Exemple : 192.168.1.10
          [192].[168].[001].[010]
```

Chaque octet peut aller de **0 à 255** (2⁸ = 256 valeurs possibles).

En binaire :
```
192 = 1100 0000
168 = 1010 1000
  1 = 0000 0001
 10 = 0000 1010
```

Une adresse IP est divisée en deux parties :
- **Partie réseau** – identifie le réseau
- **Partie hôte** – identifie la machine dans le réseau

La ligne de séparation est définie par le **masque de sous-réseau**.

---

## 2. Classes d'adresses IP

| Classe | Plage de départ | Plage de fin      | Masque par défaut | Nb de réseaux | Nb d'hôtes/réseau |
|--------|-----------------|-------------------|-------------------|---------------|-------------------|
| A      | 1.0.0.0         | 126.255.255.255   | /8  (255.0.0.0)   | 126           | 16 777 214        |
| B      | 128.0.0.0       | 191.255.255.255   | /16 (255.255.0.0) | 16 384        | 65 534            |
| C      | 192.0.0.0       | 223.255.255.255   | /24 (255.255.255.0) | 2 097 152   | 254               |
| D      | 224.0.0.0       | 239.255.255.255   | —                 | Multicast     | —                 |
| E      | 240.0.0.0       | 255.255.255.255   | —                 | Réservé       | —                 |

> **Note :** `127.x.x.x` est réservé à la boucle locale (loopback) et n'appartient à aucune classe routée.

---

## 3. Plages privées (RFC 1918)

Ces plages ne sont **pas routées sur Internet**. Elles sont réservées aux réseaux locaux (LAN).

| Plage CIDR    | Première adresse | Dernière adresse  | Classe |
|---------------|------------------|-------------------|--------|
| 10.0.0.0/8    | 10.0.0.0         | 10.255.255.255    | A      |
| 172.16.0.0/12 | 172.16.0.0       | 172.31.255.255    | B      |
| 192.168.0.0/16| 192.168.0.0      | 192.168.255.255   | C      |

---

## 4. Plages spéciales et réservées

| Plage CIDR        | Nom                  | Usage                                         |
|-------------------|----------------------|-----------------------------------------------|
| 0.0.0.0/8         | Ce réseau            | Adresse indéterminée / source non-spécifiée   |
| 127.0.0.0/8       | Loopback             | Tester la pile TCP/IP locale (`127.0.0.1`)    |
| 169.254.0.0/16    | Link-local (APIPA)   | Auto-configuration sans serveur DHCP          |
| 100.64.0.0/10     | Shared Address Space | Utilisé par les opérateurs (CGNAT)            |
| 192.0.2.0/24      | TEST-NET-1           | Documentation et exemples (RFC 5737)          |
| 198.51.100.0/24   | TEST-NET-2           | Documentation et exemples (RFC 5737)          |
| 203.0.113.0/24    | TEST-NET-3           | Documentation et exemples (RFC 5737)          |
| 255.255.255.255/32| Broadcast limité     | Diffusion sur le réseau local                 |

---

## 5. Masque de sous-réseau et notation CIDR

Le **masque de sous-réseau** indique combien de bits sont réservés à la partie réseau.

```
Masque 255.255.255.0 = 1111 1111 . 1111 1111 . 1111 1111 . 0000 0000
                       |<-------- 24 bits réseau -------->| 8 bits hôte
```

La **notation CIDR** (Classless Inter-Domain Routing) exprime ce nombre directement après l'adresse :

```
192.168.1.0/24   →  masque = 255.255.255.0
10.0.0.0/8       →  masque = 255.0.0.0
172.16.0.0/12    →  masque = 255.240.0.0
```

---

## 6. Tableau des masques CIDR (/8 à /30)

| CIDR | Masque            | Nb d'adresses | Nb d'hôtes utilisables | Incrément de bloc |
|------|-------------------|---------------|------------------------|-------------------|
| /8   | 255.0.0.0         | 16 777 216    | 16 777 214             | 256 (3e octet)    |
| /9   | 255.128.0.0       | 8 388 608     | 8 388 606              | 128               |
| /10  | 255.192.0.0       | 4 194 304     | 4 194 302              | 64                |
| /11  | 255.224.0.0       | 2 097 152     | 2 097 150              | 32                |
| /12  | 255.240.0.0       | 1 048 576     | 1 048 574              | 16                |
| /13  | 255.248.0.0       | 524 288       | 524 286                | 8                 |
| /14  | 255.252.0.0       | 262 144       | 262 142                | 4                 |
| /15  | 255.254.0.0       | 131 072       | 131 070                | 2                 |
| /16  | 255.255.0.0       | 65 536        | 65 534                 | 256 (3e octet)    |
| /17  | 255.255.128.0     | 32 768        | 32 766                 | 128               |
| /18  | 255.255.192.0     | 16 384        | 16 382                 | 64                |
| /19  | 255.255.224.0     | 8 192         | 8 190                  | 32                |
| /20  | 255.255.240.0     | 4 096         | 4 094                  | 16                |
| /21  | 255.255.248.0     | 2 048         | 2 046                  | 8                 |
| /22  | 255.255.252.0     | 1 024         | 1 022                  | 4                 |
| /23  | 255.255.254.0     | 512           | 510                    | 2                 |
| /24  | 255.255.255.0     | 256           | 254                    | 256 (4e octet)    |
| /25  | 255.255.255.128   | 128           | 126                    | 128               |
| /26  | 255.255.255.192   | 64            | 62                     | 64                |
| /27  | 255.255.255.224   | 32            | 30                     | 32                |
| /28  | 255.255.255.240   | 16            | 14                     | 16                |
| /29  | 255.255.255.248   | 8             | 6                      | 8                 |
| /30  | 255.255.255.252   | 4             | 2                      | 4                 |

> **/31** et **/32** : /31 = lien point-à-point (2 adresses, 0 hôte utilisable classique), /32 = hôte unique.

---

## 7. Calculs essentiels

### Adresse réseau

Effectuer un **ET logique (AND)** bit à bit entre l'adresse IP et le masque.

```
IP      : 192.168.1.130  →  1100 0000 . 1010 1000 . 0000 0001 . 1000 0010
Masque  : 255.255.255.192→  1111 1111 . 1111 1111 . 1111 1111 . 1100 0000
                         AND
Réseau  : 192.168.1.128  →  1100 0000 . 1010 1000 . 0000 0001 . 1000 0000
```

### Adresse de broadcast

Mettre tous les **bits hôte à 1** dans l'adresse réseau.

```
Réseau  : 192.168.1.128  →  1100 0000 . 1010 1000 . 0000 0001 . 1000 0000
Bits hôte à 1            →                                       0011 1111
Broadcast: 192.168.1.191 →  1100 0000 . 1010 1000 . 0000 0001 . 1011 1111
```

### Nombre d'hôtes utilisables

```
Nb hôtes = 2^(bits_hôte) - 2
```
On soustrait 2 pour exclure l'**adresse réseau** et l'**adresse de broadcast**.

**Exemples :**
| CIDR | Bits hôte | Calcul       | Hôtes utilisables |
|------|-----------|--------------|-------------------|
| /24  | 8         | 2⁸ − 2 = 254 | 254               |
| /26  | 6         | 2⁶ − 2 = 62  | 62                |
| /30  | 2         | 2² − 2 = 2   | 2                 |

### Vérifier si deux adresses sont sur le même réseau

Deux adresses sont sur le **même réseau** si leur adresse réseau (IP AND masque) est identique.

```
192.168.1.10  /26  →  réseau : 192.168.1.0
192.168.1.70  /26  →  réseau : 192.168.1.64
→ Réseaux différents : pas de communication directe sans routeur.
```

---

## 8. Routage – Bases

### Passerelle par défaut (default gateway)

La **passerelle par défaut** est l'adresse du routeur auquel une machine envoie les paquets destinés à un réseau inconnu (hors de son propre sous-réseau).

- Elle doit être dans le **même sous-réseau** que la machine.
- Elle ne peut pas être l'adresse réseau ni le broadcast.

### Table de routage

Chaque équipement possède une table de routage qui liste les destinations connues :

| Destination   | Masque           | Passerelle    | Interface |
|---------------|------------------|---------------|-----------|
| 192.168.1.0   | 255.255.255.0    | —             | eth0      |
| 0.0.0.0       | 0.0.0.0          | 192.168.1.254 | eth0      |

- `0.0.0.0/0` = **route par défaut** (utilisée si aucune autre règle ne correspond).
- Le routeur cherche la règle la **plus spécifique** (préfixe le plus long) avant d'appliquer la route par défaut.

### Next hop (saut suivant)

Le **next hop** est l'adresse du prochain routeur sur le chemin vers la destination. Si la destination est directement accessible, il n'y a pas de next hop (connexion directe).

### Résumé du processus de routage

```
Machine A  →  paquet vers 8.8.8.8
           →  8.8.8.8 est-il dans mon sous-réseau ?  Non
           →  envoyer à la passerelle par défaut (192.168.1.254)
           →  le routeur consulte sa table de routage
           →  transmet au prochain routeur / à Internet
```

---

## 9. Règles à retenir

1. **L'adresse réseau** (tous les bits hôte à 0) n'est **pas assignable** à une machine.
2. **L'adresse broadcast** (tous les bits hôte à 1) n'est **pas assignable** à une machine.
3. La **passerelle** doit être dans le **même sous-réseau** que la machine.
4. Deux machines sur des **sous-réseaux différents** ne peuvent pas communiquer sans **routeur**.
5. Les plages **10.x.x.x**, **172.16–31.x.x** et **192.168.x.x** sont **privées** et non routées sur Internet.
6. `127.0.0.1` est toujours la **boucle locale** — elle ne quitte jamais la machine.
7. Un masque valide est toujours une suite de **1 continus** suivie de **0 continus** en binaire.
8. Pour un lien **point-à-point** (entre deux routeurs), un **/30** est la norme (2 hôtes utiles).
