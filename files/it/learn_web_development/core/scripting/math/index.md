---
title: Matematica di base in JavaScript — numeri e operatori
short-title: Numeri e operatori
slug: Learn_web_development/Core/Scripting/Math
l10n:
  sourceCommit: 6f1b699dd8891431bbfe0bc3bb803f929fa6032e
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Variables", "Learn_web_development/Core/Scripting/Test_your_skills/Math", "Learn_web_development/Core/Scripting")}}

A questo punto del corso, parliamo della matematica in JavaScript: come usare gli {{Glossary("Operator", "operatori")}} e altre funzionalità per manipolare con successo i numeri secondo le nostre necessità.

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
          <li>Operazioni numeriche di base in JavaScript, come addizione, sottrazione, moltiplicazione e divisione.</li>
          <li>I numeri non sono numeri se sono definiti come stringhe e possono causare calcoli errati.</li>
          <li>Conversione delle stringhe in numeri con <code>Number()</code>.</li>
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

Va bene, forse no. Alcune persone amano la matematica, altre la odiano da quando hanno dovuto imparare le tabelline e la divisione in colonna a scuola, e altre si collocano a metà tra questi due estremi. Ma nessuno può negare che la matematica sia una parte fondamentale della vita, senza la quale non si può andare molto lontano. Questo è particolarmente vero quando si impara a programmare in JavaScript, o in qualsiasi altro linguaggio: gran parte di ciò che si fa dipende dall'elaborazione di dati numerici, dal calcolo di nuovi valori e così via. Non sorprenderà quindi sapere che JavaScript mette a disposizione un insieme completo di funzioni matematiche.

Questo articolo tratta solo gli aspetti di base che è necessario conoscere ora.

### Tipi di numeri

Nella programmazione, persino il semplice sistema numerico decimale che tutti conosciamo bene è più complicato di quanto si possa pensare. Usiamo termini diversi per descrivere diversi tipi di numeri decimali, ad esempio:

- Gli **interi** sono numeri senza parte frazionaria. Possono essere positivi o negativi, ad esempio 10, 400 o -5.
- I **numeri in virgola mobile** (float) hanno un separatore decimale e cifre decimali, ad esempio 12.5 e 56.7786543.

Esistono persino diversi tipi di sistemi numerici. Il decimale è in base 10, cioè usa le cifre da 0 a 9, ma esistono anche:

- Il **binario** — il linguaggio di livello più basso dei computer; usa 0 e 1.
- L'**ottale** — in base 8, usa le cifre da 0 a 7.
- L'**esadecimale** — in base 16, usa le cifre da 0 a 9 e poi le lettere da a a f. Questi numeri potrebbero essere già stati incontrati durante l'impostazione dei [colori in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#hexadecimal_rgb_values).

**Prima di preoccuparsi che il cervello possa fondersi, fermarsi qui!** Per cominciare, in tutto questo corso verranno usati soltanto numeri decimali; raramente, se non mai, sarà necessario pensare ad altri tipi.

La seconda buona notizia è che, a differenza di altri linguaggi di programmazione, JavaScript ha un solo tipo di dati per rappresentare i numeri di base, sia interi sia decimali. Proprio così, {{jsxref("Number")}}. Ciò significa che, qualunque sia il tipo di numeri gestito in JavaScript, viene trattato nello stesso modo.

> [!NOTE]
> JavaScript ha un secondo tipo numerico, {{Glossary("BigInt", "BigInt")}}, usato per interi molto, molto grandi. In questo corso, tuttavia, verranno considerati solo i valori `Number`.

### Per me sono tutti numeri

Proviamo rapidamente alcuni numeri per riprendere familiarità con la sintassi di base necessaria. Inserire i comandi elencati di seguito nella [console JavaScript degli strumenti di sviluppo](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

1. Per prima cosa, dichiariamo un paio di variabili e inizializziamole rispettivamente con un intero e un float, quindi digitiamo nuovamente i nomi delle variabili per verificare che sia tutto in ordine:

   ```js
   const myInt = 5;
   const myFloat = 6.667;
   myInt;
   myFloat;
   ```

2. I valori numerici vengono digitati senza virgolette: provare a dichiarare e inizializzare altre variabili contenenti numeri prima di proseguire.
3. Ora verifichiamo che entrambe le variabili originali abbiano lo stesso tipo di dati. JavaScript dispone di un operatore chiamato {{jsxref("Operators/typeof", "typeof")}} che svolge questa funzione. Inserire le due righe seguenti così come sono:

   ```js
   typeof myInt;
   typeof myFloat;
   ```

   In entrambi i casi dovrebbe essere restituito `"number"`: questo semplifica molto le cose rispetto ad avere tipi di dati diversi per numeri diversi e doverli gestire in modo differente. Meno male!

### Metodi `Number` utili

L'oggetto [`Number`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number), una cui istanza rappresenta tutti i numeri standard usati in JavaScript, fornisce diversi metodi per manipolare i numeri. Non vengono trattati qui in dettaglio, perché per ora si vogliono coprire solo gli aspetti essenziali; tuttavia, dopo aver letto questo modulo un paio di volte, vale la pena consultare le pagine di riferimento dell'oggetto per scoprire cosa è disponibile.

Ad esempio, per arrotondare un numero a un numero fisso di cifre decimali, usare il metodo [`toFixed()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed). Digitare le righe seguenti nella [console](https://firefox-source-docs.mozilla.org/devtools-user/web_console/index.html) del browser:

```js
const lotsOfDecimal = 1.7665849587;
lotsOfDecimal;
const twoDecimalPlaces = lotsOfDecimal.toFixed(2);
twoDecimalPlaces;
```

### Conversione nei tipi di dati numerici

Talvolta un numero può essere memorizzato come tipo stringa, il che rende difficile usarlo nei calcoli. Ciò accade più comunemente quando i dati vengono inseriti in un input di un [modulo](/it/docs/Learn_web_development/Extensions/Forms) e il [tipo di input è text](/it/docs/Web/HTML/Reference/Elements/input/text). Esiste un modo per risolvere il problema: passare il valore stringa al costruttore [`Number()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/Number) per ottenere una versione numerica dello stesso valore.

Ad esempio, provare a digitare queste righe nella console:

```js
let myNumber = "74";
myNumber += 3;
```

Il risultato è 743, non 77, perché `myNumber` è in realtà definito come stringa. È possibile verificarlo digitando quanto segue:

```js
typeof myNumber;
```

Per correggere il calcolo, si può fare così:

```js
let myNumber = "74";
myNumber = Number(myNumber) + 3;
```

Il risultato sarà quindi 77, come previsto inizialmente.

## Operatori aritmetici

Gli operatori aritmetici vengono usati per eseguire calcoli matematici in JavaScript:

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
      <td>Somma due numeri.</td>
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
      <td>Moltiplica due numeri.</td>
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
      <td>Resto (talvolta chiamato modulo)</td>
      <td>
        <p>
          Restituisce il resto dopo aver diviso il numero a sinistra in più
          parti intere uguali al numero a destra.
        </p>
      </td>
      <td>
        <p>
          <code>8 % 3</code> (restituisce 2, poiché 3 è contenuto due volte in
          8, lasciando un resto di 2).
        </p>
      </td>
    </tr>
    <tr>
      <td><code>**</code></td>
      <td>Esponente</td>
      <td>
        Eleva un numero <code>base</code> alla potenza <code>exponent</code>,
        ovvero il numero <code>base</code> moltiplicato per sé stesso
        <code>exponent</code> volte.
      </td>
      <td>
        <code>5 ** 2</code> (restituisce <code>25</code>, che equivale a
        <code>5 * 5</code>).
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> I numeri coinvolti in un'operazione aritmetica vengono talvolta chiamati {{Glossary("Operand", "operandi")}}.

> [!NOTE]
> Talvolta gli esponenti possono essere espressi usando il metodo meno recente {{jsxref("Math.pow()")}}, che funziona in modo molto simile. Ad esempio, in `Math.pow(7, 3)`, `7` è la base e `3` è l'esponente, quindi il risultato dell'espressione è `343`. `Math.pow(7, 3)` equivale a `7**3`.

Probabilmente non è necessario insegnare la matematica di base, ma è utile verificare la comprensione di come sia rappresentata in JavaScript. Provare a inserire gli esempi seguenti nella [console JavaScript degli strumenti di sviluppo](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per acquisire familiarità con la sintassi.

1. Per prima cosa, provare a inserire alcuni semplici esempi personali, come:

   ```js
   10 + 7;
   9 * 8;
   60 % 3;
   ```

2. È anche possibile dichiarare e inizializzare alcuni numeri nelle variabili e usarli nelle somme: le variabili si comporteranno esattamente come i valori che contengono ai fini della somma. Ad esempio:

   ```js
   const num1 = 10;
   const num2 = 50;
   9 * num1;
   num1 ** 3;
   num2 / num1;
   ```

3. Infine, per questa sezione, provare a inserire alcune espressioni più complesse, come:

   ```js
   5 + 10 * 3;
   (num2 % 9) * num1;
   num2 + num1 / 8 + 2;
   ```

Alcune parti di quest'ultima serie di calcoli potrebbero non produrre il risultato atteso; la sezione seguente spiega perché.

### Precedenza degli operatori

Esaminiamo l'ultimo esempio precedente, supponendo che `num2` contenga il valore 50 e `num1` contenga il valore 10, come indicato inizialmente:

```js
num2 + num1 / 8 + 2;
```

Una persona potrebbe leggere questa espressione come _"50 più 10 uguale 60"_, poi _"8 più 2 uguale 10"_ e infine _"60 diviso 10 uguale 6"_.

Il browser, invece, esegue _"10 diviso 8 uguale 1.25"_, poi _"50 più 1.25 più 2 uguale 53.25"_.

Questo avviene a causa della **precedenza degli operatori**: alcuni operatori vengono applicati prima di altri nel calcolo del risultato di un'operazione, chiamata _espressione_ nella programmazione. La precedenza degli operatori in JavaScript è la stessa della matematica di base: in questo caso, prima moltiplicazione e divisione, poi addizione e sottrazione, con il calcolo valutato da sinistra a destra.

Per ignorare la precedenza degli operatori, è possibile racchiudere tra parentesi le parti che devono essere calcolate per prime. Quindi, per ottenere un risultato pari a 6, si potrebbe fare così:

```js
(num2 + num1) / (8 + 2);
```

Provare a inserire la riga precedente nella console per verificarlo.

Se un'espressione include l'operatore di elevamento a potenza (`**`), questo viene valutato dopo le espressioni tra parentesi ma prima degli altri [operatori aritmetici](#operatori_aritmetici). Ad esempio:

```js
2 + 3 ** 2;
```

Inserendo questo nella console, il browser esegue _"3 elevato alla potenza di 2 uguale 9"_, poi _"2 più 9 uguale 11"_.

Provare a inserire le espressioni seguenti nella console per dimostrare che le espressioni tra parentesi vengono valutate prima dell'elevamento a potenza:

```js
4 + 2 ** 3;
(4 + 2) ** 3;
```

Nel primo caso, il browser esegue _"2 elevato alla potenza di 3 uguale 8"_, poi _"8 più 4"_. Nel secondo caso, esegue _"4 più 2 uguale 6"_, poi _"6 elevato alla potenza di 3"_.

> [!NOTE]
> Un elenco completo di tutti gli operatori JavaScript e della loro precedenza è disponibile in [Precedenza degli operatori](/it/docs/Web/JavaScript/Reference/Operators/Operator_precedence).

## Operatori di incremento e decremento

Talvolta è necessario aumentare o diminuire ripetutamente di uno il valore di una variabile numerica. Questo può essere fatto comodamente usando gli operatori di incremento (`++`) e decremento (`--`). `++` è stato usato nel gioco "Indovina il numero" nell'articolo [Primi passi con JavaScript](/it/docs/Learn_web_development/Core/Scripting/A_first_splash), quando è stato aggiunto 1 alla variabile `guessCount` per tenere traccia di quanti tentativi rimangono all'utente dopo ogni turno.

```js
guessCount++;
```

Proviamo a usarli nella console. Per iniziare, si noti che non è possibile applicarli direttamente a un numero; ciò potrebbe sembrare strano, ma viene assegnato a una variabile un nuovo valore aggiornato, non viene eseguita un'operazione sul valore stesso. Il seguente codice restituirà un errore:

```js example-bad
3++;
```

È quindi possibile incrementare solo una variabile esistente. Provare questo:

```js
let num1 = 4;
num1++;
```

Bene, stranezza numero 2! Eseguendo questo codice verrà restituito il valore 4: ciò avviene perché il browser restituisce il valore corrente, _quindi_ incrementa la variabile. È possibile vedere che la variabile è stata incrementata restituendone nuovamente il valore:

```js
num1;
```

Lo stesso vale per `--`: provare quanto segue:

```js
let num2 = 6;
num2--;
num2;
```

> [!NOTE]
> È possibile fare in modo che il browser esegua l'operazione nell'ordine opposto, ovvero incrementi o decrementi la variabile _prima_ di restituire il valore, inserendo l'operatore all'inizio della variabile anziché alla fine. Provare di nuovo gli esempi precedenti, ma questa volta usare `++num1` e `--num2`.

## Operatori di assegnazione

Gli operatori di assegnazione sono operatori che assegnano un valore a una variabile. È già stato usato molte volte il più basilare, `=`, che assegna alla variabile a sinistra il valore indicato a destra:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x = y; // x now contains the same value y contains, 4
```

Esistono però tipi più complessi, che forniscono scorciatoie utili per mantenere il codice più ordinato ed efficiente. I più comuni sono elencati di seguito:

<table class="standard-table no-markdown">
  <thead>
    <tr>
      <th scope="col">Operatore</th>
      <th scope="col">Nome</th>
      <th scope="col">Scopo</th>
      <th scope="col">Esempio</th>
      <th scope="col">Abbreviazione di</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>+=</code></td>
      <td>Assegnazione con addizione</td>
      <td>
        Aggiunge il valore a destra al valore della variabile a sinistra, quindi
        restituisce il nuovo valore della variabile.
      </td>
      <td><code>x += 4;</code></td>
      <td><code>x = x + 4;</code></td>
    </tr>
    <tr>
      <td><code>-=</code></td>
      <td>Assegnazione con sottrazione</td>
      <td>
        Sottrae il valore a destra dal valore della variabile a sinistra e
        restituisce il nuovo valore della variabile.
      </td>
      <td><code>x -= 3;</code></td>
      <td><code>x = x - 3;</code></td>
    </tr>
    <tr>
      <td><code>*=</code></td>
      <td>Assegnazione con moltiplicazione</td>
      <td>
        Moltiplica il valore della variabile a sinistra per il valore a destra e
        restituisce il nuovo valore della variabile.
      </td>
      <td><code>x *= 3;</code></td>
      <td><code>x = x * 3;</code></td>
    </tr>
    <tr>
      <td><code>/=</code></td>
      <td>Assegnazione con divisione</td>
      <td>
        Divide il valore della variabile a sinistra per il valore a destra e
        restituisce il nuovo valore della variabile.
      </td>
      <td><code>x /= 5;</code></td>
      <td><code>x = x / 5;</code></td>
    </tr>
  </tbody>
</table>

Provare a digitare alcuni degli esempi precedenti nella console per farsi un'idea di come funzionano. In ciascun caso, provare a indovinare il risultato prima di digitare la seconda riga.

Si noti che è possibile usare senza problemi altre variabili sul lato destro di ogni espressione, ad esempio:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x *= y; // x now contains the value 12
```

> [!NOTE]
> Sono disponibili molti [altri operatori di assegnazione](/it/docs/Web/JavaScript/Guide/Expressions_and_operators#assignment_operators), ma questi sono quelli di base da imparare ora.

## Ridimensionare una casella canvas

In questo esercizio, verranno manipolati numeri e operatori per modificare la dimensione di una casella. La casella viene disegnata usando un'API del browser chiamata [Canvas API](/it/docs/Web/API/Canvas_API). Non è necessario preoccuparsi di come funzioni: per ora basta concentrarsi sulla matematica. La larghezza e l'altezza della casella, in pixel, sono definite dalle variabili `x` e `y`, a cui viene inizialmente assegnato il valore 50.

```html hidden live-sample___canvas-exercise
<canvas id="my-canvas" width="400" height="200"></canvas>
<p></p>
```

```js live-sample___canvas-exercise
const canvas = document.getElementById("my-canvas");
const para = document.querySelector("p");
const ctx = canvas.getContext("2d");

// Edit the following two lines ONLY
let x = 50;
let y = 50;

ctx.clearRect(0, 0, canvas.width, canvas.height);
ctx.fillStyle = "green";
ctx.fillRect(10, 10, x, y);
para.textContent = `The rectangle is ${x}px wide and ${y}px high.`;
```

{{EmbedLiveSample("canvas-exercise", '100%', 300)}}

Aprire l'esempio precedente in MDN Playground facendo clic sul pulsante **"Play"**, quindi seguire l'elenco di istruzioni seguente per ingrandire o ridurre la casella a determinate dimensioni, usando in ogni caso specifici operatori e/o valori:

- Modificare la riga che calcola `x` affinché la casella sia ancora larga `50px`, ma il valore 50 venga calcolato usando i numeri 43 e 7 e un operatore aritmetico.
- Modificare la riga che calcola `y` affinché la casella sia alta `75px`, ma il valore 75 venga calcolato usando i numeri 25 e 3 e un operatore aritmetico.
- Modificare la riga che calcola `x` affinché la casella sia larga `100px`, ma il valore 100 venga calcolato usando tre numeri e gli operatori di sottrazione e divisione.
- Modificare la riga che calcola `y` affinché la casella sia alta `200px`, ma il valore 200 venga calcolato usando i numeri 2 e `x` e l'operatore di moltiplicazione.

Non importa se il codice viene modificato in modo errato. È sempre possibile premere il pulsante Reset e ricominciare.

## Operatori di confronto

Talvolta è necessario eseguire test vero/falso e poi agire di conseguenza in base al risultato: per farlo, si usano gli **operatori di confronto**.

| Operatore | Nome                   | Scopo                                                                   | Esempio       |
| --------- | ---------------------- | ----------------------------------------------------------------------- | ------------- |
| `===`     | Uguaglianza stretta    | Verifica se i valori a sinistra e a destra sono identici                | `5 === 2 + 4` |
| `!==`     | Disuguaglianza stretta | Verifica se i valori a sinistra e a destra **non** sono identici        | `5 !== 2 + 3` |
| `<`       | Minore di              | Verifica se il valore a sinistra è minore di quello a destra.           | `10 < 6`      |
| `>`       | Maggiore di            | Verifica se il valore a sinistra è maggiore di quello a destra.         | `10 > 20`     |
| `<=`      | Minore o uguale a      | Verifica se il valore a sinistra è minore o uguale a quello a destra.   | `3 <= 2`      |
| `>=`      | Maggiore o uguale a    | Verifica se il valore a sinistra è maggiore o uguale a quello a destra. | `5 >= 4`      |

> [!NOTE]
> Alcune persone usano `==` e `!=` nei test di uguaglianza e disuguaglianza. Sono operatori validi in JavaScript, ma differiscono da `===`/`!==`. Le prime versioni verificano se i valori sono uguali, ma non i rispettivi tipi di dati. Le seconde versioni, strette, verificano l'uguaglianza sia dei valori sia dei tipi di dati. Le versioni strette tendono a produrre meno errori, pertanto se ne consiglia l'uso.

Provando a inserire alcuni di questi valori in una console, si vedrà che restituiscono tutti valori `true`/`false`, ovvero i booleani menzionati nell'articolo precedente. Questi sono molto utili perché permettono di prendere decisioni nel codice e vengono usati ogni volta che è necessario effettuare una scelta. Ad esempio, i booleani possono essere usati per:

- Visualizzare l'etichetta di testo corretta su un pulsante a seconda che una funzionalità sia attivata o disattivata.
- Visualizzare un messaggio di fine gioco se una partita è terminata o un messaggio di vittoria se la partita è stata vinta.
- Visualizzare il saluto stagionale appropriato in base al periodo festivo.
- Ingrandire o ridurre una mappa a seconda del livello di zoom selezionato.

Vedremo come scrivere tale logica quando verranno esaminate le istruzioni condizionali in un articolo futuro. Per ora, osserviamo un rapido esempio:

```html live-sample___conditional
<button>Start machine</button>
<p>The machine is stopped.</p>
```

```js live-sample___conditional
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

{{EmbedLiveSample("conditional", '100%', 100)}}

L'operatore di uguaglianza viene usato all'interno della funzione `updateBtn()`. In questo caso, non viene verificato se due espressioni matematiche hanno lo stesso valore: viene verificato se il contenuto testuale di un pulsante contiene una determinata stringa. Tuttavia, si applica lo stesso principio. Se il contenuto testuale del pulsante è "Start machine" quando viene premuto, l'etichetta viene modificata in "Stop machine" e il testo viene aggiornato di conseguenza. Se il contenuto testuale del pulsante è "Stop machine" quando viene premuto, la visualizzazione viene nuovamente invertita.

> [!NOTE]
> Un controllo di questo tipo, che passa da uno stato all'altro, viene generalmente chiamato **toggle**. Alterna due stati: luce accesa e luce spenta, camminare e correre, ecc.

## Riepilogo

In questo articolo sono state trattate le informazioni fondamentali da conoscere, per ora, sui numeri in JavaScript. I numeri compariranno continuamente durante l'apprendimento di JavaScript, quindi è una buona idea affrontare subito questo argomento. Chi non ama la matematica può consolarsi sapendo che questo capitolo è stato piuttosto breve.

Nel prossimo articolo verranno proposti alcuni test per verificare quanto queste informazioni siano state comprese e memorizzate.

## Vedere anche

- [Numeri e stringhe](/it/docs/Web/JavaScript/Guide/Numbers_and_strings)
- [Espressioni e operatori](/it/docs/Web/JavaScript/Guide/Expressions_and_operators)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Test_your_skills/Variables", "Learn_web_development/Core/Scripting/Test_your_skills/Math", "Learn_web_development/Core/Scripting")}}
