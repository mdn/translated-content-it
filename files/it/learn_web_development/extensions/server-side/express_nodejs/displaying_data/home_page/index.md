---
title: Pagina iniziale
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page
l10n:
  sourceCommit: afcdfa050626bb7eb05ee693df8997020db9ff2e
---

La prima pagina che creeremo sarà la pagina iniziale del sito web, accessibile dalla radice del sito (`/`) o del catalogo (`catalog/`). Visualizzerà del testo statico che descrive il sito, insieme ai "conteggi" calcolati dinamicamente di diversi tipi di record nel database.

Abbiamo già creato una route per la pagina iniziale. Per completare la pagina dobbiamo aggiornare la funzione controller affinché recuperi i "conteggi" dei record dal database e creare una view (template) da utilizzare per il rendering della pagina.

> [!NOTE]
> Verrà utilizzato Mongoose per ottenere informazioni dal database.
> Prima di continuare, potrebbe essere utile rileggere la sezione [introduzione a Mongoose](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#mongoose_primer) sulla [ricerca dei record](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose#searching_for_records).

## Route

Le route della pagina index sono state create in un [tutorial precedente](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes).
Come promemoria, tutte le funzioni route sono definite in **/routes/catalog.js**:

```js
// GET catalog home page.
router.get("/", book_controller.index); // This actually maps to /catalog/ because we import the route with a /catalog prefix
```

La funzione index del controller del libro passata come parametro (`book_controller.index`) ha un'implementazione "segnaposto" definita in **/controllers/bookController.js**:

```js
exports.index = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
};
```

È questa funzione controller che verrà estesa per ottenere informazioni dai modelli e quindi sottoporle a rendering tramite un template (view).

## Controller

La funzione controller index deve recuperare informazioni sul numero di record `Book`, `BookInstance` (tutti), `BookInstance` (disponibili), `Author` e `Genre` presenti nel database, sottoporre a rendering questi dati in un template per creare una pagina HTML e quindi restituirla in una risposta HTTP.

Aprire **/controllers/bookController.js**. Nella parte superiore del file dovrebbe essere visibile la funzione `index()` esportata.

```js
const Book = require("../models/book");

exports.index = async (req, res, next) => {
  res.send("NOT IMPLEMENTED: Site Home Page");
};
```

Sostituire tutto il codice precedente con il seguente frammento di codice.
La prima operazione consiste nell'importare (`require()`) tutti i modelli.
Questo è necessario perché verranno utilizzati per ottenere i conteggi dei documenti.

```js
const Book = require("../models/book");
const Author = require("../models/author");
const Genre = require("../models/genre");
const BookInstance = require("../models/bookinstance");

exports.index = async (req, res, next) => {
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
};
```

Viene utilizzato il metodo [`countDocuments()`](<https://mongoosejs.com/docs/api/model.html#Model.countDocuments()>) per ottenere il numero di istanze di ciascun modello.
Questo metodo viene chiamato su un modello, con un insieme opzionale di condizioni da soddisfare, e restituisce un oggetto `Query`.
La query può essere eseguita chiamando [`exec()`](https://mongoosejs.com/docs/api/query.html#Query.prototype.exec), che restituisce una `Promise` che viene soddisfatta con un risultato oppure rifiutata in caso di errore del database.

Poiché le query per il conteggio dei documenti sono indipendenti l'una dall'altra, viene utilizzato [`Promise.all()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise/all) per eseguirle in parallelo.
Il metodo restituisce una nuova promise il cui completamento viene atteso tramite [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) (l'esecuzione viene sospesa _in questa funzione_ in corrispondenza di `await`).
Quando tutte le query sono completate, la promise restituita da `all()` viene soddisfatta, l'esecuzione della funzione gestore della route continua e l'array viene popolato con i risultati delle query del database.

Viene quindi chiamato [`res.render()`](https://expressjs.com/en/5x/api/#res.render), specificando una view (template) denominata '**index**' e oggetti che associano i risultati delle query del database al template della view.
I dati vengono forniti come coppie chiave-valore e sono accessibili nel template utilizzando la chiave.

> [!NOTE]
> Se viene utilizzata una chiave/variabile in un template Pug senza che sia stata passata, verrà sottoposta a rendering come stringa vuota e valutata come `false` nelle espressioni.
> Altri linguaggi di template potrebbero richiedere il passaggio di valori per tutti gli oggetti utilizzati.

Si noti che il codice è molto semplice perché si può presumere che le query del database abbiano successo.
Se una delle operazioni sul database non riesce, l'eccezione generata causerà il rifiuto della Promise e Express passerà l'errore al gestore middleware `next` nella catena.

## View

Aprire **/views/index.pug** e sostituirne il contenuto con il testo seguente.

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

La view è semplice. Viene esteso il template di base **layout.pug**, sovrascrivendo il `block` denominato '**content**'. La prima intestazione `h1` sarà il testo con escape della variabile `title` passata alla funzione `render()` — si noti l'uso di `h1=` affinché il testo seguente venga trattato come un'espressione JavaScript. Viene quindi incluso un paragrafo che introduce LocalLibrary.

Sotto l'intestazione _Contenuto dinamico_ viene elencato il numero di copie di ciascun modello.
Si noti che i valori del template per i dati sono le chiavi specificate quando è stato chiamato `render()` nella funzione gestore della route.

> [!NOTE]
> I valori di conteggio non sono stati sottoposti a escape (ovvero, è stata utilizzata la sintassi `!{}`) perché i valori di conteggio sono calcolati. Se le informazioni fossero state fornite dagli utenti finali, la variabile sarebbe stata sottoposta a escape per la visualizzazione.

## Come appare?

A questo punto dovrebbe essere stato creato tutto il necessario per visualizzare la pagina index. Eseguire l'applicazione e aprire il browser all'indirizzo `http://localhost:3000/`. Se tutto è configurato correttamente, il sito dovrebbe apparire in modo simile alla seguente schermata.

![Pagina iniziale - sito Express Local Library](locallibary_express_home.png)

> [!NOTE]
> Non sarà ancora possibile _utilizzare_ i link della barra laterale perché gli URL, le view e i template per tali pagine non sono stati definiti. Provando a farlo si otterranno errori come "NOT IMPLEMENTED: Book list", ad esempio, a seconda del link selezionato. Questi letterali stringa (che verranno sostituiti con dati appropriati) sono stati specificati nei diversi controller presenti nella cartella "controllers".

## Passaggi successivi

- Tornare a [Tutorial su Express Parte 5: Visualizzazione dei dati della libreria](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data).
- Proseguire al sottoarticolo successivo della parte 5: [Pagina dell'elenco dei libri](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page).
