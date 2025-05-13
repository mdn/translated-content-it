---
title: Pagina dettagli libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La _Pagina dettagli libro_ deve mostrare le informazioni per un particolare `Book` (identificato utilizzando il valore del campo `_id` generato automaticamente), insieme alle informazioni su ciascuna copia associata nella biblioteca (`BookInstance`). Ogni volta che mostriamo un autore, un genere o un'istanza di libro, questi dovrebbero essere collegati alla pagina di dettaglio associata per quell'elemento.

## Controller

Aprire **/controllers/bookController.js**. Trovare il metodo esportato `book_detail()` nel controller e sostituirlo con il seguente codice.

```js
// Display detail page for a specific book.
exports.book_detail = asyncHandler(async (req, res, next) => {
  // Get details of books, book instances for specific book
  const [book, bookInstances] = await Promise.all([
    Book.findById(req.params.id).populate("author").populate("genre").exec(),
    BookInstance.find({ book: req.params.id }).exec(),
  ]);

  if (book === null) {
    // No results.
    const err = new Error("Book not found");
    err.status = 404;
    return next(err);
  }

  res.render("book_detail", {
    title: book.title,
    book: book,
    book_instances: bookInstances,
  });
});
```

> [!NOTE]
> Non è necessario richiedere moduli aggiuntivi in questo passaggio, poiché abbiamo già importato le dipendenze quando abbiamo implementato il controller della home page.

L'approccio è esattamente lo stesso descritto per la [pagina dettagli genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
La funzione del controller della rotta utilizza `Promise.all()` per interrogare il `Book` specificato e le sue copie associate (`BookInstance`) in parallelo.
Se non viene trovato alcun libro corrispondente, viene restituito un oggetto Error con errore "404: Not Found".
Se il libro viene trovato, le informazioni recuperate dal database vengono renderizzate utilizzando il template "book_detail".
Poiché la chiave 'title' viene utilizzata per dare il nome alla pagina web (come definito nell'intestazione in 'layout.pug'), questa volta stiamo passando `results.book.title` durante la renderizzazione della pagina web.

## Vista

Creare **/views/book_detail.pug** e aggiungere il testo sottostante.

```pug
extends layout

block content
  h1 Title: !{book.title}

  p #[strong Author: ]
    a(href=book.author.url) #{book.author.name}
  p #[strong Summary:] !{book.summary}
  p #[strong ISBN:] #{book.isbn}
  p #[strong Genre: ]
    each val, index in book.genre
      a(href=val.url) #{val.name}
      if index < book.genre.length - 1
        |,&nbsp;

  div(style='margin-left:20px;margin-top:20px')
    h2(style='font-size: 1.5rem;') Copies

    each val in book_instances
      hr
      if val.status=='Available'
        p.text-success #{val.status}
      else if val.status=='Maintenance'
        p.text-danger #{val.status}
      else
        p.text-warning #{val.status}
      p #[strong Imprint:] #{val.imprint}
      if val.status!='Available'
        p #[strong Due back:] #{val.due_back}
      p #[strong Id: ]
        a(href=val.url) #{val._id}

    else
      p There are no copies of this book in the library.
```

Si noti il `!` che precede `!{book.title}` e `!{book.summary}`, che garantisce che i valori non siano sfuggiti per la visualizzazione.
Questo viene fatto perché abbiamo già sanitizzato i dati che stiamo visualizzando programmaticamente, e una sanitizzazione ulteriore mostrerebbe il nostro "markup sanitizzato" invece della versione sicura del testo originale.
Abbiamo scelto di non fare lo stesso per Author, Genre e così via (anche se potremmo), perché non ci aspettiamo che includano caratteri "pericolosi" che richiedano sanitizzazione.

Quasi tutto il resto in questo template è stato dimostrato nelle sezioni precedenti.

> [!NOTE]
> L'elenco dei generi associati al libro è implementato nel template come segue. Questo aggiunge una virgola e uno spazio non interrompibile dopo ogni genere associato al libro tranne l'ultimo.
>
> ```pug
>   p #[strong Genre: ]
>     each val, index in book.genre
>       a(href=val.url) #{val.name}
>       if index < book.genre.length - 1
>         |,&nbsp;
> ```

## Come appare?

Eseguire l'applicazione e aprire il browser su `http://localhost:3000/`. Selezionare il link _Tutti i libri_, quindi selezionare uno dei libri. Se tutto è configurato correttamente, la pagina dovrebbe apparire come nella seguente schermata.

![Pagina Dettagli Libro - Sito Express Biblioteca Locale](locallibary_express_book_detail.png)

## Prossimi passi

- Ritorna a [Express Tutorial Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedere al sottoarticolo successivo della parte 5: [Pagina dettagli autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page).
