---
title: Memorizzare le informazioni necessarie — Variabili
short-title: Variables
slug: Learn_web_development/Core/Scripting/Variables
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting")}}

Dopo aver letto gli ultimi articoli, dovreste ormai sapere cos'è JavaScript, cosa può fare per voi, come usarlo insieme ad altre tecnologie web e quale aspetto hanno le sue principali caratteristiche a un livello elevato. In questo articolo, andremo ai veri fondamentali, guardando come lavorare con i blocchi costitutivi più basilari di JavaScript — le Variabili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono le variabili e perché sono così importanti.</li>
          <li>Dichiarare variabili con <code>let</code>, inizializzarle con valori e riattribuire nuovi valori.</li>
          <li>Creare costanti con <code>const</code>.</li>
          <li>La differenza tra variabili e costanti, e quando usare ciascuna.</li>
          <li>Le migliori pratiche per la denominazione delle variabili.</li>
          <li>I diversi tipi di valore che possono essere memorizzati nelle variabili — stringhe, numeri, booleani, array e oggetti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti necessari

Durante l'articolo, vi verrà chiesto di digitare righe di codice per testare la vostra comprensione del contenuto. Se state usando un browser desktop, il posto migliore per digitare il vostro codice di esempio è la console JavaScript del vostro browser (vedere [Cosa sono gli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per maggiori informazioni su come accedere a questo strumento).

## Cos'è una variabile?

Una variabile è un contenitore per un valore, come un numero che potremmo usare in un calcolo o una stringa che potremmo usare come parte di una frase.

### Esempio di variabile

Osserviamo un esempio:

```html
<button id="button_A">Press me</button>
<h3 id="heading_A"></h3>
```

```js
const buttonA = document.querySelector("#button_A");
const headingA = document.querySelector("#heading_A");

let count = 1;

buttonA.onclick = () => {
  buttonA.textContent = "Try again!";
  headingA.textContent = `${count} clicks so far`;
  count += 1;
};
```

{{ EmbedLiveSample('Variable_example', '100%', 120) }}

In questo esempio, premendo il pulsante si esegue del codice. Prima cambia il testo sul pulsante stesso. Secondo, mostra un messaggio con il numero di volte in cui il pulsante è stato premuto. Il numero è memorizzato in una variabile. Ogni volta che l'utente preme il pulsante, il numero nella variabile aumenta di uno.

### Senza una variabile

Per capire perché questo è così utile, pensiamo a come scriveremmo questo esempio senza usare una variabile per memorizzare il conteggio. Finirebbe per apparire qualcosa di simile a questo:

```html example-bad
<button id="button_B">Press me</button>
<h3 id="heading_B"></h3>
```

```js example-bad
const buttonB = document.querySelector("#button_B");
const headingB = document.querySelector("#heading_B");

buttonB.onclick = () => {
  buttonB.textContent = "Try again!";
  headingB.textContent = "1 click so far";
};
```

{{ EmbedLiveSample('Without_a_variable', '100%', 120) }}

Potrebbe non essere ancora del tutto chiara la sintassi che stiamo usando, ma dovreste riuscire a cogliere l'idea. Senza una variabile, non abbiamo modo di sapere quante volte il pulsante è stato cliccato. Il messaggio per l'utente diventerà rapidamente irrilevante quando non si può ricordare alcuna informazione.

Le variabili hanno semplicemente senso, e man mano che si impara JavaScript, inizieranno a diventare una seconda natura.

Una cosa speciale delle variabili è che possono contenere praticamente qualsiasi cosa — non solo stringhe e numeri. Le variabili possono anche contenere dati complessi e persino funzioni intere per fare cose straordinarie. Imparerete di più su questo man mano che andate avanti.

> [!NOTE]
> Diciamo che le variabili contengono valori. Questa è una distinzione importante da fare. Le variabili non sono i valori stessi; sono contenitori per i valori. Potete pensarle come piccole scatole di cartone in cui potete riporre le cose.

![Uno screenshot di tre scatole tridimensionali di cartone che dimostrano esempi di variabili JavaScript. Ogni scatola contiene valori ipotetici che rappresentano vari tipi di dati JavaScript. I valori di esempio sono "Bob", true e 35 rispettivamente.](boxes.png)

## Dichiarare una variabile

Per usare una variabile, prima dovete crearla — più precisamente, chiamiamo questo processo dichiarare la variabile. Per farlo, digitiamo la parola chiave `let` seguita dal nome che volete dare alla vostra variabile:

```js
let myName;
let myAge;
```

Qui stiamo creando due variabili chiamate `myName` e `myAge`. Provate a digitare queste righe nella console del vostro browser web. Dopodiché, provate a creare una variabile (o due) con nomi a vostra scelta.

> [!NOTE]
> In JavaScript, tutte le istruzioni di codice dovrebbero terminare con un punto e virgola (`;`) — il vostro codice potrebbe funzionare correttamente per linee singole, ma probabilmente non funzionerà quando state scrivendo più righe di codice assieme. Cercate di abituarvi a includerlo.

Potete verificare se questi valori esistono ora nell'ambiente di esecuzione digitando solo il nome della variabile, ad esempio

```js
myName;
myAge;
```

Attualmente non hanno valore; sono contenitori vuoti. Quando inserite i nomi delle variabili, dovrebbe essere restituito un valore di `undefined`. Se non esistono, riceverete un messaggio di errore — provate a digitare

```js
scoobyDoo;
```

> [!NOTE]
> Non confondete una variabile che esiste ma non ha un valore definito con una variabile che non esiste affatto — sono cose molto diverse. Nella analogia della scatola vista sopra, non esistere significherebbe che non c'è alcuna scatola (variabile) per contenere un valore. Nessun valore definito significherebbe che c'è una scatola, ma non ha alcun valore al suo interno.

## Inizializzare una variabile

Una volta dichiarata una variabile, potete inizializzarla con un valore. Lo fate digitando il nome della variabile, seguito da un segno di uguale (`=`), seguito dal valore che volete assegnarle. Ad esempio:

```js
myName = "Chris";
myAge = 37;
```

Provate a tornare alla console ora e digitare queste righe. Dovreste vedere il valore assegnato alla variabile ritornato nella console per confermarlo, in ogni caso. Ancora, potete restituire i vostri valori variabili digitando i loro nomi nella console — provate di nuovo questi:

```js
myName;
myAge;
```

Potete dichiarare e inizializzare una variabile allo stesso tempo, in questo modo:

```js
let myDog = "Rover";
```

Questo è probabilmente ciò che farete la maggior parte delle volte, poiché è più veloce che fare le due azioni su due righe separate.

## Una nota su var

Vi capiterà probabilmente di vedere anche un altro modo per dichiarare le variabili, usando la parola chiave `var`:

```js
var myName;
var myAge;
```

Quando JavaScript fu creato per la prima volta, questo era l'unico modo per dichiarare variabili. Il design di `var` è confuso e incline agli errori. Così `let` è stato creato nelle versioni moderne di JavaScript, una nuova parola chiave per creare variabili che funziona in modo leggermente diverso rispetto a `var`, risolvendo i suoi problemi nel processo.

Di seguito vengono spiegate alcune semplici differenze. Non andremo ora in tutte le differenze, ma inizierete a scoprirle man mano che imparerete di più su JavaScript (se volete davvero leggerle ora, sentitevi liberi di consultare la nostra [pagina di riferimento di let](/it/docs/Web/JavaScript/Reference/Statements/let)).

Per cominciare, se scrivete un programma JavaScript su più linee che dichiara e inizializza una variabile, potete effettivamente dichiarare una variabile con `var` dopo averla inizializzata e funzionerà comunque. Ad esempio:

```js
myName = "Chris";

function logName() {
  console.log(myName);
}

logName();

var myName;
```

> [!NOTE]
> Questo non funzionerà digitando singole righe in una console JavaScript, solo quando si eseguono più righe di JavaScript in un documento web.

Questo funziona a causa del **hoisting** — leggete [hoisting di var](/it/docs/Web/JavaScript/Reference/Statements/var#hoisting) per maggiori dettagli sull'argomento.

L'hoisting non funziona più con `let`. Se sostituiamo `var` con `let` nell'esempio sopra, fallirà con un errore. Questo è un bene — dichiarare una variabile dopo averla inizializzata porta a un codice confuso e più difficile da capire.

In secondo luogo, quando usate `var`, potete dichiarare la stessa variabile quante volte volete, ma con `let` non potete. Il seguente funzionerebbe:

```js
var myName = "Chris";
var myName = "Bob";
```

Ma il seguente provocherebbe un errore alla seconda linea:

```js example-bad
let myName = "Chris";
let myName = "Bob";
```

Dovreste invece fare questo:

```js
let myName = "Chris";
myName = "Bob";
```

Ancora, questa è una decisione linguistica sensata. Non c'è motivo di ridichiarare variabili — rende solo le cose più confuse.

Per questi motivi e altri, raccomandiamo di usare `let` nel vostro codice, piuttosto che `var`. A meno che non stiate esplicitamente scrivendo supporto per browser molto vecchi, non c'è più alcun motivo per usare `var`, poiché tutti i browser moderni supportano `let` dal 2015.

> [!NOTE]
> Se state provando questo codice nella console del vostro browser, preferite copiare e incollare ciascuno dei blocchi di codice qui come un intero. C'è una [funzione nella console di Chrome](https://docs.google.com/document/d/1NP_FnHr4WCZRp7exgUklvNiXrH3nujcfwvp2pzMQ8-0/edit#heading=h.7y5hynxk52e9) dove le dichiarazioni di variabili con `let` e `const` sono permesse:
>
> ```plain
> > let myName = "Chris";
>   let myName = "Bob";
> // Come un input: SyntaxError: Identifier 'myName' has already been declared
>
> > let myName = "Chris";
> > let myName = "Bob";
> // Come due input: entrambi riescono
> ```

## Aggiornare una variabile

Una volta che una variabile è stata inizializzata con un valore, potete cambiare (o aggiornare) quel valore assegnandole un valore differente. Provate a inserire le seguenti righe nella console:

```js
myName = "Bob";
myAge = 40;
```

### Un inciso sulle regole di denominazione delle variabili

Potete chiamare una variabile praticamente come volete, ma ci sono delle limitazioni. In generale, dovreste attenermi a usare solo caratteri latini (0-9, a-z, A-Z) e il carattere di sottolineatura.

- Non dovreste usare altri caratteri perché potrebbero causare errori o essere difficili da capire per un pubblico internazionale.
- Non usate sottolineature all'inizio dei nomi delle variabili — questo è usato in alcuni costrutti JavaScript per significare cose specifiche, quindi potrebbe creare confusione.
- Non usate numeri all'inizio delle variabili. Questo non è permesso e causa un errore.
- Una convenzione sicura da seguire è il {{Glossary("camel_case", "lower camel case")}}, dove concatenate più parole, usando il minuscolo per l'intera prima parola e poi maiuscole per le parole successive. Abbiamo utilizzato questo metodo per i nomi delle variabili nell'articolo finora.
- Fate sì che i nomi delle variabili siano intuitivi, in modo che descrivano i dati che contengono. Non usate solo lettere o numeri singole, o frasi lunghe.
- Le variabili sono sensibili alle maiuscole e minuscole — quindi `myage` è una variabile diversa da `myAge`.
- Un ultimo punto: è anche necessario evitare di usare parole riservate di JavaScript come nomi delle variabili — con ciò intendiamo le parole che compongono la sintassi effettiva di JavaScript! Quindi, non potete usare parole come `var`, `function`, `let`, e `for` come nomi delle variabili. I browser le riconoscono come elementi di codice diversi, e in questo modo riceverete errori.

> [!NOTE]
> Potete trovare un elenco piuttosto completo di parole chiave riservate da evitare in [Grammatica lessicale — parole chiave](/it/docs/Web/JavaScript/Reference/Lexical_grammar#keywords).

Esempi di buoni nomi:

```plain example-good
age
myAge
init
initialColor
finalOutputValue
audio1
audio2
```

Esempi di cattivi nomi:

```plain example-bad
1
a
_12
myage
MYAGE
var
Document
skjfndskjfnbdskjfb
thisisareallylongvariablenameman
```

Provate ora a creare alcune altre variabili, tenendo a mente le linee guida sopra.

## Tipi di variabile

Ci sono alcuni diversi tipi di dati che possiamo memorizzare nelle variabili. In questa sezione li descriveremo brevemente, quindi nei prossimi articoli li imparerete in dettaglio.

### Numeri

Potete memorizzare numeri nelle variabili, sia numeri interi come 30 (chiamati anche numeri interi) o numeri decimali come 2.456 (chiamati anche float o numeri a virgola mobile). Non è necessario dichiarare i tipi di variabile in JavaScript, a differenza di altri linguaggi di programmazione. Quando assegnate un valore numerico a una variabile, non includete virgolette:

```js
let myAge = 17;
```

### Stringhe

Le stringhe sono porzioni di testo. Quando assegnate un valore stringa a una variabile, dovete racchiuderlo in virgole singole o doppie; altrimenti, JavaScript tenta di interpretarlo come un altro nome di variabile.

```js
let dolphinGoodbye = "So long and thanks for all the fish";
```

### Booleani

I booleani sono valori vero/falso — possono avere due valori, `true` o `false`. Questi vengono generalmente usati per testare una condizione, dopodiché si esegue il codice come appropriato. Ad esempio, un caso semplice sarebbe:

```js
let iAmAlive = true;
```

Mentre nella realtà si userebbe più così:

```js
let test = 6 < 3;
```

Questo sta usando l'operatore "minore di" (`<`) per verificare se 6 è minore di 3. Come ci si potrebbe aspettare, restituisce `false`, perché 6 non è minore di 3! Imparerete molto di più su tali operatori in seguito nel corso.

### Array

Un array è un singolo oggetto che contiene più valori racchiusi tra parentesi quadre e separati da virgole. Provate a inserire le seguenti righe nella console:

```js
let myNameArray = ["Chris", "Bob", "Jim"];
let myNumberArray = [10, 15, 40];
```

Una volta definiti questi array, potete accedere a ciascun valore in base alla loro posizione all'interno dell'array. Provate queste righe:

```js
myNameArray[0]; // should return 'Chris'
myNumberArray[2]; // should return 40
```

Le parentesi quadre specificano un valore di indice corrispondente alla posizione del valore che volete restituire. Potreste aver notato che gli array in JavaScript sono indicizzati a zero: il primo elemento si trova all'indice 0.

### Oggetti

In programmazione, un oggetto è una struttura di codice che modella un oggetto reale. Potete avere un oggetto che rappresenta una scatola e contiene informazioni sulla sua larghezza, lunghezza e altezza, o potreste avere un oggetto che rappresenta una persona, e contiene dati sul loro nome, altezza, peso, lingua parlata, come salutarli, e altro.

Provate a inserire la seguente riga nella console:

```js
let dog = { name: "Spot", breed: "Dalmatian" };
```

Per recuperare le informazioni memorizzate nell'oggetto, potete usare la seguente sintassi:

```js
dog.name;
```

## Tipizzazione dinamica

JavaScript è un "linguaggio tipizzato dinamicamente", il che significa che, a differenza di altri linguaggi, non è necessario specificare quale tipo di dato una variabile conterrà (numeri, stringhe, array, ecc.).

Ad esempio, se dichiarate una variabile e le assegnate un valore racchiuso tra virgolette, il browser tratterà la variabile come una stringa:

```js
let myString = "Hello";
```

Anche se il valore racchiuso tra virgolette è solo numeri, è comunque una stringa — non un numero — quindi fate attenzione:

```js
let myNumber = "500"; // oops, this is still a string
typeof myNumber;
myNumber = 500; // much better — now this is a number
typeof myNumber;
```

Provate a immettere le quattro righe sopra nella console una ad una, e vedere quali sono i risultati. Noterete che stiamo usando un operatore speciale chiamato [`typeof`](/it/docs/Web/JavaScript/Reference/Operators/typeof) — questo restituisce il tipo di dato della variabile che digitate dopo. La prima volta che viene chiamato, dovrebbe restituire `string`, poiché a quel punto la variabile `myNumber` contiene una stringa, `'500'`. Date un'occhiata e vedete cosa restituisce la seconda volta che lo chiamate.

## Costanti in JavaScript

Oltre alle variabili, potete dichiarare costanti. Queste sono come variabili, tranne per il fatto che:

- dovete inizializzarle quando le dichiarate.
- non potete assegnar loro un nuovo valore dopo l'inizializzazione.

Ad esempio, usando `let` potete dichiarare una variabile senza inizializzarla:

```js
let count;
```

Se provate a fare questo usando `const` vedrete un errore:

```js example-bad
const count;
```

Allo stesso modo, con `let` potete inizializzare una variabile e poi assegnarle un nuovo valore (questo si chiama anche _rassegnazione_ della variabile):

```js
let count = 1;
count = 2;
```

Se provate a fare questo usando `const` vedrete un errore:

```js example-bad
const count = 1;
count = 2;
```

Notate che anche se una costante in JavaScript deve sempre denominare lo stesso valore, potete cambiare il contenuto del valore che denomina. Questa non è una distinzione utile per tipi semplici come numeri o booleani, ma considerate un oggetto:

```js
const bird = { species: "Kestrel" };
console.log(bird.species); // "Kestrel"
```

Potete aggiornare, aggiungere o rimuovere proprietà di un oggetto dichiarato usando `const`, perché anche se il contenuto dell'oggetto è cambiato, la costante sta ancora puntando allo stesso oggetto:

```js
bird.species = "Striated Caracara";
console.log(bird.species); // "Striated Caracara"
```

## Quando usare const e quando usare let

Se con `const` non potete fare tanto quanto con `let`, perché preferireste usarlo piuttosto che `let`? In realtà `const` è molto utile. Se usate `const` per denominare un valore, dice a chiunque guardi il vostro codice che questo nome non sarà mai assegnato a un valore diverso. Ogni volta che vedranno questo nome, sapranno a cosa si riferisce.

In questo corso, adottiamo il seguente principio su quando usare `let` e quando usare `const`:

_Usate `const` quando potete, e usate `let` quando dovete._

Questo significa che se potete inizializzare una variabile quando la dichiarate, e non avete bisogno di rassegnarla in seguito, fatela una costante.

## Metti alla prova le tue abilità!

Avete raggiunto la fine di questo articolo, ma riuscite a ricordare le informazioni più importanti? Potete trovare ulteriori test per verificare di aver trattenuto queste informazioni prima di procedere — vedere [Test your skills: variables](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Variables).

Controllate anche [Practice time - Part 3: let e const](https://scrimba.com/learn-javascript-c0v/~059?via=mdn) <sup>[_partner per l'apprendimento di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> da Scrimba: Una sfida interattiva che fornisce test multipli relativi a `let` e `const`.

## Sommario

A questo punto dovreste sapere una ragionevole quantità di informazioni sulle variabili JavaScript e su come crearle. Nel prossimo articolo, ci concentreremo sui numeri in maggiore dettaglio, guardando come fare operazioni matematiche di base in JavaScript.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting")}}
