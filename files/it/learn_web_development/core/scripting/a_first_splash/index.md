---
title: Un primo tuffo in JavaScript
short-title: Percorso in JavaScript
slug: Learn_web_development/Core/Scripting/A_first_splash
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_is_JavaScript", "Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting")}}

Ora che hai imparato qualcosa sulla teoria di JavaScript e su cosa puoi fare con esso, ti forniremo un'idea di come sia il processo di creazione di un semplice programma JavaScript, guidandoti attraverso un tutorial pratico. Qui costruirai un semplice gioco "Indovina il numero", passo dopo passo.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Pensare come un programmatore.</li>
          <li>Esperienza di cosa significhi scrivere JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

Vogliamo fissare aspettative davvero chiare qui: Non sarà necessario imparare JavaScript entro la fine di questo articolo, né comprendere tutto il codice che ti chiediamo di scrivere. Invece, vogliamo darti un'idea di come le funzionalità di JavaScript lavorano insieme e di cosa significhi scrivere JavaScript. Negli articoli successivi rivisiterai tutte le funzionalità mostrate qui in modo molto più dettagliato, quindi non preoccuparti se non capisci tutto immediatamente!

> [!NOTE]
> Molte delle funzionalità di codice che vedrai in JavaScript sono le stesse di altri linguaggi di programmazione — funzioni, loop, ecc. La sintassi del codice appare diversa, ma i concetti sono ampiamente gli stessi.

## Pensare come un programmatore

Una delle cose più difficili da imparare nella programmazione non è la sintassi che devi apprendere, ma come applicarla per risolvere problemi del mondo reale. Devi iniziare a pensare come un programmatore — questo generalmente implica esaminare le descrizioni di ciò che il tuo programma deve fare, determinare quali funzionalità di codice sono necessarie per raggiungere tali obiettivi e come farle funzionare insieme.

Questo richiede una miscela di duro lavoro, esperienza con la sintassi di programmazione, pratica — e un po' di creatività. Più scrivi codice, meglio diventerai. Non possiamo promettere che svilupperai un "cervello da programmatore" in cinque minuti, ma ti daremo molte opportunità di praticare il pensare come un programmatore durante il corso.

Tenendo questo in mente, diamo un'occhiata all'esempio che svilupperemo in questo articolo e rivediamo il processo generale di scomposizione in task tangibili.

## Esempio — Gioco di indovinare il numero

In questo articolo ti mostreremo come sviluppare il semplice gioco che puoi vedere qui sotto:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game", 900, 300)}}

Provalo — familiarizza con il gioco prima di proseguire.

Immagina che il tuo capo ti abbia dato il seguente incarico per creare questo gioco:

> Voglio che tu crei un semplice gioco del tipo "indovina il numero". Dovrebbe scegliere un numero casuale tra 1 e 100, poi sfidare il giocatore a indovinare il numero in 10 tentativi. Dopo ogni turno, il giocatore dovrebbe essere informato se ha indovinato o sbagliato, e se ha sbagliato, se la congettura era troppo bassa o troppo alta. Dovrebbe anche dire al giocatore quali numeri ha precedentemente indovinato. Il gioco finirà una volta che il giocatore ha indovinato correttamente, o una volta che ha esaurito i tentativi. Quando il gioco termina, il giocatore dovrebbe avere un'opzione per ricominciare a giocare.

Osservando questo incarico, la prima cosa che possiamo fare è iniziare a scomporlo in compiti semplici e attuabili, con l'approccio mentale di un programmatore:

1. Generare un numero casuale tra 1 e 100.
2. Registrare il numero di turno in cui si trova il giocatore. Inizia da 1.
3. Fornire al giocatore un modo per indovinare quale sia il numero.
4. Una volta che un'indovinata è stata presentata, registrarla da qualche parte in modo che l'utente possa vedere i suoi precedenti tentativi.
5. Successivamente, verificare se è il numero corretto.
6. Se è corretto:

   1. Visualizzare un messaggio di congratulazioni.
   2. Impedire al giocatore di inserire altri tentativi (ciò influirebbe sul gioco).
   3. Visualizzare un controllo che consente al giocatore di ricominciare il gioco.

7. Se è sbagliato e il giocatore ha ancora tentativi:

   1. Dire al giocatore che ha sbagliato e se la sua congettura era troppo alta o troppo bassa.
   2. Permettergli di inserire un altro tentativo.
   3. Incrementare il numero di turno di 1.

8. Se è sbagliato e il giocatore non ha più tentativi:

   1. Dire al giocatore che è game over.
   2. Impedire al giocatore di inserire altri tentativi (ciò influirebbe sul gioco).
   3. Visualizzare un controllo che consente al giocatore di ricominciare il gioco.

9. Una volta che il gioco viene riavviato, assicurarsi che la logica del gioco e l'interfaccia utente siano completamente resettati, quindi tornare al passo 1.

Procediamo ora, osservando come possiamo trasformare questi passaggi in codice, costruendo l'esempio ed esplorando le caratteristiche di JavaScript mentre andiamo avanti.

### Configurazione iniziale

Per iniziare questo tutorial, vogliamo che tu faccia una copia locale del file [number-guessing-game-start.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html) ([vedilo in azione qui](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game-start.html)). Aprilo sia nel tuo editor di testo che nel tuo browser web. Al momento vedrai un semplice titolo, un paragrafo di istruzioni e un modulo per inserire un tentativo, ma il modulo al momento non farà nulla.

Il posto dove aggiungeremo tutto il nostro codice è all'interno dell'elemento {{htmlelement("script")}} alla fine dell'HTML:

```html
<script>
  // Your JavaScript goes here
</script>
```

### Aggiungere variabili per memorizzare i nostri dati

Iniziamo. Prima di tutto, aggiungi le seguenti linee all'interno del tuo elemento {{htmlelement("script")}}:

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

Questa sezione di codice imposta le variabili e le costanti di cui abbiamo bisogno per memorizzare i dati che il nostro programma utilizzerà.

Le variabili sono fondamentalmente nomi per valori (come numeri o stringhe di testo). Crei una variabile con la parola chiave `let` seguita da un nome per la tua variabile.

Le costanti sono anche utilizzate per nominare valori, ma a differenza delle variabili non puoi modificare il valore una volta impostato. In questo caso, stiamo usando costanti per memorizzare riferimenti a parti della nostra interfaccia utente. Il testo all'interno di alcuni di questi elementi potrebbe cambiare, ma ogni costante fa sempre riferimento allo stesso elemento HTML con cui è stata inizializzata. Crei una costante con la parola chiave `const` seguita da un nome per la costante.

Puoi assegnare un valore alla tua variabile o costante con un segno di uguale (`=`) seguito dal valore che vuoi dargli.

Nel nostro esempio:

- La prima variabile — `randomNumber` — viene assegnata a un numero casuale tra 1 e 100, calcolato utilizzando un algoritmo matematico.
- Le prime tre costanti vengono utilizzate per memorizzare un riferimento ai paragrafi dei risultati nel nostro HTML e vengono usate per inserire valori nei paragrafi più avanti nel codice (nota come sono all'interno di un elemento `<div>`, che viene esso stesso utilizzato per selezionare tutti e tre più avanti per il reset, quando ricominciamo il gioco):

  ```html
  <div class="resultParas">
    <p class="guesses"></p>
    <p class="lastResult"></p>
    <p class="lowOrHi"></p>
  </div>
  ```

- Le due costanti successive memorizzano riferimenti all'input di testo del modulo e al pulsante di invio e vengono utilizzate per controllare l'invio del tentativo più avanti.

  ```html
  <label for="guessField">Enter a guess: </label>
  <input type="number" id="guessField" class="guessField" />
  <input type="submit" value="Submit guess" class="guessSubmit" />
  ```

- Le nostre ultime due variabili memorizzano un conteggio dei tentativi pari a 1 (usato per tenere traccia di quanti tentativi ha fatto il giocatore) e un riferimento a un pulsante di reset che non esiste ancora (ma esisterà più tardi).

> [!NOTE]
> Imparerai molto di più sulle variabili e le costanti più avanti nel corso, partendo dall'articolo [Memorizzare le informazioni di cui hai bisogno — Variabili](/it/docs/Learn_web_development/Core/Scripting/Variables).

### Funzioni

Successivamente, aggiungi quanto segue sotto il tuo codice JavaScript precedente:

```js
function checkGuess() {
  alert("I am a placeholder");
}
```

Le funzioni sono blocchi di codice riutilizzabili che puoi scrivere una volta e eseguire più volte, risparmiando la necessità di continuare a ripetere il codice tutto il tempo. Questo è davvero utile. Ci sono diversi modi per definire funzioni, ma per ora ci concentreremo su un tipo semplice. Qui abbiamo definito una funzione utilizzando la parola chiave `function`, seguita da un nome, con parentesi messe dopo di esso. Dopo di che, mettiamo due parentesi graffe (`{ }`). All'interno delle parentesi graffe va tutto il codice che vogliamo eseguire ogni volta che chiamiamo la funzione.

Quando vogliamo eseguire il codice, scriviamo il nome della funzione seguito dalle parentesi.

Proviamo ora. Salva il tuo codice e ricarica la pagina nel tuo browser. Poi vai nella [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) e inserisci la seguente linea:

```js
checkGuess();
```

Dopo aver premuto <kbd>Return</kbd>/<kbd>Enter</kbd>, dovresti vedere un'alert che dice `I am a placeholder`; abbiamo definito una funzione nel nostro codice che crea un'alert ogni volta che la chiamiamo.

> [!NOTE]
> Imparerai molto di più sulle funzioni più avanti nell'articolo [Funzioni — blocchi di codice riutilizzabili](/it/docs/Learn_web_development/Core/Scripting/Functions).

### Operatori

Gli operatori JavaScript ci permettono di eseguire test, fare operazioni matematiche, unire stringhe e altre cose simili.

Se non hai già fatto, salva il tuo codice, aggiorna la pagina nel tuo browser e apri la [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools). Poi possiamo provare a digitare gli esempi mostrati di seguito — digita ciascuno dalla colonna "Esempio" esattamente come mostrato, premendo <kbd>Return</kbd>/<kbd>Enter</kbd> dopo ciascuno, e vedi quali risultati restituiscono.

Iniziamo con gli operatori aritmetici, per esempio:

| Operatore | Nome          | Esempio  |
| --------- | --------------| -------- |
| `+`       | Addizione     | `6 + 9`  |
| `-`       | Sottrazione   | `20 - 15`|
| `*`       | Moltiplicazione| `3 * 7` |
| `/`       | Divisione     | `10 / 5` |

Ci sono anche dei operatori di scorciatoia disponibili, chiamati [operatori di assegnazione composti](/it/docs/Web/JavaScript/Reference/Operators#assignment_operators). Per esempio, se vuoi aggiungere un nuovo numero a uno esistente e restituire il risultato, potresti fare così:

```js
let number1 = 1;
number1 += 2;
```

Questo è equivalente a

```js
let number2 = 1;
number2 = number2 + 2;
```

Quando siamo eseguiamo test vero/falso (ad esempio all'interno delle condizioni — vedi [sotto](#condizionali)) usiamo [operatori di confronto](/it/docs/Web/JavaScript/Reference/Operators). Per esempio:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Operatore</th>
      <th scope="col">Nome</th>
      <th scope="col">Esempio</th>
    </tr>
    <tr>
      <td><code>===</code></td>
      <td>Uguaglianza stretta (è esattamente uguale?)</td>
      <td>
        <pre class="brush: js">
5 === 2 + 4 // false
'Chris' === 'Bob' // false
5 === 2 + 3 // true
2 === '2' // false; numero rispetto a stringa
</pre
        >
      </td>
    </tr>
    <tr>
      <td><code>!==</code></td>
      <td>Non uguaglianza (non è lo stesso?)</td>
      <td>
        <pre class="brush: js">
5 !== 2 + 4 // true
'Chris' !== 'Bob' // true
5 !== 2 + 3 // false
2 !== '2' // true; numero rispetto a stringa
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

Le stringhe sono utilizzate per rappresentare testo. Abbiamo già visto una variabile stringa: nel seguente codice, `"I am a placeholder"` è una stringa:

```js
function checkGuess() {
  alert("I am a placeholder");
}
```

Puoi dichiarare stringhe utilizzando le virgolette doppie (`"`) o singole (`'`), ma devi utilizzare la stessa forma per l'inizio e la fine di una singola dichiarazione di stringa: non puoi scrivere `"I am a placeholder'`.

Puoi anche dichiarare stringhe utilizzando i backtick (`` ` ``). Le stringhe dichiarate in questo modo sono chiamate _template literals_ e hanno alcune proprietà speciali. In particolare, puoi incorporare altre variabili o persino espressioni al loro interno:

```js
const name = "Mahalia";

const greeting = `Hello ${name}`;
```

Questo ti fornisce un meccanismo per unire stringhe insieme.

### Condizionali

Ritornando alla nostra funzione `checkGuess()`, penso che sia sicuro dire che non vogliamo che riporti solo un messaggio segnaposto. Vogliamo che controlli se un tentativo del giocatore è corretto o meno, e risponda adeguatamente.

A questo punto, sostituisci la tua funzione `checkGuess()` corrente con questa versione:

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

Questo è un sacco di codice — phew! Passiamo attraverso ciascuna sezione ed esaminiamo cosa fa.

- La prima linea dichiara una variabile chiamata `userGuess` e imposta il suo valore al valore corrente inserito all'interno del campo di testo. Passiamo anche questo valore attraverso il costruttore built-in `Number()`, solo per assicurarci che il valore sia sicuramente un numero. Poiché non stiamo cambiando questa variabile, la dichiariamo utilizzando `const`.
- Successivamente, incontriamo il nostro primo blocco di codice condizionale. Un blocco di codice condizionale ti permette di eseguire il codice selettivamente, a seconda che una certa condizione sia vera o meno. Assomiglia un po' a una funzione, ma non lo è. La forma più semplice di blocco condizionale inizia con la parola chiave `if`, poi alcune parentesi, poi alcune parentesi graffe. All'interno delle parentesi, includiamo un test. Se il test restituisce `true`, eseguiamo il codice all'interno delle parentesi graffe. Se non lo è, non lo facciamo, e passiamo alla prossima parte del codice. In questo caso, il test verifica se la variabile `guessCount` è uguale a `1` (cioè, se questo è il primo tentativo del giocatore o meno):

  ```js
  guessCount === 1;
  ```

  Se lo è, facciamo sì che il contenuto testuale del paragrafo dei tentativi sia uguale a `Tentativi precedenti:`. Se no, non lo facciamo.

- Successivamente, utilizziamo un template literal per aggiungere il valore `userGuess` corrente alla fine del paragrafo `guesses`, con uno spazio vuoto nel mezzo.
- Il blocco successivo esegue alcuni controlli:

  - Il primo `if (){ }` verifica se il tentativo dell'utente è uguale al `randomNumber` impostato all'inizio del nostro JavaScript. Se lo è, il giocatore ha indovinato correttamente e il gioco è vinto, quindi mostriamo al giocatore un messaggio di congratulazioni con un bel colore verde, svuotiamo il contenuto della casella di informazioni sul tentativo sbagliato e eseguiamo una funzione chiamata `setGameOver()`, di cui discuteremo più avanti.
  - Ora abbiamo concatenato un altro test alla fine del precedente usando una struttura `else if (){ }`. Questo verifica se questo turno è l'ultimo turno dell'utente. Se lo è, il programma fa la stessa cosa del blocco precedente, tranne che con un messaggio di game over invece di un messaggio di congratulazioni.
  - L'ultimo blocco concatenato alla fine di questo codice (il `else { }`) contiene un codice che viene eseguito solo se nessuno degli altri due test restituisce true (cioè, il giocatore non ha indovinato giusto, ma ha altri tentativi disponibili). In questo caso gli diciamo che ha sbagliato, poi eseguiamo un altro test condizionale per verificare se il tentativo era maggiore o minore della risposta, mostrando un ulteriore messaggio appropriato per dirgli maggiore o minore.

- Le ultime tre linee nella funzione ci preparano per il prossimo tentativo da presentare. Aggiungiamo 1 alla variabile `guessCount` in modo che il giocatore usi il proprio turno (`++` è un'operazione di incremento — aumento di 1), e svuotiamo il valore del campo di testo del modulo e lo focalizziamo di nuovo, pronto per il prossimo tentativo da inserire.

### Eventi

A questo punto, abbiamo una funzione `checkGuess()` ben implementata, ma non farà nulla perché non l'abbiamo ancora chiamata. Idealmente, vogliamo chiamarla quando viene premuto il pulsante "Submit guess" e per farlo abbiamo bisogno di utilizzare un **evento**. Gli eventi sono cose che accadono nel browser — un pulsante viene cliccato, una pagina viene caricata, un video viene riprodotto, ecc. — in risposta ai quali possiamo eseguire blocchi di codice. I **listener di eventi** osservano eventi specifici e chiamano **gestori di eventi**, che sono blocchi di codice che vengono eseguiti in risposta all'attivazione di un evento.

Aggiungi la seguente linea sotto la tua funzione `checkGuess()`:

```js
guessSubmit.addEventListener("click", checkGuess);
```

Qui stiamo aggiungendo un listener di eventi al pulsante `guessSubmit`. Questo è un metodo che prende due valori di input (chiamati _argomenti_) — il tipo di evento che stiamo aspettando (in questo caso `click`) come stringa, e il codice che vogliamo eseguire quando l'evento si verifica (in questo caso la funzione `checkGuess()`). Nota che non abbiamo bisogno di specificare le parentesi quando lo scriviamo all'interno di [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).

Prova a salvare e aggiornare il tuo codice ora, e il tuo esempio dovrebbe funzionare — fino a un certo punto. L'unico problema adesso è che se indovini la risposta corretta o esaurisci i tentativi, il gioco si bloccherà perché non abbiamo ancora definito la funzione `setGameOver()` che dovrebbe essere eseguita una volta che il gioco è finito. Aggiungiamo ora il nostro codice mancante e completiamo la funzionalità dell'esempio.

### Completare la funzionalità del gioco

Aggiungiamo la funzione `setGameOver()` in fondo al nostro codice e poi esaminiamola. Aggiungi questo ora, sotto il resto del tuo JavaScript:

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

- Le prime due righe disabilitano l'input di testo del modulo e il pulsante impostando le loro proprietà disabled su `true`. Questo è necessario, perché se non lo facessimo, l'utente potrebbe inserire più tentativi dopo che il gioco è finito, il che influirebbe sul gioco.
- Le prossime tre righe generano un nuovo elemento {{htmlelement("button")}}, impostano la sua etichetta testuale su "Start new game" e lo aggiungono alla fine del nostro HTML esistente.
- L'ultima linea imposta un listener di eventi sul nostro nuovo pulsante in modo che quando viene cliccato, venga eseguita una funzione chiamata `resetGame()`.

Ora dobbiamo definire anche questa funzione! Aggiungi il seguente codice, ancora una volta alla fine del tuo JavaScript:

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

Questo lungo blocco di codice reimposta completamente tutto come era all'inizio del gioco, in modo che il giocatore possa avere un altro tentativo. Esso:

- Riporta il `guessCount` a 1.
- Svuota tutto il testo dai paragrafi informativi. Selezioniamo tutti i paragrafi all'interno di `<div class="resultParas"></div>`, quindi attraversiamo ciascuno, impostando il loro `textContent` al valore `''` (una stringa vuota).
- Rimuove il pulsante di reset dal nostro codice.
- Abilita gli elementi del modulo, e svuota e focalizza il campo di testo, pronto per un nuovo tentativo da inserire.
- Rimuove il colore di sfondo dal paragrafo `lastResult`.
- Genera un nuovo numero casuale in modo che non stia semplicemente indovinando lo stesso numero di nuovo!

**A questo punto, dovresti avere un gioco (semplice) completamente funzionante — congratulazioni!**

Tutto ciò che ci resta ora da fare in questo articolo è parlare di alcune altre caratteristiche importanti del codice che hai già visto, anche se potresti non averne realizzato.

### Loop

Una parte del codice sopra che dobbiamo esaminare più in dettaglio è il loop [for...of](/it/docs/Web/JavaScript/Reference/Statements/for...of). I loop sono un concetto molto importante nella programmazione, che ti permettono di continuare a eseguire un pezzo di codice più e più volte, fino a quando una certa condizione viene soddisfatta.

Per iniziare, vai di nuovo alla tua [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) e inserisci il seguente codice:

```js
const fruits = ["apples", "bananas", "cherries"];
for (const fruit of fruits) {
  console.log(fruit);
}
```

Che cosa è successo? Le stringhe `'apples', 'bananas', 'cherries'` sono state stampate nella tua console.

Questo è a causa del loop. La linea `const fruits = ['apples', 'bananas', 'cherries'];` crea un array. Lavoreremo attraverso [un tutorial completo sugli array](/it/docs/Learn_web_development/Core/Scripting/Arrays) più avanti in questo modulo, ma per ora: un array è una raccolta di elementi (in questo caso stringhe).

Un loop `for...of` ti dà un modo per ottenere ciascun elemento nell'array ed eseguire un po' di JavaScript su di esso. La linea `for (const fruit of fruits)` dice:

1. Prendi il primo elemento in `fruits`.
2. Imposta la variabile `fruit` a quel elemento, poi esegui il codice tra le parentesi graffe `{}`.
3. Prendi il prossimo elemento in `fruits`, e ripeti 2, fino a quando non raggiungi la fine di `fruits`.

In questo caso, il codice all'interno delle parentesi graffe sta scrivendo `fruit` nella console.

Ora esaminiamo il loop nel nostro gioco di indovinare il numero — il seguente può essere trovato all'interno della funzione `resetGame()`:

```js
const resetParas = document.querySelectorAll(".resultParas p");
for (const resetPara of resetParas) {
  resetPara.textContent = "";
}
```

Questo codice crea una variabile contenente un elenco di tutti i paragrafi all'interno `<div class="resultParas">` usando il metodo [`querySelectorAll()`](/it/docs/Web/API/Document/querySelectorAll), poi scorre attraverso ciascuno, rimuovendo il contenuto di testo di ciascuno.

Nota che anche se `resetPara` è una costante, possiamo cambiare le sue proprietà interne come `textContent`.

### Una piccola discussione sugli oggetti

Aggiungiamo un ulteriore miglioramento finale prima di arrivare a questa discussione. Aggiungi la seguente riga subito sotto la linea `let resetButton;` vicino alla parte superiore del tuo JavaScript, quindi salva il tuo file:

```js
guessField.focus();
```

Questa linea utilizza il metodo [`focus()`](/it/docs/Web/API/HTMLElement/focus) per mettere automaticamente il cursore di testo nel campo di testo {{htmlelement("input")}} non appena la pagina viene caricata, il che significa che l'utente può iniziare a digitare il suo primo tentativo subito, senza dover fare clic prima sul campo del modulo. È solo un piccolo miglioramento, ma migliora l'usabilità — dando all'utente un buon indizio visivo su cosa deve fare per giocare al gioco.

Analizziamo cosa sta succedendo qui in modo più dettagliato. In JavaScript, la maggior parte degli elementi che manipolerai nel tuo codice sono oggetti. Un oggetto è una raccolta di funzionalità correlate memorizzate in un'unica raccolta. Puoi creare i tuoi oggetti, ma è piuttosto avanzato e non lo affronteremo fino a molto più avanti nel corso. Per ora, discuteremo brevemente gli oggetti integrati che il tuo browser contiene, che ti consentono di fare molte cose utili.

In questo caso particolare, abbiamo innanzitutto creato una costante `guessField` che memorizza un riferimento all'elemento di input testo del modulo nel nostro HTML — la seguente riga può essere trovata tra le nostre dichiarazioni vicino alla parte superiore del codice:

```js
const guessField = document.querySelector(".guessField");
```

Per ottenere questo riferimento, abbiamo utilizzato il metodo [`querySelector()`](/it/docs/Web/API/Document/querySelector) dell'oggetto [`document`](/it/docs/Web/API/Document). `querySelector()` prende un'informazione — un [selettore CSS](/it/docs/Learn_web_development/Core/Styling_basics/Basic_selectors) che seleziona l'elemento di cui vuoi un riferimento.

Poiché `guessField` ora contiene un riferimento a un elemento {{htmlelement("input")}}, ha ora accesso a un certo numero di proprietà (in pratica variabili memorizzate all'interno di oggetti, alcune delle quali non possono avere i loro valori modificati) e metodi (in pratica funzioni memorizzate all'interno di oggetti). Un metodo disponibile per gli elementi di input è `focus()`, quindi ora possiamo usare questa linea per focalizzare l'input di testo:

```js
guessField.focus();
```

Le variabili che non contengono riferimenti a elementi di modulo non avranno `focus()` disponibile per loro. Ad esempio, la costante `guesses` contiene un riferimento a un elemento {{htmlelement("p")}}, e la variabile `guessCount` contiene un numero.

### Giocare con gli oggetti del browser

Giochiamo un po' con alcuni oggetti del browser.

1. Innanzitutto, apri il tuo programma in un browser.
2. Successivamente, apri i tuoi [strumenti per sviluppatori del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), e assicurati che la scheda della console JavaScript sia aperta.
3. Digita `guessField` nella console e la console ti mostra che la variabile contiene un elemento {{htmlelement("input")}}. Noterai anche che la console completa automaticamente i nomi degli oggetti che esistono all'interno dell'ambiente di esecuzione, comprese le tue variabili!
4. Ora digita il seguente:

   ```js
   guessField.value = 2;
   ```

   La proprietà `value` rappresenta il valore corrente inserito nel campo di testo. Vedrai che inserendo questo comando, abbiamo cambiato il testo nel campo di testo!

5. Ora prova a digitare `guesses` nella console e premi <kbd>Enter</kbd> (o <kbd>Return</kbd>, a seconda della tua tastiera). La console ti mostra che la variabile contiene un elemento {{htmlelement("p")}}.
6. Ora prova a inserire la seguente linea:

   ```js
   guesses.value;
   ```

   Il browser restituisce `undefined`, perché i paragrafi non hanno la proprietà `value`.

7. Per cambiare il testo all'interno di un paragrafo, hai invece bisogno della proprietà [`textContent`](/it/docs/Web/API/Node/textContent). Prova questo:

   ```js
   guesses.textContent = "Where is my paragraph?";
   ```

8. Ora per alcune cose divertenti. Prova a inserire le righe qui sotto, una per una:

   ```js
   guesses.style.backgroundColor = "yellow";
   guesses.style.fontSize = "200%";
   guesses.style.padding = "10px";
   guesses.style.boxShadow = "3px 3px 6px black";
   ```

   Ogni elemento su una pagina ha una proprietà `style`, che contiene essa stessa un oggetto le cui proprietà contengono tutti gli stili CSS inline applicati a quell'elemento. Questo ci permette di impostare dinamicamente nuovi stili CSS sugli elementi usando JavaScript.

## Sommario

Quindi questo è tutto per costruire l'esempio. Sei arrivato alla fine — ben fatto! Prova il tuo codice finale, o [gioca con la nostra versione finita qui](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game.html). Se non riesci ad ottenere il tuo esempio di funzionare, controllalo con il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game.html).

La prossima lezione potrebbe anche aiutare — in essa, discuteremo cosa può andare storto quando si scrive codice JavaScript, facendo riferimento al gioco "Indovina il numero" nel processo.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_is_JavaScript", "Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting")}}
