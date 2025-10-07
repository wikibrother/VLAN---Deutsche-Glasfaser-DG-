# VLAN
Erstellt: 07.10.2025
BIO: Andy
## Anleitung: VLAN-Konfiguration der Deutschen Glasfaser (DG) im Heimnetz

Die Nutzung eines eigenen Netzwerks mit VLANs hinter einem Glasfaseranschluss erfordert die korrekte Identifizierung der vom Internetanbieter verwendeten **VLAN-IDs**.

Da die **Deutsche Glasfaser (DG)** diese IDs nicht immer offen kommuniziert und die Dienste (Internet, Telefonie) oft auf separaten VLANs laufen, ist der einfachste Weg zur Ermittlung der konfigurierten VLANs die Analyse der Provider-Hardware.

### Die Strategie: VLANs via Router auslesen

1.  **Vorbereitung:** Schließe deinen Router (z. B. eine FRITZ!Box) direkt an das Glasfaser-Modem (ONT/NT) an und lasse ihn die Internetverbindung herstellen. Viele Router erkennen die notwendigen VLANs während der automatischen Einrichtung.
2.  **Extraktion der Daten (FRITZ!Box):**
    * Navigiere zur Benutzeroberfläche deiner FRITZ!Box.
    * Gehe in den Bereich **Internet/Zugangsdaten/AVM-Dienste** und dort zu **Diagnosedatenzusammenfassung ansehen** (oder einem ähnlichen Diagnose-Log).
    * Dort kannst du die vom Provider automatisch konfigurierten **VLAN-IDs** für Internet und ggf. Telefonie auslesen.
3.  **Übernahme der IDs:** Sobald du die relevanten VLAN-IDs (z. B. **360** für Internet, **330** für VoIP, **342** für tr069) ermittelt hast, kannst du diese in deine **Switch-Konfiguration** übernehmen.

### Beispiel: Anwendung in der Switch-Konfiguration

Beim Einsatz von Managed Switches (wie dem **Netgear GS308E/GS308Ev4**) müssen die Ports, die den getaggten Verkehr transportieren sollen, als **Trunk-Ports** konfiguriert werden, die diese spezifischen VLAN-IDs (z. B. 330, 360, 342) zulassen.

* **Ziel:** Stelle sicher, dass der Port, der zum Router führt, alle benötigten **getaggten** VLANs transportiert, damit die Dienste korrekt getrennt und verarbeitet werden.

*(**Anmerkung:** Wenn du in diesem Zusammenhang auch **IPTV** nutzt, aktiviere unbedingt auf allen Switches in diesem VLAN das **IGMP Snooping**, um das Netzwerk vor unnötigem Multicast-Traffic zu schützen.)*

## Diagnosedaten
> In den Diagnosedaten sind die VLAN aufgeführt


````
diagnosis _FRITZ.Box_7590_AX_259.08.02_07.10.25_2011.txt
````

> In der Datei nach VLAN suchen



> VLAN Internet 360


```
0: name internet (attached, active internet)
0: sync_group: sync_ata
0: iface net_upstream0 RBE/18/dsl xx:xx:xx:xx:xx:xx stay online 1 vlan 360 prio 0 (prop: default internet) (prop6: default internet)
```
> VLAN VoIP 330



```
1: name voip (attached)
1: sync_group: sync_ata
1: iface net_upstream0 RBE/18/dsl xx:xx:xx:xx:xx:xx stay online 1 vlan 330 prio 0
```
> VLAN tr069 342

```
2: name tr069 (attached)
2: sync_group: sync_ata
2: iface net_upstream0 RBE/18/dsl xx:xx:xx:xx:xx:xx stay online 1 vlan 342 prio 0
```
