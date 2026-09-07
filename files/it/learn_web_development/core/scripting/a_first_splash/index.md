---
title: Un primo tuffo in JavaScript
short-title: Guida pratica a JavaScript
slug: Learn_web_development/Core/Scripting/A_first_splash
l10n:
  sourceCommit: 4fa9407fe174a12ecdc50b680560b16021300bc1
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_is_JavaScript", "Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting")}}

Ora che sono state apprese alcune nozioni teoriche su JavaScript e su ciò che è possibile fare con esso, questa guida pratica mostrerà come creare un semplice programma JavaScript. Qui verrà realizzato, passo dopo passo, un semplice gioco "Indovina il numero".

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Pensare come un programmatore.</li>
          <li>Fare esperienza di come si scrive JavaScript.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

> [!NOTE]
> A partire da [Write your first JavaScript variable](https://scrimba.com/learn-javascript-c0v/~04?via=mdn), Scrimba<sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup> offre utili lezioni interattive che illustrano le basi di JavaScript.

È opportuno stabilire aspettative molto chiare: non ci si aspetta di imparare JavaScript entro la fine di questo articolo, né di comprendere tutto il codice che viene richiesto di scrivere. L'obiettivo è invece fornire un'idea di come le funzionalità di JavaScript operino insieme e di cosa significhi scrivere JavaScript. Negli articoli successivi verranno riprese tutte le funzionalità qui mostrate in modo molto più dettagliato, quindi non bisogna preoccuparsi se non si comprende tutto immediatamente.

> [!NOTE]
> Molte delle funzionalità del codice che verranno viste in JavaScript sono le stesse presenti in altri linguaggi di programmazione: funzioni, cicli e così via. La sintassi del codice appare diversa, ma i concetti sono comunque in gran parte gli stessi.

## Presentazione del nostro esempio di "gioco di indovinare il numero"

In questo articolo verrà mostrato come costruire il gioco visibile qui sotto:

```html hidden live-sample___guess-the-number
<h1>Number guessing game</h1>

<p>
  We have selected a random number between 1 and 100. See if you can guess it in
  10 turns or fewer. We'll tell you if your guess was too high or too low.
</p>

<div class="form">
  <label for="guessField">Enter a guess: </label>
  <input
    type="number"
    min="1"
    max="100"
    required
    id="guessField"
    class="guessField" />
  <input type="submit" value="Submit guess" class="guessSubmit" />
</div>

<div class="resultParas">
  <p class="guesses"></p>
  <p class="lastResult"></p>
  <p class="lowOrHi"></p>
</div>
```

```css hidden live-sample___guess-the-number
html {
  font-family: sans-serif;
}

body {
  width: 50%;
  max-width: 800px;
  min-width: 480px;
  margin: 0 auto;
}

.form input[type="number"] {
  width: 200px;
}

.lastResult {
  color: white;
  padding: 3px;
}
```

```js hidden live-sample___guess-the-number
let randomNumber = Math.floor(Math.random() * 100) + 1;
const guesses = document.querySelector(".guesses");
const lastResult = document.querySelector(".lastResult");
const lowOrHi = document.querySelector(".lowOrHi");
const guessSubmit = document.querySelector(".guessSubmit");
const guessField = document.querySelector(".guessField");
let guessCount = 1;
let resetButton;

function checkGuess() {
  const userGuess = Number(guessField.value);
  if (guessCount === 1) {
    guesses.textContent = "Previous guesses: ";
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

guessSubmit.addEventListener("click", checkGuess);

function setGameOver() {
  guessField.disabled = true;
  guessSubmit.disabled = true;
  resetButton = document.createElement("button");
  resetButton.textContent = "Start new game";
  document.body.appendChild(resetButton);
  resetButton.addEventListener("click", resetGame);
}

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

{{EmbedLiveSample("guess-the-number", "100%", 300)}}

Prova a giocare: acquisisci familiarità con il gioco prima di proseguire.

## Pensare come un programmatore

Una delle cose più difficili da imparare nella programmazione non è la sintassi necessaria, ma come applicarla per risolvere problemi del mondo reale. Bisogna iniziare a pensare come un programmatore: in genere questo comporta esaminare le descrizioni di ciò che il programma deve fare, individuare quali funzionalità del codice sono necessarie per raggiungere tali obiettivi e capire come farle funzionare insieme.

Questo richiede una combinazione di impegno, esperienza con la sintassi di programmazione e pratica, oltre a un po' di creatività. Più si scrive codice, più si migliora. Non possiamo promettere che il "cervello da programmatore" si svilupperà in cinque minuti, ma qui e nel resto del corso saranno offerte molte opportunità per esercitarsi a pensare come programmatori.

## Le specifiche iniziali

Immaginiamo che il responsabile abbia fornito le seguenti specifiche per creare questo gioco:

> Voglio che venga creato un semplice gioco del tipo "indovina il numero". Dovrebbe scegliere un numero casuale compreso tra 1 e 100, quindi sfidare il giocatore a indovinare il numero in 10 tentativi. Dopo ogni tentativo, il giocatore dovrebbe sapere se ha indovinato oppure no e, se ha sbagliato, se il numero inserito era troppo basso o troppo alto. Dovrebbe anche comunicare al giocatore quali numeri sono stati indovinati in precedenza. Il gioco terminerà quando il giocatore indovina correttamente oppure quando esaurisce i tentativi. Al termine del gioco, al giocatore dovrebbe essere offerta l'opzione di ricominciare a giocare.

Dopo aver esaminato queste specifiche, la prima cosa da fare è iniziare a suddividerle in semplici attività eseguibili, adottando il più possibile la mentalità di un programmatore:

1. Generare un numero casuale compreso tra 1 e 100.
2. Registrare il numero del tentativo corrente del giocatore. Inizializzarlo a 1.
3. Fornire al giocatore un modo per indovinare il numero.
4. Una volta inviato un tentativo, registrarlo innanzitutto da qualche parte affinché l'utente possa visualizzare i tentativi precedenti.
5. Quindi, verificare se il numero è corretto.
6. Se è corretto:
   1. Mostrare un messaggio di congratulazioni.
   2. Impedire al giocatore di inserire altri tentativi, poiché questo comprometterebbe il gioco.
   3. Mostrare un controllo che consenta al giocatore di riavviare il gioco.

7. Se è sbagliato e il giocatore ha ancora tentativi:
   1. Dire al giocatore che ha sbagliato e se il suo tentativo era troppo alto o troppo basso.
   2. Consentirgli di inserire un altro tentativo.
   3. Incrementare di 1 il numero del tentativo.

8. Se è sbagliato e il giocatore non ha più tentativi:
   1. Dire al giocatore che il gioco è finito.
   2. Impedire al giocatore di inserire altri tentativi, poiché questo comprometterebbe il gioco.
   3. Mostrare un controllo che consenta al giocatore di riavviare il gioco.

9. Dopo il riavvio del gioco, assicurarsi che la logica di gioco e l'interfaccia utente siano completamente reimpostate, quindi tornare al passaggio 1.

Ora procediamo, osservando come trasformare questi passaggi in codice, costruendo l'esempio ed esplorando le funzionalità di JavaScript lungo il percorso.

## Configurazione iniziale

Per iniziare questa guida, creare una copia locale del codice seguente in un nuovo file HTML usando il proprio editor di codice.

```html
<!doctype html>
<html lang="en-US">
  <head>
    <meta charset="utf-8" />

    <title>Number guessing game</title>

    <style>
      html {
        font-family: sans-serif;
      }

      body {
        width: 50%;
        max-width: 800px;
        min-width: 480px;
        margin: 0 auto;
      }

      .form input[type="number"] {
        width: 200px;
      }

      .lastResult {
        color: white;
        padding: 3px;
      }
    </style>
  </head>

  <body>
    <h1>Number guessing game</h1>

    <p>
      We have selected a random number between 1 and 100. See if you can guess
      it in 10 turns or fewer. We'll tell you if your guess was too high or too
      low.
    </p>

    <div class="form">
      <label for="guessField">Enter a guess: </label>
      <input
        type="number"
        min="1"
        max="100"
        required
        id="guessField"
        class="guessField" />
      <input type="submit" value="Submit guess" class="guessSubmit" />
    </div>

    <div class="resultParas">
      <p class="guesses"></p>
      <p class="lastResult"></p>
      <p class="lowOrHi"></p>
    </div>

    <script>
      // Your JavaScript goes here
    </script>
  </body>
</html>
```

Mantenerlo aperto nell'editor di testo e aprirlo anche nel browser web. Al momento verranno visualizzati un semplice titolo, un paragrafo di istruzioni e un modulo per inserire un tentativo, ma il modulo non farà ancora nulla.

Tutto il codice JavaScript verrà aggiunto all'interno dell'elemento {{htmlelement("script")}} nella parte inferiore dell'HTML:

```html
<script>
  // Your JavaScript goes here
</script>
```

## Aggiungere variabili per memorizzare i dati

Iniziamo. Prima di tutto, aggiungere le seguenti righe all'interno dell'elemento {{htmlelement("script")}}:

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

Questa sezione del codice imposta le variabili e le costanti necessarie per memorizzare i dati che il programma utilizzerà.

Le variabili sono essenzialmente nomi per valori, come numeri o stringhe di testo. Una variabile viene creata con la parola chiave `let`, seguita da un nome per la variabile.

Anche le costanti vengono utilizzate per assegnare un nome ai valori, ma, a differenza delle variabili, non è possibile modificare il valore dopo averlo impostato. In questo caso, vengono usate costanti per memorizzare riferimenti a parti dell'interfaccia utente. Il testo all'interno di alcuni di questi elementi può cambiare, ma ogni costante fa sempre riferimento allo stesso elemento HTML con cui è stata inizializzata. Una costante viene creata con la parola chiave `const`, seguita da un nome per la costante.

È possibile assegnare un valore a una variabile o a una costante con un segno di uguale (`=`), seguito dal valore da assegnare.

Nel nostro esempio:

- Alla prima variabile, `randomNumber`, viene assegnato un numero casuale compreso tra 1 e 100, calcolato mediante un algoritmo matematico.
- Le prime tre costanti memorizzano ciascuna un riferimento ai paragrafi dei risultati nel nostro HTML e vengono utilizzate per inserire valori nei paragrafi più avanti nel codice. Si noti che si trovano all'interno di un elemento `<div>`, che viene a sua volta usato in seguito per selezionarli tutti e tre durante il reimpostazione del gioco:

  ```html
  <div class="resultParas">
    <p class="guesses"></p>
    <p class="lastResult"></p>
    <p class="lowOrHi"></p>
  </div>
  ```

- Le due costanti successive memorizzano riferimenti all'input di testo del modulo e al pulsante di invio e vengono utilizzate in seguito per gestire l'invio del tentativo.

  ```html
  <label for="guessField">Enter a guess: </label>
  <input type="number" id="guessField" class="guessField" />
  <input type="submit" value="Submit guess" class="guessSubmit" />
  ```

- Le ultime due variabili memorizzano un conteggio dei tentativi pari a 1, usato per tenere traccia di quanti tentativi ha effettuato il giocatore, e un riferimento a un pulsante di reimpostazione che non esiste ancora, ma esisterà in seguito.

## Funzioni

Successivamente, aggiungere quanto segue sotto il precedente JavaScript:

```js
function checkGuess() {
  console.log("I am a placeholder");
}
```

Le funzioni sono blocchi di codice riutilizzabili che possono essere scritti una volta ed eseguiti ripetutamente, evitando di dover ripetere continuamente il codice. Esistono diversi modi per definire le funzioni, ma per ora ci concentreremo su un tipo semplice. Qui è stata definita una funzione usando la parola chiave `function`, seguita da un nome, con parentesi dopo di esso. Successivamente, vengono inserite due parentesi graffe (`{ }`). All'interno delle parentesi graffe va tutto il codice che si desidera eseguire ogni volta che viene chiamata la funzione.

Quando si desidera eseguire il codice, si digita il nome della funzione seguito dalle parentesi.

Proviamo ora. Salvare il codice e aggiornare la pagina nel browser. Quindi aprire la [console JavaScript degli strumenti di sviluppo](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) e inserire la seguente riga:

```js
checkGuess();
```

Dopo aver premuto <kbd>Return</kbd>/<kbd>Enter</kbd>, nella console dovrebbe comparire `I am a placeholder`; nel codice è stata definita una funzione che restituisce un messaggio segnaposto ogni volta che viene chiamata.

## Stringhe di testo

Le stringhe vengono utilizzate per rappresentare il testo. È già stata vista una variabile stringa: nel codice seguente, `"I am a placeholder"` è una stringa:

```js
function checkGuess() {
  console.log("I am a placeholder");
}
```

È possibile dichiarare le stringhe usando virgolette doppie (`"`) o virgolette singole (`'`), ma occorre usare la stessa forma per l'inizio e la fine di una singola dichiarazione di stringa: non è possibile scrivere `"I am a placeholder'`.

Le stringhe possono essere dichiarate anche usando i backtick (`` ` ``). Le stringhe dichiarate in questo modo sono chiamate _template literal_ e possiedono alcune proprietà speciali. In particolare, è possibile incorporare al loro interno altre variabili o persino espressioni:

```js
const name = "Mahalia";

const greeting = `Hello ${name}`;
```

Questo fornisce un meccanismo per concatenare le stringhe.

## Condizionali

I blocchi di codice **condizionali** consentono di eseguire selettivamente il codice, a seconda che una certa condizione sia vera o meno. Assomigliano un po' a una funzione, ma sono diversi. Esploriamo i condizionali aggiungendoli al nostro esempio.

Si può tranquillamente affermare che non desideriamo che la funzione `checkGuess()` si limiti a produrre un messaggio segnaposto. Vogliamo che verifichi se il tentativo di un giocatore è corretto oppure no e che risponda in modo appropriato.

A questo punto, sostituire l'attuale funzione `checkGuess()` con questa versione:

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

È molto codice: vediamo ciascuna sezione e spieghiamo cosa fa.

- La prima riga dichiara una costante chiamata `userGuess` e imposta il suo valore al valore corrente inserito nel campo di testo. Questo valore viene inoltre passato attraverso il costruttore integrato `Number()`, per assicurarsi che sia effettivamente un numero.
- Successivamente, incontriamo il primo blocco di codice condizionale. La forma più semplice di blocco condizionale inizia con la parola chiave `if`, seguita da alcune parentesi e quindi da alcune parentesi graffe. All'interno delle parentesi viene incluso un test. Se il test restituisce `true`, viene eseguito il codice all'interno delle parentesi graffe. In caso contrario, non viene eseguito e si passa alla parte di codice successiva. In questo caso, viene verificato se la variabile `guessCount` è uguale a `1`, ovvero se questo è il primo tentativo del giocatore:

  ```js
  guessCount === 1;
  ```

  Se lo è, il contenuto di testo del paragrafo dei tentativi viene impostato su `Previous guesses:`. In caso contrario, non viene fatto nulla.

- Successivamente, viene usato un template literal per aggiungere il valore corrente di `userGuess` alla fine del paragrafo `guesses`, con uno spazio vuoto tra i due.
- Il blocco successivo esegue alcuni controlli:
  - Il primo `if (){ }` verifica se il tentativo dell'utente è uguale a `randomNumber`, impostato all'inizio del JavaScript. Se lo è, il giocatore ha indovinato correttamente e ha vinto il gioco, quindi viene mostrato un messaggio di congratulazioni in un gradevole colore verde, viene svuotato il contenuto del riquadro delle informazioni sul tentativo basso/alto e viene eseguita una funzione chiamata `setGameOver()`, che verrà discussa più avanti.
  - Ora è stato concatenato un altro test alla fine dell'ultimo usando una struttura `else if (){ }`. Questo verifica se il turno corrente è l'ultimo turno dell'utente. Se lo è, il programma esegue la stessa operazione del blocco precedente, ma con un messaggio di fine gioco invece di un messaggio di congratulazioni.
  - Il blocco finale concatenato alla fine di questo codice, `else { }`, contiene codice che viene eseguito solo se nessuno degli altri due test restituisce true: il giocatore non ha indovinato correttamente, ma ha ancora tentativi disponibili. In questo caso viene comunicato che ha sbagliato, quindi viene eseguito un altro test condizionale per verificare se il tentativo era superiore o inferiore alla risposta, mostrando un ulteriore messaggio appropriato per indicare se è più alto o più basso.

- Le ultime tre righe della funzione preparano il programma per l'invio del tentativo successivo. Viene aggiunto 1 alla variabile `guessCount` affinché il giocatore consumi il proprio turno (`++` è un'operazione di incremento, ossia aumenta di 1), quindi viene svuotato il valore del campo di testo del modulo e vi viene nuovamente impostato il focus, pronto per l'inserimento del tentativo successivo.

## Eventi

A questo punto, abbiamo una funzione `checkGuess()` ben implementata, ma non farà nulla perché non è ancora stata chiamata. Idealmente, si desidera chiamarla quando viene premuto il pulsante "Submit guess"; per farlo è necessario usare un **evento**. Gli eventi sono azioni che avvengono nel browser, come il clic su un pulsante, il caricamento di una pagina, la riproduzione di un video e così via, in risposta alle quali è possibile eseguire blocchi di codice. Gli **event listener** osservano eventi specifici e chiamano le **funzioni di gestione degli eventi**, che vengono eseguite in risposta all'attivazione di un evento.

Aggiungere la seguente riga sotto la funzione `checkGuess()`:

```js
guessSubmit.addEventListener("click", checkGuess);
```

Qui viene aggiunto un event listener al pulsante `guessSubmit`. Si tratta di un metodo che accetta due valori di input, chiamati _argomenti_: il tipo di evento da ascoltare, in questo caso `click`, come stringa, e la funzione da eseguire quando si verifica l'evento, in questo caso `checkGuess()`. Si noti che non è necessario specificare le parentesi quando lo si scrive all'interno di [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).

Provare ora a salvare e aggiornare il codice: l'esempio dovrebbe funzionare, fino a un certo punto. L'unico problema è che, se si indovina la risposta corretta o si esauriscono i tentativi, il gioco si interromperà perché non è ancora stata definita la funzione `setGameOver()`, che dovrebbe essere eseguita al termine del gioco. Aggiungiamo ora il codice mancante e completiamo la funzionalità dell'esempio.

## Completare la funzionalità del gioco

Aggiungiamo la funzione `setGameOver()` in fondo al codice e poi analizziamola. Aggiungere questo sotto il resto del JavaScript:

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

- Le prime due righe disabilitano l'input di testo e il pulsante del modulo impostando le loro proprietà `disabled` su `true`. Questo è necessario perché, in caso contrario, l'utente potrebbe inviare altri tentativi dopo la fine del gioco, compromettendo il funzionamento.
- Le tre righe successive generano un nuovo elemento {{htmlelement("button")}}, impostano la sua etichetta di testo su "Start new game" e lo aggiungono alla fine dell'HTML esistente.
- La riga finale imposta un event listener sul nuovo pulsante affinché, quando viene fatto clic su di esso, venga eseguita una funzione chiamata `resetGame()`.

Ora occorre definire anche `resetGame()`. Aggiungere il codice seguente, ancora una volta in fondo al JavaScript:

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

Questo blocco di codice piuttosto lungo reimposta completamente tutto allo stato iniziale del gioco, consentendo al giocatore di riprovare.

Nello specifico:

- Reimposta `guessCount` a 1.
- Svuota tutto il testo dai paragrafi informativi. Vengono selezionati tutti i paragrafi all'interno di `<div class="resultParas"></div>`, quindi viene eseguito un ciclo su ciascuno di essi, impostando il rispettivo `textContent` su `""`, ossia una stringa vuota.
- Rimuove dal codice il pulsante di reimpostazione.
- Riabilita gli elementi del modulo, svuota il campo di testo e vi imposta il focus, pronto per l'inserimento di un nuovo tentativo.
- Rimuove il colore di sfondo dal paragrafo `lastResult`.
- Genera un nuovo numero casuale, così non si dovrà semplicemente indovinare di nuovo lo stesso numero.

**A questo punto, dovrebbe esserci un gioco di base completamente funzionante: congratulazioni!**

In questo articolo resta solo da esaminare alcune altre importanti funzionalità del codice già viste, anche se forse non sono state riconosciute.

## Cicli

In precedenza sono stati menzionati i **cicli**, un concetto molto importante nella programmazione, che consente di eseguire ripetutamente una porzione di codice finché non viene soddisfatta una determinata condizione.

Esploriamo un esempio di base per mostrare cosa significa. Tornare alla [console JavaScript degli strumenti di sviluppo del browser](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools), incollare il codice seguente e premere <kbd>Enter</kbd>/<kbd>Return</kbd>:

```js
const fruits = ["apples", "bananas", "cherries"];
for (const fruit of fruits) {
  console.log(fruit);
}
```

Che cosa è successo? Le stringhe `'apples', 'bananas', 'cherries'` sono state stampate nella console.

Questo accade a causa del ciclo. La riga `const fruits = ['apples', 'bananas', 'cherries'];` crea un array, che è una raccolta di valori, in questo caso stringhe.

Viene quindi usato un ciclo [`for...of`](/it/docs/Web/JavaScript/Reference/Statements/for...of) per ottenere ciascun elemento dell'array ed eseguire del JavaScript su di esso. La riga `for (const fruit of fruits)` indica:

1. Ottenere il primo valore in `fruits` e memorizzarlo in una variabile chiamata `fruit`.
2. Eseguire il codice tra le parentesi graffe `{}`, che in questo caso invia il valore `fruit` alla console.
3. Memorizzare il valore successivo dell'array in `fruit` e ripetere il passaggio 2, fino a raggiungere la fine dell'array `fruits`.

Vediamo ora il ciclo nel gioco di indovinare il numero: il seguente codice si trova all'interno della funzione `resetGame()`:

```js
const resetParas = document.querySelectorAll(".resultParas p");
for (const resetPara of resetParas) {
  resetPara.textContent = "";
}
```

Questo codice crea una variabile contenente un elenco di tutti i paragrafi all'interno di `<div class="resultParas">` usando il metodo [`querySelectorAll()`](/it/docs/Web/API/Document/querySelectorAll), quindi esegue un ciclo su ciascuno di essi, rimuovendone il contenuto testuale.

Si noti che, anche se `resetPara` è una costante, è possibile modificare le sue proprietà interne come `textContent`.

## Riepilogo

Questo conclude la creazione dell'esempio. Si è arrivati alla fine: ottimo lavoro! Provare il codice finale oppure [usare qui la nostra versione completata](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/first-splash/number-guessing-game.html). Se non si riesce a far funzionare la propria versione dell'esempio, confrontarla con il [codice sorgente](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/first-splash/number-guessing-game.html).

Anche la lezione successiva può essere utile: discute ciò che può andare storto quando si scrive codice JavaScript, facendo riferimento nel frattempo al gioco "Indovina il numero".

{{PreviousMenuNext("Learn_web_development/Core/Scripting/What_is_JavaScript", "Learn_web_development/Core/Scripting/What_went_wrong", "Learn_web_development/Core/Scripting")}}
