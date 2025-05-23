---
title: Introduzione a Express/Node
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo primo articolo su Express rispondiamo alle domande "Cos'è Node?" e "Cos'è Express?", e forniamo una panoramica su ciò che rende speciale il framework web Express. Delineeremo le caratteristiche principali e mostreremo alcuni dei principali componenti di un'applicazione Express (anche se a questo punto non avrai ancora un ambiente di sviluppo in cui testarlo).

> [!WARNING]
> Il tutorial su Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Abbiamo in programma di aggiornare la documentazione nella seconda metà del 2025.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione generale della <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps">programmazione del sito web lato server</a>, e in particolare la meccanica delle <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview">interazioni client-server nei siti web</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con cos'è Express e come si inserisce in Node, quali funzionalità fornisce e i principali componenti di un'applicazione Express.
      </td>
    </tr>
  </tbody>
</table>

## Introduzione a Node

[Node](https://nodejs.org/) (o più formalmente _Node.js_) è un ambiente di runtime open-source e multipiattaforma che permette agli sviluppatori di creare tutti i tipi di strumenti e applicazioni server-side in {{Glossary("JavaScript", "JavaScript")}}.
L'ambiente di runtime è pensato per l'uso al di fuori di un contesto browser (cioè, eseguendo direttamente su un computer o un sistema operativo server). Come tale, l'ambiente esclude le API JavaScript specifiche del browser e aggiunge il supporto per le API OS più tradizionali, inclusi le librerie HTTP e del file system.

Da una prospettiva di sviluppo server web, Node ha una serie di vantaggi:

- Ottime prestazioni! Node è stato progettato per ottimizzare il throughput e la scalabilità nelle applicazioni web ed è una buona soluzione per molti problemi comuni nello sviluppo web (ad es. applicazioni web in tempo reale).
- Il codice è scritto in "JavaScript semplice", il che significa che si spende meno tempo nel "cambio di contesto" tra linguaggi quando si scrive sia il codice lato client sia lato server.
- JavaScript è un linguaggio di programmazione relativamente nuovo e beneficia di miglioramenti nel design del linguaggio rispetto ad altri linguaggi tradizionali per server web (ad es. Python, PHP, ecc.) Molti altri linguaggi nuovi e popolari vengono compilati o convertiti in JavaScript, quindi puoi anche usare TypeScript, CoffeeScript, ClojureScript, Scala, LiveScript, ecc.
- Il gestore di pacchetti di node (npm) fornisce accesso a centinaia di migliaia di pacchetti riutilizzabili. Ha anche la migliore risoluzione delle dipendenze e può essere usato per automatizzare la maggior parte della catena di strumenti di build.
- Node.js è portatile. È disponibile su Microsoft Windows, macOS, Linux, Solaris, FreeBSD, OpenBSD, WebOS e NonStop OS. Inoltre, è ben supportato da molti fornitori di hosting web, che spesso forniscono infrastrutture specifiche e documentazione per ospitare siti Node.
- Ha un ecosistema di terze parti molto attivo e una comunità di sviluppatori, con molte persone disposte a dare una mano.

Puoi usare Node.js per creare un semplice server web utilizzando il pacchetto HTTP di Node.

### Hello Node.js

Il seguente esempio crea un server web che ascolta qualsiasi tipo di richiesta HTTP sull'URL `http://127.0.0.1:8000/` — quando viene ricevuta una richiesta, lo script risponderà con la stringa: "Hello World". Se hai già installato node, puoi seguire questi passaggi per provare l'esempio:

1. Apri Terminale (su Windows, apri l'utilità della riga di comando)
2. Crea la cartella in cui vuoi salvare il programma, ad esempio `test-node` e poi entra in essa inserendo il seguente comando nel tuo terminale:

   ```bash
   cd test-node
   ```

3. Utilizzando il tuo editor di testo preferito, crea un file chiamato `hello.js` e incolla il seguente codice:

   ```js
   // Load HTTP module
   const http = require("http");

   const hostname = "127.0.0.1";
   const port = 8000;

   // Create HTTP server
   const server = http.createServer(function (req, res) {
     // Set the response HTTP header with HTTP status and Content type
     res.writeHead(200, { "Content-Type": "text/plain" });

     // Send the response body "Hello World"
     res.end("Hello World\n");
   });

   // Prints a log once the server starts listening
   server.listen(port, hostname, function () {
     console.log(`Server running at http://${hostname}:${port}/`);
   });
   ```

4. Salva il file nella cartella che hai creato sopra.
5. Torna al terminale e digita il seguente comando:

   ```bash
   node hello.js
   ```

Infine, naviga verso `http://localhost:8000` nel tuo browser web; dovresti vedere il testo "**Hello World**" nell'angolo superiore sinistro di una pagina web altrimenti vuota.

> [!NOTE]
> Se vuoi provare un po' di codice Node.js senza dover fare alcuna configurazione locale, [Aside: The HTTP module](https://scrimba.com/learn-nodejs-c00ho9qqh6/~07du?via=mdn) di Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre una guida interattiva alla creazione di un server di base con il pacchetto Node HTTP.

## Framework web

Altri compiti comuni nello sviluppo web non sono supportati direttamente da Node stesso. Se desideri aggiungere una gestione specifica per i diversi verbi HTTP (ad es., `GET`, `POST`, `DELETE`, ecc.), gestire separatamente le richieste su diversi percorsi URL ("routes"), servire file statici o utilizzare modelli per creare dinamicamente la risposta, Node non sarà di grande aiuto da solo. Dovrai scrivere il codice da te, oppure puoi evitare di reinventare la ruota e usare un framework web!

## Introduzione a Express

[Express](https://expressjs.com/) è il framework web Node.js più popolare, ed è la libreria sottostante a una serie di altri popolari framework Node.js. Fornisce meccanismi per:

- Scrivere gestori per le richieste con diversi verbi HTTP in diversi percorsi URL (routes).
- Integrare motori di rendering "view" per generare risposte inserendo dati nei modelli.
- Impostare configurazioni comuni delle applicazioni web come la porta da usare per connettersi e la posizione dei modelli che vengono usati per il rendering della risposta.
- Aggiungere "middleware" di elaborazione delle richieste aggiuntivi in qualsiasi punto della pipeline di gestione delle richieste.

Sebbene _Express_ stesso sia abbastanza minimale, gli sviluppatori hanno creato pacchetti middleware compatibili per affrontare quasi tutti i problemi di sviluppo web. Ci sono librerie per lavorare con cookie, sessioni, accessi utente, parametri URL, dati `POST`, intestazioni di sicurezza e _molto_ altro. Puoi trovare un elenco di pacchetti middleware mantenuti dal team Express su [Express Middleware](https://expressjs.com/en/resources/middleware.html) (insieme ad un elenco di alcuni pacchetti di terze parti popolari).

> [!NOTE]
> Questa flessibilità è una lama a doppio taglio. Ci sono pacchetti middleware per affrontare quasi ogni problema o requisito, ma capire i pacchetti giusti da usare può essere a volte una sfida. Non esiste nemmeno un "modo giusto" per strutturare un'applicazione, e molti esempi che potresti trovare su Internet non sono ottimali, o mostrano solo una piccola parte di ciò che devi fare per sviluppare un'applicazione web.

## Da dove provengono Node ed Express?

Node è stato rilasciato inizialmente, solo per Linux, nel 2009. Il gestore di pacchetti npm è stato rilasciato nel 2010 e il supporto nativo di Windows è stato aggiunto nel 2012. Approfondisci su [Wikipedia](https://en.wikipedia.org/wiki/Node.js#History) se vuoi saperne di più.

Express è stato rilasciato inizialmente nel novembre 2010 ed è attualmente alla versione principale 5 delle API. Puoi controllare il [changelog](https://expressjs.com/en/changelog/#5.x) per informazioni sui cambiamenti nell'attuale versione e [GitHub](https://github.com/expressjs/express/blob/master/History.md) per note di rilascio storiche più dettagliate.

## Quanto sono popolari Node ed Express?

La popolarità di un framework web è importante perché è un indicatore del fatto che continuerà ad essere mantenuto e di quali risorse saranno probabilmente disponibili in termini di documentazione, librerie aggiuntive e supporto tecnico.

Non esiste una misura disponibile e definitiva della popolarità dei framework server-side (anche se puoi stimare la popolarità usando meccanismi come contare il numero di progetti su GitHub e le domande su Stack Overflow per ciascuna piattaforma). Una domanda migliore è se Node ed Express sono "abbastanza popolari" per evitare i problemi delle piattaforme impopolari. Continuano ad evolversi? Puoi ottenere aiuto se ne hai bisogno? C'è un'opportunità di lavoro retribuito se impari Express?

Basandoti sul numero di aziende di alto profilo che utilizzano Express, sul numero di persone che contribuiscono al codice, e sul numero di persone che forniscono supporto gratuito e a pagamento, allora sì, _Express_ è un framework popolare!

## Express è opinato?

I framework web spesso si definiscono "opinionati" o "non opinionati".

I framework opinionati sono quelli con opinioni su "come gestire" un compito particolare. Spesso supportano uno sviluppo rapido _in un particolare dominio_ (risolvendo problemi di un tipo particolare) perché il modo giusto di fare le cose è generalmente ben compreso e ben documentato. Tuttavia possono essere meno flessibili nell'affrontare problemi al di fuori del loro dominio principale e tendono a offrire meno scelte su quali componenti e approcci possono essere utilizzati.

I framework non opinionati, al contrario, hanno molte meno restrizioni sul modo migliore di unire i componenti per raggiungere un obiettivo, o anche su quali componenti debbano essere usati. Rendono più facile per gli sviluppatori usare gli strumenti più adatti per completare un compito particolare, anche a costo di trovare quei componenti da soli.

Express è non opinionato. Puoi inserire quasi qualsiasi middleware compatibile che desideri nella catena di gestione delle richieste, in quasi qualsiasi ordine tu voglia. Puoi strutturare l'applicazione in un file o in più file e utilizzando qualsiasi struttura di directory. Potresti a volte sentirti di avere troppe scelte!

## Come appare il codice Express?

In un sito web tradizionale basato su dati, un'applicazione web attende richieste HTTP dal browser web (o da un altro client). Quando viene ricevuta una richiesta, l'applicazione determina quale azione è necessaria in base al modello URL e possibilmente ai dati contenuti nei dati `POST` o `GET`. A seconda di ciò che è richiesto, può leggere o scrivere informazioni da un database o svolgere altre attività necessarie per soddisfare la richiesta. L'applicazione restituirà quindi una risposta al browser web, spesso creando dinamicamente una pagina HTML per il browser, inserendo i dati recuperati in segnaposti in un modello HTML.

Express fornisce metodi per specificare quale funzione viene chiamata per un particolare verbo HTTP (`GET`, `POST`, `PUT`, ecc.) e modello URL ("Route"), e metodi per specificare quale motore di template ("view") viene utilizzato, dove si trovano i file dei template e quale template usare per rendere una risposta. Puoi utilizzare il middleware Express per aggiungere supporto per cookie, sessioni e utenti, ottenendo parametri `POST`/`GET`, ecc. Puoi utilizzare qualsiasi meccanismo di database supportato da Node (Express non definisce alcun comportamento specifico relativo al database).

Le sezioni seguenti spiegano alcune delle cose comuni che vedrai quando lavori con il codice _Express_ e _Node_.

### Helloworld Express

Innanzitutto consideriamo l'esempio standard di [Hello World](https://expressjs.com/en/starter/hello-world.html) di Express (discutiamo ogni parte di questo qui sotto, e nelle sezioni seguenti).

> [!NOTE]
> Se hai già installato Node e Express (o se li installi come mostrato nel [prossimo articolo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment)), puoi salvare questo codice in un file di testo chiamato **app.js** e eseguirlo in un prompt dei comandi bash chiamando:
>
> **`node ./app.js`**

```js
const express = require("express");

const app = express();
const port = 3000;

app.get("/", function (req, res) {
  res.send("Hello World!");
});

app.listen(port, function () {
  console.log(`Example app listening on port ${port}!`);
});
```

Le prime due righe `require()` (importano) il modulo express e creano un [Express application](https://expressjs.com/en/4x/api.html#app). Questo oggetto, che è tradizionalmente chiamato `app`, ha metodi per il routing delle richieste HTTP, la configurazione dei middleware, il rendering delle viste HTML, la registrazione di un motore di template e la modifica delle [application settings](https://expressjs.com/en/4x/api.html#app.settings.table) che controllano come si comporta l'applicazione (ad es., la modalità ambiente, se le definizioni delle rotte sono sensibili al caso, ecc.)

La parte centrale del codice (le tre righe che iniziano con `app.get`) mostrano una _definizione di rotta_. Il metodo `app.get()` specifica una funzione di callback che verrà invocata ogni volta che c'è una richiesta HTTP `GET` con un percorso (`'/'`) relativo alla radice del sito. La funzione di callback prende un oggetto richiesta e un oggetto risposta come argomenti, e chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "Hello World!"

L'ultimo blocco avvia il server su una porta specificata ('3000') e stampa un commento di log nella console. Con il server in esecuzione, potresti andare su `localhost:3000` nel tuo browser per vedere la risposta di esempio restituita.

### Importazione e creazione di moduli

Un modulo è una libreria/file JavaScript che puoi importare in altro codice usando la funzione `require()` di Node. _Express_ stesso è un modulo, così come lo sono le librerie middleware e di database che utilizziamo nelle nostre applicazioni _Express_.

Il codice qui sotto mostra come importare un modulo per nome, usando il framework _Express_ come esempio. Innanzitutto invocchiamo la funzione `require()`, specificando il nome del modulo come stringa (`'express'`), e chiamando l'oggetto restituito per creare un [Express application](https://expressjs.com/en/4x/api.html#app). Possiamo quindi accedere alle proprietà e alle funzioni dell'oggetto applicazione.

```js
const express = require("express");

const app = express();
```

Puoi anche creare i tuoi moduli che possono essere importati nello stesso modo.

> [!NOTE] > _Vorrai_ creare i tuoi moduli, perché questo ti permette di organizzare il tuo codice in parti gestibili — un'applicazione monolitica e basata su un solo file è difficile da comprendere e mantenere. L'uso di moduli ti aiuta anche a gestire il tuo spazio dei nomi, poiché solo le variabili che esporti esplicitamente vengono importate quando usi un modulo.

Per rendere disponibili oggetti all'esterno di un modulo devi solo esporli come ulteriori proprietà sull'oggetto `exports`. Ad esempio, il modulo **square.js** qui sotto è un file che esporta i metodi `area()` e `perimeter()`:

```js
exports.area = function (width) {
  return width * width;
};
exports.perimeter = function (width) {
  return 4 * width;
};
```

Possiamo importare questo modulo usando `require()`, e poi chiamare il/i metodo/i esportato/i come mostrato:

```js
const square = require("./square"); // Here we require() the name of the file without the (optional) .js file extension

console.log(`The area of a square with a width of 4 is ${square.area(4)}`);
```

> [!NOTE]
> Puoi anche specificare un percorso assoluto al modulo (o un nome, come abbiamo fatto inizialmente).

Se vuoi esportare un oggetto completo in un'unica assegnazione invece di costruirlo una proprietà alla volta, assegnalo a `module.exports` come mostrato qui sotto (puoi anche fare ciò per rendere la radice dell'oggetto delle esportazioni un costruttore o altra funzione):

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
> Puoi pensare ad `exports` come a un [collegamento rapido](https://nodejs.org/api/modules.html#modules_exports_shortcut) a `module.exports` all'interno di un dato modulo. Infatti, `exports` è solo una variabile che viene inizializzata al valore di `module.exports` prima che il modulo venga valutato. Tale valore è un riferimento a un oggetto (un oggetto vuoto in questo caso). Questo significa che `exports` contiene un riferimento allo stesso oggetto referenziato da `module.exports`. Significa anche che assegnando un altro valore a `exports` non è più legato a `module.exports`.

Per molte più informazioni sui moduli vedere [Modules](https://nodejs.org/api/modules.html#modules_modules) (Node API docs).

### Uso delle API asincrone

Il codice JavaScript utilizza frequentemente API asincrone anziché sincrone per operazioni che possono richiedere tempo per completarsi. Un'API sincrona è una in cui ogni operazione deve completarsi prima che la successiva possa iniziare. Ad esempio, le seguenti funzioni di log sono sincrone e stamperanno il testo sulla console in ordine (Prima, Seconda).

```js
console.log("First");
console.log("Second");
```

Al contrario, un'API asincrona è una in cui l'API avvierà un'operazione e ritornerà immediatamente (prima che l'operazione sia completata). Una volta che l'operazione termina, l'API userà qualche meccanismo per eseguire ulteriori operazioni. Ad esempio, il codice qui sotto stamperà "Seconda, Prima" perché, anche se il metodo `setTimeout()` viene chiamato per primo e ritorna immediatamente, l'operazione non si completa per diversi secondi.

```js
setTimeout(function () {
  console.log("First");
}, 3000);
console.log("Second");
```

Usare API asincrone non bloccanti è ancora più importante in Node che nel browser perché _Node_ è un ambiente di esecuzione a thread singolo guidato da eventi. "Thread singolo" significa che tutte le richieste al server vengono eseguite sullo stesso thread (anziché essere generate in processi separati). Questo modello è estremamente efficiente in termini di velocità e risorse del server, ma significa che se una qualsiasi delle tue funzioni richiama metodi sincroni che richiedono molto tempo per completarsi, bloccheranno non solo la richiesta corrente, ma tutte le altre richieste gestite dalla tua applicazione web.

Ci sono diversi modi per un'API asincrona di notificare alla tua applicazione che è stata completata. Il modo più comune è registrare una funzione di callback quando richiami l'API asincrona, che verrà richiamata quando l'operazione si completa. Questo è l'approccio utilizzato sopra.

> [!NOTE]
> Usare i callback può essere piuttosto "disordinato" se hai una sequenza di operazioni asincrone dipendenti che devono essere eseguite in ordine, perché ciò causa più livelli di callback annidati. Questo problema è comunemente noto come "callback hell". Questo problema può essere ridotto con buone pratiche di codice (vedi <http://callbackhell.com/>), usando un modulo come [async](https://www.npmjs.com/package/async), o rifattorizzando il codice alle funzionalità native di JavaScript come [Promises](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise) e [async/await](/it/docs/Web/JavaScript/Reference/Statements/async_function). Node offre la funzione [`utils.promisify`](https://nodejs.org/api/util.html#utilpromisifyoriginal) per fare la conversione callback → Promise in modo ergonomico.

> [!NOTE]
> Una convenzione comune per Node ed Express è usare callback con errore-primo. In questa convenzione, il primo valore nelle tue _funzioni di callback_ è un valore di errore, mentre i successivi argomenti contengono dati di successo. C'è una buona spiegazione di perché questo approccio sia utile in questo blog: [The Node.js Way - Understanding Error-First Callbacks](https://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/) (fredkschott.com).

### Creazione dei gestori di rotte

Nel nostro esempio di Express _Hello World_ (vedi sopra), abbiamo definito una funzione di gestione delle rotte (callback) per le richieste HTTP `GET` alla radice del sito (`'/'`).

```js
app.get("/", function (req, res) {
  res.send("Hello World!");
});
```

La funzione di callback prende un oggetto richiesta e un oggetto risposta come argomenti. In questo caso, il metodo richiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "Hello World!" Ci sono un [numero di altri metodi di risposta](https://expressjs.com/en/guide/routing.html#response-methods) per terminare il ciclo richiesta/risposta, ad esempio, potresti chiamare [`res.json()`](https://expressjs.com/en/4x/api.html#res.json) per inviare una risposta JSON o [`res.sendFile()`](https://expressjs.com/en/4x/api.html#res.sendFile) per inviare un file.

> [!NOTE]
> Puoi usare qualsiasi nome di argomento ti piaccia nelle funzioni di callback; quando la callback viene invocata, il primo argomento sarà sempre la richiesta e il secondo sarà sempre la risposta. Ha senso nominarli in modo da poter identificare l'oggetto con cui stai lavorando nel corpo della callback.

L'oggetto _Express application_ fornisce anche metodi per definire gestori di rotte per tutti gli altri verbi HTTP, che sono usati principalmente nello stesso modo:

`checkout()`, `copy()`, **`delete()`**, **`get()`**, `head()`, `lock()`, `merge()`, `mkactivity()`, `mkcol()`, `move()`, `m-search()`, `notify()`, `options()`, `patch()`, **`post()`**, `purge()`, **`put()`**, `report()`, `search()`, `subscribe()`, `trace()`, `unlock()`, `unsubscribe()`.

C'è un metodo di routing speciale, `app.all()`, che verrà chiamato in risposta a qualsiasi metodo HTTP. Questo viene usato per caricare funzioni middleware in un percorso particolare per tutti i metodi di richiesta. L'esempio seguente (dalla documentazione di Express) mostra un gestore che verrà eseguito per le richieste a `/secret` indipendentemente dal verbo HTTP utilizzato (purché sia supportato dal [modulo HTTP](https://nodejs.org/docs/latest/api/http.html#httpmethods)).

```js
app.all("/secret", function (req, res, next) {
  console.log("Accessing the secret section…");
  next(); // pass control to the next handler
});
```

Le rotte ti permettono di confrontare particolari schemi di caratteri in un URL ed estrarre alcuni valori dall'URL e passarli come parametri al gestore della rotta (come attributi dell'oggetto di richiesta passato come parametro).

Spesso è utile raggruppare i gestori di rotte per una particolare parte di un sito insieme e accedervi usando un prefisso di rotta comune (ad esempio, un sito con un Wiki potrebbe avere tutte le rotte correlate al wiki in un file e accedervi con un prefisso di rotta _/wiki/_). In _Express_ questo è ottenuto utilizzando l'oggetto [`express.Router`](https://expressjs.com/en/guide/routing.html#express-router). Ad esempio, possiamo creare la nostra rotta wiki in un modulo chiamato **wiki.js**, e quindi esportare l'oggetto `Router`, come mostrato qui sotto:

```js
// wiki.js - Wiki route module

const express = require("express");

const router = express.Router();

// Home page route
router.get("/", function (req, res) {
  res.send("Wiki home page");
});

// About page route
router.get("/about", function (req, res) {
  res.send("About this wiki");
});

module.exports = router;
```

> [!NOTE]
> Aggiungere rotte all'oggetto `Router` è proprio come aggiungere rotte all'oggetto `app` (come mostrato in precedenza).

Per usare il router nel nostro file app principale dovremmo poi `require()` il modulo di rotta (**wiki.js**), quindi chiamare `use()` sull'applicazione _Express_ per aggiungere il Router al percorso di gestione del middleware. Le due rotte saranno quindi accessibili da `/wiki/` e `/wiki/about/`.

```js
const wiki = require("./wiki.js");

// …
app.use("/wiki", wiki);
```

Ti mostreremo molto di più sul lavoro con le rotte, e in particolare sull'uso del `Router`, più avanti nella sezione collegata [Routes and controllers](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes).

### Uso del middleware

Il middleware è usato ampiamente nelle app Express, per compiti che vanno dal servire file statici alla gestione degli errori, alla compressione delle risposte HTTP. Mentre le funzioni di rotta terminano il ciclo richiesta-risposta HTTP restituendo una risposta al client HTTP, le funzioni middleware _tipicamente_ eseguono un'operazione sulla richiesta o sulla risposta e quindi chiamano la prossima funzione nella "stack", che potrebbe essere un altro middleware o un gestore di rotta. L'ordine in cui viene chiamato il middleware è a discrezione dello sviluppatore dell'app.

> [!NOTE]
> Il middleware può eseguire qualsiasi operazione, eseguire qualsiasi codice, apportare modifiche alla richiesta e all'oggetto di risposta, e può _anche terminare il ciclo richiesta-risposta_. Se non termina il ciclo, deve chiamare `next()` per passare il controllo alla prossima funzione di middleware (o la richiesta rimarrà sospesa).

La maggior parte delle app userà _middleware di terze parti_ per semplificare i comuni compiti di sviluppo web, come lavorare con i cookie, le sessioni, l'autenticazione degli utenti, l'accesso ai `POST` delle richieste e ai dati JSON, il logging, ecc. Puoi trovare un [elenco di pacchetti middleware mantenuti dal team Express](https://expressjs.com/en/resources/middleware.html) (che include anche altri popolari pacchetti di terze parti). Altri pacchetti Express sono disponibili sul gestore di pacchetti npm.

Per usare il middleware di terze parti devi prima installarlo nella tua app usando npm.
Ad esempio, per installare il middleware logger di richieste HTTP [morgan](https://expressjs.com/en/resources/middleware/morgan.html), faresti così:

```bash
npm install morgan
```

Potresti quindi chiamare `use()` sull'_oggetto dell'applicazione Express_ per aggiungere il middleware alla stack:

```js
const express = require("express");
const logger = require("morgan");

const app = express();
app.use(logger("dev"));
// …
```

> [!NOTE]
> Middleware e funzioni di rotta vengono chiamate nell'ordine in cui vengono dichiarate. Per alcuni middleware l'ordine è importante (ad esempio se il middleware di sessione dipende dal middleware dei cookie, allora il gestore dei cookie deve essere aggiunto per primo). È quasi sempre il caso che il middleware venga chiamato prima di impostare le rotte, o i tuoi gestori di rotta non avranno accesso alla funzionalità aggiunta dal tuo middleware.

Puoi scrivere le tue funzioni middleware e probabilmente dovrai farlo (se non altro per creare il codice di gestione degli errori). L'**unica** differenza tra una funzione middleware e una callback del gestore di rotta è che le funzioni middleware hanno un terzo argomento `next`, che le funzioni middleware sono attese a chiamare se non sono quelle che completano il ciclo di richiesta (quando la funzione middleware viene chiamata, questo contiene la _funzione successiva_ che deve essere chiamata).

Puoi aggiungere una funzione middleware alla catena di elaborazione _per tutte le risposte_ con `app.use()`, o per uno specifico verbo HTTP usando il metodo associato: `app.get()`, `app.post()`, ecc. Le rotte sono specificate allo stesso modo per entrambi i casi, anche se la rotta è facoltativa quando si chiama `app.use()`.

L'esempio qui sotto mostra come puoi aggiungere la funzione middleware utilizzando entrambe le modalità e con/senza una rotta.

```js
const express = require("express");

const app = express();

// An example middleware function
const a_middleware_function = function (req, res, next) {
  // Perform some operations
  next(); // Call next() so Express will call the next middleware function in the chain.
};

// Function added with use() for all routes and verbs
app.use(a_middleware_function);

// Function added with use() for a specific route
app.use("/some-route", a_middleware_function);

// A middleware function added for a specific HTTP verb and route
app.get("/", a_middleware_function);

app.listen(3000);
```

> [!NOTE]
> Sopra abbiamo dichiarato la funzione middleware separatamente e poi impostata come callback. Nel nostro precedente gestore di rotta abbiamo dichiarato la funzione callback quando è stata utilizzata. In JavaScript, entrambi gli approcci sono validi.

La documentazione di Express ha un sacco di eccellente documentazione riguardo [l'uso](https://expressjs.com/en/guide/using-middleware.html) e [la scrittura](https://expressjs.com/en/guide/writing-middleware.html) del middleware Express.

### Servire file statici

Puoi utilizzare il middleware [express.static](https://expressjs.com/en/4x/api.html#express.static) per servire file statici, tra cui le tue immagini, CSS e JavaScript (`static()` è l'unica funzione middleware che è realmente **parte** di _Express_). Ad esempio, useresti la riga qui sotto per servire immagini, file CSS, e file JavaScript da una directory denominata '**public'** allo stesso livello da dove chiami node:

```js
app.use(express.static("public"));
```

Qualsiasi file nella directory pubblica viene servito aggiungendo il nome del file (_relativo_ alla base della directory "public") all'URL di base. Così ad esempio:

```plain
http://localhost:3000/images/dog.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/about.html
```

Puoi chiamare `static()` più volte per servire più directory. Se un file non può essere trovato da una funzione middleware, verrà passato alla successiva middleware (l'ordine in cui viene chiamato il middleware si basa sul tuo ordine di dichiarazione).

```js
app.use(express.static("public"));
app.use(express.static("media"));
```

Puoi anche creare un prefisso virtuale per i tuoi URL statici, invece di avere i file aggiunti all'URL di base. Ad esempio, qui specifichiamo un [percorso di montaggio](https://expressjs.com/en/4x/api.html#app.use) in modo che i file vengano caricati con il prefisso "/media":

```js
app.use("/media", express.static("public"));
```

Ora, puoi caricare i file che si trovano nella directory `public` dal prefisso di percorso `/media`.

```plain
http://localhost:3000/media/images/dog.jpg
http://localhost:3000/media/video/cat.mp4
http://localhost:3000/media/cry.mp3
```

> [!NOTE]
> Vedi anche [Servire file statici in Express](https://expressjs.com/en/starter/static-files.html).

### Gestione degli errori

Gli errori sono gestiti da una o più funzioni middleware speciali che hanno quattro argomenti, invece dei soliti tre: `(err, req, res, next)`. Ad esempio:

```js
app.use(function (err, req, res, next) {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

Queste possono restituire qualsiasi contenuto richiesto, ma devono essere chiamate dopo tutte le altre chiamate `app.use()` e di rotte, in modo che siano le ultime nel processo di gestione delle richieste!

Express viene fornito con un gestore degli errori integrato, che si occupa di eventuali errori rimanenti che potrebbero essere incontrati nell'app. Questa funzione middleware di gestione degli errori predefinita viene aggiunta alla fine della stack delle funzioni middleware. Se passi un errore a `next()` e non lo gestisci in un gestore degli errori, verrà gestito dal gestore degli errori integrato; l'errore verrà scritto al client con il tracciamento dello stack.

> [!NOTE]
> Il tracciamento dello stack non è incluso nell'ambiente di produzione. Per eseguirlo in modalità produzione devi impostare la variabile d'ambiente `NODE_ENV` su `"production"`.

> [!NOTE]
> HTTP404 e altri codici di stato "errore" non sono trattati come errori. Se vuoi gestirli, puoi aggiungere una funzione middleware per farlo. Per ulteriori informazioni vedi le [FAQ](https://expressjs.com/en/starter/faq.html#how-do-i-handle-404-responses).

Per ulteriori informazioni vedi [Gestione degli errori](https://expressjs.com/en/guide/error-handling.html) (Express docs).

### Uso delle basi di dati

Le app _Express_ possono usare qualsiasi meccanismo di database supportato da _Node_ (_Express_ stesso non definisce alcun comportamento/requisito specifico per la gestione del database). Ci sono molte opzioni, tra cui PostgreSQL, MySQL, Redis, SQLite, MongoDB, ecc.

Per usarli devi prima installare il driver del database usando npm. Ad esempio, per installare il driver per il popolare NoSQL MongoDB useresti il comando:

```bash
npm install mongodb
```

Il database stesso può essere installato localmente o su un server cloud. Nel tuo codice Express richiedi il driver, ti connetti al database e poi esegui operazioni di creazione, lettura, aggiornamento e cancellazione (CRUD). L'esempio qui sotto (dalla documentazione di Express) mostra come puoi trovare record "mammal" usando MongoDB.

Questo funziona con versioni più vecchie di MongoDB versione ~ 2.2.33:

```js
const { MongoClient } = require("mongodb");

MongoClient.connect("mongodb://localhost:27017/animals", (err, db) => {
  if (err) throw err;

  db.collection("mammals")
    .find()
    .toArray((err, result) => {
      if (err) throw err;

      console.log(result);
    });
});
```

Per MongoDB versione 3.0 e superiori:

```js
const { MongoClient } = require("mongodb");

MongoClient.connect("mongodb://localhost:27017/animals", (err, client) => {
  if (err) throw err;

  const db = client.db("animals");
  db.collection("mammals")
    .find()
    .toArray((err, result) => {
      if (err) throw err;
      console.log(result);
      client.close();
    });
});
```

Un altro approccio popolare è accedere al tuo database indirettamente, tramite un Mappatore Relazionale ad Oggetti ("ORM"). In questo approccio definisci i tuoi dati come "oggetti" o "modelli" e l'ORM li mappa attraverso al formato di database sottostante. Questo approccio ha il beneficio che come sviluppatore puoi continuare a pensare in termini di oggetti JavaScript piuttosto che in termini di semantica del database, e che c'è un luogo ovvio dove eseguire la convalida e il controllo dei dati in arrivo. Parleremo di più sui database in un articolo successivo.

Per ulteriori informazioni vedi [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (Express docs).

### Rendering dei dati (views)

I motori di template (detti anche "view engines" in _Express_) ti permettono di specificare la _struttura_ di un documento di output in un template, usando segnaposti per i dati che verranno riempiti quando viene generata una pagina. I template sono spesso usati per creare HTML, ma possono anche creare altri tipi di documenti.

Express ha supporto per un numero di motori di template, in particolare Pug (precedentemente "Jade"), Mustache e EJS. Ciascuno ha i propri punti forza per affrontare casi d'uso particolari (i confronti relativi possono essere facilmente trovati via una ricerca su Internet).
Il generatore di applicazioni Express usa Jade come default, ma supporta anche diversi altri.

Nel codice delle impostazioni dell'applicazione imposti il motore di template da usare e la posizione in cui Express deve cercare i template utilizzando le impostazioni 'views' e 'view engine', come mostrato qui sotto (dovrai anche installare il pacchetto contenente la tua libreria di template, inoltre!)

```js
const express = require("express");
const path = require("path");

const app = express();

// Set directory to contain the templates ('views')
app.set("views", path.join(__dirname, "views"));

// Set view engine to use, in this case 'some_template_engine_name'
app.set("view engine", "some_template_engine_name");
```

L'aspetto del template dipenderà dal motore che usi. Supponendo che hai un file di template chiamato "index.\<template_extension>" che contiene segnaposti per le variabili di dati chiamate 'title' e "message", dovresti chiamare [`Response.render()`](https://expressjs.com/en/4x/api.html#res.render) in una funzione del gestore delle rotte per creare e inviare la risposta HTML:

```js
app.get("/", function (req, res) {
  res.render("index", { title: "About dogs", message: "Dogs rock!" });
});
```

Per ulteriori informazioni vedi [Usare i motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (Express docs).

### Struttura dei file

Express non fa assunzioni in termini di struttura o di quali componenti utilizzi. Rotte, viste, file statici e altra logica specifica dell'applicazione possono vivere in un numero qualsiasi di file con qualsiasi struttura di directory. Mentre è perfettamente possibile avere tutta l'applicazione _Express_ in un file, tipicamente ha senso dividere la tua applicazione in file in base alla funzione (ad esempio, gestione degli account, blog, forum di discussione) e al dominio del problema architettonico (ad esempio, modello, vista o controller se usi un'architettura {{Glossary("MVC", "MVC")}}).

In un argomento successivo useremo il _Express Application Generator_, che crea uno scheletro di app modulare che possiamo facilmente estendere per creare applicazioni web.

## Riepilogo

Congratulazioni, hai completato il primo passo nel tuo viaggio Express/Node! Ora dovresti capire i principali vantaggi di Express e Node e avere una vaga idea di come potrebbero apparire le principali parti di un'app Express (rotte, middleware, gestione degli errori e codice dei template). Dovresti anche capire che, essendo Express un framework non opinionato, il modo in cui unisci queste parti e le librerie che usi dipendono principalmente da te!

Naturalmente, Express è deliberatamente un framework di applicazioni web molto leggero, quindi gran parte del suo beneficio e potenziale proviene da librerie di terze parti e funzionalità. Vedremo quelle in dettaglio negli articoli seguenti. Nel nostro prossimo articolo esamineremo come impostare un ambiente di sviluppo Node, in modo che tu possa iniziare a vedere un po' di codice Express in azione.

## Vedi anche

- [Learn Node.js](https://scrimba.com/learn-nodejs-c00ho9qqh6?via=mdn) da Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce una introduzione divertente e interattiva a Node.js.
- [Learn Express.js](https://scrimba.com/learn-expressjs-c062las154?via=mdn) da Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> si basa sul link precedente, mostrando come iniziare a utilizzare il framework Express per costruire siti web lato server.
- [Modules](https://nodejs.org/api/modules.html#modules_modules) (Node API docs)
- [Express](https://expressjs.com/) (pagina principale)
- [Basic routing](https://expressjs.com/en/starter/basic-routing.html) (Express docs)
- [Routing guide](https://expressjs.com/en/guide/routing.html) (Express docs)
- [Usare i motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (Express docs)
- [Usare il middleware](https://expressjs.com/en/guide/using-middleware.html) (Express docs)
- [Scrittura di middleware da usare nelle app Express](https://expressjs.com/en/guide/writing-middleware.html) (Express docs)
- [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (Express docs)
- [Servire file statici in Express](https://expressjs.com/en/starter/static-files.html) (Express docs)
- [Gestione degli errori](https://expressjs.com/en/guide/error-handling.html) (Express docs)

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
