---
title: Configurare un ambiente di sviluppo Node
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che sai a cosa serve [Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#introducing_express), vedremo come configurare e testare un ambiente di sviluppo Node/Express su Windows, Linux (Ubuntu) o macOS. Per ciascuno di questi sistemi operativi, questo articolo fornisce ciò che è necessario per iniziare a sviluppare applicazioni Express.

> [!WARNING]
> Il tutorial di Express è scritto per Express versione 4, mentre l'ultima versione è Express 5.
> Abbiamo pianificato di aggiornare la documentazione per supportare Express 5 nella seconda metà del 2025. Fino ad allora, abbiamo aggiornato i comandi di installazione affinché installino Express 4 anziché l'ultima versione, per evitare potenziali problemi di compatibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Sapere come aprire un terminale / riga di comando. Sapere come installare pacchetti software sul sistema operativo del computer di sviluppo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Configurare un ambiente di sviluppo per Express sul tuo computer.</td>
    </tr>
  </tbody>
</table>

## Panoramica dell'ambiente di sviluppo Express

_Node_ e _Express_ rendono molto semplice configurare il tuo computer per iniziare a sviluppare applicazioni web. Questa sezione fornisce una panoramica degli strumenti necessari, spiega alcuni dei metodi più semplici per installare Node (e Express) su Ubuntu, macOS e Windows, e mostra come puoi testare la tua installazione.

### Cos'è l'ambiente di sviluppo Express?

L'ambiente di sviluppo _Express_ include un'installazione di _Nodejs_, del _gestore dei pacchetti npm_, e (opzionalmente) del _Generatore di Applicazioni Express_ sul tuo computer locale.

_Node_ e il gestore di pacchetti _npm_ vengono installati insieme da pacchetti binari preconfezionati, installer, gestori di pacchetti di sistema operativo o da sorgente (come mostrato nelle sezioni seguenti). _Express_ viene quindi installato da npm come dipendenza delle singole applicazioni web _Express_ (insieme ad altre librerie come motori di template, driver di database, middleware per l'autenticazione, middleware per servire file statici, ecc.).

_npm_ può anche essere utilizzato per installare (globalmente) il _Generatore di Applicazioni Express_, uno strumento utile per creare app scheletro _Express_ che seguono il {{Glossary("MVC", "pattern MVC")}}. Il generatore di applicazioni è opzionale perché non è _necessario_ usare questo strumento per creare app che utilizzano Express, o costruire app Express che abbiano lo stesso layout architetturale o dipendenze. Tuttavia, lo useremo perché rende l'inizio molto più semplice e promuove una struttura di applicazione modulare.

> [!NOTE]
> A differenza di alcuni altri framework web, l'ambiente di sviluppo non include un server web di sviluppo separato. In _Node_/_Express_ un'applicazione web crea ed esegue il proprio server web!

Ci sono altri strumenti periferici che fanno parte di un tipico ambiente di sviluppo, inclusi [editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o IDE per modificare il codice, e strumenti di gestione del controllo del codice sorgente come [Git](https://git-scm.com/) per gestire in modo sicuro le diverse versioni del tuo codice. Si presuppone che tu abbia già installato questo tipo di strumenti (in particolare un editor di testo).

### Quali sistemi operativi sono supportati?

_Node_ può essere eseguito su Windows, macOS, molte varianti di Linux, Docker, ecc. C'è un elenco completo nella pagina di [Download di Node.js](https://nodejs.org/en/download). Quasi tutti i computer personali dovrebbero avere le prestazioni necessarie per eseguire Node durante lo sviluppo. _Express_ è eseguito in un ambiente _Node_ e quindi può essere eseguito su qualsiasi piattaforma che esegue _Node_.

In questo articolo forniamo istruzioni di configurazione per Windows, macOS e Ubuntu Linux.

### Quale versione di Node/Express dovresti usare?

Ci sono molte [versioni di Node](https://nodejs.org/en/blog/release/) — le versioni più recenti contengono correzioni di bug, supporto per versioni più recenti degli standard ECMAScript (JavaScript) e miglioramenti alle API di Node.

Generalmente dovresti usare la versione più recente _LTS (supportato a lungo termine)_ poiché sarà più stabile rispetto alla versione "corrente" pur avendo caratteristiche relativamente recenti (e continua a essere attivamente mantenuta). Dovresti usare la versione _Current_ se hai bisogno di una funzionalità non presente nella versione LTS.

Per _Express_ dovresti sempre usare l'ultima versione disponibile.

### Che dire di database e altre dipendenze?

Altre dipendenze, come driver di database, motori di template, motori di autenticazione, ecc. fanno parte dell'applicazione e vengono importate nell'ambiente dell'applicazione usando il gestore dei pacchetti npm. Ne discuteremo in articoli specifici per le applicazioni più avanti.

## Installazione di Node

Per poter utilizzare _Express_ devi installare _Nodejs_ e il [Node Package Manager (npm)](https://docs.npmjs.com/) sul tuo sistema operativo.
Per facilitare questo processo, installeremo prima un gestore di versioni per node, e poi lo useremo per installare le ultime versioni supportate a lungo termine (LTS) di node e npm.

> [!NOTE]
> Puoi anche installare nodejs e npm con gli installer forniti su <https://nodejs.org/en/> (seleziona il pulsante per scaricare la build LTS "Raccomandata per la maggior parte degli utenti"), oppure puoi [installare usando il gestore di pacchetti del tuo sistema operativo](https://nodejs.org/en/download) (nodejs.org).
> Raccomandiamo vivamente l'uso di un gestore di versioni per node poiché rendono più facile installare, aggiornare e passare tra qualsiasi versione specifica di node e npm.

### Windows

Esistono diversi gestori di versioni per node su Windows.
Qui usiamo [nvm-windows](https://github.com/coreybutler/nvm-windows), che è altamente rispettato tra gli sviluppatori Node.

Installa l'ultima versione utilizzando l'installer di tua scelta dalla pagina [nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases).
Dopo che `nvm-windows` è stato installato, apri un prompt dei comandi (o PowerShell) ed inserisci il seguente comando per scaricare la versione LTS più recente di nodejs e npm:

```bash
nvm install lts
```

Al momento della scrittura, la versione LTS di nodejs è la 20.11.0.
Puoi impostarla come _versione corrente_ da utilizzare con il comando seguente:

```bash
nvm use 20.11.0
```

> [!NOTE]
> Se ricevi avvisi "Access Denied", dovrai eseguire questo comando in un prompt con permessi di amministratore.

Usa il comando `nvm --help` per scoprire altre opzioni della riga di comando, come elencare tutte le versioni di Node disponibili e tutte le versioni scaricate di NVM.

### Ubuntu e macOS

Esistono diversi gestori di versioni per node su Ubuntu e macOS.
[nvm](https://github.com/nvm-sh/nvm) è uno dei più popolari ed è la versione originale su cui è basato `nvm-windows`.
Vedi [nvm > Install & Update Script](https://github.com/nvm-sh/nvm#install--update-script) per le istruzioni di terminale per installare l'ultima versione di nvm.

Dopo che `nvm` è stato installato, apri un terminale e inserisci il seguente comando per scaricare la versione LTS più recente di nodejs e npm:

```bash
nvm install --lts
```

Al momento della scrittura, la versione LTS di nodejs è la 20.11.0.
Il comando `nvm list` mostra l'insieme delle versioni scaricate e la versione corrente.
Puoi impostare una versione particolare come _versione corrente_ con il comando seguente (lo stesso di `nvm-windows`).

```bash
nvm use 20.11.0
```

Usa il comando `nvm --help` per scoprire altre opzioni della riga di comando.
Queste sono spesso simili, o le stesse, di quelle offerte da `nvm-windows`.

### Testare la tua installazione di Nodejs e npm

Una volta che hai impostato `nvm` per utilizzare una particolare versione di node, puoi testare l'installazione.
Un buon modo per farlo è usare il comando "version" nel tuo terminale/prompt dei comandi e verificare che la stringa di versione attesa venga restituita:

```bash
> node -v
v20.11.0
```

Anche il gestore di pacchetti _npm_ di _Nodejs_ dovrebbe essere stato installato e può essere testato nello stesso modo:

```bash
> npm -v
10.2.4
```

Come test leggermente più eccitante, creiamo un "server puro di Node" di base che stampa "Hello World" nel browser quando visiti l'URL corretto nel tuo browser:

1. Copia il seguente testo in un file chiamato **hellonode.js**. Questo utilizza funzionalità pure di Node (niente da Express):

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

   Il codice importa il modulo "http" e lo usa per creare un server (`createServer()`) che ascolta richieste HTTP sulla porta 3000. Lo script quindi stampa un messaggio sulla console riguardo a quale URL del browser puoi usare per testare il server. La funzione `createServer()` prende come argomento una funzione di callback che verrà invocata quando si riceve una richiesta HTTP — questa restituisce una risposta con un codice di stato HTTP 200 ("OK") e il testo semplice "Hello World".

   > [!NOTE]
   > Non preoccuparti se non capisci esattamente cosa sta facendo questo codice! Spiegheremo il nostro codice in modo più dettagliato una volta che inizieremo a usare Express!

2. Avvia il server navigando nella stessa directory del tuo file `hellonode.js` nel tuo prompt dei comandi e chiamando `node` insieme al nome dello script, come segue:

   ```bash
   node hellonode.js
   ```

   Una volta avviato il server, vedrai l'output della console che indica l'indirizzo IP su cui il server è in esecuzione:

   ```plain
   Server running at http://127.0.0.1:3000/
   ```

3. Naviga all'URL `http://127.0.0.1:3000`. Se tutto funziona, il browser dovrebbe visualizzare la stringa "Hello World".

## Utilizzare npm

Accanto a _Node_ stesso, [npm](https://docs.npmjs.com/) è lo strumento più importante per lavorare con applicazioni _Node_.
`npm` viene usato per scaricare tutti i pacchetti (librerie JavaScript) che un'applicazione necessita per lo sviluppo, il test e/o la produzione, e può essere usato anche per eseguire test e strumenti utilizzati nel processo di sviluppo.

> [!NOTE]
> Dal punto di vista di Node, _Express_ è solo un altro pacchetto che è necessario installare usando npm e poi richiedere nel proprio codice.

Puoi usare npm manualmente per scaricare separatamente ciascun pacchetto necessario. Tipicamente gestiamo invece le dipendenze usando un file di definizione in testo semplice chiamato [package.json](https://docs.npmjs.com/files/package.json/). Questo file elenca tutte le dipendenze per un "pacchetto" JavaScript specifico, inclusi il nome del pacchetto, la versione, la descrizione, il file iniziale da eseguire, le dipendenze di produzione, le dipendenze di sviluppo, le versioni di _Node_ con cui può funzionare, ecc. Il file **package.json** dovrebbe contenere tutto ciò che npm necessita per scaricare ed eseguire la tua applicazione (se stavi scrivendo una libreria riutilizzabile potresti usare questa definizione per caricare il tuo pacchetto nel repository npm e renderlo disponibile per altri utenti).

### Aggiunta di dipendenze

I passaggi seguenti mostrano come puoi usare npm per scaricare un pacchetto, salvarlo nelle dipendenze del progetto e quindi richiederlo in un'applicazione Node.

> [!NOTE]
> Qui mostriamo le istruzioni per scaricare e installare il pacchetto _Express_. Più avanti mostreremo come questo pacchetto, e altri, sono già specificati per noi usando il _Generatore di Applicazioni Express_. Questa sezione è fornita perché è utile capire come funziona npm e cosa viene creato dal generatore di applicazioni.

1. Per prima cosa crea una directory per la tua nuova applicazione e naviga al suo interno:

   ```bash
   mkdir myapp
   cd myapp
   ```

2. Usa il comando npm `init` per creare un file **package.json** per la tua applicazione. Questo comando ti chiede diverse cose, tra cui il nome e la versione della tua applicazione e il nome del file del punto di ingresso iniziale (per impostazione predefinita è **index.js**). Per ora, accetta i valori predefiniti:

   ```bash
   npm init
   ```

   Se mostri il file **package.json** (`cat package.json`), vedrai i valori predefiniti che hai accettato, terminando con la licenza.

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
     "license": "ISC"
   }
   ```

3. Ora installa Express nella directory `myapp` e salvalo nell'elenco delle dipendenze del tuo file **package.json**:

   ```bash
   npm install express@^4.21.2
   ```

   La sezione dipendenze del tuo **package.json** apparirà ora alla fine del file **package.json** e includerà _Express_.

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
       "express": "^4.21.2"
     }
   }
   ```

4. Per usare la libreria Express devi chiamare la funzione `require()` nel tuo file **index.js** per includerla nella tua applicazione.
   Crea ora questo file, nella radice della directory dell'applicazione "myapp", e dagli il seguente contenuto:

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

   Questo codice mostra una "HelloWorld" web application minima con Express.
   Questo importa il modulo "express" usando `require()` e lo usa per creare un server (`app`) che ascolta richieste HTTP sulla porta 3000 e stampa un messaggio sulla console spiegando quale URL di browser puoi usare per testare il server.
   La funzione `app.get()` risponde solo a richieste HTTP `GET` con il percorso URL specificato ('/'), in questo caso chiamando una funzione per inviare il nostro messaggio _Hello World!_.

   > [!NOTE]
   > I backtick in `` `Example app listening on port ${port}!` `` ci permettono di interpolare il valore di `$port` nella stringa.

5. Puoi avviare il server chiamando node con lo script nel tuo prompt dei comandi:

   ```bash
   node index.js
   ```

   Vedrai il seguente output della console:

   ```plain
   Example app listening on port 3000
   ```

6. Naviga all'URL `http://localhost:3000/`.
   Se tutto funziona, il browser dovrebbe visualizzare la stringa "Hello World!".

### Dipendenze di sviluppo

Se una dipendenza viene utilizzata solo durante lo sviluppo, dovresti invece salvarla come "dipendenza di sviluppo" (in modo che gli utenti del tuo pacchetto non debbano installarla in produzione). Ad esempio, per usare lo strumento di linting JavaScript popolare [ESLint](https://eslint.org/), chiameresti npm come mostrato:

```bash
npm install eslint --save-dev
```

La seguente voce verrebbe quindi aggiunta al file **package.json** della tua applicazione:

```json
  "devDependencies": {
    "eslint": "^7.10.0"
  }
```

> [!NOTE]
> I "[Linters](<https://en.wikipedia.org/wiki/Lint_(software)>)" sono strumenti che eseguono analisi statiche su software per riconoscere e segnalare l'aderenza/non aderenza a un insieme di buone pratiche di codifica.

### Esecuzione di attività

Oltre a definire e scaricare le dipendenze, puoi anche definire script _nominati_ nei tuoi file **package.json** e chiamare npm per eseguirli con il comando [run-script](https://docs.npmjs.com/cli/run-script/). Questo approccio è comunemente usato per automatizzare l'esecuzione di test e parti della pipeline di sviluppo o build (ad esempio, eseguire strumenti per minimizzare JavaScript, ridurre immagini, LINT/analizzare il tuo codice, ecc.).

> [!NOTE]
> I task runners come [Gulp](https://gulpjs.com/) e [Grunt](https://gruntjs.com/) possono anche essere usati per eseguire test e altri strumenti esterni.

Ad esempio, per definire uno script per eseguire la dipendenza di sviluppo _eslint_ che abbiamo specificato nella sezione precedente potremmo aggiungere il seguente blocco di script al nostro file **package.json** (supponendo che il nostro codice sorgente dell'applicazione sia in una cartella /src/js):

```json
"scripts": {
  // …
  "lint": "eslint src/js"
  // …
}
```

Per spiegare un po' più nel dettaglio, `eslint src/js` è un comando che potremmo inserire nel nostro terminale/riga di comando per eseguire `eslint` sui file JavaScript contenuti nella directory `src/js` all'interno della nostra directory dell'app. Includendo quanto sopra all'interno del file package.json della nostra app fornisce un collegamento per questo comando — `lint`.

Saremmo poi in grado di eseguire _eslint_ utilizzando npm chiamando:

```bash
npm run-script lint
# OR (using the alias)
npm run lint
```

Questo esempio potrebbe non sembrare più corto rispetto al comando originale, ma puoi includere comandi molto più grandi all'interno dei tuoi script npm, inclusi catene di più comandi. Puoi identificare un singolo script npm che esegue tutti i tuoi test in una sola volta.

## Installare il Generatore di Applicazioni Express

Lo strumento [Generatore di Applicazioni Express](https://expressjs.com/en/starter/generator.html) genera uno "scheletro" di applicazione Express. Installa il generatore usando npm come mostrato:

```bash
npm install express-generator -g
```

> [!NOTE]
> Potrebbe essere necessario prefissare questa riga con `sudo` su Ubuntu o macOS. Il flag `-g` installa lo strumento globalmente in modo che tu possa chiamarlo da qualsiasi posizione.

Per creare un'app _Express_ chiamata "helloworld" con le impostazioni predefinite, naviga dove vuoi crearla e esegui l'app come mostrato:

```bash
express helloworld
```

> [!NOTE]
> A meno che tu non stia usando una vecchia versione di nodejs (< 8.2.0), potresti alternativamente saltare l'installazione ed eseguire express-generator con [npx](https://github.com/npm/npx#readme).
> Questo ha lo stesso effetto di installare e quindi eseguire `express-generator` ma non installa il pacchetto sul tuo sistema:
>
> ```bash
> npx express-generator helloworld
> ```

Puoi anche specificare la libreria di template da usare e un numero di altre impostazioni.
Usa il comando `help` per vedere tutte le opzioni:

```bash
express --help
```

Il generatore creerà la nuova app Express in una sottocartella della tua posizione corrente, mostrando i progressi di costruzione sulla console.
Al completamento, lo strumento mostrerà i comandi che devi inserire per installare le dipendenze Node e avviare l'app.

La nuova app avrà un file **package.json** nella directory principale.
Puoi aprirlo per vedere quali dipendenze sono installate, inclusi Express e la libreria di template Jade:

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

Installa tutte le dipendenze per l'app helloworld usando npm come mostrato:

```bash
cd helloworld
npm install
```

Quindi esegui l'app (i comandi sono leggermente diversi per Windows e Linux/macOS), come mostrato di seguito:

```bash
# Run helloworld on Windows with Command Prompt
SET DEBUG=helloworld:* & npm start

# Run helloworld on Windows with PowerShell
SET DEBUG=helloworld:* | npm start

# Run helloworld on Linux/macOS
DEBUG=helloworld:* npm start
```

Il comando DEBUG crea un log utile, risultante in un output simile al seguente:

```bash
>SET DEBUG=helloworld:* & npm start

> helloworld@0.0.0 start D:\GitHub\express-tests\helloworld
> node ./bin/www

  helloworld:server Listening on port 3000 +0ms
```

Apri un browser e naviga a `http://localhost:3000/` per vedere la pagina di benvenuto predefinita di Express.

![Express - Schermata predefinita dell'app generata](express_default_screen.png)

Parleremo di più riguardo l'app generata quando arriveremo all'articolo sulla generazione di un'applicazione scheletro.

## Riepilogo

Ora hai un ambiente di sviluppo Node configurato sul tuo computer che può essere utilizzato per creare applicazioni web Express. Hai anche visto come npm può essere utilizzato per importare Express in un'applicazione, e anche come puoi creare applicazioni usando lo strumento di Generazione Applicazioni Express e quindi eseguirle.

Nel prossimo articolo inizieremo a lavorare attraverso un tutorial per costruire un'applicazione web completa usando questo ambiente e gli strumenti associati.

## Vedi anche

- [Pagina dei Download](https://nodejs.org/en/download) (nodejs.org)
- [Installazione di Express](https://expressjs.com/en/starter/installing.html) (expressjs.com)
- [Generatore di Applicazioni Express](https://expressjs.com/en/starter/generator.html) (expressjs.com)
- [Uso di Node.js con sottosistema Windows per Linux](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/) (docs.microsoft.com)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
