# 🚦 Moodle Lernstands-Ampel (Widget)

Ein smartes JavaScript-Widget für Moodle, das den Lernfortschritt aus einer **Lernlandkarte** visualisiert und direkt in ein **Datenbank-Modul** zurückschreibt. Es ermöglicht Schülern eine schnelle Status-Rückmeldung (🔴, 🟠, 🟢) direkt im Kurs-Interface, ohne die Seite verlassen zu müssen.

---

## ✨ Features

* **Nahtlose Integration in die Blockleiste:** Läuft als HTML-Block in der Blockleiste.
* **Auto-Erkennung:** Identifiziert die aktuelle Aufgabe automatisch anhand des Seitentitels oder Inhalts.
* **Zwei-Wege-Sync:** Liest Daten aus der Moodle-Datenbank und speichert Status-Updates via `fetch` (kein Neuladen nötig).
* **Teacher-Zone:** Kompakte Auswertungs-Sektion für Lehrkräfte (nur sichtbar, wenn Bearbeitungsrechte vorliegen).
* **SuS-Zone:** Kompakte Auswertungs-Sektion für SuS in der Blockleiste-Lernlandkarte.
* **Aktivitätsseiten-Block bei SuS:** zeigt die zur Aktivität passende Ampel zum Feedback absenden.
* **Offline-Fallback:** Nutzt `localStorage` für sofortiges visuelles Feedback.

---

## 🛠️ Vorbereitung in Moodle
⚠️Aktivität Datenbank Einstellungen  - Freigabe JA aktivieren nicht vergessen.⚠️

Damit das Widget funktioniert, müssen eine **Lernlandkarte** und ein **Datenbank-Modul** im Kurs existieren.

BYCS-LK können die Vorlage "Moodle-Widget Lernfortschritt-Ampel (🔴🟠🟢)" nehmen 

### 1. Das Datenbank-Modul konfigurieren
Erstelle eine Datenbank mit zwei Feldern vom Typ **"Kurzer Text"**:
* Feld 1 Name: `Aufgabe`
* Feld 2 Name: `Status`

#### Vorlagen-Setup (WICHTIG):
Kopiere diesen Code in die **Listenansicht** deiner Datenbank, damit das Widget die Daten auslesen kann.

**Tabellen-Kopf (Header):**

```html
<table class="table table-striped">
  <thead>
    <tr><th>Schüler/in</th><th>Aufgabe</th><th>Status</th></tr>
  </thead>
  <tbody>
```

**Wiederholter Bereich (Repeated entry):**
```html
<tr class="db-reihe">
  <td class="db-user">##user##</td>
  <td class="db-task">[[Aufgabe]]</td>
  <td class="db-status">[[Status]]</td>
</tr>
```

**Tabellen-Fuß (Footer):**
```html
  </tbody>
</table>
```

## 🔍 Konfiguration & Installation

1. IDs ermitteln
Du benötigst vier spezifische IDs aus deinem Moodle-Kurs:
 * DATENBANK_MODUL_ID: Die Zahl am Ende der URL deiner Datenbank (.../mod/data/view.php?id=XXXX).
 * LERNLANDKARTE_ID: Die Zahl am Ende der URL deiner Landkarte (.../mod/learningmap/view.php?id=XXXX).
 * FELD_IDs ermitteln: * Gehe in der Datenbank auf den Reiter "Eintrag hinzufügen".
   * Klicke mit der rechten Maustaste in das Eingabefeld für "Aufgabe" -> "Untersuchen".
   * Suche im HTML-Code nach name="field_12345". Kopiere diesen Wert für FELD_AUFGABE und FELD_STATUS.

2. Skript anpassen
Ersetze die Werte im APP_CONFIG Block am Anfang deines Codes:

```html
window.APP_CONFIG = {
    DATENBANK_MODUL_ID: "XXXXXXX",  // Deine Datenbank-ID
    LERNLANDKARTE_ID: "XXXXXXX",    // Deine Landkarten-ID
    FELD_AUFGABE: "field_XXXXXXX",  // Deine Feld-ID (Aufgabe)
    FELD_STATUS: "field_XXXXXXX"    // Deine Feld-ID (Status)
};
```

3. Einbetten in Moodle
 * Kopiere den gesamten HTML/Script-Code.
 * Erstelle einen Moodle-Block vom Typ "Textblock".
 * Schalte den Moodle-Editor auf HTML-Modus (<> Symbol) um.
 * Füge den Code ein und speichere.

📖 Funktionsweise
 * Initialisierung: Das Skript lädt beim Seitenaufruf alle bisherigen Einträge aus der Datenbank.
 * Mapping: Es scannt die Lernlandkarte im Hintergrund ab, um alle verfügbaren Aufgabennamen zu kennen.
 * Kontext-Logik:
 * Auf der Lernlandkarte zeigt das Widget eine komplette Liste aller Aufgaben als Übersicht.
   * In einer Aktivität (z.B. Test/Seite) vergleicht das Skript den Seitentitel mit den Aufgabennamen und zeigt nur die passende "Fokus-Ampel".
 * Speichervorgang: Beim Klick auf 🔴, 🟠 oder 🟢 wird im Hintergrund ein Formular an die Moodle-Datenbank gesendet.

👨‍🏫 Auswertung für Lehrkräfte

Das Widget erkennt automatisch Bearbeitungsrechte. Über den Button "👨‍🏫 Auswertung" erhältst du eine Live-Übersicht der Schülerergebnisse direkt im Kurs-Interface.

📄 Lizenz

Dieses Projekt ist unter der GNU General Public License v3.0 (GPL-3.0) lizenziert.

---

## 👤 Kontakt & Support
Erstellt von **Christian_Augsburg**
🐘 Mastodon: [@Christian_Augsburg@mastodon.social](https://mastodon.social/@Christian_Augsburg)

