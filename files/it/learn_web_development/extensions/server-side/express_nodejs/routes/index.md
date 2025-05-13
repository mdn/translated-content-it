---
title: "Tutorial di Express Parte 4: Rotte e controller"
short-title: "4: Rotte e controller"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/routes
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo tutorial configureremo le rotte (codice per la gestione degli URL) con funzioni di gestione "fittizie" per tutti gli endpoint delle risorse che ci serviranno nel sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Alla fine avremo una struttura modulare per il nostro codice di gestione delle rotte, che potremo estendere con funzioni di gestione reali negli articoli successivi. Avremo anche una buona comprensione di come creare rotte modulari utilizzando Express!

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Legga l' <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction">Introduzione a Express/Node</a>.
        Completi i temi dei tutorial precedenti (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose">Express Tutorial Parte 3: Usare un Database (con Mongoose)</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come creare semplici rotte.
        Configurare tutti i nostri endpoint URL.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Nell'[ultimo articolo del tutorial](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose) abbiamo definito i modelli _Mongoose_ per interagire con il database e utilizzato uno script (autonomo) per creare alcuni record iniziali della libreria. Ora possiamo scrivere il codice per presentare quell'informazione agli utenti. La prima cosa che dobbiamo fare è determinare quali informazioni vogliamo essere in grado di visualizzare nelle nostre pagine e quindi definire URL appropriati per restituire quelle risorse. Poi dovremo creare le rotte (gestori URL) e le vista (template) per visualizzare quelle pagine.

Il diagramma seguente viene fornito come promemoria del flusso principale dei dati e delle cose che devono essere implementate quando si gestisce una richiesta/risposta HTTP. Oltre alle viste e alle rotte, il diagramma mostra i "controller" — funzioni che separano il codice per instradare le richieste dal codice che effettivamente le elabora.

Poiché abbiamo già creato i modelli, le cose principali che dovremo creare sono:

- "Rotte" per inoltrare le richieste supportate (e qualsiasi informazione codificata negli URL delle richieste) alle funzioni controller appropriate.
- Funzioni controller per ottenere i dati richiesti dai modelli, creare una pagina HTML che visualizzi i dati e restituirla all'utente per la visualizzazione nel browser.
- Viste (template) utilizzate dai controller per rendere i dati.

![Diagramma del principale flusso di dati di un server express MVC: le "rotte" ricevono le richieste HTTP inviate al server Express e le inoltrano alla funzione "controller" appropriata. Il controller legge e scrive i dati dai modelli. I modelli sono collegati al database per fornire l'accesso ai dati al server. I controller utilizzano 'views', chiamate anche templates, per renderizzare i dati. Il Controller invia la risposta HTML HTTP al client come una risposta HTTP.](mvc_express.png)

In definitiva potremmo avere pagine per mostrare elenchi e informazioni dettagliate per libri, generi, autori e copie di libri, insieme a pagine per creare, aggiornare e cancellare i record. È molto da documentare in un solo articolo. Pertanto, la maggior parte di questo articolo si concentrerà sulla configurazione delle nostre rotte e controller per restituire contenuti "fittizi". Estenderemo i metodi del controller nei nostri articoli successivi per lavorare con i dati del modello.

La prima sezione seguente fornisce una breve "introduzione" su come utilizzare il middleware Express [Router](https://expressjs.com/en/4x/api.html#router). Utilizzeremo poi tale conoscenza nelle sezioni successive quando configureremo le rotte del LocalLibrary.

## Introduzione alle rotte

Una rotta è una sezione del codice Express che associa un verbo HTTP (`GET`, `POST`, `PUT`, `DELETE`, ecc.), un percorso/pattern URL e una funzione che viene chiamata per gestire quel pattern.

Esistono diversi modi per creare rotte. Per questo tutorial utilizzeremo il middleware [`express.Router`](https://expressjs.com/en/guide/routing.html#express-router) poiché ci consente di raggruppare i gestori delle rotte per una parte specifica di un sito together e accedervi utilizzando un prefisso comune delle rotte. Teniamo tutte le nostre rotte relative alla biblioteca in un modulo "catalog", e, se aggiungiamo rotte per gestire account utente o altre funzioni, possiamo tenerle separate in gruppi diversi.

> [!NOTE]
> Abbiamo discusso brevemente delle rotte dell'applicazione Express nella nostra [Introduzione a Express > Creazione dei gestori delle rotte](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#creating_route_handlers). Oltre a fornire un miglior supporto per la modularità (come discusso nella prima sottosezione qui sotto), usare _Router_ è molto simile alla definizione delle rotte direttamente sull'_oggetto dell'applicazione Express_.

Il resto di questa sezione fornisce una panoramica su come `Router` può essere utilizzato per definire le rotte.

### Definizione e utilizzo di moduli di rotta separati

Il codice di seguito fornisce un esempio concreto di come possiamo creare un modulo di rotta e poi usarlo in un'applicazione _Express_.

Per prima cosa creiamo rotte per un wiki in un modulo chiamato **wiki.js**. Il codice importa prima l'oggetto dell'applicazione Express, lo utilizza per ottenere un oggetto `Router` e poi aggiunge un paio di rotte ad esso utilizzando il metodo `get()`. Infine il modulo esporta l'oggetto `Router`.

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
> Sopra stiamo definendo i nostri callback dei gestori delle rotte direttamente nelle funzioni del router. Nel LocalLibrary definiremo questi callback in un modulo controller separato.

Per utilizzare il modulo router nel nostro file app principale, dobbiamo prima `require()` il modulo della rotta (**wiki.js**). Poi chiamiamo `use()` sull'applicazione _Express_ per aggiungere il Router al percorso di gestione del middleware, specificando un percorso URL di 'wiki'.

```js
const wiki = require("./wiki.js");
// …
app.use("/wiki", wiki);
```

Le due rotte definite nel nostro modulo delle rotte wiki sono poi accessibili da `/wiki/` e `/wiki/about/`.

### Funzioni di rotta

Il nostro modulo sopracitato definisce un paio di tipiche funzioni di rotta. La rotta "about" (riprodotta in basso) è definita utilizzando il metodo `Router.get()`, che risponde solo alle richieste GET HTTP. Il primo argomento di questo metodo è il percorso URL mentre il secondo è una funzione di callback che verrà invocata se viene ricevuta una richiesta GET HTTP con il percorso.

```js
router.get("/about", function (req, res) {
  res.send("About this wiki");
});
```

Il callback prende tre argomenti (solitamente nominati come mostrato: `req`, `res`, `next`), che conterranno l'oggetto di richiesta HTTP, la risposta HTTP e la funzione _next_ nella catena dei middleware.

> [!NOTE]
> Le funzioni del router sono [middleware di Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#using_middleware), il che significa che devono completare (rispondere a) la richiesta o chiamare la funzione `next` nella catena. Nel caso sopra completiamo la richiesta usando `send()`, quindi l'argomento `next` non viene utilizzato (e scegliamo di non specificarlo).
>
> La funzione del router sopra prende un singolo callback, ma è possibile specificare quanti argomenti di callback si desidera, o un array di funzioni di callback. Ogni funzione fa parte della catena del middleware e verrà chiamata nell'ordine in cui viene aggiunta alla catena (a meno che una funzione precedente completi la richiesta).

La funzione di callback qui chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "About this wiki" quando riceviamo una richiesta GET con il percorso (`/about`). Ci sono un [numero di altri metodi di risposta](https://expressjs.com/en/guide/routing.html#response-methods) per terminare il ciclo di richiesta/risposta. Per esempio, si potrebbe chiamare [`res.json()`](https://expressjs.com/en/4x/api.html#res.json) per inviare una risposta JSON o [`res.sendFile()`](https://expressjs.com/en/4x/api.html#res.sendFile) per inviare un file. Il metodo di risposta che useremo più spesso mentre costruiamo la libreria è [`render()`](https://expressjs.com/en/4x/api.html#res.render), che crea e restituisce file HTML utilizzando template e dati—parleremo molto di più di ciò in un articolo successivo!

### Verbi HTTP

Gli esempi di sopra delle rotte utilizzano il metodo `Router.get()` per rispondere alle richieste GET HTTP con un certo percorso.

`Router` fornisce anche metodi di rotta per tutti gli altri verbi HTTP, che sono usati per lo più nello stesso modo: `post()`, `put()`, `delete()`, `options()`, `trace()`, `copy()`, `lock()`, `mkcol()`, `move()`, `purge()`, `propfind()`, `proppatch()`, `unlock()`, `report()`, `mkactivity()`, `checkout()`, `merge()`, `m-search()`, `notify()`, `subscribe()`, `unsubscribe()`, `patch()`, `search()`, e `connect()`.

Per esempio, il codice qui sotto si comporta proprio come la rotta `/about` di prima, ma risponde solo alle richieste POST HTTP.

```js
router.post("/about", (req, res) => {
  res.send("About this wiki");
});
```

### Percorsi delle rotte

I percorsi delle rotte definiscono gli endpoint a cui è possibile effettuare richieste. Gli esempi che abbiamo visto fino ad ora sono stati solo stringhe e sono usati esattamente come scritti: '/', '/about', '/book', '/any-random.path'.

I percorsi delle rotte possono anche essere pattern di stringhe. I pattern di stringhe utilizzano una forma di sintassi delle espressioni regolari per definire _pattern_ di endpoint che verranno corrispondenti. La sintassi è elencata di seguito (si noti che il trattino (`-`) e il punto (`.`) nella sintassi basata su stringa sono interpretati letteralmente):

- `?` : L'endpoint deve avere 0 o 1 del carattere precedente (o gruppo), ad esempio, un percorso di rotta di `'/ab?cd'` corrisponderà agli endpoint `acd` o `abcd`.
- `+` : L'endpoint deve avere 1 o più del carattere precedente (o gruppo), ad esempio, un percorso di rotta di `'/ab+cd'` corrisponderà agli endpoint `abcd`, `abbcd`, `abbbcd`, e così via.
- `*` : L'endpoint può avere una stringa arbitraria dove il carattere `*` è posizionato. Ad esempio, un percorso di rotta di `'/ab*cd'` corrisponderà agli endpoint `abcd`, `abXcd`, `abSOMErandomTEXTcd`, e così via.
- `()` : Raggruppamento per eseguire un'altra operazione su un set di caratteri, ad esempio, `'/ab(cd)?e'` eseguirà una corrispondenza `?` sul gruppo `(cd)` — corrisponderà a `abe` e `abcde`.

I percorsi delle rotte possono anche essere espressioni regolari in JavaScript [regular expressions](/it/docs/Web/JavaScript/Guide/Regular_expressions). Ad esempio, il percorso della rotta sotto corrisponderà a `catfish` e `dogfish`, ma non a `catflap`, `catfishhead`, e così via. Si noti che il percorso per un'espressione regolare utilizza la sintassi delle espressioni regolari (non è una stringa quotata come nei casi precedenti).

```js
app.get(/.*fish$/, function (req, res) {
  // …
});
```

> [!NOTE]
> La maggior parte delle nostre rotte per il LocalLibrary utilizzerà stringhe e non espressioni regolari. Utilizzeremo anche i parametri di rotta come discusso nella sezione successiva.

### Parametri di rotta

I parametri di rotta sono _segmenti di URL nominati_ utilizzati per catturare valori in posizioni specifiche nell'URL. I segmenti nominati sono prefissati con un doppio punto e poi il nome (Ad es., `/:your_parameter_name/`). I valori catturati sono memorizzati nell'oggetto `req.params` utilizzando i nomi dei parametri come chiavi (Ad es., `req.params.your_parameter_name`).

Ad esempio, consideri un URL codificato per contenere informazioni su utenti e libri: `http://localhost:3000/users/34/books/8989`. Possiamo estrarre queste informazioni come mostrato di seguito, con i parametri di percorso `userId` e `bookId`:

```js
app.get("/users/:userId/books/:bookId", (req, res) => {
  // Access userId via: req.params.userId
  // Access bookId via: req.params.bookId
  res.send(req.params);
});
```

I nomi dei parametri di rotta devono essere costituiti da "caratteri di parola" (A-Z, a-z, 0-9, e \_).

> [!NOTE]
> L'URL _/book/create_ verrà corrispondente a una rotta come `/book/:bookId` (perché `:bookId` è un segnaposto per _qualsiasi_ stringa, quindi `create` corrisponde). La prima rotta che corrisponde a un URL in entrata verrà utilizzata, quindi se si desidera elaborare specificamente gli URL `/book/create`, il loro gestore di rotte deve essere definito prima della rotta `/book/:bookId`.

Questo è tutto ciò che serve per iniziare con le rotte - se necessario, trova maggiori informazioni sulla documentazione di Express: [Basic routing](https://expressjs.com/en/starter/basic-routing.html) e [Routing guide](https://expressjs.com/en/guide/routing.html). Le sezioni seguenti mostrano come configureremo le nostre rotte e controller per il LocalLibrary.

### Gestione degli errori nelle funzioni di rotta

Le funzioni di rotta mostrate in precedenza hanno tutti argomenti `req` e `res`, che rappresentano rispettivamente la richiesta e la risposta.
Le funzioni di rotta sono chiamate anche con un terzo argomento `next`, che può essere utilizzato per passare errori alla catena di middleware di Express.

Il codice qui sotto mostra come funziona, usando l'esempio di una query al database che prende una funzione di callback e restituisce un errore `err` o alcuni risultati.
Se viene restituito `err`, `next` è chiamato con `err` come valore nel suo primo parametro (eventualmente l'errore si propaga al nostro codice di gestione degli errori globale).
In caso di successo, i dati desiderati vengono restituiti e poi usati nella risposta.

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

### Gestione delle eccezioni nelle funzioni di rotta

La sezione precedente mostra come Express si aspetta che le funzioni di rotta restituiscano errori.
Il framework è progettato per l'uso con funzioni asincrone che prendono una funzione di callback (con un errore e un argomento del risultato), che viene chiamata quando l'operazione è completata.
Questo è un problema perché più tardi faremo query sui database Mongoose che usano API basate su [Promise](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise), e che potrebbero generare eccezioni nelle nostre funzioni di rotta (anziché restituire errori in un callback).

Perché il framework gestisca correttamente le eccezioni, devono essere catturate e poi inoltrate come errori come mostrato nella sezione precedente.

> [!NOTE]
> Si prevede che Express 5, attualmente in beta, gestisca eccezioni JavaScript nativamente.

Re-immaginando il semplice esempio della sezione precedente con `About.find().exec()` come query di database che restituisce un `promise`, potremmo scrivere la funzione di rotta all'interno di un blocco [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) in questo modo:

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

C'è abbastanza codice di contorno da aggiungere a ogni funzione.
Invece, per questo tutorial, utilizzeremo il modulo [express-async-handler](https://www.npmjs.com/package/express-async-handler).
Questo definisce una funzione wrapper che nasconde il blocco `try...catch` e il codice per inoltrare l'errore.
L'esempio sopra è ora molto semplice, perché abbiamo solo bisogno di scrivere codice per il caso in cui presumiamo successo:

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

## Rotte necessarie per il LocalLibrary

Gli URL di cui avremo bisogno per le nostre pagine sono elencati di seguito, dove _object_ è sostituito dal nome di ciascuno dei nostri modelli (libro, istanza di libro, genere, autore), _objects_ è il plurale di object, e _id_ è il campo di istanza univoco (`_id`) che viene dato a ciascuna istanza del modello Mongoose di default.

- `catalog/` — La pagina home/index.
- `catalog/<objects>/` — L'elenco di tutti i libri, istanze di libri, generi o autori (e.g., /`catalog/books/`, /`catalog/genres/`, ecc.)
- `catalog/<object>/<id>` — La pagina di dettaglio per un libro, un'istanza di libro, un genere o un autore specifico con il valore di campo `_id` dato (e.g., `/catalog/book/584493c1f4887f06c0e67d37`).
- `catalog/<object>/create` — Il modulo per creare un nuovo libro, istanza di libro, genere o autore (e.g., `/catalog/book/create`).
- `catalog/<object>/<id>/update` — Il modulo per aggiornare un libro, istanza di libro, genere o autore specifico con il valore del campo `_id` dato (e.g., `/catalog/book/584493c1f4887f06c0e67d37/update`).
- `catalog/<object>/<id>/delete` — Il modulo per eliminare un libro, un'istanza di libro, un genere o un autore specifico con il valore del campo `_id` dato (e.g., `/catalog/book/584493c1f4887f06c0e67d37/delete`).

La prima pagina home e le pagine elenco non codificano alcuna informazione aggiuntiva. Mentre i risultati restituiti dipenderanno dal tipo di modello e dal contenuto nel database, le query eseguite per ottenere l'informazione saranno sempre le stesse (analogamente il codice eseguito per la creazione dell'oggetto sarà sempre simile).

Al contrario, gli altri URL sono utilizzati per agire su un documento/istanza di modello specifico—questi codificano l'identità dell'elemento nell'URL (mostrato come `<id>` sopra). Utilizzeremo i parametri di traiettoria per estrarre l'informazione codificata e passarla al gestore delle rotte (e in un articolo successivo useremo ciò per determinare dinamicamente quale informazione ottenere dal database). Codificando l'informazione nel nostro URL abbiamo bisogno di una sola rotta per ogni risorsa di un particolare tipo (e.g., una rotta per gestire la visualizzazione di ogni singolo elemento di libro).

> [!NOTE]
> Express consente di costruire i tuoi URL nel modo che preferisci — puoi codificare l'informazione nel corpo dell'URL come sopra mostrato o utilizzare i parametri `GET` dell'URL (e.g., `/book/?id=6`). Qualunque approccio utilizzi, gli URL dovrebbero essere mantenuti puliti, logici e leggibili ([controlla il consiglio del W3C qui](https://www.w3.org/Provider/Style/URI)).

Successivamente, creiamo le nostre funzioni di callback per il gestore delle rotte e il codice delle rotte per tutti gli URL sopra.

## Creare le funzioni di callback del gestore delle rotte

Prima di definire le nostre rotte, creiamo tutte le funzioni di callback fittizie/scheletriche che esse invocheranno. I callback verranno memorizzati in moduli di "controller" separati per `Book`, `BookInstance`, `Genre`, e `Author` (si può utilizzare qualsiasi struttura di file/modulo, ma questa sembra un'adeguata granularità per questo progetto).

Inizia creando una cartella per i nostri controller nella radice del progetto (**/controllers**) e poi crea file/moduli di controller separati per la gestione di ciascuno dei modelli:

```plain
/express-locallibrary-tutorial  # the project root
  /controllers
    authorController.js
    bookController.js
    bookinstanceController.js
    genreController.js
```

I controller utilizzeranno il modulo `express-async-handler`, quindi prima di procedere, installalo nella libreria utilizzando `npm`:

```bash
npm install express-async-handler
```

### Controller degli Autori

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

Il modulo richiede prima il modello `Author` che utilizzeremo più avanti per accedere e aggiornare i nostri dati, e il wrapper `asyncHandler` che utilizzeremo per catturare eventuali eccezioni generate nelle nostre funzioni di gestione delle rotte.
Esporta poi funzioni per ciascuno degli URL che desideriamo gestire.
Si noti che le operazioni di creazione, aggiornamento ed eliminazione utilizzano moduli, e quindi hanno metodi aggiuntivi per gestire le richieste post del modulo — discuteremo di questi metodi nell'articolo sui "forms" successivamente.

Le funzioni utilizzano tutte la funzione wrapper descritta sopra in [Gestione delle eccezioni nelle funzioni di rotta](#gestione_delle_eccezioni_nelle_funzioni_di_rotta), con argomenti per la richiesta, la risposta e il next.
Le funzioni rispondono con una stringa che indica che la pagina associata non è ancora stata creata.
Se ci si aspetta che una funzione del controller riceva parametri di percorso, questi sono visualizzati nella stringa di messaggio (vedi `req.params.id` sopra).

Si noti che una volta implementate, alcune funzioni di rotta potrebbero non contenere alcun codice che può generare eccezioni.
Possiamo cambiare queste in funzioni di gestione delle rotte "normali" quando ci arriveremo.

#### Controller dei BookInstance

Apri il file **/controllers/bookinstanceController.js** e copia il seguente codice (questo segue un modello identico al modulo del controller `Author`):

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

#### Controller dei Generi

Apri il file **/controllers/genreController.js** e copia nel seguente testo (segue un modello identico ai file `Author` e `BookInstance`):

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

#### Controller dei Libri

Apri il file **/controllers/bookController.js** e copia il seguente codice.
Questo segue lo stesso modello degli altri moduli del controller, ma ha anche una funzione `index()` per visualizzare la pagina di benvenuto del sito:

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

## Creare il modulo delle rotte del catalogo

Successivamente, creiamo _rotte_ per tutti gli URL di cui [ha bisogno il sito web LocalLibrary](#rotte_necessarie_per_il_locallibrary), che chiameranno le funzioni del controller che abbiamo definito nelle sezioni precedenti.

Lo scheletro ha già una cartella **./routes** contenente rotte per l'_index_ e gli _users_.
Creare un altro file di rotta — **catalog.js** — all'interno di questa cartella, come mostrato.

```plain
/express-locallibrary-tutorial # the project root
  /routes
    index.js
    users.js
    catalog.js
```

Apri **/routes/catalog.js** e copia nel codice qui sotto:

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

Il modulo richiede Express e poi lo utilizza per creare un oggetto `Router`. Le rotte sono tutte impostate sul router, che viene poi esportato.

Le rotte sono definite utilizzando metodi `.get()` o `.post()` sull'oggetto router. Tutti i percorsi sono definiti utilizzando stringhe (non utilizziamo pattern di stringhe o espressioni regolari).
Le rotte che agiscono su una specifica risorsa (ad esempio, libro) utilizzano i parametri di percorso per ottenere l'id dell'oggetto dall'URL.

Le funzioni gestori sono tutte importate dai moduli del controller che abbiamo creato nella sezione precedente.

### Aggiorna il modulo delle rotte dell'indice

Abbiamo configurato tutte le nostre nuove rotte, ma abbiamo ancora una rotta per la pagina originale. Reindirizziamo invece questa alla nuova pagina dell'indice che abbiamo creato al percorso '/catalog'.

Apri **/routes/index.js** e sostituisci la rotta esistente con la funzione che segue.

```js
// GET home page.
router.get("/", function (req, res) {
  res.redirect("/catalog");
});
```

> [!NOTE]
> Questo è il nostro primo utilizzo del metodo di risposta [redirect()](https://expressjs.com/en/4x/api.html#res.redirect). Questo reindirizza alla pagina specificata, inviando di default il codice di stato HTTP "302 Found". È possibile modificare il codice di stato restituito se necessario e fornire percorsi assoluti o relativi.

### Aggiorna app.js

L'ultimo passaggio è aggiungere le rotte alla catena del middleware.
Facciamo ciò in `app.js`.

Apri **app.js** e richiedi la rotta catalogo sotto le altre rotte (aggiungi la terza riga mostrata di seguito, sotto alle altre due che dovrebbero essere già presenti nel file):

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
const catalogRouter = require("./routes/catalog"); // Import routes for "catalog" area of site
```

Successivamente, aggiungi la rotta catalogo allo stack del middleware sotto le altre rotte (aggiungi la terza riga mostrata di seguito, sotto alle altre due che dovrebbero essere già presenti nel file):

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
app.use("/catalog", catalogRouter); // Add catalog routes to middleware chain.
```

> [!NOTE]
> Abbiamo aggiunto il nostro modulo catalogo a un percorso `'/catalog'`. Questo viene anteposto a tutti i percorsi definiti nel modulo catalogo. Così per esempio, per accedere a un elenco di libri, l'URL sarà: `/catalog/books/`.

Questo è tutto. Dovremmo ora avere rotte e funzioni scheletriche abilitate per tutti gli URL che supportiamo nel sito web LocalLibrary.

### Testing delle rotte

Per testare le rotte, avvia il sito web utilizzando il tuo approccio abituale

- Il metodo predefinito

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

Poi naviga verso un numero di URL del LocalLibrary e verifichi che non venga visualizzata una pagina di errore (HTTP 404). Un piccolo insieme di URL è elencato di seguito per tua comodità:

- `http://localhost:3000/`
- `http://localhost:3000/catalog`
- `http://localhost:3000/catalog/books`
- `http://localhost:3000/catalog/bookinstances/`
- `http://localhost:3000/catalog/authors/`
- `http://localhost:3000/catalog/genres/`
- `http://localhost:3000/catalog/book/5846437593935e2f8c2aa226`
- `http://localhost:3000/catalog/book/create`

## Sommario

Abbiamo ora creato tutte le rotte per il nostro sito, insieme a funzioni del controller fittizie che possiamo popolare con una piena implementazione in articoli successivi. Lungo la strada abbiamo appreso molte informazioni fondamentali sulle rotte di Express, la gestione delle eccezioni e alcuni approcci per strutturare le nostre rotte e controller.

Nel nostro prossimo articolo creeremo una pagina di benvenuto appropriata per il sito, utilizzando viste (template) e informazioni memorizzate nei nostri modelli.

## Vedi anche

- [Basic routing](https://expressjs.com/en/starter/basic-routing.html) (Express docs)
- [Routing guide](https://expressjs.com/en/guide/routing.html) (Express docs)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
