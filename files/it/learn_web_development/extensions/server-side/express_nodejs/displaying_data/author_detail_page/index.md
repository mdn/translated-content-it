---
title: Pagina dei dettagli dell'autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La pagina dei dettagli dell'autore deve visualizzare le informazioni relative all'`Autore` specificato, identificato utilizzando il valore del campo `_id` (generato automaticamente), insieme a un elenco di tutti gli oggetti `Book` associati a tale `Autore`.

## Controller

Aprire **/controllers/authorController.js**.

Aggiungere le seguenti righe all'inizio del file per `require()` il modulo `Book` necessario per la pagina dei dettagli dell'autore (altri moduli come "express-async-handler" dovrebbero già essere presenti).

```js
const Book = require("../models/book");
```

Trovare il metodo controller esportato `author_detail()` e sostituirlo con il seguente codice.

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
    author: author,
    author_books: allBooksByAuthor,
  });
});
```

L'approccio è esattamente lo stesso descritto per la [pagina dei dettagli del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
La funzione del controller della rotta utilizza `Promise.all()` per interrogare in parallelo l'`Autore` specificato e le loro istanze `Book` associate.
Se non viene trovato alcun autore corrispondente, un oggetto errore viene inviato al middleware di gestione degli errori di Express.
Se l'autore viene trovato, le informazioni sul database recuperate vengono renderizzate utilizzando il template "author_detail".

## Vista

Creare **/views/author_detail.pug** e copiare il seguente testo.

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

## Come si presenta?

Eseguire l'applicazione e aprire il browser su `http://localhost:3000/`. Selezionare il link _Tutti gli autori_, quindi selezionare uno degli autori. Se tutto è configurato correttamente, il sito dovrebbe apparire come nello screenshot seguente.

![Pagina dei dettagli dell'autore - Sito della Biblioteca Locale con Express](locallibary_express_author_detail.png)

> [!NOTE]
> L'aspetto delle date del _lifespan_ dell'autore è brutto! Affronteremo questo aspetto nella sfida finale in questo articolo.

## Prossimi passi

- Tornare a [Tutorial Express Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedere all'ultima sotto-articolo della parte 5: [Pagina dei dettagli del BookInstance e sfida](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge).
