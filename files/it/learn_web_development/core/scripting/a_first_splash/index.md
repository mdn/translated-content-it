---
title: Un primo tuffo in JavaScript
short-title: Esercitazione su JavaScript
slug: Learn_web_development/Core/Scripting/A_first_splash
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_is_JavaScript", "Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting")}}

Ora che ha appreso qualcosa sulla teoria di JavaScript e cosa può fare con esso, le daremo un'idea di quale sia il processo di creazione di un semplice programma JavaScript, guidandola attraverso un tutorial pratico. Qui costruirà un semplice gioco "Indovina il numero", passo dopo passo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Pensare come un programmatore.</li>
          <li>Esperienza su com'è scrivere in JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Vogliamo stabilire chiaramente le aspettative: non ci si aspetta che lei impari JavaScript entro la fine di questo articolo, o che comprenda tutto il codice che le chiediamo di scrivere. Invece, vogliamo darle un'idea di come le funzionalità di JavaScript si integrino tra loro, e di com'è scrivere codice JavaScript. In articoli successivi tornerà su tutte le funzionalità mostrate qui in modo molto più dettagliato, quindi non si preoccupi se non comprende tutto immediatamente!

> [!NOTE]
> Molte delle funzionalità del codice che vedrà in JavaScript sono le stesse di altri linguaggi di programmazione — funzioni, cicli, ecc. La sintassi del codice appare diversa, ma i concetti sono in gran parte gli stessi.

## Pensare come un programmatore

Una delle cose più difficili da imparare nella programmazione non è la sintassi che deve imparare, ma come applicarla per risolvere problemi del mondo reale. Deve iniziare a pensare come un programmatore — ciò generalmente implica leggere le descrizioni di ciò che il suo programma deve fare, determinare quali funzionalità del codice sono necessarie per realizzare quelle cose e come farle lavorare insieme.

Questo richiede una miscela di duro lavoro, esperienza con la sintassi di programmazione e pratica — oltre a un po' di creatività. Più scrive codice, meglio diventerà. Non possiamo promettere che svilupperà un "cervello da programmatore" in cinque minuti, ma le daremo molte opportunità di praticare il pensiero da programmatore lungo il corso.

Con questo in mente, diamo un'occhiata all'esempio che costruiremo in questo articolo, e rivediamo il processo generale di scomposizione in compiti tangibili.

## Esempio — Gioco di indovinare il numero

In questo articolo le mostreremo come costruire il semplice gioco che può vedere qui sotto:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game", 900, 300)}}

Provi a giocarci — familiarizzi con il gioco prima di continuare.

Immaginiamo che il suo capo le abbia dato il seguente incarico per la creazione di questo gioco:

> Voglio che crei un semplice gioco del tipo "indovina il numero". Dovrebbe scegliere un numero casuale tra 1 e 100, quindi sfidare il giocatore a indovinare il numero in 10 tentativi. Dopo ogni tentativo, dovrebbe essere detto al giocatore se ha ragione o torto, e se ha sbagliato, se il suo indovinello era troppo basso o troppo alto. Dovrebbe anche dire al giocatore quali numeri ha indovinato in precedenza. Il gioco terminerà una volta che il giocatore indovina correttamente o una volta esauriti i tentativi. Quando il gioco termina, al giocatore dovrebbe essere data un'opzione per ricominciare a giocare.

Di fronte a questa descrizione, la prima cosa che possiamo fare è iniziare a scomporla in semplici compiti realizzabili, con il maggiore approccio da programmatore possibile:

1. Generare un numero casuale tra 1 e 100.
2. Registrare il numero di turno del giocatore. Iniziare da 1.
3. Fornire al giocatore un modo per indovinare quale sia il numero.
4. Una volta inviato un indovinello, prima registrarlo da qualche parte affinché l'utente possa vedere i suoi tentativi precedenti.
5. Successivamente, verificare se è il numero corretto.
6. Se è corretto:

   1. Visualizzare un messaggio di congratulazioni.
   2. Impedire al giocatore di inserire ulteriori indovinelli (questo rovinerebbe il gioco).
   3. Visualizzare un controllo che consente al giocatore di ricominciare il gioco.

7. Se è sbagliato e il giocatore ha ancora tentativi:

   1. Dire al giocatore che ha torto e se il suo indovinello era troppo alto o troppo basso.
   2. Consentire di inserire un altro indovinello.
   3. Incrementare il numero del turno di 1.

8. Se è sbagliato e il giocatore non ha più tentativi:

   1. Dire al giocatore che è game over.
   2. Impedire al giocatore di inserire ulteriori indovinelli (questo rovinerebbe il gioco).
   3. Visualizzare un controllo che consente al giocatore di ricominciare il gioco.

9. Una volta che il gioco ricomincia, assicurarsi che la logica del gioco e l'interfaccia utente siano completamente resettate, quindi tornare al passaggio 1.

Procediamo ora, esaminando come possiamo trasformare questi passaggi in codice, costruendo l'esempio ed esplorando le funzionalità di JavaScript strada facendo.

### Impostazione iniziale

Per iniziare questo tutorial, le chiediamo di fare una copia locale del file [number-guessing-game-start.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html) ([vedi la versione live qui](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html)). Lo apra sia nel suo editor di testo che nel suo browser web. Al momento vedrà un semplice titolo, un paragrafo di istruzioni e un modulo per inserire un indovinello, ma il modulo attualmente non farà nulla.

Il luogo in cui aggiungeremo tutto il nostro codice è all'interno dell'elemento {{htmlelement("script")}} alla fine dell'HTML:

```html
<script>
  // Your JavaScript goes here
</script>
```

### Aggiungere variabili per memorizzare i nostri dati

Iniziamo. Prima di tutto, aggiunga le seguenti righe all'interno dell'elemento {{htmlelement("script")}}:

```js
let randomNumber = Math.floor(Math.random() * 100) + 1;

const guesses = document.querySelector(".guesses");
const lastResult = document.querySelector(".lastResult");
const lowOrHi = document.querySelector(".lowOrHi");

const guessSubmit = document.querySelector(".guessSubmit");
const guessField = document.querySelector(".guessField");

let guessCount = 1;
let resetButton;
```

Questa sezione del codice imposta le variabili e le costanti di cui abbiamo bisogno per memorizzare i dati che il nostro programma utilizzerà.

Le variabili sono fondamentalmente nomi per valori (come numeri o stringhe di testo). Si crea una variabile con la parola chiave `let` seguita da un nome per la sua variabile.

Le costanti vengono utilizzate anche per dare un nome ai valori, ma a differenza delle variabili, non può modificare il valore una volta impostato. In questo caso, utilizziamo costanti per memorizzare riferimenti a parti della nostra interfaccia utente. Il testo all'interno di alcuni di questi elementi potrebbe cambiare, ma ciascuna costante farà sempre riferimento allo stesso elemento HTML con cui è stata inizializzata. Si crea una costante con la parola chiave `const` seguita da un nome per la costante.

Si può assegnare un valore alla sua variabile o costante con un segno di uguale (`=`) seguito dal valore che si desidera dare.

Nel nostro esempio:

- La prima variabile — `randomNumber` — è assegnata a un numero casuale tra 1 e 100, calcolato usando un algoritmo matematico.
- Le prime tre costanti sono fatte ognuna per memorizzare un riferimento ai paragrafi dei risultati nel nostro HTML, e sono usate per inserire valori nei paragrafi più avanti nel codice (noti come sono all'interno di un elemento `<div>`, che è esso stesso usato per selezionare tutti e tre in seguito per il ripristino, quando ricominciamo il gioco):

  ```html
  <div class="resultParas">
    <p class="guesses"></p>
    <p class="lastResult"></p>
    <p class="lowOrHi"></p>
  </div>
  ```

- Le due costanti successive memorizzano riferimenti al campo di testo del modulo e al pulsante di invio, e sono utilizzate per controllare l'invio dell'indovinello più avanti.

  ```html
  <label for="guessField">Enter a guess: </label>
  <input type="number" id="guessField" class="guessField" />
  <input type="submit" value="Submit guess" class="guessSubmit" />
  ```

- Le nostre ultime due variabili memorizzano un conteggio degli indovinelli pari a 1 (usato per tenere traccia di quanti indovinelli ha avuto il giocatore) e un riferimento a un pulsante di ripristino che non esiste ancora (ma esisterà più tardi).

> [!NOTE]
> Imparerà molto di più su variabili e costanti in seguito nel corso, a partire dall'articolo [Memorizzare le informazioni di cui hai bisogno — Variabili](/it/docs/Learn_web_development/Core/Scripting/Variables).

### Funzioni

Successivamente, aggiunga il seguente codice sotto il suo JavaScript precedente:

```js
function checkGuess() {
  alert("I am a placeholder");
}
```

Le funzioni sono blocchi di codice riutilizzabili che è possibile scrivere una volta ed eseguire più volte, risparmiando la necessità di continuare a ripetere il codice tutto il tempo. Questo è davvero utile. Ci sono diversi modi per definire funzioni, ma per ora ci concentreremo su un tipo semplice. Qui abbiamo definito una funzione utilizzando la parola chiave `function`, seguita da un nome, con parentesi messe dopo di esso. Dopo di ciò, mettiamo due parentesi graffe (`{ }`). All'interno delle parentesi graffe va tutto il codice che desideriamo eseguire ogni volta che chiamiamo la funzione.

Quando vogliamo eseguire il codice, digitiamo il nome della funzione seguito dalle parentesi.

Provveiamo a farlo ora. Salvi il suo codice e aggiorni la pagina nel suo browser. Poi entri negli [strumenti per sviluppatori console JavaScript](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) ed inserisca la seguente riga:

```js
checkGuess();
```

Dopo aver premuto <kbd>Return</kbd>/<kbd>Enter</kbd>, dovrebbe comparire un avviso che dice `I am a placeholder`; abbiamo definito una funzione nel nostro codice che crea un avviso ogni volta che lo chiamiamo.

> [!NOTE]
> Imparerà molto di più sulle funzioni in seguito nell'articolo [Funzioni — blocchi di codice riutilizzabili](/it/docs/Learn_web_development/Core/Scripting/Functions).

### Operatori

Gli operatori JavaScript ci permettono di eseguire test, effettuare calcoli, unire stringhe, e altre cose simili.

Se non l'ha ancora fatto, salvi il suo codice, aggiorni la pagina nel suo browser, e apra la [console degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools). Poi possiamo provare a digitare gli esempi mostrati di seguito — digiti ciascuno nell'esatta forma mostrata nelle colonne "Esempio", premendo <kbd>Return</kbd>/<kbd>Enter</kbd> dopo ogni uno, e vedere quali risultati ritornano.

Per prima cosa guardiamo agli operatori aritmetici, per esempio:

| Operatore | Nome           | Esempio   |
| --------- | -------------- | --------- |
| `+`       | Addizione      | `6 + 9`   |
| `-`       | Sottrazione    | `20 - 15` |
| `*`       | Moltiplicazione| `3 * 7`   |
| `/`       | Divisione      | `10 / 5`  |

Ci sono anche alcuni operatori scorciatoia disponibili, chiamati [operatori di assegnazione compositi](/it/docs/Web/JavaScript/Reference/Operators#assignment_operators). Ad esempio, se vuole aggiungere un nuovo numero a uno esistente e restituire il risultato, potrebbe fare così:

```js
let number1 = 1;
number1 += 2;
```

Questo è equivalente a

```js
let number2 = 1;
number2 = number2 + 2;
```

Quando eseguiamo test di verifiche vere/falsi (per esempio all'interno di condizionali — vedi [sotto](#condizionali)) utilizziamo gli [operatori di confronto](/it/docs/Web/JavaScript/Reference/Operators). Per esempio:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Operatore</th>
      <th scope="col">Nome</th>
      <th scope="col">Esempio</th>
    </tr>
    <tr>
      <td><code>===</code></td>
      <td>Uguaglianza stretta (è esattamente lo stesso?)</td>
      <td>
        <pre class="brush: js">
5 === 2 + 4 // false
'Chris' === 'Bob' // false
5 === 2 + 3 // true
2 === '2' // false; numero contro stringa
</pre
        >
      </td>
    </tr>
    <tr>
      <td><code>!==</code></td>
      <td>Disuguaglianza (non è lo stesso?)</td>
      <td>
        <pre class="brush: js">
5 !== 2 + 4 // true
'Chris' !== 'Bob' // true
5 !== 2 + 3 // false
2 !== '2' // true; numero contro stringa
</pre
        >
      </td>
    </tr>
    <tr>
      <td><code>&#x3C;</code></td>
      <td>Minore di</td>
      <td>
        <pre class="brush: js">
6 &#x3C; 10 // true
20 &#x3C; 10 // false</pre
        >
      </td>
    </tr>
    <tr>
      <td><code>></code></td>
      <td>Maggiore di</td>
      <td>
        <pre class="brush: js">
6 > 10 // false
20 > 10 // true</pre
        >
      </td>
    </tr>
  </thead>
</table>

### Stringhe di testo

Le stringhe sono utilizzate per rappresentare il testo. Abbiamo già visto una stringa variabile: nel seguente codice, `"I am a placeholder"` è una stringa:

```js
function checkGuess() {
  alert("I am a placeholder");
}
```

Si possono dichiarare stringhe utilizzando le virgolette doppie (`"`) o le virgolette singole (`'`), ma deve usare la stessa forma per l'inizio e la fine di una singola dichiarazione di stringa: non può scrivere `"I am a placeholder'`.

Può anche dichiarare stringhe usando le virgolette rovesciate (`` ` ``). Le stringhe dichiarate in questo modo sono chiamate _template literals_ e hanno alcune proprietà speciali. In particolare, può incorporare altre variabili o addirittura espressioni al loro interno:

```js
const name = "Mahalia";

const greeting = `Hello ${name}`;
```

Questo le offre un meccanismo per unire stringhe insieme.

### Condizionali

Tornando alla nostra funzione `checkGuess()`, penso di poter dire con certezza che non vogliamo che si limiti a sputare un messaggio di sostituzione. Vogliamo che controlli se un'indovinello del giocatore sia corretto o meno, e risponda in modo appropriato.

A questo punto, sostituisca la sua attuale funzione `checkGuess()` con questa versione invece:

```js
function checkGuess() {
  const userGuess = Number(guessField.value);
  if (guessCount === 1) {
    guesses.textContent = "Previous guesses:";
  }
  guesses.textContent = `${guesses.textContent} ${userGuess}`;

  if (userGuess === randomNumber) {
    lastResult.textContent = "Congratulations! You got it right!";
    lastResult.style.backgroundColor = "green";
    lowOrHi.textContent = "";
    setGameOver();
  } else if (guessCount === 10) {
    lastResult.textContent = "!!!GAME OVER!!!";
    lowOrHi.textContent = "";
    setGameOver();
  } else {
    lastResult.textContent = "Wrong!";
    lastResult.style.backgroundColor = "red";
    if (userGuess < randomNumber) {
      lowOrHi.textContent = "Last guess was too low!";
    } else if (userGuess > randomNumber) {
      lowOrHi.textContent = "Last guess was too high!";
    }
  }

  guessCount++;
  guessField.value = "";
  guessField.focus();
}
```

Questo è un sacco di codice — phew! Passiamo attraverso ogni sezione e spieghiamo cosa fa.

- La prima riga dichiara una variabile chiamata `userGuess` e ne imposta il valore al valore attuale inserito all'interno del campo di testo. Passiamo anche questo valore attraverso il costruttore incorporato `Number()`, giusto per essere sicuri che il valore sia sicuramente un numero. Poiché non stiamo cambiando questa variabile, la dichiareremo usando `const`.
- Successivamente, incontriamo il nostro primo blocco di codice condizionale. Un blocco di codice condizionale consente di eseguire codice in modo selettivo, a seconda che una determinata condizione sia vera o meno. Sembra un po' come una funzione, ma non lo è. La forma più semplice di blocco condizionale inizia con la parola chiave `if`, quindi alcune parentesi, poi alcune parentesi graffe. Dentro le parentesi, includiamo un test. Se il test restituisce `true`, eseguiamo il codice all'interno delle parentesi graffe. Se no, non lo facciamo, e passiamo al prossimo pezzetto di codice. In questo caso, il test sta verificando se la variabile `guessCount` è uguale a `1` (cioè, se questo è il primo turno del giocatore o no):

  ```js
  guessCount === 1;
  ```

  Se lo è, rendiamo il contenuto testuale del paragrafo delle indovinelli uguale a `Previous guesses:`. Se no, no.

- Successivamente, usiamo un template literal per aggiungere il valore `userGuess` corrente alla fine del paragrafo `guesses`, con uno spazio vuoto in mezzo.
- Il blocco seguente esegue alcuni controlli:

  - Il primo `if (){ }` controlla se l'indovinello dell'utente è uguale al `randomNumber` impostato in cima al nostro JavaScript. Se lo è, il giocatore ha indovinato correttamente e il gioco è vinto, quindi mostriamo al giocatore un messaggio di congratulazioni con un bel colore verde, cancelliamo il contenuto della casella di informazioni Bassa/Alta dei indovinelli e eseguiamo una funzione chiamata `setGameOver()`, di cui discuteremo più avanti.
  - Ora abbiamo concatenato un altro test alla fine dell'ultimo utilizzando una struttura `else if (){ }`. Questo controlla se questo turno è l'ultimo turno dell'utente. Se lo è, il programma fa la stessa cosa del blocco precedente, eccetto con un messaggio di game over invece di un messaggio di congratulazioni.
  - L'ultimo blocco concatenato alla fine di questo codice (l'`else { }`) contiene codice che viene eseguito solo se nessuno degli altri due test restituisce true (cioè, il giocatore non ha indovinato correttamente, ma ha ancora indovinelli disponibili). In questo caso diciamo loro che hanno torto, poi eseguiamo un altro test condizionale per verificare se l'indovinello era superiore o inferiore alla risposta, visualizzando un ulteriore messaggio appropriato per dirgli più alto o più basso.

- Le ultime tre righe nella funzione ci preparano per il tentativo successivo da inviare. Aggiungiamo 1 alla variabile `guessCount` così che il giocatore usi il suo turno (`++` è un'operazione di incremento — si aumenta di 1), e svuotiamo il valore dal campo di testo del modulo e lo rifocalizziamo, pronto per l'inserimento del prossimo tentativo.

### Eventi

A questo punto, abbiamo una funzione `checkGuess()` ben implementata, ma non farà nulla perché non l'abbiamo ancora chiamata. Idealmente, vogliamo chiamarla quando il pulsante "Invia indovinello" viene premuto, e per fare ciò dobbiamo usare un **evento**. Gli eventi sono cose che accadono nel browser — un pulsante che viene cliccato, una pagina che viene caricata, un video che viene riprodotto, ecc. — in risposta a cui possiamo eseguire blocchi di codice. I **listener di eventi** osservano eventi specifici e chiamano i **gestori di eventi**, che sono blocchi di codice che vengono eseguiti in risposta a un evento che si attiva.

Aggiunga la seguente riga sotto la sua funzione `checkGuess()`:

```js
guessSubmit.addEventListener("click", checkGuess);
```

Qui stiamo aggiungendo un listener di eventi al pulsante `guessSubmit`. Questo è un metodo che prende due valori di input (chiamati _argomenti_) — il tipo di evento che stiamo aspettando (in questo caso `click`) come stringa, e il codice che vogliamo eseguire quando l'evento si verifica (in questo caso la funzione `checkGuess()`). Noti che non è necessario specificare le parentesi quando lo scrive all'interno di [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).

Provi a salvare e aggiornare il suo codice ora, e il suo esempio dovrebbe funzionare — fino a un certo punto. L'unico problema ora è che se indovina la risposta corretta o esaurisce gli indovinelli, il gioco si romperà perché non abbiamo ancora definito la funzione `setGameOver()` che dovrebbe essere eseguita una volta che il gioco è terminato. Aggiungiamo ora il nostro codice mancante e completiamo la funzionalità dell'esempio.

### Completamento della funzionalità del gioco

Aggiungiamo quella funzione `setGameOver()` in fondo al nostro codice e poi la esaminiamo. La aggiunga ora, sotto il resto del suo JavaScript:

```js
function setGameOver() {
  guessField.disabled = true;
  guessSubmit.disabled = true;
  resetButton = document.createElement("button");
  resetButton.textContent = "Start new game";
  document.body.append(resetButton);
  resetButton.addEventListener("click", resetGame);
}
```

- Le prime due righe disabilitano il campo di testo del modulo e il pulsante impostando la loro proprietà disabled su `true`. Questo è necessario, perché se non lo facessimo, l'utente potrebbe inviare più indovinelli dopo che il gioco è finito, il che potrebbe rovinare tutto.
- Le tre righe successive generano un nuovo elemento {{htmlelement("button")}}, ne impostano l'etichetta del testo su "Start new game", e lo aggiungono alla fine del nostro HTML esistente.
- L'ultima riga imposta un listener di eventi sul nostro nuovo pulsante, quindi quando viene cliccato, viene eseguita una funzione chiamata `resetGame()`.

Ora dobbiamo definire anche questa funzione! Aggiunga il seguente codice, ancora una volta, alla fine del suo JavaScript:

```js
function resetGame() {
  guessCount = 1;

  const resetParas = document.querySelectorAll(".resultParas p");
  for (const resetPara of resetParas) {
    resetPara.textContent = "";
  }

  resetButton.parentNode.removeChild(resetButton);

  guessField.disabled = false;
  guessSubmit.disabled = false;
  guessField.value = "";
  guessField.focus();

  lastResult.style.backgroundColor = "white";

  randomNumber = Math.floor(Math.random() * 100) + 1;
}
```

Questo lungo blocco di codice reimposta completamente tutto come era all'inizio del gioco, in modo che il giocatore possa avere un altro tentativo. Essa:

- Riporta `guessCount` a 1.
- Svuota tutto il testo dai paragrafi delle informazioni. Selezioniamo tutti i paragrafi all'interno di `<div class="resultParas"></div>`, poi scorre ciascuno di essi, impostando il loro `textContent` su `''` (una stringa vuota).
- Rimuove il pulsante di ripristino dal nostro codice.
- Attiva gli elementi del modulo e svuota e rifocalizza il campo di testo, pronta per un nuovo indovinello da inserire.
- Rimuove il colore di sfondo dal paragrafo `lastResult`.
- Genera un nuovo numero casuale in modo da non star semplicemente indovinando lo stesso numero di nuovo!

**A questo punto, dovrebbe avere un gioco pienamente funzionante (anche se semplice) — congratulazioni!**

Tutto quello che ci resta da fare ora in questo articolo è discutere alcune altre importanti funzionalità del codice che ha già visto, anche se forse non le ha riconosciute.

### Cicli

Una parte del codice sopra che dobbiamo esaminare in modo più dettagliato è il ciclo [for...of](/it/docs/Web/JavaScript/Reference/Statements/for...of). I cicli sono un concetto molto importante nella programmazione, che ci consente di continuare a eseguire un pezzo di codice più e più volte, fino a quando una determinata condizione è soddisfatta.

Per cominciare, vada di nuovo alla sua [console degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) e inserisca quanto segue:

```js
const fruits = ["apples", "bananas", "cherries"];
for (const fruit of fruits) {
  console.log(fruit);
}
```

Cosa è successo? Le stringhe `'apples', 'bananas', 'cherries'` sono state stampate nella sua console.

Questo è dovuto al ciclo. La linea `const fruits = ['apples', 'bananas', 'cherries'];` crea un array. Affronteremo [una completa lezione sugli Array](/it/docs/Learn_web_development/Core/Scripting/Arrays) più avanti in questo modulo, ma per ora: un array è una collezione di elementi (in questo caso stringhe).

Un ciclo `for...of` le dà un modo per ottenere ciascun elemento nell'array ed eseguire del JavaScript su di esso. La linea `for (const fruit of fruits)` dice:

1. Ottieni il primo elemento in `fruits`.
2. Imposta la variabile `fruit` su quell'elemento, poi esegui il codice tra le parentesi `{}` graffe.
3. Ottieni il prossimo elemento in `fruits`, e ripeti il punto 2, fino a raggiungere la fine di `fruits`.

In questo caso, il codice all'interno delle parentesi graffe sta scrivendo su `fruit` nella console.

Ora guardiamo al ciclo nel nostro gioco di indovinare il numero — i seguenti possono essere trovati all'interno della funzione `resetGame()`:

```js
const resetParas = document.querySelectorAll(".resultParas p");
for (const resetPara of resetParas) {
  resetPara.textContent = "";
}
```

Questo codice crea una variabile contenente un elenco di tutti i paragrafi all'interno di `<div class="resultParas">` usando il metodo [`querySelectorAll()`](/it/docs/Web/API/Document/querySelectorAll), quindi scorre ognuno di essi, rimuovendo il contenuto di testo di ciascuno.

Noti che anche se `resetPara` è una costante, possiamo cambiare le sue proprietà interne come `textContent`.

### Una piccola discussione sugli oggetti

Aggiungiamo un altro ultimo miglioramento prima di arrivare a questa discussione. Aggiunga la seguente linea appena sotto quella `let resetButton;` vicino alla parte superiore del suo JavaScript, poi salvi il suo file:

```js
guessField.focus();
```

Questa linea utilizza il metodo [`focus()`](/it/docs/Web/API/HTMLElement/focus) per mettere automaticamente il cursore del testo nel campo di testo {{htmlelement("input")}} non appena la pagina viene caricata, il che significa che l'utente può iniziare a digitare il suo primo indovinello subito, senza dover cliccare prima sul campo modulo. È solo una piccola aggiunta, ma migliora l'usabilità — fornendo all'utente un buon indizio visivo su cosa deve fare per giocare.

Analizziamo cosa sta succedendo qui in dettaglio. In JavaScript, la maggior parte degli elementi che manipolerà nel suo codice sono oggetti. Un oggetto è una raccolta di funzionalità correlate memorizzate in un unico raggruppamento. Può creare i suoi oggetti, ma è piuttosto avanzato e non ne tratteremo fino a molto più avanti nel corso. Per ora, discuteremo solo brevemente degli oggetti incorporati che il suo browser contiene, che le permettono di fare molte cose utili.

In questo caso particolare, abbiamo prima creato una costante `guessField` che memorizza un riferimento al campo di testo del modulo nel nostro HTML — la seguente riga può essere trovata tra le nostre dichiarazioni vicino all'inizio del codice:

```js
const guessField = document.querySelector(".guessField");
```

Per ottenere questo riferimento, abbiamo utilizzato il metodo [`querySelector()`](/it/docs/Web/API/Document/querySelector) dell'oggetto [`document`](/it/docs/Web/API/Document). `querySelector()` prende un pezzo di informazione — un [selettore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) che seleziona l'elemento di cui si desidera un riferimento.

Poiché `guessField` ora contiene un riferimento ad un elemento {{htmlelement("input")}}, ora ha accesso a un numero di proprietà (fondamentalmente variabili memorizzate all'interno di oggetti, alcune delle quali non possono avere i loro valori cambiati) e metodi (fondamentalmente funzioni memorizzate all'interno di oggetti). Un metodo disponibile per gli elementi input è `focus()`, quindi ora possiamo usare questa linea per focalizzare il campo di testo:

```js
guessField.focus();
```

Le variabili che non contengono riferimenti a elementi del modulo non avranno `focus()` disponibile per loro. Ad esempio, la costante `guesses` contiene un riferimento a un elemento {{htmlelement("p")}}, e la variabile `guessCount` contiene un numero.

### Giocare con oggetti del browser

Giochiamo un po' con alcuni oggetti del browser.

1. Prima di tutto, apra il suo programma in un browser.
2. Successivamente, apra i suoi [strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), e assicuriamoci che la scheda della console JavaScript sia aperta.
3. Digiti `guessField` nella console e la console le mostrerà che la variabile contiene un elemento {{htmlelement("input")}}. Noterà anche che la console completa automaticamente nomi di oggetti che esistono all'interno dell'ambiente di esecuzione, incluse le sue variabili!
4. Ora scriva quanto segue:

   ```js
   guessField.value = 2;
   ```

   La proprietà `value` rappresenta il valore attuale inserito nel campo di testo. Vedrà che inserendo questo comando, abbiamo cambiato il testo nel campo di testo!

5. Ora provi a digitare `guesses` nella console e premi <kbd>Enter</kbd> (o <kbd>Return</kbd>, a seconda della tastiera). La console le mostrerà che la variabile contiene un elemento {{htmlelement("p")}}.
6. Ora provi a inserire la seguente linea:

   ```js
   guesses.value;
   ```

   Il browser restituisce `undefined`, perché i paragrafi non hanno la proprietà `value`.

7. Per cambiare il testo all'interno di un paragrafo, ha bisogno della proprietà [`textContent`](/it/docs/Web/API/Node/textContent) invece. Provi questo:

   ```js
   guesses.textContent = "Where is my paragraph?";
   ```

8. Ora per qualcosa di divertente. Provi a inserire le linee sottostanti, una per una:

   ```js
   guesses.style.backgroundColor = "yellow";
   guesses.style.fontSize = "200%";
   guesses.style.padding = "10px";
   guesses.style.boxShadow = "3px 3px 6px black";
   ```

   Ogni elemento su una pagina ha una proprietà `style`, che contiene un oggetto le cui proprietà contengono tutti gli stili CSS in linea applicati a quell'elemento. Questo ci permette di impostare dinamicamente nuovi stili CSS sugli elementi usando JavaScript.

## Riepilogo

Quindi, questo è tutto per costruire l'esempio. È arrivato alla fine — ben fatto! Provi il suo codice finale, o [giochi con la nostra versione finale qui](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game.html). Se non riesce a far funzionare la sua versione dell'esempio, controlli contro il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game.html).

Anche la prossima lezione potrebbe aiutare — in essa, discutiamo di cosa può andare storto quando si scrive codice JavaScript, facendo riferimento al gioco "Indovina il numero" nel processo.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_is_JavaScript", "Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting")}}
