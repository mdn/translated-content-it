---
title: "Metti alla prova le tue abilità: Stringhe"
short-title: Strings
slug: Learn_web_development/Core/Scripting/Test_your_skills/Strings
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se hai compreso i nostri articoli su [Gestione del testo — stringhe in JavaScript](/it/docs/Learn_web_development/Core/Scripting/Strings) e [Metodi utili per le stringhe](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina oppure in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se incontri difficoltà, puoi contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

> [!NOTE]
> Negli esempi seguenti, se c'è un errore nel tuo codice, verrà visualizzato nel pannello dei risultati della pagina per aiutarti a trovare la soluzione (o nella console JavaScript del browser, nel caso della versione scaricabile).

## Stringhe 1

Nel nostro primo compito sulle stringhe, iniziamo in piccolo. Hai già metà di una famosa citazione all'interno di una variabile chiamata `quoteStart`; desideriamo che tu:

1. Cerchi l'altra metà della citazione e la aggiunga all'esempio in una variabile chiamata `quoteEnd`.
2. Concatenare le due stringhe insieme per creare una singola stringa contenente la citazione completa. Salva il risultato in una variabile chiamata `finalQuote`.

Scoprirai che riceverai un errore a questo punto. Puoi risolvere il problema con `quoteStart`, in modo che la citazione completa venga visualizzata correttamente?

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings1-download.html) per lavorare nel tuo editor o in un editor online.

## Stringhe 2

In questo compito ti vengono fornite due variabili, `quote` e `substring`, che contengono due stringhe. Noi vorremmo che tu:

1. Recuperi la lunghezza della citazione e la memorizzi in una variabile chiamata `quoteLength`.
2. Trovi la posizione dell'indice dove `substring` appare in `quote` e memorizzi quel valore in una variabile chiamata `index`.
3. Usa una combinazione delle variabili che hai e delle proprietà/metodi disponibili delle stringhe per ridurre la citazione originale a "I do not like green eggs and ham." e memorizzala in una variabile chiamata `revisedQuote`.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings2-download.html) per lavorare nel tuo editor o in un editor online.

## Stringhe 3

Nel prossimo compito sulle stringhe, ti viene data la stessa citazione con cui hai finito nel compito precedente, ma è un po' rotta! Vogliamo che la corregga e la aggiorni, in questo modo:

1. Cambia il caso per ottenere la corretta capitalizzazione delle frasi (tutto minuscolo, tranne la prima lettera maiuscola). Memorizza la nuova citazione in una variabile chiamata `fixedQuote`.
2. In `fixedQuote`, sostituisci "green eggs and ham" con un altro cibo che proprio non ti piace.
3. C'è un'altra piccola correzione da fare: aggiungi un punto alla fine della citazione, e salva la versione finale in una variabile chiamata `finalQuote`.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings3-download.html) per lavorare nel tuo editor o in un editor online.

## Stringhe 4

Nel compito finale sulle stringhe, ti abbiamo dato il nome di un teorema, due valori numerici e una stringa incompleta (le parti che devono essere aggiunte sono contrassegnate con asterischi (`*`)). Vogliamo che tu modifichi il valore della stringa come segue:

1. Cambiala da una stringa letterale regolare a un template literal.
2. Sostituisci i quattro asterischi con quattro placeholder del template literal. Questi dovrebbero essere:

   1. Il nome del teorema.
   2. I due valori numerici che abbiamo.
   3. La lunghezza dell'ipotenusa di un triangolo rettangolo, dato che le altre due lunghezze dei lati sono le stesse dei due valori che abbiamo. Dovrai cercare come calcolare questo a partire da ciò che hai. Fai il calcolo all'interno del placeholder.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings4-download.html) per lavorare nel tuo editor o in un editor online.
