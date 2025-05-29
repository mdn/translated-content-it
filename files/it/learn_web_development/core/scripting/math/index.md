---
title: Matematica di base in JavaScript — numeri e operatori
short-title: Numeri e operatori
slug: Learn_web_development/Core/Scripting/Math
l10n:
  sourceCommit: ffa6f5871f50856c60983a125cef7de267be7aeb
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}

A questo punto del corso, discutiamo della matematica in JavaScript — come possiamo usare {{Glossary("Operator", "operators")}} e altre caratteristiche per manipolare i numeri per fare ciò che vogliamo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di base di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Operazioni di base sui numeri in JavaScript — addizione, sottrazione, moltiplicazione e divisione.</li>
          <li>i numeri non sono numeri se definiti come stringhe e possono causare errori nei calcoli.</li>
          <li>Conversione di stringhe in numeri con <code>Number()</code>.</li>
          <li>Precedenza degli operatori.</li>
          <li>Incremento e decremento.</li>
          <li>Operatori di assegnazione e confronto.</li>
          <li>Metodi di base dell'oggetto Math, come <code>Math.random()</code>, <code>Math.floor()</code>, e <code>Math.ceil()</code>.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## A tutti piace la matematica

Va bene, forse no. Alcuni di noi amano la matematica, alcuni l'hanno odiata fin dalla scuola quando dovevamo imparare le tavole di moltiplicazione e la divisione lunga, e alcuni di noi si trovano da qualche parte nel mezzo. Ma nessuno può negare che la matematica è una parte fondamentale della vita che non possiamo ignorare. Questo è particolarmente vero quando stiamo imparando a programmare in JavaScript (o in qualsiasi altro linguaggio) — gran parte del nostro lavoro dipende dall'elaborazione dei dati numerici, dal calcolare nuovi valori, ecc., e non sarai sorpreso di sapere che JavaScript ha un set completo di funzioni matematiche disponibili.

Questo articolo discute solo le parti di base che devi conoscere ora.

### Tipi di numeri

In programmazione, anche il semplice sistema numerico decimale che tutti conosciamo è più complicato di quanto si possa pensare. Usiamo diversi termini per descrivere diversi tipi di numeri decimali, per esempio:

- **Interi** sono numeri senza parte frazionaria. Possono essere positivi o negativi, ad esempio 10, 400 o -5.
- **Numeri in virgola mobile** (float) hanno punti e cifre decimali, per esempio 12.5, e 56.7786543.

Abbiamo anche diversi tipi di sistemi numerici! Il decimale è in base 10 (cioè usa 0–9 in ogni cifra), ma abbiamo anche cose come:

- **Binario** — Il linguaggio di più basso livello dei computer; 0s e 1s.
- **Ottale** — Base 8, usa 0–7 in ogni cifra.
- **Esadecimale** — Base 16, usa 0–9 e poi a–f in ogni cifra. Potresti aver incontrato questi numeri in precedenza quando hai impostato i [colori in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#hexadecimal_rgb_values).

**Prima di iniziare a preoccuparti di fondere il tuo cervello, fermati qui!** Per iniziare, ci atterremo solo ai numeri decimali in tutto questo corso; raramente troverai la necessità di iniziare a pensare ad altri tipi, se mai.

La seconda buona notizia è che, a differenza di altri linguaggi di programmazione, JavaScript ha solo un tipo di dati per i numeri, sia interi che decimali — hai indovinato, {{jsxref("Number")}}. Questo significa che qualunque tipo di numero tu stia gestendo in JavaScript, lo gestisci esattamente nello stesso modo.

> [!NOTE]
> Effettivamente, JavaScript ha un secondo tipo di numero, {{Glossary("BigInt", "BigInt")}}, usato per interi molto, molto grandi. Ma per i fini di questo corso, ci preoccuperemo solo dei valori `Number`.

### Per me sono tutti numeri

Facciamo rapidamente un po' di pratica con i numeri per familiarizzare con la sintassi di base che ci serve. Inserisci i comandi elencati di seguito nella tua [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

1. Prima di tutto, dichiariamo un paio di variabili e inizializziamole rispettivamente con un intero e un float, quindi digitiamo nuovamente i nomi delle variabili per verificare che tutto sia in ordine:

   ```js
   const myInt = 5;
   const myFloat = 6.667;
   myInt;
   myFloat;
   ```

2. I valori numerici vengono digitati senza virgolette — prova a dichiarare e inizializzare un paio di altre variabili che contengono numeri prima di procedere.
3. Ora verifichiamo che entrambe le nostre variabili originali siano dello stesso tipo di dati. C'è un operatore chiamato {{jsxref("Operators/typeof", "typeof")}} in JavaScript che fa questo. Inserisci le due righe sotto come mostrato:

   ```js
   typeof myInt;
   typeof myFloat;
   ```

   Dovresti ottenere `"number"` in entrambi i casi — questo rende le cose molto più facili per noi rispetto a se diversi numeri avessero tipi di dati diversi e dovessimo gestirli in modi diversi. Per fortuna!

### Metodi utili di Number

L'oggetto [`Number`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number), di cui una istanza rappresenta tutti i numeri standard che utilizzerai nel tuo JavaScript, ha una serie di metodi utili disponibili per manipolare i numeri. Non copriamo questi in dettaglio in questo articolo perché volevamo mantenerlo come introduzione e coprire solo le basi essenziali per ora; tuttavia, una volta che hai letto questo modulo un paio di volte, vale la pena andare sulle pagine di riferimento degli oggetti e imparare di più su ciò che è disponibile.

Ad esempio, per arrotondare il tuo numero a un numero fisso di cifre decimali, utilizza il metodo [`toFixed()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed). Digita le seguenti righe nella [console](https://firefox-source-docs.mozilla.org/devtools-user/web_console/index.html) del tuo browser:

```js
const lotsOfDecimal = 1.7665849587;
lotsOfDecimal;
const twoDecimalPlaces = lotsOfDecimal.toFixed(2);
twoDecimalPlaces;
```

### Conversione a tipi di dati numerici

A volte potresti finire con un numero memorizzato come tipo stringa, il che rende difficile eseguire calcoli. Questo accade più comunemente quando i dati vengono inseriti in un input di un [form](/it/docs/Learn_web_development/Extensions/Forms), e il [tipo di input è testo](/it/docs/Web/HTML/Reference/Elements/input/text). C'è un modo per risolvere questo problema — passare il valore della stringa al costruttore [`Numero()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/Number) per restituire una versione numerica dello stesso valore.

Ad esempio, prova a digitare queste righe nella tua console:

```js
let myNumber = "74";
myNumber += 3;
```

Ottieni come risultato 743, non 77, perché `myNumber` è in realtà definito come una stringa. Puoi verificarlo digitando il seguente:

```js
typeof myNumber;
```

Per correggere il calcolo, puoi fare questo:

```js
let myNumber = "74";
myNumber = Number(myNumber) + 3;
```

Il risultato è quindi 77, come inizialmente previsto.

## Operatori aritmetici

Gli operatori aritmetici vengono utilizzati per eseguire calcoli matematici in JavaScript:

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
      <td>Aggiunge due numeri insieme.</td>
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
          Restituisce il resto rimasto dopo aver diviso il numero a sinistra in un numero di porzioni intere uguali al numero a destra.
        </p>
      </td>
      <td>
        <p>
          <code>8 % 3</code> (restituisce 2, perché tre entra in 8 due volte, lasciando 2 di resto).
        </p>
      </td>
    </tr>
    <tr>
      <td><code>**</code></td>
      <td>Esponente</td>
      <td>
        Eleva un numero <code>base</code> alla potenza dell'<code>esponente</code>, cioè il numero della <code>base</code> moltiplicato per se stesso, <code>esponente</code> volte.
      </td>
      <td>
        <code>5 ** 2</code> (restituisce <code>25</code>, che è come
        <code>5 * 5</code>).
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> A volte vedrai numeri coinvolti in aritmetica indicati come {{Glossary("Operand", "operandi")}}.

> [!NOTE]
> Potresti a volte vedere esponenti espressi usando il vecchio metodo {{jsxref("Math.pow()")}}, che funziona in un modo molto simile. Ad esempio, in `Math.pow(7, 3)`, `7` è la base e `3` è l'esponente, quindi il risultato dell'espressione è `343`. `Math.pow(7, 3)` è equivalente a `7**3`.

Probabilmente non abbiamo bisogno di insegnarti come fare matematica di base, ma vorremmo verificare la tua comprensione della sintassi coinvolta. Prova a inserire gli esempi di seguito nella tua [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per familiarizzare con la sintassi.

1. Prima prova a inserire alcuni esempi semplici da solo, come

   ```js
   10 + 7;
   9 * 8;
   60 % 3;
   ```

2. Puoi anche provare a dichiarare e inizializzare alcuni numeri all'interno di variabili, e provare a usarli nelle somme — le variabili si comporteranno esattamente come i valori che contengono ai fini della somma. Ad esempio:

   ```js
   const num1 = 10;
   const num2 = 50;
   9 * num1;
   num1 ** 3;
   num2 / num1;
   ```

3. Infine per questa sezione, prova a inserire alcune espressioni più complesse, come:

   ```js
   5 + 10 * 3;
   (num2 % 9) * num1;
   num2 + num1 / 8 + 2;
   ```

Alcune parti di quest'ultimo set di calcoli potrebbero non darti esattamente il risultato che ti aspettavi; la sezione sottostante potrebbe bene dare la risposta al perché.

### Precedenza degli operatori

Guardiamo l'ultimo esempio sopra, assumendo che `num2` abbia il valore 50 e `num1` abbia il valore 10 (come originariamente indicato sopra):

```js
num2 + num1 / 8 + 2;
```

Come essere umano, potresti leggere questo come _"50 più 10 fa 60"_, poi _"8 più 2 fa 10"_, e infine _"60 diviso per 10 fa 6"_.

Ma il browser fa _"10 diviso per 8 fa 1.25"_, poi _"50 più 1,25 più 2 fa 53,25"_.

Questo perché esiste la **precedenza degli operatori** — alcuni operatori vengono applicati prima di altri quando si calcola il risultato di un calcolo (indicato come un'_espressione_, in programmazione). La precedenza degli operatori in JavaScript è la stessa di ciò che viene insegnato nelle lezioni di matematica a scuola — moltiplicazioni e divisioni vengono sempre eseguite per prime, poi addizioni e sottrazioni (il calcolo viene sempre valutato da sinistra a destra).

Se vuoi sovrascrivere la precedenza degli operatori, puoi mettere le parentesi attorno alle parti che vuoi che vengano elaborate esplicitamente per prime. Quindi, per ottenere un risultato di 6, potremmo fare questo:

```js
(num2 + num1) / (8 + 2);
```

Provalo e vedi.

> [!NOTE]
> Un elenco completo di tutti gli operatori JavaScript e la loro precedenza può essere trovato in [Precedenza degli operatori](/it/docs/Web/JavaScript/Reference/Operators/Operator_precedence).

## Operatori di incremento e decremento

A volte potrebbe essere necessario aggiungere o sottrarre ripetutamente uno a o da un valore numerico di una variabile. Questo può essere fatto comodamente usando gli operatori di incremento (`++`) e decremento (`--`). Abbiamo usato `++` nel nostro game "Indovina il numero" nel nostro [primo approccio al JavaScript](/it/docs/Learn_web_development/Core/Scripting/A_first_splash), quando abbiamo aggiunto 1 alla nostra variabile `guessCount` per tenere traccia di quanti tentativi rimangono all'utente dopo ogni turno.

```js
guessCount++;
```

Proviamo a giocare con questi nella tua console. Per cominciare, nota che non puoi applicarli direttamente a un numero, il che potrebbe sembrarti strano, ma stiamo assegnando a una variabile un nuovo valore aggiornato, non operando sul valore stesso. Il seguente restituirà un errore:

```js example-bad
3++;
```

Quindi, puoi incrementare solo una variabile esistente. Prova questo:

```js
let num1 = 4;
num1++;
```

Okay, stranezza numero 2! Quando lo fai, vedrai un valore di 4 restituito — questo perché il browser restituisce il valore corrente, _poi_ incrementa la variabile. Puoi vedere che è stata incrementata se restituisci nuovamente il valore della variabile:

```js
num1;
```

La stessa cosa vale per `--`: prova quanto segue

```js
let num2 = 6;
num2--;
num2;
```

> [!NOTE]
> Puoi fare in modo che il browser faccia il contrario — incrementare/decrementare la variabile _poi_ restituire il valore — mettendo l'operatore all'inizio della variabile invece che alla fine. Prova di nuovo gli esempi sopra, ma questa volta usa `++num1` e `--num2`.

## Operatori di assegnazione

Gli operatori di assegnazione sono operatori che assegnano un valore a una variabile. Abbiamo già usato il più semplice, `=`, molte volte — assegna alla variabile a sinistra il valore indicato a destra:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x = y; // x now contains the same value y contains, 4
```

Ma ci sono tipi più complessi, che forniscono scorciatoie utili per mantenere il tuo codice più pulito ed efficiente. I più comuni sono elencati di seguito:

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
      <td>Assegnazione additiva</td>
      <td>
        Aggiunge il valore a destra al valore variabile a sinistra, poi
        restituisce il nuovo valore della variabile
      </td>
      <td><code>x += 4;</code></td>
      <td><code>x = x + 4;</code></td>
    </tr>
    <tr>
      <td><code>-=</code></td>
      <td>Assegnazione sottrattiva</td>
      <td>
        Sottrae il valore a destra dal valore variabile a sinistra, e restituisce il nuovo valore della variabile
      </td>
      <td><code>x -= 3;</code></td>
      <td><code>x = x - 3;</code></td>
    </tr>
    <tr>
      <td><code>*=</code></td>
      <td>Assegnazione moltiplicativa</td>
      <td>
        Moltiplica il valore variabile a sinistra per il valore a destra, e restituisce il nuovo valore della variabile
      </td>
      <td><code>x *= 3;</code></td>
      <td><code>x = x * 3;</code></td>
    </tr>
    <tr>
      <td><code>/=</code></td>
      <td>Assegnazione divisione</td>
      <td>
        Divide il valore variabile a sinistra per il valore a destra, e restituisce il nuovo valore della variabile
      </td>
      <td><code>x /= 5;</code></td>
      <td><code>x = x / 5;</code></td>
    </tr>
  </tbody>
</table>

Prova a digitare alcuni degli esempi sopra nella tua console, per farti un'idea di come funzionano. In ogni caso, vedi se puoi indovinare quale sarà il valore prima di digitare la seconda riga.

Nota che puoi tranquillamente usare altre variabili sul lato destro di ciascuna espressione, ad esempio:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x *= y; // x now contains the value 12
```

> [!NOTE]
> Ci sono molti [altri operatori di assegnazione disponibili](/it/docs/Web/JavaScript/Guide/Expressions_and_operators#assignment_operators), ma questi sono i principali che dovresti imparare ora.

## Apprendimento attivo: dimensionare una scatola del canvas

In questo esercizio, manipolerai alcuni numeri e operatori per cambiare la dimensione di una scatola. La scatola è disegnata usando un'API del browser chiamata [Canvas API](/it/docs/Web/API/Canvas_API). Non c'è bisogno di preoccuparsi di come funziona — concentrati solo sulla matematica per ora. La larghezza e l'altezza della scatola (in pixel) sono definite dalle variabili `x` e `y`, che inizialmente hanno entrambe un valore di 50.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/maths/editable_canvas.html", '100%', 620)}}

**[Apri in una nuova finestra](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/maths/editable_canvas.html)**

Nel riquadro di codice modificabile sopra, ci sono due righe contrassegnate con un commento che vorremmo che tu aggiornassi per far crescere/ridurre la scatola a determinati dimensioni, utilizzando determinati operatori e/o valori in ciascun caso. Proviamo quanto segue:

- Cambia la riga che calcola `x` in modo che la scatola sia ancora larga 50px, ma il 50 sia calcolato utilizzando i numeri 43 e 7 e un operatore aritmetico.
- Cambia la riga che calcola `y` in modo che la scatola sia alta 75px, ma il 75 sia calcolato utilizzando i numeri 25 e 3 e un operatore aritmetico.
- Cambia la riga che calcola `x` in modo che la scatola sia larga 250px, ma il 250 sia calcolato utilizzando due numeri e l'operatore del resto (modulo).
- Cambia la riga che calcola `y` in modo che la scatola sia alta 150px, ma il 150 sia calcolato utilizzando tre numeri e gli operatori di sottrazione e divisione.
- Cambia la riga che calcola `x` in modo che la scatola sia larga 200px, ma il 200 sia calcolato utilizzando il numero 4 e un operatore di assegnazione.
- Cambia la riga che calcola `y` in modo che la scatola sia alta 200px, ma il 200 sia calcolato utilizzando i numeri 50 e 3, l'operatore di moltiplicazione e l'operatore di assegnazione addizione.
  Non dimenticare di assegnare prima un valore di default a `y` (in una riga separata), affinché l'addizione funzioni come previsto.

Non preoccuparti se rovini completamente il codice. Puoi sempre premere il pulsante Reset per far funzionare di nuovo tutto. Dopo aver risposto correttamente a tutte le domande sopra, sentiti libero di giocare ulteriormente con il codice o creare le tue sfide.

## Operatori di confronto

A volte potremmo voler eseguire test con valori true/false, e poi agire di conseguenza in base al risultato del test — per fare ciò utilizziamo **operatori di confronto**.

| Operatore | Nome                     | Scopo                                                                     | Esempio       |
| --------- | ------------------------ | ------------------------------------------------------------------------- | ------------- |
| `===`     | Uguaglianza rigorosa     | Verifica se i valori a sinistra e a destra sono identici tra loro         | `5 === 2 + 4` |
| `!==`     | Non uguaglianza rigorosa | Verifica se i valori a sinistra e a destra **non** sono identici tra loro | `5 !== 2 + 3` |
| `<`       | Minore di                | Verifica se il valore a sinistra è minore di quello a destra.             | `10 < 6`      |
| `>`       | Maggiore di              | Verifica se il valore a sinistra è maggiore di quello a destra.           | `10 > 20`     |
| `<=`      | Minore o uguale a        | Verifica se il valore a sinistra è minore o uguale a quello a destra.     | `3 <= 2`      |
| `>=`      | Maggiore o uguale a      | Verifica se il valore a sinistra è maggiore o uguale a quello a destra.   | `5 >= 4`      |

> [!NOTE]
> Potresti vedere alcune persone usare `==` e `!=` nei loro test di uguaglianza e non uguaglianza. Questi sono operatori validi in JavaScript, ma differiscono da `===`/`!==`. Le versioni precedenti verificano se i valori sono uguali ma non se i tipi di dati dei valori sono uguali. Le versioni rigorose verificano l'uguaglianza sia dei valori che dei tipi di dati. Le versioni rigorose tendono a generare meno errori, quindi ti consigliamo di usarle.

Se provi a inserire alcuni di questi valori in una console, vedrai che restituiscono tutti valori `true`/`false` — quei booleani che abbiamo menzionato nell'ultimo articolo. Questi sono molto utili, in quanto ci permettono di prendere decisioni nel nostro codice e vengono usati ogni volta che vogliamo fare una scelta di qualche tipo. Ad esempio, i booleani possono essere usati per:

- Visualizzare l'etichetta di testo corretta su un pulsante a seconda che una funzione sia attivata o disattivata
- Visualizzare un messaggio di game over se un gioco è terminato o un messaggio di vittoria se il gioco è stato vinto
- Visualizzare il saluto stagionale corretto a seconda di quale stagione delle festività è
- Zoomare una mappa in avanti o indietro a seconda di quale livello di zoom è selezionato

Vedremo come codificare tale logica quando guarderemo alle istruzioni condizionali in un articolo futuro. Per ora, guardiamo un esempio rapido:

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

Puoi vedere l'operatore di uguaglianza utilizzato appena all'interno della funzione `updateBtn()`. In questo caso, non stiamo testando se due espressioni matematiche hanno lo stesso valore — stiamo testando se il contenuto di testo di un pulsante contiene una determinata stringa — ma è comunque lo stesso principio al lavoro. Se il pulsante attualmente dice "Avvia macchina" quando viene premuto, cambiamo la sua etichetta in "Ferma macchina", e aggiorniamo l'etichetta come appropriato. Se il pulsante attualmente dice "Ferma macchina" quando viene premuto, cambiamo di nuovo il display.

> [!NOTE]
> Un tale controllo che passa tra due stati è generalmente indicato come **toggle**. Passa tra uno stato e un altro — luce accesa, luce spenta, ecc.

## Verifica le tue competenze!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare ulteriori test per verificare che hai memorizzato queste informazioni prima di procedere — vedi [Verifica le tue competenze: Matematica](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Math).

## Sommario

In questo articolo, abbiamo coperto le informazioni fondamentali che devi sapere sui numeri in JavaScript, per ora. Vedrai i numeri usati ancora e ancora durante tutto il tuo apprendimento di JavaScript, quindi è una buona idea affrontare questo argomento ora. Se sei una di quelle persone che non ama la matematica, puoi consolarti con il fatto che questo capitolo era piuttosto breve.

Nel prossimo articolo, esploreremo il testo e come JavaScript ci permette di manipolarlo.

## Vedi anche

- [Numeri e stringhe](/it/docs/Web/JavaScript/Guide/Numbers_and_strings)
- [Espressioni e operatori](/it/docs/Web/JavaScript/Guide/Expressions_and_operators)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}
