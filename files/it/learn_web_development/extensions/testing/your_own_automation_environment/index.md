---
title: Impostare il proprio ambiente di test automatizzato
short-title: Configurazione dell'ambiente di automazione
slug: Learn_web_development/Extensions/Testing/Your_own_automation_environment
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}

In questo articolo, le insegneremo come installare il suo ambiente di automazione e eseguire i suoi test utilizzando Selenium/WebDriver e una libreria di test come `selenium-webdriver` per Node. Esamineremo anche come integrare il suo ambiente di test locale con strumenti commerciali come quelli discussi nell'articolo precedente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei principi di alto livello dei
        <a href="/it/docs/Learn_web_development/Extensions/Testing/Introduction">test cross browser</a> e dei
        <a href="/it/docs/Learn_web_development/Extensions/Testing/Automated_testing">test automatizzati</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Mostrare come configurare un ambiente di test Selenium localmente ed eseguire test con esso, e come integrarlo con strumenti come LambdaTest, Sauce Labs e BrowserStack.
      </td>
    </tr>
  </tbody>
</table>

## Selenium

[Selenium](https://www.selenium.dev/) è lo strumento di automazione del browser più popolare. Esistono altri metodi, ma il modo migliore per utilizzare Selenium è tramite WebDriver, un'API potente che si basa su Selenium e fa chiamate a un browser per automatizzarlo, eseguendo azioni come "apri questa pagina web", "sposta su questo elemento nella pagina", "clicca su questo link", "verifica se il link apre questo URL", ecc. Questo è l'ideale per eseguire test automatizzati.

Come si installa e si usa WebDriver dipende dall'ambiente di programmazione che si vuole utilizzare per scrivere ed eseguire i suoi test. La maggior parte degli ambienti popolari dispone di un pacchetto o di un framework che installerà WebDriver e i binding richiesti per comunicare con WebDriver utilizzando questo linguaggio, per esempio, Java, C#, Ruby, Python, JavaScript (Node), ecc. Vedere [Configurazione di un progetto Selenium-WebDriver](https://www.selenium.dev/documentation/webdriver/getting_started/) per maggiori dettagli sulle configurazioni di Selenium per diversi linguaggi.

Diversi browser richiedono driver differenti per permettere a WebDriver di comunicare con loro e controllarli. Vedere [Piattaforme supportate da Selenium](https://www.selenium.dev/downloads/) per ulteriori informazioni su dove ottenere i driver per i browser, ecc.

Tratteremo la scrittura e l'esecuzione di test Selenium utilizzando Node.js, poiché è rapido e facile da iniziare, e un ambiente più familiare per gli sviluppatori frontend.

> [!NOTE]
> Se vuole scoprire come utilizzare WebDriver con altri ambienti server-side, consulti anche [Piattaforme supportate da Selenium](https://www.selenium.dev/downloads/) per alcuni link utili.

### Configurare Selenium in Node

1. Inizi a configurare un nuovo progetto npm, come discusso in [Configurare Node e npm](/it/docs/Learn_web_development/Extensions/Testing/Automated_testing#setting_up_node_and_npm) nell'ultimo capitolo. Lo chiami in modo diverso, come `selenium-test`.
2. Successivamente, dobbiamo installare un framework che ci permetta di lavorare con Selenium dall'interno di Node. Sceglieremo `selenium-webdriver` ufficiale di selenium, poiché la documentazione sembra abbastanza aggiornata ed è ben mantenuta. Se vuole opzioni diverse, [webdriver.io](https://webdriver.io/) e [nightwatch.js](https://nightwatchjs.org/) sono anche buone scelte. Per installare `selenium-webdriver`, esegua il seguente comando, assicurandosi di essere all'interno della cartella del progetto:

   ```bash
   npm install selenium-webdriver
   ```

> [!NOTE]
> È ancora una buona idea seguire questi passaggi anche se in precedenza ha installato `selenium-webdriver` e scaricato i driver del browser. Deve assicurarsi che tutto sia aggiornato.

Successivamente, deve scaricare i driver pertinenti per permettere a WebDriver di controllare i browser che desidera testare. Può trovare i dettagli su dove ottenerli nella pagina di [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver) (vedere la tabella nella prima sezione). Ovviamente, alcuni dei browser sono specifici per un sistema operativo, ma noi ci concentreremo su Firefox e Chrome, poiché sono disponibili su tutti i principali sistemi operativi.

1. Scarica gli ultimi driver [GeckoDriver](https://github.com/mozilla/geckodriver/releases/) (per Firefox) e [ChromeDriver](https://googlechromelabs.github.io/chrome-for-testing/#stable).
2. Decomprimili in un luogo abbastanza facile da navigare, come la radice della sua directory home.
3. Aggiunga la posizione dei driver `chromedriver` e `geckodriver` alla variabile `PATH` del suo sistema. Questo dovrebbe essere un percorso assoluto dalla radice del suo disco rigido, alla directory contenente i driver. Ad esempio, se si utilizza un Mac, il suo nome utente è bob e ha inserito i driver nella radice della sua cartella home, il percorso sarebbe `/Users/bob`.

> [!NOTE]
> Solo per ribadire, il percorso che aggiunge a `PATH` deve essere il percorso alla directory contenente i driver, non i percorsi ai driver stessi! Questo è un errore comune.

Per impostare la variabile `PATH` su un sistema macOS e sulla maggior parte dei sistemi Linux:

1. Apra il suo file `.zprofile` (o `.bash_profile` se il suo sistema utilizza la shell `bash`).
   > [!NOTE]
   > Se non riesce a vedere i file nascosti, dovrà visualizzarli, vedere [Mostra/nascondi file nascosti in macOS](https://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/) o [Mostra cartelle nascoste in Ubuntu](https://askubuntu.com/questions/470837/how-to-show-hidden-folders-in-file-manager-nautilus-on-ubuntu)).
2. Incolli il seguente testo in fondo al suo file (aggiornando il percorso come effettivamente è sul suo computer):

   ```bash
   #Add WebDriver browser drivers to PATH
   export PATH=$PATH:/Users/bob
   ```

3. Salvi e chiuda questo file, quindi riavvii il suo Terminale/Prompt dei comandi per riapplicare la configurazione di Bash.
4. Verifichi che i nuovi percorsi siano nella variabile `PATH` inserendo il seguente comando nel terminale:

   ```bash
   echo $PATH
   ```

   Dovrebbe vederlo stampato nel terminale.

> [!NOTE]
> Per impostare la variabile `PATH` su Windows, segua le istruzioni su [Come posso aggiungere una nuova cartella al mio percorso di sistema?](https://www.itprotoday.com/)

Proviamo un rapido test per assicurarci che tutto stia funzionando.

1. Crei un nuovo file all'interno della sua directory di progetto chiamato `duck_test.js`:
2. Gli dia il seguente contenuto, quindi lo salvi:

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

3. Nel terminale, si assicuri di essere all'interno della cartella del progetto, quindi inserisca il seguente comando:

   ```bash
   node duck_test
   ```

Dovrebbe vedere un'istanza di Firefox aprirsi automaticamente! DuckDuckGo verrà automaticamente caricato in una scheda, "webdriver" verrà inserito nella casella di ricerca e verrà cliccato sul pulsante di ricerca. WebDriver attenderà quindi 1 secondo; il titolo del documento verrà poi accesso e, se è "webdriver at DuckDuckGo", verrà restituito un messaggio che indica che il test è superato.

Attenderemo quindi 2 secondi, dopodiché WebDriver chiuderà l'istanza di Firefox e si fermerà.

## Testare in più browser contemporaneamente

Non c'è neanche nulla che impedisca di eseguire il test su più browser contemporaneamente. Proviamolo!

1. Crei un altro nuovo file all'interno della sua directory di progetto chiamato `duck_test_multiple.js`. Può sentirsi libero di modificare i riferimenti ad alcuni degli altri browser che abbiamo aggiunto, rimuoverli, ecc., a seconda di quali browser ha disponibili per il test sul suo sistema operativo. Dovrà assicurarsi di avere i driver del browser corretti configurati sul suo sistema. In merito a quale stringa usare all'interno del metodo `.forBrowser()` per altri browser, veda la pagina di riferimento [Browser enum](https://www.selenium.dev/selenium/docs/api/javascript/global.html#Browser).
2. Dia al suo file il seguente contenuto, quindi lo salvi:

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

3. In terminale, si assicuri di essere all'interno della cartella del progetto, quindi inserisca il seguente comando:

   ```bash
   node duck_test_multiple
   ```

> [!NOTE]
> Se sta usando un Mac e decide di testare Safari, potrebbe ricevere un messaggio di errore del tipo "Non è stato possibile creare una sessione: È necessario abilitare l'opzione 'Consenti automazione remota' nel menu Sviluppo di Safari per controllare Safari tramite WebDriver." Se riceve questo messaggio, segua l'istruzione fornita e riprovi.
>
> Potrebbe ricevere un messaggio che dice che non può aprire un'applicazione driver perché non è stata scaricata da una fonte verificata. Se riceve questo messaggio, può ignorare tale impostazione di sicurezza solo per quell'app driver. Ad esempio, su Mac, <kbd>Ctrl</kbd> + clic sull'app, scegliere _Apri_ e scegliere _Apri_ di nuovo dalla finestra di dialogo risultante.

Quindi qui abbiamo eseguito il test come prima, tranne che questa volta l'abbiamo racchiuso all'interno di una funzione, `searchTest()`. Abbiamo creato nuove istanze del browser per più browser, quindi abbiamo passato ciascuno alla funzione in modo che il test venga eseguito su tutti.

Passiamo quindi a esaminare le basi della sintassi di WebDriver, in modo più dettagliato.

## Mini corso sulla sintassi di WebDriver

Esaminiamo alcune caratteristiche chiave della sintassi di WebDriver. Per dettagli più completi, dovrebbe consultare il [riferimento API JavaScript di selenium-webdriver](https://www.selenium.dev/selenium/docs/api/javascript/) per un riferimento dettagliato e la documentazione principale di Selenium's [Selenium WebDriver](https://www.selenium.dev/documentation/webdriver/), che contiene molti esempi da cui imparare scritti in diversi linguaggi.

### Avviare un nuovo test

Per avviare un nuovo test, è necessario includere il modulo `selenium-webdriver`, importando il costruttore `Builder` e l'interfaccia `Browser`:

```js
const { Builder, Browser } = require("selenium-webdriver");
```

Usa il costruttore `Builder()` per creare una nuova istanza di un driver, concatenando il metodo `forBrowser()` per specificare quale browser desidera testare con questo builder.
Il metodo `build()` viene concatenato alla fine per costruire effettivamente l'istanza del driver (vedere il [riferimento della classe Builder](https://www.selenium.dev/selenium/docs/api/javascript/Builder.html) per informazioni dettagliate su queste funzionalità).

```js
let driver = new Builder().forBrowser(Browser.FIREFOX).build();
```

Si noti che è possibile impostare opzioni di configurazione specifiche per i browser da testare, ad esempio è possibile impostare una versione specifica e un sistema operativo per il test nel metodo `forBrowser()`:

```js
let driver = new Builder().forBrowser(Browser.FIREFOX, "130", "MAC").build();
```

Può anche impostare queste opzioni utilizzando una variabile di ambiente, ad esempio:

```bash
SELENIUM_BROWSER=firefox:130:MAC
```

Creiamo un nuovo test che ci permetta di esplorare questo codice mentre ne parliamo. All'interno della directory del progetto di test selenium, crei un nuovo file chiamato `quick_test.js` e aggiunga il seguente codice:

```js
const { Builder, Browser } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
})();
```

Può testare l'esempio inserendo il seguente comando nel suo terminale:

```bash
node quick_test
```

### Ottenere il documento da testare

Per caricare la pagina che desidera effettivamente testare, usi il metodo `get()` dell'istanza del driver che ha creato in precedenza, ad esempio:

```js
driver.get("http://www.google.com");
```

> [!NOTE]
> Vedere il [riferimento della classe WebDriver](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html) per i dettagli delle funzionalità in questa sezione e in quelle sottostanti.

Può usare qualsiasi URL per puntare alla sua risorsa, incluso un URL `file://` per testare un documento locale:

```js
driver.get("file:///Users/bob/git/examples/test_file.html");
```

oppure

```js
driver.get("http://localhost:8888/test_file.html");
```

Ma è meglio usare una posizione su un server remoto in modo che il codice sia più flessibile — quando inizia a usare un server remoto per eseguire i suoi test (vedere più avanti), il suo codice si romperà se tenta di utilizzare percorsi locali.

Aggiorni la sua funzione `example()` come segue, sostituendo il percorso segnaposto con un vero percorso locale a un file HTML sul suo computer, quindi provi a eseguirlo:

```js
const { Builder, Browser } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get("file:///Users/bob/git/examples/test_file.html");
})();
```

### Interagire con il documento

Ora che abbiamo un documento da testare, dobbiamo interagirci in qualche modo, il che di solito comporta il primo selezionare un elemento specifico per testare qualcosa su di esso. Può [selezionare elementi dell'interfaccia utente in molti modi](https://www.selenium.dev/documentation/webdriver/elements/) in WebDriver, incluso per ID, classe, nome dell'elemento, ecc. La selezione effettiva viene eseguita dal metodo `findElement()`, che accetta come parametro un metodo di selezione. Ad esempio, per selezionare un elemento tramite ID:

```js
const element = driver.findElement(By.id("myElementId"));
```

Una delle modalità più utili per trovare un elemento è tramite CSS — il metodo `By.css()` consente di selezionare un elemento utilizzando un selettore CSS.

Aggiorni ora la sua funzione `example()` come segue, quindi esegua l'esempio:

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

### Testare il suo elemento

Ci sono molti modi per interagire con i suoi documenti web e gli elementi al loro interno. Può vedere esempi comuni utili partendo da [Ottieni valori di testo](https://www.selenium.dev/documentation/webdriver/elements/information/#text-content) sulla documentazione di WebDriver.

Se volessimo ottenere il testo all'interno del nostro pulsante, potremmo fare questo:

```js
button.getText().then((text) => {
  console.log(`Button text is '${text}'`);
});
```

Aggiunga questo alla fine della sua funzione `example()` come mostrato di seguito:

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

Esegua l'esempio con `node` nello stesso modo in cui ha fatto in precedenza. Dovrebbe vedere l'etichetta di testo del pulsante riportata all'interno della console.

Facciamo qualcosa di un po' più utile. Sostituisca la voce di codice precedente con `button.click();` come mostrato di seguito:

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

Provi a eseguire di nuovo il suo test; il pulsante verrà cliccato e dovrebbe apparire un popup `alert()`. Almeno sappiamo che il pulsante funziona!

Può anche interagire con il popup. Aggiorni la funzione `example()` come segue e provi a testarla di nuovo:

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

Prossimo, proviamo a inserire del testo negli elementi del modulo. Aggiorni la funzione `example()` come segue e provi a eseguire di nuovo il suo test:

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

Può inviare pressioni di tasti che non possono essere rappresentate da caratteri normali usando le proprietà dell'oggetto `Key`. Ad esempio, sopra abbiamo usato quanto segue per tabulare tra gli input del modulo:

```js
input.sendKeys(Key.TAB);
```

### Attendere che qualcosa si completi

Ci sono volte in cui vorrà che WebDriver attenda che qualcosa si completi prima di procedere. Ad esempio, se carica una nuova pagina, vorrà attendere che il DOM della pagina finisca di caricarsi prima di provare a interagire con uno dei suoi elementi, altrimenti il test potrebbe fallire.

Nel nostro test `duck_test_multiple.js`, ad esempio, abbiamo incluso questa riga:

```js
await driver.sleep(2000);
```

Il metodo `sleep()` accetta un valore che specifica il tempo di attesa in millisecondi — il metodo restituisce una {{jsxref("Promise")}} che si risolve al termine di quel tempo. Usiamo la parola chiave `await` per mettere in pausa la funzione racchiudente fino a quando la promessa non si risolve, dopodiché il codice che segue il metodo viene eseguito.

Potremmo aggiungere un metodo `sleep()` anche al nostro test `quick_test.js` — provi ad aggiornare la sua funzione `example()` come segue:

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

Provi a eseguire il codice aggiornato. WebDriver ora riempirà il primo campo del modulo, attenderà un secondo, quindi testerà se il suo valore è stato riempito (cioè, non è vuoto) utilizzando `getAttribute()` per recuperare il valore del suo attributo `value`. Stampa quindi un messaggio nella console per segnalare il successo/fallimento.

> [!NOTE]
> C'è anche un metodo chiamato [`wait()`](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html#wait), che testa ripetutamente una condizione per un certo lasso di tempo, poi continua a eseguire il codice. Questo utilizza anche la [libreria util](https://www.selenium.dev/selenium/docs/api/javascript/lib_until.js.html), che definisce condizioni comuni da usare insieme a `wait()`.

### Spegnere i driver dopo l'uso

Dopo aver finito di eseguire un test, dovrebbe spegnere tutte le istanze del driver che ha aperto usando il metodo `driver.quit()`, per assicurarsi che non continuino a utilizzare risorse inutilmente. Aggiorni `quick_test.js` come segue:

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

Ora quando lo esegue, dovrebbe vedere il test eseguirsi e l'istanza del browser chiudersi di nuovo dopo che il test è completato.

## Migliori pratiche di test

È stato scritto molto sulle migliori pratiche per la scrittura dei test. Può trovare alcune buone informazioni di base su [Pratiche di Test](https://www.selenium.dev/documentation/test_practices/). In generale, dovrebbe assicurarsi che i suoi test siano:

1. Usi buone strategie di localizzazione: Quando interagisce con il documento, assicuri di utilizzare localizzatori e oggetti di pagina che è improbabile che cambino — se ha un elemento testabile su cui vuole eseguire un test, assicuri che abbia un ID stabile o una posizione nella pagina che può essere selezionata usando un selettore CSS, che non cambierà semplicemente con la prossima iterazione del sito. Vuole rendere i suoi test il meno fragili possibile, cioè, non si romperanno appena qualcosa cambia.
2. Scriva test atomici: Ogni test dovrebbe testare una sola cosa, rendendo facile tenere traccia di quale file di test stia testando quale criterio. Il test `duck_test.js` che abbiamo esaminato sopra è abbastanza buono, poiché testa solo una cosa — se il titolo di una pagina dei risultati di ricerca è impostato correttamente. Potremmo lavorare per dargli un nome migliore in modo che sia più facile capire cosa fa se aggiungiamo più test. Forse `results_page_title_set_correctly.js` sarebbe leggermente migliore?
3. Scriva test autonomi: Ogni test dovrebbe funzionare da solo e non dipendere da altri test per funzionare.

Inoltre, dovremmo menzionare i risultati del test/la reportistica — nei nostri esempi precedenti abbiamo riportato i risultati usando semplici dichiarazioni `console.log()`, ma è tutto fatto in JavaScript, quindi può utilizzare qualsiasi sistema di esecuzione e reportistica di test desideri, sia esso [Mocha](https://mochajs.org/), [Chai](https://www.chaijs.com/), o qualche altro strumento. Lavoriamo attraverso un rapido esempio:

1. Faccia una copia locale del nostro esempio [`mocha_test.js`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/selenium/mocha_test.js) all'interno della sua directory di progetto. Lo metta all'interno di una sottocartella chiamata `test`. Questo esempio utilizza una lunga catena di promesse per eseguire tutti i passaggi richiesti nel nostro test — i metodi basati su promesse che WebDriver utilizza devono risolversi affinché funzioni correttamente.
2. Installa il test harness di mocha eseguendo il seguente comando all'interno della sua directory di progetto:

   ```bash
   npm install --save-dev mocha
   ```

3. Ora può eseguire il test (e qualsiasi altro che inserisce all'interno della sua directory `test`) usando il seguente comando:

   ```bash
   npx mocha --no-timeouts
   ```

4. Dovrebbe includere il flag `--no-timeouts` per assicurarsi che i suoi test non finiscano per fallire a causa del timeout arbitrario di Mocha (che è di 3 secondi).

> **Nota:** [saucelabs-sample-test-frameworks](https://github.com/saucelabs-sample-test-frameworks) contiene diversi esempi utili che mostrano come impostare diverse combinazioni di strumenti di test/assertion.

## Esecuzione di test remoti

Si scopre che eseguire test su server remoti non è molto più difficile che eseguirli localmente. È necessario solo creare la sua istanza del driver, ma con alcune caratteristiche aggiuntive specificate, comprese le capacità del browser su cui desidera testare, l'indirizzo del server e le credenziali dell'utente di cui ha bisogno (se ci sono) per accedervi.

### BrowserStack

Creiamo un esempio per mostrare come ottenere l'esecuzione di un test Selenium in remoto su [BrowserStack](https://www.browserstack.com/automate):

1. All'interno della sua directory di progetto, crei un nuovo file chiamato `bstack_duck_test.js`.
2. Gli dia il seguente contenuto:

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

3. Dalla sua [pagina dei dettagli dell'account e del profilo di BrowserStack](https://www.browserstack.com/accounts/profile/details), ottenga il suo nome utente e chiave di accesso (veda _Nome utente e Chiavi di accesso_).
4. Sostituisca i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i suoi effettivi valori di nome utente e chiave di accesso (e si assicuri di mantenerli sicuri).
5. Esegua il suo test con il seguente comando:

   ```bash
   node bstack_google_test
   ```

   Il test sarà inviato a BrowserStack, e il risultato del test sarà restituito alla sua console. Questo mostra l'importanza di includere qualche tipo di mecanismo di reportistica dei risultati!

6. Ora, se torna alla [dashboard di BrowserStack Automate](https://automate.browserstack.com/dashboard/), vedrà il suo test elencato, con dettagli tra cui una registrazione video del test e diversi log dettagliati di informazioni relative ad esso:
   ![Risultati automatizzati di BrowserStack](bstack_automated_results.png)

> [!NOTE]
> L'opzione di menu _Risorse_ sulla dashboard di automazione di Browserstack contiene una moltitudine di informazioni utili sull'utilizzo per eseguire test automatizzati. Vedere [Selenium con NodeJS](https://www.browserstack.com/docs/automate/selenium/getting-started/nodejs) per informazioni specifiche su Node.

#### Compilare i dettagli del test BrowserStack programmaticamente

Può utilizzare l'API REST di BrowserStack e alcune altre funzionalità per annotare il suo test con più dettagli, come se sia passato, perché è passato, a quale progetto appartiene il test, ecc. BrowserStack non conosce questi dettagli per impostazione predefinita.

Aggiorniamo la nostra demo `bstack_duck_test.js`, per mostrare come funzionano queste funzionalità:

1. Installi il modulo [axios](https://www.npmjs.com/package/axios) eseguendo il seguente comando all'interno della sua directory di progetto:

   ```bash
   npm install axios
   ```

2. Importi il modulo axios così possiamo usarlo per inviare richieste all'API REST di BrowserStack. Aggiunga la seguente riga proprio in cima al suo codice:

   ```js
   const axios = require("axios");
   ```

3. Ora aggiorneremo il nostro oggetto `capabilities` per includere un nome di progetto — aggiunga la seguente riga prima delle parentesi graffe di chiusura, ricordando di aggiungere una virgola alla fine della riga precedente (può variare i nomi dei build e dei progetti per organizzare i test in diverse finestre nella dashboard di automazione di BrowserStack):

   ```js
   project: "DuckDuckGo test 2";
   ```

4. Successivamente, recupereremo il `sessionId` della sessione corrente, e lo useremo (insieme al suo `userName` e `accessKey`) per assemblare l'URL a cui inviare le richieste, per aggiornare i dati di BrowserStack. Includa le seguenti righe appena sotto il blocco che crea l'oggetto `driver` (che inizia con `const driver = new Builder()`):

   ```js
   let sessionId;
   let bstackURL;

   driver.session_.then((sessionData) => {
     sessionId = sessionData.id_;
     bstackURL = `https://${capabilities["bstack:options"].userName}:${capabilities["bstack:options"].accessKey}@www.browserstack.com/automate/sessions/${sessionId}.json`;
   });
   ```

5. Infine, aggiorni il blocco `if ... else` vicino alla fine del codice per inviare chiamate API appropriate a BrowserStack a seconda che il test sia passato o fallito:

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

Una volta completato il test, inviamo una chiamata API a BrowserStack per aggiornare il test con uno stato di superato o fallito, e un motivo per il risultato.

Se ora torna alla sua [dashboard di BrowserStack Automate](https://automate.browserstack.com/dashboard/), dovrebbe vedere la sua sessione di test come prima, ma con i suoi dati personalizzati allegati. Mostra uno stato di "PASSED", e la ragione riportata dall'API REST per il passaggio:

![Risultati personalizzati di BrowserStack](bstack_custom_results.png)

### Sauce Labs

Esaminiamo un esempio che dimostra come ottenere test Selenium da eseguire in remoto su Sauce Labs:

1. All'interno della sua directory di progetto, crei un nuovo file chiamato `sauce_google_test.js`.
2. Gli dia il seguente contenuto:

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

3. Dalle sue [impostazioni utente di Sauce Labs](https://app.saucelabs.com/user-settings), ottenga il suo nome utente e chiave di accesso. Sostituisca i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i suoi effettivi valori di nome utente e chiave di accesso (e si assicuri di mantenerli sicuri).
4. Esegua il suo test con il seguente comando:

   ```bash
   node sauce_google_test
   ```

   Il test sarà inviato a Sauce Labs, e il risultato del test sarà restituito alla sua console. Questo mostra l'importanza di includere qualche tipo di meccanismo di reportistica dei risultati!

5. Ora, se va alla sua pagina [dashboard dei test automatizzati di Sauce Labs](https://app.saucelabs.com/dashboard/tests), vedrà il suo test elencato; da qui potrà vedere video, screenshot, e altri dati simili.
   ![Test automatizzato di Sauce Labs](sauce_labs_automated_test.png)

> [!NOTE]
> Il [Configuratore della Piattaforma](https://saucelabs.com/products/platform-configurator#/) di Sauce Labs è uno strumento utile per generare oggetti di capacità da fornire alle sue istanze del driver, in base al browser/SO su cui vuole testare.

> [!NOTE]
> Per ulteriori dettagli utili sui test con Sauce Labs e Selenium, consultare [Inizia con Selenium per il test automatizzato dei siti web](https://docs.saucelabs.com/web-apps/automated-testing/selenium/), e [Test instantanei Selenium Node.js](https://docs.saucelabs.com/web-apps/automated-testing/selenium/sample-scripts/#nodejs).

#### Compilare programmaticamente i dettagli del test in Sauce Labs

Può usare l'API di Sauce Labs per annotare il suo test con più dettagli, come se sia passato, il nome del test, ecc. Sauce Labs non conosce questi dettagli per impostazione predefinita!

Per fare ciò, deve:

1. Installare la libreria Node Sauce Labs usando il seguente comando (se non l'ha già fatto per questo progetto):

   ```bash
   npm install saucelabs --save-dev
   ```

2. Richiedere saucelabs — metterlo all'inizio del suo file `sauce_google_test.js`, appena sotto le dichiarazioni delle variabili precedenti:

   ```js
   const SauceLabs = require("saucelabs");
   ```

3. Creare una nuova istanza di SauceLabs, aggiungendo quanto segue subito sotto:

   ```js
   const saucelabs = new SauceLabs({
     username: "YOUR-USER-NAME",
     password: "YOUR-ACCESS-KEY",
   });
   ```

   Ancora una volta, sostituisca i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i suoi veri valori di nome utente e chiave di accesso (noti che il pacchetto npm di saucelabs usa confusamente `password`, non `accessKey`). Poiché sta usandoli due volte ora, potrebbe voler creare un paio di variabili helper per conservarli.

4. Sotto il blocco dove definisce la variabile `driver` (appena sotto la riga `build()`), aggiungi il seguente blocco — questo ottiene l'ID `session` del driver corretto che serve per scrivere dati al lavoro (può vederlo in azione nel prossimo blocco di codice):

   ```js
   driver.getSession().then((sessionid) => {
     driver.sessionID = sessionid.id_;
   });
   ```

5. Infine, sostituisca il blocco `driver.sleep(2000)` vicino alla fine del codice con quanto segue:

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

Qui abbiamo impostato una variabile `testPassed` a `true` o `false` a seconda che il test sia passato o fallito, quindi abbiamo utilizzato il metodo `saucelabs.updateJob()` per aggiornare i dettagli.

Se ora torna alla sua pagina [dashboard dei test automatizzati di Sauce Labs](https://app.saucelabs.com/dashboard/tests), dovrebbe vedere che il suo nuovo lavoro ora ha i dati aggiornati allegati:

![Informazioni sul lavoro aggiornato di Sauce Labs](sauce_labs_updated_job_info.png)

### Il tuo server remoto

Se non vuole usare un servizio come Sauce Labs o BrowserStack, può sempre configurare il suo server di test remoto. Esaminiamo come farlo.

1. Il server remoto di Selenium richiede Java per funzionare. Scarichi l'ultimo JDK per la sua piattaforma dalla [pagina dei download di Java SE](https://www.oracle.com/java/technologies/downloads/). Lo installi quando è stato scaricato.
2. Successivamente, scarichi l'ultimo [server standalone di Selenium](https://selenium-release.storage.googleapis.com/index.html) — questo funge da proxy tra il suo script e i driver del browser. Scegliere l'ultima versione stabile (cioè, non una beta), e dalla lista scegliere un file che inizi con "selenium-server-standalone". Quando questo è stato scaricato, metterlo in un luogo sensibile, come nella sua directory home. Se non ha ancora aggiunto la posizione al suo `PATH`, faccia ora (veda la [Configurazione di Selenium in Node](#configurare_selenium_in_node) sezione).
3. Esegua il server standalone inserendo quanto segue in un terminale sul suo computer server

   ```bash
   java -jar selenium-server-standalone-3.0.0.jar
   ```

   (aggiorni il nome del file `.jar`) in modo che corrisponda esattamente a quale file ha.

4. Il server sarà attivo su `http://localhost:4444/wd/hub` — provi ad andarci ora per vedere cosa ottiene.

Ora che il server è in esecuzione, creiamo un demo test che verrà eseguito sul server selenium remoto.

1. Crei una copia del suo file `google_test.js`, e lo chiami `google_test_remote.js`; lo metta nella sua directory di progetto.
2. Aggiorni la riga di codice (che inizia con `const driver = …`) in questo modo

   ```js
   const driver = new Builder()
     .forBrowser(Browser.FIREFOX)
     .usingServer("http://localhost:4444/wd/hub")
     .build();
   ```

3. Esegua il suo test, e dovrebbe vederlo eseguirsi come previsto; questa volta tuttavia lo eseguirà sul server standalone:

   ```bash
   node google_test_remote.js
   ```

Quindi questo è molto interessante. Lo abbiamo testato localmente, ma potrebbe configurarlo praticamente su qualsiasi server insieme ai driver del browser pertinenti, e quindi connettere i suoi script utilizzando l'URL che sceglie di esporre.

## Integrazione di Selenium con strumenti CI

Come un altro punto, è anche possibile integrare Selenium e strumenti correlati come LambdaTest, e Sauce Labs con strumenti di integrazione continua (CI) — questo è utile, in quanto consente di eseguire i suoi test tramite uno strumento CI, e solo confermare nuove modifiche nel suo repository di codice se i test passano.

Non è il caso di esaminare questo aspetto in dettaglio in questo articolo, ma suggeriremmo di iniziare con Travis CI — questo è probabilmente lo strumento CI più facile da iniziare e ha una buona integrazione con strumenti web come GitHub e Node.

Per iniziare, vedere per esempio:

- [Travis CI per principianti completi](https://docs.travis-ci.com/user/for-beginners)
- [Costruire un progetto Node.js](https://docs.travis-ci.com/user/languages/javascript-with-nodejs/) (con Travis)
- [Utilizzo di LambdaTest con Travis CI](https://www.lambdatest.com/support/docs/travis-ci-with-lambdatest/)
- [Utilizzo di LambdaTest con CircleCI](https://www.lambdatest.com/support/docs/circleci-integration-with-lambdatest/)
- [Utilizzo di LambdaTest con Jenkins](https://www.lambdatest.com/support/docs/jenkins-with-lambdatest/)
- [Utilizzo di Sauce Labs con Travis CI](https://docs.travis-ci.com/user/sauce-connect/)

> [!NOTE]
> Se desidera eseguire test continui con **automazione senza codice** può utilizzare [Endtest](https://www.endtest.io/) o [TestingBot](https://testingbot.com/).

## Sommario

Questo modulo dovrebbe essere stato divertente e dovrebbe averle dato abbastanza idee su scrivere ed eseguire test automatizzati per permetterle di iniziare a scrivere i suoi test automatizzati.

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}
