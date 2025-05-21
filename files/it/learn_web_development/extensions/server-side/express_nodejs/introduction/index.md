---
title: Introduzione a Express/Node
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo primo articolo su Express rispondiamo alle domande "Che cos'è Node?" e "Che cos'è Express?", e forniamo una panoramica su ciò che rende speciale il framework web Express. Delineeremo le caratteristiche principali e mostreremo alcuni dei principali elementi costitutivi di un'applicazione Express (anche se a questo punto non avrete ancora un ambiente di sviluppo su cui testarlo).

> [!WARNING]
> Il tutorial di Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Abbiamo in programma di aggiornare la documentazione nella seconda metà del 2025.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Una comprensione generale della <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps">programmazione di siti web lato server</a>, e in particolare delle meccaniche delle <a href="/it/docs/Learn_web_development/Extensions/Server-side/First_steps/Client-Server_overview">interazioni client-server nei siti web</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità con cos'è Express e come si integra con Node, quali funzionalità offre e i principali elementi costitutivi di un'applicazione Express.
      </td>
    </tr>
  </tbody>
</table>

## Introduzione a Node

[Node](https://nodejs.org/) (o più formalmente _Node.js_) è un ambiente runtime open-source e multipiattaforma che consente agli sviluppatori di creare strumenti e applicazioni di server-side in {{Glossary("JavaScript", "JavaScript")}}. L'ambiente runtime è progettato per essere utilizzato al di fuori del contesto del browser (cioè, eseguito direttamente su un computer o un sistema operativo server). Pertanto, l'ambiente omette le API JavaScript specifiche del browser e aggiunge supporto per API OS più tradizionali, inclusi le librerie HTTP e del file system.

Da una prospettiva di sviluppo di server web Node offre una serie di vantaggi:

- Ottime prestazioni! Node è stato progettato per ottimizzare il throughput e la scalabilità nelle applicazioni web ed è una buona soluzione per molti problemi comuni di sviluppo web (ad esempio, applicazioni web in tempo reale).
- Il codice è scritto in "semplice vecchio JavaScript", il che significa che si passa meno tempo a gestire il "cambiamento di contesto" tra i linguaggi quando si scrive sia il codice client-side che server-side.
- JavaScript è un linguaggio di programmazione relativamente nuovo e beneficia di miglioramenti nel design linguistico rispetto ad altri linguaggi tradizionali per server web (ad esempio, Python, PHP, ecc.). Molti altri linguaggi nuovi e popolari si compilano/convertono in JavaScript, quindi è possibile anche usare TypeScript, CoffeeScript, ClojureScript, Scala, LiveScript, ecc.
- Il gestore dei pacchetti di Node (npm) fornisce accesso a centinaia di migliaia di pacchetti riutilizzabili. Ha anche una risoluzione delle dipendenze di prim'ordine e può essere usato per automatizzare la maggior parte della toolchain di compilazione.
- Node.js è portabile. È disponibile su Microsoft Windows, macOS, Linux, Solaris, FreeBSD, OpenBSD, WebOS e NonStop OS. Inoltre, è ben supportato da molti fornitori di hosting web, che spesso forniscono infrastrutture e documentazione specifiche per ospitare siti Node.
- Ha un ecosistema di terze parti e una comunità di sviluppatori molto attivi, con molte persone disposte ad aiutare.

È possibile utilizzare Node.js per creare un semplice server web utilizzando il pacchetto HTTP di Node.

### Hello Node.js

Il seguente esempio crea un server web che ascolta qualsiasi tipo di richiesta HTTP sull'URL `http://127.0.0.1:8000/` — quando viene ricevuta una richiesta, lo script risponderà con la stringa: "Hello World". Se hai già installato Node, puoi seguire questi passi per provare l'esempio:

1. Apri il Terminale (su Windows, apri l'utilità della riga di comando)
2. Crea la cartella dove vuoi salvare il programma, ad esempio, `test-node` e poi entra al suo interno digitando il seguente comando nel terminale:

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

Infine, naviga su `http://localhost:8000` nel tuo browser web; dovresti vedere il testo "**Hello World**" in alto a sinistra di una pagina web altrimenti vuota.

## Framework Web

Altri compiti comuni di sviluppo web non sono direttamente supportati da Node stesso. Se vuoi aggiungere una gestione specifica per diversi verbi HTTP (ad esempio, `GET`, `POST`, `DELETE`, ecc.), gestire separatamente le richieste in diversi percorsi URL ("routes"), servire file statici o usare modelli per creare dinamicamente la risposta, Node da solo non sarà di grande aiuto. Dovrai scrivere il codice da solo, o puoi evitare di reinventare la ruota e usare un framework web!

## Introduzione a Express

[Express](https://expressjs.com/) è il framework web Node.js più popolare, ed è la libreria sottostante per numerosi altri framework Node.js popolari. Fornisce meccanismi per:

- Scrivere gestori per richieste con diversi verbi HTTP in diversi percorsi URL (routes).
- Integrarsi con motori di rendering "view" per generare risposte inserendo dati nei modelli.
- Impostare le configurazioni comuni delle applicazioni web come la porta da usare per la connessione e la posizione dei modelli che vengono usati per il rendering della risposta.
- Aggiungere ulteriore "middleware" di elaborazione delle richieste in qualsiasi punto del percorso di gestione delle richieste.

Mentre _Express_ stesso è piuttosto minimalista, gli sviluppatori hanno creato pacchetti middleware compatibili per affrontare quasi tutti i problemi di sviluppo web. Ci sono librerie per lavorare con i cookies, le sessioni, i login degli utenti, i parametri URL, i dati `POST`, le intestazioni di sicurezza e molti altri. È possibile trovare un elenco di pacchetti middleware mantenuti dal team di Express su [Express Middleware](https://expressjs.com/en/resources/middleware.html) (insieme a un elenco di alcuni pacchetti di terze parti popolari).

> [!NOTE]
> Questa flessibilità è una lama a doppio taglio. Ci sono pacchetti middleware per affrontare quasi ogni problema o requisito, ma capire i pacchetti giusti da usare a volte può essere una sfida. Inoltre, non c'è un "modo giusto" per strutturare un'applicazione, e molti esempi che potresti trovare su Internet non sono ottimali o mostrano solo una piccola parte di ciò che devi fare per sviluppare un'applicazione web.

## Da dove vengono Node ed Express?

Node è stato inizialmente rilasciato, solo per Linux, nel 2009. Il gestore di pacchetti npm è stato rilasciato nel 2010 e il supporto nativo per Windows è stato aggiunto nel 2012. Approfondisci su [Wikipedia](https://en.wikipedia.org/wiki/Node.js#History) se vuoi saperne di più.

Express è stato inizialmente rilasciato nel novembre 2010 ed è attualmente alla quinta versione principale dell'API. Puoi dare un'occhiata al [changelog](https://expressjs.com/en/changelog/#5.x) per informazioni sulle modifiche nell'attuale versione, e su [GitHub](https://github.com/expressjs/express/blob/master/History.md) per note di rilascio storiche più dettagliate.

## Quanto sono popolari Node ed Express?

La popolarità di un framework web è importante perché è un indicatore del fatto che continuerà ad essere mantenuto, e quali risorse saranno disponibili in termini di documentazione, librerie aggiuntive e supporto tecnico.

Non esiste una misura disponibile e definitiva della popolarità dei framework server-side (sebbene sia possibile stimare la popolarità usando meccanismi come il conteggio dei progetti GitHub e delle domande su Stack Overflow per ciascuna piattaforma). Una domanda migliore è se Node ed Express siano "abbastanza popolari" per evitare i problemi delle piattaforme impopolari. Stanno continuando a evolversi? Puoi ottenere aiuto se ne hai bisogno? C'è una opportunità per ottenere un lavoro pagato se impari Express?

In base al numero di aziende di alto profilo che utilizzano Express, al numero di persone che contribuiscono al codice e al numero di persone che forniscono supporto gratuito e a pagamento, allora sì, _Express_ è un framework popolare!

## Express è opinabile?

I framework web spesso si definiscono come "opinionated" o "unopinionated".

I framework "opinionated" sono quelli che hanno opinioni sul "modo giusto" di gestire un determinato compito. Spesso supportano uno sviluppo rapido _in un dominio particolare_ (risolvendo problemi di un tipo particolare) perché il modo giusto di fare qualsiasi cosa è di solito ben compreso e ben documentato. Tuttavia, possono essere meno flessibili nel risolvere problemi al di fuori del loro dominio principale e tendono a offrire meno scelte per i componenti e gli approcci che si possono usare.

I framework "unopinionated", per contro, hanno molte meno restrizioni sul modo migliore di collegare i componenti per raggiungere un obiettivo, o persino quali componenti dovrebbero essere usati. Rendono più facile per gli sviluppatori utilizzare gli strumenti più adatti per completare un compito particolare, sia pur al costo di dover trovare quei componenti da soli.

Express è non opinionated. È possibile inserire quasi tutti i middleware compatibili preferiti nella catena di gestione delle richieste, quasi nell'ordine desiderato. Puoi strutturare l'app in un file o in più file e usando qualsiasi struttura di directory. A volte potresti avere la sensazione di avere troppe scelte!

## Come appare il codice Express?

In un sito web tradizionale basato sui dati, un'applicazione web aspetta richieste HTTP dal browser web (o altro client). Quando viene ricevuta una richiesta l'applicazione determina quale azione è necessaria in base allo schema URL e possibilmente alle informazioni associate contenute nei dati `POST` o `GET`. A seconda di ciò che è richiesto, potrebbe quindi leggere o scrivere informazioni da un database o eseguire altre operazioni necessarie per soddisfare la richiesta. L'applicazione restituirà quindi una risposta al browser, spesso creando dinamicamente una pagina HTML per il browser da visualizzare inserendo i dati recuperati nei segnaposto di un template HTML.

Express fornisce metodi per specificare quale funzione viene chiamata per un particolare verbo HTTP (`GET`, `POST`, `PUT`, ecc.) e schema URL ("Route"), e metodi per specificare quale motore di template ("view") viene utilizzato, dove si trovano i file dei template e quale modello usare per generare una risposta. È possibile utilizzare il middleware Express per aggiungere supporto per i cookie, le sessioni e gli utenti, ottenere parametri `POST`/`GET`, ecc. È possibile utilizzare qualsiasi meccanismo di database supportato da Node (Express non definisce alcun comportamento correlato al database).

Le sezioni seguenti spiegano alcune delle cose comuni che vedrai quando lavori con codice _Express_ e _Node_.

### Helloworld Express

Per prima cosa consideriamo lo standard [Hello World](https://expressjs.com/en/starter/hello-world.html) di Express (discutiamo ogni parte di questo qui sotto, e nelle sezioni seguenti).

> [!NOTE]
> Se hai già installato Node ed Express (o se li installi come mostrato nel [prossimo articolo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment)), puoi salvare questo codice in un file di testo chiamato **app.js** ed eseguirlo in un prompt dei comandi bash chiamando:
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

Le prime due righe `require()` (importano) il modulo express e creano un'[applicazione Express](https://expressjs.com/en/4x/api.html#app). Questo oggetto, che è tradizionalmente denominato `app`, ha metodi per il routing delle richieste HTTP, configurazione di middleware, rendering di viste HTML, registrazione di un motore di template e modifica delle [impostazioni dell'applicazione](https://expressjs.com/en/4x/api.html#app.settings.table) che controllano il comportamento dell'applicazione (ad esempio, il modo dell'ambiente, se le definizioni delle route sono sensibili alle maiuscole e minuscole, ecc.)

La parte centrale del codice (le tre righe che iniziano con `app.get`) mostra una _definizione di route_. Il metodo `app.get()` specifica una funzione di callback che verrà invocata ogni volta che c'è una richiesta HTTP `GET` con un percorso (`'/'`) relativo alla radice del sito. La funzione di callback prende un oggetto richiesta e risposta come argomenti, e chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "Hello World!"

Il blocco finale avvia il server su una porta specificata ('3000') e stampa un commento di log sulla console. Con il server in esecuzione, potresti andare su `localhost:3000` nel tuo browser per vedere la risposta di esempio restituita.

### Importare e creare moduli

Un modulo è una libreria/file JavaScript che puoi importare in altro codice usando la funzione `require()` di Node. _Express_ stesso è un modulo, così come lo sono i middleware e le librerie dei database che usiamo nelle nostre applicazioni _Express_.

Il codice sotto mostra come importare un modulo per nome, usando il framework _Express_ come esempio. Per prima cosa invochiamo la funzione `require()`, specificando il nome del modulo come stringa (`'express'`), e chiamando l'oggetto restituito per creare un'[applicazione Express](https://expressjs.com/en/4x/api.html#app). Possiamo quindi accedere alle proprietà e alle funzioni dell'oggetto applicazione.

```js
const express = require("express");
const app = express();
```

Puoi anche creare i tuoi moduli che possono essere importati nello stesso modo.

> [!NOTE]
> Vorrai _creare_ i tuoi moduli, perché ciò ti consente di organizzare il tuo codice in parti gestibili — un'applicazione monolitica in un solo file è difficile da comprendere e mantenere. L'uso dei moduli ti aiuta anche a gestire il tuo namespace, poiché solo le variabili che esporti esplicitamente vengono importate quando utilizzi un modulo.

Per rendere disponibili gli oggetti all'esterno di un modulo devi solo esporli come proprietà aggiuntive sull'oggetto `exports`. Ad esempio, il modulo **square.js** qui sotto è un file che esporta i metodi `area()` e `perimeter()`:

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

Se vuoi esportare un oggetto completo in un'unica assegnazione anziché costruirlo una proprietà alla volta, assegnalo a `module.exports` come mostrato di seguito (puoi anche fare questo per rendere la radice dell'oggetto exports un costruttore o un'altra funzione):

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
> Puoi pensare a `exports` come un [scorciatoia](https://nodejs.org/api/modules.html#modules_exports_shortcut) per `module.exports` all'interno di un dato modulo. Infatti, `exports` è solo una variabile che viene inizializzata al valore di `module.exports` prima che il modulo venga valutato. Tale valore è un riferimento a un oggetto (oggetto vuoto in questo caso). Ciò significa che `exports` detiene un riferimento allo stesso oggetto a cui fa riferimento `module.exports`. Significa anche che assegnando un altro valore a `exports` non è più legato a `module.exports`.

Per molte altre informazioni sui moduli vedere [Modules](https://nodejs.org/api/modules.html#modules_modules) (documentazione API di Node).

### Usare API asincrone

Il codice JavaScript usa frequentemente API asincrone anziché sincrone per le operazioni che potrebbero richiedere un certo tempo per essere completate. Un'API sincrona è quella in cui ciascuna operazione deve essere completata prima che la successiva possa iniziare. Ad esempio, le seguenti funzioni di log sono sincrone e stamperanno il testo sulla console in ordine (First, Second).

```js
console.log("First");
console.log("Second");
```

Al contrario, un'API asincrona è quella in cui l'API avvia un'operazione e ritorna immediatamente (prima che l'operazione sia completa). Una volta che l'operazione termina, l'API userà qualche meccanismo per eseguire operazioni aggiuntive. Ad esempio, il codice qui sotto stamperà "Second, First" perché anche se il metodo `setTimeout()` viene chiamato per primo, e ritorna immediatamente, l'operazione non termina per alcuni secondi.

```js
setTimeout(function () {
  console.log("First");
}, 3000);
console.log("Second");
```

L'uso di API asincrone non bloccanti è ancora più importante su Node che nel browser perché _Node_ è un ambiente di esecuzione a eventi a thread singolo. "Single threaded" significa che tutte le richieste al server vengono eseguite sullo stesso thread (anziché essere spawnate in processi separati). Questo modello è estremamente efficiente in termini di velocità e risorse del server, ma significa che se una delle tue funzioni chiama metodi sincroni che richiedono molto tempo per essere completati, bloccheranno non solo la richiesta corrente, ma tutte le altre richieste gestite dalla tua applicazione web.

Ci sono diversi modi in cui un'API asincrona può notificare alla tua applicazione che è completata. Il modo più comune è registrare una funzione di callback quando si invoca l'API asincrona, che verrà chiamata quando l'operazione si completa. Questo è l'approccio utilizzato sopra.

> [!NOTE]
> Usare i callback può essere abbastanza "disordinato" se hai una sequenza di operazioni asincrone dipendenti che devono essere eseguite in ordine, perché ciò comporta più livelli di callback annidati. Questo problema è comunemente noto come "callback hell". Questo problema può essere ridotto con buone pratiche di codifica (vedi <http://callbackhell.com/>), usando un modulo come [async](https://www.npmjs.com/package/async), o rifattorizzando il codice con funzioni native JavaScript come [Promises](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise) e [async/await](/it/docs/Web/JavaScript/Reference/Statements/async_function). Node offre la funzione [`utils.promisify`](https://nodejs.org/api/util.html#utilpromisifyoriginal) per convertire il callback in Promise in modo ergonomico.

> [!NOTE]
> Una convenzione comune per Node ed Express è usare i callback error-first. In questa convenzione, il primo valore nelle _funzioni di callback_ è un valore di errore, mentre i successivi argomenti contengono i dati di successo. C'è una buona spiegazione del perché questo approccio sia utile in questo blog: [The Node.js Way - Understanding Error-First Callbacks](https://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/) (fredkschott.com).

### Creare gestori di route

Nel nostro esempio _Hello World_ Express (vedi sopra), abbiamo definito una funzione di gestore di route (callback) per le richieste HTTP `GET` alla radice del sito (`'/'`).

```js
app.get("/", function (req, res) {
  res.send("Hello World!");
});
```

La funzione di callback prende un oggetto richiesta e uno di risposta come argomenti. In questo caso, il metodo chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "Hello World!" Ci sono [diversi altri metodi di risposta](https://expressjs.com/en/guide/routing.html#response-methods) per terminare il ciclo di richiesta/risposta; ad esempio, potresti chiamare [`res.json()`](https://expressjs.com/en/4x/api.html#res.json) per inviare una risposta JSON o [`res.sendFile()`](https://expressjs.com/en/4x/api.html#res.sendFile) per inviare un file.

> [!NOTE]
> Puoi usare qualsiasi nome di argomento ti piaccia nelle funzioni di callback; quando il callback viene invocato il primo argomento sarà sempre la richiesta e il secondo sarà sempre la risposta. Ha senso nominarli in modo tale da poter identificare l'oggetto con cui stai lavorando nel corpo del callback.

L'oggetto _Express application_ fornisce anche metodi per definire i gestori di route per tutti gli altri verbi HTTP, che vengono usati per lo più nello stesso modo:

`checkout()`, `copy()`, **`delete()`**, **`get()`**, `head()`, `lock()`, `merge()`, `mkactivity()`, `mkcol()`, `move()`, `m-search()`, `notify()`, `options()`, `patch()`, **`post()`**, `purge()`, **`put()`**, `report()`, `search()`, `subscribe()`, `trace()`, `unlock()`, `unsubscribe()`.

Esiste un metodo di routing speciale, `app.all()`, che verrà chiamato in risposta a qualsiasi metodo HTTP. Questo è usato per caricare funzioni middleware in un percorso particolare per tutti i metodi di richiesta. L'esempio seguente (dalla documentazione di Express) mostra un gestore che verrà eseguito per le richieste a `/secret` indipendentemente dal verbo HTTP utilizzato (a condizione che sia supportato dal [modulo http](https://nodejs.org/docs/latest/api/http.html#httpmethods)).

```js
app.all("/secret", function (req, res, next) {
  console.log("Accessing the secret section…");
  next(); // pass control to the next handler
});
```

Le route consentono di abbinare particolari schemi di caratteri in un URL, estrarre alcuni valori dall'URL e passarli come parametri al gestore della route (come attributi dell'oggetto richiesta passato come parametro).

Spesso è utile raggruppare i gestori di rotta per una parte particolare di un sito insieme e accedervi usando un prefisso di route comune (ad esempio, un sito con un Wiki potrebbe avere tutte le route relative al wiki in un file e farle accedere con un prefisso di route di _/wiki/_). In _Express_ questo è realizzato usando l'oggetto [`express.Router`](https://expressjs.com/en/guide/routing.html#express-router). Ad esempio, possiamo creare la nostra route wiki in un modulo denominato **wiki.js**, e poi esportare l'oggetto `Router`, come mostrato qui sotto:

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
> Aggiungere route all'oggetto `Router` è proprio come aggiungere route all'oggetto `app` (come mostrato in precedenza).

Per usare il router nel nostro file principale dell'app dovremmo quindi `require()` il modulo route (**wiki.js**), quindi chiamare `use()` sull'applicazione Express per aggiungere il Router al percorso di gestione del middleware. Le due route saranno quindi accessibili da `/wiki/` e `/wiki/about/`.

```js
const wiki = require("./wiki.js");
// …
app.use("/wiki", wiki);
```

Mostreremo molto di più sul lavoro con le route, e in particolare sull'uso di `Router`, successivamente nella sezione collegata [Routes and controllers](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes).

### Usare il middleware

Il middleware è usato estensivamente nelle app Express, per compiti che vanno dal servire file statici alla gestione degli errori, alla compressione delle risposte HTTP. Mentre le funzioni di route terminano il ciclo di richiesta-risposta HTTP restituendo una risposta al client HTTP, le funzioni middleware _tipicamente_ eseguono qualche operazione sulla richiesta o sulla risposta e poi chiamano la prossima funzione nello "stack", che potrebbe essere un altro middleware o un gestore di route. L'ordine in cui il middleware viene chiamato dipende dallo sviluppatore dell'app.

> [!NOTE]
> Il middleware può eseguire qualsiasi operazione, eseguire qualsiasi codice, fare modifiche alla richiesta e alla risposta, e può _anche terminare il ciclo di richiesta-risposta_. Se non termina il ciclo, deve chiamare `next()` per passare il controllo alla prossima funzione middleware (o la richiesta rimarrà sospesa).

La maggior parte delle app utilizzerà middleware _di terze parti_ per semplificare compiti comuni di sviluppo web come lavorare con cookie, sessioni, autenticazione utenti, accesso ai dati di richiesta `POST` e JSON, logging, ecc. Puoi trovare un [elenco di pacchetti middleware mantenuti dal team di Express](https://expressjs.com/en/resources/middleware.html) (che include anche altri pacchetti di terze parti popolari). Altri pacchetti Express sono disponibili sul gestore di pacchetti npm.

Per usare il middleware di terze parti, devi prima installarlo nella tua app usando npm. Ad esempio, per installare il middleware logger di richieste HTTP [morgan](https://expressjs.com/en/resources/middleware/morgan.html), faresti così:

```bash
npm install morgan
```

Potresti quindi chiamare `use()` sull'oggetto _Express application_ per aggiungere il middleware allo stack:

```js
const express = require("express");
const logger = require("morgan");
const app = express();
app.use(logger("dev"));
// …
```

> [!NOTE]
> Il middleware e le funzioni di routing vengono chiamati nell'ordine in cui vengono dichiarati. Per alcuni middleware l'ordine è importante (ad esempio, se il middleware delle sessioni dipende dal middleware dei cookie, il gestore dei cookie deve essere aggiunto per primo). È quasi sempre il caso che il middleware venga chiamato prima di impostare le route, o i gestori delle route non avranno accesso alla funzionalità aggiunta dal middleware.

Puoi scrivere le tue funzioni middleware e probabilmente dovrai farlo (anche solo per creare codice di gestione degli errori). L'unica differenza tra una funzione middleware e un callback di gestore di route è che le funzioni middleware hanno un terzo argomento `next`, che si suppone le funzioni middleware debbano chiamare se non sono quelle che completano il ciclo di richiesta (quando la funzione middleware è chiamata, questo contiene la funzione _next_ che deve essere chiamata).

Puoi aggiungere una funzione middleware alla catena di elaborazione per _tutte le risposte_ con `app.use()`, o per un verbo HTTP specifico usando il metodo associato: `app.get()`, `app.post()`, ecc. Le route sono specificate allo stesso modo in entrambi i casi, anche se la route è facoltativa quando si chiama `app.use()`.

L'esempio sotto mostra come puoi aggiungere la funzione middleware usando entrambi gli approcci, e con/senza una route.

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
> Sopra dichiariamo la funzione middleware separatamente e poi la impostiamo come callback. Nel nostro precedente esempio di funzione di gestore di route abbiamo dichiarato la funzione callback quando è stata usata. In JavaScript, entrambi gli approcci sono validi.

La documentazione di Express ha molta altra eccellente documentazione su [uso](https://expressjs.com/en/guide/using-middleware.html) e [scrittura](https://expressjs.com/en/guide/writing-middleware.html) del middleware Express.

### Servire file statici

Puoi usare il middleware [express.static](https://expressjs.com/en/4x/api.html#express.static) per servire file statici, includendo le tue immagini, CSS e JavaScript (`static()` è l'unica funzione middleware che è effettivamente **parte** di _Express_). Ad esempio, useresti la linea sotto per servire immagini, file CSS e JavaScript da una directory nominata '**public'** allo stesso livello del punto in cui chiami node:

```js
app.use(express.static("public"));
```

Qualsiasi file nella directory public viene servito aggiungendo il suo nome file (_relativo_ alla base della directory "public") all'URL di base. Quindi ad esempio:

```plain
http://localhost:3000/images/dog.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/about.html
```

Puoi chiamare `static()` più volte per servire più directory. Se un file non può essere trovato da una funzione middleware, verrà passato alla successiva middleware (l'ordine in cui le middleware vengono chiamate si basa sull'ordine di dichiarazione).

```js
app.use(express.static("public"));
app.use(express.static("media"));
```

Puoi anche creare un prefisso virtuale per i tuoi URL statici, invece di avere i file aggiunti all'URL di base. Ad esempio, qui [specifichiamo un path di montaggio](https://expressjs.com/en/4x/api.html#app.use) in modo che i file siano caricati con il prefisso "/media":

```js
app.use("/media", express.static("public"));
```

Ora, puoi caricare i file che si trovano nella directory `public` dal prefisso del percorso `/media`.

```plain
http://localhost:3000/media/images/dog.jpg
http://localhost:3000/media/video/cat.mp4
http://localhost:3000/media/cry.mp3
```

> [!NOTE]
> Vedere anche [Servire file statici in Express](https://expressjs.com/en/starter/static-files.html).

### Gestione degli errori

Gli errori sono gestiti da una o più funzioni middleware speciali che hanno quattro argomenti, anziché i soliti tre: `(err, req, res, next)`. Ad esempio:

```js
app.use(function (err, req, res, next) {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

Questi possono restituire qualsiasi contenuto richiesto, ma devono essere chiamati dopo tutte le altre chiamate `app.use()` e dei percorsi in modo che siano l'ultimo middleware nel processo di gestione delle richieste!

Express viene fornito con un gestore degli errori integrato, che si occupa di eventuali errori residui che potrebbero essere incontrati nell'app. Questa funzione middleware di gestione degli errori di default è aggiunta alla fine dello stack delle funzioni middleware. Se passi un errore a `next()` e non lo gestisci in un gestore degli errori, sarà gestito dal gestore di errori integrato; l'errore sarà scritto al client con il stack trace.

> [!NOTE]
> Lo stack trace non è incluso nell'ambiente di produzione. Per eseguirlo in modalità produzione devi impostare la variabile di ambiente `NODE_ENV` a `"production"`.

> [!NOTE]
> HTTP404 e altri codici di stato "errore" non sono trattati come errori. Se vuoi gestirli, puoi aggiungere una funzione middleware per farlo. Per ulteriori informazioni vedere le [FAQ](https://expressjs.com/en/starter/faq.html#how-do-i-handle-404-responses).

Per ulteriori informazioni vedere [Gestione degli errori](https://expressjs.com/en/guide/error-handling.html) (documentazione di Express).

### Usare i database

Le app _Express_ possono utilizzare qualsiasi meccanismo di database supportato da _Node_ (Express stesso non definisce alcun comportamento/requisito specifico aggiuntivo per la gestione dei database). Ci sono molte opzioni, tra cui PostgreSQL, MySQL, Redis, SQLite, MongoDB, ecc.

Per usarli devi prima installare il driver del database usando npm. Ad esempio, per installare il driver per il popolare NoSQL MongoDB useresti il comando:

```bash
npm install mongodb
```

Il database stesso può essere installato localmente o su un server cloud. Nel tuo codice Express richiedi il driver, ti connetti al database, e poi esegui operazioni di creazione, lettura, aggiornamento e cancellazione (CRUD). L'esempio sotto (dalla documentazione di Express) mostra come puoi trovare i record "mammiferi" usando MongoDB.

Questo funziona con versioni più vecchie di MongoDB ~ 2.2.33:

```js
const MongoClient = require("mongodb").MongoClient;

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
const MongoClient = require("mongodb").MongoClient;

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

Un altro approccio popolare è accedere al tuo database indirettamente, tramite un Object Relational Mapper ("ORM"). In questo approccio definisci i tuoi dati come "oggetti" o "modelli" e l'ORM mappa questi all'interno del formato del database sottostante. Questo approccio ha il vantaggio che come sviluppatore puoi continuare a pensare in termini di oggetti JavaScript piuttosto che di semantica di database, e che c'è un posto ovvio per eseguire la validazione e il controllo dei dati in entrata. Parleremo più di database in un articolo successivo.

Per ulteriori informazioni vedere [Integrazione dei database](https://expressjs.com/en/guide/database-integration.html) (documentazione di Express).

### Rendering dei dati (views)

I motori di template (riferiti anche come "view engines" in _Express_) ti permettono di specificare la _struttura_ di un documento di output in un template, usando segnaposto per i dati che verranno compilati quando una pagina viene generata. I template sono spesso usati per creare HTML, ma possono anche creare altri tipi di documenti.

Express supporta un numero di motori di template, in particolare Pug (precedentemente "Jade"), Mustache, ed EJS. Ognuno ha i suoi punti di forza per affrontare casi d'uso particolari (confronti relativi possono essere facilmente trovati tramite una ricerca su Internet). Il generatore di applicazioni Express usa Jade come suo predefinito, ma supporta anche diversi altri.

Nel codice delle impostazioni della tua applicazione imposti il motore di template da usare e il luogo in cui Express dovrebbe cercare i template utilizzando le impostazioni 'views' e 'view engine', come mostrato sotto (dovrai anche installare il pacchetto contenente la tua libreria di template anche!)

```js
const express = require("express");
const path = require("path");
const app = express();

// Set directory to contain the templates ('views')
app.set("views", path.join(__dirname, "views"));

// Set view engine to use, in this case 'some_template_engine_name'
app.set("view engine", "some_template_engine_name");
```

L'aspetto del template dipenderà dal motore che usi. Supponendo che tu abbia un file di template chiamato "index.\<template_extension>" che contiene segnaposto per variabili di dati chiamate 'title' e "message", chiamerai [`Response.render()`](https://expressjs.com/en/4x/api.html#res.render) in una funzione di gestore di route per creare e inviare la risposta HTML:

```js
app.get("/", function (req, res) {
  res.render("index", { title: "About dogs", message: "Dogs rock!" });
});
```

Per ulteriori informazioni vedere [Uso dei motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione di Express).

### Struttura dei file

Express non fa ipotesi in termini di struttura o quali componenti si usano. Le route, le viste, i file statici, e altra logica specifica dell'applicazione possono vivere in un qualsiasi numero di file con qualsiasi struttura di directory. Anche se è perfettamente possibile avere l'intera applicazione _Express_ in un unico file, di solito ha senso dividere l'applicazione in file basati su funzione (ad es., gestione degli account, blog, forum di discussione) e dominio del problema architetturale (ad es., modello, vista o controller se hai intenzione di usare un'{{Glossary("MVC", "architettura MVC")}}).

In un argomento successivo useremo il _Generatore di Applicazioni Express_, che crea uno scheletro app modulare che possiamo facilmente estendere per creare applicazioni web.

## Riepilogo

Congratulazioni, hai completato il primo passo nel tuo viaggio con Express/Node! Dovresti ora comprendere i principali benefici di Express e Node, e approssimativamente come potrebbero apparire le principali parti di un'app Express (routes, middleware, gestione degli errori e codice di template). Dovresti anche capire che con Express che è un framework non opinionated, il modo in cui metti insieme queste parti e le librerie che usi dipendono in gran parte da te!

Ovviamente Express è intenzionalmente un framework leggerissimo per applicazioni web, quindi gran parte dei suoi benefici e potenziale viene dalle librerie e dalle funzionalità di terze parti. Esamineremo questi in modo più dettagliato negli articoli successivi. Nel nostro prossimo articolo andremo a vedere come impostare un ambiente di sviluppo Node, così che tu possa cominciare a vedere il codice di Express in azione.

## Vedere anche

- [Modules](https://nodejs.org/api/modules.html#modules_modules) (documentazione API di Node)
- [Express](https://expressjs.com/) (pagina principale)
- [Basic routing](https://expressjs.com/en/starter/basic-routing.html) (documentazione di Express)
- [Routing guide](https://expressjs.com/en/guide/routing.html) (documentazione di Express)
- [Uso dei motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione di Express)
- [Uso del middleware](https://expressjs.com/en/guide/using-middleware.html) (documentazione di Express)
- [Scrivere middleware per l'uso nelle app Express](https://expressjs.com/en/guide/writing-middleware.html) (documentazione di Express)
- [Integrazione dei database](https://expressjs.com/en/guide/database-integration.html) (documentazione di Express)
- [Servire file statici in Express](https://expressjs.com/en/starter/static-files.html) (documentazione di Express)
- [Gestione degli errori](https://expressjs.com/en/guide/error-handling.html) (documentazione di Express)

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
