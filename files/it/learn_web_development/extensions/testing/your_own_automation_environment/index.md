---
title: Configurare il proprio ambiente di automazione dei test
short-title: Configurazione dell'ambiente di automazione
slug: Learn_web_development/Extensions/Testing/Your_own_automation_environment
l10n:
  sourceCommit: 6030ef1aadf967b80e2c79c3d3463cccc8ea0c95
---

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}

In questo articolo verrà illustrato come installare il proprio ambiente di automazione ed eseguire test usando Selenium/WebDriver e una libreria di test come selenium-webdriver per Node. Verrà inoltre esaminato come integrare l'ambiente di test locale con strumenti commerciali come quelli discussi nell'articolo precedente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; conoscenza
        dei principi di alto livello dei
        <a href="/it/docs/Learn_web_development/Extensions/Testing/Introduction">test cross-browser</a> e dei
        <a href="/it/docs/Learn_web_development/Extensions/Testing/Automated_testing">test automatizzati</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Mostrare come configurare localmente un ambiente di test Selenium ed eseguire test con esso, e come integrarlo con strumenti quali Sauce Labs e BrowserStack.
      </td>
    </tr>
  </tbody>
</table>

## Selenium

[Selenium](https://www.selenium.dev/) è lo strumento di automazione del browser più diffuso. Esistono altri modi, ma il modo migliore per usare Selenium è tramite WebDriver, una potente API che si basa su Selenium ed effettua chiamate a un browser per automatizzarlo, eseguendo azioni quali "apri questa pagina web", "spostati su questo elemento della pagina", "fai clic su questo collegamento", "verifica se il collegamento apre questo URL" e così via. È ideale per eseguire test automatizzati.

Il modo in cui si installa e si usa WebDriver dipende dall'ambiente di programmazione che si desidera usare per scrivere ed eseguire i test. Gli ambienti più diffusi dispongono di un pacchetto o framework che installerà WebDriver e i binding necessari per comunicare con WebDriver usando tale linguaggio, ad esempio Java, C#, Ruby, Python, JavaScript (Node) e così via. Per maggiori dettagli sulle configurazioni Selenium per linguaggi diversi, vedere [Configurare un progetto Selenium-WebDriver](https://www.selenium.dev/documentation/webdriver/getting_started/).

Browser diversi richiedono driver diversi per consentire a WebDriver di comunicare con essi e controllarli. Per ulteriori informazioni su dove ottenere i driver del browser e così via, vedere [Piattaforme supportate da Selenium](https://www.selenium.dev/downloads/).

Verranno trattati la scrittura e l'esecuzione di test Selenium usando Node.js, poiché è rapido e semplice da iniziare a usare, nonché un ambiente più familiare per gli sviluppatori front-end.

> [!NOTE]
> Per scoprire come usare WebDriver con altri ambienti server-side, consultare anche [Piattaforme supportate da Selenium](https://www.selenium.dev/downloads/) per alcuni collegamenti utili.

### Configurare Selenium in Node

1. Per iniziare, configurare un nuovo progetto npm, come illustrato in [Configurare Node e npm](/it/docs/Learn_web_development/Extensions/Testing/Automated_testing#setting_up_node_and_npm) nel capitolo precedente. Assegnargli un nome diverso, come `selenium-test`.
2. Successivamente, è necessario installare un framework che consenta di lavorare con Selenium all'interno di Node. Verrà scelto il framework ufficiale di Selenium [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver), poiché la documentazione sembra abbastanza aggiornata ed è ben mantenuto. Per opzioni diverse, anche [webdriver.io](https://webdriver.io/) e [nightwatch.js](https://nightwatchjs.org/) sono buone scelte. Per installare selenium-webdriver, eseguire il comando seguente, assicurandosi di trovarsi nella cartella del progetto:

   ```bash
   npm install selenium-webdriver
   ```

> [!NOTE]
> È comunque consigliabile seguire questi passaggi anche se selenium-webdriver e i driver del browser sono stati installati in precedenza. È necessario assicurarsi che tutto sia aggiornato.

Successivamente, è necessario scaricare i driver pertinenti per consentire a WebDriver di controllare i browser sui quali eseguire i test. I dettagli su dove ottenerli sono disponibili nella pagina di [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver) (vedere la tabella nella prima sezione). Ovviamente, alcuni browser sono specifici del sistema operativo, ma verranno usati Firefox e Chrome, poiché sono disponibili su tutti i principali sistemi operativi.

1. Scaricare le versioni più recenti dei driver [GeckoDriver](https://github.com/mozilla/geckodriver/releases/) (per Firefox) e [ChromeDriver](https://googlechromelabs.github.io/chrome-for-testing/#stable).
2. Estrarli in una posizione facilmente raggiungibile, ad esempio la radice della directory utente home.
3. Aggiungere la posizione dei driver `chromedriver` e `geckodriver` alla variabile di sistema `PATH`. Deve essere un percorso assoluto dalla radice del disco rigido alla directory che contiene i driver. Ad esempio, su una macchina macOS, con nome utente bob e driver collocati nella radice della cartella home, il percorso sarebbe `/Users/bob`.

> [!NOTE]
> Per ribadirlo, il percorso aggiunto a `PATH` deve essere il percorso della directory che contiene i driver, non i percorsi dei driver stessi. Questo è un errore comune.

Per impostare la variabile `PATH` su un sistema macOS e sulla maggior parte dei sistemi Linux:

1. Aprire il file `.zprofile` (oppure `.bash_profile` se il sistema usa la shell `bash`).
   > [!NOTE]
   > Se non sono visibili i file nascosti, sarà necessario visualizzarli; vedere [Mostrare/nascondere rapidamente i file nascosti in macOS](https://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/) oppure [Mostrare le cartelle nascoste in Ubuntu](https://askubuntu.com/questions/470837/how-to-show-hidden-folders-in-file-manager-nautilus-on-ubuntu)).
2. Incollare quanto segue in fondo al file, aggiornando il percorso in base a quello effettivo sulla macchina:

   ```bash
   # Add WebDriver browser drivers to PATH
   export PATH=$PATH:/Users/bob
   ```

3. Salvare e chiudere il file, quindi riavviare il Terminale/prompt dei comandi per riapplicare la configurazione Bash.
4. Verificare che i nuovi percorsi siano nella variabile `PATH` inserendo quanto segue nel terminale:

   ```bash
   echo $PATH
   ```

   Il valore dovrebbe essere stampato nel terminale.

> [!NOTE]
> Per impostare la variabile `PATH` su Windows, seguire le istruzioni in [Come posso aggiungere una nuova cartella al percorso di sistema?](https://stackoverflow.com/questions/44272416/add-a-folder-to-the-path-environment-variable-in-windows-10-with-screenshots)

Proviamo un test rapido per verificare che tutto funzioni.

1. Creare un nuovo file nella directory del progetto chiamato `duck_test.js`:
2. Assegnargli il seguente contenuto, quindi salvarlo:

   ```js
   const { Builder, Browser, By, Key, until } = require("selenium-webdriver");

   (async function example() {
     const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
     try {
       await driver.get("https://duckduckgo.com/");
       await driver.findElement(By.name("q")).sendKeys("webdriver", Key.RETURN);
       await driver.wait(until.titleIs("webdriver at DuckDuckGo"), 1000);
       console.log("Test passed!");
     } catch (e) {
       console.log(`Error: ${e}`);
     } finally {
       await driver.sleep(2000); // Delay long enough to see search page!
       await driver.quit();
     }
   })();
   ```

   > [!NOTE]
   > Questa funzione è una {{Glossary("IIFE", "IIFE")}} (Immediately Invoked Function Expression).

3. Nel terminale, assicurarsi di trovarsi nella cartella del progetto, quindi inserire il comando seguente:

   ```bash
   node duck_test
   ```

Dovrebbe aprirsi automaticamente un'istanza di Firefox. DuckDuckGo verrà caricato automaticamente in una scheda, verrà inserito "webdriver" nella casella di ricerca e verrà fatto clic sul pulsante di ricerca. WebDriver attenderà quindi 1 secondo; verrà quindi letto il titolo del documento e, se è "webdriver at DuckDuckGo", verrà restituito un messaggio che indica che il test è superato.

Vengono quindi attesi 2 secondi, dopo i quali WebDriver chiuderà l'istanza di Firefox e si fermerà.

## Eseguire test in più browser contemporaneamente

Nulla impedisce inoltre di eseguire il test su più browser simultaneamente. Proviamo.

1. Creare un altro file nella directory del progetto chiamato `duck_test_multiple.js`. È possibile modificare i riferimenti ad alcuni degli altri browser aggiunti, rimuoverli e così via, in base ai browser disponibili per i test nel sistema operativo. Sarà necessario assicurarsi di avere configurato nel sistema i driver del browser corretti. Per sapere quale stringa usare nel metodo `.forBrowser()` per altri browser, vedere la pagina di riferimento [Browser enum](https://www.selenium.dev/selenium/docs/api/javascript/global.html#Browser).
2. Assegnare al file il seguente contenuto, quindi salvarlo:

   ```js
   const { Builder, Browser, By, Key } = require("selenium-webdriver");

   const driverFx = new Builder().forBrowser(Browser.FIREFOX).build();
   const driverChr = new Builder().forBrowser(Browser.CHROME).build();

   async function searchTest(driver) {
     try {
       await driver.get("https://duckduckgo.com/");
       await driver.findElement(By.name("q")).sendKeys("webdriver", Key.RETURN);
       await driver.sleep(2000);
       const title = await driver.getTitle();
       if (title === "webdriver at DuckDuckGo") {
         console.log("Test passed");
       } else {
         console.log("Test failed");
       }
     } finally {
       driver.quit();
     }
   }

   searchTest(driverFx);
   searchTest(driverChr);
   ```

3. Nel terminale, assicurarsi di trovarsi nella cartella del progetto, quindi inserire il comando seguente:

   ```bash
   node duck_test_multiple
   ```

> [!NOTE]
> Se si usa un Mac e si decide di testare Safari, potrebbe essere visualizzato un messaggio di errore simile a "Could not create a session: You must enable the 'Allow Remote Automation' option in Safari's Develop menu to control Safari via WebDriver." In questo caso, seguire l'istruzione indicata e riprovare.
>
> Potrebbe essere visualizzato un messaggio che indica l'impossibilità di aprire un'app driver perché non è stata scaricata da una fonte verificata. In questo caso, è possibile ignorare tale impostazione di sicurezza solo per quell'app driver. Ad esempio, su Mac, fare <kbd>Ctrl</kbd> + clic sull'app, scegliere _Apri_ e scegliere nuovamente _Apri_ nella finestra di dialogo risultante.

Qui il test è stato eseguito come prima, tranne che questa volta è stato racchiuso all'interno di una funzione, `searchTest()`. Sono state create nuove istanze del browser per più browser, quindi ciascuna è stata passata alla funzione affinché il test venga eseguito su tutti.

Passiamo ora a esaminare più in dettaglio le basi della sintassi di WebDriver.

## Corso rapido sulla sintassi di WebDriver

Diamo un'occhiata ad alcune caratteristiche fondamentali della sintassi di webdriver. Per dettagli più completi, consultare il [riferimento API JavaScript di selenium-webdriver](https://www.selenium.dev/selenium/docs/api/javascript/) e la documentazione principale di Selenium, [Selenium WebDriver](https://www.selenium.dev/documentation/webdriver/), che contiene numerosi esempi da cui imparare scritti in linguaggi diversi.

### Avviare un nuovo test

Per avviare un nuovo test, è necessario includere il modulo `selenium-webdriver`, importando il costruttore `Builder` e l'interfaccia `Browser`:

```js
const { Builder, Browser } = require("selenium-webdriver");
```

Il costruttore `Builder()` viene usato per creare una nuova istanza di un driver, concatenando il metodo `forBrowser()` per specificare il browser sul quale si desidera eseguire il test con questo builder.
Il metodo `build()` viene concatenato alla fine per costruire effettivamente l'istanza del driver (per informazioni dettagliate su queste funzionalità, vedere il [riferimento della classe Builder](https://www.selenium.dev/selenium/docs/api/javascript/Builder.html)).

```js
let driver = new Builder().forBrowser(Browser.FIREFOX).build();
```

Si noti che è possibile impostare opzioni di configurazione specifiche per i browser da testare; ad esempio, è possibile impostare una versione e un sistema operativo specifici da testare nel metodo `forBrowser()`:

```js
let driver = new Builder().forBrowser(Browser.FIREFOX, "130", "MAC").build();
```

Queste opzioni possono anche essere impostate usando una variabile d'ambiente, ad esempio:

```bash
SELENIUM_BROWSER=firefox:130:MAC
```

Creiamo un nuovo test per esplorare questo codice mentre ne parliamo. Nella directory del progetto di test Selenium, creare un nuovo file chiamato `quick_test.js` e aggiungervi il codice seguente:

```js
const { Builder, Browser } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
})();
```

È possibile testare l'esempio inserendo il comando seguente nel terminale:

```bash
node quick_test
```

### Ottenere il documento da testare

Per caricare la pagina che si desidera effettivamente testare, usare il metodo `get()` dell'istanza del driver creata in precedenza, ad esempio:

```js
driver.get("http://www.google.com");
```

> [!NOTE]
> Per i dettagli sulle funzionalità in questa sezione e in quelle seguenti, vedere il [riferimento della classe WebDriver](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html).

È possibile usare qualsiasi URL per puntare alla risorsa, incluso un URL `file://` per testare un documento locale:

```js
driver.get("file:///Users/bob/git/examples/test_file.html");
```

oppure

```js
driver.get("http://localhost:8888/test_file.html");
```

Tuttavia, è preferibile usare una posizione su un server remoto affinché il codice sia più flessibile: quando si inizierà a usare un server remoto per eseguire i test (come illustrato più avanti), il codice non funzionerà se si tenta di usare percorsi locali.

Aggiornare la funzione `example()` come segue, sostituendo il percorso segnaposto con un percorso locale reale a un file HTML sul computer, quindi provare a eseguirla:

```js
const { Builder, Browser } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get("file:///Users/bob/git/examples/test_file.html");
})();
```

### Interagire con il documento

Ora che è disponibile un documento da testare, è necessario interagirvi in qualche modo, operazione che solitamente implica dapprima la selezione di un elemento specifico su cui testare qualcosa. È possibile [selezionare elementi dell'interfaccia utente in molti modi](https://www.selenium.dev/documentation/webdriver/elements/) in WebDriver, anche tramite ID, classe, nome dell'elemento e così via. La selezione effettiva viene eseguita dal metodo `findElement()`, che accetta come parametro un metodo di selezione. Ad esempio, per selezionare un elemento tramite ID:

```js
const element = driver.findElement(By.id("myElementId"));
```

Uno dei modi più utili per trovare un elemento è tramite CSS: il metodo `By.css()` consente di selezionare un elemento usando un selettore CSS.

Aggiornare ora la funzione `example()` come segue, quindi eseguire l'esempio:

```js
const { Builder, Browser, By } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get(
    "https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html",
  );
  const button = driver.findElement(By.css("button:nth-of-type(1)"));
})();
```

### Testare l'elemento

Esistono molti modi per interagire con documenti web e gli elementi al loro interno. È possibile vedere esempi comuni utili a partire da [Ottenere valori di testo](https://www.selenium.dev/documentation/webdriver/elements/information/#text-content) nella documentazione WebDriver.

Per ottenere il testo all'interno del pulsante, è possibile fare quanto segue:

```js
button.getText().then((text) => {
  console.log(`Button text is '${text}'`);
});
```

Aggiungere ora questo codice in fondo alla funzione `example()`, come mostrato di seguito:

```js
const { Builder, Browser, By } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();

  driver.get(
    "https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html",
  );

  const button = driver.findElement(By.css("button:nth-of-type(1)"));

  button.getText().then((text) => {
    console.log(`Button text is '${text}'`);
  });
})();
```

Eseguire l'esempio con `node` nello stesso modo usato in precedenza. L'etichetta di testo del pulsante dovrebbe essere riportata nella console.

Facciamo qualcosa di più utile. Sostituire la precedente istruzione di codice con `button.click();`, come mostrato di seguito:

```js
const { Builder, Browser, By } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get(
    "https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html",
  );

  const button = driver.findElement(By.css("button:nth-of-type(1)"));

  button.click();
})();
```

Provare a eseguire nuovamente il test; verrà fatto clic sul pulsante e dovrebbe comparire una finestra popup `alert()`. Almeno è possibile verificare che il pulsante funzioni.

È possibile interagire anche con il popup. Aggiornare la funzione `example()` come segue e provare a testarla nuovamente:

```js
const { Builder, Browser, By, until } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();

  driver.get(
    "https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html",
  );

  const button = driver.findElement(By.css("button:nth-of-type(1)"));

  button.click();

  await driver.wait(until.alertIsPresent());

  const alert = driver.switchTo().alert();

  alert.getText().then((text) => {
    console.log(`Alert text is '${text}'`);
  });

  alert.accept();
})();
```

Proviamo ora a inserire del testo negli elementi del modulo. Aggiornare la funzione `example()` come segue e provare a eseguire nuovamente il test:

```js
const { Builder, Browser, By, Key } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get(
    "https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html",
  );

  const input = driver.findElement(By.id("name"));
  input.sendKeys("Bob Smith");

  input.sendKeys(Key.TAB);

  const input2 = driver.findElement(By.id("age"));
  input2.sendKeys("65");
})();
```

È possibile inviare pressioni di tasti che non possono essere rappresentate da caratteri normali usando le proprietà dell'oggetto `Key`. Ad esempio, in precedenza è stato usato quanto segue per passare da un input del modulo all'altro:

```js
input.sendKeys(Key.TAB);
```

### Attendere il completamento di un'operazione

In alcuni casi sarà necessario fare in modo che WebDriver attenda il completamento di un'operazione prima di proseguire. Ad esempio, se viene caricata una nuova pagina, sarà necessario attendere che il DOM della pagina termini il caricamento prima di tentare di interagire con uno qualsiasi dei suoi elementi; in caso contrario, il test probabilmente fallirà.

Ad esempio, nel test `duck_test_multiple.js` è stata inclusa questa riga:

```js
await driver.sleep(2000);
```

Il metodo `sleep()` accetta un valore che specifica il tempo di attesa in millisecondi: il metodo restituisce una {{jsxref("Promise")}} che viene risolta al termine di tale intervallo. La parola chiave `await` viene usata per mettere in pausa la funzione che la racchiude fino alla risoluzione della promise, dopo la quale viene eseguito il codice successivo al metodo.

È possibile aggiungere un metodo `sleep()` anche al test `quick_test.js`: provare ad aggiornare la funzione `example()` in questo modo:

```js
const { Builder, Browser, By, Key } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get(
    "https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html",
  );

  const input = driver.findElement(By.id("name"));
  input.sendKeys("Bob Smith");

  driver.sleep(1000).then(() => {
    input.getAttribute("value").then((value) => {
      if (value !== "") {
        console.log("Form input filled out");
      } else {
        console.log("Text could not be entered");
      }
    });
  });
})();
```

Provare a eseguire il codice aggiornato. WebDriver compilerà ora il primo campo del modulo, attenderà un secondo, quindi verificherà se il suo valore è stato compilato, ossia se non è vuoto, usando `getAttribute()` per recuperare il valore dell'attributo `value`. Quindi stamperà un messaggio nella console per segnalare l'esito positivo o negativo.

> [!NOTE]
> Esiste anche un metodo chiamato [`wait()`](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html#wait), che testa ripetutamente una condizione per un certo periodo di tempo, quindi prosegue l'esecuzione del codice. Usa inoltre la [libreria util](https://www.selenium.dev/selenium/docs/api/javascript/lib_until.js.html), che definisce condizioni comuni da usare insieme a `wait()`.

### Arrestare i driver dopo l'uso

Dopo aver terminato l'esecuzione di un test, è necessario arrestare tutte le istanze del driver aperte usando il metodo `driver.quit()`, per assicurarsi che non continuino a usare risorse inutilmente. Aggiornare `quick_test.js` come segue:

```js
const { Builder, Browser, By, Key } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get(
    "https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/accessibility/native-keyboard-accessibility.html",
  );

  const input = driver.findElement(By.id("name"));
  input.sendKeys("Bob Smith");

  driver.sleep(1000).then(() => {
    input
      .getAttribute("value")
      .then((value) => {
        if (value !== "") {
          console.log("Form input filled out");
        } else {
          console.log("Text could not be entered");
        }
      })
      .finally(() => {
        driver.quit();
      });
  });
})();
```

Ora, durante l'esecuzione, dovrebbe essere visibile il test in esecuzione e l'istanza del browser dovrebbe chiudersi nuovamente dopo il completamento del test.

## Buone pratiche per i test

È stato scritto molto sulle buone pratiche per la scrittura dei test. Alcune utili informazioni di base sono disponibili in [Pratiche di test](https://www.selenium.dev/documentation/test_practices/). In generale, assicurarsi che i test:

1. Usino buone strategie di localizzazione: durante l'[interazione con il documento](#interagire_con_il_documento), assicurarsi di usare locator e page object che difficilmente cambieranno. Se è presente un elemento testabile sul quale si desidera eseguire un test, assicurarsi che abbia un ID stabile o una posizione nella pagina selezionabile tramite un selettore CSS, che non cambierà semplicemente con la successiva iterazione del sito. È opportuno rendere i test il meno fragili possibile, cioè che non si interrompano semplicemente quando qualcosa cambia.
2. Scrivano test atomici: ogni test dovrebbe verificare una sola cosa, rendendo semplice tenere traccia di quale file di test verifica quale criterio. Il test `duck_test.js` esaminato in precedenza è piuttosto valido, poiché verifica una sola cosa: se il titolo di una pagina di risultati di ricerca è impostato correttamente. Si potrebbe assegnargli un nome migliore, in modo che sia più facile capire cosa fa se vengono aggiunti altri test. Forse `results_page_title_set_correctly.js` sarebbe leggermente migliore.
3. Scrivano test autonomi: ogni test dovrebbe funzionare da solo e non dipendere da altri test per funzionare.

Inoltre, è opportuno menzionare i risultati/report dei test: negli esempi precedenti sono stati riportati risultati usando semplici istruzioni `console.log()`, ma tutto ciò viene eseguito in JavaScript, quindi è possibile usare qualsiasi sistema di esecuzione e report dei test desiderato, sia esso [Mocha](https://mochajs.org/), [Chai](https://www.chaijs.com/) o un altro strumento. Analizziamo un rapido esempio:

1. Creare una copia locale dell'esempio [`mocha_test.js`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/selenium/mocha_test.js) nella directory del progetto. Collocarlo in una sottocartella chiamata `test`. Questo esempio usa una lunga catena di promise per eseguire tutti i passaggi richiesti nel test: i metodi basati su promise usati da WebDriver devono essere risolti affinché funzioni correttamente.
2. Installare l'harness di test Mocha eseguendo il comando seguente nella directory del progetto:

   ```bash
   npm install --save-dev mocha
   ```

3. Ora è possibile eseguire il test, e qualsiasi altro test inserito nella directory `test`, usando il comando seguente:

   ```bash
   npx mocha --no-timeouts
   ```

4. Includere il flag `--no-timeouts` per assicurarsi che i test non finiscano per fallire a causa del timeout arbitrario di Mocha, che è di 3 secondi.

> [!NOTE]
> [saucelabs-sample-test-frameworks](https://github.com/saucelabs-sample-test-frameworks) contiene diversi esempi utili che mostrano come configurare varie combinazioni di strumenti di test/assertion.

## Eseguire test remoti

L'esecuzione di test su server remoti non è molto più difficile dell'esecuzione locale. È sufficiente creare l'istanza del driver, ma con alcune funzionalità aggiuntive specificate, incluse le capability del browser su cui eseguire il test, l'indirizzo del server e le credenziali utente necessarie, se presenti, per accedervi.

### BrowserStack

Creiamo un esempio per mostrare come eseguire un test Selenium in remoto su [BrowserStack](https://www.browserstack.com/automate):

1. Nella directory del progetto, creare un nuovo file chiamato `bstack_duck_test.js`.
2. Assegnargli il seguente contenuto:

   ```js
   const { Builder, By, Key } = require("selenium-webdriver");

   // Input capabilities
   const capabilities = {
     "bstack:options": {
       os: "OS X",
       osVersion: "Sonoma",
       browserVersion: "17.0",
       local: "false",
       seleniumVersion: "3.14.0",
       userName: "YOUR-USER-NAME",
       accessKey: "YOUR-ACCESS-KEY",
     },
     browserName: "Safari",
   };

   const driver = new Builder()
     .usingServer("http://hub-cloud.browserstack.com/wd/hub")
     .withCapabilities(capabilities)
     .build();

   (async function bStackGoogleTest() {
     try {
       await driver.get("https://duckduckgo.com/");
       await driver.findElement(By.name("q")).sendKeys("webdriver", Key.RETURN);
       await driver.sleep(2000);
       const title = await driver.getTitle();
       if (title === "webdriver at DuckDuckGo") {
         console.log("Test passed");
       } else {
         console.log("Test failed");
       }
     } finally {
       await driver.sleep(4000); // Delay long enough to see search page!
       await driver.quit();
     }
   })();
   ```

3. Dalla pagina [Dettagli account e profilo](https://www.browserstack.com/accounts/profile/details) di BrowserStack, recuperare il nome utente e la chiave di accesso, vedere _Username and Access Keys_.
4. Sostituire i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i valori effettivi del nome utente e della chiave di accesso, assicurandosi di mantenerli protetti.
5. Eseguire il test con il comando seguente:

   ```bash
   node bstack_google_test
   ```

   Il test verrà inviato a BrowserStack e il risultato del test verrà restituito alla console. Questo mostra l'importanza di includere un qualche meccanismo di report dei risultati.

6. Se ora si torna alla [dashboard Automate di BrowserStack](https://automate.browserstack.com/dashboard/), verrà visualizzato il test nell'elenco, con dettagli tra cui una registrazione video del test e vari log dettagliati delle informazioni a esso relative:
   ![Risultati automatizzati BrowserStack](bstack_automated_results.png)

> [!NOTE]
> L'opzione di menu _Resources_ nella dashboard di automazione BrowserStack contiene molte informazioni utili sul suo utilizzo per eseguire test automatizzati. Per informazioni specifiche per Node, vedere [Selenium con NodeJS](https://www.browserstack.com/docs/automate/selenium/getting-started/nodejs).

#### Compilare programmaticamente i dettagli del test BrowserStack

È possibile usare l'API REST di BrowserStack e altre capability per annotare il test con maggiori dettagli, ad esempio se è stato superato, perché è stato superato, di quale progetto fa parte il test e così via. BrowserStack non conosce questi dettagli per impostazione predefinita.

Aggiorniamo la demo `bstack_duck_test.js` per mostrare come funzionano queste funzionalità:

1. Installare il modulo [axios](https://www.npmjs.com/package/axios) eseguendo il comando seguente nella directory del progetto:

   ```bash
   npm install axios
   ```

2. Importare il modulo axios per poterlo usare nell'invio di richieste all'API REST di BrowserStack. Aggiungere la riga seguente all'inizio del codice:

   ```js
   const axios = require("axios");
   ```

3. Ora verrà aggiornato l'oggetto `capabilities` per includere un nome di progetto: aggiungere la riga seguente prima della parentesi graffa di chiusura, ricordando di aggiungere una virgola alla fine della riga precedente. È possibile variare i nomi di build e progetto per organizzare i test in diverse finestre nella dashboard di automazione BrowserStack:

   ```js
   const capabilities = {
     // …
     project: "DuckDuckGo test 2",
   };
   ```

4. Successivamente, verrà recuperato il `sessionId` della sessione corrente e verrà usato, insieme a `userName` e `accessKey`, per assemblare l'URL a cui inviare richieste per aggiornare i dati BrowserStack. Includere le righe seguenti subito sotto il blocco che crea l'oggetto `driver`, che inizia con `const driver = new Builder()`:

   ```js
   let sessionId;
   let bstackURL;

   driver.session_.then((sessionData) => {
     sessionId = sessionData.id_;
     bstackURL = `https://${capabilities["bstack:options"].userName}:${capabilities["bstack:options"].accessKey}@www.browserstack.com/automate/sessions/${sessionId}.json`;
   });
   ```

5. Infine, aggiornare il blocco `if...else` vicino alla fine del codice per inviare chiamate API appropriate a BrowserStack a seconda che il test sia riuscito o fallito:

   ```js
   if (title === "webdriver at DuckDuckGo") {
     console.log("Test passed");
     axios.put(bstackURL, {
       status: "passed",
       reason: "DuckDuckGo results showed correct title",
     });
   } else {
     console.log("Test failed");
     axios.put(bstackURL, {
       status: "failed",
       reason: "DuckDuckGo results showed wrong title",
     });
   }
   ```

Al termine del test, viene inviata una chiamata API a BrowserStack per aggiornare il test con uno stato superato o non superato e una motivazione del risultato.

Se ora si torna alla [dashboard Automate di BrowserStack](https://automate.browserstack.com/dashboard/), la sessione di test dovrebbe essere visibile come prima, ma con i dati personalizzati associati. Mostra lo stato "PASSED" e il motivo del superamento riportato dall'API REST:

![Risultati personalizzati BrowserStack](bstack_custom_results.png)

### Sauce Labs

Vediamo un esempio che dimostra l'esecuzione remota di test Selenium su Sauce Labs:

1. Nella directory del progetto, creare un nuovo file chiamato `sauce_google_test.js`.
2. Assegnargli il seguente contenuto:

   ```js
   const { Builder, By, Key } = require("selenium-webdriver");

   const username = "YOUR-USER-NAME";
   const accessKey = "YOUR-ACCESS-KEY";

   const driver = new Builder()
     .withCapabilities({
       browserName: "chrome",
       platform: "Windows XP",
       version: "43.0",
       username,
       accessKey,
     })
     .usingServer(
       `https://${username}:${accessKey}@ondemand.saucelabs.com:443/wd/hub`,
     )
     .build();

   driver.get("http://www.google.com");

   driver.findElement(By.name("q")).sendKeys("webdriver");

   driver.sleep(1000).then(() => {
     driver.findElement(By.name("q")).sendKeys(Key.TAB);
   });

   driver.findElement(By.name("btnK")).click();

   driver.sleep(2000).then(() => {
     driver.getTitle().then((title) => {
       if (title === "webdriver - Google Search") {
         console.log("Test passed");
       } else {
         console.log("Test failed");
       }
     });
   });

   driver.quit();
   ```

3. Dalle [impostazioni utente di Sauce Labs](https://app.saucelabs.com/user-settings), recuperare il nome utente e la chiave di accesso. Sostituire i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i valori effettivi del nome utente e della chiave di accesso, assicurandosi di mantenerli protetti.
4. Eseguire il test con il comando seguente:

   ```bash
   node sauce_google_test
   ```

   Il test verrà inviato a Sauce Labs e il risultato del test verrà restituito alla console. Questo mostra l'importanza di includere un qualche meccanismo di report dei risultati.

5. Se ora si va alla pagina della [dashboard dei test automatizzati Sauce Labs](https://app.saucelabs.com/dashboard/tests), verrà visualizzato il test nell'elenco; da qui sarà possibile vedere video, screenshot e altri dati simili.
   ![Test automatizzato Sauce Labs](sauce_labs_automated_test.png)

> [!NOTE]
> Il [Platform Configurator](https://saucelabs.com/products/platform-configurator#/) di Sauce Labs è uno strumento utile per generare oggetti capability da fornire alle istanze del driver, in base al browser/sistema operativo sul quale si desidera eseguire i test.

> [!NOTE]
> Per ulteriori dettagli utili sui test con Sauce Labs e Selenium, consultare [Introduzione a Selenium per i test automatizzati dei siti web](https://docs.saucelabs.com/web-apps/automated-testing/selenium/) e [Test Selenium Node.js istantanei](https://docs.saucelabs.com/web-apps/automated-testing/selenium/sample-scripts/#nodejs).

#### Compilare programmaticamente i dettagli dei test Sauce Labs

È possibile usare l'API Sauce Labs per annotare il test con maggiori dettagli, ad esempio se è stato superato, il nome del test e così via. Sauce Labs non conosce questi dettagli per impostazione predefinita.

Per farlo, è necessario:

1. Installare il wrapper Node di Sauce Labs usando il comando seguente, se non è già stato fatto per questo progetto:

   ```bash
   npm install saucelabs --save-dev
   ```

2. Richiedere saucelabs: inserire questo all'inizio del file `sauce_google_test.js`, subito sotto le precedenti dichiarazioni di variabili:

   ```js
   const SauceLabs = require("saucelabs");
   ```

3. Creare una nuova istanza di SauceLabs aggiungendo quanto segue subito sotto:

   ```js
   const saucelabs = new SauceLabs({
     username: "YOUR-USER-NAME",
     password: "YOUR-ACCESS-KEY",
   });
   ```

   Anche in questo caso, sostituire i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i valori effettivi del nome utente e della chiave di accesso. Si noti che il pacchetto npm saucelabs usa, in modo piuttosto confuso, `password` anziché `accessKey`. Poiché ora vengono usati due volte, potrebbe essere opportuno creare un paio di variabili di supporto per memorizzarli.

4. Sotto il blocco in cui viene definita la variabile `driver`, subito sotto la riga `build()`, aggiungere il blocco seguente: questo ottiene il corretto `sessionID` del driver necessario per scrivere dati nel job. Lo si può vedere in azione nel blocco di codice successivo:

   ```js
   driver.getSession().then((sessionid) => {
     driver.sessionID = sessionid.id_;
   });
   ```

5. Infine, sostituire il blocco `driver.sleep(2000)` vicino alla fine del codice con il seguente:

   ```js
   driver.sleep(2000).then(() => {
     driver.getTitle().then((title) => {
       let testPassed = false;
       if (title === "webdriver - Google Search") {
         console.log("Test passed");
         testPassed = true;
       } else {
         console.error("Test failed");
       }

       saucelabs.updateJob(driver.sessionID, {
         name: "Google search results page title test",
         passed: testPassed,
       });
     });
   });
   ```

Qui è stata impostata una variabile `testPassed` su `true` o `false` a seconda che il test sia superato o fallisca, quindi è stato usato il metodo `saucelabs.updateJob()` per aggiornare i dettagli.

Se ora si torna alla pagina della [dashboard dei test automatizzati Sauce Labs](https://app.saucelabs.com/dashboard/tests), il nuovo job dovrebbe ora avere i dati aggiornati associati:

![Informazioni aggiornate sul job Sauce Labs](sauce_labs_updated_job_info.png)

### Il proprio server remoto

Se non si desidera usare un servizio come Sauce Labs o BrowserStack, è sempre possibile configurare il proprio server di test remoto. Vediamo come farlo.

1. Il server remoto Selenium richiede Java per essere eseguito. Scaricare l'ultimo JDK per la piattaforma dalla [pagina dei download Java SE](https://www.oracle.com/java/technologies/downloads/). Installarlo una volta scaricato.
2. Successivamente, scaricare l'ultimo [server standalone Selenium](https://selenium-release.storage.googleapis.com/index.html), che funge da proxy tra lo script e i driver del browser. Scegliere il numero di versione stabile più recente, ossia non beta, e nell'elenco scegliere un file che inizia con "selenium-server-standalone". Al termine del download, collocarlo in una posizione appropriata, ad esempio nella directory home. Se la posizione non è stata ancora aggiunta a `PATH`, farlo ora, vedere la sezione [Configurare Selenium in Node](#configurare_selenium_in_node).
3. Eseguire il server standalone inserendo quanto segue in un terminale sul computer server:

   ```bash
   java -jar selenium-server-standalone-3.0.0.jar
   ```

   Aggiornare il nome file `.jar` affinché corrisponda esattamente al file disponibile.

4. Il server verrà eseguito su `http://localhost:4444/wd/hub`: provare a visitare ora questo indirizzo per vedere cosa viene restituito.

Ora che il server è in esecuzione, creiamo un test dimostrativo che verrà eseguito sul server Selenium remoto.

1. Creare una copia del file `google_test.js` e chiamarla `google_test_remote.js`; collocarla nella directory del progetto.
2. Aggiornare la riga di codice, che inizia con `const driver = …`, come segue:

   ```js
   const driver = new Builder()
     .forBrowser(Browser.FIREFOX)
     .usingServer("http://localhost:4444/wd/hub")
     .build();
   ```

3. Eseguire il test: dovrebbe essere eseguito come previsto; questa volta, tuttavia, verrà eseguito sul server standalone:

   ```bash
   node google_test_remote.js
   ```

Questo è molto utile. Il test è stato eseguito localmente, ma questa configurazione può essere predisposta su quasi qualsiasi server insieme ai driver del browser pertinenti, per poi connettere gli script a esso usando l'URL scelto per esporlo.

## Integrare Selenium con strumenti CI

È inoltre possibile integrare Selenium e strumenti correlati come Sauce Labs con strumenti di {{Glossary("continuous_integration", "integrazione continua")}} (CI). È utile perché consente di eseguire i test tramite uno strumento CI e di effettuare il commit di nuove modifiche al repository del codice solo se i test vengono superati.

L'analisi dettagliata di quest'area non rientra nello scopo di questo articolo, ma è consigliabile iniziare con Travis CI: è probabilmente lo strumento CI più semplice da iniziare a usare e dispone di una buona integrazione con strumenti web quali GitHub e Node.

Per iniziare, vedere ad esempio:

- [Travis CI per principianti assoluti](https://docs.travis-ci.com/user/for-beginners)
- [Creare un progetto Node.js](https://docs.travis-ci.com/user/languages/javascript-with-nodejs/) (con Travis)
- [Usare Sauce Labs con Travis CI](https://docs.travis-ci.com/user/sauce-connect/)

> [!NOTE]
> Per eseguire test continui con **automazione senza codice**, è possibile usare [Endtest](https://endtest.io/) oppure [TestingBot](https://testingbot.com/).

## Riepilogo

Questo modulo dovrebbe essere stato divertente e dovrebbe aver fornito informazioni sufficienti sulla scrittura e sull'esecuzione di test automatizzati per iniziare a scrivere i propri test automatizzati.

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}
