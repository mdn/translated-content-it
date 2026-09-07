---
title: Pagina dei dettagli dell'autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

La pagina dei dettagli dell'autore deve visualizzare le informazioni sull'`Author` specificato, identificato tramite il valore del relativo campo `_id` (generato automaticamente), insieme a un elenco di tutti gli oggetti `Book` associati a tale `Author`.

## Controller

Aprire **/controllers/authorController.js**.

Aggiungere le seguenti righe all'inizio del file per eseguire `require()` del modulo `Book` necessario alla pagina dei dettagli dell'autore.

```js
const Book = require("../models/book");
```

Trovare il metodo controller esportato `author_detail()` e sostituirlo con il codice seguente.

```js
// Display detail page for a specific Author.
exports.author_detail = async (req, res, next) => {
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
};
```

L'approccio è esattamente lo stesso descritto per la [pagina dei dettagli del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
La funzione del controller della route utilizza `Promise.all()` per interrogare in parallelo l'`Author` specificato e le relative istanze di `Book` associate.
Se non viene trovato alcun autore corrispondente, viene inviato un oggetto `Error` al middleware di gestione degli errori di Express.
Se l'autore viene trovato, le informazioni recuperate dal database vengono visualizzate utilizzando il template "author_detail".

## Vista

Creare **/views/author_detail.pug** e copiarvi il testo seguente.

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

Tutto ciò che è presente in questo template è stato illustrato nelle sezioni precedenti.

## Che aspetto ha?

Eseguire l'applicazione e aprire il browser su `http://localhost:3000/`. Selezionare il collegamento _Tutti gli autori_, quindi selezionare uno degli autori. Se tutto è configurato correttamente, il sito dovrebbe avere un aspetto simile allo screenshot seguente.

![Pagina dei dettagli dell'autore - sito Express Local Library](locallibary_express_author_detail.png)

> [!NOTE]
> L'aspetto delle date della _durata della vita_ dell'autore è poco gradevole. Questo problema verrà affrontato nella sfida finale di questo articolo.

## Passaggi successivi

- Tornare a [Tutorial di Express - Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proseguire con il sottoarticolo finale della parte 5: [Pagina dei dettagli di BookInstance e sfida](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge).
