---
title: Crea Modulo Genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form
l10n:
  sourceCommit: f2dc3d5367203c860cf1a71ce0e972f018523849
---

Questo sottoarticolo mostra come definire la nostra pagina per creare oggetti `Genre` (questo è un buon punto di partenza perché il `Genre` ha solo un campo, il suo `name`, e nessuna dipendenza). Come qualsiasi altra pagina, dobbiamo impostare rotte, controller e viste.

## Importa metodi di validazione e sanificazione

Per utilizzare _express-validator_ nei nostri controller dobbiamo _richiedere_ le funzioni che vogliamo usare dal modulo `'express-validator'`.

Apri **/controllers/genreController.js** e aggiungi la seguente riga all'inizio del file, prima di qualsiasi funzione gestore di rotta:

```js
const { body, validationResult } = require("express-validator");
```

Nota che `require("express-validator")` è semplicemente una chiamata di funzione che restituisce un oggetto, e noi [distruggiamo](/it/docs/Web/JavaScript/Reference/Operators/Destructuring) le due proprietà, `body` e `validationResult`, dall'oggetto, così possiamo usarle come variabili direttamente.

## Controller—get route

Trova il metodo del controller esportato `genre_create_get()` e sostituiscilo con il seguente codice. Questo rende la vista **genre_form.pug**, passando una variabile titolo.

```js
// Display Genre create form on GET.
exports.genre_create_get = (req, res, next) => {
  res.render("genre_form", { title: "Create Genre" });
};
```

Nota che questo sostituisce il gestore asincrono di placeholder che abbiamo aggiunto in [Express Tutorial Part 4: Routes and controllers](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#genre_controller) con una funzione di gestore di rotta "normale" di express. Non abbiamo bisogno del wrapper `asyncHandler()` per questa rotta, perché non contiene codice che può generare un'eccezione.

## Controller—post route

Trova il metodo del controller esportato `genre_create_post()` e sostituiscilo con il seguente codice.

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

La prima cosa da notare è che invece di essere una singola funzione middleware (con argomenti `(req, res, next)`) il controller specifica un _array_ di funzioni middleware. L'array viene passato alla funzione del router e ogni metodo viene chiamato in ordine.

> [!NOTE]
> Questo approccio è necessario, perché i validator sono funzioni middleware.

Il primo metodo nell'array definisce un validatore del corpo (`body()`) che valida e sanifica il campo. Questo usa `trim()` per rimuovere qualsiasi spazio bianco finale/iniziale, verifica che il campo _name_ non sia vuoto, e poi usa `escape()` per rimuovere eventuali caratteri HTML pericolosi.

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

Dopo aver specificato i validator, creiamo una funzione middleware per estrarre eventuali errori di validazione. Usiamo `isEmpty()` per controllare se ci sono errori nel risultato della validazione. Se ci sono, rendiamo nuovamente il modulo, passando il nostro oggetto genere sanificato e l'array di messaggi di errore (`errors.array()`).

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
  }
  // Data from form is valid.
  // …
});
```

Se i dati del nome del genere sono validi, eseguiamo una ricerca senza distinzione di maiuscole e minuscole per vedere se un `Genre` con lo stesso nome esiste già (poiché non vogliamo creare record duplicati o quasi duplicati che differiscono solo per il caso delle lettere, come: "Fantasy", "fantasy", "FaNtAsY", e così via). Per ignorare il caso delle lettere e gli accenti durante la ricerca, concatenamo il metodo [`collation()`](<https://mongoosejs.com/docs/api/query.html#Query.prototype.collation()>), specificando la locale 'en' e una forza di 2 (per ulteriori informazioni vedere l'argomento [Collation](https://www.mongodb.com/docs/manual/reference/collation/) di MongoDB).

Se esiste già un `Genre` con un nome corrispondente, reindirizziamo alla sua pagina di dettaglio. In caso contrario, salviamo il nuovo `Genre` e reindirizziamo alla sua pagina di dettaglio. Nota che qui `await` attende il risultato della query del database, seguendo lo stesso schema degli altri gestori di rotta.

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

Questo stesso schema viene utilizzato in tutti i nostri controller post: eseguiamo i validator (con i sanificatori), quindi controlliamo gli errori e rendiamo nuovamente il modulo con le informazioni sugli errori o salviamo i dati.

## View

La stessa vista è resa in entrambi i controller/rotture `GET` e `POST` quando creiamo un nuovo `Genre` (e in seguito viene utilizzata anche quando _aggiorniamo_ un `Genre`). Nel caso di `GET`, il modulo è vuoto e passiamo solo una variabile di titolo. Nel caso di `POST`, l'utente ha precedentemente inserito dati non validi—nella variabile `genre` facciamo tornare una versione sanificata dei dati inseriti e nella variabile `errors` facciamo tornare un array di messaggi di errore. Il codice qui sotto mostra il codice del controller per rendere il template in entrambi i casi.

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

Crea **/views/genre_form.pug** e copia il testo seguente.

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

Gran parte di questo template sarà familiare dai nostri tutorial precedenti. Prima, estendiamo il template base **layout.pug** e sostituiamo il `block` denominato '**content**'. Abbiamo quindi un'intestazione con il `title` che abbiamo passato dal controller (tramite il metodo `render()`).

Successivamente, abbiamo il codice pug per il nostro modulo HTML che utilizza `method="POST"` per inviare i dati al server, e poiché l' `action` è una stringa vuota, invierà i dati allo stesso URL della pagina.

Il modulo definisce un unico campo richiesto di tipo "text" chiamato "name". Il valore predefinito del campo dipende dal fatto che la variabile `genre` sia definita. Se chiamato dalla rotta `GET`, sarà vuoto poiché questo è un nuovo modulo. Se chiamato da una rotta `POST`, conterrà il valore (non valido) inserito originariamente dall'utente.

L'ultima parte della pagina è il codice di errore. Questo stampa un elenco di errori, se la variabile error è stata definita (in altre parole, questa sezione non apparirà quando il template è reso sulla rotta `GET`).

> [!NOTE]
> Questo è solo uno dei modi per rendere gli errori. Puoi anche ottenere i nomi dei campi interessati dalla variabile error, e usare questi per controllare dove vengono resi i messaggi di errore, se applicare CSS personalizzati, ecc.

## Come appare?

Esegui l'applicazione, apri il browser su `http://localhost:3000/`, poi seleziona il link _Crea nuovo genere_. Se tutto è configurato correttamente, il tuo sito dovrebbe assomigliare a quanto mostrato nello screenshot successivo. Dopo che inserisci un valore, dovrebbe essere salvato e verrai portato alla pagina di dettaglio del genere.

![Pagina Creazione Genere - sito Express Local Library](locallibary_express_genre_create_empty.png)

L'unico errore che validiamo lato server è che il campo del genere deve avere almeno tre caratteri. Lo screenshot qui sotto mostra come apparirebbe l'elenco degli errori se fornisci un genere con solo uno o due caratteri (evidenziati in giallo).

![La sezione Crea Genere dell'applicazione Local library. La colonna di sinistra ha una barra di navigazione verticale. La sezione di destra è il modulo di creazione di un nuovo Genere con un'intestazione che dice 'Crea Genere'. C'è un campo di input etichettato 'Genere'. C'è un pulsante di invio in basso. C'è un messaggio di errore che dice 'Nome del genere richiesto' direttamente sotto il pulsante di invio. Il messaggio di errore è stato evidenziato dall'autore di questo articolo. Non c'è indicazione visiva nel modulo che il genere è richiesto né che il messaggio di errore appare solo in caso di errore.](locallibary_express_genre_create_error.png)

> [!NOTE]
> La nostra validazione utilizza `trim()` per assicurarsi che lo spazio bianco non sia accettato come nome del genere. Convalidiamo anche che il campo non sia vuoto sul lato client aggiungendo l' {{Glossary("Boolean/HTML", "attributo booleano")}} `required` alla definizione del campo nel modulo:
>
> ```pug
> input#name.form-control(type='text', placeholder='Fantasy, Poetry etc.' name='name' required value=(undefined===genre ? '' : genre.name) )
> ```

## Prossimi passaggi

1. Torna a [Express Tutorial Parte 6: Lavorare con i moduli.](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms)
2. Procedi al prossimo sottoarticolo della parte 6: [Crea modulo Autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form).
