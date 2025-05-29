---
title: Creare un modulo Autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

Questo sottoarticolo mostra come definire una pagina per creare oggetti `Author`.

## Importare metodi di validazione e sanificazione

Come nel [modulo per i generi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form), per usare _express-validator_ dobbiamo _richiedere_ le funzioni che vogliamo utilizzare.

Apri **/controllers/authorController.js** e aggiungi la seguente riga all'inizio del file (sopra le funzioni di route):

```js
const { body, validationResult } = require("express-validator");
```

## Controller—route GET

Trova il metodo del controller esportato `author_create_get()` e sostituiscilo con il seguente codice. Questo renderizza la vista **author_form.pug**, passando una variabile `title`.

```js
// Display Author create form on GET.
exports.author_create_get = (req, res, next) => {
  res.render("author_form", { title: "Create Author" });
};
```

## Controller—route POST

Trova il metodo del controller esportato `author_create_post()`, e sostituiscilo con il seguente codice.

```js
// Handle Author create on POST.
exports.author_create_post = [
  // Validate and sanitize fields.
  body("first_name")
    .trim()
    .isLength({ min: 1 })
    .escape()
    .withMessage("First name must be specified.")
    .isAlphanumeric()
    .withMessage("First name has non-alphanumeric characters."),
  body("family_name")
    .trim()
    .isLength({ min: 1 })
    .escape()
    .withMessage("Family name must be specified.")
    .isAlphanumeric()
    .withMessage("Family name has non-alphanumeric characters."),
  body("date_of_birth", "Invalid date of birth")
    .optional({ values: "falsy" })
    .isISO8601()
    .toDate(),
  body("date_of_death", "Invalid date of death")
    .optional({ values: "falsy" })
    .isISO8601()
    .toDate(),

  // Process request after validation and sanitization.
  asyncHandler(async (req, res, next) => {
    // Extract the validation errors from a request.
    const errors = validationResult(req);

    // Create Author object with escaped and trimmed data
    const author = new Author({
      first_name: req.body.first_name,
      family_name: req.body.family_name,
      date_of_birth: req.body.date_of_birth,
      date_of_death: req.body.date_of_death,
    });

    if (!errors.isEmpty()) {
      // There are errors. Render form again with sanitized values/errors messages.
      res.render("author_form", {
        title: "Create Author",
        author,
        errors: errors.array(),
      });
      return;
    }
    // Data from form is valid.

    // Save author.
    await author.save();
    // Redirect to new author record.
    res.redirect(author.url);
  }),
];
```

> [!WARNING]
> Non validare mai i _nomi_ utilizzando `isAlphanumeric()` (come abbiamo fatto sopra) poiché molti nomi usano altri set di caratteri.
> Lo facciamo qui per dimostrare come il validatore viene usato, e come può essere concatenato con altri validatori e la segnalazione degli errori.

La struttura e il comportamento di questo codice sono quasi esattamente gli stessi della creazione di un oggetto `Genre`. Prima validiamo e sanifichiamo i dati. Se i dati sono invalidi, riesponiamo il modulo insieme ai dati che l'utente ha originariamente inserito e a un elenco di messaggi di errore. Se i dati sono validi, salviamo il nuovo record dell'autore e reindirizziamo l'utente alla pagina dei dettagli dell'autore.

Diversamente dall'handler della post per il `Genre`, non verifichiamo se l'oggetto `Author` esiste già prima di salvarlo. Si potrebbe argomentare che dovremmo, anche se al momento possiamo avere più autori con lo stesso nome.

Il codice di validazione dimostra diverse nuove funzionalità:

- Possiamo concatenare i validatori, utilizzando `withMessage()` per specificare il messaggio di errore da visualizzare se il metodo di validazione precedente fallisce.
  Questo rende molto facile fornire messaggi di errore specifici senza molta duplicazione di codice.

  ```js
  [
    // Validate and sanitize fields.
    body("first_name")
      .trim()
      .isLength({ min: 1 })
      .escape()
      .withMessage("First name must be specified.")
      .isAlphanumeric()
      .withMessage("First name has non-alphanumeric characters."),
    // …
  ];
  ```

- Possiamo utilizzare la funzione `optional()` per eseguire una validazione successiva solo se un campo è stato inserito (ciò ci consente di validare campi opzionali).
  Ad esempio, di seguito verifichiamo che la data di nascita opzionale sia una data conforme a ISO8601 (l'oggetto `{ values: "falsy" }` passato significa che accetteremo sia una stringa vuota che `null` come valore vuoto).

  ```js
  [
    body("date_of_birth", "Invalid date of birth")
      .optional({ values: "falsy" })
      .isISO8601()
      .toDate(),
  ];
  ```

- I parametri vengono ricevuti dalla richiesta come stringhe. Possiamo utilizzare `toDate()` (o `toBoolean()`) per convertirli nei tipi JavaScript corretti (come mostrato alla fine della catena di validatori sopra).

## Vista

Crea **/views/author_form.pug** e copia il testo seguente.

```pug
extends layout

block content
  h1=title

  form(method='POST')
    div.form-group
      label(for='first_name') First Name:
      input#first_name.form-control(type='text', placeholder='First name (Christian)' name='first_name' required value=(undefined===author ? '' : author.first_name) )
      label(for='family_name') Family Name:
      input#family_name.form-control(type='text', placeholder='Family name (Surname)' name='family_name' required value=(undefined===author ? '' : author.family_name))
    div.form-group
      label(for='date_of_birth') Date of birth:
      input#date_of_birth.form-control(type='date' name='date_of_birth' value=(undefined===author ? '' : author.date_of_birth) )
    button.btn.btn-primary(type='submit') Submit

  if errors
    ul
      for error in errors
        li!= error.msg
```

La struttura e il comportamento di questa vista sono esattamente gli stessi del modello **genre_form.pug**, quindi non lo descriveremo di nuovo.

> [!NOTE]
> Alcuni browser non supportano l'input `type="date"`, quindi non otterrai il widget del selezionatore di date o il placeholder predefinito `gg/mm/aaaa`, ma avrai invece un campo di testo semplice vuoto. Un'alternativa è aggiungere esplicitamente l'attributo `placeholder='gg/mm/aaaa'` in modo che su browser meno capaci si ottengano ancora informazioni sul formato di testo desiderato.

### Sfida: Aggiunta della data di morte

Il modello sopra manca di un campo per l'inserimento della `date_of_death`. Crea il campo seguendo lo stesso schema del gruppo di moduli per la data di nascita!

## Che aspetto ha?

Esegui l'applicazione, apri il tuo browser su `http://localhost:3000/`, quindi seleziona il link _Create new author_. Se tutto è stato configurato correttamente, il tuo sito dovrebbe apparire come nello screenshot seguente. Dopo aver inserito un valore, dovrebbe essere salvato e verrai indirizzato alla pagina dei dettagli dell'autore.

![Pagina Creazione Autore - Express Local Library site](locallibary_express_author_create_empty.png)

> [!NOTE]
> Se sperimenti con vari formati di input per le date, potresti scoprire che il formato `aaaa-mm-gg` funziona in modo anomalo. Ciò è dovuto al fatto che JavaScript tratta le stringhe di data come contenenti l'orario delle 0 ore, ma inoltre tratta le stringhe di data in quel formato (lo standard ISO 8601) come contenenti l'orario delle 0 ore UTC, anziché l'ora locale. Se il tuo fuso orario è a ovest rispetto al UTC, la data visualizzata, essendo locale, sarà un giorno prima della data che hai inserito. Questa è una delle varie complessità (come i nomi di famiglia composti da più parole e i libri con più autori) che non stiamo affrontando qui.

## Passi successivi

- Torna a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Procedi al prossimo sottoarticolo della parte 6: [Creare un modulo Libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form).
