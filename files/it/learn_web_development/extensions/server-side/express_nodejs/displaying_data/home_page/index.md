---
title: Home page
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

La prima pagina che creeremo sarà la home page del sito web, accessibile sia dalla radice del sito (`/`) che dalla radice del catalogo (`catalog/`). Questa visualizzerà del testo statico che descrive il sito, insieme a "conteggi" calcolati dinamicamente dei diversi tipi di record nel database.

Abbiamo già creato una rotta per la home page. Per completare la pagina, dobbiamo aggiornare la nostra funzione del controller per ottenere i "conteggi" dei record dal database e creare una view (template) che possiamo utilizzare per renderizzare la pagina.

> [!NOTE]
> Utilizzeremo Mongoose per ottenere informazioni dal database.
> Prima di procedere, potrebbe essere utile rileggere la sezione del [Mongoose primer](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#mongoose_primer) sui [record di ricerca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#searching_for_records).

## Rotta

Abbiamo creato le rotte per la nostra pagina indice in un [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes).
Come promemoria, tutte le funzioni di rotta sono definite in **/routes/catalog.js**:

```js
// GET catalog home page.
router.get("/", book_controller.index); // This actually maps to /catalog/ because we import the route with a /catalog prefix
```

La funzione index del controller dei libri passata come parametro (`book_controller.index`) ha un'implementazione "placeholder" definita in **/controllers/bookController.js**:

```js
exports.index = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
});
```

È questa funzione del controller che estendiamo per ottenere informazioni dai nostri modelli e quindi renderizzarla utilizzando un template (view).

## Controller

La funzione index del controller deve recuperare informazioni su quanti record `Book`, `BookInstance` (tutti), `BookInstance` (disponibili), `Author` e `Genre` abbiamo nel database, rendere questi dati in un template per creare una pagina HTML e quindi restituirla in una risposta HTTP.

Apri **/controllers/bookController.js**. In alto nel file dovresti vedere la funzione `index()` esportata.

```js
const Book = require("../models/book");
const asyncHandler = require("express-async-handler");

exports.index = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
});
```

Sostituisci tutto il codice sopra con il seguente frammento di codice.
La prima cosa che fa è importare (`require()`) tutti i modelli.
Dobbiamo farlo perché li utilizzeremo per ottenere i conteggi dei documenti.
Il codice richiede anche "express-async-handler", che fornisce un wrapper per [gestire le eccezioni sollevate nelle funzioni handler delle rotte](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#handling_exceptions_in_route_functions).

```js
const Book = require("../models/book");
const Author = require("../models/author");
const Genre = require("../models/genre");
const BookInstance = require("../models/bookinstance");

const asyncHandler = require("express-async-handler");

exports.index = asyncHandler(async (req, res, next) => {
  // Get details of books, book instances, authors and genre counts (in parallel)
  const [
    numBooks,
    numBookInstances,
    numAvailableBookInstances,
    numAuthors,
    numGenres,
  ] = await Promise.all([
    Book.countDocuments({}).exec(),
    BookInstance.countDocuments({}).exec(),
    BookInstance.countDocuments({ status: "Available" }).exec(),
    Author.countDocuments({}).exec(),
    Genre.countDocuments({}).exec(),
  ]);

  res.render("index", {
    title: "Local Library Home",
    book_count: numBooks,
    book_instance_count: numBookInstances,
    book_instance_available_count: numAvailableBookInstances,
    author_count: numAuthors,
    genre_count: numGenres,
  });
});
```

Utilizziamo il metodo [`countDocuments()`](<https://mongoosejs.com/docs/api/model.html#Model.countDocuments()>) per ottenere il numero di istanze di ciascun modello.
Questo metodo viene chiamato su un modello, con un set opzionale di condizioni da soddisfare, e restituisce un oggetto `Query`.
La query può essere eseguita chiamando [`exec()`](https://mongoosejs.com/docs/api/query.html#Query.prototype.exec), che restituisce un `Promise` che viene risolto con un risultato, o rifiutato se c'è un errore nel database.

Poiché le query per i conteggi dei documenti sono indipendenti l'una dall'altra, utilizziamo [`Promise.all()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) per eseguirle in parallelo.
Il metodo restituisce una nuova promessa che attenderemo (`await`) per completamento (l'esecuzione si interrompe all'interno _di questa funzione_ al `await`).
Quando tutte le query si completano, la promessa restituita da `all()` si risolve, continuando l'esecuzione della funzione handler della rotta e popolando l'array con i risultati delle query nel database.

Quindi chiamiamo [`res.render()`](https://expressjs.com/en/4x/api.html#res.render), specificando un view (template) chiamato '**index**' e oggetti che mappano i risultati delle query del database al template della view.
I dati sono forniti come coppie chiave-valore e possono essere accessibili nel template utilizzando la chiave.

> [!NOTE]
> Se si utilizza una chiave/variabile in un template Pug che non è stata passata, verrà renderizzata come una stringa vuota e valutata come `false` nelle espressioni.
> Altri linguaggi di template potrebbero richiedere che si forniscano valori per tutti gli oggetti utilizzati.

Nota che il codice è molto semplice perché possiamo assumere che le query del database avranno successo.
Se una qualsiasi delle operazioni del database fallisce, l'eccezione sollevata verrà catturata da `asyncHandler()` e passata al middleware handler `next` nella catena.

## View

Apri **/views/index.pug** e sostituisci il suo contenuto con il testo di seguito.

```pug
extends layout

block content
  h1= title
  p Welcome to #[em LocalLibrary], a very basic Express website developed as a tutorial example on the Mozilla Developer Network.

  h2 Dynamic content

  p The library has the following record counts:

  ul
    li #[strong Books:] !{book_count}
    li #[strong Copies:] !{book_instance_count}
    li #[strong Copies available:] !{book_instance_available_count}
    li #[strong Authors:] !{author_count}
    li #[strong Genres:] !{genre_count}
```

La view è semplice. Estendiamo il template base **layout.pug**, sovrascrivendo il `block` chiamato '**content**'. Il primo intestazione `h1` sarà il testo escapato per la variabile `title` che è stata passata alla funzione `render()`—nota l'uso di `h1=` in modo che il testo seguente sia trattato come un'espressione JavaScript. Poi includiamo un paragrafo che introduce la LocalLibrary.

Sotto l'intestazione _Contenuto dinamico_, elenchiamo il numero di copie di ciascun modello.
Nota che i valori dei template per i dati sono le chiavi che sono state specificate quando `render()` è stato chiamato nella funzione handler della rotta.

> [!NOTE]
> Non abbiamo utilizzato l'escapement per i valori dei conteggi (cioè abbiamo usato la sintassi `!{}`) perché i valori dei conteggi sono calcolati. Se le informazioni fossero state fornite dagli utenti finali, avremmo fatto l'escapement della variabile per la visualizzazione.

## Come appare?

A questo punto dovremmo aver creato tutto il necessario per visualizzare la pagina indice. Esegui l'applicazione e apri il browser a `http://localhost:3000/`. Se tutto è configurato correttamente, il tuo sito dovrebbe sembrare simile alla seguente schermata.

![Home page - Express Local Library site](locallibary_express_home.png)

> [!NOTE]
> Non potrai _usare_ i link della barra laterale perché gli URL, le view e i template per quelle pagine non sono stati definiti. Se provi, otterrai errori come "NON IMPLEMENTATO: Elenco libri" per esempio, a seconda del link su cui clicchi. Questi letterali di stringa (che verranno sostituiti con dati appropriati) sono stati specificati nei vari controller che risiedono nel tuo file "controllers".

## Prossimi passi

- Torna a [Espressione Tutorial Parte 5: Visualizzazione dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Procedi al prossimo sottoarticolo della parte 5: [Pagina elenco libri](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page).
