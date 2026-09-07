---
title: Pagina di dettaglio del genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

La pagina di _dettaglio_ del genere deve visualizzare le informazioni per una particolare istanza di genere, utilizzando come identificatore il valore del campo `_id` generato automaticamente.
L'ID del record del genere richiesto è codificato alla fine dell'URL ed estratto automaticamente in base alla definizione della route (**/genre/:id**).
È quindi accessibile nel controller tramite i parametri della richiesta: `req.params.id`.

La pagina deve visualizzare il nome del genere e un elenco di tutti i libri del genere con collegamenti alla pagina dei dettagli di ciascun libro.

## Controller

Aprire **/controllers/genreController.js** e richiedere il modulo `Book` all'inizio del file (il file dovrebbe già eseguire `require()` del modulo `Genre`).

```js
const Book = require("../models/book");
```

Trovare il metodo del controller esportato `genre_detail()` e sostituirlo con il codice seguente.

```js
// Display detail page for a specific Genre.
exports.genre_detail = async (req, res, next) => {
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
};
```

Per prima cosa si usa `Genre.findById()` per ottenere le informazioni su `Genre` per un ID specifico e `Book.find()` per ottenere tutti i record dei libri che hanno lo stesso ID del genere associato.
Poiché le due richieste non dipendono l'una dall'altra, si usa `Promise.all()` per eseguire le query al database in parallelo (questo stesso approccio per eseguire query in parallelo è stato illustrato nella [pagina iniziale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page#controller)).

Si esegue `await` sulla promise restituita e, una volta risolta, si controllano i risultati.
Se il genere non esiste nel database (ad esempio, potrebbe essere stato eliminato), `findById()` verrà completato correttamente senza risultati.
In questo caso si desidera visualizzare una pagina "non trovata", quindi si crea un oggetto `Error` e lo si passa alla funzione middleware `next` nella catena.

> [!NOTE]
> Gli errori passati alla funzione middleware `next` vengono propagati fino al codice di gestione degli errori (configurato quando è stato [generato lo scheletro dell'app](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website#app.js). Per ulteriori informazioni, vedere [Gestione degli errori](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#handling_errors) e [Gestione di errori ed eccezioni nelle funzioni delle route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#handling_errors_and_exceptions_in_the_route_functions)).

Se viene trovato il `genre`, si chiama `render()` per visualizzare la view.
Il template della view è **genre_detail** (.pug).
I valori per il titolo, `genre` e `booksInGenre` vengono passati al template utilizzando le chiavi corrispondenti (`title`, `genre` e `genre_books`).

## View

Creare **/views/genre_detail.pug** e compilarlo con il testo seguente:

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

La view è molto simile a tutti gli altri template. La differenza principale è che non si usa il `title` passato per la prima intestazione (anche se viene usato nel template sottostante **layout.pug** per impostare il titolo della pagina).

## Che aspetto ha?

Eseguire l'applicazione e aprire il browser all'indirizzo `http://localhost:3000/`. Selezionare il collegamento _Tutti i generi_, quindi selezionare uno dei generi (ad esempio, "Fantasy"). Se tutto è configurato correttamente, la pagina dovrebbe avere un aspetto simile allo screenshot seguente.

![Pagina di dettaglio del genere - sito Express Local Library](locallibary_express_genre_detail.png)

> [!NOTE]
> Potrebbe verificarsi un errore simile a quello riportato di seguito se `req.params.id` (o qualsiasi altro ID) non può essere convertito in un [`mongoose.Types.ObjectId()`](https://mongoosejs.com/docs/api/mongoose.html#Mongoose.prototype.Types).
>
> ```bash
> Cast to ObjectId failed for value " 59347139895ea23f9430ecbb" at path "_id" for model "Genre"
> ```
>
> La causa più probabile è che l'ID passato ai metodi mongoose non sia effettivamente un ID.
> [`Mongoose.prototype.isValidObjectId()`](<https://mongoosejs.com/docs/api/mongoose.html#Mongoose.prototype.isValidObjectId()>) può essere utilizzato per verificare se un particolare ID è valido.

## Passaggi successivi

- Tornare a [Tutorial Express Parte 5: Visualizzazione dei dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Passare al prossimo sottoarticolo della parte 5: [Pagina di dettaglio del libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page).
