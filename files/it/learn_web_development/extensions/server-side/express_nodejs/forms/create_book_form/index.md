---
title: Modulo di Creazione Libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo sottoarticolo mostra come definire una pagina/modulo per creare oggetti di tipo `Book`. Questo è un po' più complicato rispetto alle pagine equivalenti di `Author` o `Genre` perché dobbiamo ottenere e visualizzare i record disponibili di `Author` e `Genre` nel nostro modulo `Book`.

## Importare metodi di validazione e sanificazione

Aprire **/controllers/bookController.js** e aggiungere la seguente riga all'inizio del file (prima delle funzioni di rotta):

```js
const { body, validationResult } = require("express-validator");
```

## Controller—rotta get

Trova il metodo del controller esportato `book_create_get()` e sostituiscilo con il seguente codice:

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

Questo utilizza `await` sul risultato di `Promise.all()` per ottenere tutti gli oggetti `Author` e `Genre` in parallelo (lo stesso approccio utilizzato in [Express Tutorial Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data)).
Questi vengono poi passati alla vista **`book_form.pug`** come variabili denominate `authors` e `genres` (insieme al `title` della pagina).

## Controller—rotta post

Trova il metodo del controller esportato `book_create_post()` e sostituiscilo con il seguente codice:

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

La struttura e il comportamento di questo codice sono quasi esattamente gli stessi delle funzioni di rotta post per i moduli [`Genre`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form) e [`Author`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form). Prima validiamo e sanifichiamo i dati. Se i dati sono invalidi, rivediamo nuovamente il modulo insieme ai dati che erano stati originariamente inseriti dall'utente e a una lista di messaggi di errore. Se i dati sono validi, salviamo quindi il nuovo record `Book` e reindirizziamo l'utente alla pagina dei dettagli del libro.

La differenza principale rispetto al codice per la gestione degli altri moduli è come sanifichiamo le informazioni sui generi.
Il modulo restituisce un array di elementi `Genre` (mentre per altri campi restituisce una stringa).
Per validare le informazioni dobbiamo prima convertire la richiesta in un array (passaggio necessario per il prossimo step).

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

Utilizziamo quindi un carattere jolly (`*`) nel sanificatore per validare individualmente ciascuna delle voci dell'array dei generi. Il codice seguente mostra come - questo si traduce in "sanifica ogni elemento sotto il termine chiave `genre`".

```js
[
  // …
  body("genre.*").escape(),
  // …
];
```

La differenza finale rispetto al codice per la gestione degli altri moduli è che dobbiamo passare nel modulo tutti i generi e gli autori esistenti.
Per indicare i generi che sono stati selezionati dall'utente, itera attraverso tutti i generi e aggiungi il parametro `checked="true"` a quelli che si trovavano nei nostri dati post (come riprodotto nel frammento di codice sotto).

```js
// Mark our selected genres as checked.
for (const genre of allGenres) {
  if (book.genre.includes(genre._id)) {
    genre.checked = "true";
  }
}
```

## Vista

Crea **/views/book_form.pug** e copia nel testo sottostante.

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

La struttura e il comportamento della vista sono quasi gli stessi del template **genre_form.pug**.

Le principali differenze sono nel modo in cui implementiamo i campi di tipo selezione: `Author` e `Genre`.

- Il set di generi è visualizzato come caselle di controllo e utilizza il valore `checked` stabilito nel controller per determinare se la casella deve essere selezionata o meno.
- Il set di autori è visualizzato come una lista a tendina a selezione singola ordinata alfabeticamente (la lista passata al template è già ordinata, quindi non è necessario farlo nel template).
  Se l'utente ha precedentemente selezionato un autore del libro (ad esempio, al momento di correggere valori dei campi invalidi dopo l'invio iniziale del modulo, o quando si aggiornano i dettagli del libro) l'autore sarà ri-selezionato quando il modulo viene mostrato. Qui determiniamo quale autore selezionare confrontando l'id dell'opzione autore attuale con il valore precedentemente inserito dall'utente (passato tramite la variabile `book`).

> [!NOTE]
> Se c'è un errore nel modulo inviato, quindi, quando il modulo deve essere ridisegnato, l'id del nuovo autore del libro e gli id degli autori dei libri esistenti sono di tipo `Schema.Types.ObjectId`. Quindi, per confrontarli dobbiamo prima convertirli in stringhe.

## Che aspetto ha?

Eseguire l'applicazione, aprire il browser su `http://localhost:3000/`, quindi selezionare il link _Create new book_. Se tutto è impostato correttamente, il sito dovrebbe apparire come lo screenshot seguente. Dopo aver inviato un libro valido, dovrebbe essere salvato e sarete portati alla pagina dei dettagli del libro.

![Screenshot del modulo di Creazione Libro nella libreria locale vuota su localhost:3000. La pagina è divisa in due colonne. La colonna stretta a sinistra ha una barra di navigazione verticale con 10 link separati in due sezioni da una linea orizzontale di colore chiaro. La sezione superiore include link a dati già creati. I link inferiori vanno ai moduli per creare nuovi dati. La colonna larga a destra ha il modulo di creazione del libro con un'intestazione 'Create Book' e quattro campi di input etichettati 'Title', 'Author', 'Summary', 'ISBN' e 'Genre' seguiti da quattro caselle di controllo del genere: fantasy, science fiction, poesia francese e azione. In fondo al modulo c'è un pulsante 'Submit'.](locallibary_express_book_create_empty.png)

## Prossimi passi

Tornare a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).

Procedere al prossimo sottoarticolo della parte 6: [Creare un modulo BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form).
