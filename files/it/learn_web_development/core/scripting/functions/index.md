---
title: Functions — blocchi di codice riutilizzabili
short-title: Functions
slug: Learn_web_development/Core/Scripting/Functions
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}

Un altro concetto essenziale nel coding è rappresentato dalle **funzioni**, che consentono di memorizzare un blocco di codice che esegue un unico compito all'interno di un blocco definito e quindi chiamare quel codice ogni volta che ne ha bisogno utilizzando un unico comando breve, invece di dover digitare lo stesso codice più volte. In questo articolo esploreremo i concetti fondamentali delle funzioni, come la sintassi di base, come invocarle e definirle, il loro ambito e i parametri.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo delle funzioni: consentire la creazione di blocchi di codice riutilizzabili che possono essere richiamati ovunque necessario.</li>
          <li>Le funzioni sono utilizzate ovunque in JavaScript e alcune sono integrate nel browser mentre altre sono definite dall'utente.</li>
          <li>La differenza tra funzioni e metodi.</li>
          <li>Invocazione delle funzioni.</li>
          <li>Funzioni anonime e arrow functions.</li>
          <li>Definizione dei parametri delle funzioni, passaggio degli argomenti nelle chiamate di funzione.</li>
          <li>Ambiti globali e di funzione/blocco.</li>
          <li>Una comprensione di cosa sono le funzioni callback.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Dove trovo le funzioni?

In JavaScript, troverà funzioni ovunque. Infatti, abbiamo utilizzato funzioni durante tutto il corso finora, solo che non ne abbiamo parlato molto. Tuttavia, è giunto il momento di iniziare a parlare esplicitamente delle funzioni e di esplorare davvero la loro sintassi.

Praticamente ogni volta che usa una struttura JavaScript che presenta una coppia di parentesi — `()` — e **non** sta usando una struttura del linguaggio comune come un [ciclo for](/it/docs/Learn_web_development/Core/Scripting/Loops#the_standard_for_loop), [ciclo while o do...while](/it/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while), o un [statement if...else](/it/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements), sta utilizzando una funzione.

## Funzioni integrate nel browser

Abbiamo usato molto le funzioni integrate nel browser in questo corso.

Ogni volta che manipolavamo una stringa di testo, ad esempio:

```js
const myText = "I am a string";
const newString = myText.replace("string", "sausage");
console.log(newString);
// the replace() string function takes a source string,
// and a target string and replaces the source string,
// with the target string, and returns the newly formed string
```

Oppure ogni volta che manipolavamo un array:

```js
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// the join() function takes an array, joins
// all the array items together into a single
// string, and returns this new string
```

Oppure ogni volta che generavamo un numero casuale:

```js
const myNumber = Math.random();
// the random() function generates a random number between
// 0 and up to but not including 1, and returns that number
```

Stavamo usando una _funzione_!

> [!NOTE]
> Si senta libero di inserire queste righe nella console JavaScript del suo browser per familiarizzare nuovamente con la loro funzionalità, se necessario.

Il linguaggio JavaScript ha molte funzioni integrate che le consentono di fare cose utili senza dover scrivere tutto quel codice da soli. In effetti, parte del codice che chiama quando **invoca** (un termine sofisticato per dire eseguire o avviare) una funzione integrata del browser potrebbe non essere scritto in JavaScript — molte di queste funzioni richiamano parti del codice di background del browser, che è scritto in gran parte in linguaggi di sistema di basso livello come C++, non in linguaggi web come JavaScript.

Ricordi che alcune funzioni integrate del browser non fanno parte del linguaggio JavaScript di base — alcune sono definite come parte delle API del browser, che si costruiscono sopra il linguaggio predefinito per fornire ancora più funzionalità (si riferisca a [questa prima sezione del nostro corso](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#so_what_can_it_really_do) per ulteriori descrizioni). Esamineremo l'uso delle API del browser in modo più dettagliato in un modulo successivo.

## Funzioni contro metodi

Le **funzioni** che sono parte di oggetti sono chiamate **metodi**; imparerà gli oggetti più avanti nel modulo. Per ora, volevamo solo chiarire eventuali confusioni su metodo contro funzione — probabilmente incontrerà entrambi i termini mentre esplora le risorse correlate disponibili sul Web.

Il codice integrato che abbiamo utilizzato finora si presenta in entrambe le forme: **funzioni** e **metodi.** Può controllare l'elenco completo delle funzioni integrate, nonché degli oggetti integrati e i loro rispettivi metodi [nel nostro riferimento JavaScript](/it/docs/Web/JavaScript/Reference/Global_Objects).

Ha anche visto molte **funzioni personalizzate** nel corso finora — funzioni definite nel suo codice, non all'interno del browser. Ogni volta che ha visto un nome personalizzato seguito da parentesi, stava usando una funzione personalizzata. Nel nostro esempio [random-canvas-circles.html](https://mdn.github.io/learning-area/javascript/building-blocks/loops/random-canvas-circles.html) (vedi anche il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html)) dal nostro [articolo sui cicli](/it/docs/Learn_web_development/Core/Scripting/Loops), abbiamo incluso una funzione personalizzata `draw()` che appare così:

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

invece di dover riscrivere tutto quel codice ogni volta che vogliamo ripeterlo. Le funzioni possono contenere qualsiasi codice si desideri — si possono persino chiamare altre funzioni dall'interno delle funzioni. La funzione sopra, ad esempio, chiama la funzione `random()` tre volte, che è definita dal seguente codice:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}
```

Avevamo bisogno di questa funzione perché la funzione integrata del browser [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random) genera solo un numero decimale casuale tra 0 e 1. Volevamo un numero intero casuale tra 0 e un numero specificato.

## Invocazione delle funzioni

Ormai dovrebbe esserle chiaro, ma nel caso, per utilizzare effettivamente una funzione dopo che è stata definita, deve essere eseguita — o invocata. Questo si fa includendo il nome della funzione nel codice da qualche parte, seguito da parentesi.

```js
function myFunction() {
  alert("hello");
}

myFunction();
// calls the function once
```

> [!NOTE]
> Questa forma di creazione di una funzione è anche nota come _dichiarazione di funzione_. Viene sempre sollevata in modo che possa chiamare la funzione sopra la definizione della funzione e funzionerà bene.

## Parametri delle funzioni

Alcune funzioni richiedono che vengano specificati dei **parametri** quando le sta invocando — questi sono valori che devono essere inclusi all'interno delle parentesi della funzione, di cui ha bisogno per svolgere correttamente il suo lavoro.

> [!NOTE]
> I parametri vengono talvolta chiamati argomenti, proprietà o persino attributi.

Ad esempio, la funzione integrata del browser [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random) non richiede alcun parametro. Quando chiamata, restituisce sempre un numero casuale tra 0 e 1:

```js
const myNumber = Math.random();
```

La funzione `replace()` integrata del browser [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace), tuttavia, necessita di due parametri — la sottostringa da trovare nella stringa principale e la sottostringa con cui sostituire quella stringa:

```js
const myText = "I am a string";
const newString = myText.replace("string", "sausage");
```

> [!NOTE]
> Quando deve specificare più parametri, essi sono separati da virgole.

### Parametri opzionali

A volte i parametri sono opzionali — non è necessario specificarli. Se non lo fa, la funzione generalmente adotterà un qualche tipo di comportamento predefinito. Ad esempio, il parametro della funzione array [`join()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Array/join) è opzionale:

```js
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// returns 'I love chocolate frogs'

const madeAnotherString = myArray.join();
console.log(madeAnotherString);
// returns 'I,love,chocolate,frogs'
```

Se non viene incluso alcun parametro per specificare un carattere di giunzione/delimitazione, viene utilizzata una virgola per impostazione predefinita.

### Parametri predefiniti

Se sta scrivendo una funzione e desidera supportare parametri opzionali, può specificare valori predefiniti aggiungendo `=` dopo il nome del parametro, seguito dal valore predefinito:

```js
function hello(name = "Chris") {
  console.log(`Hello ${name}!`);
}

hello("Ari"); // Hello Ari!
hello(); // Hello Chris!
```

## Funzioni anonime e arrow functions

Finora abbiamo creato solo una funzione in questo modo:

```js
function myFunction() {
  alert("hello");
}
```

Ma può anche creare una funzione che non ha un nome:

```js
(function () {
  alert("hello");
});
```

Questa è chiamata **funzione anonima**, perché non ha un nome. Spesso vedrà funzioni anonime quando una funzione si aspetta di ricevere un'altra funzione come parametro. In questo caso, il parametro funzione viene spesso passato come funzione anonima.

> [!NOTE]
> Questa forma di creazione di una funzione è anche nota come _espressione di funzione_. A differenza delle dichiarazioni di funzione, le espressioni di funzione non vengono sollevate.

### Esempio di funzione anonima

Ad esempio, supponiamo che voglia eseguire del codice quando l'utente digita in una casella di testo. Per fare ciò può chiamare la funzione [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener) della casella di testo. Questa funzione si aspetta che lei passi (almeno) due parametri:

- il nome dell'evento da ascoltare, che in questo caso è [`keydown`](/it/docs/Web/API/Element/keydown_event)
- una funzione da eseguire quando si verifica l'evento.

Quando l'utente preme un tasto, il browser chiamerà la funzione fornita e le passerà un parametro contenente informazioni su questo evento, incluso il particolare tasto premuto dall'utente:

```js
function logKey(event) {
  console.log(`You pressed "${event.key}".`);
}

textBox.addEventListener("keydown", logKey);
```

Invece di definire una funzione `logKey()` separata, può passare una funzione anonima a `addEventListener()`:

```js
textBox.addEventListener("keydown", function (event) {
  console.log(`You pressed "${event.key}".`);
});
```

### Arrow functions

Se passa una funzione anonima in questo modo, c'è una forma alternativa che può usare, chiamata **arrow function**. Invece di `function(event)`, scrive `(event) =>`:

```js
textBox.addEventListener("keydown", (event) => {
  console.log(`You pressed "${event.key}".`);
});
```

Se la funzione accetta solo un parametro, può omettere le parentesi attorno al parametro:

```js-nolint
textBox.addEventListener("keydown", event => {
  console.log(`You pressed "${event.key}".`);
});
```

Infine, se la sua funzione contiene solo una riga che è un'istruzione `return`, può anche omettere le parentesi graffe e la keyword `return` e restituire implicitamente l'espressione. Nel seguente esempio, stiamo usando il metodo {{jsxref("Array.prototype.map()", "map()")}} di `Array` per raddoppiare ogni valore nell'array originale:

```js-nolint
const originals = [1, 2, 3];

const doubled = originals.map(item => item * 2);

console.log(doubled); // [2, 4, 6]
```

Il metodo `map()` prende ciascun elemento dell'array a turno, passandolo alla funzione data. Poi prende il valore restituito da quella funzione e lo aggiunge a un nuovo array.

Così nell'esempio sopra, `item => item * 2` è la funzione arrow equivalente a:

```js
function doubleItem(item) {
  return item * 2;
}
```

Può usare la stessa sintassi concisa per riscrivere l'esempio di `addEventListener`.

```js
textBox.addEventListener("keydown", (event) =>
  console.log(`You pressed "${event.key}".`),
);
```

In questo caso, il valore di `console.log()`, che è `undefined`, viene implicitamente restituito dalla funzione di callback.

Si consiglia di utilizzare le arrow functions, poiché possono rendere il suo codice più breve e più leggibile. Per saperne di più, veda la [sezione sulle arrow functions nella guida JavaScript](/it/docs/Web/JavaScript/Guide/Functions#arrow_functions) e la nostra [pagina di riferimento sulle arrow functions](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

> [!NOTE]
> Ci sono alcune sottili differenze tra le arrow functions e le funzioni normali. Sono al di fuori dell'ambito di questo tutorial introduttivo e sono improbabili che facciano differenza nei casi che abbiamo discusso qui. Per saperne di più, veda la [documentazione di riferimento sulle arrow functions](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

### Esempio live di arrow function

Ecco un esempio completo e funzionante dell'esempio "keydown" di cui abbiamo discusso sopra:

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

Il risultato - provi a digitare nella casella di testo e veda l'output:

{{EmbedLiveSample("Arrow function live sample", 100, 100)}}

## Ambito delle funzioni e conflitti

Parliamo un po' di {{Glossary("scope", "ambito")}} — un concetto molto importante quando si ha a che fare con le funzioni. Quando crea una funzione, le variabili e altre cose definite all'interno della funzione sono all'interno del loro proprio **ambito**, il che significa che sono bloccate nei loro propri compartimenti separati, inaccessibili dal codice al di fuori delle funzioni.

Il livello più alto al di fuori di tutte le sue funzioni è chiamato **ambito globale**. I valori definiti nell'ambito globale sono accessibili da ogni parte del codice.

JavaScript è impostato in questo modo per vari motivi — ma principalmente per sicurezza e organizzazione. A volte non si vogliono variabili accessibili da tutte le parti del codice — script esterni che si chiamano da altre parti potrebbero iniziare a interferire con il suo codice e causare problemi perché succede che usano gli stessi nomi di variabile di altre parti del codice, causando conflitti. Ciò potrebbe essere fatto in modo doloso o semplicemente per errore.

Ad esempio, supponiamo di avere un file HTML che chiama due file JavaScript esterni, e entrambi hanno una variabile e una funzione definiti che utilizzano lo stesso nome:

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

Vedrà che il secondo script non si carica e non si esegue affatto, e un errore viene stampato nella console: `Uncaught SyntaxError: Identifier 'name' has already been declared`. Questo perché la costante `name` è già dichiarata in `first.js`, e non si può dichiarare la stessa costante due volte nello stesso ambito. Poiché il secondo script non è stato caricato, la funzione `greeting()` da `second.js` non è disponibile per essere chiamata. Pertanto, vedrà una finestra di avviso che visualizza `Hello Chris: welcome to our company.`

Provi a rimuovere la seconda riga `const name = "Zaptec";` da `second.js` e a ricaricare la pagina. Ora entrambi gli script vengono eseguiti, e la finestra di avviso dice `Our company is called Chris.` Le funzioni sono autorizzate a essere dichiarate di nuovo, e l'ultima dichiarazione viene utilizzata. Le dichiarazioni precedenti vengono effettivamente sovrascritte.

> [!NOTE]
> Può vedere questo esempio [in esecuzione live su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/functions/conflict.html) (vedere anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/building-blocks/functions)).

Mantenere parti del suo codice bloccate nelle funzioni evita tali problemi ed è considerata la migliore pratica.

È un po' come uno zoo. I leoni, le zebre, le tigri e i pinguini sono tenuti nei loro recinti e hanno accesso solo a ciò che è all'interno dei loro recinti — nello stesso modo degli ambiti delle funzioni. Se fossero in grado di entrare in altri recinti, potrebbero verificarsi problemi. Nel migliore dei casi, animali diversi si sentirebbero davvero a disagio in habitat non familiari — un leone o una tigre si sentirebbero terribilmente nel dominio acquoso e ghiacciato dei pinguini. Nel peggiore dei casi, i leoni e le tigri potrebbero provare a mangiare i pinguini!

![Quattro diversi animali racchiusi nel rispettivo habitat in uno zoo](mdn-mozilla-zoo.png)

Il guardiano dello zoo è come l'ambito globale — ha le chiavi per accedere a ogni recinto, rifornire cibo, curare animali malati, ecc.

### Apprendimento attivo: Giocare con l'ambito

Vediamo un esempio reale per dimostrare lo scoping.

1. Innanzitutto, faccia una copia locale del nostro esempio [function-scope.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-scope.html). Questo contiene due funzioni chiamate `a()` e `b()`, e tre variabili — `x`, `y` e `z` — due delle quali sono definite all'interno delle funzioni, e una nell'ambito globale. Contiene anche una terza funzione chiamata `output()`, che prende un singolo parametro e lo visualizza in un paragrafo sulla pagina.
2. Apra l'esempio nel suo browser e nel suo editor di testo.
3. Apra la console JavaScript nei suoi strumenti per sviluppatori del browser. Nella console JavaScript, inserisca il seguente comando:

   ```js
   output(x);
   ```

   Dovrebbe vedere il valore della variabile `x` stampato nella vista del browser.

4. Ora provi a inserire quanto segue nella sua console

   ```js
   output(y);
   output(z);
   ```

   Entrambi questi dovrebbero generare un errore nella console del tipo "[ReferenceError: y is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined)". Perché? A causa dell'ambito di funzione, `y` e `z` sono bloccati all'interno delle funzioni `a()` e `b()`, quindi `output()` non può accedervi quando è chiamato dall'ambito globale.

5. Tuttavia, cosa succede quando è chiamato dall'interno di un'altra funzione? Provi a modificare `a()` e `b()` in modo che appaiano così:

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

   Salvi il codice e lo ricarichi nel suo browser, poi provi a chiamare le funzioni `a()` e `b()` dalla console JavaScript:

   ```js
   a();
   b();
   ```

   Dovrebbe vedere i valori `y` e `z` stampati nella vista del browser. Questo funziona bene, poiché le funzioni `output()` sono chiamate all'interno delle altre funzioni — nello stesso ambito in cui le variabili che sta stampando sono definite, in ciascun caso. `output()` stesso è disponibile ovunque, poiché è definito nell'ambito globale.

6. Ora provi a aggiornare il suo codice in questo modo:

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

7. Salvi e ricarichi di nuovo, quindi provi di nuovo nella sua console JavaScript:

   ```js
   a();
   b();
   ```

   Entrambe le chiamate `a()` e `b()` dovrebbero stampare il valore di x nella vista del browser. Queste funzionano bene perché anche se le chiamate a `output()` non sono nello stesso ambito in cui `x` è definito, `x` è una variabile globale quindi è disponibile all'interno di tutto il codice, ovunque.

8. Infine, provi a aggiornare il suo codice in questo modo:

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

9. Salvi e ricarichi di nuovo, quindi provi di nuovo nella sua console JavaScript:

   ```js
   a();
   b();
   ```

   Questa volta le chiamate `a()` e `b()` genereranno il fastidioso errore [ReferenceError: _nome della variabile_ is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined) nella console — questo perché le chiamate a `output()` e le variabili che stanno tentando di stampare non sono nello stesso ambito di funzione — le variabili sono effettivamente invisibili a quelle chiamate di funzione.

> [!NOTE]
> Le stesse regole di scoping non si applicano ai blocchi di ciclo (es. `for() { }`) e ai blocchi condizionali (es. `if () { }`) — sembrano molto simili, ma non sono la stessa cosa! Si assicuri di non confondersi.

> [!NOTE]
> L'errore [ReferenceError: "x" is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined) è uno dei più comuni che incontrerà. Se riceve questo errore ed è sicuro di aver definito la variabile in questione, controlli in quale ambito si trova.

## Metti alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver conservato queste informazioni prima di andare avanti — veda [Test your skills: Functions](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Functions). Questi test richiedono abilità trattate nei due articoli successivi, quindi potrebbe volerli leggere prima di provarli.

## Riepilogo

Questo articolo ha esplorato i concetti fondamentali dietro le funzioni, aprendo la strada al prossimo in cui ci concentreremo sugli aspetti pratici e la guideremo attraverso i passaggi per costruire la sua funzione personalizzata.

## Veda anche

- [Guida dettagliata sulle funzioni](/it/docs/Web/JavaScript/Guide/Functions) — copre alcune funzionalità avanzate non incluse qui.
- [Riferimento alle funzioni](/it/docs/Web/JavaScript/Reference/Functions)
- [Usare le funzioni per scrivere meno codice](https://scrimba.com/the-frontend-developer-career-path-c0j/~04g?via=mdn), Scrimba <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> - Una lezione interattiva che fornisce un'introduzione utile alle funzioni.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops", "Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}
