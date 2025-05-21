---
title: Valori di ritorno delle funzioni
slug: Learn_web_development/Core/Scripting/Return_values
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Build_your_own_function","Learn_web_development/Core/Scripting/Events", "Learn_web_development/Core/Scripting")}}

C'è un ultimo concetto essenziale sulle funzioni di cui dobbiamo discutere — i valori di ritorno. Alcune funzioni non restituiscono un valore significativo, ma altre sì. È importante capire quali sono i loro valori, come utilizzarli nel codice e come fare in modo che le funzioni restituiscano valori utili. Tratteremo tutti questi aspetti di seguito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>Una comprensione di <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a> e dei <a href="/it/docs/Learn_web_development/Core/Styling_basics">fondamenti di CSS</a>, familiarità con le basi delle funzioni JavaScript come trattato nella lezione precedente.</td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Cosa sono i valori di ritorno.</li>
          <li>Come usare i valori di ritorno delle funzioni esistenti.</li>
          <li>Aggiungere valori di ritorno alle proprie funzioni.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Cosa sono i valori di ritorno?

I **valori di ritorno** sono esattamente ciò che suggerisce il nome — i valori che una funzione restituisce quando completa la sua esecuzione. Hai già incontrato i valori di ritorno diverse volte, anche se potresti non averci pensato esplicitamente.

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

Quando la funzione completa la sua esecuzione, restituisce un valore, che è una nuova stringa con la sostituzione effettuata. Nel codice sopra, il risultato di questo valore di ritorno viene salvato nella variabile `newString`.

Se osservi la pagina di riferimento della funzione [`replace()`](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace) su MDN, vedrai una sezione chiamata [valore di ritorno](/it/docs/Web/JavaScript/Reference/Global_Objects/String/replace#return_value). È molto utile sapere e comprendere quali valori vengono restituiti dalle funzioni, quindi cerchiamo di includere questa informazione dove possibile.

Alcune funzioni non restituiscono alcun valore. (In questi casi, le nostre pagine di riferimento elencano il valore di ritorno come [`void`](/it/docs/Web/JavaScript/Reference/Operators/void) o [`undefined`](/it/docs/Web/JavaScript/Reference/Global_Objects/undefined).) Ad esempio, nella funzione [`displayMessage()`](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-stage-4.html#L50) costruita nell'articolo precedente, non viene restituito un valore specifico quando la funzione viene invocata. Si limita a far apparire una casella da qualche parte sullo schermo — e basta!

In generale, un valore di ritorno viene usato quando la funzione è un passaggio intermedio in un qualche tipo di calcolo. Vuoi ottenere un risultato finale, che coinvolge alcuni valori che devono essere calcolati da una funzione. Dopo che la funzione ha calcolato il valore, può restituire il risultato in modo che possa essere memorizzato in una variabile; e puoi usare questa variabile nella fase successiva del calcolo.

## Usare valori di ritorno nelle proprie funzioni

Per restituire un valore da una funzione personalizzata, è necessario utilizzare la parola chiave [`return`](/it/docs/Web/JavaScript/Reference/Statements/return). Abbiamo visto questo in azione di recente nel nostro esempio [random-canvas-circles.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/loops/random-canvas-circles.html). La nostra funzione `draw()` disegna 100 cerchi casuali da qualche parte su un {{htmlelement("canvas")}} HTML:

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

All'interno di ogni iterazione del ciclo, vengono effettuate tre chiamate alla funzione `random()`, per generare un valore casuale per la _coordinata x_, la _coordinata y_ e il _raggio_ del cerchio corrente, rispettivamente. La funzione `random()` prende un parametro — un numero intero — e restituisce un numero intero casuale tra `0` e quel numero. Si presenta in questo modo:

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

Quindi quando esegui il seguente:

```js
ctx.arc(random(WIDTH), random(HEIGHT), random(50), 0, 2 * Math.PI);
```

Se le tre chiamate a `random()` restituiscono i valori `500`, `200` e `35`, rispettivamente, la riga verrebbe effettivamente eseguita come se fosse questa:

```js
ctx.arc(500, 200, 35, 0, 2 * Math.PI);
```

Le chiamate alle funzioni sulla riga vengono eseguite per prime e i loro valori di ritorno vengono sostituiti alle chiamate alle funzioni, prima che la linea stessa venga quindi eseguita.

## Apprendimento attivo: Una funzione con valore di ritorno

Proviamo a scrivere alcune funzioni con valori di ritorno.

1. Fai una copia locale del file [function-library.html](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-library.html) da GitHub. Questa è una semplice pagina HTML contenente un campo di testo {{htmlelement("input")}} e un paragrafo. C'è anche un elemento {{htmlelement("script")}}, in cui abbiamo memorizzato un riferimento a entrambi gli elementi HTML in due variabili. Questa pagina ti permetterà di inserire un numero nella casella di testo e visualizzare diversi numeri relativi a esso qui sotto.

2. Aggiungi alcune funzioni utili a questo `<script>` sotto le due righe esistenti:

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

   Le funzioni `squared()` e `cubed()` sono abbastanza ovvie — restituiscono il quadrato o il cubo del numero fornito come parametro. La funzione `factorial()` restituisce il [fattoriale](https://en.wikipedia.org/wiki/Factorial) del numero dato.

3. Includi un modo per stampare le informazioni sul numero inserito nell'input di testo aggiungendo il seguente gestore di eventi sotto le funzioni esistenti:

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

4. Salva il tuo codice, caricalo in un browser e prova a usarlo.

Ecco alcune spiegazioni per la funzione `addEventListener` al passaggio 3 sopra:

- Aggiungendo un ascoltatore all'evento `change`, questa funzione viene eseguita ogni volta che l'evento `change` viene attivato sull'input di testo — cioè quando viene inserito un nuovo valore nel testo `input` e inviato (ad esempio, inserisci un valore, poi deseleziona l'input premendo <kbd>Tab</kbd> o <kbd>Return</kbd>). Quando questa funzione anonima viene eseguita, il valore nell'`input` viene memorizzato nella costante `num`.
- La dichiarazione if stampa un messaggio di errore se il valore inserito non è un numero. La condizione verifica se l'espressione `isNaN(num)` restituisce `true`. La funzione [`isNaN()`](/it/docs/Web/JavaScript/Reference/Global_Objects/isNaN) testa se il valore `num` non è un numero — in tal caso, restituisce `true`, e in caso contrario, restituisce `false`.
- Se la condizione restituisce `false`, il valore `num` è un numero e la funzione stampa una frase all'interno dell'elemento paragrafo che indica i valori quadrati, cubi e fattoriali del numero. La frase chiama le funzioni `squared()`, `cubed()` e `factorial()` per calcolare i valori richiesti.

> [!NOTE]
> Se hai difficoltà a far funzionare l'esempio, confronta il tuo codice con la [versione finale su GitHub](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/functions/function-library-finished.html) (vedi anche [esecuzione dal vivo](https://mdn.github.io/learning-area/javascript/building-blocks/functions/function-library-finished.html)), o chiedici aiuto.

## Ora tocca a te!

A questo punto, ti invitiamo a provare a scrivere alcune tue funzioni e aggiungerle alla libreria. Che ne dici della radice quadrata o cubica del numero? O la circonferenza di un cerchio con un determinato raggio?

Alcuni suggerimenti extra legati alle funzioni:

- Guarda un altro esempio di scrittura di _gestione degli errori_ nelle funzioni. In generale è una buona idea controllare che i parametri necessari siano validati e che eventuali parametri opzionali abbiano una sorta di valore predefinito fornito. In questo modo, il tuo programma sarà meno incline a generare errori.
- Rifletti sull'idea di creare una _libreria di funzioni_. Man mano che progredisci nella tua carriera di programmazione, inizierai a fare le stesse cose più e più volte. È una buona idea creare la tua libreria di funzioni di utilità per fare questo tipo di cose. Puoi copiarle nel nuovo codice, o addirittura applicarle a pagine HTML ovunque ne hai bisogno.

## Metti alla prova le tue abilità!

Hai raggiunto la fine di questo articolo, ma ricordi le informazioni più importanti? Puoi trovare ulteriori test per verificare che tu abbia conservato queste informazioni prima di procedere — vedi [Metti alla prova le tue abilità: Funzioni](/it/docs/Learn_web_development/Core/Scripting/Test_your_skills/Functions).

## Conclusione

Quindi, eccoci qui — le funzioni sono divertenti, molto utili, e sebbene ci sia molto da dire riguardo alla loro sintassi e funzionalità, sono abbastanza comprensibili.

Se c'è qualcosa che non hai capito, non esitare a rileggere l'articolo o [contattarci](/it/docs/MDN/Community/Communication_channels) per chiedere aiuto.

## Vedi anche

- [Funzioni in dettaglio](/it/docs/Web/JavaScript/Reference/Functions) — una guida dettagliata che copre informazioni più avanzate sulle funzioni.
- [Funzioni di callback in JavaScript](https://www.impressivewebs.com/callback-functions-javascript/) — un pattern comune in JavaScript è passare una funzione a un'altra funzione _come argomento_. Viene poi chiamata all'interno della prima funzione. Questo è un po' oltre lo scopo di questo corso, ma vale la pena di studiarlo prima o poi.

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Build_your_own_function","Learn_web_development/Core/Scripting/Events", "Learn_web_development/Core/Scripting")}}
