---
title: Matematica di base in JavaScript — numeri e operatori
short-title: Numeri e operatori
slug: Learn_web_development/Core/Scripting/Math
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}

In questo punto del corso, discutiamo della matematica in JavaScript — come possiamo usare gli {{Glossary("Operator", "operatori")}} e altre funzionalità per manipolare con successo i numeri per fare il nostro volere.

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
          <li>Operazioni numeriche di base in JavaScript — sommare, sottrarre, moltiplicare e dividere.</li>
          <li>I numeri non sono numeri se sono definiti come stringhe, e possono causare errori nei calcoli.</li>
          <li>Conversione di stringhe in numeri con <code>Number()</code>.</li>
          <li>Precedenza degli operatori.</li>
          <li>Incremento e decremento.</li>
          <li>Operatori di assegnazione e confronto.</li>
          <li>Metodi di base dell'oggetto Math, come <code>Math.random()</code>, <code>Math.floor()</code> e <code>Math.ceil()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## A tutti piace la matematica

D'accordo, magari non a tutti. Alcuni di noi amano la matematica, altri l'hanno detestata dai tempi delle tabelline e della divisione lunga a scuola, e alcuni si trovano in una posizione intermedia. Ma nessuno di noi può negare che la matematica è una parte fondamentale della vita dalla quale non possiamo andare molto lontano senza. Questo è particolarmente vero quando stiamo imparando a programmare in JavaScript (o in qualsiasi altro linguaggio) — tanto di ciò che facciamo si basa sul processamento di dati numerici, il calcolo di nuovi valori, e così via, che non sorprenderà sapere che JavaScript ha un set di funzioni matematiche molto completo.

Questo articolo discute solo le parti di base che è necessario conoscere adesso.

### Tipi di numeri

In programmazione, anche il sistema semplice dei numeri decimali che conosciamo bene è più complicato di quanto si possa pensare. Usiamo termini diversi per descrivere diversi tipi di numeri decimali, per esempio:

- **Interi** sono numeri senza parte frazionaria. Possono essere positivi o negativi, ad esempio 10, 400, o -5.
- **Numeri in virgola mobile** (float) hanno punti decimali e posizioni decimali, per esempio 12.5 e 56.7786543.

Abbiamo persino diversi tipi di sistemi numerici! Decimale è in base 10 (significa che usa 0–9 in ogni cifra), ma abbiamo anche cose come:

- **Binario** — Il linguaggio di livello più basso dei computer; 0 e 1.
- **Ottale** — Base 8, usa 0–7 in ogni cifra.
- **Esadecimale** — Base 16, usa 0–9 e poi a–f in ogni cifra. Potresti aver incontrato questi numeri prima quando impostavi i [colori in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#hexadecimal_rgb_values).

**Prima che inizi a preoccuparti per far fondere il tuo cervello, fermati lì!** Per cominciare, rimarremo solo con i numeri decimali durante questo corso; raramente ci sarà bisogno di iniziare a pensare ad altri tipi, se mai.

La seconda buona notizia è che, a differenza di alcuni altri linguaggi di programmazione, JavaScript ha solo un tipo di dato per i numeri, sia interi che decimali — hai indovinato, {{jsxref("Number")}}. Ciò significa che qualunque tipo di numeri con cui stai lavorando in JavaScript, li gestisci nello stesso modo.

> [!NOTE]
> In realtà, JavaScript ha un secondo tipo di numero, {{Glossary("BigInt", "BigInt")}}, usato per interi molto, molto grandi. Tuttavia, per le finalità di questo corso, ci preoccuperemo solo dei valori `Number`.

### È tutto numeri per me

Giochiamo rapidamente con alcuni numeri per riprendere familiarità con la sintassi base di cui abbiamo bisogno. Immetti i comandi elencati di seguito nella tua [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

1. Prima di tutto, dichiariamo un paio di variabili e inizializziamole rispettivamente con un intero e un float, quindi digita nuovamente i nomi delle variabili per verificare che tutto sia in ordine:

   ```js
   const myInt = 5;
   const myFloat = 6.667;
   myInt;
   myFloat;
   ```

2. I valori numerici vengono scritti senza virgolette — prova a dichiarare e inizializzare alcune altre variabili contenenti numeri prima di andare avanti.
3. Ora controlliamo che entrambe le nostre variabili originali siano dello stesso tipo di dati. C'è un operatore chiamato {{jsxref("Operators/typeof", "typeof")}} in JavaScript che lo fa. Inserisci le due righe seguenti così come sono:

   ```js
   typeof myInt;
   typeof myFloat;
   ```

   Dovresti ottenere `"number"` restituito in entrambi i casi — questo rende le cose molto più facili per noi rispetto a se i numeri diversi avessero tipi di dati diversi e dovessimo gestirli in modi differenti. Per fortuna!

### Metodi utili di Number

L'oggetto [`Number`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number), un'istanza del quale rappresenta tutti i numeri standard che userai nel tuo JavaScript, ha una serie di metodi utili disponibili per te per manipolare i numeri. Non copriamo questi in dettaglio in questo articolo perché volevamo mantenerlo come un'introduzione e coprire solo le cose essenziali veramente di base per ora; tuttavia, una volta che hai letto attraverso questo modulo un paio di volte, vale la pena andare alle pagine di riferimento dell'oggetto e imparare di più su cosa è disponibile.

Per esempio, per arrotondare il tuo numero a un numero fisso di posizioni decimali, usa il metodo [`toFixed()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed). Digita le seguenti righe nella console del tuo browser:

```js
const lotsOfDecimal = 1.766584958675746364;
lotsOfDecimal;
const twoDecimalPlaces = lotsOfDecimal.toFixed(2);
twoDecimalPlaces;
```

### Conversione ai tipi di dati numerici

A volte puoi trovarti con un numero che è memorizzato come tipo stringa, il che rende difficile eseguire calcoli con esso. Questo accade più comunemente quando i dati vengono inseriti in un [modulo](/it/docs/Learn_web_development/Extensions/Forms) di input, e il [tipo di input è testo](/it/docs/Web/HTML/Reference/Elements/input/text). C'è un modo per risolvere questo problema — passare il valore della stringa nel costruttore [`Number()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/Number) per restituire una versione numerica dello stesso valore.

Per esempio, prova a digitare queste righe nella tua console:

```js
let myNumber = "74";
myNumber += 3;
```

Ottieni come risultato 743, non 77, perché `myNumber` è in realtà definito come una stringa. Puoi testarlo digitando quanto segue:

```js
typeof myNumber;
```

Per correggere il calcolo, puoi fare così:

```js
let myNumber = "74";
myNumber = Number(myNumber) + 3;
```

Il risultato è quindi 77, come inizialmente previsto.

## Operatori aritmetici

Gli operatori aritmetici sono usati per eseguire calcoli matematici in JavaScript:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Operatore</th>
      <th scope="col">Nome</th>
      <th scope="col">Scopo</th>
      <th scope="col">Esempio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>+</code></td>
      <td>Addizione</td>
      <td>Sommare due numeri insieme.</td>
      <td><code>6 + 9</code></td>
    </tr>
    <tr>
      <td><code>-</code></td>
      <td>Sottrazione</td>
      <td>Sottrae il numero a destra da quello a sinistra.</td>
      <td><code>20 - 15</code></td>
    </tr>
    <tr>
      <td><code>*</code></td>
      <td>Moltiplicazione</td>
      <td>Moltiplica due numeri insieme.</td>
      <td><code>3 * 7</code></td>
    </tr>
    <tr>
      <td><code>/</code></td>
      <td>Divisione</td>
      <td>Divide il numero a sinistra per quello a destra.</td>
      <td><code>10 / 5</code></td>
    </tr>
    <tr>
      <td><code>%</code></td>
      <td>Resto (a volte chiamato modulo)</td>
      <td>
        <p>
          Restituisce il resto lasciato dopo aver diviso il numero a sinistra
          in un numero di porzioni intere uguali al numero a destra.
        </p>
      </td>
      <td>
        <p>
          <code>8 % 3</code> (restituisce 2, poiché tre entra in 8 due volte, lasciando 2 di resto).
        </p>
      </td>
    </tr>
    <tr>
      <td><code>**</code></td>
      <td>Esponente</td>
      <td>
        Eleva un numero di <code>base</code> alla potenza di un <code>esponente</code>,
        cioè il numero di <code>base</code> moltiplicato per se stesso,
        <code>esponente</code> volte.
      </td>
      <td>
        <code>5 ** 2</code> (restituisce <code>25</code>, che è lo stesso di
        <code>5 * 5</code>).
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> A volte i numeri coinvolti in operazioni aritmetiche vengono chiamati {{Glossary("Operand", "operandi")}}.

> [!NOTE]
> Potresti a volte vedere gli esponenti espressi usando il metodo più vecchio {{jsxref("Math.pow()")}}, che funziona in un modo molto simile. Per esempio, in `Math.pow(7, 3)`, `7` è la base e `3` è l'esponente, quindi il risultato dell'espressione è `343`. `Math.pow(7, 3)` è equivalente a `7**3`.

Probabilmente non è necessario insegnarti come fare operazioni matematiche di base, ma vorremmo testare la tua comprensione della sintassi coinvolta. Prova a inserire gli esempi sottostanti nella tua [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per familiarizzare con la sintassi.

1. Per prima cosa, prova a inserire alcuni esempi semplici tuoi, come

   ```js
   10 + 7;
   9 * 8;
   60 % 3;
   ```

2. Puoi anche provare a dichiarare e inizializzare alcuni numeri dentro le variabili, e provare a usare quelle nelle somme — le variabili si comporteranno esattamente come i valori che contengono ai fini del calcolo. Per esempio:

   ```js
   const num1 = 10;
   const num2 = 50;
   9 * num1;
   num1 ** 3;
   num2 / num1;
   ```

3. Infine, per questa sezione, prova a inserire alcune espressioni più complesse, come:

   ```js
   5 + 10 * 3;
   (num2 % 9) * num1;
   num2 + num1 / 8 + 2;
   ```

Alcune parti di quest'ultimo set di calcoli potrebbero non darti esattamente il risultato che ti aspettavi; la sezione sottostante potrebbe benissimo dare la risposta al perché.

### Precedenza degli operatori

Analizziamo l'ultimo esempio di sopra, assumendo che `num2` contenga il valore 50 e `num1` contenga il valore 10 (come dichiarato originariamente sopra):

```js
num2 + num1 / 8 + 2;
```

Come essere umano, potresti leggere questo come _"50 più 10 uguale 60"_, poi _"8 più 2 uguale 10"_, e infine _"60 diviso 10 uguale 6"_.

Ma il browser fa _"10 diviso 8 uguale 1.25"_, poi _"50 più 1.25 più 2 uguale 53.25"_.

Questo è perché della **precedenza degli operatori** — alcuni operatori vengono applicati prima di altri quando si calcola il risultato di un calcolo (chiamato _espressione_, in programmazione). La precedenza degli operatori in JavaScript è la stessa che viene insegnata nelle lezioni di matematica a scuola — si fanno sempre prima moltiplicazione e divisione, poi addizione e sottrazione (il calcolo è sempre valutato da sinistra a destra).

Se vuoi sovrascrivere la precedenza degli operatori, puoi mettere le parentesi attorno alle parti che vuoi siano trattate esplicitamente prima. Così, per ottenere un risultato di 6, potremmo fare questo:

```js
(num2 + num1) / (8 + 2);
```

Prova tu stesso e guarda.

> [!NOTE]
> Un elenco completo di tutti gli operatori JavaScript e della loro precedenza può essere trovato in [Precedenza degli operatori](/it/docs/Web/JavaScript/Reference/Operators/Operator_precedence).

## Operatori di incremento e decremento

A volte vuoi aggiungere o sottrarre ripetutamente uno a o da un valore di variabile numerica. Questo può essere fatto comodamente utilizzando gli operatori di incremento (`++`) e decremento (`--`). Abbiamo usato `++` nel nostro gioco "Indovina il numero" nel nostro [primo tuffo in JavaScript](/it/docs/Learn_web_development/Core/Scripting/A_first_splash), quando abbiamo aggiunto 1 alla nostra variabile `guessCount` per tenere traccia di quanti tentativi il giocatore aveva fatto dopo ogni turno.

```js
guessCount++;
```

Proviamo a giocare con questi nella tua console. Per cominciare, nota che non puoi applicare questi direttamente a un numero, il che potrebbe sembrare strano, ma stiamo assegnando a una variabile un nuovo valore aggiornato, non operando sul valore stesso. Il seguente restituirà un errore:

```js example-bad
3++;
```

Quindi, puoi solo incrementare una variabile esistente. Prova questo:

```js
let num1 = 4;
num1++;
```

Ok, stranezza numero 2! Quando fai questo, vedrai un valore di 4 restituito — questo perché il browser restituisce il valore corrente, _poi_ incrementa la variabile. Puoi vedere che è stato incrementato se restituisci nuovamente il valore della variabile:

```js
num1;
```

La stessa cosa è vera per `--`: prova il seguente

```js
let num2 = 6;
num2--;
num2;
```

> [!NOTE]
> Puoi far sì che il browser lo faccia al contrario — incrementa/decrementa la variabile _poi_ restituisce il valore — mettendo l'operatore all'inizio della variabile invece che alla fine. Prova di nuovo i suddetti esempi, ma questa volta usa `++num1` e `--num2`.

## Operatori di assegnazione

Gli operatori di assegnazione sono operatori che assegnano un valore a una variabile. Abbiamo già usato il più basilare, `=`, molte volte — assegna alla variabile a sinistra il valore indicato a destra:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x = y; // x now contains the same value y contains, 4
```

Ma ci sono alcuni tipi più complessi, che forniscono scorciatoie utili per mantenere il tuo codice più ordinato ed efficiente. I più comuni sono elencati di seguito:

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col">Operatore</th>
      <th scope="col">Nome</th>
      <th scope="col">Scopo</th>
      <th scope="col">Esempio</th>
      <th scope="col">Scorciatoia per</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>+=</code></td>
      <td>Assegnazione dell'addizione</td>
      <td>
        Aggiunge il valore a destra al valore della variabile a sinistra, poi
        restituisce il nuovo valore della variabile
      </td>
      <td><code>x += 4;</code></td>
      <td><code>x = x + 4;</code></td>
    </tr>
    <tr>
      <td><code>-=</code></td>
      <td>Assegnazione della sottrazione</td>
      <td>
        Sottrae il valore a destra al valore della variabile a sinistra,
        e restituisce il nuovo valore della variabile
      </td>
      <td><code>x -= 3;</code></td>
      <td><code>x = x - 3;</code></td>
    </tr>
    <tr>
      <td><code>*=</code></td>
      <td>Assegnazione della moltiplicazione</td>
      <td>
        Moltiplica il valore della variabile a sinistra per il valore a destra, e
        restituisce il nuovo valore della variabile
      </td>
      <td><code>x *= 3;</code></td>
      <td><code>x = x * 3;</code></td>
    </tr>
    <tr>
      <td><code>/=</code></td>
      <td>Assegnazione della divisione</td>
      <td>
        Divide il valore della variabile a sinistra per il valore a destra, e
        restituisce il nuovo valore della variabile
      </td>
      <td><code>x /= 5;</code></td>
      <td><code>x = x / 5;</code></td>
    </tr>
  </tbody>
</table>

Prova a digitare alcuni degli esempi sopra nella tua console, per farti un'idea di come funzionano. In ogni caso, cerca di indovinare quale sarà il valore prima di digitare la seconda riga.

Nota che puoi utilizzare tranquillamente altre variabili sul lato destro di ciascuna espressione, per esempio:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x *= y; // x now contains the value 12
```

> [!NOTE]
> Ci sono molti [altri operatori di assegnazione disponibili](/it/docs/Web/JavaScript/Guide/Expressions_and_operators#assignment_operators), ma questi sono quelli di base che dovresti imparare ora.

## Apprendimento attivo: ridimensionare una casella canvas

In questo esercizio, manipolerai alcuni numeri e operatori per modificare la dimensione di una casella. La casella è disegnata utilizzando un'API del browser chiamata [Canvas API](/it/docs/Web/API/Canvas_API). Non preoccuparti di come funziona — concentrati sulla matematica per ora. La larghezza e l'altezza della casella (in pixel) sono definite dalle variabili `x` e `y`, che hanno inizialmente entrambi un valore di 50.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/maths/editable_canvas.html", '100%', 620)}}

**[Apri in una nuova finestra](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/maths/editable_canvas.html)**

Nel box di codice modificabile sopra, ci sono due righe marcate con un commento che ti invitiamo ad aggiornare per fare in modo che la casella cresca/si riduca a determinate dimensioni, utilizzando certi operatori e/o valori in ciascun caso. Proviamo quanto segue:

- Cambia la riga che calcola `x` in modo che la casella sia ancora larga 50px, ma il 50 è calcolato usando i numeri 43 e 7 e un operatore aritmetico.
- Cambia la riga che calcola `y` in modo che la casella sia alta 75px, ma il 75 è calcolato usando i numeri 25 e 3 e un operatore aritmetico.
- Cambia la riga che calcola `x` in modo che la casella sia larga 250px, ma il 250 è calcolato usando due numeri e l'operatore del resto (modulo).
- Cambia la riga che calcola `y` in modo che la casella sia alta 150px, ma il 150 è calcolato usando tre numeri e gli operatori di sottrazione e divisione.
- Cambia la riga che calcola `x` in modo che la casella sia larga 200px, ma il 200 è calcolata usando il numero 4 e un operatore di assegnazione.
- Cambia la riga che calcola `y` in modo che la casella sia alta 200px, ma il 200 è calcolato usando i numeri 50 e 3, l'operatore di moltiplicazione e l'operatore di assegnazione additiva.
  Non dimenticare di assegnare prima un valore predefinito a `y` (in una riga separata), in modo che l'aggiunta funzioni come previsto.

Non preoccuparti se rovini completamente il codice. Puoi sempre premere il pulsante Reset per far funzionare di nuovo le cose. Dopo aver risposto correttamente a tutte le domande sopra, sentiti libero di giocare ancora con il codice o creare le tue sfide.

## Operatori di confronto

A volte vogliamo eseguire test vero/falso, quindi agire di conseguenza a seconda del risultato di quel test — per fare ciò usiamo gli **operatori di confronto**.

| Operatore | Nome                     | Scopo                                                                          | Esempio       |
| --------- | ------------------------ | ------------------------------------------------------------------------------ | ------------- |
| `===`     | Uguaglianza stretta      | Testa se i valori a sinistra e a destra sono identici tra loro                  | `5 === 2 + 4` |
| `!==`     | Disuguaglianza stretta   | Testa se i valori a sinistra e a destra non sono identici tra loro              | `5 !== 2 + 3` |
| `<`       | Minore di                | Testa se il valore a sinistra è minore di quello a destra.                      | `10 < 6`      |
| `>`       | Maggiore di              | Testa se il valore a sinistra è maggiore di quello a destra.                    | `10 > 20`     |
| `<=`      | Minore o uguale a        | Testa se il valore a sinistra è minore o uguale a quello a destra.              | `3 <= 2`      |
| `>=`      | Maggiore o uguale a      | Testa se il valore a sinistra è maggiore o uguale a quello a destra.            | `5 >= 4`      |

> [!NOTE]
> Potresti vedere alcune persone usare `==` e `!=` nei loro test di uguaglianza e disuguaglianza. Questi sono operatori validi in JavaScript, ma differiscono da `===`/`!==`. Le versioni precedenti testano se i valori sono gli stessi, ma non se i tipi di dati dei valori sono gli stessi. Le versioni strette testano l'uguaglianza sia dei valori che dei tipi di dati. Le versioni strette tendono a risultare in meno errori, quindi ti consigliamo di usarle.

Se provi a inserire alcuni di questi valori in una console, vedrai che tutti restituiscono valori `true`/`false` — quegli booleani di cui abbiamo parlato nell'articolo precedente. Questi sono molto utili, poiché ci permettono di fare decisioni nel nostro codice, e sono utilizzati ogni volta che vogliamo fare una scelta di qualche tipo. Per esempio, i booleani possono essere utilizzati per:

- Visualizzare l'etichetta di testo corretta su un pulsante a seconda che una funzionalità sia attivata o disattivata
- Visualizzare un messaggio di game over se un gioco è finito o un messaggio di vittoria se il gioco è stato vinto
- Visualizzare il saluto stagionale corretto a seconda di quale stagione festiva è
- Zoomare una mappa dentro o fuori a seconda di quale livello di zoom è selezionato

Vedremo come codificare tale logica quando guarderemo alle dichiarazioni condizionali in un futuro articolo. Per ora, diamo un'occhiata a un esempio rapido:

```html
<button>Start machine</button>
<p>The machine is stopped.</p>
```

```js
const btn = document.querySelector("button");
const txt = document.querySelector("p");

btn.addEventListener("click", updateBtn);

function updateBtn() {
  if (btn.textContent === "Start machine") {
    btn.textContent = "Stop machine";
    txt.textContent = "The machine has started!";
  } else {
    btn.textContent = "Start machine";
    txt.textContent = "The machine is stopped.";
  }
}
```

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/maths/conditional.html", '100%', 100)}}

**[Apri in una nuova finestra](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/maths/conditional.html)**

Puoi vedere l'operatore di uguaglianza essere usato appena dentro la funzione `updateBtn()`. In questo caso, non stiamo testando se due espressioni matematiche hanno lo stesso valore — stiamo testando se il contenuto di testo di un pulsante contiene una certa stringa — ma è ancora lo stesso principio in atto. Se il pulsante attualmente dice "Avvia macchina" quando viene premuto, cambiamo la sua etichetta in "Ferma macchina", e aggiorniamo l'etichetta come appropriato. Se il pulsante attualmente dice "Ferma macchina" quando viene premuto, riportiamo nuovamente la visualizzazione.

> [!NOTE]
> Tale controllo che scambia tra due stati è generalmente riferito come un **interruttore**. Si alterna tra uno stato e un altro — luce accesa, luce spenta, ecc.

## Metti alla prova le tue abilità!

Sei arrivato alla fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare alcuni ulteriori test per verificare se hai mantenuto queste informazioni prima di andare avanti — vedi [Metti alla prova le tue abilità: Matematica](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Math).

## Riepilogo

In questo articolo, abbiamo coperto le informazioni fondamentali che devi sapere sui numeri in JavaScript, per ora. Vedrai i numeri usati ancora e ancora, lungo tutto il tuo apprendimento di JavaScript, quindi è una buona idea toglierseli di torno ora. Se sei una di quelle persone a cui non piace la matematica, puoi trovare conforto nel fatto che questo capitolo è stato piuttosto breve.

Nel prossimo articolo, esploreremo il testo e come JavaScript ci consente di manipolarlo.

## Vedi anche

- [Numeri e stringhe](/it/docs/Web/JavaScript/Guide/Numbers_and_strings)
- [Espressioni e operatori](/it/docs/Web/JavaScript/Guide/Expressions_and_operators)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}
