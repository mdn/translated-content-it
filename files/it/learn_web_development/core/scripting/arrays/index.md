---
title: Array
slug: Learn_web_development/Core/Scripting/Arrays
l10n:
  sourceCommit: 6a5c619dfad295ca9a9d317a4088908cfd33e686
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting")}}

In questa lezione esamineremo gli array, un modo ordinato di memorizzare una lista di elementi dati sotto un unico nome di variabile. Qui vedremo perché questo è utile, poi esploreremo come creare un [array](/it/docs/Web/JavaScript/Reference/Global_Objects/Array), recuperare, aggiungere e rimuovere elementi memorizzati in un array, e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>. Familiarità con tipi di dati di base come numeri e stringhe, come trattato nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cos'è un array — una struttura che contiene una lista di variabili.</li>
          <li>La sintassi degli array — <code>[a, b, c]</code> e la sintassi degli accessori, <code>myArray[x]</code>.</li>
          <li>Modifica dei valori dell'array con <code>myArray[x] = y</code>.</li>
          <li>Manipolazione degli array utilizzando proprietà e metodi comuni come <code>length</code>, <code>push()</code>, <code>pop()</code>, <code>join()</code>, e <code>split()</code>.</li>
          <li>Metodi avanzati per array come <code>forEach()</code>, <code>map()</code>, e <code>filter()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cos'è un array?

Gli array sono generalmente descritti come "oggetti simili a liste"; sono fondamentalmente oggetti singoli che contengono valori multipli memorizzati in una lista. Gli oggetti array possono essere memorizzati in variabili e gestiti in modo simile a qualsiasi altro tipo di valore, la differenza è che possiamo accedere a ciascun valore all'interno della lista individualmente, e fare cose super utili ed efficienti con la lista, come scorrere attraverso essa e fare la stessa cosa per ogni valore. Forse abbiamo una serie di articoli di prodotti e i loro prezzi memorizzati in un array e vogliamo scorrerli tutti e stamparli su una fattura, mentre sommiamo tutti i prezzi insieme e stampiamo il prezzo totale in fondo.

Se non avessimo gli array, dovremmo memorizzare ogni elemento in una variabile separata, poi chiamare il codice che fa la stampa e l'aggiunta separatamente per ogni elemento. Questo sarebbe molto più lungo da scrivere, meno efficiente e più soggetto a errori. Se avessimo 10 articoli da aggiungere alla fattura sarebbe già fastidioso, ma che dire di 100 articoli o 1000? Torneremo su questo esempio più avanti nell'articolo.

Come negli articoli precedenti, impariamo i veri fondamenti degli array inserendo alcuni esempi nella [console degli sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

> [!NOTE]
> Lo script [Aside: Intro to arrays](https://scrimba.com/the-frontend-developer-career-path-c0j/~06e?via=mdn) di Scrimba <sup>[_Partner educativo MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> fornisce un'introduzione interattiva utile sugli array con esempi pratici e una sfida per testare la tua conoscenza.

## Creare array

Gli array sono costituiti da parentesi quadre e elementi separati da virgole.

1. Supponiamo di voler memorizzare una lista della spesa in un array. Incolla il codice seguente nella console:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   console.log(shopping);
   ```

2. Nell'esempio sopra, ogni elemento è una stringa, ma in un array possiamo memorizzare diversi tipi di dati — stringhe, numeri, oggetti e anche altri array. Possiamo anche mescolare i tipi di dati in un singolo array — non dobbiamo limitarci a memorizzare solo numeri in un array, e in un altro solo stringhe. Ad esempio:

   ```js
   const sequence = [1, 1, 2, 3, 5, 8, 13];
   const random = ["tree", 795, [0, 1, 2]];
   ```

3. Prima di procedere, crea qualche esempio di array.

## Trovare la lunghezza di un array

È possibile scoprire la lunghezza di un array (quanti elementi contiene) esattamente come si scopre la lunghezza (in caratteri) di una stringa — utilizzando la proprietà {{jsxref("Array.prototype.length","length")}}. Prova quanto segue:

```js
const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
console.log(shopping.length); // 5
```

## Accedere e modificare gli elementi di un array

Gli array sono [collezioni indicizzate](/it/docs/Web/JavaScript/Guide/Indexed_collections). Gli elementi in un array sono numerati, a partire da zero. Questo numero è chiamato _indice_ dell'elemento. Quindi il primo elemento ha indice 0, il secondo ha indice 1, e così via. Puoi accedere a singoli elementi nell'array utilizzando la notazione a parentesi quadre e fornendo l'indice dell'elemento, nello stesso modo in cui [accedevi alle lettere in una stringa](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#retrieving_a_specific_string_character).

1. Inserisci quanto segue nella tua console:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   console.log(shopping[0]);
   // returns "bread"
   ```

2. Puoi anche modificare un elemento in un array assegnando un nuovo valore a un singolo elemento dell'array. Prova questo:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   shopping[0] = "tahini";
   console.log(shopping);
   // shopping will now return [ "tahini", "milk", "cheese", "hummus", "noodles" ]
   ```

   > [!NOTE]
   > L'abbiamo già detto, ma come promemoria — JavaScript inizia l'indicizzazione degli array da zero!

3. Nota che un array dentro un altro array è chiamato array multidimensionale. Puoi accedere a un elemento all'interno di un array che si trova a sua volta dentro un altro array collegando insieme due insiemi di parentesi quadre. Ad esempio, per accedere a uno degli elementi dentro l'array che è il terzo elemento dentro l'array `random` (vedi sezione precedente), potremmo fare qualcosa come questo:

   ```js
   const random = ["tree", 795, [0, 1, 2]];
   random[2][2];
   ```

4. Prova a fare alcune modifiche ai tuoi esempi di array prima di procedere. Sperimenta un po', e vedi cosa funziona e cosa no.

## Trovare l'indice degli elementi in un array

Se non conosci l'indice di un elemento, puoi utilizzare il metodo {{jsxref("Array.prototype.indexOf()","indexOf()")}}.
Il metodo `indexOf()` prende un elemento come argomento e restituirà l'indice dell'elemento o `-1` se l'elemento non è presente nell'array:

```js
const birds = ["Parrot", "Falcon", "Owl"];
console.log(birds.indexOf("Owl")); //  2
console.log(birds.indexOf("Rabbit")); // -1
```

## Aggiungere elementi

Per aggiungere uno o più elementi alla fine di un array possiamo utilizzare {{jsxref("Array.prototype.push()","push()")}}. Nota che devi includere uno o più elementi che desideri aggiungere alla fine dell'array.

```js
const cities = ["Manchester", "Liverpool"];
cities.push("Cardiff");
console.log(cities); // [ "Manchester", "Liverpool", "Cardiff" ]
cities.push("Bradford", "Brighton");
console.log(cities); // [ "Manchester", "Liverpool", "Cardiff", "Bradford", "Brighton" ]
```

La nuova lunghezza dell'array viene restituita al completamento della chiamata del metodo. Se volessi memorizzare la nuova lunghezza dell'array in una variabile, potresti fare qualcosa di simile:

```js
const cities = ["Manchester", "Liverpool"];
const newLength = cities.push("Bristol");
console.log(cities); // [ "Manchester", "Liverpool", "Bristol" ]
console.log(newLength); // 3
```

Per aggiungere un elemento all'inizio dell'array, usa {{jsxref("Array.prototype.unshift()","unshift()")}}:

```js
const cities = ["Manchester", "Liverpool"];
cities.unshift("Edinburgh");
console.log(cities); // [ "Edinburgh", "Manchester", "Liverpool" ]
```

## Rimuovere elementi

Per rimuovere l'ultimo elemento dall'array, usa {{jsxref("Array.prototype.pop()","pop()")}}.

```js
const cities = ["Manchester", "Liverpool"];
cities.pop();
console.log(cities); // [ "Manchester" ]
```

Il metodo `pop()` restituisce l'elemento che è stato rimosso. Per salvare quell'elemento in una nuova variabile, potresti fare questo:

```js
const cities = ["Manchester", "Liverpool"];
const removedCity = cities.pop();
console.log(removedCity); // "Liverpool"
```

Per rimuovere il primo elemento da un array, usa {{jsxref("Array.prototype.shift()","shift()")}}:

```js
const cities = ["Manchester", "Liverpool"];
cities.shift();
console.log(cities); // [ "Liverpool" ]
```

Se conosci l'indice di un elemento, puoi rimuoverlo dall'array utilizzando {{jsxref("Array.prototype.splice()","splice()")}}:

```js
const cities = ["Manchester", "Liverpool", "Edinburgh", "Carlisle"];
const index = cities.indexOf("Liverpool");
if (index !== -1) {
  cities.splice(index, 1);
}
console.log(cities); // [ "Manchester", "Edinburgh", "Carlisle" ]
```

In questa chiamata a `splice()`, il primo argomento indica dove iniziare a rimuovere gli elementi, e il secondo argomento indica quanti elementi dovrebbero essere rimossi. Quindi puoi rimuovere più di un elemento:

```js
const cities = ["Manchester", "Liverpool", "Edinburgh", "Carlisle"];
const index = cities.indexOf("Liverpool");
if (index !== -1) {
  cities.splice(index, 2);
}
console.log(cities); // [ "Manchester", "Carlisle" ]
```

## Accedere a ogni elemento

Molto spesso vorrai accedere a ogni elemento nell'array. Puoi farlo utilizzando l'istruzione {{jsxref("statements/for...of","for...of")}}:

```js
const birds = ["Parrot", "Falcon", "Owl"];

for (const bird of birds) {
  console.log(bird);
}
```

A volte vorrai fare la stessa cosa a ciascun elemento in un array, ottenendo un array contenente gli elementi modificati. Puoi farlo utilizzando {{jsxref("Array.prototype.map()","map()")}}. Il codice seguente prende un array di numeri e raddoppia ciascun numero:

```js
function double(number) {
  return number * 2;
}
const numbers = [5, 2, 7, 6];
const doubled = numbers.map(double);
console.log(doubled); // [ 10, 4, 14, 12 ]
```

Diamo una funzione al `map()`, e `map()` chiama la funzione una volta per ogni elemento nell'array, passando l'elemento. Aggiunge poi il valore restituito da ciascuna chiamata di funzione a un nuovo array, e infine restituisce il nuovo array.

A volte vorrai creare un nuovo array contenente solo gli elementi nell'array originale che soddisfano un determinato test. Puoi farlo utilizzando {{jsxref("Array.prototype.filter()","filter()")}}. Il codice seguente prende un array di stringhe e restituisce un array contenente solo le stringhe più lunghe di 8 caratteri:

```js
function isLong(city) {
  return city.length > 8;
}
const cities = ["London", "Liverpool", "Totnes", "Edinburgh"];
const longer = cities.filter(isLong);
console.log(longer); // [ "Liverpool", "Edinburgh" ]
```

Come `map()`, diamo una funzione al metodo `filter()`, e `filter()` chiama questa funzione per ogni elemento dell'array, passando l'elemento. Se la funzione restituisce `true`, l'elemento viene aggiunto a un nuovo array. Infine restituisce il nuovo array.

## Convertire tra stringhe e array

Spesso ti troverai di fronte a dati grezzi contenuti in una lunga stringa, e potresti voler separare gli elementi utili in una forma più utile e poi fare cose con loro, come visualizzarli in una tabella di dati. Per fare questo, possiamo utilizzare il metodo {{jsxref("String.prototype.split()","split()")}}. Nella sua forma più semplice, questo metodo prende un singolo parametro, il carattere su cui vuoi separare la stringa, e restituisce le sottostringhe tra il separatore come elementi di un array.

> [!NOTE]
> Ok, tecnicamente si tratta di un metodo delle stringhe, non di un metodo degli array, ma l'abbiamo inserito insieme agli array perché si inserisce bene qui.

1. Giochiamo con questo, per vedere come funziona. Innanzitutto, crea una stringa nella tua console:

   ```js
   const data = "Manchester,London,Liverpool,Birmingham,Leeds,Carlisle";
   ```

2. Ora dividiamola ad ogni virgola:

   ```js
   const cities = data.split(",");
   cities;
   ```

3. Infine, prova a trovare la lunghezza del tuo nuovo array e a recuperare alcuni elementi da esso:

   ```js
   cities.length;
   cities[0]; // the first item in the array
   cities[1]; // the second item in the array
   cities[cities.length - 1]; // the last item in the array
   ```

4. Puoi anche procedere al contrario usando il metodo {{jsxref("Array.prototype.join()","join()")}}. Prova quanto segue:

   ```js
   const commaSeparated = cities.join(",");
   commaSeparated;
   ```

5. Un altro modo di convertire un array in una stringa è utilizzare il metodo {{jsxref("Array.prototype.toString()","toString()")}}. `toString()` è probabilmente più semplice di `join()` perché non prende un parametro, ma è più limitato. Con `join()` puoi specificare diversi separatori, mentre `toString()` usa sempre una virgola. (Prova a eseguire il Passo 4 con un carattere diverso da una virgola.)

   ```js
   const dogNames = ["Rocket", "Flash", "Bella", "Slugger"];
   dogNames.toString(); // Rocket,Flash,Bella,Slugger
   ```

## Apprendimento attivo: Stampa di quei prodotti

Ritorniamo all'esempio che abbiamo descritto in precedenza — stampare i nomi dei prodotti e i prezzi su una fattura, quindi sommare i prezzi e stamparli in fondo. Nell'esempio modificabile qui sotto ci sono commenti contenenti numeri — ognuno di questi segna un punto in cui devi aggiungere qualcosa al codice. Sono i seguenti:

1. Sotto il commento `// number 1` ci sono un certo numero di stringhe, ciascuna contenente un nome prodotto e un prezzo separati da un due punti. Vorremmo che tu trasformassi questo in un array e lo memorizzassi in un array chiamato `products`.
2. Sotto il commento `// number 2`, inizia un ciclo `for...of()` per attraversare ogni elemento dell'array `products`.
3. Sotto il commento `// number 3` vogliamo che tu scriva una linea di codice che divida l'elemento attuale dell'array (`name:price`) in due elementi separati, uno contenente solo il nome e uno contenente solo il prezzo. Se non sei sicuro di come fare, consulta l'articolo [Useful string methods](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods) per un aiuto, o meglio ancora, guarda la sezione [Converting between strings and arrays](#convertire_tra_stringhe_e_array) di questo articolo.
4. Come parte della riga di codice sopra, vorrai anche convertire il prezzo da una stringa a un numero. Se non ti ricordi come farlo, controlla il [primo articolo sulle stringhe](/it/docs/Learn_web_development/Core/Scripting/Strings#numbers_vs._strings).
5. C'è una variabile chiamata `total` che viene creata e data un valore di 0 all'inizio del codice. All'interno del ciclo (sotto `// number 4`) vogliamo che tu aggiunga una riga che sommi il prezzo dell'elemento corrente a quel totale in ogni iterazione del ciclo, in modo che alla fine del codice il totale corretto venga stampato sulla fattura. Potresti aver bisogno di un [operatore di assegnazione](/it/docs/Learn_web_development/Core/Scripting/Math#assignment_operators) per farlo.
6. Vogliamo che tu cambi la riga appena sotto `// number 5` in modo che la variabile `itemText` sia uguale a "nome dell'elemento corrente — $prezzo corrente dell'elemento", ad esempio "Scarpe — $23,99" in ogni caso, in modo che le informazioni corrette per ciascun elemento siano stampate nella fattura. Questa è semplice concatenazione di stringhe, che dovrebbe esserti familiare.
7. Infine, sotto il commento `// number 6`, dovrai aggiungere un `}` per segnare la fine del ciclo `for...of()`.

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

## Apprendimento attivo: Le 5 migliori ricerche

Un buon uso per metodi come {{jsxref("Array.prototype.push()","push()")}} e {{jsxref("Array.prototype.pop()","pop()")}} è quando si mantiene un record di elementi attualmente attivi in un'app web. In una scena animata, ad esempio, potresti avere un array di oggetti che rappresentano la grafica di sfondo attualmente visualizzata, e potresti voler visualizzare solo 50 elementi alla volta, per motivi di performance o per evitare il disordine. Mentre nuovi oggetti vengono creati e aggiunti all'array, quelli più vecchi possono essere cancellati dall'array per mantenere il numero desiderato.

In questo esempio mostreremo un uso molto più semplice — qui ti forniremo un falso sito di ricerca, con una casella di ricerca. L'idea è che, quando i termini vengono inseriti nella casella di ricerca, i primi 5 termini di ricerca precedenti vengano visualizzati nella lista. Quando il numero di termini supera 5, il termine più vecchio inizia a essere cancellato ogni volta che un nuovo termine viene aggiunto alla cima, in modo che i 5 termini precedenti siano sempre visualizzati.

> [!NOTE]
> In un'app reale di ricerca, probabilmente potresti cliccare sui termini di ricerca precedenti per tornare alle ricerche precedenti, e visualizzerebbe risultati di ricerca effettivi! Manteniamo le cose semplici per ora.

Per completare l'app, ti chiediamo di:

1. Aggiungere una riga sotto il commento `// number 1` che aggiunga il valore attualmente inserito nel campo di ricerca all'inizio dell'array. Questo può essere recuperato usando `searchInput.value`.
2. Aggiungere una riga sotto il commento `// number 2` che rimuova il valore attualmente alla fine dell'array.

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

## Metti alla prova le tue competenze!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di proseguire — vedi [Testa le tue competenze: Array](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Arrays).

## Conclusione

Dopo aver letto questo articolo, siamo sicuri che sarai d'accordo sul fatto che gli array sono estremamente utili; li vedrai apparire ovunque in JavaScript, spesso in associazione con i cicli per fare la stessa cosa a ogni elemento di un array. Ti insegneremo tutto sui cicli più avanti nel modulo.

Nel prossimo articolo ti proporremo una sfida per testare la tua comprensione degli articoli precedenti.

## Vedi anche

- {{jsxref("Array")}}
  - : La pagina di riferimento dell'oggetto `Array` fornisce una guida di riferimento dettagliata alle funzionalità discusse in questa pagina e molte altre funzionalità degli `Array`.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting")}}
