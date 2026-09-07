---
title: Introduzione a Express/Node
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction
l10n:
  sourceCommit: f4c14731a1a157fc8d8f7357ac4d74d14a7d7fb5
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo primo articolo su Express rispondiamo alle domande "Che cos'è Node?" e "Che cos'è Express?", e forniamo una panoramica di ciò che rende speciale il framework web Express. Descriveremo le funzionalità principali e mostreremo alcuni dei componenti fondamentali di un'applicazione Express (anche se a questo punto non sarà ancora disponibile un ambiente di sviluppo in cui testarla).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione generale della <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps">programmazione di siti web lato server</a>, e in particolare dei meccanismi delle <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview">interazioni client-server nei siti web</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con Express e con il suo rapporto con Node, con le funzionalità che fornisce e con i componenti principali di un'applicazione Express.
      </td>
    </tr>
  </tbody>
</table>

## Introduzione a Node

[Node](https://nodejs.org/) (o, più formalmente, _Node.js_) è un ambiente di runtime open source e multipiattaforma che consente agli sviluppatori di creare ogni tipo di strumento e applicazione lato server in {{Glossary("JavaScript", "JavaScript")}}.
Il runtime è progettato per essere usato al di fuori di un contesto browser (ossia, eseguendolo direttamente sul sistema operativo di un computer o server). Di conseguenza, l'ambiente omette le API JavaScript specifiche del browser e aggiunge il supporto per API del sistema operativo più tradizionali, incluse le librerie HTTP e del file system.

Dal punto di vista dello sviluppo di web server, Node offre numerosi vantaggi:

- Ottime prestazioni! Node è stato progettato per ottimizzare il throughput e la scalabilità nelle applicazioni web ed è una buona soluzione per molti problemi comuni dello sviluppo web (ad esempio, le applicazioni web in tempo reale).
- Il codice è scritto in "semplice JavaScript", il che significa dedicare meno tempo al cambio di contesto tra linguaggi quando si scrive sia codice lato client sia lato server.
- JavaScript è un linguaggio di programmazione relativamente recente e beneficia dei miglioramenti nel design del linguaggio rispetto ad altri linguaggi tradizionali per web server (ad esempio Python, PHP e così via). Molti altri linguaggi nuovi e popolari compilano/convertono in JavaScript, quindi è possibile usare anche TypeScript, CoffeeScript, ClojureScript, Scala, LiveScript e così via.
- Il package manager di Node (npm) fornisce accesso a centinaia di migliaia di pacchetti riutilizzabili. Offre inoltre una risoluzione delle dipendenze tra le migliori della categoria e può essere usato per automatizzare gran parte della toolchain di build.
- Node.js è portabile. È disponibile per Microsoft Windows, macOS, Linux, Solaris, FreeBSD, OpenBSD, WebOS e NonStop OS. Inoltre, è ben supportato da molti provider di web hosting, che spesso forniscono infrastrutture e documentazione specifiche per ospitare siti Node.
- Dispone di un ecosistema di terze parti e di una comunità di sviluppatori molto attivi, con molte persone disponibili ad aiutare.

È possibile usare Node.js per creare un semplice web server usando il pacchetto HTTP di Node.

### Hello Node.js

L'esempio seguente crea un web server che rimane in ascolto di qualsiasi tipo di richiesta HTTP all'URL `http://127.0.0.1:8000/`: quando riceve una richiesta, lo script risponde con la stringa "Hello World". Se Node è già installato, è possibile seguire questi passaggi per provare l'esempio:

1. Aprire il Terminale (su Windows, aprire l'utilità della riga di comando).
2. Creare la cartella in cui si desidera salvare il programma, ad esempio `test-node`, quindi accedervi immettendo il seguente comando nel terminale:

   ```bash
   cd test-node
   ```

3. Usando il proprio editor di testo preferito, creare un file chiamato `hello.js` e incollarvi il codice seguente:

   ```js
   // Load HTTP module
   const http = require("http");

   const hostname = "127.0.0.1";
   const port = 8000;

   // Create HTTP server
   const server = http.createServer((req, res) => {
     // Set the response HTTP header with HTTP status and Content type
     res.writeHead(200, { "Content-Type": "text/plain" });

     // Send the response body "Hello World"
     res.end("Hello World\n");
   });

   // Prints a log once the server starts listening
   server.listen(port, hostname, () => {
     console.log(`Server running at http://${hostname}:${port}/`);
   });
   ```

4. Salvare il file nella cartella creata in precedenza.
5. Tornare al terminale e digitare il comando seguente:

   ```bash
   node hello.js
   ```

Infine, aprire `http://localhost:8000` nel browser web: dovrebbe apparire il testo "**Hello World**" nell'angolo superiore sinistro di una pagina web altrimenti vuota.

> [!NOTE]
> Per sperimentare del codice Node.js senza dover configurare alcun ambiente locale, [Aside: The HTTP module](https://scrimba.com/learn-nodejs-c00ho9qqh6/~07du?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce una guida interattiva per configurare un server di base con il pacchetto HTTP di Node.

## Framework web

Altre attività comuni dello sviluppo web non sono supportate direttamente da Node stesso. Se si desidera aggiungere una gestione specifica per diversi verbi HTTP (ad esempio `GET`, `POST`, `DELETE` e così via), gestire separatamente le richieste in diversi percorsi URL ("route"), servire file statici o usare template per creare dinamicamente la risposta, Node da solo non sarà di grande utilità. Sarà necessario scrivere il codice personalmente oppure evitare di reinventare la ruota e usare un framework web!

## Introduzione a Express

[Express](https://expressjs.com/) è il framework web Node.js più popolare ed è la libreria sottostante di numerosi altri popolari framework Node.js. Fornisce meccanismi per:

- Scrivere handler per richieste con diversi verbi HTTP in differenti percorsi URL (route).
- Integrarsi con motori di rendering delle "view" per generare risposte inserendo dati nei template.
- Impostare configurazioni comuni dell'applicazione web, come la porta da usare per la connessione e la posizione dei template usati per il rendering della risposta.
- Aggiungere ulteriore elaborazione delle richieste tramite "middleware" in qualsiasi punto della pipeline di gestione delle richieste.

Sebbene _Express_ sia di per sé piuttosto minimale, gli sviluppatori hanno creato pacchetti middleware compatibili per affrontare quasi ogni problema dello sviluppo web. Esistono librerie per lavorare con cookie, sessioni, accessi utente, parametri URL, dati `POST`, header di sicurezza e _molto_ altro. È possibile trovare un elenco dei pacchetti middleware gestiti dal team di Express su [Express Middleware](https://expressjs.com/en/resources/middleware/) (insieme a un elenco di alcuni popolari pacchetti di terze parti).

> [!NOTE]
> Questa flessibilità è un'arma a doppio taglio. Esistono pacchetti middleware per affrontare quasi qualsiasi problema o requisito, ma individuare i pacchetti giusti da usare può talvolta essere una sfida. Non esiste inoltre un unico "modo corretto" per strutturare un'applicazione e molti esempi reperibili su Internet non sono ottimali oppure mostrano soltanto una piccola parte di ciò che occorre fare per sviluppare un'applicazione web.

## Da dove provengono Node ed Express?

Node è stato rilasciato inizialmente, soltanto per Linux, nel 2009. Il package manager npm è stato rilasciato nel 2010 e il supporto nativo per Windows è stato aggiunto nel 2012. Consultare [Wikipedia](https://en.wikipedia.org/wiki/Node.js#History) per ulteriori informazioni.

Express è stato rilasciato inizialmente nel novembre 2010 e attualmente la sua API è alla versione principale 5. È possibile consultare il [changelog](https://expressjs.com/en/changelog/#5.x) per informazioni sulle modifiche della versione corrente e [GitHub](https://github.com/expressjs/express/blob/master/History.md) per note di rilascio storiche più dettagliate.

## Quanto sono popolari Node ed Express?

La popolarità di un framework web è importante perché indica se continuerà a essere mantenuto e quali risorse saranno probabilmente disponibili in termini di documentazione, librerie aggiuntive e supporto tecnico.

Non esiste una misura definitiva e prontamente disponibile della popolarità dei framework lato server (anche se è possibile stimare la popolarità usando meccanismi come il conteggio del numero di progetti GitHub e di domande Stack Overflow per ciascuna piattaforma). Una domanda migliore è se Node ed Express siano "abbastanza popolari" da evitare i problemi delle piattaforme impopolari. Stanno continuando a evolversi? È possibile ottenere aiuto quando serve? Esistono opportunità di lavoro retribuito imparando Express?

In base al numero di aziende di alto profilo che usano Express, al numero di persone che contribuiscono alla codebase e al numero di persone che forniscono supporto sia gratuito sia a pagamento, la risposta è sì: _Express_ è un framework popolare!

## Express è opinionated?

I framework web spesso si definiscono "opinionated" o "unopinionated".

I framework opinionated hanno opinioni sul "modo corretto" di gestire un determinato compito. Spesso supportano lo sviluppo rapido _in un particolare dominio_ (risolvendo problemi di un tipo specifico), perché il modo corretto di fare qualsiasi cosa è generalmente ben compreso e ben documentato. Tuttavia, possono essere meno flessibili nella risoluzione di problemi al di fuori del loro dominio principale e tendono a offrire meno scelte riguardo ai componenti e agli approcci utilizzabili.

Al contrario, i framework unopinionated hanno molte meno restrizioni sul modo migliore di collegare componenti per raggiungere un obiettivo, o perfino sui componenti da usare. Rendono più semplice per gli sviluppatori usare gli strumenti più adatti a completare un particolare compito, sebbene sia necessario individuare tali componenti autonomamente.

Express è unopinionated. È possibile inserire quasi qualsiasi middleware compatibile nella catena di gestione delle richieste, in quasi qualsiasi ordine. È possibile strutturare l'app in un file o in più file, usando qualsiasi struttura di directory. Talvolta potrebbe sembrare di avere troppe possibilità di scelta!

## Che aspetto ha il codice Express?

In un sito web tradizionale basato sui dati, un'applicazione web attende richieste HTTP dal browser web (o da un altro client). Quando riceve una richiesta, l'applicazione determina quale azione è necessaria in base al pattern URL e, possibilmente, alle informazioni associate contenute nei dati `POST` o `GET`. In base a ciò che è richiesto, può quindi leggere o scrivere informazioni da un database oppure eseguire altre attività necessarie per soddisfare la richiesta. L'applicazione restituisce quindi una risposta al browser web, spesso creando dinamicamente una pagina HTML da visualizzare nel browser inserendo i dati recuperati nei segnaposto di un template HTML.

Express fornisce metodi per specificare quale funzione viene chiamata per un particolare verbo HTTP (`GET`, `POST`, `PUT` e così via) e pattern URL ("Route"), nonché metodi per specificare quale motore di template ("view") viene usato, dove si trovano i file dei template e quale template usare per il rendering di una risposta. È possibile usare il middleware Express per aggiungere supporto a cookie, sessioni e utenti, ottenere parametri `POST`/`GET` e così via. È possibile usare qualsiasi meccanismo di database supportato da Node (Express non definisce alcun comportamento relativo ai database).

Le sezioni seguenti spiegano alcuni elementi comuni che si incontrano lavorando con codice _Express_ e _Node_.

### Helloworld Express

Consideriamo innanzitutto l'esempio standard [Hello World](https://expressjs.com/en/starter/hello-world/) di Express (ogni parte verrà trattata più avanti e nelle sezioni seguenti).

> [!NOTE]
> Se Node ed Express sono già installati (o vengono installati come mostrato nel [prossimo articolo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment)), è possibile salvare questo codice in un file di testo chiamato **app.js** ed eseguirlo in un prompt dei comandi bash chiamando:
>
> **`node ./app.js`**

```js
const express = require("express");

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}!`);
});
```

Le prime due righe eseguono `require()` (importano) il modulo express e creano un'[applicazione Express](https://expressjs.com/en/5x/api/#app). Questo oggetto, denominato tradizionalmente `app`, dispone di metodi per instradare richieste HTTP, configurare middleware, eseguire il rendering di view HTML, registrare un motore di template e modificare le [impostazioni dell'applicazione](https://expressjs.com/en/5x/api/#app.settings.table) che controllano il comportamento dell'applicazione (ad esempio, la modalità dell'ambiente, se le definizioni delle route distinguono tra maiuscole e minuscole e così via).

La parte centrale del codice (le tre righe che iniziano con `app.get`) mostra una _definizione di route_. Il metodo `app.get()` specifica una funzione callback che verrà invocata ogni volta che arriva una richiesta HTTP `GET` con un percorso (`'/'`) relativo alla radice del sito. La funzione callback riceve come argomenti un oggetto richiesta e un oggetto risposta e chiama [`send()`](https://expressjs.com/en/5x/api/#res.send) sulla risposta per restituire la stringa "Hello World!"

Il blocco finale avvia il server su una porta specificata ('3000') e stampa un messaggio di log nella console. Con il server in esecuzione, è possibile aprire `localhost:3000` nel browser per visualizzare la risposta dell'esempio.

### Importare e creare moduli

Un modulo è una libreria/file JavaScript che può essere importato in altro codice usando la funzione `require()` di Node. _Express_ stesso è un modulo, così come lo sono le librerie di middleware e database usate nelle applicazioni _Express_.

Il codice seguente mostra come importare un modulo tramite nome, usando come esempio il framework _Express_. Innanzitutto si invoca la funzione `require()`, specificando il nome del modulo come stringa (`'express'`), quindi si chiama l'oggetto restituito per creare un'[applicazione Express](https://expressjs.com/en/5x/api/#app). È quindi possibile accedere alle proprietà e alle funzioni dell'oggetto applicazione.

```js
const express = require("express");

const app = express();
```

È possibile inoltre creare moduli propri, importabili allo stesso modo.

> [!NOTE]
> Sarà opportuno creare moduli propri, poiché questo consente di organizzare il codice in parti gestibili: un'applicazione monolitica in un unico file è difficile da comprendere e mantenere. L'uso dei moduli aiuta inoltre a gestire lo spazio dei nomi, perché quando si usa un modulo vengono importate soltanto le variabili esportate esplicitamente.

Per rendere disponibili oggetti al di fuori di un modulo, è sufficiente esporli come proprietà aggiuntive dell'oggetto `exports`. Ad esempio, il modulo **square.js** seguente è un file che esporta i metodi `area()` e `perimeter()`:

```js
exports.area = function (width) {
  return width * width;
};
exports.perimeter = function (width) {
  return 4 * width;
};
```

È possibile importare questo modulo usando `require()`, quindi chiamare i metodi esportati come mostrato:

```js
const square = require("./square"); // Here we require() the name of the file without the (optional) .js file extension

console.log(`The area of a square with a width of 4 is ${square.area(4)}`);
```

> [!NOTE]
> È anche possibile specificare un percorso assoluto al modulo (oppure un nome, come fatto inizialmente).

Se si desidera esportare un oggetto completo con una sola assegnazione anziché costruirlo una proprietà alla volta, assegnarlo a `module.exports` come mostrato di seguito (è anche possibile farlo per rendere la radice dell'oggetto exports un costruttore o un'altra funzione):

```js
module.exports = {
  area(width) {
    return width * width;
  },

  perimeter(width) {
    return 4 * width;
  },
};
```

> [!NOTE]
> È possibile considerare `exports` come una [scorciatoia](https://nodejs.org/api/modules.html#modules_exports_shortcut) per `module.exports` all'interno di un dato modulo. In effetti, `exports` è soltanto una variabile inizializzata con il valore di `module.exports` prima che il modulo venga valutato. Tale valore è un riferimento a un oggetto (in questo caso un oggetto vuoto). Questo significa che `exports` mantiene un riferimento allo stesso oggetto referenziato da `module.exports`. Significa inoltre che, assegnando un altro valore a `exports`, esso non sarà più associato a `module.exports`.

Per molte più informazioni sui moduli, consultare [Modules](https://nodejs.org/api/modules.html#modules_modules) (documentazione API di Node).

### Usare API asincrone

Il codice JavaScript usa frequentemente API asincrone anziché sincrone per operazioni che possono richiedere un po' di tempo per essere completate. Un'API sincrona è un'API in cui ogni operazione deve terminare prima che possa iniziare l'operazione successiva. Ad esempio, le seguenti funzioni di log sono sincrone e stamperanno il testo nella console in ordine (First, Second).

```js
console.log("First");
console.log("Second");
```

Al contrario, un'API asincrona è un'API che avvia un'operazione e restituisce immediatamente il controllo (prima che l'operazione sia completata). Quando l'operazione termina, l'API usa un meccanismo per eseguire operazioni aggiuntive. Ad esempio, il codice seguente stamperà "Second, First" perché, anche se il metodo `setTimeout()` viene chiamato per primo e restituisce subito il controllo, l'operazione non viene completata per diversi secondi.

```js
setTimeout(() => {
  console.log("First");
}, 3000);
console.log("Second");
```

L'uso di API asincrone non bloccanti è ancora più importante in Node che nel browser, poiché le applicazioni _Node_ sono spesso scritte come ambiente di esecuzione guidato dagli eventi a singolo thread. "A singolo thread" significa che tutte le richieste al server vengono eseguite sullo stesso thread (anziché essere avviate in processi separati). Questo modello è estremamente efficiente in termini di velocità e risorse del server. Tuttavia, significa anche che se una qualsiasi funzione chiama metodi sincroni che richiedono molto tempo per completarsi, questi bloccheranno non solo la richiesta corrente, ma ogni altra richiesta gestita dall'applicazione web.

Esistono diversi modi in cui un'API asincrona può notificare all'applicazione di aver completato l'operazione. Storicamente, l'approccio usato consisteva nel registrare una funzione callback quando si invoca l'API asincrona, che viene poi chiamata al completamento dell'operazione (è l'approccio usato sopra).

> [!NOTE]
> L'uso di callback può diventare piuttosto complesso quando esiste una sequenza di operazioni asincrone dipendenti che devono essere eseguite in ordine, poiché questo porta a più livelli di callback annidate. Questo problema è comunemente noto come "callback hell".

> [!NOTE]
> Una convenzione comune per Node ed Express è usare callback error-first. In questa convenzione, il primo valore nelle _funzioni callback_ è un valore di errore, mentre gli argomenti successivi contengono i dati in caso di successo. Questo blog offre una buona spiegazione del motivo per cui questo approccio è utile: [The Node.js Way - Understanding Error-First Callbacks](https://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/) (fredkschott.com).

Il codice JavaScript moderno usa più comunemente [Promises](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise) e [async/await](/it/docs/Web/JavaScript/Reference/Statements/async_function) per gestire il flusso asincrono del programma.
Usare le promise quando possibile. Se si lavora con codice che usa callback, è possibile usare la funzione [`utils.promisify`](https://nodejs.org/api/util.html#utilpromisifyoriginal) di Node.js per gestire in modo pratico la conversione da callback a Promise.

### Creare handler delle route

Nell'esempio _Hello World_ di Express (vedere sopra), è stata definita una funzione handler di route (callback) per richieste HTTP `GET` alla radice del sito (`'/'`).

```js
app.get("/", (req, res) => {
  res.send("Hello World!");
});
```

La funzione callback riceve come argomenti un oggetto richiesta e un oggetto risposta. In questo caso, il metodo chiama [`send()`](https://expressjs.com/en/5x/api/#res.send) sulla risposta per restituire la stringa "Hello World!" Esistono diversi [altri metodi di risposta](https://expressjs.com/en/guide/routing/#response-methods) per terminare il ciclo richiesta/risposta; ad esempio, è possibile chiamare [`res.json()`](https://expressjs.com/en/5x/api/#res.json) per inviare una risposta JSON oppure [`res.sendFile()`](https://expressjs.com/en/5x/api/#res.sendFile) per inviare un file.

> [!NOTE]
> Nelle funzioni callback è possibile usare qualsiasi nome per gli argomenti; quando la callback viene invocata, il primo argomento sarà sempre la richiesta e il secondo sarà sempre la risposta. Ha senso denominarli in modo da poter identificare l'oggetto con cui si sta lavorando nel corpo della callback.

L'oggetto _applicazione Express_ fornisce anche metodi per definire handler di route per tutti gli altri verbi HTTP, che vengono perlopiù usati esattamente nello stesso modo:

`checkout()`, `copy()`, **`delete()`**, **`get()`**, `head()`, `lock()`, `merge()`, `mkactivity()`, `mkcol()`, `move()`, `m-search()`, `notify()`, `options()`, `patch()`, **`post()`**, `purge()`, **`put()`**, `report()`, `search()`, `subscribe()`, `trace()`, `unlock()`, `unsubscribe()`.

Esiste un metodo speciale di routing, `app.all()`, che viene chiamato in risposta a qualsiasi metodo HTTP. Viene usato per caricare funzioni middleware in un determinato percorso per tutti i metodi di richiesta. L'esempio seguente (dalla documentazione di Express) mostra un handler che verrà eseguito per le richieste a `/secret` indipendentemente dal verbo HTTP usato, purché sia supportato dal [modulo http](https://nodejs.org/docs/latest/api/http.html#httpmethods).

```js
app.all("/secret", (req, res, next) => {
  console.log("Accessing the secret section…");
  next(); // pass control to the next handler
});
```

Le route consentono di trovare corrispondenze con particolari pattern di caratteri in un URL, estrarre alcuni valori dall'URL e passarli come parametri all'handler di route (come attributi dell'oggetto richiesta passato come parametro).

Spesso è utile raggruppare gli handler di route per una particolare parte di un sito e accedervi usando un prefisso di route comune (ad esempio, un sito con una Wiki potrebbe avere tutte le route relative alla wiki in un file e renderle accessibili con un prefisso di route _/wiki/_). In _Express_ questo si ottiene usando l'oggetto [`express.Router`](https://expressjs.com/en/guide/routing/#express-router). Ad esempio, è possibile creare la route wiki in un modulo chiamato **wiki.js**, quindi esportare l'oggetto `Router`, come mostrato di seguito:

```js
// wiki.js - Wiki route module

const express = require("express");

const router = express.Router();

// Home page route
router.get("/", (req, res) => {
  res.send("Wiki home page");
});

// About page route
router.get("/about", (req, res) => {
  res.send("About this wiki");
});

module.exports = router;
```

> [!NOTE]
> L'aggiunta di route all'oggetto `Router` è identica all'aggiunta di route all'oggetto `app` (come mostrato in precedenza).

Per usare il router nel file principale dell'app, è quindi necessario eseguire `require()` del modulo di route (**wiki.js**) e chiamare `use()` sull'applicazione _Express_ per aggiungere il Router al percorso di gestione del middleware. Le due route saranno quindi accessibili da `/wiki/` e `/wiki/about/`.

```js
const wiki = require("./wiki.js");

// …
app.use("/wiki", wiki);
```

Più avanti, nella sezione collegata [Route e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes), verranno mostrati molti altri dettagli sul lavoro con le route e, in particolare, sull'uso di `Router`.

### Usare il middleware

Il middleware viene usato ampiamente nelle app Express, per attività che vanno dalla distribuzione di file statici alla gestione degli errori, fino alla compressione delle risposte HTTP. Mentre le funzioni di route terminano il ciclo richiesta-risposta HTTP restituendo una risposta al client HTTP, le funzioni middleware _tipicamente_ eseguono un'operazione sulla richiesta o sulla risposta e poi chiamano la funzione successiva nello "stack", che può essere altro middleware oppure un handler di route. L'ordine in cui viene chiamato il middleware dipende dallo sviluppatore dell'app.

> [!NOTE]
> Il middleware può eseguire qualsiasi operazione, eseguire qualsiasi codice, apportare modifiche agli oggetti richiesta e risposta e può _anche terminare il ciclo richiesta-risposta_. Se non termina il ciclo, deve chiamare `next()` per passare il controllo alla funzione middleware successiva (altrimenti la richiesta rimarrà in sospeso).

La maggior parte delle app usa middleware _di terze parti_ per semplificare attività comuni dello sviluppo web, come lavorare con cookie, sessioni, autenticazione utente, accesso a dati `POST` e JSON della richiesta, logging e così via. È possibile trovare un [elenco di pacchetti middleware gestiti dal team di Express](https://expressjs.com/en/resources/middleware/) (che include anche altri popolari pacchetti di terze parti). Altri pacchetti Express sono disponibili nel package manager npm.

Per usare un middleware di terze parti, occorre prima installarlo nell'app usando npm.
Ad esempio, per installare il middleware di logging delle richieste HTTP [morgan](https://expressjs.com/en/resources/middleware/morgan/), occorre eseguire:

```bash
npm install morgan
```

È quindi possibile chiamare `use()` sull'_oggetto applicazione Express_ per aggiungere il middleware allo stack:

```js
const express = require("express");
const logger = require("morgan");

const app = express();
app.use(logger("dev"));
// …
```

> [!NOTE]
> Le funzioni middleware e di routing vengono chiamate nell'ordine in cui sono dichiarate. Per alcuni middleware l'ordine è importante (ad esempio, se il middleware di sessione dipende dal middleware dei cookie, l'handler dei cookie deve essere aggiunto per primo). Quasi sempre il middleware viene chiamato prima dell'impostazione delle route, altrimenti gli handler di route non avranno accesso alle funzionalità aggiunte dal middleware.

È possibile scrivere funzioni middleware personalizzate e probabilmente sarà necessario farlo (anche solo per creare codice di gestione degli errori). L'**unica** differenza tra una funzione middleware e una callback di handler di route è che le funzioni middleware hanno un terzo argomento `next`, che devono chiamare se non sono quelle che completano il ciclo di richiesta (quando la funzione middleware viene chiamata, questo contiene la funzione _successiva_ che deve essere chiamata).

È possibile aggiungere una funzione middleware alla catena di elaborazione per _tutte le risposte_ con `app.use()`, oppure per uno specifico verbo HTTP usando il metodo associato: `app.get()`, `app.post()` e così via. Le route vengono specificate allo stesso modo in entrambi i casi, sebbene la route sia facoltativa quando si chiama `app.use()`.

L'esempio seguente mostra come aggiungere la funzione middleware usando entrambi gli approcci, con e senza una route.

```js
const express = require("express");

const app = express();

// An example middleware function
function middlewareFunction(req, res, next) {
  // Perform some operations
  next(); // Call next() so Express will call the next middleware function in the chain.
}

// Function added with use() for all routes and verbs
app.use(middlewareFunction);

// Function added with use() for a specific route
app.use("/some-route", middlewareFunction);

// A middleware function added for a specific HTTP verb and route
app.get("/", middlewareFunction);

app.listen(3000);
```

> [!NOTE]
> Sopra la funzione middleware viene dichiarata separatamente e poi impostata come callback. Nella precedente funzione handler di route, la funzione callback veniva dichiarata nel momento in cui era usata. In JavaScript, entrambi gli approcci sono validi.

La documentazione di Express offre ulteriore eccellente documentazione sull'[uso](https://expressjs.com/en/guide/using-middleware/) e sulla [scrittura](https://expressjs.com/en/guide/writing-middleware/) del middleware Express.

### Servire file statici

È possibile usare il middleware [express.static](https://expressjs.com/en/5x/api/#express.static) per servire file statici, incluse immagini, CSS e JavaScript (`static()` è l'unica funzione middleware che è effettivamente **parte** di _Express_). Ad esempio, usare la riga seguente per servire immagini, file CSS e file JavaScript da una directory denominata '**public'** allo stesso livello da cui viene chiamato Node:

```js
app.use(express.static("public"));
```

Tutti i file nella directory public vengono serviti aggiungendo il loro nome file (_relativo_ alla directory di base "public") all'URL di base. Ad esempio:

```plain
http://localhost:3000/images/dog.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/about.html
```

È possibile chiamare `static()` più volte per servire più directory. Se un file non viene trovato da una funzione middleware, verrà passato al middleware successivo (l'ordine in cui viene chiamato il middleware dipende dall'ordine di dichiarazione).

```js
app.use(express.static("public"));
app.use(express.static("media"));
```

È inoltre possibile creare un prefisso virtuale per gli URL statici, anziché aggiungere i file all'URL di base. Ad esempio, qui viene [specificato un percorso di mount](https://expressjs.com/en/5x/api/#app.use) affinché i file siano caricati con il prefisso "/media":

```js
app.use("/media", express.static("public"));
```

Ora è possibile caricare i file nella directory `public` dal prefisso di percorso `/media`.

```plain
http://localhost:3000/media/images/dog.jpg
http://localhost:3000/media/video/cat.mp4
http://localhost:3000/media/cry.mp3
```

> [!NOTE]
> Vedere anche [Serving static files in Express](https://expressjs.com/en/starter/static-files/).

### Gestire gli errori

Gli errori vengono gestiti da una o più funzioni middleware speciali che hanno quattro argomenti, anziché i soliti tre: `(err, req, res, next)`. Ad esempio:

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

Queste possono restituire qualsiasi contenuto necessario, ma devono essere chiamate dopo tutte le altre chiamate a `app.use()` e alle route, in modo da essere l'ultimo middleware nel processo di gestione delle richieste!

Express include un handler di errori integrato, che si occupa di tutti gli errori rimanenti che potrebbero verificarsi nell'app. Questa funzione middleware predefinita per la gestione degli errori viene aggiunta alla fine dello stack delle funzioni middleware. Se viene passato un errore a `next()` e non viene gestito in un handler di errori, sarà gestito dall'handler integrato; l'errore verrà scritto al client con lo stack trace.

> [!NOTE]
> Lo stack trace non è incluso nell'ambiente di produzione. Per eseguirlo in modalità produzione è necessario impostare la variabile d'ambiente `NODE_ENV` su `"production"`.

> [!NOTE]
> HTTP404 e altri codici di stato di "errore" non vengono trattati come errori. Per gestirli, è possibile aggiungere una funzione middleware. Per ulteriori informazioni, vedere le [FAQ](https://expressjs.com/en/starter/faq/#how-do-i-handle-404-responses).

Per ulteriori informazioni, vedere [Error handling](https://expressjs.com/en/guide/error-handling/) (documentazione di Express).

### Usare database

Le app _Express_ possono usare qualsiasi meccanismo di database supportato da _Node_ (_Express_ stesso non definisce comportamenti/requisiti aggiuntivi specifici per la gestione dei database). Esistono molte opzioni, tra cui PostgreSQL, MySQL, Redis, SQLite, MongoDB e così via.

Per usarli, occorre prima installare il driver del database usando npm. Ad esempio, per installare il driver per il popolare database NoSQL MongoDB, usare il comando:

```bash
npm install mongodb
```

Il database stesso può essere installato localmente o su un server cloud. Nel codice Express si importa il driver, ci si connette al database e quindi si eseguono operazioni di creazione, lettura, aggiornamento ed eliminazione (CRUD).
L'esempio seguente mostra come trovare record "mammal" usando MongoDB:

```js
const { MongoClient } = require("mongodb");

const uri = "mongodb://localhost:27017";
const client = new MongoClient(uri);

async function run() {
  try {
    await client.connect();
    const db = client.db("animals");
    const mammals = await db.collection("mammals").find().toArray();
    console.log(mammals);
  } finally {
    await client.close();
  }
}

run().catch(console.error);
```

Un altro approccio popolare consiste nell'accedere al database indirettamente, tramite un Object Relational Mapper ("ORM"). In questo approccio si definiscono i dati come "oggetti" o "modelli" e l'ORM li mappa nel formato del database sottostante. Questo approccio offre il vantaggio di consentire allo sviluppatore di continuare a ragionare in termini di oggetti JavaScript anziché di semantica del database, nonché di avere un punto evidente in cui eseguire la validazione e il controllo dei dati in ingresso. I database verranno trattati più approfonditamente in un articolo successivo.

Per ulteriori informazioni, vedere [Database integration](https://expressjs.com/en/guide/database-integration/) (documentazione di Express).

### Eseguire il rendering dei dati (view)

I motori di template (chiamati anche "view engine" in _Express_) consentono di specificare la _struttura_ di un documento di output in un template, usando segnaposto per i dati che verranno riempiti quando viene generata una pagina. I template vengono spesso usati per creare HTML, ma possono creare anche altri tipi di documenti.

Express supporta diversi motori di template, in particolare Pug (in precedenza "Jade"), Mustache ed EJS. Ciascuno ha i propri punti di forza per affrontare casi d'uso specifici (confronti relativi possono essere facilmente trovati tramite una ricerca su Internet).
Il generatore di applicazioni Express usa Jade come predefinito, ma supporta anche molti altri motori.

Nel codice delle impostazioni dell'applicazione, impostare il motore di template da usare e la posizione in cui Express deve cercare i template usando le impostazioni 'views' e 'view engine', come mostrato di seguito (sarà inoltre necessario installare il pacchetto contenente la libreria di template).

```js
const express = require("express");
const path = require("path");

const app = express();

// Set directory to contain the templates ('views')
app.set("views", path.join(__dirname, "views"));

// Set view engine to use, in this case 'some_template_engine_name'
app.set("view engine", "some_template_engine_name");
```

L'aspetto del template dipenderà dal motore usato. Supponendo di avere un file di template denominato "index.\<template_extension>" contenente segnaposto per variabili di dati denominate 'title' e "message", chiamare [`Response.render()`](https://expressjs.com/en/5x/api/#res.render) in una funzione handler di route per creare e inviare la risposta HTML:

```js
app.get("/", (req, res) => {
  res.render("index", { title: "About dogs", message: "Dogs rock!" });
});
```

Per ulteriori informazioni, vedere [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines/) (documentazione di Express).

### Struttura dei file

Express non fa supposizioni riguardo alla struttura o ai componenti usati. Route, view, file statici e altra logica specifica dell'applicazione possono trovarsi in qualsiasi numero di file con qualsiasi struttura di directory. Sebbene sia perfettamente possibile avere l'intera applicazione _Express_ in un solo file, in genere è opportuno suddividere l'applicazione in file in base alla funzione (ad esempio gestione degli account, blog, forum di discussione) e al dominio del problema architetturale (ad esempio modello, view o controller, se si usa un'{{Glossary("MVC", "architettura MVC")}}).

In un argomento successivo verrà usato l'_Express Application Generator_, che crea uno scheletro modulare dell'app facilmente estendibile per creare applicazioni web.

## Riepilogo

Complimenti, il primo passo nel percorso con Express/Node è completato! Ora dovrebbero essere chiari i principali vantaggi di Express e Node, nonché l'aspetto approssimativo delle parti principali di un'app Express (route, middleware, gestione degli errori e codice dei template). Dovrebbe inoltre essere chiaro che, poiché Express è un framework unopinionated, il modo in cui queste parti vengono assemblate e le librerie usate dipendono in gran parte dallo sviluppatore.

Naturalmente Express è intenzionalmente un framework per applicazioni web molto leggero, quindi gran parte dei suoi vantaggi e del suo potenziale deriva da librerie e funzionalità di terze parti. Questi aspetti verranno esaminati più nel dettaglio negli articoli seguenti. Nel prossimo articolo verrà illustrata la configurazione di un ambiente di sviluppo Node, così da poter iniziare a vedere del codice Express in azione.

## Vedere anche

- [Learn Node.js](https://scrimba.com/learn-nodejs-c00ho9qqh6?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce un'introduzione divertente e interattiva a Node.js.
- [Learn Express.js](https://scrimba.com/learn-expressjs-c062las154?via=mdn) di Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> si basa sul collegamento precedente, mostrando come iniziare a usare il framework Express per creare siti web lato server.
- [Modules](https://nodejs.org/api/modules.html#modules_modules) (documentazione API di Node)
- [Express](https://expressjs.com/) (pagina iniziale)
- [Basic routing](https://expressjs.com/en/starter/basic-routing/) (documentazione di Express)
- [Routing guide](https://expressjs.com/en/guide/routing/) (documentazione di Express)
- [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines/) (documentazione di Express)
- [Using middleware](https://expressjs.com/en/guide/using-middleware/) (documentazione di Express)
- [Writing middleware for use in Express apps](https://expressjs.com/en/guide/writing-middleware/) (documentazione di Express)
- [Database integration](https://expressjs.com/en/guide/database-integration/) (documentazione di Express)
- [Serving static files in Express](https://expressjs.com/en/starter/static-files/) (documentazione di Express)
- [Error handling](https://expressjs.com/en/guide/error-handling/) (documentazione di Express)

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
