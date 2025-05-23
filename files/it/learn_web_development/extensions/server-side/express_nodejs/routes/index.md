---
title: "Guida Express Parte 4: Routes e Controllers"
short-title: "4: Routes e Controllers"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/routes
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo tutorial, imposteremo routes (codice di gestione degli URL) con funzioni di gestione "dummy" per tutti gli endpoint di risorse di cui avremo bisogno nel sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Una volta completato, avremo una struttura modulare per il nostro codice di gestione delle route, che possiamo estendere con funzioni di gestione reali negli articoli successivi. Avremo anche una buona comprensione di come creare routes modulari utilizzando Express!

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction">introduzione a Express/Node</a>.
        Completare i precedenti argomenti del tutorial (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose">Express Tutorial Parte 3: Uso di un Database (con Mongoose)</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Capire come creare semplici routes.
        Configurare tutti i nostri endpoint URL.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Nell'[articolo del tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose) abbiamo definito modelli _Mongoose_ per interagire con il database e utilizzato uno script (autonomo) per creare alcuni record iniziali della biblioteca. Ora possiamo scrivere il codice per presentare tali informazioni agli utenti. La prima cosa che dobbiamo fare è determinare quali informazioni vogliamo essere in grado di visualizzare nelle nostre pagine e quindi definire URL appropriati per restituire tali risorse. Successivamente, dovremo creare le routes (gestori URL) e le view (template) per visualizzare quelle pagine.

Il diagramma sotto è fornito come promemoria del flusso principale di dati e delle cose che devono essere implementate quando si gestisce una richiesta/risposta HTTP. Oltre alle view e alle routes, il diagramma mostra i "controller" - funzioni che separano il codice per instradare le richieste dal codice che effettivamente elabora le richieste.

Poiché abbiamo già creato i modelli, le principali cose che dovremo creare sono:

- "Routes" per inoltrare le richieste supportate (e qualsiasi informazione codificata negli URL delle richieste) alle funzioni controller appropriate.
- Funzioni controller per ottenere i dati richiesti dai modelli, creare una pagina HTML che mostra i dati e restituirla all'utente da visualizzare nel browser.
- View (template) utilizzate dai controller per rendere i dati.

![Diagramma del flusso principale di dati di un server Express MVC: le 'Routes' ricevono le richieste HTTP inviate al server Express e le trasmettono alla funzione 'controller' appropriata. Il controller legge e scrive dati dai modelli. I modelli sono connessi al database per fornire accesso ai dati al server. I controller utilizzano 'view', chiamate anche template, per rendere i dati. Il Controller invia la risposta HTML HTTP al client come risposta HTTP.](mvc_express.png)

Alla fine potremmo avere pagine per mostrare liste e dettagli di libri, generi, autori e istanze di libri, insieme a pagine per creare, aggiornare ed eliminare record. È tanto da documentare in un solo articolo. Pertanto, la maggior parte di questo articolo si concentrerà sull'impostazione delle nostre routes e dei controller per restituire contenuti "dummy". Estenderemo i metodi del controller nei nostri successivi articoli per lavorare con i dati del modello.

La prima sezione di seguito fornisce un breve "primer" su come utilizzare il middleware Express [Router](https://expressjs.com/en/4x/api.html#router). Useremo poi quella conoscenza nelle sezioni seguenti quando configuriamo le rotte della LocalLibrary.

## Primer sulle routes

Una route è una sezione del codice di Express che associa un verbo HTTP (`GET`, `POST`, `PUT`, `DELETE`, ecc.), un percorso/pattern URL, e una funzione che viene chiamata per gestire quel pattern.

Ci sono diversi modi per creare routes. Per questo tutorial, useremo il middleware [`express.Router`](https://expressjs.com/en/guide/routing.html#express-router) poiché ci consente di raggruppare i gestori delle route per una parte particolare di un sito e accedervi usando un prefisso di route comune. Terremo tutte le nostre rotte legate alla biblioteca in un modulo "catalog", e, se aggiungiamo routes per gestire account utente o altre funzioni, possiamo tenerle raggruppate separatamente.

> [!NOTE]
> Abbiamo discusso brevemente delle rotte delle applicazioni Express nella nostra [Introduzione a Express > Creazione di gestori di route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#creating_route_handlers). Oltre a fornire un miglior supporto alla modularizzazione (come discusso nella prima sottosezione qui sotto), usare _Router_ è molto simile a definire route direttamente sull'_oggetto applicazione Express_.

Il resto di questa sezione fornisce una panoramica su come il `Router` può essere utilizzato per definire le routes.

### Definire e utilizzare moduli di route separati

Il codice sotto fornisce un esempio concreto di come possiamo creare un modulo route e poi usarlo in un'applicazione _Express_.

Prima creiamo routes per una wiki in un modulo chiamato **wiki.js**. Il codice prima importa l'oggetto applicazione Express, lo usa per ottenere un oggetto `Router` e poi aggiunge un paio di routes usando il metodo `get()`. Alla fine il modulo esporta l'oggetto `Router`.

```js
// wiki.js - Wiki route module.

const express = require("express");

const router = express.Router();

// Home page route.
router.get("/", function (req, res) {
  res.send("Wiki home page");
});

// About page route.
router.get("/about", function (req, res) {
  res.send("About this wiki");
});

module.exports = router;
```

> [!NOTE]
> Sopra, stiamo definendo i callback dei nostri gestori di route direttamente nelle funzioni del router. Nella LocalLibrary, definiremo questi callback in un modulo controller separato.

Per utilizzare il modulo router nel nostro file principale dell'app, prima facciamo `require()` del modulo route (**wiki.js**). Poi chiamiamo `use()` sull'applicazione _Express_ per aggiungere il Router al percorso di gestione del middleware, specificando un percorso URL 'wiki'.

```js
const wiki = require("./wiki.js");

// …
app.use("/wiki", wiki);
```

Le due route definite nel nostro modulo route wiki sono quindi accessibili da `/wiki/` e `/wiki/about/`.

### Funzioni route

Il nostro modulo sopra definisce un paio di funzioni route tipiche. La route "about" (riprodotta di seguito) è definita usando il metodo `Router.get()`, che risponde solo a richieste HTTP GET. Il primo argomento di questo metodo è il percorso URL mentre il secondo è una funzione callback che verrà invocata se viene ricevuta una richiesta HTTP GET con quel percorso.

```js
router.get("/about", function (req, res) {
  res.send("About this wiki");
});
```

Il callback accetta tre argomenti (di solito nominati come mostrato: `req`, `res`, `next`), che contengono rispettivamente l'oggetto della richiesta HTTP, la risposta HTTP, e la funzione _next_ nella catena del middleware.

> [!NOTE]
> Le funzioni router sono [middleware di Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#using_middleware), il che significa che devono completare (rispondere a) la richiesta o chiamare la funzione `next` nella catena. Nel caso sopra, completiamo la richiesta usando `send()`, quindi l'argomento `next` non viene usato (e scegliamo di non specificarlo).
>
> La funzione router sopra accetta un singolo callback, ma è possibile specificare quanti più argomenti di callback si desidera, o un array di funzioni callback. Ogni funzione fa parte della catena middleware e verrà chiamata nell'ordine in cui è aggiunta alla catena (a meno che una funzione precedente non completi la richiesta).

La funzione callback qui chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "About this wiki" quando riceviamo una richiesta GET con quel percorso (`/about`). Ci sono [un certo numero di altri metodi di risposta](https://expressjs.com/en/guide/routing.html#response-methods) per terminare il ciclo di richiesta/risposta. Ad esempio, si potrebbe chiamare [`res.json()`](https://expressjs.com/en/4x/api.html#res.json) per inviare una risposta JSON o [`res.sendFile()`](https://expressjs.com/en/4x/api.html#res.sendFile) per inviare un file. Il metodo di risposta che useremo più spesso mentre costruiamo la biblioteca è [`render()`](https://expressjs.com/en/4x/api.html#res.render), che crea e restituisce file HTML usando template e dati - ne parleremo molto più avanti in un articolo successivo!

### Verbi HTTP

Le route di esempio sopra utilizzano il metodo `Router.get()` per rispondere alle richieste HTTP GET con un certo percorso.

Il `Router` fornisce anche metodi di route per tutti gli altri verbi HTTP, che sono usati principalmente allo stesso modo: `post()`, `put()`, `delete()`, `options()`, `trace()`, `copy()`, `lock()`, `mkcol()`, `move()`, `purge()`, `propfind()`, `proppatch()`, `unlock()`, `report()`, `mkactivity()`, `checkout()`, `merge()`, `m-search()`, `notify()`, `subscribe()`, `unsubscribe()`, `patch()`, `search()`, e `connect()`.

Ad esempio, il codice sotto si comporta proprio come la route `/about` precedente, ma risponde solo alle richieste HTTP POST.

```js
router.post("/about", (req, res) => {
  res.send("About this wiki");
});
```

### Percorsi delle route

I percorsi delle route definiscono gli endpoint ai quali possono essere effettuate le richieste. Gli esempi che abbiamo visto finora sono stati solo stringhe, e vengono usati esattamente come scritto: '/', '/about', '/book', '/any-random.path'.

I percorsi delle route possono anche essere pattern di stringa. I pattern di stringa usano una forma di sintassi delle espressioni regolari per definire _pattern_ di endpoint che verranno abbinati. La sintassi è elencata di seguito (nota che il trattino (`-`) e il punto (`.`) sono interpretati letteralmente dai percorsi basati su stringa):

- `?`: L'endpoint deve avere 0 o 1 del carattere precedente (o gruppo), ad esempio, un percorso di route di `'/ab?cd'` corrisponderà agli endpoint `acd` o `abcd`.
- `+`: L'endpoint deve avere 1 o più del carattere precedente (o gruppo), ad esempio, un percorso di route di `'/ab+cd'` corrisponderà agli endpoint `abcd`, `abbcd`, `abbbcd`, e così via.
- `*`: L'endpoint può avere una stringa arbitraria dove è posizionato il carattere `*`. Ad esempio un percorso di route di `'/ab*cd'` corrisponderà agli endpoint `abcd`, `abXcd`, `abSOMErandomTEXTcd`, e così via.
- `()`: Raggruppare una partita su un insieme di caratteri per eseguire un'altra operazione su di essa, ad esempio, `'/ab(cd)?e'` eseguirà un match di tipo `?` sul gruppo `(cd)` — corrispondendo a `abe` e `abcde`.

I percorsi delle route possono anche essere espressioni regolari [JavaScript](/it/docs/Web/JavaScript/Guide/Regular_expressions). Ad esempio, il percorso di route sotto corrisponderà a `catfish` e `dogfish`, ma non a `catflap`, `catfishhead`, e così via. Nota che il percorso per un'espressione regolare usa la sintassi delle espressioni regolari (non è una stringa come nei casi precedenti).

```js
app.get(/.*fish$/, function (req, res) {
  // …
});
```

> [!NOTE]
> La maggior parte delle nostre route per la LocalLibrary utilizzeranno stringhe e non espressioni regolari. Utilizzeremo anche parametri di route come discusso nella prossima sezione.

### Parametri di route

I parametri di route sono _segmenti di URL denominati_ che vengono utilizzati per catturare i valori in posizioni specifiche nell'URL. I segmenti denominati vengono prefissati con un due punti e quindi il nome (ad esempio, `/:your_parameter_name/`). I valori catturati vengono memorizzati nell'oggetto `req.params` utilizzando i nomi dei parametri come chiavi (ad esempio, `req.params.your_parameter_name`).

Quindi, ad esempio, consideriamo un URL codificato per contenere informazioni sugli utenti e sui libri: `http://localhost:3000/users/34/books/8989`. Possiamo estrarre queste informazioni come mostrato di seguito, con i parametri di percorso `userId` e `bookId`:

```js
app.get("/users/:userId/books/:bookId", (req, res) => {
  // Access userId via: req.params.userId
  // Access bookId via: req.params.bookId
  res.send(req.params);
});
```

I nomi dei parametri di route devono essere composti da "caratteri di parola" (A-Z, a-z, 0-9, e \_).

> [!NOTE]
> L'URL _/book/create_ verrà corrisposto da una route come `/book/:bookId` (perché `:bookId` è un segnaposto per _qualsiasi_ stringa, quindi `create` corrisponde). La prima route che corrisponde a un URL in entrata verrà utilizzata, quindi se si desidera elaborare specificamente URL `/book/create`, il loro gestore di route deve essere definito prima della route `/book/:bookId`.

Questo è tutto ciò che serve per iniziare con le routes - se necessario, puoi trovare ulteriori informazioni nei documenti di Express: [Instradamento di base](https://expressjs.com/en/starter/basic-routing.html) e [Guida all'instradamento](https://expressjs.com/en/guide/routing.html). Le sezioni seguenti mostrano come configureremo le nostre route e controller per la LocalLibrary.

### Gestire errori nelle funzioni di route

Le funzioni di route mostrate in precedenza hanno tutte argomenti `req` e `res`, che rappresentano rispettivamente la richiesta e la risposta.
Le funzioni di route sono anche chiamate con un terzo argomento `next`, che può essere usato per passare errori alla catena del middleware Express.

Il codice sotto mostra come funziona, usando l'esempio di una query al database che prende una funzione di callback, e restituisce un errore `err` o alcuni risultati.
Se `err` è restituito, `next` viene chiamato con `err` come valore nel suo primo parametro (alla fine l'errore si propaga al nostro codice globale di gestione degli errori).
In caso di successo i dati desiderati vengono restituiti e poi utilizzati nella risposta.

```js
router.get("/about", (req, res, next) => {
  About.find({}).exec((err, queryResults) => {
    if (err) {
      return next(err);
    }
    // Successful, so render
    res.render("about_view", { title: "About", list: queryResults });
  });
});
```

### Gestione delle eccezioni nelle funzioni di route

La sezione precedente mostra come Express si aspetti che le funzioni di route restituiscano errori.
Il framework è progettato per l'uso con funzioni asincrone che prendono una funzione di callback (con un errore e un argomento di risultato), che viene chiamata quando l'operazione termina.
Questo è un problema perché più avanti faremo query al database con Mongoose che utilizzano API basate su [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise), e che potrebbero generare eccezioni nelle nostre funzioni di route (piuttosto che restituire errori in un callback).

Affinché il framework gestisca correttamente le eccezioni, esse devono essere catturate e poi inoltrate come errori come mostrato nella sezione precedente.

> [!NOTE]
> Express 5, che attualmente è in beta, dovrebbe gestire nativamente le eccezioni JavaScript.

Re-immaginando il semplice esempio della sezione precedente con `About.find().exec()` come query al database che restituisce una promessa, potremmo scrivere la funzione di route all'interno di un blocco [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) in questo modo:

```js
exports.get("/about", async function (req, res, next) {
  try {
    const successfulResult = await About.find({}).exec();
    res.render("about_view", { title: "About", list: successfulResult });
  } catch (error) {
    return next(error);
  }
});
```

Questo è un bel po' di codice boilerplate da aggiungere a ogni funzione.
Invece, per questo tutorial useremo il modulo [express-async-handler](https://www.npmjs.com/package/express-async-handler).
Questo definisce una funzione wrapper che nasconde il blocco `try...catch` e il codice per inoltrare l'errore.
Lo stesso esempio ora è molto semplice, perché dobbiamo solo scrivere il codice per il caso in cui assumiamo successo:

```js
// Import the module
const asyncHandler = require("express-async-handler");

exports.get(
  "/about",
  asyncHandler(async (req, res, next) => {
    const successfulResult = await About.find({}).exec();
    res.render("about_view", { title: "About", list: successfulResult });
  }),
);
```

## Routes necessarie per la LocalLibrary

Gli URL che alla fine ci serviranno per le nostre pagine sono elencati di seguito, dove _object_ è sostituito dal nome di ciascuno dei nostri modelli (book, bookinstance, genre, author), _objects_ è il plurale di object, e _id_ è il campo istanza univoco (`_id`) che viene assegnato a ciascuna istanza del modello Mongoose per impostazione predefinita.

- `catalog/` — La pagina principale/indice.
- `catalog/<objects>/` — L'elenco di tutti i libri, bookinstance, generi, o autori (es., /`catalog/books/`, /`catalog/genres/`, ecc.)
- `catalog/<object>/<id>` — La pagina di dettaglio per un libro, bookinstance, genere o autore specifico con il valore di campo `_id` indicato (es., `/catalog/book/584493c1f4887f06c0e67d37`).
- `catalog/<object>/create` — Il modulo per creare un nuovo libro, bookinstance, genere o autore (es., `/catalog/book/create`).
- `catalog/<object>/<id>/update` — Il modulo per aggiornare un libro, bookinstance, genere o autore specifico con il valore di campo `_id` indicato (es., `/catalog/book/584493c1f4887f06c0e67d37/update`).
- `catalog/<object>/<id>/delete` — Il modulo per eliminare un libro, bookinstance, genere o autore specifico con il valore di campo `_id` indicato (es., `/catalog/book/584493c1f4887f06c0e67d37/delete`).

La prima home page e le pagine elenco non codificano alcuna informazione aggiuntiva. Mentre i risultati restituiti dipenderanno dal tipo di modello e dal contenuto nel database, le query eseguite per ottenere le informazioni saranno sempre le stesse (allo stesso modo il codice eseguito per la creazione di oggetti sarà sempre simile).

Per contro, gli altri URL vengono usati per agire su un documento/istanza di modello specifico—questi codificano l'identità dell'elemento nell'URL (mostrato come `<id>` sopra). Utilizzeremo parametri di percorso per estrarre le informazioni codificate e passarle al gestore di route (e in un articolo successivo useremo questo per determinare dinamicamente quali informazioni ottenere dal database). Codificando l'informazione nel nostro URL abbiamo bisogno solo di una route per ogni risorsa di un particolare tipo (ad esempio, una route per gestire la visualizzazione di ogni singolo libro).

> [!NOTE]
> Express permette di costruire i tuoi URL nel modo che preferisci — puoi codificare le informazioni nel corpo dell'URL come mostrato sopra o usare parametri URL `GET` (ad esempio, `/book/?id=6`). Indipendentemente dall'approccio usato, gli URL dovrebbero essere mantenuti puliti, logici e leggibili ([consulta i consigli del W3C qui](https://www.w3.org/Provider/Style/URI)).

Successivamente creiamo le nostre funzioni di callback dei gestori di route e il codice di route per tutti gli URL sopra.

## Creare le funzioni callback del gestore di route

Prima di definire le nostre routes, creeremo prima tutte le funzioni di callback dummy/scheletro che verranno invocate. I callback saranno memorizzati in moduli "controller" separati per `Book`, `BookInstance`, `Genre`, e `Author` (è possibile utilizzare qualsiasi struttura di file/modulo, ma questa sembra un'appropriatezza granularità per questo progetto).

Inizia creando una cartella per i nostri controller nella radice del progetto (**/controllers**) e poi crea file/moduli di controller separati per gestire ciascuno dei modelli:

```plain
/express-locallibrary-tutorial  # the project root
  /controllers
    authorController.js
    bookController.js
    bookinstanceController.js
    genreController.js
```

I controller utilizzeranno il modulo `express-async-handler`, quindi prima di procedere, installalo nella libreria usando `npm`:

```bash
npm install express-async-handler
```

### Controller Author

Apri il file **/controllers/authorController.js** e digita il seguente codice:

```js
const Author = require("../models/author");
const asyncHandler = require("express-async-handler");

// Display list of all Authors.
exports.author_list = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author list");
});

// Display detail page for a specific Author.
exports.author_detail = asyncHandler(async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: Author detail: ${req.params.id}`);
});

// Display Author create form on GET.
exports.author_create_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author create GET");
});

// Handle Author create on POST.
exports.author_create_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author create POST");
});

// Display Author delete form on GET.
exports.author_delete_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author delete GET");
});

// Handle Author delete on POST.
exports.author_delete_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author delete POST");
});

// Display Author update form on GET.
exports.author_update_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author update GET");
});

// Handle Author update on POST.
exports.author_update_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Author update POST");
});
```

Il modulo prima richiede il modello `Author` che useremo poi per accedere e aggiornare i nostri dati, e il wrapper `asyncHandler` che useremo per catturare eventuali eccezioni lanciate nelle nostre funzioni gestore di route.
Poi esporta le funzioni per ciascuno degli URL che desideriamo gestire.
Nota che le operazioni di creazione, aggiornamento ed eliminazione utilizzano moduli e quindi hanno anche metodi aggiuntivi per gestire le richieste post del modulo — discuteremo quei metodi nell'articolo "forms" più avanti.

Le funzioni usano tutte la funzione wrapper descritta sopra in [Gestione delle eccezioni nelle funzioni di route](#gestione_delle_eccezioni_nelle_funzioni_di_route), con argomenti per la richiesta, la risposta e next.
Le funzioni rispondono con una stringa che indica che la pagina associata non è ancora stata creata.
Se una funzione del controller dovrebbe ricevere parametri di percorso, questi vengono mostrati nella stringa del messaggio (vedi `req.params.id` sopra).

Nota che una volta implementato, alcune funzioni di route potrebbero non contenere codice che può lanciare eccezioni.
Possiamo riportare quelle a funzioni gestore di route "normali" quando arriveremo a loro.

#### Controller BookInstance

Apri il file **/controllers/bookinstanceController.js** e copia il seguente codice (segue un modello identico al modulo controller `Author`):

```js
const BookInstance = require("../models/bookinstance");
const asyncHandler = require("express-async-handler");

// Display list of all BookInstances.
exports.bookinstance_list = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance list");
});

// Display detail page for a specific BookInstance.
exports.bookinstance_detail = asyncHandler(async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: BookInstance detail: ${req.params.id}`);
});

// Display BookInstance create form on GET.
exports.bookinstance_create_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance create GET");
});

// Handle BookInstance create on POST.
exports.bookinstance_create_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance create POST");
});

// Display BookInstance delete form on GET.
exports.bookinstance_delete_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance delete GET");
});

// Handle BookInstance delete on POST.
exports.bookinstance_delete_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance delete POST");
});

// Display BookInstance update form on GET.
exports.bookinstance_update_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance update GET");
});

// Handle bookinstance update on POST.
exports.bookinstance_update_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: BookInstance update POST");
});
```

#### Controller Genre

Apri il file **/controllers/genreController.js** e copia il testo seguente (segue un modello identico ai file `Author` e `BookInstance`):

```js
const Genre = require("../models/genre");
const asyncHandler = require("express-async-handler");

// Display list of all Genre.
exports.genre_list = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre list");
});

// Display detail page for a specific Genre.
exports.genre_detail = asyncHandler(async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: Genre detail: ${req.params.id}`);
});

// Display Genre create form on GET.
exports.genre_create_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre create GET");
});

// Handle Genre create on POST.
exports.genre_create_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre create POST");
});

// Display Genre delete form on GET.
exports.genre_delete_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre delete GET");
});

// Handle Genre delete on POST.
exports.genre_delete_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre delete POST");
});

// Display Genre update form on GET.
exports.genre_update_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre update GET");
});

// Handle Genre update on POST.
exports.genre_update_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Genre update POST");
});
```

#### Controller Book

Apri il file **/controllers/bookController.js** e copia il seguente codice.
Segue lo stesso modello degli altri moduli controller, ma ha inoltre una funzione `index()` per visualizzare la pagina di benvenuto del sito:

```js
const Book = require("../models/book");
const asyncHandler = require("express-async-handler");

exports.index = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
});

// Display list of all books.
exports.book_list = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book list");
});

// Display detail page for a specific book.
exports.book_detail = asyncHandler(async (req, res, next) => {
  res.send(`NOT IMPLEMENTED: Book detail: ${req.params.id}`);
});

// Display book create form on GET.
exports.book_create_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book create GET");
});

// Handle book create on POST.
exports.book_create_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book create POST");
});

// Display book delete form on GET.
exports.book_delete_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book delete GET");
});

// Handle book delete on POST.
exports.book_delete_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book delete POST");
});

// Display book update form on GET.
exports.book_update_get = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book update GET");
});

// Handle book update on POST.
exports.book_update_post = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Book update POST");
});
```

## Creare il modulo di route del catalogo

Successivamente, creiamo _route_ per tutti gli URL [necessari al sito web LocalLibrary](#routes_necessarie_per_la_locallibrary), che chiameranno le funzioni del controller che abbiamo definito nelle sezioni precedenti.

Lo scheletro ha già una cartella **./routes** contenente route per l'_index_ e gli _users_.
Crea un altro file route — **catalog.js** — all'interno di questa cartella, come mostrato.

```plain
/express-locallibrary-tutorial # the project root
  /routes
    index.js
    users.js
    catalog.js
```

Apri **/routes/catalog.js** e copia il codice sotto:

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

Il modulo richiede Express e poi lo usa per creare un oggetto `Router`. Le rotte sono tutte impostate sul router, che viene poi esportato.

Le rotte sono definite utilizzando metodi `.get()` o `.post()` sull'oggetto router. Tutti i percorsi sono definiti usando stringhe (non usiamo pattern di stringa o espressioni regolari).
Le rotte che agiscono su una risorsa specifica (ad es., libro) utilizzano parametri di percorso per ottenere l'id dell'oggetto dall'URL.

Le funzioni del gestore sono tutte importate dai moduli dei controller creati nella sezione precedente.

### Aggiornare il modulo di route index

Abbiamo impostato tutte le nostre nuove rotte, ma abbiamo ancora una rotta alla pagina originale. Invece, reindirizziamo a questa la nuova pagina di indice che abbiamo creato al percorso '/catalog'.

Apri **/routes/index.js** e sostituisci la rotta esistente con la funzione qui sotto.

```js
// GET home page.
router.get("/", function (req, res) {
  res.redirect("/catalog");
});
```

> [!NOTE]
> Questo è il nostro primo utilizzo del metodo di risposta [redirect()](https://expressjs.com/en/4x/api.html#res.redirect). Questo reindirizza alla pagina specificata, inviando per default il codice di stato HTTP "302 Found". È possibile cambiare il codice di stato restituito se necessario e fornire percorsi assoluti o relativi.

### Aggiornare app.js

L'ultimo passo è aggiungere le rotte alla catena del middleware.
Facciamolo in `app.js`.

Apri **app.js** e richiedi il percorso del catalogo sotto le altre routes (aggiungi la terza riga mostrata sotto, sotto le altre due che dovrebbero essere già presenti nel file):

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
const catalogRouter = require("./routes/catalog"); // Import routes for "catalog" area of site
```

Successivamente, aggiungi la rotta del catalogo allo stack middleware sotto le altre route (aggiungi la terza riga mostrata sotto, sotto le altre due che dovrebbero essere già presenti nel file):

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
app.use("/catalog", catalogRouter); // Add catalog routes to middleware chain.
```

> [!NOTE]
> Abbiamo aggiunto il nostro modulo del catalogo a un percorso `'/catalog'`. Questo viene preposto a tutti i percorsi definiti nel modulo del catalogo. Quindi, ad esempio, per accedere a un elenco di libri, l'URL sarà: `/catalog/books/`.

Questo è tutto. Dovremmo ora avere rotte e funzioni scheletro abilitate per tutti gli URL che supporteremo eventualmente sul sito web della LocalLibrary.

### Testare le routes

Per testare le rotte, prima avvia il sito web usando il tuo solito approccio.

- Metodo predefinito

  ```bash
  # Windows
  SET DEBUG=express-locallibrary-tutorial:* & npm start

  # macOS or Linux
  DEBUG=express-locallibrary-tutorial:* npm start
  ```

- Se hai precedentemente configurato [nodemon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website#enable_server_restart_on_file_changes), puoi invece usare:

  ```bash
  npm run serverstart
  ```

Quindi naviga in un numero di URL della LocalLibrary e verifica di non ricevere una pagina di errore (HTTP 404). Un piccolo insieme di URL è elencato di seguito per tua comodità:

- `http://localhost:3000/`
- `http://localhost:3000/catalog`
- `http://localhost:3000/catalog/books`
- `http://localhost:3000/catalog/bookinstances/`
- `http://localhost:3000/catalog/authors/`
- `http://localhost:3000/catalog/genres/`
- `http://localhost:3000/catalog/book/5846437593935e2f8c2aa226`
- `http://localhost:3000/catalog/book/create`

## Riepilogo

Abbiamo ora creato tutte le routes per il nostro sito, insieme a funzioni di controller dummy che possiamo popolare con un'implementazione completa in articoli successivi. Lungo il percorso abbiamo imparato molte informazioni fondamentali sulle route di Express, la gestione delle eccezioni e alcuni approcci per strutturare le nostre route e i controller.

Nel nostro prossimo articolo, creeremo una pagina di benvenuto appropriata per il sito, utilizzando view (template) e informazioni memorizzate nei nostri modelli.

## Vedi anche

- [Instradamento di base](https://expressjs.com/en/starter/basic-routing.html) (Documentazione Express)
- [Guida all'instradamento](https://expressjs.com/en/guide/routing.html) (Documentazione Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
