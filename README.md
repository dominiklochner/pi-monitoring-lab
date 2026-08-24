# Pi Monitoring Lab

Monitoring- und Operations-Skills auf demselben Raspberry Pi wie [Pi Network Lab](https://github.com/dominiklochner/pi-network-lab) und [Pi Docker Stack](https://github.com/dominiklochner/pi-docker-stack) — Schritt für Schritt aufgebaut entlang der CompTIA-Network+-Domäne "Network Operations".

## Ziel

Dem Netzwerk beim Arbeiten zusehen, statt nur Dienste einzurichten: Traffic mitschneiden und verstehen, Geräte automatisiert abfragen, Logs zentral auswerten, einen Normalzustand definieren und Backups wirklich testen — nicht nur einrichten.

## Phasen

| Phase | Thema | Network+ Domäne |
|---|---|---|
| 0 | Doku-Grundlage (Netzwerkdiagramm, Asset-Inventar) | Domain 3: Network Operations |
| 1 | Packet Capture (tcpdump/Wireshark) | Domain 3 (Monitoring), reinforced: Domain 1 (OSI) |
| 2 | SNMP-Monitoring | Domain 3: Network Operations |
| 3 | Log-Aggregation | Domain 3: Network Operations |
| 4 | Baseline & Configuration Management | Domain 3: Network Operations |
| 5 | Disaster Recovery (Backup + echter Restore-Test) | Domain 3: Network Operations |

## Umgebung

- Raspberry Pi (derselbe wie bei [Pi Network Lab](https://github.com/dominiklochner/pi-network-lab), Raspberry Pi OS Lite)
- MacBook Pro (M1) als Steuerzentrale / SSH-Client / Wireshark

## Struktur

```
docs/         Ein Dokument pro Phase
diagrams/     Netzwerkdiagramme (Mermaid)
screenshots/  Belege pro Phase
```

## Verwandte Projekte

- [pi-network-lab](https://github.com/dominiklochner/pi-network-lab) — native Netzwerkdienste auf demselben Pi (DNS, VPN, Firewall), wird hier überwacht
- [pi-docker-stack](https://github.com/dominiklochner/pi-docker-stack) — Container-Stack auf demselben Pi
- [homelab-network-project](https://github.com/dominiklochner/homelab-network-project) — Windows Server AD, OPNsense, VLANs
