---
title: "Metti alla prova le tue competenze: stringhe"
short-title: "Test: stringhe"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Strings
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting")}}

L'obiettivo di questo test delle competenze è aiutare a valutare se sono stati compresi gli articoli [Gestire il testo — stringhe in JavaScript](/it/docs/Learn_web_development/Core/Scripting/Strings) e [Metodi utili per le stringhe](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods).

> [!NOTE]
> Per ottenere aiuto, leggere la guida all'uso [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Stringhe 1

Nel primo esercizio sulle stringhe, si inizia in piccolo. È già presente metà di una citazione famosa all'interno di una variabile chiamata `quoteStart` e occorre completarla.

Per completare l'esercizio:

1. Cercare l'altra metà della citazione e aggiungerla all'esempio all'interno di una variabile chiamata `quoteEnd`.
2. Concatenare le due stringhe per creare un'unica stringa contenente la citazione completa. Salvare il risultato all'interno di una variabile chiamata `finalQuote`.
3. A questo punto verrà visualizzato un errore. È possibile correggere il problema con `quoteStart`, in modo che la citazione completa venga visualizzata correttamente?

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___strings-1 live-sample___strings-2 live-sample___strings-3 live-sample___strings-4 live-sample___strings-1-finish live-sample___strings-2-finish live-sample___strings-3-finish live-sample___strings-4-finish
<section></section>
```

```css hidden live-sample___strings-1 live-sample___strings-2 live-sample___strings-3 live-sample___strings-4 live-sample___strings-1-finish live-sample___strings-2-finish live-sample___strings-3-finish live-sample___strings-4-finish
* {
  box-sizing: border-box;
}

p {
  color: purple;
  margin: 0.5em 0;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'esercizio è il seguente (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("strings-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js-nolint live-sample___strings-1
const quoteStart = 'Don't judge each day by the harvest you reap ';

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = finalQuote;
section.appendChild(para1);
```

L'output aggiornato dovrebbe essere simile al seguente:

{{ EmbedLiveSample("strings-1-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js-nolint
// You need to escape the quote
const quoteStart = 'Don\'t judge each day by the harvest you reap ';

const quoteEnd = "but by the seeds that you plant.";

const finalQuote = `${quoteStart}${quoteEnd}`;

// Don't edit the code below here!
// ...
```

```js hidden live-sample___strings-1-finish
const quoteStart = "Don't judge each day by the harvest you reap ";

const quoteEnd = "but by the seeds that you plant.";

const finalQuote = `${quoteStart}${quoteEnd}`;

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = finalQuote;
section.appendChild(para1);
```

</details>

## Stringhe 2

In questo esercizio vengono fornite due variabili, `quote` e `substring`, che contengono due stringhe.

Per completare l'esercizio:

1. Recuperare la lunghezza della citazione e memorizzarla in una variabile chiamata `quoteLength`.
2. Trovare la posizione dell'indice in cui `substring` appare in `quote` e memorizzare tale valore in una variabile chiamata `index`.
3. Usare una combinazione delle variabili disponibili e delle proprietà/metodi delle stringhe disponibili per ridurre la citazione originale a "I do not like green eggs and ham.", quindi memorizzarla in una variabile chiamata `revisedQuote`.

Il punto di partenza dell'esercizio è il seguente (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("strings-2", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___strings-2
const quote = "I do not like green eggs and ham. I do not like them, Sam-I-Am.";
const substring = "green eggs and ham";

// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
section.innerHTML = " ";
const para1 = document.createElement("p");
para1.textContent = `The quote is ${quoteLength} characters long.`;
const para2 = document.createElement("p");
para2.textContent = revisedQuote;
section.appendChild(para1);
section.appendChild(para2);
```

L'output aggiornato dovrebbe essere simile al seguente:

{{ EmbedLiveSample("strings-2-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

const quoteLength = quote.length;
const index = quote.indexOf(substring);
const revisedQuote = quote.slice(0, index + substring.length + 1);

// Don't edit the code below here!
// ...
```

```js hidden live-sample___strings-2-finish
const quote = "I do not like green eggs and ham. I do not like them, Sam-I-Am.";
const substring = "green eggs and ham";

const quoteLength = quote.length;
const index = quote.indexOf(substring);
const revisedQuote = quote.slice(0, index + substring.length + 1);

const section = document.querySelector("section");
section.innerHTML = " ";
const para1 = document.createElement("p");
para1.textContent = `The quote is ${quoteLength} characters long.`;
const para2 = document.createElement("p");
para2.textContent = revisedQuote;
section.appendChild(para1);
section.appendChild(para2);
```

</details>

## Stringhe 3

Nel prossimo esercizio sulle stringhe, viene fornita la stessa citazione ottenuta nell'esercizio precedente, ma è in qualche modo danneggiata. Occorre correggerla e aggiornarla.

Per completare l'esercizio:

1. Modificare le maiuscole/minuscole in modo da ottenere una corretta frase con iniziale maiuscola (tutto in minuscolo, tranne la prima lettera maiuscola). Memorizzare la nuova citazione in una variabile chiamata `fixedQuote`.
2. In `fixedQuote`, sostituire "green eggs and ham" con un altro cibo che non piace affatto.
3. Rimane un'ultima piccola correzione da effettuare: aggiungere un punto alla fine della citazione e salvare la versione finale in una variabile chiamata `finalQuote`.

Il punto di partenza dell'esercizio è il seguente (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("strings-3", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___strings-3
const quote = "I dO nOT lIke gREen eGgS anD HAM";

// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = finalQuote;
section.appendChild(para1);
```

L'output aggiornato dovrebbe essere simile al seguente:

{{ EmbedLiveSample("strings-3-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

let fixedQuote = quote.toLowerCase();
const firstLetter = fixedQuote.slice(0, 1);
fixedQuote = fixedQuote.replace(firstLetter, firstLetter.toUpperCase());
fixedQuote = fixedQuote.replace("green eggs and ham", "pickled onions");
const finalQuote = `${fixedQuote}.`;

// Don't edit the code below here!
// ...
```

```js hidden live-sample___strings-3-finish
const quote = "I dO nOT lIke gREen eGgS anD HAM";

let fixedQuote = quote.toLowerCase();
const firstLetter = fixedQuote.slice(0, 1);
fixedQuote = fixedQuote.replace(firstLetter, firstLetter.toUpperCase());
fixedQuote = fixedQuote.replace("green eggs and ham", "pickled onions");
const finalQuote = `${fixedQuote}.`;

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = finalQuote;
section.appendChild(para1);
```

</details>

## Stringhe 4

Nell'ultimo esercizio sulle stringhe, vengono forniti il nome di un teorema, due valori numerici e una stringa incompleta (le parti da aggiungere sono contrassegnate da asterischi (`*`)). Occorre modificare il valore della stringa.

Per completare l'esercizio:

1. Modificare la stringa da un normale letterale stringa a un template literal.
2. Sostituire i quattro asterischi con quattro espressioni incorporate in un template literal. Dovrebbero essere:
   1. Il nome del teorema.
   2. I due valori numerici disponibili.
   3. La lunghezza dell'ipotenusa di un triangolo rettangolo, supponendo che le lunghezze degli altri due lati corrispondano ai due valori disponibili. Sarà necessario cercare come calcolarla a partire dai dati disponibili. Eseguire il calcolo all'interno del segnaposto.

Il punto di partenza dell'esercizio è il seguente:

{{ EmbedLiveSample("strings-4", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___strings-4
const theorem = "Pythagorean theorem";

const a = 5;
const b = 8;

// Don't edit the code above here!

// Edit the string literal
const myString =
  "Using *, we can work out that if the two shortest sides of a right-angled triangle have lengths of * and *, the length of the hypotenuse is *.";

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = myString;
section.appendChild(para1);
```

L'output aggiornato dovrebbe essere simile al seguente:

{{ EmbedLiveSample("strings-4-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

const myString = `Using ${theorem}, we can work out that if the two shortest sides of a right-angled triangle have lengths of ${a} and ${b},
  the length of the hypotenuse is ${Math.sqrt(a ** 2 + b ** 2)}.`;

// Don't edit the code below here!
// ...
```

```js hidden live-sample___strings-4-finish
const theorem = "Pythagorean theorem";

const a = 5;
const b = 8;

const myString = `Using ${theorem}, we can work out that if the two shortest sides of a right-angled triangle have lengths of ${a} and ${b},
  the length of the hypotenuse is ${Math.sqrt(a ** 2 + b ** 2)}.`;

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = myString;
section.appendChild(para1);
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting")}}
