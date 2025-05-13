---
title: Creare la pagina del modulo BookInstance
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo sotto-articolo mostra come definire una pagina/modulo per creare oggetti `BookInstance`.
È molto simile al modulo che abbiamo utilizzato per [creare oggetti `Book`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form).

## Importare metodi di validazione e sanitizzazione

Aprire **/controllers/bookinstanceController.js** e aggiungere le seguenti righe all'inizio del file:

```js
const { body, validationResult } = require("express-validator");
```

## Controller—rotta GET

All'inizio del file, richiedere il modulo _Book_ (necessario perché ogni `BookInstance` è associato a un particolare `Book`).

```js
const Book = require("../models/book");
```

Trovare il metodo controller esportato `bookinstance_create_get()` e sostituirlo con il seguente codice.

```js
// Display BookInstance create form on GET.
exports.bookinstance_create_get = asyncHandler(async (req, res, next) => {
  const allBooks = await Book.find({}, "title").sort({ title: 1 }).exec();

  res.render("bookinstance_form", {
    title: "Create BookInstance",
    book_list: allBooks,
  });
});
```

Il controller ottiene una lista ordinata di tutti i libri (`allBooks`) e la passa tramite `book_list` alla vista **`bookinstance_form.pug`** (insieme a un `title`).
Notare che nessun libro è stato selezionato quando visualizziamo per la prima volta questo modulo, quindi non passiamo la variabile `selected_book` a `render()`.
A causa di ciò, `selected_book` avrà un valore di `undefined` nel template.

## Controller—rotta POST

Trovare il metodo controller esportato `bookinstance_create_post()` e sostituirlo con il seguente codice.

```js
// Handle BookInstance create on POST.
exports.bookinstance_create_post = [
  // Validate and sanitize fields.
  body("book", "Book must be specified").trim().isLength({ min: 1 }).escape(),
  body("imprint", "Imprint must be specified")
    .trim()
    .isLength({ min: 1 })
    .escape(),
  body("status").escape(),
  body("due_back", "Invalid date")
    .optional({ values: "falsy" })
    .isISO8601()
    .toDate(),

  // Process request after validation and sanitization.
  asyncHandler(async (req, res, next) => {
    // Extract the validation errors from a request.
    const errors = validationResult(req);

    // Create a BookInstance object with escaped and trimmed data.
    const bookInstance = new BookInstance({
      book: req.body.book,
      imprint: req.body.imprint,
      status: req.body.status,
      due_back: req.body.due_back,
    });

    if (!errors.isEmpty()) {
      // There are errors.
      // Render form again with sanitized values and error messages.
      const allBooks = await Book.find({}, "title").sort({ title: 1 }).exec();

      res.render("bookinstance_form", {
        title: "Create BookInstance",
        book_list: allBooks,
        selected_book: bookInstance.book._id,
        errors: errors.array(),
        bookinstance: bookInstance,
      });
      return;
    } else {
      // Data from form is valid
      await bookInstance.save();
      res.redirect(bookInstance.url);
    }
  }),
];
```

La struttura e il comportamento di questo codice sono gli stessi di quando creiamo gli altri oggetti. Per prima cosa si validano e sanificano i dati. Se i dati non sono validi, si visualizza nuovamente il modulo insieme ai dati originariamente inseriti dall'utente e a una lista di messaggi di errore.
Se i dati sono validi, si salva il nuovo record `BookInstance` e si reindirizza l'utente alla pagina dei dettagli.

## Vista

Creare **/views/bookinstance_form.pug** e copiare il seguente testo.

```pug
extends layout

block content
  h1=title

  form(method='POST')
    div.form-group
      label(for='book') Book:
      select#book.form-control(name='book' required)
        option(value='') --Please select a book--
        for book in book_list
          if selected_book==book._id.toString()
            option(value=book._id, selected) #{book.title}
          else
            option(value=book._id) #{book.title}

    div.form-group
      label(for='imprint') Imprint:
      input#imprint.form-control(type='text' placeholder='Publisher and date information' name='imprint' required value=(undefined===bookinstance ? '' : bookinstance.imprint) )
    div.form-group
      label(for='due_back') Date when book available:
      input#due_back.form-control(type='date' name='due_back' value=(undefined===bookinstance ? '' : bookinstance.due_back_yyyy_mm_dd))

    div.form-group
      label(for='status') Status:
      select#status.form-control(name='status' required)
        option(value='') --Please select a status--
        each val in ['Maintenance', 'Available', 'Loaned', 'Reserved']
          if undefined===bookinstance || bookinstance.status!=val
            option(value=val)= val
          else
            option(value=val selected)= val

    button.btn.btn-primary(type='submit') Submit

  if errors
    ul
      for error in errors
        li!= error.msg
```

> [!NOTE]
> Il template sopra codifica in modo rigido i valori _Status_ (Manutenzione, Disponibile, ecc.) e non "ricorda" i valori inseriti dall'utente.
> Se lo desidera, consideri di reimplementare la lista, passando i dati delle opzioni dal controller e impostando il valore selezionato quando il modulo viene visualizzato nuovamente.

La struttura e il comportamento della vista sono quasi gli stessi del template **book_form.pug**, quindi non lo analizzeremo in dettaglio.
L'unica cosa da notare è la riga in cui impostiamo la data di "restituzione" su `bookinstance.due_back_yyyy_mm_dd` se stiamo popolando il campo della data per un'istanza esistente.

```pug
input#due_back.form-control(type='date', name='due_back' value=(undefined===bookinstance ? '' : bookinstance.due_back_yyyy_mm_dd))
```

Il valore della data deve essere impostato nel formato `YYYY-MM-DD` perché è quello atteso dagli [elementi `<input>` con `type="date"`](/it/docs/Web/HTML/Reference/Elements/input/date), tuttavia la data non è memorizzata in questo formato quindi dobbiamo convertirla prima di impostare il valore nel controllo.
Il metodo `due_back_yyyy_mm_dd()` viene aggiunto al modello `BookInstance` nella sezione successiva.

## Modello—metodo virtuale `due_back_yyyy_mm_dd()`

Aprire il file in cui è stato definito il modello `BookInstanceSchema` (**models/bookinstance.js**).
Aggiungere la funzione virtuale `due_back_yyyy_mm_dd()` mostrata di seguito (dopo la funzione virtuale `due_back_formatted()`):

```js
BookInstanceSchema.virtual("due_back_yyyy_mm_dd").get(function () {
  return DateTime.fromJSDate(this.due_back).toISODate(); // format 'YYYY-MM-DD'
});
```

## Come appare?

Eseguire l'applicazione e aprire il browser su `http://localhost:3000/`.
Poi selezionare il link _Create new book instance (copy)_. Se tutto è impostato correttamente, il suo sito dovrebbe apparire come nello screenshot seguente. Dopo aver inviato un `BookInstance` valido, dovrebbe essere salvato e sarà reindirizzata alla pagina dei dettagli.

![Crea una BookInstance dell'applicazione Local library screenshot da localhost:3000. La pagina è divisa in due colonne. La colonna sinistra stretta ha una barra di navigazione verticale con 10 link separati in due sezioni da una linea orizzontale chiara. La sezione superiore collegata ai dati già creati. I link inferiori vanno ai moduli per creare nuovi dati. La colonna destra larga ha il modulo per creare una nuova istanza del libro con un'intestazione 'Create BookInstance' e quattro campi di input etichettati 'Book', 'Imprint', 'Date when book available' e 'Status'. Il modulo è compilato. C'è un pulsante 'Submit' in fondo al modulo.](locallibary_express_bookinstance_create_empty.png)

## Prossimi passi

- Tornare a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Procedere al prossimo sotto-articolo della parte 6: [Modulo Elimina Autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form).
