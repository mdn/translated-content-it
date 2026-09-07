---
title: Metodi utili per le stringhe
short-title: Metodi per le stringhe
slug: Learn_web_development/Core/Scripting/Useful_string_methods
l10n:
  sourceCommit: 003b6ceec6ecd0a3e36046a8515ab7fbc8dc220d
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting/Test_your_skills/Strings", "Learn_web_development/Core/Scripting")}}

Ora che sono state esaminate le basi essenziali delle stringhe, è il momento di passare al livello successivo e iniziare a pensare alle operazioni utili che si possono eseguire sulle stringhe con i metodi integrati, come trovare la lunghezza di una stringa di testo, unire e dividere stringhe, sostituire un carattere in una stringa con un altro e altro ancora.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>. Conoscenza delle <a href="/it/docs/Learn_web_development/Core/Scripting/Strings">basi delle stringhe</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
          Manipolazione delle stringhe tramite proprietà e metodi comuni integrati in JavaScript.
      </td>
    </tr>
  </tbody>
</table>

## Le stringhe come oggetti

In JavaScript, la maggior parte dei valori può essere utilizzata come se fosse un oggetto. Quando viene creata una stringa, ad esempio usando

```js
const string = "This is my string";
```

anche se la variabile stessa non è un oggetto, dispone comunque di un gran numero di proprietà e metodi, grazie al fatto che può essere utilizzata come oggetto durante l'accesso alle proprietà. Questo è visibile andando alla pagina dell'oggetto {{jsxref("String")}} e osservando l'elenco sul lato della pagina.

**Ora, prima che il cervello inizi a fondersi, niente paura!** Non è affatto necessario conoscere la maggior parte di queste cose nelle prime fasi del percorso di apprendimento. Tuttavia, ce ne sono alcune che potrebbero essere usate abbastanza spesso e che verranno esaminate qui.

Inseriamo alcuni esempi nella [console per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

## Trovare la lunghezza di una stringa

È semplice: si usa la proprietà {{jsxref("String.prototype.length", "length")}}. Provare a inserire le seguenti righe:

```js
const browserType = "mozilla";
browserType.length;
```

Il risultato dovrebbe essere il numero 7, perché "mozilla" è lunga 7 caratteri. Questo è utile per molti motivi; per esempio, si potrebbero voler trovare le lunghezze di una serie di nomi per visualizzarli in ordine di lunghezza, oppure informare un utente che il nome utente inserito in un campo di un modulo è troppo lungo se supera una certa lunghezza.

## Recuperare un carattere specifico di una stringa

In modo analogo, è possibile restituire qualsiasi carattere all'interno di una stringa utilizzando la **notazione con parentesi quadre**: ciò significa includere parentesi quadre (`[]`) alla fine del nome della variabile. All'interno delle parentesi quadre va inserito il numero del carattere da restituire; per esempio, per recuperare la prima lettera si farebbe così:

```js
browserType[0];
```

Ricordare: i computer contano a partire da 0, non da 1!

Per recuperare l'ultimo carattere di _qualsiasi_ stringa, si potrebbe usare la riga seguente, combinando questa tecnica con la proprietà `length` vista sopra:

```js
browserType[browserType.length - 1];
```

La lunghezza della stringa "mozilla" è 7, ma poiché il conteggio inizia da 0, la posizione dell'ultimo carattere è 6; usare `length-1` consente di ottenere l'ultimo carattere.

## Verificare se una stringa contiene una sottostringa

Talvolta è necessario scoprire se una stringa più piccola è presente all'interno di una più grande (in genere si dice _se una sottostringa è presente all'interno di una stringa_). Questo può essere fatto utilizzando il metodo {{jsxref("String.prototype.includes()", "includes()")}}, che accetta un solo {{Glossary("parameter", "parametro")}}: la sottostringa da cercare.

Restituisce `true` se la stringa contiene la sottostringa, altrimenti `false`.

```js
const browserType = "mozilla";

if (browserType.includes("zilla")) {
  console.log("Found zilla!");
} else {
  console.log("No zilla here!");
}
```

Spesso sarà necessario sapere se una stringa inizia o termina con una determinata sottostringa. Questa esigenza è abbastanza comune da avere due metodi specifici: {{jsxref("String.prototype.startsWith()", "startsWith()")}} e {{jsxref("String.prototype.endsWith()", "endsWith()")}}:

```js
const browserType = "mozilla";

if (browserType.startsWith("zilla")) {
  console.log("It starts with zilla!");
} else {
  console.log("It DOESN'T start with zilla!");
}
```

```js
const browserType = "mozilla";

if (browserType.endsWith("zilla")) {
  console.log("It ends with zilla!");
} else {
  console.log("It DOESN'T end with zilla!");
}
```

## Trovare la posizione di una sottostringa in una stringa

È possibile trovare la posizione di una sottostringa all'interno di una stringa più grande utilizzando il metodo {{jsxref("String.prototype.indexOf()", "indexOf()")}}. Questo metodo accetta due {{Glossary("parameter", "parametri")}}: la sottostringa da cercare e un parametro facoltativo che specifica il punto iniziale della ricerca.

Se la stringa contiene la sottostringa, `indexOf()` restituisce l'indice della prima occorrenza della sottostringa. Se la stringa non contiene la sottostringa, `indexOf()` restituisce `-1`.

```js
const tagline = "MDN - Resources for developers, by developers";
console.log(tagline.indexOf("developers")); // 20
```

Partendo da `0`, contando il numero di caratteri (compresi gli spazi) dall'inizio della stringa, la prima occorrenza della sottostringa `"developers"` si trova all'indice `20`.

```js
console.log(tagline.indexOf("x")); // -1
```

Questo, invece, restituisce `-1` perché il carattere `x` non è presente nella stringa.

Ora che è noto come trovare la prima occorrenza di una sottostringa, come si trovano le occorrenze successive? È possibile farlo passando come secondo parametro del metodo un valore maggiore dell'indice dell'occorrenza precedente.

```js
const firstOccurrence = tagline.indexOf("developers");
const secondOccurrence = tagline.indexOf("developers", firstOccurrence + 1);

console.log(firstOccurrence); // 20
console.log(secondOccurrence); // 35
```

Qui viene indicato al metodo di cercare la sottostringa `"developers"` a partire dall'indice `21` (`firstOccurrence + 1`) e viene restituito l'indice `35`.

## Estrarre una sottostringa da una stringa

È possibile estrarre una sottostringa da una stringa utilizzando il metodo {{jsxref("String.prototype.slice()", "slice()")}}. A questo metodo vengono passati:

- l'indice dal quale iniziare l'estrazione;
- l'indice al quale interrompere l'estrazione. Questo è esclusivo, il che significa che il carattere a questo indice non viene incluso nella sottostringa estratta.

Per esempio:

```js
const browserType = "mozilla";
console.log(browserType.slice(1, 4)); // "ozi"
```

Il carattere all'indice `1` è `"o"` e il carattere all'indice 4 è `"l"`. Vengono quindi estratti tutti i caratteri a partire da `"o"` e fino a prima di `"l"`, ottenendo `"ozi"`.

Se si sa di voler estrarre tutti i caratteri rimanenti in una stringa dopo un determinato carattere, non è necessario includere il secondo parametro. È invece sufficiente includere la posizione del carattere dal quale estrarre i caratteri rimanenti della stringa. Provare quanto segue:

```js
browserType.slice(2); // "zilla"
```

Questo restituisce `"zilla"`: ciò avviene perché la posizione del carattere 2 è la lettera `"z"` e, poiché non è stato incluso un secondo parametro, la sottostringa restituita contiene tutti i caratteri rimanenti nella stringa.

> [!NOTE]
> `slice()` dispone anche di altre opzioni; consultare la pagina {{jsxref("String.prototype.slice()", "slice()")}} per scoprire cos'altro è possibile fare.

## Modificare le maiuscole e le minuscole

I metodi per stringhe {{jsxref("String.prototype.toLowerCase()", "toLowerCase()")}} e {{jsxref("String.prototype.toUpperCase()", "toUpperCase()")}} ricevono una stringa e convertono rispettivamente tutti i caratteri in minuscolo o maiuscolo. Questo può essere utile, per esempio, quando si desidera normalizzare tutti i dati inseriti dagli utenti prima di memorizzarli in un database.

Provare a inserire le righe seguenti per vedere cosa accade:

```js
const radData = "My NaMe Is MuD";
console.log(radData.toLowerCase());
console.log(radData.toUpperCase());
```

## Aggiornare parti di una stringa

È possibile sostituire una sottostringa all'interno di una stringa con un'altra sottostringa usando il metodo {{jsxref("String.prototype.replace()", "replace()")}}.

In questo esempio vengono forniti due parametri: la stringa da sostituire e la stringa con cui sostituirla:

```js
const browserType = "mozilla";
const updated = browserType.replace("moz", "van");

console.log(updated); // "vanilla"
console.log(browserType); // "mozilla"
```

Notare che `replace()`, come molti metodi per stringhe, non modifica la stringa sulla quale è stato chiamato, ma restituisce una nuova stringa. Per aggiornare la variabile originale `browserType`, occorrerebbe fare qualcosa di simile:

```js
let browserType = "mozilla";
browserType = browserType.replace("moz", "van");

console.log(browserType); // "vanilla"
```

Notare inoltre che ora `browserType` deve essere dichiarata usando `let`, non `const`, perché viene riassegnata.

Tenere presente che `replace()` in questa forma modifica soltanto la prima occorrenza della sottostringa. Per modificare tutte le occorrenze, è possibile usare {{jsxref("String.prototype.replaceAll()", "replaceAll()")}}:

```js
let quote = "To be or not to be";
quote = quote.replaceAll("be", "code");

console.log(quote); // "To code or not to code"
```

## Sfide di apprendimento

In questa sezione è possibile cimentarsi nella scrittura di codice per la manipolazione delle stringhe. In ogni esercizio seguente è presente un array di stringhe e un ciclo che elabora ogni valore dell'array e lo visualizza in un elenco puntato. Non è necessario comprendere gli array o i cicli in questo momento: saranno spiegati in articoli futuri. In ogni caso, basta scrivere il codice che produrrà le stringhe nel formato desiderato.

Aprire ogni esempio nell'MDN Playground usando il pulsante **"Play"** nella parte superiore dell'esempio interattivo, quindi seguire le istruzioni per risolvere il problema. Se si rimane bloccati, è possibile visualizzare le soluzioni sotto l'esempio interattivo in ogni caso.

È possibile usare il pulsante "Reset" nell'MDN Playground per ripristinare il codice se viene commesso un errore e non si riesce più a farlo funzionare.

### Filtrare i messaggi di auguri

Nel primo esercizio si inizierà in modo semplice: è presente un array di messaggi per biglietti di auguri, ma si desidera ordinarli per elencare soltanto i messaggi natalizi. Occorre completare un test condizionale nella struttura `if ()` per verificare ogni stringa e stamparla nell'elenco solo se è un messaggio natalizio.

Riflettere su come verificare se il messaggio in ogni caso è un messaggio natalizio. Quale stringa è presente in tutti quei messaggi e quale metodo può essere usato per verificare se è presente?

```html hidden live-sample___string-methods-1
<ul></ul>
```

```js live-sample___string-methods-1
const list = document.querySelector("ul");
const greetings = [
  "Happy Birthday!",
  "Merry Christmas my love",
  "A happy Christmas to all the family",
  "You're all I want for Christmas",
  "Get well soon",
];

for (const greeting of greetings) {
  // Your conditional test needs to go inside the parentheses
  // in the line below, replacing what's currently there
  if (greeting) {
    const listItem = document.createElement("li");
    listItem.textContent = greeting;
    list.appendChild(listItem);
  }
}
```

{{ EmbedLiveSample("string-methods-1", "100%", 150) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere questo aspetto:

```js
const list = document.querySelector("ul");
const greetings = [
  "Happy Birthday!",
  "Merry Christmas my love",
  "A happy Christmas to all the family",
  "You're all I want for Christmas",
  "Get well soon",
];

for (const greeting of greetings) {
  if (greeting.includes("Christmas")) {
    const listItem = document.createElement("li");
    listItem.textContent = greeting;
    list.appendChild(listItem);
  }
}
```

</details>

### Correggere le maiuscole

Questo esercizio contiene i nomi di città del Regno Unito, ma l'uso delle maiuscole è completamente errato. Occorre modificarli affinché siano tutti in minuscolo, tranne la prima lettera maiuscola. Un buon modo per farlo è:

1. Convertire in minuscolo l'intera stringa contenuta nella variabile `city` e memorizzarla in una nuova variabile.
2. Recuperare la prima lettera della stringa nella nuova variabile e memorizzarla in un'altra variabile.
3. Usando quest'ultima variabile come sottostringa, sostituire la prima lettera della stringa in minuscolo con la prima lettera della stringa in minuscolo convertita in maiuscolo. Memorizzare il risultato di questa procedura di sostituzione in un'altra nuova variabile.
4. Modificare il valore della variabile `result` affinché sia uguale al risultato finale, non a `city`.

> [!NOTE]
> Un suggerimento: i parametri dei metodi per stringhe non devono essere necessariamente letterali stringa; possono anche essere variabili, o persino variabili sulle quali viene chiamato un metodo.

```html hidden live-sample___string-methods-2
<ul></ul>
```

```js live-sample___string-methods-2
const list = document.querySelector("ul");
const cities = ["lonDon", "ManCHESTer", "BiRmiNGHAM", "liVERpoOL"];

for (const city of cities) {
  // write your code just below here

  const result = city;
  const listItem = document.createElement("li");
  listItem.textContent = result;
  list.appendChild(listItem);
}
```

{{ EmbedLiveSample("string-methods-2", "100%", 150) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere questo aspetto:

```js
const list = document.querySelector("ul");
const cities = ["lonDon", "ManCHESTer", "BiRmiNGHAM", "liVERpoOL"];

for (const city of cities) {
  const lower = city.toLowerCase();
  const firstLetter = lower.slice(0, 1);
  const capitalized = lower.replace(firstLetter, firstLetter.toUpperCase());
  const result = capitalized;
  const listItem = document.createElement("li");
  listItem.textContent = result;
  list.appendChild(listItem);
}
```

</details>

### Creare nuove stringhe da parti precedenti

In quest'ultimo esercizio, l'array contiene stringhe di informazioni sulle stazioni ferroviarie del Nord dell'Inghilterra. Le stringhe sono elementi di dati che contengono il codice di stazione di tre lettere, seguito da alcuni dati leggibili da una macchina, seguito da un punto e virgola, seguito dal nome della stazione leggibile dalle persone. Per esempio:

```plain
MAN675847583748sjt567654;Manchester Piccadilly
```

Si desidera estrarre il codice e il nome della stazione e unirli in una stringa con la struttura seguente:

```plain
MAN: Manchester Piccadilly
```

Si consiglia di procedere in questo modo:

1. Estrarre il codice di stazione di tre lettere e memorizzarlo in una nuova variabile.
2. Trovare il numero di indice del carattere punto e virgola.
3. Estrarre il nome della stazione leggibile dalle persone usando il numero di indice del carattere punto e virgola come punto di riferimento e memorizzarlo in una nuova variabile.
4. Concatenare le due nuove variabili e un letterale stringa per creare la stringa finale.
5. Modificare il valore della variabile `result` affinché sia la stringa finale, non `station`.

```html hidden live-sample___string-methods-3
<ul></ul>
```

```js live-sample___string-methods-3
const list = document.querySelector("ul");
const stations = [
  "MAN675847583748sjt567654;Manchester Piccadilly",
  "GNF576746573fhdg4737dh4;Greenfield",
  "LIV5hg65hd737456236dch46dg4;Liverpool Lime Street",
  "SYB4f65hf75f736463;Stalybridge",
  "HUD5767ghtyfyr4536dh45dg45dg3;Huddersfield",
];

for (const station of stations) {
  // write your code just below here

  const result = station;
  const listItem = document.createElement("li");
  listItem.textContent = result;
  list.appendChild(listItem);
}
```

{{ EmbedLiveSample("string-methods-3", "100%", 150) }}

<details>
<summary>Fare clic qui per mostrare la soluzione</summary>

Il JavaScript completato dovrebbe avere questo aspetto:

```js
const list = document.querySelector("ul");
const stations = [
  "MAN675847583748sjt567654;Manchester Piccadilly",
  "GNF576746573fhdg4737dh4;Greenfield",
  "LIV5hg65hd737456236dch46dg4;Liverpool Lime Street",
  "SYB4f65hf75f736463;Stalybridge",
  "HUD5767ghtyfyr4536dh45dg45dg3;Huddersfield",
];

for (const station of stations) {
  const code = station.slice(0, 3);
  const semiColonIndex = station.indexOf(";");
  const name = station.slice(semiColonIndex + 1);
  const result = `${code}: ${name}`;
  const listItem = document.createElement("li");
  listItem.textContent = result;
  list.appendChild(listItem);
}
```

</details>

## Riepilogo

Non si può evitare il fatto che la capacità di gestire parole e frasi nella programmazione sia molto importante, in particolare in JavaScript, dato che i siti web riguardano la comunicazione con le persone. Questo articolo ha fornito le basi necessarie, per ora, sulla manipolazione delle stringhe. Queste conoscenze saranno utili quando si affronteranno argomenti più complessi in futuro.

Nel prossimo articolo verranno proposti alcuni test per verificare quanto bene sono state comprese e memorizzate le informazioni fornite sulle stringhe e sui metodi per stringhe.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting/Test_your_skills/Strings", "Learn_web_development/Core/Scripting")}}
