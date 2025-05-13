---
title: "Tutorial Express Parte 2: Creare lo scheletro di un sito web"
short-title: "2: Sito web scheletrico"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo secondo articolo del nostro [Tutorial Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) mostra come creare un progetto di sito web "scheletro", che può poi essere popolato con rotte specifiche del sito, template/viste e richieste al database.

> [!WARNING]
> Il tutorial di Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Pianifichiamo di aggiornare la documentazione nella seconda metà del 2025.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">Configurare un ambiente di sviluppo Node</a>.
          Revisione del Tutorial Express.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Essere in grado di avviare i propri nuovi progetti di siti web usando il <em>Generatore di Applicazioni Express</em>.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Questo articolo mostra come creare un sito web "scheletro" usando lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator.html), che può quindi essere popolato con rotte specifiche del sito, viste/template e chiamate al database. In questo caso, useremo lo strumento per creare la struttura del nostro [sito web Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website), al quale aggiungeremo successivamente tutto il codice necessario per il sito. Il processo è estremamente semplice, richiedendo solo di invocare il generatore dal terminale con un nuovo nome di progetto, specificando, opzionalmente, anche il motore di template del sito e il generatore CSS.

Le seguenti sezioni mostrano come chiamare il generatore di applicazioni e forniscono una piccola spiegazione sulle diverse opzioni di viste/CSS. Spiegheremo anche come è strutturato il sito scheletro. Alla fine, mostreremo come è possibile eseguire il sito per verificare che funzioni.

> [!NOTE]
>
> - Il _Generatore di Applicazioni Express_ non è l'unico generatore per applicazioni Express, e il progetto generato non è l'unico modo valido per strutturare i file e le directory. Tuttavia, il sito generato ha una struttura modulare che è facile da estendere e comprendere. Per informazioni su un'applicazione Express _minimale_, vedere [Esempio Hello world](https://expressjs.com/en/starter/hello-world.html) (documentazione Express).
> - Il _Generatore di Applicazioni Express_ dichiara la maggior parte delle variabili usando `var`.
>   Abbiamo cambiato la maggior parte di queste in [`const`](/it/docs/Web/JavaScript/Reference/Statements/const) (e alcune in [`let`](/it/docs/Web/JavaScript/Reference/Statements/let)) nel tutorial, poiché vogliamo dimostrare la pratica moderna di JavaScript.
> - Questo tutorial utilizza la versione di _Express_ e le altre dipendenze che sono definite nel **package.json** creato dal _Generatore di Applicazioni Express_.
>   Queste non sono (necessariamente) l'ultima versione, e potrebbe essere opportuno aggiornarle quando si distribuisce un'applicazione reale in produzione.

## Utilizzare il generatore di applicazioni

Dovrebbe aver già installato il generatore come parte della [configurazione di un ambiente di sviluppo Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment). Come promemoria rapido, si installa lo strumento generatore a livello globale usando il gestore di pacchetti npm, come mostrato:

```bash
npm install express-generator -g
```

Il generatore ha una serie di opzioni, che è possibile visualizzare nel terminale utilizzando il comando `--help` (o `-h`):

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

È possibile specificare a express di creare un progetto all'interno della directory _corrente_ utilizzando il motore di visualizzazione _Jade_ e CSS semplice (se si specifica un nome di directory, il progetto verrà creato in una sottocartella con quel nome).

```bash
express
```

È inoltre possibile scegliere un motore di visualizzazione (template) utilizzando `--view` e/o un motore di generazione CSS utilizzando `--css`.

> [!NOTE]
> Le altre opzioni per scegliere i motori di template (ad esempio `--hogan`, `--ejs`, `--hbs`, ecc.) sono deprecate. Utilizzare `--view` (o `-v`).

### Quale motore di visualizzazione dovrei usare?

Il _Generatore di Applicazioni Express_ consente di configurare diversi motori di visualizzazione/template popolari, inclusi [EJS](https://www.npmjs.com/package/ejs), [Hbs](https://github.com/pillarjs/hbs), [Pug](https://pugjs.org/api/getting-started.html) (Jade), [Twig](https://www.npmjs.com/package/twig), e [Vash](https://www.npmjs.com/package/vash), sebbene scelga Jade per impostazione predefinita se non si specifica un'opzione di visualizzazione. Express stesso può supportare un gran numero di altri linguaggi di template [out of the box](https://github.com/expressjs/express/wiki#template-engines).

> [!NOTE]
> Se si desidera utilizzare un motore di template non supportato dal generatore, vedere [Utilizzare i motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express) e la documentazione del motore di visualizzazione desiderato.

In generale, è opportuno scegliere un motore di template che fornisca tutte le funzionalità necessarie e permetta di essere produttivi prima — o in altre parole, nello stesso modo in cui si sceglie qualsiasi altro componente! Alcune cose da considerare quando si confrontano i motori di template:

- Tempo di produttività — Se il suo team ha già esperienza con un linguaggio di template, è probabile che saranno più produttivi usando quel linguaggio. In caso contrario, dovrebbe considerare la curva di apprendimento relativa per i motori di template candidati.
- Popolarità e attività — Esaminare la popolarità del motore e se ha una comunità attiva. È importante poter ottenere supporto quando sorgono problemi durante la vita del sito web.
- Stile — Alcuni motori di template utilizzano un markup specifico per indicare il contenuto inserito all'interno di HTML "normale", mentre altri costruiscono l'HTML utilizzando una sintassi diversa (ad esempio, utilizzando l'indentazione e i nomi dei blocchi).
- Prestazioni/tempo di rendering.
- Caratteristiche — dovrebbe considerare se i motori che sta esaminando hanno le seguenti caratteristiche disponibili:

  - Ereditarietà dei layout: Consente di definire un template di base e quindi "ereditare" solo le parti di esso che si desidera siano differenti per una pagina particolare. Questo è tipicamente un approccio migliore rispetto a costruire template includendo un numero di componenti richiesti o costruire un template da zero ogni volta.
  - Supporto "Include": Consente di costruire template includendo altri template.
  - Sintassi concisa per il controllo dei cicli e delle variabili.
  - Capacità di filtrare i valori delle variabili a livello di template, come rendere le variabili in maiuscolo o formattare un valore di data.
  - Capacità di generare formati di output diversi dall'HTML, come JSON o XML.
  - Supporto per operazioni asincrone e streaming.
  - Caratteristiche lato client. Se un motore di template può essere utilizzato sul client, ciò consente la possibilità di avere tutto o la maggior parte del rendering svolto lato client.

> [!NOTE]
> Ci sono molte risorse su Internet per aiutarla a confrontare le diverse opzioni!

Per questo progetto, useremo il motore di template [Pug](https://pugjs.org/api/getting-started.html) (questo è il motore precedentemente denominato Jade), poiché è uno dei linguaggi di template Express/JavaScript più popolari e supportato dal generatore senza modifiche.

### Quale motore di fogli di stile CSS dovrei usare?

Il _Generatore di Applicazioni Express_ consente di creare un progetto configurato per utilizzare i motori di fogli di stile CSS più comuni: [LESS](https://lesscss.org/), [SASS](https://sass-lang.com/), [Stylus](https://stylus-lang.com/).

> [!NOTE]
> CSS ha alcune limitazioni che rendono difficili determinati compiti. I motori di fogli di stile CSS consentono di utilizzare una sintassi più potente per definire il proprio CSS e quindi compilare la definizione in semplice CSS per i browser.

Come per i motori di template, dovrebbe utilizzare il motore di fogli di stile che consentirà al suo team di essere più produttivo. Per questo progetto, useremo il semplice CSS (il predefinito) poiché i nostri requisiti CSS non sono sufficientemente complicati da giustificare l'uso di qualcos'altro.

### Quale database dovrei usare?

Il codice generato non utilizza/include alcun database. Le app _Express_ possono utilizzare qualsiasi [meccanismo di database](https://expressjs.com/en/guide/database-integration.html) supportato da _Node_ (Express stesso non definisce alcun comportamento/requisiti aggiuntivi specifici per la gestione del database).

Discuteremo come integrare un database in un articolo successivo.

## Creare il progetto

Per l'app di esempio _Local Library_ che andremo a costruire, creeremo un progetto denominato _express-locallibrary-tutorial_ utilizzando la libreria di template _Pug_ e nessun motore CSS.

Per prima cosa, navigare nel punto in cui si desidera creare il progetto e quindi eseguire il _Generatore di Applicazioni Express_ nel prompt dei comandi come mostrato:

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
> Apra tutti i file generati e cambi le dichiarazioni `var` in `const` prima di procedere (il resto del tutorial presuppone che lo abbia fatto).

## Eseguire il sito web scheletrico

A questo punto, abbiamo un progetto scheletro completo. Il sito web non fa ancora molto, ma vale la pena eseguirlo per mostrare che funziona.

1. Innanzitutto, installare le dipendenze (il comando `install` scaricherà tutti i pacchetti di dipendenze elencati nel file **package.json** del progetto).

   ```bash
   cd express-locallibrary-tutorial
   npm install
   ```

2. Quindi eseguire l'applicazione.

   - Nel prompt CMD di Windows, usa questo comando:

     ```batch
     SET DEBUG=express-locallibrary-tutorial:* & npm start
     ```

   - In Windows PowerShell, usa questo comando:

     ```powershell
     ENV:DEBUG = "express-locallibrary-tutorial:*"; npm start
     ```

     > [!NOTE]
     > I comandi PowerShell non sono coperti in questo tutorial (I comandi "Windows" forniti presuppongono che stia usando il prompt CMD di Windows.)

   - Su macOS o Linux, usa questo comando:

     ```bash
     DEBUG=express-locallibrary-tutorial:* npm start
     ```

3. Carichi `http://localhost:3000/` nel suo browser per accedere all'applicazione.

Dovrebbe vedere una pagina del browser che appare così:

![Browser per il sito web generato predefinito di Express](expressgeneratorskeletonwebsite.png)

Congratulazioni! Ora ha un'applicazione Express funzionante che può essere accessibile tramite la porta 3000.

> [!NOTE]
> Potrebbe anche avviare l'app utilizzando solo il comando `npm start`. Specificare la variabile DEBUG come mostrato abilita il logging/la debug console. Ad esempio, quando visita la pagina sopra vedrà un output di debug come questo:
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

Eventuali modifiche apportate al suo sito web Express attualmente non sono visibili finché non si riavvia il server. Diventa rapidamente molto irritante dover fermare e riavviare il server ogni volta che si apporta una modifica, quindi vale la pena prendersi il tempo per automatizzare il riavvio del server quando necessario.

Uno strumento conveniente a questo scopo è [nodemon](https://github.com/remy/nodemon). Questo viene generalmente installato globalmente (poiché è uno "strumento"), ma qui lo installeremo e utilizzeremo localmente come _dipendenza dello sviluppatore_, in modo che qualsiasi sviluppatore che lavori al progetto lo ottenga automaticamente quando installa l'applicazione. Utilizzi il seguente comando nella directory principale del progetto scheletro:

```bash
npm install --save-dev nodemon
```

Se sceglie comunque di installare [nodemon](https://github.com/remy/nodemon) globalmente sulla sua macchina, e non solo nel file **package.json** del suo progetto:

```bash
npm install -g nodemon
```

Se apre il file **package.json** del suo progetto vedrà ora una nuova sezione con questa dipendenza:

```json
 "devDependencies": {
    "nodemon": "^3.1.3"
}
```

Poiché lo strumento non è installato globalmente, non possiamo avviarlo dal terminale (a meno che non lo aggiungiamo al percorso) ma possiamo chiamarlo da uno script npm perché npm conosce tutti i pacchetti installati. Trova la sezione `scripts` del tuo package.json. Inizialmente, conterrà una sola riga, che inizia con `"start"`. Aggiorni la sezione mettendo una virgola alla fine di quella riga e aggiungendo le righe `"devstart"` e `"serverstart"`:

- Su Linux e macOS, la sezione scripts sarà così:

  ```json
    "scripts": {
      "start": "node ./bin/www",
      "devstart": "nodemon ./bin/www",
      "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
    },
  ```

- Su Windows, il valore di "serverstart" sarebbe invece così (se si utilizza il prompt dei comandi):

  ```bash
  "serverstart": "SET DEBUG=express-locallibrary-tutorial:* & npm run devstart"
  ```

Possiamo ora avviare il server praticamente allo stesso modo di prima, ma usando il comando `devstart`.

> [!NOTE]
> Ora se modifica qualsiasi file nel progetto il server si riavvierà (o può riavviarlo digitando `rs` nel prompt dei comandi in qualsiasi momento). Occorrerà ancora ricaricare il browser per aggiornare la pagina.
>
> Dobbiamo ora chiamare `npm run <nome-script>` anziché semplicemente `npm start`, perché "start" è effettivamente un comando npm che è mappato sullo script denominato. Avremmo potuto sostituire il comando nel _start_ script, ma vogliamo usare _nodemon_ solo durante lo sviluppo, quindi ha senso creare un nuovo comando script.
>
> Il comando `serverstart` aggiunto agli script nel **package.json** sopra è un ottimo esempio. Utilizzare questo approccio significa che non deve più digitare un comando lungo per avviare il server. Nota che il comando specifico aggiunto allo script funziona solo per macOS o Linux.

## Il progetto generato

Diamo ora un'occhiata al progetto che abbiamo appena creato.

### Struttura della directory

Il progetto generato, ora che ha installato le dipendenze, ha la seguente struttura di file (i file sono gli elementi **non** prefissati con "/").
Il file **package.json** definisce le dipendenze dell'applicazione e altre informazioni.
Definisce anche uno script di avvio che chiamerà il punto di ingresso dell'applicazione, il file JavaScript **/bin/www**.
Questo imposta parte della gestione degli errori dell'applicazione e quindi carica **app.js** per fare il resto del lavoro.
Le rotte dell'app sono memorizzate in moduli separati nella directory **routes/**.
I template sono memorizzati nella directory **/views**.

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

Le seguenti sezioni descrivono i file in maggior dettaglio.

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

La sezione scripts definisce innanzitutto uno script "_start_", che è ciò che invochiamo quando chiamiamo `npm start` per avviare il server (questo script è stato aggiunto dal _Generatore di Applicazioni Express_). Dalla definizione dello script, può vedere che questo effettivamente avvia il file JavaScript **./bin/www** con _node_.

Abbiamo già modificato questa sezione in [Abilitare il riavvio del server su modifiche ai file](#abilitare_il_riavvio_del_server_su_modifiche_ai_file) aggiungendo gli script _devstart_ e _serverstart_.
Questi possono essere usati per avviare lo stesso file **./bin/www** con _nodemon_ anziché _node_ (questa versione degli script è per Linux e macOS, come discusso sopra).

```json
  "scripts": {
    "start": "node ./bin/www",
    "devstart": "nodemon ./bin/www",
    "serverstart": "DEBUG=express-locallibrary-tutorial:* npm run devstart"
  },
```

Le dipendenze includono il pacchetto _express_ e il pacchetto per il nostro motore di visualizzazione selezionato (_pug_).
Inoltre, abbiamo i seguenti pacchetti che sono utili in molte applicazioni web:

- [cookie-parser](https://www.npmjs.com/package/cookie-parser): Usato per analizzare l'intestazione dei cookie e popolare `req.cookies` (fornisce in sostanza un metodo conveniente per accedere a informazioni sui cookie).
- [debug](https://www.npmjs.com/package/debug): Una piccola utility di debug node modellata sulla tecnica di debugging del core di node.
- [morgan](https://www.npmjs.com/package/morgan): Un middleware logger di richieste HTTP per node.
- [http-errors](https://www.npmjs.com/package/http-errors): Crea errori HTTP dove necessario (per la gestione degli errori in express).

Le versioni predefinite nel progetto generato sono un po' datate.
Sostituire la sezione delle dipendenze del file `package.json` con il seguente testo, che specifica le versioni più recenti di queste librerie al momento della scrittura:

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

Poi aggiornare le dipendenze installate usando il comando:

```bash
npm install
```

> [!NOTE]
> È una buona idea aggiornare regolarmente alle versioni compatibili più recenti delle sue librerie di dipendenza — questo può essere fatto automatico o semi-automatico come parte di una configurazione di integrazione continua.
>
> Di solito gli aggiornamenti delle librerie alla versione minore e patch rimangono compatibili.
> Abbiamo prefissato ciascuna versione con `^` sopra in modo da poter aggiornare automaticamente alla versione `minor.patch` più recente eseguendo:
>
> ```bash
> npm update --save
> ```
>
> Le versioni maggiori cambiano la compatibilità.
> Per quegli aggiornamenti dovremo aggiornare manualmente il `package.json` e il codice che utilizza la libreria, e testare estensivamente il progetto.

### File www

Il file **/bin/www** è il punto di ingresso dell'applicazione! La prima cosa che questo fa è `require()` il "vero" punto di ingresso dell'applicazione (**app.js**, nella radice del progetto) che imposta e restituisce l'oggetto dell'applicazione [`express()`](https://expressjs.com/en/api.html).
`require()` è il [modo CommonJS](https://nodejs.org/api/modules.html) per importare codice JavaScript, JSON e altri file nel file corrente.
Qui specifichiamo il modulo **app.js** usando un percorso relativo e omettiamo l'estensione (.**js**) del file opzionale.

```js
#!/usr/bin/env node

/**
 * Module dependencies.
 */

const app = require("../app");
```

> [!NOTE]
> Node.js 14 e versioni successive supportano le dichiarazioni `import` ES6 per importare moduli JavaScript (ECMAScript).
> Per utilizzare questa funzione, deve aggiungere `"type": "module",` al file Express **package.json**, tutti i moduli della sua applicazione devono usare `import` anziché `require()`, e per _importazioni relative_ deve includere l'estensione del file (per ulteriori informazioni vedere la [documentazione di Node](https://nodejs.org/api/esm.html#introduction)).
> Mentre ci sono vantaggi nell'uso di `import`, questo tutorial usa `require()` per allinearsi alla [documentazione di Express](https://expressjs.com/en/starter/hello-world.html).

Il resto del codice in questo file imposta un server HTTP node con `app` impostato su una porta specifica (definita in una variabile d'ambiente o 3000 se la variabile non è definita), e inizia ad ascoltare e segnalare errori del server e connessioni. Per ora non è davvero necessario sapere altro sul codice (tutto in questo file è "boilerplate"), ma sentiti libero di rivederlo se sei interessato.

### app.js

Questo file crea un oggetto applicazione `express` (chiamato `app`, per convenzione), imposta l'applicazione con diverse impostazioni e middleware, e quindi esporta l'app dal modulo. Il codice qui sotto mostra solo le parti del file che creano ed esportano l'oggetto app:

```js
const express = require("express");
const app = express();
// …
module.exports = app;
```

Nel file di punto di ingresso **www** sopra, è questo oggetto `module.exports` che viene fornito al chiamante quando questo file è importato.

Esaminiamo in dettaglio il file **app.js**. Per prima cosa, importiamo alcune librerie node utili nel file usando `require()`, inclusi _http-errors_, _express_, _morgan_ e _cookie-parser_ che abbiamo precedentemente scaricato per la nostra applicazione usando npm; e _path_, che è una libreria core di Node per l'analisi di percorsi di file e directory.

```js
const createError = require("http-errors");
const express = require("express");
const path = require("path");
const cookieParser = require("cookie-parser");
const logger = require("morgan");
```

Poi `require()` i moduli dalla nostra directory delle rotte. Questi moduli/file contengono codice per gestire insiemi particolari di rotte "collegate" (percorsi URL). Quando estenderemo l'applicazione scheletro, ad esempio per elencare tutti i libri nella biblioteca, aggiungeremo un nuovo file per gestire le rotte relative ai libri.

```js
const indexRouter = require("./routes/index");
const usersRouter = require("./routes/users");
```

> [!NOTE]
> A questo punto, abbiamo solo _importato_ il modulo; non abbiamo ancora usato le sue rotte (questo accade appena un po' più avanti nel file).

Successivamente, creiamo l'oggetto `app` usando il nostro modulo _express_ importato, e poi lo usiamo per impostare il motore di visualizzazione (template). Ci sono due parti per impostare il motore. Prima, impostiamo il valore `"views"` per specificare la cartella dove saranno memorizzati i template (in questo caso la sottocartella **/views**). Poi impostiamo il valore `"view engine"` per specificare la libreria di template (in questo caso "pug").

```js
const app = express();

// view engine setup
app.set("views", path.join(__dirname, "views"));
app.set("view engine", "pug");
```

Il successivo gruppo di funzioni chiama `app.use()` per aggiungere le librerie _middleware_ che abbiamo importato sopra nella catena di gestione delle richieste.
Ad esempio, `express.json()` e `express.urlencoded()` sono necessari per popolare [`req.body`](https://expressjs.com/en/api.html#req.body) con i campi del modulo.
Dopo queste librerie, usiamo anche il middleware `express.static`, che fa sì che _Express_ serva tutti i file statici nella directory **/public** nella radice del progetto.

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
> I percorsi specificati sopra (`"/"` e `"/users"`) sono trattati come prefissi per le rotte definite nei file importati.
> Quindi, ad esempio, se il modulo **users** importato definisce una rotta per `/profile`, si accederebbe a quella rotta a `/users/profile`. Ne parleremo di più sulle rotte in un articolo successivo.

L'ultimo middleware nel file aggiunge metodi gestori per errori e risposte HTTP 404.

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

L'oggetto applicazione Express (app) è ora completamente configurato. L'ultimo passaggio è aggiungerlo alle esportazioni del modulo (questo è ciò che consente di essere importato da **/bin/www**).

```js
module.exports = app;
```

### Rotte

Il file di rotta **/routes/users.js** è mostrato di seguito (i file delle rotte condividono una struttura simile, quindi non è necessario mostrare anche **index.js**).
Prima, carica il modulo _express_ e lo usa per ottenere un oggetto `express.Router`.
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

La rotta definisce un callback che verrà invocato ogni volta che viene rilevata una richiesta HTTP `GET` con il pattern corretto. Il pattern corrispondente è la rotta specificata quando il modulo è importato (`"/users"`) più quello definito in questo file (`"/"`). In altre parole, questa rotta sarà utilizzata quando una URL di `/users/` viene ricevuta.

> [!NOTE]
> Provi questo eseguendo il server con node e visitando l'URL nel suo browser: `http://localhost:3000/users/`. Dovrebbe visualizzare un messaggio: 'rispondi con una risorsa'.

Una cosa di interesse sopra è che la funzione di callback ha il terzo argomento `next` e quindi è una funzione middleware piuttosto che un semplice callback di rotta. Anche se il codice attualmente non utilizza l'argomento `next`, potrebbe essere utile in futuro se si volessero aggiungere più gestori di route al percorso `'/'`.

### Viste (template)

Le viste (template) sono memorizzate nella directory **/views** (come specificato in **app.js**) e hanno l'estensione del file **.pug**. Il metodo [`Response.render()`](https://expressjs.com/en/4x/api.html#res.render) è usato per rendere un template specificato insieme ai valori delle variabili nominate passate in un oggetto, e poi inviare il risultato come una risposta. Nel codice qui sotto da **/routes/index.js** può vedere come quella route renderizzica una risposta usando il template "index" passando la variabile del template "title".

```js
/* GET home page. */
router.get("/", (req, res, next) => {
  res.render("index", { title: "Express" });
});
```

Il template corrispondente per la route sopra è riportato di seguito (**index.pug**). Parleremo più della sintassi più avanti. Tutto quello che deve sapere per ora è che la variabile `title` (con valore `'Express'`) è inserita dove specificato nel template.

```pug
extends layout

block content
  h1= title
  p Welcome to #{title}
```

## Sfida personale

Creare una nuova route in **/routes/users.js** che visualizzerà il testo "Lei è così in gamba" all'URL `/users/cool/`. Testarlo eseguendo il server e visitando `http://localhost:3000/users/cool/` nel suo browser

## Sommario

Ha ora creato un progetto di sito scheletro per la [Biblioteca Locale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website) e verificato che funzioni utilizzando _node_. La cosa più importante, comprende anche come il progetto è strutturato, quindi ha una buona idea di dove dobbiamo fare modifiche per aggiungere rotte e viste per la nostra biblioteca locale.

Successivamente, inizieremo a modificare lo scheletro in modo che funzioni come sito web della biblioteca.

## Vedere anche

- [Express application generator](https://expressjs.com/en/starter/generator.html) (documentazione Express)
- [Using template engines with Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
