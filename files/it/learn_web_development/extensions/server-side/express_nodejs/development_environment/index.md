---
title: Configurare un ambiente di sviluppo Node
short-title: Configurazione dell'ambiente di sviluppo
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment
l10n:
  sourceCommit: 77d90a23ee0a3b5486a7963f68ad4e56efb06a7b
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora che sai a cosa serve [Express](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction#introducing_express), ti mostreremo come configurare e testare un ambiente di sviluppo Node/Express su Windows, Linux (Ubuntu) o macOS. Per ciascuno di questi sistemi operativi, questo articolo fornisce ciò di cui hai bisogno per iniziare a sviluppare app Express.

> [!WARNING]
> Il tutorial su Express è scritto per la versione 4 di Express, mentre l'ultima versione è Express 5.
> Prevediamo di aggiornare la documentazione per supportare Express 5 nella seconda metà del 2025. Fino ad allora, abbiamo aggiornato i comandi di installazione in modo che installino Express 4 anziché l'ultima versione, per evitare potenziali problemi di compatibilità.

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
      <td>Impostare un ambiente di sviluppo per Express sul computer.</td>
    </tr>
  </tbody>
</table>

## Panoramica dell'ambiente di sviluppo Express

_Node_ ed _Express_ rendono molto facile configurare il tuo computer per iniziare a sviluppare applicazioni web. Questa sezione fornisce una panoramica degli strumenti necessari, spiega alcuni dei metodi più semplici per installare Node (ed Express) su Ubuntu, macOS e Windows, e mostra come puoi testare la tua installazione.

### Che cos'è l'ambiente di sviluppo Express?

L'ambiente di sviluppo _Express_ include un'installazione di _Nodejs_, il _gestore dei pacchetti npm_ e (opzionalmente) l'_Express Application Generator_ sul tuo computer locale.

_Node_ e il gestore di pacchetti _npm_ sono installati insieme da pacchetti binari predefiniti, installatori, gestori di pacchetti del sistema operativo o dal codice sorgente (come mostrato nelle sezioni seguenti). _Express_ viene poi installato da npm come dipendenza delle tue singole applicazioni web _Express_ (insieme ad altre librerie come motori di template, driver di database, middleware di autenticazione, middleware per servire file statici, ecc.).

_npm_ può anche essere utilizzato per installare (globalmente) l'_Express Application Generator_, uno strumento utile per creare app web _Express_ scheletriche che seguono il {{Glossary("MVC", "modello MVC")}}. Il generatore di applicazioni è opzionale perché non è _necessario_ utilizzare questo strumento per creare applicazioni che utilizzano Express, o per costruire applicazioni Express che abbiano lo stesso layout architetturale o le stesse dipendenze. Tuttavia, lo useremo perché rende l'inizio più facile e promuove una struttura modulare dell'applicazione.

> [!NOTE]
> A differenza di alcuni altri framework web, l'ambiente di sviluppo non include un server web di sviluppo separato. In _Node_/_Express_ un'applicazione web crea e gestisce il proprio server web!

Ci sono altri strumenti periferici che fanno parte di un tipico ambiente di sviluppo, tra cui [editor di testo](https://developer.mozilla.org/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors) o ambienti di sviluppo integrati (IDE) per modificare il codice, e strumenti di gestione del controllo versione come [Git](https://git-scm.com/) per gestire in sicurezza le diverse versioni del tuo codice. Supponiamo che tu abbia già installato questi tipi di strumenti (in particolare un editor di testo).

### Quali sistemi operativi sono supportati?

_Node_ può essere eseguito su Windows, macOS, molte distribuzioni Linux, Docker, ecc. C'è un elenco completo nella pagina dei [Download di Node.js](https://nodejs.org/en/download). Quasi qualsiasi computer personale dovrebbe avere le prestazioni necessarie per eseguire Node durante lo sviluppo. _Express_ viene eseguito in un ambiente _Node_ e quindi può funzionare su qualsiasi piattaforma che esegua _Node_.

In questo articolo forniamo istruzioni di configurazione per Windows, macOS e Ubuntu Linux.

### Che versione di Node/Express dovresti usare?

Ci sono molti [rilasci di Node](https://nodejs.org/en/blog/release/) — le versioni più recenti contengono correzioni di bug, supporto per versioni più recenti degli standard ECMAScript (JavaScript) e miglioramenti delle API di Node.

In generale, dovresti usare la versione più recente _LTS (supportata a lungo termine)_ poiché sarà più stabile rispetto alla versione "corrente", pur avendo funzionalità relativamente recenti (ed è ancora attivamente mantenuta). Dovresti usare la versione _Corrente_ se hai bisogno di una funzionalità che non è presente nella versione LTS.

Per _Express_ dovresti sempre usare l'ultima versione.

### Che dire di database e altre dipendenze?

Altre dipendenze, come driver di database, motori di template, motori di autenticazione, ecc. fanno parte dell'applicazione e vengono importate nell'ambiente applicativo usando il gestore di pacchetti npm. Ne parleremo in articoli successivi specifici per le applicazioni.

## Installare Node

Per usare _Express_, dovrai installare _Nodejs_ e il [Node Package Manager (npm)](https://docs.npmjs.com/) sul tuo sistema operativo. Per semplificare questo passaggio, installeremo prima un gestore di versioni di node, quindi lo useremo per installare le versioni Long Term Supported (LTS) più recenti di node e npm.

> [!NOTE]
> Puoi anche installare nodejs e npm con gli installer forniti su <https://nodejs.org/en/> (seleziona il pulsante per scaricare il build LTS "consigliato per la maggior parte degli utenti"), oppure puoi [installare usando il package manager per il tuo sistema operativo](https://nodejs.org/en/download) (nodejs.org).
> Consigliamo vivamente di utilizzare un gestore di versioni di node poiché rendono più facile l'installazione, l'aggiornamento e il passaggio tra qualsiasi versione particolare di node e npm.

### Windows

Ci sono diversi gestori di versioni di node per Windows. Qui usiamo [nvm-windows](https://github.com/coreybutler/nvm-windows), molto rispettato tra gli sviluppatori node.

Installa l'ultima versione utilizzando l'installer di tua scelta dalla pagina [nvm-windows/releases](https://github.com/coreybutler/nvm-windows/releases). Dopo che `nvm-windows` è stato installato, apri un prompt dei comandi (o PowerShell) ed inserisci il seguente comando per scaricare l'ultima versione LTS di nodejs e npm:

```bash
nvm install lts
```

Al momento della stesura, la versione LTS di nodejs è 20.11.0. Puoi impostarla come _versione corrente_ da utilizzare con il comando qui sotto:

```bash
nvm use 20.11.0
```

> [!NOTE]
> Se ricevi avvisi di "Accesso negato", dovrai eseguire questo comando in un prompt con i permessi di amministrazione.

Usa il comando `nvm --help` per trovare altre opzioni della riga di comando, come elencare tutte le versioni di node disponibili e tutte le versioni di NVM scaricate.

### Ubuntu e macOS

Ci sono diversi gestori di versioni di node per Ubuntu e macOS. [nvm](https://github.com/nvm-sh/nvm) è uno dei più popolari ed è la versione originale su cui si basa `nvm-windows`. Vedi [nvm > Install & Update Script](https://github.com/nvm-sh/nvm#install--update-script) per le istruzioni da terminale per installare l'ultima versione di nvm.

Dopo che `nvm` è stato installato, apri un terminale ed inserisci il seguente comando per scaricare l'ultima versione LTS di nodejs e npm:

```bash
nvm install --lts
```

Al momento della stesura, la versione LTS di nodejs è 20.11.0. Il comando `nvm list` mostra il set di versioni scaricate e la versione corrente. Puoi impostare una versione particolare come _versione corrente_ con il comando qui sotto (lo stesso di `nvm-windows`)

```bash
nvm use 20.11.0
```

Usa il comando `nvm --help` per trovare altre opzioni della riga di comando. Queste sono spesso simili, o uguali, a quelle offerte da `nvm-windows`.

### Testare la tua installazione di Nodejs e npm

Una volta impostato `nvm` per utilizzare una particolare versione di node, puoi testare l'installazione. Un buon modo per farlo è utilizzare il comando "version" nel tuo terminale/prompt dei comandi e verificare che venga restituita la stringa della versione prevista:

```bash
> node -v
v20.11.0
```

Il gestore di pacchetti _Nodejs_ _npm_ dovrebbe anche essere stato installato, e può essere testato allo stesso modo:

```bash
> npm -v
10.2.4
```

Come test leggermente più entusiasmante, creiamo un server "pure node" molto basilare che stampa "Hello World" nel browser quando visiti l'URL corretto nel tuo browser:

1. Copia il seguente testo in un file denominato **hellonode.js**. Questo utilizza solo le funzionalità di Node (niente di Express):

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

   Il codice importa il modulo "http" e lo utilizza per creare un server (`createServer()`) che ascolta le richieste HTTP sulla porta 3000. Lo script poi stampa un messaggio sulla console riguardo a quale URL del browser puoi usare per testare il server. La funzione `createServer()` prende come argomento una funzione di callback che verrà invocata quando viene ricevuta una richiesta HTTP — questa restituisce una risposta con un codice di stato HTTP di 200 ("OK") e il testo semplice "Hello World".

   > [!NOTE]
   > Non ti preoccupare se non capisci esattamente cosa faccia questo codice al momento! Spiegheremo il nostro codice in modo più dettagliato una volta che inizieremo a usare Express!

2. Avvia il server navigando nella stessa directory del tuo file `hellonode.js` nel prompt dei comandi, e chiamando `node` insieme al nome dello script, come mostrato di seguito:

   ```bash
   node hellonode.js
   ```

   Una volta avviato il server, vedrai un'uscita della console che indica l'indirizzo IP su cui il server è in esecuzione:

   ```plain
   Server running at http://127.0.0.1:3000/
   ```

3. Naviga all'URL `http://127.0.0.1:3000`. Se tutto funziona, il browser dovrebbe visualizzare la stringa "Hello World".

## Utilizzare npm

Accanto a _Node_ stesso, [npm](https://docs.npmjs.com/) è lo strumento più importante per lavorare con le applicazioni _Node_. `npm` viene utilizzato per scaricare qualsiasi pacchetto (librerie JavaScript) di cui un'applicazione ha bisogno per lo sviluppo, il test e/o la produzione, e può anche essere usato per eseguire test e strumenti usati nel processo di sviluppo.

> [!NOTE]
> Dal punto di vista di Node, _Express_ è solo un altro pacchetto che devi installare usando npm e poi includere nel tuo codice.

Puoi usare manualmente npm per scaricare separatamente ogni pacchetto necessario. In genere, invece, gestiamo le dipendenze usando un file di definizione in formato testo semplice chiamato [package.json](https://docs.npmjs.com/files/package.json/). Questo file elenca tutte le dipendenze per uno specifico "pacchetto" JavaScript, inclusi il nome del pacchetto, la versione, la descrizione, il file iniziale da eseguire, le dipendenze di produzione, le dipendenze di sviluppo, le versioni di _Node_ con cui può funzionare, ecc. Il file **package.json** dovrebbe contenere tutto ciò che npm ha bisogno per scaricare e eseguire la tua applicazione (se stessi scrivendo una libreria riutilizzabile, potresti usare questa definizione per caricare il tuo pacchetto nel repository npm e renderlo disponibile per altri utenti).

### Aggiungere dipendenze

I passaggi seguenti mostrano come puoi usare npm per scaricare un pacchetto, salvarlo tra le dipendenze del progetto e poi includerlo in un'applicazione Node.

> [!NOTE]
> Qui mostriamo le istruzioni per scaricare e installare il pacchetto _Express_. In seguito mostreremo come questo pacchetto, e altri, siano già specificati per noi utilizzando l'_Express Application Generator_. Questa sezione è fornita perché è utile capire come funziona npm e cosa viene creato dal generatore di applicazioni.

1. Prima crea una directory per la tua nuova applicazione e naviga al suo interno:

   ```bash
   mkdir myapp
   cd myapp
   ```

2. Usa il comando npm `init` per creare un file **package.json** per la tua applicazione. Questo comando ti richiederà una serie di cose, tra cui il nome e la versione della tua applicazione e il nome del file di punto di ingresso iniziale (per impostazione predefinita è **index.js**). Per ora, accetta semplicemente i valori predefiniti:

   ```bash
   npm init
   ```

   Se visualizzi il file **package.json** (`cat package.json`), vedrai i valori predefiniti che hai accettato, terminando con la licenza.

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

   La sezione delle dipendenze del tuo **package.json** ora apparirà alla fine del file **package.json** e includerà _Express_.

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

4. Per usare la libreria Express, devi chiamare la funzione `require()` nel tuo file **index.js** per includerla nella tua applicazione. Crea questo file ora, nella directory principale dell'applicazione "myapp", e dagli il seguente contenuto:

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

   Questo codice mostra una minima applicazione web Express "HelloWorld". Importa il modulo "express" usando `require()` e lo usa per creare un server (`app`) che ascolta le richieste HTTP sulla porta 3000 e stampa un messaggio sulla console che spiega quale URL del browser puoi usare per testare il server. La funzione `app.get()` risponde solo alle richieste `GET` HTTP con il percorso URL specificato ('/'), in questo caso chiamando una funzione per inviare il nostro messaggio _Hello World!_.

   > [!NOTE]
   > I backticks in `` `Example app listening on port ${port}!` `` ci permettono di interpolare il valore di `$port` nella stringa.

5. Puoi avviare il server chiamando node con lo script nel tuo prompt dei comandi:

   ```bash
   node index.js
   ```

   Vedrai la seguente uscita sulla console:

   ```plain
   Example app listening on port 3000
   ```

6. Naviga all'URL `http://localhost:3000/`. Se tutto funziona, il browser dovrebbe visualizzare la stringa "Hello World!".

### Dipendenze di sviluppo

Se una dipendenza viene utilizzata solo durante lo sviluppo, dovresti invece salvarla come "dipendenza di sviluppo" (in modo che gli utenti del tuo pacchetto non debbano installarla in produzione). Ad esempio, per usare il popolare strumento di linting JavaScript [ESLint](https://eslint.org/) puoi chiamare npm come mostrato:

```bash
npm install eslint --save-dev
```

Il seguente ingresso verrà poi aggiunto al file **package.json** della tua applicazione:

```json
  "devDependencies": {
    "eslint": "^7.10.0"
  }
```

> [!NOTE]
> I "[Linters](<https://en.wikipedia.org/wiki/Lint_(software)>)" sono strumenti che eseguono analisi statiche sul software per riconoscere e segnalare l'aderenza/non aderenza a un determinato set di best practice di codifica.

### Eseguire compiti

Oltre a definire e scaricare dipendenze puoi anche definire script _denominati_ nei tuoi file **package.json** e chiamare npm per eseguirli con il comando [run-script](https://docs.npmjs.com/cli/run-script/). Questo approccio è comunemente usato per automatizzare l'esecuzione di test e parte della toolchain di sviluppo o build (ad esempio, eseguire strumenti per minimizzare JavaScript, ridurre le immagini, LINT/analizzare il tuo codice, ecc.).

> [!NOTE]
> Runner di compiti come [Gulp](https://gulpjs.com/) e [Grunt](https://gruntjs.com/) possono anche essere usati per eseguire test e altri strumenti esterni.

Ad esempio, per definire uno script per eseguire la dipendenza di sviluppo _eslint_ che abbiamo specificato nella sezione precedente, potremmo aggiungere il seguente blocco di script al nostro file **package.json** (assumendo che il sorgente della nostra applicazione sia in una cartella /src/js):

```json
"scripts": {
  // …
  "lint": "eslint src/js"
  // …
}
```

Per spiegare un po' più a fondo, `eslint src/js` è un comando che potremmo inserire nel nostro terminale/riga di comando per eseguire `eslint` sui file JavaScript contenuti nella directory `src/js` all'interno della nostra directory dell'app. Includere il contenuto sopra nel file package.json della nostra app fornisce una scorciatoia per questo comando — `lint`.

Saremmo poi in grado di eseguire _eslint_ usando npm chiamando:

```bash
npm run-script lint
# OR (using the alias)
npm run lint
```

Questo esempio potrebbe non sembrare più breve del comando originale, ma puoi includere comandi molto più grandi all'interno dei tuoi script npm, comprese catene di comandi multipli. Potresti identificare uno script npm singolo che esegue tutti i tuoi test contemporaneamente.

## Installare l'Express Application Generator

Lo strumento [Express Application Generator](https://expressjs.com/en/starter/generator.html) genera uno "scheletro" dell'applicazione Express. Installa il generatore usando npm come mostrato:

```bash
npm install express-generator -g
```

> [!NOTE]
> Potrebbe essere necessario anteporre a questa riga il comando `sudo` su Ubuntu o macOS. Il flag `-g` installa lo strumento globalmente in modo che tu possa chiamarlo da qualsiasi posizione.

Per creare un'app _Express_ denominata "helloworld" con le impostazioni predefinite, naviga dove vuoi crearla e avvia l'app come mostrato:

```bash
express helloworld
```

> [!NOTE]
> A meno che tu non stia utilizzando una versione vecchia di nodejs (< 8.2.0), potresti alternativamente saltare l'installazione e eseguire express-generator con [npx](https://github.com/npm/npx#readme).
> Questo ha lo stesso effetto di installare e poi eseguire `express-generator`, ma non installa il pacchetto sul tuo sistema:
>
> ```bash
> npx express-generator helloworld
> ```

Puoi anche specificare la libreria di template da usare e un numero di altre impostazioni. Usa il comando `help` per vedere tutte le opzioni:

```bash
express --help
```

Il generatore creerà la nuova app Express in una sottocartella della tua posizione attuale, mostrando i progressi di costruzione sulla console. Completata l'operazione, lo strumento visualizzerà i comandi che devi inserire per installare le dipendenze di Node e avviare l'app.

La nuova app avrà un file **package.json** nella sua directory principale. Puoi aprire questo file per vedere quali dipendenze sono installate, inclusi Express e la libreria di template Jade:

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

Il comando DEBUG crea un log utile, risultando in un'uscita simile a quanto segue:

```bash
>SET DEBUG=helloworld:* & npm start

> helloworld@0.0.0 start D:\GitHub\express-tests\helloworld
> node ./bin/www

  helloworld:server Listening on port 3000 +0ms
```

Apri un browser e naviga su `http://localhost:3000/` per vedere la pagina di benvenuto predefinita di Express.

![Express - Schermata Predefinita dell'App Generata](express_default_screen.png)

Parleremo di più dell'app generata quando arriveremo all'articolo sulla generazione di un'applicazione scheletro.

## Riassunto

Ora hai un ambiente di sviluppo Node configurato e funzionante sul tuo computer che può essere utilizzato per creare applicazioni web Express. Hai anche visto come npm può essere usato per importare Express in un'applicazione, e anche come puoi creare applicazioni usando lo strumento Express Application Generator e quindi eseguirle.

Nel prossimo articolo iniziamo a seguire un tutorial per costruire un'applicazione web completa utilizzando questo ambiente e gli strumenti associati.

## Vedi anche

- [Pagina dei download](https://nodejs.org/en/download) (nodejs.org)
- [Installare Express](https://expressjs.com/en/starter/installing.html) (expressjs.com)
- [Express Application Generator](https://expressjs.com/en/starter/generator.html) (expressjs.com)
- [Usare Node.js con Windows Subsystem per Linux](https://learn.microsoft.com/en-us/windows/dev-environment/javascript/) (docs.microsoft.com)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction", "Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
