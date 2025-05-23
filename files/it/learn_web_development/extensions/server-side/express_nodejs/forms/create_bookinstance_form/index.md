---
title: Creare il modulo BookInstance
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form
l10n:
  sourceCommit: f2dc3d5367203c860cf1a71ce0e972f018523849
---

Questo sottoarticolo mostra come definire una pagina/modulo per creare oggetti `BookInstance`.
È molto simile al modulo che abbiamo utilizzato per [creare oggetti `Book`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form).

## Importare metodi di validazione e sanificazione

Apri **/controllers/bookinstanceController.js** e aggiungi le seguenti righe all'inizio del file:

```js
const { body, validationResult } = require("express-validator");
```

## Controller—rotta GET

All'inizio del file, importa il modulo _Book_ (necessario perché ogni `BookInstance` è associato a un particolare `Book`).

```js
const Book = require("../models/book");
```

Trova il metodo controller esportato `bookinstance_create_get()` e sostituiscilo con il seguente codice.

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

Il controller ottiene un elenco ordinato di tutti i libri (`allBooks`) e lo passa tramite `book_list` alla vista **`bookinstance_form.pug`** (insieme a un `title`).
Nota che nessun libro è stato selezionato quando mostriamo per la prima volta questo modulo, quindi non passiamo la variabile `selected_book` a `render()`.
Per questo motivo, `selected_book` avrà un valore di `undefined` nel modello.

## Controller—rotta POST

Trova il metodo controller esportato `bookinstance_create_post()` e sostituiscilo con il seguente codice.

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
    }
    // Data from form is valid
    await bookInstance.save();
    res.redirect(bookInstance.url);
  }),
];
```

La struttura e il comportamento di questo codice sono gli stessi di quando creiamo gli altri oggetti.
Prima validiamo e sanifichiamo i dati. Se i dati sono invalidi, mostriamo nuovamente il modulo insieme ai dati che l'utente ha inserito originariamente e a una lista di messaggi di errore.
Se i dati sono validi, salviamo il nuovo record `BookInstance` e reindirizziamo l'utente alla pagina di dettaglio.

## Vista

Crea **/views/bookinstance_form.pug** e copia il testo di seguito.

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
> Il modello sopra codifica in modo statico i valori dello _Status_ (Manutenzione, Disponibile, ecc.) e non "ricorda" i valori inseriti dall'utente.
> Se lo desideri, considera di reimplementare l'elenco, passando i dati delle opzioni dal controller e impostando il valore selezionato quando il modulo viene mostrato nuovamente.

La struttura e il comportamento della vista sono quasi gli stessi del modello **book_form.pug**, quindi non lo esamineremo in dettaglio.
L'unica cosa da notare è la riga dove impostiamo la data di "consegna" a `bookinstance.due_back_yyyy_mm_dd` se stiamo popolando l'input di data per un'istanza esistente.

```pug
input#due_back.form-control(type='date', name='due_back' value=(undefined===bookinstance ? '' : bookinstance.due_back_yyyy_mm_dd))
```

Il valore della data deve essere impostato nel formato `YYYY-MM-DD` poiché è quello atteso dagli [`<input>` con `type="date"`](/it/docs/Web/HTML/Reference/Elements/input/date), tuttavia la data non è memorizzata in questo formato e quindi dobbiamo convertirla prima di impostare il valore nel controllo.
Il metodo `due_back_yyyy_mm_dd()` è aggiunto al modello `BookInstance` nella prossima sezione.

## Modello—metodo virtuale `due_back_yyyy_mm_dd()`

Apri il file in cui hai definito il modello `BookInstanceSchema` (**models/bookinstance.js**).
Aggiungi la funzione virtuale `due_back_yyyy_mm_dd()` mostrata di seguito (dopo la funzione virtuale `due_back_formatted()`):

```js
BookInstanceSchema.virtual("due_back_yyyy_mm_dd").get(function () {
  return DateTime.fromJSDate(this.due_back).toISODate(); // format 'YYYY-MM-DD'
});
```

## Come appare?

Esegui l'applicazione e apri il tuo browser su `http://localhost:3000/`.
Poi seleziona il link _Create new book instance (copy)_. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire come nello screenshot seguente. Dopo aver inviato un `BookInstance` valido, dovrebbe essere salvato e verrai portato alla pagina di dettaglio.

![Creare BookInstance dell'applicazione Libreria locale screenshot da localhost:3000. La pagina è divisa in due colonne. La stretta colonna sinistra ha una barra di navigazione verticale con 10 link separati in due sezioni da una linea orizzontale di colore chiaro. La sezione superiore collega ai dati già creati. I link nella sezione inferiore portano ai moduli per creare nuovi dati. La larga colonna destra ha il modulo crea book instance con un'intestazione 'Create BookInstance' e quattro campi di input etichettati 'Book', 'Imprint', 'Date when book available' e 'Status'. Il modulo è compilato. C'è un pulsante 'Submit' in fondo al modulo.](locallibary_express_bookinstance_create_empty.png)

## Passi successivi

- Torna a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Procedi al prossimo sottoarticolo della parte 6: [Modulo di eliminazione autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form).
