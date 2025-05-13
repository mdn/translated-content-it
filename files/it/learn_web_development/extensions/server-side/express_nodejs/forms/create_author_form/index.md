---
title: Creare un modulo Autore
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo sottoarticolo mostra come definire una pagina per creare oggetti `Author`.

## Importa metodi di validazione e sanificazione

Come con il [modulo per il genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form), per utilizzare _express-validator_ dobbiamo _richiedere_ le funzioni che vogliamo usare.

Aprire **/controllers/authorController.js** e aggiungere la seguente riga all'inizio del file (sopra le funzioni di route):

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

Trova il metodo del controller esportato `author_create_post()` e sostituiscilo con il seguente codice.

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
        author: author,
        errors: errors.array(),
      });
      return;
    } else {
      // Data from form is valid.

      // Save author.
      await author.save();
      // Redirect to new author record.
      res.redirect(author.url);
    }
  }),
];
```

> [!WARNING]
> Non validare mai i _nomi_ utilizzando `isAlphanumeric()` (come abbiamo fatto sopra) poiché ci sono molti nomi che usano altri set di caratteri.
> Lo facciamo qui per dimostrare come si usa il validatore e come può essere concatenato con altri validatori e segnalazione di errori.

La struttura e il comportamento di questo codice sono quasi esattamente gli stessi della creazione di un oggetto `Genre`. Prima validiamo e sanifichiamo i dati. Se i dati non sono validi, ripresentiamo il modulo insieme ai dati che sono stati originariamente inseriti dall'utente e a un elenco di messaggi di errore. Se i dati sono validi, salviamo il nuovo record autore e reindirizziamo l'utente alla pagina di dettaglio dell'autore.

Diversamente dal gestore POST per `Genre`, non controlliamo se l'oggetto `Author` esiste già prima di salvarlo. Potremmo teoricamente farlo, anche se così com'è ora possiamo avere più autori con lo stesso nome.

Il codice di validazione dimostra diverse nuove funzionalità:

- Possiamo concatenare i validator, utilizzando `withMessage()` per specificare il messaggio di errore da visualizzare se il metodo di validazione precedente fallisce.
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

- Possiamo usare la funzione `optional()` per eseguire una convalida successiva solo se un campo è stato inserito (questo ci permette di validare campi opzionali).
  Per esempio, di seguito controlliamo che la data opzionale di nascita sia una data conforme a ISO8601 (l'oggetto `{ values: "falsy" }` passato significa che accetteremo sia una stringa vuota che `null` come valore vuoto).

  ```js
  [
    body("date_of_birth", "Invalid date of birth")
      .optional({ values: "falsy" })
      .isISO8601()
      .toDate(),
  ];
  ```

- I parametri vengono ricevuti dalla richiesta come stringhe. Possiamo usare `toDate()` (o `toBoolean()`) per convertirli nei tipi JavaScript appropriati (come mostrato alla fine della catena del validatore sopra).

## Vista

Crea **/views/author_form.pug** e copia il testo qui sotto.

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
> Alcuni browser non supportano l'input `type="date"`, quindi non si otterrà il widget di selezione della data o il segnaposto predefinito `gg/mm/aaaa`, ma si otterrà invece un semplice campo di testo vuoto. Una soluzione è aggiungere esplicitamente l'attributo `placeholder='gg/mm/aaaa'` in modo che sui browser meno capaci si avrà comunque l'informazione sul formato del testo desiderato.

### Sfida: Aggiungere la data di morte

Il modello sopra manca di un campo per inserire il `date_of_death`. Creare il campo seguendo lo stesso schema del gruppo di moduli per la data di nascita!

## Come appare?

Esegui l'applicazione, apri il browser su `http://localhost:3000/`, quindi seleziona il link _Create new author_. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire simile allo screenshot seguente. Dopo aver inserito un valore, dovrebbe essere salvato e sarai indirizzato alla pagina di dettaglio dell'autore.

![Pagina Creazione Autore - Sito Express Local Library](locallibary_express_author_create_empty.png)

> [!NOTE]
> Se si esperimentano vari formati di input per le date, si può scoprire che il formato `aaaa-mm-gg` si comporta in modo anomalo. Ciò è dovuto al fatto che JavaScript tratta le stringhe di data come inclusi il tempo di 0 ore, ma inoltre tratta le stringhe di data in quel formato (lo standard ISO 8601) come incluse il tempo di 0 ore UTC, piuttosto che l'ora locale. Se il tuo fuso orario è a ovest di UTC, la visualizzazione della data, essendo locale, sarà un giorno prima della data inserita. Questa è una delle diverse complessità (come cognomi plurinominali e libri con più autori) che non stiamo affrontando qui.

## Prossimi passi

- Torna a [Tutorial Express Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Procedi al successivo sottoarticolo della parte 6: [Modulo di creazione libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form).
