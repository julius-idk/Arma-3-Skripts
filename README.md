# Arma 3 – Simple Quad Airdrop Script (SQF)

**Funktion** Per Tastenkombi **CTRL + O** kann jeder Spieler ein **Quad** anfordern. Es wird per **Fallschirm** abgeworfen, ein **Kartenmarker** hilft beim Finden, und nach Nutzung räumt sich alles wieder auf. **Cooldown:** 5 Minuten pro Spieler.

## Features
- **Einfache Bedienung:** Keybind drücken → Scroll-Menü (Standart Ingame Funktion) → „Request / Confirm / Cancel“
- **Fallschirmabwurf** exakt zur Spielerposition
- **Marker** pro Team/Seite, Auto-Cleanup (Einsteigen / Zerstören / Löschen / Timeout)
- **Hold-Action „Delete Quad“** vor Ort
- **Mehrspieler-robust:** `remoteExec`, `publicVariable`, JIP-Handling, Respawn-Reinit
- **Sicherheit:** Doppeltstart-Schutz, Cooldown

## Demo
Showcase-Kurzvideo: https://youtu.be/V0gydLsA_js

## Installation
1. Den gesammten Skript ingame per Debug Console ausführen 
2. In `mission_demo/init.sqf` (Beispiel unten) einbinden.  
3. Spiel starten, **CTRL + O** drücken, mit Mausrad bestätigen.

## Technik
- Netzwerk: `remoteExec`, `publicVariable`, JIP-IDs
- Events: KeyDown, AddAction, GetIn/Killed/Deleted, Respawn
- Cleanup: Timer + Event-basiert

## Rechtlicher Hinweis
Nur in autorisierten Umgebungen nutzen. Keine Tests auf fremden Servern ohne Erlaubnis.

## Lizenz
MIT
