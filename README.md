# Teller-Match

Gemeinsam Essen auswählen: 72 Gerichte, persönliche Bewertungen, gemeinsame Matches und eigene Gerichte mit Fotos.

## Eure Handys verbinden

1. [Teller-Match öffnen](https://danizahnweh-oss.github.io/teller-match/) und den eigenen Namen wählen.
2. Auf dem ersten Handy unter **Mehr → Gemeinsamen Bereich erstellen** einen privaten Bereich anlegen.
3. **Einladungslink erstellen**, kopieren und persönlich an die andere Person schicken.
4. Den Link auf dem zweiten Handy öffnen und **Einladung annehmen** wählen. Der zweite Platz wird automatisch zugewiesen.
5. Auf beiden Geräten unter **Gerätewechsel und Wiederherstellung** den persönlichen Wiederherstellungscode anzeigen und sicher aufbewahren.

Einladungen gelten 24 Stunden und sind einmal verwendbar. Nicht auf beiden Handys getrennte Bereiche erstellen. Für den Beitritt den Browser verwenden, in dem bisher bewertet wurde, damit dessen lokale Daten übernommen werden können.

Bewertungen, Matches, eigene Gerichte, Fotos, Namen und die Ausschlussliste werden automatisch geteilt. Die Auswahl des eigenen Namens ist nach der Verbindung an den persönlichen Platz gebunden. Ohne Internet werden Änderungen gespeichert und beim nächsten Verbindungsaufbau übertragen. Bei einer ausstehenden Übertragung steht unter **Mehr** eine Wiederholungsfunktion bereit.

## Alte Daten und Gerätewechsel

Beim ersten Verbinden werden nur die Bewertungen der auf diesem Gerät zuvor gewählten Person und deren eigene Gerichte übernommen. Bereits per Code importierte Bewertungen der anderen Person werden nicht erneut als eigene hochgeladen. Die ursprüngliche lokale Sicherung und die lokalen Fotodateien bleiben erhalten. Bestehende Online-Bewertungen werden durch einen erneuten Import nicht überschrieben.

Auf einem neuen Gerät unter **Mehr → Vorhandenen Platz wiederherstellen** den persönlichen Code eingeben. Dadurch wird der Zugriff des bisherigen Geräts entzogen. Anschließend gilt ein neuer Wiederherstellungscode. Ohne ein noch angemeldetes Gerät oder einen gültigen Wiederherstellungscode lässt sich der anonyme Platz nicht wiederherstellen. Browserdaten nicht löschen, solange Änderungen oder Fotos noch nicht übertragen sind.

## Datenhaltung und Betrieb

- Eigenes Supabase-Projekt `teller-match`, Region Frankfurt, kostenloser Tarif; Website auf GitHub Pages.
- Anmeldung über anonyme Supabase-Geräteidentitäten. Nur Mitglieder desselben Bereichs dürfen dessen Daten lesen; Bewertungen schreibt ausschließlich die jeweilige Person.
- Alle Tabellen besitzen Row Level Security. Schreibzugriffe erfolgen über die geprüften Datenbankfunktionen. Einladungen und Wiederherstellungscodes liegen serverseitig nur als SHA-256-Hashes vor.
- Fotos werden vor dem Übertragen auf höchstens 900 Pixel verkleinert. Der private Speicher akzeptiert JPEG bis 2 MB. Bildzugriffe benötigen eine gültige Mitgliedschaft und zeitlich begrenzte Links (fünf Minuten).
- `sync-config.js` enthält ausschließlich die öffentliche Projektadresse und den **Publishable Key**. Verwaltungsschlüssel, Service-Role-Schlüssel, Datenbankpasswörter und persönliche Codes gehören niemals ins Repository.
- `supabase/migrations/202610220001_sync.sql` enthält die vollständige Ersteinrichtung. Nur auf ein neues Projekt anwenden; nicht wiederholt über eine bestehende Installation ausführen.
- Supabase kann kostenlose Projekte bei längerer Inaktivität pausieren. In diesem Fall das Projekt im Dashboard wieder aufnehmen. Es wurden keine kostenpflichtigen Funktionen aktiviert.

## Entwicklung und Prüfung

Node.js ab Version 22 verwenden. `npm ci` installiert die festgeschriebenen Abhängigkeiten, `npm run build` baut die lokal ausgelieferte Supabase-Bibliothek. `npm test` prüft Offline-Warteschlange, Datenbankregeln, Kopplung, abgelaufene Einladungen, Zurücksetzen und Wiederherstellung mit einer getrennten lokalen PostgreSQL-Testdatenbank.

`TM_RUN_LIVE=1 node tests/live.mjs` ist ein ausdrücklich gestarteter Integrationstest gegen das konfigurierte Supabase-Projekt. Er erzeugt ausschließlich eigene private Testkonten und einen Testbereich; geprüft werden auch Echtzeit-Events und private Fotoübertragung. Diese Testdaten bleiben getrennt von den echten Bereichen bestehen.

GitHub Pages veröffentlicht `main` automatisch. Nach Änderungen Tests ausführen, committen, pushen und den Pages-Build kontrollieren. Auf zwei Geräten verifizieren, dass beide denselben Bereich verwenden und Änderungen ohne Neuladen erscheinen.

Die Ausschlussliste ist ein Abgleich gegen die hinterlegten Zutaten, keine vollständige Allergenprüfung. Quellen und Lizenzen der Gerichtefotos stehen in [BILDNACHWEISE.md](BILDNACHWEISE.md).
