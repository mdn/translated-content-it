---
title: Modulo Create Author
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

Questo sottoarticolo mostra come definire una pagina per creare oggetti `Author`.

## Importare i metodi di validazione e sanitizzazione

Come per il [modulo del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form), per utilizzare _express-validator_ è necessario effettuare il _require_ delle funzioni che si desidera usare.

Aprire **/controllers/authorController.js** e aggiungere la seguente riga all'inizio del file (sopra le funzioni di route):

```js
const { body, validationResult } = require("express-validator");
```

## Controller—route get

Trovare il metodo controller esportato `author_create_get()` e sostituirlo con il codice seguente. Questo esegue il rendering della view **author_form.pug**, passando una variabile `title`.

```js
// Display Author create form on GET.
exports.author_create_get = (req, res, next) => {
  res.render("author_form", { title: "Create Author" });
};
```

## Controller—route post

Trovare il metodo controller esportato `author_create_post()` e sostituirlo con il codice seguente.

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
  async (req, res, next) => {
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
    // Save and redirect to new author record.
    await author.save();
    res.redirect(author.url);
  },
];
```

> [!WARNING]
> Non validare mai i _nomi_ usando `isAlphanumeric()` (come fatto sopra), poiché molti nomi utilizzano altri set di caratteri.
> Viene fatto qui per dimostrare come si usa il validatore e come può essere concatenato con altri validatori e con la segnalazione degli errori.

La struttura e il comportamento di questo codice sono quasi esattamente gli stessi della creazione di un oggetto `Genre`. Prima vengono validati e sanitizzati i dati. Se i dati non sono validi, il modulo viene visualizzato nuovamente insieme ai dati originariamente inseriti dall'utente e a un elenco di messaggi di errore. Se i dati sono validi, viene salvato il nuovo record dell'autore e l'utente viene reindirizzato alla pagina dei dettagli dell'autore.

A differenza del gestore post di `Genre`, non viene verificato se l'oggetto `Author` esiste già prima di salvarlo. Sarebbe probabilmente opportuno farlo, poiché nello stato attuale possono esistere più autori con lo stesso nome.

Il codice di validazione dimostra diverse nuove funzionalità:

- È possibile concatenare i validatori, usando `withMessage()` per specificare il messaggio di errore da visualizzare se il precedente metodo di validazione fallisce.
  Questo rende molto semplice fornire messaggi di errore specifici senza molta duplicazione del codice.

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

- È possibile usare la funzione `optional()` per eseguire una validazione successiva solo se è stato inserito un campo (questo consente di validare i campi facoltativi).
  Ad esempio, di seguito viene verificato che la data di nascita facoltativa sia una data conforme a ISO8601 (l'oggetto `{ values: "falsy" }` passato indica che verranno accettati sia una stringa vuota sia `null` come valore vuoto).

  ```js
  [
    body("date_of_birth", "Invalid date of birth")
      .optional({ values: "falsy" })
      .isISO8601()
      .toDate(),
  ];
  ```

- I parametri vengono ricevuti dalla richiesta come stringhe. È possibile usare `toDate()` (o `toBoolean()`) per convertirli nei tipi JavaScript appropriati (come mostrato alla fine della catena di validatori sopra).

## View

Creare **/views/author_form.pug** e copiarvi il testo seguente.

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

La struttura e il comportamento di questa view sono esattamente gli stessi del template **genre_form.pug**, quindi non verranno descritti nuovamente.

> [!NOTE]
> Alcuni browser non supportano l'input `type="date"`, quindi non verranno visualizzati il widget datepicker né il segnaposto predefinito `dd/mm/yyyy`; verrà invece mostrato un campo di testo semplice vuoto. Una soluzione alternativa consiste nell'aggiungere esplicitamente l'attributo `placeholder='dd/mm/yyyy'`, in modo che anche nei browser con minori funzionalità siano comunque disponibili informazioni sul formato di testo desiderato.

### Sfida: aggiungere la data di morte

Nel template precedente manca un campo per inserire `date_of_death`. Creare il campo seguendo lo stesso schema del gruppo di campi del modulo per la data di nascita.

## Come appare?

Eseguire l'applicazione, aprire il browser all'indirizzo `http://localhost:3000/`, quindi selezionare il collegamento _Create new author_. Se tutto è configurato correttamente, il sito dovrebbe apparire simile allo screenshot seguente. Dopo aver inserito un valore, questo dovrebbe essere salvato e si verrà reindirizzati alla pagina dei dettagli dell'autore.

![Pagina di creazione dell'autore - sito Express Local Library](locallibary_express_author_create_empty.png)

> [!NOTE]
> Sperimentando vari formati di input per le date, si potrebbe notare che il formato `yyyy-mm-dd` si comporta in modo anomalo. Questo accade perché JavaScript tratta le stringhe di data come se includessero l'ora 0, ma tratta inoltre le stringhe di data in quel formato (lo standard ISO 8601) come se includessero l'ora 0 UTC, anziché l'ora locale. Se il fuso orario è a ovest di UTC, la visualizzazione della data, essendo locale, sarà un giorno precedente rispetto alla data inserita. Questa è una delle numerose complessità (come i cognomi composti da più parole e i libri con più autori) che non vengono affrontate qui.

## Passaggi successivi

- Tornare a [Tutorial Express Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
- Proseguire al sottoarticolo successivo della parte 6: [Modulo Create Book](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_book_form).
