---
title: Debugging e gestione degli errori in JavaScript
short-title: Debugging e gestione degli errori
slug: Learn_web_development/Core/Scripting/Debugging_JavaScript
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}

In questa lezione, torneremo sul tema del debugging di JavaScript (che abbiamo affrontato per la prima volta in [Che cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)). Qui approfondiremo le tecniche per rintracciare gli errori, ma esamineremo anche come scrivere codice in modo difensivo e gestire gli errori nel proprio codice, evitando problemi fin dall'inizio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Utilizzare gli strumenti per sviluppatori del browser per ispezionare il JavaScript eseguito sulla propria pagina e vedere quali errori genera.</li>
          <li>Utilizzare <code>console.log()</code> e <code>console.error()</code> per il debugging.</li>
          <li>Debugging avanzato di JavaScript con gli strumenti di sviluppo del browser.</li>
          <li>Gestione degli errori con <code>conditionals</code>, <code>try...catch</code> e <code>throw</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo sui tipi di errori in JavaScript

In precedenza nel modulo, in [Che cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong), abbiamo esaminato ampiamente i tipi di errori che possono verificarsi nei programmi JavaScript e abbiamo detto che possono essere suddivisi all'incirca in due tipi: errori di sintassi e errori logici. Ti abbiamo anche aiutato a comprendere alcuni tipi comuni di messaggi di errore JavaScript e ti abbiamo mostrato come eseguire un semplice debugging usando le dichiarazioni [`console.log()`](/it/docs/Web/API/console/log_static).

In questo articolo, approfondiremo gli strumenti che hai a disposizione per rintracciare gli errori e vedremo anche modi per prevenire gli errori fin dall'inizio.

## Linterilizzare il tuo codice

Dovresti assicurarti che il tuo codice sia valido prima di cercare di rintracciare errori specifici. Usa il [servizio di validazione del markup del W3C](https://validator.w3.org/), il [servizio di validazione CSS](https://jigsaw.w3.org/css-validator/) e un linter JavaScript come [ESLint](https://eslint.org/play/) per assicurarti che il tuo codice sia valido. Questo permetterà probabilmente di scoprire una serie di errori, permettendoti di concentrarti sugli errori che rimangono.

### Plugin per editor di codice

Non è molto conveniente dover copiare e incollare il codice su una pagina web per controllarne la validità più volte. Ti consiglieremmo di installare un plugin linter sul tuo editor di codice, in modo da poter ricevere rapporti sugli errori mentre scrivi il codice. Prova a cercare ESLint nell'elenco di plugin o estensioni del tuo editor di codice e installalo.

## Problemi comuni di JavaScript

Ci sono diversi problemi comuni di JavaScript di cui devi essere consapevole, come:

- Problemi di sintassi e logica di base (ancora, dai un'occhiata a [Risoluzione dei problemi di JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)).
- Assicurarsi che le variabili, ecc., siano definite nello scope corretto e che non ci siano conflitti tra elementi dichiarati in posti diversi (vedi [Scope delle funzioni e conflitti](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)).
- Confusione riguardo a [`this`](/it/docs/Web/JavaScript/Reference/Operators/this), in termini di quale scope si applica e quindi se il suo valore è quello previsto. Puoi leggere [Cos'è "this"?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this) per un'introduzione leggera; dovresti anche studiare esempi come [questo](https://github.com/mdn/learning-area/blob/7ed039d17e820c93cafaff541aa65d874dde8323/javascript/oojs/assessment/main.js#L143), che mostra un tipico modello di salvataggio di uno scope `this` in una variabile separata, quindi utilizzando quella variabile in funzioni annidate per essere sicuri di applicare la funzionalità al corretto scope `this`.
- Uso scorretto di funzioni all'interno di cicli che iterano con una variabile globale (più in generale "sbagliare lo scope").

> [!CALLOUT]
> Ad esempio, in [bad-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/bad-for-loop.html) (vedi [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/bad-for-loop.html)), eseguiamo un ciclo di 10 iterazioni usando una variabile definita con `var`, creando ogni volta un paragrafo e aggiungendo un gestore eventi [onclick](/it/docs/Web/API/Element/click_event) ad esso. Quando cliccato, vogliamo che ciascuno mostri un messaggio di avviso contenente il suo numero (il valore di `i` al momento della sua creazione). Invece tutti riportano `i` come 11 — perché il ciclo `for` esegue tutte le sue iterazioni prima che le funzioni annidate vengano invocate.
>
> La soluzione più semplice è dichiarare la variabile di iterazione con `let` anziché `var`—il valore di `i` associato alla funzione è quindi unico per ciascuna iterazione. Vedi [good-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/good-for-loop.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/good-for-loop.html)) per una versione che funziona.

- Assicurarsi che le [operazioni asincrone](/it/docs/Learn_web_development/Extensions/Async_JS) siano completate prima di tentare di usare i valori che restituiscono. Questo di solito significa comprendere come usare _promises_: usando correttamente [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) o eseguendo il codice per gestire il risultato di una chiamata asincrona nel gestore {{jsxref("Promise.then()", "then()")}} della promise. Vedi [Come usare le promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per un'introduzione a questo argomento.

> **Nota:** [Buggy JavaScript Code: The 10 Most Common Mistakes JavaScript Developers Make](https://www.toptal.com/javascript/10-most-common-javascript-mistakes) offre alcune belle discussioni su questi errori comuni e altro.

## La console JavaScript del browser

Gli strumenti per sviluppatori del browser hanno molte funzionalità utili per aiutare a eseguire il debugging di JavaScript. Per cominciare, la console JavaScript riporterà errori nel tuo codice.

Fai una copia locale del nostro esempio [fetch-broken](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/fetch-broken/) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-broken)).

Se guardi la console, vedrai un messaggio di errore. La formulazione esatta dipende dal browser, ma sarà qualcosa come: "Uncaught TypeError: heroes is not iterable", e il numero di riga di riferimento è 25. Se guardiamo il codice sorgente, la sezione di codice rilevante è questa:

```js
function showHeroes(jsonObj) {
  const heroes = jsonObj["members"];

  for (const hero of heroes) {
    // …
  }
}
```

Quindi il codice va in errore non appena proviamo a utilizzare `jsonObj` (che come ci si potrebbe aspettare, dovrebbe essere un [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)). Questo dovrebbe essere recuperato da un file `.json` esterno usando la seguente chiamata [`fetch()`](/it/docs/Web/API/Window/fetch):

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
populateHeader(response);
showHeroes(response);
```

Ma questo fallisce.

## L'API console

Potresti già sapere cosa c'è che non va nel codice, ma esploriamolo un po' di più per mostrare come potresti indagare su di esso. Iniziamo con l'[API console](/it/docs/Web/API/console), che permette al codice JavaScript di interagire con la console JavaScript del browser. Ha un certo numero di funzionalità disponibili; hai già incontrato [`console.log()`](/it/docs/Web/API/console/log_static), che stampa un messaggio personalizzato nella console.

Prova ad aggiungere una chiamata a `console.log()` per registrare il valore di ritorno di `fetch()`, come questo:

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
console.log(`Response value: ${response}`);
populateHeader(response);
showHeroes(response);
```

Aggiorna la pagina nel browser. Questa volta, prima del messaggio di errore, vedrai un nuovo messaggio registrato nella console:

```plain
Response value: [object Promise]
```

L'output di `console.log()` mostra che il valore di ritorno di `fetch()` non è il dato JSON, è una {{jsxref("Promise")}}. La funzione `fetch()` è asincrona: restituisce una `Promise` che viene soddisfatta solo quando la risposta effettiva è stata ricevuta dalla rete. Prima di poter utilizzare la risposta, dobbiamo aspettare che la `Promise` sia soddisfatta.

### `console.error()` e stack di chiamate

Come un breve esame a parte, proviamo a usare un metodo console diverso per segnalare l'errore — [`console.error()`](/it/docs/Web/API/Console/error). Nel tuo codice, sostituisci

```js
console.log(`Response value: ${response}`);
```

con

```js
console.error(`Response value: ${response}`);
```

Salva il tuo codice e aggiorna il browser e ora vedrai il messaggio segnalato come errore, con lo stesso colore e icona come l'errore non gestito al di sotto. Inoltre, ora ci sarà una freccia di espansione/compressione accanto al messaggio. Se premi questo, vedrai una singola linea che ti dice la linea nel file JavaScript da cui l'errore ha origine. Infatti, anche la linea dell'errore non gestito _ha_ questo, ma ha due linee:

```plain
showHeroes http://localhost:7800/js-debug-test/index.js:25
<anonymous> http://localhost:7800/js-debug-test/index.js:10
```

Questo significa che l'errore proviene dalla funzione `showHeroes()` alla linea 25, come abbiamo notato prima. Se guardi il tuo codice, vedrai che la chiamata anonima alla linea 10 è la linea che sta chiamando `showHeroes()`. Queste linee sono definite **call stack**, e possono essere davvero utili quando si cerca di rintracciare l'origine di un errore che coinvolge diverse posizioni nel tuo codice.

La chiamata `console.error()` non è molto utile in questo caso, ma può essere utile per generare uno stack di chiamate se uno non è già disponibile.

### Correzione dell'errore

Comunque, torniamo a cercare di correggere il nostro errore. Possiamo accedere alla risposta dalla `Promise` soddisfatta concatenando il metodo {{jsxref("Promise.prototype.then()", "then()")}} alla fine della chiamata `fetch()`. Possiamo quindi passare il valore di risposta risultante alle funzioni che lo accettano, in questo modo:

```js
fetch(requestURL).then((response) => {
  populateHeader(response);
  showHeroes(response);
});
```

Salva e aggiorna, e vedi se il tuo codice funziona. Spoiler alert — la modifica sopra non ha corretto il problema. Purtroppo, abbiamo **ancora lo stesso errore**!

> [!NOTE]
> Riassumendo, ogni volta che qualcosa non funziona e un valore non sembra essere come dovrebbe a un certo punto nel tuo codice, puoi usare `console.log()`, `console.error()`, o un'altra funzione simile per stampare il valore e vedere cosa sta succedendo.

## Utilizzo del debugger JavaScript

Indaghiamo ulteriormente su questo problema utilizzando una funzionalità più sofisticata degli strumenti per sviluppatori del browser: il [debugger JavaScript](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html) così come viene chiamato in Firefox.

> [!NOTE]
> Strumenti simili sono disponibili in altri browser; la scheda [Sources](https://developer.chrome.com/docs/devtools/#sources) in Chrome, Debugger in Safari (vedi [Strumenti di sviluppo di Safari](https://developer.apple.com/safari/tools/)), ecc.

In Firefox, la scheda Debugger appare così:

![Debug di Firefox](debugger-tab.png)

- A sinistra, puoi selezionare lo script che vuoi debuggare (in questo caso ne abbiamo solo uno).
- Il pannello centrale mostra il codice nello script selezionato.
- Il pannello di destra mostra dettagli utili relativi all'ambiente corrente — _Breakpoints_, _Callstack_ e _Scopes_ attivi.

La caratteristica principale di tali strumenti è la possibilità di aggiungere breakpoints nel codice — questi sono punti in cui l'esecuzione del codice si ferma, e a quel punto puoi esaminare l'ambiente nel suo stato attuale e vedere cosa sta succedendo.

Esploriamo l'uso dei breakpoints:

1. L'errore viene lanciato sulla stessa riga di prima — `for (const hero of heroes) {` — linea 26 nello screenshot qui sotto. Clicca su questa linea nel pannello centrale per aggiungere un breakpoint ad essa (vedrai una freccia blu apparire sopra di essa).
2. Ora aggiorna la pagina (<kbd>Cmd</kbd>/<kbd>Ctrl</kbd> + <kbd>R</kbd>) — il browser interromperà l'esecuzione del codice su quella linea. A questo punto, il lato destro si aggiornerà per mostrare quanto segue:

![Debug di Firefox con un breakpoint](breakpoint.png)

- Sotto _Breakpoints_, vedrai i dettagli del breakpoint impostato.
- Sotto _Call Stack_, vedrai alcune voci — questo è fondamentalmente lo stesso dello stack di chiamate che abbiamo esaminato prima nella sezione `console.error()`. _Call Stack_ mostra un elenco delle funzioni che sono state invocate per causare l'invocazione della funzione attuale. In alto, abbiamo `showHeroes()`, la funzione in cui ci troviamo attualmente, e al secondo abbiamo `onload`, che memorizza la funzione gestore eventi contenente la chiamata a `showHeroes()`.
- Sotto _Scopes_, vedrai lo scope attualmente attivo per la funzione che stiamo esaminando. Ne abbiamo solo tre — `showHeroes`, `block`, e `Window` (lo scope globale). Ogni scope può essere espanso per mostrare i valori delle variabili all'interno dello scope quando l'esecuzione del codice è stata fermata.

Possiamo scoprire alcune informazioni molto utili qui:

1. Espandi lo scope `showHeroes` — puoi vedere da questo che la variabile heroes è `undefined`, indicando che l'accesso alla proprietà `members` di `jsonObj` (prima linea della funzione) non ha funzionato.
2. Puoi anche vedere che la variabile `jsonObj` sta memorizzando un oggetto [`Response`](/it/docs/Web/API/Response), non un oggetto JSON.

L'argomento di `showHeroes()` è il valore con cui la promise `fetch()` è stata soddisfatta. Quindi questa promise non è nel formato JSON: è un oggetto `Response`. C'è un passaggio aggiuntivo necessario per recuperare il contenuto della risposta come un oggetto JSON.

Vorremmo che provassi a risolvere questo problema da solo. Per aiutarti a iniziare, consulta la documentazione per l'oggetto [`Response`](/it/docs/Web/API/Response). Se ti blocchi, puoi trovare il codice sorgente corretto su <https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-fixed>.

> [!NOTE]
> La scheda debugger ha molte altre funzioni utili che non abbiamo discusso qui, ad esempio breakpoint condizionali ed espressioni di watch. Per molte più informazioni, consulta la pagina [Debugger](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html).

## Gestione degli errori JavaScript nel tuo codice

HTML e CSS sono permissivi — errori e caratteristiche non riconosciute possono spesso essere gestiti a causa della natura dei linguaggi. Ad esempio, CSS ignorerà le proprietà non riconosciute, e il resto del codice funzionerà spesso e volentieri. Tuttavia, JavaScript non è permissivo come HTML e CSS — se il motore JavaScript incontra errori o sintassi non riconosciuta, lancerà spesso errori.

Esploriamo una strategia comune per gestire gli errori JavaScript nel tuo codice. Le sezioni seguenti sono progettate per essere seguite creando una copia del file template qui sotto come `handling-errors.html` sulla tua macchina locale, aggiungendo i frammenti di codice tra i tag di apertura e chiusura `<script>` e `</script>`, quindi aprendo il file in un browser e guardando l'output nella console JavaScript degli strumenti di sviluppo.

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

Un uso comune dei [condizionali JavaScript](/it/docs/Learn_web_development/Core/Scripting/Conditionals) è per la gestione degli errori. I condizionali permettono di eseguire codice diverso a seconda del valore di una variabile. Spesso vorrai usare questo in modo difensivo, per evitare di lanciare un errore se il valore non esiste o è del tipo sbagliato, o per catturare un errore se il valore causerebbe un ritorno di risultato errato, che potrebbe causare problemi successivamente.

Vediamo un esempio. Supponiamo di avere una funzione che prende come argomento l'altezza dell'utente in pollici e restituisce la sua altezza in metri, a 2 decimali. Questo potrebbe apparire così:

```js
function inchesToMeters(num) {
  const mVal = (num * 2.54) / 100;
  const m2dp = mVal.toFixed(2);
  return m2dp;
}
```

1. Nel tuo esempio di `<script>`, dichiara un `const` chiamato `height` e assegnagli un valore di `70`:

   ```js
   const height = 70;
   ```

2. Copia la funzione sopra sotto linea precedente.

3. Chiama la funzione, passandogli il costante `height` come argomento, e registra il valore di ritorno nella console:

   ```js
   console.log(inchesToMeters(height));
   ```

4. Carica l'esempio in un browser e guarda la console JavaScript degli strumenti di sviluppo. Dovresti vedere un valore di `1.78` registrato su di essa.

5. Quindi questo funziona bene in isolamento. Ma cosa succede se i dati forniti sono mancanti o non corretti? Prova questi scenari:

   - Se cambi il valore di `height` a `"70"` (cioè, `70` espresso come una stringa), l'esempio dovrebbe ... funzionare ancora bene. Questo perché il calcolo sulla prima linea della stringa forza il valore in un tipo di dati numero. Questo va bene in un caso semplice come questo, ma in un codice più complesso, i dati sbagliati possono portare a tutti i tipi di bug, alcuni dei quali sottili e difficili da rilevare!
   - Se cambi `height` con un valore che non può essere forzato in un numero, come `"70 inches"` o `["Bob", 70]`, o {{jsxref("NaN")}}, l'esempio dovrebbe restituire il risultato come `NaN`. Questo potrebbe causare tutti i tipi di problemi, ad esempio se vuoi includere l'altezza dell'utente da qualche parte nell'interfaccia utente del sito web.
   - Se rimuovi del tutto il valore di `height` (commentando la riga con `//` all'inizio), la console mostrerà un errore simile a "Uncaught ReferenceError: height is not defined", del tipo che potrebbe bloccare completamente la tua applicazione.

   Ovviamente, nessuna di queste soluzioni è ottimale. Come possiamo difenderci dai dati errati?

6. Aggiungiamo un condizionale all'interno della nostra funzione per testare se i dati sono corretti prima di tentare di fare il calcolo. Prova a sostituire la tua funzione attuale con la seguente:

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

7. Ora, se provi di nuovo i primi due scenari, vedrai il nostro messaggio leggermente più utile restituito, per darti un'idea di cosa bisogna fare per risolvere il problema. Potresti mettere qualsiasi cosa lì che ti piace, incluso tentare di eseguire codice per correggere il valore di `num`, ma questo non è raccomandato — questa funzione ha uno scopo semplice, e dovresti gestire la correzione del valore da qualche altra parte nel sistema.

   > [!NOTE]
   > Nella dichiarazione `if()`, prima testiamo se il tipo di dati di `num` è `"number"` usando l'operatore [`typeof`](/it/docs/Web/JavaScript/Reference/Operators/typeof), ma testiamo anche se {{jsxref("isNaN()", "!isNaN(num)")}} restituisce `false`. Dobbiamo fare questo per difendere contro il caso specifico in cui `num` sia impostato su `NaN`, poiché, stranamente, `typeof NaN` restituisce `"number"`!

8. Tuttavia, se riprovi il terzo scenario, vedrai ancora l'errore "Uncaught ReferenceError: height is not defined" lanciato verso di te. Non puoi correggere il fatto che un valore non sia disponibile dall'interno di una funzione che sta tentando di usare il valore.

Come gestiamo questo? Bene, è probabilmente meglio che la nostra funzione restituisca un errore personalizzato quando non riceve i dati corretti. Vedremo come farlo per primo, poi gestiremo tutti gli errori insieme.

### Lanciare errori personalizzati

Puoi generare un errore personalizzato in qualsiasi punto del tuo codice usando l'istruzione [`throw`](/it/docs/Web/JavaScript/Reference/Statements/throw), associata al costruttore {{jsxref("Error.Error", "Error()")}}. Vediamo questo in azione.

1. Nella tua funzione, sostituisci la linea `console.log()` all'interno del blocco `else` della tua funzione con la seguente linea:

   ```js
   throw new Error("A number was not provided. Please correct the input.");
   ```

2. Esegui di nuovo l'esempio, ma assicurati che `num` sia impostato su un valore errato (cioè, non numerico). Questa volta, dovresti vedere il tuo errore personalizzato lanciato, insieme ad uno stack di chiamate utile per aiutarti a localizzare l'origine dell'errore (anche se nota che il messaggio dice ancora che l'errore è "non gestito", o "non gestito"). OK, quindi gli errori sono fastidiosi, ma questo è molto più utile rispetto a eseguire la funzione con successo e restituire un valore non numerico che potrebbe causare problemi successivamente.

Allora, come gestiamo poi tutti quegli errori?

### try...catch

L'istruzione [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) è progettata appositamente per gestire gli errori. Ha la seguente struttura:

```js
try {
  // Run some code
} catch (error) {
  // Handle any errors
}
```

All'interno del blocco `try`, provi a eseguire del codice. Se questo codice viene eseguito senza che venga lanciato nessun errore, tutto va bene, e il blocco `catch` viene ignorato. Tuttavia, se un errore viene lanciato, il blocco `catch` viene eseguito, permettendoti di accedere all'oggetto {{jsxref("Error")}} che rappresenta l'errore e permettendoti di eseguire codice per gestirlo.

Usiamo `try...catch` nel nostro codice.

1. Sostituisci la riga `console.log()` che chiama la funzione `inchesToMeters()` alla fine del tuo script con il seguente blocco. Ora stiamo eseguendo la nostra linea `console.log()` all'interno di un blocco `try`, e gestendo qualsiasi errore venga restituito all'interno di un blocco `catch` corrispondente.

   ```js
   try {
     console.log(inchesToMeters(height));
   } catch (error) {
     console.error(error);
     console.log("Insert code to handle the error");
   }
   ```

2. Salva e aggiorna, e dovresti ora vedere due cose:

   - Il messaggio di errore e lo stack di chiamate come prima, ma questa volta, senza un'etichetta di "uncaught", o "non gestito".
   - Il messaggio registrato "Inserisci il codice per gestire l'errore".

3. Ora prova ad aggiornare `num` con un valore corretto (numerico), e vedrai il risultato del calcolo registrato, senza messaggi di errore.

Questo è significativo — qualsiasi errore lanciato non è più non gestito, quindi non fermerà l'applicazione. Puoi eseguire qualsiasi codice vuoi per gestire l'errore. Qui sopra stiamo solo registrando un messaggio, ma per esempio, potresti chiamare una qualsiasi funzione che è stata eseguita prima per chiedere all'utente di inserire la sua altezza, questa volta chiedendogli di correggere l'errore di input. Potresti anche usare un'istruzione `if...else` per eseguire codice di gestione degli errori diverso a seconda del tipo di errore restituito.

### Rilevamento delle funzionalità

Il rilevamento delle funzionalità è utile quando stai pianificando di usare nuove funzionalità JavaScript che potrebbero non essere supportate su tutti i browser. Testa la funzionalità e poi esegui codice condizionalmente per fornire un'esperienza accettabile sia nei browser che supportano sia in quelli che non supportano la funzionalità. Come esempio veloce, l'[API di Geolocalizzazione](/it/docs/Web/API/Geolocation_API) (che espone i dati di posizione disponibili per il dispositivo su cui il browser web è in esecuzione) ha un punto di ingresso principale per il suo utilizzo — una proprietà `geolocation` disponibile sull'oggetto globale [Navigator](/it/docs/Web/API/Navigator). Pertanto, puoi rilevare se il browser supporta la geolocalizzazione o meno utilizzando una struttura `if()` simile a quella che abbiamo visto prima:

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition((position) => {
    // show the location on a map, perhaps using the Google Maps API
  });
} else {
  // Give the user a choice of static maps instead
}
```

Puoi trovare alcuni altri esempi di rilevamento delle funzionalità in [Alternative allo sniffing UA](/it/docs/Web/HTTP/Guides/Browser_detection_using_the_user_agent#alternatives_to_ua_sniffing).

## Come trovare aiuto

Ci sono molti altri problemi che incontrerai con JavaScript (e HTML e CSS!), quindi la conoscenza di come trovare risposte online è inestimabile.

Tra le migliori fonti di informazioni di supporto ci sono MDN (è dove ti trovi ora!), [stackoverflow.com](https://stackoverflow.com/), e [caniuse.com](https://caniuse.com/).

- Per usare il Mozilla Developer Network (MDN), la maggior parte delle persone fa una ricerca del motore di ricerca sulla tecnologia su cui sta cercando informazioni, più il termine "mdn", ad esempio, "mdn HTML video".
- [caniuse.com](https://caniuse.com/) fornisce informazioni di supporto, insieme a pochi link utili a risorse esterne. Ad esempio, vedi <https://caniuse.com/#search=video> (devi solo inserire la funzionalità che stai cercando nella casella di testo).
- [stackoverflow.com](https://stackoverflow.com/) (SO) è un sito di forum dove puoi porre domande e avere colleghi sviluppatori che condividono le loro soluzioni, cercare post precedenti, e aiutare altri sviluppatori. Ti consigliamo di verificare e vedere se c'è già una risposta alla tua domanda prima di postare una nuova domanda. Ad esempio, abbiamo cercato su SO "disabling autofocus on HTML dialog" e molto rapidamente abbiamo trovato [Disable showModal auto-focusing using HTML attributes](https://stackoverflow.com/questions/63267581/disable-showmodal-auto-focusing-using-html-attributes).

Oltre a questo, prova a cercare nel tuo motore di ricerca preferito una risposta al tuo problema. È spesso utile cercare messaggi di errore specifici se li hai — altri sviluppatori avranno probabilmente avuto gli stessi problemi che hai tu.

## Sommario

Quindi, questo è il debugging e la gestione degli errori in JavaScript. Semplice, vero? Forse non così semplice, ma questo articolo dovrebbe almeno darti un inizio, e qualche idea su come affrontare i problemi relativi a JavaScript che incontrerai.

Questo è tutto per il modulo Scripting dinamico con JavaScript; congratulazioni per aver raggiunto la fine! Nel prossimo modulo, ti aiuteremo ad esplorare framework e librerie JavaScript.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}
