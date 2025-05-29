---
title: Creare modulo genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form
l10n:
  sourceCommit: 2c0f972d873ea2db5163dbcb12987847124751ad
---

Questo sott’articolo mostra come definire la nostra pagina per creare oggetti `Genre` (questo è un buon punto di partenza poiché il `Genre` ha solo un campo, il suo `name`, e nessuna dipendenza). Come qualsiasi altra pagina, dobbiamo impostare rotte, controller e viste.

## Importare metodi di validazione e sanificazione

Per utilizzare l'_express-validator_ nei nostri controller dobbiamo _richiedere_ le funzioni che vogliamo utilizzare dal modulo `'express-validator'`.

Aprire **/controllers/genreController.js** e aggiungere la seguente riga all'inizio del file, prima di qualsiasi funzione gestore di route:

```js
const { body, validationResult } = require("express-validator");
```

Nota che `require("express-validator")` è solo una chiamata a funzione che restituisce un oggetto, e noi [destrutturiamo](/it/docs/Web/JavaScript/Reference/Operators/Destructuring) le due proprietà, `body` e `validationResult`, dall'oggetto, in modo da poterle utilizzare come variabili direttamente.

## Controller—route get

Trova il metodo controller esportato `genre_create_get()` e sostituiscilo con il seguente codice.
Questo rende la vista **genre_form.pug**, passando una variabile `title`.

```js
// Display Genre create form on GET.
exports.genre_create_get = (req, res, next) => {
  res.render("genre_form", { title: "Create Genre" });
};
```

Nota che questo sostituisce il gestore asincrono segnaposto che abbiamo aggiunto nell'[Express Tutorial Parte 4: Rotte e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#genre_controller) con una funzione gestore di route "normale" di express.
Non abbiamo bisogno del wrapper `asyncHandler()` per questa route, perché non contiene codice che può generare un'eccezione.

## Controller—route post

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
        genre,
        errors: errors.array(),
      });
      return;
    }
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
  }),
];
```

La prima cosa da notare è che invece di essere una singola funzione middleware (con argomenti `(req, res, next)`), il controller specifica un _array_ di funzioni middleware.
L'array viene passato alla funzione del router e ciascun metodo viene chiamato in ordine.

> [!NOTE]
> Questo approccio è necessario, perché i validatori sono funzioni middleware.

Il primo metodo nell'array definisce un body validator (`body()`) che valida e sanifica il campo. Questo usa `trim()` per rimuovere qualsiasi spazio vuoto alla fine/inizio, verifica che il campo _name_ non sia vuoto e quindi usa `escape()` per rimuovere qualsiasi carattere HTML pericoloso).

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

Dopo aver specificato i validatori creiamo una funzione middleware per estrarre eventuali errori di validazione. Usiamo `isEmpty()` per controllare se esistono errori nel risultato della validazione. Se ci sono, rendiamo nuovamente il modulo, passando il nostro oggetto genere sanificato e l'array di messaggi di errore (`errors.array()`).

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
      genre,
      errors: errors.array(),
    });
    return;
  }
  // Data from form is valid.
  // …
});
```

Se i dati del nome del genere sono validi, eseguiamo una ricerca senza distinzione tra maiuscole e minuscole per vedere se esiste già un `Genre` con lo stesso nome (poiché non vogliamo creare record duplicati o quasi duplicati che variano solo per distinzione delle lettere, come: "Fantasy", "fantasy", "FaNtAsY", e così via).
Per ignorare le lettere maiuscole e gli accenti durante la ricerca, colleghiamo il metodo [`collation()`](<https://mongoosejs.com/docs/api/query.html#Query.prototype.collation()>) specificando la locale 'en' e la forza 2 (per ulteriori informazioni vedere l'argomento [Collation di MongoDB](https://www.mongodb.com/docs/manual/reference/collation/)).

Se esiste un `Genre` con un nome corrispondente, reindirizziamo alla sua pagina di dettaglio.
In caso contrario, salviamo il nuovo `Genre` e reindirizziamo alla sua pagina di dettaglio.
Nota che qui `await` sull'esito della query del database, seguendo lo stesso schema delle altre funzioni gestore di route.

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

Questo stesso schema viene usato in tutti i nostri controller post: eseguiamo i validatori (con i sanificatori), quindi controlliamo eventuali errori e ri-rendiamo il modulo con le informazioni sugli errori oppure salviamo i dati.

## Vista

La stessa vista viene resa in entrambe le route/controller `GET` e `POST` quando creiamo un nuovo `Genre` (e in seguito viene anche usata quando _aggiorniamo_ un `Genre`). Nel caso `GET` il modulo è vuoto e passiamo solo una variabile `title`. Nel caso `POST` l'utente ha precedentemente inserito dati non validi — nella variabile `genre` restituiamo una versione sanificata dei dati inseriti e nella variabile `errors` restituiamo un array di messaggi di errore.
Il codice sottostante mostra il codice del controller per rendere il template in entrambi i casi.

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

Crea **/views/genre_form.pug** e copia il testo sottostante.

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

Gran parte di questo template sarà familiare dai nostri tutorial precedenti. Innanzitutto, estendiamo il template base **layout.pug** e sovrascriviamo il `block` chiamato '**content**'. Quindi abbiamo un'intestazione con il `title` che abbiamo passato dal controller (tramite il metodo `render()`).

Successivamente, abbiamo il codice pug per il nostro modulo HTML che utilizza `method="POST"` per inviare i dati al server e, poiché l'`action` è una stringa vuota, invierà i dati allo stesso URL della pagina.

Il modulo definisce un singolo campo obbligatorio di tipo "text" chiamato "name". Il _valore_ predefinito del campo dipende dal fatto se la variabile `genre` sia definita. Se chiamato dalla route `GET` sarà vuoto poiché questo è un nuovo modulo. Se chiamato da una route `POST` conterrà il valore (non valido) originariamente inserito dall'utente.

L'ultima parte della pagina è il codice per gli errori. Questo stampa un elenco di errori, se la variabile error è stata definita (in altre parole, questa sezione non apparirà quando il template è reso nella route `GET`).

> [!NOTE]
> Questo è solo un modo per rendere gli errori. Puoi anche ottenere i nomi dei campi interessati dalla variabile error e usarli per controllare dove vengono resi i messaggi di errore, se applicare CSS personalizzato, ecc.

## Che aspetto ha?

Esegui l'applicazione, apri il browser su `http://localhost:3000/`, quindi seleziona il link _Create new genre_. Se tutto è configurato correttamente, il tuo sito dovrebbe apparire come nell'immagine seguente. Dopo aver inserito un valore, dovrebbe essere salvato e verrai portato alla pagina di dettaglio del genere.

![Pagina di creazione del genere - Sito della biblioteca locale Express](locallibary_express_genre_create_empty.png)

L'unico errore che convalidiamo lato server è che il campo del genere deve avere almeno tre caratteri. Lo screenshot sottostante mostra come apparirebbe l'elenco degli errori se fornisci un genere con uno o due soli caratteri (evidenziati in giallo).

![La sezione Crea Genere dell'applicazione Biblioteca Locale. La colonna a sinistra ha una barra di navigazione verticale. La sezione a destra è il modulo per creare un nuovo Genere con un'intestazione che recita 'Crea Genere'. C'è un campo di input etichettato 'Genere'. C'è un pulsante di invio in fondo. C'è un messaggio di errore che recita 'Nome del Genere richiesto' direttamente sotto il pulsante di invio. Il messaggio di errore è stato evidenziato dall'autore di questo articolo. Non c'è alcuna indicazione visiva nel modulo che il genere sia richiesto né che il messaggio di errore appaia solo in caso di errore.](locallibary_express_genre_create_error.png)

> [!NOTE]
> La nostra validazione utilizza `trim()` per assicurarsi che gli spazi bianchi non siano accettati come nome del genere. Convalidiamo anche che il campo non sia vuoto lato client aggiungendo l'{{Glossary("Boolean/HTML", "attributo booleano")}} `required` alla definizione del campo nel modulo:
>
> ```pug
> input#name.form-control(type='text', placeholder='Fantasy, Poetry etc.' name='name' required value=(undefined===genre ? '' : genre.name) )
> ```

## Passi successivi

1. Torna a [Express Tutorial Parte 6: Lavorare con i moduli](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms).
2. Procedi al prossimo sott'articolo della parte 6: [Creare modulo autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form).
