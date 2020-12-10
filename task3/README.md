# Task 3: Kommunikation mit REST API (CRUD-Befehle)

Unsere Web-App soll dynamischer werden. Wir wollen unter `/blog`-Route unsere Blog-Beiträge darstellen.

Für diesen Workshop wurde eine REST-API vorbereitet. Die URL für Blogbeiträge: `https://ngx-training.com/posts`.

Angular bietet uns einen `HttpClientModule`, der uns die Kommunikation mit der API vereinfacht.

Wir importieren den `HttpClientModule`in unserem `AppModule`.

Für die Kommunikation mit der API erstellen wir eien Service `<feature-name>.service.ts` unter `services`-Ordner:

```
ng g s services/blog/blog.service.ts
```

In dem `BlogService` holen wir uns den `HttpClient` per Dependency Injection in dem Konstruktor rein:
```
constructor(private http: HttpClient) { }
```

Danach erstellen wir eine `static` varibale `blogUrl`, die uns unsere URL für den Request zusammenbaut:
```
private static blogUrl = (apiUrl: string) => `${apiUrl}/posts`;

constructor(private http: HttpClient) { }
```

Jetzt sind wir soweit, dass wir den ersten API-Aufruf machen können. Wir erstellen uns die Methode `getList()` und laden uns die Liste der Blogbeiträge von der API:
```
getList(): Observable<Post[]> {
  const url = BlogService.blogUrl(environment.apiUrl);
  return this.http.get<Post[]>(url);
}
```

