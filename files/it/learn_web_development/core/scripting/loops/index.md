---
title: Codice iterativo
short-title: Loops
slug: Learn_web_development/Core/Scripting/Loops
l10n:
  sourceCommit: ad310baff9ae8f5e4efd19c158125fe765287c16
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Conditionals","Learn_web_development/Core/Scripting/Test_your_skills/Loops", "Learn_web_development/Core/Scripting")}}

I linguaggi di programmazione sono molto utili per completare rapidamente attività ripetitive, da molteplici calcoli di base fino a quasi qualsiasi altra situazione in cui ci siano molti elementi di lavoro simili da completare. Qui verranno esaminate le strutture di ciclo disponibili in JavaScript che gestiscono queste esigenze.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript trattate nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo dei cicli: una struttura di codice che consente di fare qualcosa di molto simile molte volte senza ripetere lo stesso codice per ogni iterazione.</li>
          <li>Tipi di ciclo generali come <code>for</code> e <code>while</code>.</li>
          <li>Iterare sulle collezioni con costrutti come <code>for...of</code> e <code>map()</code>.</li>
          <li>Uscire dai cicli e continuare.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Perché i cicli sono utili?

I cicli consistono nell'eseguire la stessa operazione ripetutamente. Spesso il codice sarà leggermente diverso a ogni passaggio del ciclo, oppure verrà eseguito lo stesso codice ma con variabili diverse.

### Esempio di codice iterativo

Si supponga di voler disegnare 100 cerchi casuali su un elemento {{htmlelement("canvas")}} (premere il pulsante _Update_ per eseguire di nuovo l'esempio più volte e visualizzare diversi insiemi casuali):

```html hidden
<button>Update</button> <canvas></canvas>
```

```css hidden
html {
  width: 100%;
  height: inherit;
  background: #dddddd;
}

canvas {
  display: block;
}

body {
  margin: 0;
}

button {
  position: absolute;
  top: 5px;
  left: 5px;
}
```

{{ EmbedLiveSample('Looping_code_example', '100%', 400) }}

Ecco il codice JavaScript che implementa questo esempio:

```js
const btn = document.querySelector("button");
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");

canvas.width = document.documentElement.clientWidth;
canvas.height = document.documentElement.clientHeight;

function random(number) {
  return Math.floor(Math.random() * number);
}

function draw() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  for (let i = 0; i < 100; i++) {
    ctx.beginPath();
    ctx.fillStyle = "rgb(255 0 0 / 50%)";
    ctx.arc(
      random(canvas.width),
      random(canvas.height),
      random(50),
      0,
      2 * Math.PI,
    );
    ctx.fill();
  }
}

btn.addEventListener("click", draw);
```

### Con e senza un ciclo

Non è necessario comprendere tutto il codice per ora, ma esaminiamo la parte del codice che disegna effettivamente i 100 cerchi:

```js
for (let i = 0; i < 100; i++) {
  ctx.beginPath();
  ctx.fillStyle = "rgb(255 0 0 / 50%)";
  ctx.arc(
    random(canvas.width),
    random(canvas.height),
    random(50),
    0,
    2 * Math.PI,
  );
  ctx.fill();
}
```

L'idea di base dovrebbe essere chiara: viene usato un ciclo per eseguire 100 iterazioni di questo codice, ciascuna delle quali disegna un cerchio in una posizione casuale nella pagina. `random(x)`, definita in precedenza nel codice, restituisce un numero intero compreso tra `0` e `x-1`.
La quantità di codice necessaria sarebbe la stessa sia per disegnare 100 cerchi, 1000 o 10.000.
È necessario modificare un solo numero.

Se qui non venisse usato un ciclo, sarebbe necessario ripetere il seguente codice per ogni cerchio da disegnare:

```js
ctx.beginPath();
ctx.fillStyle = "rgb(255 0 0 / 50%)";
ctx.arc(
  random(canvas.width),
  random(canvas.height),
  random(50),
  0,
  2 * Math.PI,
);
ctx.fill();
```

Questo diventerebbe molto noioso e difficile da mantenere.

## Iterare su una collezione

Nella maggior parte dei casi, quando si usa un ciclo, si dispone di una collezione di elementi e si vuole fare qualcosa con ogni elemento.

Un tipo di collezione è {{jsxref("Array")}}, incontrato nel capitolo [Array](/it/docs/Learn_web_development/Core/Scripting/Arrays) di questo corso.
Ma in JavaScript esistono anche altre collezioni, incluse {{jsxref("Set")}} e {{jsxref("Map")}}.

### Il ciclo for...of

Lo strumento di base per iterare su una collezione è il ciclo {{jsxref("Statements/for...of","for...of")}}:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (const cat of cats) {
  console.log(cat);
}
```

In questo esempio, `for (const cat of cats)` significa:

1. Data la collezione `cats`, ottenere il primo elemento della collezione.
2. Assegnarlo alla variabile `cat`, quindi eseguire il codice tra le parentesi graffe `{}`.
3. Ottenere l'elemento successivo e ripetere il passaggio (2) fino a raggiungere la fine della collezione.

### map() e filter()

JavaScript dispone anche di cicli più specializzati per le collezioni; qui ne verranno menzionati due.

È possibile usare `map()` per fare qualcosa a ogni elemento di una collezione e creare una nuova collezione contenente gli elementi modificati:

```js
function toUpper(string) {
  return string.toUpperCase();
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const upperCats = cats.map(toUpper);

console.log(upperCats);
// [ "LEOPARD", "SERVAL", "JAGUAR", "TIGER", "CARACAL", "LION" ]
```

Qui viene passata una funzione a {{jsxref("Array.prototype.map()","cats.map()")}}, e `map()` chiama la funzione una volta per ogni elemento dell'array, passandole l'elemento. Aggiunge quindi il valore restituito da ogni chiamata di funzione a un nuovo array e, infine, restituisce il nuovo array. In questo caso, la funzione fornita converte l'elemento in maiuscolo, quindi l'array risultante contiene tutti i gatti in maiuscolo:

```js-nolint
[ "LEOPARD", "SERVAL", "JAGUAR", "TIGER", "CARACAL", "LION" ]
```

È possibile usare {{jsxref("Array.prototype.filter()","filter()")}} per testare ogni elemento di una collezione e creare una nuova collezione contenente solo gli elementi corrispondenti:

```js
function lCat(cat) {
  return cat.startsWith("L");
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter(lCat);

console.log(filtered);
// [ "Leopard", "Lion" ]
```

Questo è molto simile a `map()`, tranne per il fatto che la funzione passata restituisce un [booleano](/it/docs/Learn_web_development/Core/Scripting/Variables#booleans): se restituisce `true`, l'elemento viene incluso nel nuovo array.
La funzione verifica che l'elemento inizi con la lettera "L", quindi il risultato è un array contenente solo i gatti i cui nomi iniziano con "L":

```js-nolint
[ "Leopard", "Lion" ]
```

Si noti che `map()` e `filter()` vengono spesso usati entrambi con le _espressioni di funzione_, che verranno trattate nella lezione sulle [funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions).
Usando le espressioni di funzione, l'esempio precedente potrebbe essere riscritto in modo molto più compatto:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter((cat) => cat.startsWith("L"));
console.log(filtered);
// [ "Leopard", "Lion" ]
```

## Il ciclo for standard

Nell'esempio del "disegno dei cerchi" precedente, non esiste una collezione di elementi su cui iterare: si vuole semplicemente eseguire lo stesso codice 100 volte.
In un caso come questo, è possibile usare il ciclo {{jsxref("Statements/for","for")}}.
Ha la seguente sintassi:

```js-nolint
for (initializer; condition; final-expression) {
  // code to run
}
```

Qui sono presenti:

1. La parola chiave `for`, seguita da alcune parentesi.
2. All'interno delle parentesi ci sono tre elementi, separati da punti e virgola:
   1. Un **inizializzatore**: solitamente è una variabile impostata a un numero, che viene incrementata per contare il numero di volte in cui il ciclo è stato eseguito.
      A volte viene anche definita **variabile contatore**.
   2. Una **condizione**: definisce quando il ciclo deve smettere di iterare.
      Generalmente è un'espressione che include un operatore di confronto, un test per verificare se è stata raggiunta la condizione di uscita.
   3. Un'**espressione finale**: viene sempre valutata (o eseguita) ogni volta che il ciclo ha completato un'intera iterazione.
      Di solito serve a incrementare (o, in alcuni casi, decrementare) la variabile contatore, avvicinandola al punto in cui la condizione non è più `true`.

3. Alcune parentesi graffe che contengono un blocco di codice: questo codice verrà eseguito ogni volta che il ciclo itera.

> [!NOTE]
> [Aside: Loops](https://scrimba.com/learn-javascript-c0v/~02a?via=mdn) di Scrimba<sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre un'utile spiegazione interattiva della sintassi del ciclo `for`.

### Calcolare i quadrati

Esaminiamo un esempio reale per visualizzare più chiaramente cosa fanno questi elementi.

```html hidden
<button id="calculate">Calculate</button>
<button id="clear">Clear</button>
<pre id="results"></pre>
```

```js
const results = document.querySelector("#results");

function calculate() {
  for (let i = 1; i < 10; i++) {
    const newResult = `${i} x ${i} = ${i * i}`;
    results.textContent += `${newResult}\n`;
  }
  results.textContent += "\nFinished!\n\n";
}

const calculateBtn = document.querySelector("#calculate");
const clearBtn = document.querySelector("#clear");

calculateBtn.addEventListener("click", calculate);
clearBtn.addEventListener("click", () => (results.textContent = ""));
```

Questo produce il seguente output:

{{ EmbedLiveSample('Calculating squares', '100%', 250) }}

Questo codice calcola i quadrati dei numeri da 1 a 9 e scrive il risultato. Il nucleo del codice è il ciclo `for` che esegue il calcolo.

Scomponiamo la riga `for (let i = 1; i < 10; i++)` nei suoi tre elementi:

1. `let i = 1`: la variabile contatore, `i`, inizia da `1`. Si noti che è necessario usare `let` per il contatore, perché viene incrementato con `i++` (che è una riassegnazione) ogni volta che si attraversa il ciclo.
2. `i < 10`: continuare a eseguire il ciclo finché `i` è minore di `10`.
3. `i++`: aggiungere uno a `i` a ogni passaggio del ciclo.

All'interno del ciclo, viene calcolato il quadrato del valore corrente di `i`, cioè: `i * i`. Viene creata una stringa che esprime il calcolo effettuato e il risultato, e questa stringa viene aggiunta al testo di output. Viene anche aggiunto `\n`, in modo che la stringa successiva aggiunta inizi su una nuova riga. Quindi:

1. Durante la prima esecuzione, `i = 1`, quindi verrà aggiunto `1 x 1 = 1`.
2. Durante la seconda esecuzione, `i = 2`, quindi verrà aggiunto `2 x 2 = 4`.
3. E così via…
4. Quando `i` diventa uguale a `10`, il ciclo smette di essere eseguito e passa direttamente alla parte di codice successiva al ciclo, stampando il messaggio `Finished!` su una nuova riga.

### Iterare sulle collezioni con un ciclo for

È possibile usare un ciclo `for` per iterare su una collezione, invece di un ciclo `for...of`.

Esaminiamo di nuovo l'esempio `for...of` precedente:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (const cat of cats) {
  console.log(cat);
}
```

Quel codice potrebbe essere riscritto così:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (let i = 0; i < cats.length; i++) {
  console.log(cats[i]);
}
```

In questo ciclo, `i` inizia da `0` e il ciclo termina quando `i` raggiunge la lunghezza dell'array.
All'interno del ciclo, viene quindi usato `i` per accedere a ciascun elemento dell'array a turno.

Questo funziona correttamente e, nelle prime versioni di JavaScript, `for...of` non esisteva, quindi questo era il modo standard per iterare su un array.
Tuttavia, offre maggiori possibilità di introdurre bug nel codice. Ad esempio:

- `i` potrebbe iniziare da `1`, dimenticando che il primo indice dell'array è zero, non 1.
- il ciclo potrebbe terminare con `i <= cats.length`, dimenticando che l'ultimo indice dell'array è `length - 1`.

Per ragioni come queste, di solito è meglio usare `for...of`, se possibile.

Talvolta è comunque necessario usare un ciclo `for` per iterare su un array.
Ad esempio, nel codice seguente si vuole registrare un messaggio che elenchi i gatti:

```js
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

for (const cat of cats) {
  myFavoriteCats += `${cat}, `;
}

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, Jasmine, "
```

La frase finale dell'output non è formulata molto bene:

```plain
My cats are called Pete, Biggles, Jasmine,
```

Sarebbe preferibile gestire l'ultimo gatto in modo diverso, in questo modo:

```plain
My cats are called Pete, Biggles, and Jasmine.
```

Ma per farlo è necessario sapere quando si è nell'iterazione finale del ciclo e, per farlo, si può usare un ciclo `for` ed esaminare il valore di `i`:

```js
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

for (let i = 0; i < cats.length; i++) {
  if (i === cats.length - 1) {
    // We are at the end of the array
    myFavoriteCats += `and ${cats[i]}.`;
  } else {
    myFavoriteCats += `${cats[i]}, `;
  }
}

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, and Jasmine."
```

## Uscire dai cicli con break

Se si vuole uscire da un ciclo prima che tutte le iterazioni siano completate, è possibile usare l'istruzione [break](/it/docs/Web/JavaScript/Reference/Statements/break).
Questa è già stata incontrata nell'articolo precedente, esaminando le [istruzioni switch](/it/docs/Learn_web_development/Core/Scripting/Conditionals#switch_statements): quando in un'istruzione switch viene incontrato un caso che corrisponde all'espressione di input, l'istruzione `break` esce immediatamente dall'istruzione switch e passa al codice successivo.

Lo stesso vale per i cicli: un'istruzione `break` uscirà immediatamente dal ciclo e farà passare il browser a qualsiasi codice che la segue.

Si supponga di voler cercare in un array di contatti e numeri di telefono e restituire solo il numero che si desidera trovare.
Innanzitutto, un semplice HTML: un {{htmlelement("input")}} di testo che consente di inserire un nome da cercare, un elemento {{htmlelement("button")}} per inviare una ricerca e un elemento {{htmlelement("p")}} per visualizzare i risultati:

```html
<label for="search">Search by contact name: </label>
<input id="search" type="text" />
<button>Search</button>

<p></p>
```

Passiamo ora a JavaScript:

```js
const contacts = [
  "Chris:2232322",
  "Sarah:3453456",
  "Bill:7654322",
  "Mary:9998769",
  "Dianne:9384975",
];
const para = document.querySelector("p");
const input = document.querySelector("input");
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  const searchName = input.value.toLowerCase();
  input.value = "";
  input.focus();
  para.textContent = "";
  for (const contact of contacts) {
    const splitContact = contact.split(":");
    if (splitContact[0].toLowerCase() === searchName) {
      para.textContent = `${splitContact[0]}'s number is ${splitContact[1]}.`;
      break;
    }
  }
  if (para.textContent === "") {
    para.textContent = "Contact not found.";
  }
});
```

{{ EmbedLiveSample('Exiting_loops_with_break', '100%', 100) }}

1. Innanzitutto, sono presenti alcune definizioni di variabili: c'è un array di informazioni sui contatti, in cui ogni elemento è una stringa contenente un nome e un numero di telefono separati da due punti.
2. Successivamente, viene associato un event listener al pulsante (`btn`) in modo che, quando viene premuto, venga eseguito del codice per effettuare la ricerca e restituire i risultati.
3. Il valore inserito nell'input di testo viene memorizzato in una variabile denominata `searchName`, quindi l'input di testo viene svuotato e messo di nuovo a fuoco, pronto per la ricerca successiva.
   Si noti che viene anche eseguito il metodo [`toLowerCase()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) sulla stringa, in modo che le ricerche non distinguano tra maiuscole e minuscole.
4. Si arriva ora alla parte interessante, il ciclo `for...of`:
   1. All'interno del ciclo, il contatto corrente viene prima diviso sul carattere dei due punti e i due valori risultanti vengono memorizzati in un array denominato `splitContact`.
   2. Viene quindi usata un'istruzione condizionale per verificare se `splitContact[0]` (il nome del contatto, anch'esso convertito in minuscolo con [`toLowerCase()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)) è uguale al valore `searchName` inserito.
      Se lo è, viene inserita una stringa nel paragrafo per indicare il numero del contatto e viene usato `break` per terminare il ciclo.

5. Dopo il ciclo, viene verificato se è stato impostato un contatto e, in caso contrario, il testo del paragrafo viene impostato su "Contact not found.".

> [!NOTE]
> È anche possibile visualizzare il [codice sorgente completo su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/contact-search.html) (e [vederlo in esecuzione](https://mdn.github.io/learning-area/javascript/building-blocks/loops/contact-search.html)).

## Saltare iterazioni con continue

L'istruzione [continue](/it/docs/Web/JavaScript/Reference/Statements/continue) funziona in modo simile a `break`, ma invece di uscire completamente dal ciclo, passa all'iterazione successiva del ciclo.
Esaminiamo un altro esempio che prende un numero come input e restituisce solo i numeri che sono quadrati di interi (numeri interi).

L'HTML è sostanzialmente lo stesso dell'ultimo esempio: un semplice input numerico e un paragrafo per l'output.

```html
<label for="number">Enter number: </label>
<input id="number" type="number" />
<button>Generate integer squares</button>

<p>Output:</p>
```

Anche JavaScript è per lo più lo stesso, sebbene il ciclo stesso sia leggermente diverso:

```js
const para = document.querySelector("p");
const input = document.querySelector("input");
const btn = document.querySelector("button");

btn.addEventListener("click", () => {
  para.textContent = "Output: ";
  const num = input.value;
  input.value = "";
  input.focus();
  for (let i = 1; i <= num; i++) {
    let sqRoot = Math.sqrt(i);
    if (Math.floor(sqRoot) !== sqRoot) {
      continue;
    }
    para.textContent += `${i} `;
  }
});
```

Ecco l'output:

{{ EmbedLiveSample('Skipping_iterations_with_continue', '100%', 100) }}

1. In questo caso, l'input deve essere un numero (`num`). Al ciclo `for` viene fornito un contatore che inizia da 1 (poiché in questo caso non interessa 0), una condizione di uscita che indica che il ciclo si fermerà quando il contatore diventa maggiore dell'input `num`, e un iteratore che aggiunge 1 al contatore ogni volta.
2. All'interno del ciclo, viene trovata la radice quadrata di ogni numero usando [`Math.sqrt(i)`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt), quindi viene verificato se la radice quadrata è un intero controllando se è uguale a se stessa quando è stata arrotondata per difetto all'intero più vicino (questo è ciò che [`Math.floor()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/floor) fa al numero che riceve).
3. Se la radice quadrata e la radice quadrata arrotondata per difetto non sono uguali (`!==`), significa che la radice quadrata non è un intero, quindi non interessa. In tal caso, viene usata l'istruzione `continue` per passare all'iterazione successiva del ciclo senza registrare il numero da alcuna parte.
4. Se la radice quadrata è un intero, il blocco `if` viene completamente saltato, quindi l'istruzione `continue` non viene eseguita; viene invece concatenato il valore corrente di `i` più uno spazio alla fine del contenuto del paragrafo.

> [!NOTE]
> È anche possibile visualizzare il [codice sorgente completo su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/integer-squares.html) (e [vederlo in esecuzione](https://mdn.github.io/learning-area/javascript/building-blocks/loops/integer-squares.html)).

## while e do...while

`for` non è l'unico tipo di ciclo generale disponibile in JavaScript. In realtà ne esistono molti altri e, sebbene non sia necessario comprenderli tutti ora, vale la pena osservare la struttura di un altro paio di essi per poter riconoscere le stesse funzionalità in modo leggermente diverso.

Innanzitutto, esaminiamo il ciclo [`while`](/it/docs/Web/JavaScript/Reference/Statements/while). La sintassi di questo ciclo è la seguente:

```js-nolint
initializer
while (condition) {
  // code to run

  final-expression
}
```

Funziona in modo molto simile al ciclo `for`, tranne per il fatto che la variabile inizializzatore viene impostata prima del ciclo e l'espressione finale viene inclusa all'interno del ciclo dopo il codice da eseguire, invece di includere questi due elementi all'interno delle parentesi.
La condizione viene inclusa all'interno delle parentesi, precedute dalla parola chiave `while` anziché da `for`.

Gli stessi tre elementi sono ancora presenti e sono ancora definiti nello stesso ordine in cui si trovano nel ciclo for.
Questo perché è necessario definire un inizializzatore prima di poter verificare se la condizione è vera o meno.
L'espressione finale viene quindi eseguita dopo l'esecuzione del codice all'interno del ciclo (è stata completata un'iterazione), cosa che avverrà solo se la condizione è ancora vera.

Esaminiamo di nuovo l'esempio della lista di gatti, ma riscritto per usare un ciclo while:

```js
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

let i = 0;

while (i < cats.length) {
  if (i === cats.length - 1) {
    myFavoriteCats += `and ${cats[i]}.`;
  } else {
    myFavoriteCats += `${cats[i]}, `;
  }

  i++;
}

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, and Jasmine."
```

> [!NOTE]
> Funziona ancora esattamente come previsto: è possibile [vederlo in esecuzione su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/loops/while.html) (e visualizzare il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/while.html)).

Il ciclo [`do...while`](/it/docs/Web/JavaScript/Reference/Statements/do...while) è molto simile, ma offre una variante della struttura while:

```js-nolint
initializer
do {
  // code to run

  final-expression
} while (condition)
```

In questo caso, l'inizializzatore viene nuovamente prima, prima che il ciclo inizi. La parola chiave precede direttamente le parentesi graffe contenenti il codice da eseguire e l'espressione finale.

La differenza principale tra un ciclo `do...while` e un ciclo `while` è che _il codice all'interno di un ciclo `do...while` viene sempre eseguito almeno una volta_. Questo perché la condizione viene dopo il codice all'interno del ciclo. Quindi quel codice viene sempre eseguito, poi viene verificato se è necessario eseguirlo nuovamente. Nei cicli `while` e `for`, la verifica viene prima, quindi il codice potrebbe non essere mai eseguito.

Riscriviamo nuovamente l'esempio dell'elenco dei gatti per usare un ciclo `do...while`:

```js
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

let i = 0;

do {
  if (i === cats.length - 1) {
    myFavoriteCats += `and ${cats[i]}.`;
  } else {
    myFavoriteCats += `${cats[i]}, `;
  }

  i++;
} while (i < cats.length);

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, and Jasmine."
```

> [!NOTE]
> Anche in questo caso, funziona esattamente come previsto: è possibile [vederlo in esecuzione su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/loops/do-while.html) (e visualizzare il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/do-while.html)).

> [!WARNING]
> Con qualsiasi tipo di ciclo, è necessario assicurarsi che l'inizializzatore venga incrementato o, a seconda del caso, decrementato, affinché la condizione diventi infine falsa.
> In caso contrario, il ciclo continuerà per sempre e il browser ne forzerà l'arresto oppure andrà in crash. Questo è chiamato **ciclo infinito**.

## Implementare un conto alla rovescia per il lancio

In questo esercizio, si vuole stampare un semplice conto alla rovescia per il lancio nella casella di output, da 10 fino a Blastoff.

Per completare l'esercizio:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio in MDN Playground.
2. Aggiungere codice per eseguire un ciclo da 10 fino a 0. È stato fornito un inizializzatore: `let i = 10;`.
3. Per ogni iterazione, creare un nuovo paragrafo e aggiungerlo al `<div>` di output, selezionato usando `const output = document.querySelector('.output');`. Sono state fornite tre righe di codice all'interno di commenti che devono essere usate da qualche parte nel ciclo:
   1. `const para = document.createElement('p');` — crea un nuovo paragrafo.
   2. `output.appendChild(para);` — aggiunge il paragrafo al `<div>` di output.
   3. `para.textContent =` — rende il testo all'interno del paragrafo uguale a qualsiasi elemento venga inserito sul lato destro, dopo il segno di uguale.
4. Per i diversi numeri di iterazione elencati di seguito, scrivere codice per inserire il testo richiesto all'interno del paragrafo (saranno necessarie un'istruzione condizionale e più righe `para.textContent =`):
   1. Se il numero è 10, stampare "Countdown 10" nel paragrafo.
   2. Se il numero è 0, stampare "Blast off!" nel paragrafo.
   3. Per qualsiasi altro numero, stampare solo il numero nel paragrafo.
5. Ricordare di includere un iteratore. Tuttavia, in questo esempio il conteggio diminuisce dopo ogni iterazione, non aumenta, quindi **non** serve `i++`: come si itera verso il basso?

> [!NOTE]
> Se si inizia a digitare il ciclo (ad esempio `(while(i>=0)`), il browser potrebbe bloccarsi in un ciclo infinito perché non è stata ancora inserita la condizione finale. Prestare quindi attenzione. Per gestire questo problema, è possibile iniziare a scrivere il codice in un commento e rimuovere il commento dopo aver terminato.

Se viene commesso un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ in MDN Playground. Se si rimane davvero bloccati, è possibile visualizzare la soluzione sotto l'output live.

```html hidden live-sample___loops-1
<div class="output"></div>
```

```css hidden live-sample___loops-1
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

.output {
  height: 410px;
  overflow: auto;
}
```

```js live-sample___loops-1
const output = document.querySelector(".output");
output.textContent = "";

// let i = 10;

// const para = document.createElement('p');
// para.textContent = ;
// output.appendChild(para);
```

{{ EmbedLiveSample("loops-1", "100%", 200) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe apparire più o meno così:

```js
const output = document.querySelector(".output");
output.textContent = "";

let i = 10;

while (i >= 0) {
  const para = document.createElement("p");
  if (i === 10) {
    para.textContent = `Countdown ${i}`;
  } else if (i === 0) {
    para.textContent = "Blast off!";
  } else {
    para.textContent = i;
  }

  output.appendChild(para);

  i--;
}
```

</details>

## Compilare una lista di invitati

In questo esercizio, si vuole prendere una lista di nomi memorizzata in un array e inserirla in una lista di invitati. Ma non è così semplice: non si vogliono far entrare Phil e Lola perché sono avidi e maleducati e mangiano sempre tutto il cibo. Ci sono due liste, una per gli invitati da ammettere e una per gli invitati da rifiutare.

Per completare l'esercizio:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio in MDN Playground.
2. Scrivere un ciclo che iteri sull'array `people`.
3. Durante ogni iterazione del ciclo, verificare se l'elemento corrente dell'array è uguale a "Phil" o "Lola" usando un'istruzione condizionale:
   1. Se lo è, concatenare l'elemento dell'array alla fine di `textContent` del paragrafo `refused`, seguito da una virgola e uno spazio.
   2. Se non lo è, concatenare l'elemento dell'array alla fine di `textContent` del paragrafo `admitted`, seguito da una virgola e uno spazio.

Sono già stati forniti:

- `refused.textContent +=` — l'inizio di una riga che concatenerà qualcosa alla fine di `refused.textContent`.
- `admitted.textContent +=` — l'inizio di una riga che concatenerà qualcosa alla fine di `admitted.textContent`.

Domanda bonus: dopo aver completato correttamente le attività precedenti, rimarranno due liste di nomi separati da virgole, ma non saranno ordinate: alla fine di ciascuna ci sarà una virgola. Si riesce a capire come scrivere righe che rimuovano l'ultima virgola in ciascun caso e aggiungano un punto finale?
Per assistenza, consultare l'articolo sui [metodi utili per le stringhe](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods).

Se viene commesso un errore, è possibile cancellare il lavoro usando il pulsante _Reset_ in MDN Playground. Se si rimane davvero bloccati, è possibile visualizzare la soluzione sotto l'output live.

```html hidden live-sample___loops-2
<div class="output">
  <p class="admitted">Admit:</p>
  <p class="refused">Refuse:</p>
</div>
```

```css hidden live-sample___loops-2
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

.output {
  height: 100px;
  overflow: auto;
}
```

```js live-sample___loops-2
const people = [
  "Chris",
  "Anne",
  "Colin",
  "Terri",
  "Phil",
  "Lola",
  "Sam",
  "Kay",
  "Bruce",
];

const admitted = document.querySelector(".admitted");
const refused = document.querySelector(".refused");
admitted.textContent = "Admit: ";
refused.textContent = "Refuse: ";

// loop starts here

// refused.textContent += ...;
// admitted.textContent += ...;
```

{{ EmbedLiveSample("loops-2", "100%", 200) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe apparire più o meno così:

```js
const people = [
  "Chris",
  "Anne",
  "Colin",
  "Terri",
  "Phil",
  "Lola",
  "Sam",
  "Kay",
  "Bruce",
];

const admitted = document.querySelector(".admitted");
const refused = document.querySelector(".refused");

admitted.textContent = "Admit: ";
refused.textContent = "Refuse: ";

for (const person of people) {
  if (person === "Phil" || person === "Lola") {
    refused.textContent += `${person}, `;
  } else {
    admitted.textContent += `${person}, `;
  }
}

refused.textContent = `${refused.textContent.slice(0, -2)}.`;
admitted.textContent = `${admitted.textContent.slice(0, -2)}.`;
```

</details>

## Quale tipo di ciclo usare?

Se si sta iterando su un array o su un altro oggetto che lo supporta e non è necessario accedere alla posizione dell'indice di ogni elemento, allora `for...of` è la scelta migliore. È più facile da leggere e ci sono meno possibilità di errore.

Per altri utilizzi, i cicli `for`, `while` e `do...while` sono in gran parte intercambiabili.
Tutti possono essere usati per risolvere gli stessi problemi e la scelta dipenderà principalmente dalle preferenze personali: quale risulta più facile da ricordare o più intuitivo.
Si consiglia `for`, almeno all'inizio, poiché probabilmente è il più semplice per ricordare tutto: inizializzatore, condizione ed espressione finale devono essere ordinatamente inseriti nelle parentesi, quindi è facile vedere dove si trovano e verificare che non ne manchi nessuno.

Esaminiamoli tutti di nuovo.

Prima `for...of`:

```js-nolint
for (const item of array) {
  // code to run
}
```

`for`:

```js-nolint
for (initializer; condition; final-expression) {
  // code to run
}
```

`while`:

```js-nolint
initializer
while (condition) {
  // code to run

  final-expression
}
```

e infine `do...while`:

```js-nolint
initializer
do {
  // code to run

  final-expression
} while (condition)
```

> [!NOTE]
> Esistono anche altri tipi/funzionalità di cicli, utili in situazioni avanzate o specializzate e oltre lo scopo di questo articolo. Per approfondire i cicli, leggere la [guida avanzata sui cicli e l'iterazione](/it/docs/Web/JavaScript/Guide/Loops_and_iteration).

## Riepilogo

Questo articolo ha illustrato i concetti di base e le diverse opzioni disponibili per l'esecuzione iterativa del codice in JavaScript.
Ora dovrebbe essere chiaro perché i cicli sono un buon meccanismo per gestire codice ripetitivo e si dovrebbe essere pronti a usarli nei propri esempi.

Nel prossimo articolo verranno proposti alcuni test che consentiranno di verificare quanto bene siano state comprese e memorizzate queste informazioni.

## Vedi anche

- [Cicli e iterazione in dettaglio](/it/docs/Web/JavaScript/Guide/Loops_and_iteration)
- [Riferimento di for...of](/it/docs/Web/JavaScript/Reference/Statements/for...of)
- [Riferimento dell'istruzione for](/it/docs/Web/JavaScript/Reference/Statements/for)
- Riferimenti di [while](/it/docs/Web/JavaScript/Reference/Statements/while) e [do...while](/it/docs/Web/JavaScript/Reference/Statements/do...while)
- Riferimenti di [break](/it/docs/Web/JavaScript/Reference/Statements/break) e [continue](/it/docs/Web/JavaScript/Reference/Statements/continue)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Conditionals","Learn_web_development/Core/Scripting/Test_your_skills/Loops", "Learn_web_development/Core/Scripting")}}
