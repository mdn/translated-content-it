---
title: Impostare un ambiente di sviluppo Node
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che sa cos'è [Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#introducing_express), le mostreremo come impostare e testare un ambiente di sviluppo Node/Express su Windows, o Linux (Ubuntu), o macOS. Per ciascuno di questi sistemi operativi, questo articolo fornisce ciò che è necessario per iniziare a sviluppare applicazioni Express.

> [!WARNING]
> Il tutorial di Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Abbiamo in programma di aggiornare la documentazione per supportare Express 5 nella seconda metà del 2025. Fino ad allora, abbiamo aggiornato i comandi di installazione in modo che installino Express 4 piuttosto che l'ultima versione, per evitare potenziali problemi di compatibilità.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Se si sa come aprire un terminale / riga di comando. Se si sa come installare pacchetti software sul sistema operativo del computer di sviluppo.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>Impostare un ambiente di sviluppo per Express sul suo computer.</td>
    </tr>
  </tbody>
</table>

## Panoramica dell'ambiente di sviluppo Express

_Node_ ed _Express_ rendono molto facile configurare il proprio computer per iniziare a sviluppare applicazioni web. Questa sezione offre una panoramica degli strumenti necessari, spiega alcuni dei metodi più semplici per installare Node (ed Express) su Ubuntu, macOS e Windows, e mostra come testare l'installazione.

### Cos'è l'ambiente di sviluppo Express?

L'ambiente di sviluppo _Express_ include un'installazione di _Nodejs_, il _gestore di pacchetti npm_ e (facoltativamente) l'_Express Application Generator_ sul computer locale.

_Node_ e il gestore di pacchetti _npm_ sono installati insieme da pacchetti binari preconfezionati, installer, gestori di pacchetti del sistema operativo o dal codice sorgente (come mostrato nelle sezioni seguenti). _Express_ viene poi installato da npm come dipendenza delle singole applicazioni web _Express_ (insieme ad altre librerie come i motori di template, i driver del database, middleware di autenticazione, middleware per servire file statici, ecc.).

_npm_ può essere anche utilizzato per installare (globalmente) l'_Express Application Generator_, uno strumento utile per creare scheletri di applicazioni web _Express_ che seguono il {{Glossary("MVC", "pattern MVC")}}. Il generatore di applicazioni è facoltativo perché non è _necessario_ utilizzare questo strumento per creare applicazioni che utilizzano Express, o costruire applicazioni Express che seguono lo stesso modello architettonico o le stesse dipendenze. Tuttavia, lo useremo perché facilita l'inizio e promuove una struttura applicativa modulare.

> [!NOTE]
> A differenza di alcuni altri framework web, l'ambiente di sviluppo non include un server web di sviluppo separato. In _Node_/_Express_ un'applicazione web crea e gestisce il proprio server web!

Ci sono altri strumenti periferici che fanno parte di un tipico ambiente di sviluppo, inclusi [editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o IDE per modificare il codice, e strumenti di gestione del controllo del codice sorgente come [Git](https://git-scm.com/) per gestire in sicurezza diverse versioni del codice. Stiamo assumendo che lei abbia già installato questo tipo di strumenti (in particolare un editor di testo).

### Quali sistemi operativi sono supportati?

_Node_ può essere eseguito su Windows, macOS, molte versioni di Linux, Docker, ecc. C'è un elenco completo sulla pagina dei [Download](https://nodejs.org/en/download) di Node.js. Quasi qualsiasi computer personale dovrebbe avere le prestazioni necessarie per eseguire Node durante lo sviluppo. _Express_ è eseguito in un ambiente _Node_ e quindi può essere eseguito su qualsiasi piattaforma che esegue _Node_.

In questo articolo forniamo istruzioni di configurazione per Windows, macOS e Ubuntu Linux.

### Quale versione di Node/Express dovrebbe usare?

Ci sono molte [versioni di Node](https://nodejs.org/en/blog/release/) — le versioni più recenti contengono correzioni di bug, supporto per le versioni più recenti degli standard ECMAScript (JavaScript), e miglioramenti alle API di Node.

Generalmente si dovrebbe utilizzare la versione più recente _LTS (supporto a lungo termine)_ perché sarà più stabile della versione "corrente" pur avendo funzionalità relativamente recenti (ed è ancora mantenuta attivamente). Si dovrebbe usare la versione _Corrente_ se si ha bisogno di una funzionalità non presente nella versione LTS.

Per _Express_ dovrebbe sempre utilizzare l'ultima versione.

### E per quanto riguarda i database e altre dipendenze?

Altre dipendenze, come i driver di database, i motori di template, i motori di autenticazione, ecc., fanno parte dell'applicazione, e sono importati nell'ambiente dell'applicazione utilizzando il gestore di pacchetti npm. Ne discuteremo in articoli specifici per le app più avanti.

## Installare Node

Per utilizzare _Express_ dovrà installare _Nodejs_ e il [Node Package Manager (npm)](https://docs.npmjs.com/) sul suo sistema operativo.
Per semplificare questo processo, installeremo prima un gestore di versioni di node, e quindi lo useremo per installare le più recenti versioni LTS (Long Term Supported) di node e npm.

> [!NOTE]
> È possibile installare nodejs e npm anche con gli installatori forniti su <https://nodejs.org/en/> (selezioni il pulsante per scaricare la build LTS "Consigliata per la maggior parte degli utenti"), oppure [installando tramite il gestore di pacchetti per il suo sistema operativo](https://nodejs.org/en/download) (nodejs.org).
> Consigliamo vivamente di utilizzare un gestore di versioni di node poiché questi rendono più facile installare, aggiornare e passare tra una versione particolare di node e npm.

### Windows

Esistono diversi gestori di versioni di node per Windows.
Qui utilizziamo [nvm-windows](https://github.com/coreybutler/nvm-windows), che è molto rispettato tra gli sviluppatori node.

Installi la versione più recente utilizzando l'installer di sua scelta dalla pagina [nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases).
Una volta completata l'installazione di `nvm-windows`, apra un prompt dei comandi (o PowerShell) ed inserisca il seguente comando per scaricare l'ultima versione LTS di nodejs e npm:

```bash
nvm install lts
```

Al momento della scrittura, la versione LTS di nodejs è 20.11.0.
Può impostare questa come _versione corrente_ da usare con il comando seguente:

```bash
nvm use 20.11.0
```

> [!NOTE]
> Se riceve avvisi di "Accesso negato", dovrà eseguire questo comando in un prompt con permessi di amministrazione.

Utilizzi il comando `nvm --help` per scoprire altre opzioni della riga di comando, come elencare tutte le versioni di node disponibili, e tutte le versioni di NVM scaricate.

### Ubuntu e macOS

Esistono diversi gestori di versioni di node per Ubuntu e macOS.
[nvm](https://github.com/nvm-sh/nvm) è uno dei più popolari e la versione originale su cui è basato `nvm-windows`.
Consultare [nvm > Install & Update Script](https://github.com/nvm-sh/nvm#install--update-script) per le istruzioni del terminale per installare l'ultima versione di nvm.

Dopo aver installato `nvm`, apra un terminale ed inserisca il seguente comando per scaricare l'ultima versione LTS di nodejs e npm:

```bash
nvm install --lts
```

Al momento della scrittura, la versione LTS di nodejs è 20.11.0.
Il comando `nvm list` mostra il set di versioni scaricato e la versione attuale.
Può impostare una particolare versione come _versione corrente_ con il comando seguente (lo stesso di `nvm-windows`)

```bash
nvm use 20.11.0
```

Utilizzi il comando `nvm --help` per scoprire altre opzioni della riga di comando.
Queste sono spesso simili o uguali a quelle offerte da `nvm-windows`.

### Testare l'installazione di Nodejs e npm

Una volta impostato `nvm` per utilizzare una particolare versione di node, può testare l'installazione.
Un buon modo per farlo è utilizzare il comando "version" nel suo terminale/prompt dei comandi e verificare che la stringa della versione prevista venga restituita:

```bash
> node -v
v20.11.0
```

Anche il gestore di pacchetti _npm_ di _Nodejs_ dovrebbe essere stato installato, e può essere testato allo stesso modo:

```bash
> npm -v
10.2.4
```

Per un test leggermente più entusiasmante, creiamo un server "pure node" molto semplice che stampa "Hello World" nel browser quando visita l'URL corretto nel suo browser:

1. Copi il testo seguente in un file denominato **hellonode.js**. Questo utilizza funzioni pure di Node (nulla da Express):

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

   Il codice importa il modulo "http" e lo utilizza per creare un server (`createServer()`) che ascolta le richieste HTTP sulla porta 3000. Lo script poi stampa un messaggio sulla console riguardo l'URL del browser che può utilizzare per testare il server. La funzione `createServer()` prende come argomento una funzione di callback che verrà invocata quando viene ricevuta una richiesta HTTP — questa restituisce una risposta con un codice di stato HTTP di 200 ("OK") e il testo semplice "Hello World".

   > [!NOTE]
   > Non si preoccupi se non capisce esattamente cosa fa questo codice al momento! Spiegheremo il nostro codice in modo più dettagliato una volta che inizieremo a usare Express!

2. Avvii il server entrando nella stessa directory del suo file `hellonode.js` nel prompt dei comandi e chiamando `node` insieme al nome dello script, in questo modo:

   ```bash
   node hellonode.js
   ```

   Una volta avviato il server, vedrà una stampa in console che indica l'indirizzo IP su cui il server è in esecuzione:

   ```plain
   Server running at http://127.0.0.1:3000/
   ```

3. Navighi all'URL `http://127.0.0.1:3000`. Se tutto funziona, il browser dovrebbe mostrare la stringa "Hello World".

## Usare npm

Accanto a _Node_ stesso, [npm](https://docs.npmjs.com/) è lo strumento più importante per lavorare con applicazioni _Node_.
`npm` viene utilizzato per recuperare tutti i pacchetti (librerie JavaScript) di cui un'applicazione ha bisogno per lo sviluppo, il test e/o la produzione, e può anche essere utilizzato per eseguire test e strumenti utilizzati nel processo di sviluppo.

> [!NOTE]
> Dal punto di vista di Node, _Express_ è solo un altro pacchetto che è necessario installare utilizzando npm e poi richiedere nel proprio codice.

Può utilizzare manualmente npm per recuperare separatamente ciascun pacchetto necessario. Tipicamente gestiamo invece le dipendenze utilizzando un file di definizione di testo piano denominato [package.json](https://docs.npmjs.com/files/package.json/). Questo file elenca tutte le dipendenze per un specifico "pacchetto" JavaScript, inclusi il nome del pacchetto, la versione, la descrizione, il file iniziale da eseguire, le dipendenze di produzione, le dipendenze di sviluppo, le versioni di _Node_ con cui può funzionare, ecc. Il file **package.json** dovrebbe contenere tutto ciò di cui npm ha bisogno per recuperare ed eseguire la sua applicazione (se stesse scrivendo una libreria riutilizzabile potrebbe usare questa definizione per caricare il suo pacchetto nel repository npm e renderlo disponibile per altri utenti).

### Aggiungere dipendenze

I seguenti passaggi mostrano come può utilizzare npm per scaricare un pacchetto, salvarlo nell'elenco delle dipendenze del progetto e poi richiederlo in un'applicazione Node.

> [!NOTE]
> Qui mostriamo le istruzioni per recuperare e installare il pacchetto _Express_. Più avanti mostreremo come questo pacchetto, e altri, sono già specificati per noi utilizzando l'_Express Application Generator_. Questa sezione è fornita perché è utile comprendere come funziona npm e cosa viene creato dal generatore di applicazioni.

1. Prima crei una directory per la sua nuova applicazione e si posizioni al suo interno:

   ```bash
   mkdir myapp
   cd myapp
   ```

2. Usi il comando npm `init` per creare un file **package.json** per la tua applicazione. Questo comando la invita a fornire una serie di informazioni, incluso il nome e la versione della sua applicazione e il nome del file del punto di ingresso iniziale (di default è **index.js**). Per ora, accetti semplicemente i valori predefiniti:

   ```bash
   npm init
   ```

   Se visualizza il file **package.json** (`cat package.json`), vedrà i valori predefiniti che ha accettato, terminando con la licenza.

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

3. Ora installi Express nella directory `myapp` e lo salvi nell'elenco delle dipendenze del suo file **package.json**:

   ```bash
   npm install express@^4.21.2
   ```

   La sezione delle dipendenze del suo **package.json** apparirà ora alla fine del file **package.json** e includerà _Express_.

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

4. Per utilizzare la libreria Express, chiami la funzione `require()` nel suo file **index.js** per includerla nella sua applicazione.
   Crei questo file ora, nella radice della directory dell'applicazione "myapp", e gli fornisca il seguente contenuto:

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
   Importa il modulo "express" utilizzando `require()` e lo usa per creare un server (`app`) che ascolta le richieste HTTP sulla porta 3000 e stampa un messaggio sulla console spiegando quale URL del browser può usare per testare il server.
   La funzione `app.get()` risponde solo alle richieste HTTP `GET` con il percorso URL specificato ('/'), in questo caso chiamando una funzione per inviare il nostro messaggio _Hello World!_.

   > [!NOTE]
   > Le virgolette inverse in `` `Example app listening on port ${port}!` `` ci consentono di interpolare il valore di `$port` nella stringa.

5. Può avviare il server chiamando node con lo script nel suo prompt dei comandi:

   ```bash
   node index.js
   ```

   Vedrà il seguente output della console:

   ```plain
   Example app listening on port 3000
   ```

6. Navighi all'URL `http://localhost:3000/`.
   Se tutto funziona, il browser dovrebbe mostrare la stringa "Hello World!".

### Dipendenze di sviluppo

Se una dipendenza viene utilizzata solo durante lo sviluppo, dovrebbe invece salvarla come "dipendenza di sviluppo" (in modo che gli utenti del suo pacchetto non debbano installarla in produzione). Ad esempio, per utilizzare il popolare strumento di linting JavaScript [ESLint](https://eslint.org/) chiamerebbe npm come mostrato:

```bash
npm install eslint --save-dev
```

La seguente voce sarebbe quindi aggiunta al **package.json** della tua applicazione:

```json
  "devDependencies": {
    "eslint": "^7.10.0"
  }
```

> [!NOTE]
> I "[Linters](<https://en.wikipedia.org/wiki/Lint_(software)>)" sono strumenti che eseguono analisi statiche sul software per riconoscere e segnalare l'aderenza/non aderenza ad alcuni best practice di codifica.

### Esecuzione di attività

Oltre a definire e recuperare le dipendenze, può anche definire script _denominati_ nei suoi file **package.json** e chiamare npm per eseguirli con il comando [run-script](https://docs.npmjs.com/cli/run-script/). Questo approccio è comunemente utilizzato per automatizzare l'esecuzione di test e parti della toolchain di sviluppo o di build (ad esempio, eseguendo strumenti per minimizzare il JavaScript, ridurre le immagini, LINT/analizzare il suo codice, ecc.).

> [!NOTE]
> Strumenti di esecuzione di attività come [Gulp](https://gulpjs.com/) e [Grunt](https://gruntjs.com/) possono anche essere utilizzati per eseguire test e altri strumenti esterni.

Ad esempio, per definire uno script per eseguire la dipendenza di sviluppo _eslint_ che abbiamo specificato nella sezione precedente potremmo aggiungere il seguente blocco di script al nostro file **package.json** (ipotizzando che il sorgente della nostra applicazione sia nella cartella /src/js):

```json
"scripts": {
  // …
  "lint": "eslint src/js"
  // …
}
```

Per spiegare un po' di più, `eslint src/js` è un comando che potremmo inserire nel nostro terminale/riga di comando per eseguire `eslint` sui file JavaScript contenuti nella directory `src/js` all'interno della directory della nostra app. Includere quanto sopra all'interno del file package.json della nostra app fornisce un collegamento a questo comando — `lint`.

Saremmo quindi in grado di eseguire _eslint_ usando npm chiamando:

```bash
npm run-script lint
# OR (using the alias)
npm run lint
```

Questo esempio potrebbe non sembrare più corto del comando originale, ma può includere comandi molto più grandi all'interno dei suoi script npm, comprese catene di più comandi. Potrebbe identificare un singolo script npm che esegue tutti i suoi test contemporaneamente.

## Installazione dell'Express Application Generator

Lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator.html) genera uno "scheletro" di applicazione Express. Installi il generatore utilizzando npm come mostrato:

```bash
npm install express-generator -g
```

> [!NOTE]
> Potrebbe essere necessario anteporre questa linea con `sudo` su Ubuntu o macOS. Il flag `-g` installa lo strumento globalmente in modo che possa accedervi ovunque.

Per creare un'app _Express_ denominata "helloworld" con le impostazioni predefinite, si posizioni dove vuole crearla ed esegua l'app come mostrato:

```bash
express helloworld
```

> [!NOTE]
> A meno che non stia utilizzando una vecchia versione di nodejs (< 8.2.0), potrebbe alternativamente saltare l'installazione ed eseguire express-generator con [npx](https://github.com/npm/npx#readme).
> Questo ha lo stesso effetto di installare e poi eseguire `express-generator`, ma non installa il pacchetto sul suo sistema:
>
> ```bash
> npx express-generator helloworld
> ```

Può anche specificare la libreria di template da utilizzare e una serie di altre impostazioni.
Utilizzi il comando `help` per vedere tutte le opzioni:

```bash
express --help
```

Il generatore creerà la nuova app Express in una sottodirectory della sua posizione corrente, visualizzando l'avanzamento della creazione sulla console.
Al termine, lo strumento mostrerà i comandi che deve immettere per installare le dipendenze di Node e avviare l'app.

La nuova app avrà un file **package.json** nella sua directory radice.
Può aprirlo per vedere quali dipendenze sono installate, inclusi Express e la libreria di template Jade:

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

Installi tutte le dipendenze per l'app helloworld utilizzando npm come mostrato:

```bash
cd helloworld
npm install
```

Poi esegua l'app (i comandi sono leggermente diversi per Windows e Linux/macOS), come mostrato di seguito:

```bash
# Run helloworld on Windows with Command Prompt
SET DEBUG=helloworld:* & npm start

# Run helloworld on Windows with PowerShell
SET DEBUG=helloworld:* | npm start

# Run helloworld on Linux/macOS
DEBUG=helloworld:* npm start
```

Il comando DEBUG crea una registrazione utile, risultante in un output come il seguente:

```bash
>SET DEBUG=helloworld:* & npm start

> helloworld@0.0.0 start D:\GitHub\express-tests\helloworld
> node ./bin/www

  helloworld:server Listening on port 3000 +0ms
```

Apra un browser e navighi su `http://localhost:3000/` per vedere la pagina di benvenuto predefinita di Express.

![Express - Generated App Default Screen](express_default_screen.png)

Parleremo di più dell'app generata quando arriveremo all'articolo sulla generazione di una applicazione scheletro.

## Riepilogo

Ora ha un ambiente di sviluppo Node configurato e funzionante sul suo computer che può essere utilizzato per creare applicazioni web Express. Ha anche visto come npm può essere utilizzato per importare Express in un'applicazione, e anche come si possono creare applicazioni utilizzando lo strumento Express Application Generator e poi eseguirle.

Nel prossimo articolo inizieremo a lavorare attraverso un tutorial per costruire un'applicazione web completa utilizzando questo ambiente e gli strumenti associati.

## Vedi anche

- Pagina dei [Download](https://nodejs.org/en/download) (nodejs.org)
- [Installing Express](https://expressjs.com/en/starter/installing.html) (expressjs.com)
- [Express Application Generator](https://expressjs.com/en/starter/generator.html) (expressjs.com)
- [Using Node.js with Windows subsystem for Linux](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/) (docs.microsoft.com)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
