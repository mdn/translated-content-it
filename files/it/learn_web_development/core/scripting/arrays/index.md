---
title: Array
slug: Learn_web_development/Core/Scripting/Arrays
l10n:
  sourceCommit: 0abb70602b0b3b11a2909c417a03e10eabd607a8
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Strings", "Learn_web_development/Core/Scripting/Test_your_skills/Arrays", "Learn_web_development/Core/Scripting")}}

In questa lezione esamineremo gli array, un modo pratico per memorizzare un elenco di elementi di dati sotto un singolo nome di variabile. Vedremo perché questo è utile, poi esploreremo come creare un [array](/it/docs/Web/JavaScript/Reference/Global_Objects/Array), recuperare, aggiungere e rimuovere elementi memorizzati in un array e molto altro.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>. Familiarità con i tipi di dati di base come numeri e stringhe, trattati nelle lezioni precedenti.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cos'è un array: una struttura che contiene un elenco di variabili.</li>
          <li>La sintassi degli array: <code>[a, b, c]</code> e la sintassi di accesso, <code>myArray[x]</code>.</li>
          <li>Modificare i valori degli array con <code>myArray[x] = y</code>.</li>
          <li>Manipolare gli array usando proprietà e metodi comuni come <code>length</code>, <code>push()</code>, <code>pop()</code>, <code>join()</code> e <code>split()</code>.</li>
          <li>Metodi avanzati per gli array come <code>forEach()</code>, <code>map()</code> e <code>filter()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è un array?

Gli array sono generalmente descritti come "oggetti simili a elenchi"; sono fondamentalmente singoli oggetti che contengono più valori memorizzati in un elenco. Gli oggetti array possono essere memorizzati nelle variabili e gestiti più o meno allo stesso modo di qualsiasi altro tipo di valore; la differenza è che è possibile accedere singolarmente a ogni valore dell'elenco e svolgere operazioni molto utili ed efficienti con l'elenco, come iterarlo ed eseguire la stessa operazione su ogni valore. Ad esempio, potrebbe esserci una serie di prodotti e relativi prezzi memorizzati in un array e si potrebbe volerli iterare tutti per stamparli su una fattura, sommando al contempo tutti i prezzi e stampando il prezzo totale in fondo.

Se non esistessero gli array, sarebbe necessario memorizzare ogni elemento in una variabile separata, quindi chiamare separatamente per ogni elemento il codice che esegue la stampa e la somma. Questo codice sarebbe molto più lungo da scrivere, meno efficiente e più soggetto a errori. Se ci fossero 10 elementi da aggiungere alla fattura sarebbe già fastidioso, ma cosa succederebbe con 100 elementi, o 1000? Torneremo su questo esempio più avanti nell'articolo.

Come negli articoli precedenti, impariamo le basi essenziali degli array inserendo alcuni esempi nella [console per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

> [!NOTE]
> Lo scrim di Scrimba [Aside: Intro to arrays](https://scrimba.com/the-frontend-developer-career-path-c0j/~06e?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre un'utile introduzione interattiva agli array, con spiegazioni guidate di esempi e una sfida per verificare le proprie conoscenze.

## Creare array

Gli array sono costituiti da parentesi quadre ed elementi separati da virgole.

1. Si supponga di voler memorizzare una lista della spesa in un array. Incollare il codice seguente nella console:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   console.log(shopping);
   ```

2. Nell'esempio precedente, ogni elemento è una stringa, ma in un array è possibile memorizzare vari tipi di dati: stringhe, numeri, oggetti e persino altri array. È inoltre possibile combinare tipi di dati in un singolo array: non è necessario limitarsi a memorizzare soltanto numeri in un array e soltanto stringhe in un altro. Ad esempio:

   ```js
   const sequence = [1, 1, 2, 3, 5, 8, 13];
   const random = ["tree", 795, [0, 1, 2]];
   ```

3. Prima di procedere, creare alcuni array di esempio.

## Trovare la lunghezza di un array

È possibile scoprire la lunghezza di un array, ovvero quanti elementi contiene, esattamente nello stesso modo in cui si scopre la lunghezza, in caratteri, di una stringa: usando la proprietà {{jsxref("Array.prototype.length","length")}}. Provare quanto segue:

```js
const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
console.log(shopping.length); // 5
```

## Accedere agli elementi di un array e modificarli

Gli array sono [collezioni indicizzate](/it/docs/Web/JavaScript/Guide/Indexed_collections). Gli elementi di un array sono numerati a partire da zero. Questo numero è chiamato _indice_ dell'elemento. Il primo elemento ha quindi indice 0, il secondo ha indice 1 e così via. È possibile accedere ai singoli elementi dell'array usando la notazione con parentesi quadre e fornendo l'indice dell'elemento, nello stesso modo in cui si [accedeva alle lettere in una stringa](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods#retrieving_a_specific_string_character).

1. Inserire quanto segue nella console:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   console.log(shopping[0]);
   // returns "bread"
   ```

2. È inoltre possibile modificare un elemento in un array assegnando un nuovo valore a un singolo elemento dell'array. Provare questo:

   ```js
   const shopping = ["bread", "milk", "cheese", "hummus", "noodles"];
   shopping[0] = "tahini";
   console.log(shopping);
   // shopping will now return [ "tahini", "milk", "cheese", "hummus", "noodles" ]
   ```

   > [!NOTE]
   > È già stato detto, ma come promemoria: JavaScript inizia a indicizzare gli array da zero!

3. Un array all'interno di un array è chiamato array multidimensionale. È possibile accedere a un elemento dentro un array che si trova a sua volta all'interno di un altro array concatenando due coppie di parentesi quadre. Ad esempio, per accedere a uno degli elementi dell'array che costituisce il terzo elemento dell'array `random` (vedere la sezione precedente), si potrebbe fare qualcosa di simile:

   ```js
   const random = ["tree", 795, [0, 1, 2]];
   random[2][2];
   ```

4. Provare a effettuare altre modifiche agli array di esempio prima di proseguire. Sperimentare un po' e verificare cosa funziona e cosa non funziona.

## Trovare l'indice degli elementi in un array

Se non si conosce l'indice di un elemento, è possibile usare il metodo {{jsxref("Array.prototype.indexOf()","indexOf()")}}.
Il metodo `indexOf()` accetta un elemento come argomento e restituisce l'indice dell'elemento oppure `-1` se l'elemento non è presente nell'array:

```js
const birds = ["Parrot", "Falcon", "Owl"];
console.log(birds.indexOf("Owl")); //  2
console.log(birds.indexOf("Rabbit")); // -1
```

## Aggiungere elementi

Per aggiungere uno o più elementi alla fine di un array è possibile usare {{jsxref("Array.prototype.push()","push()")}}. Occorre includere uno o più elementi da aggiungere alla fine dell'array.

```js
const cities = ["Manchester", "Liverpool"];
cities.push("Cardiff");
console.log(cities); // [ "Manchester", "Liverpool", "Cardiff" ]
cities.push("Bradford", "Brighton");
console.log(cities); // [ "Manchester", "Liverpool", "Cardiff", "Bradford", "Brighton" ]
```

Al termine della chiamata al metodo viene restituita la nuova lunghezza dell'array. Per memorizzare la nuova lunghezza dell'array in una variabile, si potrebbe fare qualcosa di simile:

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

## Rimuovere elementi

Per rimuovere l'ultimo elemento dall'array, usare {{jsxref("Array.prototype.pop()","pop()")}}.

```js
const cities = ["Manchester", "Liverpool"];
cities.pop();
console.log(cities); // [ "Manchester" ]
```

Il metodo `pop()` restituisce l'elemento rimosso. Per salvare quell'elemento in una nuova variabile, si potrebbe fare così:

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

Se si conosce l'indice di un elemento, è possibile rimuoverlo dall'array usando {{jsxref("Array.prototype.splice()","splice()")}}:

```js
const cities = ["Manchester", "Liverpool", "Edinburgh", "Carlisle"];
const index = cities.indexOf("Liverpool");
if (index !== -1) {
  cities.splice(index, 1);
}
console.log(cities); // [ "Manchester", "Edinburgh", "Carlisle" ]
```

In questa chiamata a `splice()`, il primo argomento indica da dove iniziare a rimuovere gli elementi, mentre il secondo argomento indica quanti elementi devono essere rimossi. È quindi possibile rimuovere più di un elemento:

```js
const cities = ["Manchester", "Liverpool", "Edinburgh", "Carlisle"];
const index = cities.indexOf("Liverpool");
if (index !== -1) {
  cities.splice(index, 2);
}
console.log(cities); // [ "Manchester", "Carlisle" ]
```

## Accedere a ogni elemento

Molto spesso sarà necessario accedere a ogni elemento dell'array. È possibile farlo usando l'istruzione {{jsxref("Statements/for...of","for...of")}}:

```js
const birds = ["Parrot", "Falcon", "Owl"];

for (const bird of birds) {
  console.log(bird);
}
```

A volte sarà necessario eseguire la stessa operazione su ogni elemento di un array, ottenendo un array contenente gli elementi modificati. È possibile farlo usando {{jsxref("Array.prototype.map()","map()")}}. Il codice seguente prende un array di numeri e raddoppia ciascun numero:

```js
function double(number) {
  return number * 2;
}
const numbers = [5, 2, 7, 6];
const doubled = numbers.map(double);
console.log(doubled); // [ 10, 4, 14, 12 ]
```

Viene fornita una funzione a `map()`, che chiama la funzione una volta per ogni elemento dell'array, passandole l'elemento. Quindi aggiunge il valore restituito da ogni chiamata di funzione a un nuovo array e, infine, restituisce il nuovo array.

A volte sarà necessario creare un nuovo array che contenga soltanto gli elementi dell'array originale che soddisfano un determinato test. È possibile farlo usando {{jsxref("Array.prototype.filter()","filter()")}}. Il codice seguente prende un array di stringhe e restituisce un array contenente soltanto le stringhe lunghe più di 8 caratteri:

```js
function isLong(city) {
  return city.length > 8;
}
const cities = ["London", "Liverpool", "Totnes", "Edinburgh"];
const longer = cities.filter(isLong);
console.log(longer); // [ "Liverpool", "Edinburgh" ]
```

Come per `map()`, viene fornita una funzione al metodo `filter()`, che chiama questa funzione per ogni elemento dell'array, passandole l'elemento. Se la funzione restituisce `true`, l'elemento viene aggiunto a un nuovo array. Infine restituisce il nuovo array.

## Convertire tra stringhe e array

Spesso vengono forniti dati grezzi contenuti in un'unica lunga stringa e può essere utile separare gli elementi utili in una forma più gestibile per poi elaborarli, ad esempio visualizzandoli in una tabella di dati. A questo scopo, è possibile usare il metodo {{jsxref("String.prototype.split()","split()")}}. Nella sua forma più semplice, accetta un singolo parametro, il carattere in corrispondenza del quale separare la stringa, e restituisce le sottostringhe tra i separatori come elementi di un array.

> [!NOTE]
> Tecnicamente questo è un metodo delle stringhe, non degli array, ma è stato incluso nella sezione degli array perché qui si adatta bene.

1. Sperimentiamo per vedere come funziona. Per prima cosa, creare una stringa nella console:

   ```js
   const data = "Manchester,London,Liverpool,Birmingham,Leeds,Carlisle";
   ```

2. Ora separiamola in corrispondenza di ogni virgola:

   ```js
   const cities = data.split(",");
   cities;
   ```

3. Infine, provare a trovare la lunghezza del nuovo array e a recuperare alcuni elementi da esso:

   ```js
   cities.length;
   cities[0]; // the first item in the array
   cities[1]; // the second item in the array
   cities[cities.length - 1]; // the last item in the array
   ```

4. È anche possibile procedere nella direzione opposta usando il metodo {{jsxref("Array.prototype.join()","join()")}}. Provare quanto segue:

   ```js
   const commaSeparated = cities.join(",");
   commaSeparated;
   ```

5. Un altro modo per convertire un array in una stringa è usare il metodo {{jsxref("Array.prototype.toString()","toString()")}}. `toString()` è probabilmente più semplice di `join()` poiché non accetta parametri, ma è più limitante. Con `join()` è possibile specificare separatori diversi, mentre `toString()` usa sempre una virgola. Provare a eseguire il passaggio 4 con un carattere diverso dalla virgola.

   ```js
   const dogNames = ["Rocket", "Flash", "Bella", "Slugger"];
   dogNames.toString(); // Rocket,Flash,Bella,Slugger
   ```

## Stampare quei prodotti

Ora è il momento di mettere in pratica quanto appreso. In questo esercizio si tornerà all'esempio descritto in precedenza: stampare i nomi e i prezzi dei prodotti su una fattura, poi sommare i prezzi e stampare il totale in fondo. Seguire i passaggi seguenti per implementare la logica necessaria.

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio in MDN Playground.
2. Sotto il commento `// Part 1` sono presenti diverse stringhe, ciascuna contenente un nome di prodotto e un prezzo separati da due punti. Rimuovere il commento da queste righe e trasformarle in un array chiamato `products`.
3. Sotto il commento `// Part 2`, avviare un ciclo `for...of()` per esaminare ogni elemento dell'array `products`.
4. Sotto il commento `// Part 3` scrivere una riga di codice che separi l'elemento corrente dell'array (`name:price`) in due elementi distinti: uno contenente il nome e l'altro contenente il prezzo. Se non è chiaro come farlo, consultare l'articolo [Metodi utili per le stringhe](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods) oppure, ancora meglio, la sezione [Convertire tra stringhe e array](#convertire-tra-stringhe-e-array) di questo articolo.
5. Come parte della riga di codice precedente, sarà necessario convertire anche il prezzo da una stringa a un numero. Se non si ricorda come farlo, consultare il [primo articolo sulle stringhe](/it/docs/Learn_web_development/Core/Scripting/Strings#numbers_vs._strings).
6. Nella parte superiore del codice viene creata una variabile chiamata `total`, alla quale viene assegnato il valore `0`. All'interno del ciclo, sotto `// Part 4`, aggiungere una riga che sommi il prezzo dell'elemento corrente a quel totale a ogni iterazione del ciclo, in modo che alla fine del codice il totale corretto venga stampato sulla fattura. Potrebbe essere necessario un [operatore di assegnazione](/it/docs/Learn_web_development/Core/Scripting/Math#assignment_operators).
7. Modificare la riga successiva a `// Part 5` in modo che la variabile `itemText` sia uguale a "nome dell'elemento corrente — $prezzo dell'elemento corrente", ad esempio "Shoes — $23.99" in ogni caso, così che sulla fattura vengano stampate le informazioni corrette per ogni elemento. Si tratta di una semplice concatenazione di stringhe, che dovrebbe essere già nota dopo aver seguito il materiale didattico fino a questo punto.
8. Infine, sotto il commento `// Part 6`, aggiungere una `}` per indicare la fine del ciclo `for...of()`.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ in MDN Playground. Se ci si blocca completamente, è possibile visualizzare la soluzione sotto l'output live.

```html hidden live-sample___arrays-1
<h2>Live output</h2>

<div class="output">
  <ul></ul>

  <p></p>
</div>
```

```css hidden live-sample___arrays-1
.output {
  min-height: 100px;
}
```

```js live-sample___arrays-1
const list = document.querySelector(".output ul");
const totalBox = document.querySelector(".output p");
let total = 0;
list.textContent = "";
totalBox.textContent = "";
// Part 1
// "Underpants:6.99",
// "Socks:5.99",
// "T-shirt:14.99",
// "Trousers:31.99",
// "Shoes:23.99",

// Part 2

// Part 3

// Part 4

// Part 5
let itemText = 0;

const listItem = document.createElement("li");
listItem.textContent = itemText;
list.appendChild(listItem);

// Part 6

totalBox.textContent = `Total: $${total.toFixed(2)}`;
```

{{ EmbedLiveSample("arrays-1", "100%", 200) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
const list = document.querySelector(".output ul");
const totalBox = document.querySelector(".output p");
let total = 0;
list.textContent = "";
totalBox.textContent = "";

const products = [
  "Underpants:6.99",
  "Socks:5.99",
  "T-shirt:14.99",
  "Trousers:31.99",
  "Shoes:23.99",
];

for (const product of products) {
  const subArray = product.split(":");
  const name = subArray[0];
  const price = Number(subArray[1]);
  total += price;
  const itemText = `${name} — $${price}`;

  const listItem = document.createElement("li");
  listItem.textContent = itemText;
  list.appendChild(listItem);
}

totalBox.textContent = `Total: $${total.toFixed(2)}`;
```

</details>

## Memorizzare le 5 ricerche precedenti

Completiamo un altro esercizio, per continuare a fare pratica.

Un buon utilizzo dei metodi degli array come {{jsxref("Array.prototype.push()","push()")}} e {{jsxref("Array.prototype.pop()","pop()")}} è la gestione di un registro degli elementi attualmente attivi in una web app. In una scena animata, ad esempio, potrebbe esserci un array di oggetti che rappresentano la grafica di sfondo attualmente visualizzata, e si potrebbe voler visualizzarne soltanto 50 contemporaneamente per ragioni di prestazioni o per evitare confusione. Quando vengono creati nuovi oggetti e aggiunti all'array, quelli più vecchi possono essere eliminati dall'array per mantenere il numero desiderato.

In questo esempio mostreremo un utilizzo molto più semplice: viene fornito un sito di ricerca fittizio, con una casella di ricerca. L'idea è che, quando vengono inseriti termini nella casella di ricerca, i 5 termini di ricerca precedenti vengano visualizzati nell'elenco. Quando il numero di termini supera 5, l'ultimo termine inizia a essere eliminato ogni volta che un nuovo termine viene aggiunto in cima, quindi vengono sempre visualizzati i 5 termini precedenti.

> [!NOTE]
> In una vera app di ricerca, probabilmente sarebbe possibile fare clic sui termini di ricerca precedenti per tornare alle ricerche precedenti e verrebbero visualizzati risultati di ricerca reali. Per ora manteniamo l'esempio semplice.

Per completare l'esempio, occorre:

1. Fare clic su **"Play"** nel blocco di codice seguente per modificare l'esempio in MDN Playground.
2. Aggiungere una riga sotto il commento `// Part 1` che aggiunga il valore corrente inserito nell'input di ricerca all'inizio dell'array. Questo valore può essere recuperato usando `searchInput.value`.
3. Aggiungere una riga sotto il commento `// Part 2` che rimuova il valore attualmente alla fine dell'array.

In caso di errore, è possibile cancellare il lavoro usando il pulsante _Reset_ in MDN Playground. Se ci si blocca completamente, è possibile visualizzare la soluzione sotto l'output live.

```html hidden live-sample___arrays-2
<div class="output">
  <label for="search-box">Enter a search term: </label>
  <input id="search-box" type="search" />
  <button>Search</button>

  <ul></ul>
</div>
```

```css hidden live-sample___arrays-2
.output {
  margin: 1rem;
}
```

```js live-sample___arrays-2
const list = document.querySelector(".output ul");
const searchInput = document.querySelector(".output input");
const searchBtn = document.querySelector(".output button");

list.textContent = "";

const myHistory = [];
const MAX_HISTORY = 5;

searchBtn.addEventListener("click", () => {
  // we will only allow a term to be entered if the search input isn't empty
  if (searchInput.value !== "") {
    // Part 1

    // empty the list so that we don't display duplicate entries
    // the display is regenerated every time a search term is entered.
    list.textContent = "";

    // loop through the array, and display all the search terms in the list
    for (const itemText of myHistory) {
      const listItem = document.createElement("li");
      listItem.textContent = itemText;
      list.appendChild(listItem);
    }

    // If the array length is 5 or more, remove the oldest search term
    if (myHistory.length >= MAX_HISTORY) {
      // Part 2
    }

    // empty the search input and focus it, ready for the next term to be entered
    searchInput.value = "";
    searchInput.focus();
  }
});
```

{{ EmbedLiveSample("arrays-2", "100%", 200) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
const list = document.querySelector(".output ul");
const searchInput = document.querySelector(".output input");
const searchBtn = document.querySelector(".output button");

list.textContent = "";

const myHistory = [];
const MAX_HISTORY = 5;

searchBtn.addEventListener("click", () => {
  // we will only allow a term to be entered if the search input isn't empty
  if (searchInput.value !== "") {
    myHistory.unshift(searchInput.value);

    // empty the list so that we don't display duplicate entries
    // the display is regenerated every time a search term is entered.
    list.textContent = "";

    // loop through the array, and display all the search terms in the list
    for (const itemText of myHistory) {
      const listItem = document.createElement("li");
      listItem.textContent = itemText;
      list.appendChild(listItem);
    }

    // If the array length is 5 or more, remove the oldest search term
    if (myHistory.length >= MAX_HISTORY) {
      myHistory.pop();
    }

    // empty the search input and focus it, ready for the next term to be entered
    searchInput.value = "";
    searchInput.focus();
  }
});
```

</details>

## Riepilogo

Dopo aver letto questo articolo, sarà certamente evidente che gli array sono estremamente utili; compaiono ovunque in JavaScript, spesso in associazione con i cicli per eseguire la stessa operazione su ogni elemento di un array. I cicli saranno trattati più approfonditamente più avanti nel modulo.

Nel prossimo articolo verranno proposti alcuni test per verificare quanto bene sono state comprese e ricordate le informazioni fornite sugli array.

## Vedere anche

- {{jsxref("Array")}}
  - : La pagina di riferimento dell'oggetto `Array` fornisce una guida di riferimento dettagliata alle funzionalità trattate in questa pagina e a molte altre funzionalità di `Array`.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Strings", "Learn_web_development/Core/Scripting/Test_your_skills/Arrays", "Learn_web_development/Core/Scripting")}}
