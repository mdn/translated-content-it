---
title: Valori di ritorno delle funzioni
slug: Learn_web_development/Core/Scripting/Return_values
l10n:
  sourceCommit: b8c317e606fff19152e9431be45986c50846b0ac
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Build_your_own_function","Learn_web_development/Core/Scripting/Test_your_skills/Functions", "Learn_web_development/Core/Scripting")}}

C'è un ultimo concetto essenziale relativo alle funzioni da discutere: i valori di ritorno. Alcune funzioni non restituiscono un valore significativo, mentre altre sì. È importante capire quali siano questi valori, come usarli nel proprio codice e come fare in modo che le funzioni restituiscano valori utili. Di seguito verranno trattati tutti questi aspetti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Conoscenza di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi delle funzioni JavaScript illustrate nella lezione precedente.</td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Che cosa sono i valori di ritorno.</li>
          <li>Come usare i valori di ritorno delle funzioni esistenti.</li>
          <li>Aggiungere valori di ritorno alle proprie funzioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Che cosa sono i valori di ritorno?

I **valori di ritorno** sono esattamente ciò che il nome suggerisce: i valori che una funzione restituisce quando termina. I valori di ritorno sono già comparsi diverse volte, anche se forse non sono stati considerati esplicitamente.

Torniamo a un esempio familiare (da un [articolo precedente](/it/docs/Learn_web_development/Core/Scripting/Functions#built-in_browser_functions) di questa serie):

```js
const myText = "The weather is cold";
const newString = myText.replace("cold", "warm");
console.log(newString); // Should print "The weather is warm"
// the replace() string function takes a string,
// replaces one substring with another, and returns
// a new string with the replacement made
```

La funzione [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace) viene invocata sulla stringa `myText` e riceve due parametri:

- La sottostringa da trovare (`"cold"`).
- La stringa con cui sostituirla (`"warm"`).

Quando la funzione termina (finisce la sua esecuzione), restituisce un valore, ovvero una nuova stringa con la sostituzione effettuata. Nel codice precedente, il risultato di questo valore di ritorno viene salvato nella variabile `newString`.

Osservando la pagina di riferimento MDN della funzione [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace), è presente una sezione denominata [valore di ritorno](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace#return_value). È molto utile conoscere e comprendere quali valori vengono restituiti dalle funzioni, quindi si cerca di includere questa informazione ovunque possibile.

Alcune funzioni non restituiscono alcun valore. In questi casi, le pagine di riferimento elencano il valore di ritorno come [`void`](/it/docs/Web/JavaScript/Reference/Operators/void) oppure [`undefined`](/it/docs/Web/JavaScript/Reference/Global_Objects/undefined). Per esempio, nella funzione [`displayMessage()`](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-stage-4.html#L50) creata nell'articolo precedente, non viene restituito alcun valore specifico quando la funzione viene invocata. Fa semplicemente apparire una casella in un punto dello schermo: tutto qui.

In generale, un valore di ritorno viene usato quando la funzione rappresenta un passaggio intermedio in un calcolo di qualche tipo. Si vuole ottenere un risultato finale che coinvolge alcuni valori da calcolare mediante una funzione. Dopo aver calcolato il valore, la funzione può restituire il risultato affinché venga memorizzato in una variabile; questa variabile può poi essere usata nella fase successiva del calcolo.

## Come restituire un valore

Per restituire un valore da una funzione personalizzata, è necessario usare la keyword [`return`](/it/docs/Web/JavaScript/Reference/Statements/return). Questo è stato visto di recente nel nostro esempio [random-canvas-circles.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html). La funzione `draw()` disegna 100 cerchi casuali in un punto qualsiasi di un elemento HTML {{htmlelement("canvas")}}:

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

All'interno di ogni iterazione del ciclo vengono effettuate tre chiamate alla funzione `random()`, rispettivamente per generare un valore casuale per la _coordinata x_, la _coordinata y_ e il _raggio_ del cerchio corrente. La funzione `random()` accetta un parametro, un numero intero, e restituisce un numero intero casuale compreso tra `0` e quel numero. Ha questo aspetto:

```js
function random(number) {
  return Math.floor(Math.random() * number);
}
```

Questo potrebbe essere scritto nel modo seguente:

```js
function random(number) {
  const result = Math.floor(Math.random() * number);
  return result;
}
```

Tuttavia, la prima versione è più rapida da scrivere e più compatta.

A ogni chiamata della funzione viene restituito il risultato del calcolo `Math.floor(Math.random() * number)`. Questo valore di ritorno compare nel punto in cui è stata chiamata la funzione, quindi il codice prosegue.

Quindi, quando viene eseguito quanto segue:

```js
ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
```

Se le tre chiamate a `random()` restituissero rispettivamente i valori `500`, `200` e `35`, la riga verrebbe effettivamente eseguita come se fosse questa:

```js
ctx.arc(500, 200, 35, 0, 2 * Math.PI);
```

Le chiamate di funzione nella riga vengono eseguite per prime e i loro valori di ritorno sostituiscono le chiamate di funzione prima dell'esecuzione della riga stessa.

## Implementare i valori di ritorno delle funzioni

Proviamo a scrivere alcune funzioni che includano valori di ritorno.

1. Creare una copia locale del file [function-library.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-library.html) da GitHub. Si tratta di una semplice pagina HTML contenente un campo di testo {{htmlelement("input")}} e un paragrafo. È presente anche un elemento {{htmlelement("script")}}, nel quale è stato memorizzato un riferimento a entrambi gli elementi HTML in due variabili. Questa pagina consentirà di inserire un numero nella casella di testo e visualizzare sotto diversi numeri correlati.

2. Aggiungere alcune funzioni utili a questo elemento `<script>`, sotto le due righe esistenti:

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

   Le funzioni `squared()` e `cubed()` sono abbastanza evidenti: restituiscono il quadrato o il cubo del numero fornito come parametro. La funzione `factorial()` restituisce il [fattoriale](https://en.wikipedia.org/wiki/Factorial) del numero dato.

3. Includere un modo per stampare informazioni sul numero inserito nell'input di testo, aggiungendo il seguente event handler sotto le funzioni esistenti:

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

4. Salvare il codice, caricarlo in un browser e provarlo.

Ecco alcune spiegazioni relative alla funzione `addEventListener()` del passaggio 3:

- Aggiungendo un event listener `change`, questa funzione viene eseguita ogni volta che l'evento `change` viene attivato sull'input di testo, ovvero quando viene inserito e inviato un nuovo valore nell'`input` di testo. Inserire un valore, quindi rimuovere il focus dall'input premendo <kbd>Tab</kbd> o <kbd>Return</kbd>. Quando questa funzione anonima viene eseguita, il valore nell'`input` viene memorizzato nella costante `num`.
- L'istruzione `if` stampa un messaggio di errore se il valore inserito non è un numero. La condizione verifica se l'espressione `isNaN(num)` restituisce `true`. La funzione [`isNaN()`](/it/docs/Web/JavaScript/Reference/Global_Objects/isNaN) verifica se il valore `num` non è un numero: in tal caso restituisce `true`, altrimenti restituisce `false`.
- Se la condizione restituisce `false`, il valore `num` è un numero e la funzione stampa all'interno dell'elemento paragrafo una frase che indica i valori del quadrato, del cubo e del fattoriale del numero. La frase chiama le funzioni `squared()`, `cubed()` e `factorial()` per calcolare i valori richiesti.

> [!NOTE]
> In caso di difficoltà nel far funzionare l'esempio, confrontare il codice con la [versione completata su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-library-finished.html) (è possibile anche [vederla in esecuzione](https://mdn.github.io/learning-area/javascript/building-blocks/functions/function-library-finished.html)).

### Aggiungere alcune funzioni personali!

A questo punto, provare a scrivere un paio di funzioni personali e aggiungerle alla libreria. Che ne sarebbe della radice quadrata o cubica del numero? Oppure della circonferenza di un cerchio con un determinato raggio?

Alcuni suggerimenti aggiuntivi relativi alle funzioni:

- Osservare un altro esempio di scrittura della _gestione degli errori_ nelle funzioni. In genere, è una buona idea verificare che tutti i parametri necessari siano convalidati e che ai parametri facoltativi venga fornito un valore predefinito. In questo modo, sarà meno probabile che il programma generi errori.
- Considerare l'idea di creare una _libreria di funzioni_. Con il proseguire dell'attività di programmazione, si inizierà a svolgere più volte gli stessi tipi di operazioni. È una buona idea creare una libreria personale di funzioni di utilità per eseguire queste operazioni. Sarà possibile copiarle in nuovo codice oppure applicarle alle pagine HTML ovunque siano necessarie.

## Riepilogo

Ecco quindi: le funzioni sono divertenti, molto utili e, sebbene ci sia molto da dire riguardo alla loro sintassi e funzionalità, sono abbastanza comprensibili.

Nel prossimo articolo verranno proposti alcuni test per verificare quanto siano state comprese e memorizzate tutte le informazioni fornite sulle funzioni negli ultimi articoli.

## Vedere anche

- [Funzioni in dettaglio](/it/docs/Web/JavaScript/Reference/Functions): una guida dettagliata che tratta informazioni più avanzate sulle funzioni.
- [Funzioni di callback in JavaScript](https://www.impressivewebs.com/callback-functions-javascript/): un pattern JavaScript comune consiste nel passare una funzione a un'altra funzione _come argomento_. Viene quindi chiamata all'interno della prima funzione. Questo argomento va leggermente oltre lo scopo di questo corso, ma vale la pena approfondirlo presto.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Build_your_own_function","Learn_web_development/Core/Scripting/Test_your_skills/Functions", "Learn_web_development/Core/Scripting")}}
