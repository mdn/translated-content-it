---
title: Pagina dei dettagli dell'autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

La pagina dei dettagli dell'autore deve visualizzare le informazioni sull'`Author` specificato, identificato utilizzando il valore del campo `_id` (generato automaticamente), insieme a un elenco di tutti gli oggetti `Book` associati a quell'`Author`.

## Controller

Apri **/controllers/authorController.js**.

Aggiungi le seguenti righe all'inizio del file per `require()` il modulo `Book` necessario per la pagina dei dettagli dell'autore (altri moduli come "express-async-handler" dovrebbero già essere presenti).

```js
const Book = require("../models/book");
```

Trova il metodo controller esportato `author_detail()` e sostituiscilo con il seguente codice.

```js
// Display detail page for a specific Author.
exports.author_detail = asyncHandler(async (req, res, next) => {
  // Get details of author and all their books (in parallel)
  const [author, allBooksByAuthor] = await Promise.all([
    Author.findById(req.params.id).exec(),
    Book.find({ author: req.params.id }, "title summary").exec(),
  ]);

  if (author === null) {
    // No results.
    const err = new Error("Author not found");
    err.status = 404;
    return next(err);
  }

  res.render("author_detail", {
    title: "Author Detail",
    author,
    author_books: allBooksByAuthor,
  });
});
```

L'approccio è esattamente lo stesso descritto per la [Pagina dei dettagli del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
La funzione del controller del percorso utilizza `Promise.all()` per eseguire query sull'`Author` specificato e sulle istanze di `Book` associate in parallelo.
Se non viene trovato alcun autore corrispondente, un oggetto Error viene inviato al middleware di gestione degli errori di Express.
Se l'autore viene trovato, le informazioni recuperate dal database vengono renderizzate utilizzando il template "author_detail".

## Vista

Crea **/views/author_detail.pug** e copia nel seguente testo.

```pug
extends layout

block content

  h1 Author: #{author.name}
  p #{author.date_of_birth} - #{author.date_of_death}

  div(style='margin-left:20px;margin-top:20px')

    h2(style='font-size: 1.5rem;') Books
    if author_books.length
      dl
        each book in author_books
          dt
            a(href=book.url) #{book.title}
          dd #{book.summary}
    else
      p This author has no books.
```

Tutto in questo template è stato dimostrato nelle sezioni precedenti.

## Che aspetto ha?

Esegui l'applicazione e apri il tuo browser a `http://localhost:3000/`. Seleziona il link _All Authors_, quindi seleziona uno degli autori. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire come nella schermata seguente.

![Pagina dei Dettagli dell'Autore - Sito Express Local Library](locallibary_express_author_detail.png)

> [!NOTE]
> L'aspetto delle date di _durata di vita_ dell'autore è brutto! Affronteremo questo problema nella sfida finale di questo articolo.

## Prossimi passi

- Ritorna a [Express Tutorial Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi all'ultimo sottoarticolo della parte 5: [Pagina dei dettagli di BookInstance e sfida](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge).
