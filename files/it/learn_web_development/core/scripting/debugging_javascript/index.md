---
title: Debugging e gestione degli errori JavaScript
short-title: Debugging e gestione degli errori
slug: Learn_web_development/Core/Scripting/Debugging_JavaScript
l10n:
  sourceCommit: 418fefaa02f8e1ea53d53cb6fc510a4dc4100dc5
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/House_data_UI","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}

In questa lezione, torneremo all'argomento del debugging JavaScript (esaminato per la prima volta in [Cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)). Qui approfondiremo le tecniche per individuare gli errori e spiegheremo come programmare in modo difensivo e gestire gli errori nel codice, evitando i problemi fin dall'inizio.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Uso degli strumenti di sviluppo del browser per ispezionare il JavaScript in esecuzione nella pagina e vedere quali errori genera.</li>
          <li>Uso di <code>console.log()</code> e <code>console.error()</code> per il debugging.</li>
          <li>Debugging JavaScript avanzato con gli strumenti di sviluppo del browser.</li>
          <li>Gestione degli errori con <code>conditionals</code>, <code>try...catch</code> e <code>throw</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo dei tipi di errore JavaScript

In precedenza nel modulo, in [Cosa è andato storto?](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong), abbiamo esaminato in generale i tipi di errore che possono verificarsi nei programmi JavaScript, affermando che possono essere suddivisi approssimativamente in due tipi: errori di sintassi ed errori logici. Abbiamo inoltre aiutato a comprendere alcuni tipi comuni di messaggi di errore JavaScript e mostrato come eseguire un semplice debugging usando istruzioni [`console.log()`](/it/docs/Web/API/console/log_static).

In questo articolo, esamineremo più approfonditamente gli strumenti disponibili per individuare gli errori e i modi per prevenirli fin dall'inizio.

## Eseguire il linting del codice

È consigliabile convalidare prima il codice, prima di cercare errori specifici. Usare il [servizio di convalida del markup](https://validator.w3.org/) del W3C, il [servizio di convalida CSS](https://jigsaw.w3.org/css-validator/) e un linter JavaScript come [ESLint](https://eslint.org/play/) per assicurarsi che il codice sia valido. Questo probabilmente rivelerà alcuni errori, che potranno poi essere corretti, consentendo di concentrarsi sugli errori rimanenti.

### Plugin per editor di codice

Non è molto pratico dover copiare e incollare ripetutamente il codice in una pagina web per verificarne la validità. È consigliabile installare un plugin linter nell'editor di codice, per segnalare gli errori mentre si scrive il codice. Provare a cercare ESLint nell'elenco di plugin o estensioni dell'editor di codice e installarlo.

## Problemi JavaScript comuni

Esistono diversi problemi JavaScript comuni di cui tenere conto, tra cui:

- Problemi di sintassi e logica di base (consultare nuovamente [Risoluzione dei problemi JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)).
- Assicurarsi che le variabili e così via siano definite nell'ambito corretto e che non si verifichino conflitti tra elementi dichiarati in posizioni diverse (vedere [Ambito delle funzioni e conflitti](/it/docs/Learn_web_development/Core/Scripting/Functions#function_scope_and_conflicts)).
- Confusione riguardo [`this`](/it/docs/Web/JavaScript/Reference/Operators/this), in termini di quale ambito si applichi e quindi se il relativo valore sia quello previsto. È possibile leggere [Che cos'è "this"?](/it/docs/Learn_web_development/Core/Scripting/Object_basics#what_is_this) per una breve introduzione; è inoltre opportuno studiare esempi come [questo](https://github.com/mdn/learning-area/blob/7ed039d17e820c93cafaff541aa65d874dde8323/javascript/oojs/assessment/main.js#L143), che mostra un modello tipico per salvare un ambito `this` in una variabile separata e usare poi tale variabile nelle funzioni annidate, in modo da assicurarsi di applicare la funzionalità all'ambito `this` corretto.
- Uso non corretto delle funzioni all'interno di cicli che iterano con una variabile globale (più in generale, "sbagliare l'ambito").

> [!CALLOUT]
> Per esempio, in [bad-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/bad-for-loop.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/bad-for-loop.html)), si eseguono 10 iterazioni usando una variabile definita con `var`, creando ogni volta un paragrafo e aggiungendovi un gestore di eventi [onclick](/it/docs/Web/API/Element/click_event). Facendo clic, ciascuno dovrebbe visualizzare un messaggio di avviso contenente il proprio numero (il valore di `i` nel momento in cui è stato creato). Invece, tutti riportano `i` come 11, perché il ciclo `for` completa tutte le proprie iterazioni prima che le funzioni annidate vengano invocate.
>
> La soluzione più semplice consiste nel dichiarare la variabile di iterazione con `let` anziché `var`: il valore di `i` associato alla funzione è quindi univoco per ogni iterazione. Consultare [good-for-loop.html](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/good-for-loop.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/javascript/good-for-loop.html)) per una versione funzionante.

- Assicurarsi che le [operazioni asincrone](/it/docs/Learn_web_development/Extensions/Async_JS) siano completate prima di tentare di usare i valori restituiti. Di solito ciò significa comprendere come usare le _promises_: usare [`await`](/it/docs/Web/JavaScript/Reference/Operators/await) in modo appropriato oppure eseguire il codice che gestisce il risultato di una chiamata asincrona nel gestore {{jsxref("Promise.then()", "then()")}} della promise. Consultare [Come usare le promise](/it/docs/Learn_web_development/Extensions/Async_JS/Promises) per un'introduzione all'argomento.

> [!NOTE]
> [Buggy JavaScript Code: The 10 Most Common Mistakes JavaScript Developers Make](https://www.toptal.com/developers/javascript/10-most-common-javascript-mistakes) contiene alcune buone discussioni su questi errori comuni e altri ancora.

## La console JavaScript del browser

Gli strumenti di sviluppo del browser dispongono di molte funzionalità utili per eseguire il debugging JavaScript. Per iniziare, la console JavaScript segnalerà gli errori nel codice.

Creare una copia locale del nostro esempio [fetch-broken](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/javascript/fetch-broken/) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-broken)).

Osservando la console, verrà visualizzato un messaggio di errore. Il testo esatto dipende dal browser, ma sarà simile a: "Uncaught TypeError: heroes is not iterable", e il numero di riga indicato sarà 25. Osservando il codice sorgente, la sezione di codice rilevante è la seguente:

```js
function showHeroes(jsonObj) {
  const heroes = jsonObj["members"];

  for (const hero of heroes) {
    // …
  }
}
```

Il codice quindi si interrompe non appena si prova a usare `jsonObj` (che, come ci si potrebbe aspettare, dovrebbe essere un [oggetto JSON](/it/docs/Learn_web_development/Core/Scripting/JSON)). Questo dovrebbe essere recuperato da un file `.json` esterno usando la seguente chiamata [`fetch()`](/it/docs/Web/API/Window/fetch):

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
populateHeader(response);
showHeroes(response);
```

Tuttavia, questa operazione non riesce.

## L'API Console

Potrebbe essere già chiaro quale sia il problema nel codice, ma vediamo come investigarlo. Si inizierà con l'API [Console](/it/docs/Web/API/console), che consente al codice JavaScript di interagire con la console JavaScript del browser. Sono disponibili diverse funzionalità; è già stato incontrato [`console.log()`](/it/docs/Web/API/console/log_static), che stampa un messaggio personalizzato nella console.

Provare ad aggiungere una chiamata a `console.log()` per registrare il valore restituito da `fetch()`, in questo modo:

```js
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/json/superheroes.json";

const response = fetch(requestURL);
console.log(`Response value: ${response}`);
populateHeader(response);
showHeroes(response);
```

Aggiornare la pagina nel browser. Questa volta, prima del messaggio di errore, verrà visualizzato un nuovo messaggio registrato nella console:

```plain
Response value: [object Promise]
```

L'output di `console.log()` mostra che il valore restituito da `fetch()` non sono i dati JSON, ma una {{jsxref("Promise")}}. La funzione `fetch()` è asincrona: restituisce una `Promise` che viene soddisfatta solo quando la risposta effettiva è stata ricevuta dalla rete. Prima di poter usare la risposta, è necessario attendere che la `Promise` venga soddisfatta.

### `console.error()` e stack di chiamate

Come breve digressione, proviamo a usare un metodo della console diverso per segnalare l'errore: [`console.error()`](/it/docs/Web/API/Console/error_static). Nel codice, sostituire

```js
console.log(`Response value: ${response}`);
```

con

```js
console.error(`Response value: ${response}`);
```

Salvare il codice e aggiornare il browser; il messaggio verrà ora segnalato come errore, con lo stesso colore e la stessa icona dell'errore non intercettato sottostante. Inoltre, accanto al messaggio sarà visibile una freccia per espandere o comprimere. Premendola, verrà visualizzata una singola riga che indica la riga del file JavaScript in cui ha avuto origine l'errore. Anche la riga dell'errore non intercettato presenta questa caratteristica, ma ha due righe:

```plain
showHeroes http://localhost:7800/js-debug-test/index.js:25
<anonymous> http://localhost:7800/js-debug-test/index.js:10
```

Ciò significa che l'errore è causato dalla funzione `showHeroes()`, riga 25, come osservato in precedenza. Osservando il codice, si noterà che la chiamata anonima alla riga 10 chiama `showHeroes()`. Queste righe sono indicate come **stack di chiamate** e possono essere molto utili quando si tenta di rintracciare l'origine di un errore che coinvolge diverse posizioni nel codice.

La chiamata `console.error()` non è particolarmente utile in questo caso, ma può essere utile per generare uno stack di chiamate se non ne è già disponibile uno.

### Correzione dell'errore

In ogni caso, torniamo a tentare di correggere l'errore. È possibile accedere alla risposta della `Promise` soddisfatta concatenando il metodo {{jsxref("Promise.prototype.then()", "then()")}} alla fine della chiamata `fetch()`. È quindi possibile passare il valore della risposta risultante alle funzioni che lo accettano, in questo modo:

```js
fetch(requestURL).then((response) => {
  populateHeader(response);
  showHeroes(response);
});
```

Salvare e aggiornare, quindi verificare se il codice funziona. Attenzione, spoiler: la modifica precedente non ha risolto il problema. Purtroppo, **l'errore è ancora lo stesso**!

> [!NOTE]
> In sintesi, ogni volta che qualcosa non funziona e un valore non sembra essere ciò che dovrebbe essere in un punto del codice, è possibile usare `console.log()`, `console.error()` o un'altra funzione simile per stampare il valore e vedere cosa sta succedendo.

## Uso del debugger JavaScript

Esaminiamo ulteriormente questo problema usando una funzionalità più sofisticata degli strumenti di sviluppo del browser: il [debugger JavaScript](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html), come viene chiamato in Firefox.

> [!NOTE]
> Strumenti simili sono disponibili in altri browser: la [scheda Sources](https://developer.chrome.com/docs/devtools/#sources) in Chrome, Debugger in Safari (vedere [Safari Web Development Tools](https://developer.apple.com/safari/tools/)) e così via.

In Firefox, la scheda Debugger ha questo aspetto:

![Debugger di Firefox](debugger-tab.png)

- A sinistra, è possibile selezionare lo script da sottoporre a debugging (in questo caso ce n'è soltanto uno).
- Il pannello centrale mostra il codice nello script selezionato.
- Il pannello a destra mostra dettagli utili sull'ambiente corrente: _Breakpoints_, _Callstack_ e gli _Scopes_ attualmente attivi.

La funzionalità principale di tali strumenti è la possibilità di aggiungere breakpoint al codice: si tratta di punti in cui l'esecuzione del codice si arresta e, a quel punto, è possibile esaminare l'ambiente nel suo stato corrente e vedere cosa sta succedendo.

Esploriamo l'uso dei breakpoint:

1. L'errore viene generato sulla stessa riga di prima: `for (const hero of heroes) {`, riga 26 nello screenshot seguente. Fare clic sul numero di riga nel pannello centrale per aggiungervi un breakpoint; apparirà una freccia blu sopra di esso.
2. Ora aggiornare la pagina (<kbd>Cmd</kbd>/<kbd>Ctrl</kbd> + <kbd>R</kbd>): il browser sospenderà l'esecuzione del codice su quella riga. A questo punto, la parte destra si aggiornerà mostrando quanto segue:

![Debugger di Firefox con un breakpoint](breakpoint.png)

- In _Breakpoints_, verranno visualizzati i dettagli del breakpoint impostato.
- In _Call Stack_, verranno visualizzate alcune voci: è sostanzialmente la stessa cosa dello stack di chiamate esaminato in precedenza nella sezione relativa a `console.error()`. _Call Stack_ mostra un elenco delle funzioni invocate che hanno causato l'invocazione della funzione corrente. In alto si trova `showHeroes()`, la funzione in cui ci si trova attualmente, e al secondo posto si trova `onload`, che memorizza la funzione del gestore di eventi contenente la chiamata a `showHeroes()`.
- In _Scopes_, verrà visualizzato l'ambito attualmente attivo per la funzione esaminata. Ce ne sono soltanto tre: `showHeroes`, `block` e `Window` (l'ambito globale). Ciascun ambito può essere espanso per mostrare i valori delle variabili al suo interno quando l'esecuzione è arrestata.

Qui è possibile trovare informazioni molto utili:

1. Espandere l'ambito `showHeroes`: è possibile vedere che la variabile heroes è `undefined`, il che indica che l'accesso alla proprietà `members` di `jsonObj` (la prima riga della funzione) non ha funzionato.
2. È inoltre possibile vedere che la variabile `jsonObj` memorizza un oggetto [`Response`](/it/docs/Web/API/Response), non un oggetto JSON.

L'argomento di `showHeroes()` è il valore con cui è stata soddisfatta la promise `fetch()`. Questa promise non è in formato JSON: è un oggetto `Response`. È necessario un passaggio aggiuntivo per recuperare il contenuto della risposta come oggetto JSON.

Provare a correggere il problema autonomamente. Per iniziare, consultare la documentazione dell'oggetto [`Response`](/it/docs/Web/API/Response). In caso di difficoltà, il codice sorgente corretto è disponibile in <https://github.com/mdn/learning-area/tree/main/tools-testing/cross-browser-testing/javascript/fetch-fixed>.

> [!NOTE]
> La scheda debugger dispone di molte altre funzionalità utili che non vengono discusse qui. Per esempio, breakpoint condizionali ed espressioni di controllo. Per molte più informazioni, consultare la pagina [Debugger](https://firefox-source-docs.mozilla.org/devtools-user/debugger/index.html).

## Gestire gli errori JavaScript nel codice

HTML e CSS sono permissivi: gli errori e le funzionalità non riconosciute possono spesso essere gestiti grazie alla natura dei linguaggi. Per esempio, CSS ignora le proprietà non riconosciute e il resto del codice spesso continua a funzionare. JavaScript, tuttavia, non è permissivo quanto HTML e CSS; se il motore JavaScript incontra errori o sintassi non riconosciuta, spesso genera errori.

Esploriamo una strategia comune per gestire gli errori JavaScript nel codice. Le sezioni seguenti sono progettate per essere seguite creando una copia del file modello mostrato di seguito come `handling-errors.html` sul computer locale, aggiungendo gli snippet di codice tra i tag di apertura e chiusura `<script>` e `</script>`, quindi aprendo il file in un browser e osservando l'output nella console JavaScript degli strumenti di sviluppo.

```html
<!doctype html>
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

Un uso comune dei [condizionali JavaScript](/it/docs/Learn_web_development/Core/Scripting/Conditionals) è la gestione degli errori. I condizionali consentono di eseguire codice diverso in base al valore di una variabile. Spesso è opportuno usarli in modo difensivo per evitare di generare un errore se il valore non esiste o è del tipo errato, oppure per intercettare un errore se il valore produrrebbe un risultato non corretto, che potrebbe causare problemi in seguito.

Vediamo un esempio. Supponiamo di avere una funzione che accetta come argomento l'altezza dell'utente in pollici e restituisce l'altezza in metri, con 2 cifre decimali. Potrebbe avere questo aspetto:

```js
function inchesToMeters(num) {
  const mVal = (num * 2.54) / 100;
  const m2dp = mVal.toFixed(2);
  return m2dp;
}
```

1. Nell'elemento `<script>` del file di esempio, dichiarare una `const` chiamata `height` e assegnarle il valore `70`:

   ```js
   const height = 70;
   ```

2. Copiare la funzione precedente sotto la riga precedente.

3. Chiamare la funzione, passando la costante `height` come argomento, e registrare il valore restituito nella console:

   ```js
   console.log(inchesToMeters(height));
   ```

4. Caricare l'esempio in un browser e osservare la console JavaScript degli strumenti di sviluppo. Dovrebbe essere visualizzato il valore `1.78`.

5. Quindi questo funziona correttamente in isolamento. Ma cosa accade se i dati forniti sono mancanti o errati? Provare questi scenari:
   - Se si modifica il valore di `height` in `"70"` (ovvero `70` espresso come stringa), l'esempio dovrebbe... continuare a funzionare correttamente. Ciò avviene perché il calcolo sulla prima riga della stringa converte il valore nel tipo di dati numero. Questo è accettabile in un caso semplice come questo, ma in un codice più complesso dati errati possono causare ogni tipo di bug, alcuni sottili e difficili da individuare.
   - Se si modifica `height` in un valore che non può essere convertito in un numero, come `"70 inches"` o `["Bob", 70]`, oppure {{jsxref("NaN")}}, l'esempio dovrebbe restituire `NaN` come risultato. Ciò potrebbe causare ogni tipo di problema, per esempio se si desidera includere l'altezza dell'utente da qualche parte nell'interfaccia utente del sito web.
   - Se si rimuove del tutto il valore di `height` (commentandolo aggiungendo `//` all'inizio della riga), la console mostrerà un errore simile a "Uncaught ReferenceError: height is not defined", del tipo che potrebbe arrestare completamente l'applicazione.

   Evidentemente, nessuno di questi risultati è ideale. Come difendersi dai dati errati?

6. Aggiungiamo un condizionale nella funzione per verificare se i dati sono validi prima di tentare il calcolo. Provare a sostituire la funzione corrente con la seguente:

   ```js
   function inchesToMeters(num) {
     if (typeof num !== "number" || Number.isNaN(num)) {
       console.log("A number was not provided. Please correct the input.");
       return undefined;
     }
     const mVal = (num * 2.54) / 100;
     const m2dp = mVal.toFixed(2);
     return m2dp;
   }
   ```

7. Ora, riprovando i primi due scenari, verrà visualizzato un messaggio leggermente più utile, che fornisce un'indicazione di ciò che deve essere fatto per risolvere il problema. Al suo interno potrebbe essere inserito qualunque elemento, incluso provare a eseguire codice per correggere il valore di `num`, ma non è consigliato: questa funzione ha un unico scopo semplice e la correzione del valore dovrebbe essere gestita altrove nel sistema.

   > [!NOTE]
   > Nell'istruzione `if()`, viene prima verificato che il tipo di dati di `num` sia `"number"` usando l'operatore [`typeof`](/it/docs/Web/JavaScript/Reference/Operators/typeof), quindi viene verificato che {{jsxref("Number.isNaN", "Number.isNaN(num)")}} restituisca `false`. Questo è necessario per difendersi dal caso specifico in cui `num` sia impostato su `NaN`, poiché `typeof NaN` restituisce comunque `"number"`.

8. Tuttavia, riprovando il terzo scenario, verrà ancora generato l'errore "Uncaught ReferenceError: height is not defined". Non è possibile correggere il fatto che un valore non sia disponibile dall'interno di una funzione che tenta di usare quel valore.

Come gestire questo caso? È preferibile fare in modo che la funzione restituisca un errore personalizzato quando non riceve i dati corretti. Vedremo prima come farlo, quindi gestiremo tutti gli errori insieme.

### Generare errori personalizzati

È possibile generare un errore personalizzato in qualsiasi punto del codice usando l'istruzione [`throw`](/it/docs/Web/JavaScript/Reference/Statements/throw), insieme al costruttore {{jsxref("Error.Error", "Error()")}}. Vediamo come funziona.

1. Nella funzione, sostituire la riga `console.log()` all'interno del blocco `else` della funzione con la riga seguente:

   ```js
   throw new Error("A number was not provided. Please correct the input.");
   ```

2. Eseguire nuovamente l'esempio, assicurandosi però che `num` sia impostato su un valore errato, cioè non numerico. Questa volta verrà visualizzato l'errore personalizzato generato, insieme a un utile stack di chiamate che aiuta a individuare l'origine dell'errore. Si noti però che il messaggio indica ancora che l'errore è "uncaught" o "unhandled". Gli errori sono fastidiosi, ma questo comportamento è molto più utile dell'esecuzione riuscita della funzione con la restituzione di un valore non numerico che potrebbe causare problemi in seguito.

Come gestire dunque tutti questi errori?

### try...catch

L'istruzione [`try...catch`](/it/docs/Web/JavaScript/Reference/Statements/try...catch) è progettata specificamente per gestire gli errori. Ha la seguente struttura:

```js
try {
  // Run some code
} catch (error) {
  // Handle any errors
}
```

All'interno del blocco `try`, si tenta di eseguire del codice. Se questo codice viene eseguito senza generare un errore, tutto procede correttamente e il blocco `catch` viene ignorato. Tuttavia, se viene generato un errore, viene eseguito il blocco `catch`, che fornisce accesso all'oggetto {{jsxref("Error")}} che rappresenta l'errore e consente di eseguire codice per gestirlo.

Usiamo `try...catch` nel codice.

1. Sostituire la riga `console.log()` che chiama la funzione `inchesToMeters()` alla fine dello script con il blocco seguente. La riga `console.log()` viene ora eseguita all'interno di un blocco `try` e gli eventuali errori restituiti vengono gestiti all'interno del blocco `catch` corrispondente.

   ```js
   try {
     console.log(inchesToMeters(height));
   } catch (error) {
     console.error(error);
     console.log("Insert code to handle the error");
   }
   ```

2. Salvare e aggiornare; ora dovrebbero essere visibili due elementi:
   - Il messaggio di errore e lo stack di chiamate sono come prima, ma questa volta senza l'etichetta "uncaught" o "unhandled".
   - Il messaggio registrato "Insert code to handle the error".

3. Ora provare ad aggiornare `num` con un valore valido, cioè numerico: verrà visualizzato il risultato del calcolo, senza messaggi di errore.

Questo è significativo: tutti gli errori generati vengono ora gestiti, quindi non causeranno l'arresto dell'applicazione. È possibile eseguire qualsiasi codice per gestire l'errore. In precedenza viene registrato un messaggio di base, ma per esempio potrebbe essere chiamata una funzione che chiede nuovamente all'utente di inserire la propria altezza, chiedendo però questa volta di correggere l'errore di input. Si potrebbe anche usare un'istruzione `if...else` per eseguire codice di gestione degli errori diverso a seconda del tipo di errore restituito.

### Rilevamento delle funzionalità

Il rilevamento delle funzionalità è utile quando si pianifica di usare nuove funzionalità JavaScript che potrebbero non essere supportate in tutti i browser. È possibile verificare la funzionalità, quindi eseguire condizionalmente codice per fornire un'esperienza accettabile nei browser che supportano la funzionalità e in quelli che non la supportano. Come esempio rapido, l'[API Geolocation](/it/docs/Web/API/Geolocation_API), che espone i dati di posizione disponibili per il dispositivo su cui è in esecuzione il browser web, ha un punto di ingresso principale per il proprio utilizzo: una proprietà `geolocation` disponibile sull'oggetto globale [Navigator](/it/docs/Web/API/Navigator). Pertanto, è possibile rilevare se il browser supporta o meno la geolocalizzazione usando una struttura `if()` simile a quella vista in precedenza:

```js
if ("geolocation" in navigator) {
  navigator.geolocation.getCurrentPosition((position) => {
    // show the location on a map, perhaps using the Google Maps API
  });
} else {
  // Give the user a choice of static maps instead
}
```

Sono disponibili altri esempi di rilevamento delle funzionalità in [Alternative al rilevamento tramite user agent](/it/docs/Web/HTTP/Guides/Browser_detection_using_the_user_agent#alternatives_to_ua_sniffing).

## Trovare aiuto

Esistono molti altri problemi che possono essere incontrati con JavaScript, HTML e CSS, rendendo preziosa la capacità di trovare risposte online.

Tra le migliori fonti di informazioni di supporto vi sono MDN, [stackoverflow.com](https://stackoverflow.com/) e [caniuse.com](https://caniuse.com/).

- Per usare MDN, la maggior parte delle persone esegue una ricerca con un motore di ricerca della tecnologia su cui si desiderano informazioni, più il termine "mdn"; per esempio, "mdn HTML video".
- [caniuse.com](https://caniuse.com/) fornisce informazioni sul supporto, insieme ad alcuni utili collegamenti a risorse esterne. Per esempio, vedere <https://caniuse.com/#search=video> (è necessario inserire la funzionalità cercata nella casella di testo).
- [stackoverflow.com](https://stackoverflow.com/) (SO) è un sito forum dove è possibile porre domande e ricevere soluzioni da altri sviluppatori, cercare post precedenti e aiutare altri sviluppatori. Prima di pubblicare una nuova domanda, cercare una risposta alla propria domanda per verificare se ne esista già una. Per esempio, è stata cercata su SO la frase "disabling autofocus on HTML dialog" ed è stato rapidamente trovato [Disable showModal auto-focusing using HTML attributes](https://stackoverflow.com/questions/63267581/disable-showmodal-auto-focusing-using-html-attributes).

A parte questo, provare a cercare una risposta al problema con il motore di ricerca preferito. Spesso è utile cercare messaggi di errore specifici, se disponibili: è probabile che altri sviluppatori abbiano avuto gli stessi problemi.

## Riepilogo

Questo è dunque il debugging JavaScript e la gestione degli errori. Semplice, vero? Forse non proprio, ma questo articolo dovrebbe almeno fornire un punto di partenza e alcune idee su come affrontare i problemi relativi a JavaScript che si incontreranno.

Questo conclude il modulo Scripting dinamico con JavaScript; congratulazioni per aver raggiunto la fine! Nel prossimo modulo verrà esplorato il tema di [framework e librerie JavaScript](/it/docs/Learn_web_development/Core/Frameworks_libraries).

{{PreviousMenuNext("Learn_web_development/Core/Scripting/House_data_UI","Learn_web_development/Core/Frameworks_libraries", "Learn_web_development/Core/Scripting")}}
