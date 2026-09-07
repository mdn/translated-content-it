---
title: Configurazione di un ambiente di sviluppo Node
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment
l10n:
  sourceCommit: 75165f9f9bde9bce3093a0d9d908a239c519a9ce
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che è noto a cosa serve [Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#introducing_express), verrà illustrato come configurare e testare un ambiente di sviluppo Node/Express su Windows, Linux (Ubuntu) o macOS. Per ciascuno di questi sistemi operativi, questo articolo fornisce ciò che serve per iniziare a sviluppare app Express.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Sapere come aprire un terminale / una riga di comando. Sapere come installare pacchetti software nel sistema operativo del computer di sviluppo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Configurare un ambiente di sviluppo per Express sul computer.</td>
    </tr>
  </tbody>
</table>

## Panoramica dell'ambiente di sviluppo Express

_Node_ ed _Express_ semplificano molto la configurazione del computer per iniziare a sviluppare applicazioni web. Questa sezione offre una panoramica degli strumenti necessari, illustra alcuni dei metodi più semplici per installare Node (ed Express) su Ubuntu, macOS e Windows, e mostra come testare l'installazione.

### Che cos'è l'ambiente di sviluppo Express?

L'ambiente di sviluppo _Express_ include un'installazione di _Node.js_, il _gestore di pacchetti npm_ e, facoltativamente, l'_Express Application Generator_ sul computer locale.

_Node_ e il gestore di pacchetti _npm_ vengono installati insieme da pacchetti binari predisposti, programmi di installazione, gestori di pacchetti del sistema operativo o dal codice sorgente (come mostrato nelle sezioni seguenti). _Express_ viene quindi installato da npm come dipendenza delle singole applicazioni web _Express_ (insieme ad altre librerie come motori di template, driver di database, middleware di autenticazione, middleware per servire file statici e così via).

_npm_ può essere utilizzato anche per installare globalmente l'_Express Application Generator_, un utile strumento per creare app web _Express_ scheletriche che seguono il {{Glossary("MVC", "pattern MVC")}}. Il generatore di applicazioni è facoltativo perché non è necessario usare questo strumento per creare app che utilizzano Express o per costruire app Express con la stessa struttura architetturale o le stesse dipendenze. Verrà tuttavia utilizzato, poiché semplifica molto l'avvio e promuove una struttura modulare dell'applicazione.

> [!NOTE]
> A differenza di altri framework web, l'ambiente di sviluppo non include un server web di sviluppo separato. In _Node_/_Express_, un'applicazione web crea ed esegue il proprio server web.

Esistono altri strumenti periferici che fanno parte di un tipico ambiente di sviluppo, inclusi [editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o IDE per modificare il codice, e strumenti di gestione del controllo del codice sorgente come [Git](https://git-scm.com/) per gestire in sicurezza le diverse versioni del codice. Si presume che questi tipi di strumenti siano già installati, in particolare un editor di testo.

### Quali sistemi operativi sono supportati?

_Node_ può essere eseguito su Windows, macOS, molte varianti di Linux, Docker e così via. È disponibile un elenco completo nella pagina [Downloads](https://nodejs.org/en/download) di Node.js. Quasi tutti i computer personali dovrebbero avere le prestazioni necessarie per eseguire Node durante lo sviluppo. _Express_ viene eseguito in un ambiente _Node_ e può quindi essere eseguito su qualsiasi piattaforma che esegua _Node_.

In questo articolo vengono fornite istruzioni di configurazione per Windows, macOS e Ubuntu Linux.

### Quale versione di Node/Express utilizzare?

Esistono molte [release di Node](https://nodejs.org/en/blog/release/) — le release più recenti contengono correzioni di bug, supporto per versioni più recenti degli standard ECMAScript (JavaScript) e miglioramenti alle API di Node.

In generale, è consigliabile utilizzare la release _LTS (long-term supported)_ più recente, poiché sarà più stabile della release "current", pur offrendo funzionalità relativamente recenti ed essendo ancora mantenuta attivamente. È consigliabile utilizzare la release _Current_ se è necessaria una funzionalità non presente nella versione LTS.

Per _Express_ è consigliabile utilizzare la release LTS più recente di Node.

### E i database e le altre dipendenze?

Altre dipendenze, come driver di database, motori di template, motori di autenticazione e così via, fanno parte dell'applicazione e vengono importate nell'ambiente dell'applicazione utilizzando il gestore di pacchetti npm. Verranno trattate in articoli successivi specifici per le app.

## Installazione di Node

Per utilizzare _Express_ è necessario installare _Node.js_ e il [Node Package Manager (npm)](https://docs.npmjs.com/) sul sistema operativo.
Per semplificare l'operazione, verrà prima installato un gestore delle versioni di node, che verrà poi utilizzato per installare le versioni più recenti con supporto a lungo termine (LTS) di node e npm.

> [!NOTE]
> È anche possibile installare nodejs e npm con i programmi di installazione forniti su <https://nodejs.org/en/> (selezionare il pulsante per scaricare la build LTS "Recommended for most users"), oppure [installarli tramite il gestore di pacchetti del sistema operativo](https://nodejs.org/en/download) (nodejs.org).
> È vivamente consigliato utilizzare un gestore delle versioni di node, poiché semplifica l'installazione, l'aggiornamento e il passaggio tra versioni specifiche di node e npm.

### Windows

Esistono diversi gestori delle versioni di node per Windows.
Qui viene utilizzato [nvm-windows](https://github.com/coreybutler/nvm-windows), molto apprezzato tra gli sviluppatori node.

Installare la versione più recente utilizzando il programma di installazione preferito dalla pagina [nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases).
Dopo l'installazione di `nvm-windows`, aprire un prompt dei comandi, o PowerShell, e immettere il comando seguente per scaricare la versione LTS più recente di nodejs e npm:

```bash
nvm install lts
```

Al momento della stesura di questo articolo, la versione LTS di nodejs è 22.17.0.
È possibile impostarla come _versione corrente_ da utilizzare con il comando seguente:

```bash
nvm use 22.17.0
```

> [!NOTE]
> Se vengono visualizzati avvisi "Access Denied", sarà necessario eseguire questo comando in un prompt con permessi di amministrazione.

Utilizzare il comando `nvm --help` per scoprire altre opzioni della riga di comando, come elencare tutte le versioni node disponibili e tutte le versioni NVM scaricate.

### Ubuntu e macOS

Esistono diversi gestori delle versioni di node per Ubuntu e macOS.
[nvm](https://github.com/nvm-sh/nvm) è uno dei più popolari ed è la versione originale su cui si basa `nvm-windows`.
Consultare [nvm > Install & Update Script](https://github.com/nvm-sh/nvm#install--update-script) per le istruzioni nel terminale per installare la versione più recente di nvm.

Dopo l'installazione di `nvm`, aprire un terminale e immettere il comando seguente per scaricare la versione LTS più recente di nodejs e npm:

```bash
nvm install --lts
```

Al momento della stesura di questo articolo, la versione LTS di nodejs è 22.17.0.
Il comando `nvm list` mostra l'insieme delle versioni scaricate e la versione corrente.
È possibile impostare una versione specifica come _versione corrente_ con il comando seguente, lo stesso utilizzato per `nvm-windows`:

```bash
nvm use 22.17.0
```

Utilizzare il comando `nvm --help` per scoprire altre opzioni della riga di comando.
Queste sono spesso simili o identiche a quelle offerte da `nvm-windows`.

### Test dell'installazione di Node.js e npm

Dopo aver impostato `nvm` per utilizzare una versione node specifica, è possibile testare l'installazione.
Un buon modo per farlo è utilizzare il comando "version" nel terminale/prompt dei comandi e verificare che venga restituita la stringa di versione prevista:

```bash
> node -v
v22.17.0
```

Dovrebbe essere stato installato anche il gestore di pacchetti _npm_ di _Node.js_, che può essere testato nello stesso modo:

```bash
> npm -v
10.9.2
```

Come test leggermente più interessante, verrà creato un server "node puro" molto semplice che stampa "Hello World" nel browser quando viene visitato l'URL corretto:

1. Copiare il testo seguente in un file denominato **hellonode.js**. Questo utilizza funzionalità Node pure, senza elementi di Express:

   ```js
   // Load HTTP module
   const http = require("http");

   const hostname = "127.0.0.1";
   const port = 3000;

   // Create HTTP server and listen on port 3000 for requests
   const server = http.createServer((req, res) => {
     // Set the response HTTP header with HTTP status and Content type
     res.statusCode = 200;
     res.setHeader("Content-Type", "text/plain");
     res.end("Hello World\n");
   });

   // Listen for request on port 3000, and as a callback function have the port listened on logged
   server.listen(port, hostname, () => {
     console.log(`Server running at http://${hostname}:${port}/`);
   });
   ```

   Il codice importa il modulo "http" e lo utilizza per creare un server (`createServer()`) che rimane in ascolto delle richieste HTTP sulla porta 3000. Lo script stampa poi un messaggio nella console relativo all'URL del browser utilizzabile per testare il server. La funzione `createServer()` riceve come argomento una funzione di callback che verrà invocata quando viene ricevuta una richiesta HTTP: questa restituisce una risposta con un codice di stato HTTP 200 ("OK") e il testo semplice "Hello World".

   > [!NOTE]
   > Non è importante comprendere ancora esattamente il funzionamento di questo codice. Il codice verrà spiegato in maggiore dettaglio quando si inizierà a utilizzare Express.

2. Avviare il server passando, nel prompt dei comandi, alla stessa directory del file `hellonode.js` e chiamando `node` insieme al nome dello script, come segue:

   ```bash
   node hellonode.js
   ```

   Dopo l'avvio del server, verrà visualizzato un output nella console che indica l'indirizzo IP su cui è in esecuzione il server:

   ```plain
   Server running at http://127.0.0.1:3000/
   ```

3. Passare all'URL `http://127.0.0.1:3000`. Se tutto funziona correttamente, il browser dovrebbe visualizzare la stringa "Hello World".

## Utilizzo di npm

Dopo _Node_ stesso, [npm](https://docs.npmjs.com/) è lo strumento più importante per lavorare con le applicazioni _Node_.
`npm` viene utilizzato per recuperare tutti i pacchetti, ovvero librerie JavaScript, necessari a un'applicazione per lo sviluppo, il testing e/o la produzione, e può essere utilizzato anche per eseguire test e strumenti impiegati nel processo di sviluppo.

> [!NOTE]
> Dal punto di vista di Node, _Express_ è semplicemente un altro pacchetto da installare usando npm e quindi da richiedere nel proprio codice.

È possibile utilizzare manualmente npm per recuperare separatamente ogni pacchetto necessario. In genere, invece, le dipendenze vengono gestite utilizzando un file di definizione in testo semplice denominato [package.json](https://docs.npmjs.com/files/package.json/). Questo file elenca tutte le dipendenze di uno specifico "pacchetto" JavaScript, inclusi nome, versione, descrizione, file iniziale da eseguire, dipendenze di produzione, dipendenze di sviluppo, versioni di _Node_ con cui può funzionare e così via. Il file **package.json** dovrebbe contenere tutto ciò che npm necessita per recuperare ed eseguire l'applicazione. Se si scrivesse una libreria riutilizzabile, questa definizione potrebbe essere utilizzata per caricare il pacchetto nel repository npm e renderlo disponibile ad altri utenti.

### Aggiunta di dipendenze

I passaggi seguenti mostrano come utilizzare npm per scaricare un pacchetto, salvarlo nelle dipendenze del progetto e quindi richiederlo in un'applicazione Node.

> [!NOTE]
> Qui vengono mostrate le istruzioni per recuperare e installare il pacchetto _Express_. In seguito verrà mostrato come questo pacchetto e altri siano già specificati dall'_Express Application Generator_. Questa sezione è inclusa perché è utile comprendere come funziona npm e cosa viene creato dal generatore di applicazioni.

1. Creare innanzitutto una directory per la nuova applicazione e passare a essa:

   ```bash
   mkdir myapp
   cd myapp
   ```

2. Utilizzare il comando npm `init` per creare un file **package.json** per l'applicazione. Questo comando richiede varie informazioni, tra cui il nome e la versione dell'applicazione e il nome del file del punto di ingresso iniziale, che per impostazione predefinita è **index.js**. Per ora, accettare semplicemente i valori predefiniti:

   ```bash
   npm init
   ```

   Se viene visualizzato il file **package.json** (`cat package.json`), saranno visibili i valori predefiniti accettati, che terminano con la licenza.

   ```json
   {
     "name": "myapp",
     "version": "1.0.0",
     "main": "index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "author": "",
     "license": "ISC",
     "description": ""
   }
   ```

3. Ora installare Express nella directory `myapp` e salvarlo nell'elenco delle dipendenze del file **package.json**:

   ```bash
   npm install express
   ```

   La sezione delle dipendenze del file **package.json** verrà ora visualizzata alla fine del file e includerà _Express_.

   ```json
   {
     "name": "myapp",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "author": "",
     "license": "ISC",
     "dependencies": {
       "express": "^5.1.0"
     }
   }
   ```

4. Per utilizzare la libreria Express, chiamare la funzione `require()` nel file **index.js** per includerla nell'applicazione.
   Creare ora questo file nella radice della directory dell'applicazione "myapp" e assegnargli il contenuto seguente:

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

   Questo codice mostra un'applicazione web Express minima "HelloWorld".
   Importa il modulo "express" utilizzando `require()` e lo usa per creare un server (`app`) che rimane in ascolto delle richieste HTTP sulla porta 3000 e stampa un messaggio nella console che indica quale URL del browser può essere utilizzato per testare il server.
   La funzione `app.get()` risponde solo alle richieste HTTP `GET` con il percorso URL specificato ('/'), in questo caso chiamando una funzione per inviare il messaggio _Hello World!_.

   > [!NOTE]
   > I backtick in `` `Example app listening on port ${port}!` `` consentono di interpolare il valore di `$port` nella stringa.

5. È possibile avviare il server chiamando node con lo script nel prompt dei comandi:

   ```bash
   node index.js
   ```

   Verrà visualizzato il seguente output nella console:

   ```plain
   Example app listening on port 3000
   ```

6. Passare all'URL `http://localhost:3000/`.
   Se tutto funziona correttamente, il browser dovrebbe visualizzare la stringa "Hello World!".

### Dipendenze di sviluppo

Se una dipendenza viene utilizzata solo durante lo sviluppo, dovrebbe invece essere salvata come "dipendenza di sviluppo", in modo che gli utenti del pacchetto non debbano installarla in produzione. Ad esempio, per utilizzare il popolare strumento di linting JavaScript [ESLint](https://eslint.org/), npm verrebbe chiamato come mostrato:

```bash
npm install eslint --save-dev
```

La seguente voce verrebbe quindi aggiunta al file **package.json** dell'applicazione:

```json
{
  "devDependencies": {
    "eslint": "^9.30.1"
  }
}
```

> [!NOTE]
> I "[linter](<https://en.wikipedia.org/wiki/Lint_(software)>)" sono strumenti che eseguono analisi statica del software per riconoscere e segnalare l'aderenza o la mancata aderenza a un insieme di buone pratiche di codifica.

### Esecuzione di attività

Oltre a definire e recuperare dipendenze, è possibile definire script con nome nei file **package.json** e chiamare npm per eseguirli con il comando [run-script](https://docs.npmjs.com/cli/commands/npm-run/). Questo approccio viene comunemente utilizzato per automatizzare l'esecuzione di test e parti della toolchain di sviluppo o build, ad esempio eseguendo strumenti per minimizzare JavaScript, ridurre le immagini, eseguire LINT/analizzare il codice e così via.

> [!NOTE]
> Per eseguire test e altri strumenti esterni possono essere utilizzati anche task runner come [Gulp](https://gulpjs.com/) e [Grunt](https://gruntjs.com/).

Ad esempio, per definire uno script che esegua la dipendenza di sviluppo _eslint_ specificata nella sezione precedente, si potrebbe aggiungere il blocco script seguente al file **package.json**, supponendo che il codice sorgente dell'applicazione si trovi in una cartella `/src/js`:

```json
{
  "scripts": {
    // …
    "lint": "eslint src/js"
    // …
  }
}
```

Per spiegare più dettagliatamente, `eslint src/js` è un comando che può essere immesso nel terminale/riga di comando per eseguire `eslint` sui file JavaScript contenuti nella directory `src/js` all'interno della directory dell'app. Includere quanto sopra nel file package.json dell'app fornisce una scorciatoia per questo comando: `lint`.

Sarebbe quindi possibile eseguire _eslint_ usando npm chiamando:

```bash
npm run-script lint
# OR (using the alias)
npm run lint
```

Questo esempio potrebbe non sembrare più breve del comando originale, ma negli script npm possono essere inclusi comandi molto più grandi, comprese catene di più comandi. Si potrebbe identificare un singolo script npm che esegue tutti i test contemporaneamente.

## Installazione dell'Express Application Generator

Lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator/) genera uno "scheletro" di applicazione Express. Installare il generatore utilizzando npm come mostrato:

```bash
npm install express-generator -g
```

> [!NOTE]
> Su Ubuntu o macOS potrebbe essere necessario anteporre `sudo` a questa riga. Il flag `-g` installa lo strumento globalmente, in modo che possa essere chiamato da qualsiasi posizione.

Per creare un'app _Express_ denominata "helloworld" con le impostazioni predefinite, passare alla posizione in cui si desidera crearla ed eseguire l'app come mostrato:

```bash
express helloworld
```

> [!NOTE]
> A meno che non venga utilizzata una vecchia versione di nodejs (< 8.2.0), è possibile in alternativa saltare l'installazione ed eseguire express-generator con [npx](https://github.com/npm/npx#readme).
> Questo ha lo stesso effetto dell'installazione e della successiva esecuzione di `express-generator`, ma non installa il pacchetto nel sistema:
>
> ```bash
> npx express-generator helloworld
> ```

È possibile specificare anche la libreria di template da utilizzare e varie altre impostazioni.
Utilizzare il comando `help` per visualizzare tutte le opzioni:

```bash
express --help
```

Il generatore creerà la nuova app Express in una sottocartella della posizione corrente, mostrando l'avanzamento della build nella console.
Al termine, lo strumento visualizzerà i comandi da immettere per installare le dipendenze Node e avviare l'app.

La nuova app avrà un file **package.json** nella sua directory radice.
È possibile aprirlo per vedere quali dipendenze sono installate, inclusi Express e la libreria di template Jade:

```json
{
  "name": "helloworld",
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
    "jade": "~1.11.0",
    "morgan": "~1.9.1"
  }
}
```

Installare tutte le dipendenze per l'app helloworld utilizzando npm come mostrato:

```bash
cd helloworld
npm install
```

Quindi eseguire l'app, tenendo presente che i comandi sono leggermente diversi per Windows e Linux/macOS, come mostrato di seguito:

```bash
# Run helloworld on Windows with Command Prompt
SET DEBUG=helloworld:* & npm start

# Run helloworld on Windows with PowerShell
SET DEBUG=helloworld:* | npm start

# Run helloworld on Linux/macOS
DEBUG=helloworld:* npm start
```

Il comando DEBUG crea log utili, producendo un output simile al seguente:

```bash
>SET DEBUG=helloworld:* & npm start

> helloworld@0.0.0 start D:\GitHub\express-tests\helloworld
> node ./bin/www

  helloworld:server Listening on port 3000 +0ms
```

Aprire un browser e passare a `http://localhost:3000/` per visualizzare la pagina di benvenuto predefinita di Express.

![Express - Schermata predefinita dell'app generata](express_default_screen.png)

L'app generata verrà trattata più approfonditamente nell'articolo sulla generazione di un'applicazione scheletrica.

## Riepilogo

Ora sul computer è disponibile un ambiente di sviluppo Node funzionante che può essere utilizzato per creare applicazioni web Express. È stato inoltre illustrato come npm possa essere utilizzato per importare Express in un'applicazione e come creare applicazioni utilizzando lo strumento Express Application Generator per poi eseguirle.

Nel prossimo articolo verrà avviato un tutorial per creare un'applicazione web completa utilizzando questo ambiente e gli strumenti associati.

## Vedere anche

- Pagina [Downloads](https://nodejs.org/en/download) (nodejs.org)
- [Installazione di Express](https://expressjs.com/en/starter/installing/) (expressjs.com)
- [Express Application Generator](https://expressjs.com/en/starter/generator/) (expressjs.com)
- [Utilizzo di Node.js con il sottosistema Windows per Linux](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/) (docs.microsoft.com)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
