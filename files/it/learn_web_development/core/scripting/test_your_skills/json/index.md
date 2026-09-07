---
title: "Metti alla prova le tue competenze: JSON"
short-title: "Test: JSON"
slug: Learn_web_development/Core/Scripting/Test_your_skills/JSON
l10n:
  sourceCommit: b36d59a0df933597c7d3b55e363f7a59e30d3ba3
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Scripting/House_data_UI", "Learn_web_development/Core/Scripting")}}

Lo scopo di questo test delle competenze è aiutare a valutare se è stato compreso il nostro articolo su [come lavorare con JSON](/it/docs/Learn_web_development/Core/Scripting/JSON).

> [!NOTE]
> Per ottenere aiuto, consultare la nostra guida all'uso di [Metti alla prova le tue competenze](/it/docs/Learn_web_development#test_your_skills). È anche possibile contattarci utilizzando uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## JSON 1

L'unico compito in questo articolo riguarda l'accesso ai dati JSON e il loro utilizzo nella pagina. I dati JSON relativi ad alcune gatte madri e ai loro gattini sono disponibili in [sample.json](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/json/sample.json). Il JSON viene caricato nella pagina come stringa di testo e reso disponibile nel parametro `catString` della funzione `displayCatInfo()`.

Per completare il compito, riempire le parti mancanti della funzione `displayCatInfo()` per memorizzare:

- I nomi delle tre gatte madri, separati da virgole, nella variabile `motherInfo`.
- Il numero totale di gattini e quanti sono maschi e femmine nella variabile `kittenInfo`.

I valori di queste variabili vengono quindi stampati sullo schermo all'interno di paragrafi.

Alcuni suggerimenti/domande:

- I dati JSON sono forniti come testo all'interno della funzione `displayCatInfo()`. Sarà necessario analizzarli in JSON prima di poterne estrarre dei dati.
- Probabilmente sarà opportuno usare un ciclo esterno per scorrere le gatte e aggiungere i loro nomi alla stringa della variabile `motherInfo`, e un ciclo interno per scorrere tutti i gattini, sommare il totale dei gattini complessivi/maschi/femmine e aggiungere questi dettagli alla stringa della variabile `kittenInfo`.
- L'ultimo nome di gatto madre deve avere una "e" prima e un punto finale dopo. Come assicurarsi che questo funzioni indipendentemente dal numero di gatti nel JSON?
- Perché le righe `para1.textContent = motherInfo;` e `para2.textContent = kittenInfo;` si trovano all'interno della funzione `displayCatInfo()` e non alla fine dello script? Ciò ha a che fare con il codice asincrono.

Il punto di partenza del compito è simile a questo:

{{ EmbedLiveSample("json-1", "100%", 60) }}

Ecco il codice sottostante per questo punto di partenza:

```html hidden live-sample___json-1 live-sample___json-1-finish
<p class="one"></p>
<p class="two"></p>
```

```css hidden live-sample___json-1 live-sample___json-1-finish
p {
  color: purple;
  margin: 0.5em 0;
}

* {
  box-sizing: border-box;
}
```

```js live-sample___json-1
const para1 = document.querySelector(".one");
const para2 = document.querySelector(".two");
let motherInfo = "The mother cats are called ";
let kittenInfo;
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/tasks/json/sample.json";

fetch(requestURL)
  .then((response) => response.text())
  .then((text) => displayCatInfo(text));

// Don't edit the code above here!

function displayCatInfo(catString) {
  let total = 0;
  let male = 0;

  // Add your code here

  // Don't edit the code below here!

  para1.textContent = motherInfo;
  para2.textContent = kittenInfo;
}
```

L'output aggiornato dovrebbe apparire così:

{{ EmbedLiveSample("json-1-finish", "100%", 80) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe essere simile a questo:

```js
// ...
// Don't edit the code above here!

function displayCatInfo(catString) {
  let total = 0;
  let male = 0;

  const cats = JSON.parse(catString);

  for (let i = 0; i < cats.length; i++) {
    for (const kitten of cats[i].kittens) {
      total++;
      if (kitten.gender === "m") {
        male++;
      }
    }

    if (i < cats.length - 1) {
      motherInfo += `${cats[i].name}, `;
    } else {
      motherInfo += `and ${cats[i].name}.`;
    }
  }

  kittenInfo = `There are ${total} kittens in total, ${male} males and ${
    total - male
  } females.`;

  // Don't edit the code below here!

  para1.textContent = motherInfo;
  para2.textContent = kittenInfo;
}
```

```js hidden live-sample___json-1-finish
const para1 = document.querySelector(".one");
const para2 = document.querySelector(".two");
let motherInfo = "The mother cats are called ";
let kittenInfo;
const requestURL =
  "https://mdn.github.io/learning-area/javascript/oojs/tasks/json/sample.json";

fetch(requestURL)
  .then((response) => response.text())
  .then((text) => displayCatInfo(text));

function displayCatInfo(catString) {
  let total = 0;
  let male = 0;

  const cats = JSON.parse(catString);

  for (let i = 0; i < cats.length; i++) {
    for (const kitten of cats[i].kittens) {
      total++;
      if (kitten.gender === "m") {
        male++;
      }
    }

    if (i < cats.length - 1) {
      motherInfo += `${cats[i].name}, `;
    } else {
      motherInfo += `and ${cats[i].name}.`;
    }
  }

  kittenInfo = `There are ${total} kittens in total, ${male} males and ${
    total - male
  } females.`;

  para1.textContent = motherInfo;
  para2.textContent = kittenInfo;
}
```

</details>

{{PreviousMenuNext("Learn_web_development/Core/Scripting/JSON","Learn_web_development/Core/Scripting/House_data_UI", "Learn_web_development/Core/Scripting")}}
