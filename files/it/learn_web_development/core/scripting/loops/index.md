---
title: Cicli nel codice
short-title: Loops
slug: Learn_web_development/Core/Scripting/Loops
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Conditionals","Learn_web_development/Core/Scripting/Functions", "Learn_web_development/Core/Scripting")}}

I linguaggi di programmazione sono molto utili per completare rapidamente compiti ripetitivi, da calcoli base multipli a quasi qualsiasi altra situazione in cui è necessario completare molti elementi di lavoro simili. Qui esamineremo le strutture di loop disponibili in JavaScript che soddisfano tali esigenze.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo dei cicli — una struttura di codice che consente di fare qualcosa di molto simile molte volte senza ripetere lo stesso codice per ogni iterazione.</li>
          <li>Tipi generali di cicli come <code>for</code> e <code>while</code>.</li>
          <li>Iterare tra le collezioni con costrutti come <code>for...of</code> e <code>map()</code>.</li>
          <li>Interrompere i cicli e continuare.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Perché i cicli sono utili?

I cicli riguardano principalmente fare la stessa cosa ripetutamente. Spesso, il codice sarà leggermente diverso ogni volta che il ciclo si ripete, oppure lo stesso codice verrà eseguito ma con variabili diverse.

### Esempio di codice con ciclo

Supponiamo di voler disegnare 100 cerchi casuali su un elemento {{htmlelement("canvas")}} (premi il pulsante _Aggiorna_ per eseguire l'esempio più volte e vedere set casuali diversi):

```html hidden
<button>Update</button> <canvas></canvas>
```

```css hidden
html {
  width: 100%;
  height: inherit;
  background: #ddd;
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

document.addEventListener("DOMContentLoaded", () => {
  canvas.width = document.documentElement.clientWidth;
  canvas.height = document.documentElement.clientHeight;
});

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

Non devi necessariamente capire tutto il codice per ora, ma diamo un'occhiata alla parte del codice che effettivamente disegna i 100 cerchi:

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

Dovresti farti un'idea di base — stiamo usando un ciclo per eseguire 100 iterazioni di questo codice, ciascuna delle quali disegna un cerchio in una posizione casuale sulla pagina. `random(x)`, definita in precedenza nel codice, restituisce un numero intero tra `0` e `x-1`. La quantità di codice necessaria sarebbe la stessa indipendentemente dal fatto che stiamo disegnando 100 cerchi, 1000 o 10.000. Solo un numero deve cambiare.

Se non stessimo usando un ciclo qui, dovremmo ripetere il seguente codice per ogni cerchio che desideriamo disegnare:

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

La maggior parte delle volte quando usi un ciclo, avrai una collezione di elementi e vorrai fare qualcosa con ogni elemento.

Un tipo di collezione è l'{{jsxref("Array")}}, che abbiamo incontrato nel capitolo [Array](/it/docs/Learn_web_development/Core/Scripting/Arrays) di questo corso. Ma ci sono anche altre collezioni in JavaScript, inclusi {{jsxref("Set")}} e {{jsxref("Map")}}.

### Il ciclo for...of

Lo strumento di base per iterare su una collezione è il ciclo {{jsxref("statements/for...of","for...of")}}:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (const cat of cats) {
  console.log(cat);
}
```

In questo esempio, `for (const cat of cats)` dice:

1. Dalla collezione `cats`, prendi il primo elemento della collezione.
2. Assegnalo alla variabile `cat` e quindi esegui il codice tra le parentesi graffe `{}`.
3. Prendi l'elemento successivo e ripeti (2) fino a raggiungere la fine della collezione.

### map() e filter()

JavaScript ha anche cicli più specializzati per le collezioni, e ne menzioneremo due qui.

Puoi usare `map()` per fare qualcosa a ogni elemento di una collezione e creare una nuova collezione contenente gli elementi modificati:

```js
function toUpper(string) {
  return string.toUpperCase();
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const upperCats = cats.map(toUpper);

console.log(upperCats);
// [ "LEOPARD", "SERVAL", "JAGUAR", "TIGER", "CARACAL", "LION" ]
```

Qui passiamo una funzione in {{jsxref("Array.prototype.map()","cats.map()")}}, e `map()` chiama la funzione una volta per ogni elemento dell'array, passandogli l'elemento. Successivamente aggiunge il valore restituito da ciascuna chiamata di funzione a un nuovo array, e infine restituisce il nuovo array. In questo caso la funzione che forniamo converte l'elemento in maiuscolo, quindi l'array risultante contiene tutti i nostri gatti in maiuscolo:

```js-nolint
[ "LEOPARD", "SERVAL", "JAGUAR", "TIGER", "CARACAL", "LION" ]
```

Puoi usare {{jsxref("Array.prototype.filter()","filter()")}} per testare ciascun elemento di una collezione e creare una nuova collezione contenente solo gli elementi che corrispondono:

```js
function lCat(cat) {
  return cat.startsWith("L");
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter(lCat);

console.log(filtered);
// [ "Leopard", "Lion" ]
```

Questo assomiglia molto a `map()`, tranne per il fatto che la funzione che passiamo restituisce un [boolean](/it/docs/Learn_web_development/Core/Scripting/Variables#booleans): se restituisce `true`, allora l'elemento è incluso nel nuovo array. La nostra funzione testa che l'elemento inizi con la lettera "L", quindi il risultato è un array contenente solo i gatti i cui nomi iniziano con "L":

```js-nolint
[ "Leopard", "Lion" ]
```

Nota che `map()` e `filter()` sono spesso usati con _espressioni di funzione_, che imparerai nella nostra lezione sulle [Funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions). Usando le espressioni di funzione potremmo riscrivere l'esempio sopra per essere molto più compatto:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter((cat) => cat.startsWith("L"));
console.log(filtered);
// [ "Leopard", "Lion" ]
```

## Il ciclo for standard

Nell'esempio "disegnare cerchi" sopra, non hai una collezione di elementi da attraversare: desideri davvero solo eseguire lo stesso codice 100 volte. In un caso come questo, puoi usare il ciclo {{jsxref("statements/for","for")}}. Ha la seguente sintassi:

```js-nolint
for (initializer; condition; final-expression) {
  // code to run
}
```

Qui abbiamo:

1. La parola chiave `for`, seguita da alcune parentesi.
2. All'interno delle parentesi abbiamo tre elementi, separati da punti e virgola:

   1. Un **inizializzatore** — questo è di solito una variabile impostata su un numero, che viene incrementata per contare il numero di volte in cui il ciclo è stato eseguito. Talvolta viene anche chiamata variabile **contatore**.
   2. Una **condizione** — questa definisce quando il ciclo dovrebbe smettere di ciclare. Questo è generalmente un'espressione che include un operatore di confronto, un test per vedere se la condizione di uscita è stata soddisfatta.
   3. Un **final-expression** — questa viene sempre valutata (o eseguita) ogni volta che il ciclo ha completato un intero passaggio. Di solito serve per incrementare (o in alcuni casi decrementare) la variabile contatore, per avvicinarla al punto in cui la condizione non è più `true`.

3. Alcune parentesi graffe che contengono un blocco di codice — questo codice verrà eseguito ogni volta che il ciclo si ripete.

### Calcolare quadrati

Osserviamo un esempio reale per visualizzare più chiaramente cosa fanno.

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

Questo ci dà il seguente output:

{{ EmbedLiveSample('Calculating squares', '100%', 250) }}

Questo codice calcola i quadrati per i numeri da 1 a 9 e scrive il risultato. Il fulcro del codice è il ciclo `for` che esegue il calcolo.

Suddividiamo la riga `for (let i = 1; i < 10; i++)` nei suoi tre pezzi:

1. `let i = 1`: la variabile contatore, `i`, inizia a `1`. Nota che dobbiamo usare `let` per il contatore, perché lo riassegniamo ogni volta che percorriamo il ciclo.
2. `i < 10`: continua a percorrere il ciclo finché `i` è minore di `10`.
3. `i++`: aggiungi uno a `i` ogni volta che percorri il ciclo.

All'interno del ciclo, calcoliamo il quadrato del valore corrente di `i`, cioè: `i * i`. Creiamo una stringa che esprime il calcolo effettuato e il risultato, e aggiungiamo questa stringa al testo di output. Aggiungiamo anche `\n`, così la prossima stringa che aggiungiamo inizierà su una nuova riga. Quindi:

1. Durante il primo passaggio, `i = 1`, quindi aggiungeremo `1 x 1 = 1`.
2. Durante il secondo passaggio, `i = 2`, quindi aggiungeremo `2 x 2 = 4`.
3. E così via…
4. Quando `i` diventa uguale a `10`, smetteremo di eseguire il ciclo e passeremo direttamente al codice successivo sotto il ciclo, stampando il messaggio `Finished!` su una nuova riga.

### Iterare attraverso le collezioni con un ciclo for

Puoi usare un ciclo `for` per iterare attraverso una collezione, invece di un ciclo `for...of`.

Vediamo di nuovo il nostro esempio `for...of` sopra:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (const cat of cats) {
  console.log(cat);
}
```

Potremmo riscrivere quel codice così:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (let i = 0; i < cats.length; i++) {
  console.log(cats[i]);
}
```

In questo ciclo stiamo iniziando `i` a `0` e fermandoci quando `i` raggiunge la lunghezza dell'array. Poi all'interno del ciclo, stiamo usando `i` per accedere a ciascun elemento dell'array a turno.

Questo funziona bene, e nelle prime versioni di JavaScript, `for...of` non esisteva, quindi questo era il modo standard per iterare attraverso un array. Tuttavia, offre più possibilità di introdurre errori nel tuo codice. Per esempio:

- potresti iniziare `i` a `1`, dimenticando che il primo indice dell'array è zero, non 1.
- potresti fermarti a `i <= cats.length`, dimenticando che l'ultimo indice dell'array è a `length - 1`.

Per ragioni come queste, è generalmente meglio utilizzare `for...of` se puoi.

A volte è ancora necessario utilizzare un ciclo `for` per iterare attraverso un array. Per esempio, nel codice sotto vogliamo registrare un messaggio che elenchi i nostri gatti:

```js
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

for (const cat of cats) {
  myFavoriteCats += `${cat}, `;
}

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, Jasmine, "
```

La frase finale di output non è molto ben formata:

```plain
My cats are called Pete, Biggles, Jasmine,
```

Preferiremmo che gestisse l'ultimo gatto in modo diverso, così:

```plain
My cats are called Pete, Biggles, and Jasmine.
```

Ma per fare questo dobbiamo sapere quando siamo sull'ultima iterazione del ciclo, e per farlo possiamo usare un ciclo `for` e esaminare il valore di `i`:

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

Se vuoi uscire da un ciclo prima di completare tutte le iterazioni, puoi usare l'istruzione [break](/it/docs/Web/JavaScript/Reference/Statements/break). Abbiamo già incontrato questo concetto nell'articolo precedente quando abbiamo esaminato le [istruzioni switch](/it/docs/Learn_web_development/Core/Scripting/Conditionals#switch_statements) — quando un caso è soddisfatto in una switch statement che corrisponde all'espressione di input, l'istruzione `break` esce immediatamente dalla switch statement e passa al controllo del codice successivo.

Lo stesso succede con i cicli — un'istruzione `break` uscirà immediatamente dal ciclo e farà sì che il browser passi a qualsiasi codice segua.

Diciamo che vogliamo cercare attraverso un array di contatti e numeri di telefono e restituire solo il numero che vogliamo trovare? Prima di tutto, un po' di HTML semplice — un {{htmlelement("input")}} di testo che ci consente di inserire un nome da cercare, un elemento {{htmlelement("button")}} per inviare una ricerca e un elemento {{htmlelement("p")}} per visualizzare i risultati:

```html
<label for="search">Search by contact name: </label>
<input id="search" type="text" />
<button>Search</button>

<p></p>
```

Ora passiamo a JavaScript:

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

1. Prima di tutto, abbiamo alcune definizioni di variabili — abbiamo un array di informazioni di contatto, con ciascun elemento che è una stringa contenente un nome e un numero di telefono separati da due punti.
2. Successivamente, colleghiamo un event listener al pulsante (`btn`) in modo che quando viene premuto, del codice venga eseguito per effettuare la ricerca e restituire i risultati.
3. Memorizziamo il valore inserito nel campo di testo in una variabile chiamata `searchName`, prima di svuotare il campo di testo e rifocalizzarlo, pronto per la ricerca successiva. Nota che eseguiamo anche il metodo [`toLowerCase()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) sulla stringa, in modo che le ricerche siano insensibili alle maiuscole.
4. Passiamo ora alla parte interessante, il ciclo `for...of`:

   1. All'interno del ciclo, innanzitutto dividiamo il contatto corrente al carattere due punti, e memorizziamo i due valori risultanti in un array chiamato `splitContact`.
   2. Utilizziamo quindi un'istruzione condizionale per testare se `splitContact[0]` (il nome del contatto, nuovamente reso in minuscolo con [`toLowerCase()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)) è uguale al `searchName` inserito. Se lo è, inseriamo un messaggio nel paragrafo per segnalare il numero del contatto e usiamo `break` per terminare il ciclo.

5. Dopo il ciclo, controlliamo se abbiamo impostato un contatto, e se non lo abbiamo fatto impostiamo il testo del paragrafo su "Contatto non trovato.".

> [!NOTE]
> Puoi visualizzare il [codice sorgente completo su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/contact-search.html) anche (vedi anche [che gira dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/loops/contact-search.html)).

## Saltare le iterazioni con continue

L'istruzione [continue](/it/docs/Web/JavaScript/Reference/Statements/continue) funziona in modo simile a `break`, ma invece di uscire completamente dal ciclo, passa alla prossima iterazione del ciclo. Vediamo un altro esempio che prende un numero come input e restituisce solo i numeri che sono quadrati di numeri interi (numeri interi).

L'HTML è fondamentalmente lo stesso dell'esempio precedente — un semplice input numerico e un paragrafo per l'output.

```html
<label for="number">Enter number: </label>
<input id="number" type="number" />
<button>Generate integer squares</button>

<p>Output:</p>
```

Il JavaScript è praticamente lo stesso, anche se il ciclo stesso è un po' diverso:

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

1. In questo caso, l'input dovrebbe essere un numero (`num`). Il ciclo `for` viene fornito con un contatore che inizia a 1 (poiché non ci interessa 0 in questo caso), una condizione di uscita che dice che il ciclo si fermerà quando il contatore diventa maggiore dell'input `num`, e un iteratore che aggiunge 1 al contatore ogni volta.
2. All'interno del ciclo, troviamo la radice quadrata di ogni numero utilizzando [`Math.sqrt(i)`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt), quindi controlliamo se la radice quadrata è un intero testando se è uguale a se stessa quando è stata arrotondata per difetto a un intero (questo è ciò che [`Math.floor()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/floor) fa al numero che riceve in ingresso).
3. Se la radice quadrata e la radice quadrata arrotondata per difetto non sono uguali (`!==`), significa che la radice quadrata non è un intero, quindi non ci interessa. In tal caso, usiamo l'istruzione `continue` per passare alla successiva iterazione del ciclo senza registrare il numero da nessuna parte.
4. Se la radice quadrata è un intero, saltiamo completamente il blocco `if`, quindi l'istruzione `continue` non viene eseguita; invece, concatenamo il valore corrente `i` più uno spazio alla fine del contenuto del paragrafo.

> [!NOTE]
> Puoi visualizzare il [codice sorgente completo su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/integer-squares.html) anche (vedi anche [che gira dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/loops/integer-squares.html)).

## while e do...while

`for` non è l'unico tipo di ciclo generale disponibile in JavaScript. Ce ne sono in realtà molti altri e, sebbene non sia necessario comprenderli tutti ora, vale la pena dare un'occhiata alla struttura di alcuni altri in modo da poter riconoscere le stesse funzionalità operare in un modo leggermente diverso.

Innanzitutto, diamo un'occhiata al ciclo [`while`](/it/docs/Web/JavaScript/Reference/Statements/while). La sintassi di questo ciclo è la seguente:

```js-nolint
initializer
while (condition) {
  // code to run

  final-expression
}
```

Questo funziona in un modo molto simile al ciclo `for`, tranne per il fatto che la variabile inizializzatrice è impostata prima del ciclo, e l'espressione finale è inclusa all'interno del ciclo dopo il codice da eseguire, piuttosto che questi due elementi essere inclusi all'interno delle parentesi. La condizione è inclusa all'interno delle parentesi, che sono precedute dalla parola chiave `while` piuttosto che da `for`.

I tre elementi sono ancora presenti e sono ancora definiti nello stesso ordine in cui si trovano nel ciclo for. Questo perché è necessario disporre di un inizializzatore definito prima di poter verificare se la condizione è vera o meno. L'espressione finale viene quindi eseguita dopo che il codice all'interno del ciclo è stato eseguito (un'iterazione è stata completata), il che accadrà solo se la condizione è ancora vera.

Vediamo di nuovo il nostro esempio della lista di gatti, ma riscritto per utilizzare un ciclo while:

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
> Questo funziona ancora esattamente come previsto — dà un'occhiata a esso [in esecuzione dal vivo su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/loops/while.html) (vedi anche il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/while.html)).

Il ciclo [`do...while`](/it/docs/Web/JavaScript/Reference/Statements/do...while) è molto simile, ma fornisce una variazione sulla struttura while:

```js-nolint
initializer
do {
  // code to run

  final-expression
} while (condition)
```

In questo caso, l'inizializzatore viene nuovamente prima, prima che inizi il ciclo. La parola chiave precede direttamente le parentesi graffe che contengono il codice da eseguire e l'espressione finale.

La differenza principale tra un ciclo `do...while` e un ciclo `while` è che _il codice all'interno di un ciclo `do...while` viene eseguito sempre almeno una volta_. Questo perché la condizione arriva dopo il codice all'interno del ciclo. Quindi eseguiamo sempre quel codice, poi verifichiamo se dobbiamo eseguirlo di nuovo. Nei cicli `while` e `for`, il controllo viene prima, quindi il codice potrebbe non essere mai eseguito.

Riscriviamo nuovamente il nostro esempio dell'elenco dei gatti per utilizzare un ciclo `do...while`:

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
> Di nuovo, questo funziona esattamente come previsto — dà un'occhiata a esso [in esecuzione dal vivo su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/loops/do-while.html) (vedi anche il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/do-while.html)).

> [!WARNING]
> Con qualsiasi tipo di ciclo, devi assicurarti che l'inizializzatore venga incrementato o, a seconda del caso, decrementato, in modo che la condizione diventi infine falsa.
> Se no, il ciclo continuerà all'infinito, e il browser o lo fermerà forzatamente, o si bloccherà. Questo è chiamato un **ciclo infinito**.

## Apprendimento attivo: conto alla rovescia

In questo esercizio, vogliamo che tu stampi un semplice conto alla rovescia sull'area di output, da 10 a Blastoff.
Nello specifico, vogliamo che tu:

- Cicla da 10 a 0. Ti abbiamo fornito un inizializzatore — `let i = 10;`.
- Per ogni iterazione, crea un nuovo paragrafo e aggiungilo al `<div>` di output, che abbiamo selezionato utilizzando `const output = document.querySelector('.output');`.
  Nei commenti, ti abbiamo fornito tre righe di codice che devono essere utilizzate da qualche parte all'interno del ciclo:

  - `const para = document.createElement('p');` — crea un nuovo paragrafo.
  - `output.appendChild(para);` — aggiunge il paragrafo al `<div>` di output.
  - `para.textContent =` — rende il testo all'interno del paragrafo uguale a ciò che metti a destra del segno di uguaglianza.

- Diversi numeri di iterazione richiedono diversi testi da mettere nel paragrafo per quell'iterazione (avrai bisogno di un'istruzione condizionale e molteplici righe `para.textContent =`):

  - Se il numero è 10, stampa "Countdown 10" nel paragrafo.
  - Se il numero è 0, stampa "Blast off!" nel paragrafo.
  - Per qualsiasi altro numero, stampa solo il numero nel paragrafo.

- Ricorda di includere un iteratore! Tuttavia, in questo esempio stiamo contando all'indietro dopo ciascuna iterazione, non verso l'alto, quindi non vuoi `i++` — come fai a iterare verso il basso?

> [!NOTE]
> Se inizi a digitare il ciclo (ad esempio (while(i>=0)), il browser potrebbe bloccarsi perché non hai ancora inserito la condizione finale. Quindi fai attenzione a questo. Puoi iniziare a scrivere il tuo codice in un commento per affrontare questo problema e rimuovere il commento dopo aver finito.

Se commetti un errore, puoi sempre reimpostare l'esempio con il pulsante "Reset".
Se ti blocchi davvero, premi "Mostra soluzione" per vedere una soluzione.

```html hidden
<h2>Live output</h2>
<div class="output" style="height: 410px;overflow: auto;"></div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>
<textarea id="code" class="playable-code" style="height: 300px;width: 95%">
const output = document.querySelector('.output');
output.textContent = "";

// let i = 10;

// const para = document.createElement('p');
// para.textContent = ;
// output.appendChild(para);
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
```

```css
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
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
let code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  eval(textarea.value);
}

reset.addEventListener("click", function () {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = jsSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", function () {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

let jsSolution = `const output = document.querySelector('.output');
output.textContent = "";

let i = 10;

while (i >= 0) {
  const para = document.createElement('p');
  if (i === 10) {
    para.textContent = \`Countdown \${i}\`;
  } else if (i === 0) {
    para.textContent = 'Blast off!';
  } else {
    para.textContent = i;
  }

  output.appendChild(para);

  i--;
}`;

let solutionEntry = jsSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = function (e) {
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

// Update the saved userCode every time the user updates the text area code

textarea.onkeyup = () => {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_Launch_countdown', '100%', 900) }}

## Apprendimento attivo: Riempire una lista degli ospiti

In questo esercizio, vogliamo che tu prenda un elenco di nomi memorizzati in un array e li metta in una lista degli ospiti. Ma non è così facile — non vogliamo far entrare Phil e Lola perché sono avidi e maleducati, e mangiano sempre tutto il cibo! Abbiamo due elenchi, uno per gli ospiti da ammettere e uno per gli ospiti da rifiutare.

Nello specifico, vogliamo che tu:

- Scrivi un ciclo che itererà attraverso l'array `people`.
- Durante ogni iterazione del ciclo, controlla se l'elemento corrente dell'array è uguale a "Phil" o "Lola" utilizzando un'istruzione condizionale:

  - Se lo è, concatena l'elemento dell'array alla fine del `textContent` del paragrafo `refused`, seguito da una virgola e uno spazio.
  - Se non lo è, concatena l'elemento dell'array alla fine del `textContent` del paragrafo `admitted`, seguito da una virgola e uno spazio.

Ti abbiamo già fornito:

- `refused.textContent +=` — l'inizio della riga che concatenerà qualcosa alla fine del `textContent` di `refused`.
- `admitted.textContent +=` — l'inizio della riga che concatenerà qualcosa alla fine del `textContent` di `admitted`.

Domanda bonus extra — dopo aver completato i compiti sopra con successo, ti ritroverai con due elenchi di nomi, separati da virgole, ma saranno disordinati — ci sarà una virgola alla fine di ciascuno. Hai capito come scrivere righe che eliminano l'ultima virgola in ciascun caso e aggiungere un punto alla fine? Dai un'occhiata all'articolo [Metodi utili sulle stringhe](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods) per avere aiuto.

Se commetti un errore, puoi sempre reimpostare l'esempio con il pulsante "Reset".
Se ti blocchi davvero, premi "Mostra soluzione" per vedere una soluzione.

```html hidden
<h2>Live output</h2>
<div class="output" style="height: 100px;overflow: auto;">
  <p class="admitted">Admit:</p>
  <p class="refused">Refuse:</p>
</div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>
<textarea id="code" class="playable-code" style="height: 400px;width: 95%">
const people = ['Chris', 'Anne', 'Colin', 'Terri', 'Phil', 'Lola', 'Sam', 'Kay', 'Bruce'];

const admitted = document.querySelector('.admitted');
const refused = document.querySelector('.refused');
admitted.textContent = 'Admit: ';
refused.textContent = 'Refuse: ';

// loop starts here

// refused.textContent += ;
// admitted.textContent += ;

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
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
let code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  eval(textarea.value);
}

reset.addEventListener("click", function () {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = jsSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", function () {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

const jsSolution = `
const people = ['Chris', 'Anne', 'Colin', 'Terri', 'Phil', 'Lola', 'Sam', 'Kay', 'Bruce'];

const admitted = document.querySelector('.admitted');
const refused = document.querySelector('.refused');

admitted.textContent = 'Admit: ';
refused.textContent = 'Refuse: ';

for (const person of people) {
  if (person === 'Phil' || person === 'Lola') {
    refused.textContent += \`\${person}, \`;
  } else {
    admitted.textContent += \`\${person}, \`;
  }
}

refused.textContent = refused.textContent.slice(0,refused.textContent.length-2) + '.';
admitted.textContent = admitted.textContent.slice(0,admitted.textContent.length-2) + '.';`;

let solutionEntry = jsSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = function (e) {
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

// Update the saved userCode every time the user updates the text area code

textarea.onkeyup = () => {
  // We only want to save the state when the user code is being shown,
  // not the solution, so that solution is not saved over the user code
  if (solution.value === "Show solution") {
    userEntry = textarea.value;
  } else {
    solutionEntry = textarea.value;
  }

  updateCode();
};
```

{{ EmbedLiveSample('Active_learning_Filling_in_a_guest_list', '100%', 680) }}

## Quale tipo di loop dovresti usare?

Se stai iterando attraverso un array o qualche altro oggetto che lo supporti, e non hai bisogno di accedere alla posizione dell'indice di ogni elemento, allora `for...of` è la scelta migliore. È più facile da leggere e ci sono meno possibilità che qualcosa vada storto.

Per altri usi, i cicli `for`, `while`, e `do...while` sono largamente intercambiabili. Possono tutti essere usati per risolvere gli stessi problemi, e quale usare dipenderà in gran parte dalle tue preferenze personali — quale trovi più facile da ricordare o più intuitivo. Raccomanderemmo `for`, almeno all'inizio, poiché è probabilmente il più facile per ricordare tutto ciò che serve — l'inizializzatore, la condizione e l'espressione finale devono andare tutte ordinatamente tra parentesi, quindi è facile vedere dove si trovano e controllare che non manchino.

Rivediamo di nuovo tutti.

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
> Ci sono anche altri tipi/funzionalità di cicli, che sono utili in situazioni avanzate/specializzate e oltre l'ambito di questo articolo. Se vuoi approfondire il tuo apprendimento sui cicli, leggi la nostra [Guida avanzata ai cicli e iterazioni](/it/docs/Web/JavaScript/Guide/Loops_and_iteration).

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare che tu abbia assimilato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Cicli](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Loops).

## Sommario

Questo articolo ti ha rivelato i concetti di base e le diverse opzioni disponibili quando si cicla il codice in JavaScript. Ora dovresti avere chiaro il motivo per cui i cicli sono un buon meccanismo per affrontare il codice ripetitivo e pronto a usarli nei tuoi esempi!

Prossimamente, analizzeremo le funzioni.

## Vedi anche

- [Dettaglio dei cicli e delle iterazioni](/it/docs/Web/JavaScript/Guide/Loops_and_iteration)
- [Riferimento for...of](/it/docs/Web/JavaScript/Reference/Statements/for...of)
- [Riferimento all'istruzione for](/it/docs/Web/JavaScript/Reference/Statements/for)
- [Riferimenti a while](/it/docs/Web/JavaScript/Reference/Statements/while) e [do...while](/it/docs/Web/JavaScript/Reference/Statements/do...while)
- [Riferimenti a break](/it/docs/Web/JavaScript/Reference/Statements/break) e [continue](/it/docs/Web/JavaScript/Reference/Statements/continue)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Conditionals","Learn_web_development/Core/Scripting/Functions", "Learn_web_development/Core/Scripting")}}
