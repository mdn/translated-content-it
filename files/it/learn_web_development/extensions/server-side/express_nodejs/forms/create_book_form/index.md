---
title: Creare il modulo Book
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo sottotitolo mostra come definire una pagina/modulo per creare oggetti `Book`. Questo è un po' più complicato delle pagine equivalenti per `Author` o `Genre` perché è necessario ottenere e visualizzare i record disponibili di `Author` e `Genre` nel nostro modulo `Book`.

## Importare i metodi di validazione e sanificazione

Apri **/controllers/bookController.js** e aggiungi la seguente riga all'inizio del file (prima delle funzioni di rotta):

```js
const { body, validationResult } = require("express-validator");
```

## Controller—get route

Trova il metodo controller esportato `book_create_get()` e sostituiscilo con il seguente codice:

```js
// Display book create form on GET.
exports.book_create_get = asyncHandler(async (req, res, next) => {
  // Get all authors and genres, which we can use for adding to our book.
  const [allAuthors, allGenres] = await Promise.all([
    Author.find().sort({ family_name: 1 }).exec(),
    Genre.find().sort({ name: 1 }).exec(),
  ]);

  res.render("book_form", {
    title: "Create Book",
    authors: allAuthors,
    genres: allGenres,
  });
});
```

Questo usa `await` sul risultato di `Promise.all()` per ottenere tutti gli oggetti `Author` e `Genre` in parallelo (lo stesso approccio usato in [Parte 5 del tutorial Express: Visualizzare i dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data)).
Questi vengono poi passati alla vista **`book_form.pug`** come variabili chiamate `authors` e `genres` (insieme al `title` della pagina).

## Controller—post route

Trova il metodo controller esportato `book_create_post()` e sostituiscilo con il seguente codice.

```js
// Handle book create on POST.
exports.book_create_post = [
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

    // Create a Book object with escaped and trimmed data.
    const book = new Book({
      title: req.body.title,
      author: req.body.author,
      summary: req.body.summary,
      isbn: req.body.isbn,
      genre: req.body.genre,
    });

    if (!errors.isEmpty()) {
      // There are errors. Render form again with sanitized values/error messages.

      // Get all authors and genres for form.
      const [allAuthors, allGenres] = await Promise.all([
        Author.find().sort({ family_name: 1 }).exec(),
        Genre.find().sort({ name: 1 }).exec(),
      ]);

      // Mark our selected genres as checked.
      for (const genre of allGenres) {
        if (book.genre.includes(genre._id)) {
          genre.checked = "true";
        }
      }
      res.render("book_form", {
        title: "Create Book",
        authors: allAuthors,
        genres: allGenres,
        book: book,
        errors: errors.array(),
      });
    } else {
      // Data from form is valid. Save book.
      await book.save();
      res.redirect(book.url);
    }
  }),
];
```

La struttura e il comportamento di questo codice sono praticamente identici alle funzioni di rotta post per i moduli [`Genre`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form) e [`Author`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form). Prima validiamo e sanifichiamo i dati. Se i dati non sono validi, reindirizziamo il modulo insieme ai dati che erano stati originariamente immessi dall'utente e un elenco di messaggi di errore. Se i dati sono validi, salviamo il nuovo record `Book` e reindirizziamo l'utente alla pagina dei dettagli del libro.

La principale differenza rispetto all'altro codice di gestione dei moduli è come sanifichiamo le informazioni del genere.
Il modulo restituisce un array di elementi `Genre` (mentre per gli altri campi restituisce una stringa).
Per validare le informazioni, prima convertiamo la richiesta in un array (necessario per il passaggio successivo).

```js
[
  // Convert the genre to an array.
  (req, res, next) => {
    if (!Array.isArray(req.body.genre)) {
      req.body.genre =
        typeof req.body.genre === "undefined" ? [] : [req.body.genre];
    }
    next();
  },
  // …
];
```

Usiamo quindi un carattere jolly (`*`) nel sanificatore per validare individualmente ciascuna delle voci dell'array di generi. Il codice sottostante mostra come: questo si traduce in "sanifica ogni elemento sotto la chiave `genre`".

```js
[
  // …
  body("genre.*").escape(),
  // …
];
```

La differenza finale rispetto agli altri codici di gestione dei moduli è che dobbiamo passare al modulo tutti i generi e gli autori esistenti.
Per contrassegnare i generi selezionati dall'utente, iteriamo su tutti i generi e aggiungiamo il parametro `checked="true"` a quelli presenti nei nostri dati di post (come riprodotto nel frammento di codice sottostante).

```js
// Mark our selected genres as checked.
for (const genre of allGenres) {
  if (book.genre.includes(genre._id)) {
    genre.checked = "true";
  }
}
```

## Vista

Crea **/views/book_form.pug** e copia il testo sottostante.

```pug
extends layout

block content
  h1= title

  form(method='POST')
    div.form-group
      label(for='title') Title:
      input#title.form-control(type='text', placeholder='Name of book' name='title' required value=(undefined===book ? '' : book.title) )
    div.form-group
      label(for='author') Author:
      select#author.form-control(name='author' required)
        option(value='') --Please select an author--
        for author in authors
          if book
            if author._id.toString()===book.author._id.toString()
              option(value=author._id selected) #{author.name}
            else
              option(value=author._id) #{author.name}
          else
            option(value=author._id) #{author.name}
    div.form-group
      label(for='summary') Summary:
      textarea#summary.form-control(placeholder='Summary' name='summary' required)= undefined===book ? '' : book.summary
    div.form-group
      label(for='isbn') ISBN:
      input#isbn.form-control(type='text', placeholder='ISBN13' name='isbn' value=(undefined===book ? '' : book.isbn) required)
    div.form-group
      label Genre:
      div
        for genre in genres
          div(style='display: inline; padding-right:10px;')
            if genre.checked
              input.checkbox-input(type='checkbox', name='genre', id=genre._id, value=genre._id, checked)
            else
              input.checkbox-input(type='checkbox', name='genre', id=genre._id, value=genre._id)
            label(for=genre._id) &nbsp;#{genre.name}
    button.btn.btn-primary(type='submit') Submit

  if errors
    ul
      for error in errors
        li!= error.msg
```

La struttura e il comportamento della vista sono praticamente identici al modello **genre_form.pug**.

Le principali differenze riguardano come implementiamo i campi di tipo selezione: `Author` e `Genre`.

- L'insieme dei generi viene visualizzato come caselle di controllo e utilizza il valore `checked` impostato nel controller per determinare se la casella deve essere selezionata.
- L'insieme degli autori viene visualizzato come un elenco a discesa ordinato alfabeticamente a selezione singola (l'elenco passato al template è già ordinato, quindi non è necessario farlo nel template).
  Se l'utente ha precedentemente selezionato un autore di libri (cioè, quando corregge valori di campo non validi dopo l'invio iniziale del modulo, o quando aggiorna i dettagli del libro) l'autore verrà riselezionato quando il modulo viene visualizzato. Qui determiniamo quale autore selezionare confrontando l'id dell'opzione autore corrente con il valore precedentemente inserito dall'utente (passato tramite la variabile `book`).

> [!NOTE]
> Se c'è un errore nel modulo inviato, allora, quando il modulo deve essere ristampato, l'id del nuovo autore del libro e gli id degli autori dei libri esistenti sono di tipo `Schema.Types.ObjectId`. Quindi per confrontarli dobbiamo prima convertirli in stringhe.

## Come appare?

Esegui l'applicazione, apri il tuo browser su `http://localhost:3000/`, quindi seleziona il link _Create new book_. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire come nel seguente screenshot. Dopo aver inviato un libro valido, dovrebbe essere salvato e sarai portato alla pagina dei dettagli del libro.

![Screenshot del modulo vuoto Crea libro della libreria locale su localhost:3000. La pagina è divisa in due colonne. La stretta colonna sinistra ha una barra di navigazione verticale con 10 link separati in due sezioni da una linea orizzontale chiara. La sezione superiore collega ai dati già creati. I link in basso vanno ai moduli per creare nuovi dati. La larga colonna destra ha il modulo crea libro con un'intestazione 'Create Book' e quattro campi di input etichettati 'Title', 'Author', 'Summary', 'ISBN' e 'Genre' seguiti da quattro caselle di controllo di genere: fantasy, science fiction, french poetry e action. C'è un pulsante 'Submit' in fondo al modulo.](locallibary_express_book_create_empty.png)

## Passi successivi

Torna alla [Parte 6 del tutorial Express: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).

Prosegui al prossimo sotto-articolo della parte 6: [Crea il modulo BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form).
