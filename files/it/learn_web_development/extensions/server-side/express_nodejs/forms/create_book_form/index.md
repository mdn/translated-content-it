---
title: Modulo Creazione Libro
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

Questo sottoarticolo mostra come definire una pagina/modulo per creare oggetti `Book`. Questo è un po' più complicato rispetto alle pagine equivalenti `Author` o `Genre` perché dobbiamo ottenere e visualizzare i record di `Author` e `Genre` disponibili nel nostro modulo `Book`.

## Importare metodi di validazione e sanificazione

Aprire **/controllers/bookController.js** e aggiungere la seguente linea all'inizio del file (prima delle funzioni di routing):

```js
const { body, validationResult } = require("express-validator");
```

## Controller—rotta get

Trovare il metodo controller `book_create_get()` esportato e sostituirlo con il seguente codice:

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

Questo utilizza `await` sul risultato di `Promise.all()` per ottenere tutti gli oggetti `Author` e `Genre` in parallelo (lo stesso approccio utilizzato in [Express Tutorial Parte 5: Visualizzare i dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data)).
Questi vengono poi passati alla vista **`book_form.pug`** come variabili denominate `authors` e `genres` (insieme al `title` della pagina).

## Controller—rotta post

Trovare il metodo controller `book_create_post()` esportato e sostituirlo con il seguente codice.

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
        book,
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

La struttura e il comportamento di questo codice sono quasi esattamente gli stessi delle funzioni delle rotte post per i moduli [`Genre`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form) e [`Author`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form). Prima si valida e sanifica i dati. Se i dati sono non validi, viene re-visualizzato il modulo insieme ai dati inseriti originalmente dall'utente e a un elenco di messaggi di errore. Se i dati sono validi, si salva il nuovo record `Book` e si reindirizza l'utente alla pagina dei dettagli del libro.

La principale differenza rispetto al codice di gestione di altri moduli è come si sanificano le informazioni sul genere.
Il modulo restituisce un array di elementi `Genre` (mentre per altri campi restituisce una stringa).
Per convalidare le informazioni si converte prima la richiesta in un array (necessario per il passo successivo).

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

Si utilizza quindi un carattere jolly (`*`) nel sanitizer per convalidare individualmente ciascuna voce dell'array genere. Il codice sotto mostra come - questo si traduce in "sanificare ogni elemento sotto la chiave `genre`".

```js
[
  // …
  body("genre.*").escape(),
  // …
];
```

La differenza finale rispetto al codice di gestione di altri moduli è che dobbiamo passare tutti i generi e gli autori esistenti al modulo.
Per contrassegnare i generi che sono stati selezionati dall'utente, iteriamo attraverso tutti i generi e aggiungiamo il parametro `checked="true"` a quelli che erano nei dati del nostro post (come riprodotto nel frammento di codice sottostante).

```js
// Mark our selected genres as checked.
for (const genre of allGenres) {
  if (book.genre.includes(genre._id)) {
    genre.checked = "true";
  }
}
```

## Vista

Creare **/views/book_form.pug** e copiare il testo sottostante.

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

Le principali differenze riguardano come implementiamo i campi di tipo selezione: `Author` e `Genre`.

- L'insieme dei generi è visualizzato come checkbox e utilizza il valore `checked` impostato nel controller per determinare se la casella debba essere selezionata o meno.
- L'insieme degli autori è visualizzato come una lista a discesa a selezione singola ordinata alfabeticamente (la lista passata al template è già ordinata, quindi non dobbiamo farlo nel template).
  Se l'utente ha precedentemente selezionato un autore del libro (cioè, quando si correggono valori di campo non validi dopo l'invio iniziale del modulo o quando si aggiornano i dettagli del libro), l'autore sarà nuovamente selezionato quando il modulo viene visualizzato. Qui determiniamo quale autore selezionare confrontando l'id dell'opzione autore corrente con il valore inserito precedentemente dall'utente (passato tramite la variabile `book`).

> [!NOTE]
> Se c'è un errore nel modulo inviato, allora, quando il modulo deve essere ri-renderizzato, l'id del nuovo autore del libro e gli id degli autori dei libri esistenti sono di tipo `Schema.Types.ObjectId`. Quindi per confrontarli dobbiamo prima convertirli in stringhe.

## Come appare?

Eseguire l'applicazione, aprire il browser su `http://localhost:3000/`, quindi selezionare il link _Create new book_. Se tutto è impostato correttamente, il tuo sito dovrebbe apparire come nello screenshot seguente. Dopo aver inviato un libro valido, dovrebbe essere salvato e verrà indirizzato alla pagina dei dettagli del libro.

![Screenshot del modulo Creazione Libro vuoto della Libreria Locale su localhost:3000. La pagina è divisa in due colonne. La colonna stretta a sinistra ha una barra di navigazione verticale con 10 link separati in due sezioni da una linea orizzontale chiara. La sezione superiore ha dei link per i dati già creati. I link inferiori portano ai moduli per creare nuovi dati. La larga colonna a destra ha il modulo di creazione libro con una intestazione 'Create Book' e quattro campi di input etichettati 'Title', 'Author', 'Summary', 'ISBN' e 'Genre' seguito da quattro checkbox di genere: fantasy, science fiction, french poetry e action. C'è un pulsante 'Submit' in fondo al modulo.](locallibary_express_book_create_empty.png)

## Prossimi passi

Torna a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).

Procedi al prossimo sottoarticolo della parte 6: [Modulo Creazione Istanza Libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form).
