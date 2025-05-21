---
title: Metodi utili per le stringhe
short-title: Metodi delle stringhe
slug: Learn_web_development/Core/Scripting/Useful_string_methods
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting")}}

Ora che abbiamo esaminato le basi delle stringhe, facciamo un passo avanti e iniziamo a pensare a quali operazioni utili possiamo fare sulle stringhe con i metodi integrati, come trovare la lunghezza di una stringa di testo, unire e dividere le stringhe, sostituire un carattere in una stringa con un altro e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti del CSS</a>. Conoscenza delle <a href="/it/docs/Learn_web_development/Core/Scripting/Strings">basi delle stringhe</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
          Manipolazione delle stringhe utilizzando le proprietà e i metodi comuni integrati in JavaScript.
      </td>
    </tr>
  </tbody>
</table>

## Stringhe come oggetti

La maggior parte dei valori può essere utilizzata come se fossero oggetti in JavaScript. Quando si crea una stringa, per esempio utilizzando

```js
const string = "This is my string";
```

anche se la variabile in sé non è un oggetto, ha comunque un gran numero di proprietà e metodi disponibili, in virtù del fatto di poter essere utilizzabile come un oggetto quando si accede alle proprietà. Puoi vedere questo se vai alla pagina dell'oggetto {{jsxref("String")}} e guardi l'elenco sul lato della pagina!

**Ora, prima che il tuo cervello inizi a fondere, non preoccuparti!** Non hai davvero bisogno di conoscere la maggior parte di queste cose all'inizio del tuo percorso di apprendimento. Ma ci sono alcune cose che potresti usare abbastanza spesso che esamineremo qui.

Inseriamo alcuni esempi nella [console degli sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

## Trovare la lunghezza di una stringa

Questo è facile — si usa la proprietà {{jsxref("String.prototype.length", "length")}}. Prova a inserire le seguenti righe:

```js
const browserType = "mozilla";
browserType.length;
```

Questo dovrebbe restituire il numero 7, perché "mozilla" è lunga 7 caratteri. Questo è utile per molti motivi; per esempio, potresti voler trovare le lunghezze di una serie di nomi per poterli visualizzare in ordine di lunghezza, o informare un utente che un nome utente che ha inserito in un campo modulo è troppo lungo se supera una certa lunghezza.

## Recuperare un carattere specifico di una stringa

In relazione a questo, puoi restituire qualsiasi carattere all'interno di una stringa utilizzando la **notazione a parentesi quadre** — questo significa che si includono parentesi quadre (`[]`) alla fine del nome della variabile. All'interno delle parentesi quadre, si include il numero del carattere che si vuole restituire, quindi per esempio per recuperare la prima lettera si farebbe così:

```js
browserType[0];
```

Ricorda: i computer contano da 0, non da 1!

Per recuperare l'ultimo carattere di _qualsiasi_ stringa, potremmo usare la seguente riga, combinando questa tecnica con la proprietà `length` che abbiamo visto sopra:

```js
browserType[browserType.length - 1];
```

La lunghezza della stringa "mozilla" è 7, ma siccome il conteggio inizia da 0, la posizione dell'ultimo carattere è 6; usando `length-1` otteniamo l'ultimo carattere.

## Verificare se una stringa contiene una sottostringa

A volte vorrai sapere se una stringa più piccola è presente all'interno di una più grande (generalmente diciamo _se una sottostringa è presente all'interno di una stringa_). Questo può essere fatto utilizzando il metodo {{jsxref("String.prototype.includes()", "includes()")}}, che prende un singolo {{Glossary("parameter", "parametro")}} — la sottostringa che vuoi cercare.

Ritorna `true` se la stringa contiene la sottostringa, e `false` altrimenti.

```js
const browserType = "mozilla";

if (browserType.includes("zilla")) {
  console.log("Found zilla!");
} else {
  console.log("No zilla here!");
}
```

Spesso vorrai sapere se una stringa inizia o termina con una particolare sottostringa. Questo è un bisogno abbastanza comune da avere due metodi speciali per questo: {{jsxref("String.prototype.startsWith()", "startsWith()")}} e {{jsxref("String.prototype.endsWith()", "endsWith()")}}:

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

Puoi trovare la posizione di una sottostringa all'interno di una stringa più grande utilizzando il metodo {{jsxref("String.prototype.indexOf()", "indexOf()")}}. Questo metodo prende due {{Glossary("parameter", "parametri")}} – la sottostringa che vuoi cercare, e un parametro opzionale che specifica il punto di partenza della ricerca.

Se la stringa contiene la sottostringa, `indexOf()` ritorna l'indice della prima occorrenza della sottostringa. Se la stringa non contiene la sottostringa, `indexOf()` ritorna `-1`.

```js
const tagline = "MDN - Resources for developers, by developers";
console.log(tagline.indexOf("developers")); // 20
```

Partendo da `0`, se conti il numero di caratteri (incluso lo spazio bianco) dall'inizio della stringa, la prima occorrenza della sottostringa `"developers"` è all'indice `20`.

```js
console.log(tagline.indexOf("x")); // -1
```

Questo, invece, ritorna `-1` perché il carattere `x` non è presente nella stringa.

Quindi ora che sai come trovare la prima occorrenza di una sottostringa, come fai a trovare le occorrenze successive? Puoi farlo passando un valore maggiore dell'indice della precedente occorrenza come secondo parametro del metodo.

```js
const firstOccurrence = tagline.indexOf("developers");
const secondOccurrence = tagline.indexOf("developers", firstOccurrence + 1);

console.log(firstOccurrence); // 20
console.log(secondOccurrence); // 35
```

Qui stiamo dicendo al metodo di cercare la sottostringa `"developers"` a partire dall'indice `21` (`firstOccurrence + 1`), e ritorna l'indice `35`.

## Estrarre una sottostringa da una stringa

Puoi estrarre una sottostringa da una stringa utilizzando il metodo {{jsxref("String.prototype.slice()", "slice()")}}. Gli passi:

- l'indice da cui iniziare l'estrazione
- l'indice a cui fermarsi. Questo è esclusivo, il che significa che il carattere a questo indice non è incluso nella sottostringa estratta.

Per esempio:

```js
const browserType = "mozilla";
console.log(browserType.slice(1, 4)); // "ozi"
```

Il carattere all'indice `1` è `"o"`, e il carattere all'indice 4 è `"l"`. Quindi estraiamo tutti i caratteri a partire da `"o"` e terminando appena prima di `"l"`, ottenendo `"ozi"`.

Se sai che vuoi estrarre tutti i caratteri rimanenti in una stringa dopo un certo carattere, non devi includere il secondo parametro. Invece, devi solo includere la posizione del carattere da cui vuoi estrarre il resto dei caratteri in una stringa. Prova quanto segue:

```js
browserType.slice(2); // "zilla"
```

Questo ritorna `"zilla"` — ciò avviene perché la posizione del carattere 2 è la lettera `"z"`, e poiché non hai incluso un secondo parametro, la sottostringa restituita include tutti i caratteri rimanenti nella stringa.

> **Nota:** `slice()` ha altre opzioni; studia la pagina {{jsxref("String.prototype.slice()", "slice()")}} per vedere cos'altro puoi scoprire.

## Cambiare maiuscole e minuscole

I metodi delle stringhe {{jsxref("String.prototype.toLowerCase()", "toLowerCase()")}} e {{jsxref("String.prototype.toUpperCase()", "toUpperCase()")}} prendono una stringa e convertono tutti i caratteri rispettivamente in minuscole o maiuscole. Questo può essere utile per esempio se vuoi normalizzare tutti i dati inseriti dagli utenti prima di memorizzarli in un database.

Proviamo a inserire le seguenti righe per vedere cosa succede:

```js
const radData = "My NaMe Is MuD";
console.log(radData.toLowerCase());
console.log(radData.toUpperCase());
```

## Aggiornare parti di una stringa

Puoi sostituire una sottostringa all'interno di una stringa con un'altra sottostringa utilizzando il metodo {{jsxref("String.prototype.replace()", "replace()")}}.

In questo esempio, stiamo fornendo due parametri — la stringa che vogliamo sostituire e la stringa con cui vogliamo sostituirla:

```js
const browserType = "mozilla";
const updated = browserType.replace("moz", "van");

console.log(updated); // "vanilla"
console.log(browserType); // "mozilla"
```

Nota che `replace()`, come molti metodi di stringa, non cambia la stringa su cui è stato chiamato, ma restituisce una nuova stringa. Se vuoi aggiornare la variabile originale `browserType`, dovresti fare qualcosa del genere:

```js
let browserType = "mozilla";
browserType = browserType.replace("moz", "van");

console.log(browserType); // "vanilla"
```

Nota anche che ora dobbiamo dichiarare `browserType` usando `let`, non `const`, perché la stiamo riassegnando.

Tieni presente che `replace()` in questa forma cambia solo la prima occorrenza della sottostringa. Se vuoi cambiare tutte le occorrenze, puoi usare {{jsxref("String.prototype.replaceAll()", "replaceAll()")}}:

```js
let quote = "To be or not to be";
quote = quote.replaceAll("be", "code");

console.log(quote); // "To code or not to code"
```

## Esempi di apprendimento attivo

In questa sezione, ti invitiamo a provare a scrivere del codice per la manipolazione delle stringhe. In ogni esercizio qui sotto, abbiamo un array di stringhe e un loop che elabora ogni valore nell'array e lo visualizza in un elenco puntato. Non è necessario comprendere gli array o i loop in questo momento — saranno spiegati in articoli futuri. Tutto ciò che devi fare in ogni caso è scrivere il codice che produrrà le stringhe nel formato che desideriamo.

Ogni esempio viene fornito con un pulsante "Reset", che puoi utilizzare per reimpostare il codice se commetti un errore e non riesci a farlo funzionare di nuovo, e un pulsante "Mostra soluzione" che puoi premere per vedere una risposta possibile se rimani davvero bloccato.

### Filtrare i messaggi di saluto

Nel primo esercizio, inizieremo in modo semplice — abbiamo un array di messaggi di biglietti di auguri, ma vogliamo ordinarli per elencare solo i messaggi di Natale. Ti invitiamo a riempire un test condizionale all'interno della struttura `if ()` per testare ogni stringa e stamparla nell'elenco solo se è un messaggio di Natale.

Pensa a come potresti verificare se il messaggio in ciascun caso è un messaggio di Natale. Quale stringa è presente in tutti quei messaggi e quale metodo potresti utilizzare per verificare se è presente?

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

### Correggere le maiuscole

In questo esercizio, abbiamo i nomi delle città del Regno Unito, ma le maiuscole sono tutte sbagliate. Ti invitiamo a modificarli in modo che siano tutte minuscole, tranne la prima lettera che deve essere maiuscola. Un buon modo per farlo è:

1. Convertire l'intera stringa contenuta nella variabile `city` in minuscole e memorizzarla in una nuova variabile.
2. Estrarre la prima lettera della stringa in questa nuova variabile e memorizzarla in un'altra variabile.
3. Usando quest'ultima variabile come sottostringa, sostituire la prima lettera della stringa in minuscole con la prima lettera della stringa in minuscole cambiata in maiuscole. Memorizzare il risultato di questa procedura di sostituzione in un'altra nuova variabile.
4. Modificare il valore della variabile `result` in modo che sia uguale al risultato finale, non a `city`.

> [!NOTE]
> Un suggerimento — i parametri dei metodi delle stringhe non devono essere letterali; possono anche essere variabili, o anche variabili con un metodo invocato su di esse.

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

### Creare nuove stringhe da vecchie parti

In questo ultimo esercizio, l'array contiene una serie di stringhe che contengono informazioni sulle stazioni ferroviarie nel Nord dell'Inghilterra. Le stringhe sono elementi di dati che contengono il codice stazione a tre lettere, seguito da alcuni dati leggibili dalle macchine, seguiti da un punto e virgola, seguiti dal nome della stazione leggibile dall'uomo. Per esempio:

```plain
MAN675847583748sjt567654;Manchester Piccadilly
```

Vogliamo estrarre il codice stazione e il nome e metterli insieme in una stringa con la seguente struttura:

```plain
MAN: Manchester Piccadilly
```

Consigliamo di farlo in questo modo:

1. Estrarre il codice stazione a tre lettere e memorizzarlo in una nuova variabile.
2. Trovare il numero dell'indice del carattere del punto e virgola.
3. Estrarre il nome della stazione leggibile dall'uomo usando il numero di indice del carattere del punto e virgola come punto di riferimento, e memorizzarlo in una nuova variabile.
4. Concatenare le due nuove variabili e un letterale di stringa per creare la stringa finale.
5. Modificare il valore della variabile `result` nella stringa finale, non `station`.

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

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare di aver conservato queste informazioni prima di passare oltre — vedi [Testa le tue abilità: Stringhe](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Strings).

## Conclusione

Non puoi sfuggire al fatto che essere in grado di gestire parole e frasi nella programmazione è molto importante — particolarmente in JavaScript, poiché i siti web riguardano la comunicazione con le persone. Questo articolo ti ha dato le basi che devi sapere sulla manipolazione delle stringhe per ora. Questo dovrebbe servirti bene mentre entri in argomenti più complessi in futuro. Successivamente, esamineremo l'ultimo importante tipo di dati su cui dobbiamo concentrarci a breve termine — gli array.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting")}}
