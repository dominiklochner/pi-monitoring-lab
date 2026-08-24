# Phase 1 — Packet Capture

## Ziel

Traffic auf dem Pi mitschneiden und in Wireshark analysieren, um Netzwerkprotokolle praktisch statt nur theoretisch zu verstehen — direkter Bezug zu Domain 1 (OSI-Modell) und Domain 5 (Troubleshooting-Methodik).

## Werkzeuge

- `tcpdump` auf dem Pi (Mitschnitt)
- Wireshark auf dem Mac (Analyse), Transfer per `scp`

## Stolperstein: AppArmor blockiert `/tmp`

Erster Versuch, nach `/tmp/dns-capture.pcap` zu schreiben, scheiterte zweimal mit `Permission denied` — auch nachdem Sudo-Rechte per `sudo -v` explizit erneuert wurden. Ursache: AppArmor auf Debian 13 (trixie) schränkt `tcpdump`s Schreibrechte ein, `/tmp` ist nicht freigegeben. Workaround: ins Home-Verzeichnis schreiben (`~/dateiname.pcap`), das funktioniert ohne Anpassung. AppArmor-Details noch nicht weiter untersucht (kein Blocker, Workaround reicht für dieses Projekt).

**Nebenlektion:** ein per `sudo cmd &` gestarteter Hintergrundprozess lässt sich nicht mit `sudo kill %1` beenden — `%1` ist eine Bash-Job-Referenz, die nur die Shell selbst versteht, nicht das externe `kill`-Programm, das `sudo` aufruft. Zwei funktionierende Alternativen: die tatsächliche PID aus `jobs -l` nehmen, oder beim Start `$!` in eine Variable sichern (`PID=$!; ...; sudo kill $PID`) — letzteres ab hier Standard-Vorgehen für Hintergrund-Mitschnitte.

## Szenario 1: DNS-Query gegen Pi-hole

```bash
sudo tcpdump -i eth0 -w ~/dns-capture.pcap port 53 &
dig @127.0.0.1 github.com
sleep 1
sudo kill <PID>
```

**Befund:** Anfrage und Antwort (Paket 1 → Paket 4) über die **Transaction ID** (`0xc24b`) verknüpft — Kernschutzmechanismus gegen DNS-Spoofing/-Poisoning (Domain 4): eine gefälschte Antwort müsste dieselbe ID treffen. Query-Paket: `Answer RRs: 0`, `Additional RRs: 1` (EDNS-Metadaten, kein echter Record). Antwort-Paket: `Answer RRs: 1` (die aufgelöste IP `140.82.121.3`).

Screenshot: `screenshots/phase1-osi-ssh.png` (OSI-Schichten am Beispiel einer laufenden SSH-Verbindung — Ethernet II = L2, IPv4 = L3, TCP = L4, SSH = L7; TTL 64, IPv4-Header 20 Bytes)

## Szenario 2: TCP Three-Way-Handshake

```bash
sudo tcpdump -i eth0 -w ~/tcp-handshake.pcap 'tcp port 443' &
TCPDUMP_PID=$!
sleep 1
curl -Is https://example.com -o /dev/null
sleep 1
sudo kill $TCPDUMP_PID
```

**Befund:** Handshake wie erwartet — Client eröffnet immer (er will etwas vom Server, der Server wartet passiv):

```
1  Mac → Server:443   [SYN]
2  Server:443 → Mac   [SYN, ACK]
3  Mac → Server:443   [ACK]
```

Erst danach beginnt der eigentliche Datenaustausch. Relevanz fürs Troubleshooting: kommt kein SYN-ACK zurück, liegt das Problem meist an einer Firewall unterwegs oder einem nicht lauschenden Dienst auf dem Zielport — nicht am Client selbst.

Screenshot: `screenshots/phase1-tcp-handshake.png`

## Szenario 3: WireGuard-Handshake

*(offen — folgt in einer nächsten Session)*

## Nächster Schritt

→ WireGuard-Handshake-Mitschnitt, danach Phase 2 (SNMP)
