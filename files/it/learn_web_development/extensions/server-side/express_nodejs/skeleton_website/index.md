---
title: "Express Tutorial Parte 2: Creare un sito web scheletro"
short-title: "2: Sito web scheletro"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo secondo articolo nel nostro [Tutorial Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) mostra come creare un progetto di sito web "scheletro" che puoi poi popolare con percorsi specifici del sito, template/viste e chiamate al database.

> [!WARNING]
> Il tutorial Express è scritto per la versione 4 di Express, mentre la versione più recente è Express 5.
> Abbiamo intenzione di aggiornare la documentazione nella seconda metà del 2025.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">Impostare un ambiente di sviluppo Node</a>.
          Rivedere il Tutorial Express.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di iniziare i propri nuovi progetti di siti web usando l'<em>Express Application Generator</em>.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Questo articolo mostra come creare un sito web "scheletro" usando lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator.html), che poi puoi popolare con percorsi specifici del sito, viste/template e chiamate al database. In questo caso, useremo lo strumento per creare il framework per il nostro [sito web Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), a cui successivamente aggiungeremo tutto il codice necessario per il sito. Il processo è estremamente semplice, richiedendo solo l'invocazione del generatore sulla riga di comando con un nuovo nome di progetto, specificando eventualmente anche il motore di template del sito e il generatore CSS.

Le sezioni seguenti mostrano come chiamare il generatore di applicazioni e forniscono una piccola spiegazione sulle diverse opzioni di visualizzazione/CSS. Spiegheremo anche come è strutturato il sito web scheletro. Infine, mostreremo come puoi eseguire il sito web per verificare che funzioni.

> [!NOTE]
>
> - L'_Express Application Generator_ non è l'unico generatore per applicazioni Express, e il progetto generato non è l'unico modo valido per strutturare i tuoi file e directory. Tuttavia, il sito generato ha una struttura modulare che è facile da estendere e comprendere. Per informazioni su un'applicazione minima di Express, vedi [Esempio Hello world](https://expressjs.com/en/starter/hello-world.html) (documentazione Express).
> - L'_Express Application Generator_ dichiara la maggior parte delle variabili usando `var`.
>   Abbiamo cambiato la maggior parte di queste in [`const`](/it/docs/Web/JavaScript/Reference/Statements/const) (e alcune in [`let`](/it/docs/Web/JavaScript/Reference/Statements/let)) nel tutorial, perché vogliamo dimostrare pratiche moderne di JavaScript.
> - Questo tutorial utilizza la versione di _Express_ e altre dipendenze definite nel **package.json** creato dall'_Express Application Generator_.
>   Queste non sono (necessariamente) le versioni più recenti, e potresti volerle aggiornare quando distribuisci un'applicazione reale in produzione.

## Uso del generatore di applicazioni

Dovresti già aver installato il generatore come parte della configurazione di un [ambiente di sviluppo Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment). Come rapido promemoria, installi lo strumento di generazione a livello di sito utilizzando il gestore di pacchetti npm, come mostrato:

```bash
npm install express-generator -g
```

Il generatore ha una serie di opzioni, che puoi visualizzare sulla riga di comando usando il comando `--help` (o `-h`):

```bash
> express --help

    Usage: express [options] [dir]

  Options:

        --version        output the version number
    -e, --ejs            add ejs engine support
        --pug            add pug engine support
        --hbs            add handlebars engine support
    -H, --hogan          add hogan.js engine support
    -v, --view <engine>  add view <engine> support (dust|ejs|hbs|hjs|jade|pug|twig|vash) (defaults to jade)
        --no-view        use static html instead of view engine
    -c, --css <engine>   add stylesheet <engine> support (less|stylus|compass|sass) (defaults to plain CSS)
        --git            add .gitignore
    -f, --force          force on non-empty directory
    -h, --help           output usage information
```

Puoi specificare express per creare un progetto all'interno della directory _attuale_ utilizzando il motore di visualizzazione _Jade_ e CSS semplice (se specifichi un nome di directory, il progetto verrà creato in una sottocartella con quel nome).

```bash
express
```

Puoi anche scegliere un motore di visualizzazione (template) utilizzando `--view` e/o un motore di generazione CSS utilizzando `--css`.

> [!NOTE]
> Le altre opzioni per scegliere i motori di template (ad es., `--hogan`, `--ejs`, `--hbs`, ecc.) sono deprecate. Usa `--view` (o `-v`).

### Quale motore di visualizzazione dovrei usare?

L'_Express Application Generator_ ti consente di configurare un numero di motori di visualizzazione/template popolari, tra cui [EJS](https://www.npmjs.com/package/ejs), [Hbs](https://github.com/pillarjs/hbs), [Pug](https://pugjs.org/api/getting-started.html) (Jade), [Twig](https://www.npmjs.com/package/twig) e [Vash](https://www.npmjs.com/package/vash), anche se sceglie Jade per default se non specifichi un'opzione di visualizzazione. Express stesso può anche supportare un gran numero di altri linguaggi di template [out of the box](https://github.com/expressjs/express/wiki#template-engines).

> [!NOTE]
> Se vuoi usare un motore di template non supportato dal generatore, consulta [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express) e la documentazione per il tuo motore di visualizzazione obiettivo.

In generale, dovresti selezionare un motore di templating che fornisce tutte le funzionalità di cui hai bisogno e ti permette di essere produttivo prima — o in altre parole, nello stesso modo in cui scegli qualsiasi altro componente! Alcune delle cose da considerare quando confronti i motori di template:

- Tempo di produttività — Se il tuo team ha già esperienza con un linguaggio di templating, è probabile che sarà più produttivo usando quello. Se non ce l'ha, allora dovresti considerare la curva di apprendimento relativa per i motori di template candidati.
- Popolarità e attività — Rivedi la popolarità del motore e se ha una comunità attiva. È importante poter ottenere supporto quando sorgono problemi durante il ciclo di vita del sito web.
- Stile — Alcuni motori di template utilizzano markup specifico per indicare il contenuto inserito all'interno di HTML "ordinario", mentre altri costruiscono l'HTML usando una sintassi diversa (ad esempio, usando rientri e nomi di blocco).
- Prestazioni/tempi di rendering.
- Funzionalità — dovresti considerare se i motori che stai esaminando hanno le seguenti funzionalità disponibili:

  - Ereditarietà layout: Ti permette di definire un template base e poi "ereditare" solo le parti di esso che vuoi che siano diverse per una pagina particolare. Questo è tipicamente un approccio migliore rispetto a costruire template includendo un certo numero di componenti richiesti o costruendo un template da zero ogni volta.
  - Supporto "Include": Ti permette di costruire template includendo altri template.
  - Sintassi concisa per variabili e controllo dei loop.
  - Capacità di filtrare i valori delle variabili a livello di template, come rendere le variabili maiuscole, o formattare un valore di data.
  - Capacità di generare formati di output diversi da HTML, come JSON o XML.
  - Supporto per operazioni asincrone e streaming.
  - Funzionalità lato client. Se un motore di templating può essere utilizzato sul client, questo permette la possibilità di avere tutto o la maggior parte del rendering fatto sul lato client.

> [!NOTE]
> Ci sono molte risorse su Internet per aiutarti a confrontare le diverse opzioni!

Per questo progetto, useremo il motore di template [Pug](https://pugjs.org/api/getting-started.html) (questo è il motore precedentemente chiamato Jade), poiché è uno dei linguaggi di templating per Express/JavaScript più popolari ed è supportato nativamente dal generatore.

### Quale motore di fogli di stile CSS dovrei usare?

L'_Express Application Generator_ ti permette di creare un progetto configurato per usare i motori di fogli di stile CSS più comuni: [LESS](https://lesscss.org/), [SASS](https://sass-lang.com/), [Stylus](https://stylus-lang.com/).

> [!NOTE]
> CSS ha alcune limitazioni che rendono certi compiti difficili. I motori di fogli di stile CSS ti permettono di usare una sintassi più potente per definire i tuoi CSS e poi compilare la definizione in semplice CSS che i browser possono utilizzare.

Come per i motori di templating, dovresti usare il motore di fogli di stile che permetterà al tuo team di essere il più produttivo possibile. Per questo progetto, useremo il CSS standard (il predefinito), poiché i nostri requisiti CSS non sono sufficientemente complicati da giustificare l'uso di altro.

### Quale database dovrei usare?

Il codice generato non utilizza/incluse alcun database. Le app _Express_ possono usare qualsiasi [meccanismo di database](https://expressjs.com/en/guide/database-integration.html) supportato da _Node_ (Express stesso non definisce alcun comportamento/additional requirements specifico per la gestione dei database).

Discuteremo come integrare un database in un articolo successivo.

## Creare il progetto

Per l'app _Biblioteca Locale_ di esempio che costruiremo, creeremo un progetto chiamato _express-locallibrary-tutorial_ utilizzando la libreria di template _Pug_ e nessun motore CSS.

Prima, naviga dove vuoi creare il progetto e poi esegui l'_Express Application Generator_ nel prompt dei comandi come mostrato:

```bash
express express-locallibrary-tutorial --view=pug
```

Il generatore creerà (e elencherà) i file del progetto.

```plain
   create : express-locallibrary-tutorial\
   create : express-locallibrary-tutorial\public\
   create : express-locallibrary-tutorial\public\javascripts\
   create : express-locallibrary-tutorial\public\images\
   create : express-locallibrary-tutorial\public\stylesheets\
   create : express-locallibrary-tutorial\public\stylesheets\style.css
   create : express-locallibrary-tutorial\routes\
   create : express-locallibrary-tutorial\routes\index.js
   create : express-locallibrary-tutorial\routes\users.js
   create : express-locallibrary-tutorial\views\
   create : express-locallibrary-tutorial\views\error.pug
   create : express-locallibrary-tutorial\views\index.pug
   create : express-locallibrary-tutorial\views\layout.pug
   create : express-locallibrary-tutorial\app.js
   create : express-locallibrary-tutorial\package.json
   create : express-locallibrary-tutorial\bin\
   create : express-locallibrary-tutorial\bin\www

   change directory:
     > cd express-locallibrary-tutorial

   install dependencies:
     > npm install

   run the app (Bash (Linux or macOS))
     > DEBUG=express-locallibrary-tutorial:* npm start

   run the app (PowerShell (Windows))
     > $ENV:DEBUG = "express-locallibrary-tutorial:*"; npm start

   run the app (Command Prompt (Windows)):
     > SET DEBUG=express-locallibrary-tutorial:* & npm start
```

Alla fine dell'output, il generatore fornisce istruzioni su come installare le dipendenze (come elencate nel file **package.json**) e come eseguire l'applicazione su diversi sistemi operativi.

> [!NOTE]
> I file creati dal generatore definiscono tutte le variabili come `var`.
> Apri tutti i file generati e cambia le dichiarazioni `var` in `const` prima di continuare (il resto del tutorial presume che tu lo abbia fatto).

## Esecuzione del sito web scheletro

A questo punto, abbiamo un progetto scheletro completo. Il sito web non _fa_ ancora molto, ma vale la pena eseguirlo per dimostrare che funziona.

1. Per prima cosa, installa le dipendenze (il comando `install` scaricherà tutti i pacchetti di dipendenze elencati nel file **package.json** del progetto).

   ```bash
   cd express-locallibrary-tutorial
   npm install
   ```

2. Poi esegui l'applicazione.

   - Sul prompt CMD di Windows, usa questo comando:

     ```batch
     SET DEBUG=express-locallibrary-tutorial:* & npm start
     ```

   - Su Windows PowerShell, usa questo comando:

     ```powershell
     ENV:DEBUG = "express-locallibrary-tutorial:*"; npm start
     ```

     > [!NOTE]
     > I comandi PowerShell non sono coperti in questo tutorial (i comandi "Windows" forniti presuppongono che stai usando il prompt CMD di Windows.)

   - Su macOS o Linux, usa questo comando:

     ```bash
     DEBUG=express-locallibrary-tutorial:* npm start
     ```

3. Poi carica `http://localhost:3000/` nel tuo browser per accedere all'app.

Dovresti vedere una pagina del browser che appare così:

![Browser per il sito web generato di default di Express](expressgeneratorskeletonwebsite.png)

Congratulazioni! Ora hai un'applicazione Express funzionante che può essere accessibile tramite la porta 3000.

> [!NOTE]
> Potresti anche avviare l'app semplicemente usando il comando `npm start`. Specificare la variabile DEBUG come mostrato abilita il logging/debugging della console. Per esempio, quando visiti la pagina sopra vedrai un output di debug simile a questo:
>
> ```bash
> SET DEBUG=express-locallibrary-tutorial:* & npm start
> ```
>
> ```plain
> > express-locallibrary-tutorial@0.0.0 start D:\github\mdn\test\exprgen\express-locallibrary-tutorial
> > node ./bin/www
>
>   express-locallibrary-tutorial:server Listening on port 3000 +0ms
> GET / 304 490.296 ms - -
> GET /stylesheets/style.css 200 4.886 ms - 111
> ```

## Abilitare il riavvio del server sui cambiamenti dei file

Qualsiasi cambiamento apportato al sito web Express attualmente non è visibile finché non riavvii il server. Diventa rapidamente molto irritante dover fermare e riavviare il server ogni volta che si effettua una modifica, quindi vale la pena automatizzare il riavvio del server quando necessario.

Uno strumento conveniente per questo scopo è [nodemon](https://github.com/remy/nodemon). Questo è di solito installato globalmente (poiché è uno "strumento"), ma qui lo installeremo e lo useremo localmente come una _dipendenza per lo sviluppo_, in modo che qualsiasi sviluppatore che lavori con il progetto lo ottenga automaticamente quando installa l'applicazione. Usa il seguente comando nella directory radice per il progetto scheletro:

```bash
npm install --save-dev nodemon
```

Se scegli ancora di installare [nodemon](https://github.com/remy/nodemon) globalmente sulla tua macchina, e non solo al file **package.json** del tuo progetto:

```bash
npm install -g nodemon
```

Se apri il file **package.json** del tuo progetto ora vedrai una nuova sezione con questa dipendenza:

```json
 "devDependencies": {
    "nodemon": "^3.1.3"
}
```

Poiché lo strumento non è installato globalmente, non possiamo avviarlo dalla riga di comando (a meno che non lo aggiungiamo al percorso) ma possiamo chiamarlo da uno script npm perché npm sa tutto dei pacchetti installati. Trova la sezione `scripts` del tuo package.json. Inizialmente, conterrà una riga che inizia con `"start"`. Aggiornala mettendoci una virgola alla fine di quella riga e aggiungendo le righe `"devstart"` e `"serverstart"`:

- Su Linux e macOS, la sezione scripts apparirà così:

  ```json
    "scripts": {
      "start": "node ./bin/www",
      "devstart": "nodemon ./bin/www",
      "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
    },
  ```

- Su Windows, il valore "serverstart" sarebbe invece simile a questo (se si usa il prompt dei comandi):

  ```bash
  "serverstart": "SET DEBUG=express-locallibrary-tutorial:* & npm run devstart"
  ```

Ora possiamo avviare il server in quasi esattamente lo stesso modo di prima, ma usando il comando `devstart`.

> [!NOTE]
> Ora, se modifichi qualsiasi file nel progetto, il server si riavvierà (o puoi riavviarlo digitando `rs` sul prompt dei comandi in qualsiasi momento). Dovrai comunque ricaricare il browser per aggiornare la pagina.
>
> Ora dobbiamo chiamare `npm run <script-name>` piuttosto che solo `npm start`, perché "start" è effettivamente un comando npm che è mappato allo script nominato. Avremmo potuto sostituire il comando nello script _start_ ma vogliamo usare _nodemon_ solo durante lo sviluppo, quindi ha senso creare un nuovo comando di script.
>
> Il comando `serverstart` aggiunto agli script nel **package.json** sopra è un ottimo esempio. Usando questo approccio, non devi più digitare un comando lungo per avviare il server. Nota che il comando particolare aggiunto allo script funziona solo per macOS o Linux.

## Il progetto generato

Diamo ora un'occhiata al progetto che abbiamo appena creato.

### Struttura delle directory

Il progetto generato, ora che hai installato le dipendenze, ha la seguente struttura di file (i file sono gli elementi **non** prefissati con "/"). Il file **package.json** definisce le dipendenze dell'applicazione e altre informazioni. Definisce anche uno script di avvio che chiamerà il punto di ingresso dell'applicazione, il file JavaScript **/bin/www**. Questo imposta alcune delle gestione degli errori dell'applicazione e poi carica **app.js** per fare il resto del lavoro. I percorsi dell'app sono archiviati in moduli separati sotto la directory **routes/**. I template sono archiviati sotto la directory /**views**.

```plain
express-locallibrary-tutorial
    app.js
    /bin
        www
    package.json
    package-lock.json
    /node_modules
        [about 6700 subdirectories and files]
    /public
        /images
        /javascripts
        /stylesheets
            style.css
    /routes
        index.js
        users.js
    /views
        error.pug
        index.pug
        layout.pug
```

Le sezioni seguenti descrivono i file in un po' più di dettaglio.

### package.json

Il file **package.json** definisce le dipendenze dell'applicazione e altre informazioni:

```json
{
  "name": "express-locallibrary-tutorial",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "start": "node ./bin/www"
  },
  "dependencies": {
    "cookie-parser": "~1.4.4",
    "debug": "~2.6.9",
    "express": "~4.16.1",
    "http-errors": "~1.6.3",
    "morgan": "~1.9.1",
    "pug": "2.0.0-beta11"
  },
  "devDependencies": {
    "nodemon": "^3.1.3"
  }
}
```

La sezione scripts definisce prima uno script "_start_", che è ciò che stiamo invocando quando chiamiamo `npm start` per avviare il server (questo script è stato aggiunto dall'_Express Application Generator_). Dalla definizione dello script, puoi vedere che questo effettivamente avvia il file JavaScript **./bin/www** con _node_.

Abbiamo già modificato questa sezione in [Abilitare il riavvio del server sui cambiamenti dei file](#abilitare_il_riavvio_del_server_sui_cambiamenti_dei_file) aggiungendo gli script _devstart_ e _serverstart_. Questi possono essere usati per avviare lo stesso file **./bin/www** con _nodemon_ piuttosto che _node_ (questa versione degli script è per Linux e macOS, come discusso sopra).

```json
  "scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www",
    "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
  },
```

Le dipendenze includono il pacchetto _express_ e il pacchetto per il nostro motore di visualizzazione selezionato (_pug_). Inoltre, abbiamo i seguenti pacchetti che sono utili in molte applicazioni web:

- [cookie-parser](https://www.npmjs.com/package/cookie-parser): Usato per analizzare l'intestazione del cookie e popolare `req.cookies` (fornisce essenzialmente un metodo conveniente per accedere alle informazioni sui cookie).
- [debug](https://www.npmjs.com/package/debug): Una piccola utilità di debugging per node modellata sulla tecnica di debugging del core di node.
- [morgan](https://www.npmjs.com/package/morgan): Un middleware di registrazione delle richieste HTTP per node.
- [http-errors](https://www.npmjs.com/package/http-errors): Crea errori HTTP dove necessario (per la gestione degli errori di express).

Le versioni predefinite nel progetto generato sono un po' obsolete. Sostituisci la sezione dipendenze del tuo file `package.json` con il seguente testo, che specifica le versioni più recenti di queste librerie al momento della scrittura:

```json
  "dependencies": {
    "cookie-parser": "^1.4.6",
    "debug": "^4.3.5",
    "express": "^4.19.2",
    "http-errors": "~2.0.0",
    "morgan": "^1.10.0",
    "pug": "3.0.3"
  },
```

Poi aggiorna le tue dipendenze installate usando il comando:

```bash
npm install
```

> [!NOTE]
> È una buona idea aggiornare regolarmente alle versioni compatibili più recenti delle librerie di dipendenze — questo può essere fatto automaticamente o semi-automaticamente come parte di un setup di integrazione continua.
>
> Solitamente gli aggiornamenti delle librerie alla versione minore e patch rimangono compatibili. Abbiamo prefissato ogni versione con `^` sopra, in modo da poter aggiornare automaticamente all'ultima versione `minor.patch` eseguendo:
>
> ```bash
> npm update --save
> ```
>
> Le versioni major cambiano la compatibilità. Per quegli aggiornamenti, dovremo aggiornare manualmente il `package.json` e il codice che utilizza la libreria, e testare ampiamente il progetto.

### file www

Il file **/bin/www** è il punto di ingresso dell'applicazione! La primissima cosa che fa è `require()` il "vero" punto di ingresso dell'applicazione (**app.js**, nella radice del progetto) che configura e restituisce l'oggetto applicazione [`express()`](https://expressjs.com/en/api.html). `require()` è il modo [CommonJS](https://nodejs.org/api/modules.html) per importare codice JavaScript, JSON, e altri file nel file corrente. Qui specifichiamo il modulo **app.js** utilizzando un percorso relativo e omettiamo l'estensione del file opzionale (.**js**).

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

const app = require("../app");
```

> [!NOTE]
> Node.js 14 e versioni successive supportano le dichiarazioni `import` ES6 per l'importazione di moduli JavaScript (ECMAScript). Per usare questa funzionalità, devi aggiungere `"type": "module",` al tuo file **package.json** di Express, tutti i moduli nella tua applicazione devono utilizzare `import` anziché `require()`, e per le _importazioni relative_ devi includere l'estensione del file (per maggiori informazioni vedi la [documentazione Node](https://nodejs.org/api/esm.html#introduction)).
> Anche se ci sono benefici nell'usare `import`, questo tutorial usa `require()` per corrispondere alla [documentazione di Express](https://expressjs.com/en/starter/hello-world.html).

Il resto del codice in questo file imposta un server HTTP node con `app` impostata su una porta specifica (definita in una variabile d'ambiente o 3000 se la variabile non è definita), e inizia ad ascoltare e a riportare errori e connessioni del server. Per ora non hai bisogno di sapere altro sul codice (tutto in questo file è "boilerplate"), ma sentiti libero di rivederlo se sei interessato.

### app.js

Questo file crea un oggetto applicazione `express` (chiamato `app`, per convenzione), imposta l'applicazione con varie impostazioni e middleware, e poi esporta l'app dal modulo. Il codice sottostante mostra solo le parti del file che creano ed esportano l'oggetto app:

```js
const express = require("express");

const app = express();
// …
module.exports = app;
```

Nel file del punto di ingresso **www** sopra, è questo oggetto `module.exports` che viene fornito al chiamante quando questo file viene importato.

Vediamo ora il file **app.js** in dettaglio. Prima, importiamo alcune utili librerie node nel file usando `require()`, tra cui _http-errors_, _express_, _morgan_ e _cookie-parser_ che abbiamo precedentemente scaricato per la nostra applicazione utilizzando npm; e _path_, che è una libreria core di Node per l'analisi dei percorsi di file e directory.

```js
const createError = require("http-errors");
const express = require("express");
const path = require("path");
const cookieParser = require("cookie-parser");
const logger = require("morgan");
```

Poi `require()` moduli dalla nostra directory dei percorsi. Questi moduli/file contengono codice per gestire particolari insiemi di "percorsi" (percorsi URL) correlati. Quando estenderemo l'applicazione scheletro, ad esempio per elencare tutti i libri nella biblioteca, aggiungeremo un nuovo file per gestire i percorsi relativi ai libri.

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
```

> [!NOTE]
> A questo punto, abbiamo solo _importato_ il modulo; non abbiamo ancora usato i suoi percorsi (questo accade poco più in basso nel file).

Successivamente, creiamo l'oggetto `app` utilizzando il nostro modulo _express_ importato, e poi lo utilizziamo per impostare il motore delle viste (template). Ci sono due parti per impostare il motore. Prima, impostiamo il valore `"views"` per specificare la cartella dove saranno conservati i template (in questo caso la sottocartella **/views**). Poi impostiamo il valore `"view engine"` per specificare la libreria del template (in questo caso "pug").

```js
const app = express();

// view engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Il set successivo di funzioni chiama `app.use()` per aggiungere le librerie _middleware_ che abbiamo importato sopra nella catena di gestione delle richieste. Ad esempio, `express.json()` e `express.urlencoded()` sono necessari per popolare [`req.body`](https://expressjs.com/en/api.html#req.body) con i campi del modulo. Dopo queste librerie utilizziamo anche il middleware `express.static`, che fa sì che _Express_ serva tutti i file statici nella directory **/public** nella radice del progetto.

```js
app.use(logger("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

app.use(express.static(path.join(__dirname, "public")));
```

Ora che tutto il middleware è configurato, aggiungiamo il nostro codice di gestione dei percorsi (precedentemente importato) alla catena di gestione delle richieste. Il codice importato definirà percorsi particolari per le diverse _parti_ del sito:

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
```

> [!NOTE]
> I percorsi specificati sopra (`"/"` e `"/users"`) sono trattati come prefisso per i percorsi definiti nei file importati. Quindi, ad esempio, se il modulo **users** importato definisce un percorso per `/profile`, accedi a quel percorso su `/users/profile`. Parleremo più dei percorsi in un articolo successivo.

L'ultimo middleware nel file aggiunge i metodi gestori per gli errori e le risposte HTTP 404.

```js
// catch 404 and forward to error handler
app.use((req, res, next) => {
  next(createError(404));
});

// error handler
app.use((err, req, res, next) => {
  // set locals, only providing error in development
  res.locals.message = err.message;
  res.locals.error = req.app.get("env") === "development" ? err : {};

  // render the error page
  res.status(err.status || 500);
  res.render("error");
});
```

L'oggetto applicazione Express (app) è ora completamente configurato. L'ultimo passaggio è aggiungerlo alle esportazioni del modulo (questo è ciò che permette che venga importato da **/bin/www**).

```js
module.exports = app;
```

### Percorsi

Il file del percorso **/routes/users.js** è mostrato di seguito (i file di percorso condividono una struttura simile, quindi non abbiamo bisogno di mostrare anche **index.js**). Prima, carica il modulo _express_ e lo usa per ottenere un oggetto `express.Router`. Poi specifica un percorso su quell'oggetto e infine esporta il router dal modulo (questo è ciò che permette al file di essere importato in **app.js**).

```js
const express = require("express");

const router = express.Router();

/* GET users listing. */
router.get("/", (req, res, next) => {
  res.send("respond with a resource");
});

module.exports = router;
```

Il percorso definisce un callback che verrà invocato ogni volta che viene rilevata una richiesta HTTP `GET` con il pattern corretto. Il pattern corrispondente è il percorso specificato quando il modulo viene importato (`"/users"`) più quello definito in questo file (`"/"`). In altre parole, questo percorso sarà utilizzato quando viene ricevuto un URL di `/users/`.

> [!NOTE]
> Prova questo eseguendo il server con node e visitando l'URL nel tuo browser: `http://localhost:3000/users/`. Dovresti vedere un messaggio: 'rispondi con una risorsa'.

Una cosa interessante sopra è che la funzione di callback ha il terzo argomento `next`, ed è quindi una funzione middleware piuttosto che un semplice callback di percorso. Anche se il codice attualmente non usa l'argomento `next`, potrebbe essere utile in futuro se vuoi aggiungere più gestori di percorsi al percorso `'/'`.

### Viste (template)

Le viste (template) sono conservate nella directory **/views** (come specificato in **app.js**) e hanno l'estensione del file **.pug**. Il metodo [`Response.render()`](https://expressjs.com/en/4x/api.html#res.render) è usato per eseguire il rendering di un template specificato insieme ai valori delle variabili nominate passate in un oggetto, e poi inviare il risultato come risposta. Nel codice sottostante da **/routes/index.js** puoi vedere come quel percorso esegue il rendering di una risposta usando il template "index" passando la variabile del template "title".

```js
/* GET home page. */
router.get("/", (req, res, next) => {
  res.render("index", { title: "Express" });
});
```

Il template corrispondente per il percorso sopra è dato di seguito (**index.pug**). Parleremo più della sintassi più tardi. Tutto ciò che devi sapere per ora è che la variabile `title` (con valore `'Express'`) viene inserita dove specificato nel template.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Sfida te stesso

Crea un nuovo percorso in **/routes/users.js** che mostrerà il testo "_You're so cool_" all'URL `/users/cool/`. Provalo eseguendo il server e visitando `http://localhost:3000/users/cool/` nel tuo browser.

## Riassunto

Hai ora creato un progetto di sito web scheletro per la [Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) e verificato che funziona usando _node_. Soprattutto, comprendi anche come è strutturato il progetto, quindi hai una buona idea di dove dobbiamo fare cambiamenti per aggiungere percorsi e viste per la nostra biblioteca locale.

Successivamente, inizieremo a modificare lo scheletro in modo che funzioni come un sito web di biblioteca.

## Vedi anche

- [Express application generator](https://expressjs.com/en/starter/generator.html) (documentazione Express)
- [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
