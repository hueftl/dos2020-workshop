# Aufgabe 1: Projekt & Projektstruktur erstellen

Die Entwicklungsumgebung steht und wir können mit unserem Angular-Projekt starten!

## Schritt 1: Projekt erstellen

Mit Hilfe der Angular CLI erstellen wir unser Projekt.

```
ng new <project-name>
```

Die gewünschten Einstellungen in der CLI-Abfragen auswählen.

Nach dem das Projekt angelegt wurde, gehen wir nun in den Projekt-Ordner:

```
cd <project-name>
```

## Shritt 2: Angular-Material Komponenten zu dem Projekt hinzufügen

Mit Hilfer der Angular Schematics fügen wir Angular Material Komponenten unserem Projekt zu.

```
ng add @angular/material
```

## Schritt 3: Projektstruktur erstellen

Bei kleinen Projekten wird an die Projektstruktur nicht so wirklich gedacht. Doch das Projekt kann wachsen und es werden immer mehr und mehr Komponenten, Services, Module, etc. Es wird unübersichtlich und das führt dazu, dass die Produktivität bei der Entwicklung sinkt. Um das zu vermeiden, ist eine saubere Struktur in dem Angularprojekt sehr wichtig.

Es gibt keine ideale Vorgabe für die Struktur des Angular-Projekts. Doch das Framework Angular bietet einige hilfreiche Features dafür. So können wir mittels Modulen, Komponenten und Services unsere App logisch aufteilen.

Der beste Weg ist es, die Angular-App im Voraus ein wenig planen. Dabei kann man grob die gewünschten Features notieren. Anhand der Skizze kann man die Struktur aufbauen.

Beispiel:

Unsere Angular-App wird folgende Bereiche haben:
- Startseite (home)
- Login (login)
- Registrierung (register)
- Blog (blog)
  - Liste von Blogbeiträgen (blog-list)
  - Detailansicht eines Blogbeitrags (blog-single)
- Admin (admin)
  - Dashboard (dashboard)
  - Verwaltung von Blogbeiträgen (posts)

Neben den vorhandenen Features werden noch Helfer wie z.B. Module, Service, Intercepter, Interfaces, etc. benötigt. Für die Aufteilung dieser gibt es verschiedene Möglichkeiten. Wir würden wie folgt vorgehen:
- services - Ordner für alle unsere Services
  - `<feature-name>` - Services werden soweit es geht pro Feature aufgeteilt. Hier werden auch die dazugehörigen Interfaces abgelegt.
- helpers - Ordner für Intercepter, Guards
- ui - Ordner für UI Helfer (Pipes, Directives, UI-Lib Module)

Wir haben jetzt fast alles. Wir haben jetzt Features, welche wir nicht direkt am Anfang des initialen Ladens der Anwendung brauchen. Diese würden wir in eigene Feature-Module aufteilen, um diese später nur dann laden, wenn diese wirklich benötigt werden. Diese Module werden per LazyLoading geladen.

So dann erstellen wir unsere Struktur!