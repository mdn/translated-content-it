---
title: Modulo di creazione del genere
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_genre_form
l10n:
  sourceCommit: 8443cb34d9944d8eb8e2c5add598bec26ed6d21f
---

Questo sottoarticolo mostra come definire la pagina per creare oggetti `Genre` (è un buon punto di partenza perché `Genre` ha un solo campo, `name`, e nessuna dipendenza). Come per tutte le altre pagine, è necessario configurare route, controller e viste.

## Importare i metodi di validazione e sanitizzazione

Per utilizzare _express-validator_ nei controller, è necessario effettuare il _require_ delle funzioni che si desidera usare dal modulo `'express-validator'`.

Aprire **/controllers/genreController.js** e aggiungere la seguente riga all'inizio del file, prima di qualsiasi funzione di gestione delle route:

```js
const { body, validationResult } = require("express-validator");
```

Si noti che `require("express-validator")` è solo una chiamata di funzione che restituisce un oggetto e che vengono [destrutturate](/it/docs/Web/JavaScript/Reference/Operators/Destructuring) le due proprietà, `body` e `validationResult`, dall'oggetto, in modo da poterle usare direttamente come variabili.

## Controller: route get

Trovare il metodo del controller esportato `genre_create_get()` e sostituirlo con il codice seguente.
Questo esegue il rendering della vista **genre_form.pug**, passando una variabile title.

```js
// Display Genre create form on GET.
exports.genre_create_get = (req, res, next) => {
  res.render("genre_form", { title: "Create Genre" });
};
```

## Controller: route post

Trovare il metodo del controller esportato `genre_create_post()` e sostituirlo con il codice seguente.

```js
// Handle Genre create on POST.
exports.genre_create_post = [
  // Validate and sanitize the name field.
  body("name", "Genre name must contain at least 3 characters")
    .trim()
    .isLength({ min: 3 })
    .escape(),

  // Process request after validation and sanitization.
  async (req, res, next) => {
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
      return;
    }

    // New genre. Save and redirect to its detail page.
    await genre.save();
    res.redirect(genre.url);
  },
];
```

La prima cosa da notare è che, anziché essere una singola funzione middleware (con argomenti `(req, res, next)`), il controller specifica un _array_ di funzioni middleware.
L'array viene passato alla funzione router e ogni metodo viene chiamato in ordine.

> [!NOTE]
> Questo approccio è necessario perché i validator sono funzioni middleware.

Il primo metodo nell'array definisce un validator per il body (`body()`) che valida e sanitizza il campo. Usa `trim()` per rimuovere eventuali spazi iniziali/finali, verifica che il campo _name_ non sia vuoto e quindi usa `escape()` per rimuovere eventuali caratteri HTML pericolosi).

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

Dopo aver specificato i validator, si crea una funzione middleware per estrarre eventuali errori di validazione. Si usa `isEmpty()` per verificare se sono presenti errori nel risultato della validazione. In tal caso, viene nuovamente eseguito il rendering del modulo, passando l'oggetto genere sanitizzato e l'array dei messaggi di errore (`errors.array()`).

```js
// Process request after validation and sanitization.
async (req, res, next) => {
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
};
```

Se i dati del nome del genere sono validi, viene eseguita una ricerca senza distinzione tra maiuscole e minuscole per verificare se esiste già un `Genre` con lo stesso nome (non si desidera creare record duplicati o quasi duplicati che differiscono solo per le maiuscole/minuscole, come "Fantasy", "fantasy", "FaNtAsY" e così via).
Per ignorare maiuscole/minuscole e accenti durante la ricerca, viene concatenato il metodo [`collation()`](<https://mongoosejs.com/docs/api/query.html#Query.prototype.collation()>), specificando la locale 'en' e strength pari a 2 (per ulteriori informazioni, vedere l'argomento MongoDB [Collation](https://www.mongodb.com/docs/manual/reference/collation/)).

Se esiste già un `Genre` con un nome corrispondente, viene eseguito un reindirizzamento alla relativa pagina di dettaglio.
In caso contrario, viene salvato il nuovo `Genre` e viene eseguito un reindirizzamento alla relativa pagina di dettaglio.
Si noti che qui si usa `await` sul risultato della query al database, seguendo lo stesso schema adottato negli altri gestori di route.

```js
// Check if Genre with same name already exists.
const genreExists = await Genre.findOne({ name: req.body.name })
  .collation({ locale: "en", strength: 2 })
  .exec();
if (genreExists) {
  // Genre exists, redirect to its detail page.
  res.redirect(genreExists.url);
}

// New genre. Save and redirect to its detail page.
await genre.save();
res.redirect(genre.url);
```

Questo stesso schema viene usato in tutti i controller post: vengono eseguiti i validator (con i sanitizzatori), quindi vengono verificati gli errori e il modulo viene nuovamente renderizzato con le informazioni sugli errori oppure i dati vengono salvati.

## Vista

La stessa vista viene renderizzata sia nei controller/route `GET` sia in quelli `POST` quando viene creato un nuovo `Genre` (e in seguito viene utilizzata anche quando si _aggiorna_ un `Genre`). Nel caso `GET`, il modulo è vuoto e viene passata solo una variabile title. Nel caso `POST`, l'utente ha inserito in precedenza dati non validi: nella variabile `genre` viene restituita una versione sanitizzata dei dati inseriti e nella variabile `errors` viene restituito un array di messaggi di errore.
Il codice seguente mostra il codice del controller per il rendering del template in entrambi i casi.

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

Creare **/views/genre_form.pug** e copiarvi il testo seguente.

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

Gran parte di questo template sarà familiare dai tutorial precedenti. Innanzitutto, viene esteso il template di base **layout.pug** e viene sovrascritto il `block` denominato '**content**'. È quindi presente un'intestazione con il `title` passato dal controller (tramite il metodo `render()`).

Successivamente, è presente il codice pug per il modulo HTML, che usa `method="POST"` per inviare i dati al server e, poiché `action` è una stringa vuota, invia i dati allo stesso URL della pagina.

Il modulo definisce un singolo campo obbligatorio di tipo "text" denominato "name". Il _value_ predefinito del campo dipende dal fatto che la variabile `genre` sia definita. Se viene chiamato dalla route `GET`, sarà vuoto poiché si tratta di un nuovo modulo. Se viene chiamato da una route `POST`, conterrà il valore (non valido) originariamente inserito dall'utente.

L'ultima parte della pagina è il codice relativo agli errori. Questo stampa un elenco di errori, se la variabile error è stata definita (in altre parole, questa sezione non apparirà quando il template viene renderizzato nella route `GET`).

> [!NOTE]
> Questo è solo uno dei modi per renderizzare gli errori. È anche possibile ottenere i nomi dei campi interessati dalla variabile error e usarli per controllare dove vengono renderizzati i messaggi di errore, se applicare CSS personalizzato e così via.

## Come appare?

Avviare l'applicazione, aprire il browser all'indirizzo `http://localhost:3000/`, quindi selezionare il collegamento _Create new genre_. Se tutto è configurato correttamente, il sito dovrebbe avere un aspetto simile allo screenshot seguente. Dopo aver inserito un valore, questo dovrebbe essere salvato e verrà visualizzata la pagina di dettaglio del genere.

![Pagina di creazione del genere - sito Express Local Library](locallibary_express_genre_create_empty.png)

L'unico errore convalidato lato server è che il campo del genere deve avere almeno tre caratteri. Lo screenshot seguente mostra l'aspetto dell'elenco degli errori se viene fornito un genere con solo uno o due caratteri (evidenziato in giallo).

![La sezione Create Genre dell'applicazione Local library. La colonna sinistra contiene una barra di navigazione verticale. La sezione destra contiene il modulo per creare un nuovo Genre, con un'intestazione che riporta 'Create Genre'. È presente un campo di input etichettato 'Genre'. In basso è presente un pulsante di invio. Direttamente sotto il pulsante Submit è presente un messaggio di errore che riporta 'Genre name required'. Il messaggio di errore è stato evidenziato dall'autore di questo articolo. Nel modulo non vi è alcuna indicazione visiva che il genere sia obbligatorio né che il messaggio di errore venga visualizzato solo in caso di errore.](locallibary_express_genre_create_error.png)

> [!NOTE]
> La validazione usa `trim()` per assicurare che gli spazi vuoti non siano accettati come nome di un genere. Viene inoltre verificato lato client che il campo non sia vuoto aggiungendo l'{{Glossary("Boolean/HTML", "attributo booleano")}} `required` alla definizione del campo nel modulo:
>
> ```pug
> input#name.form-control(type='text', placeholder='Fantasy, Poetry etc.' name='name' required value=(undefined===genre ? '' : genre.name) )
> ```

## Passaggi successivi

1. Tornare a [Tutorial Express Parte 6: Lavorare con i moduli.](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms)
2. Procedere al sottoarticolo successivo della parte 6: [Modulo di creazione dell'autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/forms/Create_author_form).
