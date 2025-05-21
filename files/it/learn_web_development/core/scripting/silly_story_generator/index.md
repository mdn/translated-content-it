---
title: "Sfida: Generatore di storie buffe"
short-title: "Sfida: Generatore di storie"
slug: Learn_web_development/Core/Scripting/Silly_story_generator
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting")}}

In questa sfida ti verrà chiesto di applicare alcune delle conoscenze acquisite negli articoli di questo modulo per creare un'app divertente che genera storie buffe casuali. Buon divertimento!

## Punto di partenza

Per iniziare questa sfida, dovresti:

- [Scarica il file HTML](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/assessment-start/index.html) per l'esempio, salva una copia locale come `index.html` in una nuova directory sul tuo computer, e svolgi la sfida localmente per cominciare. Include anche il CSS per stilizzare l'esempio.
- Vai alla [pagina contenente il testo grezzo](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/assessment-start/raw-text.txt) e tienila aperta in un'altra scheda del browser. Ti servirà più avanti.

In alternativa, puoi usare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/). Puoi incollare l'HTML, il CSS e il JavaScript in uno di questi editor online. Se l'editor online che stai usando non ha un pannello JavaScript separato, senti libero di metterlo inline in un elemento `<script>` all'interno della pagina HTML.

> [!NOTE]
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Sono stati forniti alcuni HTML/CSS grezzi e alcune stringhe di testo e funzioni JavaScript; devi scrivere il JavaScript necessario per trasformarlo in un programma funzionante, che faccia quanto segue:

- Genera una storia buffa quando il pulsante "Genera storia casuale" viene premuto.
- Sostituisce il nome predefinito "Bob" nella storia con un nome personalizzato, solo se un nome personalizzato è inserito nel campo di testo "Inserisci nome personalizzato" prima che il pulsante di generazione venga premuto.
- Converte i valori e le unità di peso e temperatura predefiniti degli USA nella storia negli equivalenti UK, se il pulsante radio del Regno Unito è selezionato prima che il pulsante di generazione venga premuto.
- Genera una nuova storia buffa casuale ogni volta che il pulsante viene premuto.

La seguente schermata mostra un esempio di quello che dovrebbe generare il programma finito:

![L'app del generatore di storie buffe consiste in un campo di testo, due pulsanti radio e un pulsante per generare una storia casuale.](screen_shot_2018-09-19_at_10.01.38_am.png)

Per darti un'idea più chiara, [dai un'occhiata all'esempio finito](https://mdn.github.io/learning-area/javascript/introduction-to-js-1/assessment-finished/) (non sbirciare il codice sorgente!)

## Passi da completare

Le seguenti sezioni descrivono cosa devi fare.

Impostazione di base:

1. Crea un nuovo file chiamato `main.js`, nella stessa directory del tuo file `index.html`.
2. Applica il file JavaScript esterno al tuo HTML inserendo un elemento {{htmlelement("script")}} nel tuo HTML che fa riferimento a `main.js`. Mettilo appena prima del tag di chiusura `</body>`.

Variabili iniziali e funzioni:

1. Nel file di testo grezzo, copia tutto il codice sotto l'intestazione "1. DEFINIZIONI DI VARIABILI E FUNZIONI COMPLETE" e incollalo all'inizio del file `main.js`. Ti fornisce tre variabili che memorizzano riferimenti al campo di testo "Inserisci nome personalizzato" (`customName`), al pulsante "Genera storia casuale" (`randomize`), e all'elemento {{htmlelement("p")}} in fondo al corpo HTML in cui verrà copiata la storia (`story`), rispettivamente. Inoltre hai una funzione chiamata `randomValueFromArray()` che prende un array e restituisce uno degli elementi memorizzati all'interno dell'array a caso.
2. Ora guarda la seconda sezione del file di testo grezzo — "2. STRINGHE DI TESTO GREZZO". Questa contiene stringhe di testo che fungeranno da input nel nostro programma. Vorremmo che le contenessi all'interno di variabili dentro `main.js`:

   1. Memorizza la prima stringa di testo, lunga, all'interno di una variabile chiamata `storyText`.
   2. Memorizza il primo set di tre stringhe all'interno di un array chiamato `insertX`.
   3. Memorizza il secondo set di tre stringhe all'interno di un array chiamato `insertY`.
   4. Memorizza il terzo set di tre stringhe all'interno di un array chiamato `insertZ`.

Posizionare il gestore degli eventi e la funzione incompleta:

1. Ora torna al file di testo grezzo.
2. Copia il codice trovato sotto l'intestazione "3. EVENT LISTENER E DEFINIZIONE DI FUNZIONE PARZIALE" e incollalo in fondo al tuo file `main.js`. Questo:

   - Aggiunge un event listener di tipo click alla variabile `randomize` in modo che quando il pulsante che rappresenta viene cliccato, la funzione `result()` viene eseguita.
   - Aggiunge una definizione della funzione `result()` parzialmente completata al tuo codice. Per il resto della sfida, compilerai le linee all'interno di questa funzione per completarla e farla funzionare correttamente.

Completare la funzione `result()`:

1. Crea una nuova variabile chiamata `newStory`, e imposta il suo valore uguale a `storyText`. Questo è necessario in modo da poter creare una nuova storia casuale ogni volta che il pulsante viene premuto e la funzione viene eseguita. Se apportassimo modifiche direttamente a `storyText`, potremmo generare una nuova storia solo una volta.
2. Crea tre nuove variabili chiamate `xItem`, `yItem`, e `zItem`, e rendile uguali al risultato della chiamata `randomValueFromArray()` sui tuoi tre array (il risultato in ciascun caso sarà un elemento casuale fuori ciascun array su cui viene chiamata la funzione). Ad esempio puoi chiamare la funzione e farla restituire una stringa casuale fuori da `insertX` scrivendo `randomValueFromArray(insertX)`.
3. Successivamente vogliamo sostituire i tre segnaposti nella stringa `newStory` — `:insertx:`, `:inserty:`, e `:insertz:` — con le stringhe memorizzate in `xItem`, `yItem`, e `zItem`. Ci sono due possibili metodi di stringa che ti aiuteranno qui — in ciascun caso, fai in modo che la chiamata al metodo sia uguale a `newStory`, così che ogni volta che viene chiamato, `newStory` è uguale a se stesso, ma con le sostituzioni effettuate. Quindi ogni volta che il pulsante viene premuto, questi segnaposti sono sostituiti con una stringa buffa casuale. Come ulteriore suggerimento, a seconda del metodo scelto, potrebbe essere necessario eseguire una delle chiamate due volte.
4. All'interno del primo blocco `if`, aggiungi un'altra chiamata al metodo di sostituzione delle stringhe per sostituire il nome 'Bob' trovato nella stringa `newStory` con la variabile `name`. In questo blocco diciamo "Se un valore è stato inserito nel campo di input del testo `customName`, sostituisci Bob nella storia con quel nome personalizzato."
5. All'interno del secondo blocco `if`, stiamo verificando se il pulsante radio `uk` è stato selezionato. Se sì, vogliamo convertire i valori di peso e temperatura nella storia da libbre e Fahrenheit in pietre e gradi centigradi. Ciò che devi fare è il seguente:

   1. Cerca le formule per convertire le libbre in pietre, e il Fahrenheit in centigradi.
   2. All'interno della linea che definisce la variabile `weight`, sostituisci 300 con un calcolo che converte 300 libbre in pietre. Concatenare `' stone'` alla fine del risultato della chiamata totale a `Math.round()`.
   3. All'interno della linea che definisce la variabile `temperature`, sostituisci 94 con un calcolo che converte 94 Fahrenheit in centigradi. Concatenare `' centigrade'` alla fine del risultato della chiamata totale a `Math.round()`.
   4. Subito sotto le due definizioni di variabili, aggiungi due linee di sostituzione delle stringhe che sostituiscono '94 fahrenheit' con il contenuto della variabile `temperature`, e '300 pounds' con il contenuto della variabile `weight`.

6. Infine, nella penultima linea della funzione, imposta la proprietà `textContent` della variabile `story` (che fa riferimento al paragrafo) uguale a `newStory`.

## Suggerimenti e consigli

- Non è necessario modificare l'HTML in nessun modo, tranne che per applicare il JavaScript al tuo HTML.
- Se non sei sicuro che il JavaScript sia applicato correttamente al tuo HTML, prova a rimuovere temporaneamente tutto il resto dal file JavaScript, aggiungendo un semplice pezzo di JavaScript che sai creerà un effetto evidente, poi salva e aggiorna. Il seguente codice ad esempio rende il background dell'elemento {{htmlelement("html")}} rosso — quindi l'intera finestra del browser dovrebbe diventare rossa se il JavaScript è applicato correttamente:

  ```js
  document.querySelector("html").style.backgroundColor = "red";
  ```

- [`Math.round()`](/it/docs/Web/JavaScript/Reference/Global_Objects/Math/round) è un metodo JavaScript incorporato che arrotonda il risultato di un calcolo al numero intero più vicino.
- Ci sono tre istanze di stringhe che devono essere sostituite. Puoi ripetere il metodo `replace()` più volte, o puoi usare `replaceAll()`. Ricorda, le stringhe sono immutabili!

{{PreviousMenuNext("Learn_web_development/Core/Scripting/Arrays", "Learn_web_development/Core/Scripting/Conditionals", "Learn_web_development/Core/Scripting")}}
