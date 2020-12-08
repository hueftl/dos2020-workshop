# Task 2: Routing & Navigation

Wir haben eine Projekt-Struktur für unsere Real-World Web-App erstellt. Anhand unserer Projekt-Struktur können wir schnell erkennen, dass wir unterschiedliche Seiten haben. Um diese Seiten in unserer App zu erreichen, brauchen wir Routing. Angular erspart uns die Arbeit und bringt out-of-the-box einen `RoutingModule`, der uns die Umsetzung des Routings ermöglicht.

## Schritt 1: RoutingModule für unsere App erstellen

Wir müssen für unsere App einen `AppRoutingModule` auf der root-Ebene nebem dem `AppModule` erstellen. Dazu nutzen wir folgenden befehl:
```
ng g m app-routing --flat
```

**WICHTIG:** Angular Styleguide gibt uns folgende Regeln für die Bennenung von `RoutingModule`:
```
[modulename]-routing
```
D.h. dass bei der Verwendung von FeatureModulen, welche per `LazyLoading` geladen werden, nebem dem `FeatureModule` auch entsprechender `RoutingModule` erstellt werden muss.

Beispiel:

Unsere Blog-Seite wird zum `FeatureModule`. In dem Ordner `blog` erstellen wir ein `BlogModule` und daneben unser `BlogRoutingModule`.

In dem erstellten `AppRoutingModule` müssen wir den `RoutingModule` in den `imports` importieren und in `exports` exportiren.

Bei dem Importieren von `RoutingModule` müssen wir diesem unser `routes`-Array in der `forRoot()`-Methode übergeben.

**WICHTIG:** In der ganzen Angular-App kann nur ein `RoutingModule.forRoot()` sein. Die FeatureModule müssen `.forChild(routes)` verwenden.

```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';

const routes: Routes = [];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

## Schritt 2: Routen erstellen

Als nächstes müssen wir in unserem `routes`-Array unsere Routen definieren. 
Ein Routen-Objekt sieht wie folgt aus:
```
{
  path: '<path-name>',
  component: <KomponentenKlassenName>
}
```
Die Route wird dann über `http://my-domain.com/<path-name>` im Browser aufgerufen und die entsprechende Komponente wird geladen.

In dem `routes`-Array erstellen wir für den Anfang folgende Routen:
```
const routes: Routes = [
  {
    path: '', // 
    component: HomeComponent,
  },
  {
    path: 'login',
    component: LoginComponent
  }
];
```

