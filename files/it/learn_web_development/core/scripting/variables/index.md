---
title: Conservare le informazioni di cui ha bisogno — Variabili
short-title: Variables
slug: Learn_web_development/Core/Scripting/Variables
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting")}}

Dopo aver letto gli ultimi articoli, dovrebbe ora sapere cos'è JavaScript, cosa può fare per lei, come usarlo insieme ad altre tecnologie web e quale aspetto abbiano le sue principali funzionalità da un livello alto. In questo articolo, affronteremo i veri fondamentali, analizzando come lavorare con i blocchi costitutivi più basilari di JavaScript — le Variabili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di base di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono le variabili e perché sono così importanti.</li>
          <li>Dichiarare variabili con <code>let</code>, inizializzarle con valori e riassegnarle con nuovi valori.</li>
          <li>Creare costanti con <code>const</code>.</li>
          <li>La differenza tra variabili e costanti e quando utilizzare ciascuna.</li>
          <li>Migliori pratiche di denominazione delle variabili.</li>
          <li>I diversi tipi di valore che possono essere conservati in variabili: stringhe, numeri, booleani, array e oggetti.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti di cui ha bisogno

Nel corso di questo articolo, verrà chiesto di digitare righe di codice per testare la sua comprensione del contenuto. Se sta utilizzando un browser desktop, il posto migliore per digitare il suo codice di esempio è la console JavaScript del suo browser (consultare [Quali sono gli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per ulteriori informazioni su come accedere a questo strumento).

## Cos'è una variabile?

Una variabile è un contenitore per un valore, come un numero che potremmo usare in un'operazione, o una stringa che potremmo usare come parte di una frase.

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

In questo esempio, premendo il pulsante si esegue del codice. In primo luogo, cambia il testo sul pulsante stesso. In secondo luogo, mostra un messaggio con il numero di volte in cui il pulsante è stato premuto. Il numero è memorizzato in una variabile. Ogni volta che l'utente preme il pulsante, il numero nella variabile incrementa di uno.

### Senza una variabile

Per capire perché questo sia così utile, pensiamo a come scriveremmo questo esempio senza usare una variabile per mantenere il conteggio. Finirebbe per apparire qualcosa del genere:

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

Potrebbe non comprendere appieno la sintassi che stiamo usando (ancora!), ma dovrebbe essere in grado di cogliere l'idea. Senza una variabile, non abbiamo un modo per sapere quante volte il pulsante è stato cliccato. Il messaggio per l'utente diventerà rapidamente irrilevante quando nessuna informazione può essere ricordata.

Le variabili hanno semplicemente senso, e man mano che imparerà di più su JavaScript inizieranno a diventare di seconda natura.

Una cosa speciale delle variabili è che possono contenere praticamente qualsiasi cosa, non solo stringhe e numeri. Le variabili possono anche contenere dati complessi e persino intere funzioni per fare cose straordinarie. Imparerà di più su questo man mano che va avanti.

> [!NOTE]
> Diciamo che le variabili contengono valori. Questa è una distinzione importante da fare. Le variabili non sono i valori stessi; sono contenitori per i valori. Si può pensare a loro come a piccoli scatoloni di cartone in cui si possono conservare oggetti.

![Uno screenshot di tre scatole di cartone tridimensionali che dimostrano esempi di variabili JavaScript. Ogni scatola contiene valori ipotetici che rappresentano vari tipi di dati JavaScript. I valori di esempio sono "Bob", true e 35 rispettivamente.](boxes.png)

## Dichiarare una variabile

Per usare una variabile, bisogna prima crearla — più accuratamente, chiamiamo questo processo dichiarare la variabile. Per farlo, si digita la parola chiave `let` seguita dal nome che desidera dare alla sua variabile:

```js
let myName;
let myAge;
```

Qui stiamo creando due variabili chiamate `myName` e `myAge`. Provi a digitare queste linee nella console del suo browser web. Dopodiché, provi a creare una variabile (o due) con scelte di nomi proprie.

> [!NOTE]
> In JavaScript, tutte le istruzioni di codice dovrebbero terminare con un punto e virgola (`;`) — il suo codice può funzionare correttamente per linee singole, ma probabilmente non funzionerà quando scrive più linee di codice insieme. Provi a prendere l'abitudine di includerlo.

Può verificare se questi valori ora esistono nell'ambiente di esecuzione digitando solo il nome della variabile, ad esempio:

```js
myName;
myAge;
```

Attualmente non hanno valore; sono contenitori vuoti. Quando inserisce i nomi delle variabili, dovrebbe ricevere il valore `undefined`. Se non esistono, otterrà un messaggio di errore — provi a digitare

```js
scoobyDoo;
```

> [!NOTE]
> Non confonda una variabile che esiste ma non ha valore definito con una variabile che non esiste affatto — sono cose molto diverse. Nell'analogia della scatola che ha visto sopra, non esistere significherebbe che non c'è alcuna scatola (variabile) in cui inserire un valore. Nessun valore definito significherebbe che c'è una scatola, ma non ha alcun valore al suo interno.

## Inizializzare una variabile

Una volta che ha dichiarato una variabile, può inizializzarla con un valore. Lo fa digitando il nome della variabile, seguito da un segno di uguale (`=`), seguito dal valore che desidera assegnarle. Ad esempio:

```js
myName = "Chris";
myAge = 37;
```

Provi ora a tornare alla console e digitare queste righe. Dovrebbe vedere il valore assegnato alla variabile restituire nella console per confermarlo, in ogni caso. Ancora una volta, può visualizzare i valori delle sue variabili digitando i loro nomi nella console — provi di nuovo questi:

```js
myName;
myAge;
```

Può dichiarare e inizializzare una variabile allo stesso tempo, in questo modo:

```js
let myDog = "Rover";
```

Questo è probabilmente ciò che farà la maggior parte del tempo, poiché è più veloce che fare le due azioni su due linee separate.

## Una nota su var

Probabilmente vedrà anche un modo diverso di dichiarare le variabili, utilizzando la parola chiave `var`:

```js
var myName;
var myAge;
```

Quando JavaScript fu creato per la prima volta, questo era l'unico modo per dichiarare variabili. Il design di `var` è confuso e soggetto a errori. Quindi è stato creato `let` nelle versioni moderne di JavaScript, una nuova parola chiave per creare variabili che funziona in modo leggermente diverso rispetto a `var`, correggendone i problemi nel processo.

Un paio di semplici differenze sono spiegate di seguito. Non esamineremo tutte le differenze ora, ma inizierà a scoprirle man mano che impara di più su JavaScript (se davvero desidera leggerle adesso, si senta libero di consultare la nostra [pagina di riferimento su let](/it/docs/Web/JavaScript/Reference/Statements/let)).

Per iniziare, se scrive un programma JavaScript su più righe che dichiara e inizializza una variabile, può effettivamente dichiarare una variabile con `var` dopo averla inizializzata e funzionerà comunque. Ad esempio:

```js
myName = "Chris";

function logName() {
  console.log(myName);
}

logName();

var myName;
```

> [!NOTE]
> Questo non funzionerà quando si digita su linee individuali in una console JavaScript, solo quando si eseguono più righe di JavaScript in un documento web.

Questo funziona grazie al **sollevamento** — legga [var hoisting](/it/docs/Web/JavaScript/Reference/Statements/var#hoisting) per maggiori dettagli sull'argomento.

Il sollevamento non funziona più con `let`. Se cambiassimo `var` in `let` nell'esempio sopra, fallirebbe con un errore. Questo è positivo — dichiarare una variabile dopo averla inizializzata porta a codice confuso e più difficile da capire.

In secondo luogo, quando usa `var`, può dichiarare la stessa variabile tutte le volte che vuole, ma con `let` no. Il seguente funzionerebbe:

```js
var myName = "Chris";
var myName = "Bob";
```

Ma il seguente lancerebbe un errore alla seconda linea:

```js example-bad
let myName = "Chris";
let myName = "Bob";
```

Dovrebbe fare questo invece:

```js
let myName = "Chris";
myName = "Bob";
```

Ancora una volta, questa è una decisione di linguaggio sensata. Non c'è motivo di ridefinire variabili — questo rende le cose solo più confuse.

Per questi motivi e altri, raccomandiamo di usare `let` nel suo codice, piuttosto che `var`. A meno che non stia scrivendo esplicitamente supporto per browser antichi, non c'è più alcuna ragione di usare `var` poiché tutti i browser moderni supportano `let` dal 2015.

> [!NOTE]
> Se sta provando questo codice nella console del suo browser, preferisca copiare e incollare ciascuno dei blocchi di codice qui come un tutt'uno. In Chrome c'è una [funzione nella console](https://docs.google.com/document/d/1NP_FnHr4WCZRp7exgUklvNiXrH3nujcfwvp2pzMQ8-0/edit#heading=h.7y5hynxk52e9) in cui sono consentite le ridefinizioni delle variabili con `let` e `const`:
>
> ```plain
> > let myName = "Chris";
>   let myName = "Bob";
> // Come input unico: SyntaxError: Identifier 'myName' has already been declared
>
> > let myName = "Chris";
> > let myName = "Bob";
> // Come due input: entrambi riescono
> ```

## Aggiornare una variabile

Una volta che una variabile è stata inizializzata con un valore, può cambiare (o aggiornare) quel valore assegnandole un valore diverso. Provi a immettere le seguenti righe nella sua console:

```js
myName = "Bob";
myAge = 40;
```

### Una parentesi sulle regole di denominazione delle variabili

Può chiamare una variabile praticamente come preferisce, ma ci sono delle limitazioni. In generale, dovrebbe attenersi a utilizzare solo caratteri latini (0-9, a-z, A-Z) e il carattere underscore.

- Non dovrebbe usare altri caratteri perché potrebbero causare errori o essere difficili da capire per un pubblico internazionale.
- Non usi underscore all'inizio dei nomi delle variabili — questo è usato in determinati costrutti JavaScript per significare cose specifiche, quindi potrebbe creare confusione.
- Non usi numeri all'inizio delle variabili. Questo non è consentito e causa un errore.
- Una convenzione sicura da seguire è il {{Glossary("camel_case", "camel case minuscolo")}}, dove si uniscono più parole, usando le minuscole per tutta la prima parola e poi si capita delle parole successive. L'abbiamo utilizzato per i nostri nomi di variabili nell'articolo finora.
- Faccia in modo che i nomi delle variabili siano intuitivi, in modo che descrivano i dati che contengono. Non usi solo lettere/numeri singoli, o frasi lunghe.
- Le variabili sono case sensitive — quindi `myage` è una variabile diversa da `myAge`.
- Un ultimo punto: deve anche evitare di usare parole riservate di JavaScript come nomi di variabili — con questo intendiamo le parole che compongono la sintassi effettiva di JavaScript! Quindi, non può usare parole come `var`, `function`, `let` e `for` come nomi di variabili. I browser le riconoscono come elementi di codice diversi, e quindi otterrà errori.

> [!NOTE]
> Può trovare un elenco abbastanza completo di parole chiave riservate da evitare su [Grammar lessicale — parole chiave](/it/docs/Web/JavaScript/Reference/Lexical_grammar#keywords).

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

Provi ora a creare alcune variabili, tenendo presente le indicazioni sopra.

## Tipi di variabili

Esistono diversi tipi di dati che possiamo salvare in variabili. In questa sezione descriveremo brevemente questi tipi, poi li imparerà in dettaglio in futuri articoli.

### Numeri

Può salvare numeri nelle variabili, siano essi numeri interi come 30 (anche chiamati interi) o numeri decimali come 2.456 (anche chiamati float o numeri in virgola mobile). Non ha bisogno di dichiarare i tipi di variabile in JavaScript, a differenza di alcuni altri linguaggi di programmazione. Quando assegna un valore numerico a una variabile, non includa le virgolette:

```js
let myAge = 17;
```

### Stringhe

Le stringhe sono pezzi di testo. Quando assegna un valore stringa a una variabile, deve racchiuderla in virgolette singole o doppie; altrimenti, JavaScript tenterà di interpretarla come un altro nome di variabile.

```js
let dolphinGoodbye = "So long and thanks for all the fish";
```

### Booleani

I booleani sono valori vero/falso — possono avere due valori, `true` o `false`. Sono generalmente utilizzati per testare una condizione, dopo la quale viene eseguito il codice appropriato. Quindi ad esempio, un caso semplice sarebbe:

```js
let iAmAlive = true;
```

Mentre in realtà verrebbe utilizzato più come segue:

```js
let test = 6 < 3;
```

Questo utilizza l'operatore "minore di" (`<`) per verificare se 6 è minore di 3. Come potrebbe aspettarsi, restituisce `false`, perché 6 non è minore di 3! Imparerà molto di più su tali operatori più avanti nel corso.

### Array

Un array è un singolo oggetto che contiene più valori racchiusi tra parentesi quadre e separati da virgole. Provare a inserire le seguenti righe nella sua console:

```js
let myNameArray = ["Chris", "Bob", "Jim"];
let myNumberArray = [10, 15, 40];
```

Dopo che questi array sono stati definiti, può accedere a ciascun valore in base alla loro posizione all'interno dell'array. Provare queste righe:

```js
myNameArray[0]; // should return 'Chris'
myNumberArray[2]; // should return 40
```

Le parentesi quadre specificano un valore di indice corrispondente alla posizione del valore che si desidera restituire. Potrebbe aver notato che gli array in JavaScript sono indicizzati a zero: il primo elemento si trova all'indice 0.

### Oggetti

In programmazione, un oggetto è una struttura di codice che modella un oggetto della vita reale. Può avere un oggetto che rappresenta una scatola e contiene informazioni sulla sua larghezza, lunghezza e altezza, oppure potrebbe avere un oggetto che rappresenta una persona, e contiene dati sul loro nome, altezza, peso, quale lingua parlano, come salutarli e altro.

Provi a inserire la seguente riga nella sua console:

```js
let dog = { name: "Spot", breed: "Dalmatian" };
```

Per recuperare le informazioni memorizzate nell'oggetto, può usare la seguente sintassi:

```js
dog.name;
```

## Tipizzazione dinamica

JavaScript è un "linguaggio a tipizzazione dinamica", il che significa che, a differenza di altri linguaggi, non è necessario specificare che tipo di dato conterrà una variabile (numeri, stringhe, array, ecc.).

Ad esempio, se dichiara una variabile e le assegna un valore racchiuso tra virgolette, il browser tratta la variabile come una stringa:

```js
let myString = "Hello";
```

Anche se il valore racchiuso tra virgolette è solo cifre, è comunque una stringa — non un numero — quindi si faccia attenzione:

```js
let myNumber = "500"; // oops, this is still a string
typeof myNumber;
myNumber = 500; // much better — now this is a number
typeof myNumber;
```

Provi a inserire le quattro righe sopra nella sua console una per una e vedere quali sono i risultati. Noterà che stiamo utilizzando un operatore speciale chiamato [`typeof`](/it/docs/Web/JavaScript/Reference/Operators/typeof) — questo restituisce il tipo di dato della variabile che digita dopo di esso. La prima volta che viene chiamato dovrebbe restituire `string`, poiché in quel momento la variabile `myNumber` contiene una stringa, `'500'`. Dia un'occhiata e veda cosa restituisce la seconda volta che la chiama.

## Costanti in JavaScript

Oltre alle variabili, può dichiarare costanti. Queste sono come le variabili, tranne per il fatto che:

- deve inizializzarle quando le dichiara.
- non può assegnare loro un nuovo valore dopo averle inizializzate.

Ad esempio, usando `let` può dichiarare una variabile senza inizializzarla:

```js
let count;
```

Se prova a fare questo utilizzando `const`, vedrà un errore:

```js example-bad
const count;
```

Allo stesso modo, con `let` può inizializzare una variabile e quindi assegnarle un nuovo valore (questo viene anche chiamato _rassegnazione_ della variabile):

```js
let count = 1;
count = 2;
```

Se prova a fare questo utilizzando `const`, vedrà un errore:

```js example-bad
const count = 1;
count = 2;
```

Si noti che, sebbene una costante in JavaScript debba sempre nominare lo stesso valore, può cambiare il contenuto del valore che nomina. Questa non è una distinzione utile per i tipi semplici come numeri o booleani, ma consideri un oggetto:

```js
const bird = { species: "Kestrel" };
console.log(bird.species); // "Kestrel"
```

Può aggiornare, aggiungere o rimuovere proprietà di un oggetto dichiarato utilizzando `const`, perché anche se il contenuto dell'oggetto è cambiato, la costante punta ancora allo stesso oggetto:

```js
bird.species = "Striated Caracara";
console.log(bird.species); // "Striated Caracara"
```

## Quando usare const e quando usare let

Se non può fare tanto con `const` quanto con `let`, perché preferirebbe usarlo piuttosto che `let`? In effetti, `const` è molto utile. Se usa `const` per nominare un valore, dice a chiunque guardi il suo codice che questo nome non verrà mai assegnato a un valore diverso. Ogni volta che vedono questo nome, sapranno a cosa si riferisce.

In questo corso, adottiamo il seguente principio su quando usare `let` e quando usare `const`:

_Usi `const` quando può, e usi `let` quando deve._

Questo significa che se può inizializzare una variabile quando la dichiara, e non ha bisogno di riassegnarla più tardi, la faccia una costante.

## Testi le sue competenze!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver mantenuto queste informazioni prima di andare avanti — veda [Metta alla prova le sue competenze: variabili](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Variables).

Dia anche un'occhiata a [Tempo di pratica - Parte 3: let e const](https://scrimba.com/learn-javascript-c0v/~059?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> da Scrimba: una sfida interattiva che offre diversi test riguardanti `let` e `const`.

## Riepilogo

A questo punto dovrebbe sapere una quantità ragionevole sulle variabili JavaScript e come crearle. Nell'articolo successivo, ci concentreremo sui numeri in maggior dettaglio, esaminando come fare i calcoli di base in JavaScript.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting/Math", "Learn_web_development/Core/Scripting")}}
