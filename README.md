# VLSM-Network-Configuration-Using-Cisco-Packet-Tracer


## 📌 Description
Ce projet présente un TP pratique sur le **VLSM (Variable Length Subnet Mask)** réalisé avec **Cisco Packet Tracer**.  
L’objectif est de concevoir un plan d’adressage IP optimisé à partir du réseau `192.168.1.0/24`, puis de configurer les routeurs, les PCs et le routage statique.

---

# 🎯 Objectifs
- Comprendre le principe du VLSM
- Découper un réseau en sous-réseaux adaptés
- Configurer des routeurs Cisco
- Mettre en place le routage statique
- Tester la connectivité réseau avec `ping`

---

# 🖧 Architecture Réseau
Le réseau contient :
- 3 Routeurs (R1, R2, R3)
- Plusieurs LANs
- Des liaisons WAN série
- Un serveur Web
- Des PCs clients

---

# 📍 Plan d’adressage

| Réseau | Adresse | Masque |
|--------|----------|---------|
| LAN R1 | 192.168.1.0/26 | 255.255.255.192 |
| LAN R2 Fa0/1 | 192.168.1.64/27 | 255.255.255.224 |
| LAN R2 Fa0/0 | 192.168.1.96/28 | 255.255.255.240 |
| LAN R3 | 192.168.1.112/28 | 255.255.255.240 |
| WAN R1-R2 | 192.168.1.128/30 | 255.255.255.252 |
| WAN R1-R3 | 192.168.1.132/30 | 255.255.255.252 |
| WAN R2-R3 | 192.168.1.136/30 | 255.255.255.252 |

---

# ⚙️ Configuration des Routeurs

## 🔹 Routeur R1

```bash
enable
configure terminal

interface g0/0
ip address 192.168.1.62 255.255.255.192
no shutdown

interface s0/1/1
ip address 192.168.1.129 255.255.255.252
clock rate 56000
no shutdown

interface s0/1/0
ip address 192.168.1.133 255.255.255.252
clock rate 56000
no shutdown

ip route 192.168.1.112 255.255.255.240 192.168.1.134
ip route 0.0.0.0 0.0.0.0 192.168.1.130

end
copy running-config startup-config
```

---

## 🔹 Routeur R2

```bash
enable
configure terminal

interface g0/0
ip address 192.168.1.110 255.255.255.240
no shutdown

interface g0/1
ip address 192.168.1.94 255.255.255.224
no shutdown

interface s0/1/0
ip address 192.168.1.130 255.255.255.252
no shutdown

interface s0/1/1
ip address 192.168.1.137 255.255.255.252
clock rate 56000
no shutdown

ip route 192.168.1.0 255.255.255.192 192.168.1.129
ip route 192.168.1.112 255.255.255.240 192.168.1.137

end
copy running-config startup-config
```

---

## 🔹 Routeur R3

```bash
enable
configure terminal

interface g0/0
ip address 192.168.1.126 255.255.255.240
no shutdown

interface s0/1/1
ip address 192.168.1.138 255.255.255.252
no shutdown

interface s0/1/0
ip address 192.168.1.134 255.255.255.252
no shutdown

ip route 192.168.1.0 255.255.255.192 192.168.1.133
ip route 0.0.0.0 0.0.0.0 192.168.1.137

end
copy running-config startup-config
```

---

# 💻 Configuration des PCs

## PC-1A
```bash
IP Address : 192.168.1.1
Subnet Mask : 255.255.255.192
Gateway : 192.168.1.62
```

## PC-1B
```bash
IP Address : 192.168.1.97
Subnet Mask : 255.255.255.240
Gateway : 192.168.1.110
```

## PC-1C
```bash
IP Address : 192.168.1.113
Subnet Mask : 255.255.255.240
Gateway : 192.168.1.126
```

---

# 🌐 Configuration du Serveur

```bash
IP Address : 192.168.1.65
Subnet Mask : 255.255.255.224
Gateway : 192.168.1.94
```

---

# ✅ Vérification

## Tester la connectivité
```bash
ping 192.168.1.1
ping 192.168.1.129
ping 192.168.1.137
```

## Afficher la table de routage
```bash
show ip route
```

---

# 🛠️ Technologies utilisées
- Cisco Packet Tracer
- VLSM
- Routage Statique
- Réseaux TCP/IP

---

# 🔮 Perspectives

Ce projet permet de comprendre et maîtriser les bases du découpage réseau avec la technique VLSM ainsi que la configuration des routeurs Cisco.  
Dans les prochaines étapes, plusieurs améliorations peuvent être réalisées :

- Implémenter des protocoles de routage dynamique comme RIP, OSPF ou EIGRP
- Ajouter des mécanismes de sécurité réseau
- Configurer des VLANs pour améliorer la segmentation
- Mettre en place un serveur DHCP pour l’attribution automatique des adresses IP
- Ajouter un pare-feu et des ACL (Access Control Lists)
- Étendre l’architecture réseau avec davantage de routeurs et de sous-réseaux
- Simuler une connexion Internet réelle
- Surveiller le réseau avec des outils d’administration réseau

Ce projet constitue une base solide pour approfondir les compétences en administration et configuration des réseaux informatiques.

# 📚 Auteur
Projet réalisé dans le cadre d’un TP Réseaux – VLSM.
