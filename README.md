# Tudilo

**Your day. Your buddy.**

Tudilo ist ein freundlicher, lokaler Aufgaben- und Listenbegleiter. Dieser erste Entwurf ist eine installierbare, responsive PWA – bewusst ohne Backend und ohne Store-Abhängigkeit.

## Prototype 0.1

- Dashboard mit Tagesübersicht und Buddy-Begrüßung
- Listen mit Kategorien erstellen
- Aufgaben hinzufügen und abhaken
- Notizen lokal festhalten
- Speicherung im Browser via localStorage
- PWA-Manifest und Service Worker für Offline-App-Shell
- Responsive Layout für Smartphone und Tablet

## Lokal starten

```bash
python3 -m http.server 8080
```

Dann `http://localhost:8080` öffnen. Für die Installation als PWA ist HTTPS erforderlich (localhost ist für Entwicklung ausgenommen).

## Wichtige Grenzen

Apple-Notizen- und Apple-Kalender-Anbindung sind **noch nicht implementiert**. Eine normale iOS-PWA kann nicht frei im Hintergrund auf die Datenbanken dieser Apps zugreifen. Die Notes-Integration wird deshalb über eine getestete Kurzbefehle-Brücke oder eine native Erweiterung umgesetzt. Bis dahin werden keine Apple-Synchronisationen vorgetäuscht.

## Design

- Midnight Blue: `#253442`
- Warm Sand: `#E7CFA8`
- Sage: `#A9BBAE`
- Soft Cream: `#F3F0E9`

## Nächste Schritte

1. Datenmodell auf IndexedDB migrieren und Export/Backup ergänzen.
2. Listen- und Aufgabenverwaltung um Bearbeiten, Löschen, Fälligkeit und Priorität erweitern.
3. Apple-Kurzbefehle-Prototyp für eine Apple-Notiz pro Tudilo-Liste testen.
4. Kalender-Workflow und Konfliktregeln spezifizieren.
5. PWA-Icons, Screenshots und Installationshinweise ergänzen.
