---
title: Cosa è andato storto? Risolvere problemi JavaScript
short-title: Troubleshooting
slug: Learn_web_development/Core/Scripting/What_went_wrong
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting")}}

Quando hai costruito il gioco "Indovina il numero" nell'articolo precedente, potresti aver notato che non funzionava. Non temere — questo articolo mira a salvarti dal tirarti i capelli per tali problemi fornendoti alcuni suggerimenti su come trovare e correggere errori nei programmi JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, esperienza di base con la scrittura di JavaScript.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere i tipi di errore che possono verificarsi in JavaScript.</li>
          <li>Utilizzare <code>console.log()</code> per eseguire il debug degli errori.</li>
          <li>Esperienza di base con l'uso della console JavaScript degli strumenti di sviluppo del browser.</li>
          <li>Familiarità di base con i messaggi di errore JavaScript e cosa significano.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Tipi di errore

In generale, quando fai qualcosa di sbagliato nel codice, ci sono due tipi principali di errori che incontrerai:

- **Errori di sintassi**: Questi sono errori di battitura nel tuo codice che in realtà fanno sì che il programma non venga eseguito affatto, o si fermi a metà strada — di solito ti verranno forniti anche dei messaggi di errore. Questi di solito non sono troppo difficili da risolvere, a patto che tu sia familiare con gli strumenti giusti e sappia cosa significano i messaggi di errore!
- **Errori logici**: Questi sono errori in cui la sintassi è effettivamente corretta ma il codice non è quello che intendevi, il che significa che il programma si esegue correttamente ma fornisce risultati errati. Questi sono spesso più difficili da risolvere rispetto agli errori di sintassi, poiché di solito non c'è un messaggio di errore che ti indirizzi alla fonte dell'errore.

Va bene, quindi non è proprio _così_ semplice — ci sono alcuni altri differenziatori man mano che approfondisci. Ma le classificazioni sopra saranno sufficienti in questa fase iniziale della tua carriera. Esamineremo entrambi questi tipi andando avanti.

## Un esempio errato

Per iniziare, torniamo al nostro gioco di indovinare il numero — tranne che questa volta esploreremo una versione che ha alcuni errori introdotti deliberatamente. Vai su GitHub e fai una copia locale di [number-game-errors.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html) (vedilo [in esecuzione dal vivo qui](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html)).

1. Per iniziare, apri la copia locale nel tuo editor di testo preferito e nel tuo browser.
2. Prova a giocare — noterai che quando premi il pulsante "Submit guess", non funziona!

> [!NOTE]
> Potresti benissimo avere la tua versione dell'esempio di gioco che non funziona, che potresti voler correggere! Vorremmo comunque che tu lavorassi attraverso l'articolo con la nostra versione, in modo che tu possa apprendere le tecniche che stiamo insegnando qui. Poi potrai tornare indietro e provare a correggere il tuo esempio.

A questo punto, consultiamo la console per sviluppatori per vedere se riporta errori di sintassi, quindi cerchiamo di risolverli. Scoprirai come farlo qui sotto.

## Correggere errori di sintassi

All'inizio del corso ti abbiamo fatto digitare alcuni semplici comandi JavaScript nella [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) (se non ricordi come aprirla nel tuo browser, segui il link precedente per scoprire come). Ciò che è ancora più utile è che la console ti fornisce messaggi di errore ogni volta che esiste un errore di sintassi nel JavaScript che viene fornito al motore JavaScript del browser. Ora andiamo a caccia.

1. Vai alla scheda in cui hai aperto `number-game-errors.html` e apri la tua console JavaScript. Dovresti vedere un messaggio di errore simile al seguente: !["Pagina demo 'Number guessing game' in Firefox. Un errore è visibile nella console JavaScript: "X TypeError: guessSubmit.addeventListener is not a function [Learn More] (number-game-errors.html:87:19)".](not-a-function.png)
2. La prima riga del messaggio di errore è:

   ```plain
   Uncaught TypeError: guessSubmit.addeventListener is not a function
   number-game-errors.html:87:19
   ```

   - La prima parte, `Uncaught TypeError: guessSubmit.addeventListener is not a function`, ci dice qualcosa su cosa è andato storto.
   - La seconda parte, `number-game-errors.html:87:19`, ci dice dove nel codice è avvenuto l'errore: riga 87, carattere 19 del file "number-game-errors.html".

3. Se guardiamo alla riga 87 nel nostro editor di codice, troveremo questa riga:

   ```js
   guessSubmit.addeventListener("click", checkGuess);
   ```

4. Il messaggio di errore dice "guessSubmit.addeventListener is not a function", il che significa che la funzione che stiamo chiamando non è riconosciuta dall'interprete JavaScript. Spesso, questo messaggio di errore significa effettivamente che abbiamo scritto qualcosa in modo errato. Se non sei sicuro dell'ortografia corretta di un pezzo di sintassi, è spesso utile cercare la funzionalità su MDN. Il modo migliore per farlo attualmente è cercare "mdn _nome-della-funzionalità_" con il tuo motore di ricerca preferito. Ecco un collegamento rapido per risparmiarti del tempo in questo caso: [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).
5. Quindi, guardando questa pagina, sembra che abbiamo scritto il nome della funzione in modo errato! Ricorda che JavaScript è sensibile alle maiuscole e minuscole, quindi qualsiasi minima differenza di ortografia o maiuscole causerà un errore. Cambiare `addeventListener` in `addEventListener` dovrebbe risolvere questo problema. Fallo ora.

> [!NOTE]
> Consulta la nostra pagina di riferimento [TypeError: "x" is not a function](/it/docs/Web/JavaScript/Reference/Errors/Not_a_function) per maggiori dettagli su questo errore.

### Errori di sintassi secondo round

1. Salva la tua pagina e aggiorna, e dovresti vedere che l'errore è sparito.
2. Ora, se provi a inserire una supposizione e premi il pulsante "Submit guess", vedrai un altro errore! ![Screenshot della stessa demo "Number guessing game". Questa volta, un errore diverso è visibile nella console, che legge "X TypeError: lowOrHi is null".](variable-is-null.png)
3. Questa volta l'errore riportato è:

   ```plain
   Uncaught TypeError: can't access property "textContent", lowOrHi is null
   ```

   A seconda del browser che stai utilizzando, potresti vedere un messaggio diverso qui. Il messaggio sopra è quello che ti mostrerà Firefox, ma Chrome, per esempio, ti mostrerà questo:

   ```plain
   Uncaught TypeError: Cannot set properties of null (setting 'textContent')
   ```

   È lo stesso errore, ma i diversi browser lo descrivono in modo diverso.

   > [!NOTE]
   > Questo errore non è stato visualizzato non appena la pagina è stata caricata perché si è verificato all'interno di una funzione (all'interno del blocco `checkGuess() { }`). Come imparerai in modo più dettagliato nel nostro articolo successivo sulle [funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions), il codice all'interno delle funzioni viene eseguito in un ambito separato rispetto al codice fuori dalle funzioni. In questo caso, il codice non è stato eseguito e l'errore non è stato lanciato fino a quando la funzione `checkGuess()` è stata eseguita dalla riga 87.

4. Il numero di riga indicato nell'errore è il 79. Dai un'occhiata alla riga 79, e vedrai il seguente codice:

   ```js
   lowOrHi.textContent = "Last guess was too high!";
   ```

5. Questa riga sta cercando di impostare la proprietà `textContent` della variabile `lowOrHi` su una stringa di testo, ma non funziona perché `lowOrHi` non contiene ciò che dovrebbe. Vediamo perché — prova a cercare altre istanze di `lowOrHi` nel codice. La prima istanza che troverai è alla riga 51:

   ```js
   const lowOrHi = document.querySelector("lowOrHi");
   ```

6. A questo punto stiamo cercando di far sì che la variabile contenga un riferimento a un elemento nel documento HTML. Vediamo cosa contiene la variabile dopo che questa riga è stata eseguita. Aggiungi il seguente codice alla riga 54:

   ```js
   console.log(lowOrHi);
   ```

   Questo codice stamperà il valore di `lowOrHi` nella console dopo che abbiamo cercato di impostarlo alla riga 51. Vedi [`console.log()`](/it/docs/Web/API/Console/log_static) per maggiori informazioni.

7. Salva e aggiorna, e dovresti ora vedere il risultato di `console.log()` nella tua console. ![Screenshot della stessa demo. Un'istruzione di log è visibile nella console, leggendo semplicemente "null".](console-log-output.png) Certo, il valore di `lowOrHi` è `null` a questo punto, e questo si abbina con il messaggio di errore di Firefox `lowOrHi is null`. Quindi c'è sicuramente un problema con la riga 51. Il valore [`null`](/it/docs/Web/JavaScript/Reference/Operators/null) significa "niente", o "nessun valore". Quindi il nostro codice per impostare `lowOrHi` su un elemento sta andando storto.

8. Pensiamo a quale potrebbe essere il problema. La riga 51 sta utilizzando un metodo [`document.querySelector()`](/it/docs/Web/API/Document/querySelector) per ottenere un riferimento a un elemento selezionandolo con un selettore CSS. Guardando più in alto nel nostro file, possiamo trovare il paragrafo in questione:

   ```html
   <p class="lowOrHi"></p>
   ```

9. Quindi abbiamo bisogno di un selettore di classi qui, che inizi con un punto (`.`), ma il selettore che viene passato nel metodo `querySelector()` alla riga 51 non ha un punto. Questo potrebbe essere il problema! Prova a cambiare `lowOrHi` in `.lowOrHi` alla riga 51.
10. Prova a salvare e aggiornare di nuovo, e la tua dichiarazione `console.log()` dovrebbe restituire l'elemento `<p>` che vogliamo. Uff! Un altro errore risolto! Puoi ora eliminare la tua riga `console.log()`, o mantenerla per riferimento successivo — a tua scelta.

> [!NOTE]
> Vedi la nostra pagina di riferimento [TypeError: "x" is (not) "y"](/it/docs/Web/JavaScript/Reference/Errors/Unexpected_type) per maggiori dettagli su questo errore.

### Errori di sintassi terzo round

1. Ora, se provi a giocare di nuovo, dovresti avere più successo — il gioco dovrebbe funzionare perfettamente, fino a quando non termini il gioco, o indovinando il numero giusto, o esaurendo i tentativi.
2. A quel punto, il game fallisce di nuovo, e lo stesso errore viene visualizzato che abbiamo avuto all'inizio — "TypeError: resetButton.addeventListener is not a function"! Tuttavia, questa volta è elencato come proveniente dalla linea 95.
3. Guardando al numero di riga 95, è facile vedere che abbiamo commesso lo stesso errore qui. Dobbiamo di nuovo solo cambiare `addeventListener` in `addEventListener`. Fallo ora.

## Un errore logico

A questo punto, il gioco dovrebbe funzionare perfettamente, tuttavia dopo aver giocato un po' di volte noterai senza dubbio che il gioco sceglie sempre il numero 1 come numero "casuale" che devi indovinare. Decisamente non esattamente come vogliamo che il gioco si sviluppi!

C'è sicuramente un problema nella logica del gioco da qualche parte — il gioco non restituisce un errore; semplicemente non sta funzionando correttamente.

1. Cerca la variabile `randomNumber`, e le righe in cui il numero casuale viene impostato per la prima volta. L'istanza che memorizza il numero casuale che vogliamo indovinare all'inizio del gioco dovrebbe essere intorno alla riga numero 47:

   ```js
   let randomNumber = Math.floor(Math.random()) + 1;
   ```

2. E quella che genera il numero casuale prima di ciascun gioco successivo è intorno alla riga 114:

   ```js
   randomNumber = Math.floor(Math.random()) + 1;
   ```

3. Per controllare se queste righe sono effettivamente il problema, torniamo al nostro amico `console.log()` ancora una volta — inserisci la seguente riga subito sotto ciascuna delle due righe sopra:

   ```js
   console.log(randomNumber);
   ```

4. Salva e aggiorna, poi gioca a qualche gioco — vedrai che `randomNumber` è uguale a 1 in ogni punto in cui è registrato nella console.

### Lavorare attraverso la logica

Per risolvere questo problema, consideriamo come funziona questa linea. Innanzitutto, invochiamo [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random), che genera un numero decimale casuale tra 0 e 1, e.g., 0.5675493843.

```js
Math.random();
```

Successivamente, passiamo il risultato dell'invocazione di `Math.random()` attraverso [`Math.floor()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/floor), che arrotonda il numero passato fino al numero intero più vicino. Aggiungiamo quindi 1 a quel risultato:

```js
Math.floor(Math.random()) + 1;
```

Arrotondare un numero decimale casuale tra 0 e 1 verso il basso restituirà sempre 0, quindi aggiungere 1 restituirà sempre 1. Dobbiamo moltiplicare il numero casuale per 100 prima di arrotondarlo verso il basso. Il seguente ci darebbe un numero casuale tra 0 e 99:

```js
Math.floor(Math.random() * 100);
```

Da qui la nostra necessità di aggiungere 1, per ottenere un numero casuale tra 1 e 100:

```js
Math.floor(Math.random() * 100) + 1;
```

Prova ad aggiornare entrambe le righe in questo modo, poi salva e aggiorna— il gioco dovrebbe ora funzionare come intendiamo!

## Altri errori comuni

Ci sono altri errori comuni che incontrerai nel tuo codice. Questa sezione evidenzia la maggior parte di essi.

### Il programma dice sempre che hai vinto, indipendentemente dalla supposizione che inserisci

Questo potrebbe essere un altro sintomo di confusione tra gli operatori di assegnazione e di uguaglianza rigorosa. Per esempio, se cambiassimo questa riga all'interno di `checkGuess()`:

```js
if (userGuess === randomNumber) {
```

in

```js
if (userGuess = randomNumber) {
```

il test restituirebbe sempre `true`, causando al programma di segnalare che il gioco è stato vinto. Fai attenzione!

### SyntaxError: missing ) after argument list

Questo è abbastanza semplice — generalmente significa che hai dimenticato la parentesi di chiusura alla fine di una chiamata a funzione/metodo.

> [!NOTE]
> Vedi la nostra pagina di riferimento [SyntaxError: missing ) after argument list](/it/docs/Web/JavaScript/Reference/Errors/Missing_parenthesis_after_argument_list) per maggiori dettagli su questo errore.

### SyntaxError: missing : after property id

Questo errore di solito si riferisce a un oggetto JavaScript formattato in modo errato, ma in questo caso siamo riusciti a ottenerlo cambiando

```js
function checkGuess() {
```

in

```js
function checkGuess( {
```

Questo ha fatto sì che il browser pensasse che stessimo cercando di passare il contenuto della funzione alla funzione come argomento. Fai attenzione a quelle parentesi!

### SyntaxError: missing } after function body

Questo è facile — generalmente significa che ti è sfuggito uno dei tuoi parentesi graffe in una funzione o struttura condizionale. Abbiamo ottenuto questo errore cancellando una delle parentesi graffe di chiusura vicino al fondo della funzione `checkGuess()`.

### SyntaxError: expected expression, got '_string_' o SyntaxError: string literal contains an unescaped line break

Questi errori generalmente significano che hai dimenticato il segno di apertura o di chiusura di una stringa. Nel primo errore sopra, _string_ verrebbe sostituito con il carattere (o i caratteri) inaspettato trovato dal browser invece di un segno di virgolette all'inizio di una stringa. Il secondo errore significa che la stringa non è stata terminata con un segno di virgolette.

Per tutti questi errori, pensa a come abbiamo affrontato gli esempi che abbiamo esaminato nel walkthrough. Quando si verifica un errore, guarda il numero di riga che ti viene fornito, vai a quella riga e vedi se riesci a individuare cosa c'è di sbagliato. Tieni presente che l'errore non è necessariamente su quella riga, e anche che l'errore potrebbe non essere causato dallo stesso problema che abbiamo citato sopra!

> [!NOTE]
> Consulta le nostre pagine di riferimento [SyntaxError: Unexpected token](/it/docs/Web/JavaScript/Reference/Errors/Unexpected_token) e [SyntaxError: string literal contains an unescaped line break](/it/docs/Web/JavaScript/Reference/Errors/String_literal_EOL) per maggiori dettagli su questi errori.

## Sommario

Ecco, dunque, le basi per capire gli errori nei semplici programmi JavaScript. Non sarà sempre così semplice capire cosa c'è di sbagliato nel tuo codice, ma almeno questo ti risparmierà alcune ore di sonno e ti permetterà di progredire un po' più velocemente quando le cose non vanno bene, specialmente nelle fasi iniziali del tuo percorso di apprendimento.

## Vedi anche

- Ci sono molti altri tipi di errori che non sono elencati qui; stiamo compilando un riferimento che spiega cosa significano in dettaglio — vedi il [riferimento agli errori JavaScript](/it/docs/Web/JavaScript/Reference/Errors).
- Se ti imbatti in qualsiasi errore nel tuo codice che non sai come risolvere dopo aver letto questo articolo, puoi ottenere aiuto! Chiedi aiuto sui [canali di comunicazione](/it/docs/MDN/Community/Communication_channels). Dicci quale è il tuo errore, e cercheremo di helparti. Un elenco del tuo codice sarebbe utile anche.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting")}}
