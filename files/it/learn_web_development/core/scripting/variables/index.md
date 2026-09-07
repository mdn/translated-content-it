---
title: Memorizzare le informazioni necessarie — Variabili
short-title: Variables
slug: Learn_web_development/Core/Scripting/Variables
l10n:
  sourceCommit: 9d3d642daf9df9ece138fa39972edc5f7d6dcd6b
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting/Test_your_skills/Variables", "Learn_web_development/Core/Scripting")}}

Dopo aver letto gli ultimi due articoli, dovrebbe essere chiaro che cos'è JavaScript, cosa può fare, come usarlo insieme alle altre tecnologie web e quali sono, a grandi linee, le sue principali caratteristiche. In questo articolo verranno trattate le vere basi, osservando come lavorare con i blocchi costitutivi più elementari di JavaScript: le variabili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cosa sono le variabili e perché sono così importanti.</li>
          <li>Dichiarare variabili con <code>let</code>, inizializzarle con valori e riassegnarle con nuovi valori.</li>
          <li>Creare costanti con <code>const</code>.</li>
          <li>La differenza tra variabili e costanti e quando usare ciascuna di esse.</li>
          <li>Buone pratiche per assegnare nomi alle variabili.</li>
          <li>I diversi tipi di valori che possono essere memorizzati nelle variabili: stringhe, numeri, booleani, array e oggetti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti necessari

Nel corso di questo articolo verrà richiesto di digitare righe di codice per verificare la comprensione dei contenuti. Se si utilizza un browser desktop, il luogo migliore per digitare il codice di esempio è la console JavaScript del browser (consultare [Cosa sono gli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per maggiori informazioni su come accedere a questo strumento).

## Che cos'è una variabile?

Una variabile è un contenitore per un valore, come un numero che potrebbe essere usato in una somma o una stringa che potrebbe essere usata come parte di una frase.

### Esempio di variabile

Vediamo un esempio:

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

In questo esempio, premendo il pulsante viene eseguito del codice. Innanzitutto, viene modificato il testo del pulsante stesso. In secondo luogo, viene mostrato un messaggio con il numero di volte in cui il pulsante è stato premuto. Il numero viene memorizzato in una variabile. Ogni volta che l'utente preme il pulsante, il numero nella variabile aumenta di uno.

### Senza una variabile

Per capire perché tutto ciò è così utile, consideriamo come sarebbe scritto questo esempio senza usare una variabile per memorizzare il conteggio. Il risultato sarebbe simile a questo:

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

La sintassi utilizzata potrebbe non essere ancora completamente chiara, ma il concetto dovrebbe risultare comprensibile. Senza una variabile, non esiste un modo per sapere quante volte il pulsante è stato selezionato. Il messaggio per l'utente diventerebbe rapidamente irrilevante, poiché nessuna informazione può essere ricordata.

Le variabili sono semplicemente sensate e, man mano che si apprenderà altro su JavaScript, inizieranno a diventare naturali.

Una caratteristica speciale delle variabili è che possono contenere praticamente qualsiasi cosa, non solo stringhe e numeri. Le variabili possono anche contenere dati complessi e perfino intere funzioni che svolgono operazioni straordinarie. Questo verrà approfondito nel prosieguo.

> [!NOTE]
> Si dice che le variabili contengono valori. È una distinzione importante. Le variabili non sono i valori stessi; sono contenitori di valori. Si possono immaginare come piccole scatole di cartone nelle quali riporre oggetti.

![Una schermata di tre scatole di cartone tridimensionali che mostrano esempi di variabili JavaScript. Ogni scatola contiene valori ipotetici che rappresentano vari tipi di dati JavaScript. I valori di esempio sono rispettivamente "Bob", true e 35.](boxes.png)

## Dichiarare una variabile

Per usare una variabile, occorre prima crearla; più precisamente, questa operazione viene chiamata dichiarazione della variabile. Per farlo, si digita la parola chiave `let` seguita dal nome che si vuole assegnare alla variabile:

```js
let myName;
let myAge;
```

Qui vengono create due variabili chiamate `myName` e `myAge`. Provare a digitare queste righe nella console del browser web. Successivamente, provare a creare una variabile, o due, con nomi scelti liberamente.

> [!NOTE]
> In JavaScript, tutte le istruzioni di codice dovrebbero terminare con un punto e virgola (`;`): il codice potrebbe funzionare correttamente su singole righe, ma probabilmente non quando si scrivono più righe di codice insieme. È consigliabile abituarsi a includerlo.

È possibile verificare se questi valori esistono ora nell'ambiente di esecuzione digitando soltanto il nome della variabile, ad esempio:

```js
myName;
myAge;
```

Attualmente non hanno alcun valore; sono contenitori vuoti. Quando si immettono i nomi delle variabili, dovrebbe essere restituito il valore `undefined`. Se non esistono, verrà visualizzato un messaggio di errore: provare a digitare

```js
scoobyDoo;
```

> [!NOTE]
> Non confondere una variabile che esiste ma non ha un valore definito con una variabile che non esiste affatto: sono due cose molto diverse. Nell'analogia delle scatole vista sopra, la non esistenza significherebbe che non c'è alcuna scatola (variabile) in cui inserire un valore. Nessun valore definito significherebbe invece che esiste una scatola, ma non contiene alcun valore.

## Inizializzare una variabile

Dopo aver dichiarato una variabile, è possibile inizializzarla con un valore. Si fa digitando il nome della variabile, seguito da un segno di uguale (`=`), seguito dal valore che si vuole assegnarle. Ad esempio:

```js
myName = "Chris";
myAge = 37;
```

Tornare ora alla console e digitare queste righe. In ogni caso, dovrebbe essere visualizzato nella console il valore assegnato alla variabile, come conferma. Anche in questo caso, è possibile restituire i valori delle variabili digitandone il nome nella console: provare nuovamente queste istruzioni:

```js
myName;
myAge;
```

È possibile dichiarare e inizializzare una variabile allo stesso tempo, in questo modo:

```js
let myDog = "Rover";
```

Probabilmente sarà ciò che verrà fatto nella maggior parte dei casi, poiché è più veloce che eseguire le due operazioni su due righe separate.

## Una nota su var

Probabilmente sarà visibile anche un modo diverso per dichiarare le variabili, usando la parola chiave `var`:

```js
var myName;
var myAge;
```

Quando JavaScript fu creato inizialmente, questo era l'unico modo per dichiarare variabili. Il design di `var` è confuso e soggetto a errori. Nelle versioni moderne di JavaScript è stato quindi creato `let`, una nuova parola chiave per creare variabili che funziona in modo leggermente diverso da `var`, risolvendo nel processo i suoi problemi.

Di seguito vengono spiegate alcune semplici differenze. Non verranno trattate ora tutte le differenze, ma inizieranno a emergere man mano che si apprenderà di più su JavaScript. Per approfondirle subito, è possibile consultare la nostra [pagina di riferimento di let](/it/docs/Web/JavaScript/Reference/Statements/let).

Per iniziare, se si scrive un programma JavaScript su più righe che dichiara e inizializza una variabile, è possibile dichiarare effettivamente una variabile con `var` dopo averla inizializzata e il programma funzionerà comunque. Ad esempio:

```js
myName = "Chris";

function logName() {
  console.log(myName);
}

logName();

var myName;
```

> [!NOTE]
> Questo non funziona quando si digitano singole righe in una console JavaScript, ma solo quando si eseguono più righe di JavaScript in un documento web.

Questo funziona a causa dell'**hoisting**: leggere [hoisting di var](/it/docs/Web/JavaScript/Reference/Statements/var#hoisting) per maggiori dettagli sull'argomento.

L'hoisting non funziona più con `let`. Se nell'esempio precedente si sostituisse `var` con `let`, si verificherebbe un errore. Questo è positivo: dichiarare una variabile dopo averla inizializzata produce codice confuso e più difficile da comprendere.

In secondo luogo, usando `var`, è possibile dichiarare la stessa variabile tutte le volte desiderate, mentre con `let` non è possibile. Quanto segue funzionerebbe:

```js
var myName = "Chris";
var myName = "Bob";
```

Ma quanto segue genererebbe un errore sulla seconda riga:

```js example-bad
let myName = "Chris";
let myName = "Bob";
```

Sarebbe invece necessario fare questo:

```js
let myName = "Chris";
myName = "Bob";
```

Anche in questo caso, si tratta di una scelta di progettazione sensata del linguaggio. Non c'è motivo di ridichiarare le variabili: rende soltanto le cose più confuse.

Per questi e altri motivi, si consiglia di usare `let` nel codice anziché `var`. A meno che non si stia scrivendo esplicitamente il supporto per browser molto vecchi, non c'è più alcun motivo per usare `var`, poiché tutti i browser moderni supportano `let` dal 2015.

> [!NOTE]
> Se questo codice viene provato nella console del browser, è preferibile copiare e incollare ciascuno dei blocchi di codice qui presenti nella sua interezza. Esiste una [funzionalità nella console di Chrome](https://docs.google.com/document/d/1NP_FnHr4WCZRp7exgUklvNiXrH3nujcfwvp2pzMQ8-0/edit#heading=h.7y5hynxk52e9) che consente le ridichiarazioni di variabili con `let` e `const`:
>
> ```plain
> > let myName = "Chris";
>   let myName = "Bob";
> // As one input: SyntaxError: Identifier 'myName' has already been declared
>
> > let myName = "Chris";
> > let myName = "Bob";
> // As two inputs: both succeed
> ```

## Aggiornare una variabile

Dopo che una variabile è stata inizializzata con un valore, è possibile cambiare, o aggiornare, quel valore assegnandole un valore diverso. Provare a inserire le seguenti righe nella console:

```js
myName = "Bob";
myAge = 40;
```

### Una parentesi sulle regole per i nomi delle variabili

È possibile chiamare una variabile praticamente come si desidera, ma esistono alcune limitazioni. In generale, è opportuno limitarsi a usare caratteri latini (0-9, a-z, A-Z) e il carattere di sottolineatura.

- Non si dovrebbero usare altri caratteri, poiché possono causare errori o risultare difficili da comprendere per un pubblico internazionale.
- Non usare caratteri di sottolineatura all'inizio dei nomi delle variabili: vengono usati in alcuni costrutti JavaScript per indicare significati specifici, quindi potrebbero creare confusione.
- Non usare numeri all'inizio delle variabili. Non è consentito e causa un errore.
- Una convenzione sicura da seguire è il {{Glossary("camel_case", "lower camel case")}}, in cui si uniscono più parole usando le lettere minuscole per tutta la prima parola e inizializzando con una lettera maiuscola le parole successive. Questa convenzione è stata usata finora per i nomi delle variabili nell'articolo.
- Rendere intuitivi i nomi delle variabili, in modo che descrivano i dati che contengono. Non usare soltanto singole lettere o numeri, né frasi molto lunghe.
- Le variabili distinguono tra maiuscole e minuscole: quindi `myage` è una variabile diversa da `myAge`.
- Un ultimo punto: è inoltre necessario evitare di usare parole riservate di JavaScript come nomi delle variabili, ovvero le parole che costituiscono la sintassi effettiva di JavaScript. Non è quindi possibile usare parole come `var`, `function`, `let` e `for` come nomi di variabili. I browser le riconoscono come elementi di codice diversi e si otterranno quindi errori.

> [!NOTE]
> Un elenco abbastanza completo delle parole chiave riservate da evitare è disponibile in [Grammatica lessicale — parole chiave](/it/docs/Web/JavaScript/Reference/Lexical_grammar#keywords).

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

Provare ora a creare altre variabili, tenendo presenti le indicazioni precedenti.

## Tipi di variabile

Esistono diversi tipi di dati che possono essere memorizzati nelle variabili. In questa sezione verranno descritti brevemente; negli articoli futuri saranno analizzati più in dettaglio.

### Numeri

È possibile memorizzare numeri nelle variabili, sia numeri interi come 30, detti anche integer, sia numeri decimali come 2.456, detti anche float o numeri a virgola mobile. In JavaScript non è necessario dichiarare i tipi delle variabili, a differenza di altri linguaggi di programmazione. Quando si assegna a una variabile un valore numerico, non si includono virgolette:

```js
let myAge = 17;
```

### Stringhe

Le stringhe sono porzioni di testo. Quando si assegna a una variabile un valore stringa, occorre racchiuderlo tra virgolette singole o doppie; altrimenti JavaScript cerca di interpretarlo come il nome di un'altra variabile.

```js
let dolphinGoodbye = "So long and thanks for all the fish";
```

### Booleani

I booleani sono valori vero/falso: possono avere due valori, `true` o `false`. Vengono generalmente usati per verificare una condizione, dopo la quale il codice viene eseguito in modo appropriato. Ad esempio, un caso semplice sarebbe:

```js
let iAmAlive = true;
```

In realtà, verrebbe invece usato più o meno in questo modo:

```js
let test = 6 < 3;
```

Qui viene usato l'operatore "minore di" (`<`) per verificare se 6 è minore di 3. Come prevedibile, restituisce `false`, perché 6 non è minore di 3. Più avanti nel corso verrà approfondito l'uso di operatori di questo tipo.

### Array

Un array è un singolo oggetto che contiene più valori racchiusi tra parentesi quadre e separati da virgole. Provare a inserire le seguenti righe nella console:

```js
let myNameArray = ["Chris", "Bob", "Jim"];
let myNumberArray = [10, 15, 40];
```

Dopo aver definito questi array, è possibile accedere a ciascun valore in base alla sua posizione nell'array. Provare queste righe:

```js
myNameArray[0]; // should return 'Chris'
myNumberArray[2]; // should return 40
```

Le parentesi quadre specificano un valore di indice corrispondente alla posizione del valore che si desidera restituire. Si potrebbe aver notato che gli array in JavaScript hanno indice a base zero: il primo elemento si trova all'indice 0.

### Oggetti

Nella programmazione, un oggetto è una struttura di codice che modella un oggetto del mondo reale. Si può avere un oggetto che rappresenta una scatola e contiene informazioni sulla sua larghezza, lunghezza e altezza, oppure un oggetto che rappresenta una persona e contiene dati sul suo nome, altezza, peso, lingua parlata, come salutarla e altro ancora.

Provare a inserire la seguente riga nella console:

```js
let dog = { name: "Spot", breed: "Dalmatian" };
```

Per recuperare le informazioni memorizzate nell'oggetto, è possibile usare la seguente sintassi:

```js
dog.name;
```

## Tipizzazione dinamica

JavaScript è un "linguaggio a tipizzazione dinamica", il che significa che, a differenza di alcuni altri linguaggi, non è necessario specificare quale tipo di dati conterrà una variabile, come numeri, stringhe o array.

Ad esempio, se si dichiara una variabile e le si assegna un valore racchiuso tra virgolette, il browser tratta la variabile come una stringa:

```js
let myString = "Hello";
```

Anche se il valore racchiuso tra virgolette contiene soltanto cifre, resta una stringa, non un numero: occorre quindi fare attenzione.

```js
let myNumber = "500"; // oops, this is still a string
typeof myNumber;
myNumber = 500; // much better — now this is a number
typeof myNumber;
```

Provare a inserire nella console le quattro righe precedenti, una alla volta, e osservare i risultati. Si noterà l'uso di un operatore speciale chiamato [`typeof`](/it/docs/Web/JavaScript/Reference/Operators/typeof), che restituisce il tipo di dati della variabile digitata dopo di esso. Alla prima chiamata dovrebbe restituire `string`, poiché a quel punto la variabile `myNumber` contiene una stringa, `'500'`. Osservare cosa restituisce alla seconda chiamata.

## Costanti in JavaScript

Oltre alle variabili, è possibile dichiarare costanti. Sono simili alle variabili, tranne per il fatto che:

- devono essere inizializzate al momento della dichiarazione;
- non è possibile assegnare loro un nuovo valore dopo averle inizializzate.

Ad esempio, usando `let` è possibile dichiarare una variabile senza inizializzarla:

```js
let count;
```

Se si prova a farlo usando `const`, verrà visualizzato un errore:

```js example-bad
const count;
```

Analogamente, con `let` è possibile inizializzare una variabile e poi assegnarle un nuovo valore, operazione chiamata anche _riassegnazione_ della variabile:

```js
let count = 1;
count = 2;
```

Se si prova a farlo usando `const`, verrà visualizzato un errore:

```js example-bad
const count = 1;
count = 2;
```

Si noti che, sebbene una costante in JavaScript debba sempre denominare lo stesso valore, è possibile modificare il contenuto del valore che denomina. Non è una distinzione utile per tipi semplici come numeri o booleani, ma si consideri un oggetto:

```js
const bird = { species: "Kestrel" };
console.log(bird.species); // "Kestrel"
```

È possibile aggiornare, aggiungere o rimuovere proprietà di un oggetto dichiarato usando `const`, perché, anche se il contenuto dell'oggetto è cambiato, la costante punta ancora allo stesso oggetto:

```js
bird.species = "Striated Caracara";
console.log(bird.species); // "Striated Caracara"
```

## Quando usare const e quando usare let

Se con `const` non si possono fare tante operazioni quante con `let`, perché preferirlo a `let`? In realtà `const` è molto utile. Se si usa `const` per denominare un valore, si comunica a chiunque legga il codice che quel nome non verrà mai assegnato a un valore diverso. Ogni volta che vedrà quel nome, saprà a cosa si riferisce.

In questo corso viene adottato il seguente principio per decidere quando usare `let` e quando usare `const`:

_Usare `const` quando possibile e `let` quando necessario._

Ciò significa che, se è possibile inizializzare una variabile quando viene dichiarata e non è necessario riassegnarla in seguito, occorre renderla una costante.

## Riepilogo

A questo punto dovrebbe essere stata acquisita una discreta conoscenza delle variabili JavaScript e di come crearle. Nel prossimo articolo verranno proposti alcuni test per verificare quanto bene queste informazioni siano state comprese e memorizzate.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting/Test_your_skills/Variables", "Learn_web_development/Core/Scripting")}}
