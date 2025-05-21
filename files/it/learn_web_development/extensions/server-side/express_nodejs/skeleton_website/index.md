---
title: "Tutorial Express Parte 2: Creazione di uno scheletro di sito web"
short-title: "2: Scheletro del sito web"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo secondo articolo del nostro [Tutorial Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) mostra come creare un progetto di sito web "scheletro" che puoi poi popolare con percorsi specifici per il sito, modelli/viste e chiamate al database.

> [!WARNING]
> Il tutorial Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Abbiamo in programma di aggiornare la documentazione nella seconda metà del 2025.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">Configurare un ambiente di sviluppo Node</a>. 
        Rivedere il Tutorial Express.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di avviare propri nuovi progetti di siti web utilizzando <em>Express Application Generator</em>.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Questo articolo mostra come è possibile creare un sito web "scheletro" utilizzando lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator.html), che puoi poi popolare con percorsi specifici per il sito, viste/modelli e chiamate al database. In questo caso, utilizzeremo lo strumento per creare la struttura per il nostro [sito web della Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), al quale successivamente aggiungeremo tutto il codice necessario al sito. Il processo è estremamente semplice, richiedendo solo di invocare il generatore sulla riga di comando con un nuovo nome di progetto, eventualmente specificando anche il motore di modelli del sito e il generatore CSS.

Le sezioni seguenti mostrano come chiamare il generatore di applicazioni e forniscono una piccola spiegazione sulle diverse opzioni di vista/CSS. Spiegheremo anche come è strutturato il sito web scheletro. Alla fine, mostreremo come è possibile eseguire il sito web per verificare che funzioni.

> [!NOTE]
>
> - L'_Express Application Generator_ non è l'unico generatore per applicazioni Express, e il progetto generato non è l'unico modo praticabile per strutturare i file e le directory. Tuttavia, il sito generato ha una struttura modulare che è facile da estendere e comprendere. Per informazioni su una applicazione Express _minimale_, vedere [Esempio Hello world](https://expressjs.com/en/starter/hello-world.html) (documentazione Express).
> - L'_Express Application Generator_ dichiara la maggior parte delle variabili utilizzando `var`.
>   Abbiamo modificato la maggior parte di queste in [`const`](/it/docs/Web/JavaScript/Reference/Statements/const) (e alcune in [`let`](/it/docs/Web/JavaScript/Reference/Statements/let)) nel tutorial, perché vogliamo dimostrare le pratiche moderne di JavaScript.
> - Questo tutorial utilizza la versione di _Express_ e delle altre dipendenze che sono definite nel **package.json** creato dall'_Express Application Generator_.
>   Queste non sono (necessariamente) l'ultima versione, e potresti volerle aggiornare quando implementi una vera applicazione in produzione.

## Utilizzo del generatore di applicazioni

Dovresti già aver installato il generatore come parte della [configurazione di un ambiente di sviluppo Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment). Come promemoria veloce, si installa lo strumento generatore a livello di sito utilizzando il gestore di pacchetti npm, come mostrato:

```bash
npm install express-generator -g
```

Il generatore ha una serie di opzioni, che puoi visualizzare sulla riga di comando utilizzando il comando `--help` (o `-h`):

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

Puoi specificare che express crei un progetto all'interno della directory _corrente_ utilizzando il motore di viste _Jade_ e CSS semplice (se specifichi un nome di directory allora il progetto sarà creato in una sottocartella con quel nome).

```bash
express
```

Puoi anche scegliere un motore per le viste (template) utilizzando `--view` e/o un motore di generazione CSS utilizzando `--css`.

> [!NOTE]
> Le altre opzioni per la scelta di motori di template (ad esempio, `--hogan`, `--ejs`, `--hbs` ecc.) sono deprecate. Usa `--view` (o `-v`).

### Quale motore di viste dovrei usare?

L'_Express Application Generator_ ti permette di configurare un numero di popolari motori di viste/template, inclusi [EJS](https://www.npmjs.com/package/ejs), [Hbs](https://github.com/pillarjs/hbs), [Pug](https://pugjs.org/api/getting-started.html) (Jade), [Twig](https://www.npmjs.com/package/twig), e [Vash](https://www.npmjs.com/package/vash), sebbene scelga Jade di default se non specifichi un'opzione di vista. Express stesso può anche supportare un gran numero di altri linguaggi di templating [out of the box](https://github.com/expressjs/express/wiki#template-engines).

> [!NOTE]
> Se vuoi usare un motore di template che non è supportato dal generatore allora consulta [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express) e la documentazione per il tuo motore di viste target.

In generale, dovresti selezionare un motore di templating che fornisca tutte le funzionalità di cui hai bisogno e ti permetta di essere produttivo più rapidamente — o in altre parole, nello stesso modo in cui scegli qualsiasi altro componente! Alcune delle cose da considerare quando si confrontano i motori di template:

- Tempo alla produttività — Se il tuo team ha già esperienza con un linguaggio di templating allora è probabile che siano produttivi più velocemente usando quel linguaggio. In caso contrario, dovresti considerare la curva di apprendimento relativa per i motori di template candidati.
- Popolarità e attività — Rivedi la popolarità del motore e se ha una comunità attiva. È importante poter ottenere supporto quando sorgono problemi durante il ciclo di vita del sito web.
- Stile — Alcuni motori di template utilizzano un markup specifico per indicare contenuti inseriti all'interno di HTML "ordinario", mentre altri costruiscono l'HTML utilizzando una sintassi differente (ad esempio, utilizzando indentazione e nomi di blocchi).
- Prestazioni/tempo di rendering.
- Funzionalità — dovresti considerare se i motori che esamini hanno le seguenti funzionalità disponibili:

  - Ereditarietà del layout: Permette di definire un template base e poi "ereditare" solo le parti di esso che vuoi siano differenti per una pagina particolare. Questo è tipicamente un approccio migliore rispetto a costruire template includendo un numero di componenti richiesti o costruendo un template da zero ogni volta.
  - Supporto "Include": Permette di costruire template includendo altri template.
  - Sintassi di controllo di variabili e loop concisa.
  - Capacità di filtrare i valori delle variabili a livello del template, come rendere le variabili in maiuscolo, o formattare un valore di data.
  - Capacità di generare formati di output diversi dall'HTML, come JSON o XML.
  - Supporto per operazioni asincrone e streaming.
  - Funzioni lato client. Se un motore di templating può essere utilizzato sul client, questo consente la possibilità di avere tutto o la maggior parte del rendering fatto lato client.

> [!NOTE]
> Ci sono molte risorse su Internet per aiutarti a confrontare le diverse opzioni!

Per questo progetto, utilizzeremo il motore di templating [Pug](https://pugjs.org/api/getting-started.html) (questo è il motore precedentemente noto come Jade), poiché è uno dei linguaggi di templating più popolari per Express/JavaScript ed è supportato direttamente dal generatore.

### Quale motore CSS dovrei usare?

L'_Express Application Generator_ ti permette di creare un progetto configurato per utilizzare i più comuni motori di fogli di stile CSS: [LESS](https://lesscss.org/), [SASS](https://sass-lang.com/), [Stylus](https://stylus-lang.com/).

> [!NOTE]
> CSS ha alcune limitazioni che rendono certi compiti difficili. I motori di fogli di stile CSS ti permettono di usare una sintassi più potente per definire il tuo CSS e poi compilare la definizione in semplice CSS per essere utilizzato dai browser.

Come per i motori di template, dovresti usare il motore di fogli di stile che permetterà al tuo team di essere più produttivo. Per questo progetto, utilizzeremo CSS puro (il default) poiché i nostri requisiti CSS non sono sufficientemente complicati da giustificare l'uso di qualcos'altro.

### Quale database dovrei usare?

Il codice generato non usa/ include alcun database. Le applicazioni _Express_ possono usare qualsiasi [meccanismo di database](https://expressjs.com/en/guide/database-integration.html) supportato da _Node_ (_Express_ stesso non definisce alcun comportamento/ requisito specifico aggiuntivo per la gestione del database).

Discuteremo come integrare un database in un articolo successivo.

## Creazione del progetto

Per l'app di esempio _Local Library_ che stiamo per costruire, creeremo un progetto chiamato _express-locallibrary-tutorial_ utilizzando la libreria template _Pug_ e nessun motore CSS.

Per prima cosa, spostati nella directory in cui vuoi creare il progetto e poi esegui l'_Express Application Generator_ nel prompt dei comandi come mostrato:

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

Alla fine dell'output, il generatore fornisce istruzioni su come installare le dipendenze (come elencato nel file **package.json**) e come eseguire l'applicazione su diversi sistemi operativi.

> [!NOTE]
> I file creati dal generatore definiscono tutte le variabili come `var`.
> Apri tutti i file generati e cambia le dichiarazioni `var` in `const` prima di continuare (il resto del tutorial presume che tu abbia fatto questo).

## Esecuzione del sito web scheletro

A questo punto, abbiamo un progetto scheletro completo. Il sito web non fa molto ancora, ma vale la pena eseguirlo per mostrare che funziona.

1. Innanzitutto, installa le dipendenze (il comando `install` scaricherà tutti i pacchetti di dipendenze elencati nel file **package.json** del progetto).

   ```bash
   cd express-locallibrary-tutorial
   npm install
   ```

2. Poi esegui l'applicazione.

   - Nel prompt CMD di Windows, usa questo comando:

     ```batch
     SET DEBUG=express-locallibrary-tutorial:* & npm start
     ```

   - In Windows PowerShell, usa questo comando:

     ```powershell
     ENV:DEBUG = "express-locallibrary-tutorial:*"; npm start
     ```

     > [!NOTE]
     > I comandi PowerShell non sono trattati in questo tutorial (i comandi "Windows" forniti presuppongono che si utilizzi il prompt CMD di Windows).

   - Su macOS o Linux, usa questo comando:

     ```bash
     DEBUG=express-locallibrary-tutorial:* npm start
     ```

3. Poi carica `http://localhost:3000/` nel tuo browser per accedere all'app.

Dovresti vedere una pagina del browser che appare così:

![Browser per sito web generato da Express app di default](expressgeneratorskeletonwebsite.png)

Congratulazioni! Ora hai un'applicazione Express funzionante che può essere accessibile attraverso la porta 3000.

> [!NOTE]
> Potresti anche avviare l'app usando solo il comando `npm start`. Specificare la variabile DEBUG come mostrato abilita il logging/la debug sulla console. Ad esempio, quando visiti la pagina sopra vedrai un output di debug come questo:
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

## Abilitare il riavvio del server al cambiare del file

Qualsiasi modifica apportata al tuo sito web Express attualmente non è visibile fino a che non riavvii il server. Diventa rapidamente molto irritante dover fermare e riavviare il server ogni volta che si apporta una modifica, quindi vale la pena di prendere il tempo per automatizzare il riavvio del server quando necessario.

Uno strumento conveniente a questo scopo è [nodemon](https://github.com/remy/nodemon). Questo viene solitamente installato globalmente (poiché è un "tool"), ma qui lo installeremo e utilizzeremo localmente come _developer dependency_, in modo che qualsiasi sviluppatore che lavora con il progetto lo ottenga automaticamente quando installa l'applicazione. Usa il seguente comando nella directory principale del progetto scheletro:

```bash
npm install --save-dev nodemon
```

Se scegli ancora di installare [nodemon](https://github.com/remy/nodemon) globalmente sulla tua macchina e non solo nel file **package.json** del tuo progetto:

```bash
npm install -g nodemon
```

Se apri il file **package.json** del tuo progetto ora vedrai una nuova sezione con questa dipendenza:

```json
 "devDependencies": {
    "nodemon": "^3.1.3"
}
```

Poiché lo strumento non è stato installato a livello globale non possiamo avviarlo dalla riga di comando (a meno che non lo aggiungiamo al percorso) ma possiamo chiamarlo da uno script npm perché npm conosce tutti i pacchetti installati. Trova la sezione `scripts` del tuo package.json. Inizialmente, conterrà una linea, che inizia con `"start"`. Aggiorna mettendo una virgola alla fine di quella linea, e aggiungendo le righe `"devstart"` e `"serverstart"`:

- Su Linux e macOS, la sezione scripts apparirà come segue:

  ```json
    "scripts": {
      "start": "node ./bin/www",
      "devstart": "nodemon ./bin/www",
      "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
    },
  ```

- Su Windows, il valore "serverstart" apparirà invece così (se si usa il prompt dei comandi):

  ```bash
  "serverstart": "SET DEBUG=express-locallibrary-tutorial:* & npm run devstart"
  ```

Ora possiamo avviare il server quasi esattamente come prima, ma usando il comando `devstart`.

> [!NOTE]
> Ora se si modifica qualsiasi file nel progetto, il server si riavvierà (o si può riavviare digitando `rs` sulla riga di comando in qualsiasi momento). Sarà comunque necessario ricaricare il browser per aggiornare la pagina.
>
> Ora dobbiamo chiamare `npm run <script-name>` piuttosto che solo `npm start`, perché "start" è effettivamente un comando npm che è mappato allo script denominato. Avremmo potuto sostituire il comando nello script _start_, ma vogliamo utilizzare _nodemon_ solo durante lo sviluppo, quindi ha senso creare un nuovo comando script.
>
> Il comando `serverstart` aggiunto agli script nel **package.json** sopra è un ottimo esempio. Usando questo approccio non devi più digitare un comando lungo per avviare il server. Nota che il comando particolare aggiunto allo script funziona solo per macOS o Linux.

## Il progetto generato

Diamo ora un'occhiata al progetto che abbiamo appena creato.

### Struttura della directory

Il progetto generato, ora che hai installato le dipendenze, ha la seguente struttura di file (i file sono gli elementi **non** preceduti da "/"). Il file **package.json** definisce le dipendenze dell'applicazione e altre informazioni. Definisce anche uno script di avvio che chiamerà il punto di ingresso dell'applicazione, il file JavaScript **/bin/www**. Questo imposta alcuni dei gestori di errore dell'applicazione e poi carica **app.js** per fare il resto del lavoro. Le rotte dell'app sono memorizzate in moduli separati sotto la directory **routes/**. I template sono memorizzati sotto la directory **/views**.

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

La sezione degli script definisce prima uno script "_start_", che è ciò che stiamo invocando quando chiamiamo `npm start` per avviare il server (questo script è stato aggiunto dall'_Express Application Generator_). Dalla definizione dello script, puoi vedere che questo effettivamente avvia il file JavaScript **./bin/www** con _node_.

Abbiamo già modificato questa sezione in [Abilitare il riavvio del server al cambiare del file](#abilitare_il_riavvio_del_server_al_cambiare_del_file) aggiungendo gli script _devstart_ e _serverstart_. Questi possono essere utilizzati per avviare lo stesso file **./bin/www** con _nodemon_ piuttosto che _node_ (questa versione degli script è per Linux e macOS, come discusso sopra).

```json
  "scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www",
    "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
  },
```

Le dipendenze includono il pacchetto _express_ e il pacchetto per il nostro motore di viste selezionato (_pug_). Inoltre, abbiamo i seguenti pacchetti che sono utili in molte applicazioni web:

- [cookie-parser](https://www.npmjs.com/package/cookie-parser): Usato per analizzare l'intestazione dei cookie e popolare `req.cookies` (fornisce essenzialmente un metodo conveniente per accedere alle informazioni sui cookie).
- [debug](https://www.npmjs.com/package/debug): Una piccola utilità di debug per node modellata secondo la tecnica di debugging del core di node.
- [morgan](https://www.npmjs.com/package/morgan): Un middleware logger di richieste HTTP per node.
- [http-errors](https://www.npmjs.com/package/http-errors): Crea errori HTTP quando necessario (per la gestione degli errori di express).

Le versioni predefinite nel progetto generato sono un po' datate. Sostituisci la sezione delle dipendenze del tuo file `package.json` con il seguente testo, che specifica le versioni più recenti di queste librerie al momento della scrittura:

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

Poi aggiorna le dipendenze installate utilizzando il comando:

```bash
npm install
```

> [!NOTE]
> È una buona idea aggiornare regolarmente alle ultime versioni compatibili delle librerie di dipendenza - questo può persino essere fatto automaticamente o semi-automaticamente come parte di una configurazione di integrazione continua.
>
> Di solito gli aggiornamenti delle librerie alla versione minore e patch rimangono compatibili.
> Abbiamo prefissato ciascuna versione con `^` sopra in modo da poter aggiornare automaticamente l'ultima versione `minor.patch` eseguendo:
>
> ```bash
> npm update --save
> ```
>
> Le versioni principali cambiano la compatibilità. Per quegli aggiornamenti dovremo aggiornare manualmente il `package.json` e il codice che utilizza la libreria, e rieseguire ampiamente i test del progetto.

### File www

Il file **/bin/www** è il punto di ingresso all'applicazione! La primissima cosa che fa è `require()` il "vero" punto di ingresso dell'applicazione (**app.js**, nella root del progetto) che imposta e restituisce l'oggetto dell'applicazione [`express()`](https://expressjs.com/en/api.html). `require()` è il modo [CommonJS way](https://nodejs.org/api/modules.html) di importare codice JavaScript, JSON e altri file nel file corrente. Qui specifichiamo il modulo **app.js** utilizzando un percorso relativo e omettiamo l'estensione facoltativa (.**js**) del file.

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

const app = require("../app");
```

> [!NOTE]
> Node.js 14 e versioni successive supportano le dichiarazioni `import` ES6 per importare moduli JavaScript (ECMAScript). Per usare questa funzione devi aggiungere `"type": "module",` al tuo file Express **package.json**, tutti i moduli nella tua applicazione devono usare `import` piuttosto che `require()`, e per _importazioni relative_ è necessario includere l'estensione del file (per maggiori informazioni vedi la [documentazione Node](https://nodejs.org/api/esm.html#introduction)).
> Sebbene ci siano vantaggi nell'usare `import`, questo tutorial utilizza `require()` per corrispondere alla [documentazione di Express](https://expressjs.com/en/starter/hello-world.html).

Il resto del codice in questo file imposta un server HTTP node con `app` impostato su una specifica porta (definita in una variabile di ambiente o 3000 se la variabile non è definita), e inizia ad ascoltare e segnalare errori del server e connessioni. Per ora non hai veramente bisogno di sapere nient'altro sul codice (tutto in questo file è "boilerplate"), ma sentiti libero di rivederlo se sei interessato.

### app.js

Questo file crea un oggetto applicazione `express` (chiamato `app`, per convenzione), configura l'applicazione con varie impostazioni e middleware, e poi esporta l'app dal modulo. Il codice sotto mostra solo le parti del file che creano ed esportano l'oggetto app:

```js
const express = require("express");
const app = express();
// …
module.exports = app;
```

Nel file punto di ingresso **www** sopra, è questo oggetto `module.exports` che viene fornito al chiamante quando questo file è importato.

Esaminiamo il file **app.js** in dettaglio. Prima, importiamo alcune librerie node utili nel file utilizzando `require()`, inclusi _http-errors_, _express_, _morgan_ e _cookie-parser_ che abbiamo precedentemente scaricato per la nostra applicazione usando npm; e _path_, che è una libreria core di Node per analizzare percorsi di file e directory.

```js
const createError = require("http-errors");
const express = require("express");
const path = require("path");
const cookieParser = require("cookie-parser");
const logger = require("morgan");
```

Poi `require()` i moduli dalla nostra directory routes. Questi moduli/file contengono codice per gestire particolari set di "routes" (percorsi URL) correlati. Quando estenderemo l'applicazione scheletro, ad esempio per elencare tutti i libri nella biblioteca, aggiungeremo un nuovo file per gestire le route correlate ai libri.

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
```

> [!NOTE]
> A questo punto, abbiamo solo _importato_ il modulo; non abbiamo ancora effettivamente utilizzato le sue route (questo avviene solo un po' più avanti nel file).

Successivamente, creiamo l'oggetto `app` usando il nostro modulo _express_ importato e poi lo utilizziamo per configurare il motore di viste (template). Ci sono due parti per configurare il motore. Prima, impostiamo il valore `"views"` per specificare la cartella in cui saranno memorizzati i template (in questo caso la sottocartella **/views**). Poi impostiamo il valore `"view engine"` per specificare la libreria di modelli (in questo caso "pug").

```js
const app = express();

// view engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Il prossimo set di funzioni chiama `app.use()` per aggiungere le librerie _middleware_ che abbiamo importato sopra nella catena di gestione delle richieste. Ad esempio, `express.json()` e `express.urlencoded()` sono necessari per popolare [`req.body`](https://expressjs.com/en/api.html#req.body) con i campi del modulo. Dopo queste librerie utilizziamo anche il middleware `express.static`, che fa sì che _Express_ serva tutti i file statici nella directory **/public** nella root del progetto.

```js
app.use(logger("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

app.use(express.static(path.join(__dirname, "public")));
```

Ora che tutti gli altri middleware sono configurati, aggiungiamo il nostro codice di gestione delle route (precedentemente importato) alla catena di gestione delle richieste. Il codice importato definirà particolari route per le diverse _parti_ del sito:

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
```

> [!NOTE]
> I percorsi specificati sopra (`"/"` e `"/users"`) sono trattati come un prefisso per le route definite nei file importati. Quindi, ad esempio, se il modulo **users** importato definisce una route per `/profile`, accederesti a quella route su `/users/profile`. Parleremo di più sulle route in un articolo successivo.

L'ultimo middleware nel file aggiunge metodi di gestione per errori e risposte HTTP 404.

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

L'oggetto applicazione Express (app) è ora completamente configurato. L'ultimo passaggio è aggiungerlo ai module exports (questo è ciò che consente di essere importato da **/bin/www**).

```js
module.exports = app;
```

### Routes

Il file di route **/routes/users.js** è mostrato di seguito (i file di route condividono una struttura simile, quindi non abbiamo bisogno di mostrare anche **index.js**). Prima, carica il modulo _express_ e lo utilizza per ottenere un oggetto `express.Router`. Poi specifica una route su quell'oggetto e, infine, esporta il router dal modulo (questo è ciò che consente al file di essere importato in **app.js**).

```js
const express = require("express");
const router = express.Router();

/* GET users listing. */
router.get("/", (req, res, next) => {
  res.send("respond with a resource");
});

module.exports = router;
```

La route definisce un callback che verrà invocato ogni volta che viene rilevata una richiesta HTTP `GET` con il pattern corretto. Il pattern corrisponde alla route specificata quando il modulo è importato (`"/users"`) più qualunque cosa sia definita in questo file (`"/"`). In altre parole, questa route sarà utilizzata quando viene ricevuto un URL di `/users/`.

> [!NOTE]
> Prova questo avviando il server con node e visitando l'URL nel tuo browser: `http://localhost:3000/users/`. Dovresti vedere un messaggio: 'respond with a resource'.

Una cosa di interesse sopra è che la funzione di callback ha il terzo argomento `next`, ed è quindi una funzione middleware piuttosto che un semplice callback di route. Sebbene il codice attualmente non usi l'argomento `next`, potrebbe essere utile in futuro se vuoi aggiungere più gestori di route al percorso `'/'`.

### Views (template)

Le viste (template) sono memorizzate nella directory **/views** (come specificato in **app.js**) e hanno l'estensione file **.pug**. Il metodo [`Response.render()`](https://expressjs.com/en/4x/api.html#res.render) viene utilizzato per renderizzare un template specificato insieme ai valori delle variabili denominate passate in un oggetto, e poi inviare il risultato come risposta. Nel codice sotto da **/routes/index.js** puoi vedere come quella route renderizza una risposta utilizzando il template "index" passando la variabile di template "title".

```js
/* GET home page. */
router.get("/", (req, res, next) => {
  res.render("index", { title: "Express" });
});
```

Il template corrispondente per la route sopra è dato di seguito (**index.pug**). Parleremo più della sintassi in seguito. Tutto quello che devi sapere per ora è che la variabile `title` (con valore `'Express'`) è inserita dove specificato nel template.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Sfida personale

Crea una nuova route in **/routes/users.js** che visualizzerà il testo "_You're so cool_" all'URL `/users/cool/`. Testa eseguendo il server e visitando `http://localhost:3000/users/cool/` nel tuo browser.

## Sommario

Hai ora creato un progetto di sito web scheletro per la [Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) e verificato che funzioni utilizzando _node_. Ancora più importante, comprendi anche come è strutturato il progetto, quindi hai una buona idea di dove dobbiamo fare modifiche per aggiungere le route e le viste per la nostra biblioteca locale.

Successivamente, inizieremo a modificare lo scheletro in modo che funzioni come un sito web di biblioteca.

## Vedi anche

- [Express application generator](https://expressjs.com/en/starter/generator.html) (documentazione Express)
- [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
