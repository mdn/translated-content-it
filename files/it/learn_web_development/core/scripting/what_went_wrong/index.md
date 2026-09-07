---
title: Cosa è andato storto? Risoluzione dei problemi JavaScript
short-title: Troubleshooting
slug: Learn_web_development/Core/Scripting/What_went_wrong
l10n:
  sourceCommit: 2cd73547fb33680e060f65c5fb25d962a44f3cdc
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting")}}

Durante la creazione del gioco "Indovina il numero" nell'articolo precedente, potrebbe essere emerso che non funzionava. Niente paura: questo articolo ha lo scopo di evitare di strapparsi i capelli per problemi simili, fornendo alcuni suggerimenti su come trovare e correggere gli errori nei programmi JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, esperienza di base nella scrittura di JavaScript.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere i tipi di errore che possono verificarsi in JavaScript.</li>
          <li>Usare <code>console.log()</code> per eseguire il debug degli errori.</li>
          <li>Esperienza di base nell'uso della console JavaScript dei DevTools del browser.</li>
          <li>Familiarità di base con i messaggi di errore JavaScript e il loro significato.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Tipi di errore

In generale, quando si commette un errore nel codice, si incontrano tre tipi principali di errori:

- **Errori di sintassi**: sono errori di ortografia nel codice che impediscono del tutto l'esecuzione del programma oppure ne interrompono il funzionamento a metà; di solito vengono forniti anche alcuni messaggi di errore. In genere non sono troppo difficili da correggere, purché si conoscano gli strumenti giusti e il significato dei messaggi di errore.
- **Errori logici**: sono errori in cui la sintassi è corretta, ma il codice non corrisponde a ciò che era previsto, quindi il programma viene eseguito correttamente ma fornisce risultati errati. Spesso sono più difficili da correggere degli errori di sintassi, poiché di solito non viene visualizzato un messaggio di errore che indichi l'origine del problema.
- **Errori di runtime**: si verificano quando il codice ha una sintassi corretta e può quindi iniziare l'esecuzione, ma qualcosa va storto durante l'esecuzione. Ad esempio, tentare di chiamare qualcosa che non è effettivamente una funzione provoca un errore di runtime. La sintassi è corretta, ma l'operazione in sé non può essere eseguita.

Va bene, non è proprio _così_ semplice: esistono altre distinzioni approfondendo l'argomento. Tuttavia, le classificazioni sopra riportate sono sufficienti in questa fase iniziale del percorso. Questi tipi verranno esaminati nella sezione successiva.

## Un esempio con errori

Per iniziare, torniamo al gioco di indovinare il numero, ma questa volta verrà esplorata una versione con alcuni errori intenzionali. Andare su GitHub e creare una copia locale di [number-game-errors.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html) (è possibile vederlo [in esecuzione qui](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html)).

1. Per iniziare, aprire la copia locale nel proprio editor di testo preferito e nel browser.
2. Provare a giocare: si noterà che, premendo il pulsante "Submit guess", non funziona.

> [!NOTE]
> Potrebbe essere disponibile una propria versione dell'esempio di gioco che non funziona e che si desidera correggere. È comunque consigliabile seguire l'articolo usando la nostra versione, per apprendere le tecniche illustrate qui. In seguito sarà possibile tornare al proprio esempio e provare a correggerlo.

A questo punto, consultiamo la console per sviluppatori per verificare se segnala errori, quindi proviamo a correggerli. Di seguito viene spiegato come fare.

## Correggere gli errori segnalati nella console

In precedenza nel corso, sono stati inseriti alcuni semplici comandi JavaScript nella [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) (se non si ricorda come aprirla nel browser, seguire il collegamento precedente per scoprirlo). Ancora più utile è il fatto che la console fornisce messaggi di errore ogni volta che esiste un errore nel JavaScript fornito al motore JavaScript del browser. Ora iniziamo la ricerca.

1. Andare alla scheda in cui è aperto `number-game-errors.html` e aprire la console JavaScript. Dovrebbe essere visualizzato un messaggio di errore simile al seguente: !["Number guessing game" demo page in Firefox. One error is visible in the JavaScript console: "X TypeError: guessSubmit.addeventListener is not a function [Learn More] (number-game-errors.html:87:19)".](not-a-function.png)
2. La prima riga del messaggio di errore è:

   ```plain
   Uncaught TypeError: guessSubmit.addeventListener is not a function
   number-game-errors.html:87:19
   ```

   - La prima parte, `Uncaught TypeError: guessSubmit.addeventListener is not a function`, indica qualcosa riguardo a ciò che è andato storto.
   - La seconda parte, `number-game-errors.html:87:19`, indica dove nel codice si è verificato l'errore: riga 87, carattere 19 del file "number-game-errors.html".

3. Osservando la riga 87 nell'editor di codice, si troverà questa riga:

   ```js
   guessSubmit.addeventListener("click", checkGuess);
   ```

4. Il messaggio di errore riporta "guessSubmit.addeventListener is not a function", il che significa che l'interprete JavaScript non riconosce la funzione chiamata. Spesso, questo messaggio di errore significa in realtà che qualcosa è stato scritto in modo errato. Se non si è sicuri dell'ortografia corretta di una parte della sintassi, cercarla su MDN. Il modo migliore per farlo attualmente è cercare "mdn _name-of-feature_" con il proprio motore di ricerca preferito. Ecco una scorciatoia per risparmiare tempo in questo caso: [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).
5. Osservando questa pagina, sembra che l'errore sia nel nome della funzione scritto in modo errato. Si tratta di un errore di sintassi. Ricordare che JavaScript distingue tra maiuscole e minuscole, pertanto qualsiasi lieve differenza nell'ortografia o nell'uso delle maiuscole causerà un errore. La modifica di `addeventListener` in `addEventListener` dovrebbe risolvere il problema. Effettuare ora questa modifica.

> [!NOTE]
> Per maggiori dettagli su questo errore, consultare la pagina di riferimento [TypeError: "x" is not a function](/it/docs/Web/JavaScript/Reference/Errors/Not_a_function).

### Correggere un errore di runtime

1. Salvare la pagina e aggiornarla: l'errore dovrebbe essere scomparso.
2. Ora, se si prova a inserire un tentativo e a premere il pulsante Submit guess, verrà visualizzato un altro errore. ![Screenshot of the same "Number guessing game" demo. This time, a different error is visible in the console, reading "X TypeError: lowOrHi is null".](variable-is-null.png)
3. Questa volta, l'errore segnalato è:

   ```plain
   Uncaught TypeError: can't access property "textContent", lowOrHi is null
   ```

   A seconda del browser utilizzato, qui potrebbe essere visualizzato un messaggio diverso. Il messaggio sopra riportato è quello mostrato da Firefox, mentre Chrome, ad esempio, mostrerà questo:

   ```plain
   Uncaught TypeError: Cannot set properties of null (setting 'textContent')
   ```

   Si tratta dello stesso errore, ma browser diversi lo descrivono in modo diverso.

   > [!NOTE]
   > Questo errore non è comparso al caricamento della pagina perché si è verificato all'interno di una funzione (il blocco `checkGuess() { }`). Come verrà spiegato più dettagliatamente nel successivo [articolo sulle funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions), il codice all'interno delle funzioni viene eseguito in uno scope separato rispetto al codice esterno alle funzioni. In questo caso, il codice non è stato eseguito e l'errore non è stato generato finché la funzione `checkGuess()` non è stata eseguita dalla riga 87.

4. Il numero di riga indicato nell'errore è 79. Osservare la riga 79: verrà visualizzato il seguente codice:

   ```js
   lowOrHi.textContent = "Last guess was too high!";
   ```

5. Questa riga tenta di impostare la proprietà `textContent` della variabile `lowOrHi` su una stringa di testo, ma non funziona perché `lowOrHi` non contiene ciò che dovrebbe contenere. Vediamo perché: provare a cercare altre occorrenze di `lowOrHi` nel codice. La prima occorrenza si trova alla riga 51:

   ```js
   const lowOrHi = document.querySelector("lowOrHi");
   ```

6. A questo punto, si sta tentando di fare in modo che la variabile contenga un riferimento a un elemento nell'HTML del documento. Vediamo quale valore contiene la variabile dopo l'esecuzione di questa riga. Aggiungere il seguente codice alla riga 54:

   ```js
   console.log(lowOrHi);
   ```

   Questo codice stamperà il valore di `lowOrHi` nella console dopo aver tentato di impostarlo alla riga 51. Consultare [`console.log()`](/it/docs/Web/API/Console/log_static) per ulteriori informazioni.

7. Salvare e aggiornare: ora dovrebbe essere visualizzato il risultato di `console.log()` nella console. ![Screenshot of the same demo. One log statement is visible in the console, reading simply "null".](console-log-output.png) Effettivamente, il valore di `lowOrHi` è `null` a questo punto, e questo corrisponde al messaggio di errore Firefox `lowOrHi is null`. Quindi esiste sicuramente un problema alla riga 51. Il valore [`null`](/it/docs/Web/JavaScript/Reference/Operators/null) significa "niente" oppure "nessun valore". Il codice che imposta `lowOrHi` su un riferimento a un elemento non funziona correttamente.

8. Consideriamo quale potrebbe essere il problema. La riga 51 usa il metodo [`document.querySelector()`](/it/docs/Web/API/Document/querySelector) per selezionare un riferimento a un elemento tramite un selettore CSS. Più in alto nel file, è possibile trovare il paragrafo in questione:

   ```html
   <p class="lowOrHi"></p>
   ```

9. Serve quindi un selettore di classe, che inizia con un punto (`.`). Tuttavia, il selettore passato al metodo `querySelector()` alla riga 51 non contiene alcun punto. Questo potrebbe essere il problema. Provare a modificare `lowOrHi` in `.lowOrHi` alla riga 51.
10. Provare a salvare e aggiornare di nuovo: l'istruzione `console.log()` dovrebbe restituire l'elemento `<p>` desiderato. Finalmente! Un altro errore corretto. Ora è possibile eliminare la riga `console.log()` oppure conservarla come riferimento per il futuro: la scelta è libera.

> [!NOTE]
> Per maggiori dettagli su questo errore, consultare la pagina di riferimento [TypeError: "x" is (not) "y"](/it/docs/Web/JavaScript/Reference/Errors/Unexpected_type).

### Correggere un altro errore di sintassi

1. Ora, riprovando a giocare, si dovrebbe avere maggiore successo: il gioco dovrebbe funzionare perfettamente, finché non termina, indovinando il numero corretto oppure esaurendo i tentativi.
2. A quel punto, il gioco fallisce di nuovo e viene visualizzato lo stesso errore ottenuto all'inizio: "TypeError: resetButton.addeventListener is not a function". Questa volta, tuttavia, è indicato come proveniente dalla riga 95.
3. Osservando la riga 95, è facile vedere che è stato commesso lo stesso errore. Occorre modificare di nuovo `addeventListener` in `addEventListener`. Effettuare ora questa modifica.

## Un errore logico

A questo punto, il gioco dovrebbe funzionare correttamente. Dopo aver giocato alcune volte, tuttavia, si noterà senza dubbio che il gioco sceglie sempre 1 come numero "casuale" da indovinare. Non è certo il comportamento desiderato.

C'è sicuramente un problema nella logica del gioco: il gioco non restituisce un errore, semplicemente non funziona correttamente.

1. Cercare la variabile `randomNumber` e le righe in cui viene impostato inizialmente il numero casuale. L'occorrenza che memorizza il numero casuale da indovinare all'inizio del gioco dovrebbe trovarsi intorno alla riga 47:

   ```js
   let randomNumber = Math.floor(Math.random()) + 1;
   ```

2. Quella che genera il numero casuale prima di ogni partita successiva si trova intorno alla riga 114:

   ```js
   randomNumber = Math.floor(Math.random()) + 1;
   ```

3. Per verificare se queste righe costituiscono effettivamente il problema, ricorriamo di nuovo al nostro amico `console.log()`: inserire la seguente riga direttamente sotto ciascuna delle due righe precedenti:

   ```js
   console.log(randomNumber);
   ```

4. Salvare e aggiornare, quindi giocare alcune partite: si vedrà che `randomNumber` è uguale a 1 in ogni punto in cui viene registrato nella console.

### Analizzare la logica

Per risolvere il problema, consideriamo come funziona questa riga. Per prima cosa, viene invocato [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random), che genera un numero decimale casuale compreso tra 0 e 1, ad esempio 0.5675493843.

```js
Math.random();
```

Successivamente, il risultato dell'invocazione di `Math.random()` viene passato a [`Math.floor()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/floor), che arrotonda per difetto il numero passato all'intero più vicino. Quindi viene aggiunto 1 al risultato:

```js
Math.floor(Math.random()) + 1;
```

Arrotondare per difetto un numero decimale casuale compreso tra 0 e 1 restituirà sempre 0, quindi aggiungervi 1 restituirà sempre 1. Occorre moltiplicare il numero casuale per 100 prima di arrotondarlo per difetto. Il seguente codice fornirebbe un numero casuale compreso tra 0 e 99:

```js
Math.floor(Math.random() * 100);
```

Occorre quindi aggiungere 1 per ottenere un numero casuale compreso tra 1 e 100:

```js
Math.floor(Math.random() * 100) + 1;
```

Provare ad aggiornare entrambe le righe in questo modo, quindi salvare e aggiornare: ora il gioco dovrebbe funzionare come previsto.

## Altri errori comuni

Esistono altri errori comuni che possono comparire nel codice. Questa sezione evidenzia la maggior parte di essi.

### Il gioco termina dopo il primo tentativo errato

Questo potrebbe essere un altro sintomo della confusione tra gli operatori di assegnazione e di uguaglianza stretta. Ad esempio, se si modificasse questa riga all'interno di `checkGuess()`:

```js
} else if (guessCount === 10) {
```

in

```js
} else if (guessCount = 10) {
```

Il test restituirebbe sempre `true`, causando l'esecuzione di `setGameOver()` da parte del programma dopo il primo tentativo errato. Fare attenzione.

### SyntaxError: missing ) after argument list

Questo è piuttosto semplice: in genere significa che è stata omessa la parentesi di chiusura alla fine di una chiamata a funzione/metodo.

> [!NOTE]
> Per maggiori dettagli su questo errore, consultare la pagina di riferimento [SyntaxError: missing ) after argument list](/it/docs/Web/JavaScript/Reference/Errors/Missing_parenthesis_after_argument_list).

### SyntaxError: missing : after property id

Questo errore è solitamente correlato a un oggetto JavaScript formato in modo errato, ma in questo caso è stato ottenuto modificando

```js
function checkGuess() {
```

in

```js
function checkGuess( {
```

Questo ha indotto il browser a pensare che si stesse tentando di passare il contenuto della funzione come argomento. Fare attenzione a queste parentesi.

### SyntaxError: missing } after function body

Questo è semplice: in genere significa che manca una parentesi graffa in una funzione o struttura condizionale. Questo errore è stato ottenuto eliminando una delle parentesi graffe di chiusura vicino alla fine della funzione `checkGuess()`.

### SyntaxError: expected expression, got '_string_' oppure SyntaxError: string literal contains an unescaped line break

Questi errori significano generalmente che è stata omessa una virgoletta di apertura o di chiusura in un valore stringa. Nel primo errore sopra, _string_ verrebbe sostituito da caratteri imprevisti anziché da una virgoletta all'inizio di una stringa. Il secondo errore indica che la stringa non è stata terminata con una virgoletta.

Per tutti questi errori, considerare come sono stati affrontati gli esempi esaminati nella procedura guidata. Quando si verifica un errore, osservare il numero di riga fornito, andare a quella riga e verificare se è possibile individuare il problema. Tenere presente che l'errore non sarà necessariamente in quella riga e potrebbe non essere causato dallo stesso problema citato in precedenza.

> [!NOTE]
> Per maggiori dettagli su questi errori, consultare le pagine di riferimento [SyntaxError: Unexpected token](/it/docs/Web/JavaScript/Reference/Errors/Unexpected_token) e [SyntaxError: string literal contains an unescaped line break](/it/docs/Web/JavaScript/Reference/Errors/String_literal_EOL).

## Riepilogo

Ecco quindi le basi per individuare gli errori nei semplici programmi JavaScript. Non sarà sempre così facile capire cosa non va nel codice, ma almeno questo consentirà di risparmiare qualche ora di sonno e di progredire un po' più rapidamente quando le cose non vanno come previsto, specialmente nelle prime fasi del percorso di apprendimento.

## Vedi anche

- Esistono molti altri tipi di errori non elencati qui; è in fase di compilazione un riferimento che ne spiega dettagliatamente il significato. Consultare il [riferimento agli errori JavaScript](/it/docs/Web/JavaScript/Reference/Errors).
- Se si incontrano errori nel codice che non si è sicuri di come correggere dopo aver letto questo articolo, è possibile ottenere aiuto. Chiedere aiuto sui [canali di comunicazione](/it/docs/MDN/Community/Communication_channels). Descrivere l'errore e proveremo ad aiutare. Sarebbe utile anche un elenco del codice.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting")}}
