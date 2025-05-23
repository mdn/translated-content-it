---
title: Introduzione ai test automatizzati
short-title: Test automatizzati
slug: Learn_web_development/Extensions/Testing/Automated_testing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing/Your_own_automation_environment", "Learn_web_development/Extensions/Testing")}}

Eseguire manualmente i test su diversi browser e dispositivi, più volte al giorno, può diventare tedioso e dispendioso in termini di tempo. Per gestire questo in modo efficiente, è opportuno familiarizzare con gli strumenti di automazione. In questo articolo, esaminiamo cosa è disponibile, come usare i task runner e come utilizzare le basi delle app commerciali di automazione dei test browser come Sauce Labs, BrowserStack e TestingBot.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>;
        un'idea dei principi di alto livello del <a href="/it/docs/Learn_web_development/Extensions/Testing/Introduction">collaudo cross-browser</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Fornire una comprensione di cosa implica il collaudo automatizzato, come può semplificare il lavoro e come utilizzare alcuni dei prodotti commerciali che lo rendono più facile.
      </td>
    </tr>
  </tbody>
</table>

## L'automazione facilita le cose

In tutto questo modulo abbiamo dettagliato diversi modi per testare i tuoi siti web e app, spiegando quale potrebbe essere la portata del tuo collaudo cross-browser in termini di browser da testare, considerazioni sull'accessibilità e altro ancora. Sembra un sacco di lavoro, vero?

Siamo d'accordo - testare tutto ciò di cui abbiamo parlato nei precedenti articoli manualmente può essere davvero noioso. Fortunatamente, ci sono strumenti che possono aiutarci ad automatizzare parte di questo compito. Ci sono due modi principali per automatizzare i test di cui abbiamo parlato in questo modulo:

1. Utilizzare un task runner come [Grunt](https://gruntjs.com/) o [Gulp](https://gulpjs.com/), o [npm scripts](https://docs.npmjs.com/misc/scripts/) per eseguire test e pulire il codice durante il processo di build. Questo è un ottimo modo per eseguire compiti come l'analisi del codice (linting), la minimizzazione, l'aggiunta di prefissi CSS o la trascrizione di nuove funzionalità JavaScript per ottenere la massima portata cross-browser e così via.
2. Usare un sistema di automazione del browser come [Selenium](https://www.selenium.dev/) per eseguire test specifici su browser installati e restituire i risultati, avvisandoti dei fallimenti nei browser appena si verificano. Le app commerciali di collaudo cross-browser come [Sauce Labs](https://saucelabs.com/) e [BrowserStack](https://www.browserstack.com/) si basano su Selenium, ma ti permettono di accedere alla loro configurazione da remoto utilizzando un'interfaccia, risparmiandoti la fatica di impostare il tuo sistema di test.

Vedremo come impostare il tuo sistema di collaudo basato su Selenium nel prossimo articolo. In questo articolo, vedremo come impostare un task runner e utilizzare le funzionalità di base dei sistemi commerciali come quelli menzionati sopra.

> [!NOTE]
> Le due categorie sopra non sono mutuamente esclusive. È possibile configurare un task runner per accedere a un servizio come Sauce Labs o LambdaTest tramite un'API, eseguire test cross-browser e restituire i risultati. Lo vedremo di seguito.

## Usare un task runner per automatizzare gli strumenti di collaudo

Come abbiamo detto sopra, puoi accelerare drasticamente compiti comuni come l'analisi del codice (linting) e la minimizzazione usando un task runner per eseguire tutto ciò di cui hai bisogno automaticamente a un certo punto nel tuo processo di build. Ad esempio, questo potrebbe essere ogni volta che salvi un file, o in un altro momento. In questa sezione vedremo come automatizzare l'esecuzione dei task con Node e Gulp, un'opzione accessibile ai principianti.

### Configurare Node e npm

La maggior parte degli strumenti oggi si basa su {{Glossary("Node.js", "Node.js")}}, quindi sarà necessario installarlo insieme al suo gestore dei pacchetti, [`npm`](https://www.npmjs.com/):

1. Il modo più semplice per installare e aggiornare Node.js e `npm` è tramite un gestore delle versioni di node: segui le istruzioni su [Installare Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment#installing_node) per farlo.
2. Assicurati di [testare che l'installazione sia avvenuta con successo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment#testing_your_nodejs_and_npm_installation) prima di continuare.
3. Se hai installato precedentemente Node.js/`npm`, dovresti aggiornarli alle versioni più recenti. Questo può essere fatto utilizzando il gestore delle versioni di node per installare le ultime versioni LTS (ricordando di nuovo le istruzioni collegate sopra).

Per iniziare a usare i pacchetti basati su Node/npm sui tuoi progetti, devi configurare le directory del tuo progetto come progetti npm. Questo è facile da fare.

Ad esempio, creiamo prima una directory di test che ci permetta di sperimentare senza paura di rompere nulla.

1. Creare una nuova directory in un luogo sensato utilizzando l'interfaccia del gestore dei file, o, da riga di comando, navigando nel percorso desiderato ed eseguendo il seguente comando:

   ```bash
   mkdir node-test
   ```

2. Per rendere questa directory un progetto npm, è sufficiente entrare nella directory di test e inizializzarla, con quanto segue:

   ```bash
   cd node-test
   npm init
   ```

3. Questo secondo comando ti farà molte domande per raccogliere le informazioni necessarie per configurare il progetto; puoi selezionare semplicemente i valori predefiniti per ora.
4. Una volta che tutte le domande sono state poste, ti verrà chiesto se le informazioni inserite sono corrette. Digita `yes` e premi Invio/Return e npm genererà un file `package.json` nella tua directory.

Questo file è essenzialmente un file di configurazione per il progetto. Puoi personalizzarlo in seguito, ma per ora somiglierà a questo:

```json
{
  "name": "node-test",
  "version": "1.0.0",
  "description": "Test for npm projects",
  "main": "index.js",
  "scripts": {
    "test": "test"
  },
  "author": "Chris Mills",
  "license": "MIT"
}
```

Con questo, sei pronto a proseguire.

### Impostare l'automazione con Gulp

Vediamo come impostare Gulp e usarlo per automatizzare alcuni strumenti di collaudo.

1. Per iniziare, crea un progetto npm di test usando la procedura dettagliata in fondo alla sezione precedente.
   Inoltre, aggiorna il file `package.json` con la riga: `"type": "module"` in modo che risulti simile a questo:

   ```json
   {
     "name": "node-test",
     "version": "1.0.0",
     "description": "Test for npm projects",
     "main": "index.js",
     "scripts": {
       "test": "test"
     },
     "author": "Chris Mills",
     "license": "MIT",
     "type": "module"
   }
   ```

2. Successivamente, avrai bisogno di alcuni contenuti di esempio HTML, CSS e JavaScript per testare il tuo sistema — fai copie dei nostri file di esempio [index.html](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/index.html), [main.js](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/main.js), e [style.css](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/style.css) in una sottocartella chiamata `src` all'interno della tua cartella di progetto.
   Puoi provare i tuoi contenuti di test se vuoi, ma tieni presente che tali strumenti non funzionano bene con JS/CSS in-line scritto nel file HTML — hai bisogno di file separati.
3. Installare gulp globalmente (significa, sarà disponibile su tutti i progetti) utilizzando il seguente comando:

   ```bash
   npm install --global gulp-cli
   ```

4. Esegui successivamente il seguente comando all'interno della tua directory radice del progetto npm per configurare gulp come dipendenza del tuo progetto:

   ```bash
   npm install --save-dev gulp
   ```

5. Ora crea un nuovo file all'interno della tua directory di progetto chiamato `gulpfile.mjs`. Questo è il file che eseguirà tutte le nostre attività. All'interno di questo file, inserisci quanto segue:

   ```js
   import gulp from "gulp";

   export default function (cb) {
     console.log("Gulp running");
     cb();
   }
   ```

   Si richiede il modulo `gulp` che abbiamo installato precedentemente e poi esporta un'attività predefinita che non fa nulla tranne che stampare un messaggio nel terminale — questo è utile per farci sapere che Gulp sta funzionando. Nelle prossime sezioni, cambieremo questa dichiarazione `export default` in qualcosa di più utile.

   Ogni attività di gulp viene esportata nello stesso formato di base — `exports function taskName(cb) {...}`. Ogni funzione prende un parametro — un callback da eseguire quando l'attività è completata.

6. Puoi eseguire l'attività predefinita di gulp con il seguente comando — prova questo ora:

   ```bash
   gulp
   ```

### Aggiungere alcune attività reali a Gulp

Ora siamo pronti ad aggiungere più attività al nostro file Gulp. Ogni aggiunta potrebbe richiedere di modificare il file `gulpfile.mjs` nel seguente modo:

- Quando ti chiediamo di aggiungere delle dichiarazioni `import`, aggiungile sotto la dichiarazione `import` esistente.
- Quando ti chiediamo di aggiungere una nuova dichiarazione `export function ...`, aggiungila alla fine del file.
- Quando ti chiediamo di cambiare l'esportazione predefinita, cambia la dichiarazione `export default` nel modo specificato.

Quindi il tuo file `gulpfile.mjs` crescerà in questo modo:

```js
import gulp from "gulp";
// Add any new imports here

// Our latest default export
// export default ...

// Add any new task exports here
// export function ...
// export function ...
```

Per aggiungere alcune vere attività a Gulp, dobbiamo pensare a cosa vogliamo fare. Un insieme ragionevole di funzionalità di base da eseguire sul nostro progetto è il seguente:

- html-tidy, css-lint e js-hint per svolgere l'analisi del codice e segnalare/riparare errori comuni in HTML/CSS/JS (vedi [gulp-htmltidy](https://www.npmjs.com/package/gulp-htmltidy), [gulp-csslint](https://www.npmjs.com/package/gulp-csslint), [gulp-jshint](https://www.npmjs.com/package/gulp-jshint)).
- Autoprefixer per scansionare il nostro CSS e aggiungere prefissi browser solo dove necessario (vedi [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer)).
- babel per trascrivere eventuali nuove funzionalità sintattiche di JavaScript in sintassi tradizionale che funzionano nei browser più anziani (vedi [gulp-babel](https://www.npmjs.com/package/gulp-babel)).

Vedi i link sopra per le istruzioni complete sui diversi pacchetti gulp che stiamo utilizzando.

Per utilizzare ogni plugin, è necessario prima installarlo tramite npm, quindi richiederne le dipendenze in cima al file `gulpfile.mjs`, quindi aggiungere il tuo test alla fine di esso, e infine esportare il nome del tuo task per renderlo disponibile tramite il comando di gulp.

#### html-tidy

1. Installa usando la seguente linea:

   ```bash
   npm install --save-dev gulp-htmltidy
   ```

   > **Nota:** `--save-dev` aggiunge il pacchetto come dipendenza al tuo progetto. Se guardi nel file `package.json` del tuo progetto, vedrai una voce per esso nella proprietà `devDependencies`.

2. Aggiungi la seguente dipendenza a `gulpfile.mjs`:

   ```js
   import htmltidy from "gulp-htmltidy";
   ```

3. Aggiungi il seguente test alla fine di `gulpfile.mjs`:

   ```js
   export function html() {
     return gulp
       .src("src/index.html")
       .pipe(htmltidy())
       .pipe(gulp.dest("build"));
   }
   ```

4. Cambia l'esportazione predefinita in:

   ```js
   export default html;
   ```

Qui stiamo prendendo il nostro file di sviluppo `index.html` con `gulp.src()`, che ci permette di prendere un file sorgente per farci qualcosa.

Poi usiamo la funzione `pipe()` per passare quella sorgente a un altro comando per farci qualcos'altro. Possiamo concatenare quanti ne vogliamo. Eseguiamo prima `htmltidy()` sulla sorgente, che percorre e corregge errori nel nostro file. La seconda funzione `pipe()` scrive il file HTML di output nella directory `build`.

Nella versione di input del file, potresti aver notato che abbiamo messo un elemento {{htmlelement("p")}} vuoto; htmltidy lo ha rimosso nel momento in cui il file di output è stato creato.

#### Autoprefixer e css-lint

1. Installa usando le seguenti righe:

   ```bash
   npm install --save-dev gulp-autoprefixer
   npm install --save-dev gulp-csslint
   ```

2. Aggiungi le seguenti dipendenze a `gulpfile.mjs`:

   ```js
   import autoprefixer from "gulp-autoprefixer";
   import csslint from "gulp-csslint";
   ```

3. Aggiungi il seguente test alla fine di `gulpfile.mjs`:

   ```js
   export function css() {
     return gulp
       .src("src/style.css")
       .pipe(csslint())
       .pipe(csslint.formatter("compact"))
       .pipe(
         autoprefixer({
           cascade: false,
         }),
       )
       .pipe(gulp.dest("build"));
   }
   ```

4. Aggiungi la seguente proprietà a `package.json`:

   ```json
   "browserslist": [
     "last 5 versions"
   ]
   ```

5. Cambia il task predefinito in:

   ```js
   export default gulp.series(html, css);
   ```

Qui prendiamo il nostro file `style.css`, lo eseguiamo con csslint (che emette un elenco di eventuali errori nel tuo CSS sul terminale), poi lo eseguiamo attraverso autoprefixer per aggiungere eventuali prefissi necessari per far funzionare nuove funzionalità CSS nei browser più vecchi. Alla fine della catena `pipe`, abbiamo l'output del nostro CSS modificato e prefisso nella directory `build`. Nota che questo funziona solo se csslint non trova nessun errore — prova a rimuovere una parentesi graffa dal tuo file CSS e ri-eseguire gulp per vedere che output ottieni!

#### js-hint e babel

1. Installa usando le seguenti righe:

   ```bash
   npm install --save-dev gulp-babel @babel/preset-env
   npm install --save-dev @babel/core
   npm install jshint gulp-jshint --save-dev
   ```

2. Aggiungi le seguenti dipendenze a `gulpfile.mjs`:

   ```js
   import babel from "gulp-babel";
   import jshint from "gulp-jshint";
   ```

3. Aggiungi il seguente test alla fine di `gulpfile.mjs`:

   ```js
   export function js() {
     return gulp
       .src("src/main.js")
       .pipe(jshint())
       .pipe(jshint.reporter("default"))
       .pipe(
         babel({
           presets: ["@babel/env"],
         }),
       )
       .pipe(gulp.dest("build"));
   }
   ```

4. Cambia il task predefinito in:

   ```js
   export default gulp.series(html, css, js);
   ```

Qui prendiamo il nostro file `main.js`, lo eseguiamo con `jshint` e emettiamo i risultati nel terminale usando `jshint.reporter`; quindi passiamo il file a babel, che lo converte in sintassi di vecchio stile e genera il risultato nella directory `build`. Il nostro codice originale includeva una [funzione arrotondata](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions), che babel ha modificato in una funzione di vecchio stile.

#### Ulteriori idee

Una volta impostato tutto, puoi eseguire il comando `gulp` all'interno della tua directory di progetto, e dovresti ottenere un output simile a questo:

![Output in un editor di codice dove le linee mostrano il tempo di inizio o fine delle attività, il nome dell'attività, e la durata delle attività 'Completate'.](gulp-output.png)

Puoi quindi provare i file prodotti dalle tue attività automatizzate visualizzandoli all'interno della directory `build`, e caricando `build/index.html` nel tuo browser web.

Se ricevi degli errori, verifica di aver aggiunto tutte le dipendenze e i test come mostrato sopra; prova anche a commentare le sezioni di codice HTML/CSS/JavaScript e poi a ri-eseguire gulp per vedere se riesci a isolare qual è il problema.

Gulp viene fornito con una funzione `watch()` che puoi usare per monitorare i tuoi file ed eseguire i test ogni volta che salvi un file. Ad esempio, prova ad aggiungere il seguente alla fine del tuo `gulpfile.mjs`:

```js
export function watch() {
  gulp.watch("src/*.html", html);
  gulp.watch("src/*.css", css);
  gulp.watch("src/*.js", js);
}
```

Ora prova a inserire il comando `gulp watch` nel tuo terminale. Gulp ora monitorerà la tua directory ed eseguirà i task appropriati ogni volta che salvi una modifica a un file HTML, CSS o JavaScript.

> [!NOTE]
> Il carattere `*` è un carattere jolly — qui stiamo dicendo "esegui questi task quando qualsiasi file di questi tipi viene salvato". Potresti anche utilizzare i caratteri jolly nei tuoi task principali, ad esempio `gulp.src('src/*.css')` prenderebbe tutti i tuoi file CSS e poi eseguirebbe i task in pipe su di essi.

C'è molto altro che puoi fare con Gulp. La [directory dei plugin di Gulp](https://gulpjs.com/plugins/) ha letteralmente migliaia di plugin da cercare.

### Altri task runner

Ci sono molti altri task runner disponibili. Certamente non stiamo cercando di dire che Gulp sia la soluzione migliore, ma funziona per noi ed è abbastanza accessibile ai principianti. Potresti anche provare a utilizzare altre soluzioni:

- Grunt funziona in modo molto simile a Gulp, tranne che si basa su task specificati in un file di configurazione, piuttosto che su JavaScript scritto. Vedi [Getting started with Grunt per maggiori dettagli](https://gruntjs.com/getting-started).
- Puoi anche eseguire task direttamente usando npm scripts situati all'interno del tuo file `package.json`, senza bisogno di installare alcun tipo di sistema di task runner extra. Questo funziona sul presupposto che cose come i plugin di Gulp siano fondamentalmente dei wrapper attorno a strumenti da riga di comando. Quindi, se riesci a capire come eseguire gli strumenti usando la riga di comando, puoi quindi eseguirli usando npm scripts. È un po' più complicato da gestire, ma può essere gratificante per coloro che hanno solide abilità con la riga di comando. [Why npm scripts?](https://css-tricks.com/why-npm-scripts/) fornisce una buona introduzione con una buona dose di ulteriori informazioni.

## Usare servizi di collaudo commerciali per accelerare il collaudo del browser

Ora diamo uno sguardo ai servizi di collaudo di browser di terze parti e cosa possono fare per noi.

Quando usi questi tipi di servizi, fornisci un URL della pagina che vuoi testare insieme a informazioni, come quali browser vuoi che venga testata. L'app quindi configura una nuova macchina virtuale con il sistema operativo e il browser a cui fai riferimento, e restituisce i risultati del test sotto forma di screenshot, video, file di log, testo, ecc. Questo è molto utile e decisamente più conveniente che dover configurare tutte le combinazioni di OS/browser da solo.

Puoi quindi fare un passo avanti, usando un'API per accedere alle funzionalità in modo programmato, il che significa che tali app possono essere combinate con task runner, come i propri ambienti Selenium locali e altri, per creare test automatizzati.

> [!NOTE]
> Ci sono altri sistemi di collaudo di browser commerciali disponibili ma in questo articolo ci concentreremo su BrowserStack, Sauce Labs e TestingBot. Non stiamo dicendo che questi siano necessariamente i migliori strumenti disponibili, ma sono buoni strumenti che sono semplici da usare per i principianti.

### BrowserStack

#### Iniziare con BrowserStack

Per iniziare:

1. Crea un [account di prova su BrowserStack](https://www.browserstack.com/users/sign_up).
2. Effettua l'accesso. Questo dovrebbe avvenire automaticamente dopo aver verificato il tuo indirizzo email.
3. Clicca sul link _Live_ nel menu di navigazione in alto per andare su Collaudo Manuale Live.

#### Le basi: Test manuali

La dashboard di BrowserStack Live ti consente di scegliere su quale dispositivo e browser vuoi testare — piattaforme a sinistra, dispositivi a destra. Seleziona un dispositivo per vedere la scelta dei browser disponibili su quel dispositivo.

![Scelte di Test](browserstack-test-choices-sized.png)

Cliccando su una di quelle icone di browser si caricherà la tua scelta di piattaforma, dispositivo e browser — scegli una ora, e provala.

![Dispositivi di Test](browserstack-test-device-sized.png)

Puoi inserire URL nella barra degli indirizzi, scorrere verso l'alto e il basso trascinando con il mouse e utilizzare gesti appropriati (ad esempio, pizzica per zoomare, due dita per scorrere) sui touchpad di dispositivi supportati come i MacBook. Non tutte le funzionalità sono disponibili su tutti i dispositivi.

Vedrai anche un menu che ti permette di controllare la sessione.

![Menu di Test](browserstack-test-menu-sized.png)

Le funzionalità disponibili variano a seconda del browser caricato e possono includere controlli per:

- Visualizzare informazioni sul browser corrente
- Passare ad altri browser
- Testare URL locali
- Impostare il livello di zoom e attivare la modalità orizzontale
- Salvare e caricare segnalibri
- Catturare/annotare screenshot e segnalare bug
- Accedere agli strumenti di sviluppo del browser
- Cambiare la posizione segnalata
- Limitare la rete
- Accedere ai lettori di schermo

#### Avanzato: L'API di BrowserStack

BrowserStack ha anche un'API [restful](https://www.browserstack.com/docs/automate/api-reference/selenium/introduction) che ti permette di recuperare programmáticamente i dettagli del tuo piano account, sessioni, build, ecc.

Diamo un'occhiata rapida a come potremmo accedere all'API usando Node.js.

1. Innanzitutto, configura un nuovo progetto npm per testarlo, come dettagliato in [Configurare Node e npm](#configurare_node_e_npm). Usa un nome di directory diverso rispetto a prima, come `bstack-test` per esempio.
2. Crea un nuovo file all'interno della tua root di progetto chiamato `call_bstack.js` e assegnagli il seguente contenuto:

   ```js
   const axios = require("axios");

   const bsUser = "BROWSERSTACK_USERNAME";
   const bsKey = "BROWSERSTACK_ACCESS_KEY";
   const baseUrl = `https://${bsUser}:${bsKey}@www.browserstack.com/automate/`;

   function getPlanDetails() {
     axios.get(`${baseUrl}plan.json`).then((response) => {
       console.log(response.data);
     });
     /* Response:
       {
         automate_plan: <string>,
         terminal_access: <string>.
         parallel_sessions_running: <int>,
         team_parallel_sessions_max_allowed: <int>,
         parallel_sessions_max_allowed: <int>,
         queued_sessions: <int>,
         queued_sessions_max_allowed: <int>
       }
       */
   }

   getPlanDetails();
   ```

3. Sostituisci i segnaposto per il nome utente BrowserStack e la chiave di accesso con i tuoi valori effettivi. Questi possono essere recuperati dai tuoi [Dettagli Account & Profilo di BrowserStack](https://www.browserstack.com/accounts/profile/details), sotto la sezione _Authentication & Security_.
4. Installa il modulo [axios](https://www.npmjs.com/package/axios) che stiamo utilizzando nel codice per gestire l'invio di richieste HTTP eseguendo il seguente comando nel tuo terminale (abbiamo scelto axios perché è semplice, popolare e ben supportato):

   ```bash
   npm install axios
   ```

5. Assicurati che il tuo file JavaScript sia salvato, ed eseguilo eseguendo il seguente comando nel tuo terminale. Dovresti vedere un oggetto stampato nel terminale contenente i dettagli del tuo piano BrowserStack.

   ```bash
   node call_bstack
   ```

Di seguito abbiamo fornito anche alcune altre funzioni pronte che potresti trovare utili quando lavori con l'API restful di BrowserStack.

Questa funzione restituisce dettagli riassuntivi di tutte le build automatizzate create in precedenza (vedi il prossimo articolo per i dettagli sui test automatizzati di [BrowserStack](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#browserstack)):

```js
function getBuilds() {
  axios.get(`${baseUrl}builds.json`).then((response) => {
    console.log(response.data);
  });

  /* Response:
  [
    {
      automation_build: {
        name: <string>,
        hashed_id: <string>,
        duration: <int>,
        status: <string>,
        build_tag: <string>,
        public_url: <string>
      }
    },
    {
      automation_build: {
        name: <string>,
        hashed_id: <string>,
        duration: <int>,
        status: <string>,
        build_tag: <string>,
        public_url: <string>
      }
    },
    // …
  ]
  */
}
```

Questa funzione restituisce i dettagli delle sessioni specifiche per una particolare build:

```js
function getSessionsInBuild(build) {
  const buildId = build.automation_build.hashed_id;
  axios.get(`${baseUrl}builds/${buildId}/sessions.json`).then((response) => {
    console.log(response.data);
  });
  /* Response:
  [
    {
      automation_session: {
        name: <string>,
        duration: <int>,
        os: <string>,
        os_version: <string>,
        browser_version: <string>,
        browser: <string>,
        device: <string>,
        status: <string>,
        hashed_id: <string>,
        reason: <string>,
        build_name: <string>,
        project_name: <string>,
        logs: <string>,
        browser_url: <string>,
        public_url: <string>,
        appium_logs_url: <string>,
        video_url: <string>,
        browser_console_logs_url: <string>,
        har_logs_url: <string>,
        selenium_logs_url: <string>
      }
    },
    {
      automation_session: {
        // …
      }
    },
    // …
  ]
  */
}
```

La seguente funzione restituisce i dettagli per una sessione particolare:

```js
function getSessionDetails(session) {
  const sessionId = session.automation_session.hashed_id;
  axios.get(`${baseUrl}sessions/${sessionId}.json`).then((response) => {
    console.log(response.data);
  });
  /* Response:
  {
    automation_session: {
      name: <string>,
      duration: <int>,
      os: <string>,
      os_version: <string>,
      browser_version: <string>,
      browser: <string>,
      device: <string>,
      status: <string>,
      hashed_id: <string>,
      reason: <string>,
      build_name: <string>,
      project_name: <string>,
      logs: <string>,
      browser_url: <string>,
      public_url: <string>,
      appium_logs_url: <string>,
      video_url: <string>,
      browser_console_logs_url: <string>,
      har_logs_url: <string>,
      selenium_logs_url: <string>
    }
  }
  */
}
```

#### Avanzato: Test automatizzati

Tratteremo [l'esecuzione di test automatizzati su BrowserStack](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#browserstack) nel prossimo articolo.

### Sauce Labs

#### Iniziare con Sauce Labs

Iniziamo con una prova su Sauce Labs.

1. Crea un account di prova su Sauce Labs.
2. Effettua l'accesso. Questo dovrebbe avvenire automaticamente dopo aver verificato il tuo indirizzo email.

#### Le basi: Test manuali

La [dashboard di Sauce Labs](https://app.saucelabs.com/dashboard/manual) ha molte opzioni disponibili. Per ora, assicurati di essere sulla scheda _Test Manuali_.

1. Clicca su _Inizia una nuova sessione manuale_.
2. Nella schermata successiva, digita l'URL della pagina che vuoi testare (usa <https://mdn.github.io/learning-area/javascript/building-blocks/events/show-video-box-fixed.html>, per esempio), poi scegli una combinazione browser/OS da testare utilizzando i diversi pulsanti e liste. Ci sono molte scelte, come vedrai! ![seleziona sessione manuals di sauce](sauce-manual-session.png)
3. Quando clicchi su Inizia sessione, apparirà una schermata di caricamento, che avvia una macchina virtuale che esegue la combinazione scelta.
4. Quando il caricamento è terminato, puoi quindi iniziare a testare in remoto il sito web che gira nel browser scelto. ![Test Sauce in esecuzione](sauce-test-running.png)
5. Da qui puoi vedere il layout come apparirebbe nel browser che stai testando, spostare il mouse in giro e provare a cliccare sui pulsanti, ecc. Il menu in alto ti permette di:

   - Fermare la sessione
   - Dare a qualcun altro un URL in modo che possano osservare il test in remoto.
   - Copiare testo/note in un appunti remoto.
   - Scattare uno screenshot.
   - Testare in modalità schermo intero.

Una volta fermata la sessione, tornerai alla scheda Test Manuali, dove vedrai una voce per ognuna delle sessioni manuali precedenti che hai iniziato. Cliccando su una di queste voci viene mostrato maggiori dati per la sessione. Qui puoi scaricare eventuali screenshot che hai scattato, guardare un video di quella sessione, visualizzare log di dati e altro ancora.

> [!NOTE]
> Questo è già molto utile e decisamente più comodo che dover configurare da soli tutti questi emulatori e macchine virtuali.

#### Avanzato: L'API di Sauce Labs

Sauce Labs ha un'API [restful](https://docs.saucelabs.com/dev/api/) che ti consente di recuperare programmáticamente dettagli del tuo account e test esistenti, e annotare i test con ulteriori dettagli, come lo stato di successo/fallimento che non è registrabile solo attraverso testi manuali. Ad esempio, potresti voler eseguire uno dei tuoi test Selenium in remoto usando Sauce Labs per testare una certa combinazione browser/OS, e quindi passare i risultati del test a Sauce Labs.

Ha diversi client disponibili per consentirti di effettuare chiamate all'API utilizzando il tuo ambiente preferito, sia esso PHP, Java, Node.js, ecc.

Diamo uno sguardo rapido a come si potrebbe accedere all'API usando Node.js e [node-saucelabs](https://github.com/saucelabs/node-saucelabs).

1. Innanzitutto, configura un nuovo progetto npm per testarlo, come dettagliato in [Configurare Node e npm](#configurare_node_e_npm). Usa un nome di directory diverso rispetto a prima, come `sauce-test` per esempio.
2. Installa il wrapper Node Sauce Labs utilizzando il seguente comando:

   ```bash
   npm install saucelabs
   ```

3. Crea un nuovo file all'interno della tua root di progetto chiamato `call_sauce.js`. Assegnagli il seguente contenuto:

   ```js
   const SauceLabs = require("saucelabs").default;

   (async () => {
     const myAccount = new SauceLabs({
       username: "your-sauce-username",
       password: "your-sauce-api-key",
     });

     // Get full WebDriver URL from the client depending on region:
     console.log(myAccount.webdriverEndpoint);

     // Get job details of last run job
     const jobs = await myAccount.listJobs("your-sauce-username", {
       limit: 1,
       full: true,
     });

     console.log(jobs);
   })();
   ```

4. Dovrai riempire le parti indicate col tuo nome utente di Sauce Labs e chiave API. Questi possono essere recuperati dalla tua pagina [Impostazioni Utente](https://app.saucelabs.com/user-settings). Riempi queste parti ora.
5. Assicurati che tutto sia salvato ed esegui il tuo file in questo modo:

   ```bash
   node call_sauce
   ```

#### Avanzato: Test automatizzati

Tratteremo l'esecuzione effettiva dei test su Sauce Lab nel prossimo articolo.

### TestingBot

#### Iniziare con TestingBot

Iniziamo con una prova su TestingBot.

1. Crea un [account di prova su TestingBot](https://testingbot.com/users/sign_up).
2. Effettua l'accesso. Questo dovrebbe avvenire automaticamente dopo aver verificato il tuo indirizzo email.

#### Le basi: Test manuali

La [dashboard di TestingBot](https://testingbot.com/members) elenca le varie opzioni tra cui puoi scegliere. Per ora, assicurati di trovarti sulla scheda _Testing Web dal Vivo_.

1. Inserire l'URL della pagina che vuoi testare.
2. Scegliere la combinazione di browser/OS che vuoi testare selezionando la combinazione nella griglia.
   ![Scelte di Test](screen_shot_2019-04-19_at_14.55.33.png)
3. Quando clicchi su _Avvia Browser_, apparirà una schermata di caricamento, che avvierà una macchina virtuale che esegue la combinazione scelta.
4. Quando il caricamento è terminato, puoi quindi iniziare a testare in remoto il sito web che gira nel browser scelto.
5. Da qui puoi vedere il layout come apparirebbe nel browser che stai testando, spostare il mouse in giro e provare a cliccare sui pulsanti, ecc. Il menu laterale ti permette di:

   - Fermare la sessione
   - Cambiare la risoluzione dello schermo
   - Copiare testo/note in un appunti remoto
   - Scattare, modificare e scaricare screenshot
   - Testare in modalità schermo intero.

Una volta fermata la sessione, tornerai alla pagina _Testing Web dal Vivo_, dove vedrai una voce per ognuna delle sessioni manuali precedenti che hai iniziato. Cliccando su una di queste voci viene mostrato maggiori dati per la sessione. Qui puoi scaricare eventuali screenshot che hai scattato, guardare un video di quella sessione e visualizzare i log per la sessione.

#### Avanzato: L'API di TestingBot

TestingBot ha un'API [restful](https://testingbot.com/support/api) che ti consente di recuperare programmáticamente dettagli del tuo account e test esistenti, e annotare i test con ulteriori dettagli, come lo stato di successo/fallimento che non è registrabile solo attraverso i test manuali.

TestingBot ha diversi client API che puoi usare per interagire con l'API, tra cui client per NodeJS, Python, Ruby, Java e PHP.

Di seguito è fornito un esempio su come interagire con l'API di TestingBot con il client NodeJS [testingbot-api](https://www.npmjs.com/package/testingbot-api).

1. Innanzitutto, configura un nuovo progetto npm per testarlo, come dettagliato in [Configurare Node e npm](#configurare_node_e_npm). Usa un nome di directory diverso rispetto a prima, come `tb-test` per esempio.
2. Installa il wrapper Node TestingBot utilizzando il seguente comando:

   ```bash
   npm install testingbot-api
   ```

3. Crea un nuovo file all'interno della tua root di progetto chiamato `tb.js`. Assegnagli il seguente contenuto:

   ```js
   const TestingBot = require("testingbot-api");

   let tb = new TestingBot({
     api_key: "your-tb-key",
     api_secret: "your-tb-secret",
   });

   tb.getTests(function (err, tests) {
     console.log(tests);
   });
   ```

4. Dovrai riempire le parti indicate con il tuo Key e Secret di TestingBot. Puoi trovarle nella [dashboard di TestingBot](https://testingbot.com/members/user/edit).
5. Assicurati che tutto sia salvato, ed esegui il file:

   ```bash
   node tb.js
   ```

#### Avanzato: Test automatizzati

Tratteremo l'esecuzione effettiva dei test automatizzati su TestingBot nel prossimo articolo.

## Riepilogo

È stata una lunga corsa, ma sono sicuro che puoi cominciare a vedere i benefici di utilizzare strumenti di automazione per fare un po' del lavoro pesante in termini di collaudo.

Nel prossimo articolo, vedremo come impostare il nostro sistema locale di automazione usando Selenium, e come combinarlo con servizi come Sauce Labs, BrowserStack e TestingBot.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing/Your_own_automation_environment", "Learn_web_development/Extensions/Testing")}}
