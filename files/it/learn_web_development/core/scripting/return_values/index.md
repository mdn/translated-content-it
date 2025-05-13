---
title: Valori di ritorno delle funzioni
slug: Learn_web_development/Core/Scripting/Return_values
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Build_your_own_function","Learn_web_development/Core/Scripting/Events", "Learn_web_development/Core/Scripting")}}

C'è un ultimo concetto essenziale sulle funzioni di cui dobbiamo discutere: i valori di ritorno. Alcune funzioni non restituiscono un valore significativo, ma altre sì. È importante comprendere quali sono i loro valori, come utilizzarli nel suo codice e come far sì che le funzioni restituiscano valori utili. Tratteremo tutti questi aspetti di seguito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi delle funzioni in JavaScript come trattato nella lezione precedente.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono i valori di ritorno.</li>
          <li>Come utilizzare i valori di ritorno delle funzioni esistenti.</li>
          <li>Aggiungere valori di ritorno alle proprie funzioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono i valori di ritorno?

**I valori di ritorno** sono esattamente ciò che sembrano — i valori che una funzione restituisce quando termina. Ha già incontrato i valori di ritorno diverse volte, anche se potrebbe non averci pensato esplicitamente.

Torniamo a un esempio familiare (da un [articolo precedente](/it/docs/Learn_web_development/Core/Scripting/Functions#built-in_browser_functions) in questa serie):

```js
const myText = "The weather is cold";
const newString = myText.replace("cold", "warm");
console.log(newString); // Should print "The weather is warm"
// the replace() string function takes a string,
// replaces one substring with another, and returns
// a new string with the replacement made
```

La funzione [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace) viene invocata sulla stringa `myText` e vengono passati due parametri:

- La sottostringa da trovare (`"cold"`).
- La stringa con cui sostituirla (`"warm"`).

Quando la funzione completa (termina l'esecuzione), restituisce un valore, che è una nuova stringa con la sostituzione effettuata. Nel codice sopra, il risultato di questo valore di ritorno è salvato nella variabile `newString`.

Se consulta la pagina di riferimento di MDN per la funzione [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace), vedrà una sezione chiamata [return value](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace#return_value). È molto utile sapere e capire quali valori vengono restituiti dalle funzioni, quindi cerchiamo di includere queste informazioni ovunque possibile.

Alcune funzioni non restituiscono alcun valore. (In questi casi, le nostre pagine di riferimento elencano il valore di ritorno come [`void`](/it/docs/Web/JavaScript/Reference/Operators/void) o [`undefined`](/it/docs/Web/JavaScript/Reference/Global_Objects/undefined).) Ad esempio, nella funzione [`displayMessage()`](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-stage-4.html#L50) che abbiamo creato nell'articolo precedente, non viene restituito alcun valore specifico quando la funzione viene invocata. Fa solo apparire una casella da qualche parte sullo schermo — tutto qui!

In generale, un valore di ritorno viene utilizzato quando la funzione è un passaggio intermedio in un calcolo di qualche tipo. Si vuole arrivare a un risultato finale, che coinvolge alcuni valori che devono essere calcolati da una funzione. Dopo che la funzione calcola il valore, può restituire il risultato affinché possa essere memorizzato in una variabile; e può utilizzare questa variabile nella fase successiva del calcolo.

## Utilizzo dei valori di ritorno nelle proprie funzioni

Per restituire un valore da una funzione personalizzata, è necessario utilizzare la parola chiave [`return`](/it/docs/Web/JavaScript/Reference/Statements/return). L'abbiamo visto in azione di recente nel nostro esempio [random-canvas-circles.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html). La nostra funzione `draw()` disegna 100 cerchi casuali da qualche parte su un {{htmlelement("canvas")}} HTML:

```js
function draw() {
  ctx.clearRect(0, 0, WIDTH, HEIGHT);
  for (let i = 0; i < 100; i++) {
    ctx.beginPath();
    ctx.fillStyle = "rgb(255 0 0 / 50%)";
    ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
    ctx.fill();
  }
}
```

All'interno di ogni iterazione del loop, vengono effettuate tre chiamate alla funzione `random()`, per generare un valore casuale per l'attuale _coordinata x_, _coordinata y_ e _raggio_ del cerchio, rispettivamente. La funzione `random()` accetta un parametro — un numero intero — e restituisce un numero intero casuale compreso tra `0` e quel numero. Si presenta così:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}
```

Questo potrebbe essere scritto come segue:

```js
function random(number) {
  const result = Math.floor(Math.random() * number);
  return result;
}
```

Ma la prima versione è più veloce da scrivere e più compatta.

Stiamo restituendo il risultato del calcolo `Math.floor(Math.random() * number)` ogni volta che la funzione viene chiamata. Questo valore di ritorno appare nel punto in cui la funzione è stata chiamata, e il codice continua.

Quindi quando esegue il seguente:

```js
ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
```

Se le tre chiamate a `random()` restituiscono i valori `500`, `200` e `35`, rispettivamente, la riga verrebbe effettivamente eseguita come se fosse questa:

```js
ctx.arc(500, 200, 35, 0, 2 * Math.PI);
```

Le chiamate di funzione sulla riga vengono eseguite per prime e i loro valori di ritorno vengono sostituiti alle chiamate di funzione, prima che l'intera riga venga poi eseguita.

## Apprendimento attivo: Una funzione valore di ritorno

Proviamo a scrivere alcune funzioni con valori di ritorno.

1. Crei una copia locale del file [function-library.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-library.html) da GitHub. Questa è una semplice pagina HTML che contiene un campo {{htmlelement("input")}} di testo e un paragrafo. C'è anche un elemento {{htmlelement("script")}}, nel quale abbiamo memorizzato un riferimento a entrambi gli elementi HTML in due variabili. Questa pagina le permetterà di inserire un numero nella casella di testo e visualizzare diversi numeri correlati sotto.

2. Aggiungi alcune funzioni utili a questo elemento `<script>` sotto le due righe esistenti:

   ```js
   function squared(num) {
     return num * num;
   }

   function cubed(num) {
     return num * num * num;
   }

   function factorial(num) {
     if (num < 0) return undefined;
     if (num === 0) return 1;
     let x = num - 1;
     while (x > 1) {
       num *= x;
       x--;
     }
     return num;
   }
   ```

   Le funzioni `squared()` e `cubed()` sono abbastanza ovvie — restituiscono il quadrato o il cubo del numero dato come parametro. La funzione `factorial()` restituisce il [fattoriale](https://en.wikipedia.org/wiki/Factorial) del numero dato.

3. Includa un modo per stampare informazioni sul numero inserito nel campo di testo aggiungendo il seguente gestore di eventi sotto le funzioni esistenti:

   ```js
   input.addEventListener("change", () => {
     const num = parseFloat(input.value);
     if (isNaN(num)) {
       para.textContent = "You need to enter a number!";
     } else {
       para.textContent = `${num} squared is ${squared(num)}. `;
       para.textContent += `${num} cubed is ${cubed(num)}. `;
       para.textContent += `${num} factorial is ${factorial(num)}. `;
     }
   });
   ```

4. Salvi il codice, lo carichi in un browser e lo provi.

Ecco alcune spiegazioni per la funzione `addEventListener` al passaggio 3 sopra:

- Aggiungendo un listener all'evento `change`, questa funzione viene eseguita ogni volta che l'evento `change` si verifica nel campo di testo — cioè quando viene inserito un nuovo valore nel testo `input` e inviato (ad esempio, inserire un valore, quindi deselezionare l'input premendo <kbd>Tab</kbd> o <kbd>Invio</kbd>). Quando viene eseguita questa funzione anonima, il valore nell'`input` viene memorizzato nella costante `num`.
- La struttura if stampa un messaggio di errore se il valore inserito non è un numero. La condizione verifica se l'espressione `isNaN(num)` restituisce `true`. La funzione [`isNaN()`](/it/docs/Web/JavaScript/Reference/Global_Objects/isNaN) testa se il valore `num` non è un numero — se è così, restituisce `true`, e se non lo è, restituisce `false`.
- Se la condizione restituisce `false`, il valore `num` è un numero e la funzione stampa una frase all'interno dell'elemento paragrafo che indica i valori del quadrato, del cubo e del fattoriale del numero. La frase chiama le funzioni `squared()`, `cubed()` e `factorial()` per calcolare i valori richiesti.

> [!NOTE]
> Se ha difficoltà a far funzionare l'esempio, controlli il suo codice confrontandolo con la [versione finita su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-library-finished.html) (veda anche [un'anteprima dell'esecuzione live](https://mdn.github.io/learning-area/javascript/building-blocks/functions/function-library-finished.html)), o ci chieda aiuto.

## Ora è il suo turno!

A questo punto, vorremmo che provasse a scrivere alcune funzioni proprie e aggiungerle alla libreria. Che ne dice della radice quadrata o cubica del numero? O della circonferenza di un cerchio con un raggio dato?

Alcuni suggerimenti aggiuntivi relativi alle funzioni:

- Osservi un altro esempio di scrittura di _gestione degli errori_ nelle funzioni. In generale, è una buona idea verificare che eventuali parametri necessari siano validati e che i parametri opzionali abbiano fornito un valore predefinito. In questo modo, il suo programma sarà meno incline a generare errori.
- Pensi all'idea di creare una _libreria di funzioni_. Man mano che progredirà nella sua carriera di programmazione, inizierà a fare le stesse cose più e più volte. È una buona idea creare la propria libreria di funzioni utilitarie per fare questo genere di cose. Potrà copiarle nel nuovo codice, o anche solo applicarle alle pagine HTML ovunque ne abbia bisogno.

## Metta alla prova le sue abilità!

Ha raggiunto la fine di questo articolo, ma riesce a ricordare le informazioni più importanti? Può trovare ulteriori test per verificare di aver memorizzato queste informazioni prima di procedere — veda [Test your skills: Functions](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Functions).

## Conclusione

Ecco tutto: le funzioni sono divertenti, molto utili e, anche se c'è molto da dire riguardo alla loro sintassi e funzionalità, sono abbastanza comprensibili.

Se c'è qualcosa che non ha capito, senta liberamente di rileggere l'articolo o di [contattarci](/it/docs/MDN/Community/Communication_channels) per chiedere aiuto.

## Vedi anche

- [Funzioni in-depth](/it/docs/Web/JavaScript/Reference/Functions) — una guida dettagliata che copre informazioni più avanzate relative alle funzioni.
- [Callback functions in JavaScript](https://www.impressivewebs.com/callback-functions-javascript/) — un pattern comune in JavaScript è quello di passare una funzione a un'altra funzione _come argomento_. Viene quindi chiamata all'interno della prima funzione. Questo è un po' oltre lo scopo di questo corso, ma vale la pena studiarlo prima di troppo tempo.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Build_your_own_function","Learn_web_development/Core/Scripting/Events", "Learn_web_development/Core/Scripting")}}
