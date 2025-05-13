---
title: Funzioni — blocchi di codice riutilizzabili
short-title: Functions
slug: Learn_web_development/Core/Scripting/Functions
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops","Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}

Un altro concetto essenziale nella programmazione è quello delle **funzioni**, che permettono di memorizzare un pezzo di codice che esegue un singolo compito all'interno di un blocco definito, e di chiamare quel codice ogni volta che serve utilizzando un singolo comando breve, piuttosto che dover digitare lo stesso codice più volte. In questo articolo esploreremo concetti fondamentali sulle funzioni come la sintassi di base, come invocarle e definirle, l'ambito, e i parametri.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e i <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Lo scopo delle funzioni — permettere la creazione di blocchi di codice riutilizzabili che possono essere chiamati dove necessario.</li>
          <li>Le funzioni sono utilizzate ovunque in JavaScript e alcune sono integrate nel browser, mentre altre sono definite dall'utente.</li>
          <li>La differenza tra funzioni e metodi.</li>
          <li>Invocare funzioni.</li>
          <li>Funzioni anonime e funzioni freccia.</li>
          <li>Definizione dei parametri delle funzioni, passaggio degli argomenti alle chiamate di funzione.</li>
          <li>Ambito globale e ambito delle funzioni/blocchi.</li>
          <li>Una comprensione di cosa sono le funzioni callback.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Dove trovo le funzioni?

In JavaScript, troverà funzioni ovunque. In effetti, abbiamo utilizzato funzioni per tutto il corso fino ad ora; semplicemente non ne abbiamo parlato molto. Ora è tempo, tuttavia, di iniziare a parlare esplicitamente delle funzioni e di esplorare davvero la loro sintassi.

Praticamente ogni volta che si utilizza una struttura JavaScript che presenta una coppia di parentesi — `()` — e **non** sta usando una struttura di linguaggio comune incorporata come un [ciclo for](/it/docs/Learn_web_development/Core/Scripting/Loops#the_standard_for_loop), [ciclo while o do...while](/it/docs/Learn_web_development/Core/Scripting/Loops#while_and_do...while), o [istruzione if...else](/it/docs/Learn_web_development/Core/Scripting/Conditionals#if...else_statements), sta usando una funzione.

## Funzioni del browser integrate

Abbiamo usato molto le funzioni integrate nel browser in questo corso.

Ogni volta che manipolavamo una stringa di testo, per esempio:

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
> Sentiti libero di inserire queste righe nella console JavaScript del suo browser per riprendere familiarità con la loro funzionalità, se necessario.

Il linguaggio JavaScript ha molte funzioni integrate che permettono di fare cose utili senza dover scrivere tutto quel codice da sola. In effetti, alcuni del codice che sta chiamando quando **invoca** (un modo elegante per dire eseguire o far girare) una funzione integrata del browser non potrebbe essere scritto in JavaScript — molte di queste funzioni stanno chiamando parti del codice di background del browser, scritto in gran parte in linguaggi di sistema di basso livello come C++, non in linguaggi web come JavaScript.

Si tenga presente che alcune funzioni del browser integrate non fanno parte del linguaggio JavaScript di base — alcune sono definite come parte delle API del browser, che si costruiscono sopra il linguaggio di default per fornire ancora più funzionalità (fare riferimento a [questa sezione iniziale del nostro corso](/it/docs/Learn_web_development/Core/Scripting/What_is_JavaScript#so_what_can_it_really_do) per ulteriori descrizioni). Approfondiremo l'uso delle API del browser in un modulo successivo.

## Funzioni versus metodi

Le **funzioni** che fanno parte degli oggetti sono chiamate **metodi**; imparerà a conoscere gli oggetti più avanti nel modulo. Per ora, volevamo solo chiarire qualsiasi possibile confusione su metodo versus funzione — è probabile che incontri entrambi i termini mentre consulta le risorse correlate disponibili sul Web.

Il codice integrato che abbiamo utilizzato finora è presente in entrambe le forme: **funzioni** e **metodi.** Può controllare l'elenco completo delle funzioni integrate, così come gli oggetti integrati e i loro corrispondenti metodi [qui](/it/docs/Web/JavaScript/Reference/Global_Objects).

Ha anche visto molte **funzioni personalizzate** nel corso finora — funzioni definite nel suo codice, non all'interno del browser. Ogni volta che vedeva un nome personalizzato con parentesi subito dopo, stava usando una funzione personalizzata. Nel nostro esempio [random-canvas-circles.html](https://mdn.github.io/learning-area/javascript/building-blocks/loops/random-canvas-circles.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html)) dal nostro [articolo sui cicli](/it/docs/Learn_web_development/Core/Scripting/Loops), abbiamo incluso una funzione personalizzata `draw()` che appariva così:

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

piuttosto che dover scrivere tutto quel codice ogni volta che vogliamo ripeterlo. Le funzioni possono contenere qualsiasi codice lei desideri — può persino chiamare altre funzioni dall'interno delle funzioni. L'interpretazione sopra chiama ad esempio la funzione `random()` tre volte, definita dal seguente codice:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}
```

Avevamo bisogno di questa funzione perché la funzione `Math.random()` integrata del browser genera solo un numero decimale casuale tra 0 e 1. Noi volevamo un numero intero casuale tra 0 e un numero specificato.

## Invocare le funzioni

Probabilmente adesso è chiaro, ma per sicurezza, per usare effettivamente una funzione dopo che è stata definita, bisogna farla eseguire — o invocarla. Questo viene fatto includendo il nome della funzione nel codice da qualche parte, seguito da parentesi.

```js
function myFunction() {
  alert("hello");
}

myFunction();
// calls the function once
```

> [!NOTE]
> Questa forma di creare una funzione è anche conosciuta come dichiarazione di funzione. Viene sempre elevata, in modo che possa chiamare la funzione al di sopra della sua definizione e funzionerà comunque.

## Parametri delle funzioni

Alcune funzioni richiedono che vengano specificati dei **parametri** quando vengono invocate — questi sono valori che devono essere inclusi all'interno delle parentesi della funzione, di cui essa ha bisogno per svolgere correttamente il suo lavoro.

> [!NOTE]
> I parametri sono talvolta chiamati argomenti, proprietà, o addirittura attributi.

Ad esempio, la funzione integrata del browser `Math.random()` non richiede alcun parametro. Quando chiamata, restituisce sempre un numero casuale tra 0 e 1:

```js
const myNumber = Math.random();
```

La funzione integrata `replace()` della stringa del browser invece ha bisogno di due parametri — la sottostringa da trovare nella stringa principale e la sottostringa con cui sostituire quella stringa:

```js
const myText = "I am a string";
const newString = myText.replace("string", "sausage");
```

> [!NOTE]
> Quando ha bisogno di specificare più parametri, sono separati da virgole.

### Parametri opzionali

A volte i parametri sono opzionali — non è necessario specificarli. Se non lo fa, la funzione adotterà generalmente un certo tipo di comportamento predefinito. Ad esempio, il parametro della funzione `join()` dell'array è opzionale:

```js
const myArray = ["I", "love", "chocolate", "frogs"];
const madeAString = myArray.join(" ");
console.log(madeAString);
// returns 'I love chocolate frogs'

const madeAnotherString = myArray.join();
console.log(madeAnotherString);
// returns 'I,love,chocolate,frogs'
```

Se non viene incluso alcun parametro per specificare un carattere di unione/delimitazione, verrà utilizzata una virgola per default.

### Parametri di default

Se sta scrivendo una funzione e vuole supportare parametri opzionali, può specificare valori di default aggiungendo `=` dopo il nome del parametro, seguito dal valore di default:

```js
function hello(name = "Chris") {
  console.log(`Hello ${name}!`);
}

hello("Ari"); // Hello Ari!
hello(); // Hello Chris!
```

## Funzioni anonime e funzioni freccia

Finora abbiamo creato una funzione come segue:

```js
function myFunction() {
  alert("hello");
}
```

Ma lei può anche creare una funzione che non ha un nome:

```js
(function () {
  alert("hello");
});
```

Questa è chiamata **funzione anonima**, perché non ha un nome. Spesso vedrà funzioni anonime quando una funzione si aspetta di ricevere un'altra funzione come parametro. In questo caso, il parametro della funzione viene spesso passato come funzione anonima.

> [!NOTE]
> Questa forma di creare una funzione è anche conosciuta come espressione di funzione. A differenza delle dichiarazioni di funzione, le espressioni di funzione non vengono elevati.

### Esempio di funzione anonima

Ad esempio, supponiamo che voglia eseguire del codice quando l'utente digita in una casella di testo. Per fare ciò, può chiamare la funzione `addEventListener()` della casella di testo. Questa funzione si aspetta che lei le passi (almeno) due parametri:

- il nome dell'evento da ascoltare, che in questo caso è [`keydown`](/it/docs/Web/API/Element/keydown_event)
- una funzione da eseguire quando l'evento si verifica.

Quando l'utente preme un tasto, il browser chiamerà la funzione che ha fornito, e le passerà un parametro contenente informazioni su questo evento, incluso il particolare tasto che l'utente ha premuto:

```js
function logKey(event) {
  console.log(`You pressed "${event.key}".`);
}

textBox.addEventListener("keydown", logKey);
```

Invece di definire una funzione `logKey()` separata, può passare una funzione anonima in `addEventListener()`:

```js
textBox.addEventListener("keydown", function (event) {
  console.log(`You pressed "${event.key}".`);
});
```

### Funzioni freccia

Se si passa una funzione anonima in questo modo, c'è una forma alternativa che può usare, chiamata **funzione freccia**. Invece di `function(event)`, si scrive `(event) =>`:

```js
textBox.addEventListener("keydown", (event) => {
  console.log(`You pressed "${event.key}".`);
});
```

Se la funzione assume solo un parametro, può omettere le parentesi attorno al parametro:

```js-nolint
textBox.addEventListener("keydown", event => {
  console.log(`You pressed "${event.key}".`);
});
```

Infine, se la sua funzione contiene solo una riga che è una dichiarazione `return`, può anche omettere le parentesi e la parola chiave `return` e restituire implicitamente l'espressione. Nell'esempio seguente, stiamo usando il metodo {{jsxref("Array.prototype.map()","map()")}} di `Array` per raddoppiare ogni valore nell'array originale:

```js-nolint
const originals = [1, 2, 3];

const doubled = originals.map(item => item * 2);

console.log(doubled); // [2, 4, 6]
```

Il metodo `map()` prende ciascun elemento nell'array a turno, passandolo nella funzione data. Quindi prende il valore restituito da quella funzione e lo aggiunge a un nuovo array.

Quindi nell'esempio sopra, `item => item * 2` è l'equivalente con funzione freccia di:

```js
function doubleItem(item) {
  return item * 2;
}
```

Può utilizzare la stessa sintassi concisa per riscrivere l'esempio `addEventListener`.

```js
textBox.addEventListener("keydown", (event) =>
  console.log(`You pressed "${event.key}".`),
);
```

In questo caso, il valore di `console.log()`, che è `undefined`, viene restituito implicitamente dalla funzione callback.

Le consigliamo di usare le funzioni freccia, poiché possono rendere il suo codice più breve e più leggibile. Per saperne di più, veda la [sezione sulle funzioni freccia nella guida JavaScript](/it/docs/Web/JavaScript/Guide/Functions#arrow_functions), e la nostra [pagina di riferimento sulle funzioni freccia](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

> [!NOTE]
> Ci sono alcune differenze sottili tra le funzioni freccia e le funzioni normali. Sono al di fuori dell'ambito di questo tutorial introduttivo e probabilmente non faranno differenza nei casi che abbiamo discusso qui. Per saperne di più, veda la [documentazione di riferimento delle funzioni freccia](/it/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

### Esempio pratico di funzione freccia

Ecco un esempio completo funzionante dell'esempio "keydown" di cui abbiamo discusso sopra:

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

Il risultato - provi a digitare nella casella di testo e veda l'output:

{{EmbedLiveSample("Arrow function live sample", 100, 100)}}

## Ambito della funzione e conflitti

Parliamo un po' di {{Glossary("scope", "ambito")}} — un concetto molto importante quando si ha a che fare con le funzioni. Quando lei crea una funzione, le variabili e altre cose definite all'interno della funzione si trovano all'interno del proprio ambito separato, il che significa che sono chiuse in compartimenti separati e non raggiungibili dal codice al di fuori delle funzioni.

L'ambito globale fuori da tutte le sue funzioni è chiamato **ambito globale**. I valori definiti nell'ambito globale sono accessibili da ovunque nel codice.

JavaScript è strutturato in questo modo per vari motivi — ma principalmente per motivi di sicurezza e organizzazione. A volte non si vuole che le variabili siano accessibili da ovunque nel codice — gli script esterni che richiama da altre fonti potrebbero iniziare a interferire con il suo codice e causare problemi perché usano gli stessi nomi di variabili di altre parti del codice, causando conflitti. Questo potrebbe essere fatto in modo dannoso, o semplicemente per errore.

Ad esempio, supponiamo di avere un file HTML che richiama due file JavaScript esterni, entrambi con una variabile e una funzione definite che usano lo stesso nome:

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

Vedrà che il secondo script non viene caricato né eseguito affatto, e viene stampato un errore nella console: `Uncaught SyntaxError: Identifier 'name' has already been declared`. Questo perché la costante `name` è già dichiarata in `first.js`, e non può dichiarare la stessa costante due volte nello stesso ambito. Poiché il secondo script non è stato caricato, la funzione `greeting()` di `second.js` non è disponibile per essere chiamata. Pertanto, vedrà una finestra di avviso che visualizza `Hello Chris: welcome to our company.`.

Provi a rimuovere la seconda riga `const name = "Zaptec";` da `second.js` e ricaricare la pagina. Ora entrambi gli script vengono eseguiti, e la finestra di avviso dice `Our company is called Chris.`. Le funzioni possono essere ridefinite, e viene utilizzata l'ultima dichiarazione effettuata. Le dichiarazioni precedenti vengono effettivamente sovrascritte.

> [!NOTE]
> Può vedere questo esempio [in esecuzione dal vivo su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/functions/conflict.html) (vedi anche il [codice sorgente](https://github.com/mdn/learning-area/tree/main/javascript/building-blocks/functions)).

Mantenere parti del suo codice chiuse in funzioni evita tali problemi, ed è considerata una buona pratica.

È un po' come uno zoo. I leoni, le zebre, le tigri e i pinguini sono tenuti nei loro rispettivi recinti e hanno accesso solo alle cose all'interno dei loro recinti — nello stesso modo degli ambiti delle funzioni. Se fossero in grado di entrare in altri recinti, sorgerebbero problemi. Nel migliore dei casi, diversi animali si sentirebbero davvero a disagio in habitat sconosciuti — un leone o una tigre si sentirebbe terribile nel dominio acquatico e glaciale dei pinguini. Nel peggiore dei casi, i leoni e le tigri potrebbero cercare di mangiare i pinguini!

![Quattro diversi animali racchiusi nel rispettivo habitat in uno Zoo](mdn-mozilla-zoo.png)

Il guardiano dello zoo è come l'ambito globale — ha le chiavi per accedere a ogni recinto, riempire il cibo, curare gli animali malati, ecc.

### Apprendimento attivo: Giocare con l'ambito

Esaminiamo un esempio reale per dimostrare l'ambito.

1. Prima copie localmente il nostro esempio [function-scope.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-scope.html). Questo contiene due funzioni chiamate `a()` e `b()`, e tre variabili — `x`, `y`, e `z` — due delle quali sono definite all'interno delle funzioni, e una nell'ambito globale. Contiene anche una terza funzione chiamata `output()`, che prende un singolo parametro e lo stampa in un paragrafo sulla pagina.
2. Apra l'esempio nel suo browser e nel suo editor di testo.
3. Apra la console JavaScript nei suoi strumenti per sviluppatori del browser. Nella console JavaScript, inserisca il seguente comando:

   ```js
   output(x);
   ```

   Dovrebbe vedere il valore della variabile `x` stampato sulla visualizzazione del browser.

4. Ora provi a inserire il seguente nella sua console

   ```js
   output(y);
   output(z);
   ```

   Entrambe dovrebbero generare un errore nella console del tipo "[ReferenceError: y is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined)". Perché? A causa dell'ambito della funzione, `y` e `z` sono bloccati all'interno delle funzioni `a()` e `b()`, quindi `output()` non può accedervi quando chiamato dall'ambito globale.

5. Tuttavia, cosa succede quando è chiamato dall'interno di un'altra funzione? Provare a modificare `a()` e `b()` in modo che appaiano così:

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

   Salvi il codice e lo ricarichi nel suo browser, quindi provi a chiamare le funzioni `a()` e `b()` dalla console JavaScript:

   ```js
   a();
   b();
   ```

   Dovrebbe vedere i valori `y` e `z` stampati nella visualizzazione del browser. Questo funziona bene, poiché la funzione `output()` viene chiamata all'interno delle altre funzioni — nello stesso ambito in cui sono definiti i parametri che sta stampando, in ciascun caso. La funzione `output()` stessa è disponibile ovunque, poiché è definita nell'ambito globale.

6. Ora provi a aggiornare il suo codice come segue:

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

7. Salvi e ricarichi di nuovo, e provi questo ancora nella sua console JavaScript:

   ```js
   a();
   b();
   ```

   Entrambe le chiamate alle funzioni `a()` e `b()` dovrebbero stampare il valore di x sulla visualizzazione del browser. Questo funziona bene perché anche se le chiamate `output()` non sono nello stesso ambito in cui `x` è definita, `x` è una variabile globale quindi è disponibile all'interno di tutto il codice, ovunque.

8. Infine, provi a aggiornare il suo codice così:

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

9. Salvi e ricarichi di nuovo, e provi questo ancora nella sua console JavaScript:

   ```js
   a();
   b();
   ```

   Questa volta le chiamate a `a()` e `b()` genereranno quel fastidioso errore [ReferenceError: _variable name_ is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined) nella console — questo perché le chiamate `output()` e le variabili che stanno tentando di stampare non si trovano negli stessi ambiti delle funzioni — le variabili sono effettivamente invisibili a quelle chiamate di funzione.

> [!NOTE]
> Le stesse regole di scopo non si applicano ai blocchi di ciclo (ad esempio, `for() { }`) e ai blocchi condizionali (ad esempio, `if () { }`) — sembrano molto simili, ma non sono la stessa cosa! Stia attento a non confonderli.

> [!NOTE]
> L'errore [ReferenceError: "x" is not defined](/it/docs/Web/JavaScript/Reference/Errors/Not_defined) è uno dei più comuni che incontrerà. Se riceve questo errore e lei è sicura di aver definito la variabile in questione, controlli in quale ambito si trova.

## Metta alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare alcuni ulteriori test per verificare di aver assimilato queste informazioni prima di procedere — veda [Metta alla prova le sue abilità: Funzioni](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Functions). Questi test richiedono abilità che sono trattate nei prossimi due articoli, quindi potrebbe volerli leggere prima di provarli.

## Riepilogo

Questo articolo ha esplorato i concetti fondamentali dietro le funzioni, spianando la strada per il prossimo in cui diventeremo pratici e la guideremo attraverso i passaggi per costruire la sua funzione personalizzata.

## Veda anche

- [Guida dettagliata alle funzioni](/it/docs/Web/JavaScript/Guide/Functions) — copre alcune funzionalità avanzate non incluse qui.
- [Riferimenti alle funzioni](/it/docs/Web/JavaScript/Reference/Functions)
- [Usare le funzioni per scrivere meno codice](https://scrimba.com/the-frontend-developer-career-path-c0j/~04g?via=mdn), Scrimba <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> - Una lezione interattiva che fornisce un'introduzione utile alle funzioni.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Loops","Learn_web_development/Core/Scripting/Build_your_own_function", "Learn_web_development/Core/Scripting")}}
