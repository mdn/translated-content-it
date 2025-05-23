---
title: Aggiorna modulo libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form
l10n:
  sourceCommit: f2dc3d5367203c860cf1a71ce0e972f018523849
---

Questo ultimo sottoarticolo mostra come definire una pagina per aggiornare gli oggetti `Book`. La gestione del modulo durante l'aggiornamento di un libro è molto simile a quella per la creazione di un libro, tranne che devi popolare il modulo nel percorso `GET` con i valori dal database.

## Controller—percorso get

Apri **/controllers/bookController.js**. Trova il metodo controller esportato `book_update_get()` e sostituiscilo con il seguente codice.

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
    book: book,
  });
});
```

Il controller ottiene l'id del `Book` da aggiornare dal parametro dell'URL (`req.params.id`).
Attende la promessa restituita da `Promise.all()` per ottenere il record specificato di `Book` (popolando i campi di genere e autore) e tutti i record di `Author` e `Genre`.

Quando le operazioni vengono completate, la funzione verifica se sono stati trovati libri e se nessuno è stato trovato invia un errore "Libro non trovato" al middleware di gestione errori.

> [!NOTE]
> Non trovare risultati di un libro non è **un errore** per una ricerca, ma lo è per questa applicazione perché sappiamo che deve esserci un record di libro corrispondente! Il codice sopra confronta (`book===null`) nel callback, ma potrebbe anche aver concatenato il metodo [orFail()](<https://mongoosejs.com/docs/api/query.html#Query.prototype.orFail()>) alla query.

Quindi contrassegniamo i generi attualmente selezionati come verificati e rendiamo la vista **book_form.pug**, passando variabili per `title`, libro, tutti gli `authors` e tutti i `genres`.

## Controller—percorso post

Trova il metodo controller esportato `book_update_post()` e sostituiscilo con il seguente codice.

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
        book: book,
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

Questo è molto simile al percorso post utilizzato quando si crea un `Book`.
Per prima cosa, convalidiamo e sanifichiamo i dati del libro dal modulo e li utilizziamo per creare un nuovo oggetto `Book` (impostando il suo valore `_id` sull'id dell'oggetto da aggiornare). Se ci sono errori quando convalidiamo i dati, allora renderizziamo nuovamente il modulo, visualizzando inoltre i dati inseriti dall'utente, gli errori e le liste di generi e autori. Se non ci sono errori, quindi chiamiamo `Book.findByIdAndUpdate()` per aggiornare il documento `Book` e poi reindirizziamo alla sua pagina di dettaglio.

## Vista

Non c'è bisogno di cambiare la vista per il modulo (**/views/book_form.pug**) dato che lo stesso modello funziona sia per la creazione che per l'aggiornamento del libro.

## Aggiungi un pulsante di aggiornamento

Apri la vista **book_detail.pug** e assicurati che ci siano link sia per eliminare che per aggiornare i libri in fondo alla pagina, come mostrato di seguito.

```pug
  hr
  p
    a(href=book.url+'/delete') Delete Book
  p
    a(href=book.url+'/update') Update Book
```

Dovresti ora essere in grado di aggiornare i libri dalla pagina _Dettaglio libro_.

## Che aspetto ha?

Esegui l'applicazione, apri il browser su `http://localhost:3000/`, seleziona il link _Tutti i libri_, quindi seleziona un libro particolare. Infine, seleziona il link _Aggiorna Libro_.

Il modulo dovrebbe apparire esattamente come la pagina _Crea libro_, solo con un titolo 'Aggiorna libro', e precompilato con i valori del record.

![La sezione di aggiornamento libro dell'applicazione Biblioteca locale. La colonna di sinistra ha una barra di navigazione verticale. La colonna di destra ha un modulo per aggiornare il libro con un'intestazione che recita 'Aggiorna libro'. Ci sono cinque campi di input etichettati Titolo, Autore, Sommario, ISBN, Genere. Il genere è un campo opzione con casella di selezione. C'è un pulsante etichettato 'Invia' alla fine.](locallibary_express_book_update_noerrors.png)

> [!NOTE]
> Le altre pagine per l'aggiornamento degli oggetti possono essere implementate nello stesso modo. L'abbiamo lasciato come sfida.

## Prossimi passi

- Torna a [Guida pratica di Express Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
