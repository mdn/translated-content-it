---
title: Modulo di Aggiornamento Libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Update_Book_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo ultimo sotto-articolo mostra come definire una pagina per aggiornare oggetti `Book`. La gestione del modulo quando si aggiorna un libro è molto simile a quella per la creazione di un libro, tranne per il fatto che bisogna popolare il modulo nel percorso `GET` con i valori dal database.

## Controller—percorso get

Apri **/controllers/bookController.js**. Trova il metodo `book_update_get()` del controller esportato e sostituiscilo con il seguente codice.

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

Il controller ottiene l'id del `Book` da aggiornare dal parametro URL (`req.params.id`).
Attende (`awaits`) la promessa restituita da `Promise.all()` per ottenere il record `Book` specificato (popolando i suoi campi genre e author) e tutti i record `Author` e `Genre`.

Quando le operazioni sono completate, la funzione controlla se sono stati trovati dei libri, e se nessuno è stato trovato invia un errore "Libro non trovato" al middleware di gestione degli errori.

> [!NOTE]
> Non trovare alcun risultato di libro non è un **errore** per una ricerca — ma lo è per questa applicazione perché sappiamo che deve esserci un record del libro corrispondente! Il codice sopra confronta per (`book===null`) nel callback, ma potrebbe ugualmente aver concatenato il metodo [orFail()](<https://mongoosejs.com/docs/api/query.html#Query.prototype.orFail()>) alla query.

Quindi marchiamo i generi attualmente selezionati come controllati e poi rendiamo la vista **book_form.pug**, passando variabili per `title`, `book`, tutti gli `authors` e tutti i `genres`.

## Controller—percorso post

Trova il metodo `book_update_post()` del controller esportato e sostituiscilo con il seguente codice.

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
    } else {
      // Data from form is valid. Update the record.
      const updatedBook = await Book.findByIdAndUpdate(req.params.id, book, {});
      // Redirect to book detail page.
      res.redirect(updatedBook.url);
    }
  }),
];
```

Questo è molto simile al percorso post usato quando si crea un `Book`.
Per prima cosa, convalidiamo e sanifichiamo i dati del libro dal modulo e li usiamo per creare un nuovo oggetto `Book` (impostando il suo valore `_id` sull'id dell'oggetto da aggiornare). Se ci sono errori quando convalidiamo i dati, allora renderizziamo nuovamente il modulo, visualizzando inoltre i dati inseriti dall'utente, gli errori e le liste di generi e autori. Se non ci sono errori chiamiamo `Book.findByIdAndUpdate()` per aggiornare il documento `Book`, e quindi reindirizziamo alla sua pagina di dettaglio.

## Vista

Non è necessario modificare la vista per il modulo (**/views/book_form.pug**) poiché lo stesso template funziona sia per la creazione che per l'aggiornamento del libro.

## Aggiungi un pulsante di aggiornamento

Apri la vista **book_detail.pug** e assicurati che ci siano link sia per eliminare che per aggiornare i libri in fondo alla pagina, come mostrato di seguito.

```pug
  hr
  p
    a(href=book.url+'/delete') Delete Book
  p
    a(href=book.url+'/update') Update Book
```

Ora dovresti essere in grado di aggiornare i libri dalla pagina _Book detail_.

## Come appare?

Esegui l'applicazione, apri il browser su `http://localhost:3000/`, seleziona il link _All books_, quindi seleziona un libro particolare. Infine seleziona il link _Update Book_.

Il modulo dovrebbe apparire proprio come la pagina _Create book_, solo con un titolo di 'Update book', e pre-popolato con i valori del record.

![La sezione di aggiornamento del libro dell'applicazione Local library. La colonna di sinistra ha una barra di navigazione verticale. La colonna di destra ha un modulo per aggiornare il libro con un'intestazione che recita 'Update book'. Ci sono cinque campi di input denominati Titolo, Autore, Sommario, ISBN, Genere. Genere è un campo opzione a checkbox. Alla fine c'è un pulsante etichettato 'Submit'.](locallibary_express_book_update_noerrors.png)

> [!NOTE]
> Anche le altre pagine per aggiornare gli oggetti possono essere implementate più o meno allo stesso modo. Abbiamo lasciato che questo fosse una sfida.

## Prossimi passi

- Torna a [Express Tutorial Part 6: Working with forms](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
