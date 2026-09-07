---
title: "Metti alla prova le tue competenze: Funzioni"
short-title: "Test: Funzioni"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Functions
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Return_values","Learn_web_development/Core/Scripting/Events", "Learn_web_development/Core/Scripting")}}

Lo scopo di questo test sulle competenze è aiutare a valutare se sono stati compresi gli articoli [Funzioni — blocchi di codice riutilizzabili](/it/docs/Learn_web_development/Core/Scripting/Functions), [Crea la tua funzione](/it/docs/Learn_web_development/Core/Scripting/Build_your_own_function) e [Valori restituiti dalle funzioni](/it/docs/Learn_web_development/Core/Scripting/Return_values).

> [!NOTE]
> Per ottenere aiuto, leggere la Guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: utile da considerare

Alcune delle domande seguenti richiedono di scrivere del codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle, ad esempio creando nuovi elementi HTML, impostando i relativi contenuti di testo su specifici valori stringa e annidandoli all'interno di elementi esistenti nella pagina, il tutto tramite JavaScript.

Questo argomento non è ancora stato insegnato esplicitamente nel corso, ma sono già stati mostrati alcuni esempi che lo utilizzano e sarebbe opportuno svolgere qualche ricerca sulle API del DOM necessarie per rispondere con successo alle domande. Un buon punto di partenza è il tutorial [Introduzione allo scripting del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Sfida interattiva

Prima di tutto, viene proposta una divertente sfida interattiva sui valori restituiti dalle funzioni, creata dal nostro [partner didattico](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds), [Scrimba](https://scrimba.com/home).

Guardare lo scrim incorporato e completare l'attività sulla timeline (l'icona del piccolo fantasma) seguendo le istruzioni e modificando il codice. Al termine, è possibile riprendere la visione dello scrim per verificare come la soluzione dell'insegnante si confronta con la propria.

<mdn-scrim-inline url="https://scrimba.com/learn-javascript-c0v/~02h" scrimtitle="Restituzione di valori nelle funzioni" survey="true"></mdn-scrim-inline>

## Funzioni 1

Per completare il primo esercizio sulle funzioni:

1. Definire una funzione, `chooseName()`, che stampi un nome casuale dall'array fornito (`names`) nel paragrafo fornito (`para`).
2. Chiamare la funzione `chooseName()` una volta.

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___functions-1 live-sample___functions-3 live-sample___functions-4 live-sample___functions-1-finish
<p></p>
```

```css hidden live-sample___functions-1 live-sample___functions-3 live-sample___functions-4 live-sample___functions-1-finish
* {
  box-sizing: border-box;
}

p {
  color: purple;
  margin: 0.5em 0;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'esercizio è simile a questo (non viene ancora mostrato nulla):

{{ EmbedLiveSample("functions-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___functions-1
const names = [
  "Chris",
  "Li Kang",
  "Anne",
  "Francesca",
  "Mustafa",
  "Tina",
  "Bert",
  "Jada",
];
const para = document.querySelector("p");

// Don't edit the code above here!

// Add your code here
```

Il codice aggiornato dovrebbe produrre un nome casuale:

{{ EmbedLiveSample("functions-1-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
// ...
// Don't edit the code above here!

function chooseName() {
  const randomNumber = Math.floor(Math.random() * names.length);
  const choice = names[randomNumber];
  para.textContent = choice;
}

chooseName();
```

```js hidden live-sample___functions-1-finish
const names = [
  "Chris",
  "Li Kang",
  "Anne",
  "Francesca",
  "Mustafa",
  "Tina",
  "Bert",
  "Jada",
];
const para = document.querySelector("p");

function chooseName() {
  const randomNumber = Math.floor(Math.random() * names.length);
  const choice = names[randomNumber];
  para.textContent = choice;
}

chooseName();
```

</details>

## Funzioni 2

Questo esercizio richiede di creare una funzione che disegni un rettangolo sull'elemento `<canvas>` fornito (variabile di riferimento `canvas`, contesto disponibile in `ctx`), in base alle cinque variabili di input fornite:

- `x` — la coordinata x del rettangolo.
- `y` — la coordinata y del rettangolo.
- `width` — la larghezza del rettangolo.
- `height` — l'altezza del rettangolo.
- `color` — il colore del rettangolo.

Il punto di partenza dell'esercizio è simile a questo (non viene ancora mostrato nulla nel `<canvas>`):

{{ EmbedLiveSample("functions-2", "100%", 180) }}

Ecco il codice sottostante per questo punto di partenza:

```html hidden live-sample___functions-2 live-sample___functions-2-finish
<canvas width="240" height="160"></canvas>
```

```css hidden live-sample___functions-2 live-sample___functions-2-finish
canvas {
  border: 1px solid black;
}
```

```js live-sample___functions-2
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
const x = 50;
const y = 60;
const width = 100;
const height = 75;
const color = "blue";

// Don't edit the code above here!

// Add your code here
```

L'output aggiornato dovrebbe avere un aspetto simile a questo:

{{ EmbedLiveSample("functions-2-finish", "100%", 180) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
// ...
// Don't edit the code above here!

function drawSquare(x, y, width, height, color) {
  ctx.fillStyle = "white";
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = color;
  ctx.fillRect(x, y, width, height);
}

drawSquare(x, y, width, height, color);
```

```js hidden live-sample___functions-2-finish
const canvas = document.querySelector("canvas");
const ctx = canvas.getContext("2d");
const x = 50;
const y = 60;
const width = 100;
const height = 75;
const color = "blue";

function drawSquare(x, y, width, height, color) {
  ctx.fillStyle = "white";
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  ctx.fillStyle = color;
  ctx.fillRect(x, y, width, height);
}

drawSquare(x, y, width, height, color);
```

</details>

## Funzioni 3

In questo esercizio, si torna al problema proposto nell'Esercizio 1, con l'obiettivo di apportare tre miglioramenti.

Per completare l'esercizio:

1. Rifattorizzare il codice che genera il numero casuale in una funzione separata chiamata `random()`, che accetta come parametri due limiti generici entro cui deve trovarsi il numero casuale e restituisce il risultato.
2. Aggiornare la funzione `chooseName()` affinché utilizzi la funzione per i numeri casuali, accetti come parametro l'array da cui scegliere (rendendola più flessibile) e restituisca il risultato.
3. Stampare il risultato restituito nel `textContent` del paragrafo (`para`).

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("functions-3", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___functions-3
const names = [
  "Chris",
  "Li Kang",
  "Anne",
  "Francesca",
  "Mustafa",
  "Tina",
  "Bert",
  "Jada",
];
const para = document.querySelector("p");

// Don't edit the code above here!

// Update the code below here

function chooseName() {
  const randomNumber = Math.floor(Math.random() * names.length);
  const choice = names[randomNumber];
  para.textContent = choice;
}

chooseName();
```

Non viene fornito il contenuto completato per questo esercizio, poiché appare uguale al punto di partenza. Il codice è stato semplicemente rifattorizzato.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
// ...
// Don't edit the code above here!

function random(min, max) {
  const num = Math.floor(Math.random() * (max - min)) + min;
  return num;
}

function chooseName(array) {
  const choice = array[random(0, array.length)];
  return choice;
}

para.textContent = chooseName(names);
```

```js hidden
const names = [
  "Chris",
  "Li Kang",
  "Anne",
  "Francesca",
  "Mustafa",
  "Tina",
  "Bert",
  "Jada",
];
const para = document.querySelector("p");

function random(min, max) {
  const num = Math.floor(Math.random() * (max - min)) + min;
  return num;
}

function chooseName(array) {
  const choice = array[random(0, array.length)];
  return choice;
}

para.textContent = chooseName(names);
```

</details>

## Funzioni 4

In questo esercizio, è disponibile un array di nomi e viene usato {{jsxref("Array.filter()")}} per ottenere un array contenente solo i nomi più corti di 5 caratteri. Attualmente al filtro viene passata una funzione denominata `isShort()`. Questa controlla la lunghezza del nome, restituendo `true` se il nome è lungo meno di 5 caratteri e `false` altrimenti.

Per completare l'esercizio, aggiornare il codice affinché la funzionalità all'interno di `isShort()` venga invece inclusa direttamente nella chiamata a `filter()` come arrow function. Provare a renderla il più compatta possibile.

Il punto di partenza dell'esercizio è simile a questo:

{{ EmbedLiveSample("functions-4", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___functions-4
const names = [
  "Chris",
  "Li Kang",
  "Anne",
  "Francesca",
  "Mustafa",
  "Tina",
  "Bert",
  "Jada",
];
const para = document.querySelector("p");

// Don't edit the code above here!

// Update the code below here

function isShort(name) {
  return name.length < 5;
}

const shortNames = names.filter(isShort);
para.textContent = shortNames;
```

Non viene fornito il contenuto completato per questo esercizio, poiché appare uguale al punto di partenza. Il codice è stato semplicemente rifattorizzato.

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile a questo:

```js
// ...
// Don't edit the code above here!

// Update the code below here

const shortNames = names.filter((name) => name.length < 5);
para.textContent = shortNames;
```

```js hidden
const names = [
  "Chris",
  "Li Kang",
  "Anne",
  "Francesca",
  "Mustafa",
  "Tina",
  "Bert",
  "Jada",
];
const para = document.querySelector("p");

const shortNames = names.filter((name) => name.length < 5);
para.textContent = shortNames;
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Return_values","Learn_web_development/Core/Scripting/Events", "Learn_web_development/Core/Scripting")}}
