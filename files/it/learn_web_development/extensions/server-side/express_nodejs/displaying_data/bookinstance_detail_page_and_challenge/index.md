---
title: Pagina di dettaglio BookInstance e sfida
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

## Pagina di dettaglio BookInstance

La pagina di dettaglio `BookInstance` deve visualizzare le informazioni per ciascun `BookInstance`, identificato utilizzando il valore del campo `_id` (generato automaticamente). Questo includerà il nome del `Book` (come link alla _pagina di dettaglio del Book_) insieme ad altre informazioni nel record.

### Controller

Apri **/controllers/bookinstanceController.js**.
Trova il metodo controller esportato `bookinstance_detail()` e sostituiscilo con il seguente codice.

```js
// Display detail page for a specific BookInstance.
exports.bookinstance_detail = asyncHandler(async (req, res, next) => {
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
});
```

L'implementazione è molto simile a quella utilizzata per le altre pagine di dettaglio del modello.
La funzione del controller della route chiama `BookInstance.findById()` con l'ID di una specifica istanza di libro estratta dall'URL (usando la route) e accessibile all'interno del controller tramite i parametri della richiesta: `req.params.id`.
Poi chiama `populate()` per ottenere i dettagli del `Book` associato.
Se un `BookInstance` corrispondente non viene trovato, viene inviato un errore al middleware di Express.
Altrimenti, i dati restituiti vengono renderizzati usando la vista **bookinstance_detail.pug**.

### Vista

Crea **/views/bookinstance_detail.pug** e copia il contenuto qui sotto.

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

Tutto in questo template è stato dimostrato nelle sezioni precedenti.

### Che aspetto ha?

Esegui l'applicazione e apri il browser su `http://localhost:3000/`. Seleziona il link _All book-instances_, quindi seleziona uno degli elementi. Se tutto è configurato correttamente, il tuo sito dovrebbe assomigliare a quanto mostrato nello screenshot seguente.

![Pagina di dettaglio BookInstance - sito Local Library Express](locallibary_express_bookinstance_detail.png)

## Sfida

Attualmente, la maggior parte delle _date_ visualizzate sul sito usa il formato predefinito di JavaScript (ad esempio, _Tue Oct 06 2020 15:49:58 GMT+1100 (AUS Eastern Daylight Time)_). La sfida per questo articolo è migliorare l'aspetto della visualizzazione delle date per le informazioni sulla vita dell'`Author` (data di morte/nascita) e per le pagine di dettaglio del _BookInstance_ utilizzando il formato: Oct 6th, 2016.

> [!NOTE]
> Puoi usare lo stesso approccio che abbiamo utilizzato per la _Lista delle istanze dei libri_ (aggiungendo la proprietà virtuale per la durata della vita al modello `Author` e usando [luxon](https://www.npmjs.com/package/luxon) per formattare le stringhe di data).

Per completare questa sfida, è necessario:

1. Sostituire la variabile `due_back` con `due_back_formatted` nella pagina di dettaglio del _BookInstance_.
2. Aggiornare il modello `Author` per aggiungere una proprietà virtuale per la durata della vita. La durata della vita dovrebbe apparire come: _data_di_nascita - data_di_morte_, dove entrambi i valori hanno lo stesso formato di data di `BookInstance.due_back_formatted`.
3. Utilizzare `Author.lifespan` in tutte le viste in cui si usano attualmente esplicitamente `date_of_birth` e `date_of_death`.

## Prossimi passi

- Ritorna a [Express Tutorial Part 5: Visualizzare i dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data#displaying_library_data_tutorial_subarticles).
