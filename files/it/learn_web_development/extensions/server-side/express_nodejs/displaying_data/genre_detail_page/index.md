---
title: Pagina dei dettagli del genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

La pagina _dettaglio_ del genere deve visualizzare le informazioni per una particolare istanza di genere, utilizzando il suo valore di campo `_id` generato automaticamente come identificatore.
L'ID del record del genere richiesto è codificato alla fine dell'URL ed estratto automaticamente in base alla definizione del percorso (**/genre/:id**).
Viene quindi accesso all'interno del controller tramite i parametri della richiesta: `req.params.id`.

La pagina dovrebbe visualizzare il nome del genere e un elenco di tutti i libri del genere con collegamenti alla pagina dei dettagli di ciascun libro.

## Controller

Aprire **/controllers/genreController.js** e richiedere il modulo `Book` all'inizio del file (il file dovrebbe già `require()` i moduli `Genre` e "express-async-handler").

```js
const Book = require("../models/book");
```

Trovi il metodo `genre_detail()` esportato dal controller e sostituiscilo con il seguente codice.

```js
// Display detail page for a specific Genre.
exports.genre_detail = asyncHandler(async (req, res, next) => {
  // Get details of genre and all associated books (in parallel)
  const [genre, booksInGenre] = await Promise.all([
    Genre.findById(req.params.id).exec(),
    Book.find({ genre: req.params.id }, "title summary").exec(),
  ]);
  if (genre === null) {
    // No results.
    const err = new Error("Genre not found");
    err.status = 404;
    return next(err);
  }

  res.render("genre_detail", {
    title: "Genre Detail",
    genre: genre,
    genre_books: booksInGenre,
  });
});
```

Utilizziamo innanzitutto `Genre.findById()` per ottenere le informazioni del genere per un ID specifico, e `Book.find()` per ottenere tutti i record dei libri che hanno lo stesso ID di genere associato.
Poiché le due richieste non dipendono l'una dall'altra, utilizziamo `Promise.all()` per eseguire le query del database in parallelo (questo stesso approccio per eseguire query in parallelo è stato dimostrato nella [pagina principale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page#controller)).

Attendiamo che la promessa restituita si risolva e poi controlliamo i risultati.
Se il genere non esiste nel database (ad esempio, potrebbe essere stato eliminato), `findById()` restituirà con successo senza risultati.
In questo caso vogliamo visualizzare una pagina "non trovata", quindi creiamo un oggetto `Error` e lo passiamo alla funzione middleware `next` nella catena.

> [!NOTE]
> Gli errori passati alla funzione middleware `next` si propagano al nostro codice di gestione degli errori (questo è stato configurato quando abbiamo [generato lo scheletro dell'app](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website#app.js) - per maggiori informazioni vedere [Gestione degli errori](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#handling_errors)).

Se il `genre` viene trovato, allora chiamiamo `render()` per visualizzare la vista.
Il template della vista è **genre_detail** (.pug).
I valori per il titolo, `genre` e `booksInGenre` vengono passati nel template utilizzando le chiavi corrispondenti (`title`, `genre` e `genre_books`).

## Vista

Creare **/views/genre_detail.pug** e riempirlo con il testo seguente:

```pug
extends layout

block content

  h1 Genre: #{genre.name}

  div(style='margin-left:20px;margin-top:20px')

    h2(style='font-size: 1.5rem;') Books
    if genre_books.length
      dl
        each book in genre_books
          dt
            a(href=book.url) #{book.title}
          dd #{book.summary}
    else
      p This genre has no books.
```

La vista è molto simile a tutti gli altri nostri template. La differenza principale è che non utilizziamo il `title` passato per il primo titolo (anche se è utilizzato nel template sottostante **layout.pug** per impostare il titolo della pagina).

## Come appare?

Esegua l'applicazione e apra il suo browser su `http://localhost:3000/`. Selezioni il collegamento _Tutti i generi_, quindi selezioni uno dei generi (ad es., "Fantasy"). Se tutto è configurato correttamente, la sua pagina dovrebbe somigliare a quella dello screenshot seguente.

![Pagina dei dettagli del genere - Sito della Biblioteca locale Express](locallibary_express_genre_detail.png)

> [!NOTE]
> Potrebbe ottenere un errore simile a quello sotto riportato se `req.params.id` (o qualsiasi altro ID) non può essere convertito in un [`mongoose.Types.ObjectId()`](https://mongoosejs.com/docs/api/mongoose.html#Mongoose.prototype.Types).
>
> ```bash
> Cast to ObjectId failed for value " 59347139895ea23f9430ecbb" at path "_id" for model "Genre"
> ```
>
> La causa più probabile è che l'ID passato nei metodi di mongoose non sia effettivamente un ID.
> [`Mongoose.prototype.isValidObjectId()`](<https://mongoosejs.com/docs/api/mongoose.html#Mongoose.prototype.isValidObjectId()>) può essere utilizzato per verificare se un particolare ID è valido.

## Prossimi passaggi

- Torni a [Express Tutorial Parte 5: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proceda al prossimo sottoarticolo della parte 5: [Pagina dei dettagli del libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page).
