---
title: "Tutorial di Express Parte 2: Creazione di un sito web scheletro"
short-title: "2: Sito web scheletro"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website
l10n:
  sourceCommit: cb25e0acbd9f0af27c4a99965cb962230d49a35d
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo secondo articolo nel nostro [Tutorial di Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) mostra come è possibile creare un progetto sito web "scheletro" che può essere quindi popolato con rotte, template/visualizzazioni e chiamate al database specifiche del sito.

> [!WARNING]
> Il tutorial di Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Abbiamo in programma di aggiornare la documentazione nella seconda metà del 2025.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">Impostare un ambiente di sviluppo Node</a>.
          Rivedere il Tutorial di Express.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di avviare i propri nuovi progetti web utilizzando l'<em>Express Application Generator</em>.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Questo articolo mostra come si può creare un sito web "scheletro" utilizzando lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator.html), che può essere poi popolato con percorsi specifici del sito, visualizzazioni/template e chiamate al database. In questo caso, utilizzeremo lo strumento per creare l'impalcatura per il nostro [sito web Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), al quale successivamente aggiungeremo tutto il codice necessario per il sito. Il processo è estremamente semplice, richiedendo solo di invocare il generatore sulla riga di comando con un nuovo nome di progetto, specificando eventualmente anche il motore di template del sito e il generatore CSS.

Le sezioni seguenti mostrano come chiamare il generatore di applicazione e forniscono una piccola spiegazione sulle diverse opzioni di visualizzazione/CSS. Spiegheremo anche come è strutturato il sito web scheletro. Alla fine, mostreremo come è possibile eseguire il sito web per verificare che funzioni.

> [!NOTE]
>
> - L'_Express Application Generator_ non è l'unico generatore per le applicazioni Express, e il progetto generato non è l'unico modo valido per strutturare i vostri file e directory. Tuttavia, il sito generato ha una struttura modulare che è facile da estendere e capire. Per informazioni su un'applicazione Express _minimale_, vedere l'[esempio Hello world](https://expressjs.com/en/starter/hello-world.html) (documentazione Express).
> - L'_Express Application Generator_ dichiara la maggior parte delle variabili utilizzando `var`.
>   Abbiamo cambiato la maggior parte di queste in [`const`](/it/docs/Web/JavaScript/Reference/Statements/const) (e alcune in [`let`](/it/docs/Web/JavaScript/Reference/Statements/let)) nel tutorial, perché vogliamo dimostrare la pratica moderna di JavaScript.
> - Questo tutorial utilizza la versione di _Express_ e altre dipendenze che sono definite nel **package.json** creato dall'_Express Application Generator_.
>   Queste non sono (necessariamente) le ultime versioni, e potreste volerle aggiornare quando si distribuisce un'applicazione reale in produzione.

## Utilizzo del generatore di applicazione

Dovreste già aver installato il generatore come parte dell'[impostazione di un ambiente di sviluppo Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment). Come promemoria rapido, si installa lo strumento generatore a livello di sito utilizzando il gestore di pacchetti npm, come mostrato:

```bash
npm install express-generator -g
```

Il generatore ha un certo numero di opzioni, che è possibile visualizzare sulla riga di comando utilizzando il comando `--help` (o `-h`):

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

Si può specificare express per creare un progetto all'interno della directory _corrente_ utilizzando il motore di visualizzazione _Jade_ e CSS semplice (se si specifica un nome di directory, il progetto sarà creato in una sottocartella con quel nome).

```bash
express
```

È possibile scegliere anche un motore di visualizzazione (template) utilizzando `--view` e/o un motore di generazione CSS utilizzando `--css`.

> [!NOTE]
> Le altre opzioni per la scelta dei motori di template (ad esempio, `--hogan`, `--ejs`, `--hbs` ecc.) sono deprecate. Utilizzare `--view` (o `-v`).

### Quale motore di visualizzazione dovrei usare?

L'_Express Application Generator_ consente di configurare un numero di motori di visualizzazione/template popolari, tra cui [EJS](https://www.npmjs.com/package/ejs), [Hbs](https://github.com/pillarjs/hbs), [Pug](https://pugjs.org/api/getting-started.html) (Jade), [Twig](https://www.npmjs.com/package/twig) e [Vash](https://www.npmjs.com/package/vash), anche se sceglie Jade per impostazione predefinita se non si specifica un'opzione di visualizzazione. Express stesso può anche supportare un gran numero di altri linguaggi di templating [compatibili nativamente](https://github.com/expressjs/express/wiki#template-engines).

> [!NOTE]
> Se si desidera usare un motore di template che non è supportato dal generatore, vedere [Usare i motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express) e documentazione per il motore di visualizzazione target.

In generale, dovreste selezionare un motore di template che offra tutte le funzionalità di cui avete bisogno e vi permetta di essere produttivi rapidamente — in altre parole, nello stesso modo in cui scegliete qualsiasi altro componente! Alcune delle cose da considerare quando si confrontano i motori di template:

- Tempo alla produttività — Se il vostro team ha già esperienza con un linguaggio di templating, è probabile che sarà produttivo più velocemente utilizzando quel linguaggio. Se no, allora dovreste considerare la curva di apprendimento relativa per i motori di template candidati.
- Popolarità e attività — Verificate la popolarità del motore e se ha una comunità attiva. È importante poter ottenere supporto quando sorgono problemi durante la vita del sito web.
- Stile — Alcuni motori di template utilizzano una specifica sintassi per indicare il contenuto inserito all'interno di HTML "ordinario", mentre altri costruiscono l'HTML usando una sintassi diversa (ad esempio, utilizzando indentazione e nomi di blocco).
- Tempi di prestazione/rendering.
- Caratteristiche — dovreste considerare se i motori che state valutando hanno le seguenti caratteristiche disponibili:

  - Ereditarietà layout: Consente di definire un template di base e poi "ereditare" solo le parti di esso che si vogliono rendere diverse per una particolare pagina. Questo è tipicamente un approccio migliore rispetto alla costruzione di template includendo un numero di componenti richiesti o costruendo un template da zero ogni volta.
  - Supporto "Include": Consente di costruire template includendo altri template.
  - Sintassi concisa per controllo variabili e cicli.
  - Capacità di filtrare i valori delle variabili a livello di template, come rendere le variabili maiuscole, o formattare un valore di data.
  - Capacità di generare formati di output diversi da HTML, come JSON o XML.
  - Supporto per operazioni asincrone e streaming.
  - Caratteristiche lato client. Se un motore di templating può essere utilizzato sul client, ciò consente la possibilità di avere tutto o quasi tutto il rendering fatto sul lato client.

> [!NOTE]
> Ci sono molte risorse su Internet che aiutano a confrontare le diverse opzioni!

Per questo progetto, useremo il motore di templating [Pug](https://pugjs.org/api/getting-started.html) (questo è il motore Jade recentemente rinominato), poiché è uno dei linguaggi di templating più popolari per Express/JavaScript ed è supportato nativamente dal generatore.

### Quale motore di fogli di stile CSS dovrei usare?

L'_Express Application Generator_ consente di creare un progetto configurato per l'utilizzo dei più comuni motori di fogli di stile CSS: [LESS](https://lesscss.org/), [SASS](https://sass-lang.com/), [Stylus](https://stylus-lang.com/).

> [!NOTE]
> CSS ha alcune limitazioni che rendono certi compiti difficili. I motori di fogli di stile CSS ti permettono di utilizzare una sintassi più potente per definire i tuoi CSS e poi compilare la definizione in CSS semplice da usare nei browser.

Come con i motori di templating, dovreste utilizzare il motore di fogli di stile che permetterà al vostro team di essere il più produttivo. Per questo progetto, useremo il CSS vanilla (ossia il default), poiché le nostre esigenze CSS non sono abbastanza complesse da giustificare l'utilizzo di qualcos'altro.

### Quale database dovrei usare?

Il codice generato non utilizza/inclusa alcun database. Le app _Express_ possono utilizzare qualsiasi [meccanismo di database](https://expressjs.com/en/guide/database-integration.html) supportato da _Node_ (Express stesso non definisce alcun comportamento/requisiti specifici aggiuntivi per la gestione dei database).

Discuteremo come integrare un database in un articolo successivo.

## Creazione del progetto

Per l'app esempio _Local Library_ che costruiremo, creeremo un progetto chiamato _express-locallibrary-tutorial_ utilizzando la libreria di template _Pug_ e nessun motore CSS.

Innanzitutto, navigate dove volete creare il progetto e poi eseguite l'_Express Application Generator_ nel prompt dei comandi come mostrato:

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
> Aprite tutti i file generati e cambiate le dichiarazioni `var` in `const` prima di continuare (il resto del tutorial presuppone che lo abbiate fatto).

## Esecuzione del sito web scheletro

A questo punto, abbiamo un progetto scheletro completo. Il sito web in realtà non _fa_ molto ancora, ma vale la pena eseguirlo per dimostrare che funziona.

1. Innanzitutto, installate le dipendenze (il comando `install` recupererà tutti i pacchetti di dipendenza elencati nel file **package.json** del progetto).

   ```bash
   cd express-locallibrary-tutorial
   npm install
   ```

2. Eseguite quindi l'applicazione.

   - Sul prompt CMD di Windows, utilizzate questo comando:

     ```batch
     SET DEBUG=express-locallibrary-tutorial:* & npm start
     ```

   - Su Windows PowerShell, utilizzate questo comando:

     ```powershell
     ENV:DEBUG = "express-locallibrary-tutorial:*"; npm start
     ```

     > [!NOTE]
     > I comandi PowerShell non sono trattati in questo tutorial (I comandi "Windows" forniti assumono che si stia utilizzando il prompt CMD di Windows).

   - Su macOS o Linux, utilizzate questo comando:

     ```bash
     DEBUG=express-locallibrary-tutorial:* npm start
     ```

3. Successivamente, caricate `http://localhost:3000/` nel vostro browser per accedere all'app.

Dovreste vedere una pagina del browser che assomiglia a questa:

![Browser per sito web generatore di app Express di default](expressgeneratorskeletonwebsite.png)

Congratulazioni! Ora avete un'applicazione Express funzionante che può essere accessibile tramite la porta 3000.

> [!NOTE]
> Potreste anche avviare l'app usando solo il comando `npm start`. Specificare la variabile DEBUG come mostrato abilita il logging/debugging in console. Ad esempio, quando visitate la pagina precedente, vedrete un output di debug come questo:
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

## Abilitare il riavvio del server su modifiche ai file

Le modifiche che apportate al vostro sito web Express attualmente non sono visibili fino a quando non riavviate il server. Diventa rapidamente molto irritante dover fermare e riavviare il server ogni volta che si effettua una modifica, quindi vale la pena prendersi il tempo per automatizzare il riavvio del server quando necessario.

Uno strumento conveniente per questo scopo è [nodemon](https://github.com/remy/nodemon). Questo di solito è installato globalmente (poiché è uno "strumento"), ma qui lo installeremo e utilizzeremo a livello locale come _dipendenza per lo sviluppatore_, in modo che qualsiasi sviluppatore che lavori con il progetto lo ottenga automaticamente quando installa l'applicazione. Utilizzate il comando seguente nella directory root per il progetto scheletro:

```bash
npm install --save-dev nodemon
```

Se si sceglie comunque di installare [nodemon](https://github.com/remy/nodemon) a livello globale sulla vostra macchina, e non solo nel file **package.json** del vostro progetto:

```bash
npm install -g nodemon
```

Se aprite il file **package.json** del vostro progetto, vedrete ora una nuova sezione con questa dipendenza:

```json
 "devDependencies": {
    "nodemon": "^3.1.3"
}
```

Poiché lo strumento non è installato globalmente, non possiamo avviarlo dalla riga di comando (a meno che non lo aggiungiamo al percorso), ma possiamo chiamarlo da uno script npm poiché npm conosce tutti i pacchetti installati. Trovate la sezione `scripts` del vostro package.json. Inizialmente, conterrà una linea che inizia con `"start"`. Aggiornatela mettendo una virgola alla fine di quella linea, e aggiungendo le linee `"devstart"` e `"serverstart"`:

- Su Linux e macOS, la sezione scripts apparirà così:

  ```json
    "scripts": {
      "start": "node ./bin/www",
      "devstart": "nodemon ./bin/www",
      "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
    },
  ```

- Su Windows, il valore "serverstart" apparirebbe invece così (se si utilizza il prompt dei comandi):

  ```bash
  "serverstart": "SET DEBUG=express-locallibrary-tutorial:* & npm run devstart"
  ```

Ora possiamo avviare il server quasi nello stesso modo di prima, ma utilizzando il comando `devstart`.

> [!NOTE]
> Ora se modificate qualsiasi file nel progetto, il server si riavvierà (o potete riavviarlo digitando `rs` nel prompt dei comandi in qualsiasi momento). Dovrete comunque ricaricare il browser per aggiornare la pagina.
>
> Ora dobbiamo chiamare `npm run <script-name>` piuttosto che solo `npm start`, poiché "start" è effettivamente un comando npm che è mappato allo script nominato. Avremmo potuto sostituire il comando nello script _start_ ma vogliamo usare _nodemon_ solo durante lo sviluppo, quindi ha senso creare un nuovo comando script.
>
> Il comando `serverstart` aggiunto agli script nel **package.json** sopra è un ottimo esempio. Usare questo approccio significa che non si deve più digitare un comando lungo per avviare il server. Nota che il particolare comando aggiunto allo script funziona solo per macOS o Linux.

## Il progetto generato

Diamo ora un'occhiata al progetto che abbiamo appena creato.

### Struttura della directory

Il progetto generato, ora che avete installato le dipendenze, ha la seguente struttura dei file (i file sono gli elementi **non** prefissati con "/").
Il file **package.json** definisce le dipendenze dell'applicazione e altre informazioni.
Definisce anche uno script di avvio che chiamerà il punto di ingresso dell'applicazione, il file JavaScript **/bin/www**.
Questo imposta parte della gestione degli errori dell'applicazione e poi carica **app.js** per fare il resto del lavoro.
Le rotte dell'app sono memorizzate in moduli distinti nella directory **routes/**.
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

La sezione scripts definisce innanzitutto uno script "_start_", che è ciò che stiamo invocando quando chiamiamo `npm start` per avviare il server (questo script è stato aggiunto dall'_Express Application Generator_). Dalla definizione dello script, si può vedere che questo effettivamente avvia il file JavaScript **./bin/www** con _node_.

Abbiamo già modificato questa sezione in [Abilitare il riavvio del server su modifiche ai file](#abilitare_il_riavvio_del_server_su_modifiche_ai_file) aggiungendo gli script _devstart_ e _serverstart_.
Questi possono essere utilizzati per avviare lo stesso file **./bin/www** con _nodemon_ piuttosto che _node_ (questa versione degli script è per Linux e macOS, come discusso sopra).

```json
  "scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www",
    "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
  },
```

Le dipendenze includono il pacchetto _express_ e il pacchetto per il nostro motore di visualizzazione selezionato (_pug_).
Inoltre, abbiamo i seguenti pacchetti che sono utili in molte applicazioni web:

- [cookie-parser](https://www.npmjs.com/package/cookie-parser): Utilizzato per analizzare l'intestazione dei cookie e popolare `req.cookies` (essenzialmente fornisce un metodo conveniente per accedere alle informazioni sui cookie).
- [debug](https://www.npmjs.com/package/debug): Una piccola utility di debug per node modellata dopo la tecnica di debug del nucleo di node.
- [morgan](https://www.npmjs.com/package/morgan): Un middleware di logger delle richieste HTTP per node.
- [http-errors](https://www.npmjs.com/package/http-errors): Creare errori HTTP dove necessario (per la gestione degli errori di express).

Le versioni di default nel progetto generato sono un po' obsolete.
Sostituite la sezione delle dipendenze del vostro file `package.json` con il testo seguente, che specifica le ultime versioni di queste librerie al momento della scrittura:

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

Quindi aggiornate le dipendenze installate utilizzando il comando:

```bash
npm install
```

> [!NOTE]
> È una buona idea aggiornare regolarmente alle ultime versioni compatibili delle vostre librerie di dipendenze — questo può essere fatto anche automaticamente o semi-automaticamente come parte di una configurazione di integrazione continua.
>
> Di solito gli aggiornamenti delle librerie alla versione minor e patch rimangono compatibili.
> Abbiamo prefissato ogni versione con `^` sopra in modo da poter aggiornare automaticamente alla versione `minor.patch` più recente eseguendo:
>
> ```bash
> npm update --save
> ```
>
> Le versioni major cambiano la compatibilità.
> Per quegli aggiornamenti dovremo aggiornare manualmente il `package.json` e il codice che utilizza la libreria, e testare nuovamente estensivamente il progetto.

### File www

Il file **/bin/www** è il punto di ingresso dell'applicazione! La prima cosa che fa è `require()` il "vero" punto di ingresso dell'applicazione (**app.js**, nella radice del progetto) che imposta e restituisce l'oggetto applicazione [`express()`](https://expressjs.com/en/api.html).
`require()` è il [modo CommonJS](https://nodejs.org/api/modules.html) per importare codice JavaScript, JSON, ed altri file nel file corrente.
Qui specifichiamo il modulo **app.js** usando un percorso relativo ed omettiamo l'estensione (.**js**) del file opzionale.

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

const app = require("../app");
```

> [!NOTE]
> Node.js 14 e successivi supportano le dichiarazioni `import` ES6 per importare i moduli JavaScript (ECMAScript).
> Per utilizzare questa funzione, è necessario aggiungere `"type": "module"` al file **package.json** di Express, tutti i moduli nella vostra applicazione devono utilizzare `import` piuttosto che `require()`, e per le _importazioni relative_ è necessario includere l'estensione del file (per ulteriori informazioni vedere la [documentazione di Node](https://nodejs.org/api/esm.html#introduction)).
> Anche se ci sono benefici nell'utilizzare `import`, questo tutorial utilizza `require()` per allinearsi alla [documentazione di Express](https://expressjs.com/en/starter/hello-world.html).

Il resto del codice di questo file imposta un server HTTP node con `app` imposta a una porta specifica (definita in una variabile d'ambiente o 3000 se la variabile non è definita), e inizia ad ascoltare e segnalare errori del server e connessioni. Per ora non è necessario sapere altro sul codice (tutto in questo file è "boilerplate"), ma sentitevi liberi di rivederlo se siete interessati.

### app.js

Questo file crea un oggetto applicazione `express` (chiamato `app`, per convenzione), imposta l'applicazione con varie impostazioni e middleware, e quindi esporta l'app dal modulo. Il codice seguente mostra solo le parti del file che creano ed esportano l'oggetto app:

```js
const express = require("express");

const app = express();
// …
module.exports = app;
```

Nel file di ingresso **www** sopra, è questo oggetto `module.exports` che viene fornito al chiamante quando questo file è importato.

Esaminiamo ora nel dettaglio il file **app.js**. Innanzitutto, importiamo alcune utili librerie node nel file usando `require()`, includendo _http-errors_, _express_, _morgan_ e _cookie-parser_ che abbiamo precedentemente scaricato per la nostra applicazione usando npm; e _path_, che è una libreria core di Node per analizzare percorsi di file e directory.

```js
const createError = require("http-errors");
const express = require("express");
const path = require("path");
const cookieParser = require("cookie-parser");
const logger = require("morgan");
```

Successivamente, `require()` i moduli dalla nostra directory dei percorsi. Questi moduli/file contengono codice per gestire particolari set di "rotte" (percorsi URL) correlati. Quando estenderemo l'applicazione scheletro, ad esempio per elencare tutti i libri nella biblioteca, aggiungeremo un nuovo file per gestire le rotte correlate ai libri.

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
```

> [!NOTE]
> A questo punto, abbiamo solo _importato_ il modulo; non abbiamo ancora utilizzato le sue rotte (questo avviene solo un po' più a valle nel file).

Successivamente, creiamo l'oggetto `app` utilizzando il nostro modulo _express_ importato, e poi lo utilizziamo per impostare il motore di visualizzazione (template). Ci sono due parti nella configurazione del motore. Innanzitutto, impostiamo il valore `"views"` per specificare la cartella dove i template saranno memorizzati (in questo caso la sottocartella **/views**). Poi impostiamo il valore `"view engine"` per specificare la libreria di template (in questo caso "pug").

```js
const app = express();

// view engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Il prossimo set di funzioni chiama `app.use()` per aggiungere le librerie _middleware_ che abbiamo importato sopra nella catena di gestione delle richieste.
Ad esempio, `express.json()` e `express.urlencoded()` sono necessari per popolare [`req.body`](https://expressjs.com/en/api.html#req.body) con i campi del modulo.
Dopo queste librerie, utilizziamo anche il middleware `express.static`, che fa sì che _Express_ serva tutti i file statici nella directory **/public** nella radice del progetto.

```js
app.use(logger("dev"));
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());

app.use(express.static(path.join(__dirname, "public")));
```

Ora che tutto l'altro middleware è impostato, aggiungiamo il nostro codice di gestione delle rotte (precedentemente importato) alla catena di gestione delle richieste. Il codice importato definirà rotte particolari per le diverse _parti_ del sito:

```js
app.use("/", indexRouter);
app.use("/users", usersRouter);
```

> [!NOTE]
> I percorsi specificati sopra (`"/"` e `"/users"`) sono trattati come un prefisso alle rotte definite nei file importati.
> Quindi, ad esempio, se il modulo **users** importato definisce una rotta per `/profile`, accedereste a quella rotta su `/users/profile`. Parleremo di più sulle rotte in un articolo successivo.

L'ultimo middleware nel file aggiunge metodi di gestione per gli errori e le risposte HTTP 404.

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

L'oggetto applicazione Express (app) è ora completamente configurato. L'ultimo passaggio è aggiungerlo alle esportazioni del modulo (questo è ciò che consente di importarlo da **/bin/www**).

```js
module.exports = app;
```

### Rotte

Il file della rotta **/routes/users.js** è mostrato di seguito (i file delle rotte condividono una struttura simile, quindi non è necessario mostrare anche **index.js**).
Innanzitutto, carica il modulo _express_ e lo utilizza per ottenere un oggetto `express.Router`.
Poi specifica una rotta su quell'oggetto e infine esporta il router dal modulo (questo è ciò che consente al file di essere importato in **app.js**).

```js
const express = require("express");

const router = express.Router();

/* GET users listing. */
router.get("/", (req, res, next) => {
  res.send("respond with a resource");
});

module.exports = router;
```

La rotta definisce un callback che sarà invocato ogni volta che viene rilevata una richiesta HTTP `GET` con il pattern corretto. Il pattern corrispondente è la rotta specificata quando il modulo è importato (`"/users"`) più quello che è definito in questo file (`"/"`). In altre parole, questa rotta sarà utilizzata quando viene ricevuto un URL di `/users/`.

> [!NOTE]
> Provate questo eseguendo il server con node e visitando l'URL nel vostro browser: `http://localhost:3000/users/`. Dovreste vedere un messaggio: 'respond with a resource'.

Una cosa interessante sopra è che la funzione callback ha il terzo argomento `next`, e quindi è una funzione middleware piuttosto che un semplice callback di rotta. Anche se il codice attualmente non utilizza l'argomento `next`, potrebbe essere utile in futuro se si desidera aggiungere gestori di rotta multipli al percorso `'/'`.

### Views (template)

Le visualizzazioni (template) sono memorizzate nella directory **/views** (come specificato in **app.js**) e hanno l'estensione **.pug**. Il metodo [`Response.render()`](https://expressjs.com/en/4x/api.html#res.render) viene utilizzato per rendere un template specificato insieme ai valori delle variabili nominate passate in un oggetto, e quindi inviare il risultato come risposta. Nel codice di seguito da **/routes/index.js** È possibile vedere come quella rotta rende una risposta utilizzando il template "index" passando la variabile "title" del template.

```js
/* GET home page. */
router.get("/", (req, res, next) => {
  res.render("index", { title: "Express" });
});
```

Il template corrispondente per la rotta sopra è dato qui di seguito (**index.pug**). Parleremo di più sulla sintassi più tardi. Tutto quello che dovete sapere per ora è che la variabile `title` (con il valore `'Express'`) viene inserita dove specificato nel template.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Mettiti alla prova

Crea una nuova rotta in **/routes/users.js** che visualizzerà il testo "_You're so cool_" all'URL `/users/cool/`. Prova eseguendo il server e visitando `http://localhost:3000/users/cool/` nel tuo browser.

## Sommario

Avete ora creato un progetto sito web scheletro per la [Local Library](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) e verificato che funzioni utilizzando _node_. Più importante, comprendete anche come è strutturato il progetto, così avete una buona idea di dove dobbiamo fare i cambiamenti per aggiungere rotte e visualizzazioni per la nostra libreria locale.

Successivamente, inizieremo a modificare lo scheletro in modo che funzioni come un sito web di biblioteca.

## Vedi anche

- [Generatore di applicazioni Express](https://expressjs.com/en/starter/generator.html) (documentazione Express)
- [Utilizzare i motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
