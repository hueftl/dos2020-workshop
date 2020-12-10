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
    path: '', // Leerer Pfad -> Beim Aufruf von http://my-domain.com wird z.B. die HomeComponent geladen
    component: HomeComponent,
  },
  {
    path: 'login',
    component: LoginComponent
  }
];
```

Damit unsere App bei dem aufgerufenen Pfad die richtige Komponent darstellt, müssen wir in der root-Ebene eine Platzhalter-Komponente `<router-outlet></router-outlet>`, die mit `RoutingModule` bereitgestellt wird, aufrufen. D.h. dass wir in unserer `app.component.html` diesen `<router-outlet></router-outlet>` platzieren.

**WICHTIG:** Wenn die Route auch Kind-Routen haben, dann muss die oberste Ebene in dem HTML-Komponent-File den `<router-outlet></router-outlet>` haben. Sonst wird das Routing nicht funktionieren.

Wenn wir uns in der URL vertippen und die App die Route nicht finden kann, dann bekommen wir eine "404 Page not found"-Seite. Das ist nicht so wirklich nutzerfreundlich. Für diesen Fall können wir eine Wildcard-Route in unserem `routes`-Array definieren:
```
const routes: Routes = [
  {
    path: '',
    component: HomeComponent,
  },
  {
    path: 'login',
    component: LoginComponent
  },
  // hier stehen andere Routen
  {
    path: '**',
    redirectTo: '' // Hier geben wir an, auf welchen Pfad ein Redirect stattfinden soll
  }
];
```
**WICHTIG:** Die Wildcard-Route soll immer am Ende des Arrays stehen.

Wenn unsere App wächst und wir immer mehr Routen bekommen werden, dann macht es Sinn einige Bereiche der App nur dann laden, wenn diese tatsächlich über eine bestommte Route aufgerufen werden. Diesen Mechanismus in Angular nennt man `LazyLoading`. D.h. dass wir einen bestimmten Bereich unserer App in ein extra Module (`FetureModule`) auslagern. Dieser `FeatureModule` bekommnt dann eigenes Routing. Somit können wir unsere bisschen aufsplitten und die initiale Ladezeit verkürzen.

Um das besser zu verstehen, machen wir den `blog`-Bereich (Feature) unserer App zu einem `BlogModule` mit eigenem `BlogRoutingModule`.

In der `/blog/blog-routing.module.ts` erstellen wir unsere `blogRoutes`, die wir an den `RouterModule` als `forChild` (Kind)-Routen übergeben.
Das sieht wie folgt aus:
```
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { BlogComponent } from './blog.component';
import { BlogListComponent } from './blog-list/blog-list.component';
import { BlogSingleComponent } from './blog-single/blog-single.component';

const blogRoutes: Routes = [
  {
    path: '',
    component: BlogComponent,
    children: [
      {
        path: '',
        component: BlogListComponent
      },
      {
        path: ':id', // <-- Routen-Parameter, den wir auslesen können. Syntax -> :<parameter-name> z.B. :id -> blog/123
        component: BlogSingleComponent
      }
    ]
  }
];

@NgModule({
  imports: [RouterModule.forChild(blogRoutes)],
  exports: [RouterModule]
})
export class BlogRoutingModule { }
```
Mit `children` können wir Unter-Routen definieren. Der Aufbau ist der selbe.


Als nächsten wollen wir unser `BlogModule` per `LazyLoading` laden. Dazu müssen wir in unserem `AppRoutingModule` eine `LazyLoading`-Route definieren:
```
const routes: Routes = [
  // andere Routen
  {
    path: 'blog',
    loadChildren: () => import('./blog/blog.module').then(m => m.BlogModule)
  },
  {
    path: '**',
    redirectTo: ''
  }
];
```
Mit `loadChildren` laden wir einfach unseren `BlogModule`, in welchem der `BlogRoutingModule` importiert ist. Beim `build`-Prozess wird dieser Module zu einer separaten JS-Datei gebaut und per klick auf die `/blog`-Route geladen.

So haben wir unsere Web-App etwas performanter gemacht.

**AUFGABE:** Denn `/admin`-Bereich wollen wir auch als `FeatureModule` per `LazyLoading` laden.
