---
title: Introduzione a Express/Node
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

In questo primo articolo su Express rispondiamo alle domande "Cos'è Node?" e "Cos'è Express?", e le forniamo una panoramica di ciò che rende speciale il framework web Express. Illustreremo le caratteristiche principali e le mostreremo alcuni dei principali componenti di un'applicazione Express (anche se a questo punto non avrà ancora un ambiente di sviluppo in cui testarlo).

> [!WARNING]
> Il tutorial di Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Prevediamo di aggiornare la documentazione nella seconda metà del 2025.

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
        Famigliarizzare con cos'è Express e come si integra con Node, quali funzionalità fornisce e quali sono i principali componenti di un'applicazione Express.
      </td>
    </tr>
  </tbody>
</table>

## Introduzione a Node

[Node](https://nodejs.org/) (o più formalmente _Node.js_) è un ambiente di runtime open-source e multipiattaforma che permette agli sviluppatori di creare tutti i tipi di strumenti e applicazioni lato server in {{Glossary("JavaScript", "JavaScript")}}.
L'ambiente di runtime è progettato per l'uso al di fuori del contesto del browser (ossia, eseguendo direttamente su un computer o sistema operativo server). Pertanto, l'ambiente omette le API JavaScript specifiche per il browser e aggiunge supporto per API più tradizionali del sistema operativo, tra cui librerie HTTP e file system.

Dal punto di vista dello sviluppo di server web Node presenta diversi vantaggi:

- Ottime prestazioni! Node è stato progettato per ottimizzare throughput e scalabilità nelle applicazioni web ed è una buona soluzione per molti problemi comuni nello sviluppo web (es. applicazioni web in tempo reale).
- Il codice è scritto in "tradizionale JavaScript", il che significa che si spende meno tempo ad affrontare il "cambio di contesto" tra linguaggi quando si scrive sia codice lato client che lato server.
- JavaScript è un linguaggio di programmazione relativamente nuovo e beneficia di miglioramenti nel design del linguaggio rispetto ad altri linguaggi tradizionali per server web (es. Python, PHP, ecc.) Molti altri linguaggi nuovi e popolari vengono compilati/convertiti in JavaScript, quindi può anche usare TypeScript, CoffeeScript, ClojureScript, Scala, LiveScript, ecc.
- Il gestore di pacchetti node (npm) fornisce accesso a centinaia di migliaia di pacchetti riutilizzabili. Ha anche la migliore risoluzione delle dipendenze della categoria e può essere utilizzato per automatizzare la maggior parte della toolchain di build.
- Node.js è portatile. È disponibile su Microsoft Windows, macOS, Linux, Solaris, FreeBSD, OpenBSD, WebOS, e NonStop OS. Inoltre, è ben supportato da molti fornitori di hosting web, che spesso forniscono infrastrutture specifiche e documentazione per l'hosting di siti Node.
- Ha un ecosistema di terze parti e una comunità di sviluppatori molto attivi, con molte persone disposte ad aiutare.

Può utilizzare Node.js per creare un semplice server web utilizzando il pacchetto Node HTTP.

### Ciao Node.js

Il seguente esempio crea un server web che ascolta qualsiasi tipo di richiesta HTTP sull'URL `http://127.0.0.1:8000/` — quando viene ricevuta una richiesta, lo script risponderà con la stringa: "Hello World". Se ha già installato Node, può seguire questi passi per provare l'esempio:

1. Apri Terminale (su Windows, apri l'utility della riga di comando)
2. Crea la cartella dove desidera salvare il programma, ad esempio, `test-node` e poi acceda ad essa inserendo il seguente comando nel tuo terminale:

   ```bash
   cd test-node
   ```

3. Usando il tuo editor di testo preferito, crei un file chiamato `hello.js` e vi incolli il seguente codice:

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

4. Salvi il file nella cartella creata sopra.
5. Torni al terminale e digiti il seguente comando:

   ```bash
   node hello.js
   ```

Infine, navighi su `http://localhost:8000` nel suo browser web; dovrebbe vedere il testo "**Hello World**" in alto a sinistra di una pagina web altrimenti vuota.

## Framework Web

Altri compiti comuni nello sviluppo web non sono supportati direttamente da Node stesso. Se desidera aggiungere una gestione specifica per diversi verbi HTTP (es. `GET`, `POST`, `DELETE`, ecc.), gestire separatamente le richieste in diversi percorsi URL ("routes"), servire file statici, o utilizzare template per creare dinamicamente la risposta, Node non sarà di molta utilità da solo. Dovrà scrivere il codice lei stesso, o può evitare di reinventare la ruota e usare un framework web!

## Introduzione a Express

[Express](https://expressjs.com/) è il framework web Node.js più popolare, ed è la libreria sottostante per diversi altri framework Node.js popolari. Fornisce meccanismi per:

- Scrivere gestori per le richieste con diversi verbi HTTP in diversi percorsi URL (routes).
- Integrare con i motori di rendering "view" per generare risposte inserendo dati nei template.
- Impostare le impostazioni comuni delle applicazioni web come la porta da utilizzare per la connessione e la posizione dei template utilizzati per il rendering della risposta.
- Aggiungere "middleware" di elaborazione richieste aggiuntive in qualsiasi punto all'interno della pipeline di gestione delle richieste.

Mentre _Express_ stesso è piuttosto minimalista, gli sviluppatori hanno creato pacchetti middleware compatibili per risolvere quasi qualsiasi problema di sviluppo web. Esistono librerie per lavorare con cookie, sessioni, login degli utenti, parametri URL, dati `POST`, intestazioni di sicurezza, e _molti_ altri. Può trovare un elenco di pacchetti middleware mantenuti dal team Express su [Express Middleware](https://expressjs.com/en/resources/middleware.html) (insieme a un elenco di alcuni pacchetti popolari di terze parti).

> [!NOTE]
> Questa flessibilità è una lama a doppio taglio. Ci sono pacchetti middleware per risolvere quasi ogni problema o requisito, ma capire i pacchetti giusti da usare può essere talvolta una sfida. Inoltre, non esiste un "modo giusto" per strutturare un'applicazione, e molti esempi che potrebbe trovare su Internet non sono ottimali, o mostrano solo una piccola parte di ciò che è necessario fare per sviluppare un'applicazione web.

## Da dove vengono Node e Express?

Node è stato inizialmente rilasciato, solo per Linux, nel 2009. Il gestore di pacchetti npm è stato rilasciato nel 2010, e il supporto nativo per Windows è stato aggiunto nel 2012. Si addentri in [Wikipedia](https://en.wikipedia.org/wiki/Node.js#History) se desidera sapere di più.

Express è stato rilasciato inizialmente nel novembre 2010 ed è attualmente alla quinta versione principale dell'API. Può consultare il [changelog](https://expressjs.com/en/changelog/#5.x) per informazioni sui cambiamenti nell'attuale versione, e [GitHub](https://github.com/expressjs/express/blob/master/History.md) per note di rilascio storiche più dettagliate.

## Quanto sono popolari Node ed Express?

La popolarità di un framework web è importante perché è un indicatore di se continuerà a essere mantenuto e quali risorse saranno probabilmente disponibili in termini di documentazione, librerie aggiuntive e supporto tecnico.

Non esiste una misura pronta e definitiva della popolarità dei framework lato server (anche se può stimare la popolarità utilizzando meccanismi come il conteggio dei progetti GitHub e delle domande su Stack Overflow per ciascuna piattaforma). Una domanda migliore è se Node ed Express sono abbastanza popolari da evitare i problemi delle piattaforme impopolari. Stanno continuando ad evolversi? Può ottenere aiuto se ne ha bisogno? C'è un'opportunità per lei di ottenere lavoro retribuito se apprende Express?

Basandosi sul numero di aziende di alto profilo che utilizzano Express, sul numero di persone che contribuiscono al codice e sul numero di persone che forniscono supporto sia gratuito che a pagamento, allora sì, _Express_ è un framework popolare!

## Express è opinabile?

I framework web spesso si riferiscono a se stessi come "opinabili" o "non opinabili".

I framework opinabili sono quelli con opinioni sul "modo giusto" di gestire qualsiasi compito specifico. Spesso supportano lo sviluppo rapido _in un particolare dominio_ (risolvendo problemi di un tipo particolare) perché il modo giusto di fare qualsiasi cosa è solitamente ben compreso e ben documentato. Tuttavia possono essere meno flessibili nel risolvere problemi al di fuori del loro dominio principale, e tendono a offrire meno scelte per i componenti e gli approcci che possono usare.

I framework non opinabili, per contro, hanno molto meno restrizioni sul miglior modo per collegare i componenti insieme per raggiungere un obiettivo, e persino su quali componenti dovrebbero essere utilizzati. Rendono più facile per gli sviluppatori usare gli strumenti più adatti per completare un compito particolare, anche se a scapito del fatto che deve trovare lei stesso quei componenti.

Express è non opinabile. Può inserire quasi qualsiasi middleware compatibile che le piace nella catena di gestione delle richieste, in quasi qualsiasi ordine le piaccia. Può strutturare l'app in un file o in più file, e utilizzando qualsiasi struttura di directory. Potrebbe a volte sentirsi che ha troppe scelte!

## Che aspetto ha il codice Express?

In un sito web basato sui dati tradizionale, un'applicazione web attende richieste HTTP dal browser web (o altro client). Quando viene ricevuta una richiesta l'applicazione determina qual è l'azione necessaria basandosi sul pattern URL e possibilmente informazioni associate contenute nei dati `POST` o `GET`. A seconda di ciò che è richiesto potrebbe quindi leggere o scrivere informazioni da un database o eseguire altre attività necessarie per soddisfare la richiesta. L'applicazione restituirà quindi una risposta al browser web, spesso creando dinamicamente una pagina HTML per il browser da visualizzare inserendo i dati recuperati nei segnaposto in un modello HTML.

Express fornisce metodi per specificare quale funzione viene chiamata per un particolare verbo HTTP (`GET`, `POST`, `PUT`, ecc.) e pattern URL ("Route"), e metodi per specificare quale motore di template ("view") viene usato, dove si trovano i file dei template, e quale template utilizzare per rendere una risposta. Può utilizzare il middleware Express per aggiungere supporto per cookie, sessioni, e utenti, ottenendo parametri `POST`/`GET`, ecc. Può utilizzare qualsiasi meccanismo di database supportato da Node (Express non definisce alcun comportamento relativo ai database).

Le sezioni seguenti spiegano alcune delle cose comuni che vedrà quando lavora con il codice _Express_ e _Node_.

### Helloworld Express

Per prima cosa consideriamo il classico esempio [Hello World](https://expressjs.com/en/starter/hello-world.html) di Express (discutiamo ogni parte di questo qui sotto, e nelle sezioni seguenti).

> [!NOTE]
> Se ha già installato Node e Express (o se lo installa come mostrato nel [prossimo articolo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment)), può salvare questo codice in un file di testo chiamato **app.js** ed eseguirlo in un prompt dei comandi bash chiamando:
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

Le prime due righe `require()` (importano) il modulo express e creano un'[applicazione Express](https://expressjs.com/en/4x/api.html#app). Questo oggetto, che è tradizionalmente chiamato `app`, ha metodi per instradare richieste HTTP, configurare middleware, rendere viste HTML, registrare un motore di template e modificare le [impostazioni dell'applicazione](https://expressjs.com/en/4x/api.html#app.settings.table) che controllano il comportamento dell'applicazione (es. la modalità ambiente, se le definizioni delle route sono case sensitive, ecc.)

La parte centrale del codice (le tre righe che iniziano con `app.get`) mostra una _definizione di route_. Il metodo `app.get()` specifica una funzione di callback che verrà invocata ogni volta che c'è una richiesta HTTP `GET` con un percorso (`'/'`) relativo alla radice del sito. La funzione di callback prende un oggetto richiesta e un oggetto risposta come argomenti, e chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "Hello World!"

L'ultimo blocco avvia il server su una porta specificata ('3000') e stampa un commento di log sulla console. Con il server in esecuzione, potrebbe andare su `localhost:3000` nel suo browser per vedere la risposta di esempio restituita.

### Importazione e creazione di moduli

Un modulo è una libreria/file JavaScript che può importare in altro codice usando la funzione `require()` di Node. _Express_ stesso è un modulo, così come le librerie middleware e di database che utilizziamo nelle nostre applicazioni _Express_.

Il codice di seguito mostra come importare un modulo per nome, usando lo _strumento Express_ come esempio. Inizialmente invochiamo la funzione `require()`, specificando il nome del modulo come stringa (`'express'`), e chiamando l'oggetto restituito per creare un'[applicazione Express](https://expressjs.com/en/4x/api.html#app). Possiamo quindi accedere alle proprietà e alle funzioni dell'oggetto applicazione.

```js
const express = require("express");
const app = express();
```

Può anche creare i suoi moduli che possono essere importati allo stesso modo.

> [!NOTE]
> Vorrà _creare_ i suoi moduli, perché ciò le consente di organizzare il suo codice in parti gestibili — un'applicazione monolitica a singolo file è difficile da comprendere e mantenere. L'utilizzo di moduli aiuta anche a gestire il suo namespace, perché solo le variabili che esplicitamente esporta vengono importate quando utilizza un modulo.

Per rendere gli oggetti disponibili al di fuori di un modulo è sufficiente esporli come proprietà aggiuntive sull'oggetto `exports`. Ad esempio, il modulo **square.js** qui sotto è un file che esporta i metodi `area()` e `perimeter()`:

```js
exports.area = function (width) {
  return width * width;
};
exports.perimeter = function (width) {
  return 4 * width;
};
```

Può importare questo modulo usando `require()`, e poi chiamare il/i metodo/i esportato/i come mostrato:

```js
const square = require("./square"); // Here we require() the name of the file without the (optional) .js file extension
console.log(`The area of a square with a width of 4 is ${square.area(4)}`);
```

> [!NOTE]
> Può anche specificare un percorso assoluto verso il modulo (o un nome, come abbiamo fatto inizialmente).

Se vuole esportare un oggetto completo in un'unica assegnazione invece di costruirlo una proprietà alla volta, lo assegni a `module.exports` come mostrato di seguito (può anche fare questo per rendere la radice dell'oggetto esportato un costruttore o altra funzione):

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
> Può pensare a `exports` come a un [collegamento](https://nodejs.org/api/modules.html#modules_exports_shortcut) a `module.exports` all'interno di un determinato modulo. Infatti, `exports` è solo una variabile che viene inizializzata al valore di `module.exports` prima che il modulo venga valutato. Quel valore è un riferimento a un oggetto (oggetto vuoto in questo caso). Ciò significa che `exports` tiene un riferimento allo stesso oggetto riferito da `module.exports`. Significa anche che assegnando un altro valore a `exports` non è più legato a `module.exports`.

Per molte più informazioni sui moduli veda [Moduli](https://nodejs.org/api/modules.html#modules_modules) (Node API docs).

### Utilizzo delle API asincrone

Il codice JavaScript utilizza frequentemente API asincrone anziché sincrone per operazioni che possono richiedere un po' di tempo per completarsi. Un'API sincrona è una in cui ogni operazione deve completarsi prima che la successiva possa iniziare. Ad esempio, le funzioni di log seguenti sono sincrone e stamperanno il testo sulla console in ordine (primo, secondo).

```js
console.log("First");
console.log("Second");
```

Per contro, un'API asincrona è una in cui l'API avvierà un'operazione e restituirà immediatamente (prima che l'operazione sia completa). Una volta che l'operazione termina, l'API utilizzerà qualche meccanismo per eseguire operazioni aggiuntive. Ad esempio, il codice qui sotto stamperà "Secondo, Primo" perché anche se il metodo `setTimeout()` viene chiamato per primo, e restituisce immediatamente, l'operazione non si completa per diversi secondi.

```js
setTimeout(function () {
  console.log("First");
}, 3000);
console.log("Second");
```

L'uso di API asincrone non bloccanti è ancora più importante su Node rispetto al browser perché _Node_ è un ambiente di esecuzione a thread singolo basato su eventi. "A thread singolo" significa che tutte le richieste al server vengono eseguite sullo stesso thread (anziché essere suddivise in processi separati). Questo modello è estremamente efficiente in termini di velocità e risorse del server, ma significa che se una delle sue funzioni chiama metodi sincronizzati che impiegano molto tempo a completarsi, bloccheranno non solo la richiesta corrente, ma ogni altra richiesta gestita dalla sua applicazione web.

Ci sono diversi modi in cui un'API asincrona può notificare alla sua applicazione che si è completata. Il modo più comune è registrare una funzione di callback quando si invoca l'API asincrona, che verrà richiamata quando l'operazione finisce. Questo è l'approccio utilizzato sopra.

> [!NOTE]
> Usare le callback può essere piuttosto "disordinato" se si ha una sequenza di operazioni asincrone dipendenti che devono essere eseguite in ordine, perché questo risulta in più livelli di callback nidificati. Questo problema è comunemente noto come "inferno delle callback". Questo problema può essere ridotto con buone pratiche di codifica (vedi <http://callbackhell.com/>), usando un modulo come [async](https://www.npmjs.com/package/async), o rifattorizzando il codice su funzionalità native JavaScript come [Promises](/it/docs/Web/JavaScript/Reference/Global_Objects/Promise) e [async/await](/it/docs/Web/JavaScript/Reference/Statements/async_function). Node offre la funzione [`utils.promisify`](https://nodejs.org/api/util.html#utilpromisifyoriginal) per fare la conversione callback → Promise in modo ergonomico.

> [!NOTE]
> Una convenzione comune per Node ed Express è usare callback orientate agli errori. In questa convenzione, il primo valore nelle sue _funzioni di callback_ è un valore di errore, mentre gli argomenti successivi contengono dati di successo. C'è una buona spiegazione del perché questo approccio è utile in questo blog: [The Node.js Way - Understanding Error-First Callbacks](https://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/) (fredkschott.com).

### Creazione di gestori di route

Nel nostro esempio di _Hello World_ Express (vedi sopra), abbiamo definito una funzione di gestore di route (callback) per le richieste HTTP `GET` alla radice del sito (`'/'`).

```js
app.get("/", function (req, res) {
  res.send("Hello World!");
});
```

La funzione di callback prende un oggetto richiesta e un oggetto risposta come argomenti. In questo caso, il metodo chiama [`send()`](https://expressjs.com/en/4x/api.html#res.send) sulla risposta per restituire la stringa "Hello World!" Ci sono un [certo numero di altri metodi di risposta](https://expressjs.com/en/guide/routing.html#response-methods) per terminare il ciclo della richiesta/risposta, ad esempio, potrebbe chiamare [`res.json()`](https://expressjs.com/en/4x/api.html#res.json) per inviare una risposta JSON o [`res.sendFile()`](https://expressjs.com/en/4x/api.html#res.sendFile) per inviare un file.

> [!NOTE]
> Può usare qualsiasi nome di argomento le piaccia nelle funzioni di callback; quando la callback viene invocata il primo argomento sarà sempre la richiesta e il secondo sarà sempre la risposta. Ha senso nominarli in modo che possa identificare l'oggetto con cui sta lavorando nel corpo della callback.

L'oggetto _Express application_ fornisce anche metodi per definire gestori di route per tutti gli altri verbi HTTP, che vengono utilizzati per lo più nello stesso modo:

`checkout()`, `copy()`, **`delete()`**, **`get()`**, `head()`, `lock()`, `merge()`, `mkactivity()`, `mkcol()`, `move()`, `m-search()`, `notify()`, `options()`, `patch()`, **`post()`**, `purge()`, **`put()`**, `report()`, `search()`, `subscribe()`, `trace()`, `unlock()`, `unsubscribe()`.

C'è un metodo di routing speciale, `app.all()`, che verrà chiamato in risposta a qualsiasi metodo HTTP. Questo è usato per caricare funzioni middleware in un particolare percorso per tutti i metodi di richiesta. L'esempio seguente (dalla documentazione di Express) mostra un gestore che verrà eseguito per le richieste a `/secret` indipendentemente dal verbo HTTP utilizzato (purché sia supportato dal [http module](https://nodejs.org/docs/latest/api/http.html#httpmethods)).

```js
app.all("/secret", function (req, res, next) {
  console.log("Accessing the secret section…");
  next(); // pass control to the next handler
});
```

Le route consentono di abbinare particolari pattern di caratteri in un URL, ed estrarre alcuni valori dall'URL e passarli come parametri al gestore della route (come attributi dell'oggetto richiesta passato come parametro).

Spesso è utile raggruppare i gestori di route per una particolare parte di un sito insieme e accedervi tramite un prefisso di route comune (es., un sito con una wiki potrebbe avere tutte le route correlate alla wiki in un file e accedervi con un prefisso di route di _/wiki/_). In _Express_ questo viene ottenuto utilizzando l'oggetto [`express.Router`](https://expressjs.com/en/guide/routing.html#express-router). Ad esempio, possiamo creare la nostra route wiki in un modulo chiamato **wiki.js**, e quindi esportare l'oggetto `Router`, come mostrato di seguito:

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

Per usare il router nel nostro file app principale `require()` il modulo di route (**wiki.js**), e poi si chiami `use()` sull'applicazione Express per aggiungere il Router al percorso di gestione middleware. Le due route saranno quindi accessibili da `/wiki/` e `/wiki/about/`.

```js
const wiki = require("./wiki.js");
// …
app.use("/wiki", wiki);
```

Le mostreremo molto di più su come lavorare con le route, e in particolare su come usare il `Router`, più avanti nella sezione collegata [Route e controller](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes).

### Uso di middleware

Il middleware è usato ampiamente nelle app Express, per compiti che vanno dalla fornitura di file statici alla gestione degli errori, alla compressione delle risposte HTTP. Mentre le funzioni delle route terminano il ciclo della richiesta-risposta HTTP restituendo alcune risposte al client HTTP, le funzioni middleware _tipicamente_ eseguono qualche operazione sulla richiesta o risposta e poi chiamano la funzione successiva nella "stack", che potrebbe essere un altro middleware o un gestore di route. L'ordine in cui viene chiamato il middleware è a discrezione dello sviluppatore dell'app.

> [!NOTE]
> Il middleware può eseguire qualsiasi operazione, eseguire qualsiasi codice, apportare modifiche all'oggetto richiesta e risposta, e può _anche terminare il ciclo della richiesta-risposta_. Se non termina il ciclo, deve chiamare `next()` per passare il controllo alla funzione middleware successiva (o la richiesta rimarrà sospesa).

La maggior parte delle app utilizzerà middleware di _terze parti_ per semplificare compiti comuni di sviluppo web come lavorare con cookie, sessioni, autenticazione utenti, accesso ai dati `POST` e JSON della richiesta, logging, ecc. Può trovare un [elenco di pacchetti middleware mantenuti dal team di Express](https://expressjs.com/en/resources/middleware.html) (che include anche altri pacchetti popolari di terze parti). Altri pacchetti Express sono disponibili sul gestore di pacchetti npm.

Per utilizzare middleware di terze parti deve prima installarlo nella sua app usando npm. 
Ad esempio, per installare il middleware logger delle richieste HTTP [morgan](https://expressjs.com/en/resources/middleware/morgan.html), farebbe così:

```bash
npm install morgan
```

Potrebbe quindi chiamare `use()` sull'_oggetto applicazione di Express_ per aggiungere il middleware alla stack:

```js
const express = require("express");
const logger = require("morgan");
const app = express();
app.use(logger("dev"));
// …
```

> [!NOTE]
> Le funzioni di middleware e di routing vengono chiamate nell'ordine in cui vengono dichiarate. Per alcuni middleware l'ordine è importante (ad esempio, se il middleware delle sessioni dipende dal middleware dei cookie, il gestore dei cookie deve essere aggiunto per primo). È quasi sempre il caso che il middleware venga chiamato prima di impostare le route, o i suoi handler di route non avranno accesso alla funzionalità aggiunta dal suo middleware.

Può scrivere le sue funzioni middleware, ed è probabile che debba farlo (se non altro per creare codice di gestione degli errori). L'_unica_ differenza tra una funzione middleware e una callback di gestione delle route è che le funzioni middleware hanno un terzo argomento `next`, che ci si aspetta che le funzioni middleware chiamino se non sono quelle che completano il ciclo della richiesta (quando la funzione middleware è chiamata, questo contiene la funzione _next_ che deve essere chiamata).

Può aggiungere una funzione middleware alla catena di elaborazione per _tutte le risposte_ con `app.use()`, o per un verbo HTTP specifico usando il metodo associato: `app.get()`, `app.post()`, ecc. Le route vengono specificate allo stesso modo per entrambi i casi, anche se la route è opzionale quando si chiama `app.use()`.

L'esempio qui sotto mostra come può aggiungere la funzione middleware usando entrambi gli approcci, e/o con o senza una route.

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
> Sopra dichiariamo separatamente la funzione middleware e poi la impostiamo come callback. Nella nostra precedente funzione di handler di route abbiamo dichiarato la funzione di callback quando è stata utilizzata. In JavaScript, entrambi gli approcci sono validi.

La documentazione di Express ha molta più documentazione eccellente sull'[uso](https://expressjs.com/en/guide/using-middleware.html) e sulla [scrittura](https://expressjs.com/en/guide/writing-middleware.html) delle middleware di Express.

### Fornitura di file statici

Può utilizzare il middleware [express.static](https://expressjs.com/en/4x/api.html#express.static) per fornire file statici, inclusi le sue immagini, CSS e JavaScript (`static()` è l'unica funzione middleware che è effettivamente **parte** di _Express_). Ad esempio, userebbe la riga sottostante per fornire immagini, file CSS e file JavaScript da una directory chiamata '**public'** allo stesso livello di dove chiami node:

```js
app.use(express.static("public"));
```

Qualsiasi file nella directory pubblica viene servito aggiungendo il relativo nome del file (_relativo_ alla directory base "public") all'URL base. Quindi, ad esempio:

```plain
http://localhost:3000/images/dog.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/about.html
```

Può chiamare `static()` più volte per servire più directory. Se un file non può essere trovato da una funzione middleware viene passato alla funzione middleware successiva (l'ordine in cui viene chiamato il middleware si basa sul suo ordine dichiarativo).

```js
app.use(express.static("public"));
app.use(express.static("media"));
```

Può anche creare un prefisso virtuale per i suoi URL statici, anziché aggiungere i file all'URL base. Ad esempio, qui specifichiamo un [percorso montato](https://expressjs.com/en/4x/api.html#app.use) in modo che i file siano caricati con il prefisso "/media":

```js
app.use("/media", express.static("public"));
```

Ora, può caricare i file che sono nella directory `public` dal prefisso di percorso `/media`.

```plain
http://localhost:3000/media/images/dog.jpg
http://localhost:3000/media/video/cat.mp4
http://localhost:3000/media/cry.mp3
```

> [!NOTE]
> Vedi anche [Fornitura di file statici in Express](https://expressjs.com/en/starter/static-files.html).

### Gestione degli errori

Gli errori vengono gestiti da una o più funzioni middleware speciali che hanno quattro argomenti, invece dei soliti tre: `(err, req, res, next)`. Ad esempio:

```js
app.use(function (err, req, res, next) {
  console.error(err.stack);
  res.status(500).send("Something broke!");
});
```

Queste possono restituire qualsiasi contenuto richiesto, ma devono essere chiamate dopo tutte le altre chiamate `app.use()` e route in modo che siano l'ultimo middleware nel processo di gestione delle richieste!

Express viene fornito con un gestore degli errori integrato, che si occupa di eventuali errori rimanenti che potrebbero essere incontrati nell'app. Questa funzione middleware predefinita per la gestione degli errori viene aggiunta alla fine della stack della funzione middleware. Se passa un errore a `next()` e non lo gestisce in un handler di errori, verrà gestito dal gestore degli errori integrato; l'errore sarà scritto al client con il traccia dello stack.

> [!NOTE]
> Il traccia dello stack non è incluso nell'ambiente di produzione. Per eseguirlo in modalità produzione deve impostare la variabile d'ambiente `NODE_ENV` su `"production"`.

> [!NOTE]
> Gli HTTP404 e altri codici di stato di "errore" non sono trattati come errori. Se vuole gestire questi, può aggiungere una funzione middleware per farlo. Per più informazioni vedi le [FAQ](https://expressjs.com/en/starter/faq.html#how-do-i-handle-404-responses).

Per più informazioni vedi [Gestione degli errori](https://expressjs.com/en/guide/error-handling.html) (Doc Express).

### Utilizzo dei database

Le app _Express_ possono utilizzare qualsiasi meccanismo di database supportato da _Node_ (Express stesso non definisce alcun comportamento/requisiti specifici aggiuntivi per la gestione dei database). Ci sono molte opzioni, tra cui PostgreSQL, MySQL, Redis, SQLite, MongoDB, ecc.

Per utilizzarli deve prima installare il driver del database usando npm. Ad esempio, per installare il driver per il popolare NoSQL MongoDB userebbe il comando:

```bash
npm install mongodb
```

Il database stesso può essere installato localmente o su un server cloud. Nel suo codice Express si necessiterà il driver, si connetterà al database e poi eseguire operazioni di creazione, lettura, aggiornamento e eliminazione (CRUD). L'esempio seguente (dalla documentazione di Express) mostra come potrebbe trovare record "mammiferi" utilizzando MongoDB.

Questo funziona con versioni più vecchie di MongoDB versione ~ 2.2.33:

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

Per MongoDB versione 3.0 e successive:

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

Un altro approccio popolare è accedere al suo database indirettamente, tramite un Object Relational Mapper ("ORM"). In questo approccio definisce i suoi dati come "oggetti" o "modelli" e l'ORM li mappa attraverso al formato del database sottostante. Questo approccio ha il vantaggio che come sviluppatore può continuare a pensare in termini di oggetti JavaScript piuttosto che di semantica del database, e che c'è un luogo ovvio in cui eseguire la convalida e il controllo dei dati in ingresso. Parleremo di più sui database in un articolo successivo.

Per ulteriori informazioni vedi [Integrazione del database](https://expressjs.com/en/guide/database-integration.html) (Doc Express).

### Rendering dei dati (views)

I motori di template (denominati anche "motori di visualizzazione" in _Express_) consentono di specificare la _struttura_ di un documento in uscita in un template, utilizzando segnaposto per i dati che verranno riempiti quando una pagina viene generata. I template spesso vengono utilizzati per creare HTML, ma possono anche creare altri tipi di documenti.

Express supporta un certo numero di motori di template, in particolare Pug (precedentemente "Jade"), Mustache e EJS. Ognuno ha i suoi punti di forza per affrontare casi di utilizzo particolari (confronti relativi possono essere facilmente trovati tramite una ricerca Internet).
Il generatore di applicazioni Express utilizza Jade come predefinito, ma supporta anche molti altri.

Nel codice delle impostazioni dell'applicazione imposta il motore di template da utilizzare e la posizione in cui Express dovrebbe cercare template utilizzando le impostazioni 'views' e 'view engine', come mostrato di seguito (dovrà anche installare il pacchetto contenente la sua libreria di template!)

```js
const express = require("express");
const path = require("path");
const app = express();

// Set directory to contain the templates ('views')
app.set("views", path.join(__dirname, "views"));

// Set view engine to use, in this case 'some_template_engine_name'
app.set("view engine", "some_template_engine_name");
```

L'aspetto del template dipenderà da quale motore utilizza. Assumendo che abbia un file di modello denominato "index.\<template_extension>" che contiene segnaposto per le variabili dati denominate 'title' e "message", potrebbe chiamare [`Response.render()`](https://expressjs.com/en/4x/api.html#res.render) in una funzione di gestore delle route per creare e inviare la risposta HTML:

```js
app.get("/", function (req, res) {
  res.render("index", { title: "About dogs", message: "Dogs rock!" });
});
```

Per ulteriori informazioni vedi [Uso dei motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (Doc Express).

### Struttura dei file

Express non fa ipotesi in termini di struttura o di quali componenti utilizza. Le route, le viste, i file statici e altra logica specifica dell'applicazione possono risiedere in un numero qualsiasi di file con qualsiasi struttura di directory. Mentre è perfettamente possibile avere l'intera applicazione _Express_ in un file, tipicamente ha senso suddividere la sua applicazione in file basati sulla funzione (es. gestione degli account, blog, bacheche) e sul dominio del problema architettonico (es. modello, vista o controller se per caso utilizza un'architettura {{Glossary("MVC", "MVC")}}).

In un argomento successivo utilizzeremo il _Generatore di Applicazioni Express_, che crea uno scheletro di app modulare che possiamo facilmente estendere per creare applicazioni web.

## Sommario

Congratulazioni, ha completato il primo passo nel suo viaggio con Express/Node! Dovrebbe ora comprendere i principali vantaggi di Express e Node, e approssimativamente come potrebbero apparire le principali parti di un'app Express (routes, middleware, gestione degli errori, e codice di template). Dovrebbe anche comprendere che con Express essendo un framework non opinabile, il modo in cui mette insieme queste parti e le librerie che utilizza sono in gran parte a sua discrezione!

Ovviamente Express è deliberatamente un framework di applicazioni web molto leggero, quindi gran parte del suo vantaggio e potenziale deriva dalle librerie e dalle caratteristiche di terze parti. Le esamineremo in maggior dettaglio nei seguenti articoli. Nel nostro prossimo articolo guarderemo come impostare un ambiente di sviluppo Node, in modo che possa iniziare a vedere un po' di codice Express in azione.

## Vedi anche

- [Moduli](https://nodejs.org/api/modules.html#modules_modules) (Node API docs)
- [Express](https://expressjs.com/) (home page)
- [Basic routing](https://expressjs.com/en/starter/basic-routing.html) (Doc Express)
- [Routing guide](https://expressjs.com/en/guide/routing.html) (Doc Express)
- [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html) (Doc Express)
- [Using middleware](https://expressjs.com/en/guide/using-middleware.html) (Doc Express)
- [Writing middleware for use in Express apps](https://expressjs.com/en/guide/writing-middleware.html) (Doc Express)
- [Database integration](https://expressjs.com/en/guide/database-integration.html) (Doc Express)
- [Serving static files in Express](https://expressjs.com/en/starter/static-files.html) (Doc Express)
- [Error handling](https://expressjs.com/en/guide/error-handling.html) (Doc Express)

{{NextMenu("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
