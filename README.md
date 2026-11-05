# Home Assistant: Apple Erinnerungen ↔ To-do-Listen

Konfigurationen, um Apple Erinnerungen mit Home Assistant zu synchronisieren
und die Listen in einem Dashboard anzuzeigen.

## 1. To-do-Listen in HA anlegen

Einstellungen > Geräte & Dienste > Integration hinzufügen > **Local To-do**.
Lege pro Liste einen Eintrag an. Die Dashboards erwarten diese IDs:

| Liste    | Entität               |
|----------|-----------------------|
| Einkauf  | `todo.einkaufsliste`  |
| Haushalt | `todo.haushalt`       |
| Termine  | `todo.termine`        |

Heissen deine Entitäten anders, passe die IDs in den YAML-Dateien an.

## 2. Synchronisation mit Apple Erinnerungen

**Variante A: Reminders Sync in der Companion-App (empfohlen)**
iPhone/iPad > HA-App > Einstellungen > Companion App > **Reminders Sync**.
Verbinde pro Liste eine Erinnerungen-Liste mit einer HA-To-do-Liste und lege fest,
welche Seite bei Konflikten gewinnt.
Hinweis: Bei geöffneter App kommen Änderungen innert Sekunden an, sonst erst beim
nächsten Öffnen oder beim nächsten Hintergrund-Sync.

**Variante B: Mac als Brücke (nahezu Echtzeit)**
ReminderRelay läuft als Hintergrunddienst auf macOS und synchronisiert in beide
Richtungen. Standard ist eine Abfrage alle 30 s (einstellbar von 10 s bis 5 min).
Dafür muss der Mac dauerhaft laufen.

## 3. Dashboard

| Datei | Voraussetzung | Beschreibung |
|-------|---------------|--------------|
| [`dashboards/listen.yaml`](dashboards/listen.yaml) | keine | Abschnitte-Layout mit Übersichts-Tiles und je einer To-do-Karte pro Liste |
| [`dashboards/listen-mushroom.yaml`](dashboards/listen-mushroom.yaml) | HACS + Mushroom | Wie oben, mit Begrüssung, Zähler-Chips und Status-Karten |

**Einfügen:** Einstellungen > Dashboards > *Dashboard hinzufügen* > *Neues Dashboard
von Grund auf* > Dashboard öffnen > ✏️ Bearbeiten > ⋮ > *Raw-Konfigurationseditor*
und dort den Inhalt der Datei einfügen.

Sortierung pro Karte über `display_order`: `manual`, `alpha_asc`, `alpha_desc`,
`duedate_asc`, `duedate_desc`.

## 4. Sprachsteuerung mit Claude als Assist-Agent

Ist Claude (Integration *Anthropic*) als Konversationsagent in deiner
Assist-Pipeline eingerichtet und sind die To-do-Entitäten für Assist freigegeben
(Einstellungen > Sprachassistenten > Entitäten freigeben), kannst du zum Beispiel sagen:

> «Setz Batterien auf die Einkaufsliste.»

Der Eintrag landet in `todo.einkaufsliste` und über Reminders Sync auch in Apple
Erinnerungen.
