---
title: Modello base di LocalLibrary
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Ora che abbiamo capito come estendere i modelli utilizzando Pug, iniziamo creando un modello base per il progetto. Questo avrà una barra laterale con collegamenti alle pagine che speriamo di creare negli articoli tutoriali (ad esempio, per visualizzare e creare libri, generi, autori, ecc.) e un'area principale per i contenuti che sovrascriveremo in ciascuna delle nostre pagine individuali.

Apri **/views/layout.pug** e sostituisci il contenuto con il codice seguente.

```pug
doctype html
html(lang='en')
  head
    title= title
    meta(charset='utf-8')
    meta(name='viewport', content='width=device-width, initial-scale=1')
    link(rel="stylesheet", href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css", integrity="sha384-xOolHFLEh07PJGoPkLv1IbcEPTNtaed2xpHsD9ESMhqIYd0nLMwNLD69Npy4HI+N", crossorigin="anonymous")
    script(src="https://code.jquery.com/jquery-3.5.1.slim.min.js", integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj", crossorigin="anonymous")
    script(src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.min.js", integrity="sha384-+sLIOodYLS7CIrQpBjl+C7nPvqq+FbNUBDunl/OZv93DB7Ln/533i8e/mZXLi/P+", crossorigin="anonymous")
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

Il modello utilizza (e include) JavaScript e CSS da [Bootstrap](https://getbootstrap.com/) per migliorare il layout e la presentazione della pagina HTML. Utilizzare Bootstrap o un altro framework web lato client è un modo rapido per creare una pagina attraente che possa scalare bene su diverse dimensioni del browser e ci permette anche di gestire la presentazione della pagina senza dover entrare nei dettagli—qui vogliamo solo concentrarci sul codice lato server!

> [!NOTE]
> Gli script vengono caricati cross-origin, quindi più avanti nel tutorial, quando aggiungeremo il middleware di sicurezza, dovremo esplicitamente consentire il caricamento di questi file.
> Per maggiori informazioni, vedi [Distribuzione > Utilizzare Helmet per proteggersi da vulnerabilità note](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/deployment#use_helmet_to_protect_against_well_known_vulnerabilities).

Il layout dovrebbe essere abbastanza ovvio se hai letto la nostra [Guida sui modelli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer) sopra. Nota l'uso di `block content` come segnaposto per dove verrà posizionato il contenuto delle nostre pagine individuali.

Il modello base fa anche riferimento a un file CSS locale (**style.css**) che fornisce un po' di stile aggiuntivo. Apri **/public/stylesheets/style.css** e sostituisci il suo contenuto con il seguente codice CSS:

```css
.sidebar-nav {
  margin-top: 20px;
  padding: 0;
  list-style: none;
}
```

Ora abbiamo un modello base per creare pagine con una barra laterale. Nelle prossime sezioni lo utilizzeremo per definire le pagine individuali.

## Prossimi passi

- Torna a [Express Tutorial Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi al prossimo sottoarticolo della parte 5: [Pagina iniziale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page).
