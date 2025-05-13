---
title: Cosa è andato storto? Risoluzione dei problemi JavaScript
short-title: Troubleshooting
slug: Learn_web_development/Core/Scripting/What_went_wrong
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting")}}

Quando ha costruito il gioco "Indovina il numero" nell'articolo precedente, potrebbe aver scoperto che non funzionava. Non si preoccupi — questo articolo mira a salvarla dal disperarsi su tali problemi fornendole alcuni consigli su come trovare e correggere errori nei programmi JavaScript.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e delle <a href="/it/docs/Learn_web_development/Core/Styling_basics">basi del CSS</a>, esperienza di base con la scrittura di JavaScript.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere i tipi di errore che possono verificarsi in JavaScript.</li>
          <li>Utilizzare <code>console.log()</code> per il debugging degli errori.</li>
          <li>Esperienza di base con l'uso della console JavaScript degli strumenti di sviluppo del browser.</li>
          <li>Familiarità di base con i messaggi di errore JavaScript e cosa significano.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Tipi di errore

In genere, quando si commette un errore nel codice, ci sono due principali tipi di errore con cui ci si imbatterà:

- **Errori di sintassi**: Questi sono errori di battitura nel codice che effettivamente causano il mancato funzionamento del programma o l'arresto del suo funzionamento a metà — di solito, verranno forniti anche dei messaggi di errore. Questi errori sono generalmente non troppo difficili da risolvere, a patto di familiarizzare con gli strumenti giusti e conoscere il significato dei messaggi di errore!
- **Errori logici**: Questi sono errori in cui la sintassi è effettivamente corretta ma il codice non è quello che intendeva essere, il che significa che il programma funziona correttamente ma fornisce risultati errati. Questi sono spesso più difficili da risolvere rispetto agli errori di sintassi, poiché di solito non esiste un messaggio di errore che la indirizza alla fonte dell'errore.

Ok, quindi non è proprio _così_ semplice — ci sono altri differenziatori quando si va più a fondo. Ma le classificazioni di cui sopra saranno sufficienti in questa fase iniziale della sua carriera. Esamineremo entrambi questi tipi in seguito.

## Un esempio errato

Per iniziare, torniamo al nostro gioco dell'indovinare i numeri — tranne che questa volta esploreremo una versione con alcuni errori deliberati introdotti. Vada su GitHub e si faccia una copia locale di [number-game-errors.html](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html) (vedilo [in esecuzione dal vivo qui](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/troubleshooting/number-game-errors.html)).

1. Per iniziare, apra la copia locale nel suo editor di testo preferito e nel suo browser.
2. Provi a giocare al gioco — noterà che quando preme il pulsante "Invia ipotesi", non funziona!

> [!NOTE]
> Potrebbe avere anche la sua versione dell'esempio di gioco che non funziona, che potrebbe desiderare di correggere! Vorremmo comunque che lei seguisse l'articolo con la nostra versione, in modo da poter apprendere le tecniche che stiamo insegnando qui. Poi può tornare indietro e provare a correggere il suo esempio.

A questo punto, consultiamo la console degli sviluppatori per vedere se segnala degli errori di sintassi, quindi proviamo a correggerli. Imparerà come fare qui sotto.

## Correzione degli errori di sintassi

All'inizio del corso le abbiamo fatto digitare alcuni semplici comandi JavaScript nella [console JavaScript degli strumenti per sviluppatori](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) (se non ricorda come aprire questa nel suo browser, segua il link precedente per scoprire come). Ciò che è ancora più utile è che la console le fornisce messaggi di errore ogni volta che esiste un errore di sintassi all'interno del JavaScript inviato al motore JavaScript del browser. Ora andiamo a caccia.

1. Vada alla scheda in cui ha aperto `number-game-errors.html`, e apra la sua console JavaScript. Dovrebbe vedere un messaggio di errore simile al seguente: ![Pagina demo "Gioco di indovinare il numero" in Firefox. Un errore è visibile nella console JavaScript: "X TypeError: guessSubmit.addeventListener is not a function [Learn More] (number-game-errors.html:87:19)".](not-a-function.png)
2. La prima riga del messaggio di errore è:

   ```plain
   Uncaught TypeError: guessSubmit.addeventListener is not a function
   number-game-errors.html:87:19
   ```

   - La prima parte, `Uncaught TypeError: guessSubmit.addeventListener is not a function`, ci dice qualcosa su cosa è andato storto.
   - La seconda parte, `number-game-errors.html:87:19`, ci indica dove nel codice è stato originato l'errore: riga 87, carattere 19 del file "number-game-errors.html".

3. Se guardiamo la riga 87 nel nostro editor di codice, troveremo questa riga:

   ```js
   guessSubmit.addeventListener("click", checkGuess);
   ```

4. Il messaggio di errore dice "guessSubmit.addeventListener is not a function", il che significa che la funzione che stiamo chiamando non è riconosciuta dall'interprete JavaScript. Spesso, questo messaggio di errore significa effettivamente che abbiamo scritto qualcosa male. Se non si è sicuri dell'ortografia corretta di un elemento di sintassi, è spesso utile cercare la funzione su MDN. Il modo migliore per farlo attualmente è cercare "mdn _nome-della-funzione_" con il suo motore di ricerca preferito. Ecco un collegamento diretto per risparmiarle del tempo in questo caso: [`addEventListener()`](/it/docs/Web/API/EventTarget/addEventListener).
5. Quindi, guardando questa pagina, l'errore sembra essere che abbiamo scritto il nome della funzione in modo errato! Ricordi che JavaScript è sensibile alle maiuscole, quindi qualsiasi differenza di ortografia o di maiuscole causerà un errore. Cambiare `addeventListener` in `addEventListener` dovrebbe risolvere il problema. Lo faccia ora.

> [!NOTE]
> Veda la nostra pagina di riferimento [TypeError: "x" is not a function](/it/docs/Web/JavaScript/Reference/Errors/Not_a_function) per maggiori dettagli su questo errore.

### Errori di sintassi round due

1. Salvi la sua pagina e la aggiorni, dovrebbe vedere che l'errore è scomparso.
2. Ora se prova a inserire un'ipotesi e premere il pulsante Invia ipotesi, vedrà un altro errore! ![Screenshot della stessa demo "Gioco di indovinare il numero". Questa volta, un errore diverso è visibile nella console, che legge "X TypeError: lowOrHi is null".](variable-is-null.png)
3. Questa volta l'errore riportato è:

   ```plain
   Uncaught TypeError: can't access property "textContent", lowOrHi is null
   ```

   A seconda del browser che sta utilizzando, potrebbe vedere un messaggio diverso qui. Il messaggio sopra è quello che Firefox le mostrerà, ma Chrome, per esempio, le mostrerà questo:

   ```plain
   Uncaught TypeError: Cannot set properties of null (setting 'textContent')
   ```

   È lo stesso errore, ma i browser diversi lo descrivono in modo diverso.

   > [!NOTE]
   > Questo errore non è venuto fuori appena la pagina è stata caricata perché è avvenuto all'interno di una funzione (all'interno del blocco `checkGuess() { }`). Come imparerà in maggior dettaglio nel nostro successivo [articolo sulle funzioni](/it/docs/Learn_web_development/Core/Scripting/Functions), il codice all'interno delle funzioni viene eseguito in un ambito separato rispetto al codice al di fuori delle funzioni. In questo caso, il codice non è stato eseguito e l'errore non è stato generato finché la funzione `checkGuess()` non è stata eseguita dalla riga 87.

4. Il numero di riga fornito nell'errore è 79. Dia un'occhiata alla riga 79, e vedrà il seguente codice:

   ```js
   lowOrHi.textContent = "Last guess was too high!";
   ```

5. Questa riga sta cercando di impostare la proprietà `textContent` della variabile `lowOrHi` su una stringa di testo, ma non funziona perché `lowOrHi` non contiene quello che dovrebbe. Vediamo perché — provi a cercare altre istanze di `lowOrHi` nel codice. La prima istanza che troverà sarà alla riga 51:

   ```js
   const lowOrHi = document.querySelector("lowOrHi");
   ```

6. A questo punto stiamo cercando di far contenere alla variabile un riferimento a un elemento nell'HTML del documento. Vediamo cosa contiene la variabile dopo che questa riga è stata eseguita. Aggiunga il seguente codice alla riga 54:

   ```js
   console.log(lowOrHi);
   ```

   Questo codice stamperà il valore di `lowOrHi` sulla console dopo aver provato a impostarlo nella riga 51. Veda [`console.log()`](/it/docs/Web/API/Console/log_static) per ulteriori informazioni.

7. Salvi e aggiorni, e ora dovrebbe vedere il risultato del `console.log()` nella sua console. ![Screenshot della stessa demo. Un'istruzione di log è visibile nella console, leggendo semplicemente "null".](console-log-output.png) Sicuramente, il valore di `lowOrHi` è `null` a questo punto, e questo corrisponde al messaggio di errore di Firefox `lowOrHi is null`. Quindi c'è sicuramente un problema con la riga 51. Il valore [`null`](/it/docs/Web/JavaScript/Reference/Operators/null) significa "niente", o "nessun valore". Quindi il nostro codice per impostare `lowOrHi` su un elemento sta andando male.

8. Pensiamo a quale potrebbe essere il problema. La riga 51 sta utilizzando un metodo [`document.querySelector()`](/it/docs/Web/API/Document/querySelector) per ottenere un riferimento a un elemento selezionandolo con un selettore CSS. Guardando più in alto nel nostro file, possiamo trovare il paragrafo in questione:

   ```html
   <p class="lowOrHi"></p>
   ```

9. Quindi abbiamo bisogno di un selettore di classe qui, che inizia con un punto (`.`), ma il selettore passato al metodo `querySelector()` nella riga 51 non ha un punto. Questo potrebbe essere il problema! Provi a cambiare `lowOrHi` in `.lowOrHi` nella riga 51.
10. Provi a salvare e aggiornare di nuovo, e la sua istruzione `console.log()` dovrebbe ora restituire il `<p>` elemento che vogliamo. Phew! Altro errore risolto! Può cancellare la sua linea di `console.log()` ora, o tenerla per riferimento futuro — a sua scelta.

> [!NOTE]
> Veda la nostra pagina di riferimento [TypeError: "x" is (not) "y"](/it/docs/Web/JavaScript/Reference/Errors/Unexpected_type) per maggiori dettagli su questo errore.

### Errori di sintassi round tre

1. Ora, se prova a giocare di nuovo, dovrebbe ottenere più successo — il gioco dovrebbe funzionare perfettamente, fino a quando non termina il gioco, sia indovinando il numero giusto, sia esaurendo i tentativi.
2. A quel punto, il gioco fallisce di nuovo, e viene riportato lo stesso errore che abbiamo ottenuto all'inizio — "TypeError: resetButton.addeventListener is not a function"! Tuttavia, questa volta è elencato come proveniente dalla riga 95.
3. Guardando il numero di riga 95, è facile vedere che abbiamo commesso lo stesso errore qui. Dobbiamo cambiare nuovamente `addeventListener` in `addEventListener`. Lo faccia ora.

## Un errore logico

A questo punto, il gioco dovrebbe funzionare correttamente, tuttavia dopo aver giocato alcune volte noterà senza dubbio che il gioco sceglie sempre 1 come numero "casuale" da indovinare. Sicuramente non è proprio come vogliamo che il gioco si svolga!

C'è sicuramente un problema nella logica del gioco da qualche parte — il gioco non produce un errore; semplicemente non funziona correttamente.

1. Cerchi la variabile `randomNumber`, e le righe dove il numero casuale è impostato per la prima volta. L'istanza che memorizza il numero casuale che vogliamo indovinare all'inizio del gioco dovrebbe essere intorno alla riga numero 47:

   ```js
   let randomNumber = Math.floor(Math.random()) + 1;
   ```

2. E quella che genera il numero casuale prima di ogni gioco successivo è intorno alla riga 114:

   ```js
   randomNumber = Math.floor(Math.random()) + 1;
   ```

3. Per verificare se queste righe sono effettivamente il problema, rivolgiamoci al nostro amico `console.log()` di nuovo — inserisca la seguente riga direttamente sotto ciascuna delle sopra due righe:

   ```js
   console.log(randomNumber);
   ```

4. Salvi e aggiorni, poi giochi alcuni giochi — vedrà che `randomNumber` è uguale a 1 in ogni punto in cui viene registrato sulla console.

### Lavorare attraverso la logica

Per risolvere questo problema, consideriamo come questa riga sta funzionando. Innanzitutto, invochiamo [`Math.random()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/random), che genera un numero decimale casuale tra 0 e 1, ad esempio, 0.5675493843.

```js
Math.random();
```

Successivamente, passiamo il risultato di invocare `Math.random()` attraverso [`Math.floor()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/floor), che arrotonda il numero passato al minor numero intero più vicino. Aggiungiamo quindi 1 a quel risultato:

```js
Math.floor(Math.random()) + 1;
```

Arrotondare un numero decimale casuale tra 0 e 1 al ribasso restituirà sempre 0, quindi aggiungere 1 a esso restituirà sempre 1. Dobbiamo moltiplicare il numero casuale per 100 prima di arrotondare al ribasso. Il seguente ci darebbe un numero casuale tra 0 e 99:

```js
Math.floor(Math.random() * 100);
```

Da qui, vogliamo aggiungere 1, per ottenere un numero casuale tra 1 e 100:

```js
Math.floor(Math.random() * 100) + 1;
```

Provi ad aggiornare entrambe le righe in questo modo, quindi salvi e aggiorni di nuovo — il gioco dovrebbe ora funzionare come intendevamo!

## Altri errori comuni

Ci sono altri errori comuni con cui ci si imbatterà nel codice. Questa sezione evidenzia la maggior parte di essi.

### Il programma dice sempre che ha vinto, indipendentemente dall'ipotesi che inserisce

Questo potrebbe essere un altro sintomo della confusione tra gli operatori di assegnazione e gli operatori di ugualianza stretta. Ad esempio, se cambiamo questa riga all'interno di `checkGuess()`:

```js
if (userGuess === randomNumber) {
```

in

```js
if (userGuess = randomNumber) {
```

il test restituirà sempre `true`, causando al programma di segnalare che il gioco è stato vinto. Stia attento!

### SyntaxError: missing ) after argument list

Questo errore è piuttosto semplice — in genere significa che ha dimenticato la parentesi di chiusura alla fine di una chiamata di funzione/metodo.

> [!NOTE]
> Veda la nostra pagina di riferimento [SyntaxError: missing ) after argument list](/it/docs/Web/JavaScript/Reference/Errors/Missing_parenthesis_after_argument_list) per maggiori dettagli su questo errore.

### SyntaxError: missing : after property id

Questo errore di solito riguarda un oggetto JavaScript formattato in modo errato, ma in questo caso siamo riusciti a ottenerlo cambiando

```js
function checkGuess() {
```

in

```js
function checkGuess( {
```

Questo ha causato che il browser pensasse che stessimo cercando di passare i contenuti della funzione alla funzione come argomento. Stia attento con quelle parentesi!

### SyntaxError: missing } after function body

Questo è semplice — generalmente significa che ha dimenticato una delle sue parentesi graffe di chiusura da una funzione o struttura condizionale. Abbiamo ottenuto questo errore eliminando una delle parentesi graffe di chiusura in fondo alla funzione `checkGuess()`.

### SyntaxError: expected expression, got '_string_' or SyntaxError: string literal contains an unescaped line break

Questi errori significano generalmente che ha lasciato fuori l'apertura o la chiusura delle virgolette di un valore stringa. Nel primo errore sopra, _string_ verrebbe sostituito con il/i carattere/i imprevisto/i che il browser ha trovato invece di un segno di virgolette all'inizio di una stringa. Il secondo errore significa che la stringa non è stata terminata con un segno di virgolette.

Per tutti questi errori, pensi a come abbiamo affrontato gli esempi che abbiamo esaminato durante la guida. Quando insorge un errore, guardi il numero di riga che le viene dato, vada a quella riga e veda se riesce a individuare cosa c'è che non va. Tenga presente che l'errore non si trova necessariamente su quella riga, e che l'errore potrebbe non essere causato dallo stesso problema che abbiamo citato sopra!

> [!NOTE]
> Veda le nostre pagine di riferimento [SyntaxError: Unexpected token](/it/docs/Web/JavaScript/Reference/Errors/Unexpected_token) e [SyntaxError: string literal contains an unescaped line break](/it/docs/Web/JavaScript/Reference/Errors/String_literal_EOL) per maggiori dettagli su questi errori.

## Riepilogo

Quindi ecco qui, le basi per capire gli errori nei semplici programmi JavaScript. Non sarà sempre così semplice capire cosa c'è che non va nel suo codice, ma almeno questo le risparmierà qualche ora di sonno e le permetterà di progredire un po' più velocemente quando le cose non vanno come previsto, specialmente nelle fasi iniziali del suo percorso di apprendimento.

## Vedi anche

- Ci sono molti altri tipi di errore che non sono elencati qui; stiamo compilando un riferimento che spiega cosa significano in dettaglio — veda la [riferimento errori JavaScript](/it/docs/Web/JavaScript/Reference/Errors).
- Se si imbatte in errori nel suo codice che non è sicuro di come risolvere dopo aver letto questo articolo, può ottenere aiuto! Chieda aiuto sui [canali di comunicazione](/it/docs/MDN/Community/Communication_channels). Ci dica quale errore ha, e cercheremo di aiutarla. Anche una lista del suo codice sarebbe utile.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/A_first_splash", "Learn_web_development/Core/Scripting/Variables", "Learn_web_development/Core/Scripting")}}
