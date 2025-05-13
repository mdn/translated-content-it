---
title: Debugging e gestione degli errori in JavaScript
short-title: Debugging e gestione degli errori
slug: Learn_web_development/Core/Scripting/Debugging_JavaScript
l10n:
  sourceCommit: 759102220c07fb140b3e06971cd5981d8f0f134f
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}

In questa lezione, torneremo sul tema del debugging di JavaScript (che abbiamo inizialmente esaminato in [Cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)). Qui approfondiremo le tecniche per individuare gli errori, ma esamineremo anche come programmare in modo difensivo e gestire gli errori nel codice, evitando problemi fin dall'inizio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una conoscenza di base di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Utilizzare gli strumenti di sviluppo del browser per ispezionare il JavaScript in esecuzione sulla sua pagina e vedere quali errori genera.</li>
          <li>Usare <code>console.log()</code> e <code>console.error()</code> per il debugging.</li>
          <li>Debugging avanzato di JavaScript con i devtools del browser.</li>
          <li>Gestione degli errori con <code>conditional</code>, <code>try...catch</code> e <code>throw</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo sui tipi di errore JavaScript

In precedenza nel modulo, in [Cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong), abbiamo esaminato in generale i tipi di errore che possono verificarsi nei programmi JavaScript, e abbiamo detto che possono essere suddivisi grossomodo in due tipi: errori di sintassi e errori logici. Abbiamo anche aiutato a comprendere alcuni tipi comuni di messaggi di errore JavaScript, e abbiamo mostrato come effettuare un semplice debugging utilizzando istruzioni [`console.log()`](/it/docs/Web/API/console/log_static).

In questo articolo, approfondiremo gli strumenti a disposizione per individuare gli errori, e esamineremo anche modi per prevenire gli errori.

## Linting del codice

Prima di provare a individuare errori specifici, è necessario assicurarsi che il codice sia valido. Utilizzi il servizio di [validazione del Markup del W3C](https://validator.w3.org/), il servizio di [validazione CSS](https://jigsaw.w3.org/css-validator/), e un linter JavaScript come [ESLint](https://eslint.org/play/) per assicurarsi che il codice sia valido. Questo probabilmente evidenzierà una serie di errori, permettendoti di concentrarti sugli errori rimanenti.

### Plugin per l'editor di codice

Non è molto conveniente dover copiare e incollare il codice su una pagina web per verificarne la validità ripetutamente. Consigliamo di installare un plugin linter sul suo editor di codice, in modo da ricevere segnalazioni di errori mentre scrive il codice. Provi a cercare ESLint nell'elenco di plugin o estensioni del suo editor di codice, e lo installi.

## Problemi comuni in JavaScript

Ci sono diversi problemi comuni in JavaScript di cui dovrà tenere conto, come ad esempio:

- Problemi di sintassi e logica di base (ancora, controlli [Risoluzione dei problemi JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)).
- Assicurarsi che le variabili, ecc. siano definite nel corretto ambito e che non stia incontrando conflitti tra elementi dichiarati in luoghi diversi (vedi [Ambito delle funzioni e conflitti](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)).
- Confusione riguardo a [this](/it/docs/Web/JavaScript/Reference/Operators/this), in termini di a quale ambito si applica, e quindi se il suo valore è quello che si intende. Può leggere [Che cos'è "this"?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this) per un'introduzione leggera; dovrebbe anche studiare esempi come [questo](https://github.com/mdn/learning-area/blob/7ed039d17e820c93cafaff541aa65d874dde8323/javascript/oojs/assessment/main.js#L143), che mostra un tipico modello di salvataggio di un ambito `this` in una variabile separata, poi utilizzare quella variabile nelle funzioni annidate in modo da essere sicuro di applicare funzionalità all'ambito `this` corretto.
- Utilizzo scorretto di funzioni all'interno di loop che iterano con una variabile globale (in generale "ottenendo l'ambito sbagliato").

> [!CALLOUT]
> Ad esempio, in [bad-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/bad-for-loop.html) (veda [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/bad-for-loop.html)), ci iteriamo attraverso 10 iterazioni usando una variabile definita con `var`, ogni volta creando un paragrafo e aggiungendo un gestore di eventi [onclick](/it/docs/Web/API/Element/click_event) ad esso. Quando viene cliccato, vogliamo che ognuno mostri un messaggio di avviso contenente il suo numero (il valore di `i` al momento della sua creazione). Invece segnalano tutti `i` come 11 — perché il `for` loop esegue tutte le sue iterazioni prima che le funzioni annidate siano invocate.
>
> La soluzione più semplice è dichiarare la variabile dell'iterazione con `let` invece di `var`—il valore di `i` associato alla funzione è quindi unico per ogni iterazione. Veda [good-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/good-for-loop.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/good-for-loop.html)) per una versione che funziona.

- Assicurarsi che le [operazioni asincrone](/it/docs/Learn_web_development/Extensions/Async_JS) siano completate prima di cercare di utilizzare i valori che restituiscono. Questo di solito significa comprendere come utilizzare le _promesse_: utilizzare correttamente [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) o eseguire il codice per gestire il risultato di una chiamata asincrona nel gestore {{jsxref("Promise.then()", "then()")}} della promessa. Veda [Come utilizzare le promesse](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per un'introduzione a questo argomento.

> **Note:** [Buggy JavaScript Code: The 10 Most Common Mistakes JavaScript Developers Make](https://www.toptal.com/javascript/10-most-common-javascript-mistakes) contiene alcune utili discussioni su questi errori comuni e altri ancora.

## La console JavaScript del browser

Gli strumenti di sviluppo del browser hanno molte funzionalità utili per aiutare a eseguire il debugging di JavaScript. Per iniziare, la console JavaScript segnalerà gli errori nel suo codice.

Faccia una copia locale del nostro esempio [fetch-broken](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/fetch-broken/) (veda anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-broken)).

Se guarda la console, vedrà un messaggio di errore. La formulazione esatta dipende dal browser, ma sarà qualcosa del tipo: "Uncaught TypeError: heroes is not iterable" e viene indicato il numero della riga 25. Se esaminiamo il codice sorgente, la sezione rilevante è questa:

```js
function showHeroes(jsonObj) {
  const heroes = jsonObj["members"];

  for (const hero of heroes) {
    // …
  }
}
```

Il codice si arresta non appena tentiamo di utilizzare `jsonObj` (che come potrebbe aspettarsi, dovrebbe essere un [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)). Questo dovrebbe essere recuperato da un file `.json` esterno utilizzando la seguente chiamata [`fetch()`](/it/docs/Web/API/Window/fetch):

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
populateHeader(response);
showHeroes(response);
```

Ma questo non riesce.

## L'API della Console

Potresti già sapere cosa c'è di sbagliato in questo codice, ma esploriamolo un po' di più per mostrare come potrebbe indagare su questo. Inizieremo con l'[API della Console](/it/docs/Web/API/console), che consente al codice JavaScript di interagire con la console JavaScript del browser. Ha numerose funzionalità disponibili; ha già incontrato [`console.log()`](/it/docs/Web/API/console/log_static), che stampa un messaggio personalizzato nella console.

Provi ad aggiungere una chiamata `console.log()` per registrare il valore restituito di `fetch()`, in questo modo:

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
console.log(`Response value: ${response}`);
populateHeader(response);
showHeroes(response);
```

Aggiorni la pagina nel browser. Questa volta, prima del messaggio di errore, vedrà un nuovo messaggio registrato nella console:

```plain
Response value: [object Promise]
```

L'output di `console.log()` mostra che il valore restituito di `fetch()` non è il dato JSON, ma è un {{jsxref("Promise")}}. La funzione `fetch()` è asincrona: restituisce una `Promise` che viene soddisfatta solo quando la risposta effettiva viene ricevuta dalla rete. Prima di poter utilizzare la risposta, dobbiamo attendere che la `Promise` sia soddisfatta.

### `console.error()` e call stack

Come breve digressione, proviamo a utilizzare un metodo diverso della console per segnalare l'errore — [`console.error()`](/it/docs/Web/API/Console/error). Nel suo codice, sostituisca

```js
console.log(`Response value: ${response}`);
```

con

```js
console.error(`Response value: ${response}`);
```

Salvi il suo codice e aggiorni il browser e ora vedrà il messaggio segnalato come errore, con lo stesso colore e icona dell'errore non gestito sotto di esso. Inoltre, ora ci sarà una freccia di espansione/collasso accanto al messaggio. Se preme su questa, vedrà una singola riga che indica la riga nel file JavaScript in cui è originato l'errore. In effetti, anche la riga di errore non gestita _ha_ questo, ma ha due righe:

```plain
showHeroes http://localhost:7800/js-debug-test/index.js:25
<anonymous> http://localhost:7800/js-debug-test/index.js:10
```

Questo significa che l'errore proviene dalla funzione `showHeroes()`, riga 25, come notato in precedenza. Se guarda il suo codice, vedrà che la chiamata anonima sulla riga 10 è la linea che sta chiamando `showHeroes()`. Queste linee si riferiscono a uno **stack di chiamate**, e possono essere davvero utili quando si cerca di individuare la fonte di un errore che coinvolge molti luoghi diversi nel suo codice.

La chiamata `console.error()` non è così utile in questo caso, ma può essere utile per generare uno stack di chiamate se uno non è già disponibile.

### Risolvere l'errore

Comunque, torniamo a cercare di correggere il nostro errore. Possiamo accedere alla risposta dalla `Promise` soddisfatta concatenando il metodo {{jsxref("Promise.prototype.then()", "then()")}} alla fine della chiamata `fetch()`. Possiamo quindi passare il valore di risposta risultante alle funzioni che lo accettano, in questo modo:

```js
fetch(requestURL).then((response) => {
  populateHeader(response);
  showHeroes(response);
});
```

Salvi e aggiorni, e verifichi se il suo codice funziona. Spoiler — la modifica sopra non ha risolto il problema. Purtroppo, **abbiamo ancora lo stesso errore**!

> [!NOTE]
> Per riassumere, ogni volta che qualcosa non funziona e un valore non sembra essere quello che dovrebbe essere in un determinato punto del suo codice, può utilizzare `console.log()`, `console.error()`, o un'altra funzione simile per stampare il valore e vedere cosa sta succedendo.

## Utilizzo del debugger JavaScript

Indaghiamo ulteriormente su questo problema utilizzando una caratteristica più sofisticata degli strumenti di sviluppo del browser: il [debugger JavaScript](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html) come viene chiamato in Firefox.

> [!NOTE]
> Strumenti simili sono disponibili in altri browser; la [scheda Sources](https://developer.chrome.com/docs/devtools/#sources) in Chrome, Debugger in Safari (veda [Safari Web Development Tools](https://developer.apple.com/safari/tools/)), ecc.

In Firefox, la scheda Debugger si presenta così:

![Debugger di Firefox](debugger-tab.png)

- A sinistra, può selezionare lo script che desidera eseguire il debug (in questo caso ne abbiamo solo uno).
- Il pannello centrale mostra il codice nello script selezionato.
- Il pannello a destra mostra dettagli utili pertinenti all'ambiente corrente — _Breakpoints_, _Callstack_ e _Scopes_ attivi al momento.

La caratteristica principale di tali strumenti è la possibilità di aggiungere breakpoints al codice — questi sono punti in cui l'esecuzione del codice si ferma, e a quel punto può esaminare l'ambiente nel suo stato corrente e vedere cosa sta succedendo.

Esploriamo l'utilizzo dei breakpoints:

1. L'errore viene generato alla stessa riga di prima — `for (const hero of heroes) {` — riga 26 nello screenshot qui sotto. Clicchi su questa linea nel pannello centrale per aggiungere un breakpoint (vedrà una freccia blu apparire sopra essa).
2. Ora aggiorni la pagina (<kbd>Cmd</kbd>/<kbd>Ctrl</kbd> + <kbd>R</kbd>) — il browser sospenderà l'esecuzione del codice su quella riga. A quel punto, il lato destro si aggiornerà per mostrare quanto segue:

![Debugger di Firefox con un breakpoint](breakpoint.png)

- Sotto _Breakpoints_, vedrà i dettagli del punto di interruzione impostato.
- Sotto _Call Stack_, vedrà alcune voci — questo è sostanzialmente lo stesso dello stack di chiamate che abbiamo esaminato prima nella sezione `console.error()`. _Call Stack_ mostra un elenco delle funzioni che sono state invocate per causare l'invocazione della funzione corrente. In cima, abbiamo `showHeroes()`, la funzione in cui ci troviamo attualmente, e in seconda posizione abbiamo `onload`, che memorizza la funzione gestore eventi contenente la chiamata a `showHeroes()`.
- Sotto _Scopes_, vedrà l'ambito attualmente attivo per la funzione che stiamo guardando. Ne abbiamo solo tre — `showHeroes`, `block` e `Window` (l'ambito globale). Ogni ambito può essere espanso per mostrare i valori delle variabili all'interno dell'ambito quando l'esecuzione del codice è stata sospesa.

Possiamo trovare alcune informazioni molto utili qui dentro:

1. Espanda l'ambito `showHeroes` — può vedere da questo che la variabile heroes è `undefined`, indicando che l'accesso alla proprietà `members` di `jsonObj` (prima riga della funzione) non ha funzionato.
2. Può anche vedere che la variabile `jsonObj` memorizza un oggetto [`Response`](/it/docs/Web/API/Response), non un oggetto JSON.

L'argomento di `showHeroes()` è il valore con cui è stata soddisfatta la promessa `fetch()`. Quindi questa promessa non è nel formato JSON: è un oggetto `Response`. C'è un passaggio aggiuntivo necessario per recuperare il contenuto della risposta come un oggetto JSON.

Vorremmo che provasse a risolvere questo problema da solo. Per iniziare, consulti la documentazione per l'oggetto [`Response`](/it/docs/Web/API/Response). Se si blocca, può trovare il codice sorgente corretto su <https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-fixed>. 

> [!NOTE]
> La scheda del debugger ha molte altre utili funzionalità che non abbiamo discusso qui, ad esempio breakpoint condizionali e espressioni di controllo. Per molte altre informazioni, vedete la pagina [Debugger](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html).

## Gestione degli errori JavaScript nel suo codice

HTML e CSS sono permissivi — errori e funzionalità non riconosciute possono spesso essere gestiti a causa della natura dei linguaggi. Ad esempio, CSS ignorerà le proprietà non riconosciute, e il resto del codice funzionerà spesso correttamente. JavaScript non è tanto permissivo quanto HTML e CSS però — se il motore JavaScript incontra errori o sintassi non riconosciute, spesso genererà errori.

Esploriamo una strategia comune per gestire gli errori JavaScript nel suo codice. Le seguenti sezioni sono progettate per essere seguite facendo una copia del template di seguito come `handling-errors.html` sulla sua macchina locale, aggiungendo i frammenti di codice tra i tag di apertura e chiusura `<script>` e `</script>`, quindi aprendo il file in un browser e guardando l'output nella console JavaScript dei devtools.

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

Un uso comune dei [condizionali JavaScript](/it/docs/Learn_web_development/Core/Scripting/Conditionals) è quello di gestire gli errori. I condizionali consentono di eseguire diverso codice a seconda del valore di una variabile. Spesso vorrà utilizzare questo in modo difensivo, per evitare di generare un errore se il valore non esiste o è del tipo errato, o per catturare un errore se il valore causerebbe la restituzione di un risultato errato, il che potrebbe causare problemi in seguito.

Guardiamo un esempio. Supponiamo di avere una funzione che prende come argomento l'altezza dell'utente in pollici e restituisce la sua altezza in metri, arrotondata a 2 decimali. Potrebbe apparire così:

```js
function inchesToMeters(num) {
  const mVal = (num * 2.54) / 100;
  const m2dp = mVal.toFixed(2);
  return m2dp;
}
```

1. Nell'elemento `<script>` del suo esempio, dichiari una `const` chiamata `height` e le assegni un valore di `70`:

   ```js
   const height = 70;
   ```

2. Copi la funzione sopra sotto la linea precedente.

3. Chiamai la funzione, passandole la costante `height` come argomento, e registri il valore restituito nella console:

   ```js
   console.log(inchesToMeters(height));
   ```

4. Carichi l'esempio in un browser e guardi la console JavaScript dei devtools. Dovrebbe vedere un valore di `1.78` registrato.

5. Quindi questo funziona bene in isolamento. Ma cosa succede se i dati forniti sono mancanti o non corretti? Provi questi scenari:

   - Se cambia il valore `height` in `"70"` (cioè, `70` espresso come stringa), l'esempio dovrebbe ... funzionare ancora correttamente. Questo perché il calcolo sulla prima linea della stringa coercizza il valore in un tipo di dato numero. Questo va bene in un caso semplice come questo, ma in un codice più complesso i dati errati possono portare a tutti i tipi di bug, alcuni dei quali sottili e difficili da rilevare!
   - Se cambia `height` in un valore che non può essere coercito a un numero, come `"70 inches"` o `["Bob", 70]`, o {{jsxref("NaN")}}, l'esempio dovrebbe restituire il risultato come `NaN`. Questo potrebbe causare tutti i tipi di problemi, ad esempio se vuole includere l'altezza dell'utente da qualche parte nell'interfaccia utente del sito web.
   - Se rimuove completamente il valore `height` (commentandolo con `//` all'inizio della linea), la console mostrerà un errore del tipo "Uncaught ReferenceError: height is not defined", il che potrebbe portare la sua applicazione a fermarsi bruscamente.

   Ovviamente, nessuno di questi esiti è positivo. Come possiamo difenderci dai dati errati?

6. Aggiungiamo un condizionale all'interno della nostra funzione per verificare se i dati sono corretti prima di provare a eseguire il calcolo. Provi a sostituire la sua funzione corrente con la seguente:

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

7. Ora, se prova prima i due scenari, vedrà il nostro messaggio leggermente più utile restituito, per darle un'idea di cosa deve fare per correggere il problema. Potrebbe mettere qualsiasi cosa lì che le piace, incluso provare a eseguire codice per correggere il valore di `num`, ma questo non è consigliato — questa funzione ha un solo scopo semplice, e dovrebbe gestire la correzione del valore da qualche altra parte nel sistema.

   > [!NOTE]
   > Nella dichiarazione `if()`, prima verifichiamo se il tipo di dato di `num` è `"number"` utilizzando l'operatore [`typeof`](/it/docs/Web/JavaScript/Reference/Operators/typeof), ma verifichiamo anche se {{jsxref("isNaN()", "!isNaN(num)")}} restituisce `false`. Dobbiamo farlo per difenderci dal caso specifico in cui `num` è impostato su `NaN`, poiché stranamente, `typeof NaN` restituisce `"number"`!

8. Tuttavia, se prova ancora il terzo scenario, riceverà comunque un errore "Uncaught ReferenceError: height is not defined". Non può risolvere il fatto che un valore non sia disponibile dall'interno di una funzione che sta cercando di utilizzare il valore.

Come gestiamo questo? Beh, è probabilmente meglio far sì che la nostra funzione restituisca un errore personalizzato quando non riceve i dati corretti. Esamineremo come farlo prima, poi gestiremo tutti gli errori insieme.

### Lancio di errori personalizzati

Può lanciare un errore personalizzato in qualsiasi punto del suo codice utilizzando l'istruzione [`throw`](/it/docs/Web/JavaScript/Reference/Statements/throw), accoppiata con il costruttore {{jsxref("Error.Error", "Error()")}}. Vediamo questo in azione.

1. Nella sua funzione, sostituisca la linea `console.log()` all'interno del blocco `else` della sua funzione con la seguente linea:

   ```js
   throw new Error("A number was not provided. Please correct the input.");
   ```

2. Esegua di nuovo il suo esempio, ma si accerti che `num` sia impostato su un valore errato (cioè, non numerico). Questa volta, dovrebbe vedere il suo errore personalizzato lanciato, insieme a uno stack di chiamate utile per aiutarla a individuare la fonte dell'errore (anche se noti che il messaggio ci dice ancora che l'errore è "non gestito"). OK, quindi gli errori sono fastidiosi, ma questo è di gran lunga più utile rispetto all'esecuzione della funzione con successo e alla restituzione di un valore non numerico che potrebbe causare problemi in seguito.

Quindi, come gestiamo tutti quegli errori?

### try...catch

L'istruzione [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) è progettata appositamente per gestire gli errori. Ha la seguente struttura:

```js
try {
  // Run some code
} catch (error) {
  // Handle any errors
}
```

All'interno del blocco `try`, prova a eseguire un po' di codice. Se questo codice viene eseguito senza che venga lanciato alcun errore, tutto va bene e il blocco `catch` viene ignorato. Tuttavia, se viene lanciato un errore, viene eseguito il blocco `catch`, che fornisce l'accesso all'oggetto {{jsxref("Error")}} che rappresenta l'errore e le consente di eseguire il codice per gestirlo.

Utilizziamo `try...catch` nel nostro codice.

1. Sostituisca la linea `console.log()` che chiama la funzione `inchesToMeters()` alla fine del suo script con il seguente blocco. Ora stiamo eseguendo la nostra linea `console.log()` all'interno di un blocco `try`, e gestendo eventuali errori che restituisce dentro un corrispondente blocco `catch`.

   ```js
   try {
     console.log(inchesToMeters(height));
   } catch (error) {
     console.error(error);
     console.log("Insert code to handle the error");
   }
   ```

2. Salvi e aggiorni, e ora dovrebbe vedere due cose:

   - Il messaggio di errore e lo stack di chiamate come prima, ma questa volta, senza un'etichetta di "non gestito", o "unhandled".
   - Il messaggio registrato "Insert code to handle the error".

3. Ora provi a aggiornare `num` con un valore buono (numerico), e vedrà il risultato del calcolo registrato, senza il messaggio di errore.

Questo è significativo — eventuali errori lanciati non sono più non gestiti, quindi non porteranno l'applicazione a un arresto improvviso. Può eseguire qualsiasi codice preferito per gestire l'errore. Sopra stiamo solo registrando un messaggio, ma ad esempio potrebbe chiamare qualsiasi funzione eseguita in precedenza per chiedere all'utente di inserire la loro altezza, stavolta chiedendogli di correggere l'errore di input. Potrebbe persino utilizzare un'istruzione `if...else` per eseguire diverso codice di gestione degli errori a seconda di quale tipo di errore viene restituito.

### Rilevamento funzionalità

Il rilevamento delle funzionalità è utile quando si intende utilizzare nuove funzionalità JavaScript che potrebbero non essere supportate in tutti i browser. Verifichi la funzionalità, quindi esegua condizionatamente il codice per fornire un'esperienza accettabile sia nei browser che supportano sia in quelli che non supportano la funzionalità. Come rapido esempio, l'[API Geolocation](/it/docs/Web/API/Geolocation_API) (che espone i dati di localizzazione disponibili per il dispositivo su cui il browser web è in esecuzione) ha un punto di accesso principale per il suo utilizzo — una proprietà `geolocation` disponibile sull'oggetto globale [Navigator](/it/docs/Web/API/Navigator). Pertanto, può rilevare se il browser supporta o meno la geolocalizzazione utilizzando una struttura `if()` simile a quella che abbiamo visto in precedenza:

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition((position) => {
    // show the location on a map, perhaps using the Google Maps API
  });
} else {
  // Give the user a choice of static maps instead
}
```

Può trovare alcuni esempi ulteriori di rilevamento delle funzionalità in [Alternative al rilevamento degli UA](/it/docs/Web/HTTP/Guides/Browser_detection_using_the_user_agent#alternatives_to_ua_sniffing).

## Ricerca di aiuto

Ci sono molti altri problemi con cui si troverà a combattere in JavaScript (e HTML e CSS!), il che rende la conoscenza di come trovare risposte online inestimabile.

Tra le migliori fonti di informazioni di supporto ci sono MDN (è dove si trova ora!), [stackoverflow.com](https://stackoverflow.com/), e [caniuse.com](https://caniuse.com/).

- Per utilizzare il Mozilla Developer Network (MDN), la maggior parte delle persone effettua una ricerca tramite motore di ricerca della tecnologia su cui sta cercando di trovare informazioni, più il termine "mdn", ad esempio, "mdn HTML video".
- [caniuse.com](https://caniuse.com/) fornisce informazioni di supporto, insieme a pochi utili link a risorse esterne. Ad esempio, veda <https://caniuse.com/#search=video> (deve solo inserire la funzionalità che sta cercando nel campo di testo).
- [stackoverflow.com](https://stackoverflow.com/) (SO) è un sito forum dove può porre domande e i colleghi sviluppatori condivideranno le loro soluzioni, cercare post precedenti e aiutare altri sviluppatori. Le è consigliato di verificare se esiste già una risposta alla sua domanda, prima di postare una nuova domanda. Ad esempio, abbiamo cercato "disabilitare l'autofocus su dialog HTML" in SO, e molto rapidamente è apparso [Disable showModal auto-focusing using HTML attributes](https://stackoverflow.com/questions/63267581/disable-showmodal-auto-focusing-using-html-attributes).

Oltre a questo, provi a cercare nel suo motore di ricerca preferito una risposta al suo problema. Spesso è utile cercare messaggi di errore specifici se li ha — altri sviluppatori probabilmente avranno avuto gli stessi problemi suoi.

## Sommario

Quindi, questo è il debugging e la gestione degli errori JavaScript. Semplice eh? Forse non così semplice, ma questo articolo dovrebbe almeno darle un inizio, e alcune idee su come affrontare i problemi legati a JavaScript che incontrerà.

Questo è tutto per il modulo di scripting dinamico con JavaScript; congratulazioni per aver raggiunto la fine! Nel prossimo modulo la aiuteremo ad esplorare i framework e le librerie JavaScript.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}
