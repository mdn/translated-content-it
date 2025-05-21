---
title: "Metti alla prova le tue abilità: Array"
short-title: Arrays
slug: Learn_web_development/Core/Scripting/Test_your_skills/Arrays
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se hai compreso il nostro articolo sugli [Array](/it/docs/Learn_web_development/Core/Scripting/Arrays).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se incontri difficoltà, puoi contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Arrays 1

Iniziamo con un po' di pratica di base con gli array. In questo compito ti chiediamo di creare un array di tre elementi, memorizzato all'interno di una variabile chiamata `myArray`. Gli elementi possono essere qualsiasi cosa tu voglia, ad esempio i tuoi cibi o band preferiti.

Successivamente, modifica i primi due elementi dell'array usando la notazione con parentesi e assegnazione. Quindi aggiungi un nuovo elemento all'inizio dell'array.

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays1-download.html) per lavorare nel tuo proprio editor o in un editor online.

## Arrays 2

Passiamo ora a un altro compito. Qui ti viene fornita una stringa su cui lavorare. Ti chiediamo di:

1. Convertire la stringa in un array, rimuovendo i caratteri `+` nel processo. Salva il risultato in una variabile chiamata `myArray`.
2. Memorizzare la lunghezza dell'array in una variabile chiamata `arrayLength`.
3. Memorizzare l'ultimo elemento dell'array in una variabile chiamata `lastItem`.

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays2-download.html) per lavorare nel tuo proprio editor o in un editor online.

## Arrays 3

Per questo compito sugli array, ti forniamo un array di partenza e lavorerai in una direzione abbastanza opposta. Devi:

1. Rimuovere l'ultimo elemento dell'array.
2. Aggiungere due nuovi nomi alla fine dell'array.
3. Scorrere ogni elemento dell'array e aggiungere il suo numero di indice dopo il nome tra parentesi, ad esempio `Ryu (0)`. Nota che non spieghiamo come fare questo nell'articolo sugli Array, quindi dovrai fare qualche ricerca.
4. Infine, unire gli elementi dell'array insieme in una stringa chiamata `myString`, con un separatore `"-"`.

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays3-download.html) per lavorare nel tuo proprio editor o in un editor online.

## Arrays 4

Per questo compito sugli array, ti forniamo un array di partenza con i nomi di alcuni uccelli.

- Trova l'indice dell'elemento `"Eagles"` e usalo per rimuovere l'elemento `"Eagles"`.
- Crea un nuovo array a partire da questo, chiamato `eBirds`, che contenga solo gli uccelli dell'array originale i cui nomi iniziano con la lettera "E". Nota che {{jsxref("String.prototype.startsWith()", "startsWith()")}} è un ottimo modo per verificare se una stringa inizia con uno specifico carattere.

Se funziona, dovresti vedere `"Emus,Egrets"` apparire nella pagina.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays4-download.html) per lavorare nel tuo proprio editor o in un editor online.
