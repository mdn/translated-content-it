---
title: "Metti alla prova le tue competenze: Variabili"
short-title: "Test: Variabili"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Variables
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting")}}

L'obiettivo di questo test di competenze è aiutare a valutare se è stato compreso l'articolo [Memorizzare le informazioni necessarie — Variabili](/it/docs/Learn_web_development/Core/Scripting/Variables).

> [!NOTE]
> Per ricevere assistenza, leggere la guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È inoltre possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Sfida interattiva

Prima di tutto, viene proposta una divertente sfida interattiva sulle variabili creata dal nostro [partner di apprendimento](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds), [Scrimba](https://scrimba.com/home).

Guardare lo scrim incorporato e completare l'attività sulla timeline (l'icona del piccolo fantasma) seguendo le istruzioni e modificando il codice. Al termine, è possibile riprendere la visione dello scrim per verificare come la soluzione dell'insegnante si confronta con la propria.

<mdn-scrim-inline url="https://scrimba.com/learn-javascript-c0v/~011" scrimtitle="Esercitazione sulle variabili" survey="true"></mdn-scrim-inline>

## Variabili 1

Per completare questa attività, aggiungere una nuova riga per correggere il valore memorizzato nella variabile `myName` esistente inserendo il proprio nome.

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___variables-1 live-sample___variables-2 live-sample___variables-1-finish live-sample___variables-2-finish
<section></section>
```

```css hidden live-sample___variables-1 live-sample___variables-2 live-sample___variables-1-finish live-sample___variables-2-finish
* {
  box-sizing: border-box;
}

p {
  color: purple;
  margin: 0.5em 0;
}
```

<!-- Codice specifico dell'esempio -->

Il punto di partenza dell'attività è simile a questo:

{{ EmbedLiveSample("variables-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___variables-1
let myName = "Paul";

// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para = document.createElement("p");
para.textContent = myName;
section.appendChild(para);
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("variables-1-finish", "100%", 60) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

myName = "Chris";

// Don't edit the code below here!
// ...
```

```js hidden live-sample___variables-1-finish
let myName = "Paul";

myName = "Chris";

const section = document.querySelector("section");
const para = document.createElement("p");
para.textContent = myName;
section.appendChild(para);
```

</details>

## Variabili 2

L'ultima attività per il momento: in questo caso viene fornito del codice esistente, che contiene due errori. Il pannello dei risultati dovrebbe mostrare il nome `Chris` e un'affermazione sull'età che Chris avrà tra 20 anni. Correggere il problema e l'output.

Il punto di partenza dell'attività è simile a questo (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("variables-2", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___variables-2
// Fix the following code

const myName = "Default";
myName = "Chris";

let myAge = "42";

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
para1.textContent = myName;
para2.textContent = `In 20 years, I will be ${myAge + 20}`;
section.appendChild(para1);
section.appendChild(para2);
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("variables-2-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per visualizzare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// Turn the const into a let, so the value can be changed
let myName = "Default";
myName = "Chris";

// myAge needs to have a number datatype
let myAge = 42;

// Don't edit the code below here!
// ...
```

```js hidden live-sample___variables-2-finish
let myName = "Default";
myName = "Chris";
let myAge = 42;

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
para1.textContent = myName;
para2.textContent = `In 20 years, I will be ${myAge + 20}`;
section.appendChild(para1);
section.appendChild(para2);
```

</details>

## Vedi anche

Consultare [Tempo di esercitazione - Parte 3: let e const](https://scrimba.com/learn-javascript-c0v/~059?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> di Scrimba: una sfida interattiva che propone più test relativi a `let` e `const`.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting")}}
