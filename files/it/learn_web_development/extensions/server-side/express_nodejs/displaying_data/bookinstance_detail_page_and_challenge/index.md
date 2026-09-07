---
title: Pagina di dettaglio di BookInstance e sfida
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

## Pagina di dettaglio di BookInstance

La pagina di dettaglio di `BookInstance` deve visualizzare le informazioni per ciascun `BookInstance`, identificato tramite il valore del campo `_id` (generato automaticamente). Ciò includerà il nome del `Book` (come collegamento alla _pagina di dettaglio del Book_) insieme ad altre informazioni nel record.

### Controller

Aprire **/controllers/bookinstanceController.js**.
Trovare il metodo controller esportato `bookinstance_detail()` e sostituirlo con il codice seguente.

```js
// Display detail page for a specific BookInstance.
exports.bookinstance_detail = async (req, res, next) => {
  const bookInstance = await BookInstance.findById(req.params.id)
    .populate("book")
    .exec();

  if (bookInstance === null) {
    // No results.
    const err = new Error("Book copy not found");
    err.status = 404;
    return next(err);
  }

  res.render("bookinstance_detail", {
    title: "Book:",
    bookinstance: bookInstance,
  });
};
```

L'implementazione è molto simile a quella usata per le pagine di dettaglio degli altri modelli.
La funzione controller della route chiama `BookInstance.findById()` con l'ID di una specifica istanza di libro estratto dall'URL (usando la route) e accessibile nel controller tramite i parametri della richiesta: `req.params.id`.
Quindi chiama `populate()` per ottenere i dettagli del `Book` associato.
Se non viene trovato un `BookInstance` corrispondente, viene inviato un errore al middleware di Express.
Altrimenti, i dati restituiti vengono visualizzati usando la vista **bookinstance_detail.pug**.

### Vista

Creare **/views/bookinstance_detail.pug** e copiare il contenuto seguente.

```pug
extends layout

block content

  h1 ID: #{bookinstance._id}

  p #[strong Title: ]
    a(href=bookinstance.book.url) #{bookinstance.book.title}
  p #[strong Imprint:] #{bookinstance.imprint}

  p #[strong Status: ]
    if bookinstance.status=='Available'
      span.text-success #{bookinstance.status}
    else if bookinstance.status=='Maintenance'
      span.text-danger #{bookinstance.status}
    else
      span.text-warning #{bookinstance.status}

  if bookinstance.status!='Available'
    p #[strong Due back:] #{bookinstance.due_back}
```

Tutto ciò che è presente in questo template è stato illustrato nelle sezioni precedenti.

### Come appare?

Eseguire l'applicazione e aprire il browser all'indirizzo `http://localhost:3000/`. Selezionare il collegamento _All book-instances_, quindi selezionare uno degli elementi. Se tutto è configurato correttamente, il sito dovrebbe apparire simile allo screenshot seguente.

![Pagina di dettaglio di BookInstance - sito Express Local Library](locallibary_express_bookinstance_detail.png)

## Sfida

Attualmente, la maggior parte delle _date_ visualizzate sul sito usa il formato JavaScript predefinito (ad esempio, _Tue Oct 06 2020 15:49:58 GMT+1100 (AUS Eastern Daylight Time)_). La sfida di questo articolo consiste nel migliorare l'aspetto della visualizzazione delle date per le informazioni sulla durata della vita di `Author` (data di morte/nascita) e per le pagine di _dettaglio di BookInstance_, usando il formato: Oct 6th, 2016.

> [!NOTE]
> È possibile usare lo stesso approccio utilizzato per l'_elenco delle istanze di libro_ (aggiungendo la proprietà virtuale per la durata della vita al modello `Author` e usando [luxon](https://www.npmjs.com/package/luxon) per formattare le stringhe di data).

Per completare questa sfida, è necessario:

1. Sostituire la variabile `due_back` con `due_back_formatted` nella pagina di _dettaglio di BookInstance_.
2. Aggiornare il modello `Author` per aggiungere una proprietà virtuale per la durata della vita. La durata della vita dovrebbe avere questo aspetto: _date_of_birth - date_of_death_, dove entrambi i valori hanno lo stesso formato di data di `BookInstance.due_back_formatted`.
3. Usare `Author.lifespan` in tutte le viste in cui attualmente vengono usati esplicitamente `date_of_birth` e `date_of_death`.

## Passaggi successivi

- Tornare a [Tutorial Express Parte 5: Visualizzazione dei dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data#displaying_library_data_tutorial_subarticles).
