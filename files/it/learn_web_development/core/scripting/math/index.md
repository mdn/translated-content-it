---
title: "Elementi di base della matematica in JavaScript: numeri e operatori"
short-title: Numeri e operatori
slug: Learn_web_development/Core/Scripting/Math
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}

A questo punto del corso, parliamo di matematica in JavaScript — come possiamo usare {{Glossary("Operator", "operatori")}} e altre funzionalità per manipolare con successo i numeri secondo le nostre esigenze.

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
          <li>Operazioni di base con i numeri in JavaScript — sommare, sottrarre, moltiplicare e dividere.</li>
          <li>I numeri non sono numeri se sono definiti come stringhe e possono causare errori nei calcoli.</li>
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

## Tutti amano la matematica

Ok, magari no. Alcuni di noi amano la matematica, altri l'hanno odiata dall'infanzia quando dovevano imparare le tabelline e la divisione lunga a scuola, mentre alcuni di noi si trovano da qualche parte nel mezzo. Ma nessuno di noi può negare che la matematica è una parte fondamentale della vita, senza la quale non possiamo andare molto lontano. Questo è particolarmente vero quando stiamo imparando a programmare in JavaScript (o in qualsiasi altro linguaggio, tra l'altro) — gran parte di ciò che facciamo si basa sull'elaborazione di dati numerici, calcolando nuovi valori e così via, tanto che non sarete sorpresi di apprendere che JavaScript ha una serie completa di funzioni matematiche disponibili.

Questo articolo tratta solo le parti di base che è necessario conoscere ora.

### Tipi di numeri

Nel mondo della programmazione, anche il semplice sistema numerico decimale, che tutti noi conosciamo così bene, è più complicato di quanto si possa pensare. Usiamo termini diversi per descrivere diversi tipi di numeri decimali, ad esempio:

- **Interi** sono numeri senza la parte frazionaria. Possono essere positivi o negativi, ad es., 10, 400 o -5.
- **Numeri in virgola mobile** (float) hanno punti decimali e cifre decimali, ad esempio 12.5, e 56.7786543.

Abbiamo persino diversi tipi di sistemi numerici! Il sistema decimale è in base 10 (significa che usa 0–9 in ogni cifra), ma abbiamo anche cose come:

- **Binario** — Il linguaggio di livello più basso dei computer; 0 e 1.
- **Ottale** — Base 8, utilizza 0–7 in ogni cifra.
- **Esadecimale** — Base 16, utilizza 0–9 e poi a–f in ogni cifra. Potreste aver incontrato questi numeri quando si impostano i [colori in CSS](/it/docs/Learn_web_development/Core/Styling_basics/Values_and_units#hexadecimal_rgb_values).

**Prima che cominciate a preoccuparvi che il vostro cervello stia per sciogliersi, fermatevi qui!** Innanzitutto, ci limiteremo ai numeri decimali durante tutto questo corso; raramente avrete bisogno di iniziare a pensare ad altri tipi, se mai.

La seconda buona notizia è che, a differenza di alcuni altri linguaggi di programmazione, JavaScript ha solo un tipo di dato per i numeri, sia interi che decimali — avete indovinato, {{jsxref("Number")}}. Ciò significa che qualunque tipo di numeri stiate gestendo in JavaScript, li tratterete esattamente nello stesso modo.

> [!NOTE]
> In realtà, JavaScript ha un secondo tipo di numero, {{Glossary("BigInt", "BigInt")}}, utilizzato per interi molto, molto grandi. Ma per gli scopi di questo corso, ci occuperemo solo dei valori `Number`.

### Per me, sono solo numeri

Facciamo rapidamente un po' di pratica con i numeri per rinfrescare le basi della sintassi di cui abbiamo bisogno. Inserite i comandi elencati di seguito nella vostra [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools).

1. Innanzitutto, dichiariamoci un paio di variabili e inizializziamole rispettivamente con un intero e un float, quindi digitate i nomi delle variabili per verificare che tutto sia in ordine:

   ```js
   const myInt = 5;
   const myFloat = 6.667;
   myInt;
   myFloat;
   ```

2. I valori numerici vengono digitati senza virgolette — provate a dichiarare e inizializzare alcune altre variabili contenenti numeri prima di proseguire.
3. Ora verifichiamo che entrambe le nostre variabili originali siano dello stesso tipo di dati. C'è un operatore chiamato {{jsxref("Operators/typeof", "typeof")}} in JavaScript che fa questo. Inserite le due righe qui sotto come mostrato:

   ```js
   typeof myInt;
   typeof myFloat;
   ```

   Dovrebbe tornare `"number"` in entrambi i casi — questo rende le cose molto più facili per noi rispetto al caso in cui diversi numeri avessero tipi di dati diversi e dovessimo gestirli in modi diversi. Per fortuna!

### Metodi utili di Number

L'oggetto [`Number`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number), un'istanza di cui rappresenta tutti i numeri standard che userete nel vostro JavaScript, dispone di una serie di metodi utili per manipolare i numeri. Non li trattiamo in dettaglio in questo articolo perché volevamo mantenerlo come un'introduzione e coprire solo gli elementi essenziali per ora; tuttavia, una volta che avete letto questo modulo un paio di volte, vale la pena andare alle pagine di riferimento dell'oggetto e imparare di più su ciò che è disponibile.

Ad esempio, per arrotondare il vostro numero a un numero fisso di cifre decimali, utilizzate il metodo [`toFixed()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/toFixed). Inserite le righe seguenti nella [console](https://firefox-source-docs.mozilla.org/devtools-user/web_console/index.html) del vostro browser:

```js
const lotsOfDecimal = 1.766584958675746364;
lotsOfDecimal;
const twoDecimalPlaces = lotsOfDecimal.toFixed(2);
twoDecimalPlaces;
```

### Conversione in tipi di dati numerici

A volte potreste ottenere un numero che è memorizzato come tipo stringa, il che rende difficile eseguire calcoli con esso. Questo accade più comunemente quando i dati vengono inseriti in un campo di input di un [modulo](/it/docs/Learn_web_development/Extensions/Forms), e il [tipo di input è testo](/it/docs/Web/HTML/Reference/Elements/input/text). Esiste un modo per risolvere questo problema — passando il valore stringa nel costruttore [`Number()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Number/Number) per restituire una versione numerica dello stesso valore.

Ad esempio, provate a digitare queste righe nella vostra console:

```js
let myNumber = "74";
myNumber += 3;
```

Si ottiene il risultato 743, non 77, perché `myNumber` è effettivamente definito come una stringa. Potete verificare questo digitando il seguente:

```js
typeof myNumber;
```

Per correggere il calcolo, potete fare questo:

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
      <td>Sottrae il numero di destra da quello di sinistra.</td>
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
      <td>Divide il numero di sinistra per quello di destra.</td>
      <td><code>10 / 5</code></td>
    </tr>
    <tr>
      <td><code>%</code></td>
      <td>Resto (a volte chiamato modulo)</td>
      <td>
        <p>
          Restituisce il resto rimasto dopo aver diviso il numero di sinistra in
          un numero di porzioni intere uguali al numero di destra.
        </p>
      </td>
      <td>
        <p>
          <code>8 % 3</code> (resta 2, in quanto tre va in 8 due volte, lasciando
          2 come resto).
        </p>
      </td>
    </tr>
    <tr>
      <td><code>**</code></td>
      <td>Esponente</td>
      <td>
        Eleva un numero di <code>base</code> alla potenza di
        <code>esponente</code>, cioè il numero di <code>base</code> moltiplicato
        per se stesso, <code>esponente</code> volte.
      </td>
      <td>
        <code>5 ** 2</code> (restituisce <code>25</code>, che è lo stesso di
        <code>5 * 5</code>).
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> A volte vedrete i numeri coinvolti in un'operazione aritmetica riferiti come {{Glossary("Operand", "operandi")}}.

> [!NOTE]
> A volte potreste vedere gli esponenti espressi usando il metodo più vecchio {{jsxref("Math.pow()")}}, che funziona in modo molto simile. Ad esempio, in `Math.pow(7, 3)`, `7` è la base e `3` è l'esponente, quindi il risultato dell'espressione è `343`. `Math.pow(7, 3)` è equivalente a `7**3`.

Probabilmente non abbiamo bisogno di insegnarvi a fare la matematica di base, ma vorremmo testare la vostra comprensione della sintassi coinvolta. Provate a inserire gli esempi seguenti nella vostra [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) per familiarizzare con la sintassi.

1. Per prima cosa provate a inserire alcuni esempi semplici di vostro, come

   ```js
   10 + 7;
   9 * 8;
   60 % 3;
   ```

2. Potete anche provare a dichiarare e inizializzare alcuni numeri all'interno delle variabili e provare a usarli nelle somme — le variabili si comporteranno esattamente come i valori che contengono ai fini della somma. Ad esempio:

   ```js
   const num1 = 10;
   const num2 = 50;
   9 * num1;
   num1 ** 3;
   num2 / num1;
   ```

3. Infine, per questa sezione, provate a inserire alcune espressioni più complesse, come:

   ```js
   5 + 10 * 3;
   (num2 % 9) * num1;
   num2 + num1 / 8 + 2;
   ```

Parti di quest'ultimo insieme di calcoli potrebbero non darvi esattamente il risultato che vi aspettavate; la sezione successiva potrebbe darvi la risposta sul perché.

### Precedenza degli operatori

Guardiamo l'ultimo esempio sopra, assumendo che `num2` contenga il valore 50 e `num1` il valore 10 (come dichiarato originariamente sopra):

```js
num2 + num1 / 8 + 2;
```

Come essere umano, potreste leggere questo come _"50 più 10 fa 60"_, poi _"8 più 2 fa 10"_, e infine _"60 diviso 10 fa 6"_.

Ma il browser fa _"10 diviso 8 fa 1.25"_, poi _"50 più 1.25 più 2 fa 53.25"_.

Questo è a causa della **precedenza degli operatori** — alcuni operatori vengono applicati prima di altri quando si calcola il risultato di un calcolo (riferito come _espressione_ in programmazione). La precedenza degli operatori in JavaScript è la stessa di quella insegnata nelle classi di matematica a scuola — moltiplicare e dividere sono sempre fatti prima, poi sommare e sottrarre (il calcolo viene sempre valutato da sinistra a destra).

Se volete sovrascrivere la precedenza degli operatori, potete mettere parentesi attorno alle parti che volete vengano trattate esplicitamente per prime. Quindi per ottenere un risultato di 6, potremmo fare questo:

```js
(num2 + num1) / (8 + 2);
```

Provate e vedete.

> [!NOTE]
> Un elenco completo di tutti gli operatori JavaScript e della loro precedenza può essere trovato in [Precedenza degli operatori](/it/docs/Web/JavaScript/Reference/Operators/Operator_precedence).

## Operatori di incremento e decremento

A volte si vorrà aggiungere o sottrarre ripetutamente uno da un valore variabile numerico. Ciò può essere fatto comodamente usando gli operatori di incremento (`++`) e decremento (`--`). Abbiamo usato `++` nel nostro gioco "Indovina il numero" nel nostro [primo contatto con JavaScript](/it/docs/Learn_web_development/Core/Scripting/A_first_splash), quando abbiamo aggiunto 1 alla nostra variabile `guessCount` per tenere traccia di quante volte l'utente ha ancora a disposizione dopo ogni turno.

```js
guessCount++;
```

Proviamo a giocare con questi nella vostra console. Per cominciare, notate che non potete applicarli direttamente a un numero, il che potrebbe sembrare strano, ma stiamo assegnando a una variabile un nuovo valore aggiornato, non operando sul valore stesso. Il seguente restituirà un errore:

```js example-bad
3++;
```

Quindi, potete solo incrementare una variabile esistente. Provate questo:

```js
let num1 = 4;
num1++;
```

Ok, stranezza numero 2! Quando fate questo, vedrete che viene restituito un valore di 4 — questo perché il browser restituisce il valore corrente, _poi_ incrementa la variabile. Potete vedere che è stata incrementata se restituite di nuovo il valore della variabile:

```js
num1;
```

Lo stesso vale per `--` : provate quanto segue

```js
let num2 = 6;
num2--;
num2;
```

> [!NOTE]
> Potete far sì che il browser lo faccia nell'ordine opposto — incrementare/decrementare la variabile _poi_ restituire il valore — mettendo l'operatore all'inizio della variabile invece che alla fine. Provate di nuovo gli esempi di sopra, ma questa volta utilizzate `++num1` e `--num2`.

## Operatori di assegnazione

Gli operatori di assegnazione sono operatori che assegnano un valore a una variabile. Abbiamo già usato il più basilare `=`, molte volte — assegna alla variabile sulla sinistra il valore dichiarato sulla destra:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x = y; // x now contains the same value y contains, 4
```

Ma ci sono alcuni tipi più complessi, che forniscono scorciatoie utili per mantenere il codice più ordinato ed efficiente. I più comuni sono elencati di seguito:

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
      <td>Assegnazione di addizione</td>
      <td>
        Aggiunge il valore sulla destra al valore variabile sulla sinistra e
        poi restituisce il nuovo valore variabile
      </td>
      <td><code>x += 4;</code></td>
      <td><code>x = x + 4;</code></td>
    </tr>
    <tr>
      <td><code>-=</code></td>
      <td>Assegnazione di sottrazione</td>
      <td>
        Sottrae il valore sulla destra dal valore variabile sulla sinistra e
        restituisce il nuovo valore variabile
      </td>
      <td><code>x -= 3;</code></td>
      <td><code>x = x - 3;</code></td>
    </tr>
    <tr>
      <td><code>*=</code></td>
      <td>Assegnazione di moltiplicazione</td>
      <td>
        Moltiplica il valore variabile sulla sinistra per il valore sulla destra e
        restituisce il nuovo valore variabile
      </td>
      <td><code>x *= 3;</code></td>
      <td><code>x = x * 3;</code></td>
    </tr>
    <tr>
      <td><code>/=</code></td>
      <td>Assegnazione di divisione</td>
      <td>
        Divide il valore variabile sulla sinistra per il valore sulla destra e
        restituisce il nuovo valore variabile
      </td>
      <td><code>x /= 5;</code></td>
      <td><code>x = x / 5;</code></td>
    </tr>
  </tbody>
</table>

Provate a digitare alcuni degli esempi sopra nella vostra console, per avere un'idea di come funzionano. In ciascun caso, vedete se riuscite a indovinare quale sia il valore prima di digitare la seconda riga.

Notate che potete utilizzare tranquillamente altre variabili sul lato destro di ciascuna espressione, ad esempio:

```js
let x = 3; // x contains the value 3
let y = 4; // y contains the value 4
x *= y; // x now contains the value 12
```

> [!NOTE]
> Ci sono molti [altri operatori di assegnazione disponibili](/it/docs/Web/JavaScript/Guide/Expressions_and_operators#assignment_operators), ma questi sono i fondamenti che dovreste imparare ora.

## Apprendimento attivo: dimensionare un riquadro canvas

In questo esercizio, manipolerete alcuni numeri e operatori per cambiare la dimensione di un riquadro. Il riquadro è disegnato utilizzando un'API del browser chiamata [Canvas API](/it/docs/Web/API/Canvas_API). Non c'è bisogno di preoccuparsi di come funziona — concentratevi solo sulla matematica per ora. La larghezza e l'altezza del riquadro (in pixel) sono definite dalle variabili `x` e `y`, che inizialmente vengono date con un valore di 50.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/maths/editable_canvas.html", '100%', 620)}}

**[Apri in una nuova finestra](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/maths/editable_canvas.html)**

Nel riquadro codice modificabile sopra, ci sono due righe contrassegnate con un commento che vi invitiamo ad aggiornare per far crescere/diminuire il riquadro a determinate dimensioni, utilizzando determinati operatori e/o valori in ciascun caso. Proviamo quanto segue:

- Modificate la riga che calcola `x` affinché il riquadro sia ancora largo 50px, ma il 50 è calcolato usando i numeri 43 e 7 e un operatore aritmetico.
- Modificate la riga che calcola `y` affinché il riquadro sia alto 75px, ma il 75 è calcolato usando i numeri 25 e 3 e un operatore aritmetico.
- Modificate la riga che calcola `x` affinché il riquadro sia largo 250px, ma il 250 è calcolato usando due numeri e l'operatore di resto (modulo).
- Modificate la riga che calcola `y` affinché il riquadro sia alto 150px, ma il 150 è calcolato usando tre numeri e gli operatori di sottrazione e divisione.
- Modificate la riga che calcola `x` affinché il riquadro sia largo 200px, ma il 200 è calcolato usando il numero 4 e un operatore di assegnazione.
- Modificate la riga che calcola `y` affinché il riquadro sia alto 200px, ma il 200 è calcolato usando i numeri 50 e 3, l'operatore di moltiplicazione e l'operatore di assegnazione di addizione. Non dimenticate di assegnare prima un valore predefinito a `y` (in una riga separata), in modo che l'addizione funzioni come previsto.

Non preoccupatevi se rovinate totalmente il codice. Potete sempre premere il pulsante Ripristina per farlo funzionare di nuovo. Dopo aver risposto correttamente a tutte le domande sopra, sentitevi liberi di giocare ulteriormente con il codice o di creare le vostre sfide.

## Operatori di confronto

A volte vorremo eseguire test che restituiscano vero/falso, quindi agire di conseguenza a seconda del risultato di quel test — per fare questo usiamo gli **operatori di confronto**.

| Operatore | Nome                     | Scopo                                                                       | Esempio       |
| --------- | ------------------------ | --------------------------------------------------------------------------- | ------------- |
| `===`     | Uguaglianza rigorosa     | Verifica se i valori di sinistra e destra sono identici tra loro            | `5 === 2 + 4` |
| `!==`     | Disuguaglianza rigorosa  | Verifica se i valori di sinistra e destra **non** sono identici tra loro    | `5 !== 2 + 3` |
| `<`       | Minore di                | Verifica se il valore di sinistra è più piccolo di quello di destra.        | `10 < 6`      |
| `>`       | Maggiore di              | Verifica se il valore di sinistra è maggiore di quello di destra.           | `10 > 20`     |
| `<=`      | Minore o uguale a        | Verifica se il valore di sinistra è più piccolo o uguale a quello di destra.| `3 <= 2`      |
| `>=`      | Maggiore o uguale a      | Verifica se il valore di sinistra è maggiore o uguale a quello di destra.   | `5 >= 4`      |

> [!NOTE]
> Potreste vedere alcune persone usare `==` e `!=` nei loro test per uguaglianza e disuguaglianza. Questi sono operatori validi in JavaScript, ma differiscono da `===`/`!==`. Le versioni precedenti testano se i valori sono uguali ma non se i tipi di dati dei valori sono uguali. Le versioni rigorose testano l'uguaglianza sia dei valori che dei loro tipi di dati. Le versioni rigorose tendono a provocare meno errori, quindi vi consigliamo di usarle.

Se provate a inserire alcuni di questi valori in una console, vedrete che tutti restituiscono valori `true`/`false` — quei booleani che abbiamo menzionato nell'ultimo articolo. Questi sono molto utili, poiché ci permettono di prendere decisioni nel nostro codice, e vengono usati ogni volta che vogliamo fare una scelta di qualche tipo. Ad esempio, i booleani possono essere utilizzati per:

- Visualizzare il testo corretto su un pulsante a seconda che una funzionalità sia attivata o disattivata
- Visualizzare un messaggio di game over se un gioco è finito o un messaggio di vittoria se il gioco è stato vinto
- Visualizzare il saluto stagionale corretto a seconda di quale stagione di vacanza è
- Ingrandire o ridurre una mappa a seconda del livello di zoom selezionato

Vedremo come codificare questa logica quando esamineremo le istruzioni condizionali in un futuro articolo. Per ora, esaminiamo un esempio rapido:

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

È possibile vedere l'operatore di uguaglianza utilizzato proprio all'interno della funzione `updateBtn()`. In questo caso, non stiamo testando se due espressioni matematiche hanno lo stesso valore — stiamo testando se il contenuto del testo di un pulsante contiene una certa stringa — ma è comunque lo stesso principio in gioco. Se il pulsante attualmente dice "Avvia macchina" quando viene premuto, cambiamo l'etichetta in "Ferma macchina" e aggiorniamo l'etichetta di conseguenza. Se il pulsante attualmente dice "Ferma macchina" quando viene premuto, cambiamo l'etichetta di nuovo.

> [!NOTE]
> Un controllo che alterna tra due stati è generalmente chiamato **toggle**. Ciò alterna tra uno stato e l'altro - luce accesa, luce spenta, ecc.

## Testa le tue abilità!

Hai raggiunto la fine di questo articolo, ma riesci a ricordare le informazioni più importanti? Puoi trovare altri test per verificare di aver conservato queste informazioni prima di procedere — consulta [Testa le tue abilità: Matematica](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Math).

## Sommario

In questo articolo, abbiamo trattato le informazioni fondamentali che è necessario conoscere sui numeri in JavaScript, per ora. Vedrete i numeri utilizzati ancora e ancora, durante tutto il vostro apprendimento di JavaScript, quindi è una buona idea affrontare questo ora. Se siete una di quelle persone a cui non piace la matematica, potete confortarvi con il fatto che questo capitolo era piuttosto breve.

Nell'articolo successivo, esploreremo il testo e come JavaScript ci permette di manipolarlo.

## Vedi anche

- [Numeri e stringhe](/it/docs/Web/JavaScript/Guide/Numbers_and_strings)
- [Espressioni e operatori](/it/docs/Web/JavaScript/Guide/Expressions_and_operators)

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting/Strings", "Learn_web_development/Core/Scripting")}}
