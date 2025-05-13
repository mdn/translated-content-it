---
title: Prendere decisioni nel codice — condizionali
short-title: Conditionals
slug: Learn_web_development/Core/Scripting/Conditionals
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting")}}

In qualsiasi linguaggio di programmazione, il codice deve prendere decisioni ed eseguire azioni a seconda degli input diversi. Ad esempio, in un gioco, se il numero di vite del giocatore è 0, allora il gioco è finito. In un'app meteo, se viene consultata al mattino, mostra un grafico dell'alba; mostra stelle e luna se è notte. In questo articolo, esploreremo come funzionano le cosiddette istruzioni condizionali in JavaScript.

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
          <li>Comprendere cosa sia un condizionale — una struttura di codice per eseguire percorsi di codice differenti a seconda di un risultato di test.</li>
          <li>Implementare condizioni utilizzando <code>if</code>/<code>else</code>/<code>else if</code>.</li>
          <li>Usare operatori di confronto per creare test.</li>
          <li>Implementare la logica AND, OR e NOT nei test.</li>
          <li>Istruzioni switch.</li>
          <li>Operatori ternari.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Può averlo a una condizione!

Gli esseri umani (e altri animali) prendono decisioni continuamente che influenzano le loro vite, dalle piccole ("dovrei mangiare un biscotto o due?") alle grandi ("dovrei rimanere nel mio paese e lavorare nella fattoria della mia famiglia o trasferirmi in America e studiare astrofisica?")

Le istruzioni condizionali ci permettono di rappresentare tali processi decisionali in JavaScript, dalla scelta che deve essere fatta (ad esempio, "un biscotto o due"), al risultato di tali scelte (magari il risultato di "ho mangiato un biscotto" potrebbe essere "mi sentivo ancora affamato", e il risultato di "ho mangiato due biscotti" potrebbe essere "mi sono sentito sazio, ma mia madre mi ha sgridato per aver mangiato tutti i biscotti").

![Un personaggio simile a un cartone animato che tiene in mano un barattolo di biscotti etichettato 'Cookies'. C'è un punto interrogativo sopra la testa del personaggio. Ci sono due nuvolette di dialogo. La nuvoletta di dialogo a sinistra ha un biscotto. La nuvoletta di dialogo a destra ha due biscotti. Insieme implica che il personaggio sta cercando di decidere se vuole un biscotto o due biscotti.](cookie-choice-small.png)

## Istruzioni if...else

Vediamo il tipo di istruzione condizionale più comune che utilizzerà in JavaScript — l'umile [`if...else` statement](/it/docs/Web/JavaScript/Reference/Statements/if...else).

### Sintassi base if...else

La sintassi base `if...else` appare così:

```js
if (condition) {
  /* code to run if condition is true */
} else {
  /* run some other code instead */
}
```

Qui abbiamo:

1. La parola chiave `if` seguita da alcune parentesi.
2. Una condizione da testare, posta all'interno delle parentesi (tipicamente "questo valore è maggiore di quest'altro valore?", o "questo valore esiste?"). La condizione utilizza gli [operatori di confronto](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators) di cui abbiamo parlato in precedenza nel modulo e restituisce `true` o `false`.
3. Un set di parentesi graffe, all'interno delle quali abbiamo del codice — questo può essere qualsiasi codice vogliamo, e viene eseguito solo se la condizione restituisce `true`.
4. La parola chiave `else`.
5. Un altro set di parentesi graffe, all'interno del quale abbiamo un altro po' di codice — questo può essere qualsiasi codice vogliamo, e viene eseguito solo se la condizione non è `true` — o in altre parole, la condizione è `false`.

Questo codice è abbastanza leggibile per un umano — sta dicendo "**se** la **condizione** restituisce `true`, esegui il codice A, **altrimenti** esegui il codice B".

Deve notare che non è obbligato a includere l'`else` e il secondo blocco di parentesi graffe — il seguente è anche un codice perfettamente legale:

```js
if (condition) {
  /* code to run if condition is true */
}

/* run some other code */
```

Tuttavia, deve essere prudente qui — in questo caso, il secondo blocco di codice non è controllato dall'istruzione condizionale, quindi viene eseguito **sempre**, indipendentemente dal fatto che la condizione restituisca `true` o `false`. Questo non è necessariamente un male, ma potrebbe non essere ciò che desidera — spesso si desidera eseguire un blocco di codice _o_ l'altro, non entrambi.

Come ultimo punto, anche se non è raccomandato, a volte può vedere istruzioni `if...else` scritte senza le parentesi graffe:

```js example-bad
if (condition) /* code to run if condition is true */
else /* run some other code instead */
```

Questa sintassi è perfettamente valida, ma è molto più facile capire il codice se usa le parentesi graffe per delimitare i blocchi di codice, e usa più righe e rientro.

### Un esempio reale

Per capire meglio questa sintassi, consideriamo un esempio reale. Immagini un bambino a cui viene chiesto di aiutare con un compito dalla madre o dal padre. Il genitore potrebbe dire "Ehi tesoro! Se mi aiuti andando a fare la spesa, ti darò una paghetta extra così potrai permetterti quel giocattolo che volevi." In JavaScript, potremmo rappresentare questo così:

```js
let shoppingDone = false;
let childAllowance;

if (shoppingDone === true) {
  childAllowance = 10;
} else {
  childAllowance = 5;
}
```

Questo codice come mostrato risulta sempre nella variabile `shoppingDone` che ritorna `false`, il che significa delusione per il nostro povero bambino. Tocca a noi fornire un meccanismo per consentire al genitore di impostare la variabile `shoppingDone` su `true` se il bambino ha fatto la spesa.

> [!NOTE]
> Potete vedere una [versione più completa di questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/allowance-updater.html) (vedete anche l’esecuzione [dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/allowance-updater.html)).

### else if

L'ultimo esempio ci ha fornito due scelte, o risultati — ma cosa succede se desideriamo più di due?

Esiste un modo per concatenare ulteriori scelte/risultati al suo `if...else` — utilizzando `else if`. Ogni scelta aggiuntiva richiede un ulteriore blocco da inserire tra `if () { }` e `else { }` — consulti il seguente esempio più complesso, che potrebbe essere parte di una semplice applicazione di previsione meteorologica:

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

1. Qui abbiamo un elemento HTML {{htmlelement("select")}} che ci consente di effettuare varie scelte meteorologiche, e un semplice paragrafo.
2. Nel JavaScript, noi memorizziamo un riferimento sia agli elementi {{htmlelement("select")}} che {{htmlelement("p")}}, e aggiungiamo un rilevatore di eventi all'elemento `<select>` in modo tale che quando il suo valore viene cambiato, la funzione `setWeather()` venga eseguita.
3. Quando questa funzione è eseguita, prima impostiamo una variabile chiamata `choice` al valore corrente selezionato nell'elemento `<select>`. Quindi utilizziamo un'istruzione condizionale per mostrare testo diverso all'interno del paragrafo a seconda del valore di `choice`. Si noti come tutte le condizioni sono testate nei blocchi `else if () { }`, tranne la prima, che è testata in un blocco `if () { }`.
4. L'ultima scelta, all'interno del blocco `else { }`, è fondamentalmente un'opzione di "ultima risorsa" — il codice al suo interno verrà eseguito se nessuna delle condizioni è `true`. In questo caso, serve per svuotare il testo dal paragrafo se non è selezionato nulla, ad esempio, se un utente decide di ri-selezionare l'opzione di segnaposto "--Make a choice--" mostrata all'inizio.

> [!NOTE]
> Può anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-else-if.html) ([vederlo girare dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/simple-else-if.html) anche lì.)

### Una nota sugli operatori di confronto

Gli operatori di confronto sono usati per testare le condizioni all'interno delle nostre istruzioni condizionali. Abbiamo esaminato per la prima volta gli operatori di confronto nel nostro articolo su [Fondamenti di matematica in JavaScript — numeri e operatori](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators). Le nostre scelte sono:

- `===` e `!==` — testano se un valore è identico a, o non identico a, un altro.
- `<` e `>` — testano se un valore è minore di o maggiore di un altro.
- `<=` e `>=` — testano se un valore è minore di o uguale a, o maggiore di o uguale a, un altro.

Volevamo fare una menzione speciale ai valori booleani di test (`true`/`false`), e a un modello comune che incontrerà ripetutamente. Qualsiasi valore che non è `false`, `undefined`, `null`, `0`, `NaN`, o una stringa vuota (`''`) in realtà restituisce `true` quando testato come istruzione condizionale, quindi può usare un nome di variabile da solo per testare se è `true`, o anche se esiste (cioè, non è indefinito). Quindi, ad esempio:

```js
let cheese = "Cheddar";

if (cheese) {
  console.log("Yay! Cheese available for making cheese on toast.");
} else {
  console.log("No cheese on toast for you today.");
}
```

E, tornando al nostro esempio precedente sul bambino che fa un compito per il genitore, potrebbe scriverlo così:

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

### Annidamento di if...else

È perfettamente accettabile inserire un'istruzione `if...else` all'interno di un'altra — annidarle. Ad esempio, potremmo aggiornare la nostra applicazione meteo per mostrare un ulteriore set di scelte a seconda di quale sia la temperatura:

```js
if (choice === "sunny") {
  if (temperature < 86) {
    para.textContent = `It is ${temperature} degrees outside — nice and sunny. Let's go out to the beach, or the park, and get an ice cream.`;
  } else if (temperature >= 86) {
    para.textContent = `It is ${temperature} degrees outside — REALLY HOT! If you want to go outside, make sure to put some sunscreen on.`;
  }
}
```

Anche se tutto il codice lavora insieme, ciascuna istruzione `if...else` funziona completamente indipendente dalle altre.

### Operatori logici: AND, OR e NOT

Se desidera testare più condizioni senza scrivere istruzioni `if...else` annidate, gli [operatori logici](/it/docs/Web/JavaScript/Reference/Operators) possono aiutarla. Quando utilizzati nelle condizioni, i primi due fanno quanto segue:

- `&&` — AND; le permette di concatenare due o più espressioni in modo che tutte debbano individualmente valutare `true` affinché l'espressione complessiva restituisca `true`.
- `||` — OR; le permette di concatenare due o più espressioni in modo tale che una o più di esse debbano individualmente valutare `true` affinché l'espressione complessiva restituisca `true`.

Per darle un esempio di AND, il frammento di esempio precedente può essere riscritto in questo modo:

```js
if (choice === "sunny" && temperature < 86) {
  para.textContent = `It is ${temperature} degrees outside — nice and sunny. Let's go out to the beach, or the park, and get an ice cream.`;
} else if (choice === "sunny" && temperature >= 86) {
  para.textContent = `It is ${temperature} degrees outside — REALLY HOT! If you want to go outside, make sure to put some sunscreen on.`;
}
```

Ad esempio, il primo blocco di codice verrà eseguito solo se `choice === 'sunny'` _e_ `temperature < 86` restituiscono `true`.

Vediamo un rapido esempio OR:

```js
if (iceCreamVanOutside || houseStatus === "on fire") {
  console.log("You should leave the house quickly.");
} else {
  console.log("Probably should just stay in then.");
}
```

L'ultimo tipo di operatore logico, NOT, espresso dall'operatore `!`, può essere utilizzato per negare un'espressione. Combiniamolo con l'OR nell'esempio sopra:

```js
if (!(iceCreamVanOutside || houseStatus === "on fire")) {
  console.log("Probably should just stay in then.");
} else {
  console.log("You should leave the house quickly.");
}
```

In questo frammento, se l'istruzione OR restituisce `true`, l'operatore NOT lo negherà in modo che l'espressione complessiva restituisca `false`.

Può combinare quanti più operatori logici desidera, in qualunque struttura. Il seguente esempio esegue il codice solo se le due istruzioni OR restituiscono true, significando che l'intera istruzione AND restituirà true:

```js
if ((x === 5 || y > 3 || z <= 10) && (loggedIn || userName === "Steve")) {
  // run the code
}
```

Un errore comune quando si utilizza l'operatore logico OR nelle istruzioni condizionali è provare a dichiarare la variabile di cui si controlla il valore una sola volta, quindi fornire un elenco di valori che potrebbe assumere per restituire true, separati da operatori `||` (OR). Ad esempio:

```js example-bad
if (x === 5 || 7 || 10 || 20) {
  // run my code
}
```

In questo caso, la condizione all'interno di `if ()` sarà sempre valutata come vera poiché 7 (o qualsiasi altro valore diverso da zero) si valuta sempre come `true`. Questa condizione sta in realtà dicendo "se x è uguale a 5, oppure 7 è vero — cosa che è sempre". Questo non è logicamente ciò che vogliamo! Per far funzionare questo deve specificare un test completo su ciascun lato di ogni operatore OR:

```js
if (x === 5 || x === 7 || x === 10 || x === 20) {
  // run my code
}
```

## Istruzioni switch

Le istruzioni `if...else` svolgono bene il compito di abilitare il codice condizionale, ma non sono prive di svantaggi. Sono principalmente buone per i casi in cui ha un paio di scelte, e ciascuna richiede una quantità ragionevole di codice da eseguire, e/o le condizioni sono complesse (ad esempio, più operatori logici). Per i casi in cui desidera semplicemente impostare una variabile su una determinata scelta di valore o stampare una determinata affermazione a seconda di una condizione, la sintassi può essere un po' ingombrante, specialmente se ha un gran numero di scelte.

In tal caso, le [istruzioni `switch`](/it/docs/Web/JavaScript/Reference/Statements/switch) sono sue amiche — prendono come input un'espressione/valore singolo, e quindi esaminano diverse scelte fino a trovarne una che corrisponde a tale valore, eseguendo il codice corrispondente associato. Ecco un po' di pseudocodice, per darle un'idea:

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

1. La parola chiave `switch`, seguita da un insieme di parentesi.
2. Un'espressione o valore all'interno delle parentesi.
3. La parola chiave `case`, seguita da una scelta che l'espressione/valore potrebbe essere, seguita da un due punti.
4. Un po' di codice da eseguire se la scelta corrisponde all'espressione.
5. Un'istruzione `break`, seguita da un punto e virgola. Se la scelta precedente corrisponde all'espressione/valore, il browser termina l'esecuzione del blocco di codice qui, e passa a qualsiasi codice che appare sotto l'istruzione switch.
6. Quante altre casistiche (punti 3–5) voglia.
7. La parola chiave `default`, seguita esattamente dallo stesso schema di codice di uno dei casi (punti 3–5), ad eccezione del fatto che `default` non ha una scelta dopo di essa, e non è necessario il `break` dato che non c'è niente da eseguire dopo di esso nel blocco comunque. Questa è l'opzione predefinita che viene eseguita se nessuna delle scelte corrisponde.

> [!NOTE]
> Non è obbligato a includere la sezione `default` — può ometterla in sicurezza se non c'è possibilità che l'espressione possa risultare uguale a un valore sconosciuto. Se c'è una possibilità di ciò, tuttavia, deve includerla per gestire casi sconosciuti.

### Un esempio di switch

Diamo un'occhiata a un esempio reale — riscriveremo la nostra applicazione meteo per utilizzare un'istruzione switch al suo posto:

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
> Può anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-switch.html) (vederlo [in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/simple-switch.html) anche lì.)

## Operatore ternario

C'è un ultimo bit di sintassi che vogliamo introdurre prima di farti giocare con alcuni esempi. L'[operatore ternario o condizionale](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator) è un piccolo pezzo di sintassi che testa una condizione e restituisce un valore/espressione se è `true`, e un altro se è `false` — questo può essere utile in alcune situazioni, e può occupare molto meno codice rispetto a un blocco `if...else` se disponi di due scelte che sono selezionate tramite una condizione `true`/`false`. Il pseudocodice sembra così:

```js-nolint
condition ? run this code : run this code instead
```

Quindi vediamo un esempio:

```js
const greeting = isBirthday
  ? "Happy birthday Mrs. Smith — we hope you have a great day!"
  : "Good morning Mrs. Smith.";
```

Qui abbiamo una variabile chiamata `isBirthday` — se questo è `true`, diamo al nostro ospite un messaggio di buon compleanno; altrimenti, le diamo il saluto giornaliero standard.

### Esempio di operatore ternario

L'operatore ternario non è solo per impostare i valori delle variabili; può anche eseguire funzioni, o righe di codice — tutto ciò che vuole. Il seguente esempio dal vivo mostra un semplice selettore di temi dove lo stile per il sito è applicato utilizzando un operatore ternario.

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

Qui abbiamo un elemento {{htmlelement('select')}} per scegliere un tema (nero o bianco), inoltre un semplice {{htmlelement("Heading_Elements", "h1")}} per visualizzare un titolo del sito. Abbiamo anche una funzione chiamata `update()` che accetta due colori come parametri (input). Il colore di sfondo del sito viene impostato al primo colore fornito, e il colore del testo viene impostato al secondo colore fornito.

Infine, abbiamo anche un gestore di eventi [onchange](/it/docs/Web/API/HTMLElement/change_event) che serve a eseguire una funzione contenente un operatore ternario. Inizia con una condizione di test — `select.value === 'black'`. Se questo restituisce `true`, eseguiamo la funzione `update()` con parametri di nero e bianco, il che significa che finiamo con un colore di sfondo nero e un colore di testo bianco. Se restituisce `false`, eseguiamo la funzione `update()` con parametri di bianco e nero, il che significa che i colori del sito sono invertiti.

> [!NOTE]
> Può anche [trovare questo esempio su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/simple-ternary.html) (vederlo [in esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/simple-ternary.html) anche lì.)

## Apprendimento attivo: Un calendario semplice

In questo esempio, ci aiuterà a completare una semplice applicazione di calendario. Nel codice ha:

- Un elemento {{htmlelement("select")}} per consentire all'utente di scegliere tra diversi mesi.
- Un gestore di eventi `onchange` per rilevare quando il valore selezionato nel menu `<select>` viene modificato.
- Una funzione chiamata `createCalendar()` che disegna il calendario e visualizza il mese corretto nell'elemento {{htmlelement("Heading_Elements", "h1")}}.

Abbiamo bisogno che scriva un'istruzione condizionale all'interno della funzione `createCalendar()`, subito sotto il commento `// ADD CONDITIONAL HERE`. Dovrebbe:

1. Osservare il mese selezionato (memorizzato nella variabile `choice`. Questo sarà il valore dell'elemento `<select>` dopo che il valore è cambiato, quindi "Gennaio" ad esempio.)
2. Assegnare alla variabile `days` un valore pari al numero di giorni nel mese selezionato. Per farlo deve consultare il numero di giorni di ciascun mese dell'anno. Può ignorare gli anni bisestili ai fini di questo esempio.

Suggerimenti:

- È consigliato usare OR logico per raggruppare più mesi insieme in una singola condizione; molti di essi condividono lo stesso numero di giorni.
- Pensare a quale numero di giorni è più comune e usarlo come valore predefinito.

Se commette un errore, può sempre reimpostare l'esempio con il pulsante "Reset". Se si blocca davvero, prema "Show solution" per vedere una soluzione.

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

In questo esempio, si prenderà l'esempio dell'operatore ternario visto in precedenza e si convertirà l'operatore ternario in un'istruzione switch per consentire più scelte nel semplice sito web. Guardi il {{htmlelement("select")}} — questa volta vedrà che non ha due opzioni di tema, ma cinque. Deve aggiungere un'istruzione switch proprio sotto il commento `// ADD SWITCH STATEMENT`:

- Accetterà la variabile `choice` come espressione di input.
- Per ciascun caso, la scelta dovrebbe essere uguale a uno dei possibili valori `<option>` che possono essere selezionati, cioè, `white`, `black`, `purple`, `yellow`, o `psychedelic`. Si noti che i valori dell'opzione sono in minuscolo, mentre le _etichette_ delle opzioni, come visualizzate nell'output dal vivo, sono in maiuscolo. Dovrebbe usare i valori in minuscolo nel suo codice.
- Per ogni caso, la funzione `update()` dovrebbe essere eseguita, e essere passati due valori di colore, il primo per il colore di sfondo e il secondo per il colore del testo. Ricordi che i valori dei colori sono stringhe, quindi devono essere avvolti tra virgolette.

Se commette un errore, può sempre reimpostare l'esempio con il pulsante "Reset". Se si blocca davvero, prema "Show solution" per vedere una soluzione.

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

## Metti alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma può ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare che abbia conservato queste informazioni prima di passare oltre — veda [Metti alla prova le sue abilità: Condizionali](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Conditionals).

## Sommario

Questo è tutto ciò che realmente ha bisogno di sapere sulle strutture condizionali in JavaScript per ora! Successivamente, esamineremo il ciclo di codici.

## Vedi anche

- [Operatori di confronto](/it/docs/Learn_web_development/Core/Scripting/Math#comparison_operators)
- [Istruzioni condizionali in dettaglio](/it/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#conditional_statements)
- [Riferimento if...else](/it/docs/Web/JavaScript/Reference/Statements/if...else)
- [Riferimento all'operatore condizionale (ternario)](/it/docs/Web/JavaScript/Reference/Operators/Conditional_operator)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting")}}
