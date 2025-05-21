---
title: Gestione del testo — stringhe in JavaScript
short-title: Strings
slug: Learn_web_development/Core/Scripting/Strings
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting")}}

Successivamente, rivolgeremo la nostra attenzione alle stringhe: è così che vengono chiamati i pezzi di testo nella programmazione. In questo articolo, esamineremo tutte le cose comuni che è davvero essenziale conoscere sulle stringhe quando si impara JavaScript, come la creazione di stringhe, l'escape delle virgolette nelle stringhe e l'unione delle stringhe.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e le <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi del CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Creazione di letterali di stringa.</li>
          <li>L'importanza che le virgolette corrispondano.</li>
          <li>Concatenazione di stringhe.</li>
          <li>Escape di caratteri nelle stringhe.</li>
          <li>Letterali template, inclusi l'uso di variabili e letterali template multilinea.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Il potere delle parole

Le parole sono molto importanti per gli esseri umani: rappresentano una grande parte del nostro modo di comunicare. Poiché il web è un mezzo largamente basato sul testo progettato per consentire agli esseri umani di comunicare e condividere informazioni, è utile avere il controllo sulle parole che vi appaiono. {{Glossary("HTML", "HTML")}} fornisce struttura e significato al testo, {{Glossary("CSS", "CSS")}} ci consente di stilizzarlo con precisione, e JavaScript offre molte funzionalità per la manipolazione delle stringhe. Queste includono la creazione di messaggi di benvenuto personalizzati e prompt, la visualizzazione delle etichette di testo corrette quando necessario, l'ordinamento dei termini nell'ordine desiderato e molto altro.

Praticamente tutti i programmi che abbiamo mostrato finora nel corso hanno coinvolto un po' di manipolazione delle stringhe.

## Dichiarazione delle stringhe

Le stringhe vengono trattate inizialmente in modo simile ai numeri, ma scavando più in profondità inizierete a vedere alcune differenze notevoli. Iniziamo inserendo alcune righe di base nella [console sviluppatore del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per familiarizzare.

Per iniziare, inserite le seguenti righe:

```js
const string = "The revolution will not be televised.";
console.log(string);
```

Proprio come abbiamo fatto con i numeri, dichiariamo una variabile, la inizializziamo con un valore stringa e poi restituiamo il valore. L'unica differenza qui è che quando si scrive una stringa, è necessario circondare il valore con virgolette.

Se non lo fate, o mancate una delle virgolette, si otterrà un errore. Provate ad inserire le seguenti righe:

```js example-bad
const badString1 = This is a test;
const badString2 = 'This is a test;
const badString3 = This is a test';
```

Queste righe non funzionano perché qualsiasi testo senza virgolette intorno è interpretato come nome di variabile, nome di proprietà, parola riservata o simile. Se il browser non riconosce il testo non quotato, viene generato un errore (ad es., "missing; before statement"). Se il browser riesce a rilevare dove inizia una stringa ma non la sua fine (a causa della seconda virgoletta mancante), viene segnalato un errore di "literal di stringa non terminato". Se il vostro programma genera tali errori, tornate indietro e controllate tutte le vostre stringhe per assicurarvi che non manchino virgolette.

Il seguente funzionerà se avete precedentemente definito la variabile `string` — provate ora:

```js
const badString = string;
console.log(badString);
```

`badString` è ora impostato per avere lo stesso valore di `string`.

### Virgolettine singole, doppie e backtick

In JavaScript, potete scegliere virgolette singole (`'`), virgolette doppie (`"`) o backtick (`` ` ``) per racchiudere le vostre stringhe. Tutti i seguenti funzioneranno:

```js-nolint
const single = 'Single quotes';
const double = "Double quotes";
const backtick = `Backtick`;

console.log(single);
console.log(double);
console.log(backtick);
```

È necessario utilizzare lo stesso carattere per l'inizio e la fine di una stringa, altrimenti verrà generato un errore:

```js-nolint example-bad
const badQuotes = 'This is not allowed!";
```

Le stringhe dichiarate utilizzando virgolette singole e quelle dichiarate utilizzando virgolette doppie sono le stesse, e quale si utilizza dipende dalla preferenza personale — anche se è buona pratica scegliere uno stile e usarlo in modo coerente nel codice.

Le stringhe dichiarate utilizzando backtick sono un tipo speciale di stringa chiamato [_letterale template_](/it/docs/Web/JavaScript/Reference/Template_literals). Nella maggior parte dei modi, i letterali template sono come le stringhe normali, ma hanno alcune proprietà speciali:

- potete [incorporare JavaScript](#incorporare_javascript) in essi
- potete dichiarare letterali template su [più linee](#stringhe_multilinea)

## Incorporare JavaScript

All'interno di un letterale template, potete racchiudere variabili JavaScript o espressioni all'interno di `${ }`, e il risultato sarà incluso nella stringa:

```js
const name = "Chris";
const greeting = `Hello, ${name}`;
console.log(greeting); // "Hello, Chris"
```

Potete utilizzare la stessa tecnica per unire due variabili:

```js
const one = "Hello, ";
const two = "how are you?";
const joined = `${one}${two}`;
console.log(joined); // "Hello, how are you?"
```

Unire le stringhe in questo modo è chiamato _concatenazione_.

### Concatenazione nel contesto

Diamo un'occhiata alla concatenazione usata in azione:

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

Qui, stiamo utilizzando la funzione [`window.prompt()`](/it/docs/Web/API/Window/prompt), che chiede all'utente di rispondere a una domanda tramite una finestra di dialogo popup e poi memorizza il testo inserito all'interno di una variabile data — in questo caso `name`. Successivamente, visualizziamo una stringa che inserisce il nome in un messaggio di saluto generico.

### Concatenazione usando "+"

Potete usare `${}` solo con i letterali template, non con le stringhe normali. Potete concatenare le stringhe normali utilizzando l'operatore `+`:

```js
const greeting = "Hello";
const name = "Chris";
console.log(greeting + ", " + name); // "Hello, Chris"
```

Tuttavia, i letterali template solitamente offrono codice più leggibile:

```js
const greeting = "Hello";
const name = "Chris";
console.log(`${greeting}, ${name}`); // "Hello, Chris"
```

### Inclusione di espressioni nelle stringhe

Potete includere espressioni JavaScript nei letterali template, oltre alle sole variabili, e i risultati verranno inclusi nel risultato:

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

I letterali template rispettano le interruzioni di riga nel codice sorgente, quindi potete scrivere stringhe che si estendono su più linee in questo modo:

```js
const newline = `One day you finally knew
what you had to do, and began,`;
console.log(newline);

/*
One day you finally knew
what you had to do, and began,
*/
```

Per ottenere un'uscita equivalente utilizzando una stringa normale, dovreste includere caratteri di interruzione di riga (`\n`) nella stringa:

```js
const newline = "One day you finally knew\nwhat you had to do, and began,";
console.log(newline);

/*
One day you finally knew
what you had to do, and began,
*/
```

Consultate la nostra pagina di riferimento [Letterali template](/it/docs/Web/JavaScript/Reference/Template_literals) per ulteriori esempi e dettagli sulle funzionalità avanzate.

## Inclusione di virgolette nelle stringhe

Poiché utilizziamo le virgolette per indicare l'inizio e la fine delle stringhe, come possiamo includere virgolette reali nelle stringhe? Sappiamo che questo non funzionerà:

```js-nolint example-bad
const badQuotes = "She said "I think so!"";
```

Una opzione comune è utilizzare uno degli altri caratteri per dichiarare la stringa:

```js-nolint
const goodQuotes1 = 'She said "I think so!"';
const goodQuotes2 = `She said "I'm not going in there!"`;
```

Un'altra opzione è _fare l'escape_ del carattere di virgolette problematico. Fare l'escape dei caratteri significa che facciamo qualcosa per assicurarci che vengano riconosciuti come testo, non parte del codice. In JavaScript, si fa questo mettendo una barra inversa proprio davanti al carattere. Provate questo:

```js-nolint
const bigmouth = 'I\'ve got no right to take my place…';
console.log(bigmouth);
```

Potete utilizzare la stessa tecnica per inserire altri caratteri speciali. Consultate [Sequenze di escape](/it/docs/Web/JavaScript/Reference/Lexical_grammar#escape_sequences) per ulteriori dettagli.

## Numeri vs. stringhe

Cosa succede quando proviamo a concatenare una stringa e un numero? Proviamolo nella nostra console:

```js
const name = "Front ";
const number = 242;
console.log(name + number); // "Front 242"
```

Potreste aspettarvi che questo restituisca un errore, ma funziona perfettamente. Come i numeri dovrebbero essere visualizzati come stringhe è abbastanza ben definito, quindi il browser converte automaticamente il numero in una stringa e concatena le due stringhe.

Se avete una variabile numerica che volete convertire in una stringa o una variabile stringa che volete convertire in un numero, potete utilizzare le seguenti due costruzioni:

- La funzione {{jsxref("Number/Number", "Number()")}} converte in numero qualsiasi cosa le venga passata, se possibile. Provate quanto segue:

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

Queste costruzioni possono essere davvero utili in alcune situazioni. Ad esempio, se un utente inserisce un numero in un campo di testo di un modulo, è una stringa. Tuttavia, se desiderate aggiungere questo numero a qualcos'altro, avrete bisogno che sia un numero, quindi potreste farlo passare attraverso `Number()` per gestirlo. Abbiamo fatto esattamente questo nel nostro [Gioco di indovinello numerico](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game.html), nella funzione `checkGuess`.

## Sommario

Quindi, questi sono i principi di base delle stringhe trattati in JavaScript. Nel prossimo articolo, costruiremo su questo, esaminando alcuni dei metodi integrati disponibili per le stringhe in JavaScript e come possiamo utilizzarli per manipolare le nostre stringhe in just the form desiderata.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting")}}
