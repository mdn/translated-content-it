---
title: "Metti alla prova le tue abilità: Stringhe"
short-title: Strings
slug: Learn_web_development/Core/Scripting/Test_your_skills/Strings
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se ha compreso i nostri articoli [Gestione del testo — stringhe in JavaScript](/it/docs/Learn_web_development/Core/Scripting/Strings) e [Metodi utili delle stringhe](/it/docs/Learn_web_development/Core/Scripting/Useful_string_methods).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se è bloccato, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

> [!NOTE]
> Negli esempi qui sotto, se c'è un errore nel suo codice, verrà visualizzato nel pannello dei risultati sulla pagina, per aiutarla a trovare la risposta (o nella console JavaScript del browser, nel caso della versione scaricabile).

## Stringhe 1

Nel nostro primo esercizio sulle stringhe, iniziamo con qualcosa di semplice. Ha già metà di una citazione famosa all'interno di una variabile chiamata `quoteStart`; le chiediamo di:

1. Cercare l'altra metà della citazione e aggiungerla all'esempio all'interno di una variabile chiamata `quoteEnd`.
2. Concatenare le due stringhe per fare una singola stringa contenente l'intera citazione. Salvare il risultato in una variabile chiamata `finalQuote`.

Scoprirà che a questo punto riceverà un errore. Riesce a risolvere il problema con `quoteStart`, in modo che l'intera citazione venga visualizzata correttamente?

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings1-download.html) per lavorare nel suo editor o in un editor online.

## Stringhe 2

In questo compito le vengono fornite due variabili, `quote` e `substring`, che contengono due stringhe. Le chiediamo di:

1. Recuperare la lunghezza della citazione e memorizzarla in una variabile chiamata `quoteLength`.
2. Trovare la posizione dell'indice in cui `substring` appare in `quote` e memorizzare quel valore in una variabile chiamata `index`.
3. Utilizzare una combinazione delle variabili che ha e delle proprietà/metodi disponibili delle stringhe per ridurre la citazione originale a "I do not like green eggs and ham.", e memorizzarla in una variabile chiamata `revisedQuote`.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings2-download.html) per lavorare nel suo editor o in un editor online.

## Stringhe 3

Nel prossimo compito sulle stringhe, le viene fornita la stessa citazione con cui ha terminato nel compito precedente, ma è in qualche modo rotta! Vogliamo che la corregga e la aggiorni in questo modo:

1. Cambi la scrittura in modo che sia corretta la capitalizzazione della frase (tutto minuscolo, eccetto la prima lettera maiuscola). Memorizzi la nuova citazione in una variabile chiamata `fixedQuote`.
2. In `fixedQuote`, sostituisca "green eggs and ham" con un altro cibo che davvero non le piace.
3. C'è ancora una piccola correzione da fare — aggiunga un punto alla fine della citazione e salvi la versione finale in una variabile chiamata `finalQuote`.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings3-download.html) per lavorare nel suo editor o in un editor online.

## Stringhe 4

Nell'ultimo compito sulle stringhe, le abbiamo dato il nome di un teorema, due valori numerici e una stringa incompleta (le parti che devono essere aggiunte sono contrassegnate da asterischi (`*`)). Vogliamo che cambi il valore della stringa come segue:

1. Modifichi la stringa da un normale letterale di stringa a un template literal.
2. Sostituisca i quattro asterischi con quattro segnaposto literal di template. Questi dovrebbero essere:

   1. Il nome del teorema.
   2. I due valori numerici che abbiamo.
   3. La lunghezza dell'ipotenusa di un triangolo rettangolo, dato che le due altre lunghezze dei lati sono uguali ai due valori che abbiamo. Dovrà cercare come calcolarlo da ciò che ha. Effettui il calcolo all'interno del segnaposto.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/strings/strings4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/strings/strings4-download.html) per lavorare nel suo editor o in un editor online.
