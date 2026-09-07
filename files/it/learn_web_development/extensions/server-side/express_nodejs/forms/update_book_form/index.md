---
title: Modulo di aggiornamento del libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form
l10n:
  sourceCommit: b5ee197a87ea18acbc4dd9544efa8c0e46253785
---

Questo sottoarticolo finale mostra come definire una pagina per aggiornare gli oggetti `Book`. La gestione del modulo durante l'aggiornamento di un libro è molto simile a quella per la creazione di un libro, tranne per il fatto che nella route `GET` è necessario popolare il modulo con i valori provenienti dal database.

## Controller: route get

Aprire **/controllers/bookController.js**. Individuare il metodo controller esportato `book_update_get()` e sostituirlo con il codice seguente.

```js
// Display book update form on GET.
exports.book_update_get = async (req, res, next) => {
  // Get book, authors and genres for form.
  const [book, allAuthors, allGenres] = await Promise.all([
    Book.findById(req.params.id).populate("author").exec(),
    Author.find().sort({ family_name: 1 }).exec(),
    Genre.find().sort({ name: 1 }).exec(),
  ]);

  if (book === null) {
    // No results.
    const err = new Error("Book not found");
    err.status = 404;
    return next(err);
  }

  // Mark our selected genres as checked.
  allGenres.forEach((genre) => {
    if (book.genre.includes(genre._id)) genre.checked = "true";
  });

  res.render("book_form", {
    title: "Update Book",
    authors: allAuthors,
    genres: allGenres,
    book,
  });
};
```

Il controller ottiene l'id del `Book` da aggiornare dal parametro URL (`req.params.id`).
Esegue `await` sulla promise restituita da `Promise.all()` per ottenere il record `Book` specificato (popolandone i campi genere e autore), nonché tutti i record `Author` e `Genre`.

Quando le operazioni sono completate, la funzione verifica se sono stati trovati libri e, se non ne viene trovato nessuno, invia un errore "Book not found" al middleware di gestione degli errori.

> [!NOTE]
> Non trovare risultati di libri **non è un errore** per una ricerca, ma lo è per questa applicazione perché è noto che deve esistere un record libro corrispondente. Il codice precedente verifica (`book===null`) nel callback, ma avrebbe potuto altrettanto bene concatenare il metodo [`orFail()`](<https://mongoosejs.com/docs/api/query.html#Query.prototype.orFail()>) alla query.

Successivamente, vengono contrassegnati come selezionati i generi attualmente scelti e viene quindi eseguito il rendering della vista **book_form.pug**, passando le variabili per `title`, il libro, tutti gli `authors` e tutti i `genres`.

## Controller: route post

Individuare il metodo controller esportato `book_update_post()` e sostituirlo con il codice seguente.

```js
// Handle book update on POST.
exports.book_update_post = [
  // Convert the genre to an array.
  (req, res, next) => {
    if (!Array.isArray(req.body.genre)) {
      req.body.genre =
        typeof req.body.genre === "undefined" ? [] : [req.body.genre];
    }
    next();
  },

  // Validate and sanitize fields.
  body("title", "Title must not be empty.")
    .trim()
    .isLength({ min: 1 })
    .escape(),
  body("author", "Author must not be empty.")
    .trim()
    .isLength({ min: 1 })
    .escape(),
  body("summary", "Summary must not be empty.")
    .trim()
    .isLength({ min: 1 })
    .escape(),
  body("isbn", "ISBN must not be empty").trim().isLength({ min: 1 }).escape(),
  body("genre.*").escape(),

  // Process request after validation and sanitization.
  async (req, res, next) => {
    // Extract the validation errors from a request.
    const errors = validationResult(req);

    // Create a Book object with escaped/trimmed data and old id.
    const book = new Book({
      title: req.body.title,
      author: req.body.author,
      summary: req.body.summary,
      isbn: req.body.isbn,
      genre: typeof req.body.genre === "undefined" ? [] : req.body.genre,
      _id: req.params.id, // This is required, or a new ID will be assigned!
    });

    if (!errors.isEmpty()) {
      // There are errors. Render form again with sanitized values/error messages.

      // Get all authors and genres for form
      const [allAuthors, allGenres] = await Promise.all([
        Author.find().sort({ family_name: 1 }).exec(),
        Genre.find().sort({ name: 1 }).exec(),
      ]);

      // Mark our selected genres as checked.
      for (const genre of allGenres) {
        if (book.genre.indexOf(genre._id) > -1) {
          genre.checked = "true";
        }
      }
      res.render("book_form", {
        title: "Update Book",
        authors: allAuthors,
        genres: allGenres,
        book,
        errors: errors.array(),
      });
      return;
    }

    // Data from form is valid. Update the record.
    const updatedBook = await Book.findByIdAndUpdate(req.params.id, book, {});
    // Redirect to book detail page.
    res.redirect(updatedBook.url);
  },
];
```

Questo è molto simile alla route post utilizzata durante la creazione di un `Book`.
Innanzitutto vengono convalidati e sanificati i dati del libro provenienti dal modulo e vengono usati per creare un nuovo oggetto `Book` (impostando il valore `_id` sull'id dell'oggetto da aggiornare). Se si verificano errori durante la convalida dei dati, il modulo viene nuovamente visualizzato, mostrando inoltre i dati immessi dall'utente, gli errori e gli elenchi di generi e autori. Se non sono presenti errori, viene chiamato `Book.findByIdAndUpdate()` per aggiornare il documento `Book`, quindi viene eseguito il reindirizzamento alla sua pagina dei dettagli.

## Vista

Non è necessario modificare la vista del modulo (**/views/book_form.pug**), poiché lo stesso template funziona sia per la creazione sia per l'aggiornamento del libro.

## Aggiungere un pulsante di aggiornamento

Aprire la vista **book_detail.pug** e assicurarsi che nella parte inferiore della pagina siano presenti collegamenti sia per eliminare sia per aggiornare i libri, come mostrato di seguito.

```pug
  hr
  p
    a(href=book.url+'/delete') Delete Book
  p
    a(href=book.url+'/update') Update Book
```

Ora dovrebbe essere possibile aggiornare i libri dalla pagina _Dettagli del libro_.

## Che aspetto ha?

Eseguire l'applicazione, aprire il browser all'indirizzo `http://localhost:3000/`, selezionare il collegamento _Tutti i libri_, quindi selezionare un libro specifico. Infine, selezionare il collegamento _Aggiorna libro_.

Il modulo dovrebbe essere identico alla pagina _Crea libro_, ma con il titolo 'Aggiorna libro' e precompilato con i valori del record.

![La sezione di aggiornamento del libro dell'applicazione Local library. La colonna sinistra contiene una barra di navigazione verticale. La colonna destra contiene un modulo per aggiornare il libro, con un'intestazione che riporta 'Aggiorna libro'. Sono presenti cinque campi di input denominati Titolo, Autore, Riepilogo, ISBN e Genere. Genere è un campo di opzioni con caselle di controllo. Alla fine è presente un pulsante denominato 'Invia'.](locallibary_express_book_update_noerrors.png)

> [!NOTE]
> Le altre pagine per l'aggiornamento degli oggetti possono essere implementate in modo molto simile. Questo viene lasciato come esercizio.

## Passaggi successivi

- Tornare a [Tutorial Express Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
