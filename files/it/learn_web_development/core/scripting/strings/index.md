---
title: Gestione del testo — stringhe in JavaScript
short-title: Strings
slug: Learn_web_development/Core/Scripting/Strings
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting")}}

Successivamente, ci concentreremo sulle stringhe — questo è il termine utilizzato in programmazione per indicare parti di testo. In questo articolo, esamineremo tutte le cose comuni che dovreste sapere riguardo le stringhe quando state imparando JavaScript, come creare stringhe, scappare le virgolette nelle stringhe e unire le stringhe.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprendere <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e i <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Creare stringhe letterali.</li>
          <li>La necessità di far combaciare le virgolette.</li>
          <li>Concatenazione di stringhe.</li>
          <li>Esclusione dei caratteri nelle stringhe.</li>
          <li>Template literals, incluso l'uso delle variabili e template literals multilinea.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Il potere delle parole

Le parole sono molto importanti per gli esseri umani — sono una grande parte del nostro modo di comunicare. Poiché il web è un mezzo principalmente basato sul testo progettato per consentire agli esseri umani di comunicare e condividere informazioni, è utile per noi avere controllo sulle parole che appaiono su di esso. {{Glossary("HTML", "HTML")}} fornisce struttura e significato al testo, {{Glossary("CSS", "CSS")}} ci permette di stilizzarlo con precisione, e JavaScript offre molte funzionalità per manipolare le stringhe. Queste includono la creazione di messaggi di benvenuto personalizzati, la visualizzazione delle etichette di testo giuste quando necessario, l'ordinamento dei termini nell'ordine desiderato e molto altro.

Praticamente tutti i programmi che vi abbiamo mostrato finora nel corso hanno coinvolto qualche tipo di manipolazione delle stringhe.

## Dichiarazione delle stringhe

Le stringhe sono trattate in modo simile ai numeri a prima vista, ma quando approfondite inizierete a vedere alcune differenze notevoli. Iniziamo inserendo alcune righe di base nella [console degli strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per familiarizzare.

Per cominciare, inserite le seguenti righe:

```js
const string = "The revolution will not be televised.";
console.log(string);
```

Proprio come abbiamo fatto con i numeri, stiamo dichiarando una variabile, inizializzandola con un valore stringa e quindi restituendo il valore. L'unica differenza qui è che quando si scrive una stringa, è necessario circondare il valore con virgolette.

Se non lo fate, o mancate una delle virgolette, otterrete un errore. Provate a inserire le seguenti righe:

```js example-bad
const badString1 = This is a test;
const badString2 = 'This is a test;
const badString3 = This is a test';
```

Queste righe non funzionano perché qualsiasi testo senza virgolette intorno è interpretato come un nome di variabile, nome di proprietà, parola riservata, o simile. Se il browser non riconosce il testo non quotato, viene generato un errore (es. "manca ; prima della dichiarazione"). Se il browser riesce a rilevare l'inizio di una stringa ma non la sua fine (a causa della seconda virgoletta mancante), segnala un errore "literal stringa non terminata". Se il vostro programma genera tali errori, allora ritornate e controllate tutte le vostre stringhe per assicurarvi di non avere virgolette mancanti.

Il seguente funzionerà se avete precedentemente definito la variabile `string` — provateci ora:

```js
const badString = string;
console.log(badString);
```

`badString` è ora impostato per avere lo stesso valore di `string`.

### Singole virgolette, doppie virgolette e backticks

In JavaScript, potete scegliere virgolette singole (`'`), virgolette doppie (`"`) o backticks (`` ` ``) per racchiudere le vostre stringhe. Tutti i seguenti funzioneranno:

```js-nolint
const single = 'Single quotes';
const double = "Double quotes";
const backtick = `Backtick`;

console.log(single);
console.log(double);
console.log(backtick);
```

Dovete usare lo stesso carattere per l'inizio e la fine di una stringa, altrimenti otterrete un errore:

```js-nolint example-bad
const badQuotes = 'This is not allowed!";
```

Le stringhe dichiarate usando virgolette singole e stringhe dichiarate usando virgolette doppie sono le stesse, e quale usare dipende dalla preferenza personale — sebbene sia buona pratica scegliere uno stile e usarlo in modo coerente nel vostro codice.

Le stringhe dichiarate usando backticks sono un tipo speciale di stringa chiamato [_template literal_](/it/docs/Web/JavaScript/Reference/Template_literals). Nella maggior parte dei modi, i template literals sono come le normali stringhe, ma hanno alcune proprietà speciali:

- potete [inserire JavaScript](#inserimento_di_javascript) in essi
- potete dichiarare i template literals su [più righe](#stringhe_multilinea)

## Inserimento di JavaScript

All'interno di un template literal, potete racchiudere variabili o espressioni JavaScript all'interno di `${ }`, e il risultato sarà incluso nella stringa:

```js
const name = "Chris";
const greeting = `Hello, ${name}`;
console.log(greeting); // "Hello, Chris"
```

Potete usare la stessa tecnica per unire insieme due variabili:

```js
const one = "Hello, ";
const two = "how are you?";
const joined = `${one}${two}`;
console.log(joined); // "Hello, how are you?"
```

Unire stringhe insieme in questo modo si chiama _concatenazione_.

### Concatenazione nel contesto

Vediamo un esempio di concatenazione in azione:

```html live-sample___string-concat
<button>Press me</button>
<div id="greeting"></div>
```

```js live-sample___string-concat
const button = document.querySelector("button");

function greet() {
  const name = prompt("What is your name?");
  const greeting = document.querySelector("#greeting");
  greeting.textContent = `Hello ${name}, nice to see you!`;
}

button.addEventListener("click", greet);
```

{{EmbedLiveSample('string-concat', , '50', , , , , 'allow-modals')}}

Qui, stiamo usando la funzione [`window.prompt()`](/it/docs/Web/API/Window/prompt), che chiede all'utente di rispondere a una domanda tramite una finestra di dialogo pop-up e quindi memorizza il testo inserito all'interno di una variabile data — in questo caso `name`. Successivamente visualizziamo una stringa che inserisce il nome in un messaggio di benvenuto generico.

### Concatenazione usando "+"

Potete usare `${}` solo con i template literals, non con le stringhe normali. Potete concatenare stringhe normali usando l'operatore `+`:

```js
const greeting = "Hello";
const name = "Chris";
console.log(greeting + ", " + name); // "Hello, Chris"
```

Tuttavia, i template literals solitamente vi offrono un codice più leggibile:

```js
const greeting = "Hello";
const name = "Chris";
console.log(`${greeting}, ${name}`); // "Hello, Chris"
```

### Inclusione di espressioni nelle stringhe

Potete includere espressioni JavaScript nei template literals, oltre alle variabili, e i risultati saranno inclusi nel risultato:

```js
const song = "Fight the Youth";
const score = 9;
const highestScore = 10;
const output = `I like the song ${song}. I gave it a score of ${
  (score / highestScore) * 100
}%.`;
console.log(output); // "I like the song Fight the Youth. I gave it a score of 90%."
```

## Stringhe multilinea

I template literals rispettano le interruzioni di linea nel codice sorgente, quindi potete scrivere stringhe che si estendono su più righe come questa:

```js
const newline = `One day you finally knew
what you had to do, and began,`;
console.log(newline);

/*
One day you finally knew
what you had to do, and began,
*/
```

Per avere l'equivalente output usando una stringa normale dovreste includere i caratteri di interruzione di linea (`\n`) nella stringa:

```js
const newline = "One day you finally knew\nwhat you had to do, and began,";
console.log(newline);

/*
One day you finally knew
what you had to do, and began,
*/
```

Consultate la nostra pagina di riferimento sui [Template literals](/it/docs/Web/JavaScript/Reference/Template_literals) per altri esempi e dettagli su funzionalità avanzate.

## Inclusione di virgolette nelle stringhe

Dato che utilizziamo le virgolette per indicare l'inizio e la fine delle stringhe, come possiamo includere effettivamente le virgolette nelle stringhe? Sappiamo che questo non funzionerà:

```js-nolint example-bad
const badQuotes = "She said "I think so!"";
```

Un'opzione comune è usare uno degli altri caratteri per dichiarare la stringa:

```js-nolint
const goodQuotes1 = 'She said "I think so!"';
const goodQuotes2 = `She said "I'm not going in there!"`;
```

Un'altra opzione è _scappare_ il segno di citazione problematica. Scappare i caratteri significa che facciamo qualcosa a essi per assicurarci che siano riconosciuti come testo, non parte del codice. In JavaScript, lo facciamo mettendo una barra rovesciata appena prima del carattere. Provate questo:

```js-nolint
const bigmouth = 'I\'ve got no right to take my place…';
console.log(bigmouth);
```

Potete usare la stessa tecnica per inserire altri caratteri speciali. Consultate [Sequenze di scappamento](/it/docs/Web/JavaScript/Reference/Lexical_grammar#escape_sequences) per ulteriori dettagli.

## Numeri vs. stringhe

Cosa succede quando proviamo a concatenare una stringa e un numero? Vediamo nella nostra console:

```js
const name = "Front ";
const number = 242;
console.log(name + number); // "Front 242"
```

Potreste aspettarvi che questo restituisca un errore, ma funziona perfettamente. Come i numeri dovrebbero essere visualizzati come stringhe è abbastanza ben definito, quindi il browser converte automaticamente il numero in una stringa e concatena le due stringhe.

Se avete una variabile numerica che volete convertire in una stringa o una variabile stringa che volete convertire in un numero, potete usare le seguenti due costruzioni:

- La funzione {{jsxref("Number/Number", "Number()")}} converte qualsiasi cosa le venga passata in un numero se può. Provate il seguente:

  ```js
  const myString = "123";
  const myNum = Number(myString);
  console.log(typeof myNum);
  // number
  ```

- Viceversa, la funzione {{jsxref("String/String", "String()")}} converte il suo argomento in una stringa. Provate questo:

  ```js
  const myNum2 = 123;
  const myString2 = String(myNum2);
  console.log(typeof myString2);
  // string
  ```

Queste costruzioni possono essere veramente utili in alcune situazioni. Ad esempio, se un utente inserisce un numero in un campo di testo di un modulo, è una stringa. Tuttavia, se volete aggiungere questo numero a qualcosa, avrete bisogno che sia un numero, quindi potreste passarlo attraverso `Number()` per gestire questo. Abbiamo fatto esattamente questo nel nostro [Gioco del Indovinello del Numero](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game.html), nella funzione `checkGuess`.

## Sommario

Quindi questi sono i fondamentali delle stringhe coperti in JavaScript. Nel prossimo articolo, costruiremo su questo, esaminando alcuni dei metodi incorporati disponibili per le stringhe in JavaScript e come possiamo usarli per manipolare le nostre stringhe nella forma desiderata.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting")}}
