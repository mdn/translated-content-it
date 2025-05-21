---
title: "Metti alla prova le tue abilità: Funzioni"
short-title: Functions
slug: Learn_web_development/Core/Scripting/Test_your_skills/Functions
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se hai compreso i nostri articoli [Funzioni — blocchi di codice riutilizzabili](/it/docs/Learn_web_development/Core/Scripting/Functions), [Costruisci la tua funzione](/it/docs/Learn_web_development/Core/Scripting/Build_your_own_function) e [Valori di ritorno delle funzioni](/it/docs/Learn_web_development/Core/Scripting/Return_values).

> [!NOTE]
> Puoi provare le soluzioni scaricando il codice e mettendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se c'è un errore, verrà registrato nel pannello dei risultati sulla pagina o nella console JavaScript del browser per aiutarti.
>
> Se rimani bloccato, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: considerata utile

Alcune delle domande qui sotto richiedono di scrivere del codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle — come creare nuovi elementi HTML, impostare i loro contenuti testuali su valori stringa specifici e annidarli all'interno di elementi esistenti sulla pagina — tutto tramite JavaScript.

Non abbiamo ancora spiegato esplicitamente questo argomento nel corso, ma avrai visto alcuni esempi che lo utilizzano, e vorremmo che tu facessi delle ricerche su quali API del DOM sono necessarie per rispondere correttamente alle domande. Un buon punto di partenza è il nostro tutorial [Introduzione al scripting del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Funzioni 1

Per il primo compito, devi creare una semplice funzione — `chooseName()` — che stampa un nome casuale dall'array fornito (`names`) nel paragrafo fornito (`para`), e poi eseguirla una volta.

Prova a aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions1-download.html) per lavorare nel tuo editor o in un editor online.

## Funzioni 2

Per il nostro secondo compito relativo alle funzioni, è necessario creare una funzione che disegna un rettangolo sul `<canvas>` fornito (variabile di riferimento `canvas`, contesto disponibile in `ctx`), basandosi sulle cinque variabili di input fornite:

- `x` — la coordinata x del rettangolo.
- `y` — la coordinata y del rettangolo.
- `width` — la larghezza del rettangolo.
- `height` — l'altezza del rettangolo.
- `color` — il colore del rettangolo.

Dovresti ripulire il canvas prima di disegnarvi, in modo che quando il codice viene aggiornato nella versione live, non ci siano molti rettangoli disegnati uno sopra l'altro.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions2-download.html) per lavorare nel tuo editor o in un editor online.

## Funzioni 3

In questo compito, ritorni al problema posto nel Compito 1, con l'obiettivo di migliorarlo. I tre miglioramenti che vogliamo che tu faccia sono:

1. Rifattorizza il codice che genera il numero casuale in una funzione separata chiamata `random()`, che prende come parametri due limiti generici tra cui il numero casuale dovrebbe essere compreso, e restituisce il risultato.
2. Aggiorna la funzione `chooseName()` in modo che utilizzi la funzione del numero casuale, prenda l'array da cui scegliere come parametro (rendendola più flessibile), e restituisca il risultato.
3. Stampa il risultato restituito nel testo del paragrafo (`para`) utilizzando `textContent`.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions3-download.html) per lavorare nel tuo editor o in un editor online.

## Funzioni 4

In questo compito, abbiamo un array di nomi, e stiamo usando {{jsxref("Array.filter()")}} per ottenere un array di soli nomi più corti di 5 caratteri. Al momento il filtro passa una funzione denominata `isShort()` che controlla la lunghezza del nome, restituendo `true` se il nome è più corto di 5 caratteri, e `false` altrimenti.

Vorremmo che tu cambiassi questo in una funzione freccia. Vedi quanto compattamente puoi farlo.

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/functions/functions4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/functions/functions4-download.html) per lavorare nel tuo editor o in un editor online.
