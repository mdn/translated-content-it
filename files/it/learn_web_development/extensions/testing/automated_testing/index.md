---
title: Introduzione ai test automatizzati
short-title: Test automatizzati
slug: Learn_web_development/Extensions/Testing/Automated_testing
l10n:
  sourceCommit: 6030ef1aadf967b80e2c79c3d3463cccc8ea0c95
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing/Your_own_automation_environment", "Learn_web_development/Extensions/Testing")}}

Eseguire manualmente i test su diversi browser e dispositivi, più volte al giorno, può diventare tedioso e richiedere molto tempo. Per gestire tutto ciò in modo efficiente, occorre familiarizzare con gli strumenti di automazione. In questo articolo verranno esaminate le soluzioni disponibili, come usare i task runner e come utilizzare le funzionalità di base di app commerciali per l'automazione dei test nei browser, quali Sauce Labs, BrowserStack e TestingBot.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>;
        conoscenza generale degli <a href="/it/docs/Learn_web_development/Extensions/Testing/Introduction">principi dei test cross-browser</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Fornire una comprensione di cosa comportano i test automatizzati, di come possano semplificare il lavoro e di come utilizzare alcuni prodotti commerciali che rendono tutto più semplice.
      </td>
    </tr>
  </tbody>
</table>

## L'automazione semplifica le cose

Nel corso di questo modulo sono stati illustrati molti modi diversi per testare siti web e app ed è stato spiegato quale dovrebbe essere l'ambito delle attività di test cross-browser in termini di browser da testare, considerazioni sull'accessibilità e altro ancora. Sembra molto lavoro, vero?

Siamo d'accordo: testare manualmente tutte le cose esaminate negli articoli precedenti può essere davvero faticoso. Fortunatamente, esistono strumenti che aiutano ad automatizzare parte di questa fatica. Esistono due modi principali per automatizzare i test trattati in questo modulo:

1. Usare un task runner come [Grunt](https://gruntjs.com/) o [Gulp](https://gulpjs.com/), oppure gli [script npm](https://docs.npmjs.com/misc/scripts/), per eseguire test e ripulire il codice durante il processo di build. Questo è un ottimo modo per eseguire attività come il linting e la minificazione del codice, l'aggiunta di prefissi CSS o la transpilation di funzionalità JavaScript nascenti per ottenere la massima compatibilità cross-browser e così via.
2. Usare un sistema di automazione del browser come [Selenium](https://www.selenium.dev/) per eseguire test specifici sui browser installati e restituire i risultati, avvisando dei fallimenti nei browser non appena si verificano. Le app commerciali di test cross-browser come [Sauce Labs](https://saucelabs.com/) e [BrowserStack](https://www.browserstack.com/) sono basate su Selenium, ma consentono di accedere alla loro configurazione in remoto tramite un'interfaccia, evitando il problema di dover configurare un sistema di test autonomo.

Nel prossimo articolo verrà illustrato come configurare un sistema di test basato su Selenium. In questo articolo verranno invece esaminati la configurazione di un task runner e l'uso delle funzionalità di base di sistemi commerciali come quelli menzionati sopra.

> [!NOTE]
> Le due categorie precedenti non si escludono a vicenda. È possibile configurare un task runner per accedere a un servizio come Sauce Labs tramite un'API, eseguire test cross-browser e restituire i risultati. Anche questo verrà esaminato di seguito.

## Usare un task runner per automatizzare gli strumenti di test

Come detto sopra, è possibile velocizzare drasticamente attività comuni come il linting e la minificazione del codice usando un task runner per eseguire automaticamente tutto ciò che serve in un determinato punto del processo di build. Ad esempio, questo potrebbe avvenire ogni volta che viene salvato un file, oppure in un altro momento. In questa sezione verrà illustrato come automatizzare l'esecuzione delle attività con Node e Gulp, un'opzione adatta ai principianti.

### Configurare Node e npm

Oggi la maggior parte degli strumenti è basata su {{Glossary("Node.js", "Node.js")}}, quindi sarà necessario installarlo insieme al relativo gestore di pacchetti, [`npm`](https://www.npmjs.com/):

1. Il modo più semplice per installare e aggiornare Node.js e `npm` consiste nell'usare un gestore delle versioni di Node: seguire le istruzioni in [Installazione di Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment#installing_node).
2. Assicurarsi di [verificare che l'installazione sia riuscita](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment#testing_your_node.js_and_npm_installation) prima di continuare.
3. Se Node.js/`npm` è stato installato in precedenza, è necessario aggiornarli alle versioni più recenti. È possibile farlo usando il gestore delle versioni di Node per installare le versioni LTS più recenti (fare nuovamente riferimento alle istruzioni collegate sopra).

Per iniziare a usare pacchetti basati su Node/npm nei progetti, è necessario configurare le directory del progetto come progetti npm. È facile da fare.

Ad esempio, per prima cosa creiamo una directory di test che consenta di sperimentare senza il timore di rompere qualcosa.

1. Creare una nuova directory in una posizione appropriata usando l'interfaccia del file manager oppure, dalla riga di comando, raggiungendo la posizione desiderata ed eseguendo il comando seguente:

   ```bash
   mkdir node-test
   ```

2. Per rendere questa directory un progetto npm, basta entrare nella directory di test e inizializzarla con quanto segue:

   ```bash
   cd node-test
   npm init
   ```

3. Questo secondo comando porrà diverse domande per ottenere le informazioni necessarie a configurare il progetto; per ora è possibile selezionare i valori predefiniti.
4. Dopo aver posto tutte le domande, verrà chiesto se le informazioni inserite sono corrette. Digitare `yes` e premere Invio/Return; npm genererà un file `package.json` nella directory.

Questo file è essenzialmente un file di configurazione per il progetto. Potrà essere personalizzato in seguito, ma per ora avrà un aspetto simile a questo:

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

A questo punto, è possibile procedere.

### Configurare l'automazione con Gulp

Vediamo come configurare Gulp e usarlo per automatizzare alcuni strumenti di test.

1. Per iniziare, creare un progetto npm di test usando la procedura descritta alla fine della sezione precedente.
   Inoltre, aggiornare il file `package.json` con la riga `"type": "module"`, in modo che assomigli a questo:

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

2. Successivamente, sarà necessario del contenuto HTML, CSS e JavaScript di esempio su cui testare il sistema: creare copie dei file di esempio [index.html](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/index.html), [main.js](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/main.js) e [style.css](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/style.css) in una sottocartella chiamata `src` all'interno della cartella del progetto.
   Se si preferisce, è possibile provare il proprio contenuto di test, ma tenere presente che questi strumenti non funzionano bene con JS/CSS incorporati nel file HTML: sono necessari file separati.
3. Installare gulp globalmente (ossia sarà disponibile in tutti i progetti) usando il comando seguente:

   ```bash
   npm install --global gulp-cli
   ```

4. Quindi, eseguire il comando seguente nella directory radice del progetto npm per configurare gulp come dipendenza del progetto:

   ```bash
   npm install --save-dev gulp
   ```

5. Ora creare un nuovo file nella directory del progetto chiamato `gulpfile.mjs`. Questo è il file che eseguirà tutte le attività. Inserire al suo interno quanto segue:

   ```js
   import gulp from "gulp";

   export default function (cb) {
     console.log("Gulp running");
     cb();
   }
   ```

   Questo importa il modulo `gulp` installato in precedenza, quindi esporta un'attività predefinita che non fa altro che stampare un messaggio nel terminale: è utile per sapere che Gulp sta funzionando. Nelle prossime sezioni, questa istruzione `export default` verrà modificata in qualcosa di più utile.

   Ogni attività gulp viene esportata nello stesso formato di base: `exports function taskName(cb) {...}`. Ogni funzione accetta un parametro: un callback da eseguire quando l'attività è completata.

6. È possibile eseguire l'attività predefinita di gulp con il comando seguente: provarlo ora:

   ```bash
   gulp
   ```

### Aggiungere attività reali a Gulp

Ora è possibile aggiungere altre attività al file Gulp. Ogni aggiunta potrebbe richiedere di modificare il file `gulpfile.mjs` nel modo seguente:

- Quando viene richiesto di aggiungere istruzioni `import`, aggiungerle sotto l'istruzione `import` esistente.
- Quando viene richiesto di aggiungere una nuova istruzione `export function ...`, aggiungerla alla fine del file.
- Quando viene richiesto di modificare l'export predefinito, modificare l'istruzione `export default` nel modo specificato.

Il file `gulpfile.mjs` crescerà quindi in questo modo:

```js
import gulp from "gulp";
// Add any new imports here

// Our latest default export
// export default ...

// Add any new task exports here
// export function ...
// export function ...
```

Per aggiungere attività reali a Gulp, occorre pensare a ciò che si vuole fare. Un insieme ragionevole di funzionalità di base da eseguire sul progetto è il seguente:

- html-tidy, css-lint e js-hint per eseguire il linting e segnalare/correggere errori HTML/CSS/JS comuni (vedere [gulp-htmltidy](https://www.npmjs.com/package/gulp-htmltidy), [gulp-csslint](https://www.npmjs.com/package/gulp-csslint), [gulp-jshint](https://www.npmjs.com/package/gulp-jshint)).
- Autoprefixer per analizzare il CSS e aggiungere prefissi dei vendor solo dove necessario (vedere [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer)).
- babel per eseguire la transpilation delle nuove funzionalità della sintassi JavaScript in sintassi tradizionale che funziona nei browser più vecchi (vedere [gulp-babel](https://www.npmjs.com/package/gulp-babel)).

Consultare i collegamenti precedenti per istruzioni complete sui diversi pacchetti gulp utilizzati.

Per utilizzare ogni plugin, occorre prima installarlo tramite npm, quindi importare eventuali dipendenze nella parte superiore del file `gulpfile.mjs`, poi aggiungere i test nella parte inferiore del file e infine esportare il nome dell'attività affinché sia disponibile tramite il comando di gulp.

#### html-tidy

1. Installare usando la riga seguente:

   ```bash
   npm install --save-dev gulp-htmltidy
   ```

   > [!NOTE]
   > `--save-dev` aggiunge il pacchetto come dipendenza del progetto. Osservando il file `package.json` del progetto, sarà presente una voce corrispondente nella proprietà `devDependencies`.

2. Aggiungere la dipendenza seguente a `gulpfile.mjs`:

   ```js
   import htmltidy from "gulp-htmltidy";
   ```

3. Aggiungere il test seguente in fondo a `gulpfile.mjs`:

   ```js
   export function html() {
     return gulp
       .src("src/index.html")
       .pipe(htmltidy())
       .pipe(gulp.dest("build"));
   }
   ```

4. Modificare l'export predefinito in:

   ```js
   export default html;
   ```

Qui il file di sviluppo `index.html` viene ottenuto con `gulp.src()`, che permette di selezionare un file sorgente su cui eseguire un'operazione.

Successivamente viene usata la funzione `pipe()` per passare il sorgente a un altro comando che esegue un'ulteriore operazione. È possibile concatenarne quante se ne desiderano. Per prima cosa viene eseguito `htmltidy()` sul sorgente, che scorre il file e corregge gli errori. La seconda funzione `pipe()` scrive il file HTML di output nella directory `build`.

Nella versione di input del file, potrebbe essere stato notato un elemento {{htmlelement("p")}} vuoto; htmltidy lo ha rimosso prima della creazione del file di output.

#### Autoprefixer e css-lint

1. Installare usando le righe seguenti:

   ```bash
   npm install --save-dev gulp-autoprefixer
   npm install --save-dev gulp-csslint
   ```

2. Aggiungere le dipendenze seguenti a `gulpfile.mjs`:

   ```js
   import autoprefixer from "gulp-autoprefixer";
   import csslint from "gulp-csslint";
   ```

3. Aggiungere il test seguente in fondo a `gulpfile.mjs`:

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

4. Aggiungere la proprietà seguente a `package.json`:

   ```json
   {
     "browserslist": ["last 5 versions"]
   }
   ```

5. Modificare l'attività predefinita in:

   ```js
   export default gulp.series(html, css);
   ```

Qui viene ottenuto il file `style.css`, viene eseguito csslint su di esso (che restituisce nel terminale un elenco di eventuali errori nel CSS), quindi viene elaborato da autoprefixer per aggiungere i prefissi necessari per far funzionare le funzionalità CSS nascenti nei browser più vecchi. Alla fine della catena di `pipe`, il CSS modificato e con prefissi viene scritto nella directory `build`. Si noti che questo funziona solo se csslint non trova errori: provare a rimuovere una parentesi graffa dal file CSS ed eseguire nuovamente gulp per vedere quale output viene ottenuto.

#### js-hint e babel

1. Installare usando le righe seguenti:

   ```bash
   npm install --save-dev gulp-babel @babel/preset-env
   npm install --save-dev @babel/core
   npm install jshint gulp-jshint --save-dev
   ```

2. Aggiungere le dipendenze seguenti a `gulpfile.mjs`:

   ```js
   import babel from "gulp-babel";
   import jshint from "gulp-jshint";
   ```

3. Aggiungere il test seguente in fondo a `gulpfile.mjs`:

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

4. Modificare l'attività predefinita in:

   ```js
   export default gulp.series(html, css, js);
   ```

Qui viene ottenuto il file `main.js`, viene eseguito `jshint` su di esso e i risultati vengono inviati al terminale usando `jshint.reporter`; il file viene quindi passato a babel, che lo converte nella sintassi vecchio stile e restituisce il risultato nella directory `build`. Il codice originale includeva una [funzione arrow](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions), che babel ha modificato in una funzione vecchio stile.

#### Ulteriori idee

Dopo aver configurato tutto, è possibile eseguire il comando `gulp` nella directory del progetto e si dovrebbe ottenere un output simile a questo:

![Output in un editor di codice in cui le righe mostrano l'ora di inizio o fine delle attività, il nome dell'attività e la durata delle attività "Finished".](gulp-output.png)

È quindi possibile provare i file generati dalle attività automatizzate osservandoli nella directory `build` e caricando `build/index.html` nel browser web.

In caso di errori, verificare di aver aggiunto tutte le dipendenze e i test come mostrato sopra; provare anche a commentare le sezioni di codice HTML/CSS/JavaScript e rieseguire gulp per cercare di isolare il problema.

Gulp include una funzione `watch()` che può essere usata per monitorare i file ed eseguire i test ogni volta che viene salvato un file. Ad esempio, provare ad aggiungere quanto segue in fondo a `gulpfile.mjs`:

```js
export function watch() {
  gulp.watch("src/*.html", html);
  gulp.watch("src/*.css", css);
  gulp.watch("src/*.js", js);
}
```

Ora provare a immettere il comando `gulp watch` nel terminale. Gulp monitorerà ora la directory ed eseguirà le attività appropriate ogni volta che viene salvata una modifica a un file HTML, CSS o JavaScript.

> [!NOTE]
> Il carattere `*` è un carattere jolly: qui si sta dicendo "esegui queste attività quando viene salvato qualsiasi file di questi tipi". È anche possibile usare caratteri jolly nelle attività principali; ad esempio, `gulp.src('src/*.css')` selezionerebbe tutti i file CSS ed eseguirebbe poi le attività concatenate su di essi.

Con Gulp è possibile fare molto altro. La [directory dei plugin di Gulp](https://gulpjs.com/plugins/) contiene letteralmente migliaia di plugin da esplorare.

### Altri task runner

Sono disponibili molti altri task runner. Non si intende certo affermare che Gulp sia la soluzione migliore in assoluto, ma funziona bene ed è abbastanza accessibile ai principianti. Si possono provare anche altre soluzioni:

- Grunt funziona in modo molto simile a Gulp, ma si basa su attività specificate in un file di configurazione, anziché sull'uso di JavaScript scritto. Per maggiori dettagli, vedere [Introduzione a Grunt](https://gruntjs.com/getting-started).
- È anche possibile eseguire attività direttamente usando script npm presenti nel file `package.json`, senza dover installare alcun sistema task runner aggiuntivo. Questo si basa sul presupposto che elementi come i plugin Gulp siano essenzialmente wrapper attorno a strumenti da riga di comando. Pertanto, se si riesce a capire come eseguire gli strumenti dalla riga di comando, è possibile eseguirli anche usando gli script npm. È un po' più difficile da usare, ma può essere gratificante per chi possiede solide competenze nella riga di comando. [Why npm scripts?](https://css-tricks.com/why-npm-scripts/) fornisce una buona introduzione con molte ulteriori informazioni.

## Usare servizi di test commerciali per velocizzare i test nei browser

Vediamo ora i servizi commerciali di terze parti per il test nei browser e cosa possono fare.

Quando si usano questi tipi di servizi, si fornisce l'URL della pagina da testare insieme a informazioni quali i browser in cui eseguire il test. L'app configura quindi una nuova VM con il sistema operativo e il browser specificati e restituisce i risultati del test sotto forma di screenshot, video, file di log, testo e così via. Questo è molto utile e decisamente più comodo rispetto al dover configurare autonomamente tutte le combinazioni di sistemi operativi e browser.

Si può quindi fare un ulteriore passo avanti usando un'API per accedere alle funzionalità a livello programmatico: ciò significa che tali app possono essere combinate con task runner, come ambienti Selenium locali e altri, per creare test automatizzati.

> [!NOTE]
> Sono disponibili altri sistemi commerciali per il test nei browser, ma in questo articolo l'attenzione sarà rivolta a BrowserStack, Sauce Labs e TestingBot. Non si intende affermare che questi siano necessariamente gli strumenti migliori disponibili, ma sono valide soluzioni semplici da configurare e avviare per i principianti.

### BrowserStack

#### Iniziare con BrowserStack

Per iniziare:

1. Creare un [account di prova BrowserStack](https://www.browserstack.com/users/sign_up).
2. Accedere. Questo dovrebbe avvenire automaticamente dopo aver verificato l'indirizzo email.
3. Fare clic sul collegamento _Live_ nel menu di navigazione superiore per accedere ai test manuali Live.

#### Le basi: test manuali

La dashboard BrowserStack Live consente di scegliere la piattaforma, il dispositivo e il browser su cui eseguire il test.
Per i test desktop, si selezionano direttamente il sistema operativo e il browser.
Per i dispositivi mobili, si sceglie il sistema operativo mobile e il dispositivo, quindi si può selezionare un browser per la combinazione dispositivo-browser.

![Scelte del test](browserstack-test-choices-sized.png)

Facendo clic su una di queste icone del browser verrà caricata la scelta di piattaforma, dispositivo e browser: sceglierne una ora e provarla.

![Dispositivi di test](browserstack-test-device-sized.png)

È possibile inserire URL nella barra degli indirizzi, scorrere verso l'alto e verso il basso trascinando con il mouse e usare i gesti appropriati, ad esempio pizzicare/ingrandire e usare due dita per scorrere, sui touchpad di dispositivi supportati come i MacBook.

Le funzionalità disponibili variano in base al browser caricato e possono includere controlli per:

- Visualizzare informazioni sul browser corrente
- Passare ad altri browser
- Testare URL localhost
- Impostare il livello di zoom e cambiare l'orientamento
- Salvare e caricare segnalibri
- Acquisire/annotare screenshot e segnalare bug
- Accedere ai DevTools del browser
- Modificare la posizione segnalata
- Limitare la rete
- Accedere ai lettori di schermo

![Menu di test](browserstack-test-menu-sized.png)

Per ulteriori informazioni, consultare la documentazione di [BrowserStack Live](https://www.browserstack.com/docs/live).

#### Avanzato: l'API BrowserStack

BrowserStack dispone anche di un'[API RESTful](https://www.browserstack.com/docs/automate/api-reference/selenium/introduction) che consente di recuperare a livello programmatico i dettagli del piano dell'account, delle sessioni, delle build e così via.

Vediamo brevemente come accedere all'API usando Node.js.

1. Per prima cosa, configurare un nuovo progetto npm per provare questa procedura, come descritto in [Configurare Node e npm](#configurare-node-e-npm). Usare un nome di directory diverso da quello precedente, ad esempio `bstack-test`.
2. Creare un nuovo file nella radice del progetto chiamato `call_bstack.js` e assegnargli il contenuto seguente:

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

3. Sostituire i segnaposto del nome utente e della chiave di accesso BrowserStack con i valori effettivi. Possono essere recuperati da [BrowserStack Account & Profile Details](https://www.browserstack.com/accounts/profile/details), nella sezione _Authentication & Security_.
4. Installare il modulo [axios](https://www.npmjs.com/package/axios) usato nel codice per gestire l'invio delle richieste HTTP eseguendo il comando seguente nel terminale (axios è stato scelto perché semplice, popolare e ben supportato):

   ```bash
   npm install axios
   ```

5. Assicurarsi che il file JavaScript sia salvato ed eseguirlo con il comando seguente nel terminale. Nel terminale dovrebbe essere stampato un oggetto contenente i dettagli del piano BrowserStack.

   ```bash
   node call_bstack
   ```

Di seguito sono fornite anche altre funzioni pronte all'uso che potrebbero risultare utili quando si lavora con l'API RESTful BrowserStack.

Questa funzione restituisce i dettagli riepilogativi di tutte le build automatizzate create in precedenza (vedere l'articolo successivo per i [dettagli dei test automatizzati BrowserStack](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#browserstack)):

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

Questa funzione restituisce i dettagli delle sessioni specifiche di una particolare build:

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

La funzione seguente restituisce i dettagli di una particolare sessione:

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

#### Avanzato: test automatizzati

I [test BrowserStack automatizzati](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#browserstack) verranno trattati nel prossimo articolo.

### Sauce Labs

#### Iniziare con Sauce Labs

Iniziamo con una prova di Sauce Labs.

1. Creare un account di prova Sauce Labs.
2. Accedere. Questo dovrebbe avvenire automaticamente dopo aver verificato l'indirizzo email.

#### Le basi: test manuali

La [dashboard Sauce Labs](https://app.saucelabs.com/dashboard/manual) offre molte opzioni.
Dopo aver effettuato l'accesso, seguire la guida "Getting started" nella parte superiore sinistra della pagina:

1. In "Run your first test", fare clic su _Desktop browser_.
2. Nella schermata successiva, digitare l'URL di una pagina da testare, come questa pagina, quindi scegliere una combinazione browser/OS da testare usando i vari pulsanti ed elenchi.
   C'è davvero molto tra cui scegliere!
   ![selezione della sessione manuale Sauce](sauce-manual-session.png)
3. Quando si avvia il test, verrà visualizzata una schermata di caricamento e verrà avviato un ambiente che esegue la combinazione dispositivo/browser scelta.
   È quindi possibile iniziare a testare da remoto il sito web in esecuzione nel browser scelto.

A questo punto è possibile fare molte cose, come condividere un URL di test affinché qualcun altro possa osservare il test da remoto, copiare testo/note negli appunti remoti, acquisire uno screenshot, testare in modalità a schermo intero e altro ancora.

Quando si interrompe la sessione, si torna alla scheda _Live_, dove sarà presente una voce per ciascuna delle sessioni manuali avviate in precedenza.
Facendo clic su una di queste voci vengono visualizzati ulteriori dati relativi alla sessione.
Qui è possibile scaricare gli screenshot acquisiti, guardare un video della sessione, visualizzare i log dei dati e altro ancora.
Questo è già molto utile e molto più comodo rispetto al dover configurare autonomamente più emulatori e macchine virtuali.

Per ulteriori informazioni, consultare la [documentazione di Sauce Labs](https://docs.saucelabs.com/).

#### Avanzato: l'API Sauce Labs

Sauce Labs dispone di un'[API RESTful](https://docs.saucelabs.com/dev/api/) che consente di recuperare a livello programmatico i dettagli dell'account e dei test esistenti e di annotare i test con ulteriori dettagli, come il loro stato di superamento/fallimento, che non è registrabile tramite i soli test manuali. Ad esempio, si potrebbe voler eseguire uno dei propri test Selenium in remoto usando Sauce Labs per testare una determinata combinazione browser/OS e poi restituire i risultati del test a Sauce Labs.

Sono disponibili diversi client che consentono di effettuare chiamate all'API usando l'ambiente preferito, sia esso PHP, Java, Node.js e così via.

Vediamo brevemente come accedere all'API usando Node.js e [node-saucelabs](https://github.com/saucelabs/node-saucelabs).

1. Per prima cosa, configurare un nuovo progetto npm per provare questa procedura, come descritto in [Configurare Node e npm](#configurare-node-e-npm). Usare un nome di directory diverso da quello precedente, ad esempio `sauce-test`.
2. Installare il wrapper Node Sauce Labs usando il comando seguente:

   ```bash
   npm install saucelabs
   ```

3. Creare un nuovo file nella radice del progetto chiamato `call_sauce.js`. Assegnargli il contenuto seguente:

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

4. Sarà necessario inserire il nome utente Sauce Labs e la chiave API nei punti indicati. Possono essere recuperati dalla pagina [User Settings](https://app.saucelabs.com/user-settings). Inserirli ora.
5. Assicurarsi che tutto sia salvato ed eseguire il file nel modo seguente:

   ```bash
   node call_sauce
   ```

#### Avanzato: test automatizzati

L'esecuzione effettiva dei test automatizzati Sauce Lab verrà trattata nel prossimo articolo.

### TestingBot

#### Iniziare con TestingBot

Iniziamo con una prova di TestingBot.

1. Creare un [account di prova TestingBot](https://testingbot.com/users/sign_up).
2. Accedere. Questo dovrebbe avvenire automaticamente dopo aver verificato l'indirizzo email.

#### Le basi: test manuali

La [dashboard TestingBot](https://testingbot.com/members) elenca le varie opzioni disponibili. Per ora, assicurarsi di trovarsi nella scheda _Live Web Testing_.

1. Inserire l'URL della pagina da testare.
2. Scegliere la combinazione browser/OS da testare selezionandola nella griglia.
   ![Scelte del test](screen_shot_2019-04-19_at_14.55.33.png)
3. Facendo clic su _Start Browser_, verrà visualizzata una schermata di caricamento che avvia una macchina virtuale in esecuzione con la combinazione scelta.
4. Al termine del caricamento, è possibile iniziare a testare da remoto il sito web in esecuzione nel browser scelto.
5. Da qui è possibile vedere il layout come apparirebbe nel browser in fase di test, muovere il mouse e provare a fare clic sui pulsanti e così via. Il menu laterale consente di:
   - Interrompere la sessione
   - Modificare la risoluzione dello schermo
   - Copiare testo/note negli appunti remoti
   - Acquisire, modificare e scaricare screenshot
   - Testare in modalità a schermo intero.

Quando si interrompe la sessione, si torna alla pagina _Live Web Testing_, dove sarà presente una voce per ciascuna delle sessioni manuali avviate in precedenza. Facendo clic su una di queste voci vengono visualizzati ulteriori dati relativi alla sessione. Qui è possibile scaricare gli screenshot acquisiti, guardare un video del test e visualizzare i log della sessione.

#### Avanzato: l'API TestingBot

TestingBot dispone di un'[API RESTful](https://testingbot.com/support/api) che consente di recuperare a livello programmatico i dettagli dell'account e dei test esistenti e di annotare i test con ulteriori dettagli, come il loro stato di superamento/fallimento, che non è registrabile tramite i soli test manuali.

TestingBot dispone di diversi client API utilizzabili per interagire con l'API, inclusi client per Node.js, Python, Ruby, Java e PHP.

Di seguito è riportato un esempio di come interagire con l'API TestingBot tramite il client Node.js [testingbot-api](https://www.npmjs.com/package/testingbot-api).

1. Per prima cosa, configurare un nuovo progetto npm per provare questa procedura, come descritto in [Configurare Node e npm](#configurare-node-e-npm). Usare un nome di directory diverso da quello precedente, ad esempio `tb-test`.
2. Installare il wrapper Node TestingBot usando il comando seguente:

   ```bash
   npm install testingbot-api
   ```

3. Creare un nuovo file nella radice del progetto chiamato `tb.js`. Assegnargli il contenuto seguente:

   ```js
   const TestingBot = require("testingbot-api");

   let tb = new TestingBot({
     api_key: "your-tb-key",
     api_secret: "your-tb-secret",
   });

   tb.getTests((err, tests) => {
     console.log(tests);
   });
   ```

4. Sarà necessario inserire TestingBot Key e Secret nei punti indicati. Possono essere trovati nella [dashboard TestingBot](https://testingbot.com/members/user/edit).
5. Assicurarsi che tutto sia salvato ed eseguire il file:

   ```bash
   node tb.js
   ```

#### Avanzato: test automatizzati

L'esecuzione effettiva dei test automatizzati TestingBot verrà trattata nel prossimo articolo.

## Riepilogo

È stato un percorso piuttosto lungo, ma ora dovrebbero essere evidenti i vantaggi dell'uso di strumenti di automazione per svolgere parte del lavoro più impegnativo relativo ai test.

Nel prossimo articolo verrà esaminata la configurazione di un sistema di automazione locale basato su Selenium e come combinarlo con servizi quali Sauce Labs, BrowserStack e TestingBot.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing/Your_own_automation_environment", "Learn_web_development/Extensions/Testing")}}
