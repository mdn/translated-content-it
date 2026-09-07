---
title: "Metti alla prova le tue competenze: matematica"
short-title: "Test: matematica"
slug: Learn_web_development/Core/Scripting/Test_your_skills/Math
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}

Lo scopo dei test in questa pagina è aiutare a valutare se l'articolo [Matematica di base in JavaScript — numeri e operatori](/it/docs/Learn_web_development/Core/Scripting/Math) è stato compreso.

> [!NOTE]
> Per ottenere aiuto, leggere la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Matematica 1

Iniziamo testando la conoscenza degli operatori matematici di base.
Verranno creati quattro valori numerici, ne verranno sommati due, ne verrà sottratto uno da un altro e quindi i risultati verranno moltiplicati.
Infine, verrà scritto un test per dimostrare che questo valore è un numero pari.

Per completare l'attività:

1. Creare quattro variabili che contengano numeri. Assegnare alle variabili nomi appropriati.
2. Sommare le prime due variabili e memorizzare il risultato in un'altra variabile.
3. Sottrarre la quarta variabile dalla terza e memorizzare il risultato in un'altra variabile.
4. Moltiplicare i risultati dei passaggi **2** e **3** e memorizzare il risultato in una variabile chiamata `finalResult`.
5. Verificare se `finalResult` è un numero pari usando uno degli [operatori aritmetici](/it/docs/Learn_web_development/Core/Scripting/Math#arithmetic_operators). Memorizzare il risultato (`0` per pari, `1` per dispari) in una variabile chiamata `evenOddResult`.

Per superare questo test, `finalResult` dovrebbe avere il valore `48` e `evenOddResult` dovrebbe avere il valore `0`.

<!-- Codice condiviso tra gli esempi -->

```html hidden live-sample___math-1 live-sample___math-2 live-sample___math-3 live-sample___math-1-finish live-sample___math-2-finish live-sample___math-3-finish
<section></section>
```

```css hidden live-sample___math-1 live-sample___math-2 live-sample___math-3 live-sample___math-1-finish live-sample___math-2-finish live-sample___math-3-finish
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

{{ EmbedLiveSample("math-1", "100%", 80) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___math-1
let finalResult;
let evenOddResult;

// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
const finalResultCheck =
  finalResult === 48 ? `Yes, well done!` : `No, it is ${finalResult}`;
para1.textContent = `Is the finalResult 48? ${finalResultCheck}`;
const para2 = document.createElement("p");
const evenOddResultCheck =
  evenOddResult === 0
    ? "The final result is even!"
    : "The final result is odd. Hrm.";
para2.textContent = evenOddResultCheck;
section.appendChild(para1);
section.appendChild(para2);
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("math-1-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

const number1 = 4;
const number2 = 8;
const number3 = 12;
const number4 = 8;

const additionResult = number1 + number2;
const subtractionResult = number3 - number4;

finalResult = additionResult * subtractionResult;

evenOddResult = finalResult % 2;

// Don't edit the code below here!
// ...
```

```js hidden live-sample___math-1-finish
let finalResult;
let evenOddResult;

const number1 = 4;
const number2 = 8;
const number3 = 12;
const number4 = 8;

const additionResult = number1 + number2;
const subtractionResult = number3 - number4;

finalResult = additionResult * subtractionResult;

evenOddResult = finalResult % 2;

const section = document.querySelector("section");
const para1 = document.createElement("p");
const finalResultCheck =
  finalResult === 48 ? `Yes, well done!` : `No, it is ${finalResult}`;
para1.textContent = `Is the finalResult 48? ${finalResultCheck}`;
const para2 = document.createElement("p");
const evenOddResultCheck =
  evenOddResult === 0
    ? "The final result is even!"
    : "The final result is odd. Hrm.";
para2.textContent = evenOddResultCheck;
section.appendChild(para1);
section.appendChild(para2);
```

</details>

## Matematica 2

Nella seconda attività vengono forniti due calcoli con i risultati memorizzati nelle variabili `result` e `result2`.
È necessario prendere i calcoli, moltiplicarli e formattare il risultato con due cifre decimali.

Per completare l'attività:

1. Moltiplicare `result` e `result2` e assegnare nuovamente il risultato a `result` (usare l'abbreviazione di assegnazione).
2. Formattare `result` in modo che abbia due cifre decimali e memorizzarlo in una variabile chiamata `finalResult`.
3. Verificare il tipo di dati di `finalResult` usando `typeof`. Se è una `string`, convertirla nel tipo `number` e memorizzare il risultato in una variabile chiamata `finalNumber`.

Per superare questo test, `finalNumber` dovrebbe avere come risultato `4633.33`. Potrebbe essere necessario considerare la precedenza degli operatori e aggiungere o modificare alcune parentesi nelle espressioni di input per ottenere l'output corretto.

Il punto di partenza dell'attività è simile a questo (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("math-2", "100%", 80) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___math-2
// Final result should be 4633.33

let result = 7 + 13 / 9 + 7;
let result2 = (100 / 2) * 6;

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = `Your finalResult is ${finalResult}`;
const para2 = document.createElement("p");
const finalNumberCheck =
  isNaN(finalNumber) === false
    ? "finalNumber is a number type. Well done!"
    : `Oops! finalNumber is not a number.`;
para2.textContent = finalNumberCheck;
section.appendChild(para1);
section.appendChild(para2);
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("math-2-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js-nolint
// Final result should be 4633.33

let result = (7 + 13 / 9) + 7;
let result2 = 100 / 2 * 6;

result *= result2;

const finalResult = result.toFixed(2);

const finalNumber = Number(finalResult);

// Don't edit the code below here!
// ...
```

```js hidden live-sample___math-2-finish
let result = 7 + 13 / 9 + 7;
let result2 = (100 / 2) * 6;

result *= result2;
const finalResult = result.toFixed(2);
const finalNumber = Number(finalResult);

const section = document.querySelector("section");
const para1 = document.createElement("p");
para1.textContent = `Your finalResult is ${finalResult}`;
const para2 = document.createElement("p");
const finalNumberCheck =
  isNaN(finalNumber) === false
    ? "finalNumber is a number type. Well done!"
    : `Oops! finalNumber is not a number.`;
para2.textContent = finalNumberCheck;
section.appendChild(para1);
section.appendChild(para2);
```

</details>

## Matematica 3

Nell'attività finale di questo articolo, verranno scritti alcuni test.

Per completare l'attività:

1. Sono presenti tre gruppi, ciascuno composto da un'affermazione e due variabili. Per ciascun gruppo, scrivere un test che dimostri o confuti l'affermazione formulata.
2. Memorizzare i risultati di questi test nelle variabili chiamate rispettivamente `weightComparison`, `heightComparison` e `pwdMatch`.

Il punto di partenza dell'attività è simile a questo (non viene ancora visualizzato nulla):

{{ EmbedLiveSample("math-3", "100%", 80) }}

Ecco il codice sottostante per questo punto di partenza:

```js live-sample___math-3
// Statement 1: The elephant weighs less than the mouse
const eleWeight = 1000;
const mouseWeight = 2;
// Statement 2: The Ostrich is taller than the duck
const ostrichHeight = 2;
const duckHeight = 0.3;
// Statement 3: The two passwords match
const pwd1 = "stromboli";
const pwd2 = "stROmBoLi";

// Don't edit the code above here!

// Add your code here

// Don't edit the code below here!

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
const para3 = document.createElement("p");
const weightTest = weightComparison
  ? "True — elephants do weigh less than mice!?"
  : "False — of course an elephant is heavier than a mouse!";
const heightTest = heightComparison
  ? "True — an ostrich is indeed taller than a duck!"
  : "False — apparently a duck is taller than an ostrich!?";
const pwdTest = pwdMatch
  ? "True — the passwords match."
  : "False — the passwords do not match; please check them";
para1.textContent = weightTest;
section.appendChild(para1);
para2.textContent = heightTest;
section.appendChild(para2);
para3.textContent = pwdTest;
section.appendChild(para3);
```

L'output aggiornato dovrebbe essere simile a questo:

{{ EmbedLiveSample("math-3-finish", "100%", 100) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js-nolint
// ...
// Don't edit the code above here!

const weightComparison = eleWeight < mouseWeight;
const heightComparison = ostrichHeight > duckHeight;
const pwdMatch = pwd1 === pwd2;

// Don't edit the code below here!
// ...
```

```js hidden live-sample___math-3-finish
// Statement 1: The elephant weighs less than the mouse
const eleWeight = 1000;
const mouseWeight = 2;
// Statement 2: The Ostrich is taller than the duck
const ostrichHeight = 2;
const duckHeight = 0.3;
// Statement 3: The two passwords match
const pwd1 = "stromboli";
const pwd2 = "stROmBoLi";

const weightComparison = eleWeight < mouseWeight;
const heightComparison = ostrichHeight > duckHeight;
const pwdMatch = pwd1 === pwd2;

const section = document.querySelector("section");
const para1 = document.createElement("p");
const para2 = document.createElement("p");
const para3 = document.createElement("p");
const weightTest = weightComparison
  ? "True — elephants do weigh less than mice!?"
  : "False — of course an elephant is heavier than a mouse!";
const heightTest = heightComparison
  ? "True — an ostrich is indeed taller than a duck!"
  : "False — apparently a duck is taller than an ostrich!?";
const pwdTest = pwdMatch
  ? "True — the passwords match."
  : "False — the passwords do not match; please check them";
para1.textContent = weightTest;
section.appendChild(para1);
para2.textContent = heightTest;
section.appendChild(para2);
para3.textContent = pwdTest;
section.appendChild(para3);
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}
