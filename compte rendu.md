# Compte rendu du TP : VLSM

**Année universitaire : 2021/2022**
**Réalisé par : ALICHE OMAR**

---

# Introduction

Nous voulons utiliser la plage `192.168.1.0/24` pour cette architecture en utilisant la technique **VLSM**.

Le nombre d’hôtes pour les réseaux locaux est le suivant :

* LAN connecté au routeur R1 → 60 hôtes
* LAN connecté au routeur R2 via Fa0/0 → 10 hôtes
* LAN connecté au routeur R2 via Fa0/1 → 30 hôtes
* LAN connecté au routeur R3 → 7 hôtes

---

# Plan d’adressage VLSM

## 1. Réseau LAN de R1

### Besoin

60 hôtes

### Calcul

```text
2^6 - 2 = 62
```

Donc :

```text
Nouveau masque : /26
```

### Sous-réseaux obtenus

| Sous-réseau | Adresse          |
| ----------- | ---------------- |
| 1           | 192.168.1.0/26   |
| 2           | 192.168.1.64/26  |
| 3           | 192.168.1.128/26 |
| 4           | 192.168.1.192/26 |

### Réseau réservé

```text
192.168.1.0/26
Plage : 192.168.1.1 → 192.168.1.62
Masque : 255.255.255.192
```

---

# 2. Réseau LAN R2 via Fa0/1

## Bloc libre

```text
192.168.1.64/26
```

### Besoin

30 hôtes

### Calcul

```text
2^5 - 2 = 30
```

Donc :

```text
Nouveau masque : /27
```

### Sous-réseaux obtenus

| Sous-réseau | Adresse         |
| ----------- | --------------- |
| 1           | 192.168.1.64/27 |
| 2           | 192.168.1.96/27 |

### Réseau réservé

```text
192.168.1.64/27
Plage : 192.168.1.65 → 192.168.1.94
Masque : 255.255.255.224
```

---

# 3. Réseau LAN R2 via Fa0/0

## Bloc libre

```text
192.168.1.96/27
```

### Besoin

10 hôtes

### Calcul

```text
2^4 - 2 = 14
```

Donc :

```text
Nouveau masque : /28
```

### Sous-réseaux obtenus

| Sous-réseau | Adresse          |
| ----------- | ---------------- |
| 1           | 192.168.1.96/28  |
| 2           | 192.168.1.112/28 |

### Réseaux réservés

#### LAN R2 Fa0/0

```text
192.168.1.96/28
Plage : 192.168.1.97 → 192.168.1.110
Masque : 255.255.255.240
```

#### LAN R3

```text
192.168.1.112/28
Plage : 192.168.1.113 → 192.168.1.126
Masque : 255.255.255.240
```

---

# 4. Réseaux WAN

## Bloc libre

```text
192.168.1.128/26
```

### Besoin

2 hôtes par lien WAN

### Calcul

```text
2^2 - 2 = 2
```

Donc :

```text
Masque : /30
```

### Réseaux WAN réservés

| Liaison   | Réseau           | Plage       |
| --------- | ---------------- | ----------- |
| WAN R1-R2 | 192.168.1.128/30 | .129 → .130 |
| WAN R1-R3 | 192.168.1.132/30 | .133 → .134 |
| WAN R2-R3 | 192.168.1.136/30 | .137 → .138 |

Masque :

```text
255.255.255.252
```

---

# Récapitulatif des sous-réseaux

| Utilisation  | Réseau           |
| ------------ | ---------------- |
| LAN R1       | 192.168.1.0/26   |
| LAN R2 Fa0/1 | 192.168.1.64/27  |
| LAN R2 Fa0/0 | 192.168.1.96/28  |
| LAN R3       | 192.168.1.112/28 |
| WAN R1-R2    | 192.168.1.128/30 |
| WAN R1-R3    | 192.168.1.132/30 |
| WAN R2-R3    | 192.168.1.136/30 |

---

# Table d’adressage

| Périphérique  | Interface    | Adresse IP    | Masque          | Passerelle    |
| ------------- | ------------ | ------------- | --------------- | ------------- |
| PC-1A         | Carte réseau | 192.168.1.1   | 255.255.255.192 | 192.168.1.62  |
| PC-1B         | Carte réseau | 192.168.1.97  | 255.255.255.240 | 192.168.1.110 |
| PC-1C         | Carte réseau | 192.168.1.113 | 255.255.255.240 | 192.168.1.126 |
| Serveur Eagle | Carte réseau | 192.168.1.65  | 255.255.255.224 | 192.168.1.94  |

---

# Configuration des routeurs

# Routeur R1

## Interface G0/0

```bash
enable
configure terminal
interface g0/0
ip address 192.168.1.62 255.255.255.192
no shutdown
end
```

## Interface S0/1/1

```bash
configure terminal
interface s0/1/1
ip address 192.168.1.129 255.255.255.252
clock rate 56000
no shutdown
end
```

## Interface S0/1/0

```bash
configure terminal
interface s0/1/0
ip address 192.168.1.133 255.255.255.252
clock rate 56000
no shutdown
end
```

---

# Routeur R2

## Interface S0/1/0

```bash
enable
configure terminal
interface s0/1/0
ip address 192.168.1.130 255.255.255.252
no shutdown
end
```

## Interface S0/1/1

```bash
interface s0/1/1
ip address 192.168.1.137 255.255.255.252
clock rate 56000
no shutdown
end
```

## Interface G0/0

```bash
interface g0/0
ip address 192.168.1.110 255.255.255.240
no shutdown
end
```

---

# Routeur R3

## Interface S0/1/0

```bash
enable
configure terminal
interface s0/1/0
ip address 192.168.1.134 255.255.255.252
no shutdown
end
```

## Interface S0/1/1

```bash
interface s0/1/1
ip address 192.168.1.138 255.255.255.252
no shutdown
end
```

## Interface G0/0

```bash
interface g0/0
ip address 192.168.1.126 255.255.255.240
no shutdown
end
```

---

# Routage statique

# Routeur R1

```bash
configure terminal
ip route 192.168.1.112 255.255.255.240 192.168.1.134
ip route 0.0.0.0 0.0.0.0 192.168.1.130
copy running-config startup-config
```

---

# Routeur R2

```bash
show ip route

enable
configure terminal

ip route 192.168.1.0 255.255.255.192 192.168.1.129
ip route 192.168.1.112 255.255.255.240 192.168.1.138
```

---

# Routeur R3

```bash
enable
configure terminal

ip route 192.168.1.0 255.255.255.192 192.168.1.133
ip route 0.0.0.0 0.0.0.0 192.168.1.137

do show ip route
```

---

# Vérification

Tester la connectivité entre les équipements avec :

```bash
ping <adresse-ip>
```

Exemple :

```bash
ping 192.168.1.1
ping 192.168.1.129
ping 192.168.1.137
```

---

# Conclusion

Ce TP a permis de :

* Comprendre la technique VLSM
* Réaliser un plan d’adressage optimisé
* Configurer les interfaces des routeurs
* Mettre en place un routage statique
* Vérifier la connectivité réseau avec les commandes Ping
