# 🔍 Diagnostic Réseau : Panne d'Attribution d'Adresse IP (DHCP)

## 📌 Présentation du Labo
Ce laboratoire virtuel reproduit et résout une panne d'attribution d'adresse IP via DHCP dans un environnement isolé sous **Kali Linux** (sans routeur physique, sans câble et sans connexion internet).

---

## 🔴 1. Analyse de la Panne
Le client envoie des requêtes `DHCP Discover` en broadcast (`0.0.0.0` → `255.255.255.255`, UDP `68` → `67`), mais aucune réponse n'arrive de la part d'un serveur DHCP.

- **Capture Wireshark (filtre `dhcp`)** : 3 tentatives `Discover`, 0 réponse `Offer`.
- **Résultat** : Aucune adresse IPv4 attribuée. Seule une adresse link-local IPv6 (`fe80::`) est présente sur l'interface.
<img width="843" height="614" alt="1_panne_dhcp_wireshark" src="https://github.com/user-attachments/assets/a59f6d4e-5cbd-47ac-a66a-c849d12de28d" /><img width="957" height="524" alt="3_ip_statique_passerelle_simulee" src="https://github.com/user-attachments/assets/2d61299e-2016-4efe-b9b9-b38877a598f6" />
<img width="1109" height="660" alt="2_panne_dhcp_terminal" src="https://github.com/user-attachments/assets/fd72a223-cbca-41a4-8aec-1364705531e4" />



---

## 🔍 2. Démarche de Diagnostic
1. `ping 127.0.0.1` : Vérification du fonctionnement de la pile TCP/IP locale.
2. `ip a` : Vérification de la configuration de l'interface réseau.
3. **Analyse Wireshark** : Confirmation de l'absence de réponse DHCP.
4. `ping <passerelle>` : Test de la connectivité réseau local.

---

## ✅ 3. Solution de Contournement (IP Statique)
Attribution manuelle d'une adresse IP statique et ajout de la route par défaut :

<img width="957" height="524" alt="3_ip_statique_passerelle_simulee" src="https://github.com/user-attachments/assets/ff204446-942a-4e2b-815e-118d37d702e5" />

```bash
sudo ip addr add 192.168.1.150/24 dev veth-c
sudo ip route add default via 192.168.1.1
