---
title: Prendere decisioni nel codice — condizionali
short-title: Conditionals
slug: Learn_web_development/Core/Scripting/Conditionals
l10n:
  sourceCommit: 611edf6335e4a833a6f394d0d98b117e7b0a36bf
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting")}}

In qualsiasi linguaggio di programmazione, il codice deve prendere decisioni ed eseguire azioni corrispondenti a seconda dei diversi input. Per esempio, in un gioco, se il numero di vite di un giocatore è 0, allora il gioco è finito. In un'app meteo, se l'app viene consultata al mattino, mostra una grafica dell'alba; mostra stelle e una luna se è notte. In questo articolo, esploreremo come funzionano le cosiddette istruzioni condizionali in JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere cos'è un condizionale — una struttura di codice per eseguire diversi percorsi di codice a seconda del risultato di un test.</li>
          <li>Implementare condizioni usando <code>if</code>/<code>else</code>/<code>else if</code>.</li>
          <li>Usare operatori di confronto per creare test.</li>
          <li>Implementare la logica AND, OR, e NOT nei test.</li>
          <li>Istruzioni switch.</li>
          <li>Operatori ternari.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Puoi averlo a una condizione!

Gli esseri umani (e altri animali) prendono decisioni continuamente che influenzano le loro vite, da piccole ("mangio un biscotto o due?") a grandi ("dovrei rimanere nel mio paese e lavorare nella fattoria di famiglia, o dovrei trasferirmi in America e studiare astrofisica?")

Le istruzioni condizionali ci permettono di rappresentare tale decisione in JavaScript, dalla scelta che deve essere fatta (per esempio, "un biscotto o due"), all'esito risultante di tali scelte (forse l'esito di "mangiato un biscotto" potrebbe essere "ancora affamato", e l'esito di "mangiato due biscotti" potrebbe essere "sazio, ma la mamma mi ha sgridato per aver mangiato tutti i biscotti").

![Un personaggio cartoon somigliante a una persona che tiene un barattolo di biscotti etichettato 'Cookies'. C'è un punto interrogativo sopra la testa del personaggio. Ci sono due fumetti. Il fumetto a sinistra ha un biscotto. Il fumetto a destra ha due biscotti. Insieme implica che il personaggio sta cercando di decidere se vuole un biscotto o due biscotti.](cookie-choice-small.png)

## if...else statements

Diamo un'occhiata al tipo di istruzione condizionale più comune che userai in JavaScript — la modesta [`if...else` statement](/it/docs/Web/JavaScript/Reference/Statements/if...else).

### Sintassi base di if...else

La sintassi base di `if...else` si presenta così:

```js
if (condition) {
  /* code to run if condition is true */
} else {
  /* run some other code instead */
}
```

Qui abbiamo:

1. La parola chiave `if` seguita da alcune parentesi.
2. Una condizione da testare, posta all'interno delle parentesi (tipicamente "questo valore è maggiore di quest'altro valore?", o "questo valore esiste?"). La condizione fa uso degli [operatori di confronto](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators) di cui abbiamo discusso in precedenza nel modulo e restituisce `true` o `false`.
3. Un insieme di parentesi graffe, all'interno delle quali abbiamo del codice — questo può essere qualsiasi codice vogliamo, e viene eseguito solo se la condizione restituisce `true`.
4. La parola chiave `else`.
5. Un altro insieme di parentesi graffe, all'interno delle quali abbiamo più codice — questo può essere qualsiasi codice vogliamo, e viene eseguito solo se la condizione non è `true` — o in altre parole, la condizione è `false`.

Questo codice è abbastanza leggibile — sta dicendo "**se** la **condizione** restituisce `true`, esegui il codice A, **altrimenti** esegui il codice B".

Dovresti notare che non devi includere il `else` e il secondo blocco di parentesi graffe — il seguente è anche codice perfettamente legale:

```js
if (condition) {
  /* code to run if condition is true */
}

/* run some other code */
```

Tuttavia, bisogna fare attenzione qui — in questo caso, il secondo blocco di codice non è controllato dall'istruzione condizionale, quindi **viene sempre** eseguito, indipendentemente dal fatto che la condizione restituisca `true` o `false`. Questo non è necessariamente un male, ma potrebbe non essere ciò che vuoi — spesso vuoi eseguire un blocco di codice _o_ l'altro, non entrambi.

Come punto finale, anche se non è raccomandato, potresti talvolta vedere le istruzioni `if...else` scritte senza le parentesi graffe:

```js example-bad
if (condition) doSomething();
else doSomethingElse();
```

Questa sintassi è perfettamente valida, ma è molto più facile capire il codice se usi le parentesi graffe per delimitare i blocchi di codice e usi più righe e rientri.

### Un esempio reale

Per capire meglio questa sintassi, consideriamo un esempio reale. Immagina un bambino a cui viene chiesto di aiutare con una faccenda dalla madre o dal padre. Il genitore potrebbe dire "Ehi tesoro! Se mi aiuti andando a fare la spesa, ti darò una paghetta extra così potrai permetterti quel giocattolo che volevi." In JavaScript, potremmo rappresentarlo in questo modo:

```js
let shoppingDone = false;
let childAllowance;

if (shoppingDone === true) {
  childAllowance = 10;
} else {
  childAllowance = 5;
}
```

Questo codice, così come mostrato, risulta sempre nel fatto che la variabile `shoppingDone` restituisce `false`, portando delusione al nostro povero bambino. Sta a noi fornire un meccanismo per permettere al genitore di impostare la variabile `shoppingDone` su `true` se il bambino ha fatto la spesa.

> [!NOTE]
> Puoi vedere una [versione più completa di questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/allowance-updater.html) (vedi anche [l'esecuzione live](https://mdn.github.io/learning-area/javascript/building-blocks/allowance-updater.html)).

### else if

L'ultimo esempio ci ha fornito due scelte, o risultati — ma cosa succede se vogliamo più di due?

C'è un modo per concatenare ulteriori scelte/risultati al tuo `if...else` — usando `else if`. Ogni scelta extra richiede un blocco aggiuntivo da inserire tra `if () { }` e `else { }` — dai un'occhiata al seguente esempio più articolato, che potrebbe far parte di una semplice applicazione di previsione meteo:

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

1. Qui abbiamo un elemento HTML {{htmlelement("select")}} che ci permette di fare diverse scelte meteo, e un semplice paragrafo.
2. Nel JavaScript, stiamo memorizzando un riferimento sia all'elemento {{htmlelement("select")}} che al {{htmlelement("p")}}, e aggiungendo un event listener al `<select>` in modo che quando viene cambiato il suo valore, la funzione `setWeather()` venga eseguita.
3. Quando questa funzione viene eseguita, impostiamo prima una variabile chiamata `choice` sulla selezione attuale scelta nel `<select>`. Poi usiamo un'istruzione condizionale per mostrare testo diverso all'interno del paragrafo a seconda di quale sia il valore di `choice`. Nota come tutte le condizioni vengano testate in blocchi `else if () { }`, tranne per la prima, che viene testata in un blocco `if () { }`.
4. L'ultima scelta, all'interno del blocco `else { }`, è fondamentalmente un'opzione di "ultima risorsa" — il codice al suo interno verrà eseguito se nessuna delle condizioni è `true`. In questo caso, serve per svuotare il testo dal paragrafo se nulla è selezionato, per esempio, se un utente decide di riselezionare l'opzione placeholder "--Make a choice--" mostrata all'inizio.

> [!NOTE]
> Puoi anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-else-if.html) (vedi [l'esecuzione live](https://mdn.github.io/learning-area/javascript/building-blocks/simple-else-if.html)).

### Una nota sugli operatori di confronto

Gli operatori di confronto vengono usati per testare le condizioni all'interno delle istruzioni condizionali. Abbiamo esaminato per la prima volta gli operatori di confronto nel nostro articolo [Matematica di base in JavaScript — numeri e operatori](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators). Le nostre scelte sono:

- `===` e `!==` — testano se un valore è identico o non identico a un altro.
- `<` e `>` — testano se un valore è minore o maggiore di un altro.
- `<=` e `>=` — testano se un valore è minore o uguale, o maggiore o uguale a un altro.

Volevamo fare una menzione speciale della verifica dei valori booleani (`true`/`false`), e di un modello comune che incontrerai ancora e ancora. Qualsiasi valore che non sia `false`, `undefined`, `null`, `0`, `NaN`, o una stringa vuota (`''`) restituisce effettivamente `true` quando viene testato in un'istruzione condizionale, quindi puoi usare il nome di una variabile da solo per verificare se è `true`, o addirittura se esiste (cioè, non è undefined). Quindi per esempio:

```js
let cheese = "Cheddar";

if (cheese) {
  console.log("Yay! Cheese available for making cheese on toast.");
} else {
  console.log("No cheese on toast for you today.");
}
```

E, tornando al nostro esempio precedente sul bambino che fa una faccenda per il genitore, potresti scriverlo in questo modo:

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

### Nidificazione di if...else

È perfettamente lecito mettere un'istruzione `if...else` all'interno di un'altra — nidificarle. Per esempio, potremmo aggiornare la nostra applicazione di previsione meteo per mostrare una serie di scelte ulteriori a seconda di quale sia la temperatura:

```js
if (choice === "sunny") {
  if (temperature < 86) {
    para.textContent = `It is ${temperature} degrees outside — nice and sunny. Let's go out to the beach, or the park, and get an ice cream.`;
  } else if (temperature >= 86) {
    para.textContent = `It is ${temperature} degrees outside — REALLY HOT! If you want to go outside, make sure to put some sunscreen on.`;
  }
}
```

Anche se il codice funziona tutto insieme, ogni istruzione `if...else` funziona in modo completamente indipendente dall'altra.

### Operatori logici: AND, OR e NOT

Se vuoi testare più condizioni senza scrivere istruzioni `if...else` nidificate, gli [operatori logici](/it/docs/Web/JavaScript/Reference/Operators) possono aiutarti. Quando usati nelle condizioni, i primi due fanno quanto segue:

- `&&` — AND; permette di concatenare due o più espressioni in modo che tutte debbano singolarmente valutarsi come `true` affinché l'intera espressione restituisca `true`.
- `||` — OR; permette di concatenare due o più espressioni in modo che una o più di esse debba singolarmente valutarsi come `true` affinché l'intera espressione restituisca `true`.

Per darti un esempio di AND, il precedente stralcio di codice può essere riscritto così:

```js
if (choice === "sunny" && temperature < 86) {
  para.textContent = `It is ${temperature} degrees outside — nice and sunny. Let's go out to the beach, or the park, and get an ice cream.`;
} else if (choice === "sunny" && temperature >= 86) {
  para.textContent = `It is ${temperature} degrees outside — REALLY HOT! If you want to go outside, make sure to put some sunscreen on.`;
}
```

Quindi, per esempio, il primo blocco di codice sarà eseguito solo se `choice === 'sunny'` _e_ `temperature < 86` restituiscono `true`.

Osserviamo rapidamente un esempio di OR:

```js
if (iceCreamVanOutside || houseStatus === "on fire") {
  console.log("You should leave the house quickly.");
} else {
  console.log("Probably should just stay in then.");
}
```

L'ultimo tipo di operatore logico, NOT, espresso dall'operatore `!`, può essere usato per negare un'espressione. Combiniamolo con OR nell'esempio sopra:

```js
if (!(iceCreamVanOutside || houseStatus === "on fire")) {
  console.log("Probably should just stay in then.");
} else {
  console.log("You should leave the house quickly.");
}
```

In questo brano di codice, se l'istruzione OR restituisce `true`, l'operatore NOT la negherà in modo che l'espressione complessiva restituisca `false`.

Puoi combinare tra loro quanti più enunciati logici desideri, in qualsiasi struttura. L'esempio seguente esegue il codice solo se entrambi gli enunciati OR restituiscono true, il che significa che l'enunciato AND complessivo restituirà true:

```js
if ((x === 5 || y > 3 || z <= 10) && (loggedIn || userName === "Steve")) {
  // run the code
}
```

Un errore comune quando si utilizza l'operatore logico OR nelle istruzioni condizionali è provare a menzionare la variabile il cui valore si sta controllando una sola volta, e poi fornire un elenco di valori che potrebbe avere per restituire true, separati da operatori `||` (OR). Per esempio:

```js example-bad
if (x === 5 || 7 || 10 || 20) {
  // run my code
}
```

In questo caso, la condizione all'interno di `if ()` si valuterà sempre come true poiché 7 (o qualsiasi altro valore diverso da zero) si valuta sempre come `true`. Questa condizione sta effettivamente dicendo "se x è uguale a 5, o 7 è vero — cosa che è sempre". Questo logicamente non è ciò che vogliamo! Per farlo funzionare, devi specificare un test completo su entrambi i lati di ciascun operatore OR:

```js
if (x === 5 || x === 7 || x === 10 || x === 20) {
  // run my code
}
```

## switch statements

Le istruzioni `if...else` svolgono bene il compito di abilitare il codice condizionale, ma non sono prive di difetti. Sono principalmente buone per i casi in cui hai un paio di scelte, e ognuna di esse richiede una quantità ragionevole di codice da eseguire, e/o le condizioni sono complesse (per esempio, multipli operatori logici). Per i casi in cui vuoi solo impostare una variabile su una certa scelta di valore o stampare una particolare dichiarazione a seconda di una condizione, la sintassi può risultare un po' macchinosa, specialmente se hai un gran numero di scelte.

In tal caso, le [`switch` statements](/it/docs/Web/JavaScript/Reference/Statements/switch) sono tue amiche — prendono una singola espressione/valore come input, e poi cercano attraverso diverse scelte fino a trovare quella che corrisponde a quel valore, eseguendo il codice corrispondente che va insieme ad essa. Ecco un po' di pseudocodice per darti un'idea:

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

Qui abbiamo:

1. La parola chiave `switch`, seguita da un set di parentesi.
2. Un'espressione o un valore all'interno delle parentesi.
3. La parola chiave `case`, seguita da una scelta che l'espressione/valore potrebbe essere, seguita da due punti.
4. Alcuni codici da eseguire se la scelta corrisponde all'espressione.
5. Un'istruzione `break`, seguita da un punto e virgola. Se la scelta precedente corrisponde all'espressione/valore, il browser smette di eseguire il blocco di codice qui e passa a qualsiasi codice che appare sotto l'istruzione switch.
6. Quante altre case (punti 3–5) desideri.
7. La parola chiave `default`, seguita esattamente dallo stesso schema di codice di uno dei case (punti 3–5), eccetto che `default` non ha una scelta dopo di esso, e non hai bisogno dell'istruzione `break` poiché non c'è nulla da eseguire in seguito in quel blocco. Questa è l'opzione predefinita che viene eseguita se nessuna delle scelte corrisponde.

> [!NOTE]
> Non devi includere la sezione `default` — puoi ometterla tranquillamente se non c'è possibilità che l'espressione possa finire per essere uguale a un valore sconosciuto. Tuttavia, se c'è la possibilità di ciò, hai bisogno di includerla per gestire i casi sconosciuti.

### Un esempio di switch

Diamo un'occhiata a un esempio reale — riscriveremo la nostra applicazione di previsione meteo utilizzando un'istruzione switch instead:

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
> Puoi anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-switch.html) (vedi [l'esecuzione live](https://mdn.github.io/learning-area/javascript/building-blocks/simple-switch.html)).

## Operatore ternario

C'è un ultimo pezzo di sintassi che vogliamo introdurti prima che ti metti a giocare con alcuni esempi. L'[operatore condizionale o ternario](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator) è un piccolo pezzo di sintassi che testa una condizione e restituisce un valore/espressione se è `true`, e un altro se è `false` — questo può essere utile in alcune situazioni, e può occupare molto meno codice di un blocco `if...else` se hai due scelte tra cui scegliere tramite una condizione `true`/`false`. Lo pseudocodice appare così:

```js-nolint
condition ? run this code : run this code instead
```

Vediamo quindi un esempio:

```js
const greeting = isBirthday
  ? "Happy birthday Mrs. Smith — we hope you have a great day!"
  : "Good morning Mrs. Smith.";
```

Qui abbiamo una variabile chiamata `isBirthday` — se questa è `true`, diamo al nostro ospite un messaggio di buon compleanno; se no, le diamo il saluto quotidiano standard.

### Esempio di operatore ternario

L'operatore ternario non è solo per impostare valori di variabili; puoi anche eseguire funzioni, o righe di codice — tutto ciò che vuoi. Il seguente esempio dal vivo mostra un semplice selettore di tema in cui lo stile del sito viene applicato utilizzando un operatore ternario.

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

Qui abbiamo un elemento {{htmlelement('select')}} per scegliere un tema (nero o bianco), oltre a un semplice {{htmlelement("Heading_Elements", "h1")}} per visualizzare un titolo del sito web. Abbiamo anche una funzione chiamata `update()`, che prende due colori come parametri (input). Il colore di sfondo del sito web viene impostato sul primo colore fornito, e il suo colore del testo sul secondo colore fornito.

Infine, abbiamo anche un event listener [onchange](/it/docs/Web/API/HTMLElement/change_event) che serve per eseguire una funzione contenente un operatore ternario. Inizia con una condizione di test — `select.value === 'black'`. Se questa restituisce `true`, eseguiamo la funzione `update()` con parametri di nero e bianco, il che significa che abbiamo un colore di sfondo nero e un colore del testo bianco. Se restituisce `false`, eseguiamo la funzione `update()` con parametri di bianco e nero, il che significa che i colori del sito sono invertiti.

> [!NOTE]
> Puoi anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-ternary.html) (vedi [l'esecuzione live](https://mdn.github.io/learning-area/javascript/building-blocks/simple-ternary.html)).

## Apprendimento attivo: Un semplice calendario

In questo esempio, dovrai aiutarci a completare una semplice applicazione di calendario. Nel codice hai:

- Un elemento {{htmlelement("select")}} per permettere all'utente di scegliere tra diversi mesi.
- Un gestore di eventi `onchange` per rilevare quando il valore selezionato nel menu `<select>` cambia.
- Una funzione chiamata `createCalendar()` che disegna il calendario e visualizza il mese corretto nell'elemento {{htmlelement("Heading_Elements", "h1")}}.

Abbiamo bisogno che tu scriva un'istruzione condizionale all'interno della funzione `createCalendar()`, appena sotto il commento `// ADD CONDITIONAL HERE`. Dovrebbe:

1. Guardare il mese selezionato (memorizzato nella variabile `choice`. Questo sarà il valore dell'elemento `<select>` dopo che il valore cambia, quindi "Gennaio", per esempio.)
2. Assegnare la variabile `days` per essere uguale al numero di giorni nel mese selezionato. Per fare ciò, dovrai cercare il numero di giorni in ciascun mese dell'anno. Puoi ignorare gli anni bisestili per questo esempio.

Suggerimenti:

- Ti consigliamo di usare OR logico per raggruppare più mesi insieme in una singola condizione; molti di essi condividono lo stesso numero di giorni.
- Pensa a qual è il numero di giorni più comune, e usalo come valore predefinito.

Se commetti un errore, puoi sempre reimpostare l'esempio con il pulsante "Reset". Se sei veramente bloccato, premi "Mostra soluzione" per vedere una soluzione.

```html hidden
<h2>Live output</h2>
<iframe id="output" width="100%" height="600px"></iframe>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="height: 400px;width: 95%">
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
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
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
```

```js hidden
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const outputIFrame = document.querySelector("#output");
const textarea = document.getElementById("code");
const initialCode = textarea.value;
let userCode = textarea.value;

const solutionCode = `const select = document.querySelector("select");
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
createCalendar("January");`;

function outputDocument(code) {
  const outputBody = `
<div class="output" style="height: 500px; overflow: auto">
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
</div>`;

  const outputStyle = `
.output * {
  box-sizing: border-box;
}

.output ul {
  padding-left: 0;
}

.output li {
  display: block;
  float: left;
  width: 25%;
  border: 2px solid white;
  padding: 5px;
  height: 40px;
  background-color: #4a2db6;
  color: white;
}
html {
  font-family: sans-serif;
}

h2 {
  font-size: 16px;
}`;
  return `
<!doctype html>
<html>
  <head>
    <style>${outputStyle}</style>
  </head>
  <body>
    ${outputBody}
    <script>${code}<${"/"}script>
  </body>
</html>`;
}

function update() {
  output.setAttribute("srcdoc", outputDocument(textarea.value));
}

update();

textarea.addEventListener("input", update);

reset.addEventListener("click", () => {
  textarea.value = initialCode;
  userEntry = textarea.value;
  solution.value = "Show solution";
  update();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    // remember the state of the user's code
    // so we can restore it
    userCode = textarea.value;
    textarea.value = solutionCode;
    solution.value = "Hide solution";
  } else {
    textarea.value = userCode;
    solution.value = "Show solution";
  }
  update();
});

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead
textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;
  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );

  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}
```

{{ EmbedLiveSample('Active_learning_A_simple_calendar', '100%', 1210) }}

## Apprendimento attivo: Più scelte di colore

In questo esempio, prenderai l'esempio dell'operatore ternario che abbiamo visto prima e convertirai l'operatore ternario in un'istruzione switch per permetterci di applicare più scelte al semplice sito web. Guarda il {{htmlelement("select")}} — questa volta vedrai che ha cinque opzioni di tema, non due. Devi aggiungere un'istruzione switch appena sotto il commento `// ADD SWITCH STATEMENT`:

- Dovrebbe accettare la variabile `choice` come espressione di input.
- Per ogni caso, la scelta dovrebbe essere uguale a uno dei possibili valori `<option>` che possono essere selezionati, ovvero, `white`, `black`, `purple`, `yellow`, o `psychedelic`. Nota che i valori delle opzioni sono in minuscolo, mentre le etichette delle opzioni, come visualizzate nell'output dal vivo, sono in maiuscolo. Dovresti usare i valori in minuscolo nel tuo codice.
- Per ogni caso, la funzione `update()` dovrebbe essere eseguita, e ricevere due valori di colore, il primo per il colore di sfondo e il secondo per il colore del testo. Ricorda che i valori di colore sono stringhe, quindi devono essere racchiusi tra virgolette.

Se commetti un errore, puoi sempre reimpostare l'esempio con il pulsante "Reset". Se sei veramente bloccato, premi "Mostra soluzione" per vedere una soluzione.

```html hidden
<h2>Live output</h2>
<iframe id="output" width="100%" height="350px"></iframe>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="height: 400px;width: 95%">
const select = document.querySelector('select');
const html = document.querySelector('.output');

select.addEventListener('change', () => {
  const choice = select.value;

  // ADD SWITCH STATEMENT
});

function update(bgColor, textColor) {
  html.style.backgroundColor = bgColor;
  html.style.color = textColor;
}
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css hidden
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
```

```js hidden
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
const outputIFrame = document.querySelector("#output");
const textarea = document.getElementById("code");
const initialCode = textarea.value;
let userCode = textarea.value;

const solutionCode = `const select = document.querySelector('select');
const html = document.querySelector('.output');

select.addEventListener('change', () => {
  const choice = select.value;

  switch(choice) {
    case 'black':
      update('black','white');
      break;
    case 'white':
      update('white','black');
      break;
    case 'purple':
      update('purple','white');
      break;
    case 'yellow':
      update('yellow','purple');
      break;
    case 'psychedelic':
      update('lime','purple');
      break;
  }
});

function update(bgColor, textColor) {
  html.style.backgroundColor = bgColor;
  html.style.color = textColor;
}`;

function outputDocument(code) {
  const outputBody = `
<div class="output" style="height: 300px;">
  <label for="theme">Select theme: </label>
  <select id="theme">
    <option value="white">White</option>
    <option value="black">Black</option>
    <option value="purple">Purple</option>
    <option value="yellow">Yellow</option>
    <option value="psychedelic">Psychedelic</option>
  </select>

  <h1>This is my website</h1>
</div>`;

  return `
<!doctype html>
<html>
  <head>
  </head>
  <body>
    ${outputBody}
    <script>${code}<${"/"}script>
  </body>
</html>`;
}

function update() {
  output.setAttribute("srcdoc", outputDocument(textarea.value));
}

update();

textarea.addEventListener("input", update);

reset.addEventListener("click", () => {
  textarea.value = initialCode;
  userEntry = textarea.value;
  solution.value = "Show solution";
  update();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    // remember the state of the user's code
    // so we can restore it
    userCode = textarea.value;
    textarea.value = solutionCode;
    solution.value = "Hide solution";
  } else {
    textarea.value = userCode;
    solution.value = "Show solution";
  }
  update();
});

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead
textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
    textarea.blur();
  }
};

function insertAtCaret(text) {
  const scrollPos = textarea.scrollTop;
  let caretPos = textarea.selectionStart;
  const front = textarea.value.substring(0, caretPos);
  const back = textarea.value.substring(
    textarea.selectionEnd,
    textarea.value.length,
  );

  textarea.value = front + text + back;
  caretPos += text.length;
  textarea.selectionStart = caretPos;
  textarea.selectionEnd = caretPos;
  textarea.focus();
  textarea.scrollTop = scrollPos;
}
```

{{ EmbedLiveSample('Active_learning_More_color_choices', '100%', 950) }}

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Condizionali](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Conditionals).

## Riassunto

Questo è tutto ciò che hai davvero bisogno di sapere sulle strutture condizionali in JavaScript per ora! Prossimamente, esamineremo il looping attraverso il codice.

## Vedi anche

- [Operatori di confronto](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators)
- [Istruzioni condizionali in dettaglio](/it/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#conditional_statements)
- [Riferimento if...else](/it/docs/Web/JavaScript/Reference/Statements/if...else)
- [Riferimento all'operatore condizionale (ternario)](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting")}}
