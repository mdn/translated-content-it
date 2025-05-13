---
title: "Metti alla prova le tue abilità: Funzioni"
short-title: Functions
slug: Learn_web_development/Core/Scripting/Test_your_skills/Functions
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se ha compreso i nostri articoli su [Funzioni — blocchi di codice riutilizzabili](/it/docs/Learn_web_development/Core/Scripting/Functions), [Costruisca la sua funzione](/it/docs/Learn_web_development/Core/Scripting/Build_your_own_function), e [Valori di ritorno delle funzioni](/it/docs/Learn_web_development/Core/Scripting/Return_values).

> [!NOTE]
> Può provare le soluzioni scaricando il codice e inserendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/). Se c'è un errore, verrà registrato nel pannello dei risultati sulla pagina o nella console JavaScript del browser per aiutarla.
>
> Se si blocca, può contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: considerata utile

Alcune delle domande seguenti richiedono di scrivere del codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle — come creare nuovi elementi HTML, impostare il loro contenuto testuale per eguagliare valori stringa specifici e nidificarli all'interno di elementi esistenti sulla pagina — tutto tramite JavaScript.

Non abbiamo ancora insegnato esplicitamente questo nel corso, ma avrà visto alcuni esempi che lo utilizzano, e vorremmo che facesse alcune ricerche sui DOM API necessari per rispondere correttamente alle domande. Un buon punto di partenza è il nostro tutorial [Introduzione allo scripting DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Funzioni 1

Per il primo compito, deve creare una funzione semplice — `chooseName()` — che stampa un nome casuale dall'array fornito (`names`) al paragrafo fornito (`para`), e poi eseguirla una volta.

Provi ad aggiornare il codice live sottostante per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions1-download.html) per lavorare nel suo editor o in un editor online.

## Funzioni 2

Per il nostro secondo compito relativo alle funzioni, deve creare una funzione che disegna un rettangolo sul `<canvas>` fornito (variabile di riferimento `canvas`, contesto disponibile in `ctx`), basato sulle cinque variabili di input fornite:

- `x` — la coordinata x del rettangolo.
- `y` — la coordinata y del rettangolo.
- `width` — la larghezza del rettangolo.
- `height` — l'altezza del rettangolo.
- `color` — il colore del rettangolo.

Vorrà cancellare il canvas prima di disegnare, in modo che quando il codice venga aggiornato nel caso della versione live, non vengano disegnati molti rettangoli uno sopra l'altro.

Provi ad aggiornare il codice live sottostante per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions2-download.html) per lavorare nel suo editor o in un editor online.

## Funzioni 3

In questo compito, torna al problema posto nel Compito 1, con l'obiettivo di migliorarlo. I tre miglioramenti che vogliamo che faccia sono:

1. Rifattorizzi il codice che genera il numero casuale in una funzione separata chiamata `random()`, che prende come parametri due limiti generici tra cui dovrebbe essere compreso il numero casuale, e restituisce il risultato.
2. Aggiorni la funzione `chooseName()` in modo che faccia uso della funzione del numero casuale, prenda l'array da cui scegliere come parametro (rendendola più flessibile), e restituisca il risultato.
3. Stampi il risultato restituito nel `textContent` del paragrafo (`para`).

Provi ad aggiornare il codice live sottostante per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions3-download.html) per lavorare nel suo editor o in un editor online.

## Funzioni 4

In questo compito, abbiamo un array di nomi, e stiamo usando {{jsxref("Array.filter()")}} per ottenere un array di soli nomi più brevi di 5 caratteri. Attualmente il filtro sta passando una funzione chiamata `isShort()` che controlla la lunghezza del nome, restituendo `true` se il nome è più corto di 5 caratteri, e `false` altrimenti.

Vorremmo che trasformasse questa in una funzione freccia. Veda quanto compatta può renderla.

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions4-download.html) per lavorare nel suo editor o in un editor online.
