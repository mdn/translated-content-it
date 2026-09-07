---
title: "Metti alla prova le tue competenze: Array"
short-title: "Test: Array"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Arrays
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting")}}

L'obiettivo di questo test di competenze è aiutare a valutare se è stato compreso il nostro articolo sugli [array](/it/docs/Learn_web_development/Core/Scripting/Arrays).

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Sfida interattiva

Prima di tutto, viene proposta una divertente sfida interattiva sugli array creata dal nostro [partner di apprendimento](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds), [Scrimba](https://scrimba.com/home).

Guardare lo scrim incorporato e completare l'attività sulla timeline (l'icona del piccolo fantasma) seguendo le istruzioni e modificando il codice. Al termine, è possibile riprendere la visione dello scrim per verificare come la soluzione dell'insegnante si confronta con la propria.

<mdn-scrim-inline url="https://scrimba.com/learn-javascript-c0v/~05e" scrimtitle="Renderizzare immagini da un array" survey="true"></mdn-scrim-inline>

> [!NOTE]
> Questa attività è piuttosto ambiziosa, poiché si basa su funzionalità JavaScript che non sono ancora state trattate esplicitamente durante il corso. Provare al meglio delle proprie possibilità e cercare online informazioni su tutto ciò di cui non si è sicuri.

## Array 1

Questa attività fornisce un po' di pratica di base con gli array:

1. Creare un array di tre elementi e memorizzarlo in una variabile chiamata `myArray`. Gli elementi possono essere qualsiasi cosa si desideri: perché non i cibi o i gruppi musicali preferiti?
2. Successivamente, modificare i primi due elementi dell'array utilizzando la notazione tra parentesi quadre e l'assegnazione.
3. Infine, aggiungere un nuovo elemento all'inizio dell'array.

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___arrays-1 live-sample___arrays-2 live-sample___arrays-3 live-sample___arrays-4 live-sample___arrays-1-finish live-sample___arrays-2-finish live-sample___arrays-3-finish live-sample___arrays-4-finish
<section></section>
```

```css hidden live-sample___arrays-1 live-sample___arrays-2 live-sample___arrays-3 live-sample___arrays-4 live-sample___arrays-1-finish live-sample___arrays-2-finish live-sample___arrays-3-finish live-sample___arrays-4-finish
* {
  box-sizing: border-box;
}

p {
  color: purple;
  margin: 0.5em 0;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'attività è il seguente (non viene ancora mostrato nulla):

{{ EmbedLiveSample("arrays-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___arrays-1
// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = `Array: ${myArray}`;
section.appendChild(para1);
```

L'output aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample("arrays-1-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js
const myArray = ["cats", "dogs", "chickens"];

myArray[0] = "horses";
myArray[1] = "pigs";

myArray.unshift("crocodiles");

// Don't edit the code below here!
// ...
```

```js hidden live-sample___arrays-1-finish
const myArray = ["cats", "dogs", "chickens"];

myArray[0] = "horses";
myArray[1] = "pigs";

myArray.unshift("crocodiles");

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = `Array: ${myArray}`;
section.appendChild(para1);
```

</details>

## Array 2

Passiamo ora a un'altra attività. Qui viene fornita una stringa con cui lavorare.

Per completare l'attività:

1. Convertire la stringa in un array, rimuovendo nel processo i caratteri `+`. Salvare il risultato in una variabile chiamata `myArray`.
2. Memorizzare la lunghezza dell'array in una variabile chiamata `arrayLength`.
3. Memorizzare l'ultimo elemento dell'array in una variabile chiamata `lastItem`.

Il punto di partenza dell'attività è il seguente (non viene ancora mostrato nulla):

{{ EmbedLiveSample("arrays-2", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___arrays-2
const myString = "Ryu+Ken+Chun-Li+Cammy+Guile+Sakura+Sagat+Juri";

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = `Array: ${myArray}`;
const para2 = document.createElement("p");
para2.textContent = `The length of the array is ${arrayLength}.`;
const para3 = document.createElement("p");
para3.textContent = `The last item in the array is "${lastItem}".`;
section.appendChild(para1);
section.appendChild(para2);
section.appendChild(para3);
```

L'output aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample("arrays-2-finish", "100%", 100) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js
const myString = "Ryu+Ken+Chun-Li+Cammy+Guile+Sakura+Sagat+Juri";

let myArray = myString.split("+");

let arrayLength = myArray.length;

let lastItem = myArray[arrayLength - 1];

// Don't edit the code below here!
// ...
```

```js hidden live-sample___arrays-2-finish
const myString = "Ryu+Ken+Chun-Li+Cammy+Guile+Sakura+Sagat+Juri";
let myArray = myString.split("+");
let arrayLength = myArray.length;
let lastItem = myArray[arrayLength - 1];

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = `Array: ${myArray}`;
const para2 = document.createElement("p");
para2.textContent = `The length of the array is ${arrayLength}.`;
const para3 = document.createElement("p");
para3.textContent = `The last item in the array is "${lastItem}".`;
section.appendChild(para1);
section.appendChild(para2);
section.appendChild(para3);
```

</details>

## Array 3

Per questa attività sugli array, viene fornito un array iniziale e si lavorerà in una direzione in qualche modo opposta. È necessario:

1. Rimuovere l'ultimo elemento dell'array.
2. Aggiungere due nuovi nomi alla fine dell'array.
3. Iterare su ciascun elemento dell'array e aggiungere il relativo numero di indice dopo il nome tra parentesi, ad esempio `Ryu (0)`. Si noti che questa operazione non viene insegnata nell'articolo sugli array, quindi sarà necessario fare qualche ricerca.
4. Infine, unire gli elementi dell'array in un'unica stringa chiamata `myString`, usando `"-"` come separatore.

Il punto di partenza dell'attività è il seguente (non viene ancora mostrato nulla):

{{ EmbedLiveSample("arrays-3", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___arrays-3
const myArray = [
  "Ryu",
  "Ken",
  "Chun-Li",
  "Cammy",
  "Guile",
  "Sakura",
  "Sagat",
  "Juri",
];

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = myString;
section.appendChild(para1);
```

L'output aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample("arrays-3-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js
const myArray = [
  "Ryu",
  "Ken",
  "Chun-Li",
  "Cammy",
  "Guile",
  "Sakura",
  "Sagat",
  "Juri",
];

myArray.pop();

myArray.push("Zangief");
myArray.push("Ibuki");

myArray.forEach((element, index) => {
  const newElement = `${element} (${index})`;
  myArray[index] = newElement;
});

const myString = myArray.join(" - ");

// Don't edit the code below here!
// ...
```

```js hidden live-sample___arrays-3-finish
const myArray = [
  "Ryu",
  "Ken",
  "Chun-Li",
  "Cammy",
  "Guile",
  "Sakura",
  "Sagat",
  "Juri",
];

myArray.pop();

myArray.push("Zangief");
myArray.push("Ibuki");

myArray.forEach((element, index) => {
  const newElement = `${element} (${index})`;
  myArray[index] = newElement;
});

const myString = myArray.join(" - ");

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = myString;
section.appendChild(para1);
```

</details>

## Array 4

Per questa attività sugli array, viene fornito un array iniziale che elenca i nomi di alcuni uccelli.

Per completare l'attività:

1. Trovare l'indice dell'elemento `"Eagles"` e usarlo per rimuovere l'elemento `"Eagles"`.
2. Creare un nuovo array a partire da questo, chiamato `eBirds`, che contenga solo gli uccelli dell'array originale i cui nomi iniziano con la lettera "E". Si noti che {{jsxref("String.prototype.startsWith()", "startsWith()")}} è un ottimo modo per verificare se una stringa inizia con un determinato carattere.

Il punto di partenza dell'attività è il seguente (non viene ancora mostrato nulla):

{{ EmbedLiveSample("arrays-4", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___arrays-4
const birds = ["Parrots", "Falcons", "Eagles", "Emus", "Caracaras", "Egrets"];

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = eBirds;
section.appendChild(para1);
```

L'output aggiornato dovrebbe avere questo aspetto:

{{ EmbedLiveSample("arrays-4-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js
const birds = ["Parrots", "Falcons", "Eagles", "Emus", "Caracaras", "Egrets"];

const eaglesIndex = birds.indexOf("Eagles");
birds.splice(eaglesIndex, 1);

function startsWithE(bird) {
  return bird.startsWith("E");
}
const eBirds = birds.filter(startsWithE);

// Don't edit the code below here!
// ...
```

```js hidden live-sample___arrays-4-finish
const birds = ["Parrots", "Falcons", "Eagles", "Emus", "Caracaras", "Egrets"];

const eaglesIndex = birds.indexOf("Eagles");
birds.splice(eaglesIndex, 1);

function startsWithE(bird) {
  return bird.startsWith("E");
}
const eBirds = birds.filter(startsWithE);

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = eBirds;
section.appendChild(para1);
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting/Silly_story_generator", "Learn_web_development/Core/Scripting")}}
