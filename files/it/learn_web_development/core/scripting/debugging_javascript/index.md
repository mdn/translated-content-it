---
title: Debugging e gestione degli errori in JavaScript
short-title: Debugging e gestione degli errori
slug: Learn_web_development/Core/Scripting/Debugging_JavaScript
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}

In questa lezione, ritorneremo al tema del debugging di JavaScript (che abbiamo già introdotto in [Cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)). Qui approfondiremo tecniche per tracciamento degli errori, ma anche vedremo come programmare in modo difensivo e gestire gli errori nel suo codice, evitando problemi fin dall'inizio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Utilizzare gli strumenti di sviluppo del browser per ispezionare il JavaScript in esecuzione sulla sua pagina e vedere quali errori sta generando.</li>
          <li>Utilizzare <code>console.log()</code> e <code>console.error()</code> per il debugging.</li>
          <li>Debugging avanzato di JavaScript con i devtools del browser.</li>
          <li>Gestione degli errori con <code>condizionali</code>, <code>try...catch</code> e <code>throw</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo sui tipi di errore di JavaScript

In precedenza nel modulo, in [Cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong), abbiamo osservato ampiamente i tipi di errore che possono verificarsi nei programmi JavaScript, e abbiamo detto che possono essere approssimativamente suddivisi in due tipi: errori di sintassi ed errori logici. Abbiamo anche aiutato a comprendere alcuni tipi comuni di messaggi di errore JavaScript, e le abbiamo mostrato come eseguire un semplice debugging utilizzando dichiarazioni [`console.log()`](/it/docs/Web/API/console/log_static).

In questo articolo, approfondiremo un po' gli strumenti a sua disposizione per rintracciare gli errori e osserveremo anche modi per prevenire gli errori in primo luogo.

## Linting del codice

Dovrebbe assicurarsi che il suo codice sia valido prima di cercare di rintracciare errori specifici. Utilizzi i servizi di convalida del W3C per [HTML](https://validator.w3.org/), [CSS](https://jigsaw.w3.org/css-validator/) e un linter JavaScript come [ESLint](https://eslint.org/play/) per assicurarsi che il suo codice sia valido. Ciò probabilmente farà emergere un numero di errori, permettendo di concentrarsi sugli errori rimanenti.

### Plugin per editor di codice

Non è molto conveniente dover copiare e incollare il codice in una pagina web per controllarne la validità più e più volte. Consigliamo di installare un plugin di linting nel suo editor di codice, in modo che possano essere riportati errori mentre scrive il codice. Provi a cercare ESLint nella lista dei plugin o estensioni del suo editor di codice e installarlo.

## Problemi comuni in JavaScript

Ci sono una serie di problemi comuni di JavaScript di cui dovrebbe essere consapevole, come:

- Problemi di base di sintassi e logica (ancora una volta, consulti [Risoluzione dei problemi di JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)).
- Assicurarsi che variabili, ecc. siano definite nel campo corretto e che non ci siano conflitti tra elementi dichiarati in luoghi diversi (vedi [Campo della funzione e conflitti](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)).
- Confusione su [`this`](/it/docs/Web/JavaScript/Reference/Operators/this), in termini di a quale campo si applica, e quindi se il suo valore è quello che intendeva. Può leggere [Cos'è "this"?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this) per una breve introduzione; dovrebbe anche studiare esempi come [questo](https://github.com/mdn/learning-area/blob/7ed039d17e820c93cafaff541aa65d874dde8323/javascript/oojs/assessment/main.js#L143), che mostra un pattern tipico di salvataggio di un campo `this` in una variabile separata, quindi utilizzando quella variabile in funzioni nidificate così da essere sicura di applicare la funzionalità al campo `this` corretto.
- Uso scorretto di funzioni all'interno di cicli che iterano con una variabile globale (più in generale "sbagliando il campo").

> [!CALLOUT]
> Ad esempio, in [bad-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/bad-for-loop.html) (vedi [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/bad-for-loop.html)), il ciclo passa attraverso 10 iterazioni utilizzando una variabile definita con `var`, creando ogni volta un paragrafo e aggiungendo un gestore per l'evento [onclick](/it/docs/Web/API/Element/click_event). Quando vengono cliccati, si desidera che ciascuno mostri un messaggio di avviso contenente il proprio numero (il valore di `i` al momento della creazione). Invece, tutti riportano `i` come 11, perché il ciclo `for` completa tutte le sue iterazioni prima che le funzioni nidificate vengano invocate.
>
> La soluzione più semplice è dichiarare la variabile di iterazione con `let` invece di `var`—il valore di `i` associato alla funzione è quindi unico per ogni iterazione. Vedi [good-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/good-for-loop.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/good-for-loop.html) per una versione che funziona).

- Assicurarsi che le [operazioni asincrone](/it/docs/Learn_web_development/Extensions/Async_JS) siano completate prima di tentare di utilizzare i valori che restituiscono. Di solito questo comporta la comprensione di come usare le _promesse_: utilizzando `await` in modo appropriato o eseguendo il codice per gestire il risultato di una chiamata asincrona nel gestore {{jsxref("Promise.then()", "then()")}} della promessa. Vedi [Come usare le promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per una introduzione su questo argomento.

> **Nota:** [Buggy JavaScript Code: The 10 Most Common Mistakes JavaScript Developers Make](https://www.toptal.com/javascript/10-most-common-javascript-mistakes) discute alcuni di questi errori comuni e molto altro.

## La console JavaScript del browser

Gli strumenti di sviluppo del browser hanno molte funzioni utili per aiutare a fare il debug di JavaScript. Per cominciare, la console JavaScript riporterà errori nel suo codice.

Faccia una copia locale del nostro esempio [fetch-broken](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/fetch-broken/) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-broken)).

Se guarda alla console, vedrà un messaggio di errore. La formulazione esatta dipende dal browser, ma sarà qualcosa del tipo: "Uncaught TypeError: heroes is not iterable", e il numero di riga referenziato è 25. Se guardiamo il codice sorgente, la sezione di codice rilevante è questa:

```js
function showHeroes(jsonObj) {
  const heroes = jsonObj["members"];

  for (const hero of heroes) {
    // …
  }
}
```

Quindi il codice si interrompe non appena tentiamo di usare `jsonObj` (che come ci si potrebbe aspettare, dovrebbe essere un [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)). Questo dovrebbe essere recuperato da un file `.json` esterno usando la seguente chiamata [`fetch()`](/it/docs/Web/API/Window/fetch):

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
populateHeader(response);
showHeroes(response);
```

Ma questo fallisce.

## L'API della Console

Potrebbe già sapere cosa non va in questo codice, ma esploriamolo un po' di più per mostrare come poter indagare su questo. Inizieremo con l'[API della Console](/it/docs/Web/API/console), che permette al codice JavaScript di interagire con la console JavaScript del browser. Ha un numero di funzionalità disponibili; ha già incontrato [`console.log()`](/it/docs/Web/API/console/log_static), che stampa un messaggio personalizzato sulla console.

Provi ad aggiungere una chiamata `console.log()` per registrare il valore di ritorno di `fetch()`, in questo modo:

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
console.log(`Response value: ${response}`);
populateHeader(response);
showHeroes(response);
```

Ricarichi la pagina nel browser. Questa volta, prima del messaggio di errore, vedrà un nuovo messaggio registrato sulla console:

```plain
Response value: [object Promise]
```

Il risultato di `console.log()` mostra che il valore di ritorno di `fetch()` non sono i dati JSON, è una {{jsxref("Promise")}}. La funzione `fetch()` è asincrona: restituisce una `Promise` che viene completata solo quando la risposta effettiva è stata ricevuta dalla rete. Prima di poter utilizzare la risposta, dobbiamo attendere che la `Promise` sia completata.

### `console.error()` e call stack

Come breve digressione, provi ad utilizzare un metodo di console diverso per riportare l'errore — [`console.error()`](/it/docs/Web/API/Console/error). Nel suo codice, sostituisca

```js
console.log(`Response value: ${response}`);
```

con

```js
console.error(`Response value: ${response}`);
```

Salvi il codice e aggiorni il browser e vedrà ora il messaggio riportato come un errore, con lo stesso colore e icona dell'errore non catturato sotto di esso. Inoltre, ci sarà ora una freccia espandibile accanto al messaggio. Se la preme, vedrà una singola linea che le dice la linea nel file JavaScript da cui ha avuto origine l'errore. Infatti, anche la linea dell'errore non catturato _ha_ questo, ma ha due linee:

```plain
showHeroes http://localhost:7800/js-debug-test/index.js:25
<anonymous> http://localhost:7800/js-debug-test/index.js:10
```

Questo significa che l'errore proviene dalla funzione `showHeroes()`, linea 25, come abbiamo notato in precedenza. Se guarda il suo codice, vedrà che la chiamata anonima alla linea 10 è la linea che sta chiamando `showHeroes()`. Queste linee sono chiamate **call stack**, e possono essere davvero utili quando si cerca di rintracciare l'origine di un errore che coinvolge diversi luoghi nel suo codice.

La chiamata `console.error()` non è così utile in questo caso, ma può essere utile per generare un call stack se non è già disponibile.

### Correzione dell'errore

Ad ogni modo, torniamo a provare a correggere il nostro errore. Possiamo accedere alla risposta della `Promise` completata concatenando il metodo {{jsxref("Promise.prototype.then()", "then()")}} alla fine della chiamata `fetch()`. Possiamo quindi passare il valore di risposta risultante nelle funzioni che lo accettano, in questo modo:

```js
fetch(requestURL).then((response) => {
  populateHeader(response);
  showHeroes(response);
});
```

Salvi e aggiorni, e verifichi se il suo codice funziona. Spoiler alert — la modifica sopra non ha risolto il problema. Sfortunatamente, **abbiamo ancora lo stesso errore**!

> [!NOTE]
> In sintesi, ogni volta che qualcosa non funziona e un valore non sembra essere quello che dovrebbe essere in un certo punto del suo codice, può usare `console.log()`, `console.error()`, o un'altra funzione simile per stampare il valore e vedere cosa sta succedendo.

## Uso del debugger JavaScript

Indaghiamo su questo problema ulteriormente utilizzando una funzione più sofisticata degli strumenti per sviluppatori del browser: il [debugger JavaScript](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html) come viene chiamato in Firefox.

> [!NOTE]
> Strumenti simili sono disponibili in altri browser; la [tab Sources](https://developer.chrome.com/docs/devtools/#sources) in Chrome, Debugger in Safari (vedi [Safari Web Development Tools](https://developer.apple.com/safari/tools/)), ecc.

In Firefox, la tab Debugger appare in questo modo:

![Debugger di Firefox](debugger-tab.png)

- A sinistra, può selezionare lo script che desidera eseguire il debug (in questo caso ne abbiamo solo uno).
- Il pannello centrale mostra il codice nello script selezionato.
- Il pannello a destra mostra dettagli utili relativi all'ambiente corrente — _Breakpoints_, _Callstack_ e _Scopes_ attivi.

La caratteristica principale di tali strumenti è la capacità di aggiungere breakpoints al codice — questi sono i punti in cui l'esecuzione del codice si ferma, e a quel punto può esaminare l'ambiente nel suo stato attuale e vedere cosa sta succedendo.

Esploriamo l'uso dei breakpoints:

1. L'errore viene generato alla stessa linea di prima — `for (const hero of heroes) {` — linea 26 nello screenshot qui sotto. Faccia clic su questa linea nel pannello centrale per aggiungere un breakpoint a essa (vedrà una freccia blu apparire sopra di essa).
2. Ora aggiorni la pagina (<kbd>Cmd</kbd>/<kbd>Ctrl</kbd> + <kbd>R</kbd>) — il browser fermerà l'esecuzione del codice su quella linea. A questo punto, il lato destro si aggiornerà per mostrare quanto segue:

![Debugger di Firefox con un breakpoint](breakpoint.png)

- Sotto _Breakpoints_, vedrà i dettagli del breakpoint che ha impostato.
- Sotto _Call Stack_, vedrà alcune voci — questo è fondamentalmente lo stesso del callstack che abbiamo esaminato in precedenza nella sezione `console.error()`. _Call Stack_ mostra un elenco delle funzioni che sono state invocate per causare l'invocazione della funzione corrente. In cima, abbiamo `showHeroes()`, la funzione in cui ci troviamo attualmente, e in secondo luogo abbiamo `onload`, che memorizza la funzione gestore eventi contenente la chiamata a `showHeroes()`.
- Sotto _Scopes_, vedrà il campo attualmente attivo per la funzione che stiamo osservando. Ne abbiamo solo tre — `showHeroes`, `block`, e `Window` (il campo globale). Ogni campo può essere espanso per mostrare i valori delle variabili all'interno del campo quando l'esecuzione del codice è stata interrotta.

Possiamo trovare alcune informazioni molto utili qui:

1. Espanda il campo `showHeroes` — può vedere da ciò che la variabile heroes è `undefined`, indicando che l'accesso alla proprietà `members` di `jsonObj` (prima linea della funzione) non ha funzionato.
2. Può anche vedere che la variabile `jsonObj` sta memorizzando un oggetto [`Response`](/it/docs/Web/API/Response), non un oggetto JSON.

L'argomento di `showHeroes()` è il valore con cui la promessa `fetch()` è stata soddisfatta. Quindi questa promessa non è in formato JSON: è un oggetto `Response`. C'è un passaggio in più necessario per recuperare il contenuto della risposta come un oggetto JSON.

Le chiediamo di provare a risolvere questo problema da solo. Per aiutarla a partire, veda la documentazione per l'oggetto [`Response`](/it/docs/Web/API/Response). Se resta bloccato, può trovare il codice sorgente corretto su <https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-fixed>.

> [!NOTE]
> La tab Debugger ha molte altre utili funzionalità che non abbiamo discusso qui, ad esempio breakpoints condizionali ed espressioni di osservazione. Per molte più informazioni, veda la pagina [Debugger](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html).

## Gestione degli errori JavaScript nel codice

HTML e CSS sono permissivi — errori e funzionalità non riconosciute possono spesso essere gestiti a causa della natura dei linguaggi. Ad esempio, CSS ignorerà le proprietà non riconosciute, e il resto del codice funzionerà spesso. Tuttavia, JavaScript non è permissivo come HTML e CSS — se il motore JavaScript incontra errori o sintassi non riconosciuta, spesso genererà errori.

Esploriamo una strategia comune per gestire gli errori JavaScript nel suo codice. Le sezioni seguenti sono progettate per essere seguite creando una copia del seguente file template come `handling-errors.html` sul suo computer locale, aggiungendo le parti di codice tra i tag `<script>` e `</script>`, quindi aprendo il file in un browser e guardando l'output nella console JavaScript dei devtools.

```html-nolint
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>Handling JS errors</title>
  </head>
  <body>
    <script>
      // Code goes below this line

    </script>
  </body>
</html>
```

### Condizionali

Un uso comune dei [condizionali JavaScript](/it/docs/Learn_web_development/Core/Scripting/Conditionals) è gestire gli errori. I condizionali le consentono di eseguire diversi codici a seconda del valore di una variabile. Spesso vorrà usarli in modo difensivo, per evitare di generare un errore se il valore non esiste o è del tipo sbagliato, o per catturare un errore se il valore causerebbe un risultato errato da restituire, il quale potrebbe causare problemi successivamente.

Vediamo un esempio. Supponiamo di avere una funzione che prende come argomento un valore pari all'altezza dell'utente in pollici e restituisce la sua altezza in metri, a 2 cifre decimali. Potrebbe apparire così:

```js
function inchesToMeters(num) {
  const mVal = (num * 2.54) / 100;
  const m2dp = mVal.toFixed(2);
  return m2dp;
}
```

1. Nell'elemento `<script>` del suo file esempio, dichiari un `const` chiamato `height` e gli assegni un valore di `70`:

   ```js
   const height = 70;
   ```

2. Copi la funzione sopra sotto la linea precedente.

3. Chiami la funzione, passando il costante `height` come argomento, e registri il valore restituito alla console:

   ```js
   console.log(inchesToMeters(height));
   ```

4. Carichi l'esempio in un browser e osservi la console JavaScript dei devtools. Dovrebbe vedere un valore di `1.78` registrato.

5. Quindi questo funziona bene in isolamento. Ma cosa succede se i dati forniti mancano o non sono corretti? Provi questi scenari:

   - Se cambia il valore `height` in `"70"` (cioè `70` espresso come stringa), l'esempio dovrebbe ... funzionare comunque. Questo perché il calcolo sulla prima linea della stringa forza coerentemente il valore in un tipo di dati numero. Questo va bene in un caso semplice come questo, ma in un codice più complesso i dati errati possono portare a tutti i tipi di bug, alcuni dei quali sottili e difficili da individuare!
   - Se cambia `height` in un valore che non può essere forzato a numero, come `"70 inches"` o `["Bob", 70]`, o {{jsxref("NaN")}}, l'esempio dovrebbe restituire il risultato come `NaN`. Questo potrebbe causare tutti i tipi di problemi, ad esempio se vuole includere l'altezza dell'utente da qualche parte nell'interfaccia utente del sito.
   - Se rimuove completamente il valore `height` (commento mettendo `//` all'inizio della linea), la console mostrerà un errore del tipo "Uncaught ReferenceError: height is not defined", il quale potrebbe bloccare il suo programma.

   Ovviamente, nessuno di questi risultati è eccezionale. Come difendiamo dai dati errati?

6. Aggiungiamo un condizionale all'interno della nostra funzione per verificare se i dati sono buoni prima di provare a fare il calcolo. Provi a sostituire la sua funzione attuale con la seguente:

   ```js
   function inchesToMeters(num) {
     if (typeof num === "number" && !isNaN(num)) {
       const mVal = (num * 2.54) / 100;
       const m2dp = mVal.toFixed(2);
       return m2dp;
     } else {
       console.log("A number was not provided. Please correct the input.");
     }
   }
   ```

7. Ora se riprova i primi due scenari, vedrà il nostro messaggio leggermente più utile restituito, dandole un'idea di ciò che deve essere fatto per risolvere il problema. Potrebbe mettere qualsiasi cosa lì dentro che vuole, inclusa l'idea di eseguire codice per correggere il valore di `num`, ma questo non è consigliato — questa funzione ha uno scopo semplice e dovrebbe gestire la correzione del valore altrove nel sistema.

   > [!NOTE]
   > Nella dichiarazione `if()`, testiamo prima se il tipo di dati di `num` è `"number"` usando l'operatore [`typeof`](/it/docs/Web/JavaScript/Reference/Operators/typeof), ma testiamo anche se {{jsxref("isNaN()", "!isNaN(num)")}} restituisce `false`. Dobbiamo farlo per difenderci dal caso specifico in cui `num` è impostato a `NaN`, poiché stranamente `typeof NaN` restituisce `"number"`!

8. Tuttavia, se prova di nuovo il terzo scenario, riceverà ancora l'errore "Uncaught ReferenceError: height is not defined". Non può risolvere il fatto che un valore non sia disponibile dall'interno di una funzione che sta cercando di utilizzare il valore.

Come gestiamo questo? Beh, è probabilmente meglio far sì che la nostra funzione restituisca un errore personalizzato quando non riceve i dati corretti. Vedremo come farlo prima, poi gestiremo tutti gli errori insieme.

### Lancio di errori personalizzati

Può lanciare un errore personalizzato in qualsiasi punto del suo codice usando l'istruzione [`throw`](/it/docs/Web/JavaScript/Reference/Statements/throw), accoppiata con il costruttore {{jsxref("Error.Error", "Error()")}}. Vediamolo in azione.

1. Nella sua funzione, sostituisca la linea `console.log()` all'interno del blocco `else` della sua funzione con la seguente linea:

   ```js
   throw new Error("A number was not provided. Please correct the input.");
   ```

2. Esegua nuovamente l'esempio, ma si assicuri che `num` sia impostato su un valore errato (cioè non numerico). Questa volta, dovrebbe vedere il suo errore personalizzato lanciato, insieme a un utile call stack per aiutarla a individuare la sorgente dell'errore (anche se noti che il messaggio dice ancora che l'errore è "non catturato", o "non gestito"). OK, quindi gli errori sono fastidiosi, ma questo è molto più utile del successo nell'esecuzione della funzione e nella restituzione di un valore non numerico che potrebbe causare problemi successivamente.

Quindi, come gestiamo tutti questi errori?

### try...catch

L'istruzione [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) è appositamente progettata per gestire gli errori. Ha la seguente struttura:

```js
try {
  // Run some code
} catch (error) {
  // Handle any errors
}
```

All'interno del blocco `try`, prova a eseguire un po' di codice. Se questo codice viene eseguito senza che venga lanciato un errore, va tutto bene, e il blocco `catch` viene ignorato. Tuttavia, se viene lanciato un errore, il blocco `catch` viene eseguito, il quale fornisce accesso all'oggetto {{jsxref("Error")}} che rappresenta l'errore e permette di eseguire il codice per gestirlo.

Usiamo `try...catch` nel nostro codice.

1. Sostituisca la linea `console.log()` che chiama la funzione `inchesToMeters()` alla fine del suo script con il seguente blocco. Ora stiamo eseguendo la nostra linea `console.log()` all'interno di un blocco `try`, e gestendo eventuali errori che restituisce all'interno di un blocco `catch` corrispondente.

   ```js
   try {
     console.log(inchesToMeters(height));
   } catch (error) {
     console.error(error);
     console.log("Insert code to handle the error");
   }
   ```

2. Salvi e aggiorni, e ora dovrebbero comparire due cose:

   - Il messaggio di errore e il call stack come prima, ma questa volta, senza etichetta di "non catturato", o "non gestito".
   - Il messaggio registrato "Inserire codice per gestire l'errore".

3. Ora provi ad aggiornare `num` a un valore valido (numero), e vedrà il risultato del calcolo registrato, senza messaggio di errore.

Questo è significativo — eventuali errori generati non sono più non gestiti, così non metteranno in crisi l'applicazione. Può eseguire qualsiasi codice desideri per gestire l'errore. Sopra stiamo semplicemente registrando un messaggio, ma ad esempio potrebbe chiamare qualsiasi funzione eseguita in precedenza per chiedere all'utente di inserire la loro altezza, questa volta chiedendogli di correggere l'errore di input. Potrebbe anche usare un'istruzione `if...else` per eseguire codice di gestione degli errori diverso a seconda del tipo di errore restituito.

### Rilevamento delle funzionalità

Il rilevamento delle funzionalità è utile quando si prevede di utilizzare nuove funzionalità di JavaScript che potrebbero non essere supportate in tutti i browser. Testi la funzionalità, quindi esegua il codice condizionalmente per fornire un'esperienza accettabile sia nei browser che supportano la funzionalità sia in quelli che non la supportano. Come esempio rapido, l'[API di geolocalizzazione](/it/docs/Web/API/Geolocation_API) (che espone i dati di posizione disponibili per il dispositivo su cui è in esecuzione il browser web) ha un punto di ingresso principale per il suo uso — una proprietà `geolocation` disponibile sull'oggetto globale [Navigator](/it/docs/Web/API/Navigator). Pertanto, può rilevare se il browser supporta o meno la geolocalizzazione utilizzando una struttura `if()` simile a quella che abbiamo visto in precedenza:

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition((position) => {
    // show the location on a map, perhaps using the Google Maps API
  });
} else {
  // Give the user a choice of static maps instead
}
```

Può trovare alcuni altri esempi di rilevamento delle funzionalità in [Alternative al riconoscimento dell'UA](/it/docs/Web/HTTP/Guides/Browser_detection_using_the_user_agent#alternatives_to_ua_sniffing).

## Trovare aiuto

Ci sono molti altri problemi con cui si imbatterà con JavaScript (e HTML e CSS!), rendendo la conoscenza di come trovare risposte online inestimabile.

Tra le migliori fonti di informazioni di supporto ci sono MDN (è proprio qui!), [stackoverflow.com](https://stackoverflow.com/) e [caniuse.com](https://caniuse.com/).

- Per utilizzare la rete degli sviluppatori Mozilla (MDN), la maggior parte delle persone cerca con un motore di ricerca la tecnologia su cui cercano informazioni, più il termine "mdn", ad esempio, "mdn HTML video".
- [caniuse.com](https://caniuse.com/) fornisce informazioni di supporto, insieme ad alcuni utili link a risorse esterne. Ad esempio, veda <https://caniuse.com/#search=video> (basta inserire la funzione che si sta cercando nella casella di testo).
- [stackoverflow.com](https://stackoverflow.com/) (SO) è un sito di forum dove può fare domande e avere altri sviluppatori che condividono le loro soluzioni, cercare post precedenti, e aiutare altri sviluppatori. Le viene consigliato di cercare e vedere se esiste già una risposta alla sua domanda, prima di postare una nuova domanda. Ad esempio, abbiamo cercato "disabilitare autofocus su dialogo HTML" su SO, e molto velocemente abbiamo trovato [Disable showModal auto-focusing using HTML attributes](https://stackoverflow.com/questions/63267581/disable-showmodal-auto-focusing-using-html-attributes).

A parte questo, provi a cercare sul suo motore di ricerca preferito una risposta al suo problema. È spesso utile cercare messaggi di errore specifici se li ha — altri sviluppatori avranno probabilmente avuto gli stessi problemi.

## Sommario

Quindi questo è il debugging e la gestione degli errori in JavaScript. Semplice, vero? Forse non così semplice, ma questo articolo dovrebbe almeno darle un inizio e alcune idee su come affrontare i problemi relativi a JavaScript che incontrerà.

Questo è tutto per il modulo Scripting dinamico con JavaScript; congratulazioni per essere arrivato alla fine! Nel prossimo modulo l'aiuteremo a esplorare i framework e le librerie JavaScript.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}
