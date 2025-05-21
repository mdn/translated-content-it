---
title: "Testa le tue capacità: Matematica"
short-title: Math
slug: Learn_web_development/Core/Scripting/Test_your_skills/Math
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo dei test in questa pagina è verificare se hai compreso l'articolo [Matematica di base in JavaScript — numeri e operatori](/it/docs/Learn_web_development/Core/Scripting/Math).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se c'è un errore nel tuo codice, sarà registrato nel pannello dei risultati su questa pagina o nella console JavaScript.
>
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Math 1

Iniziamo verificando la tua conoscenza degli operatori matematici di base.
Creerai quattro valori numerici, ne aggiungerai due, sottrai uno da un altro, quindi moltiplicherai i risultati.
Infine, dobbiamo scrivere un controllo che provi che questo valore è un numero pari.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito seguendo questi passaggi:

1. Crea quattro variabili che contengano numeri. Dai alle variabili nomi sensati.
2. Somma le prime due variabili e memorizza il risultato in un'altra variabile.
3. Sottrai la quarta variabile dalla terza e memorizza il risultato in un'altra variabile.
4. Moltiplica i risultati dei passaggi **2** e **3** e memorizza il risultato in una variabile chiamata `finalResult`.
5. Verifica se `finalResult` è un numero pari usando uno degli [operatori aritmetici](/it/docs/Learn_web_development/Core/Scripting/Math#arithmetic_operators). Memorizza il risultato (`0` per pari, `1` per dispari) in una variabile chiamata `evenOddResult`.

Per superare questo test, `finalResult` dovrebbe avere un valore di `48` e `evenOddResult` dovrebbe avere un valore di `0`.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/math/math1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/math/math1-download.html) per lavorare nel tuo editor o in un editor online.

## Math 2

Nel secondo compito, ti vengono forniti due calcoli con i risultati memorizzati nelle variabili `result` e `result2`.
È necessario prendere i calcoli, moltiplicarli e formattare il risultato a due cifre decimali.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito seguendo questi passaggi:

1. Moltiplica `result` e `result2` e assegna il risultato nuovamente a `result` (usa la scorciatoia di assegnazione).
2. Format `result` affinché abbia due cifre decimali e memorizzalo in una variabile chiamata `finalResult`.
3. Verifica il tipo di dato di `finalResult` usando `typeof`. Se è una `string`, converti in tipo `number` e memorizza il risultato in una variabile chiamata `finalNumber`.

Per superare questo test, `finalNumber` dovrebbe avere un risultato di `4633.33`.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/math/math2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/math/math2-download.html) per lavorare nel tuo editor o in un editor online.

## Math 3

Nel compito finale di questo articolo, vogliamo che scrivi alcuni test.
Ci sono tre gruppi, ciascuno costituito da un'affermazione e due variabili.
Per ognuno, scrivi un test che provi o smentisca l'affermazione fatta.
Memorizza i risultati di questi test in variabili chiamate `weightComparison`, `heightComparison` e `pwdMatch`, rispettivamente.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/math/math3.html", '100%', 550)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/math/math3-download.html) per lavorare nel tuo editor o in un editor online.
