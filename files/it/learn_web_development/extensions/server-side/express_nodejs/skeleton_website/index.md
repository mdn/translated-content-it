---
title: "Tutorial su Express - Parte 2: Creare un sito web scheletro"
short-title: "2: Sito web scheletro"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website
l10n:
  sourceCommit: afcdfa050626bb7eb05ee693df8997020db9ff2e
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo secondo articolo del [Tutorial su Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) mostra come creare un progetto di sito web "scheletro", che potrà poi essere popolato con route, template/view e chiamate al database specifici del sito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">Configurare un ambiente di sviluppo Node</a>.
          Consultare il Tutorial su Express.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di avviare nuovi progetti di siti web usando <em>Express Application Generator</em>.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Questo articolo mostra come creare un sito web "scheletro" usando lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator/), che potrà poi essere popolato con route, view/template e chiamate al database specifici del sito. In questo caso, useremo lo strumento per creare la struttura del nostro [sito web Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), al quale aggiungeremo in seguito tutto il restante codice necessario al sito. Il processo è estremamente semplice: richiede soltanto di invocare il generatore dalla riga di comando con il nome di un nuovo progetto, specificando facoltativamente anche il template engine e il generatore CSS del sito.

Le sezioni seguenti mostrano come chiamare il generatore dell'applicazione e forniscono una breve spiegazione delle diverse opzioni per view/CSS. Verrà inoltre spiegata la struttura del sito web scheletro. Alla fine, verrà mostrato come eseguire il sito web per verificarne il funzionamento.

> [!NOTE]
>
> - _Express Application Generator_ non è l'unico generatore per applicazioni Express e il progetto generato non è l'unico modo valido per strutturare file e directory. Il sito generato ha tuttavia una struttura modulare, facile da estendere e comprendere. Per informazioni su un'applicazione Express _minimale_, vedere [Esempio Hello world](https://expressjs.com/en/starter/hello-world/) (documentazione di Express).
> - _Express Application Generator_ dichiara la maggior parte delle variabili usando `var`.
>   Nel tutorial, la maggior parte di queste dichiarazioni è stata modificata in [`const`](/it/docs/Web/JavaScript/Reference/Statements/const) (e alcune in [`let`](/it/docs/Web/JavaScript/Reference/Statements/let)), per mostrare le pratiche moderne di JavaScript.
> - Questo tutorial usa la versione di _Express_ e delle altre dipendenze definite nel file **package.json** creato da _Express Application Generator_.
>   Queste non sono necessariamente le versioni più recenti e dovrebbero essere aggiornate durante il deployment di un'applicazione reale in produzione.

## Usare il generatore dell'applicazione

Il generatore dovrebbe essere già stato installato durante la [configurazione di un ambiente di sviluppo Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment). Come breve promemoria, lo strumento generatore si installa a livello di sistema usando il gestore di pacchetti npm, come mostrato:

```bash
npm install express-generator -g
```

Il generatore dispone di diverse opzioni, visualizzabili dalla riga di comando usando il comando `--help` (o `-h`):

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

È possibile specificare a express di creare un progetto nella directory _corrente_ usando il view engine _Jade_ e CSS semplice (se viene specificato un nome di directory, il progetto verrà creato in una sottocartella con tale nome).

```bash
express
```

È inoltre possibile scegliere un view engine (template) usando `--view` e/o un motore di generazione CSS usando `--css`.

> [!NOTE]
> Le altre opzioni per scegliere i template engine (ad esempio `--hogan`, `--ejs`, `--hbs` ecc.) sono deprecate. Usare `--view` (oppure `-v`).

### Quale view engine usare?

_Express Application Generator_ consente di configurare diversi popolari view/templating engine, inclusi [EJS](https://www.npmjs.com/package/ejs), [Hbs](https://github.com/pillarjs/hbs), [Pug](https://pugjs.org/api/getting-started.html) (Jade), [Twig](https://www.npmjs.com/package/twig) e [Vash](https://www.npmjs.com/package/vash), anche se seleziona Jade per impostazione predefinita quando non viene specificata un'opzione per la view. Express stesso può inoltre supportare direttamente numerosi altri linguaggi di templating [out of the box](https://github.com/expressjs/express/wiki#template-engines).

> [!NOTE]
> Per usare un template engine non supportato dal generatore, consultare [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines/) (documentazione di Express) e la documentazione del view engine desiderato.

In generale, è opportuno selezionare un templating engine che offra tutte le funzionalità necessarie e permetta di essere produttivi più rapidamente, ovvero nello stesso modo in cui viene scelto qualsiasi altro componente. Alcuni aspetti da considerare nel confronto tra template engine:

- Tempo necessario per essere produttivi — Se il team ha già esperienza con un linguaggio di templating, è probabile che possa essere produttivo più velocemente usando quel linguaggio. In caso contrario, occorre considerare la relativa curva di apprendimento dei template engine candidati.
- Popolarità e attività — Verificare la popolarità del motore e l'esistenza di una community attiva. È importante poter ottenere supporto quando emergono problemi durante il ciclo di vita del sito web.
- Stile — Alcuni template engine usano markup specifico per indicare il contenuto inserito all'interno di HTML "ordinario", mentre altri costruiscono l'HTML usando una sintassi differente, ad esempio con rientri e nomi di blocchi.
- Prestazioni/tempo di rendering.
- Funzionalità — Occorre valutare se i motori presi in considerazione offrono le seguenti funzionalità:
  - Ereditarietà del layout: consente di definire un template di base e poi "ereditare" soltanto le parti che devono essere diverse per una pagina particolare. Questo è in genere un approccio migliore rispetto a costruire template includendo vari componenti richiesti o creare un template da zero ogni volta.
  - Supporto per "include": consente di costruire template includendo altri template.
  - Sintassi concisa per variabili e controllo dei cicli.
  - Possibilità di filtrare i valori delle variabili a livello di template, ad esempio rendendo le variabili in maiuscolo o formattando un valore data.
  - Possibilità di generare formati di output diversi dall'HTML, come JSON o XML.
  - Supporto per operazioni asincrone e streaming.
  - Funzionalità lato client. Se un templating engine può essere usato sul client, è possibile eseguire tutto o gran parte del rendering lato client.

> [!NOTE]
> Esistono molte risorse su Internet per aiutare a confrontare le diverse opzioni.

Per questo progetto verrà usato il templating engine [Pug](https://pugjs.org/api/getting-started.html) (in precedenza denominato "Jade"), poiché è uno dei linguaggi di templating più diffusi per Express/JavaScript ed è supportato direttamente dal generatore.

### Quale motore di fogli di stile CSS usare?

_Express Application Generator_ consente di creare un progetto configurato per usare i motori di fogli di stile CSS più comuni: [LESS](https://lesscss.org/), [SASS](https://sass-lang.com/), [Stylus](https://stylus-lang.com/).

> [!NOTE]
> CSS presenta alcune limitazioni che rendono difficili determinate attività. I motori di fogli di stile CSS consentono di usare una sintassi più potente per definire il CSS e quindi compilare la definizione in CSS tradizionale da usare nei browser.

Come per i templating engine, occorre usare il motore di fogli di stile che consente al team di essere più produttivo. Per questo progetto verrà usato CSS vanilla, ovvero l'impostazione predefinita, poiché i requisiti CSS non sono abbastanza complessi da giustificare l'uso di altro.

### Quale database usare?

Il codice generato non usa né include database. Le app _Express_ possono usare qualsiasi [meccanismo di database](https://expressjs.com/en/guide/database-integration/) supportato da _Node_ (_Express_ non definisce alcun comportamento o requisito aggiuntivo specifico per la gestione dei database).

L'integrazione con un database verrà affrontata in un articolo successivo.

## Creare il progetto

Per l'app di esempio _Local Library_ che verrà realizzata, verrà creato un progetto denominato _express-locallibrary-tutorial_ usando la libreria di template _Pug_ e nessun motore CSS.

Per prima cosa, passare alla posizione in cui si desidera creare il progetto, quindi eseguire _Express Application Generator_ nel prompt dei comandi come mostrato:

```bash
express express-locallibrary-tutorial --view=pug
```

Il generatore creerà ed elencherà i file del progetto.

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
     > $env:DEBUG = "express-locallibrary-tutorial:*"; npm start

   run the app (Command Prompt (Windows)):
     > SET DEBUG=express-locallibrary-tutorial:* & npm start
```

Alla fine dell'output, il generatore fornisce istruzioni su come installare le dipendenze, elencate nel file **package.json**, e come eseguire l'applicazione su diversi sistemi operativi.

> [!NOTE]
> I file creati dal generatore definiscono tutte le variabili come `var`.
> Aprire tutti i file generati e modificare le dichiarazioni `var` in `const` prima di continuare (il resto del tutorial presuppone che questa operazione sia stata eseguita).

## Eseguire il sito web scheletro

A questo punto, è disponibile un progetto scheletro completo. Il sito web non _fa_ ancora molto, ma vale la pena eseguirlo per dimostrare che funziona.

1. Per prima cosa, installare le dipendenze (il comando `install` recupererà tutti i pacchetti di dipendenza elencati nel file **package.json** del progetto).

   ```bash
   cd express-locallibrary-tutorial
   npm install
   ```

2. Quindi eseguire l'applicazione.
   - Nel prompt CMD di Windows, usare questo comando:

     ```batch
     SET DEBUG=express-locallibrary-tutorial:* & npm start
     ```

   - In Windows PowerShell, usare questo comando:

     ```powershell
     $env:DEBUG = "express-locallibrary-tutorial:*"; npm start
     ```

     > [!NOTE]
     > I comandi PowerShell non sono trattati in questo tutorial (i comandi "Windows" forniti presuppongono l'uso del prompt CMD di Windows).

   - Su macOS o Linux, usare questo comando:

     ```bash
     DEBUG=express-locallibrary-tutorial:* npm start
     ```

3. Quindi caricare `http://localhost:3000/` nel browser per accedere all'app.

Dovrebbe essere visualizzata una pagina del browser simile a questa:

![Browser per il sito web predefinito del generatore di app Express](expressgeneratorskeletonwebsite.png)

Congratulazioni! Ora è disponibile un'applicazione Express funzionante, accessibile tramite la porta 3000.

> [!NOTE]
> È possibile avviare l'app anche usando semplicemente il comando `npm start`. Specificare la variabile DEBUG come mostrato abilita il logging/debugging della console. Ad esempio, visitando la pagina precedente verrà visualizzato un output di debug simile a questo:
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

## Abilitare il riavvio del server alle modifiche dei file

Attualmente, le modifiche apportate al sito web Express non sono visibili finché il server non viene riavviato. Dover arrestare e riavviare il server ogni volta che viene apportata una modifica diventa rapidamente molto fastidioso, quindi vale la pena dedicare del tempo ad automatizzare il riavvio del server quando necessario.

Uno strumento pratico a questo scopo è [nodemon](https://github.com/remy/nodemon). Solitamente viene installato globalmente, poiché è uno "strumento", ma qui verrà installato e usato localmente come _dipendenza di sviluppo_, in modo che tutti gli sviluppatori che lavorano al progetto lo ottengano automaticamente quando installano l'applicazione. Usare il seguente comando nella directory radice del progetto scheletro:

```bash
npm install --save-dev nodemon
```

Se si sceglie comunque di installare [nodemon](https://github.com/remy/nodemon) globalmente sulla macchina, e non soltanto nel file **package.json** del progetto:

```bash
npm install -g nodemon
```

Aprendo il file **package.json** del progetto sarà ora visibile una nuova sezione con questa dipendenza:

```json
{
  "devDependencies": {
    "nodemon": "^3.1.10"
  }
}
```

Poiché lo strumento non è installato globalmente, non è possibile avviarlo dalla riga di comando, a meno di aggiungerlo al percorso. Tuttavia, è possibile chiamarlo da uno script npm perché npm conosce i pacchetti installati. Individuare la sezione `scripts` del file **package.json**. Inizialmente, conterrà una riga che inizia con `"start"`. Aggiornarla aggiungendo una virgola alla fine di quella riga e inserendo le righe `"devstart"` e `"serverstart"`:

- Su Linux e macOS, la sezione degli script sarà simile a questa:

  ```json
  {
    "scripts": {
      "start": "node ./bin/www",
      "devstart": "nodemon ./bin/www",
      "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
    }
  }
  ```

- Su Windows, il valore di "serverstart" sarà invece simile a questo, se si usa il prompt dei comandi:

  ```bash
  "serverstart": "SET DEBUG=express-locallibrary-tutorial:* & npm run devstart"
  ```

Ora è possibile avviare il server quasi esattamente come in precedenza, ma usando il comando `devstart`.

> [!NOTE]
> Ora, modificando qualsiasi file nel progetto, il server verrà riavviato. In alternativa, può essere riavviato in qualsiasi momento digitando `rs` nel prompt dei comandi. Sarà comunque necessario ricaricare il browser per aggiornare la pagina.
>
> Ora è necessario chiamare `npm run <script-name>` anziché soltanto `npm start`, perché "start" è in realtà un comando npm associato allo script con quel nome. Sarebbe stato possibile sostituire il comando nello script _start_, ma _nodemon_ deve essere usato soltanto durante lo sviluppo, quindi ha senso creare un nuovo comando script.
>
> Il comando `serverstart` aggiunto agli script nel file **package.json** precedente è un ottimo esempio. Usando questo approccio non è più necessario digitare un comando lungo per avviare il server. Si noti che il comando particolare aggiunto allo script funziona soltanto su macOS o Linux.

## Il progetto generato

Diamo ora un'occhiata al progetto appena creato.
Durante il percorso verranno apportate alcune piccole modifiche.

### Struttura delle directory

Il progetto generato, dopo aver installato le dipendenze, ha la seguente struttura di file; i file sono gli elementi **non** preceduti da "/".
Il file **package.json** definisce le dipendenze dell'applicazione e altre informazioni.
Definisce inoltre uno script di avvio che chiamerà il punto di ingresso dell'applicazione, il file JavaScript **/bin/www**.
Questo configura parte della gestione degli errori dell'applicazione e poi carica **app.js** per svolgere il resto del lavoro.
Le route dell'app sono memorizzate in moduli separati nella directory **routes/**.
I template sono memorizzati nella directory /**views**.

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

Le sezioni seguenti descrivono i file in modo più dettagliato.

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
    "nodemon": "^3.1.10"
  }
}
```

La sezione scripts definisce innanzitutto uno script "_start_", invocato quando viene chiamato `npm start` per avviare il server; questo script è stato aggiunto da _Express Application Generator_. Dalla definizione dello script è possibile vedere che esso avvia effettivamente il file JavaScript **./bin/www** con _node_.

Questa sezione è già stata modificata in [Abilitare il riavvio del server alle modifiche dei file](#abilitare_il_riavvio_del_server_alle_modifiche_dei_file) aggiungendo gli script _devstart_ e _serverstart_.
Questi possono essere usati per avviare lo stesso file **./bin/www** con _nodemon_ anziché _node_; questa versione degli script è per Linux e macOS, come discusso sopra.

```json
{
  "scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www",
    "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
  }
}
```

Le dipendenze includono il pacchetto _express_ e il pacchetto per il view engine selezionato, _pug_.
Inoltre, sono disponibili i seguenti pacchetti, utili in molte applicazioni web:

- [cookie-parser](https://www.npmjs.com/package/cookie-parser): usato per analizzare l'header dei cookie e popolare `req.cookies`, fornendo essenzialmente un metodo pratico per accedere alle informazioni sui cookie.
- [debug](https://www.npmjs.com/package/debug): una piccola utility di debugging per node modellata sulla tecnica di debugging del core di node.
- [morgan](https://www.npmjs.com/package/morgan): un middleware di logging delle richieste HTTP per node.
- [http-errors](https://www.npmjs.com/package/http-errors): crea errori HTTP dove necessario, per la gestione degli errori di express.

Le versioni predefinite nel progetto generato sono leggermente obsolete.
Sostituire la sezione delle dipendenze del file `package.json` con il testo seguente, che specifica le versioni più recenti di queste librerie al momento della scrittura:

```json
{
  "dependencies": {
    "cookie-parser": "^1.4.7",
    "debug": "^4.4.1",
    "express": "^5.1.0",
    "http-errors": "~2.0.0",
    "morgan": "^1.10.0",
    "pug": "3.0.3"
  }
}
```

Quindi aggiornare le dipendenze installate usando il comando:

```bash
npm install
```

> [!NOTE]
> È consigliabile aggiornare regolarmente alle versioni compatibili più recenti delle librerie di dipendenza; questa operazione può anche essere eseguita automaticamente o semi-automaticamente come parte di una configurazione di {{Glossary("continuous_integration", "integrazione continua")}}.
>
> Generalmente, gli aggiornamenti di librerie alle versioni minor e patch restano compatibili.
> Ciascuna versione precedente è stata prefissata con `^`, in modo da poter aggiornare automaticamente alla versione `minor.patch` più recente eseguendo:
>
> ```bash
> npm update --save
> ```
>
> Le versioni major modificano la compatibilità.
> Per tali aggiornamenti sarà necessario aggiornare manualmente il file `package.json` e il codice che usa la libreria, quindi testare nuovamente il progetto in modo approfondito.

### File www

Il file **/bin/www** è il punto di ingresso dell'applicazione. La prima operazione che esegue è `require()` del punto di ingresso dell'applicazione "reale", **app.js** nella radice del progetto, che configura e restituisce l'oggetto applicazione [`express()`](https://expressjs.com/en/api/).
`require()` è il [modo CommonJS](https://nodejs.org/api/modules.html) per importare codice JavaScript, JSON e altri file nel file corrente.
Qui viene specificato il modulo **app.js** usando un percorso relativo e omettendo l'estensione file opzionale (.**js**).

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

const app = require("../app");
```

> [!NOTE]
> Node.js 14 e versioni successive supportano le istruzioni ES6 `import` per importare moduli JavaScript (ECMAScript).
> Per usare questa funzionalità, occorre aggiungere `"type": "module"` al file **package.json** di Express, tutti i moduli dell'applicazione devono usare `import` anziché `require()` e, per gli _import relativi_, deve essere inclusa l'estensione del file. Per ulteriori informazioni, vedere la [documentazione di Node](https://nodejs.org/api/esm.html#introduction).
> Sebbene l'uso di `import` offra vantaggi, questo tutorial usa `require()` per essere conforme alla [documentazione di Express](https://expressjs.com/en/starter/hello-world/).

Il resto del codice in questo file configura un server HTTP node con `app` impostato su una porta specifica, definita in una variabile d'ambiente o 3000 se la variabile non è definita, quindi avvia l'ascolto e la segnalazione di errori e connessioni del server. Per ora non è necessario conoscere altro sul codice; tutto ciò che è contenuto in questo file è "boilerplate". Tuttavia, è possibile esaminarlo liberamente se interessa.

### app.js

Questo file crea un oggetto applicazione `express`, denominato `app` per convenzione, configura l'applicazione con varie impostazioni e middleware, quindi esporta l'app dal modulo. Il codice seguente mostra soltanto le parti del file che creano ed esportano l'oggetto app:

```js
const express = require("express");

const app = express();
// …
module.exports = app;
```

Tornando al file del punto di ingresso **www** precedente, è questo oggetto `module.exports` a essere fornito al chiamante quando viene importato questo file.

Esaminiamo nel dettaglio il file **app.js**. Per prima cosa, vengono importate nel file alcune utili librerie node usando `require()`, incluse _http-errors_, _express_, _morgan_ e _cookie-parser_, scaricate in precedenza per l'applicazione usando npm, e _path_, una libreria Node core per analizzare percorsi di file e directory.

```js
const createError = require("http-errors");
const express = require("express");
const path = require("path");
const cookieParser = require("cookie-parser");
const logger = require("morgan");
```

Quindi vengono eseguiti `require()` dei moduli dalla directory routes. Questi moduli/file contengono codice per gestire particolari insiemi di "route" correlate, ovvero percorsi URL. Quando l'applicazione scheletro verrà estesa, ad esempio per elencare tutti i libri nella libreria, verrà aggiunto un nuovo file per gestire le route relative ai libri.

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
```

> [!NOTE]
> A questo punto il modulo è soltanto stato _importato_; le sue route non sono ancora state effettivamente usate, cosa che avviene poco più avanti nel file.

Successivamente, viene creato l'oggetto `app` usando il modulo _express_ importato, quindi viene usato per configurare il view engine, ovvero il template engine. La configurazione del motore avviene in due parti. Innanzitutto, viene impostato il valore `"views"` per specificare la cartella in cui saranno memorizzati i template, in questo caso la sottocartella **/views**. Quindi viene impostato il valore `"view engine"` per specificare la libreria di template, in questo caso "pug".

```js
const app = express();

// view engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Il gruppo successivo di funzioni chiama `app.use()` per aggiungere le librerie _middleware_ importate sopra alla catena di gestione delle richieste.
Ad esempio, `express.json()` e `express.urlencoded()` sono necessari per popolare [`req.body`](https://expressjs.com/en/api/#req.body) con i campi del modulo.
Dopo queste librerie viene usato anche il middleware `express.static`, che fa sì che _Express_ serva tutti i file statici nella directory **/public** nella radice del progetto.

```js
app.use(logger("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

app.use(express.static(path.join(__dirname, "public")));
```

Ora che tutti gli altri middleware sono configurati, viene aggiunto alla catena di gestione delle richieste il codice per la gestione delle route, importato in precedenza. Il codice importato definirà route particolari per le diverse _parti_ del sito:

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
```

> [!NOTE]
> I percorsi specificati sopra, `"/"` e `"/users"`, vengono trattati come prefisso delle route definite nei file importati.
> Quindi, ad esempio, se il modulo **users** importato definisce una route per `/profile`, tale route sarà accessibile in `/users/profile`. Le route verranno trattate più approfonditamente in un articolo successivo.

L'ultimo middleware nel file aggiunge metodi handler per gli errori e le risposte HTTP 404.

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

L'oggetto applicazione Express, app, è ora completamente configurato. L'ultimo passaggio consiste nell'aggiungerlo alle esportazioni del modulo; questo consente di importarlo da **/bin/www**.

```js
module.exports = app;
```

### Route

Di seguito è mostrato il file di route **/routes/users.js**; i file delle route condividono una struttura simile, quindi non è necessario mostrare anche **index.js**.
Per prima cosa carica il modulo _express_ e lo usa per ottenere un oggetto `express.Router`.
Quindi specifica una route su quell'oggetto e infine esporta il router dal modulo; questo consente al file di essere importato in **app.js**.

```js
const express = require("express");

const router = express.Router();

/* GET users listing. */
router.get("/", (req, res, next) => {
  res.send("respond with a resource");
});

module.exports = router;
```

La route definisce una callback che verrà invocata ogni volta che viene rilevata una richiesta HTTP `GET` con il pattern corretto. Il pattern corrispondente è la route specificata al momento dell'importazione del modulo, `"/users"`, più quanto definito in questo file, `"/"`. In altre parole, questa route verrà usata quando viene ricevuto un URL `/users/`.

> [!NOTE]
> Provare eseguendo il server con node e visitando l'URL nel browser: `http://localhost:3000/users/`. Dovrebbe essere visualizzato il messaggio: 'respond with a resource'.

Un aspetto interessante sopra è che la funzione callback ha come terzo argomento `next` ed è quindi una funzione middleware anziché una semplice callback di route. Sebbene il codice non usi attualmente l'argomento `next`, questo potrebbe essere utile in futuro se si desidera aggiungere più handler di route al percorso `'/'`.

### View (template)

Le view, ovvero i template, sono memorizzate nella directory **/views**, come specificato in **app.js**, e hanno l'estensione file **.pug**. Il metodo [`Response.render()`](https://expressjs.com/en/5x/api/#res.render) viene usato per eseguire il rendering di un template specificato insieme ai valori delle variabili nominate passate in un oggetto, quindi per inviare il risultato come risposta. Nel codice seguente da **/routes/index.js** è possibile vedere come quella route esegua il rendering di una risposta usando il template "index" e passando la variabile di template "title".

```js
/* GET home page. */
router.get("/", (req, res, next) => {
  res.render("index", { title: "Express" });
});
```

Il template corrispondente per la route precedente è riportato di seguito, **index.pug**. La sintassi verrà trattata più approfonditamente in seguito. Per ora è sufficiente sapere che la variabile `title`, con valore `'Express'`, viene inserita nel punto specificato nel template.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Mettiti alla prova

Creare una nuova route in **/routes/users.js** che visualizzi il testo "_You're so cool_" all'URL `/users/cool/`. Verificarla eseguendo il server e visitando `http://localhost:3000/users/cool/` nel browser.

## Riepilogo

È stato ora creato un progetto di sito web scheletro per [Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) e ne è stata verificata l'esecuzione usando _node_. Soprattutto, ora è chiara la struttura del progetto, quindi è possibile capire dove apportare le modifiche necessarie per aggiungere route e view alla libreria locale.

Successivamente, verrà iniziata la modifica dello scheletro affinché funzioni come sito web di una libreria.

## Vedi anche

- [Generatore di applicazioni Express](https://expressjs.com/en/starter/generator/) (documentazione di Express)
- [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines/) (documentazione di Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
