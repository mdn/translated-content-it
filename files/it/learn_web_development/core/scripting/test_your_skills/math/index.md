---
title: "Metti alla prova le tue abilità: Matematica"
short-title: Math
slug: Learn_web_development/Core/Scripting/Test_your_skills/Math
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo dei test presenti in questa pagina è valutare se ha compreso l'articolo [Basi della matematica in JavaScript — numeri e operatori](/it/docs/Learn_web_development/Core/Scripting/Math).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se c'è un errore nel codice, verrà registrato nel pannello dei risultati su questa pagina o nella console JavaScript.
>
> Se incontra difficoltà, può contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Matematica 1

Iniziamo testando la sua conoscenza degli operatori matematici di base.
Creerà quattro valori numerici, ne somma due, sottrae uno dall'altro e poi moltiplica i risultati.
Infine, dobbiamo scrivere una verifica che dimostri che questo valore è un numero pari.

Provi a aggiornare il codice live qui sotto per ricreare l'esempio finale seguendo questi passaggi:

1. Crei quattro variabili che contengono numeri. Chiami le variabili in modo sensato.
2. Sommi le prime due variabili insieme e memorizzi il risultato in un'altra variabile.
3. Sottragga la quarta variabile dalla terza e memorizzi il risultato in un'altra variabile.
4. Moltiplichi i risultati dei passaggi **2** e **3** e memorizzi il risultato in una variabile chiamata `finalResult`.
5. Verifichi se `finalResult` è un numero pari utilizzando uno degli [operatori aritmetici](/it/docs/Learn_web_development/Core/Scripting/Math#arithmetic_operators). Memorizzi il risultato (`0` per pari, `1` per dispari) in una variabile chiamata `evenOddResult`.

Per superare questo test, `finalResult` dovrebbe avere un valore di `48` e `evenOddResult` dovrebbe avere un valore di `0`.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/math/math1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/math/math1-download.html) per lavorare nel suo editor o in un editor online.

## Matematica 2

Nel secondo compito, le vengono forniti due calcoli con i risultati archiviati nelle variabili `result` e `result2`.
È necessario prendere i calcoli, moltiplicarli e formattare il risultato con due cifre decimali.

Provi a aggiornare il codice live qui sotto per ricreare l'esempio finale seguendo questi passaggi:

1. Moltiplichi `result` e `result2` e assegni il risultato nuovamente a `result` (usi la scorciatoia di assegnazione).
2. Formatti `result` in modo che abbia due cifre decimali e la memorizzi in una variabile chiamata `finalResult`.
3. Verifichi il tipo di dati di `finalResult` usando `typeof`. Se è una `string`, la converta in un tipo `number` e memorizzi il risultato in una variabile chiamata `finalNumber`.

Per superare questo test, `finalNumber` dovrebbe avere un risultato di `4633.33`.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/math/math2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/math/math2-download.html) per lavorare nel suo editor o in un editor online.

## Matematica 3

Nell'ultimo compito di questo articolo, desideriamo che scriva alcuni test.
Ci sono tre gruppi, ciascuno composto da un'affermazione e due variabili.
Per ognuno, scriva un test che dimostri o smentisca l'affermazione fatta.
Memorizzi i risultati di questi test in variabili chiamate `weightComparison`, `heightComparison` e `pwdMatch`, rispettivamente.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/math/math3.html", '100%', 550)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/math/math3-download.html) per lavorare nel suo editor o in un editor online.
