---
title: Funzioni — blocchi di codice riutilizzabili
short-title: Functions
slug: Learn_web_development/Core/Scripting/Functions
l10n:
  sourceCommit: dee770bad395da6f67336af7f76dcc823939244e
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Loops","Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}

Un altro concetto essenziale nella programmazione sono le **funzioni**, che consentono di memorizzare una porzione di codice che svolge una singola attività all'interno di un blocco definito, per poi richiamare quel codice ogni volta che serve usando un singolo breve comando, anziché dover digitare lo stesso codice più volte. In questo articolo verranno esplorati concetti fondamentali delle funzioni, come la sintassi di base, come richiamarle e definirle, l'ambito e i parametri.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo delle funzioni: consentire la creazione di blocchi di codice riutilizzabili che possono essere richiamati ovunque siano necessari.</li>
          <li>Le funzioni vengono usate ovunque in JavaScript.</li>
          <li>Alcune funzioni sono integrate nel browser, mentre altre sono definite dall'utente.</li>
          <li>La differenza tra funzioni e metodi.</li>
          <li>Il richiamo delle funzioni.</li>
          <li>Funzioni anonime e arrow function.</li>
          <li>La definizione dei parametri di una funzione e il passaggio di argomenti alle chiamate di funzione.</li>
          <li>Ambito globale e ambito di funzione/blocco.</li>
          <li>Comprensione di cosa sono le callback function.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Dove trovare le funzioni?

In JavaScript, le funzioni si trovano ovunque. Infatti, sono state usate durante tutto il corso finora; semplicemente non se ne è parlato molto. Ora però è il momento di iniziare a parlare esplicitamente delle funzioni e a esplorarne la sintassi.

Praticamente ogni volta che viene usata una struttura JavaScript che presenta una coppia di parentesi — `()` — e **non** viene usata una comune struttura del linguaggio come un [ciclo for](/it/docs/Learn_web_development/Core/Scripting/Loops#the_standard_for_loop), un [ciclo while o do...while](/it/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while) o un'[istruzione if...else](/it/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements), viene usata una funzione.

## Funzioni integrate nel browser

In questo corso sono state usate ampiamente funzioni integrate nel browser.

Ogni volta che è stata manipolata una stringa di testo, ad esempio:

```js
const myText = "I am a string";
const newString = myText.replace("string", "sausage");
console.log(newString);
// the replace() string function takes a source string,
// and a target string and replaces the source string,
// with the target string, and returns the newly formed string
```

Oppure ogni volta che è stato manipolato un array:

```js
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// the join() function takes an array, joins
// all the array items together into a single
// string, and returns this new string
```

Oppure ogni volta che viene generato un numero casuale:

```js
const myNumber = Math.random();
// the random() function generates a random number between
// 0 and up to but not including 1, and returns that number
```

È stata usata una _funzione_!

> [!NOTE]
> Se necessario, è possibile inserire queste righe nella console JavaScript del browser per rivederne il funzionamento.

Il linguaggio JavaScript dispone di molte funzioni integrate che consentono di svolgere attività utili senza dover scrivere tutto quel codice autonomamente. In effetti, parte del codice richiamato quando si **invoca** (un termine tecnico per eseguire) una funzione integrata del browser non potrebbe essere scritto in JavaScript: molte di queste funzioni richiamano parti del codice interno del browser, scritto in gran parte in linguaggi di sistema a basso livello come C++, non in linguaggi web come JavaScript.

Tenere presente che alcune funzioni integrate nel browser non fanno parte del linguaggio JavaScript di base: alcune sono definite come parte delle API del browser, che si basano sul linguaggio predefinito per fornire ancora più funzionalità (per ulteriori descrizioni, consultare [questa sezione iniziale del corso](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#so_what_can_it_really_do)). L'uso delle API del browser verrà approfondito in un modulo successivo.

## Funzioni rispetto ai metodi

Le **funzioni** che fanno parte di oggetti sono chiamate **metodi**; gli oggetti verranno trattati più avanti nel modulo. Per ora, si vuole soltanto chiarire ogni possibile confusione tra metodi e funzioni: è probabile incontrare entrambi i termini consultando risorse correlate sul Web.

Il codice integrato usato finora è disponibile in entrambe le forme: **funzioni** e **metodi**. È possibile consultare l'elenco completo delle funzioni integrate, nonché degli oggetti integrati e dei relativi metodi, [nel riferimento JavaScript](/it/docs/Web/JavaScript/Reference/Global_Objects).

Nel corso sono state viste anche molte **funzioni personalizzate**: funzioni definite nel proprio codice, non all'interno del browser. Ogni volta che è comparso un nome personalizzato seguito direttamente da parentesi, veniva usata una funzione personalizzata. Nell'esempio [random-canvas-circles.html](https://mdn.github.io/learning-area/javascript/building-blocks/loops/random-canvas-circles.html) (vedere anche il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html)) dell'[articolo sui cicli](/it/docs/Learn_web_development/Core/Scripting/Loops), è stata inclusa una funzione personalizzata `draw()` simile a questa:

```js
function draw() {
  ctx.clearRect(0, 0, WIDTH, HEIGHT);
  for (let i = 0; i < 100; i++) {
    ctx.beginPath();
    ctx.fillStyle = "rgb(255 0 0 / 50%)";
    ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
    ctx.fill();
  }
}
```

Questa funzione disegna 100 cerchi casuali all'interno di un elemento {{htmlelement("canvas")}}. Ogni volta che si desidera farlo, è possibile invocare la funzione in questo modo, anziché dover riscrivere tutto quel codice ogni volta che si vuole ripetere l'operazione:

```js
draw();
```

Le funzioni possono contenere qualsiasi codice, incluse chiamate ad altre funzioni. Ad esempio, la funzione `draw()` vista sopra chiama tre volte la funzione `random()`; `random()` è definita dal codice seguente:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}
```

Questa funzione era necessaria perché la funzione integrata [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random) del browser genera soltanto un numero decimale casuale compreso tra 0 e 1. Era invece desiderato un numero intero casuale compreso tra 0 e un numero specificato.

## Richiamare le funzioni

Probabilmente questo concetto è già chiaro, ma per completezza: per usare effettivamente una funzione dopo che è stata definita, occorre eseguirla, ovvero invocarla. Ciò avviene includendo da qualche parte nel codice il nome della funzione, seguito da parentesi.

```js
function myFunction() {
  alert("hello");
}

myFunction();
// calls the function once
```

> [!NOTE]
> Questa forma di creazione di una funzione è nota anche come _function declaration_. Viene sempre sottoposta a hoisting, il che significa che è possibile chiamare la funzione prima della sua definizione e funzionerà correttamente.

## Argomenti e parametri delle funzioni

Alcune funzioni richiedono **argomenti** quando vengono invocate: valori che devono essere inclusi tra le parentesi della funzione affinché la funzione svolga correttamente il proprio compito.

Verrà usato anche il termine **parametri**, spesso in modo intercambiabile con _argomenti_. Questo è spesso accettabile nelle discussioni informali, ma i due termini hanno significati diversi. I parametri sono le variabili elencate nella definizione di una funzione, mentre gli argomenti sono i valori passati alla funzione per rappresentare i parametri quando la funzione viene chiamata.

Vediamo alcuni esempi. La funzione [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random) non richiede argomenti. Quando viene chiamata, restituisce sempre un numero casuale compreso tra 0 e 1:

```js
const myNumber = Math.random();
```

La funzione [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace) delle stringhe, invece, richiede due argomenti: la sottostringa da trovare nella stringa principale e la sottostringa con cui sostituirla:

```js
const myText = "I am a string";
const newString = myText.replace("string", "sausage");
```

> [!NOTE]
> Quando è necessario specificare più parametri o argomenti, separarli con virgole.

### Parametri opzionali

Talvolta i parametri sono definiti come opzionali: non è necessario specificare gli argomenti equivalenti quando si chiama la funzione. In caso contrario, la funzione generalmente usa un valore predefinito. Ad esempio, il parametro della funzione [`join()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/join) degli array è opzionale:

```js
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// returns 'I love chocolate frogs'

const madeAnotherString = myArray.join();
console.log(madeAnotherString);
// returns 'I,love,chocolate,frogs'
```

Se non viene incluso alcun argomento per specificare un carattere di unione/delimitazione, per impostazione predefinita viene usata una virgola.

### Parametri predefiniti

Se viene scritta una funzione e si desidera definire parametri opzionali, è possibile specificare valori predefiniti aggiungendo `=` dopo il nome del parametro, seguito dal valore predefinito:

```js
function hello(name = "Chris") {
  console.log(`Hello ${name}!`);
}

hello("Ari"); // Hello Ari!
hello(); // Hello Chris!
```

## Funzioni anonime e arrow function

Finora sono state create funzioni in questo modo:

```js
function myFunction() {
  alert("hello");
}
```

Ma è anche possibile creare una funzione senza nome:

```js
(function () {
  alert("hello");
});
```

Questa è chiamata **funzione anonima**, perché non ha un nome. Le funzioni anonime sono frequenti quando una funzione si aspetta di ricevere un'altra funzione come argomento. In questo caso, spesso viene passata una funzione anonima come argomento.

> [!NOTE]
> Questa forma di creazione di una funzione è nota anche come _function expression_. A differenza delle function declaration, le function expression non sono soggette a hoisting.

### Esempio di funzione anonima

Ad esempio, supponiamo di voler eseguire del codice quando l'utente digita in una casella di testo. Per farlo, è possibile chiamare la funzione [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) della casella di testo. Questa funzione si aspetta almeno due argomenti:

- Il nome dell'evento da ascoltare, che in questo caso è [`keydown`](/it/docs/Web/API/Element/keydown_event)
- Una funzione da eseguire quando l'evento si verifica.

Quando l'utente preme un tasto, il browser chiamerà la funzione fornita e le passerà un parametro contenente informazioni su questo evento, incluso il particolare tasto premuto dall'utente:

```js
function logKey(event) {
  console.log(`You pressed "${event.key}".`);
}

textBox.addEventListener("keydown", logKey);
```

Invece di definire una funzione `logKey()` separata, è possibile passare una funzione anonima a `addEventListener()`:

```js
textBox.addEventListener("keydown", function (event) {
  console.log(`You pressed "${event.key}".`);
});
```

### Arrow function

Se viene passata una funzione anonima in questo modo, è disponibile una forma alternativa, chiamata **arrow function**. Invece di `function(event)`, si scrive `(event) =>`:

```js
textBox.addEventListener("keydown", (event) => {
  console.log(`You pressed "${event.key}".`);
});
```

Se la funzione accetta un solo argomento, è possibile omettere le parentesi attorno a esso:

```js-nolint
textBox.addEventListener("keydown", event => {
  console.log(`You pressed "${event.key}".`);
});
```

Infine, se la funzione contiene soltanto una singola riga che è un'istruzione `return`, è possibile omettere le parentesi graffe e la parola chiave `return`, restituendo implicitamente l'espressione. Nell'esempio seguente, viene usato il metodo {{jsxref("Array.prototype.map()","map()")}} di `Array` per raddoppiare ogni valore nell'array originale:

```js-nolint
const originals = [1, 2, 3];

const doubled = originals.map(item => item * 2);

console.log(doubled); // [2, 4, 6]
```

Il metodo `map()` passa ogni elemento dell'array alla funzione fornita, quindi prende il valore restituito dalla funzione e lo aggiunge a un nuovo array.

La arrow function è molto concisa; riscrivere il codice `map()` usando una normale callback function anonima sarebbe simile a questo:

```js
const doubled = originals.map(function (item) {
  return item * 2;
});
```

È possibile usare la stessa sintassi concisa delle arrow function per riscrivere l'esempio `addEventListener()`:

```js-nolint
textBox.addEventListener("keydown", (event) =>
  console.log(`You pressed "${event.key}".`)
);
```

In questo caso, il valore di `console.log()`, ovvero `undefined`, viene restituito implicitamente dalla callback function.

Si raccomanda l'uso delle arrow function, poiché possono rendere il codice più breve e leggibile. Per ulteriori informazioni, consultare la [sezione sulle arrow function nella Guida JavaScript](/it/docs/Web/JavaScript/Guide/Functions#arrow_functions) e la [pagina di riferimento sulle arrow function](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

> [!NOTE]
> Esistono alcune differenze sottili tra le arrow function e le funzioni normali. Tali differenze non rientrano nell'ambito di questo tutorial introduttivo ed è improbabile che facciano differenza nei casi trattati qui. Per ulteriori informazioni, consultare la [documentazione di riferimento sulle arrow function](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

### Esempio dal vivo di arrow function

Ecco una versione completa e funzionante dell'esempio `keydown` discusso sopra:

L'HTML:

```html
<input id="textBox" type="text" />
<div id="output"></div>
```

Il JavaScript:

```js
const textBox = document.querySelector("#textBox");
const output = document.querySelector("#output");

textBox.addEventListener("keydown", (event) => {
  output.textContent = `You pressed "${event.key}".`;
});
```

```css hidden
div {
  margin: 0.5rem 0;
}
```

Il risultato: provare a digitare nella casella di testo e osservare l'output:

{{EmbedLiveSample("Arrow function live sample", 100, 100)}}

## Ambito delle funzioni e conflitti

Parliamo un po' dell'{{Glossary("scope", "ambito")}}, un concetto importante quando si lavora con le funzioni. Quando viene creata una funzione, le variabili e altri elementi definiti al suo interno si trovano nel proprio **ambito** separato. Ciò significa che sono racchiusi in un compartimento separato e non sono raggiungibili dal codice esterno alla funzione.

Il livello più esterno, al di fuori di tutte le funzioni, è chiamato **ambito globale**. I valori definiti nell'ambito globale sono accessibili da ogni parte del codice.

JavaScript funziona in questo modo principalmente per motivi di sicurezza e organizzazione. Talvolta non si desidera che le variabili siano accessibili ovunque nel codice. Script esterni richiamati da altre parti potrebbero interferire con il codice e causare problemi se usano gli stessi nomi di variabile, generando conflitti. Ciò potrebbe avvenire in modo malevolo oppure semplicemente per errore.

Ad esempio, supponiamo di avere un file HTML che fa riferimento a due file JavaScript esterni, entrambi con una variabile e una funzione definite usando lo stesso nome:

```html
<!-- Excerpt from the HTML -->
<script src="first.js"></script>
<script src="second.js"></script>
<script>
  greeting();
</script>
```

```js
// first.js
const name = "Chris";
function greeting() {
  alert(`Hello ${name}: welcome to our company.`);
}
```

```js
// second.js
const name = "Zaptec";
function greeting() {
  alert(`Our company is called ${name}.`);
}
```

È possibile vedere questo esempio [in esecuzione su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/functions/conflict.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/building-blocks/functions)). Caricarlo in una scheda separata del browser prima di leggere la spiegazione seguente.

- Quando l'esempio viene renderizzato in un browser, inizialmente verrà visualizzata una finestra di avviso con `Hello Chris: welcome to our company.`, il che significa che la funzione `greeting()` definita nel primo file di script è stata chiamata dalla chiamata `greeting()` nello script interno.

- Il secondo script, tuttavia, non viene caricato né eseguito e nella console viene stampato un errore: `Uncaught SyntaxError: Identifier 'name' has already been declared`. Questo accade perché la costante `name` è già dichiarata in `first.js` e non è possibile dichiarare la stessa costante due volte nello stesso ambito. Poiché il secondo script non è stato caricato, la funzione `greeting()` da `second.js` non è disponibile per essere chiamata.

- Se si rimuovesse la riga `const name = "Zaptec";` da `second.js` e si ricaricasse la pagina, entrambi gli script verrebbero eseguiti. La finestra di avviso direbbe ora `Our company is called Chris.` Se una funzione viene _rideclarata_, viene usata l'ultima dichiarazione nell'ordine del sorgente. Le dichiarazioni precedenti vengono di fatto sovrascritte.

Racchiudere parti del codice nelle funzioni evita problemi di questo tipo ed è considerata una buona pratica.

È un po' come un condominio:

- Ogni appartamento è privato per le persone che vi abitano, in modo simile all'ambito di funzione: il codice all'interno di una funzione può accedere alle variabili e alle funzioni definite al suo interno, ma il codice esterno a quella funzione non può farlo. Se tutti avessero accesso agli appartamenti di tutti gli altri, sorgerebbero problemi: gli oggetti delle persone potrebbero essere spostati, danneggiati o rubati.

- L'edificio può anche avere aree comuni, come una piscina, una palestra o una sala ricreativa, accessibili a tutti. Questo è simile all'ambito globale: qualsiasi elemento dichiarato lì è accessibile a ogni funzione. Tutti possono usare gli spazi comuni, il che è ragionevole.

### Sperimentare con l'ambito

Vediamo un esempio reale per dimostrare l'uso degli ambiti.

1. Per prima cosa, creare una copia locale dell'esempio [function-scope.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-scope.html). Questo contiene due funzioni chiamate `a()` e `b()` e tre variabili — `x`, `y` e `z` — due delle quali sono definite all'interno delle funzioni e una nell'ambito globale. Contiene inoltre una terza funzione chiamata `output()`, che accetta un singolo argomento e lo visualizza in un paragrafo della pagina.
2. Aprire l'esempio in un browser e nell'editor di testo.
3. Aprire la console JavaScript negli strumenti per sviluppatori del browser. Nella console JavaScript, inserire il comando seguente:

   ```js
   output(x);
   ```

   Dovrebbe essere visualizzato il valore della variabile `x` nella viewport del browser.

4. Ora provare a inserire quanto segue nella console:

   ```js
   output(y);
   output(z);
   ```

   Entrambi dovrebbero generare nella console un errore simile a "[ReferenceError: y is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined)". Perché? A causa dell'ambito di funzione: `y` e `z` sono racchiuse nelle funzioni `a()` e `b()`, quindi `output()` non può accedervi quando viene chiamata dall'ambito globale.

5. Tuttavia, cosa accade quando viene chiamata dall'interno di un'altra funzione? Provare a modificare `a()` e `b()` affinché abbiano questo aspetto:

   ```js
   function a() {
     const y = 2;
     output(y);
   }

   function b() {
     const z = 3;
     output(z);
   }
   ```

   Salvare il codice e ricaricarlo nel browser, quindi provare a chiamare le funzioni `a()` e `b()` dalla console JavaScript:

   ```js
   a();
   b();
   ```

   Dovrebbero essere visualizzati i valori `y` e `z` nella viewport del browser. Questo funziona correttamente perché la funzione `output()` viene chiamata all'interno delle altre funzioni, nello stesso ambito in cui sono definite le variabili stampate. `output()` stessa è disponibile da qualsiasi punto, poiché è definita nell'ambito globale.

6. Ora provare ad aggiornare il codice in questo modo:

   ```js
   function a() {
     const y = 2;
     output(x);
   }

   function b() {
     const z = 3;
     output(x);
   }
   ```

7. Salvare e ricaricare nuovamente, quindi provare di nuovo questo nella console JavaScript:

   ```js
   a();
   b();
   ```

   Entrambe le chiamate `a()` e `b()` dovrebbero stampare il valore di x nella viewport del browser. Funzionano correttamente perché, anche se le chiamate a `output()` non si trovano nello stesso ambito in cui è definita `x`, `x` è una variabile globale: è disponibile all'interno di tutto il codice, ovunque.

8. Infine, provare ad aggiornare il codice in questo modo:

   ```js
   function a() {
     const y = 2;
     output(z);
   }

   function b() {
     const z = 3;
     output(y);
   }
   ```

9. Salvare e ricaricare nuovamente, quindi provare di nuovo questo nella console JavaScript:

   ```js
   a();
   b();
   ```

   Questa volta le chiamate `a()` e `b()` genereranno nella console il fastidioso errore [ReferenceError: _nome-variabile_ is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined): ciò accade perché le chiamate a `output()` e le variabili che tentano di stampare non si trovano negli stessi ambiti di funzione. Le variabili sono di fatto invisibili a quelle chiamate di funzione.

> [!NOTE]
> L'errore [ReferenceError: "x" is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined) è uno dei più comuni che si incontreranno. Se si riceve questo errore e si è certi di aver definito la variabile in questione, controllare in quale ambito si trova.

#### Una nota sull'ambito di cicli e condizionali

Vale la pena notare che l'ambito dei valori dichiarati all'interno di [condizionali](/it/docs/Learn_web_development/Core/Scripting/Conditionals) e [cicli](/it/docs/Learn_web_development/Core/Scripting/Loops) funziona come l'ambito di funzione quando si dichiarano valori con `let` e `const`. Ad esempio, se venissero aggiunti i blocchi seguenti all'esempio precedente:

```js
if (x === 1) {
  const c = 4;
  let d = 5;
}

for (let i = 0; i <= 1; i++) {
  const e = 6;
  let f = 7;
}
```

La chiamata a `output(c)`, `output(d)`, `output(e)` o `output(f)` produrrebbe lo stesso errore **"ReferenceError: [nome-variabile] is not defined"** visto in precedenza. La funzione `output()` non può accedere a queste variabili perché sono racchiuse nel proprio ambito.

La parola chiave legacy `var` funziona diversamente. Se `c`, `d`, `e` e `f` fossero dichiarate usando `var`:

```js
if (x === 1) {
  var c = 4;
  var d = 5;
}

for (let i = 0; i <= 1; i++) {
  var e = 6;
  var f = 7;
}
```

Verrebbero sottoposte a hoisting nell'ambito globale; pertanto, visualizzarle nella console, ad esempio con `output(c)`, funzionerebbe. Le variabili dichiarate con `var` all'interno delle funzioni, tuttavia, hanno comunque il loro ambito limitato a tali funzioni.

Questa incoerenza può causare confusione ed errori ed è un'altra ragione per cui dovrebbero essere usati `let` e `const` invece di `var`.

## Riepilogo

Questo articolo ha esplorato i concetti fondamentali alla base delle funzioni, preparando il terreno per il prossimo, nel quale si passerà alla pratica attraverso i passaggi necessari per costruire una funzione personalizzata.

## Vedere anche

- [Guida dettagliata alle funzioni](/it/docs/Web/JavaScript/Guide/Functions) — tratta alcune funzionalità avanzate non incluse qui.
- [Riferimento alle funzioni](/it/docs/Web/JavaScript/Reference/Functions)
- [Usare le funzioni per scrivere meno codice](https://scrimba.com/the-frontend-developer-career-path-c0j/~04g?via=mdn), Scrimba <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> - Una lezione interattiva che fornisce un'utile introduzione alle funzioni.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Loops","Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}
