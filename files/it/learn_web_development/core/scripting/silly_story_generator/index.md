---
title: "Sfida: generatore di storie buffe"
short-title: "Sfida: generatore di storie"
slug: Learn_web_development/Core/Scripting/Silly_story_generator
l10n:
  sourceCommit: 7ff752fba26e0bb950998bb5476157ff96c7d314
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Arrays", "Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting")}}

In questa sfida, il compito è prendere alcune delle conoscenze acquisite finora in questo modulo e applicarle alla creazione di una divertente app che genera storie buffe casuali. Durante il percorso verranno verificate le conoscenze su variabili, matematica, stringhe e array. Buon divertimento!

## Punto di partenza

Per iniziare, fare clic sul pulsante **Play** in uno dei pannelli di codice seguenti per aprire l'esempio fornito nel MDN Playground. Quindi seguire le istruzioni nella sezione [Descrizione del progetto](#descrizione_del_progetto) per completare la funzionalità JavaScript.

```html live-sample___silly-story-start live-sample___silly-story-finish
<div>
  <label for="custom-name">Enter custom name:</label>
  <input id="custom-name" type="text" placeholder="" />
</div>
<fieldset>
  <legend>Choose locale:</legend>
  <label for="us">US</label
  ><input id="us" type="radio" name="uk-us" value="us" checked />
  <label for="uk">UK</label
  ><input id="uk" type="radio" name="uk-us" value="uk" />
</fieldset>
<div>
  <button class="generate">Generate random story</button>
</div>
<!-- Thanks a lot to Willy Aguirre for his help with the code for this assessment -->
<p class="story"></p>
```

```css hidden live-sample___silly-story-start live-sample___silly-story-finish
body {
  font: 1.2em / 1.5 system-ui;
  margin: 0 auto;
  width: 500px;
}

fieldset {
  border: 0;
}

fieldset,
legend {
  padding: 0;
  margin: 0;
}

input[type="text"] {
  margin-top: 5px;
  padding: 5px;
  width: 50%;
  display: block;
}

div,
fieldset {
  margin-top: 20px;
}

p {
  margin-top: 10px;
  background: #ffc125;
  padding: 20px;
  visibility: hidden;
}
```

```js live-sample___silly-story-start
// Complete variable definitions and random functions

const customName = document.getElementById("custom-name");
const generateBtn = document.querySelector(".generate");
const story = document.querySelector(".story");

function randomValueFromArray(array) {
  const random = Math.floor(Math.random() * array.length);
  return array[random];
}

// Raw text strings

// Willy the Goblin
// Big Daddy
// Father Christmas

// the soup kitchen
// Disneyland
// the White House

// spontaneously combusted
// melted into a puddle on the sidewalk
// turned into a slug and slithered away

// Partial return random string function

function returnRandomStoryString() {
  // It was 94 Fahrenheit outside, so :insertx: went for a walk. When they got to :inserty:, they stared in horror for a few moments, then :insertz:. Bob saw the whole thing, but was not surprised — :insertx: weighs 300 pounds, and it was a hot day.

  return storyText;
}

// Event listener and partial generate function definition

generateBtn.addEventListener("click", generateStory);

function generateStory() {
  if (customName.value !== "") {
    const name = customName.value;
  }

  if (document.getElementById("uk").checked) {
    const weight = Math.round(300);
    const temperature = Math.round(94);
  }

  // TODO: replace "" with the correct expression
  story.textContent = "";
  story.style.visibility = "visible";
}
```

{{EmbedLiveSample("silly-story-start", "100%", 300)}}

## Descrizione del progetto

Sono state fornite alcune stringhe di testo e funzioni JavaScript; occorre scrivere il JavaScript necessario per trasformarle in un programma funzionante, che esegua le seguenti operazioni:

- Genera una storia buffa quando viene premuto il pulsante "Generate random story".
- Sostituisce il nome predefinito "Bob" nella storia con un nome personalizzato, solo se viene inserito un nome personalizzato nel campo di testo "Enter custom name" prima di premere il pulsante di generazione.
- Converte le quantità e le unità di peso e temperatura statunitensi predefinite nella storia nei corrispondenti equivalenti britannici, se viene selezionato il pulsante radio UK prima di premere il pulsante di generazione.
- Genera una nuova storia buffa casuale ogni volta che viene premuto il pulsante.

### Variabili e funzioni iniziali

Nel JavaScript, sotto il commento "Complete variable definitions and random function", sono presenti tre costanti che memorizzano riferimenti a:

- Il campo di testo "Enter custom name": `customName`.
- Il pulsante "Generate random story": `generateBtn`.
- L'elemento {{htmlelement("p")}} nella parte inferiore del body HTML in cui verrà copiata la storia: `story`.

Inoltre, è presente una funzione chiamata `randomValueFromArray()` che riceve un array come input e restituisce casualmente uno degli elementi memorizzati nell'array.

Sotto il commento "Raw text strings", sono presenti alcune stringhe di testo commentate che fungeranno da input per il programma. È necessario rimuovere il commento da queste stringhe e memorizzarle in costanti nel modo seguente:

1. Memorizzare il primo gruppo di tre stringhe in un array chiamato `characters`.
2. Memorizzare il secondo gruppo di tre stringhe in un array chiamato `places`.
3. Memorizzare il terzo gruppo di tre stringhe in un array chiamato `events`.

### Completamento della funzione `returnRandomStoryString()`

Sotto il commento "Partial return random string function" è presente una funzione `returnRandomStoryString()` parzialmente completata, contenente una lunga stringa di testo commentata e un'istruzione `return` che restituisce un valore chiamato `storyText`.

Per completare questa funzione:

1. Rimuovere il commento dalla lunga stringa di testo e memorizzarla in una variabile chiamata `storyText`. Dovrebbe essere un template literal.
2. Aggiungere tre costanti chiamate `randomCharacter`, `randomPlace` e `randomEvent` subito sopra il template literal. Dovrebbero essere impostate uguali a tre chiamate `randomValueFromArray()`, che dovrebbero restituire rispettivamente una stringa casuale dagli array `characters`, `places` ed `events`.
3. Nel template literal, sostituire le occorrenze di `:insertx:`, `:inserty:` e `:insertz:` con espressioni incorporate contenenti rispettivamente `randomCharacter`, `randomPlace` e `randomEvent`.

### Completamento della funzione `generateStory()`

Sotto il commento "Event listener and partial generate function definition" sono presenti un paio di elementi di codice:

- Una riga che aggiunge un event listener `click` alla variabile `generateBtn`, in modo che la funzione `generateStory()` venga eseguita quando si fa clic sul pulsante che essa rappresenta.
- Una definizione parzialmente completata della funzione `generateStory()`. Per il resto della sfida, occorre completare le righe all'interno di questa funzione per renderla pienamente funzionante.

Seguire questi passaggi per completare la funzione:

1. Creare una nuova variabile chiamata `newStory` e impostarne il valore uguale a una chiamata a `returnRandomStoryString()`. Questa funzione è necessaria per poter creare una nuova storia casuale ogni volta che viene premuto il pulsante. Se `newStory` venisse impostata direttamente su `storyText`, sarebbe possibile generare una nuova storia soltanto una volta.
2. All'interno del primo blocco `if`, aggiungere una chiamata a un metodo di sostituzione delle stringhe per sostituire il nome `Bob`, trovato nella stringa `newStory`, con la variabile `name`. In questo blocco si sta dicendo: "Se è stato inserito un valore nell'input di testo `customName`, sostituire `Bob` nella storia con quel nome personalizzato."
3. All'interno del secondo blocco `if`, viene verificato se è stato selezionato il pulsante radio `uk`. In tal caso, occorre convertire i valori di peso e temperatura nella storia da libbre e Fahrenheit a stone e Celsius. Ecco cosa fare:
   1. Cercare le formule per convertire le libbre in stone e i Fahrenheit in Celsius.
   2. All'interno della riga che definisce la costante `weight`, sostituire `300` con un calcolo che converta 300 libbre in stone. Concatenare `" stone"` alla fine del risultato della chiamata complessiva a `Math.round()`.
   3. All'interno della riga che definisce la variabile `temperature`, sostituire `94` con un calcolo che converta 94 Fahrenheit in Celsius. Concatenare `" Celsius"` alla fine del risultato della chiamata complessiva a `Math.round()`.
   4. Subito sotto le due definizioni di variabili, aggiungere altre due righe di sostituzione delle stringhe che sostituiscano `300 pounds` con il contenuto della variabile `weight` e `94 Fahrenheit` con il contenuto della variabile `temperature`.
4. Infine, nella penultima riga della funzione, impostare la proprietà `textContent` della variabile `story` (che fa riferimento al paragrafo) uguale a `newStory`.

## Suggerimenti

- Non è necessario modificare in alcun modo HTML e CSS.
- [`Math.round()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/round) è un metodo JavaScript integrato che arrotonda il risultato di un calcolo al numero intero più vicino.
- Ci sono tre occorrenze di stringhe da sostituire. Si potrebbe usare il metodo `replace()` o un'altra soluzione.

## Esempio

L'app completata dovrebbe funzionare come nel seguente esempio live:

{{EmbedLiveSample("silly-story-finish", "100%", 500)}}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere un aspetto simile al seguente:

```js live-sample___silly-story-finish
// Complete variable definitions and random function

const customName = document.getElementById("custom-name");
const generateBtn = document.querySelector(".generate");
const story = document.querySelector(".story");

function randomValueFromArray(array) {
  const random = Math.floor(Math.random() * array.length);
  return array[random];
}

// Solution: Raw text strings

const characters = ["Willy the Goblin", "Big Daddy", "Father Christmas"];
const places = ["the soup kitchen", "Disneyland", "the White House"];
const events = [
  "spontaneously combusted",
  "melted into a puddle on the sidewalk",
  "turned into a slug and slithered away",
];

// Solution: Partial return random string function

function returnRandomStoryString() {
  const randomCharacter = randomValueFromArray(characters);
  const randomPlace = randomValueFromArray(places);
  const randomEvent = randomValueFromArray(events);

  let storyText = `It was 94 Fahrenheit outside, so ${randomCharacter} went for a walk. When they got to ${randomPlace}, they stared in horror for a few moments, then ${randomEvent}. Bob saw the whole thing, but was not surprised — ${randomCharacter} weighs 300 pounds, and it was a hot day.`;

  return storyText;
}

// Solution: Event listener and partial generate function definition

generateBtn.addEventListener("click", generateStory);

function generateStory() {
  let newStory = returnRandomStoryString();

  if (customName.value !== "") {
    const name = customName.value;
    newStory = newStory.replace("Bob", name);
  }

  if (document.getElementById("uk").checked) {
    const weight = `${Math.round(300 / 14)} stone`;
    const temperature = `${Math.round((94 - 32) * (5 / 9))} Celsius`;
    newStory = newStory.replace("300 pounds", weight);
    newStory = newStory.replace("94 Fahrenheit", temperature);
  }

  story.textContent = newStory;
  story.style.visibility = "visible";
}
```

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Arrays", "Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting")}}
