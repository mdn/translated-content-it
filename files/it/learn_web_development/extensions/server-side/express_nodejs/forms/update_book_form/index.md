---
title: Modulo di aggiornamento del libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

Questo ultimo sottoarticolo mostra come definire una pagina per aggiornare gli oggetti `Book`. La gestione del modulo quando si aggiorna un libro è molto simile a quella per la creazione di un libro, tranne per il fatto che si deve popolare il modulo nel percorso `GET` con i valori provenienti dal database.

## Controller—get route

Apri **/controllers/bookController.js**. Trova il metodo del controller esportato `book_update_get()` e sostituiscilo con il seguente codice.

```js
// Display book update form on GET.
exports.book_update_get = asyncHandler(async (req, res, next) => {
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
});
```

Il controller ottiene l'id del `Book` da aggiornare dal parametro URL (`req.params.id`).
Attende la promessa restituita da `Promise.all()` per ottenere il record `Book` specificato (popolando i suoi campi genre e author) e tutti i record `Author` e `Genre`.

Quando le operazioni sono completate, la funzione verifica se sono stati trovati dei libri e, se non ne sono stati trovati, invia un errore "Book not found" al middleware di gestione degli errori.

> [!NOTE]
> Non trovare alcun libro non è un errore per una ricerca — ma lo è per questa applicazione perché sappiamo che deve esistere un record di libro corrispondente! Il codice sopra confronta per (`book===null`) nel callback, ma avrebbe potuto ugualmente concatenare il metodo [orFail()](<https://mongoosejs.com/docs/api/query.html#Query.prototype.orFail()>) alla query.

Quindi marchiamo i generi attualmente selezionati come selezionati e poi renderizziamo la vista **book_form.pug**, passando variabili per `title`, book, tutti gli `authors` e tutti i `genres`.

## Controller—post route

Trova il metodo del controller esportato `book_update_post()`, e sostituiscilo con il seguente codice.

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
  asyncHandler(async (req, res, next) => {
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
  }),
];
```

Questo è molto simile al percorso post usato quando si crea un `Book`.
Prima validiamo e sanifichiamo i dati del libro dal modulo e li usiamo per creare un nuovo oggetto `Book` (impostando il suo valore `_id` sull'id dell'oggetto da aggiornare). Se ci sono errori quando validiamo i dati, allora renderizziamo di nuovo il modulo, visualizzando inoltre i dati inseriti dall'utente, gli errori e le liste di generi e autori. Se non ci sono errori, allora chiamiamo `Book.findByIdAndUpdate()` per aggiornare il documento `Book`, e poi reindirizziamo alla sua pagina dei dettagli.

## Vista

Non c'è bisogno di cambiare la vista per il modulo (**/views/book_form.pug**) poiché lo stesso template funziona sia per creare che per aggiornare il libro.

## Aggiungi un pulsante di aggiornamento

Apri la vista **book_detail.pug** e assicurati che ci siano link sia per eliminare che per aggiornare i libri nella parte inferiore della pagina, come mostrato di seguito.

```pug
  hr
  p
    a(href=book.url+'/delete') Delete Book
  p
    a(href=book.url+'/update') Update Book
```

Dovresti ora essere in grado di aggiornare i libri dalla pagina dei dettagli del _Libro_.

## Che aspetto ha?

Esegui l'applicazione, apri il browser su `http://localhost:3000/`, seleziona il link _Tutti i libri_, quindi seleziona un libro particolare. Infine, seleziona il link _Aggiorna libro_.

Il modulo dovrebbe apparire esattamente come la pagina _Crea libro_, solo con un titolo di 'Aggiorna libro', e pre-popolato con i valori del record.

![La sezione di aggiornamento del libro dell'applicazione Local library. La colonna di sinistra ha una barra di navigazione verticale. La colonna di destra ha un modulo per aggiornare il libro con un'intestazione che legge 'Aggiorna libro'. Ci sono cinque campi di input etichettati Titolo, Autore, Sintesi, ISBN, Genere. Genere è un campo opzione checkbox. C'è un pulsante etichettato 'Invia' alla fine.](locallibary_express_book_update_noerrors.png)

> [!NOTE]
> Le altre pagine per l'aggiornamento degli oggetti possono essere implementate nello stesso modo. Abbiamo lasciato ciò come una sfida.

## Passi successivi

- Ritorna a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
