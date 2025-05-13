---
title: Introduzione al testing automatizzato
short-title: Testing automatizzato
slug: Learn_web_development/Extensions/Testing/Automated_testing
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing/Your_own_automation_environment", "Learn_web_development/Extensions/Testing")}}

Eseguire manualmente test su diversi browser e dispositivi, più volte al giorno, può diventare noioso e richiedere molto tempo. Per gestire questo in modo efficiente, dovrebbe familiarizzare con gli strumenti di automazione. In questo articolo, esaminiamo ciò che è disponibile, come utilizzare i task runner e come utilizzare le basi delle app commerciali di automazione dei test del browser come Sauce Labs, BrowserStack e TestingBot.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>, <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>;
        un'idea dei principi generali del <a href="/it/docs/Learn_web_development/Extensions/Testing/Introduction">testing cross-browser</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Fornire una comprensione di cosa comporta il testing automatizzato, come può semplificarle la vita e come utilizzare alcuni dei prodotti commerciali che rendono le cose più facili.
      </td>
    </tr>
  </tbody>
</table>

## L'automazione rende le cose facili

In tutto questo modulo abbiamo dettagliato molti modi diversi in cui può testare i suoi siti web e le sue app, e spiegato il tipo di portata che i suoi sforzi di testing cross-browser dovrebbero avere in termini di quali browser testare, considerazioni sull'accessibilità e altro. Sembra un sacco di lavoro, vero?

Siamo d'accordo — testare manualmente tutti gli aspetti che abbiamo esaminato negli articoli precedenti può essere davvero complicato. Fortunatamente, ci sono strumenti che ci aiutano ad automatizzare parte di questa fatica. Ci sono due modi principali in cui possiamo automatizzare i test di cui abbiamo parlato in questo modulo:

1. Utilizzare un task runner come [Grunt](https://gruntjs.com/) o [Gulp](https://gulpjs.com/), o [npm scripts](https://docs.npmjs.com/misc/scripts/) per eseguire test e pulire il codice durante il processo di build. Questo è un ottimo modo per eseguire attività come l'analisi del codice e la diminuzione delle sue dimensioni, aggiungere prefissi CSS o traspilare nuove funzionalità JavaScript per una massima copertura cross-browser, e così via.
2. Utilizzare un sistema di automazione del browser come [Selenium](https://www.selenium.dev/) per eseguire test specifici sui browser installati e restituire i risultati, avvisandola di eventuali errori nei browser man mano che emergono. Le app commerciali di testing cross-browser come [Sauce Labs](https://saucelabs.com/) e [BrowserStack](https://www.browserstack.com/) si basano su Selenium, ma le consentono di accedere alla loro configurazione da remoto utilizzando un'interfaccia, risparmiandole la fatica di configurare il suo sistema di testing personale.

Nel prossimo articolo esamineremo come configurare il suo sistema di testing basato su Selenium. In questo articolo, vedremo come configurare un task runner e utilizzare la funzionalità base dei sistemi commerciali come quelli sopra menzionati.

> [!NOTE]
> Le due categorie sopra non si escludono a vicenda. È possibile configurare un task runner per accedere a un servizio come Sauce Labs o LambdaTest tramite un'API, eseguire test cross-browser e restituire i risultati. Ne parleremo più avanti.

## Utilizzare un task runner per automatizzare gli strumenti di testing

Come detto sopra, può accelerare notevolmente attività comuni come l'analisi e la diminuzione del codice utilizzando un task runner per eseguire automaticamente tutto ciò di cui ha bisogno in un certo punto nel suo processo di build. Ad esempio, potrebbe essere ogni volta che salva un file, o in qualche altro punto. In questa sezione vedremo come automatizzare l'esecuzione di task con Node e Gulp, un'opzione amichevole per i principianti.

### Configurare Node e npm

La maggior parte degli strumenti oggi si basa su {{Glossary("Node.js", "Node.js")}}, quindi avrà bisogno di installarlo insieme al suo gestore di pacchetti, [`npm`](https://www.npmjs.com/):

1. Il modo più semplice per installare e aggiornare Node.js e `npm` è tramite un gestore di versioni di node: segua le istruzioni in [Installing Node](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment#installing_node) per farlo.
2. Si assicuri di [testare che l'installazione sia avvenuta con successo](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment#testing_your_nodejs_and_npm_installation) prima di continuare.
3. Se ha già installato Node.js/`npm`, dovrebbe aggiornarli alle loro versioni più recenti. Può farlo utilizzando il gestore di versioni di node per installare le ultime versioni LTS (fare nuovamente riferimento alle istruzioni collegate sopra).

Per iniziare a utilizzare i pacchetti basati su Node/npm nei suoi progetti, deve configurare le directory del progetto come progetti npm. Questo è facile da fare.

Ad esempio, creiamo prima una directory di test per permetterci di provare senza paura di rompere qualcosa.

1. Crei una nuova directory in un luogo sensato utilizzando l'interfaccia utente del suo gestore di file, oppure, da una riga di comando, navigando nella posizione desiderata ed eseguendo il seguente comando:

   ```bash
   mkdir node-test
   ```

2. Per rendere questa directory un progetto npm, deve solo entrare nella sua directory di test e inizializzarla con il seguente comando:

   ```bash
   cd node-test
   npm init
   ```

3. Questo secondo comando le farà molte domande per trovare le informazioni necessarie per configurare il progetto; può selezionare i valori predefiniti per ora.
4. Una volta che tutte le domande sono state poste, le verrà chiesto se le informazioni inserite sono corrette. Digiti `yes` e premi Enter/Return e npm genererà un file `package.json` nella sua directory.

Questo file è fondamentalmente un file di configurazione per il progetto. Può personalizzarlo più avanti, ma per ora apparirà qualcosa del genere:

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

Con questo, è pronta a procedere.

### Configurazione dell'automazione Gulp

Esaminiamo la configurazione di Gulp e il suo utilizzo per automatizzare alcuni strumenti di testing.

1. Per iniziare, crei un progetto npm di test utilizzando la procedura dettagliata alla fine della sezione precedente.
   Inoltre, aggiorni il file `package.json` con la riga: `"type": "module"` in modo che appaia qualcosa del genere:

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

2. Successivamente, avrà bisogno di alcuni contenuti di esempio HTML, CSS e JavaScript sui quali testare il suo sistema — faccia copie dei nostri file di esempio [index.html](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/index.html), [main.js](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/main.js), e [style.css](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/automation/style.css) in una sottocartella con il nome `src` all'interno della sua cartella di progetto.
   Può provare i suoi contenuti di test se lo desidera, ma tenga presente che tali strumenti non funzionano bene con JS/CSS inseriti nel file HTML — ha bisogno di file separati.
3. Installi gulp globalmente (cioè, sarà disponibile per tutti i progetti) usando il seguente comando:

   ```bash
   npm install --global gulp-cli
   ```

4. Successivamente, esegua il seguente comando all'interno della sua directory di progetto npm per impostare gulp come dipendenza del suo progetto:

   ```bash
   npm install --save-dev gulp
   ```

5. Ora crei un nuovo file all'interno della directory del suo progetto chiamato `gulpfile.mjs`. Questo è il file che eseguirà tutte le nostre attività. All'interno di questo file, aggiunga quanto segue:

   ```js
   import gulp from "gulp";

   export default function (cb) {
     console.log("Gulp running");
     cb();
   }
   ```

   Questo richiede il modulo `gulp` che abbiamo installato in precedenza, e poi esporta un'attività predefinita che non fa altro che stampare un messaggio sul terminale — questo è utile per farci sapere che Gulp sta funzionando. Nei prossimi paragrafi, cambieremo questa dichiarazione `export default` in qualcosa di più utile.

   Ogni attività gulp è esportata nello stesso formato di base — `exports function taskName(cb) {...}`. Ogni funzione prende un parametro — un callback da eseguire quando l'attività è completata.

6. Può eseguire l'attività predefinita di gulp con il seguente comando — provi ora:

   ```bash
   gulp
   ```

### Aggiungere alcune attività reali a Gulp

Ora siamo pronti ad aggiungere più attività al nostro file di Gulp. Ogni aggiunta potrebbe richiederle di modificare il file `gulpfile.mjs` nel seguente modo:

- Quando le chiediamo di aggiungere alcune dichiarazioni di `import`, le aggiunga sotto l'istruzione `import` esistente.
- Quando le chiediamo di aggiungere una nuova dichiarazione `export function ...`, la aggiunga alla fine del file.
- Quando le chiediamo di cambiare l'export predefinito, cambi la dichiarazione `export default` nel modo che specifichiamo.

Quindi il suo file `gulpfile.mjs` crescerà in questo modo:

```js
import gulp from "gulp";
// Add any new imports here

// Our latest default export
// export default ...

// Add any new task exports here
// export function ...
// export function ...
```

Per aggiungere alcune attività reali a Gulp, dobbiamo pensare a cosa vogliamo fare. Un insieme ragionevole di funzionalità di base da eseguire sul nostro progetto è il seguente:

- html-tidy, css-lint, e js-hint per fare lint e segnalare/riparare errori comuni di HTML/CSS/JS (vedi [gulp-htmltidy](https://www.npmjs.com/package/gulp-htmltidy), [gulp-csslint](https://www.npmjs.com/package/gulp-csslint), [gulp-jshint](https://www.npmjs.com/package/gulp-jshint)).
- Autoprefixer per scansionare il nostro CSS e aggiungere prefissi del venditore solo dove necessario (vedi [gulp-autoprefixer](https://www.npmjs.com/package/gulp-autoprefixer)).
- babel per traspilare eventuali nuove funzionalità della sintassi JavaScript in sintassi tradizionale che funziona nei browser più vecchi (vedi [gulp-babel](https://www.npmjs.com/package/gulp-babel)).

Veda i link sopra per le istruzioni complete sui diversi pacchetti gulp che utilizziamo.

Per utilizzare ogni plugin, deve prima installarlo tramite npm, quindi richiamare le dipendenze necessarie all'inizio del file `gulpfile.mjs`, quindi aggiungere il suo/i test alla fine e infine esportare il nome della sua attività per essere disponibile tramite il comando di gulp.

#### html-tidy

1. Installi usando la seguente riga:

   ```bash
   npm install --save-dev gulp-htmltidy
   ```

   > **Nota:** `--save-dev` aggiunge il pacchetto come dipendenza al suo progetto. Se guarda nel file `package.json` del suo progetto, vedrà una voce per esso nella proprietà `devDependencies`.

2. Aggiunga la seguente dipendenza a `gulpfile.mjs`:

   ```js
   import htmltidy from "gulp-htmltidy";
   ```

3. Aggiunga il seguente test alla fine di `gulpfile.mjs`:

   ```js
   export function html() {
     return gulp
       .src("src/index.html")
       .pipe(htmltidy())
       .pipe(gulp.dest("build"));
   }
   ```

4. Cambi l'export predefinito in:

   ```js
   export default html;
   ```

Qui stiamo acquisendo il nostro file di sviluppo `index.html` con `gulp.src()`, che ci permette di acquisire un file sorgente per fare qualcosa con esso.

Successivamente, utilizziamo la funzione `pipe()` per passare quella sorgente a un altro comando per fare qualcos'altro con essa. Possiamo concatenare quanti più di questi vogliamo. Per prima cosa eseguiamo `htmltidy()` sulla sorgente, che passa in rassegna e corregge gli errori nel nostro file. La seconda funzione `pipe()` scrive il file HTML di output nella directory `build`.

Nella versione di input del file, potrebbe aver notato che abbiamo messo un elemento {{htmlelement("p")}} vuoto; htmltidy l'ha rimosso al momento in cui è stato creato il file di output.

#### Autoprefixer e css-lint

1. Installi usando le seguenti righe:

   ```bash
   npm install --save-dev gulp-autoprefixer
   npm install --save-dev gulp-csslint
   ```

2. Aggiunga le seguenti dipendenze a `gulpfile.mjs`:

   ```js
   import autoprefixer from "gulp-autoprefixer";
   import csslint from "gulp-csslint";
   ```

3. Aggiunga il seguente test alla fine di `gulpfile.mjs`:

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

4. Aggiunga la seguente proprietà a `package.json`:

   ```json
   "browserslist": [
     "last 5 versions"
   ]
   ```

5. Cambi l'attività predefinita in:

   ```js
   export default gulp.series(html, css);
   ```

Qui acquisisce il suo file `style.css`, esegue csslint su di esso (che stampa un elenco di eventuali errori nel suo CSS sul terminale), quindi lo esegue tramite autoprefixer per aggiungere eventuali prefissi necessari per far funzionare le funzionalità CSS emergenti nei browser più vecchi. Alla fine della catena delle pipe, si produce il nostro CSS modificato con i prefissi nella directory `build`. Si noti che questo funziona solo se csslint non trova errori — provi a rimuovere una parentesi graffa dal suo file CSS e a rieseguire gulp per vedere quale output ottiene!

#### js-hint e babel

1. Installi usando le seguenti righe:

   ```bash
   npm install --save-dev gulp-babel @babel/preset-env
   npm install --save-dev @babel/core
   npm install jshint gulp-jshint --save-dev
   ```

2. Aggiunga le seguenti dipendenze a `gulpfile.mjs`:

   ```js
   import babel from "gulp-babel";
   import jshint from "gulp-jshint";
   ```

3. Aggiunga il seguente test alla fine di `gulpfile.mjs`:

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

4. Cambi l'attività predefinita in:

   ```js
   export default gulp.series(html, css, js);
   ```

Qui acquisisce il suo file `main.js`, esegue `jshint` su di esso e produce i risultati sul terminale utilizzando `jshint.reporter`; quindi passa il file a babel, che lo converte in sintassi vecchio stile e produce il risultato nella directory `build`. Il nostro codice originale includeva una [funzione a freccia grassa](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions), che babel ha modificato in una funzione vecchio stile.

#### Ulteriori idee

Una volta che tutto è impostato, può eseguire il comando `gulp` all'interno della sua directory di progetto, e dovrebbe ottenere un output simile a questo:

![Output in un editor di codice dove le righe mostrano il tempo di inizio o fine delle attività, il nome dell'attività e la durata delle attività 'Terminate'.](gulp-output.png)

Può quindi provare i file prodotti dalle attività automatizzate esaminandoli all'interno della directory `build`, e caricando `build/index.html` nel suo browser web.

Se riceve errori, controlli di aver aggiunto tutte le dipendenze e i test come sopra mostrato; provi anche a commentare le sezioni del codice HTML/CSS/JavaScript e quindi a rieseguire gulp per vedere se può isolare il problema.

Gulp viene fornito con una funzione `watch()` che può utilizzare per monitorare i suoi file ed eseguire i test ogni volta che salva un file. Ad esempio, provi ad aggiungere quanto segue alla fine del suo `gulpfile.mjs`:

```js
export function watch() {
  gulp.watch("src/*.html", html);
  gulp.watch("src/*.css", css);
  gulp.watch("src/*.js", js);
}
```

Ora provi a inserire il comando `gulp watch` nel suo terminale. Gulp ora monitorerà la sua directory, ed eseguirà le attività appropriate ogni volta che salva una modifica a un file HTML, CSS o JavaScript.

> [!NOTE]
> Il carattere `*` è un carattere jolly — qui stiamo dicendo "esegui queste attività quando viene salvato qualsiasi file di questi tipi". Potrebbe utilizzare i caratteri jolly anche nelle sue attività principali, ad esempio `gulp.src('src/*.css')` acquisirebbe tutti i suoi file CSS e quindi eseguirebbe le attività piping su di essi.

C'è molto di più che può fare con Gulp. La [directory dei plugin di Gulp](https://gulpjs.com/plugins/) ha letteralmente migliaia di plugin da esaminare.

### Altri task runner

Ci sono molti altri task runner disponibili. Non stiamo certamente cercando di dire che Gulp è la soluzione migliore, ma funziona per noi ed è abbastanza accessibile per i principianti. Potrebbe anche provare altre soluzioni:

- Grunt funziona in modo molto simile a Gulp, tranne per il fatto che si basa su attività specificate in un file di configurazione, piuttosto che utilizzando JavaScript scritto. Veda [Come iniziare con Grunt per ulteriori dettagli.](https://gruntjs.com/getting-started)
- Può anche eseguire attività direttamente utilizzando script npm situati all'interno del suo file `package.json`, senza bisogno di installare alcun tipo di sistema di task runner aggiuntivo. Questo funziona sul presupposto che cose come i plugin di Gulp siano sostanzialmente dei wrapper attorno agli strumenti da riga di comando. Quindi, se riesce a capire come eseguire gli strumenti utilizzando la riga di comando, può quindi eseguirli utilizzando script npm. È un po' più complicato da gestire, ma può essere gratificante per chi ha buone competenze con la riga di comando. [Perché gli script npm?](https://css-tricks.com/why-npm-scripts/) fornisce una buona introduzione con una buona quantità di ulteriori informazioni.

## Utilizzare servizi di testing commerciali per accelerare il testing del browser

Ora esaminiamo i servizi commerciali di testing di terze parti per browser e cosa possono fare per noi.

Quando utilizza questi tipi di servizi, fornisce un URL della pagina che desidera testare insieme ad informazioni, come quali browser desidera che venga testata. L'app quindi configura una nuova macchina virtuale con il sistema operativo e il browser specificati e restituisce i risultati del test sotto forma di screenshot, video, file di log, testo, ecc. Questo è molto utile e molto più conveniente che dover configurare tutte le combinazioni OS/browser da solo.

Può quindi fare un passo avanti, utilizzando un'API per accedere alla funzionalità in modo programmatico, il che significa che tali app possono essere combinate con i task runner, come il suo stesso ambiente locale di Selenium e altri, per creare test automatizzati.

> [!NOTE]
> Ci sono altri sistemi commerciali di testing dei browser disponibili ma in questo articolo ci concentreremo su BrowserStack, Sauce Labs e TestingBot. Non stiamo dicendo che questi siano necessariamente i migliori strumenti disponibili, ma sono buoni strumenti che sono semplici per i principianti per iniziare a utilizzare rapidamente.

### BrowserStack

#### Iniziare con BrowserStack

Per iniziare:

1. Crei un [account di prova su BrowserStack](https://www.browserstack.com/users/sign_up).
2. Esegua il login. Questo dovrebbe avvenire automaticamente dopo che ha verificato il suo indirizzo email.
3. Clicchi sul link _Live_ nel menu di navigazione in alto per andare su Live Manual Testing.

#### Le basi: Test manuali

La dashboard di BrowserStack Live le permette di scegliere su quale dispositivo e browser vuole provare — piattaforme a sinistra, dispositivi a destra. Selezioni un dispositivo per vedere la scelta dei browser disponibili su quel dispositivo.

![Scelte di Test](browserstack-test-choices-sized.png)

Cliccando su una di quelle icone del browser caricherà la sua scelta di piattaforma, dispositivo e browser — scelga uno ora, e lo provi.

![Dispositivi di Test](browserstack-test-device-sized.png)

Può inserire URL nella barra degli indirizzi, scorrere verso l'alto e verso il basso trascinando con il mouse e utilizzare i gesti appropriati (ad esempio, pizzicare/zoomare, due dita per scorrere) sui touchpad di dispositivi che li supportano come MacBook. Non tutte le funzionalità sono disponibili su tutti i dispositivi.

Vedrà anche un menu che le permette di controllare la sessione.

![Menu di Test](browserstack-test-menu-sized.png)

Le funzionalità disponibili variano a seconda del browser caricato, e possono includere controlli per:

- Visualizzare informazioni sul browser corrente
- Passare ad altri browser
- Testare URL locali
- Impostare il livello di zoom e attivare/disattivare l'orientamento
- Salvare e caricare segnalibri
- Catturare/annotare screenshot e riportare bug
- Accedere a DevTools nel browser
- Cambiare la posizione riportata
- Limitare la rete
- Accedere ai lettori di schermo

#### Avanzato: L'API di BrowserStack

BrowserStack ha anche un'API [restful](https://www.browserstack.com/docs/automate/api-reference/selenium/introduction) che le permette di recuperare in modo programmatico i dettagli del suo piano account, sessioni, build, ecc.

Diamo un'occhiata veloce a come accederemmo all'API utilizzando Node.js.

1. Prima, crei un nuovo progetto npm per provare questo, come dettagliato in [Configurazione di Node e npm](#configurare_node_e_npm). Usa un nome di directory diverso rispetto a prima, come `bstack-test` ad esempio.
2. Crei un nuovo file all'interno della directory principale del suo progetto chiamato `call_bstack.js` e gli dia il seguente contenuto:

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

3. Sostituisca i placeholder per il nome utente e la chiave di accesso di BrowserStack con i suoi effettivi valori. Questi possono essere recuperati dai suoi [Dettagli dell'Account & Profilo di BrowserStack](https://www.browserstack.com/accounts/profile/details), sotto la sezione _Authentication & Security_.
4. Installi il modulo [axios](https://www.npmjs.com/package/axios) che stiamo usando nel codice per gestire l'invio delle richieste HTTP eseguendo il seguente comando nel suo terminale (abbiamo scelto axios perché è semplice, popolare e ben supportato):

   ```bash
   npm install axios
   ```

5. Si assicuri che il suo file JavaScript sia salvato e lo esegua eseguendo il seguente comando nel suo terminale. Dovrebbe vedere un oggetto stampato sul terminale contenente i dettagli del suo piano BrowserStack.

   ```bash
   node call_bstack
   ```

Sotto abbiamo anche fornito alcune altre funzioni predefinite che potrebbe trovare utili quando lavora con l'API restful di BrowserStack.

Questa funzione restituisce dettagli sommari di tutte le build automatizzate create in precedenza (vedere il prossimo articolo per i [dettagli dei test automatizzati BrowserStack](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#browserstack)):

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

Questa funzione restituisce i dettagli sulle sessioni specifiche per una particolare build:

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

La seguente funzione restituisce i dettagli di una sessione particolare:

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

Nel prossimo articolo tratteremo [l'esecuzione di test automatizzati BrowserStack](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#browserstack).

### Sauce Labs

#### Iniziare con Sauce Labs

Iniziamo con un account di prova su Sauce Labs.

1. Crei un account di prova su Sauce Labs.
2. Esegua il login. Questo dovrebbe avvenire automaticamente dopo che ha verificato il suo indirizzo email.

#### Le basi: Test manuali

La [dashboard di Sauce Labs](https://app.saucelabs.com/dashboard/manual) ha molte opzioni disponibili. Per ora, assicuriamoci di essere sulla scheda _Manual Tests_.

1. Faccia clic su _Start a new manual session_.
2. Nella schermata successiva, inserisca l'URL di una pagina che desidera testare (utilizzi ad esempio <https://mdn.github.io/learning-area/javascript/building-blocks/events/show-video-box-fixed.html>), quindi scelga una combinazione di browser/OS che desidera testare utilizzando i diversi pulsanti e liste. C'è molta scelta, come vedrà!!![selezionare sessione manuale sauce](sauce-manual-session.png)
3. Quando clicca su Start session, apparirà una schermata di caricamento, che avvia una macchina virtuale che esegue la combinazione scelta.
4. Quando il caricamento è terminato, può quindi iniziare a testare in remoto il sito web in esecuzione nel browser scelto.![Test sauce in esecuzione](sauce-test-running.png)
5. Da qui può vedere il layout come apparirebbe nel browser che sta testando, muovere il mouse e provare a cliccare i pulsanti, ecc. Il menu superiore le permette di:

   - Fermare la sessione
   - Dare a qualcun altro un URL in modo che possa osservare il test da remoto.
   - Copiare testo/note a un appunto remoto.
   - Scattare uno screenshot.
   - Testare in modalità a schermo intero.

Una volta terminata la sessione, tornerà alla scheda Manual Tests, dove vedrà una voce per ciascuna delle sessioni manuali precedenti avviate. Cliccando su una di queste voci si visualizzano più dati sulla sessione. Qui può scaricare eventuali screenshot che ha fatto, guardare un video della sessione, visualizzare i log dei dati e altro ancora.

> [!NOTE]
> Questo è già molto utile e molto più comodo che dover configurare tutti questi emulatori e macchine virtuali da solo.

#### Avanzato: L'API di Sauce Labs

Sauce Labs ha un'API [restful](https://docs.saucelabs.com/dev/api/) che le permette di recuperare in modo programmatico i dettagli del suo account e dei test esistenti, e annotare i test con ulteriori dettagli, come il loro stato di successo/fallimento che non è registrabile tramite il solo testing manuale. Ad esempio, potrebbe voler eseguire uno dei suoi test Selenium in remoto utilizzando Sauce Labs per testare una certa combinazione browser/OS, e quindi passare i risultati del test a Sauce Labs.

Dispone di diversi clienti che le permettono di effettuare chiamate all'API utilizzando il suo ambiente preferito, sia esso PHP, Java, Node.js, ecc.

Diamo un'occhiata veloce a come accederemmo all'API utilizzando Node.js e [node-saucelabs](https://github.com/saucelabs/node-saucelabs).

1. Prima di tutto, configuri un nuovo progetto npm per provare questo, come dettagliato in [Configurazione di Node e npm](#configurare_node_e_npm). Usi un nome di directory diverso rispetto a prima, come `sauce-test` per esempio.
2. Installi il wrapper Node Sauce Labs usando il seguente comando:

   ```bash
   npm install saucelabs
   ```

3. Crei un nuovo file all'interno della directory principale del suo progetto chiamato `call_sauce.js`. Gli dia il seguente contenuto:

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

4. Dovrà compilare il suo nome utente e la sua chiave API di Sauce Labs nei punti indicati. Questi possono essere recuperati dalla sua pagina [User Settings](https://app.saucelabs.com/user-settings). Li compili ora.
5. Si assicuri che tutto sia salvato ed esegua il suo file in questo modo:

   ```bash
   node call_sauce
   ```

#### Avanzato: Test automatizzati

Tratteremo l'esecuzione effettiva dei test automatizzati Sauce Lab nel prossimo articolo.

### TestingBot

#### Iniziare con TestingBot

Iniziamo con un account di prova su TestingBot.

1. Crei un [account di prova su TestingBot](https://testingbot.com/users/sign_up).
2. Esegua il login. Questo dovrebbe avvenire automaticamente dopo che ha verificato il suo indirizzo email.

#### Le basi: Test manuali

La [dashboard di TestingBot](https://testingbot.com/members) elenca le varie opzioni che può scegliere. Per ora, assicuriamoci di essere sulla scheda _Live Web Testing_.

1. Inserisca l'URL della pagina che desidera testare.
2. Scelga la combinazione browser/OS che desidera testare selezionando la combinazione nella griglia.
   ![Scelte di Test](screen_shot_2019-04-19_at_14.55.33.png)
3. Quando clicca su _Start Browser_, apparirà una schermata di caricamento, che avvia una macchina virtuale che esegue la combinazione scelta.
4. Una volta completato il caricamento, può quindi iniziare a testare in remoto il sito web in esecuzione nel browser scelto.
5. Da qui può vedere il layout come apparirebbe nel browser che sta testando, muovere il mouse e provare a cliccare i pulsanti, ecc. Il menu laterale le permette di:

   - Fermare la sessione
   - Cambiare la risoluzione dello schermo
   - Copiare testo/note a un appunto remoto
   - Fare, modificare e scaricare screenshot
   - Testare in modalità a schermo intero.

Una volta terminata la sessione, tornerà alla pagina _Live Web Testing_, dove vedrà una voce per ciascuna delle sessioni manuali precedenti avviate. Cliccando su una di queste voci si visualizzano più dati sulla sessione. Qui può scaricare eventuali screenshot che ha scattato, guardare un video del test e visualizzare i log della sessione.

#### Avanzato: L'API di TestingBot

TestingBot ha un'API [restful](https://testingbot.com/support/api) che le permette di recuperare in modo programmatico i dettagli del suo account e dei test esistenti, e annotare i test con ulteriori dettagli, come il loro stato di successo/fallimento che non è registrabile tramite il solo testing manuale.

TestingBot ha diversi clienti API che può usare per interagire con l'API, tra cui clienti per NodeJS, Python, Ruby, Java e PHP.

Sotto è riportato un esempio su come interagire con l'API di TestingBot con il client NodeJS [testingbot-api](https://www.npmjs.com/package/testingbot-api).

1. Prima, configuri un nuovo progetto npm per provare questo, come dettagliato in [Configurazione di Node e npm](#configurare_node_e_npm). Usare un nome di directory diverso rispetto a prima, come `tb-test` per esempio.
2. Installi il wrapper Node TestingBot usando il seguente comando:

   ```bash
   npm install testingbot-api
   ```

3. Crei un nuovo file all'interno della directory principale del suo progetto chiamato `tb.js`. Gli dia il seguente contenuto:

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

4. Dovrà compilare la sua TestingBot Key e Secret nei punti indicati. Può trovarli nella [dashboard di TestingBot](https://testingbot.com/members/user/edit).
5. Assicuri che tutto sia salvato ed esegua il file:

   ```bash
   node tb.js
   ```

#### Avanzato: Test automatizzati

Esamineremo l'effettiva esecuzione di test automatizzati TestingBot nel prossimo articolo.

## Riepilogo

Questo è stato piuttosto un percorso, ma sicuramente può cominciare a vedere i vantaggi dell'uso di strumenti di automazione per fare parte del lavoro pesante in termini di testing.

Nel prossimo articolo, vedremo come configurare il nostro sistema di automazione locale utilizzando Selenium, e come combinarlo con servizi come Sauce Labs, BrowserStack e TestingBot.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Feature_detection", "Learn_web_development/Extensions/Testing/Your_own_automation_environment", "Learn_web_development/Extensions/Testing")}}
