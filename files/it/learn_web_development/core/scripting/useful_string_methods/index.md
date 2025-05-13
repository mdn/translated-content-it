---
title: Metodi utili per le stringhe
short-title: Metodi per le stringhe
slug: Learn_web_development/Core/Scripting/Useful_string_methods
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting")}}

Ora che abbiamo esaminato le basi delle stringhe, passiamo a un livello superiore e iniziamo a pensare quali operazioni utili possiamo eseguire sulle stringhe con i metodi integrati, come trovare la lunghezza di una stringa di testo, unire e dividere stringhe, sostituire un carattere in una stringa con un altro e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>. Conoscenza delle <a href="/it/docs/Learn_web_development/Core/Scripting/Strings">basi delle stringhe</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
          Manipolazione delle stringhe utilizzando proprietà e metodi comuni integrati in JavaScript.
      </td>
    </tr>
  </tbody>
</table>

## Stringhe come oggetti

La maggior parte dei valori può essere utilizzata come se fossero oggetti in JavaScript. Quando crea una stringa, ad esempio utilizzando

```js
const string = "This is my string";
```

sebbene la variabile stessa non sia un oggetto, ha comunque un grande numero di proprietà e metodi disponibili, grazie alla sua utilizzabilità come oggetto quando si accede alle proprietà. Può vedere questo se visita la pagina dell'oggetto {{jsxref("String")}} e guarda l'elenco sul lato della pagina!

**Ora, prima che il suo cervello inizi a sciogliersi, non si preoccupi!** Non è davvero necessario sapere molto su questi primi fasi del suo percorso di apprendimento. Ma ce ne sono alcuni che potenzialmente utilizzerà abbastanza spesso che esamineremo qui.

Inseriamo alcuni esempi nella [console degli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

## Trovare la lunghezza di una stringa

Questo è semplice — si utilizza la proprietà {{jsxref("String.prototype.length", "length")}}. Provi a inserire le seguenti righe:

```js
const browserType = "mozilla";
browserType.length;
```

Questo dovrebbe restituire il numero 7, perché "mozilla" è lunga 7 caratteri. Questo è utile per molti motivi; ad esempio, potrebbe voler trovare le lunghezze di una serie di nomi in modo da poterli visualizzare in ordine di lunghezza, o informare un utente che un nome utente inserito in un campo modulo è troppo lungo se supera una certa lunghezza.

## Recuperare un carattere specifico della stringa

A proposito, può restituire qualsiasi carattere all'interno di una stringa usando la **notazione tra parentesi quadre** — questo significa che include delle parentesi quadre (`[]`) alla fine del nome della sua variabile. All'interno delle parentesi quadre, include il numero del carattere che desidera restituire, quindi ad esempio per recuperare la prima lettera farebbe così:

```js
browserType[0];
```

Ricordi: i computer contano da 0, non da 1!

Per recuperare l'ultimo carattere di _qualsiasi_ stringa, potremmo utilizzare la seguente riga, combinando questa tecnica con la proprietà `length` che abbiamo visto sopra:

```js
browserType[browserType.length - 1];
```

La lunghezza della stringa "mozilla" è 7, ma poiché il conteggio inizia da 0, la posizione dell'ultimo carattere è 6; utilizzando `length-1` otteniamo l'ultimo carattere.

## Verificare se una stringa contiene una sottostringa

A volte potrebbe voler verificare se una stringa più piccola è presente all'interno di una più grande (generalmente si dice _se una sottostringa è presente all'interno di una stringa_). Questo può essere fatto usando il metodo {{jsxref("String.prototype.includes()", "includes()")}}, che accetta un singolo {{Glossary("parameter", "parametro")}} — la sottostringa che si desidera cercare.

Restituisce `true` se la stringa contiene la sottostringa, e `false` altrimenti.

```js
const browserType = "mozilla";

if (browserType.includes("zilla")) {
  console.log("Found zilla!");
} else {
  console.log("No zilla here!");
}
```

Spesso potrebbe voler sapere se una stringa inizia o finisce con una particolare sottostringa. Questo è un bisogno abbastanza comune che ci sono due metodi speciali per questo: {{jsxref("String.prototype.startsWith()", "startsWith()")}} e {{jsxref("String.prototype.endsWith()", "endsWith()")}}:

```js
const browserType = "mozilla";

if (browserType.startsWith("zilla")) {
  console.log("Found zilla!");
} else {
  console.log("No zilla here!");
}
```

```js
const browserType = "mozilla";

if (browserType.endsWith("zilla")) {
  console.log("Found zilla!");
} else {
  console.log("No zilla here!");
}
```

## Trovare la posizione di una sottostringa in una stringa

Può trovare la posizione di una sottostringa all'interno di una stringa più grande usando il metodo {{jsxref("String.prototype.indexOf()", "indexOf()")}}. Questo metodo accetta due {{Glossary("parameter", "parametri")}} – la sottostringa che desidera cercare e un parametro opzionale che specifica il punto di partenza della ricerca.

Se la stringa contiene la sottostringa, `indexOf()` restituisce l'indice della prima occorrenza della sottostringa. Se la stringa non contiene la sottostringa, `indexOf()` restituisce `-1`.

```js
const tagline = "MDN - Resources for developers, by developers";
console.log(tagline.indexOf("developers")); // 20
```

Partendo da `0`, se conta il numero di caratteri (inclusi gli spazi bianchi) dall'inizio della stringa, la prima occorrenza della sottostringa `"developers"` è all'indice `20`.

```js
console.log(tagline.indexOf("x")); // -1
```

Questo, invece, restituisce `-1` perché il carattere `x` non è presente nella stringa.

Ora che sa come trovare la prima occorrenza di una sottostringa, come fa a trovare le occorrenze successive? Può farlo passando un valore che sia maggiore dell'indice della precedente occorrenza come secondo parametro del metodo.

```js
const firstOccurrence = tagline.indexOf("developers");
const secondOccurrence = tagline.indexOf("developers", firstOccurrence + 1);

console.log(firstOccurrence); // 20
console.log(secondOccurrence); // 35
```

Qui stiamo dicendo al metodo di cercare la sottostringa `"developers"` partendo dall'indice `21` (`firstOccurrence + 1`), e restituisce l'indice `35`.

## Estrarre una sottostringa da una stringa

Può estrarre una sottostringa da una stringa usando il metodo {{jsxref("String.prototype.slice()", "slice()")}}. Deve passargli:

- l'indice da cui iniziare l'estrazione
- l'indice a cui fermarsi con l'estrazione. Questo è esclusivo, cioè il carattere a questo indice non è incluso nella sottostringa estratta.

Ad esempio:

```js
const browserType = "mozilla";
console.log(browserType.slice(1, 4)); // "ozi"
```

Il carattere all'indice `1` è `"o"`, e il carattere all'indice 4 è `"l"`. Quindi estraiamo tutti i caratteri a partire da `"o"` e finendo appena prima di `"l"`, dandoci `"ozi"`.

Se sa che vuole estrarre tutti i caratteri rimanenti in una stringa dopo un certo carattere, non deve includere il secondo parametro. Al contrario, deve solo includere la posizione del carattere da cui vuole estrarre i rimanenti caratteri in una stringa. Provi il seguente:

```js
browserType.slice(2); // "zilla"
```

Questo restituisce `"zilla"` — questo perché la posizione del carattere 2 è la lettera `"z"`, e perché non ha incluso un secondo parametro, la sottostringa che è stata restituita era tutti i caratteri rimanenti nella stringa.

> **Nota:** `slice()` ha anche altre opzioni; studi la pagina {{jsxref("String.prototype.slice()", "slice()")}} per vedere cos'altro può scoprire.

## Cambio di maiuscole/minuscole

I metodi delle stringhe {{jsxref("String.prototype.toLowerCase()", "toLowerCase()")}} e {{jsxref("String.prototype.toUpperCase()", "toUpperCase()")}} prendono una stringa e convertono tutti i caratteri in minuscolo o maiuscolo, rispettivamente. Questo può essere utile, ad esempio, se vuole normalizzare tutti i dati inseriti dall'utente prima di salvarli in un database.

Proviamo a inserire le seguenti righe per vedere cosa succede:

```js
const radData = "My NaMe Is MuD";
console.log(radData.toLowerCase());
console.log(radData.toUpperCase());
```

## Aggiornare parti di una stringa

Può sostituire una sottostringa all'interno di una stringa con un'altra sottostringa usando il metodo {{jsxref("String.prototype.replace()", "replace()")}}.

In questo esempio, stiamo fornendo due parametri — la stringa che vogliamo sostituire e la stringa con cui vogliamo sostituirla:

```js
const browserType = "mozilla";
const updated = browserType.replace("moz", "van");

console.log(updated); // "vanilla"
console.log(browserType); // "mozilla"
```

Noti che `replace()`, come molti metodi delle stringhe, non cambia la stringa su cui è stato chiamato, ma restituisce una nuova stringa. Se vuole aggiornare la variabile `browserType` originale, dovrebbe fare qualcosa del genere:

```js
let browserType = "mozilla";
browserType = browserType.replace("moz", "van");

console.log(browserType); // "vanilla"
```

Inoltre, noti che ora dobbiamo dichiarare `browserType` usando `let`, non `const`, perché la stiamo riassegnando.

Sia consapevole che `replace()` in questa forma cambia solo la prima occorrenza della sottostringa. Se vuole cambiare tutte le occorrenze, può usare {{jsxref("String.prototype.replaceAll()", "replaceAll()")}}:

```js
let quote = "To be or not to be";
quote = quote.replaceAll("be", "code");

console.log(quote); // "To code or not to code"
```

## Esempi di apprendimento attivo

In questa sezione, le faremo provare a scrivere del codice per la manipolazione delle stringhe. In ciascun esercizio qui sotto, abbiamo un array di stringhe e un ciclo che elabora ciascun valore nell'array e lo visualizza in un elenco puntato. Non è necessario capire gli array o i cicli in questo momento — questi saranno spiegati in articoli futuri. Tutto quello che deve fare in ciascun caso è scrivere il codice che visualizzerà le stringhe nel formato che vogliamo.

Ogni esempio ha un pulsante "Ripristina", che può utilizzare per ripristinare il codice se commette un errore e non riesce più a farlo funzionare, e un pulsante "Mostra soluzione" che può premere per vedere una possibile risposta se si blocca davvero.

### Filtrare messaggi di saluto

Nel primo esercizio, iniziamo con semplicità — abbiamo un array di messaggi di biglietti di auguri, ma vogliamo ordinarli per elencare solo i messaggi di Natale. Vogliamo che compili un test condizionale all'interno della struttura `if ()` per testare ogni stringa e stamparla nell'elenco solo se è un messaggio di Natale.

Pensi a come potrebbe testare se il messaggio in ciascun caso è un messaggio di Natale. Quale stringa è presente in tutti quei messaggi e quale metodo potrebbe utilizzare per testare se è presente?

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 125px;">
  <ul></ul>
</div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="height: 290px; width: 95%">
const list = document.querySelector('.output ul');
list.textContent = "";
const greetings = ['Happy Birthday!',
                 'Merry Christmas my love',
                 'A happy Christmas to all the family',
                 'You\'re all I want for Christmas',
                 'Get well soon'];

for (const greeting of greetings) {
  // Your conditional test needs to go inside the parentheses
  // in the line below, replacing what's currently there
  if (greeting) {
    const listItem = document.createElement('li');
    listItem.textContent = greeting;
    list.appendChild(listItem);
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
list.textContent = "";
const greetings = [
  'Happy Birthday!',
  'Merry Christmas my love',
  'A happy Christmas to all the family',
  'You\\'re all I want for Christmas',
  'Get well soon',
];

for (const greeting of greetings) {
  // Your conditional test needs to go inside the parentheses
  // in the line below, replacing what's currently there
  if (greeting.includes('Christmas')) {
    const listItem = document.createElement('li');
    listItem.textContent = greeting;
    list.appendChild(listItem);
  }
}`;

let solutionEntry = jsSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = (e) => {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
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

{{ EmbedLiveSample('Filtering_greeting_messages', '100%', 600) }}

### Correggere la capitalizzazione

In questo esercizio, abbiamo i nomi delle città nel Regno Unito, ma la capitalizzazione è tutta incasinata. Vogliamo che le modifichi in modo che siano tutte minuscole, tranne la prima lettera maiuscola. Un modo buono per farlo è:

1. Convertire l'intera stringa contenuta nella variabile `city` in minuscolo e memorizzarla in una nuova variabile.
2. Recuperare la prima lettera della stringa in questa nuova variabile e memorizzarla in un'altra variabile.
3. Usare quest'ultima variabile come sottostringa, sostituire la prima lettera della stringa minuscola con la prima lettera della stringa minuscola cambiata in maiuscolo. Memorizzare il risultato di questa procedura di sostituzione in un'altra nuova variabile.
4. Cambiare il valore della variabile `result` per essere uguale al risultato finale, non `city`.

> [!NOTE]
> Un suggerimento — i parametri dei metodi per le stringhe non devono essere letterali delle stringhe; possono anche essere variabili, o anche variabili con un metodo invocato su di esse.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 125px;">
  <ul></ul>
</div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="height: 250px; width: 95%">
const list = document.querySelector('.output ul');
list.textContent = "";
const cities = ['lonDon', 'ManCHESTer', 'BiRmiNGHAM', 'liVERpoOL'];

for (const city of cities) {
  // write your code just below here

  const result = city;
  const listItem = document.createElement('li');
  listItem.textContent = result;
  list.appendChild(listItem);
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

reset.addEventListener("click", function () {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = jsSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", function () {
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
list.textContent = "";
const cities = ['lonDon', 'ManCHESTer', 'BiRmiNGHAM', 'liVERpoOL'];

for (const city of cities) {
  // write your code just below here
  const lower = city.toLowerCase();
  const firstLetter = lower.slice(0,1);
  const capitalized = lower.replace(firstLetter,firstLetter.toUpperCase());
  const result = capitalized;
  const listItem = document.createElement('li');
  listItem.textContent = result;
  list.appendChild(listItem);
}`;

let solutionEntry = jsSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = function (e) {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
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

textarea.onkeyup = function () {
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

{{ EmbedLiveSample('Fixing_capitalization', '100%', 570) }}

### Creare nuove stringhe da parti vecchie

In quest'ultimo esercizio, l'array contiene una serie di stringhe che contengono informazioni sulle stazioni ferroviarie nel Nord dell'Inghilterra. Le stringhe sono elementi di dati che contengono il codice della stazione a tre lettere, seguito da alcuni dati leggibili dalla macchina, seguito da un punto e virgola, seguito dal nome della stazione leggibile dall'uomo. Ad esempio:

```plain
MAN675847583748sjt567654;Manchester Piccadilly
```

Vogliamo estrarre il codice e il nome della stazione e metterli insieme in una stringa con la struttura seguente:

```plain
MAN: Manchester Piccadilly
```

Consigliamo di farlo in questo modo:

1. Estrarre il codice della stazione a tre lettere e memorizzarlo in una nuova variabile.
2. Trovare il numero indice del carattere del punto e virgola.
3. Estrarre il nome della stazione leggibile dall'uomo usando il numero indice del carattere del punto e virgola come punto di riferimento e memorizzarlo in una nuova variabile.
4. Concatenare le due nuove variabili e un letterale di stringa per creare la stringa finale.
5. Cambiare il valore della variabile `result` con la stringa finale, non `station`.

```html hidden
<h2>Live output</h2>

<div class="output" style="min-height: 125px;">
  <ul></ul>
</div>

<h2>Editable code</h2>
<p class="a11y-label">
  Press Esc to move focus away from the code area (Tab inserts a tab character).
</p>

<textarea id="code" class="playable-code" style="height: 285px; width: 95%">
const list = document.querySelector('.output ul');
list.textContent = "";
const stations = ['MAN675847583748sjt567654;Manchester Piccadilly',
                  'GNF576746573fhdg4737dh4;Greenfield',
                  'LIV5hg65hd737456236dch46dg4;Liverpool Lime Street',
                  'SYB4f65hf75f736463;Stalybridge',
                  'HUD5767ghtyfyr4536dh45dg45dg3;Huddersfield'];

for (const station of stations) {
  // write your code just below here

  const result = station;
  const listItem = document.createElement('li');
  listItem.textContent = result;
  list.appendChild(listItem);
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

reset.addEventListener("click", function () {
  textarea.value = code;
  userEntry = textarea.value;
  solutionEntry = jsSolution;
  solution.value = "Show solution";
  updateCode();
});

solution.addEventListener("click", function () {
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
list.textContent = '';
const stations = ['MAN675847583748sjt567654;Manchester Piccadilly',
                  'GNF576746573fhdg4737dh4;Greenfield',
                  'LIV5hg65hd737456236dch46dg4;Liverpool Lime Street',
                  'SYB4f65hf75f736463;Stalybridge',
                  'HUD5767ghtyfyr4536dh45dg45dg3;Huddersfield'];

for (const station of stations) {
  // write your code just below here
  const code = station.slice(0,3);
  const semiColon = station.indexOf(';');
  const name = station.slice(semiColon + 1);
  const result = \`\${code}: \${name}\`;
  const listItem = document.createElement('li');
  listItem.textContent = result;
  list.appendChild(listItem);
}`;

let solutionEntry = jsSolution;

textarea.addEventListener("input", updateCode);
window.addEventListener("load", updateCode);

// stop tab key tabbing out of textarea and
// make it write a tab at the caret position instead

textarea.onkeydown = function (e) {
  if (e.code === "Tab") {
    e.preventDefault();
    insertAtCaret("\t");
  }

  if (e.code === "Escape") {
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

textarea.onkeyup = function () {
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

{{ EmbedLiveSample('Making_new_strings_from_old_parts', '100%', 600) }}

## Testa le sue capacità!

Ha raggiunto la fine di questo articolo, ma è in grado di ricordare le informazioni più importanti? Può trovare ulteriori test per verificare se ha trattenuto queste informazioni prima di andare avanti — veda [Test your skills: Strings](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Strings).

## Conclusione

Non può sfuggire al fatto che essere in grado di gestire parole e frasi nella programmazione è molto importante — particolarmente in JavaScript, poiché i siti web riguardano tutta la comunicazione con le persone. Questo articolo le ha fornito le basi di cui ha bisogno per sapere come manipolare le stringhe per ora. Questo dovrebbe servirla bene mentre affronta argomenti più complessi in futuro. Prossimamente, esamineremo l'ultimo tipo di dati principale su cui dobbiamo concentrarci a breve termine — gli array.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting")}}
