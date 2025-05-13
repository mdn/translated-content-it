---
title: Arrays
slug: Learn_web_development/Core/Scripting/Arrays
l10n:
  sourceCommit: 6a5c619dfad295ca9a9d317a4088908cfd33e686
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting")}}

In questa lezione esamineremo gli array — un modo efficiente di memorizzare una lista di elementi di dati sotto un unico nome di variabile. Approfondiremo perché questo è utile, quindi esploreremo come creare un [array](/it/docs/Web/JavaScript/Reference/Global_Objects/Array), recuperare, aggiungere e rimuovere elementi memorizzati in un array, e molto altro.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi del CSS</a>. Familiarità con i tipi di dati di base come numeri e stringhe, come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è un array — una struttura che contiene una lista di variabili.</li>
          <li>La sintassi degli array — <code>[a, b, c]</code> e la sintassi dell'accesso, <code>myArray[x]</code>.</li>
          <li>Modificare i valori degli array con <code>myArray[x] = y</code>.</li>
          <li>Manipolazione degli array usando proprietà e metodi comuni come <code>length</code>, <code>push()</code>, <code>pop()</code>, <code>join()</code>, e <code>split()</code>.</li>
          <li>Metodi avanzati degli array come <code>forEach()</code>, <code>map()</code>, e <code>filter()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un array?

Gli array sono generalmente descritti come "oggetti simili a liste"; essi sono fondamentalmente oggetti singoli che contengono più valori memorizzati in una lista. Gli oggetti array possono essere memorizzati in variabili e gestiti in modo simile a qualsiasi altro tipo di valore, con la differenza che possiamo accedere a ciascun valore all'interno della lista singolarmente, e fare cose molto utili ed efficienti con la lista, come ciclarla e fare la stessa cosa a ogni valore. Forse abbiamo una serie di articoli di prodotto e i loro prezzi memorizzati in un array, e vogliamo ciclarli tutti e stamparli su una fattura, totalizzando tutti i prezzi insieme e stampando il prezzo totale alla fine.

Se non avessimo gli array, dovremmo memorizzare ogni elemento in una variabile separata, quindi chiamare il codice che fa la stampa e l'aggiunta separatamente per ogni elemento. Questo sarebbe molto più lungo da scrivere, meno efficiente e più soggetto a errori. Se avessimo 10 elementi da aggiungere alla fattura sarebbe già fastidioso, ma cosa succede con 100 elementi, o 1000? Torneremo a questo esempio più avanti nell'articolo.

Come negli articoli precedenti, impariamo i fondamenti degli array inserendo alcuni esempi nella [console dello sviluppatore del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

> [!NOTE]
> Lo scrim di Scrimba [Aside: Intro to arrays](https://scrimba.com/the-frontend-developer-career-path-c0j/~06e?via=mdn) <sup>[_MDN learning partner_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce un'introduzione interattiva utile agli array con esempi pratici e una sfida per testare la sua conoscenza.

## Creazione di array

Gli array consistono in parentesi quadrate e elementi separati da virgole.

1. Supponiamo di voler memorizzare una lista della spesa in un array. Incolli il seguente codice nella console:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   console.log(shopping);
   ```

2. Nell'esempio precedente, ogni elemento è una stringa, ma in un array possiamo memorizzare vari tipi di dati — stringhe, numeri, oggetti, e persino altri array. Possiamo anche mescolare tipi di dati in un unico array — non siamo costretti a limitare la memorizzazione solo a numeri in un array, e in un altro solo a stringhe. Ad esempio:

   ```js
   const sequence = [1, 1, 2, 3, 5, 8, 13];
   const random = ["tree", 795, [0, 1, 2]];
   ```

3. Prima di procedere, creare alcuni array di esempio.

## Trovare la lunghezza di un array

È possibile scoprire la lunghezza di un array (quanti elementi ci sono) esattamente nello stesso modo in cui si trova la lunghezza (in caratteri) di una stringa — utilizzando la proprietà {{jsxref("Array.prototype.length","length")}}. Provare il seguente:

```js
const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
console.log(shopping.length); // 5
```

## Accesso e modifica degli elementi dell'array

Gli array sono [collezioni indicizzate](/it/docs/Web/JavaScript/Guide/Indexed_collections). Gli elementi in un array sono numerati, a partire da zero. Questo numero è chiamato _indice_ dell'elemento. Quindi il primo elemento ha indice 0, il secondo ha indice 1, e così via. Si può accedere ai singoli elementi nell'array usando la "notazione tra parentesi" e fornendo l'indice dell'elemento, allo stesso modo in cui ha [acceduto alle lettere in una stringa](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#retrieving_a_specific_string_character).

1. Inserisca quanto segue nella sua console:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   console.log(shopping[0]);
   // returns "bread"
   ```

2. Può anche modificare un elemento in un array assegnando un nuovo valore a un singolo elemento dell'array. Provi questo:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   shopping[0] = "tahini";
   console.log(shopping);
   // shopping will now return [ "tahini", "milk", "cheese", "hummus", "noodles" ]
   ```

   > [!NOTE]
   > L'abbiamo già detto, ma come promemoria — JavaScript inizia a indicizzare gli array a partire da zero!

3. Notare che un array all'interno di un array si chiama array multidimensionale. È possibile accedere a un elemento all'interno di un array che si trova esso stesso in un altro array concatenando due serie di parentesi quadre. Per esempio, per accedere a uno degli elementi all'interno dell'array che è il terzo elemento all'interno dell'array `random` (vedi la sezione precedente), si potrebbe fare qualcosa del genere:

   ```js
   const random = ["tree", 795, [0, 1, 2]];
   random[2][2];
   ```

4. Provi a fare altre modifiche ai Suoi esempi di array prima di continuare. Giochi un po', e veda cosa funziona e cosa no.

## Trovare l'indice degli elementi in un array

Se non conosce l'indice di un elemento, può utilizzare il metodo {{jsxref("Array.prototype.indexOf()","indexOf()")}}.
Il metodo `indexOf()` prende un elemento come argomento e restituirà l'indice dell'elemento o `-1` se l'elemento non è presente nell'array:

```js
const birds = ["Parrot", "Falcon", "Owl"];
console.log(birds.indexOf("Owl")); //  2
console.log(birds.indexOf("Rabbit")); // -1
```

## Aggiunta di elementi

Per aggiungere uno o più elementi alla fine di un array possiamo usare {{jsxref("Array.prototype.push()","push()")}}. Notare che è necessario includere uno o più elementi che si desidera aggiungere alla fine del Suo array.

```js
const cities = ["Manchester", "Liverpool"];
cities.push("Cardiff");
console.log(cities); // [ "Manchester", "Liverpool", "Cardiff" ]
cities.push("Bradford", "Brighton");
console.log(cities); // [ "Manchester", "Liverpool", "Cardiff", "Bradford", "Brighton" ]
```

La nuova lunghezza dell'array viene restituita quando la chiamata al metodo si completa. Se desidera memorizzare la nuova lunghezza dell'array in una variabile, potresti fare qualcosa del genere:

```js
const cities = ["Manchester", "Liverpool"];
const newLength = cities.push("Bristol");
console.log(cities); // [ "Manchester", "Liverpool", "Bristol" ]
console.log(newLength); // 3
```

Per aggiungere un elemento all'inizio dell'array, usare {{jsxref("Array.prototype.unshift()","unshift()")}}:

```js
const cities = ["Manchester", "Liverpool"];
cities.unshift("Edinburgh");
console.log(cities); // [ "Edinburgh", "Manchester", "Liverpool" ]
```

## Rimozione di elementi

Per rimuovere l'ultimo elemento dall'array, usare {{jsxref("Array.prototype.pop()","pop()")}}.

```js
const cities = ["Manchester", "Liverpool"];
cities.pop();
console.log(cities); // [ "Manchester" ]
```

Il metodo `pop()` restituisce l'elemento che è stato rimosso. Per salvare quell'elemento in una nuova variabile, si potrebbe fare questo:

```js
const cities = ["Manchester", "Liverpool"];
const removedCity = cities.pop();
console.log(removedCity); // "Liverpool"
```

Per rimuovere il primo elemento da un array, usare {{jsxref("Array.prototype.shift()","shift()")}}:

```js
const cities = ["Manchester", "Liverpool"];
cities.shift();
console.log(cities); // [ "Liverpool" ]
```

Se conosce l'indice di un elemento, può rimuoverlo dall'array usando {{jsxref("Array.prototype.splice()","splice()")}}:

```js
const cities = ["Manchester", "Liverpool", "Edinburgh", "Carlisle"];
const index = cities.indexOf("Liverpool");
if (index !== -1) {
  cities.splice(index, 1);
}
console.log(cities); // [ "Manchester", "Edinburgh", "Carlisle" ]
```

In questa chiamata a `splice()`, il primo argomento indica dove iniziare a rimuovere gli elementi, e il secondo argomento indica quanti elementi dovrebbero essere rimossi. Quindi può rimuovere più di un elemento:

```js
const cities = ["Manchester", "Liverpool", "Edinburgh", "Carlisle"];
const index = cities.indexOf("Liverpool");
if (index !== -1) {
  cities.splice(index, 2);
}
console.log(cities); // [ "Manchester", "Carlisle" ]
```

## Accesso a ogni elemento

Molto spesso vorrà accedere a ogni elemento dell'array. Può fare questo usando l'istruzione {{jsxref("statements/for...of","for...of")}}:

```js
const birds = ["Parrot", "Falcon", "Owl"];

for (const bird of birds) {
  console.log(bird);
}
```

A volte vorrà fare la stessa cosa a ciascun elemento di un array, lasciandola con un array contenente gli elementi modificati. Può fare questo usando {{jsxref("Array.prototype.map()","map()")}}. Il codice sotto prende un array di numeri e raddoppia ogni numero:

```js
function double(number) {
  return number * 2;
}
const numbers = [5, 2, 7, 6];
const doubled = numbers.map(double);
console.log(doubled); // [ 10, 4, 14, 12 ]
```

Forniamo una funzione a `map()`, e `map()` chiama la funzione una volta per ogni elemento dell'array, passando l'elemento. Aggiunge poi il valore di ritorno di ogni chiamata di funzione a un nuovo array, e infine restituisce il nuovo array.

A volte vorrà creare un nuovo array contenente solo gli elementi nell'array originale che soddisfano un certo test. Può fare questo usando {{jsxref("Array.prototype.filter()","filter()")}}. Il codice sotto prende un array di stringhe e restituisce un array contenente solo le stringhe che sono più lunghe di 8 caratteri:

```js
function isLong(city) {
  return city.length > 8;
}
const cities = ["London", "Liverpool", "Totnes", "Edinburgh"];
const longer = cities.filter(isLong);
console.log(longer); // [ "Liverpool", "Edinburgh" ]
```

Come `map()`, forniamo una funzione al metodo `filter()`, e `filter()` chiama questa funzione per ogni elemento dell'array, passando l'elemento. Se la funzione restituisce `true`, allora l'elemento viene aggiunto a un nuovo array. Infine, restituisce il nuovo array.

## Conversione tra stringhe e array

Spesso si presenterà un certo dato grezzo contenuto in una lunga stringa e si potrebbe voler separare gli elementi utili in una forma più utile e poi fare cose a essi, come visualizzarli in una tabella di dati. Per fare questo, possiamo usare il metodo {{jsxref("String.prototype.split()","split()")}}. Nella sua forma più semplice, questo prende un singolo parametro, il carattere che si desidera utilizzare per separare la stringa, e restituisce le sottostringhe tra i separatori come elementi in un array.

> [!NOTE]
> Ok, tecnicamente questo è un metodo delle stringhe, non un metodo degli array, ma lo abbiamo inserito con gli array poiché è molto pertinente qui.

1. Giochiamo con questo, per vedere come funziona. Prima di tutto, creare una stringa nella console:

   ```js
   const data = "Manchester,London,Liverpool,Birmingham,Leeds,Carlisle";
   ```

2. Ora dividiamola ad ogni virgola:

   ```js
   const cities = data.split(",");
   cities;
   ```

3. Infine, proviamo a trovare la lunghezza del nuovo array, e a recuperare alcuni elementi da esso:

   ```js
   cities.length;
   cities[0]; // the first item in the array
   cities[1]; // the second item in the array
   cities[cities.length - 1]; // the last item in the array
   ```

4. Può anche fare il contrario usando il metodo {{jsxref("Array.prototype.join()","join()")}}. Provi il seguente:

   ```js
   const commaSeparated = cities.join(",");
   commaSeparated;
   ```

5. Un altro modo di convertire un array in una stringa è usare il metodo {{jsxref("Array.prototype.toString()","toString()")}}. `toString()` è forse più semplice di `join()` poiché non prende un parametro, ma è più limitato. Con `join()` può specificare separatori diversi, mentre `toString()` usa sempre una virgola. (Provare a eseguire il Passo 4 con un carattere diverso da una virgola.)

   ```js
   const dogNames = ["Rocket", "Flash", "Bella", "Slugger"];
   dogNames.toString(); // Rocket,Flash,Bella,Slugger
   ```

## Apprendimento attivo: Stampare quei prodotti

Torniamo all'esempio che abbiamo descritto in precedenza — stampare i nomi dei prodotti e i prezzi su una fattura, poi totalizzare i prezzi e stamparli in fondo. Nell'esempio modificabile sotto ci sono commenti che contengono numeri — ognuno di questi segna un punto in cui deve aggiungere qualcosa al codice. Essi sono come segue:

1. Sotto il commento `// number 1` ci sono un certo numero di stringhe, ognuna contenente un nome di prodotto e un prezzo separati da due punti. Vorremmo che trasformasse questo in un array e lo memorizzasse in un array chiamato `products`.
2. Sotto il commento `// number 2`, inizi un ciclo `for...of()` per percorrere ogni elemento nell'array `products`.
3. Sotto il commento `// number 3` vogliamo che scriva una linea di codice che divida l'elemento corrente dell'array (`nome:prezzo`) in due elementi separati, uno contenente solo il nome e uno contenente solo il prezzo. Se non è sicuro di come fare questo, consulti l'articolo [Useful string methods](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods) per un aiuto, o meglio ancora, guardi la sezione [Conversione tra stringhe e array](#conversione_tra_stringhe_e_array) di questo articolo.
4. Come parte della linea di codice sopra, vorrà anche convertire il prezzo da una stringa a un numero. Se non si ricorda come farlo, controlli il [primo articolo sulle stringhe](/it/docs/Learn_web_development/Core/Scripting/Strings#numbers_vs._strings).
5. C'è una variabile chiamata `total` che è creata e ha un valore di 0 nella parte superiore del codice. All'interno del ciclo (sotto `// number 4`) vogliamo che aggiunga una linea che aggiunge il prezzo corrente dell'articolo a quel totale in ogni iterazione del ciclo, in modo che alla fine del codice il totale corretto sia stampato sulla fattura. Potrebbe aver bisogno di un [operatore di assegnazione](/it/docs/Learn_web_development/Core/Scripting/Math#assignment_operators) per fare questo.
6. Vogliamo che cambi la linea subito sotto `// number 5` in modo che la variabile `itemText` sia uguale a "nome dell'articolo corrente — $prezzo corrente", per esempio "Scarpe — $23.99" in ogni caso, in modo che le informazioni corrette per ogni articolo siano stampate sulla fattura. Questa è una semplice concatenazione di stringhe, che dovrebbe essere familiare a lei.
7. Infine, sotto il commento `// number 6`, avrà bisogno di aggiungere un `}` per segnare la fine del ciclo `for...of()`.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 150px;">
  <ul></ul>

  <p></p>
</div>

<h2>Editable code</h2>

<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="height: 410px;width: 95%">
const list = document.querySelector('.output ul');
const totalBox = document.querySelector('.output p');
let total = 0;
list.textContent = "";
totalBox.textContent = "";
// number 1
                'Underpants:6.99'
                'Socks:5.99'
                'T-shirt:14.99'
                'Trousers:31.99'
                'Shoes:23.99';

// number 2

  // number 3

  // number 4

  // number 5
  let itemText = 0;

  const listItem = document.createElement('li');
  listItem.textContent = itemText;
  list.appendChild(listItem);

// number 6

totalBox.textContent = 'Total: $' + total.toFixed(2);
</textarea>

<div class="playable-buttons">
  <input id="reset" type="button" value="Reset" />
  <input id="solution" type="button" value="Show solution" />
</div>
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

reset.addEventListener("click", () => {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = jsSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

const jsSolution = `const list = document.querySelector('.output ul');
const totalBox = document.querySelector('.output p');
let total = 0;
list.textContent = "";
totalBox.textContent = "";

const products = [
  'Underpants:6.99',
  'Socks:5.99',
  'T-shirt:14.99',
  'Trousers:31.99',
  'Shoes:23.99',
];

for (const product of products) {
  const subArray = product.split(':');
  const name = subArray[0];
  const price = Number(subArray[1]);
  total += price;
  const itemText = \`\${name} — $\${price}\`;

  const listItem = document.createElement('li');
  listItem.textContent = itemText;
  list.appendChild(listItem);
}

totalBox.textContent = \`Total: $\${total.toFixed(2)}\`;`;
let solutionEntry = jsSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = (event) => {
  if (event.code === "Tab") {
    event.preventDefault();
    insertAtCaret("\t");
  }
  if (event.code === "Escape") {
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
  background-color: #f5f9fa;
}
```

{{ EmbedLiveSample('Active_learning_Printing_those_products', '100%', 750) }}

## Apprendimento attivo: Le prime 5 ricerche

Un buon uso per i metodi degli array come {{jsxref("Array.prototype.push()","push()")}} e {{jsxref("Array.prototype.pop()","pop()")}} è quando sta mantenendo un record degli elementi attivi correntemente in un'app web. In una scena animata per esempio, si potrebbe avere un array di oggetti che rappresentano i grafici di sfondo attualmente visualizzati, e si potrebbe voler visualizzarne solo 50 alla volta, per ragioni di prestazioni o ingombro. Man mano che nuovi oggetti sono creati e aggiunti all'array, quelli più vecchi possono essere cancellati dall'array per mantenere il numero desiderato.

In questo esempio le mostreremo un uso molto più semplice — qui le stiamo dando un sito di ricerca fittizio, con una casella di ricerca. L'idea è che quando i termini sono inseriti nella casella di ricerca, i primi 5 termini di ricerca precedenti sono visualizzati nell'elenco. Quando il numero di termini supera 5, l'ultimo termine inizia a essere cancellato ogni volta che un nuovo termine è aggiunto in cima, quindi i 5 termini precedenti sono sempre visualizzati.

> [!NOTE]
> In un'app di ricerca reale, probabilmente sarebbe in grado di cliccare sui termini di ricerca precedenti per tornare a ricerche precedenti, e visualizzerebbe risultati di ricerca effettivi! La stiamo mantenendo semplice per ora.

Per completare l'app, noi abbiamo bisogno che Lei:

1. Aggiunga una linea sotto il commento `// number 1` che aggiunge il valore corrente inserito nella casella di ricerca all'inizio dell'array. Questo può essere recuperato usando `searchInput.value`.
2. Aggiunga una linea sotto il commento `// number 2` che rimuove il valore attualmente alla fine dell'array.

```html hidden
<h2>Live output</h2>
<div class="output" style="min-height: 150px;">
  <input type="text" /><button>Search</button>

  <ul></ul>
</div>

<h2>Editable code</h2>

<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="height: 370px; width: 95%">
const list = document.querySelector('.output ul');
const searchInput = document.querySelector('.output input');
const searchBtn = document.querySelector('.output button');

list.textContent = "";

const myHistory = [];
const MAX_HISTORY = 5;

searchBtn.onclick = () => {
  // we will only allow a term to be entered if the search input isn't empty
  if (searchInput.value !== '') {
    // number 1

    // empty the list so that we don't display duplicate entries
    // the display is regenerated every time a search term is entered.
    list.textContent = "";

    // loop through the array, and display all the search terms in the list
    for (const itemText of myHistory) {
      const listItem = document.createElement('li');
      listItem.textContent = itemText;
      list.appendChild(listItem);
    }

    // If the array length is 5 or more, remove the oldest search term
    if (myHistory.length >= MAX_HISTORY) {
      // number 2
    }

    // empty the search input and focus it, ready for the next term to be entered
    searchInput.value = '';
    searchInput.focus();
  }
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
const textarea = document.getElementById("code");
const reset = document.getElementById("reset");
const solution = document.getElementById("solution");
let code = textarea.value;
let userEntry = textarea.value;

function updateCode() {
  eval(textarea.value);
}

reset.addEventListener("click", () => {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = jsSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", () => {
  if (solution.value === "Show solution") {
    textarea.value = solutionEntry;
    solution.value = "Hide solution";
  } else {
    textarea.value = userEntry;
    solution.value = "Show solution";
  }
  updateCode();
});

const jsSolution = `const list = document.querySelector('.output ul');
const searchInput = document.querySelector('.output input');
const searchBtn = document.querySelector('.output button');

list.textContent = "";

const myHistory = [];
const MAX_HISTORY = 5;

searchBtn.onclick = () => {
  // we will only allow a term to be entered if the search input isn't empty
  if (searchInput.value !== '') {
    myHistory.unshift(searchInput.value);

    // empty the list so that we don't display duplicate entries
    // the display is regenerated every time a search term is entered.
    list.textContent = "";

    // loop through the array, and display all the search terms in the list
    for (const itemText of myHistory) {
      const listItem = document.createElement('li');
      listItem.textContent = itemText;
      list.appendChild(listItem);
    }

    // If the array length is 5 or more, remove the oldest search term
    if (myHistory.length >= MAX_HISTORY) {
      myHistory.pop();
    }

    // empty the search input and focus it, ready for the next term to be entered
    searchInput.value = '';
    searchInput.focus();
  }
}`;
let solutionEntry = jsSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = (event) => {
  if (event.code === "Tab") {
    event.preventDefault();
    insertAtCaret("\t");
  }
  if (event.code === "Escape") {
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

{{ EmbedLiveSample('Active_learning_Top_5_searches', '100%', 700) }}

## Verifica le sue abilità!

È arrivato alla fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare che abbia assimilato queste informazioni prima di andare avanti — veda [Verifica le sue abilità: Arrays](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Arrays).

## Conclusione

Dopo aver letto questo articolo, siamo sicuri che Lei sarà d'accordo sul fatto che gli array sembrano davvero utili; li vedrà apparire ovunque in JavaScript, spesso in associazione con cicli per fare la stessa cosa a ogni elemento in un array. Le insegneremo tutto sui cicli più avanti nel modulo.

Nel prossimo articolo le daremo una sfida per testare la sua comprensione degli articoli che lo precedono.

## Vedi anche

- {{jsxref("Array")}}
  - : La pagina di riferimento dell'oggetto `Array` fornisce una guida di riferimento dettagliata alle caratteristiche discusse in questa pagina, e a molte altre caratteristiche di `Array`.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting")}}
