---
title: "Tutorial Express Parte 4: Route e controller"
short-title: "4: Route e controller"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/routes
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo tutorial imposteremo le route (codice che gestisce gli URL) con funzioni gestori "fittizie" per tutti i punti di accesso alle risorse di cui avremo alla fine bisogno nel sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Al termine avremo una struttura modulare per il nostro codice di gestione delle route, che possiamo estendere con funzioni gestori reali nei seguenti articoli. Avremo anche una buona comprensione di come creare route modulari utilizzando Express!

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction">introduzione a Express/Node</a>.
        Completare i precedenti argomenti del tutorial (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose">Tutorial Express Parte 3: Utilizzo di un Database (con Mongoose)</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come creare semplici route.
        Impostare tutti i nostri endpoint URL.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Nell'[ultimo articolo del tutorial](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose) abbiamo definito i modelli _Mongoose_ per interagire con il database e utilizzato uno script (indipendente) per creare alcuni record iniziali della libreria. Ora possiamo scrivere il codice per presentare tali informazioni agli utenti. La prima cosa che dobbiamo fare è determinare quali informazioni vogliamo poter visualizzare nelle nostre pagine, e poi definire URL appropriati per restituire quelle risorse. Poi dovremo creare le route (gestori URL) e le viste (template) per visualizzare quelle pagine.

Il diagramma seguente è fornito come promemoria del flusso principale di dati e delle cose che devono essere implementate quando si gestisce una richiesta/risposta HTTP. Oltre alle viste e alle route, il diagramma mostra i "controller" — funzioni che separano il codice per instradare le richieste dal codice che effettivamente elabora le richieste.

Poiché abbiamo già creato i modelli, le principali cose che dovremo creare sono:

- "Route" per inoltrare le richieste supportate (e qualsiasi informazione codificata negli URL delle richieste) alle funzioni controller appropriate.
- Funzioni controller per ottenere i dati richiesti dai modelli, creare una pagina HTML che visualizza i dati e restituirla all'utente per la visualizzazione nel browser.
- Viste (template) utilizzate dai controller per rendere i dati.

![Diagramma del flusso principale di dati di un server MVC express: 'Routes' ricevono le richieste HTTP inviate al server Express e le inoltrano alla funzione 'controller' appropriata. Il controller legge e scrive i dati dai modelli. I modelli sono connessi al database per fornire accesso ai dati al server. I controller utilizzano 'views', chiamati anche template, per rendere i dati. Il Controller invia la risposta HTML HTTP al client come risposta HTTP.](mvc_express.png)

Alla fine potremmo avere pagine per mostrare elenchi e informazioni dettagliate per libri, generi, autori e istanze di libri, insieme a pagine per creare, aggiornare ed eliminare i record. È molto da documentare in un articolo. Pertanto la maggior parte di questo articolo sarà incentrata sulla configurazione delle nostre route e controller per restituire contenuti "fittizi". Estenderemo i metodi del controller nei nostri articoli successivi per lavorare con i dati del modello.

La prima sezione sottostante fornisce un breve "primer" su come utilizzare il middleware Express [Router](https://expressjs.com/en/4x/api.html#router). Useremo poi questa conoscenza nelle sezioni seguenti quando imposteremo le route di LocalLibrary.

## Primer sulle route

Una route è una sezione di codice di Express che associa un verbo HTTP (`GET`, `POST`, `PUT`, `DELETE`, ecc.), un percorso/pattern URL, e una funzione che viene chiamata per gestire quel pattern.

Ci sono diversi modi per creare route. Per questo tutorial useremo il middleware [`express.Router`](https://expressjs.com/en/guide/routing.html#express-router) poiché ci permette di raggruppare insieme i gestori delle route per una particolare parte di un sito e accedervi utilizzando un prefisso comune. Terremo tutte le nostre route relative alla libreria in un modulo "catalogo" e, se aggiungiamo route per gestire account utente o altre funzioni, possiamo tenerle raggruppate separatamente.

> [!NOTE]
> Abbiamo discusso brevemente delle route dell'applicazione Express nella nostra [Introduzione a Express > Creazione di gestori di route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#creating_route_handlers). Oltre a fornire un migliore supporto per la modularizzazione (come discusso nella prima sottosezione sotto), utilizzare _Router_ è molto simile a definire route direttamente sull'_oggetto applicazione Express_.

Il resto di questa sezione fornisce una panoramica su come il `Router` può essere utilizzato per definire le route.

### Definizione e utilizzo di moduli di route separati

Il codice sottostante fornisce un esempio concreto di come possiamo creare un modulo di route e poi utilizzarlo in un'applicazione _Express_.

Prima creiamo le route per il wiki in un modulo denominato **wiki.js**. Il codice prima importa l'oggetto applicazione Express, lo usa per ottenere un oggetto `Router` e quindi aggiunge un paio di route usando il metodo `get()`. Infine, il modulo esporta l'oggetto `Router`.

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
> Sopra stiamo definendo i nostri callback dei gestori di route direttamente nelle funzioni del router. In LocalLibrary definiremo questi callback in un modulo controller separato.

Per utilizzare il modulo router nel file principale dell'app prima `require()` il modulo della route (**wiki.js**). Poi chiamiamo `use()` sull'applicazione _Express_ per aggiungere il Router al percorso di gestione del middleware, specificando un percorso URL di 'wiki'.

```js
const wiki = require("./wiki.js");
// …
app.use("/wiki", wiki);
```

Le due route definite nel nostro modulo di route wiki sono quindi accessibili da `/wiki/` e `/wiki/about/`.

### Funzioni route

Il nostro modulo sopra definisce un paio di funzioni route tipiche. La route "about" (riprodotta sotto) è definita utilizzando il metodo `Router.get()`, che risponde solo alle richieste HTTP GET. Il primo argomento di questo metodo è il percorso URL mentre il secondo è una funzione di callback che verrà invocata se si riceve una richiesta HTTP GET con il percorso.

```js
router.get("/about", function (req, res) {
  res.send("About this wiki");
});
```

Il callback assume tre argomenti (solitamente nominati come mostrato: `req`, `res`, `next`), che conterranno l'oggetto Request HTTP, la risposta HTTP e la funzione _next_ nella catena middleware.

> [!NOTE]
> Le funzioni router sono [middleware di Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#using_middleware), il che significa che devono completare (rispondere a) la richiesta o chiamare la funzione `next` nella catena. Nel caso sopra completiamo la richiesta utilizzando `send()`, quindi l'argomento `next` non è utilizzato (e scegliamo di non specificarlo).
>
> La funzione router sopra accetta un singolo callback, ma si possono specificare quanti callback si desiderano, o un array di funzioni callback. Ogni funzione fa parte della catena middleware e verrà chiamata nell'ordine in cui viene aggiunta alla catena (a meno che una funzione precedente completi la richiesta).

La funzione callback qui chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "About this wiki" quando riceviamo una richiesta GET con il percorso (`/about`). Ci sono [numerosi altri metodi di risposta](https://expressjs.com/en/guide/routing.html#response-methods) per terminare il ciclo di richiesta/risposta. Per esempio, si potrebbe chiamare [`res.json()`](https://expressjs.com/en/4x/api.html#res.json) per inviare una risposta JSON o [`res.sendFile()`](https://expressjs.com/en/4x/api.html#res.sendFile) per inviare un file. Il metodo di risposta che useremo più frequentemente mentre costruiamo la libreria è [`render()`](https://expressjs.com/en/4x/api.html#res.render), che crea e restituisce file HTML utilizzando template e dati — ne parleremo molto di più in un articolo successivo!

### Verbi HTTP

Le route di esempio sopra usano il metodo `Router.get()` per rispondere alle richieste HTTP GET con un certo percorso.

Il `Router` fornisce anche metodi di route per tutti gli altri verbi HTTP, utilizzati principalmente nello stesso modo: `post()`, `put()`, `delete()`, `options()`, `trace()`, `copy()`, `lock()`, `mkcol()`, `move()`, `purge()`, `propfind()`, `proppatch()`, `unlock()`, `report()`, `mkactivity()`, `checkout()`, `merge()`, `m-search()`, `notify()`, `subscribe()`, `unsubscribe()`, `patch()`, `search()`, e `connect()`.

Per esempio, il codice sotto si comporta proprio come la precedente route `/about`, ma risponde solo alle richieste HTTP POST.

```js
router.post("/about", (req, res) => {
  res.send("About this wiki");
});
```

### Percorsi delle route

I percorsi delle route definiscono gli endpoint a cui possono essere effettuate richieste. Gli esempi che abbiamo visto finora sono stati solo stringhe e sono usati esattamente come scritti: '/', '/about', '/book', '/any-random.path'.

I percorsi delle route possono anche essere pattern di stringhe. I pattern di stringhe usano una forma di sintassi delle espressioni regolari per definire _pattern_ di endpoint che verranno abbinati. La sintassi è elencata di seguito (nota che il trattino (`-`) e il punto (`.`) sono interpretati letteralmente dai percorsi basati su stringhe):

- `?` : L'endpoint deve avere 0 o 1 del carattere (o gruppo) precedente, ad esempio, un percorso della route di `'/ab?cd'` corrisponderà agli endpoint `acd` o `abcd`.
- `+` : L'endpoint deve avere 1 o più del carattere (o gruppo) precedente, ad esempio, un percorso della route di `'/ab+cd'` corrisponderà agli endpoint `abcd`, `abbcd`, `abbbcd`, e così via.
- `*` : L'endpoint può avere una stringa arbitraria dove è posizionato il carattere `*`. Ad esempio un percorso della route di `'/ab*cd'` corrisponderà agli endpoint `abcd`, `abXcd`, `abSOMErandomTEXTcd`, e così via.
- `()` : Abbinamento di un gruppo di caratteri per eseguire un'altra operazione, ad esempio, `'/ab(cd)?e'` eseguirà un abbinamento `?` sul gruppo `(cd)` — corrisponderà a `abe` e `abcde`.

I percorsi delle route possono anche essere espressioni regolari JavaScript [regular expressions](/it/docs/Web/JavaScript/Guide/Regular_expressions). Ad esempio, il percorso della route sottostante corrisponderà a `catfish` e `dogfish`, ma non a `catflap`, `catfishhead`, e così via. Nota che il percorso per un'espressione regolare utilizza la sintassi delle espressioni regolari (non è una stringa quotata come nei casi precedenti).

```js
app.get(/.*fish$/, function (req, res) {
  // …
});
```

> [!NOTE]
> La maggior parte delle nostre route per LocalLibrary utilizzerà stringhe e non espressioni regolari. Utilizzeremo anche i parametri di route come discussi nella sezione successiva.

### Parametri di route

I parametri di route sono _segmenti URLs nominati_ utilizzati per catturare i valori in posizioni specifiche nell'URL. I segmenti nominati sono prefissi da un due punti e poi dal nome (Es., `/:your_parameter_name/`). I valori catturati sono memorizzati nell'oggetto `req.params` utilizzando i nomi dei parametri come chiavi (Es., `req.params.your_parameter_name`).

Quindi, per esempio, considera un URL codificato per contenere informazioni su utenti e libri: `http://localhost:3000/users/34/books/8989`. Possiamo estrarre queste informazioni come mostrato sotto, con i parametri `userId` e `bookId`:

```js
app.get("/users/:userId/books/:bookId", (req, res) => {
  // Access userId via: req.params.userId
  // Access bookId via: req.params.bookId
  res.send(req.params);
});
```

I nomi dei parametri di route devono essere composti da "caratteri di parola" (A-Z, a-z, 0-9 e \_).

> [!NOTE]
> URL come _/book/create_ sarà abbinato da una route come `/book/:bookId` (perché `:bookId` è un segnaposto per _qualsiasi_ stringa, quindi `create` corrisponde). La prima route che corrisponde a un URL in entrata sarà utilizzata, quindi se si desidera elaborare specificamente gli URL `/book/create`, il loro gestore di route deve essere definito prima della route `/book/:bookId`.

Questo è tutto ciò che serve per iniziare con le route - se necessario puoi trovare ulteriori informazioni nei documenti di Express: [Routing di base](https://expressjs.com/en/starter/basic-routing.html) e [Guida al routing](https://expressjs.com/en/guide/routing.html). Le sezioni seguenti mostrano come configureremo le nostre route e controller per LocalLibrary.

### Gestire gli errori nelle funzioni di route

Le funzioni di route mostrate in precedenza hanno tutte argomenti `req` e `res`, che rappresentano rispettivamente la richiesta e la risposta.
Le funzioni di route sono anche chiamate con un terzo argomento `next`, che può essere utilizzato per passare errori alla catena middleware di Express.

Il codice sottostante mostra come funziona, utilizzando l'esempio di una query al database che richiede una funzione callback, e restituisce un errore `err` o alcuni risultati.
Se viene restituito `err`, `next` viene chiamato con `err` come valore nel suo primo parametro (l'errore alla fine si propaga al nostro codice globale di gestione degli errori).
In caso di successo i dati desiderati vengono restituiti e quindi utilizzati nella risposta.

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

### Gestire le eccezioni nelle funzioni di route

La sezione precedente mostra come Express si aspetta che le funzioni di route restituiscano errori.
Il framework è progettato per l'uso con funzioni asincrone che richiedono una funzione di callback (con un argomento di errore e risultato), che viene chiamata al completamento dell'operazione.
Questo è un problema perché più avanti effettueremo query al database Mongoose che utilizzano API basate su [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise) e che potrebbero generare eccezioni nelle nostre funzioni di route (anziché restituire errori in un callback).

Affinché il framework gestisca correttamente le eccezioni, queste devono essere catturate e quindi inoltrate come errori come mostrato nella sezione precedente.

> [!NOTE]
> Express 5, attualmente in beta, dovrebbe gestire le eccezioni JavaScript nativamente.

Riconcependo il semplice esempio della sezione precedente con `About.find().exec()` come una query al database che restituisce una promessa, potremmo scrivere la funzione di route all'interno di un blocco [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) come questo:

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

Quindi c'è un bel po' di codice boilerplate da aggiungere a ogni funzione.
Invece, per questo tutorial utilizzeremo il modulo [express-async-handler](https://www.npmjs.com/package/express-async-handler).
Questo definisce una funzione wrapper che nasconde il blocco `try...catch` e il codice per inoltrare l'errore.
Lo stesso esempio ora è molto semplice, perché dobbiamo solo scrivere codice per il caso in cui assumiamo successo:

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

## Route necessarie per LocalLibrary

Gli URL di cui alla fine avremo bisogno per le nostre pagine sono elencati di seguito, dove _object_ viene sostituito dal nome di ciascuno dei nostri modelli (book, bookinstance, genre, author), _objects_ è il plurale di object, e _id_ è il campo univoco dell'istanza (`_id`) che viene assegnato a ciascuna istanza del modello Mongoose per impostazione predefinita.

- `catalog/` — La home page/indice.
- `catalog/<objects>/` — L'elenco di tutti i libri, istanze di libri, generi o autori (ad es., /`catalog/books/`, /`catalog/genres/`, ecc.)
- `catalog/<object>/<id>` — La pagina dettagliata per un libro specifico, un'istanza di libro, un genere o un autore con il valore del campo `_id` indicato (ad es., `/catalog/book/584493c1f4887f06c0e67d37)`).
- `catalog/<object>/create` — Il modulo per creare un nuovo libro, un'istanza di libro, un genere o un autore (ad es., `/catalog/book/create)`).
- `catalog/<object>/<id>/update` — Il modulo per aggiornare un libro specifico, un'istanza di libro, un genere o un autore con il valore del campo `_id` indicato (ad es., `/catalog/book/584493c1f4887f06c0e67d37/update)`).
- `catalog/<object>/<id>/delete` — Il modulo per eliminare un libro specifico, un'istanza di libro, un genere o un autore con il valore del campo `_id` indicato (ad es., `/catalog/book/584493c1f4887f06c0e67d37/delete)`).

La prima home page e le pagine di elenco non codificano alcuna informazione aggiuntiva. Mentre i risultati restituiti dipenderanno dal tipo di modello e dal contenuto del database, le query eseguite per ottenere le informazioni saranno sempre le stesse (in modo simile il codice eseguito per la creazione dell'oggetto sarà sempre simile).

Al contrario, gli altri URL vengono utilizzati per agire su un documento/istanza di modello specifico — codificano l'identità dell'elemento nell'URL (mostrato come `<id>` sopra). Utilizzeremo i parametri di percorso per estrarre le informazioni codificate e passarle al gestore della route (e in un articolo successivo utilizzeremo questo per determinare dinamicamente quali informazioni ottenere dal database). Codificando le informazioni nel nostro URL avremo bisogno solo di una route per ogni risorsa di un particolare tipo (ad es., una route per gestire la visualizzazione di ogni singolo elemento libro).

> [!NOTE]
> Express consente di costruire gli URL in qualsiasi modo si desideri — si possono codificare informazioni nel corpo dell'URL come mostrato sopra o utilizzare parametri URL `GET` (es., `/book/?id=6`). Qualunque approccio si utilizzi, gli URL dovrebbero essere mantenuti puliti, logici e leggibili ([controlla il consiglio W3C qui](https://www.w3.org/Provider/Style/URI)).

Successivamente creiamo le funzioni di callback per i gestori delle route e il codice della route per tutti gli URL sopra elencati.

## Creare le funzioni di callback dei gestori di route

Prima di definire le nostre route, creeremo prima tutte le funzioni di callback fittizie/scheletro che dovranno invocare. I callback saranno memorizzati in moduli "controller" separati per `Book`, `BookInstance`, `Genre`, e `Author` (puoi usare qualsiasi struttura di file/modulo, ma questo sembra un'adeguata granularità per questo progetto).

Inizia creando una cartella per i nostri controller nella radice del progetto (**/controllers**) e poi crea file/moduli controller separati per gestire ciascuno dei modelli:

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

Il modulo prima richiede il modello `Author` che utilizzeremo successivamente per accedere e aggiornare i nostri dati, e il wrapper `asyncHandler` che useremo per catturare eventuali eccezioni lanciate nelle nostre funzioni gestori di route.
Poi esporta funzioni per ciascuno degli URL che desideriamo gestire.
Nota che le operazioni di creazione, aggiornamento ed eliminazione utilizzano moduli, e quindi hanno metodi aggiuntivi per gestire le richieste POST dei moduli — discuteremo quei metodi nell'articolo sui "moduli" più avanti.

Le funzioni utilizzano tutte la funzione wrapper descritta sopra in [Gestire le eccezioni nelle funzioni di route](#gestire_le_eccezioni_nelle_fonctions_di_route), con argomenti per la richiesta, la risposta e il prossimo.
Le funzioni rispondono con una stringa che indica che la pagina associata non è stata ancora creata.
Se ci si aspetta che una funzione controller riceva parametri di percorso, questi vengono visualizzati nel messaggio (vedi `req.params.id` sopra).

Nota che una volta implementate, alcune funzioni di route potrebbero non contenere alcun codice che può lanciare eccezioni.
Possiamo cambiare quelle funzioni in funzioni gestori di route "normali" quando arriviamo a loro.

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

Apri il file **/controllers/genreController.js** e copia il seguente testo (segue un modello identico ai file `Author` e `BookInstance`):

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
Questo segue lo stesso modello degli altri moduli controller, ma ha anche una funzione `index()` per visualizzare la pagina di benvenuto del sito:

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

## Creare il modulo delle route del catalogo

Successivamente creiamo _route_ per tutti gli URL [necessari dal sito web di LocalLibrary](#route_necessarie_per_locallibrary), che chiameranno le funzioni controller che abbiamo definito nelle sezioni precedenti.

Lo scheletro ha già una cartella **./routes** contenente route per l'_index_ e gli _users_.
Crea un altro file di route — **catalog.js** — all'interno di questa cartella, come mostrato.

```plain
/express-locallibrary-tutorial # the project root
  /routes
    index.js
    users.js
    catalog.js
```

Apri **/routes/catalog.js** e copia il codice sottostante:

```js
const express = require("express");
const router = express.Router();

// Require controller modules.
const book_controller = require("../controllers/bookController");
const author_controller = require("../controllers/authorController");
const genre_controller = require("../controllers/genreController");
const book_instance_controller = require("../controllers/bookinstanceController");

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

Il modulo richiede Express e poi lo usa per creare un oggetto `Router`. Tutte le route sono impostate sul router, che viene poi esportato.

Le route sono definite utilizzando i metodi `.get()` o `.post()` sull'oggetto router. Tutti i percorsi sono definiti usando stringhe (non usiamo pattern di stringhe o espressioni regolari).
Le route che agiscono su qualche risorsa specifica (ad esempio, libro) utilizzano parametri di percorso per ottenere l'id dell'oggetto dall'URL.

Le funzioni gestori sono tutte importate dai moduli controller che abbiamo creato nella sezione precedente.

### Aggiornare il modulo della route index

Abbiamo impostato tutte le nostre nuove route, ma abbiamo ancora una route per la pagina originale. Preferiamo reindirizzare questa alla nuova pagina index che abbiamo creato al percorso '/catalog'.

Apri **/routes/index.js** e sostituisci la route esistente con la funzione seguente.

```js
// GET home page.
router.get("/", function (req, res) {
  res.redirect("/catalog");
});
```

> [!NOTE]
> Questo è il nostro primo utilizzo del metodo [redirect()](https://expressjs.com/en/4x/api.html#res.redirect) per la risposta. Questo reindirizza alla pagina specificata, di default inviando il codice di stato HTTP "302 Found". Puoi cambiare il codice di stato restituito se necessario, e fornire percorsi assoluti o relativi.

### Aggiornare app.js

L'ultimo passaggio è aggiungere le route alla catena del middleware.
Facciamo questo in `app.js`.

Apri **app.js** e richiedi la route del catalogo sotto le altre route (aggiungi la terza riga mostrata di seguito, sotto le altre due che dovrebbero essere già presenti nel file):

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
const catalogRouter = require("./routes/catalog"); // Import routes for "catalog" area of site
```

Successivamente, aggiungi la route del catalogo alla pila middleware sotto le altre route (aggiungi la terza riga mostrata di seguito, sotto le altre due che dovrebbero essere già presenti nel file):

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
app.use("/catalog", catalogRouter); // Add catalog routes to middleware chain.
```

> [!NOTE]
> Abbiamo aggiunto il nostro modulo catalogo a un percorso `'/catalog'`. Questo viene anteposto a tutti i percorsi definiti nel modulo catalogo. Quindi, ad esempio, per accedere a un elenco di libri, l'URL sarà: `/catalog/books/`.

Questo è tutto. Dovremmo ora avere route e funzioni scheletro abilitate per tutti gli URL che supporteremo eventualmente sul sito web LocalLibrary.

### Testare le route

Per testare le route, avvia prima il sito web utilizzando il tuo approccio usuale

- Il metodo predefinito

  ```bash
  # Windows
  SET DEBUG=express-locallibrary-tutorial:* & npm start

  # macOS or Linux
  DEBUG=express-locallibrary-tutorial:* npm start
  ```

- Se hai già configurato [nodemon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website#enable_server_restart_on_file_changes), puoi invece usare:

  ```bash
  npm run serverstart
  ```

Poi naviga verso una serie di URL di LocalLibrary e verifica che non ottieni una pagina di errore (HTTP 404). Un piccolo set di URL è elencato di seguito per tua comodità:

- `http://localhost:3000/`
- `http://localhost:3000/catalog`
- `http://localhost:3000/catalog/books`
- `http://localhost:3000/catalog/bookinstances/`
- `http://localhost:3000/catalog/authors/`
- `http://localhost:3000/catalog/genres/`
- `http://localhost:3000/catalog/book/5846437593935e2f8c2aa226`
- `http://localhost:3000/catalog/book/create`

## Riepilogo

Abbiamo ora creato tutte le route per il nostro sito, insieme a funzioni controller fittizie che possiamo popolare con un'implementazione completa in articoli successivi. Lungo il percorso abbiamo appreso molte informazioni fondamentali sulle route di Express, la gestione delle eccezioni, e alcuni approcci per strutturare le nostre route e i controller.

Nel nostro prossimo articolo creeremo una pagina di benvenuto appropriata per il sito, utilizzando viste (template) e informazioni memorizzate nei nostri modelli.

## Vedi anche

- [Routing di base](https://expressjs.com/en/starter/basic-routing.html) (documentazione Express)
- [Guida al routing](https://expressjs.com/en/guide/routing.html) (documentazione Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
