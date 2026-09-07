---
title: Template di base LocalLibrary
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template
l10n:
  sourceCommit: 4c58f4735f986a91bee1b77e336143630df727a2
---

Ora che è chiaro come estendere i template usando Pug, iniziamo creando un template di base per il progetto. Questo includerà una barra laterale con link alle pagine che si prevede di creare negli articoli del tutorial (ad esempio, per visualizzare e creare libri, generi, autori, ecc.) e un'area di contenuto principale che verrà sovrascritta in ciascuna pagina individuale.

Aprire **/views/layout.pug** e sostituirne il contenuto con il codice seguente.

```pug
doctype html
html(lang='en')
  head
    title= title
    meta(charset='utf-8')
    meta(name='viewport', content='width=device-width')
    link(rel="stylesheet", href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/css/bootstrap.min.css", integrity="sha384-LN+7fdVzj6u52u30Kp6M/trliBMCMKTyK833zpbD+pXdCLuTusPj697FH4R/5mcr", crossorigin="anonymous")
    script(src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.7/dist/js/bootstrap.bundle.min.js", integrity="sha384-ndDqU0Gzau9qJ1lfW4pNLlhNTkCfHzAVBReH9diLvGRem5+R9g2FzA8ZGN954O5Q", crossorigin="anonymous")
    link(rel='stylesheet', href='/stylesheets/style.css')
  body
    div(class='container-fluid')
      div(class='row')
        div(class='col-sm-2')
          block sidebar
            ul(class='sidebar-nav')
              li
                a(href='/catalog') Home
              li
                a(href='/catalog/books') All books
              li
                a(href='/catalog/authors') All authors
              li
                a(href='/catalog/genres') All genres
              li
                a(href='/catalog/bookinstances') All book-instances
              li
                hr
              li
                a(href='/catalog/author/create') Create new author
              li
                a(href='/catalog/genre/create') Create new genre
              li
                a(href='/catalog/book/create') Create new book
              li
                a(href='/catalog/bookinstance/create') Create new book instance (copy)

        div(class='col-sm-10')
          block content
```

Il template utilizza (e include) JavaScript e CSS di [Bootstrap](https://getbootstrap.com/) per migliorare il layout e la presentazione della pagina HTML. Usare Bootstrap o un altro web framework lato client è un modo rapido per creare una pagina dall'aspetto gradevole, che si adatti bene a diverse dimensioni del browser; consente inoltre di gestire la presentazione della pagina senza dover entrare nei dettagli: qui si vuole concentrarsi solo sul codice lato server.

> [!NOTE]
> Gli script vengono caricati cross-origin, quindi più avanti nel tutorial, quando verrà aggiunto il middleware di sicurezza, sarà necessario consentire esplicitamente il caricamento di questi file.
> Per ulteriori informazioni, vedere [Deployment > Use Helmet to protect against well known vulnerabilities](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment#use_helmet_to_protect_against_well_known_vulnerabilities).

Il layout dovrebbe essere abbastanza chiaro dopo aver letto il precedente [Introduzione ai template](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer). Notare l'uso di `block content` come segnaposto per il punto in cui verrà inserito il contenuto delle pagine individuali.

Il template di base fa riferimento anche a un file CSS locale (**style.css**) che fornisce un po' di stile aggiuntivo. Aprire **/public/stylesheets/style.css** e sostituirne il contenuto con il seguente codice CSS:

```css
.sidebar-nav {
  margin-top: 20px;
  padding: 0;
  list-style: none;
}
```

Ora è disponibile un template di base per creare pagine con una barra laterale. Nelle sezioni successive verrà utilizzato per definire le singole pagine.

## Passaggi successivi

- Tornare a [Tutorial su Express - Parte 5: Visualizzazione dei dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedere al successivo sottoarticolo della parte 5: [Pagina iniziale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page).
