---
title: Pagina di dettaglio di BookInstance e sfida
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

## Pagina di dettaglio di BookInstance

La pagina di dettaglio di `BookInstance` deve visualizzare le informazioni per ciascun `BookInstance`, identificato utilizzando il valore del campo `_id` (generato automaticamente). Questo includerà il nome del `Book` (come link alla _pagina di dettaglio del Book_) insieme ad altre informazioni nel record.

### Controller

Aprire **/controllers/bookinstanceController.js**.
Trovare il metodo del controller esportato `bookinstance_detail()` e sostituirlo con il seguente codice.

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

L'implementazione è molto simile a quella utilizzata per le altre pagine dei dettagli del modello. La funzione controller del percorso chiama `BookInstance.findById()` con l'ID di un'istanza del libro specifico estratta dall'URL (utilizzando il percorso), e accessibile all'interno del controller tramite i parametri della richiesta: `req.params.id`.
Successivamente, chiama `populate()` per ottenere i dettagli del `Book` associato.
Se non viene trovato un `BookInstance` corrispondente, viene inviato un errore al middleware di Express.
Altrimenti, i dati restituiti vengono resi utilizzando la vista **bookinstance_detail.pug**.

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

Tutto in questo template è stato dimostrato nelle sezioni precedenti.

### Come si presenta?

Eseguire l'applicazione e aprire il browser a `http://localhost:3000/`. Selezionare il link _All book-instances_, quindi selezionare uno degli elementi. Se tutto è configurato correttamente, il sito dovrebbe apparire come nell'immagine seguente.

![Pagina di dettaglio di BookInstance - sito Express Local Library](locallibary_express_bookinstance_detail.png)

## Sfida

Attualmente, la maggior parte delle _date_ visualizzate sul sito utilizza il formato predefinito di JavaScript (ad es., _Tue Oct 06 2020 15:49:58 GMT+1100 (AUS Eastern Daylight Time)_). La sfida per questo articolo è migliorare l'aspetto della visualizzazione delle date per le informazioni sulla durata di vita dell'`Author` (data di morte/nascita) e per le pagine di dettaglio di _BookInstance_ utilizzando il formato: 6 Ott, 2016.

> [!NOTE]
> Lei può utilizzare lo stesso approccio che abbiamo usato per l'elenco di _Book Instance_ (aggiungendo la proprietà virtuale per la durata di vita al modello `Author` e utilizzando [luxon](https://www.npmjs.com/package/luxon) per formattare le stringhe di data).

Per completare questa sfida, deve:

1. Sostituire la variabile `due_back` con `due_back_formatted` nella pagina di dettaglio di _BookInstance_.
2. Aggiornare il modello `Author` per aggiungere una proprietà virtuale di durata di vita. La durata di vita dovrebbe essere visualizzata come: _data_di_nascita - data_di_morte_, dove entrambi i valori hanno lo stesso formato di data di `BookInstance.due_back_formatted`.
3. Utilizzare `Author.lifespan` in tutte le viste dove attualmente usa esplicitamente `date_of_birth` e `date_of_death`.

## Prossimi passi

- Torni a [Tutorial Express Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data#displaying_library_data_tutorial_subarticles).
