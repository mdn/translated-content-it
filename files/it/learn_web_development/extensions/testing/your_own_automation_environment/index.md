---
title: Impostazione del proprio ambiente di test automatizzato
short-title: Impostazione dell'ambiente di automazione
slug: Learn_web_development/Extensions/Testing/Your_own_automation_environment
l10n:
  sourceCommit: e6d43da6c6d28a6ac92cdd47882809ffbdf987ce
---

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}

In questo articolo, insegneremo come installare il proprio ambiente di automazione e eseguire i propri test utilizzando Selenium/WebDriver e una libreria di testing come selenium-webdriver per Node. Vedremo anche come integrare il tuo ambiente di test locale con strumenti commerciali come quelli discussi nell'articolo precedente.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i core linguaggi <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei princìpi ad alto livello del
        <a href="/it/docs/Learn_web_development/Extensions/Testing/Introduction">test cross browser</a>, e
        dei <a href="/it/docs/Learn_web_development/Extensions/Testing/Automated_testing">test automatizzati</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Mostrare come configurare localmente un ambiente di test Selenium ed eseguire test, e come integrarlo con strumenti come LambdaTest, Sauce Labs e BrowserStack.
      </td>
    </tr>
  </tbody>
</table>

## Selenium

[Selenium](https://www.selenium.dev/) è lo strumento di automazione del browser più popolare. Esistono altri modi, ma il miglior modo per usare Selenium è tramite WebDriver, un'API potente che si costruisce sopra Selenium e effettua chiamate a un browser per automatizzarlo, eseguendo azioni come "apri questa pagina web", "passa sopra questo elemento sulla pagina", "clicca su questo link", "vedi se il link apre questo URL", ecc. Questo è ideale per l'esecuzione di test automatizzati.

Come installare e usare WebDriver dipende dall'ambiente di programmazione che si desidera utilizzare per scrivere ed eseguire i test. Gli ambienti più popolari dispongono di un pacchetto o framework che installerà WebDriver e i collegamenti necessari per comunicare con WebDriver usando questo linguaggio, ad esempio Java, C#, Ruby, Python, JavaScript (Node), ecc. Vedi [Impostazione di un progetto Selenium-WebDriver](https://www.selenium.dev/documentation/webdriver/getting_started/) per maggiori dettagli sulle configurazioni di Selenium per diverse lingue.

Diversi browser richiedono driver diversi per consentire a WebDriver di comunicare con e controllarli. Vedi [Piattaforme supportate da Selenium](https://www.selenium.dev/downloads/) per ulteriori informazioni su dove ottenere i driver del browser, ecc.

Copriamo la scrittura e l'esecuzione di test Selenium usando Node.js, poiché è rapido e facile da iniziare ed è un ambiente più familiare per i programmatori frontend.

> [!NOTE]
> Se desideri scoprire come usare WebDriver con altri ambienti server-side, consulta anche [Piattaforme supportate da Selenium](https://www.selenium.dev/downloads/) per alcuni link utili.

### Configurazione di Selenium in Node

1. Per iniziare, configura un nuovo progetto npm, come discusso in [Impostazione di Node e npm](/it/docs/Learn_web_development/Extensions/Testing/Automated_testing#setting_up_node_and_npm) nell'ultimo capitolo. Chiamalo diversamente, ad esempio `selenium-test`.
2. Successivamente, dobbiamo installare un framework che ci permetta di lavorare con Selenium dall'interno di Node. Sceglieremo il [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver) ufficiale di Selenium, poiché sembra che la documentazione sia abbastanza aggiornata ed è ben mantenuta. Se volessi opzioni diverse, [webdriver.io](https://webdriver.io/) e [nightwatch.js](https://nightwatchjs.org/) sono anche buone scelte. Per installare selenium-webdriver, esegui il seguente comando, assicurandoti di essere all'interno della tua cartella progetto:

   ```bash
   npm install selenium-webdriver
   ```

> [!NOTE]
> È comunque una buona idea seguire questi passaggi anche se hai precedentemente installato selenium-webdriver e scaricato i driver del browser. Assicurati che tutto sia aggiornato.

Successivamente, devi scaricare i driver rilevanti per consentire a WebDriver di controllare i browser che desideri testare. Puoi trovare i dettagli su dove ottenerli sulla pagina di [selenium-webdriver](https://www.npmjs.com/package/selenium-webdriver) (vedi la tabella nella prima sezione). Ovviamente, alcuni dei browser sono specifici per sistema operativo, ma ci atterremo a Firefox e Chrome, poiché sono disponibili in tutti i principali sistemi operativi.

1. Scarica gli ultimi driver [GeckoDriver](https://github.com/mozilla/geckodriver/releases/) (per Firefox) e [ChromeDriver](https://googlechromelabs.github.io/chrome-for-testing/#stable).
2. Decomprimi i file in una posizione abbastanza facile da navigare, come la radice della tua directory utente.
3. Aggiungi la posizione dei driver `chromedriver` e `geckodriver` alla variabile di sistema `PATH`. Questo dovrebbe essere un percorso assoluto dalla radice del tuo disco rigido alla directory contenente i driver. Ad esempio, se fossimo su una macchina macOS, il nostro nome utente fosse bob, e abbiamo inserito i nostri driver nella radice della nostra cartella personale, il percorso sarebbe `/Users/bob`.

> [!NOTE]
> Per ribadire, il percorso che aggiungi a `PATH` deve essere il percorso della directory contenente i driver, non i percorsi dei driver stessi! Questo è un errore comune.

Per impostare la tua variabile `PATH` su un sistema macOS e sulla maggior parte dei sistemi Linux:

1. Apri il tuo file `.zprofile` (o `.bash_profile` se il tuo sistema usa la shell `bash`).
   > [!NOTE]
   > Se non riesci a vedere i file nascosti, sarà necessario visualizzarli, vedi [Mostra/Nascondi i file nascosti in macOS](https://ianlunn.co.uk/articles/quickly-showhide-hidden-files-mac-os-x-mavericks/) o [Mostra le cartelle nascoste in Ubuntu](https://askubuntu.com/questions/470837/how-to-show-hidden-folders-in-file-manager-nautilus-on-ubuntu)).
2. Incolla quanto segue alla fine del tuo file (aggiornando il percorso come effettivamente è sul tuo computer):

   ```bash
   #Add WebDriver browser drivers to PATH
   export PATH=$PATH:/Users/bob
   ```

3. Salva e chiudi questo file, quindi riavvia il tuo Terminale/prompt dei comandi per riapplicare la configurazione di Bash.
4. Controlla che i nuovi percorsi siano nella variabile `PATH` inserendo quanto segue nel tuo terminale:

   ```bash
   echo $PATH
   ```

   Dovresti vederlo stampato nel terminale.

> [!NOTE]
> Per impostare la tua variabile `PATH` su Windows, segui le istruzioni su [Come posso aggiungere una nuova cartella al mio system path?](https://www.itprotoday.com/)

Proviamo un test rapido per assicurarci che tutto funzioni.

1. Crea un nuovo file all'interno della tua directory di progetto chiamato `duck_test.js`:
2. Inserisci il seguente contenuto, quindi salvalo:

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

3. Nel terminale, assicurati di essere all'interno della tua cartella progetto, quindi inserisci il seguente comando:

   ```bash
   node duck_test
   ```

Dovresti vedere un'istanza di Firefox aprirsi automaticamente! DuckDuckGo verrà caricato automaticamente in una scheda, "webdriver" verrà inserito nella casella di ricerca, e il pulsante di ricerca verrà cliccato. WebDriver aspetterà poi per 1 secondo; il titolo del documento verrà quindi accesso, e se è "webdriver at DuckDuckGo", restituiremo un messaggio per indicare che il test è passato.

Aspettiamo poi 2 secondi, dopo di che WebDriver chiuderà l'istanza di Firefox e si fermèrà.

## Testare in più browser contemporaneamente

Non c'è nulla che impedisca di eseguire il test su più browser allo stesso tempo. Proviamo!

1. Crea un altro nuovo file all'interno della tua directory di progetto chiamato `duck_test_multiple.js`. Puoi tranquillamente cambiare i riferimenti ad alcuni degli altri browser che abbiamo aggiunto, rimuoverli, ecc., a seconda dei browser disponibili sul tuo sistema operativo per effettuare i test. Dovrai assicurarti di avere i driver dei browser corretti installati nel tuo sistema. In termini di quale stringa usare all'interno del metodo `.forBrowser()` per altri browser, consulta la pagina di riferimento [Browser enum](https://www.selenium.dev/selenium/docs/api/javascript/global.html#Browser).
2. Fornisci al tuo file il seguente contenuto, quindi salvalo:

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

3. Nel terminale, assicurati di essere all'interno della tua cartella progetto, quindi inserisci il seguente comando:

   ```bash
   node duck_test_multiple
   ```

> [!NOTE]
> Se stai usando un Mac e decidi di testare Safari, potresti ricevere un messaggio di errore del tipo "Could not create a session: You must enable the 'Allow Remote Automation' option in Safari's Develop menu to control Safari via WebDriver." Se ricevi questo, segui l'istruzione fornita e riprova.
>
> Potresti ricevere un messaggio che indica che non puoi aprire un'app driver perché non è stata scaricata da una fonte verificata. Se ricevi questo, puoi ignorare quell'impostazione di sicurezza solo per quell'app driver. Ad esempio, su Mac, <kbd>Ctrl</kbd> + clic sull'app, seleziona _Apri_, e seleziona di nuovo _Apri_ dal dialogo risultante.

Quindi qui abbiamo eseguito il test come prima, eccetto che questa volta lo abbiamo incapsulato all'interno di una funzione, `searchTest()`. Abbiamo creato nuove istanze di browser per diversi browser, quindi passato ciascuno alla funzione in modo che il test venga eseguito su tutti.

Proseguiamo e diamo un'occhiata alle basi della sintassi di WebDriver, in modo più dettagliato.

## Corso intensivo di sintassi di WebDriver

Diamo un'occhiata ad alcune caratteristiche chiave della sintassi di webdriver. Per dettagli più completi, dovresti consultare la [riferimento API di selenium-webdriver JavaScript](https://www.selenium.dev/selenium/docs/api/javascript/) per un riferimento dettagliato e la documentazione principale di Selenium [Selenium WebDriver](https://www.selenium.dev/documentation/webdriver/), che contiene molti esempi da cui imparare scritti in diverse lingue.

### Avviare un nuovo test

Per avviare un nuovo test, è necessario includere il modulo `selenium-webdriver`, importando il costruttore `Builder` e l'interfaccia `Browser`:

```js
const { Builder, Browser } = require("selenium-webdriver");
```

Usi il costruttore `Builder()` per creare una nuova istanza di un driver, concatenando il metodo `forBrowser()` per specificare quale browser desideri testare con questo costruttore.
Il metodo `build()` viene concatenato alla fine per costruire effettivamente l'istanza del driver (vedi il [riferimento della classe Builder](https://www.selenium.dev/selenium/docs/api/javascript/Builder.html) per informazioni dettagliate su queste caratteristiche).

```js
let driver = new Builder().forBrowser(Browser.FIREFOX).build();
```

Nota che è possibile impostare opzioni di configurazione specifiche per i browser da testare, ad esempio puoi impostare una versione specifica e un sistema operativo da testare nel metodo `forBrowser()`:

```js
let driver = new Builder().forBrowser(Browser.FIREFOX, "130", "MAC").build();
```

Puoi anche impostare queste opzioni usando una variabile di ambiente, ad esempio:

```bash
SELENIUM_BROWSER=firefox:130:MAC
```

Creiamo un nuovo test per permetterci di esplorare questo codice mentre ne parliamo. All'interno della tua directory di progetto di test selenium, crea un nuovo file chiamato `quick_test.js`, e aggiungi il seguente codice a esso:

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

### Ottenere il documento che desideri testare

Per caricare la pagina che vuoi effettivamente testare, usi il metodo `get()` dell'istanza di driver che hai creato precedentemente, ad esempio:

```js
driver.get("http://www.google.com");
```

> [!NOTE]
> Consulta il [riferimento della classe WebDriver](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html) per i dettagli delle caratteristiche in questa sezione e quelle che la seguono.

Puoi usare qualsiasi URL per puntare alla tua risorsa, inclusi URL `file://` per testare un documento locale:

```js
driver.get("file:///Users/bob/git/examples/test_file.html");
```

o

```js
driver.get("http://localhost:8888/test_file.html");
```

Ma è meglio usare una posizione su server remoto in modo che il codice sia più flessibile — quando inizi a utilizzare un server remoto per eseguire i tuoi test (vedi più avanti), il tuo codice si romperà se provi a usare percorsi locali.

Aggiorna la tua funzione `example()` come segue, sostituendo il percorso segnaposto con un vero percorso locale a un file HTML sul tuo computer, quindi prova a eseguirlo:

```js
const { Builder, Browser } = require("selenium-webdriver");

(async function example() {
  const driver = await new Builder().forBrowser(Browser.FIREFOX).build();
  driver.get("file:///Users/bob/git/examples/test_file.html");
})();
```

### Interagire con il documento

Ora che abbiamo un documento da testare, dobbiamo interagire con esso in qualche modo, il che di solito comporta prima la selezione di un elemento specifico per testare qualcosa a riguardo. Puoi [selezionare elementi UI in molti modi](https://www.selenium.dev/documentation/webdriver/elements/) in WebDriver, inclusi per ID, classe, nome dell'elemento, ecc. La selezione effettiva è fatta dal metodo `findElement()`, il quale accetta come parametro un metodo di selezione. Ad esempio, per selezionare un elemento per ID:

```js
const element = driver.findElement(By.id("myElementId"));
```

Uno dei modi più utili per trovare un elemento è tramite CSS — il metodo `By.css()` ti consente di selezionare un elemento usando un selettore CSS.

Aggiorna ora la tua funzione `example()` come segue, quindi esegui l'esempio:

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

Ci sono molti modi per interagire con i tuoi documenti web e gli elementi al loro interno. Puoi vedere esempi comuni a partire da [Ottenere valori di testo](https://www.selenium.dev/documentation/webdriver/elements/information/#text-content) nella documentazione di WebDriver.

Se volessimo ottenere il testo all'interno del nostro pulsante, potremmo farlo così:

```js
button.getText().then((text) => {
  console.log(`Button text is '${text}'`);
});
```

Aggiungi questo alla fine della funzione `example()` ora come mostrato sotto:

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

Esegui l'esempio con `node` nello stesso modo in cui hai fatto in precedenza. Dovresti vedere l'etichetta di testo del pulsante riportata all'interno della console.

Facciamo qualcosa di un po' più utile. Sostituisci la voce di codice precedente con `button.click();` come mostrato sotto:

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

Prova a eseguire di nuovo il tuo test; il pulsante verrà cliccato e dovrebbe apparire un popup `alert()`. Almeno sappiamo che il pulsante funziona!

Puoi anche interagire con il popup. Aggiorna la funzione `example()` come segue, e prova a testarla di nuovo:

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

Successivamente, proviamo a inserire del testo negli elementi del modulo. Aggiorna la funzione `example()` come segue e prova a eseguire di nuovo il tuo test:

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

Puoi inviare tasti che non possono essere rappresentati da caratteri normali usando le proprietà dell'oggetto `Key`. Ad esempio, sopra abbiamo usato quanto segue per passare da un input del modulo all'altro:

```js
input.sendKeys(Key.TAB);
```

### Attendere che qualcosa si completi

Ci sono momenti in cui vorrai far aspettare WebDriver che qualcosa si completi prima di proseguire. Ad esempio, se carichi una nuova pagina, vorrai aspettare che il DOM della pagina finisca di caricarsi prima di tentare di interagire con uno qualsiasi dei suoi elementi, altrimenti il test probabilmente fallirà.

Nel nostro test `duck_test_multiple.js`, ad esempio, abbiamo incluso questa linea:

```js
await driver.sleep(2000);
```

Il metodo `sleep()` accetta un valore che specifica il tempo di attesa in millisecondi — il metodo restituisce un {{jsxref("Promise")}} che si risolve alla fine di quel tempo. Usiamo la keyword `await` per mettere in pausa la funzione contenente fino a quando la promessa si risolve, dopo di che il codice seguente al metodo viene eseguito.

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

Prova a eseguire il codice aggiornato. WebDriver ora riempirà il primo campo del modulo, aspetterà per un secondo, quindi testerà se il suo valore è stato riempito (cioè, non è vuoto) usando `getAttribute()` per recuperare il suo valore dell'attributo `value`. Stamperà quindi un messaggio sulla console per segnalare il successo o il fallimento.

> [!NOTE]
> Esiste anche un metodo chiamato [`wait()`](https://www.selenium.dev/selenium/docs/api/javascript/WebDriver.html#wait), che testa ripetutamente una condizione per un certo periodo di tempo, e poi continua a eseguire il codice. Questo utilizza anche la [libreria util](https://www.selenium.dev/selenium/docs/api/javascript/lib_until.js.html), che definisce condizioni comuni da usare insieme a `wait()`.

### Spegnere i driver dopo l'uso

Dopo aver terminato l'esecuzione di un test, dovresti spegnere tutte le istanze di driver che hai aperto usando il metodo `driver.quit()`, per assicurarti che non continuino a usare risorse inutilmente. Aggiorna `quick_test.js` come segue:

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

Ora, quando lo esegui, dovresti vedere il test eseguire e l'istanza del browser chiudersi di nuovo dopo che il test è completo.

## Migliori pratiche di test

È stato scritto molto sulle migliori pratiche per scrivere test. Puoi trovare alcune buone informazioni di base su [Test Practices](https://www.selenium.dev/documentation/test_practices/). In generale, dovresti assicurarti che i tuoi test siano:

1. Usando buone strategie di localizzazione: Quando stai [interagendo con il documento](#interagire_con_il_documento), assicurati di usare localizzatori e oggetti pagina che non cambiano — se hai un elemento testabile su cui vuoi eseguire un test, assicurati che abbia un ID stabile, o una posizione sulla pagina che possa essere selezionata usando un selettore CSS, che non cambierà con la prossima iterazione del sito. Vuoi rendere i tuoi test il meno fragili possibile, cioè, non si romperanno solo quando qualcosa cambia.
2. Scrivi test atomici: Ogni test dovrebbe testare una sola cosa, rendendo facile tenere traccia di quale file di test sta testando quale criterio. Il test `duck_test.js` che abbiamo visto sopra è abbastanza buono, in quanto testa solo una singola cosa — se il titolo di una pagina dei risultati di ricerca è impostato correttamente. Potremmo lavorare su dargli un nome migliore in modo che sia più facile capire cosa fa se aggiungiamo più test. Forse `results_page_title_set_correctly.js` sarebbe leggermente migliore?
3. Scrivi test autonomi: Ogni test dovrebbe funzionare da solo e non dipendere da altri test per funzionare.

Inoltre, dovremmo menzionare i risultati dei test/reporting — nei nostri esempi sopra, abbiamo riportato i risultati utilizzando semplici dichiarazioni `console.log()`, ma tutto ciò è fatto in JavaScript, quindi puoi usare qualsiasi sistema di esecuzione e reporting di test vuoi, sia esso [Mocha](https://mochajs.org/), [Chai](https://www.chaijs.com/), o qualche altro strumento. Esaminiamo attraverso un esempio rapido:

1. Fai una copia locale del nostro esempio [`mocha_test.js`](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/selenium/mocha_test.js) all'interno della tua directory di progetto. Mettilo all'interno di una sottocartella chiamata `test`. Questo esempio usa una lunga catena di promise per eseguire tutti i passaggi richiesti nel nostro test — i metodi basati su promise che WebDriver usa devono risolversi affinché funzioni correttamente.
2. Installa il framework di test mocha eseguendo il seguente comando all'interno della tua directory di progetto:

   ```bash
   npm install --save-dev mocha
   ```

3. Ora puoi eseguire il test (e altri che metti dentro la tua directory `test`) usando il seguente comando:

   ```bash
   npx mocha --no-timeouts
   ```

4. Dovresti includere l'opzione `--no-timeouts` per assicurarti che i tuoi test non falliscano a causa del timeout arbitrario di Mocha (che è di 3 secondi).

> **Nota:** [saucelabs-sample-test-frameworks](https://github.com/saucelabs-sample-test-frameworks) contiene diversi esempi utili che mostrano come configurare diverse combinazioni di strumenti di test/asserzioni.

## Eseguire test remoti

Risulta che eseguire test su server remoti non è poi così difficile rispetto a eseguirli localmente. Devi solo creare la tua istanza di driver, ma con un paio di caratteristiche in più specificate, incluse le capacità del browser su cui vuoi testare, l'indirizzo del server, e le credenziali utente che hai bisogno (se ci sono) per accedervi.

### BrowserStack

Creiamo un esempio per mostrare come ottenere l'esecuzione di un test Selenium remotamente su [BrowserStack](https://www.browserstack.com/automate):

1. All'interno della tua directory di progetto, crea un nuovo file chiamato `bstack_duck_test.js`.
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

3. Dalla tua [pagina dei dettagli dell'account e del profilo BrowserStack](https://www.browserstack.com/accounts/profile/details), ottieni il tuo nome utente e la chiave di accesso (vedi _Username and Access Keys_).
4. Sostituisci i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con il tuo nome utente e i valori della chiave di accesso effettivi (e assicurati di mantenerli sicuri).
5. Esegui il tuo test con il seguente comando:

   ```bash
   node bstack_google_test
   ```

   Il test verrà inviato a BrowserStack, e il risultato del test verrà restituito alla tua console. Ciò dimostra l'importanza di includere un qualche tipo di meccanismo di reporting dei risultati!

6. Ora, se torni alla [dashboard Automate di BrowserStack](https://automate.browserstack.com/dashboard/), vedrai il tuo test elencato, con dettagli inclusi una registrazione video del test e molteplici log dettagliati delle informazioni riguardanti:
   ![Risultati automatici di BrowserStack](bstack_automated_results.png)

> [!NOTE]
> L'opzione di menu _Resources_ sulla dashboard di automazione BrowserStack contiene una ricchezza di informazioni utili su come usarlo per eseguire test automatizzati. Vedi [Selenium con NodeJS](https://www.browserstack.com/docs/automate/selenium/getting-started/nodejs) per informazioni specifiche su Node.

#### Inserimento programmato dei dettagli del test di BrowserStack

Puoi usare l'API REST di BrowserStack e alcune altre capacità per annotare il tuo test con più dettagli, come se il test è passato, perché è passato, a quale progetto appartiene il test, ecc. BrowserStack non conosce questi dettagli di default.

Aggiorniamo il nostro demo `bstack_duck_test.js`, per mostrare come funzionano queste caratteristiche:

1. Installa il modulo [axios](https://www.npmjs.com/package/axios) eseguendo il seguente comando all'interno della tua directory di progetto:

   ```bash
   npm install axios
   ```

2. Importa il modulo axios in modo che possiamo usarlo per inviare richieste all'API REST di BrowserStack. Aggiungi la seguente linea all'inizio del tuo codice:

   ```js
   const axios = require("axios");
   ```

3. Ora aggiorneremo il nostro oggetto `capabilities` per includere un nome di progetto — aggiungi la seguente linea prima della parentesi graffa di chiusura, ricordando di aggiungere una virgola alla fine della linea precedente (puoi variare i nomi di build e project per organizzare i test in finestre diverse nella dashboard di automazione di BrowserStack):

   ```js
   const capabilities = {
     // …
     project: "DuckDuckGo test 2",
   };
   ```

4. Successivamente, recupereremo l'`sessionId` della sessione corrente, e lo useremo (insieme al tuo `userName` e `accessKey`) per assemblare l'URL a cui inviare richieste per aggiornare i dati di BrowserStack. Includi le seguenti linee appena sotto il blocco che crea l'oggetto `driver` (che inizia con `const driver = new Builder()`):

   ```js
   let sessionId;
   let bstackURL;

   driver.session_.then((sessionData) => {
     sessionId = sessionData.id_;
     bstackURL = `https://${capabilities["bstack:options"].userName}:${capabilities["bstack:options"].accessKey}@www.browserstack.com/automate/sessions/${sessionId}.json`;
   });
   ```

5. Infine, aggiorna il blocco `if ... else` vicino al fondo del codice per inviare le chiamate API appropriate a BrowserStack a seconda se il test è passato o fallito:

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

Una volta che il test è completato, inviamo una chiamata API a BrowserStack per aggiornare il test con un stato di passaggio o fallimento e una ragione per il risultato.

Se ora torni alla [dashboard Automate di BrowserStack](https://automate.browserstack.com/dashboard/), dovresti vedere la tua sessione di test come prima, ma con i tuoi dati personalizzati allegati. Mostra uno stato di "PASSED", e la ragione riportata dall'API REST per il passaggio:

![Risultati personalizzati di BrowserStack](bstack_custom_results.png)

### Sauce Labs

Diamo un'occhiata a un esempio che dimostra come ottenere l'esecuzione dei test Selenium remotamente su Sauce Labs:

1. All'interno della tua directory di progetto, crea un nuovo file chiamato `sauce_google_test.js`.
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

3. Dalle [impostazioni utente di Sauce Labs](https://app.saucelabs.com/user-settings), ottieni il tuo nome utente e la chiave di accesso. Sostituisci i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con il tuo nome utente e i valori della chiave di accesso effettivi (e assicurati di tenerli al sicuro).
4. Esegui il tuo test con il seguente comando:

   ```bash
   node sauce_google_test
   ```

   Il test verrà inviato a Sauce Labs, e il risultato del test verrà restituito alla tua console. Ciò dimostra l'importanza di includere un qualche tipo di meccanismo di reporting dei risultati!

5. Ora, se vai alla pagina [Dashboard dei test automatici di Sauce Labs](https://app.saucelabs.com/dashboard/tests), vedrai il tuo test elencato; da qui potrai vedere video, screenshot e altri dati simili.
   ![Test automatico di Sauce Labs](sauce_labs_automated_test.png)

> [!NOTE]
> Il [Platform Configurator](https://saucelabs.com/products/platform-configurator#/) di Sauce Labs è uno strumento utile per generare oggetti di capacità da fornire alle tue istanze di driver, basato su quale browser/sistema operativo desideri testare.

> [!NOTE]
> Per dettagli più utili sui test con Sauce Labs e Selenium, controlla [Getting Started with Selenium for Automated Website Testing](https://docs.saucelabs.com/web-apps/automated-testing/selenium/), e [Instant Selenium Node.js Tests](https://docs.saucelabs.com/web-apps/automated-testing/selenium/sample-scripts/#nodejs).

#### Inserire dettagli del test di Sauce Labs programmaticamente

Puoi usare l'API di Sauce Labs per annotare il tuo test con più dettagli, come se il test è passato, il nome del test, ecc. Sauce Labs non conosce questi dettagli di default!

Per farlo, devi:

1. Installare il wrapper Node di Sauce Labs usando il seguente comando (se non l'hai già fatto per questo progetto):

   ```bash
   npm install saucelabs --save-dev
   ```

2. Richiede saucelabs — inserisci questo all'inizio del tuo file `sauce_google_test.js`, appena sotto le dichiarazioni variabili precedenti:

   ```js
   const SauceLabs = require("saucelabs");
   ```

3. Crea una nuova istanza di SauceLabs, aggiungendo quanto segue subito sotto a quello:

   ```js
   const saucelabs = new SauceLabs({
     username: "YOUR-USER-NAME",
     password: "YOUR-ACCESS-KEY",
   });
   ```

   Di nuovo, sostituisci i segnaposto `YOUR-USER-NAME` e `YOUR-ACCESS-KEY` nel codice con i tuoi valori effettivi di nome utente e chiave di accesso (nota che il pacchetto npm di saucelabs usa piuttosto confusamente `password`, non `accessKey`). Poiché li usi due volte ora, potresti voler creare un paio di variabili aiuto per memorizzarli.

4. Sotto il blocco in cui definisci la variabile `driver` (appena sotto la linea `build()`), aggiungi il seguente blocco — questo ottiene l'`sessionID` corretto del driver che ci serve per scrivere dati al lavoro (puoi vederlo in azione nel blocco di codice successivo):

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

Qui abbiamo impostato una variabile `testPassed` a `true` o `false` a seconda se il test è passato o fallito, poi abbiamo usato il metodo `saucelabs.updateJob()` per aggiornare i dettagli.

Se ora torni alla pagina [Dashboard dei test automatici di Sauce Labs](https://app.saucelabs.com/dashboard/tests), dovresti vedere che il tuo nuovo lavoro ha ora i dati aggiornati allegati a esso:

![Informazioni di lavoro aggiornate di Sauce Labs](sauce_labs_updated_job_info.png)

### Il tuo server remoto

Se non vuoi usare un servizio come Sauce Labs o BrowserStack, puoi sempre configurare il tuo server di test remoto. Esaminiamo come farlo.

1. Il server remoto Selenium richiede Java per funzionare. Scarica l'ultimo JDK per la tua piattaforma dalla [pagina dei download di Java SE](https://www.oracle.com/java/technologies/downloads/). Installalo quando è scaricato.
2. Successivamente, scarica l'ultimo [server standalone di Selenium](https://selenium-release.storage.googleapis.com/index.html) — questo funge da proxy tra il tuo script e i driver del browser. Scegli il numero dell'ultima versione stabile (cioè, non una beta), e dall'elenco scegli un file che inizi con "selenium-server-standalone". Quando è stato scaricato, mettilo in un posto sensato, come nella tua directory home. Se non hai già aggiunto la posizione a `PATH`, fallo ora (vedi la sezione [Impostazione di Selenium in Node](#configurazione_di_selenium_in_node)).
3. Esegui il server standalone inserendo quanto segue in un terminale sul tuo server computer

   ```bash
   java -jar selenium-server-standalone-3.0.0.jar
   ```

   (aggiorna il nome file `.jar`) in modo che corrisponda esattamente al file che hai.

4. Il server verrà eseguito su `http://localhost:4444/wd/hub` — prova a visitarci ora per vedere cosa ottieni.

Ora che abbiamo il server in esecuzione, creiamo un test demo che verrà eseguito sul server selenium remoto.

1. Crea una copia del tuo file `google_test.js`, e chiamalo `google_test_remote.js`; mettilo nella tua directory di progetto.
2. Aggiorna la linea di codice (che inizia con `const driver = …`) come segue

   ```js
   const driver = new Builder()
     .forBrowser(Browser.FIREFOX)
     .usingServer("http://localhost:4444/wd/hub")
     .build();
   ```

3. Esegui il tuo test, e dovresti vederlo eseguire come previsto; questa volta però lo eseguirai sul server standalone:

   ```bash
   node google_test_remote.js
   ```

Quindi questo è piuttosto interessante. Abbiamo testato questo localmente, ma potresti configurarlo su praticamente qualsiasi server insieme ai driver del browser rilevanti, e poi connettere i tuoi script ad esso usando l'URL che scegli di esporlo.

## Integrazione di Selenium con strumenti CI

Come altro punto, è anche possibile integrare Selenium e strumenti relativi come LambdaTest e Sauce Labs con strumenti di integrazione continua (CI) — questo è utile, poiché significa che puoi eseguire i tuoi test tramite uno strumento CI, e commettere solo nuove modifiche al tuo repository di codice se i test passano.

È fuori ambito esaminare quest'area in dettaglio in questo articolo, ma suggeriamo di iniziare con Travis CI — questo è probabilmente lo strumento CI più semplice con cui iniziare e ha buone integrazioni con strumenti web come GitHub e Node.

Per iniziare, vedi ad esempio:

- [Travis CI per principianti completi](https://docs.travis-ci.com/user/for-beginners)
- [Costruire un progetto Node.js](https://docs.travis-ci.com/user/languages/javascript-with-nodejs/) (con Travis)
- [Usare LambdaTest con Travis CI](https://www.lambdatest.com/support/docs/travis-ci-with-lambdatest/)
- [Usare LambdaTest con CircleCI](https://www.lambdatest.com/support/docs/circleci-integration-with-lambdatest/)
- [Usare LambdaTest con Jenkins](https://www.lambdatest.com/support/docs/jenkins-with-lambdatest/)
- [Usare Sauce Labs con Travis CI](https://docs.travis-ci.com/user/sauce-connect/)

> [!NOTE]
> Se desideri eseguire test continui mediante **automazione senza codice** puoi utilizzare [Endtest](https://www.endtest.io/) o [TestingBot](https://testingbot.com/).

## Riepilogo

Questo modulo dovrebbe essere stato divertente e dovrebbe averti dato abbastanza informazioni sull'argomento della scrittura e dell'esecuzione di test automatizzati per farti iniziare con la scrittura dei tuoi test automatizzati.

{{PreviousMenu("Learn_web_development/Extensions/Testing/Automated_testing", "Learn_web_development/Extensions/Testing")}}
