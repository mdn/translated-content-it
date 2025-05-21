---
title: Pagina dei dettagli del libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La _Pagina dei dettagli del libro_ deve visualizzare le informazioni per un determinato `Book` (identificato utilizzando il valore del campo `_id` generato automaticamente), insieme alle informazioni su ciascuna copia associata nella biblioteca (`BookInstance`). Ogni volta che visualizziamo un autore, un genere o un'istanza di libro, questi dovrebbero essere collegati alla pagina dei dettagli associata all'elemento.

## Controller

Apri **/controllers/bookController.js**. Trova il metodo controller esportato `book_detail()` e sostituiscilo con il seguente codice.

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

L'approccio è esattamente lo stesso descritto per la [Pagina dei dettagli del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
La funzione del controller della rotta utilizza `Promise.all()` per interrogare in parallelo lo specifico `Book` e le sue copie associate (`BookInstance`).
Se non viene trovato nessun libro corrispondente, viene restituito un oggetto Error con un errore "404: Not Found".
Se il libro viene trovato, quindi le informazioni recuperate dal database vengono rese utilizzando il template "book_detail".
Poiché la chiave 'title' viene utilizzata per dare nome alla pagina web (come definito nell'intestazione in 'layout.pug'), questa volta stiamo passando `results.book.title` durante il rendering della pagina web.

## Vista

Crea **/views/book_detail.pug** e aggiungi il testo seguente.

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

Nota il `!` che precede `!{book.title}` e `!{book.summary}`, che garantisce che i valori non siano escape durante la visualizzazione.
Questo viene fatto perché abbiamo già sanitizzato i dati che stiamo visualizzando in modo programmatico, e sanitizzare di nuovo visualizzerebbe il nostro "markup sanitizzato" anziché la versione sicura del testo originale.
Abbiamo scelto di non fare lo stesso per Autore, Genere e così via (anche se potremmo), perché non ci aspettiamo che includano caratteri "pericolosi" che richiedano sanitizzazione.

Quasi tutto il resto in questo template è stato dimostrato nelle sezioni precedenti.

> [!NOTE]
> La lista dei generi associati al libro è implementata nel template come segue. Questo aggiunge una virgola e uno spazio non interrompente dopo ogni genere associato al libro tranne l'ultimo.
>
> ```pug
>   p #[strong Genre: ]
>     each val, index in book.genre
>       a(href=val.url) #{val.name}
>       if index < book.genre.length - 1
>         |,&nbsp;
> ```

## Che aspetto ha?

Esegui l'applicazione e apri il browser su `http://localhost:3000/`. Seleziona il link _All books_, poi seleziona uno dei libri. Se tutto è configurato correttamente, la tua pagina dovrebbe apparire simile allo screenshot seguente.

![Pagina Dettagli Libro - Sito Local Library Express](locallibary_express_book_detail.png)

## Prossimi passi

- Ritorna a [Tutorial Express Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Prosegui con il prossimo sottoarticolo della parte 5: [Pagina dei dettagli dell'autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page).
