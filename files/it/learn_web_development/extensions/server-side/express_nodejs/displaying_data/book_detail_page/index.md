---
title: Pagina di dettaglio del libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

La _pagina di dettaglio del libro_ deve visualizzare le informazioni relative a uno specifico `Book` (identificato tramite il valore del campo `_id` generato automaticamente), insieme alle informazioni su ogni copia associata nella biblioteca (`BookInstance`). Ovunque venga visualizzato un autore, un genere o un'istanza del libro, questi devono essere collegati alla pagina di dettaglio associata per quell'elemento.

## Controller

Aprire **/controllers/bookController.js**. Trovare il metodo del controller esportato `book_detail()` e sostituirlo con il codice seguente.

```js
// Display detail page for a specific book.
exports.book_detail = async (req, res, next) => {
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
    book,
    book_instances: bookInstances,
  });
};
```

> [!NOTE]
> In questo passaggio non è necessario richiedere moduli aggiuntivi, poiché le dipendenze sono già state importate durante l'implementazione del controller della pagina iniziale.

L'approccio è esattamente lo stesso descritto per la [pagina di dettaglio del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page).
La funzione del controller della route utilizza `Promise.all()` per interrogare in parallelo il `Book` specificato e le relative copie associate (`BookInstance`).
Se non viene trovato alcun libro corrispondente, viene restituito un oggetto `Error` con un errore "404: Not Found".
Se il libro viene trovato, le informazioni recuperate dal database vengono quindi renderizzate utilizzando il template "book_detail".
Poiché la chiave 'title' viene utilizzata per assegnare il nome alla pagina web (come definito nell'header in 'layout.pug'), questa volta viene passato `results.book.title` durante il rendering della pagina web.

## Vista

Creare **/views/book_detail.pug** e aggiungere il testo seguente.

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

Notare il `!` iniziale in `!{book.title}` e `!{book.summary}`, che garantisce che i valori non vengano sottoposti a escape per la visualizzazione.
Questo avviene perché i dati visualizzati sono già stati sanitizzati programmaticamente, e una seconda sanitizzazione visualizzerebbe il nostro "markup sanitizzato" anziché la versione sicura del testo originale.
Si è scelto di non fare lo stesso per Autore, Genere e così via (anche se sarebbe possibile), perché non si prevede che includano caratteri "pericolosi" che richiedano sanitizzazione.

Quasi tutto il resto di questo template è stato illustrato nelle sezioni precedenti.

> [!NOTE]
> L'elenco dei generi associati al libro è implementato nel template come segue. Questo aggiunge una virgola e uno spazio non separabile dopo ogni genere associato al libro, tranne l'ultimo.
>
> ```pug
>   p #[strong Genre: ]
>     each val, index in book.genre
>       a(href=val.url) #{val.name}
>       if index < book.genre.length - 1
>         |,&nbsp;
> ```

## Che aspetto ha?

Eseguire l'applicazione e aprire il browser all'indirizzo `http://localhost:3000/`. Selezionare il collegamento _Tutti i libri_, quindi selezionare uno dei libri. Se tutto è configurato correttamente, la pagina dovrebbe avere un aspetto simile allo screenshot seguente.

![Pagina di dettaglio del libro - sito Express Local Library](locallibary_express_book_detail.png)

## Passaggi successivi

- Tornare a [Tutorial Express Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Passare al prossimo sottoarticolo della parte 5: [Pagina di dettaglio dell'autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page).
