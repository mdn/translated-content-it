---
title: Gestire il testo — le stringhe in JavaScript
short-title: Strings
slug: Learn_web_development/Core/Scripting/Strings
l10n:
  sourceCommit: fdfe2889acd02623047231e39c9dee5df1bd646e
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Math", "Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting")}}

Ora l'attenzione si sposta sulle stringhe: questo è il nome con cui vengono chiamati i blocchi di testo nella programmazione. In questo articolo verranno esaminate tutte le nozioni comuni che è davvero necessario conoscere sulle stringhe quando si impara JavaScript, come creare stringhe, effettuare l'escape delle virgolette nelle stringhe e unire stringhe.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Creazione di letterali stringa.</li>
          <li>La necessità che le virgolette corrispondano.</li>
          <li>Concatenazione di stringhe.</li>
          <li>Escape dei caratteri nelle stringhe.</li>
          <li>Template literal, incluso l'uso di variabili e template literal su più righe.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Il potere delle parole

Le parole sono molto importanti per gli esseri umani: rappresentano una grande parte del modo in cui comunichiamo. Poiché il web è un mezzo basato in larga misura sul testo, progettato per consentire alle persone di comunicare e condividere informazioni, è utile poter controllare le parole che vi appaiono. {{Glossary("HTML", "HTML")}} fornisce struttura e significato al testo, {{Glossary("CSS", "CSS")}} consente di applicare stili precisi e JavaScript offre molte funzionalità per manipolare le stringhe. Queste includono la creazione di messaggi di benvenuto e richieste personalizzati, la visualizzazione delle etichette di testo corrette quando necessario, l'ordinamento dei termini nell'ordine desiderato e molto altro.

Quasi tutti i programmi mostrati finora nel corso hanno comportato qualche manipolazione delle stringhe.

## Dichiarare stringhe

A prima vista, le stringhe vengono gestite in modo simile ai numeri, ma osservando più in profondità inizieranno a emergere alcune differenze rilevanti. Iniziamo inserendo alcune righe di base nella [console per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per familiarizzare con esse.

Per iniziare, inserire le righe seguenti:

```js
const string = "The revolution will not be televised.";
console.log(string);
```

Come è stato fatto con i numeri, viene dichiarata una variabile, inizializzata con un valore stringa e quindi ne viene restituito il valore. L'unica differenza è che, quando si scrive una stringa, è necessario racchiudere il valore tra virgolette.

Se ciò non viene fatto, o se manca una delle virgolette, si otterrà un errore. Provare a inserire le righe seguenti:

```js example-bad
const badString1 = This is a test;
const badString2 = 'This is a test;
const badString3 = This is a test';
```

Queste righe non funzionano perché qualsiasi testo senza virgolette intorno viene interpretato come nome di variabile, nome di proprietà, parola riservata o elemento simile. Se il browser non riconosce il testo senza virgolette, viene generato un errore, ad esempio "missing; before statement". Se il browser riesce a rilevare dove inizia una stringa ma non dove termina, a causa della seconda virgoletta mancante, potrebbe segnalare un errore "unterminated string literal" oppure, nella console, passare a una nuova riga e attendere che la stringa venga completata. Se il programma genera tali errori, tornare indietro e controllare tutte le stringhe per assicurarsi che non manchino virgolette.

Quanto segue funzionerà se la variabile `string` è stata definita in precedenza: provarlo ora:

```js
const badString = string;
console.log(badString);
```

Ora `badString` è impostata per avere lo stesso valore di `string`.

### Virgolette singole, doppie virgolette e backtick

In JavaScript, è possibile scegliere tra virgolette singole (`'`), doppie virgolette (`"`) o backtick (`` ` ``) per racchiudere le stringhe. Tutti gli esempi seguenti funzioneranno:

```js-nolint
const single = 'Single quotes';
const double = "Double quotes";
const backtick = `Backtick`;

console.log(single);
console.log(double);
console.log(backtick);
```

È necessario usare lo stesso carattere per l'inizio e la fine di una stringa, altrimenti si otterrà un errore:

```js-nolint example-bad
const badQuotes = 'This is not allowed!";
```

Le stringhe dichiarate usando virgolette singole e quelle dichiarate usando doppie virgolette sono equivalenti; la scelta dipende dalle preferenze personali, anche se è buona pratica scegliere uno stile e utilizzarlo in modo coerente nel codice.

Le stringhe dichiarate usando i backtick sono un tipo speciale di stringa chiamato [_template literal_](/it/docs/Web/JavaScript/Reference/Template_literals). I template literal si comportano per lo più come stringhe normali, ma hanno alcune proprietà speciali:

- È possibile [incorporare JavaScript](#incorporare_javascript) al loro interno.
- È possibile dichiarare template literal su [più righe](#stringhe_su_più_righe).

## Incorporare JavaScript

All'interno di un template literal, è possibile racchiudere variabili o espressioni JavaScript in `${ }` e il risultato verrà incluso nella stringa:

```js
const name = "Chris";
const greeting = `Hello, ${name}`;
console.log(greeting); // "Hello, Chris"
```

È possibile usare la stessa tecnica per unire due variabili:

```js
const one = "Hello, ";
const two = "how are you?";
const joined = `${one}${two}`;
console.log(joined); // "Hello, how are you?"
```

L'unione di stringhe in questo modo è chiamata _concatenazione_.

### Concatenazione nel contesto

Vediamo la concatenazione in azione:

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

Qui viene utilizzata la funzione [`window.prompt()`](/it/docs/Web/API/Window/prompt), che chiede all'utente di rispondere a una domanda tramite una finestra di dialogo popup e quindi memorizza il testo inserito in una determinata variabile, in questo caso `name`. Viene quindi visualizzata una stringa che inserisce il nome in un messaggio di saluto generico.

### Concatenazione con "+"

È possibile usare `${}` solo con i template literal, non con le stringhe normali. È possibile concatenare stringhe normali usando l'operatore `+`:

```js
const greeting2 = "Hello";
const name2 = "Bob";
console.log(greeting2 + ", " + name2); // "Hello, Bob"
```

Tuttavia, i template literal solitamente forniscono codice più leggibile:

```js
const greeting3 = "Howdy";
const name3 = "Ramesh";
console.log(`${greeting3}, ${name3}`); // "Howdy, Ramesh"
```

### Includere espressioni nelle stringhe

Nei template literal è possibile includere espressioni JavaScript, oltre alle sole variabili, e i risultati verranno inclusi nel risultato:

```js
const song = "Fight the Youth";
const score = 9;
const highestScore = 10;
const output = `I like the song ${song}. I gave it a score of ${
  (score / highestScore) * 100
}%.`;
console.log(output); // "I like the song Fight the Youth. I gave it a score of 90%."
```

## Stringhe su più righe

I template literal rispettano le interruzioni di riga nel codice sorgente, quindi è possibile scrivere stringhe che si estendono su più righe in questo modo:

```js
const newline = `One day you finally knew
what you had to do, and began,`;
console.log(newline);

/*
One day you finally knew
what you had to do, and began,
*/
```

Per ottenere un output equivalente usando una stringa normale, sarebbe necessario includere caratteri di interruzione di riga (`\n`) nella stringa:

```js
const newline2 = "One day you finally knew\nwhat you had to do, and began,";
console.log(newline2);

/*
One day you finally knew
what you had to do, and began,
*/
```

Consultare la pagina di riferimento sui [Template literal](/it/docs/Web/JavaScript/Reference/Template_literals) per ulteriori esempi e dettagli sulle funzionalità avanzate.

## Includere virgolette nelle stringhe

Poiché le virgolette vengono usate per indicare l'inizio e la fine delle stringhe, come è possibile includere virgolette effettive nelle stringhe? È noto che questo non funzionerà:

```js-nolint example-bad
const badQuotes = "She said "I think so!"";
```

Un'opzione comune consiste nell'usare uno degli altri caratteri per dichiarare la stringa:

```js-nolint
const goodQuotes1 = 'She said "I think so!"';
const goodQuotes2 = `She said "I'm not going in there!"`;
```

Un'altra opzione consiste nell'effettuare l'_escape_ della virgoletta problematica. Effettuare l'escape dei caratteri significa fare qualcosa per assicurarsi che vengano riconosciuti come testo, non come parte del codice. In JavaScript, ciò avviene inserendo una barra rovesciata immediatamente prima del carattere. Provare questo:

```js-nolint
const bigmouth = 'I\'ve got no right to take my place…';
console.log(bigmouth);
```

È possibile usare la stessa tecnica per inserire altri caratteri speciali. Consultare le [sequenze di escape](/it/docs/Web/JavaScript/Reference/Lexical_grammar#escape_sequences) per ulteriori dettagli.

## Numeri e stringhe

Cosa accade quando si tenta di concatenare una stringa e un numero? Proviamo nella console:

```js
const coolBandName = "Front ";
const number = 242;
console.log(coolBandName + number); // "Front 242"
```

Ci si potrebbe aspettare che restituisca un errore, ma funziona senza problemi. Il modo in cui i numeri devono essere visualizzati come stringhe è definito in modo piuttosto chiaro, quindi il browser converte automaticamente il numero in una stringa e concatena le due stringhe.

Se è presente una variabile numerica da convertire in stringa oppure una variabile stringa da convertire in numero, è possibile usare i due costrutti seguenti:

- La funzione {{jsxref("Number/Number", "Number()")}} converte in un numero qualsiasi elemento le venga passato, se possibile. Provare quanto segue:

  ```js
  const myString = "123";
  const myNum = Number(myString);
  console.log(typeof myNum);
  // number
  ```

- Viceversa, la funzione {{jsxref("String/String", "String()")}} converte il proprio argomento in una stringa. Provare questo:

  ```js
  const myNum2 = 123;
  const myString2 = String(myNum2);
  console.log(typeof myString2);
  // string
  ```

Questi costrutti possono essere davvero utili in alcune situazioni. Ad esempio, se un utente inserisce un numero nel campo di testo di un modulo, questo è una stringa. Tuttavia, se si desidera aggiungere questo numero a qualcos'altro, sarà necessario che sia un numero, quindi può essere passato a `Number()` per gestirlo. È esattamente ciò che è stato fatto nel nostro [gioco di indovinare il numero](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game.html), nella funzione `checkGuess`.

## Riepilogo

Queste sono dunque le basi delle stringhe in JavaScript. Nel prossimo articolo si approfondirà l'argomento, esaminando alcuni dei metodi integrati disponibili per le stringhe in JavaScript e il modo in cui possono essere usati per manipolare le stringhe fino a ottenere esattamente la forma desiderata.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Math", "Learn_web_development/Core/Scripting/Useful_string_methods", "Learn_web_development/Core/Scripting")}}
