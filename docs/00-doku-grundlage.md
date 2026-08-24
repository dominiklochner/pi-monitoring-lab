# Phase 0 — Doku-Grundlage

## Ziel

Bevor überwacht wird, muss dokumentiert sein, was überhaupt läuft. Zwei Ergebnisse: ein Netzwerkdiagramm (physisch + logisch) und ein Asset-Inventar der aktuellen Lab-Umgebung. Selbst schon Prüfungsstoff — Dokumentation ist eigenes Thema in Domain 3.

## Schritt 1: Bestandsaufnahme auf dem Pi

Per SSH auf dem Pi ausführen und Ergebnis hier zurückmelden:

```bash
# laufende Dienste
systemctl list-units --type=service --state=running

# offene Ports (welcher Dienst hört wo)
sudo ss -tulpn

# Docker-Container, falls pi-docker-stack schon Dienste hat
docker ps

# IP-Konfiguration
ip -br addr show

# Hostname
hostnamectl
```

## Schritt 2: Befund

*(wird nach Rückmeldung von Schritt 1 befüllt)*

| Dienst | Port | Projekt | Rolle |
|---|---|---|---|
| | | | |

## Schritt 3: Netzwerkdiagramm

*(Mermaid-Diagramm folgt, sobald Schritt 2 steht — physisch: Router/FritzBox → Pi ↔ Mac; logisch: welcher Dienst auf welchem Port)*

## Schritt 4: Asset-Inventar

*(Tabelle: Gerät, IP, MAC falls relevant, Rolle, verantwortliches Projekt — folgt nach Schritt 2)*

## Nächster Schritt

→ Phase 1: Packet Capture (`docs/01-packet-capture.md`, folgt)
