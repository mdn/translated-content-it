---
title: Creare il modulo BookInstance
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

Questo sottoarticolo mostra come definire una pagina/modulo per creare oggetti `BookInstance`.
È molto simile al modulo usato per [creare oggetti `Book`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form).

## Importare i metodi di validazione e sanitizzazione

Aprire **/controllers/bookinstanceController.js** e aggiungere le righe seguenti all'inizio del file:

```js
const { body, validationResult } = require("express-validator");
```

## Controller—rotta get

All'inizio del file, richiedere il modulo _Book_ (necessario perché ogni `BookInstance` è associato a un particolare `Book`).

```js
const Book = require("../models/book");
```

Trovare il metodo controller esportato `bookinstance_create_get()` e sostituirlo con il codice seguente.

```js
// Display BookInstance create form on GET.
exports.bookinstance_create_get = async (req, res, next) => {
  const allBooks = await Book.find({}, "title").sort({ title: 1 }).exec();

  res.render("bookinstance_form", {
    title: "Create BookInstance",
    book_list: allBooks,
  });
};
```

Il controller ottiene un elenco ordinato di tutti i libri (`allBooks`) e lo passa tramite `book_list` alla vista **`bookinstance_form.pug`** (insieme a un `title`).
Notare che nessun libro è stato selezionato quando questo modulo viene visualizzato per la prima volta, quindi non viene passata la variabile `selected_book` a `render()`.
Per questo motivo, `selected_book` avrà un valore `undefined` nel template.

## Controller—rotta post

Trovare il metodo controller esportato `bookinstance_create_post()` e sostituirlo con il codice seguente.

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
  async (req, res, next) => {
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
  },
];
```

La struttura e il comportamento di questo codice sono gli stessi della creazione degli altri oggetti.
Prima vengono validati e sanitizzati i dati. Se i dati non sono validi, il modulo viene visualizzato nuovamente insieme ai dati originariamente inseriti dall'utente e a un elenco di messaggi di errore.
Se i dati sono validi, viene salvato il nuovo record `BookInstance` e l'utente viene reindirizzato alla pagina dei dettagli.

## Vista

Creare **/views/bookinstance_form.pug** e copiarvi il testo seguente.

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
> Il template precedente codifica direttamente i valori di _Status_ (Maintenance, Available, ecc.) e non "ricorda" i valori inseriti dall'utente.
> Se lo si desidera, considerare di reimplementare l'elenco, passando i dati delle opzioni dal controller e impostando il valore selezionato quando il modulo viene visualizzato nuovamente.

La struttura e il comportamento della vista sono quasi gli stessi del template **book_form.pug**, quindi non verranno esaminati in dettaglio.
L'unico aspetto da notare è la riga in cui la data di "restituzione prevista" viene impostata su `bookinstance.due_back_yyyy_mm_dd` se viene popolato l'input della data per un'istanza esistente.

```pug
input#due_back.form-control(type='date', name='due_back' value=(undefined===bookinstance ? '' : bookinstance.due_back_yyyy_mm_dd))
```

Il valore della data deve essere impostato nel formato `YYYY-MM-DD` perché è quello previsto dagli [elementi `<input>` con `type="date"`](/it/docs/Web/HTML/Reference/Elements/input/date); tuttavia, la data non viene memorizzata in questo formato, quindi deve essere convertita prima di impostare il valore nel controllo.
Il metodo `due_back_yyyy_mm_dd()` viene aggiunto al modello `BookInstance` nella sezione successiva.

## Modello—metodo virtuale `due_back_yyyy_mm_dd()`

Aprire il file in cui è stato definito il modello `BookInstanceSchema` (**models/bookinstance.js**).
Aggiungere la funzione virtuale `due_back_yyyy_mm_dd()` mostrata di seguito (dopo la funzione virtuale `due_back_formatted()`):

```js
BookInstanceSchema.virtual("due_back_yyyy_mm_dd").get(function () {
  return DateTime.fromJSDate(this.due_back).toISODate(); // format 'YYYY-MM-DD'
});
```

## Che aspetto ha?

Eseguire l'applicazione e aprire il browser all'indirizzo `http://localhost:3000/`.
Quindi selezionare il collegamento _Create new book instance (copy)_. Se tutto è configurato correttamente, il sito dovrebbe avere un aspetto simile allo screenshot seguente. Dopo aver inviato un `BookInstance` valido, questo dovrebbe essere salvato e verrà visualizzata la pagina dei dettagli.

![Screenshot della pagina Create BookInstance dell'applicazione della biblioteca locale da localhost:3000. La pagina è divisa in due colonne. La stretta colonna a sinistra contiene una barra di navigazione verticale con 10 collegamenti, separati in due sezioni da una linea orizzontale di colore chiaro. I collegamenti della sezione superiore portano ai dati già creati. I collegamenti inferiori portano ai moduli per creare nuovi dati. L'ampia colonna a destra contiene il modulo di creazione dell'istanza del libro, con il titolo "Create BookInstance" e quattro campi di input etichettati "Book", "Imprint", "Date when book available" e "Status". Il modulo è compilato. Nella parte inferiore del modulo è presente un pulsante "Submit".](locallibary_express_bookinstance_create_empty.png)

## Passaggi successivi

- Tornare a [Tutorial su Express - Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Passare al sottoarticolo successivo della parte 6: [Modulo per eliminare un autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Delete_author_form).
