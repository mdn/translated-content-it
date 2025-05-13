---
title: Creare un formulario di genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Questo sotto-articolo mostra come definiamo la nostra pagina per creare oggetti `Genre` (questo è un buon punto di partenza perché il `Genre` ha solo un campo, il suo `name`, e nessuna dipendenza). Come qualsiasi altra pagina, dobbiamo impostare route, controller e viste.

## Importare metodi di validazione e sanitizzazione

Per usare il _express-validator_ nei nostri controller dobbiamo _require_ le funzioni che vogliamo usare dal modulo `'express-validator'`.

Aprire **/controllers/genreController.js** e aggiungere la seguente linea all'inizio del file, prima di qualsiasi funzione gestore di route:

```js
const { body, validationResult } = require("express-validator");
```

> [!NOTE]
> Questa sintassi ci permette di usare `body` e `validationResult` come funzioni middleware associate, come vedrà nella sezione della route post più sotto. È equivalente a:
>
> ```js
> const validator = require("express-validator");
> const body = validator.body;
> const validationResult = validator.validationResult;
> ```

## Controller—route GET

Trovi il metodo controller esportato `genre_create_get()` e lo sostituisca con il seguente codice.
Questo rende la vista **genre_form.pug**, passando una variabile titolo.

```js
// Display Genre create form on GET.
exports.genre_create_get = (req, res, next) => {
  res.render("genre_form", { title: "Create Genre" });
};
```

Si noti che questo sostituisce il gestore asincrono segnaposto che abbiamo aggiunto in [Express Tutorial Parte 4: Route e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#genre_controller) con una funzione gestore di route express "normale".
Non abbiamo bisogno del wrapper `asyncHandler()` per questa route, perché non contiene codice che possa generare un'eccezione.

## Controller—route POST

Trovi il metodo controller esportato `genre_create_post()` e lo sostituisca con il seguente codice.

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
L'array viene passato alla funzione router e ogni metodo viene chiamato in ordine.

> [!NOTE]
> Questo approccio è necessario, perché i validatori sono funzioni middleware.

Il primo metodo nell'array definisce un validatore del corpo (`body()`) che convalida e sanitizza il campo. Usa `trim()` per rimuovere gli spazi bianchi a inizio/fine, controlla che il campo _name_ non sia vuoto, e poi usa `escape()` per rimuovere eventuali caratteri HTML pericolosi).

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

Dopo aver specificato i validatori, creiamo una funzione middleware per estrarre eventuali errori di validazione. Usiamo `isEmpty()` per verificare se ci sono errori nel risultato della validazione. Se ci sono, rendiamo di nuovo il formulario, passando nel nostro oggetto genre sanitizzato e l'array di messaggi di errore (`errors.array()`).

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

Se i dati del nome del genere sono validi, eseguiamo una ricerca insensibile al maiuscolo/minuscolo per vedere se esiste già un `Genre` con lo stesso nome (poiché non vogliamo creare record duplicati o quasi duplicati che variano solo in lettera, come: "Fantasy", "fantasy", "FaNtAsY", e così via).
Per ignorare maiuscole e accenti durante la ricerca colleghiamo il metodo [`collation()`](<https://mongoosejs.com/docs/api/query.html#Query.prototype.collation()>) specificando la locale 'en' e la forza di 2 (per ulteriori informazioni vedere l'argomento [Collation di MongoDB](https://www.mongodb.com/docs/manual/reference/collation/)).

Se un `Genre` con un nome corrispondente esiste già, reindirizziamo alla sua pagina di dettaglio.
In caso contrario, salviamo il nuovo `Genre` e reindirizziamo alla sua pagina di dettaglio.
Noti che qui `await` sul risultato della query di database, seguendo lo stesso schema delle altre funzioni gestore di route.

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

Questo stesso schema è utilizzato in tutti i nostri controller post: eseguiamo i validatori (con sanitizzatori), poi controlliamo gli errori e rendiamo di nuovo il formulario con le informazioni sull'errore o salviamo i dati.

## Vista

La stessa vista è resa sia nei controller/route `GET` che `POST` quando creiamo un nuovo `Genre` (e più avanti è anche utilizzata quando _aggiorniamo_ un `Genre`). Nel caso `GET` il formulario è vuoto e passiamo solo una variabile titolo. Nel caso `POST` l'utente ha precedentemente inserito dati non validi - nella variabile `genre` passiamo una versione sanitizzata dei dati inseriti e nella variabile `errors` passiamo un array di messaggi di errore.
Il codice di seguito mostra il codice del controller per il rendering del template in entrambi i casi.

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

Creare **/views/genre_form.pug** e copiare il testo seguente.

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

Gran parte di questo template sarà familiare dai nostri tutorial precedenti. Prima, estendiamo il template base **layout.pug** e sovrascriviamo il `blocco` chiamato '**content**'. Abbiamo poi un'intestazione con il `title` che abbiamo passato dal controller (tramite il metodo `render()`).

Successivamente, abbiamo il codice pug per il nostro formulario HTML che usa `method="POST"` per inviare i dati al server e poiché l'`action` è una stringa vuota, invierà i dati allo stesso URL della pagina.

Il formulario definisce un singolo campo obbligatorio di tipo "text" chiamato "name". Il _valore_ di default del campo dipende se il variabile `genre` è definito. Se chiamato dalla route `GET` sarà vuoto poiché si tratta di un nuovo formulario. Se chiamato da una route `POST` conterrà il valore (non valido) originariamente inserito dall'utente.

L'ultima parte della pagina è il codice di errore. Questo stampa una lista di errori, se la variabile di errore è stata definita (in altre parole, questa sezione non apparirà quando il template viene reso sulla route `GET`).

> [!NOTE]
> Questo è solo un modo per rendere gli errori. Può anche ottenere i nomi dei campi interessati dalla variabile di errore e usarli per controllare dove vengono resi i messaggi di errore, se applicare CSS personalizzato, ecc.

## Che aspetto ha?

Esegua l'applicazione, apra il suo browser in `http://localhost:3000/`, quindi selezioni il link _Create new genre_. Se tutto è impostato correttamente, dovrebbe vedere il suo sito apparire simile al seguente screenshot. Dopo aver inserito un valore, dovrebbe essere salvato e verrà portato alla pagina di dettaglio del genere.

![Pagina di Creazione Genere - Sito Express Local Library](locallibary_express_genre_create_empty.png)

L'unico errore che convalidiamo lato server è che il campo genere deve avere almeno tre caratteri. Lo screenshot qui sotto mostra come apparirebbe la lista di errori se fornisce un genere con solo uno o due caratteri (evidenziato in giallo).

![La sezione Crea Genere dell'applicazione Local library. La colonna di sinistra ha una barra di navigazione verticale. La sezione di destra è per creare un nuovo Genere con un'intestazione che dice 'Crea Genere'. C'è un campo di input etichettato 'Genre'. C'è un pulsante di invio in basso. C'è un messaggio di errore che dice 'Nome del genere richiesto' direttamente sotto il pulsante di invio. Il messaggio di errore è stato evidenziato dall'autore di questo articolo. Non c'è nessuna indicazione visiva nel formulario che il genere sia richiesto né che il messaggio di errore appaia solo in caso di errore.](locallibary_express_genre_create_error.png)

> [!NOTE]
> La nostra validazione utilizza `trim()` per garantire che gli spazi bianchi non siano accettati come nome di genere. Convalidiamo anche che il campo non sia vuoto lato client aggiungendo l'{{Glossary("Boolean/HTML", "attributo booleano")}} `required` alla definizione del campo nel formulario:
>
> ```pug
> input#name.form-control(type='text', placeholder='Fantasy, Poetry etc.' name='name' required value=(undefined===genre ? '' : genre.name) )
> ```

## Prossimi passi

1. Torni a [Express Tutorial Parte 6: Lavorare con i formulari.](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms)
2. Passi al prossimo sotto-articolo della parte 6: [Creare il formulario Autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form).
