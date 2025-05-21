---
title: Funzioni — blocchi di codice riutilizzabili
short-title: Functions
slug: Learn_web_development/Core/Scripting/Functions
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}

Un altro concetto essenziale nella programmazione è quello delle **funzioni**, che consentono di memorizzare un pezzo di codice che svolge un singolo compito all'interno di un blocco definito, e poi chiamare quel codice ogni volta che ne hai bisogno usando un singolo breve comando, invece di dover digitare lo stesso codice più volte. In questo articolo esploreremo i concetti fondamentali dietro le funzioni come la sintassi di base, come invocarle e definirle, lo scope e i parametri.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di base di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti del CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo delle funzioni — consentire la creazione di blocchi di codice riutilizzabili che possono essere chiamati ovunque siano necessari.</li>
          <li>le funzioni sono utilizzate ovunque in JavaScript e alcune sono integrate nel browser e altre sono definite dall'utente.</li>
          <li>La differenza tra funzioni e metodi.</li>
          <li>Invocare funzioni.</li>
          <li>Funzioni anonime e arrow function.</li>
          <li>Definire i parametri delle funzioni, passare argomenti alle chiamate di funzione.</li>
          <li>Scope globale e scope di funzione/blocco.</li>
          <li>Una comprensione di cosa siano le callback function.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Dove trovo le funzioni?

In JavaScript, troverai funzioni ovunque. In effetti, abbiamo usato funzioni durante tutto il corso finora; semplicemente non ne abbiamo parlato molto. Ora è il momento, tuttavia, di iniziare a parlare esplicitamente delle funzioni e di esplorare davvero la loro sintassi.

Praticamente ogni volta che utilizzi una struttura JavaScript che presenta una coppia di parentesi — `()` — e **non** stai usando una struttura comune del linguaggio integrato come un [ciclo for](/it/docs/Learn_web_development/Core/Scripting/Loops#the_standard_for_loop), [while o do...while](/it/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while), o un [if...else statement](/it/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements), stai utilizzando una funzione.

## Funzioni integrate del browser

Abbiamo utilizzato molto le funzioni integrate nel browser in questo corso.

Ogni volta che abbiamo manipolato una stringa di testo, ad esempio:

```js
const myText = "I am a string";
const newString = myText.replace("string", "sausage");
console.log(newString);
// the replace() string function takes a source string,
// and a target string and replaces the source string,
// with the target string, and returns the newly formed string
```

Oppure ogni volta che abbiamo manipolato un array:

```js
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// the join() function takes an array, joins
// all the array items together into a single
// string, and returns this new string
```

Oppure ogni volta che abbiamo generato un numero casuale:

```js
const myNumber = Math.random();
// the random() function generates a random number between
// 0 and up to but not including 1, and returns that number
```

Abbiamo usato una _funzione_!

> [!NOTE]
> Sentiti libero di inserire queste righe nella console JavaScript del tuo browser per familiarizzare nuovamente con la loro funzionalità, se necessario.

Il linguaggio JavaScript ha molte funzioni integrate per consentire di fare cose utili senza dover scrivere tutto quel codice da soli. In effetti, parte del codice che stai chiamando quando **invochi** (un termine raffinato per eseguire) una funzione integrata del browser non potrebbe essere scritto in JavaScript — molte di queste funzioni stanno chiamando parti del codice di background del browser, che è scritto in gran parte in linguaggi di sistema di basso livello come C++, non in linguaggi web come JavaScript.

Tieni presente che alcune funzioni integrate del browser non fanno parte del linguaggio JavaScript core — alcune sono definite come parte delle API dei browser, che costruiscono sopra il linguaggio predefinito per fornire ancora più funzionalità (fare riferimento a [questa prima sezione del nostro corso](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#so_what_can_it_really_do) per ulteriori descrizioni). Vedremo come usare le API dei browser in modo più dettagliato in un modulo successivo.

## Funzioni contro metodi

Le **funzioni** che fanno parte degli oggetti si chiamano **metodi**; imparerai sugli oggetti più avanti nel modulo. Per ora, volevamo chiarire eventuali dubbi riguardo a metodi e funzioni — probabilmente incontrerai entrambi i termini mentre esamini le risorse correlate disponibili sul Web.

Il codice integrato che abbiamo utilizzato finora si presenta in entrambe le forme: **funzioni** e **metodi.** Puoi controllare l'elenco completo delle funzioni integrate, nonché gli oggetti integrati e i loro metodi corrispondenti [nella nostra referenza JavaScript](/it/docs/Web/JavaScript/Reference/Global_Objects).

Hai anche visto molte **funzioni personalizzate** nel corso finora — funzioni definite nel tuo codice, non all'interno del browser. Ogni volta che hai visto un nome personalizzato con parentesi subito dopo, stavi usando una funzione personalizzata. Nel nostro esempio [random-canvas-circles.html](https://mdn.github.io/learning-area/javascript/building-blocks/loops/random-canvas-circles.html) (vedi anche il codice sorgente completo [source code](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html)) dal nostro [articolo sui cicli](/it/docs/Learn_web_development/Core/Scripting/Loops), abbiamo incluso una funzione personalizzata `draw()` che appare così:

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

Questa funzione disegna 100 cerchi casuali all'interno di un elemento {{htmlelement("canvas")}}. Ogni volta che vogliamo farlo, possiamo semplicemente invocare la funzione con questo:

```js
draw();
```

invece di dover scrivere nuovamente tutto quel codice ogni volta che vogliamo ripeterlo. Le funzioni possono contenere qualsiasi codice tu voglia — puoi persino chiamare altre funzioni dall'interno delle funzioni. La funzione sopra, ad esempio, chiama la funzione `random()` tre volte, che è definita dal seguente codice:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}
```

Avevamo bisogno di questa funzione perché la funzione integrata del browser [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random) genera solo un numero decimale casuale tra 0 e 1. Volevamo un numero intero casuale compreso tra 0 e un numero specificato.

## Invocare le funzioni

Probabilmente sei chiaro su questo adesso, ma solo nel caso, per utilizzare effettivamente una funzione dopo che è stata definita, devi eseguirla — o invocarla. Questo si fa includendo il nome della funzione nel codice da qualche parte, seguito da parentesi.

```js
function myFunction() {
  alert("hello");
}

myFunction();
// calls the function once
```

> [!NOTE]
> Questa forma di creazione di una funzione è anche nota come _dichiarazione di funzione_. È sempre innalzata (hoisted) in modo tale da poter chiamare la funzione sopra la definizione stessa e funzionerà bene.

## Parametri di funzione

Alcune funzioni richiedono che vengano specificati **parametri** quando le stai invocando — questi sono valori che devono essere inclusi all'interno delle parentesi della funzione, di cui essa ha bisogno per svolgere correttamente il proprio lavoro.

> [!NOTE]
> I parametri sono talvolta chiamati argomenti, proprietà, o anche attributi.

Come esempio, la funzione integrata del browser [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random) non richiede alcun parametro. Quando viene chiamata, restituisce sempre un numero casuale compreso tra 0 e 1:

```js
const myNumber = Math.random();
```

La funzione integrata del browser per le stringhe [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace), tuttavia, ha bisogno di due parametri — la sottostringa da trovare nella stringa principale, e la sottostringa per sostituire quella stringa:

```js
const myText = "I am a string";
const newString = myText.replace("string", "sausage");
```

> [!NOTE]
> Quando occorre specificare più parametri, essi sono separati da virgole.

### Parametri opzionali

A volte i parametri sono opzionali — non devi necessariamente specificarli. Se non lo fai, la funzione adotterà generalmente un comportamento predefinito. Ad esempio, il parametro della funzione dell'array [`join()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/join) è opzionale:

```js
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// returns 'I love chocolate frogs'

const madeAnotherString = myArray.join();
console.log(madeAnotherString);
// returns 'I,love,chocolate,frogs'
```

Se non si include un parametro per specificare un carattere di unione/delimite, di default viene utilizzata una virgola.

### Parametri predefiniti

Se stai scrivendo una funzione e vuoi supportare parametri opzionali, puoi specificare i valori predefiniti aggiungendo `=` dopo il nome del parametro, seguito dal valore predefinito:

```js
function hello(name = "Chris") {
  console.log(`Hello ${name}!`);
}

hello("Ari"); // Hello Ari!
hello(); // Hello Chris!
```

## Funzioni anonime e arrow function

Finora abbiamo appena creato una funzione in questo modo:

```js
function myFunction() {
  alert("hello");
}
```

Ma puoi anche creare una funzione che non ha un nome:

```js
(function () {
  alert("hello");
});
```

Questa è chiamata **funzione anonima**, perché non ha un nome. Spesso vedrai funzioni anonime quando una funzione si aspetta di ricevere un'altra funzione come parametro. In questo caso, il parametro funzione è spesso passato come una funzione anonima.

> [!NOTE]
> Questa forma di creazione di una funzione è anche nota come _espressione di funzione_. A differenza delle dichiarazioni di funzione, le espressioni di funzione non sono innalzate.

### Esempio di funzione anonima

Ad esempio, supponiamo che tu voglia eseguire del codice quando l'utente digita in una casella di testo. Per farlo puoi chiamare la funzione [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) della casella di testo. Questa funzione si aspetta che tu le passi (almeno) due parametri:

- il nome dell'evento da ascoltare, che in questo caso è [`keydown`](/it/docs/Web/API/Element/keydown_event)
- una funzione da eseguire quando avviene l'evento.

Quando l'utente preme un tasto, il browser chiamerà la funzione che hai fornito e le passerà un parametro contenente informazioni su questo evento, compreso il particolare tasto che l'utente ha premuto:

```js
function logKey(event) {
  console.log(`You pressed "${event.key}".`);
}

textBox.addEventListener("keydown", logKey);
```

Invece di definire una funzione `logKey()` separata, puoi passare una funzione anonima in `addEventListener()`:

```js
textBox.addEventListener("keydown", function (event) {
  console.log(`You pressed "${event.key}".`);
});
```

### Arrow function

Se passi una funzione anonima in questo modo, c'è una forma alternativa che puoi usare, chiamata **arrow function**. Invece di `function(event)`, scrivi `(event) =>`:

```js
textBox.addEventListener("keydown", (event) => {
  console.log(`You pressed "${event.key}".`);
});
```

Se la funzione prende solo un parametro, puoi omettere le parentesi attorno al parametro:

```js-nolint
textBox.addEventListener("keydown", event => {
  console.log(`You pressed "${event.key}".`);
});
```

Infine, se la tua funzione contiene solo una riga che è un'istruzione `return`, puoi anche omettere le parentesi graffe e la parola chiave `return` e restituire implicitamente l'espressione. Nell'esempio seguente, stiamo usando il metodo {{jsxref("Array.prototype.map()", "map()")}} di `Array` per raddoppiare ogni valore nell'array originale:

```js-nolint
const originals = [1, 2, 3];

const doubled = originals.map(item => item * 2);

console.log(doubled); // [2, 4, 6]
```

Il metodo `map()` prende ogni elemento nell'array in turni, passandolo alla funzione indicata. Poi prende il valore restituito da quella funzione e lo aggiunge a un nuovo array.

Quindi nell'esempio sopra, `item => item * 2` è l'equivalente della funzione arrow di:

```js
function doubleItem(item) {
  return item * 2;
}
```

Puoi utilizzare la stessa sintassi concisa per riscrivere l'esempio `addEventListener`.

```js
textBox.addEventListener("keydown", (event) =>
  console.log(`You pressed "${event.key}".`),
);
```

In questo caso, il valore di `console.log()`, che è `undefined`, viene restituito implicitamente dalla funzione di callback.

Raccomandiamo di utilizzare le arrow function, in quanto possono rendere il codice più breve e leggibile. Per saperne di più, vedi la [sezione sulle funzioni arrow nella guida JavaScript](/it/docs/Web/JavaScript/Guide/Functions#arrow_functions), e la nostra [pagina di riferimento sulle arrow function](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

> [!NOTE]
> Ci sono alcune sottili differenze tra le arrow function e le funzioni normali. Sono fuori dalla portata di questo tutorial introduttivo e è improbabile che facciano una differenza nei casi che abbiamo discusso qui. Per saperne di più, vedi la [documentazione di riferimento sulle arrow function](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

### Esempio funzionale di arrow function

Ecco un esempio funzionante completo dell'esempio "keydown" di cui abbiamo discusso sopra:

L'HTML:

```html
<input id="textBox" type="text" />
<div id="output"></div>
```

JavaScript:

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

Il risultato: prova a digitare nella casella di testo e osserva l'output:

{{EmbedLiveSample("Arrow function live sample", 100, 100)}}

## Scope delle funzioni e conflitti

Parliamo un po' di {{Glossary("scope", "scope")}} — un concetto molto importante quando si tratta di funzioni. Quando crei una funzione, le variabili e altre cose definite all'interno della funzione sono all'interno del loro proprio **scope** separato, il che significa che sono bloccate nei loro compartimenti separati, inaccessibili dal codice esterno alle funzioni.

Il livello superiore al di fuori di tutte le tue funzioni è chiamato **scope globale**. I valori definiti nello scope globale sono accessibili ovunque nel codice.

JavaScript è impostato in questo modo per vari motivi — ma principalmente per motivi di sicurezza e organizzazione. A volte non vuoi che le variabili siano accessibili ovunque nel codice — gli script esterni che chiami da altrove potrebbero iniziare a interferire con il tuo codice e causare problemi perché usano casualmente gli stessi nomi di variabile di altre parti del codice, causando conflitti. Questo potrebbe essere fatto in modo malevolo o semplicemente per errore.

Ad esempio, supponiamo che tu abbia un file HTML che richiama due file JavaScript esterni, e entrambi hanno una variabile e una funzione definite che usano lo stesso nome:

```html
<!-- Excerpt from my HTML -->
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

Vedrai che il secondo script non viene caricato e avviato affatto, e un errore viene stampato nella console: `Uncaught SyntaxError: Identifier 'name' has already been declared`. Questo perché la costante `name` è già dichiarata in `first.js`, e non puoi dichiarare la stessa costante due volte nello stesso scope. Poiché il secondo script non è stato caricato, la funzione `greeting()` da `second.js` non è disponibile per essere chiamata. Pertanto, vedrai una finestra di allerta che mostra `Hello Chris: welcome to our company.`.

Prova a rimuovere la seconda linea `const name = "Zaptec";` da `second.js` e ricarica la pagina. Ora entrambi gli script vengono eseguiti e la finestra di allerta dice `Our company is called Chris.`. Le funzioni possono essere ridefinite e l'ultima dichiarazione viene utilizzata. Le dichiarazioni precedenti vengono effettivamente sovrascritte.

> [!NOTE]
> Puoi vedere questo esempio [in esecuzione su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/functions/conflict.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/building-blocks/functions)).

Mantenere le parti del codice protette all'interno delle funzioni evita tali problemi, ed è considerata una buona pratica.

È un po' come uno zoo. I leoni, zebre, tigri, e pinguini sono tenuti nei loro recinti e hanno accesso solo agli elementi all'interno di essi — proprio come gli scope delle funzioni. Se fossero in grado di entrare in altri recinti, si verificherebbero problemi. Nel migliore dei casi, diversi animali si sentirebbero molto a disagio all'interno di habitat sconosciuti — un leone o una tigre si sentirebbero terribilmente nel dominio acquatico e ghiacciato dei pinguini. Nel peggiore dei casi, i leoni e le tigri potrebbero cercare di mangiare i pinguini!

![Quattro diversi animali racchiusi nel loro rispettivo habitat in uno Zoo](mdn-mozilla-zoo.png)

Il guardiano dello zoo è come lo scope globale — ha le chiavi per accedere a ogni recinto, rifornire il cibo, curare gli animali malati, ecc.

### Apprendimento attivo: Giocare con lo scope

Diamo un'occhiata a un esempio reale per dimostrare lo scoping.

1. Prima di tutto, fai una copia locale del nostro esempio [function-scope.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-scope.html). Questo contiene due funzioni chiamate `a()` e `b()`, e tre variabili — `x`, `y`, e `z` — due delle quali sono definite all'interno delle funzioni, e una nello scope globale. Contiene anche una terza funzione chiamata `output()`, che prende un singolo parametro e lo stampa in un paragrafo sulla pagina.
2. Apri l'esempio in un browser e nel tuo editor di testo.
3. Apri la console JavaScript nei tuoi strumenti di sviluppo del browser. Nella console JavaScript, inserisci il seguente comando:

   ```js
   output(x);
   ```

   Dovresti vedere il valore della variabile `x` stampato nella finestra del browser.

4. Ora prova a inserire quanto segue nella tua console

   ```js
   output(y);
   output(z);
   ```

   Entrambi dovrebbero generare un errore nella console del tipo "[ReferenceError: y is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined)". Perché? A causa dello scope delle funzioni, `y` e `z` sono bloccati all'interno delle funzioni `a()` e `b()`, quindi `output()` non può accedervi quando viene chiamato dallo scope globale.

5. Però, cosa succede quando viene chiamato dall'interno di un'altra funzione? Prova a modificare `a()` e `b()` in modo che appaiano così:

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

   Salva il codice e ricaricalo nel browser, poi prova a chiamare le funzioni `a()` e `b()` dalla console JavaScript:

   ```js
   a();
   b();
   ```

   Dovresti vedere i valori di `y` e `z` stampati nella finestra del browser. Questo funziona bene, in quanto la funzione `output()` viene chiamata all'interno delle altre funzioni — nello stesso scope in cui le variabili che sta stampando sono definite, in ciascun caso. `output()` stesso è disponibile ovunque, poiché è definito nello scope globale.

6. Ora prova ad aggiornare il tuo codice in questo modo:

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

7. Salva e ricarica di nuovo, e prova di nuovo questo nella tua console JavaScript:

   ```js
   a();
   b();
   ```

   Entrambe le chiamate `a()` e `b()` dovrebbero stampare il valore di `x` nella finestra del browser. Queste funzionano bene perché anche se le chiamate a `output()` non sono nello stesso scope in cui è definito `x`, `x` è una variabile globale ed è quindi disponibile all'interno di tutto il codice, ovunque.

8. Infine, prova ad aggiornare il tuo codice in questo modo:

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

9. Salva e ricarica di nuovo, e prova di nuovo questo nella tua console JavaScript:

   ```js
   a();
   b();
   ```

   Questa volta le chiamate a `a()` e `b()` genereranno quel fastidioso errore [ReferenceError: _nome variabile_ is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined) nella console — ciò è dovuto al fatto che le chiamate a `output()` e le variabili che stanno tentando di stampare non sono nello stesso scope delle funzioni — le variabili sono effettivamente invisibili a quelle chiamate di funzione.

> [!NOTE]
> Le stesse regole dello scope non si applicano ai blocchi ciclici (e.g., `for() { }`) e condizionali (e.g., `if () { }`) — sembrano molto simili, ma non sono la stessa cosa! Fai attenzione a non confondere questi aspetti.

> [!NOTE]
> L'errore [ReferenceError: "x" is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined) è uno dei più comuni che incontrerai. Se ottieni questo errore e sei sicuro di aver definito la variabile in questione, controlla in quale scope si trova.

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Funzioni](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Functions). Questi test richiedono abilità che sono coperte nei prossimi due articoli, quindi potresti volerli leggere prima di provare.

## Riassunto

Questo articolo ha esplorato i concetti fondamentali dietro le funzioni, aprendo la strada all'articolo successivo in cui ci mettiamo pratici e ti guidiamo attraverso i passaggi per costruire la tua funzione personalizzata.

## Vedi anche

- [Guida dettagliata sulle funzioni](/it/docs/Web/JavaScript/Guide/Functions) — copre alcune funzionalità avanzate non incluse qui.
- [Riferimento sulle funzioni](/it/docs/Web/JavaScript/Reference/Functions)
- [Usare le funzioni per scrivere meno codice](https://scrimba.com/the-frontend-developer-career-path-c0j/~04g?via=mdn), Scrimba <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> - Una lezione interattiva che fornisce un'introduzione utile alle funzioni.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops","Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}
