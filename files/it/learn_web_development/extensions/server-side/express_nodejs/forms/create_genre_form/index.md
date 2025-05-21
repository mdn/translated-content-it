---
title: Creare il form per il genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo sotto-articolo mostra come definiamo la nostra pagina per creare oggetti `Genre` (questo è un buon punto di partenza perché il `Genre` ha solo un campo, il suo `name`, e nessuna dipendenza). Come per qualsiasi altra pagina, dobbiamo impostare rotte, controller e viste.

## Importare metodi di validazione e sanificazione

Per utilizzare _express-validator_ nei nostri controller, dobbiamo _require_ le funzioni che vogliamo usare dal modulo `'express-validator'`.

Apri **/controllers/genreController.js** e aggiungi la seguente riga all'inizio del file, prima di qualsiasi funzione di gestione delle rotte:

```js
const { body, validationResult } = require("express-validator");
```

> [!NOTE]
> Questa sintassi ci consente di usare `body` e `validationResult` come le funzioni middleware associate, come vedrai nella sezione della rotta post qui sotto. È equivalente a:
>
> ```js
> const validator = require("express-validator");
> const body = validator.body;
> const validationResult = validator.validationResult;
> ```

## Controller—rotta GET

Trova il metodo controller esportato `genre_create_get()` e sostituiscilo con il seguente codice. Questo renderizza la vista **genre_form.pug**, passando una variabile title.

```js
// Display Genre create form on GET.
exports.genre_create_get = (req, res, next) => {
  res.render("genre_form", { title: "Create Genre" });
};
```

Nota che questo sostituisce il gestore asincrono segnaposto che abbiamo aggiunto nella [Guida Express Parte 4: Route e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#genre_controller) con una normale funzione gestore di route Express.
Non abbiamo bisogno del wrapper `asyncHandler()` per questa rotta, perché non contiene codice che può generare eccezioni.

## Controller—rotta POST

Trova il metodo controller esportato `genre_create_post()` e sostituiscilo con il seguente codice.

```js
// Handle Genre create on POST.
exports.genre_create_post = [
  // Validate and sanitize the name field.
  body("name", "Genre name must contain at least 3 characters")
    .trim()
    .isLength({ min: 3 })
    .escape(),

  // Process request after validation and sanitization.
  asyncHandler(async (req, res, next) => {
    // Extract the validation errors from a request.
    const errors = validationResult(req);

    // Create a genre object with escaped and trimmed data.
    const genre = new Genre({ name: req.body.name });

    if (!errors.isEmpty()) {
      // There are errors. Render the form again with sanitized values/error messages.
      res.render("genre_form", {
        title: "Create Genre",
        genre: genre,
        errors: errors.array(),
      });
      return;
    } else {
      // Data from form is valid.
      // Check if Genre with same name already exists.
      const genreExists = await Genre.findOne({ name: req.body.name })
        .collation({ locale: "en", strength: 2 })
        .exec();
      if (genreExists) {
        // Genre exists, redirect to its detail page.
        res.redirect(genreExists.url);
      } else {
        await genre.save();
        // New genre saved. Redirect to genre detail page.
        res.redirect(genre.url);
      }
    }
  }),
];
```

La prima cosa da notare è che invece di essere una singola funzione middleware (con argomenti `(req, res, next)`) il controller specifica un _array_ di funzioni middleware.
L'array viene passato alla funzione router e ciascun metodo viene chiamato in ordine.

> [!NOTE]
> Questo approccio è necessario perché i validatori sono funzioni middleware.

Il primo metodo nell'array definisce un validatore del corpo (`body()`) che valida e sanifica il campo. Questo usa `trim()` per rimuovere spazi vuoti finali/iniziali, controlla che il campo _name_ non sia vuoto, e poi usa `escape()` per rimuovere eventuali caratteri HTML pericolosi).

```js
[
  // Validate that the name field is not empty.
  body("name", "Genre name must contain at least 3 characters")
    .trim()
    .isLength({ min: 3 })
    .escape(),
  // …
];
```

Dopo aver specificato i validatori, creiamo una funzione middleware per estrarre eventuali errori di validazione. Usiamo `isEmpty()` per verificare se ci sono errori nel risultato della validazione. Se ci sono, rendiamo nuovamente il form, passando il nostro oggetto genere sanificato e l'array di messaggi di errore (`errors.array()`).

```js
// Process request after validation and sanitization.
asyncHandler(async (req, res, next) => {
  // Extract the validation errors from a request.
  const errors = validationResult(req);

  // Create a genre object with escaped and trimmed data.
  const genre = new Genre({ name: req.body.name });

  if (!errors.isEmpty()) {
    // There are errors. Render the form again with sanitized values/error messages.
    res.render("genre_form", {
      title: "Create Genre",
      genre: genre,
      errors: errors.array(),
    });
    return;
  } else {
    // Data from form is valid.
    // …
  }
});
```

Se i dati del nome del genere sono validi, eseguiamo una ricerca senza distinzione di maiuscole per vedere se un `Genre` con lo stesso nome esiste già (poiché non vogliamo creare record duplicati o quasi duplicati che variano solo nel caso delle lettere, come: "Fantasy", "fantasy", "FaNtAsY", e così via).
Per ignorare le maiuscole e gli accenti durante la ricerca, concatenamo il metodo [`collation()`](<https://mongoosejs.com/docs/api/query.html#Query.prototype.collation()>) specificando la locale 'en' e il strength di 2 (per ulteriori informazioni vedere l'argomento [Collation](https://www.mongodb.com/docs/manual/reference/collation/) di MongoDB).

Se un `Genre` con un nome corrispondente esiste già, reindirizziamo alla sua pagina dei dettagli.
Altrimenti, salviamo il nuovo `Genre` e reindirizziamo alla sua pagina dei dettagli.
Nota che qui `await`iamo sul risultato della query del database, seguendo lo stesso schema degli altri gestori di route.

```js
// Check if Genre with same name already exists.
const genreExists = await Genre.findOne({ name: req.body.name })
  .collation({ locale: "en", strength: 2 })
  .exec();
if (genreExists) {
  // Genre exists, redirect to its detail page.
  res.redirect(genreExists.url);
} else {
  await genre.save();
  // New genre saved. Redirect to genre detail page.
  res.redirect(genre.url);
}
```

Questo stesso schema viene utilizzato in tutti i nostri controller post: eseguiamo i validatori (con i sanificatori), quindi controlliamo per errori e rendiamo di nuovo il modulo con le informazioni sugli errori o salviamo i dati.

## Vista

La stessa vista viene resa sia nei controller/route `GET` che `POST` quando creiamo un nuovo `Genre` (e in seguito viene anche usata quando _aggiorniamo_ un `Genre`). Nel caso `GET` il modulo è vuoto, e passiamo solo una variabile title. Nel caso `POST` l'utente ha precedentemente inserito dati non validi—nella variabile `genre` rimandiamo una versione sanificata dei dati inseriti e nella variabile `errors` rimandiamo un array di messaggi di errore.
Il codice qui sotto mostra il codice del controller per rendere il template in entrambi i casi.

```js
// Render the GET route
res.render("genre_form", { title: "Create Genre" });

// Render the POST route
res.render("genre_form", {
  title: "Create Genre",
  genre,
  errors: errors.array(),
});
```

Crea **/views/genre_form.pug** e copia il testo qui sotto.

```pug
extends layout

block content

  h1 #{title}

  form(method='POST')
    div.form-group
      label(for='name') Genre:
      input#name.form-control(type='text', placeholder='Fantasy, Poetry etc.' name='name' required value=(undefined===genre ? '' : genre.name) )
    button.btn.btn-primary(type='submit') Submit

  if errors
    ul
      for error in errors
        li!= error.msg
```

Molto di questo template sarà familiare dai nostri tutorial precedenti. Innanzitutto, estendiamo il template base **layout.pug** e sovrascriviamo il `block` chiamato '**content**'. Poi abbiamo un'intestazione con il `title` che abbiamo passato dal controller (tramite il metodo `render()`).

Successivamente, abbiamo il codice pug per il nostro form HTML che utilizza `method="POST"` per inviare i dati al server e, poiché l'`action` è una stringa vuota, invierà i dati allo stesso URL della pagina.

Il form definisce un singolo campo obbligatorio di tipo "text" chiamato "name". Il _value_ predefinito del campo dipende dal fatto che la variabile `genre` sia definita. Se chiamato dalla rotta `GET` sarà vuoto poiché questo è un nuovo form. Se chiamato da una rotta `POST` conterrà il valore (non valido) inizialmente inserito dall'utente.

L'ultima parte della pagina è il codice degli errori. Questo stampa una lista di errori, se la variabile error è stata definita (in altre parole, questa sezione non apparirà quando il template viene renderizzato sulla rotta `GET`).

> [!NOTE]
> Questo è solo un modo per rendere gli errori. È anche possibile ottenere i nomi dei campi interessati dalla variabile error, e usarli per controllare dove vengono resi i messaggi di errore, se applicare CSS personalizzati, ecc.

## Come appare?

Esegui l'applicazione, apri il tuo browser su `http://localhost:3000/`, poi seleziona il link _Crea nuovo genere_. Se tutto è configurato correttamente, il tuo sito dovrebbe assomigliare allo screenshot seguente. Dopo aver inserito un valore, dovrebbe essere salvato e sarai portato alla pagina dei dettagli del genere.

![Pagina di creazione del genere - Sito Express Biblioteca Locale](locallibary_express_genre_create_empty.png)

L'unico errore che convalidiamo lato server è che il campo genre deve avere almeno tre caratteri. Lo screenshot qui sotto mostra come apparirebbe la lista degli errori se fornissi un genere con solo uno o due caratteri (evidenziato in giallo).

![La sezione Crea Genere dell'applicazione Biblioteca Locale. La colonna sinistra ha una barra di navigazione verticale. La sezione destra è il modulo per creare un nuovo Genere con un'intestazione che legge 'Crea Genere'. C'è un campo di input etichettato 'Genere'. C'è un pulsante di invio in fondo. C'è un messaggio di errore che dice 'Nome genere richiesto' direttamente sotto il pulsante di invio. Il messaggio di errore è stato evidenziato dall'autore di questo articolo. Non c'è indicazione visiva nel modulo che il genere sia richiesto né che il messaggio di errore appaia solo in caso di errore.](locallibary_express_genre_create_error.png)

> [!NOTE]
> La nostra convalida utilizza `trim()` per garantire che gli spazi bianchi non siano accettati come nome del genere. Convalidiamo anche che il campo non sia vuoto lato client aggiungendo l'{{Glossary("Boolean/HTML", "attributo booleano")}} `required` alla definizione del campo nel modulo:
>
> ```pug
> input#name.form-control(type='text', placeholder='Fantasy, Poetry etc.' name='name' required value=(undefined===genre ? '' : genre.name) )
> ```

## Prossimi passi

1. Ritorna alla [Guida Express Parte 6: Lavorare con i moduli.](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms)
2. Procedi al prossimo sotto-articolo della parte 6: [Crea modulo Autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form).
