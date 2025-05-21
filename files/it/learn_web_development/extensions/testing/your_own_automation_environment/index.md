---
title: Impostare il proprio ambiente di test automatizzato
short-title: Configurazione dell'ambiente di automazione
slug: Learn_web_development/Extensions/Testing/Your_own_automation_environment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}

In questo articolo, insegneremo come installare il proprio ambiente di automazione e eseguire i propri test utilizzando Selenium/WebDriver e una libreria di test come `selenium-webdriver` per Node. Vedremo anche come integrare il proprio ambiente di test locale con strumenti commerciali come quelli discussi nell'articolo precedente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali come <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; una comprensione dei
        <a href="/it/docs/Learn_web_development/Extensions/Testing/Introduction">principi del test cross browser</a> e del
        <a href="/it/docs/Learn_web_development/Extensions/Testing/Automated_testing">test automatizzato</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Mostrare come impostare un ambiente di test Selenium localmente ed eseguire test con esso, e come integrarlo con strumenti come LambdaTest, Sauce Labs e BrowserStack.
      </td>
    </tr>
  </tbody>
</table>

## Selenium

[Selenium](https://www.selenium.dev/) è lo strumento di automazione del browser più popolare. Esistono altri modi, ma il modo migliore per utilizzare Selenium è tramite WebDriver, una potente API che si basa su Selenium e effettua chiamate a un browser per automatizzarlo, eseguendo azioni come "apri questa pagina web", "passa sopra questo elemento sulla pagina", "clicca su questo link", "verifica se il link apre questo URL", ecc. Questo è ideale per eseguire test automatizzati.

Come installi e utilizzi WebDriver dipende dall'ambiente di programmazione che desideri utilizzare per scrivere ed eseguire i tuoi test. La maggior parte degli ambienti popolari dispone di un pacchetto o framework che installerà WebDriver e i collegamenti necessari per comunicare con WebDriver utilizzando questo linguaggio, ad esempio, Java, C#, Ruby, Python, JavaScript (Node), ecc. Consulta [Setting Up a Selenium-WebDriver Project](https://www.selenium.dev/documentation/webdriver/getting_started/) per ulteriori dettagli sulle configurazioni di Selenium per diversi linguaggi.

I diversi browser richiedono driver diversi per consentire a WebDriver di comunicare con loro e controllarli. Consulta [Platforms Supported by Selenium](https://www.selenium.dev/downloads/) per ulteriori informazioni su dove ottenere i driver dei browser, ecc.

Tratteremo la scrittura e l'esecuzione di test Selenium utilizzando Node.js, poiché è rapido e facile da avviare, ed è un ambiente più familiare per gli sviluppatori front end.

> [!NOTE]
> Se desideri scoprire come utilizzare WebDriver con altri ambienti lato server, consulta anche [Platforms Supported by Selenium](https://www.selenium.dev/downloads/) per alcuni link utili.

### Configurazione di Selenium in Node

1. Per iniziare, imposta un nuovo progetto npm, come discusso in [Setting up Node and npm](/it/docs/Learn_web_development/Extensions/Testing/Automated_testing#setting_up_node_and_npm) nel capitolo precedente. Chiamalo in modo diverso, ad esempio `selenium-test`.
2. Successivamente, dobbiamo installare un framework che ci consenta di lavorare con Selenium all'interno di Node. Sceglieremo il pacchetto ufficiale di selenium [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver), poiché la documentazione sembra abbastanza aggiornata ed è ben mantenuta. Se vuoi opzioni diverse, [webdriver.io](https://webdriver.io/) e [nightwatch.js](https://nightwatchjs.org/) sono anche buone scelte. Per installare selenium-webdriver, esegui il seguente comando, assicurandoti di essere all'interno della cartella del tuo progetto:

   ```bash
   npm install selenium-webdriver
   ```

> [!NOTE]
> È comunque una buona idea seguire questi passaggi anche se hai installato precedentemente selenium-webdriver e scaricato i driver dei browser. Dovresti assicurarti che tutto sia aggiornato.

Successivamente, devi scaricare i driver rilevanti per consentire a WebDriver di controllare i browser che vuoi testare. Puoi trovare i dettagli su dove scaricarli dalla pagina di [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver) (vedi la tabella nella prima sezione). Ovviamente, alcuni browser sono specifici del sistema operativo, ma ci concentreremo su Firefox e Chrome, poiché sono disponibili su tutti i principali sistemi operativi.

1. Scarica gli ultimi driver [GeckoDriver](https://github.com/mozilla/geckodriver/releases/) (per Firefox) e [ChromeDriver](https://googlechromelabs.github.io/chrome-for-testing/#stable).
2. Decomprimi i file in una directory facilmente raggiungibile, come la root della tua home directory utente.
3. Aggiungi il percorso dei driver `chromedriver` e `geckodriver` alla variabile `PATH` del tuo sistema. Questo dovrebbe essere un percorso assoluto dalla root del tuo disco rigido fino alla directory contenente i driver. Ad esempio, se stessimo utilizzando un computer macOS, il nostro nome utente fosse bob, e avessimo messo i nostri driver nella cartella principale della nostra home, il percorso sarebbe `/Users/bob`.

> [!NOTE]
> Solo per ribadire, il percorso che aggiungi a `PATH` deve essere il percorso alla directory contenente i driver, non i percorsi ai driver stessi! Questo è un errore comune.

Per impostare la variabile `PATH` su un sistema macOS e sulla maggior parte dei sistemi Linux:

1. Apri il tuo file `.zprofile` (o `.bash_profile` se il tuo sistema utilizza la shell `bash`).
   > [!NOTE]
   > Se non riesci a vedere i file nascosti, dovrai visualizzarli, vedi [Mostra/Nascondi file nascosti su macOS](https://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/) o [Mostra cartelle nascoste in Ubuntu](https://askubuntu.com/questions/470837/how-to-show-hidden-folders-in-file-manager-nautilus-on-ubuntu)).
2. Incolla quanto segue alla fine del tuo file (aggiornando il percorso come effettivamente presente sul tuo computer):

   ```bash
   #Add WebDriver browser drivers to PATH
   export PATH=$PATH:/Users/bob
   ```

3. Salva e chiudi questo file, quindi riavvia il tuo Terminale/prompt dei comandi per riapplicare la configurazione di Bash.
4. Verifica che i tuoi nuovi percorsi siano nella variabile `PATH` inserendo quanto segue nel tuo terminale:

   ```bash
   echo $PATH
   ```

   Dovresti vederlo stampato nel terminale.

> [!NOTE]
> Per impostare la variabile `PATH` su Windows, segui le istruzioni in [Come posso aggiungere una nuova cartella al mio percorso di sistema?](https://www.itprotoday.com/)

Proviamo un test rapido per assicurarci che tutto funzioni.

1. Crea un nuovo file all'interno della directory del tuo progetto chiamato `duck_test.js`:
2. Inserisci il seguente contenuto e poi salvalo:

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
   > Questa funzione è un {{Glossary("IIFE", "IIFE")}} (Immediately Invoked Function Expression).

3. Nel terminale, assicurati di essere all'interno della cartella del tuo progetto, quindi inserisci il seguente comando:

   ```bash
   node duck_test
   ```

Dovresti vedere un'istanza di Firefox aprirsi automaticamente! DuckDuckGo verrà caricato automaticamente in una scheda, "webdriver" verrà inserito nella casella di ricerca e il pulsante di ricerca verrà cliccato. WebDriver attenderà quindi per 1 secondo; il titolo del documento verrà quindi accesso e se è "webdriver at DuckDuckGo", verrà restituito un messaggio per indicare che il test è passato.

Attenderemo quindi 2 secondi, dopo di che WebDriver chiuderà l'istanza di Firefox e si fermerà.

## Testare in più browser contemporaneamente

Non c'è nulla che impedisca di eseguire il test su più browser contemporaneamente. Proviamo!

1. Crea un altro nuovo file all'interno della directory del tuo progetto chiamato `duck_test_multiple.js`. Puoi sentiti libero di modificare i riferimenti ad alcuni degli altri browser che abbiamo aggiunto, rimuoverli, ecc., a seconda dei browser disponibili per il test sul tuo sistema operativo. Dovrai assicurarti di avere impostato i driver del browser giusti sul tuo sistema. Per quanto riguarda quale stringa utilizzare all'interno del metodo `.forBrowser()` per altri browser, consulta la pagina di riferimento [Browser enum](https://www.selenium.dev/selenium/docs/api/javascript/global.html#Browser).
2. Dai al tuo file il seguente contenuto e poi salvalo:

   ```js
   const { Builder, Browser, By, Key } = require("selenium-webdriver");

   const driver_fx = new Builder().forBrowser(Browser.FIREFOX).build();
   const driver_chr = new Builder().forBrowser(Browser.CHROME).build();

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

   searchTest(driver_fx);
   searchTest(driver_chr);
   ```

3. Nel terminale, assicurati di essere all'interno della cartella del tuo progetto, quindi inserisci il seguente comando:

   ```bash
   node duck_test_multiple
   ```

> [!NOTE]
> Se stai usando un Mac e decidi di testare Safari, potresti ricevere un messaggio di errore del tipo "Impossibile creare una sessione: Devi abilitare l'opzione 'Consenti Automazione Remota' nel menu Sviluppo di Safari per controllare Safari tramite WebDriver." Se ricevi questo messaggio, segui l'istruzione data e riprova.
>
> Potresti ricevere un messaggio che ti dice che non puoi aprire un'app driver perché non è stata scaricata da una fonte verificata. Se ricevi questo messaggio, puoi ignorare quella impostazione di sicurezza solo per quell'app driver. Ad esempio, su Mac, <kbd>Ctrl</kbd> + clic sull'app, scegli _Apri_ e scegli nuovamente _Apri_ dalla finestra di dialogo risultante.

Così abbiamo fatto il test come prima, tranne che questa volta lo abbiamo avvolto all'interno di una funzione, `searchTest()`. Abbiamo creato nuove istanze del browser per più browser, quindi abbiamo passato ciascuno alla funzione in modo che il test sia eseguito su tutti loro.

Passiamo oltre e guardiamo le basi della sintassi di WebDriver in modo più dettagliato.

## Corso accelerato sulla sintassi di WebDriver

Diamo un'occhiata ad alcune caratteristiche chiave della sintassi del webdriver. Per dettagli più completi, puoi consultare il [Riferimento API JavaScript di selenium-webdriver](https://www.selenium.dev/selenium/docs/api/javascript/) per un riferimento dettagliato e la documentazione principale di Selenium, [Selenium WebDriver](https://www.selenium.dev/documentation/webdriver/), che contiene più esempi scritti in diversi linguaggi.

### Avvio di un nuovo test

Per avviare un nuovo test, devi includere il modulo `selenium-webdriver`, importando il costruttore `Builder` e l'interfaccia `Browser`:

```js
const { Builder, Browser } = require("selenium-webdriver");
```

Utilizzi il costruttore `Builder()` per creare una nuova istanza di un driver, concatenando il metodo `forBrowser()` per specificare quale browser vuoi testare con questo builder.
Il metodo `build()` viene concatenato alla fine per costruire effettivamente l'istanza del driver (consulta il [riferimento della classe Builder](https://www.selenium.dev/selenium/docs/api/javascript/Builder.html) per informazioni dettagliate su queste caratteristiche).

```js
let driver = new Builder().forBrowser(Browser.FIREFOX).build();
```

Nota che è possibile impostare opzioni di configurazione specifiche per i browser da testare, ad esempio puoi impostare una versione specifica e un sistema operativo da testare nel metodo `forBrowser()`:

```js
let driver = new Builder().forBrowser(Browser.FIREFOX, "130", "MAC").build();
```

Potresti anche impostare queste opzioni utilizzando una variabile d'ambiente, ad esempio:

```bash
SELENIUM_BROWSER=firefox:130:MAC
```

Creiamo un nuovo test per consentirci di esplorare questo codice mentre ne parliamo. All'interno della directory del progetto di test selenium, crea un nuovo file chiamato `quick_test.js` e aggiungi il seguente codice al suo interno:

```js
const { Builder, Browser } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
})();
```

Puoi testare l'esempio inserendo il seguente comando nel tuo terminale:

```bash
node quick_test
```

### Ottenere il documento che si desidera testare

Per caricare la pagina che si desidera effettivamente testare, si utilizza il metodo `get()` dell'istanza del driver creata in precedenza, ad esempio:

```js
driver.get("http://www.google.com");
```

> [!NOTE]
> Vedi il [riferimento della classe WebDriver](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html) per dettagli sulle funzionalità in questa sezione e quelle successive.

Puoi utilizzare qualsiasi URL per puntare alla tua risorsa, incluso un URL `file://` per testare un documento locale:

```js
driver.get("file:///Users/bob/git/examples/test_file.html");
```

oppure

```js
driver.get("http://localhost:8888/test_file.html");
```

Ma è meglio usare una posizione del server remoto in modo che il codice sia più flessibile — quando inizi a utilizzare un server remoto per eseguire i tuoi test (vedi in seguito), il tuo codice si romperà se provi a utilizzare percorsi locali.

Aggiorna la tua funzione `example()` come segue, sostituendo il percorso segnaposto con un percorso locale reale verso un file HTML sul tuo computer, quindi prova a eseguirlo:

```js
const { Builder, Browser } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get("file:///Users/bob/git/examples/test_file.html");
})();
```

### Interagire con il documento

Ora che abbiamo un documento da testare, dobbiamo interagire con esso in qualche modo, il che di solito implica prima selezionare un elemento specifico per testarne qualcosa. È possibile [selezionare elementi dell'interfaccia utente in vari modi](https://www.selenium.dev/documentation/webdriver/elements/) in WebDriver, incluso per ID, classe, nome dell'elemento, ecc. La selezione effettiva viene effettuata tramite il metodo `findElement()`, che accetta come parametro un metodo di selezione. Ad esempio, per selezionare un elemento per ID:

```js
const element = driver.findElement(By.id("myElementId"));
```

Uno dei modi più utili per trovare un elemento tramite CSS — il metodo `By.css()` ti consente di selezionare un elemento utilizzando un selettore CSS.

Aggiorna ora la tua funzione `example()` come segue e poi esegui l'esempio:

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

### Testare il tuo elemento

Ci sono molti modi per interagire con i documenti web e gli elementi al loro interno. Puoi vedere esempi comuni utili a partire da [Ottieni i valori del testo](https://www.selenium.dev/documentation/webdriver/elements/information/#text-content) nella documentazione di WebDriver.

Se volessimo ottenere il testo all'interno del nostro pulsante, potremmo farlo così:

```js
button.getText().then((text) => {
  console.log(`Button text is '${text}'`);
});
```

Aggiungi questo alla fine della funzione `example()` ora come mostrato di seguito:

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

Esegui l'esempio con `node` nello stesso modo in cui hai fatto in precedenza. Dovresti vedere l'etichetta del testo del pulsante riportata nella console.

Facciamo qualcosa di un po' più utile. Sostituisci la voce di codice precedente con `button.click();` come mostrato di seguito:

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

Prova a eseguire nuovamente il test; il pulsante verrà cliccato e apparirà un popup di `alert()`. Almeno sappiamo che il pulsante funziona!

Puoi interagire anche con il popup. Aggiorna la funzione `example()` come segue e prova a testarlo di nuovo:

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

Successivamente, proviamo a inserire del testo nei campi modulo. Aggiorna la funzione `example()` come segue e prova a eseguire nuovamente il tuo test:

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

Puoi inviare pressioni di tasti che non possono essere rappresentati da caratteri normali utilizzando le proprietà dell'oggetto `Key`. Ad esempio, sopra abbiamo utilizzato quanto segue per spostarci tra i campi del modulo:

```js
input.sendKeys(Key.TAB);
```

### Attendere il completamento di qualcosa

Ci sono momenti in cui desideri fare in modo che WebDriver attenda il completamento di qualcosa prima di continuare. Ad esempio, se carichi una nuova pagina, vorrai attendere che il DOM della pagina finisca di caricarsi prima di interagire con uno qualsiasi dei suoi elementi, altrimenti il test probabilmente fallirà.

Nel nostro test `duck_test_multiple.js`, ad esempio, abbiamo incluso questa riga:

```js
await driver.sleep(2000);
```

Il metodo `sleep()` accetta un valore che specifica il tempo di attesa in millisecondi: il metodo restituisce un {{jsxref("Promise")}} che si risolve alla fine di quel tempo. Utilizziamo la parola chiave `await` per mettere in pausa la funzione di inclusione fino a quando la promessa non si risolve, dopodiché il codice successivo al metodo viene eseguito.

Potremmo aggiungere un metodo `sleep()` anche al nostro test `quick_test.js` — prova ad aggiornare la tua funzione `example()` in questo modo:

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

Prova a eseguire il codice aggiornato. WebDriver ora compilerà il primo campo modulo, attenderà per un secondo, quindi verificherà se il suo valore è stato compilato (cioè non è vuoto) utilizzando `getAttribute()` per recuperare il suo valore di attributo `value`. Quindi stampa un messaggio nella console per riportare il successo/fallimento.

> [!NOTE]
> Esiste anche un metodo chiamato [`wait()`](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html#wait), che testa ripetutamente una condizione per una certa lunghezza di tempo, e poi continua ad eseguire il codice. Questo utilizza anche la [libreria util](https://www.selenium.dev/selenium/docs/api/javascript/lib_until.js.html), che definisce condizioni comuni da utilizzare insieme a `wait()`.

### Spegnere i driver dopo l'uso

Dopo aver terminato l'esecuzione di un test, dovresti chiudere le istanze dei driver che hai aperto utilizzando il metodo `driver.quit()`, per assicurarti che non continuino a utilizzare risorse inutilmente. Aggiorna `quick_test.js` come segue:

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

Ora, quando lo esegui, dovresti vedere l'esecuzione del test e l'istanza del browser chiudersi di nuovo dopo che il test è completato.

## Migliori pratiche di test

È stato scritto molto sulle migliori pratiche per scrivere test. Puoi trovare alcune buone informazioni di base su [Test Practices](https://www.selenium.dev/documentation/test_practices/). In generale, dovresti assicurarti che i tuoi test siano:

1. Utilizzano buone strategie di localizzazione: Quando [interagisci con il documento](#interagire_con_il_documento), assicurati di utilizzare localizzatori e oggetti della pagina che non cambieranno — se hai un elemento testabile su cui desideri eseguire un test, assicurati che abbia un ID stabile o una posizione sulla pagina che può essere selezionata utilizzando un selettore CSS, che non cambierà con la prossima iterazione del sito. Vuoi rendere i tuoi test meno fragili possibile, cioè non si romperanno quando qualcosa cambia.
2. Scrivere test atomici: Ogni test dovrebbe testare una cosa soltanto, rendendo facile tenere traccia di quale file di test sta testando quale criterio. Il test `duck_test.js` che abbiamo visto sopra è abbastanza buono, poiché testa una sola cosa: se il titolo di una pagina di risultati di ricerca è impostato correttamente. Potremmo lavorare per dargli un nome migliore in modo che sia più facile capire cosa fa se aggiungiamo altri test. Forse `results_page_title_set_correctly.js` sarebbe leggermente migliore?
3. Scrivere test autonomi: Ogni test dovrebbe funzionare da solo e non dipendere da altri test per funzionare.

Inoltre, dovremmo menzionare i risultati/reportistica dei test: abbiamo riportato i risultati nei nostri esempi sopra utilizzando semplici dichiarazioni `console.log()`, ma tutto questo è fatto in JavaScript, quindi puoi utilizzare qualsiasi sistema di esecuzione e reportistica dei test desideri, sia esso [Mocha](https://mochajs.org/), [Chai](https://www.chaijs.com/), o qualche altro strumento. Lavoriamo attraverso un esempio rapido:

1. Crea una copia locale del nostro esempio [`mocha_test.js`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/selenium/mocha_test.js) all'interno della directory del progetto. Mettilo all'interno di una sottocartella chiamata `test`. Questo esempio utilizza una lunga catena di promesse per eseguire tutti i passaggi richiesti nel nostro test: i metodi basati su promesse che WebDriver utilizza devono risolvere per funzionare correttamente.
2. Installa il framework di test mocha eseguendo il seguente comando all'interno della directory del progetto:

   ```bash
   npm install --save-dev mocha
   ```

3. Ora puoi eseguire il test (e qualsiasi altro inserito nella tua directory `test`) utilizzando il seguente comando:

   ```bash
   npx mocha --no-timeouts
   ```

4. Dovresti includere l'opzione `--no-timeouts` per assicurarti che i tuoi test non falliscano a causa del timeout arbitrario di Mocha (che è di 3 secondi).

> **Nota:** [saucelabs-sample-test-frameworks](https://github.com/saucelabs-sample-test-frameworks) contiene diversi esempi utili che mostrano come configurare diverse combinazioni di strumenti di test/asserzione.

## Eseguire test remoti

Risulta che eseguire test su server remoti non è molto più difficile che eseguirli localmente. Devi solo creare la tua istanza di driver, ma con alcune funzionalità aggiuntive specificate, inclusi le capacità del browser su cui vuoi testare, l'indirizzo del server e le credenziali utente necessarie (se presenti) per accedervi.

### BrowserStack

Creiamo un esempio per mostrare come eseguire un test Selenium da remoto su [BrowserStack](https://www.browserstack.com/automate):

1. All'interno della directory del tuo progetto, crea un nuovo file chiamato `bstack_duck_test.js`.
2. Dai al file il seguente contenuto:

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

3. Dalla pagina dei dettagli del tuo account e profilo di BrowserStack [Account & Profile details](https://www.browserstack.com/accounts/profile/details), ottieni il tuo nome utente e chiave di accesso (vedi _Username and Access Keys_).
4. Sostituisci i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i tuoi valori effettivi di nome utente e chiave di accesso (e assicurati di mantenerli sicuri).
5. Esegui il tuo test con il seguente comando:

   ```bash
   node bstack_google_test
   ```

   Il test verrà inviato a BrowserStack, e il risultato del test verrà restituito alla tua console. Questo dimostra l'importanza di includere un qualche tipo di meccanismo di reportistica dei risultati!

6. Ora, se torni alla dashboard di automazione di [BrowserStack Automate dashboard](https://automate.browserstack.com/dashboard/), vedrai il tuo test elencato, con dettagli inclusi un video della prova e molti log dettagliati di informazioni relative:
   ![Risultati automatici di BrowserStack](bstack_automated_results.png)

> [!NOTE]
> L'opzione di menu _Resources_ sulla dashboard di automazione di Browserstack contiene una vasta gamma di informazioni utili su come usarla per eseguire test automatizzati. Vedi [Selenium with NodeJS](https://www.browserstack.com/docs/automate/selenium/getting-started/nodejs) per informazioni specifiche su Node.

#### Compilare dettagli di test su BrowserStack in modo programmatico

Puoi utilizzare l'API REST di BrowserStack e alcune altre capacità per annotare il tuo test con più dettagli, come se ha superato, perché ha superato, a quale progetto appartiene il test, ecc. BrowserStack non conosce questi dettagli per impostazione predefinita.

Aggiorniamo il nostro esempio `bstack_duck_test.js`, per mostrare come funzionano queste caratteristiche:

1. Installa il modulo [axios](https://www.npmjs.com/package/axios) eseguendo il seguente comando all'interno della directory del tuo progetto:

   ```bash
   npm install axios
   ```

2. Importa il modulo axios per poterlo utilizzare per inviare richieste all'API REST di BrowserStack. Aggiungi la seguente riga proprio all'inizio del tuo codice:

   ```js
   const axios = require("axios");
   ```

3. Ora aggiorneremo il nostro oggetto `capabilities` per includere un nome di progetto — aggiungi la seguente riga prima della parentesi graffa di chiusura, ricordando di aggiungere una virgola alla fine della riga precedente (puoi variare i nomi di build e progetto per organizzare i test in finestre diverse nella dashboard di automazione di BrowserStack):

   ```js
   project: "DuckDuckGo test 2";
   ```

4. Successivamente recupereremo il `sessionId` della sessione corrente e lo useremo (insieme al tuo `userName` e `accessKey`) per assemblare l'URL a cui inviare richieste, per aggiornare i dati di BrowserStack. Includi le seguenti linee appena sotto il blocco che crea l'oggetto `driver` (che inizia con `const driver = new Builder()`) :

   ```js
   let sessionId;
   let bstackURL;

   driver.session_.then((sessionData) => {
     sessionId = sessionData.id_;
     bstackURL = `https://${capabilities["bstack:options"].userName}:${capabilities["bstack:options"].accessKey}@www.browserstack.com/automate/sessions/${sessionId}.json`;
   });
   ```

5. Infine, aggiorna il blocco `if ... else` vicino al fondo del codice per inviare chiamate API appropriate a BrowserStack a seconda se il test è passato o fallito:

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

Quando il test è completo, inviamo una chiamata API a BrowserStack per aggiornare il test con uno stato di successo o fallimento, e una motivazione per il risultato.

Se ora torni alla tua dashboard di [BrowserStack Automate](https://automate.browserstack.com/dashboard/), dovresti vedere la tua sessione di test come prima, ma con i tuoi dati personalizzati allegati. Mostra uno stato di "PASSED", e il motivo del passaggio riportato dall'API REST:

![Risultati Personalizzati di BrowserStack](bstack_custom_results.png)

### Sauce Labs

Diamo un'occhiata a un esempio che dimostra come eseguire test Selenium da remoto su Sauce Labs:

1. All'interno della directory del tuo progetto, crea un nuovo file chiamato `sauce_google_test.js`.
2. Dai al file il seguente contenuto:

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

3. Dalle tue impostazioni utente di [Sauce Labs](https://app.saucelabs.com/user-settings), ottieni il tuo nome utente e chiave di accesso. Sostituisci i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i tuoi valori effettivi di nome utente e chiave di accesso (e assicurati di tenerli sicuri).
4. Esegui il tuo test con il seguente comando:

   ```bash
   node sauce_google_test
   ```

   Il test verrà inviato a Sauce Labs, e il risultato del test verrà restituito alla tua console. Questo dimostra l'importanza di includere un qualche tipo di meccanismo di reportistica dei risultati!

5. Ora, se vai alla tua pagina della [dashboard di test automatizzati di Sauce Labs](https://app.saucelabs.com/dashboard/tests), vedrai il tuo test elencato; da qui sarai in grado di vedere video, screenshot, e altri dati simili.
   ![Test automatizzato di Sauce Labs](sauce_labs_automated_test.png)

> [!NOTE]
> Il [configuratore di piattaforma](https://saucelabs.com/products/platform-configurator#/) di Sauce Labs è uno strumento utile per generare oggetti di capacità da fornire alle tue istanze driver in base al browser/OS su cui vuoi testare.

> [!NOTE]
> Per maggiori dettagli utili su come testare con Sauce Labs e Selenium, controlla [Introduzione a Selenium per il test automatizzato dei siti web](https://docs.saucelabs.com/web-apps/automated-testing/selenium/), e [Test Selenium Node.js Istanti](https://docs.saucelabs.com/web-apps/automated-testing/selenium/sample-scripts/#nodejs).

#### Compilare i dettagli di test di Sauce Labs programmaticamente

Puoi utilizzare l'API di Sauce Labs per annotare il tuo test con più dettagli, come se ha superato, il nome del test, ecc. Sauce Labs non conosce questi dettagli per impostazione predefinita!

Per farlo, devi:

1. Installare il pacchetto Node Sauce Labs wrapper utilizzando il seguente comando (se non lo hai già fatto per questo progetto):

   ```bash
   npm install saucelabs --save-dev
   ```

2. Richiedere `saucelabs` — metti questo all'inizio del tuo file `sauce_google_test.js`, proprio sotto le dichiarazioni delle variabili precedenti:

   ```js
   const SauceLabs = require("saucelabs");
   ```

3. Creare una nuova istanza di SauceLabs, aggiungendo il seguente codice appena sotto:

   ```js
   const saucelabs = new SauceLabs({
     username: "YOUR-USER-NAME",
     password: "YOUR-ACCESS-KEY",
   });
   ```

   Anche in questo caso, sostituisci i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i tuoi valori effettivi di nome utente e chiave di accesso (nota che il pacchetto npm saucelabs utilizza piuttosto confusamente `password`, non `accessKey`). Poiché li stai utilizzando due volte ora, potresti voler creare un paio di variabili di aiuto per memorizzarli.

4. Sotto il blocco in cui definiisci la variabile `driver` (appena sotto la linea `build()`), aggiungi il seguente blocco — questo ottiene l'ID di sessione `sessionID` corretto che dobbiamo scrivere i dati al lavoro (puoi vederlo in azione nel prossimo blocco di codice):

   ```js
   driver.getSession().then((sessionid) => {
     driver.sessionID = sessionid.id_;
   });
   ```

5. Infine, sostituisci il blocco `driver.sleep(2000)` vicino al fondo del codice con quanto segue:

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

Qui abbiamo impostato una variabile `testPassed` su `true` o `false` a seconda che il test sia passato o fallito, quindi abbiamo utilizzato il metodo `saucelabs.updateJob()` per aggiornare i dettagli.

Se ora torni alla tua pagina della [dashboard di test automatizzati di Sauce Labs](https://app.saucelabs.com/dashboard/tests), dovresti vedere che il tuo nuovo lavoro ora ha i dati aggiornati allegati:

![Informazioni Aggiornate sul Lavoro di Sauce Labs](sauce_labs_updated_job_info.png)

### Il tuo server remoto

Se non vuoi utilizzare un servizio come Sauce Labs o BrowserStack, puoi sempre impostare il tuo server di test remoto. Vediamo come farlo.

1. Il server remoto Selenium richiede Java per essere eseguito. Scarica l'ultima versione del JDK per la tua piattaforma dalla pagina dei download di [Java SE](https://www.oracle.com/java/technologies/downloads/). Installalo una volta scaricato.
2. Successivamente, scarica l'ultima versione del [server standalone Selenium](https://selenium-release.storage.googleapis.com/index.html) — questo funge da proxy tra il tuo script e i driver del browser. Scegli il numero di versione stabile più recente (cioè, non una beta) e dalla lista scegli un file che inizi con "selenium-server-standalone". Una volta scaricato, mettilo in un posto sensato, come nella tua home directory. Se non hai già aggiunto la posizione al tuo `PATH`, fallo ora (vedi la sezione [Configura Selenium in Node](#configurazione_di_selenium_in_node)).
3. Esegui il server standalone inserendo il seguente comando in un terminale sul tuo computer server

   ```bash
   java -jar selenium-server-standalone-3.0.0.jar
   ```

   (aggiorna il nome del file `.jar`) in modo che corrisponda esattamente al file che hai.

4. Il server verrà eseguito su `http://localhost:4444/wd/hub` — provalo ora per vedere cosa ottieni.

Ora che abbiamo il server in esecuzione, creiamo un test demo che verrà eseguito sul server selenium remoto.

1. Crea una copia del tuo file `google_test.js`, e chiamala `google_test_remote.js`; mettila nella tua directory del progetto.
2. Aggiorna la linea di codice (che inizia con `const driver = …`) in questo modo

   ```js
   const driver = new Builder()
     .forBrowser(Browser.FIREFOX)
     .usingServer("http://localhost:4444/wd/hub")
     .build();
   ```

3. Esegui il tuo test e dovresti vedere che funziona come previsto; questa volta però lo stai eseguendo sul server standalone:

   ```bash
   node google_test_remote.js
   ```

Quindi questo è piuttosto interessante. Abbiamo testato questo localmente, ma potresti configurarlo praticamente su qualsiasi server insieme ai driver del browser rilevanti, e poi collegare i tuoi script ad esso utilizzando l'URL che scegli di esporre.

## Integrazione di Selenium con gli strumenti CI

Come altro punto, è anche possibile integrare Selenium e strumenti correlati come LambdaTest e Sauce Labs con strumenti di integrazione continua (CI) — questo è utile, poiché significa che puoi eseguire i tuoi test tramite uno strumento CI e fare il commit di nuove modifiche al tuo repository di codice solo se i test vengono superati.

È fuori portata trattare questa area nel dettaglio in questo articolo, ma suggeriremmo di iniziare con Travis CI — questo è probabilmente lo strumento CI più facile con cui iniziare e ha una buona integrazione con strumenti web come GitHub e Node.

Per iniziare, vedi ad esempio:

- [Travis CI per principianti completi](https://docs.travis-ci.com/user/for-beginners)
- [Costruire un progetto Node.js](https://docs.travis-ci.com/user/languages/javascript-with-nodejs/) (con Travis)
- [Usare LambdaTest con Travis CI](https://www.lambdatest.com/support/docs/travis-ci-with-lambdatest/)
- [Usare LambdaTest con CircleCI](https://www.lambdatest.com/support/docs/circleci-integration-with-lambdatest/)
- [Usare LambdaTest con Jenkins](https://www.lambdatest.com/support/docs/jenkins-with-lambdatest/)
- [Usare Sauce Labs con Travis CI](https://docs.travis-ci.com/user/sauce-connect/)

> [!NOTE]
> Se desideri effettuare test continui con **automazione senza codice**, puoi utilizzare [Endtest](https://www.endtest.io/) o [TestingBot](https://testingbot.com/).

## Riepilogo

Questo modulo dovrebbe essere stato divertente, e dovrebbe averti fornito abbastanza intuizioni sulla scrittura e l'esecuzione di test automatizzati da poterti mettere in moto con la scrittura dei tuoi test automatizzati.

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}
