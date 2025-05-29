---
title: Pagina dei dettagli del genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

La pagina _dettaglio_ del genere deve visualizzare le informazioni per una particolare istanza di genere, utilizzando il suo campo `_id` generato automaticamente come identificatore.
L'ID del record di genere richiesto è codificato alla fine dell'URL ed estratto automaticamente in base alla definizione del percorso (**/genre/:id**).
Viene quindi accesso all'interno del controller tramite i parametri della richiesta: `req.params.id`.

La pagina dovrebbe visualizzare il nome del genere e un elenco di tutti i libri del genere con link a ciascuna pagina di dettagli del libro.

## Controller

Apri **/controllers/genreController.js** e richiedi il modulo `Book` all'inizio del file (il file dovrebbe già `require()` il modulo `Genre` e "express-async-handler").

```js
const Book = require("../models/book");
```

Trova il metodo controller `genre_detail()` esportato e sostituiscilo con il seguente codice.

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
    genre,
    genre_books: booksInGenre,
  });
});
```

Per prima cosa usiamo `Genre.findById()` per ottenere le informazioni sul genere per uno specifico ID e `Book.find()` per ottenere tutti i record di libri che hanno lo stesso ID di genere associato.
Poiché le due richieste non dipendono l'una dall'altra, usiamo `Promise.all()` per eseguire le query del database in parallelo (questo stesso approccio per eseguire le query in parallelo è stato dimostrato nella [pagina principale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page#controller)).

`Await` sul promise restituito, e una volta stabilizzato controlliamo i risultati.
Se il genere non esiste nel database (ad esempio, potrebbe essere stato eliminato), allora `findById()` restituirà con successo senza risultati.
In questo caso vogliamo visualizzare una pagina "non trovata", quindi creiamo un oggetto `Error` e lo passiamo alla funzione middleware `next` nella catena.

> [!NOTE]
> Gli errori passati alla funzione middleware `next` si propagano attraverso il nostro codice di gestione degli errori (questo è stato impostato quando abbiamo [generato lo scheletro dell'app](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website#app.js) - per ulteriori informazioni, vedi [Gestione degli errori](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#handling_errors)).

Se il `genre` viene trovato, allora chiamiamo `render()` per visualizzare la vista.
Il modello di vista è **genre_detail** (.pug).
I valori per il titolo, `genre`, e `booksInGenre` sono passati nel modello utilizzando le chiavi corrispondenti (`title`, `genre` e `genre_books`).

## Vista

Crea **/views/genre_detail.pug** e riempilo con il testo seguente:

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

La vista è molto simile a tutti gli altri modelli. La principale differenza è che non utilizziamo il `title` passato nel primo titolo (anche se viene utilizzato nel modello sottostante **layout.pug** per impostare il titolo della pagina).

## Che aspetto ha?

Esegui l'applicazione e apri il browser su `http://localhost:3000/`. Seleziona il link _Tutti i generi_, quindi seleziona uno dei generi (ad esempio, "Fantasy"). Se tutto è impostato correttamente, la tua pagina dovrebbe apparire simile a quella dello screenshot seguente.

![Pagina dei dettagli del genere - sito Express Local Library](locallibary_express_genre_detail.png)

> [!NOTE]
> Potresti ricevere un errore simile a quello riportato di seguito se `req.params.id` (o qualsiasi altro ID) non può essere convertito in un [`mongoose.Types.ObjectId()`](https://mongoosejs.com/docs/api/mongoose.html#Mongoose.prototype.Types).
>
> ```bash
> Cast to ObjectId failed for value " 59347139895ea23f9430ecbb" at path "_id" for model "Genre"
> ```
>
> La causa più probabile è che l'ID passato nei metodi di mongoose non sia effettivamente un ID.
> [`Mongoose.prototype.isValidObjectId()`](<https://mongoosejs.com/docs/api/mongoose.html#Mongoose.prototype.isValidObjectId()>) può essere usato per verificare se un particolare ID è valido.

## Passi successivi

- Torna a [Express Tutorial Parte 5: Visualizzazione dei dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Prosegui al prossimo sottoarticolo della parte 5: [Pagina dei dettagli del libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page).
