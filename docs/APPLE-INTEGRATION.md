# Apple-Integration – Konzept und technische Grenzen

## Apple Notizen: gewünschtes Verhalten

- Eine Tudilo-Liste entspricht genau einer Apple-Notiz.
- Erst wenn in Tudilo eine Liste erstellt wird, wird die zugehörige Apple-Notiz angelegt.
- Der Titel enthält die Markierung und Kategorie, z. B. `[todilo] [wochenende] Einkauf und Haushalt`.
- Aufgaben sind Checklistenpunkte innerhalb dieser Notiz – keine einzelne Notiz pro Aufgabe.
- Änderungen sollen in beide Richtungen synchronisiert werden.
- Löschen einer Liste in Tudilo löscht die Apple-Notiz nicht automatisch.

## Technische Realität

Eine installierte PWA auf iOS erhält keinen allgemeinen Hintergrundzugriff auf Apple Notes oder deren interne Datenbank. Eine Webseite kann auch keine beliebigen nativen Notizen still im Hintergrund überwachen. Deshalb ist die Integration in diesem Prototyp ausdrücklich noch nicht aktiv.

## Geplanter Weg: Kurzbefehle als Brücke

Ein iOS-Kurzbefehl soll gezielt Tudilo-Listen samt Aufgaben an Apple Notizen übergeben und Änderungen zurückliefern. Vor einer Implementierung muss ein Proof of Concept klären, welche Aktionen die aktuelle iOS-Version für Notizen und Checklisten zuverlässig anbietet und wie Tudilo den Kurzbefehl aufruft bzw. Daten austauscht.

## Synchronisationsregeln

Für die spätere Umsetzung erforderlich:

- stabile IDs für Listen und Aufgaben
- gespeicherte Zuordnung Tudilo-Liste ↔ Apple-Notiz
- zuletzt synchronisierter Stand und Änderungsquelle
- doppelte Aufgaben verhindern
- Konflikte sichtbar machen statt stillschweigend zu überschreiben
- Notizen beim Löschen in Tudilo zunächst erhalten

## Kalender

Die Kalenderanbindung ist ebenfalls offen. Ein PWA-Prototyp kann Kalenderdateien (ICS) exportieren bzw. Kalender-Weblinks anbieten; eine zuverlässige native Zwei-Wege-Synchronisation braucht einen gesonderten iOS-Workflow oder eine native App.


## Erster Übergabe-Prototyp (Shortcuts)

Die PWA kann jetzt beim Anlegen einer Liste den iOS-Kurzbefehl **Tudilo Sync** aufrufen und eine JSON-Nutzlast übergeben. Bestehende Listen haben zusätzlich die Aktion **An Apple Notizen senden**. Das ist eine explizite Übergabe – keine stille Hintergrundsynchronisierung.

### Kurzbefehl einmalig auf dem iPhone anlegen

1. Öffne Apples App **Kurzbefehle** und erstelle einen Kurzbefehl mit dem Namen exakt `Tudilo Sync`.
2. Füge die Aktion **Eingabe aus Kurzbefehl abrufen** bzw. die Eingabe des Kurzbefehls hinzu.
3. Füge **Text aus Eingabe abrufen** hinzu, falls die Eingabe als Text vorliegt.
4. Füge **Wörterbuch aus Eingabe abrufen** hinzu, um die JSON-Daten zu lesen.
5. Erstelle den Notiztext mit dem Titel `[todilo] [KATEGORIE] LISTENNAME` und den übergebenen Aufgaben als Checklisten-Zeilen.
6. Füge die Aktion **Neue Notiz erstellen** hinzu und aktiviere bei Bedarf die Rückfrage vor dem Erstellen.

Die Übergabe enthält `id`, `title`, `category` und `items` mit Aufgaben-IDs, Titeln und Erledigt-Status. Ein wiederholtes Ausführen kann in dieser ersten Stufe noch eine weitere Notiz erzeugen. Bitte deshalb zunächst mit einer Testliste arbeiten. Das automatische Finden und Aktualisieren derselben Notiz sowie die Rücksynchronisierung werden erst nach erfolgreichem PoC ergänzt.
