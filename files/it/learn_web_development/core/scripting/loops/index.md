---
title: Codice ciclico
short-title: Loops
slug: Learn_web_development/Core/Scripting/Loops
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Conditionals","Learn_web_development/Core/Scripting/Functions", "Learn_web_development/Core/Scripting")}}

I linguaggi di programmazione sono molto utili per completare rapidamente compiti ripetitivi, dai calcoli base multipli a quasi ogni altra situazione in cui si ha molto lavoro simile da completare. Qui esamineremo le strutture di ciclo disponibili in JavaScript che soddisfano tali esigenze.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi di JavaScript come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere lo scopo dei cicli — una struttura del codice che consente di fare qualcosa di molto simile molte volte senza ripetere lo stesso codice per ciascuna iterazione.</li>
          <li>Tipi generali di ciclo come <code>for</code> e <code>while</code>.</li>
          <li>Ciclo attraverso collezioni con costrutti come <code>for...of</code> e <code>map()</code>.</li>
          <li>Uscire dai cicli e continuare.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Perché i cicli sono utili?

I cicli riguardano il fare la stessa cosa più e più volte. Spesso, il codice sarà leggermente diverso ogni volta nel ciclo, o lo stesso codice verrà eseguito ma con variabili diverse.

### Esempio di codice ciclico

Supponiamo di voler disegnare 100 cerchi casuali su un elemento {{htmlelement("canvas")}} (premere il pulsante _Aggiorna_ per eseguire nuovamente l'esempio e vedere diversi set casuali):

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

Non è necessario comprendere tutto il codice per ora, ma diamo un'occhiata alla parte del codice che effettivamente disegna i 100 cerchi:

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

Dovrebbe avere l'idea di base — stiamo usando un ciclo per eseguire 100 iterazioni di questo codice, ciascuna delle quali disegna un cerchio in una posizione casuale sulla pagina. `random(x)`, definito precedentemente nel codice, restituisce un numero intero tra `0` e `x-1`.
La quantità di codice necessaria sarebbe la stessa sia che stessimo disegnando 100 cerchi, 1000 o 10.000.
Solo un numero deve cambiare.

Se non stessimo usando un ciclo qui, dovremmo ripetere il seguente codice per ogni cerchio che vogliamo disegnare:

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

## Ciclando attraverso una collezione

La maggior parte delle volte in cui utilizza un ciclo, avrà una collezione di elementi e vorrà fare qualcosa con ogni elemento.

Un tipo di collezione è l'{{jsxref("Array")}}, che abbiamo incontrato nel capitolo [Array](/it/docs/Learn_web_development/Core/Scripting/Arrays) di questo corso.
Ma ci sono altre collezioni in JavaScript, incluse {{jsxref("Set")}} e {{jsxref("Map")}}.

### Il ciclo for...of

Lo strumento di base per ciclando attraverso una collezione è il ciclo {{jsxref("statements/for...of","for...of")}}:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

for (const cat of cats) {
  console.log(cat);
}
```

In questo esempio, `for (const cat of cats)` dice:

1. Dato la collezione `cats`, prendi il primo elemento della collezione.
2. Assegnalo alla variabile `cat` e poi esegui il codice tra le parentesi graffe `{}`.
3. Prendi il prossimo elemento e ripeti (2) fino a quando non si è raggiunta la fine della collezione.

### map() e filter()

JavaScript ha anche cicli più specializzati per le collezioni, e ne menzioneremo due qui.

Può usare `map()` per fare qualcosa a ciascun elemento in una collezione e creare una nuova collezione contenente gli elementi cambiati:

```js
function toUpper(string) {
  return string.toUpperCase();
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const upperCats = cats.map(toUpper);

console.log(upperCats);
// [ "LEOPARD", "SERVAL", "JAGUAR", "TIGER", "CARACAL", "LION" ]
```

Qui passiamo una funzione in {{jsxref("Array.prototype.map()","cats.map()")}}, e `map()` chiama la funzione una volta per ciascun elemento nell'array, passando l'elemento. Aggiunge quindi il valore restituito da ciascuna chiamata di funzione a un nuovo array, e infine ritorna il nuovo array. In questo caso, la funzione che forniamo converte l'elemento in maiuscolo, quindi l'array risultante contiene tutti i nostri gatti in maiuscolo:

```js-nolint
[ "LEOPARD", "SERVAL", "JAGUAR", "TIGER", "CARACAL", "LION" ]
```

Può usare {{jsxref("Array.prototype.filter()","filter()")}} per testare ciascun elemento in una collezione e creare una nuova collezione contenente solo gli elementi che corrispondono:

```js
function lCat(cat) {
  return cat.startsWith("L");
}

const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter(lCat);

console.log(filtered);
// [ "Leopard", "Lion" ]
```

Questo assomiglia molto a `map()`, tranne per il fatto che la funzione che passiamo restituisce un [booleano](/it/docs/Learn_web_development/Core/Scripting/Variables#booleans): se ritorna `true`, allora l'elemento è incluso nel nuovo array.
La nostra funzione testa che l'elemento inizi con la lettera "L", quindi il risultato è un array contenente solo gatti i cui nomi iniziano con "L":

```js-nolint
[ "Leopard", "Lion" ]
```

Nota che `map()` e `filter()` sono spesso usati con _espressioni di funzione_, che imparerà nella nostra lezione [Funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions).
Usando espressioni di funzione potremmo riscrivere l'esempio sopra in modo molto più compatto:

```js
const cats = ["Leopard", "Serval", "Jaguar", "Tiger", "Caracal", "Lion"];

const filtered = cats.filter((cat) => cat.startsWith("L"));
console.log(filtered);
// [ "Leopard", "Lion" ]
```

## Il ciclo for standard

Nell'esempio "disegnare cerchi" sopra, non ha una collezione di elementi da ciclando: vuole semplicemente eseguire lo stesso codice 100 volte.
In un caso simile, può usare il ciclo {{jsxref("statements/for","for")}}.
Questo ha la seguente sintassi:

```js-nolint
for (initializer; condition; final-expression) {
  // code to run
}
```

Qui abbiamo:

1. La parola chiave `for`, seguita da alcune parentesi.
2. All'interno delle parentesi abbiamo tre elementi, separati da punti e virgola:

   1. Un **inizializzatore** — questo è di solito una variabile impostata su un numero, che viene incrementata per contare il numero di volte in cui il ciclo è stato eseguito.
      È anche talvolta riferito come una **variabile contatore**.
   2. Una **condizione** — questo definisce quando il ciclo dovrebbe smettere di ciclando.
      Questo è generalmente un'espressione caratterizzata da un operatore di confronto, un test per vedere se la condizione di uscita è stata raggiunta.
   3. Un **final-expression** — questo viene sempre valutato (o eseguito) ogni volta che il ciclo è passato attraverso una completa iterazione.
      Di solito serve a incrementare (o in alcuni casi decrementare) la variabile contatore, per avvicinarla al punto in cui la condizione non è più `true`.

3. Alcune parentesi graffe che contengono un blocco di codice — questo codice verrà eseguito ogni volta che il ciclo viene iterato.

### Calcolare i quadrati

Diamo un'occhiata a un esempio reale in modo da poter visualizzare più chiaramente cosa fanno questi elementi.

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

Questo codice calcola i quadrati per i numeri da 1 a 9 e scrive il risultato. Il nucleo del codice è il ciclo `for` che esegue il calcolo.

Diamo un'occhiata alla linea `for (let i = 1; i < 10; i++)` e scomponiamola nei suoi tre pezzi:

1. `let i = 1`: la variabile contatore, `i`, inizia a `1`. Si noti che dobbiamo utilizzare `let` per il contatore, perché lo riassegniamo ogni volta che completiamo il ciclo.
2. `i < 10`: continuiamo a ciclare finché `i` è inferiore a `10`.
3. `i++`: aggiungi uno a `i` ad ogni iterazione del ciclo.

All'interno del ciclo, calcoliamo il quadrato del valore corrente di `i`, cioè: `i * i`. Creiamo una stringa che esprima il calcolo effettuato e il risultato, e aggiungiamo questa stringa al testo di output. Aggiungiamo anche `\n`, in modo che la prossima stringa che aggiungiamo inizi su una nuova linea. Quindi:

1. Durante la prima esecuzione, `i = 1`, quindi aggiungeremo `1 x 1 = 1`.
2. Durante la seconda esecuzione, `i = 2`, quindi aggiungeremo `2 x 2 = 4`.
3. E così via…
4. Quando `i` diventa uguale a `10` interrompiamo l'esecuzione del ciclo e passiamo direttamente al pezzo di codice successivo sotto il ciclo, stampando il messaggio `Finito!` su una nuova linea.

### Ciclando attraverso collezioni con un ciclo for

Può usare un ciclo `for` per iterare attraverso una collezione, invece di un ciclo `for...of`.

Diamo un'altra occhiata al nostro esempio `for...of` sopra:

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

In questo ciclo iniziamo `i` a `0`, e fermiamo quando `i` raggiunge la lunghezza dell'array.
Poi all'interno del ciclo, stiamo usando `i` per accedere a ciascun elemento nell'array a turno.

Questo funziona perfettamente, e nelle prime versioni di JavaScript, `for...of` non esisteva, quindi questo era il modo standard per iterare attraverso un array.
Tuttavia, offre più possibilità di introdurre bug nel codice. Ad esempio:

- potrebbe iniziare `i` a `1`, dimenticando che l'indice del primo array è zero, non 1.
- potrebbe fermare a `i <= cats.length`, dimenticando che l'indice dell'ultimo array è a `length - 1`.

Per motivi come questo, è di solito meglio usare `for...of` se possibile.

A volte è ancora necessario utilizzare un ciclo `for` per iterare attraverso un array.
Ad esempio, nel codice sottostante vogliamo registrare un messaggio elencando i nostri gatti:

```js
const cats = ["Pete", "Biggles", "Jasmine"];

let myFavoriteCats = "My cats are called ";

for (const cat of cats) {
  myFavoriteCats += `${cat}, `;
}

console.log(myFavoriteCats); // "My cats are called Pete, Biggles, Jasmine, "
```

La frase di output finale non è molto ben formata:

```plain
My cats are called Pete, Biggles, Jasmine,
```

Preferiremmo che gestisse il gatto finale in modo diverso, come questo:

```plain
My cats are called Pete, Biggles, and Jasmine.
```

Ma per fare ciò dobbiamo sapere quando siamo sull'ultima iterazione del ciclo, e per farlo possiamo usare un ciclo `for` e esaminare il valore di `i`:

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

Se vuole uscire da un ciclo prima che tutte le iterazioni siano state completate, può utilizzare l'istruzione [break](/it/docs/Web/JavaScript/Reference/Statements/break).
Abbiamo già visto questo nell'articolo precedente quando abbiamo esaminato [gli switch statements](/it/docs/Learn_web_development/Core/Scripting/Conditionals#switch_statements) — quando un caso è soddisfatto in uno switch statement che corrisponde all'espressione di input, l'istruzione `break` esce immediatamente dall'istruzione switch e passa al codice successivo.

È lo stesso con i cicli — un'istruzione `break` uscirà immediatamente dal ciclo e farà muovere il browser verso il codice che lo segue.

Diciamo che volevamo cercare in un array di contatti e numeri di telefono e restituire solo il numero che volevamo trovare?
Innanzitutto, un semplice HTML — un testo {{htmlelement("input")}} che ci permetta di inserire un nome da cercare, un elemento {{htmlelement("button")}} per inviare una ricerca e un elemento {{htmlelement("p")}} per visualizzare i risultati:

```html
<label for="search">Search by contact name: </label>
<input id="search" type="text" />
<button>Search</button>

<p></p>
```

Ora passiamo al JavaScript:

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

1. Prima di tutto, abbiamo alcune definizioni di variabili — abbiamo un array di informazioni sui contatti, con ciascun elemento che è una stringa contenente un nome e un numero di telefono separati da due punti.
2. Successivamente, attacchiamo un event listener al pulsante (`btn`) in modo che quando viene premuto venga eseguito del codice per eseguire la ricerca e restituire i risultati.
3. Memorizziamo il valore inserito nel campo di testo in una variabile chiamata `searchName`, prima di svuotare il campo di testo e focalizzarlo di nuovo, pronto per la prossima ricerca.
   Si noti che eseguiamo anche il metodo [`toLowerCase()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase) sulla stringa, in modo che le ricerche siano case-insensitive.
4. Ora passiamo alla parte interessante, il ciclo `for...of`:

   1. All'interno del ciclo, dividiamo prima il contatto corrente al carattere del due punti e memorizziamo i due valori risultanti in un array chiamato `splitContact`.
   2. Utilizziamo quindi un'istruzione condizionale per verificare se `splitContact[0]` (il nome del contatto, di nuovo minuscolo con [`toLowerCase()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)) è uguale al `searchName` immesso.
      Se sì, inseriamo una stringa nel paragrafo per indicare quale sia il numero del contatto e usiamo `break` per terminare il ciclo.

5. Dopo il ciclo, verifichiamo se abbiamo inserito un contatto e, se non lo abbiamo fatto, impostiamo il testo del paragrafo su "Contatto non trovato".

> [!NOTE]
> Può visualizzare il [codice sorgente completo su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/contact-search.html) (vedi anche [il funzionamento in tempo reale](https://mdn.github.io/learning-area/javascript/building-blocks/loops/contact-search.html)).

## Saltare iterazioni con continue

L'istruzione [continue](/it/docs/Web/JavaScript/Reference/Statements/continue) funziona in modo simile a `break`, ma invece di uscire completamente dal ciclo, passa all'iterazione successiva del ciclo.
Diamo un'occhiata a un altro esempio che prende un numero come input e restituisce solo i numeri che sono quadrati di interi (numeri interi).

L'HTML è sostanzialmente lo stesso dell'esempio precedente — un semplice input numerico e un paragrafo per l'output.

```html
<label for="number">Enter number: </label>
<input id="number" type="number" />
<button>Generate integer squares</button>

<p>Output:</p>
```

Il JavaScript è quasi lo stesso, anche se il ciclo stesso è un po' diverso:

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

1. In questo caso, l'input dovrebbe essere un numero (`num`). Il ciclo `for` riceve un contatore che inizia a 1 (poiché in questo caso non siamo interessati a 0), una condizione di uscita che indica che il ciclo si fermerà quando il contatore diventa più grande di `num`, e un iteratore che aggiunge 1 al contatore a ogni iterazione del ciclo.
2. All'interno del ciclo, troviamo la radice quadrata di ciascun numero utilizzando [`Math.sqrt(i)`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/sqrt), quindi verifichiamo se la radice quadrata è un numero intero testando se è uguale a se stessa quando è stata arrotondata verso il basso al numero intero più vicino (questo è ciò che [`Math.floor()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/floor) fa al numero che gli viene passato).
3. Se la radice quadrata e la radice quadrata arrotondata verso il basso non sono uguali (`!==`), significa che la radice quadrata non è un numero intero, quindi non siamo interessati. In un caso del genere, utilizziamo l'istruzione `continue` per saltare alla prossima iterazione del ciclo senza registrare il numero da nessuna parte.
4. Se la radice quadrata è un numero intero, saltiamo completamente il blocco `if`, quindi l'istruzione `continue` non viene eseguita; invece, concatenamo il valore `i` corrente più uno spazio alla fine del contenuto del paragrafo.

> [!NOTE]
> Può visualizzare il [codice sorgente completo su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/integer-squares.html) (vedi anche [il funzionamento in tempo reale](https://mdn.github.io/learning-area/javascript/building-blocks/loops/integer-squares.html)).

## while e do...while

`for` non è l'unico tipo di ciclo generale disponibile in JavaScript. In realtà ce ne sono molti altri e, sebbene non abbia bisogno di comprenderli tutti ora, vale la pena dare un'occhiata alla struttura di un paio di altri in modo da poter riconoscere le stesse caratteristiche in un modo leggermente diverso.

Per prima cosa, diamo un'occhiata al ciclo [`while`](/it/docs/Web/JavaScript/Reference/Statements/while). La sintassi di questo ciclo è la seguente:

```js-nolint
initializer
while (condition) {
  // code to run

  final-expression
}
```

Questo funziona in modo molto simile al ciclo `for`, tranne per il fatto che la variabile inizializzatrice viene impostata prima del ciclo e l'espressione finale viene inclusa all'interno del ciclo dopo il codice da eseguire, piuttosto che questi due elementi siano inclusi all'interno delle parentesi.
La condizione è inclusa all'interno delle parentesi, che sono precedute dalla parola chiave `while` piuttosto che `for`.

I tre elementi sono ancora presenti e sono ancora definiti nello stesso ordine in cui sono nel ciclo for.
Questo perché è necessario avere un'inizializzazione definita prima di poter verificare se la condizione sia vera o meno.
L'espressione finale viene quindi eseguita dopo che il codice all'interno del ciclo è stato eseguito (un'iterazione è stata completata), il che avverrà solo se la condizione è ancora vera.

Diamo un'altra occhiata al nostro esempio di lista dei gatti, ma riscritto per utilizzare un ciclo while:

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
> Questo funziona ancora proprio come previsto — dia un'occhiata a [come funziona in tempo reale su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/loops/while.html) (visualizza anche il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/while.html)).

Il ciclo [`do...while`](/it/docs/Web/JavaScript/Reference/Statements/do...while) è molto simile, ma offre una variante della struttura while:

```js-nolint
initializer
do {
  // code to run

  final-expression
} while (condition)
```

In questo caso, l'inizializzatore viene nuovamente per primo, prima che inizi il ciclo. La parola chiave precede direttamente le parentesi graffe contenenti il codice da eseguire e l'espressione finale.

La differenza principale tra un ciclo `do...while` e un ciclo `while` è che _il codice all'interno di un ciclo `do...while` è sempre eseguito almeno una volta_. Questo perché la condizione viene dopo il codice all'interno del ciclo. Quindi eseguiamo sempre quel codice, poi verifichiamo se dobbiamo eseguirlo nuovamente. Nei cicli `while` e `for`, il controllo viene prima, quindi il codice potrebbe non essere mai eseguito.

Riscriviamo di nuovo il nostro esempio di elenco di gatti per utilizzare un ciclo `do...while`:

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
> Anche in questo caso, funziona proprio come previsto — dia un'occhiata a [come funziona in tempo reale su GitHub](https://mdn.github.io/learning-area/javascript/building-blocks/loops/do-while.html) (visualizza anche il [codice sorgente completo](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/do-while.html)).

> [!WARNING]
> Con qualsiasi tipo di ciclo, deve assicurarsi che l'inizializzatore sia incrementato o, a seconda del caso, decrementato, in modo tale che la condizione diventi infine falsa.
> In caso contrario, il ciclo continuerà all'infinito, e il browser sarà costretto a interromperlo o si bloccherà. Questo è chiamato un **ciclo infinito**.

## Apprendimento attivo: conto alla rovescia per il lancio

In questo esercizio, vogliamo che stampi un semplice conto alla rovescia per il lancio nella casella di output, da 10 fino a "Blastoff".
In particolare, vogliamo che:

- Cicli da 10 a 0. Le abbiamo fornito un inizializzatore — `let i = 10;`.
- Per ciascuna iterazione, crei un nuovo paragrafo e lo alleghi all'output `<div>`, che abbiamo selezionato utilizzando `const output = document.querySelector('.output');`.
  Nei commenti, le abbiamo fornito tre righe di codice che devono essere utilizzate da qualche parte all'interno del ciclo:

  - `const para = document.createElement('p');` — crea un nuovo paragrafo.
  - `output.appendChild(para);` — aggiunge il paragrafo all'output `<div>`.
  - `para.textContent =` — rende il testo all'interno del paragrafo uguale a qualunque cosa metta sul lato destro, dopo il segno di uguale.

- Numeri di iterazione diversi richiedono testi diversi da inserire nel paragrafo per quell'iterazione (avrà bisogno di un'istruzione condizionale e di più linee `para.textContent =`):

  - Se il numero è 10, stampi "Countdown 10" nel paragrafo.
  - Se il numero è 0, stampi "Blast off!" nel paragrafo.
  - Per qualsiasi altro numero, stampi solo il numero nel paragrafo.

- Ricordi di includere un iteratore! Tuttavia, in questo esempio stiamo diminuendo dopo ogni iterazione, non aumentando, quindi NON vuol dire `i++` — come fa a iterare verso il basso?

> [!NOTE]
> Se inizia a digitare il ciclo (ad esempio (while(i>=0)), il browser potrebbe bloccarsi perché non ha ancora inserito la condizione di fine. Quindi faccia attenzione a questo. Può iniziare a scrivere il suo codice in un commento per affrontare questo problema e rimuovere il commento dopo che ha finito.

Se sbaglia, può sempre reimpostare l'esempio con il pulsante "Reset".
Se rimane davvero bloccato, preme "Mostra soluzione" per vedere una soluzione.

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

## Apprendimento attivo: compilare una lista di ospiti

In questo esercizio, vogliamo che prenda un elenco di nomi memorizzati in un array e li inserisca in una lista di ospiti. Ma non è così semplice — non vogliamo lasciare entrare Phil e Lola perché sono avidi e maleducati e mangiano sempre tutto il cibo! Abbiamo due elenchi, uno per gli ospiti da ammettere e uno per gli ospiti da rifiutare.

In particolare, vogliamo che:

- Scriva un ciclo che iteri attraverso l'array `people`.
- Durante ogni iterazione del ciclo, verifichi se l'elemento attuale dell'array è uguale a "Phil" o "Lola" usando un'istruzione condizionale:

  - Se lo è, concatenare l'elemento dell'array alla fine del `textContent` del paragrafo `refused`, seguito da una virgola e uno spazio.
  - Se non lo è, concatenare l'elemento dell'array alla fine del `textContent` del paragrafo `admitted`, seguito da una virgola e uno spazio.

Le abbiamo già fornito:

- `refused.textContent +=` — l'inizio di una riga che concatenerà qualcosa alla fine di `refused.textContent`.
- `admitted.textContent +=` — l'inizio di una riga che concatenerà qualcosa alla fine di `admitted.textContent`.

Domanda extra bonus — dopo aver completato con successo i compiti sopra, rimarranno due elenchi di nomi, separati da virgole, ma saranno disordinati — ci sarà una virgola alla fine di ciascuno.
Può capire come scrivere righe che rimuovano l'ultima virgola in ogni caso e aggiungere un punto alla fine?
Dia un'occhiata all'articolo [Metodi di stringa utili](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods) per aiuto.

Se sbaglia, può sempre reimpostare l'esempio con il pulsante "Reset".
Se rimane davvero bloccato, preme "Mostra soluzione" per vedere una soluzione.

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

## Quale tipo di ciclo dovrebbe usare?

Se sta iterando attraverso un array o qualche altro oggetto che lo supporta, e non ha bisogno di accedere alla posizione dell'indice di ciascun elemento, allora `for...of` è la scelta migliore. È più facile da leggere e c'è meno probabilità di errore.

Per altri usi, i cicli `for`, `while` e `do...while` sono in gran parte intercambiabili.
Possono essere tutti usati per risolvere gli stessi problemi, e quale usa dipenderà in gran parte dalla sua preferenza personale — quale trova più facile da ricordare o più intuitivo.
Consigliamo `for`, almeno per iniziare, poiché è probabilmente il più facile per ricordare tutto — l'inizializzatore, la condizione e l'espressione finale devono essere inseriti con cura tra parentesi, quindi è facile vedere dove si trovano e verificare che non manchi nulla.

Diamo un'occhiata di nuovo a tutti loro.

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
> Ci sono anche altri tipi/caratteristiche di ciclo, che sono utili in situazioni avanzate/specializzate e vanno oltre l'ambito di questo articolo. Se vuole approfondire il suo apprendimento sui cicli, legga la nostra guida avanzata [Loops and iteration](/it/docs/Web/JavaScript/Guide/Loops_and_iteration).

## Verifichi le sue competenze!

È arrivato alla fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare altri test per verificare che abbia trattenuto queste informazioni prima di procedere — vedere [Testa le tue competenze: Cicli](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Loops).

## Sommario

Questo articolo le ha rivelato i concetti di base e le diverse opzioni disponibili quando si ciclano i codici in JavaScript.
Ora dovrebbe essere chiaro il motivo per cui i cicli sono un buon meccanismo per gestire il codice ripetitivo e desideroso di usarli nei propri esempi!

Successivamente, esamineremo le funzioni.

## Vedi anche

- [Cicli e iterazioni in dettaglio](/it/docs/Web/JavaScript/Guide/Loops_and_iteration)
- [for...of reference](/it/docs/Web/JavaScript/Reference/Statements/for...of)
- [for statement reference](/it/docs/Web/JavaScript/Reference/Statements/for)
- Riferimenti [while](/it/docs/Web/JavaScript/Reference/Statements/while) e [do...while](/it/docs/Web/JavaScript/Reference/Statements/do...while)
- Riferimenti [break](/it/docs/Web/JavaScript/Reference/Statements/break) e [continue](/it/docs/Web/JavaScript/Reference/Statements/continue)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Conditionals","Learn_web_development/Core/Scripting/Functions", "Learn_web_development/Core/Scripting")}}
