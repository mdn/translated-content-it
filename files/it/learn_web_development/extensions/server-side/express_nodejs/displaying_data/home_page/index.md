---
title: Pagina principale
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

La prima pagina che creeremo sarà la home page del sito web, accessibile sia dalla radice del sito (`/`) che dal catalogo (`catalog/`). Questa mostrerà del testo statico che descrive il sito, insieme a "conteggi" dinamicamente calcolati dei diversi tipi di record nel database.

Abbiamo già creato una route per la home page. Per completare la pagina dobbiamo aggiornare la nostra funzione del controller per ottenere i "conteggi" dei record dal database e creare una vista (template) che possiamo usare per renderizzare la pagina.

> [!NOTE]
> Utilizzeremo Mongoose per ottenere informazioni dal database.
> Prima di continuare, potrebbe essere utile rileggere la sezione sul [primer di Mongoose](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#mongoose_primer) relativa alla [ricerca dei record](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#searching_for_records).

## Route

Abbiamo creato le route per la nostra pagina indice in un [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes).
Come promemoria, tutte le funzioni delle route sono definite in **/routes/catalog.js**:

```js
// GET catalog home page.
router.get("/", book_controller.index); // This actually maps to /catalog/ because we import the route with a /catalog prefix
```

La funzione index del controller dei libri passata come parametro (`book_controller.index`) ha un'implementazione "segnaposto" definita in **/controllers/bookController.js**:

```js
exports.index = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
});
```

È questa funzione del controller che estendiamo per ottenere informazioni dai nostri modelli e poi renderizzarla utilizzando un template (vista).

## Controller

La funzione del controller index deve ottenere informazioni sul numero di record `Book`, `BookInstance` (tutti), `BookInstance` (disponibili), `Author` e `Genre` che abbiamo nel database, renderizzare questi dati in un template per creare una pagina HTML, e poi restituirla in una risposta HTTP.

Aprire **/controllers/bookController.js**. Vicino all'inizio del file, dovrebbe vedere la funzione esportata `index()`.

```js
const Book = require("../models/book");
const asyncHandler = require("express-async-handler");

exports.index = asyncHandler(async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
});
```

Sostituire tutto il codice sopra con il seguente frammento di codice.
La prima cosa che fa è importare (`require()`) tutti i modelli.
Dobbiamo farlo perché li utilizzeremo per ottenere i conteggi dei documenti.
Il codice richiede anche "express-async-handler", che fornisce un wrapper per [catturare eccezioni lanciate nelle funzioni del gestore delle route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes#handling_exceptions_in_route_functions).

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

Usiamo il metodo [`countDocuments()`](<https://mongoosejs.com/docs/api/model.html#Model.countDocuments()>) per ottenere il numero di istanze di ciascun modello.
Questo metodo viene chiamato su un modello, con un insieme opzionale di condizioni da rispettare, e restituisce un oggetto `Query`.
La query può essere eseguita chiamando [`exec()`](https://mongoosejs.com/docs/api/query.html#Query.prototype.exec), che restituisce una `Promise` che è adempiuta con un risultato o respinta se c'è un errore nel database.

Poiché le query per i conteggi dei documenti sono indipendenti l'una dall'altra, usiamo [`Promise.all()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) per eseguirle in parallelo.
Il metodo restituisce una nuova promessa che noi [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) per completamento (l'esecuzione si sospende all'interno di _questa funzione_ al `await`).
Quando tutte le query sono completate, la promessa restituita da `all()` si adempie, continuando l'esecuzione della funzione del gestore delle route, e popolando l'array con i risultati delle query del database.

Chiamiamo quindi [`res.render()`](https://expressjs.com/en/4x/api.html#res.render), specificando una vista (template) chiamata '**index**' e oggetti che mappano i risultati delle query del database al template della vista.
I dati vengono forniti come coppie chiave-valore e possono essere accessibili nel template utilizzando la chiave.

> [!NOTE]
> Se si utilizza una chiave/variabile in un template Pug che non è stata passata, allora verrà renderizzata come una stringa vuota e valutata come `false` nelle espressioni.
> Altri linguaggi di template possono richiedere che Lei passi valori per tutti gli oggetti utilizzati.

Si noti che il codice è molto semplice perché possiamo assumere che le query del database abbiano successo.
Se una qualsiasi delle operazioni del database fallisce, l'eccezione che viene lanciata sarà catturata da `asyncHandler()` e passata al successivo gestore middleware nella catena.

## Vista

Aprire **/views/index.pug** e sostituire il suo contenuto con il testo qui sotto.

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

La vista è semplice. Estendiamo il template di base **layout.pug**, sovrascrivendo il `block` chiamato '**content**'. Il primo intestazione `h1` sarà il testo escapato per la variabile `title` che è stata passata nella funzione `render()`—nota l'uso di `h1=` in modo che il testo successivo sia trattato come un'espressione JavaScript. Includiamo quindi un paragrafo che introduce la LocalLibrary.

Sotto l'intestazione _Contenuto dinamico_ elenchiamo il numero di copie di ciascun modello.
Si noti che i valori del template per i dati sono le chiavi specificate quando `render()` è stato chiamato nella funzione del gestore delle route.

> [!NOTE]
> Non abbiamo fatto l'escape dei valori di conteggio (cioè, abbiamo usato la sintassi `!{}`) perché i valori di conteggio sono calcolati. Se le informazioni fossero fornite dagli utenti finali, faremmo l'escape della variabile per la visualizzazione.

## Che aspetto ha?

A questo punto dovremmo aver creato tutto ciò che è necessario per visualizzare la pagina indice. Eseguire l'applicazione e aprire il browser a `http://localhost:3000/`. Se tutto è configurato correttamente, il sito dovrebbe apparire come nello screenshot seguente.

![Pagina principale - Sito della biblioteca locale con Express](locallibary_express_home.png)

> [!NOTE]
> Non sarà possibile _usare_ i link della barra laterale perché gli URL, le viste e i template per quelle pagine non sono ancora stati definiti. Se si prova si otterranno errori come "NON IMPLEMENTATO: Lista dei libri" per esempio, a seconda del link cliccato. Queste stringhe letterali (che saranno sostituite con dati appropriati) sono state specificate nei diversi controller che vivono all'interno del suo file "controllers".

## Prossimi passi

- Torni a [Express Tutorial Parte 5: Visualizzazione dei dati della biblioteca](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proceda al prossimo sottoarticolo della parte 5: [Pagina della lista dei libri](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page).
