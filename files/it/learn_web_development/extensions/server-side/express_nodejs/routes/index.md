---
title: "Tutorial su Express - Parte 4: Route e controller"
short-title: "4: Route e controller"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/routes
l10n:
  sourceCommit: afcdfa050626bb7eb05ee693df8997020db9ff2e
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo tutorial verranno configurate le route (codice di gestione degli URL) con funzioni handler "fittizie" per tutti gli endpoint delle risorse che saranno infine necessari nel sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Al termine sarà disponibile una struttura modulare per il codice di gestione delle route, estendibile con funzioni handler reali negli articoli successivi. Si acquisirà inoltre un'ottima comprensione di come creare route modulari usando Express.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction">introduzione a Express/Node</a>.
        Completare gli argomenti dei tutorial precedenti (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose">Tutorial su Express - Parte 3: Uso di un database (con Mongoose)</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come creare route semplici.
        Configurare tutti gli endpoint URL.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Nell'[ultimo articolo del tutorial](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose) sono stati definiti modelli _Mongoose_ per interagire con il database ed è stato utilizzato uno script (autonomo) per creare alcuni record iniziali della biblioteca. Ora è possibile scrivere il codice per presentare tali informazioni agli utenti. La prima cosa da fare è determinare quali informazioni si desidera poter visualizzare nelle pagine, quindi definire URL appropriati per restituire tali risorse. Sarà poi necessario creare le route (handler URL) e le viste (template) per visualizzare tali pagine.

Il diagramma seguente viene fornito come promemoria del flusso principale dei dati e degli elementi che devono essere implementati nella gestione di una richiesta/risposta HTTP. Oltre alle viste e alle route, il diagramma mostra i "controller": funzioni che separano il codice per instradare le richieste dal codice che elabora effettivamente le richieste.

Poiché i modelli sono già stati creati, gli elementi principali da creare sono:

- "Route" per inoltrare le richieste supportate (e qualsiasi informazione codificata negli URL delle richieste) alle funzioni controller appropriate.
- Funzioni controller per ottenere i dati richiesti dai modelli, creare una pagina HTML che visualizzi i dati e restituirla all'utente per la visualizzazione nel browser.
- Viste (template) utilizzate dai controller per eseguire il rendering dei dati.

![Diagramma del flusso di dati principale di un server MVC Express: le "Route" ricevono le richieste HTTP inviate al server Express e le inoltrano alla funzione "controller" appropriata. Il controller legge e scrive dati dai modelli. I modelli sono collegati al database per fornire accesso ai dati al server. I controller usano le "viste", chiamate anche template, per eseguire il rendering dei dati. Il Controller invia nuovamente al client la risposta HTTP HTML come risposta HTTP.](mvc_express.png)

In definitiva potrebbero essere disponibili pagine per mostrare elenchi e informazioni dettagliate per libri, generi, autori e istanze di libri, insieme a pagine per creare, aggiornare ed eliminare record. È molto da documentare in un singolo articolo. Pertanto, gran parte di questo articolo si concentrerà sulla configurazione di route e controller affinché restituiscano contenuto "fittizio". I metodi del controller saranno estesi negli articoli successivi per lavorare con i dati dei modelli.

La prima sezione seguente fornisce una breve introduzione all'uso del middleware Express [Router](https://expressjs.com/en/5x/api/#router). Queste conoscenze saranno poi utilizzate nelle sezioni successive durante la configurazione delle route di LocalLibrary.

## Introduzione alle route

Una route è una sezione di codice Express che associa un [verbo HTTP](/it/docs/Web/HTTP/Reference/Methods) (`GET`, `POST`, `PUT`, `DELETE` e così via), un percorso/modello URL e una funzione chiamata per gestire tale modello.

Esistono diversi modi per creare route. Per questo tutorial verrà utilizzato il middleware [`express.Router`](https://expressjs.com/en/guide/routing/#express-router), poiché consente di raggruppare gli handler delle route per una particolare parte di un sito e di accedervi usando un prefisso di route comune. Tutte le route relative alla biblioteca verranno mantenute in un modulo "catalog"; se verranno aggiunte route per la gestione degli account utente o di altre funzioni, sarà possibile mantenerle raggruppate separatamente.

> [!NOTE]
> Le route dell'applicazione Express sono state discusse brevemente in [Introduzione a Express > Creazione di handler di route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#creating_route_handlers). Oltre a fornire un supporto migliore per la modularizzazione (come discusso nella prima sottosezione seguente), l'uso di _Router_ è molto simile alla definizione delle route direttamente sull'_oggetto applicazione Express_.

Il resto di questa sezione fornisce una panoramica di come `Router` può essere usato per definire le route.

### Definire e usare moduli di route separati

Il codice seguente fornisce un esempio concreto di come creare un modulo di route e poi utilizzarlo in un'applicazione _Express_.

Per prima cosa vengono create le route per una wiki in un modulo chiamato **wiki.js**. Il codice prima importa l'oggetto applicazione Express, lo usa per ottenere un oggetto `Router` e quindi aggiunge un paio di route usando il metodo `get()`. Infine, il modulo esporta l'oggetto `Router`.

```js
// wiki.js - Wiki route module.

const express = require("express");

const router = express.Router();

// Home page route.
router.get("/", (req, res) => {
  res.send("Wiki home page");
});

// About page route.
router.get("/about", (req, res) => {
  res.send("About this wiki");
});

module.exports = router;
```

> [!NOTE]
> Qui i callback degli handler di route vengono definiti direttamente nelle funzioni del router. In LocalLibrary questi callback verranno definiti in un modulo controller separato.

Per usare il modulo router nel file principale dell'applicazione, viene prima eseguito `require()` del modulo di route (**wiki.js**). Viene quindi chiamato `use()` sull'applicazione _Express_ per aggiungere il Router al percorso di gestione del middleware, specificando un percorso URL di 'wiki'.

```js
const wiki = require("./wiki.js");

// …
app.use("/wiki", wiki);
```

Le due route definite nel modulo di route della wiki sono quindi accessibili da `/wiki/` e `/wiki/about/`.

### Funzioni di route

Il modulo precedente definisce un paio di funzioni di route tipiche. La route "about" (riprodotta di seguito) è definita usando il metodo `Router.get()`, che risponde solo alle richieste HTTP GET. Il primo argomento di questo metodo è il percorso URL, mentre il secondo è una funzione callback che verrà invocata se viene ricevuta una richiesta HTTP GET con tale percorso.

```js
router.get("/about", (req, res) => {
  res.send("About this wiki");
});
```

Il callback accetta tre argomenti (solitamente denominati come mostrato: `req`, `res`, `next`), che conterranno l'oggetto HTTP Request, la risposta HTTP e la funzione _next_ nella catena middleware.

> [!NOTE]
> Le funzioni Router sono [middleware Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#using_middleware), il che significa che devono completare (rispondere a) la richiesta oppure chiamare la funzione `next` nella catena. Nel caso precedente la richiesta viene completata usando `send()`, pertanto l'argomento `next` non viene utilizzato (e si sceglie di non specificarlo).
>
> La funzione router precedente accetta un singolo callback, ma è possibile specificare quanti argomenti callback si desidera, oppure un array di funzioni callback. Ogni funzione fa parte della catena middleware e verrà chiamata nell'ordine in cui viene aggiunta alla catena (a meno che una funzione precedente non completi la richiesta).

La funzione callback qui chiama [`send()`](https://expressjs.com/en/5x/api/#res.send) sulla risposta per restituire la stringa "About this wiki" quando viene ricevuta una richiesta GET con il percorso (`/about`). Esistono diversi [altri metodi di risposta](https://expressjs.com/en/guide/routing/#response-methods) per terminare il ciclo richiesta/risposta. Ad esempio, è possibile chiamare [`res.json()`](https://expressjs.com/en/5x/api/#res.json) per inviare una risposta JSON oppure [`res.sendFile()`](https://expressjs.com/en/5x/api/#res.sendFile) per inviare un file. Il metodo di risposta utilizzato più spesso durante la costruzione della biblioteca è [`render()`](https://expressjs.com/en/5x/api/#res.render), che crea e restituisce file HTML usando template e dati: se ne parlerà molto più approfonditamente in un articolo successivo.

### Verbi HTTP

Le route di esempio precedenti usano il metodo `Router.get()` per rispondere alle richieste HTTP `GET` con un determinato percorso.

`Router` fornisce anche metodi di route per tutti gli altri [verbi HTTP](/it/docs/Web/HTTP/Reference/Methods), usati per lo più esattamente nello stesso modo: `post()`, `put()`, `delete()`, `options()`, `trace()`, `copy()`, `lock()`, `mkcol()`, `move()`, `purge()`, `propfind()`, `proppatch()`, `unlock()`, `report()`, `mkactivity()`, `checkout()`, `merge()`, `m-search()`, `notify()`, `subscribe()`, `unsubscribe()`, `patch()`, `search()` e `connect()`.

Ad esempio, il codice seguente si comporta esattamente come la precedente route `/about`, ma risponde solo alle richieste HTTP POST.

```js
router.post("/about", (req, res) => {
  res.send("About this wiki");
});
```

Idealmente, i siti web dovrebbero utilizzare il metodo di route (e il metodo HTTP) che corrisponde meglio all'operazione eseguita.
Ad esempio, un'applicazione con rendering lato client dovrebbe utilizzare `Router.get()` per leggere dal database, `Router.post()` per creare nuovi record, `Router.put()` o `Router.patch()` per aggiornare record e `Router.delete()` per eliminare dati.

Occorre tuttavia notare che le applicazioni con rendering lato server, come quella illustrata da questo tutorial, usano comunemente `Router.post()` per tutte le route che modificano dati.
Il motivo è che, per impostazione predefinita, gli elementi HTML `<form>` sono in grado di inviare solo richieste [`GET`](/it/docs/Web/HTTP/Reference/Methods/GET) e [`POST`](/it/docs/Web/HTTP/Reference/Methods/POST).

Esistono varie soluzioni alternative per questa limitazione, come codificare il verbo HTTP "desiderato" in una richiesta `POST` e usare il middleware Express [method-override](https://www.npmjs.com/package/method-override) per modificare la richiesta nel verbo HTTP appropriato prima che venga passata al router.
Per applicazioni di base, riscrivere il codice soltanto per usare i verbi HTTP corretti è generalmente eccessivo.
Potrebbe essere utile migliorare il logging del server, oppure nel caso in cui il server debba gestire contenuto con rendering sia lato server sia lato client tramite lo stesso endpoint.

### Percorsi delle route

I percorsi delle route definiscono gli endpoint presso i quali è possibile effettuare richieste. Gli esempi visti finora erano semplici stringhe e vengono usati esattamente come scritti: '/', '/about', '/book', '/any-random.path'.

I percorsi delle route possono essere anche modelli di stringa. I modelli di stringa utilizzano una forma della sintassi delle espressioni regolari per definire _modelli_ di endpoint che verranno trovati.
La maggior parte delle route di LocalLibrary utilizzerà stringhe e non espressioni regolari.
Verranno inoltre utilizzati parametri di route, come discusso nella sezione successiva.

### Parametri di route

I parametri di route sono _segmenti URL con nome_ utilizzati per acquisire valori in posizioni specifiche dell'URL. I segmenti con nome sono preceduti da due punti e poi dal nome (ad esempio, `/:your_parameter_name/`). I valori acquisiti vengono memorizzati nell'oggetto `req.params`, usando i nomi dei parametri come chiavi (ad esempio, `req.params.your_parameter_name`).

Ad esempio, si consideri un URL codificato per contenere informazioni su utenti e libri: `http://localhost:3000/users/34/books/8989`. È possibile estrarre queste informazioni come mostrato di seguito, con i parametri di percorso `userId` e `bookId`:

```js
app.get("/users/:userId/books/:bookId", (req, res) => {
  // Access userId via: req.params.userId
  // Access bookId via: req.params.bookId
  res.send(req.params);
});
```

> [!NOTE]
> L'URL _/book/create_ verrà trovato da una route come `/book/:bookId` (perché `:bookId` è un segnaposto per _qualsiasi_ stringa, quindi `create` corrisponde). Verrà utilizzata la prima route che corrisponde a un URL in ingresso, pertanto, se si desidera elaborare specificamente gli URL `/book/create`, il relativo handler di route deve essere definito prima della route `/book/:bookId`.

I nomi dei parametri di route (ad esempio, `bookId` sopra) possono essere qualsiasi identificatore JavaScript valido che inizi con una lettera, `_` o `$`. Dopo il primo carattere è possibile includere cifre, ma non trattini e spazi.
È anche possibile usare nomi che non sono identificatori JavaScript validi, inclusi spazi, trattini, emoticon o qualsiasi altro carattere, ma occorre definirli con una stringa tra virgolette e accedervi usando la notazione con parentesi quadre.
Ad esempio:

```js
app.get('/users/:"user id"/books/:"book-id"', (req, res) => {
  // Access quoted param using bracket notation
  const user = req.params["user id"];
  const book = req.params["book-id"];
  res.send({ user, book });
});
```

### Caratteri jolly

I parametri con caratteri jolly corrispondono a uno o più caratteri attraverso più segmenti, restituendo ciascun segmento come valore in un array.
Sono definiti nello stesso modo dei parametri regolari, ma sono preceduti da un asterisco.

Ad esempio, si consideri l'URL `http://localhost:3000/users/34/books/8989`: è possibile estrarre tutte le informazioni dopo `users/` con il carattere jolly `example`:

```js
app.get("/users/*example", (req, res) => {
  // req.params would contain { "example": ["34", "books", "8989"]}
  res.send(req.params);
});
```

### Parti facoltative

Le parentesi graffe possono essere utilizzate per definire parti facoltative del percorso.
Ad esempio, di seguito viene trovata una filename con qualsiasi estensione, oppure senza estensione.

```js
app.get("/file/:filename{.:ext}", (req, res) => {
  // Given URL: http://localhost:3000/file/somefile.md`
  // req.params would contain { "filename": "somefile", "ext": "md"}
  res.send(req.params);
});
```

### Caratteri riservati

I seguenti caratteri sono riservati: `(()[]?+!)`.
Per usarli, è necessario eseguirne l'escape con una barra rovesciata (`\`).

Non è inoltre possibile usare il carattere pipe (`|`) in un'espressione regolare.

Questo è tutto ciò che serve per iniziare a lavorare con le route.
Se necessario, ulteriori informazioni sono disponibili nella documentazione di Express: [Routing di base](https://expressjs.com/en/starter/basic-routing/) e [Guida al routing](https://expressjs.com/en/guide/routing/). Le sezioni seguenti mostrano come verranno configurate route e controller per LocalLibrary.

### Gestire errori ed eccezioni nelle funzioni di route

Le funzioni di route mostrate in precedenza hanno gli argomenti `req` e `res`, che rappresentano rispettivamente la richiesta e la risposta.
Alle funzioni di route viene anche passato un terzo argomento, `next`, che contiene una funzione callback chiamabile per passare eventuali errori o eccezioni alla catena middleware di Express, dove verranno infine propagati al codice globale di gestione degli errori.

A partire da Express 5, `next` viene chiamata automaticamente con il valore di rifiuto se un handler di route restituisce una [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise) che viene successivamente rifiutata; pertanto, quando si usano le promise, non è richiesto codice di gestione degli errori nelle funzioni di route.
Questo porta a codice molto compatto quando si lavora con API asincrone basate su promise, in particolare usando [`async` e `await`](/it/docs/Learn_web_development/Extensions/Async_JS/Promises#async_and_await).

Ad esempio, il codice seguente usa il metodo `find()` per interrogare un database e poi esegue il rendering del risultato.

```js
exports.get("/about", async (req, res, next) => {
  const successfulResult = await About.find({}).exec();
  res.render("about_view", { title: "About", list: successfulResult });
});
```

Il codice seguente mostra lo stesso esempio usando una catena di promise.
Si noti che, se lo si desidera, è possibile eseguire `catch()` dell'errore e implementare una gestione personalizzata.

```js
exports.get(
  "/about",
  // Removed 'async'
  (req, res, next) =>
    About.find({})
      .exec()
      .then((successfulResult) => {
        res.render("about_view", { title: "About", list: successfulResult });
      })
      .catch((err) => {
        next(err);
      }),
);
```

> [!NOTE]
> La maggior parte delle API moderne è asincrona e basata su promise, quindi la gestione degli errori è spesso così semplice.
> Certamente è tutto ciò che è realmente _necessario_ sapere sulla gestione degli errori per questo tutorial.

Express 5 intercetta e inoltra automaticamente le eccezioni generate nel codice sincrono:

```js
app.get("/", (req, res) => {
  // Express will catch this
  throw new Error("SynchronousException");
});
```

Tuttavia, è necessario eseguire [`catch()`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) delle eccezioni che si verificano nel codice asincrono invocato da handler di route o middleware. Queste non verranno intercettate dal codice predefinito:

```js
app.get("/", (req, res, next) => {
  setTimeout(() => {
    try {
      // You must catch and propagate this error yourself
      throw new Error("AsynchronousException");
    } catch (err) {
      next(err);
    }
  }, 100);
});
```

Infine, se vengono utilizzati metodi asincroni nello stile precedente che restituiscono un errore o un risultato in una funzione callback, è necessario propagare manualmente l'errore.
L'esempio seguente mostra come farlo.

```js
router.get("/about", (req, res, next) => {
  About.find({}).exec((err, queryResults) => {
    if (err) {
      // Propagate the error
      return next(err);
    }
    // Successful, so render
    res.render("about_view", { title: "About", list: queryResults });
  });
});
```

Per maggiori informazioni, vedere [Gestione degli errori](https://expressjs.com/en/guide/error-handling/).

## Route necessarie per LocalLibrary

Gli URL che saranno infine necessari per le pagine sono elencati di seguito, dove _object_ viene sostituito dal nome di ciascuno dei modelli (book, bookinstance, genre, author), _objects_ è il plurale di object e _id_ è il campo dell'istanza univoco (`_id`) assegnato per impostazione predefinita a ogni istanza del modello Mongoose.

- `catalog/` — La pagina principale/indice.
- `catalog/<objects>/` — L'elenco di tutti i libri, le istanze di libri, i generi o gli autori (ad esempio, /`catalog/books/`, /`catalog/genres/` e così via).
- `catalog/<object>/<id>` — La pagina di dettaglio di uno specifico libro, istanza di libro, genere o autore con il valore del campo `_id` specificato (ad esempio, `/catalog/book/584493c1f4887f06c0e67d37)`).
- `catalog/<object>/create` — Il modulo per creare un nuovo libro, istanza di libro, genere o autore (ad esempio, `/catalog/book/create)`).
- `catalog/<object>/<id>/update` — Il modulo per aggiornare uno specifico libro, istanza di libro, genere o autore con il valore del campo `_id` specificato (ad esempio, `/catalog/book/584493c1f4887f06c0e67d37/update)`).
- `catalog/<object>/<id>/delete` — Il modulo per eliminare uno specifico libro, istanza di libro, genere o autore con il valore del campo `_id` specificato (ad esempio, `/catalog/book/584493c1f4887f06c0e67d37/delete)`).

La prima pagina principale e le pagine degli elenchi non codificano alcuna informazione aggiuntiva. Sebbene i risultati restituiti dipenderanno dal tipo di modello e dal contenuto del database, le query eseguite per ottenere le informazioni saranno sempre le stesse (analogamente, il codice eseguito per la creazione di oggetti sarà sempre simile).

Al contrario, gli altri URL sono utilizzati per agire su una specifica istanza di documento/modello: codificano l'identità dell'elemento nell'URL (mostrata sopra come `<id>`). Verranno utilizzati parametri di percorso per estrarre le informazioni codificate e passarle all'handler di route (in un articolo successivo questo verrà usato per determinare dinamicamente quali informazioni ottenere dal database). Codificando le informazioni nell'URL, è necessaria una sola route per ogni risorsa di un particolare tipo (ad esempio, una route per gestire la visualizzazione di ogni singolo libro).

> [!NOTE]
> Express consente di costruire gli URL in qualsiasi modo: è possibile codificare informazioni nel corpo dell'URL come mostrato sopra oppure usare parametri URL `GET` (ad esempio, `/book/?id=6`). Qualunque approccio venga usato, gli URL devono rimanere puliti, logici e leggibili ([consultare qui i consigli del W3C](https://www.w3.org/Provider/Style/URI)).

Successivamente verranno create le funzioni callback degli handler di route e il codice delle route per tutti gli URL precedenti.

## Creare le funzioni callback degli handler di route

Prima di definire le route, verranno prima create tutte le funzioni callback fittizie/scheletro che invocheranno. I callback verranno memorizzati in moduli "controller" separati per `Book`, `BookInstance`, `Genre` e `Author` (è possibile usare qualsiasi struttura di file/moduli, ma questa sembra una granularità appropriata per il progetto).

Iniziare creando una cartella per i controller nella radice del progetto (**/controllers**), quindi creare file/moduli controller separati per la gestione di ciascuno dei modelli:

```plain
/express-locallibrary-tutorial  # the project root
  /controllers
    authorController.js
    bookController.js
    bookinstanceController.js
    genreController.js
```

### Controller Author

Aprire il file **/controllers/authorController.js** e inserire il codice seguente:

```js
const Author = require("../models/author");

// Display list of all Authors.
exports.author_list = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author list");
};

// Display detail page for a specific Author.
exports.author_detail = async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: Author detail: ${req.params.id}`);
};

// Display Author create form on GET.
exports.author_create_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author create GET");
};

// Handle Author create on POST.
exports.author_create_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author create POST");
};

// Display Author delete form on GET.
exports.author_delete_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author delete GET");
};

// Handle Author delete on POST.
exports.author_delete_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author delete POST");
};

// Display Author update form on GET.
exports.author_update_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author update GET");
};

// Handle Author update on POST.
exports.author_update_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author update POST");
};
```

Il modulo prima richiede il modello `Author`, che verrà successivamente usato per accedere ai dati e aggiornarli.
Quindi esporta funzioni per ciascuno degli URL che si desidera gestire.
Si noti che le operazioni di creazione, aggiornamento ed eliminazione usano moduli e quindi dispongono anche di metodi aggiuntivi per gestire le richieste post dei moduli: tali metodi verranno discussi nell'"articolo sui moduli" successivamente.

Le funzioni rispondono con una stringa che indica che la pagina associata non è ancora stata creata.
Se una funzione controller deve ricevere parametri di percorso, questi vengono prodotti nella stringa del messaggio (vedere `req.params.id` sopra).

#### Controller BookInstance

Aprire il file **/controllers/bookinstanceController.js** e copiare il codice seguente (segue un modello identico al modulo controller `Author`):

```js
const BookInstance = require("../models/bookinstance");

// Display list of all BookInstances.
exports.bookinstance_list = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance list");
};

// Display detail page for a specific BookInstance.
exports.bookinstance_detail = async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: BookInstance detail: ${req.params.id}`);
};

// Display BookInstance create form on GET.
exports.bookinstance_create_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance create GET");
};

// Handle BookInstance create on POST.
exports.bookinstance_create_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance create POST");
};

// Display BookInstance delete form on GET.
exports.bookinstance_delete_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance delete GET");
};

// Handle BookInstance delete on POST.
exports.bookinstance_delete_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance delete POST");
};

// Display BookInstance update form on GET.
exports.bookinstance_update_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance update GET");
};

// Handle bookinstance update on POST.
exports.bookinstance_update_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance update POST");
};
```

#### Controller Genre

Aprire il file **/controllers/genreController.js** e copiare il testo seguente (segue un modello identico ai file `Author` e `BookInstance`):

```js
const Genre = require("../models/genre");

// Display list of all Genre.
exports.genre_list = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre list");
};

// Display detail page for a specific Genre.
exports.genre_detail = async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: Genre detail: ${req.params.id}`);
};

// Display Genre create form on GET.
exports.genre_create_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre create GET");
};

// Handle Genre create on POST.
exports.genre_create_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre create POST");
};

// Display Genre delete form on GET.
exports.genre_delete_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre delete GET");
};

// Handle Genre delete on POST.
exports.genre_delete_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre delete POST");
};

// Display Genre update form on GET.
exports.genre_update_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre update GET");
};

// Handle Genre update on POST.
exports.genre_update_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre update POST");
};
```

#### Controller Book

Aprire il file **/controllers/bookController.js** e copiare il codice seguente.
Questo segue lo stesso modello degli altri moduli controller, ma include inoltre una funzione `index()` per visualizzare la pagina di benvenuto del sito:

```js
const Book = require("../models/book");

exports.index = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
};

// Display list of all books.
exports.book_list = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book list");
};

// Display detail page for a specific book.
exports.book_detail = async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: Book detail: ${req.params.id}`);
};

// Display book create form on GET.
exports.book_create_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book create GET");
};

// Handle book create on POST.
exports.book_create_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book create POST");
};

// Display book delete form on GET.
exports.book_delete_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book delete GET");
};

// Handle book delete on POST.
exports.book_delete_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book delete POST");
};

// Display book update form on GET.
exports.book_update_get = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book update GET");
};

// Handle book update on POST.
exports.book_update_post = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book update POST");
};
```

## Creare il modulo di route catalog

Successivamente verranno create le _route_ per tutti gli URL [necessari al sito web LocalLibrary](#route_necessarie_per_locallibrary), che chiameranno le funzioni controller definite nelle sezioni precedenti.

Lo scheletro contiene già una cartella **./routes** con route per _index_ e _users_.
Creare un altro file di route, **catalog.js**, all'interno di questa cartella, come mostrato.

```plain
/express-locallibrary-tutorial # the project root
  /routes
    index.js
    users.js
    catalog.js
```

Aprire **/routes/catalog.js** e copiare il codice seguente:

```js
const express = require("express");

// Require controller modules.
const book_controller = require("../controllers/bookController");
const author_controller = require("../controllers/authorController");
const genre_controller = require("../controllers/genreController");
const book_instance_controller = require("../controllers/bookinstanceController");

const router = express.Router();

/// BOOK ROUTES ///

// GET catalog home page.
router.get("/", book_controller.index);

// GET request for creating a Book. NOTE This must come before routes that display Book (uses id).
router.get("/book/create", book_controller.book_create_get);

// POST request for creating Book.
router.post("/book/create", book_controller.book_create_post);

// GET request to delete Book.
router.get("/book/:id/delete", book_controller.book_delete_get);

// POST request to delete Book.
router.post("/book/:id/delete", book_controller.book_delete_post);

// GET request to update Book.
router.get("/book/:id/update", book_controller.book_update_get);

// POST request to update Book.
router.post("/book/:id/update", book_controller.book_update_post);

// GET request for one Book.
router.get("/book/:id", book_controller.book_detail);

// GET request for list of all Book items.
router.get("/books", book_controller.book_list);

/// AUTHOR ROUTES ///

// GET request for creating Author. NOTE This must come before route for id (i.e. display author).
router.get("/author/create", author_controller.author_create_get);

// POST request for creating Author.
router.post("/author/create", author_controller.author_create_post);

// GET request to delete Author.
router.get("/author/:id/delete", author_controller.author_delete_get);

// POST request to delete Author.
router.post("/author/:id/delete", author_controller.author_delete_post);

// GET request to update Author.
router.get("/author/:id/update", author_controller.author_update_get);

// POST request to update Author.
router.post("/author/:id/update", author_controller.author_update_post);

// GET request for one Author.
router.get("/author/:id", author_controller.author_detail);

// GET request for list of all Authors.
router.get("/authors", author_controller.author_list);

/// GENRE ROUTES ///

// GET request for creating a Genre. NOTE This must come before route that displays Genre (uses id).
router.get("/genre/create", genre_controller.genre_create_get);

// POST request for creating Genre.
router.post("/genre/create", genre_controller.genre_create_post);

// GET request to delete Genre.
router.get("/genre/:id/delete", genre_controller.genre_delete_get);

// POST request to delete Genre.
router.post("/genre/:id/delete", genre_controller.genre_delete_post);

// GET request to update Genre.
router.get("/genre/:id/update", genre_controller.genre_update_get);

// POST request to update Genre.
router.post("/genre/:id/update", genre_controller.genre_update_post);

// GET request for one Genre.
router.get("/genre/:id", genre_controller.genre_detail);

// GET request for list of all Genre.
router.get("/genres", genre_controller.genre_list);

/// BOOKINSTANCE ROUTES ///

// GET request for creating a BookInstance. NOTE This must come before route that displays BookInstance (uses id).
router.get(
  "/bookinstance/create",
  book_instance_controller.bookinstance_create_get,
);

// POST request for creating BookInstance.
router.post(
  "/bookinstance/create",
  book_instance_controller.bookinstance_create_post,
);

// GET request to delete BookInstance.
router.get(
  "/bookinstance/:id/delete",
  book_instance_controller.bookinstance_delete_get,
);

// POST request to delete BookInstance.
router.post(
  "/bookinstance/:id/delete",
  book_instance_controller.bookinstance_delete_post,
);

// GET request to update BookInstance.
router.get(
  "/bookinstance/:id/update",
  book_instance_controller.bookinstance_update_get,
);

// POST request to update BookInstance.
router.post(
  "/bookinstance/:id/update",
  book_instance_controller.bookinstance_update_post,
);

// GET request for one BookInstance.
router.get("/bookinstance/:id", book_instance_controller.bookinstance_detail);

// GET request for list of all BookInstance.
router.get("/bookinstances", book_instance_controller.bookinstance_list);

module.exports = router;
```

Il modulo richiede Express e quindi lo usa per creare un oggetto `Router`. Tutte le route vengono configurate sul router, che viene poi esportato.

Le route sono definite usando i metodi `.get()` oppure `.post()` sull'oggetto router.
Tutti i percorsi sono definiti usando stringhe (non vengono usati modelli di stringa né espressioni regolari).
Le route che agiscono su una risorsa specifica, ad esempio un libro, usano parametri di percorso per ottenere l'id dell'oggetto dall'URL.

Le funzioni handler vengono tutte importate dai moduli controller creati nella sezione precedente.

### Aggiornare il modulo di route index

Tutte le nuove route sono state configurate, ma è ancora presente una route verso la pagina originale. Verrà invece reindirizzata alla nuova pagina indice creata nel percorso `/catalog`.

Aprire **/routes/index.js** e sostituire la route esistente con la funzione seguente.

```js
// GET home page.
router.get("/", (req, res) => {
  res.redirect("/catalog");
});
```

> [!NOTE]
> Questo è il primo utilizzo del metodo di risposta [redirect()](https://expressjs.com/en/5x/api/#res.redirect). Esso reindirizza alla pagina specificata, inviando per impostazione predefinita il codice di stato HTTP "302 Found". Se necessario, è possibile modificare il codice di stato restituito e fornire percorsi assoluti oppure relativi.

### Aggiornare app.js

L'ultimo passaggio consiste nell'aggiungere le route alla catena middleware.
Questa operazione viene eseguita in `app.js`.

Aprire **app.js** e richiedere la route catalog sotto le altre route (aggiungere la terza riga mostrata di seguito, sotto le altre due che dovrebbero essere già presenti nel file):

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
const catalogRouter = require("./routes/catalog"); // Import routes for "catalog" area of site
```

Quindi aggiungere la route catalog allo stack middleware sotto le altre route (aggiungere la terza riga mostrata di seguito, sotto le altre due che dovrebbero essere già presenti nel file):

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
app.use("/catalog", catalogRouter); // Add catalog routes to middleware chain.
```

> [!NOTE]
> Il modulo catalog è stato aggiunto nel percorso `/catalog`. Questo viene anteposto a tutti i percorsi definiti nel modulo catalog. Ad esempio, per accedere a un elenco di libri, l'URL sarà: `/catalog/books/`.

Questo è tutto. Ora dovrebbero essere disponibili route e funzioni scheletro abilitate per tutti gli URL che saranno infine supportati dal sito web LocalLibrary.

### Testare le route

Per testare le route, avviare prima il sito web usando l'approccio abituale.

- Il metodo predefinito

  ```bash
  # Windows
  SET DEBUG=express-locallibrary-tutorial:* & npm start

  # macOS or Linux
  DEBUG=express-locallibrary-tutorial:* npm start
  ```

- Se in precedenza è stato configurato [nodemon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website#enable_server_restart_on_file_changes), è possibile invece usare:

  ```bash
  npm run serverstart
  ```

Quindi passare a diversi URL di LocalLibrary e verificare che non venga visualizzata una pagina di errore (HTTP 404). Un piccolo insieme di URL è elencato di seguito per comodità:

- `http://localhost:3000/`
- `http://localhost:3000/catalog`
- `http://localhost:3000/catalog/books`
- `http://localhost:3000/catalog/bookinstances/`
- `http://localhost:3000/catalog/authors/`
- `http://localhost:3000/catalog/genres/`
- `http://localhost:3000/catalog/book/5846437593935e2f8c2aa226`
- `http://localhost:3000/catalog/book/create`

## Riepilogo

Ora sono state create tutte le route per il sito, insieme a funzioni controller fittizie che potranno essere popolate con un'implementazione completa negli articoli successivi. Nel corso del processo sono state apprese molte informazioni fondamentali sulle route Express, sulla gestione delle eccezioni e su alcuni approcci per strutturare route e controller.

Nel prossimo articolo verrà creata una vera pagina di benvenuto per il sito, usando viste (template) e informazioni memorizzate nei modelli.

## Vedere anche

- [Routing di base](https://expressjs.com/en/starter/basic-routing/) (documentazione Express)
- [Guida al routing](https://expressjs.com/en/guide/routing/) (documentazione Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
