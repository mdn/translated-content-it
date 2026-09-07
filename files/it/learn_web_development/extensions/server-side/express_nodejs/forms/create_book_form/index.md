---
title: Modulo Create Book
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

Questo sottoarticolo mostra come definire una pagina/modulo per creare oggetti `Book`. È un po' più complicato delle pagine equivalenti per `Author` o `Genre`, perché occorre recuperare e visualizzare i record `Author` e `Genre` disponibili nel modulo `Book`.

## Importare i metodi di validazione e sanitizzazione

Aprire **/controllers/bookController.js** e aggiungere la riga seguente all'inizio del file (prima delle funzioni di route):

```js
const { body, validationResult } = require("express-validator");
```

## Controller — route get

Trovare il metodo controller esportato `book_create_get()` e sostituirlo con il codice seguente:

```js
// Display book create form on GET.
exports.book_create_get = async (req, res, next) => {
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
};
```

Questo usa `await` sul risultato di `Promise.all()` per recuperare tutti gli oggetti `Author` e `Genre` in parallelo (lo stesso approccio usato in [Tutorial Express Parte 5: Visualizzare i dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data)).
Questi vengono quindi passati alla vista **`book_form.pug`** come variabili denominate `authors` e `genres` (insieme al `title` della pagina).

## Controller — route post

Trovare il metodo controller esportato `book_create_post()` e sostituirlo con il codice seguente.

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

  async (req, res, next) => {
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
      return;
    }

    // Data from form is valid. Save book.
    await book.save();
    res.redirect(book.url);
  },
];
```

La struttura e il comportamento di questo codice sono quasi esattamente gli stessi delle funzioni di route post per i moduli [`Genre`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form) e [`Author`](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form). Per prima cosa vengono validati e sanificati i dati. Se i dati non sono validi, il modulo viene visualizzato nuovamente insieme ai dati originariamente immessi dall'utente e a un elenco di messaggi di errore. Se i dati sono validi, il nuovo record `Book` viene salvato e l'utente viene reindirizzato alla pagina dei dettagli del libro.

La differenza principale rispetto all'altro codice di gestione dei moduli riguarda il modo in cui vengono sanificate le informazioni sui generi.
Il modulo restituisce un array di elementi `Genre` (mentre per gli altri campi restituisce una stringa).
Per validare le informazioni, la richiesta viene prima convertita in un array (necessario per il passaggio successivo).

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

Viene quindi usato un carattere jolly (`*`) nel sanitizzatore per validare singolarmente ciascuna voce dell'array dei generi. Il codice seguente mostra come: questo si traduce in "sanifica ogni elemento sotto la chiave `genre`".

```js
[
  // …
  body("genre.*").escape(),
  // …
];
```

L'ultima differenza rispetto all'altro codice di gestione dei moduli è che occorre passare al modulo tutti i generi e gli autori esistenti.
Per contrassegnare i generi selezionati dall'utente, vengono iterati tutti i generi e viene aggiunto il parametro `checked="true"` a quelli presenti nei dati post (come riprodotto nel frammento di codice seguente).

```js
// Mark our selected genres as checked.
for (const genre of allGenres) {
  if (book.genre.includes(genre._id)) {
    genre.checked = "true";
  }
}
```

## Vista

Creare **/views/book_form.pug** e copiarvi il testo seguente.

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

Le differenze principali riguardano l'implementazione dei campi di tipo selezione: `Author` e `Genre`.

- L'insieme dei generi viene visualizzato come caselle di controllo e usa il valore `checked` impostato nel controller per determinare se la casella debba essere selezionata o meno.
- L'insieme degli autori viene visualizzato come un elenco a discesa a selezione singola ordinato alfabeticamente (l'elenco passato al template è già ordinato, quindi non è necessario farlo nel template).
  Se l'utente ha precedentemente selezionato un autore del libro (ad esempio, durante la correzione di valori di campo non validi dopo l'invio iniziale del modulo, oppure durante l'aggiornamento dei dettagli del libro), l'autore verrà selezionato nuovamente quando il modulo viene visualizzato. Qui l'autore da selezionare viene determinato confrontando l'id dell'opzione dell'autore corrente con il valore precedentemente immesso dall'utente (passato tramite la variabile `book`).

> [!NOTE]
> Se è presente un errore nel modulo inviato, quando il modulo deve essere nuovamente renderizzato, l'id del nuovo autore del libro e gli id degli autori dei libri esistenti sono di tipo `Schema.Types.ObjectId`. Per confrontarli, occorre quindi convertirli prima in stringhe.

## Come appare?

Eseguire l'applicazione, aprire il browser all'indirizzo `http://localhost:3000/`, quindi selezionare il collegamento _Create new book_. Se tutto è configurato correttamente, il sito dovrebbe assomigliare più o meno allo screenshot seguente. Dopo l'invio di un libro valido, questo dovrebbe essere salvato e verrà aperta la pagina dei dettagli del libro.

![Screenshot del modulo vuoto Create Book di Local Library su localhost:3000. La pagina è divisa in due colonne. La stretta colonna sinistra contiene una barra di navigazione verticale con 10 collegamenti separati in due sezioni da una linea orizzontale chiara. I collegamenti nella sezione superiore puntano a dati già creati. I collegamenti nella sezione inferiore portano ai moduli per creare nuovi dati. L'ampia colonna destra contiene il modulo di creazione del libro con un'intestazione 'Create Book' e quattro campi di input etichettati 'Title', 'Author', 'Summary', 'ISBN' e 'Genre', seguiti da quattro caselle di controllo dei generi: fantasy, fantascienza, poesia francese e azione. Nella parte inferiore del modulo è presente un pulsante 'Submit'.](locallibary_express_book_create_empty.png)

## Passaggi successivi

Tornare a [Tutorial Express Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).

Procedere al sottoarticolo successivo della parte 6: [Modulo Create BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_BookInstance_form).
