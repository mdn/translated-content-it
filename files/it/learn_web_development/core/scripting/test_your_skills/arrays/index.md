---
title: "Metta alla prova le tue abilità: Array"
short-title: Arrays
slug: Learn_web_development/Core/Scripting/Test_your_skills/Arrays
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo sugli [Array](/it/docs/Learn_web_development/Core/Scripting/Arrays).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se ha difficoltà, può contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Arrays 1

Iniziamo con alcuni esercizi di base sugli array. In questo compito le chiediamo di creare un array di tre elementi, memorizzato all'interno di una variabile chiamata `myArray`. Gli elementi possono essere qualsiasi cosa voglia — che ne dice dei suoi cibi o band preferiti?

Successivamente, modifichi i primi due elementi dell'array utilizzando la semplice notazione a parentesi quadre e l'assegnazione. Poi aggiunga un nuovo elemento all'inizio dell'array.

Provi ad aggiornare il codice attivo qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays1-download.html) per lavorare nel proprio editor o in un editor online.

## Arrays 2

Passiamo ora a un altro compito. Qui le viene fornita una stringa con cui lavorare. Le chiediamo di:

1. Convertire la stringa in un array, rimuovendo i caratteri `+` durante il processo. Salvi il risultato in una variabile chiamata `myArray`.
2. Memorizzi la lunghezza dell'array in una variabile chiamata `arrayLength`.
3. Salvi l'ultimo elemento dell'array in una variabile chiamata `lastItem`.

Provi ad aggiornare il codice attivo qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays2-download.html) per lavorare nel proprio editor o in un editor online.

## Arrays 3

Per questo compito sugli array, le forniamo un array iniziale e dovrà lavorare in un certo senso in direzione opposta. Deve:

1. Rimuovere l'ultimo elemento dell'array.
2. Aggiungere due nuovi nomi alla fine dell'array.
3. Passare in rassegna ogni elemento dell'array e aggiungere il numero dell'indice dopo il nome tra parentesi, ad esempio `Ryu (0)`. Si noti che non insegniamo come fare questo nell'articolo sugli Array, quindi dovrà fare alcune ricerche.
4. Infine, unire gli elementi dell'array in una singola stringa chiamata `myString`, con un separatore di `"-"`.

Provi ad aggiornare il codice attivo qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays3-download.html) per lavorare nel proprio editor o in un editor online.

## Arrays 4

Per questo compito sugli array, le forniamo un array iniziale che elenca i nomi di alcuni uccelli.

- Trovi l'indice dell'elemento `"Eagles"` e lo utilizzi per rimuovere l'elemento `"Eagles"`.
- Crei un nuovo array da questo, chiamato `eBirds`, che contenga solo gli uccelli dell'array originale i cui nomi iniziano con la lettera "E". Si noti che {{jsxref("String.prototype.startsWith()", "startsWith()")}} è un ottimo modo per verificare se una stringa inizia con un determinato carattere.

Se funziona, dovrebbe vedere apparire `"Emus,Egrets"` nella pagina.

{{EmbedGHLiveSample("learning-area/javascript/introduction-to-js-1/tasks/arrays/arrays4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/introduction-to-js-1/tasks/arrays/arrays4-download.html) per lavorare nel proprio editor o in un editor online.
