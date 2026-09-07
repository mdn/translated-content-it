---
title: Prendere decisioni nel codice — le condizioni
short-title: Conditionals
slug: Learn_web_development/Core/Scripting/Conditionals
l10n:
  sourceCommit: 9d3d642daf9df9ece138fa39972edc5f7d6dcd6b
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting/Test_your_skills/Conditionals", "Learn_web_development/Core/Scripting")}}

In qualsiasi linguaggio di programmazione, il codice deve prendere decisioni ed eseguire azioni di conseguenza in base a diversi input. Per esempio, in un gioco, se il numero di vite del giocatore è 0, la partita è finita. In un'app meteo, se viene consultata al mattino, viene mostrata un'immagine dell'alba; se è notte, vengono mostrate stelle e luna. In questo articolo verrà illustrato il funzionamento delle cosiddette istruzioni condizionali in JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere che cos'è una condizione: una struttura di codice per eseguire percorsi di codice diversi in base al risultato di un test.</li>
          <li>Implementare condizioni usando <code>if</code>/<code>else</code>/<code>else if</code>.</li>
          <li>Usare gli operatori di confronto per creare test.</li>
          <li>Implementare la logica AND, OR e NOT nei test.</li>
          <li>Istruzioni switch.</li>
          <li>Operatori ternari.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## È possibile averlo a una condizione!

Gli esseri umani (e gli altri animali) prendono continuamente decisioni che influenzano la loro vita, da quelle piccole ("dovrei mangiare uno o due biscotti?") a quelle importanti ("dovrei rimanere nel mio paese e lavorare nella fattoria della mia famiglia, oppure trasferirmi in America e studiare astrofisica?").

Le istruzioni condizionali consentono di rappresentare questo processo decisionale in JavaScript, dalla scelta da compiere (per esempio, "uno o due biscotti") fino al risultato di tali scelte (forse il risultato di "ho mangiato un biscotto" potrebbe essere "avevo ancora fame", mentre il risultato di "ho mangiato due biscotti" potrebbe essere "ero sazio, ma la mamma mi ha rimproverato perché ho mangiato tutti i biscotti").

![Un personaggio dei cartoni animati simile a una persona tiene un barattolo di biscotti con l'etichetta "Cookies". Sopra la testa del personaggio c'è un punto interrogativo. Ci sono due fumetti. Il fumetto a sinistra contiene un biscotto. Il fumetto a destra contiene due biscotti. Nel complesso, l'immagine suggerisce che il personaggio stia cercando di decidere se mangiare uno o due biscotti.](cookie-choice-small.png)

## Istruzioni if...else

Esaminiamo di gran lunga il tipo più comune di istruzione condizionale che verrà usato in JavaScript: l'umile [`if...else` statement](/it/docs/Web/JavaScript/Reference/Statements/if...else).

### Sintassi di base di if...else

La sintassi di base di `if...else` è simile a questa:

```js
if (condition) {
  /* code to run if condition is true */
} else {
  /* run some other code instead */
}
```

Qui sono presenti:

1. La parola chiave `if` seguita da alcune parentesi.
2. Una condizione da verificare, inserita tra parentesi (in genere "questo valore è maggiore di quest'altro valore?" oppure "questo valore esiste?"). La condizione usa gli [operatori di confronto](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators) trattati precedentemente nel modulo e restituisce `true` o `false`.
3. Un insieme di parentesi graffe, al cui interno si trova del codice: può essere qualsiasi codice e viene eseguito solo se la condizione restituisce `true`.
4. La parola chiave `else`.
5. Un altro insieme di parentesi graffe, al cui interno si trova altro codice: può essere qualsiasi codice e viene eseguito solo se la condizione non è `true`, ovvero se la condizione è `false`.

Questo codice è abbastanza leggibile per gli esseri umani: afferma "**se** la **condizione** restituisce `true`, esegui il codice A, **altrimenti** esegui il codice B".

È importante notare che non è obbligatorio includere `else` e il secondo blocco tra parentesi graffe: anche il seguente codice è perfettamente valido:

```js
if (condition) {
  /* code to run if condition is true */
}

/* run some other code */
```

Tuttavia, occorre fare attenzione: in questo caso, il secondo blocco di codice non è controllato dall'istruzione condizionale, quindi viene eseguito **sempre**, indipendentemente dal fatto che la condizione restituisca `true` o `false`. Non è necessariamente un problema, ma potrebbe non essere il comportamento desiderato: spesso si vuole eseguire un blocco di codice _oppure_ l'altro, non entrambi.

Come ultimo punto, anche se non è consigliato, talvolta è possibile vedere istruzioni `if...else` scritte senza parentesi graffe:

```js example-bad
if (condition) doSomething();
else doSomethingElse();
```

Questa sintassi è perfettamente valida, ma il codice è molto più facile da comprendere se si usano le parentesi graffe per delimitare i blocchi di codice, oltre a più righe e rientri.

### Un esempio reale

Per comprendere meglio questa sintassi, consideriamo un esempio reale. Immaginiamo un bambino a cui la madre o il padre chiede aiuto con una faccenda domestica. Il genitore potrebbe dire: "Ehi tesoro! Se mi aiuti facendo la spesa, ti darò una paghetta extra così potrai permetterti quel giocattolo che desideravi". In JavaScript, si potrebbe rappresentare così:

```js
let shoppingDone = false;
let childAllowance;

if (shoppingDone === true) {
  childAllowance = 10;
} else {
  childAllowance = 5;
}
```

Il codice mostrato produce sempre la variabile `shoppingDone` con valore `false`, con conseguente delusione per il povero bambino. Spetterebbe allo sviluppatore fornire un meccanismo che consenta al genitore di impostare la variabile `shoppingDone` su `true` se il bambino ha fatto la spesa.

> [!NOTE]
> È possibile vedere una [versione più completa di questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/allowance-updater.html) (ed eseguirla [dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/allowance-updater.html).)

### else if

L'ultimo esempio offriva due scelte, o risultati, ma cosa succede se ne servono più di due?

Esiste un modo per concatenare ulteriori scelte o risultati a `if...else`, usando `else if`. Ogni scelta aggiuntiva richiede un ulteriore blocco da inserire tra `if () { }` e `else { }`. Osservare il seguente esempio più articolato, che potrebbe far parte di una semplice applicazione di previsioni meteorologiche:

```html
<label for="weather">Select the weather type today: </label>
<select id="weather">
  <option value="">--Make a choice--</option>
  <option value="sunny">Sunny</option>
  <option value="rainy">Rainy</option>
  <option value="snowing">Snowing</option>
  <option value="overcast">Overcast</option>
</select>

<p></p>
```

```js
const select = document.querySelector("select");
const para = document.querySelector("p");

select.addEventListener("change", setWeather);

function setWeather() {
  const choice = select.value;

  if (choice === "sunny") {
    para.textContent =
      "It is nice and sunny outside today. Wear shorts! Go to the beach, or the park, and get an ice cream.";
  } else if (choice === "rainy") {
    para.textContent =
      "Rain is falling outside; take a rain coat and an umbrella, and don't stay out for too long.";
  } else if (choice === "snowing") {
    para.textContent =
      "The snow is coming down — it is freezing! Best to stay in with a cup of hot chocolate, or go build a snowman.";
  } else if (choice === "overcast") {
    para.textContent =
      "It isn't raining, but the sky is grey and gloomy; it could turn any minute, so take a rain coat just in case.";
  } else {
    para.textContent = "";
  }
}
```

{{ EmbedLiveSample('else_if', '100%', 100, "", "") }}

1. Qui è presente un elemento HTML {{htmlelement("select")}} che consente di scegliere diverse condizioni meteorologiche, oltre a un semplice paragrafo.
2. Nel JavaScript, viene memorizzato un riferimento sia agli elementi {{htmlelement("select")}} sia a {{htmlelement("p")}}, e viene aggiunto un event listener all'elemento `<select>` in modo che, quando il suo valore cambia, venga eseguita la funzione `setWeather()`.
3. Quando questa funzione viene eseguita, viene innanzitutto impostata una variabile chiamata `choice` sul valore attualmente selezionato nell'elemento `<select>`. Viene quindi usata un'istruzione condizionale per mostrare testo diverso all'interno del paragrafo in base al valore di `choice`. Si noti che tutte le condizioni vengono testate in blocchi `else if () { }`, tranne la prima, che viene testata in un blocco `if () { }`.
4. L'ultima scelta, all'interno del blocco `else { }`, è in pratica un'opzione di "ultima risorsa": il codice al suo interno viene eseguito se nessuna delle condizioni è `true`. In questo caso, serve a svuotare il testo del paragrafo se non viene selezionato nulla, per esempio se un utente decide di selezionare nuovamente l'opzione segnaposto "--Make a choice--" mostrata all'inizio.

> [!NOTE]
> È possibile anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-else-if.html) ([ed eseguirlo dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/simple-else-if.html).)

### Una nota sugli operatori di confronto

Gli operatori di confronto vengono usati per verificare le condizioni all'interno delle istruzioni condizionali. Gli operatori di confronto sono stati introdotti nell'articolo [Matematica di base in JavaScript — numeri e operatori](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators). Le possibilità sono:

- `===` e `!==`: verificano se un valore è identico o non identico a un altro.
- `<` e `>`: verificano se un valore è minore o maggiore di un altro.
- `<=` e `>=`: verificano se un valore è minore o uguale, oppure maggiore o uguale, a un altro.

È utile dedicare una menzione speciale al test dei valori booleani (`true`/`false`) e a un modello comune che ricorrerà spesso. Qualsiasi valore che non sia `false`, `undefined`, `null`, `0`, `NaN` o una stringa vuota (`''`) restituisce effettivamente `true` quando viene verificato in un'istruzione condizionale; pertanto, è possibile usare da solo il nome di una variabile per verificare se è `true`, o anche se esiste, ovvero se non è undefined. Per esempio:

```js
let cheese = "Cheddar";

if (cheese) {
  console.log("Yay! Cheese available for making cheese on toast.");
} else {
  console.log("No cheese on toast for you today.");
}
```

Tornando all'esempio precedente del bambino che svolge una faccenda per il genitore, si potrebbe scrivere così:

```js
let shoppingDone = false;
let childAllowance;

// We don't need to explicitly specify 'shoppingDone === true'
if (shoppingDone) {
  childAllowance = 10;
} else {
  childAllowance = 5;
}
```

### Annidare if...else

È perfettamente valido inserire un'istruzione `if...else` all'interno di un'altra, ovvero annidarle. Per esempio, l'applicazione di previsioni meteorologiche potrebbe essere aggiornata per mostrare un ulteriore insieme di scelte in base alla temperatura:

```js
if (choice === "sunny") {
  if (temperature < 86) {
    para.textContent = `It is ${temperature} degrees outside — nice and sunny. Let's go out to the beach, or the park, and get an ice cream.`;
  } else if (temperature >= 86) {
    para.textContent = `It is ${temperature} degrees outside — REALLY HOT! If you want to go outside, make sure to put some sunscreen on.`;
  }
}
```

Anche se tutto il codice funziona insieme, ogni istruzione `if...else` opera in modo completamente indipendente dalle altre.

### Operatori logici: AND, OR e NOT

Se si desidera verificare più condizioni senza scrivere istruzioni `if...else` annidate, gli [operatori logici](/it/docs/Web/JavaScript/Reference/Operators) possono essere utili. Quando vengono usati nelle condizioni, i primi due fanno quanto segue:

- `&&` — AND; consente di concatenare due o più espressioni, in modo che tutte debbano essere valutate singolarmente come `true` affinché l'intera espressione restituisca `true`.
- `||` — OR; consente di concatenare due o più espressioni, in modo che una o più di esse debbano essere valutate singolarmente come `true` affinché l'intera espressione restituisca `true`.

Per un esempio di AND, lo snippet dell'esempio precedente può essere riscritto così:

```js
if (choice === "sunny" && temperature < 86) {
  para.textContent = `It is ${temperature} degrees outside — nice and sunny. Let's go out to the beach, or the park, and get an ice cream.`;
} else if (choice === "sunny" && temperature >= 86) {
  para.textContent = `It is ${temperature} degrees outside — REALLY HOT! If you want to go outside, make sure to put some sunscreen on.`;
}
```

Per esempio, il primo blocco di codice viene eseguito solo se sia `choice === 'sunny'` sia `temperature < 86` restituiscono `true`.

Vediamo un rapido esempio di OR:

```js
if (iceCreamVanOutside || houseStatus === "on fire") {
  console.log("You should leave the house quickly.");
} else {
  console.log("Probably should just stay in then.");
}
```

L'ultimo tipo di operatore logico, NOT, espresso dall'operatore `!`, può essere usato per negare un'espressione. Combiniamolo con OR nell'esempio precedente:

```js
if (!(iceCreamVanOutside || houseStatus === "on fire")) {
  console.log("Probably should just stay in then.");
} else {
  console.log("You should leave the house quickly.");
}
```

In questo snippet, se l'istruzione OR restituisce `true`, l'operatore NOT la nega in modo che l'espressione complessiva restituisca `false`.

È possibile combinare tutte le istruzioni logiche desiderate, con qualsiasi struttura. L'esempio seguente esegue il codice interno solo se entrambe le istruzioni OR restituiscono true, il che significa che l'istruzione AND complessiva restituirà true:

```js
if ((x === 5 || y > 3 || z <= 10) && (loggedIn || userName === "Steve")) {
  // run the code
}
```

Un errore comune nell'uso dell'operatore logico OR nelle istruzioni condizionali è tentare di indicare una sola volta la variabile di cui si sta verificando il valore e poi fornire un elenco dei valori che potrebbe assumere per restituire true, separati dagli operatori `||` (OR). Per esempio:

```js example-bad
if (x === 5 || 7 || 10 || 20) {
  // run my code
}
```

In questo caso, la condizione all'interno di `if ()` viene sempre valutata come true poiché 7, o qualsiasi altro valore diverso da zero, viene sempre valutato come `true`. Questa condizione afferma in realtà "se x è uguale a 5, oppure 7 è true, cosa che è sempre vera". Logicamente non è ciò che serve. Per far funzionare il codice, occorre specificare un test completo su entrambi i lati di ogni operatore OR:

```js
if (x === 5 || x === 7 || x === 10 || x === 20) {
  // run my code
}
```

## Istruzioni switch

Le istruzioni `if...else` svolgono bene il compito di abilitare il codice condizionale, ma non sono prive di svantaggi. Sono particolarmente adatte ai casi in cui sono disponibili alcune scelte, ciascuna delle quali richiede l'esecuzione di una quantità ragionevole di codice e/o le condizioni sono complesse, per esempio con più operatori logici. Nei casi in cui si desidera semplicemente impostare una variabile su una determinata scelta di valore o stampare un'istruzione specifica in base a una condizione, la sintassi può essere un po' macchinosa, soprattutto se sono disponibili molte scelte.

In questo caso, le [`switch` statements](/it/docs/Web/JavaScript/Reference/Statements/switch) sono utili: ricevono una singola espressione o valore come input e poi esaminano diverse scelte fino a trovarne una che corrisponde a quel valore, eseguendo il codice corrispondente. Ecco altro pseudocodice per farsi un'idea:

```js
switch (expression) {
  case choice1:
    // run this code
    break;

  case choice2:
    // run this code instead
    break;

  // include as many cases as you like

  default:
    // actually, just run this code
    break;
}
```

Qui sono presenti:

1. La parola chiave `switch`, seguita da una coppia di parentesi.
2. Un'espressione o un valore all'interno delle parentesi.
3. La parola chiave `case`, seguita da una possibile scelta per l'espressione o il valore e da due punti.
4. Del codice da eseguire se la scelta corrisponde all'espressione.
5. Un'istruzione `break`, seguita da un punto e virgola. Se la scelta precedente corrisponde all'espressione o al valore, il browser interrompe qui l'esecuzione del blocco di codice e passa al codice presente sotto l'istruzione switch.
6. Tutti gli altri casi desiderati (punti 3–5).
7. La parola chiave `default`, seguita esattamente dallo stesso schema di codice di uno dei casi (punti 3–5), tranne per il fatto che `default` non ha una scelta dopo di sé e non richiede l'istruzione `break`, poiché nel blocco non c'è comunque altro codice da eseguire. Questa è l'opzione predefinita che viene eseguita se nessuna scelta corrisponde.

> [!NOTE]
> Non è obbligatorio includere la sezione `default`: può essere omessa senza problemi se non vi è alcuna possibilità che l'espressione possa assumere un valore sconosciuto. Se questa possibilità esiste, è necessario includerla per gestire i casi sconosciuti.

### Un esempio di switch

Vediamo un esempio reale: riscriviamo invece l'applicazione di previsioni meteorologiche usando un'istruzione switch:

```html
<label for="weather">Select the weather type today: </label>
<select id="weather">
  <option value="">--Make a choice--</option>
  <option value="sunny">Sunny</option>
  <option value="rainy">Rainy</option>
  <option value="snowing">Snowing</option>
  <option value="overcast">Overcast</option>
</select>

<p></p>
```

```js
const select = document.querySelector("select");
const para = document.querySelector("p");

select.addEventListener("change", setWeather);

function setWeather() {
  const choice = select.value;

  switch (choice) {
    case "sunny":
      para.textContent =
        "It is nice and sunny outside today. Wear shorts! Go to the beach, or the park, and get an ice cream.";
      break;
    case "rainy":
      para.textContent =
        "Rain is falling outside; take a rain coat and an umbrella, and don't stay out for too long.";
      break;
    case "snowing":
      para.textContent =
        "The snow is coming down — it is freezing! Best to stay in with a cup of hot chocolate, or go build a snowman.";
      break;
    case "overcast":
      para.textContent =
        "It isn't raining, but the sky is grey and gloomy; it could turn any minute, so take a rain coat just in case.";
      break;
    default:
      para.textContent = "";
  }
}
```

{{ EmbedLiveSample('A_switch_example', '100%', 100, "", "") }}

> [!NOTE]
> È possibile anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-switch.html) (ed eseguirlo [dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/simple-switch.html).)

## Operatore ternario

C'è un ultimo elemento di sintassi da introdurre prima di passare ad alcuni esempi. L'[operatore ternario o condizionale](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator) è una breve sintassi che verifica una condizione e restituisce un valore o un'espressione se è `true`, e un altro se è `false`. Può essere utile in alcune situazioni e può richiedere molto meno codice di un blocco `if...else` quando sono disponibili due scelte selezionate mediante una condizione `true`/`false`. Lo pseudocodice è simile a questo:

```js-nolint
condition ? run this code : run this code instead
```

Vediamo quindi un esempio:

```js
const greeting = isBirthday
  ? "Happy birthday Mrs. Smith — we hope you have a great day!"
  : "Good morning Mrs. Smith.";
```

Qui è presente una variabile chiamata `isBirthday`: se è `true`, viene mostrato all'ospite un messaggio di buon compleanno; altrimenti, viene mostrato il saluto quotidiano standard.

### Esempio di operatore ternario

L'operatore ternario non serve soltanto a impostare valori di variabili: può anche eseguire funzioni o righe di codice, ovvero qualsiasi cosa. Il seguente esempio dal vivo mostra un semplice selettore di tema in cui lo stile del sito viene applicato usando un operatore ternario.

```html
<label for="theme">Select theme: </label>
<select id="theme">
  <option value="white">White</option>
  <option value="black">Black</option>
</select>

<h1>This is my website</h1>
```

```js
const select = document.querySelector("select");
const html = document.querySelector("html");
document.body.style.padding = "10px";

function update(bgColor, textColor) {
  html.style.backgroundColor = bgColor;
  html.style.color = textColor;
}

select.addEventListener("change", () =>
  select.value === "black"
    ? update("black", "white")
    : update("white", "black"),
);
```

{{ EmbedLiveSample('Ternary_operator_example', '100%', 300, "", "") }}

Qui è presente un elemento {{htmlelement('select')}} per scegliere un tema, nero o bianco, oltre a un semplice elemento {{htmlelement("Heading_Elements", "h1")}} per visualizzare il titolo di un sito web. È presente anche una funzione chiamata `update()`, che riceve due colori come parametri, ovvero input. Il colore di sfondo del sito web viene impostato sul primo colore fornito, mentre il colore del testo viene impostato sul secondo.

Infine, è presente anche un event listener [onchange](/it/docs/Web/API/HTMLElement/change_event) che esegue una funzione contenente un operatore ternario. Inizia con una condizione di test: `select.value === 'black'`. Se restituisce `true`, viene eseguita la funzione `update()` con i parametri nero e bianco, ottenendo così un colore di sfondo nero e un colore del testo bianco. Se restituisce `false`, viene eseguita la funzione `update()` con i parametri bianco e nero, invertendo così i colori del sito.

> [!NOTE]
> È possibile anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-ternary.html) (ed eseguirlo [dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/simple-ternary.html).)

## Implementare un calendario di base

In questo esempio, verrà completata un'applicazione di calendario di base. Nel codice sono presenti:

- Un elemento {{htmlelement("select")}} che consente all'utente di scegliere tra diversi mesi.
- Un gestore di eventi `change` per rilevare quando cambia il valore selezionato nel menu `<select>`.
- Una funzione chiamata `createCalendar()` che disegna il calendario e visualizza il mese corretto nell'elemento {{htmlelement("Heading_Elements", "h1")}}.

Per completare l'esempio:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio in MDN Playground.
2. Scrivere un'istruzione condizionale all'interno della funzione `createCalendar()`, subito sotto il commento `// ADD CONDITIONAL HERE`. Dovrebbe:
   1. Esaminare il mese selezionato, memorizzato nella variabile `choice`. Questo sarà il valore dell'elemento `<select>` dopo la modifica del valore, per esempio "January".
   2. Assegnare alla variabile `days` il numero di giorni del mese selezionato. Per farlo, occorre conoscere il numero di giorni di ciascun mese dell'anno. Ai fini di questo esempio, gli anni bisestili possono essere ignorati.

Suggerimenti:

- Si consiglia di usare OR logico per raggruppare più mesi in una singola condizione; molti di essi hanno lo stesso numero di giorni.
- Considerare quale numero di giorni sia il più comune e usarlo come valore predefinito.

In caso di errore, è possibile cancellare il proprio lavoro usando il pulsante _Reset_ in MDN Playground. Se si resta bloccati, è possibile visualizzare la soluzione sotto l'output dal vivo.

```html hidden live-sample___conditionals-1
<label for="month">Select month: </label>
<select id="month">
  <option value="January">January</option>
  <option value="February">February</option>
  <option value="March">March</option>
  <option value="April">April</option>
  <option value="May">May</option>
  <option value="June">June</option>
  <option value="July">July</option>
  <option value="August">August</option>
  <option value="September">September</option>
  <option value="October">October</option>
  <option value="November">November</option>
  <option value="December">December</option>
</select>

<h1></h1>

<ul></ul>
```

```css hidden live-sample___conditionals-1
html {
  font-family: sans-serif;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

body {
  margin: 10px;
  background: #f5f9fa;
}

* {
  box-sizing: border-box;
}

ul {
  padding-left: 0;
}

li {
  display: block;
  float: left;
  width: 25%;
  border: 2px solid white;
  padding: 5px;
  height: 40px;
  background-color: #4a2db6;
  color: white;
}
```

```js live-sample___conditionals-1
const select = document.querySelector("select");
const list = document.querySelector("ul");
const h1 = document.querySelector("h1");

select.addEventListener("change", () => {
  const choice = select.value;
  createCalendar(choice);
});

function createCalendar(month) {
  let days = 31;

  // ADD CONDITIONAL HERE

  list.textContent = "";
  h1.textContent = month;
  for (let i = 1; i <= days; i++) {
    const listItem = document.createElement("li");
    listItem.textContent = i;
    list.appendChild(listItem);
  }
}

select.value = "January";
createCalendar("January");
```

{{ EmbedLiveSample("conditionals-1", "100%", 550) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
const select = document.querySelector("select");
const list = document.querySelector("ul");
const h1 = document.querySelector("h1");

select.addEventListener("change", () => {
  const choice = select.value;
  createCalendar(choice);
});

function createCalendar(month) {
  let days = 31;

  if (month === "February") {
    days = 28;
  } else if (
    month === "April" ||
    month === "June" ||
    month === "September" ||
    month === "November"
  ) {
    days = 30;
  }

  list.textContent = "";
  h1.textContent = month;
  for (let i = 1; i <= days; i++) {
    const listItem = document.createElement("li");
    listItem.textContent = i;
    list.appendChild(listItem);
  }
}

select.value = "January";
createCalendar("January");
```

</details>

## Aggiungere altre scelte di colore

In questo esempio, verrà preso l'esempio dell'operatore ternario visto in precedenza e l'operatore ternario verrà convertito in un'istruzione switch per consentire di applicare più scelte al sito web. Osservare l'elemento {{htmlelement("select")}}: questa volta non ha due opzioni di tema, ma cinque.

Per completare l'esempio:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio in MDN Playground.
2. Aggiungere un'istruzione switch subito sotto il commento `// ADD SWITCH STATEMENT`:
   1. Dovrebbe accettare la variabile `choice` come espressione di input.
   2. Per ogni caso, la scelta dovrebbe essere uguale a uno dei possibili valori `<option>` selezionabili, ovvero `white`, `black`, `purple`, `yellow` oppure `psychedelic`. Si noti che i valori delle opzioni sono in minuscolo, mentre le _etichette_ delle opzioni, come visualizzate nell'output dal vivo, iniziano con una lettera maiuscola. Nel codice devono essere usati i valori in minuscolo.
   3. Per ogni caso, deve essere eseguita la funzione `update()` e devono essere passati due valori di colore: il primo per il colore di sfondo e il secondo per il colore del testo. Ricordare che i valori dei colori sono stringhe e devono quindi essere racchiusi tra virgolette.

In caso di errore, è possibile cancellare il proprio lavoro usando il pulsante _Reset_ in MDN Playground. Se si resta bloccati, è possibile visualizzare la soluzione sotto l'output dal vivo.

```html hidden live-sample___conditionals-2
<label for="theme">Select theme: </label>
<select id="theme">
  <option value="white">White</option>
  <option value="black">Black</option>
  <option value="purple">Purple</option>
  <option value="yellow">Yellow</option>
  <option value="psychedelic">Psychedelic</option>
</select>

<h1>This is my website</h1>
```

```css hidden live-sample___conditionals-2
html {
  font-family: sans-serif;
  height: 95%;
}

h2 {
  font-size: 16px;
}

.a11y-label {
  margin: 0;
  text-align: right;
  font-size: 0.7rem;
  width: 98%;
}

body {
  margin: 10px;
  height: inherit;
}
```

```js live-sample___conditionals-2
const select = document.querySelector("select");
const html = document.querySelector("html");

select.addEventListener("change", () => {
  const choice = select.value;

  // ADD SWITCH STATEMENT
});

function update(bgColor, textColor) {
  html.style.backgroundColor = bgColor;
  html.style.color = textColor;
}
```

{{ EmbedLiveSample("conditionals-2", "100%", 200) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
const select = document.querySelector("select");
const html = document.querySelector("html");

select.addEventListener("change", () => {
  const choice = select.value;

  switch (choice) {
    case "black":
      update("black", "white");
      break;
    case "white":
      update("white", "black");
      break;
    case "purple":
      update("purple", "white");
      break;
    case "yellow":
      update("yellow", "purple");
      break;
    case "psychedelic":
      update("lime", "purple");
      break;
  }
});

function update(bgColor, textColor) {
  html.style.backgroundColor = bgColor;
  html.style.color = textColor;
}
```

</details>

## Riepilogo

Questo è tutto ciò che occorre sapere per ora sulle strutture condizionali in JavaScript. Nel prossimo articolo verranno proposti alcuni test per verificare quanto queste informazioni siano state comprese e assimilate.

## Vedi anche

- [Operatori di confronto](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators)
- [Istruzioni condizionali in dettaglio](/it/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#conditional_statements)
- [Riferimento a if...else](/it/docs/Web/JavaScript/Reference/Statements/if...else)
- [Riferimento all'operatore condizionale (ternario)](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting/Test_your_skills/Conditionals", "Learn_web_development/Core/Scripting")}}
