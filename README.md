# Arma 3 – Simple Quad Airdrop Script (SQF)

**Funktion** Per Tastenkombi **CTRL + O** kann jeder Spieler ein **Quad** anfordern. Es wird per **Fallschirm** abgeworfen, ein **Kartenmarker** hilft beim Finden, und nach Nutzung räumt sich alles wieder auf. **Cooldown:** 5 Minuten pro Spieler.

## Features
- **Einfache Bedienung:** Keybind drücken → Scroll-Menü (Standart Ingame Funktion) → „Request / Confirm / Cancel“
- **Fallschirmabwurf** exakt zur Spielerposition
- **Marker** pro Team/Seite, Auto-Cleanup (Einsteigen / Zerstören / Löschen / Timeout)
- **Hold-Action „Delete Quad“** beim Anschauen des Fahrzeugs
- **Mehrspieler-robust:** `remoteExec`, `publicVariable`, JIP-Handling, Respawn-Reinit
- **Sicherheit:** Doppeltstart-Schutz, Cooldown

## Demo
Showcase-Kurzvideo: https://youtu.be/V0gydLsA_js
(Leicht veraltet, funktioniert jedoch gleich)

## Verwendung
1. Ingame die Debug Console öffnen und das Skript dort einfügen, dann mit dem Local Exec Knopf ausführen. 

## Technik
- Netzwerk: `remoteExec`, `publicVariable`, Join In Progress (JIP)-IDs
- EventHandler: Display: KeyDown, AddAction für das Scroll Menü, GetIn/Killed/Deleted für das Quad, Respawn zum re-adden des Keybinds nach dem Tod.
- Cleanup: Timer + Event-basiert

## Rechtlicher Hinweis
Darf frei kopiert, genutzt und verändert werden. 

## Lizenz
MIT
