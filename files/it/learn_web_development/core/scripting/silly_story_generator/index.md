---
title: "Sfida: Generatore di storie divertenti"
short-title: "Sfida: Generatore di storie"
slug: Learn_web_development/Core/Scripting/Silly_story_generator
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting")}}

In questa sfida Lei sarà incaricato di applicare alcune delle conoscenze acquisite negli articoli di questo modulo per creare un'app divertente che genera storie casuali e buffe. Si diverta!

## Punto di partenza

Per iniziare questa sfida, Lei dovrebbe:

- Andare a [prendere il file HTML](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/assessment-start/index.html) per l'esempio, salvarne una copia locale come `index.html` in una nuova directory da qualche parte sul Suo computer, e iniziare la sfida in locale. Questo contiene anche il CSS per stilizzare l'esempio.
- Andare alla [pagina contenente il testo grezzo](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/assessment-start/raw-text.txt) e tenerla aperta in una scheda separata del browser. Ne avrà bisogno più tardi.

In alternativa, potrebbe usare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/). Potrebbe incollare l'HTML, il CSS e il JavaScript in uno di questi editor online. Se l'editor online che sta usando non ha un pannello separato per JavaScript, è libero di metterlo in linea in un elemento `<script>` all'interno della pagina HTML.

> [!NOTE]
> Se incontra difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Brief del progetto

Le sono stati forniti alcuni file HTML/CSS di base e alcune stringhe di testo e funzioni JavaScript; dovrà scrivere il JavaScript necessario per trasformarli in un programma funzionante, che faccia quanto segue:

- Genera una storia buffa quando viene premuto il pulsante "Generate random story".
- Sostituisce il nome predefinito "Bob" nella storia con un nome personalizzato, solo se un nome personalizzato viene inserito nel campo di testo "Enter custom name" prima di premere il pulsante generate.
- Converte i valori e le unità di peso e temperatura statunitensi predefiniti nella storia in equivalenti britannici se il pulsante radio UK è selezionato prima di premere il pulsante generate.
- Genera una nuova storia buffa casuale ogni volta che viene premuto il pulsante.

Il seguente screenshot mostra un esempio di quello che dovrebbe essere l'output del programma ultimato:

![L'app generatore di storie buffe consiste di un campo di testo, due pulsanti radio e un pulsante per generare una storia casuale.](screen_shot_2018-09-19_at_10.01.38_am.png)

Per darLe un'idea, [dia un'occhiata all'esempio finito](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/assessment-finished/) (non sbirci nel codice sorgente!)

## Passi da completare

Le sezioni seguenti descrivono cosa deve fare.

Impostazione di base:

1. Crei un nuovo file chiamato `main.js`, nella stessa directory del Suo file `index.html`.
2. Applichi il file JavaScript esterno al Suo HTML inserendo un elemento {{htmlelement("script")}} nel Suo HTML che faccia riferimento a `main.js`. Lo ponga appena prima del tag di chiusura `</body>`.

Variabili e funzioni iniziali:

1. Nel file di testo grezzo, copi tutto il codice sotto l'intestazione "1. COMPLETE VARIABLE AND FUNCTION DEFINITIONS" e lo incolli all'inizio del file `main.js`. Questo Le fornisce tre variabili che memorizzano i riferimenti al campo di testo "Enter custom name" (`customName`), al pulsante "Generate random story" (`randomize`), e all'elemento {{htmlelement("p")}} alla fine del corpo HTML in cui la storia verrà copiata (`story`). Inoltre ha una funzione chiamata `randomValueFromArray()` che prende un array e restituisce uno degli elementi memorizzati nell'array in modo casuale.
2. Ora guardi la seconda sezione del file di testo grezzo — "2. RAW TEXT STRINGS". Questa contiene stringhe di testo che fungeranno da input nel nostro programma. Vorremmo che contenesse queste all'interno di variabili dentro `main.js`:

   1. Memorizzi la prima, lunga stringa di testo all'interno di una variabile chiamata `storyText`.
   2. Memorizzi il primo set di tre stringhe all'interno di un array chiamato `insertX`.
   3. Memorizzi il secondo set di tre stringhe all'interno di un array chiamato `insertY`.
   4. Memorizzi il terzo set di tre stringhe all'interno di un array chiamato `insertZ`.

Inserimento del gestore degli eventi e funzione incompleta:

1. Ora ritorni al file di testo grezzo.
2. Copi il codice trovato sotto l'intestazione "3. EVENT LISTENER AND PARTIAL FUNCTION DEFINITION" e lo incolli alla fine del Suo file `main.js`. Questo:

   - Aggiunge un gestore dell'evento click alla variabile `randomize`, così che quando il pulsante che rappresenta viene cliccato, venga eseguita la funzione `result()`.
   - Aggiunge una definizione di funzione `result()` parzialmente completata al Suo codice. Per il resto della sfida, Lei completerà le linee all'interno di questa funzione per completarla e farla funzionare correttamente.

Completamento della funzione `result()`:

1. Crei una nuova variabile chiamata `newStory`, e imposti il suo valore uguale a `storyText`. Questo è necessario per poter creare una nuova storia casuale ogni volta che il pulsante viene premuto e la funzione viene eseguita. Se apportassimo modifiche direttamente a `storyText`, potremmo generare una nuova storia solo una volta.
2. Crei tre nuove variabili chiamate `xItem`, `yItem` e `zItem`, e le renda uguali al risultato della chiamata di `randomValueFromArray()` sui Suoi tre array (il risultato in ciascun caso sarà un elemento casuale di ognuno degli array su cui viene chiamato). Ad esempio può chiamare la funzione e farle restituire una stringa casuale da `insertX` scrivendo `randomValueFromArray(insertX)`.
3. Successivamente vogliamo sostituire i tre segnaposto nella stringa `newStory` — `:insertx:`, `:inserty:`, e `:insertz:` — con le stringhe memorizzate in `xItem`, `yItem`, e `zItem`. Ci sono due possibili metodi di stringa che Le aiuteranno in questo caso — in ciascun caso, faccia sì che la chiamata al metodo sia uguale a `newStory`, così ogni volta che viene chiamato, `newStory` viene reso uguale a se stesso, ma con le sostituzioni fatte. Così ogni volta che il pulsante viene premuto, questi segnaposto vengono sostituiti ciascuno con una stringa buffa casuale. Come ulteriore suggerimento, a seconda del metodo scelto, potrebbe dover fare una delle chiamate due volte.
4. All'interno del primo blocco `if`, aggiunga un'altra chiamata al metodo di sostituzione delle stringhe per sostituire il nome 'Bob' trovato nella stringa `newStory` con la variabile `name`. In questo blocco stiamo dicendo "Se un valore è stato inserito nel campo di testo `customName`, sostituire Bob nella storia con quel nome personalizzato."
5. All'interno del secondo blocco `if`, stiamo controllando se il pulsante radio `uk` è stato selezionato. Se sì, vogliamo convertire i valori di peso e temperatura nella storia da libbre e Fahrenheit in pietre e centigradi. Quello che deve fare è quanto segue:

   1. Cerchi le formule per convertire libbre in pietre, e Fahrenheit in centigradi.
   2. All'interno della linea che definisce la variabile `weight`, sostituisca 300 con un calcolo che converte 300 libbre in pietre. Concatenare `' stone'` alla fine del risultato della chiamata complessiva a `Math.round()`.
   3. All'interno della linea che definisce la variabile `temperature`, sostituisca 94 con un calcolo che converte 94 Fahrenheit in centigradi. Concatenare `' centigrade'` alla fine del risultato della chiamata complessiva a `Math.round()`.
   4. Poco sotto le due definizioni di variabili, aggiunga due ulteriori righe di sostituzione delle stringhe che sostituiscono '94 fahrenheit' con il contenuto della variabile `temperature`, e '300 pounds' con il contenuto della variabile `weight`.

6. Infine, nella penultima linea della funzione, renda la proprietà `textContent` della variabile `story` (che si riferisce al paragrafo) uguale a `newStory`.

## Suggerimenti e consigli

- Non è necessario modificare l'HTML in alcun modo, eccetto per applicare il JavaScript al Suo HTML.
- Se Lei non è sicuro se il JavaScript è applicato correttamente al Suo HTML, provi a rimuovere temporaneamente tutto il resto dal file JavaScript, aggiungendo un semplice bit di JavaScript che Lei sa produrrà un effetto evidente, poi salvando e aggiornando. Il seguente esempio trasforma lo sfondo dell'elemento {{htmlelement("html")}} in rosso — così l'intera finestra del browser dovrebbe diventare rossa se il JavaScript è applicato correttamente:

  ```js
  document.querySelector("html").style.backgroundColor = "red";
  ```

- [`Math.round()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/round) è un metodo JavaScript integrato che arrotonda il risultato di un calcolo al numero intero più vicino.
- Ci sono tre istanze di stringhe che devono essere sostituite. Può ripetere il metodo `replace()` più volte, o usare `replaceAll()`. Ricordi, le stringhe sono immutabili!

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting")}}
