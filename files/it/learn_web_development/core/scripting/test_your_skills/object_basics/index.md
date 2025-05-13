---
title: "Metti alla prova le tue abilità: Basi degli oggetti"
short-title: Objects
slug: Learn_web_development/Core/Scripting/Test_your_skills/Object_basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo sulle [basi degli oggetti in JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se c'è un errore nel suo codice, verrà registrato nel pannello dei risultati su questa pagina o nella console JavaScript.
>
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Basi degli oggetti 1

In questo compito le viene fornito un oggetto letterale e i suoi compiti sono:

- Memorizzare il valore della proprietà `name` all'interno della variabile `catName`, usando la notazione delle parentesi quadre.
- Eseguire il metodo `greeting()` utilizzando la notazione a puntini (verrà registrato il saluto nella console del browser).
- Aggiornare il valore della proprietà `color` a `black`.

Provi ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio completo:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scaricare il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics1-download.html) per lavorare nel suo editor o in un editor online.

## Basi degli oggetti 2

Nel nostro prossimo compito, vogliamo che provi a creare il suo proprio oggetto letterale per rappresentare una delle sue band preferite. Le proprietà richieste sono:

- `name`: Una stringa che rappresenta il nome della band.
- `nationality`: Una stringa che rappresenta il paese di origine della band.
- `genre`: Che tipo di musica suona il gruppo.
- `members`: Un numero che rappresenta il numero di membri che compongono la band.
- `formed`: Un numero che rappresenta l'anno in cui la band si è formata.
- `split`: Un numero che rappresenta l'anno in cui la band si è sciolta, o `false` se sono ancora insieme.
- `albums`: Un array che rappresenta gli album pubblicati dalla band. Ogni elemento dell'array dovrebbe essere un oggetto contenente i seguenti membri:

  - `name`: Una stringa che rappresenta il nome dell'album.
  - `released`: Un numero che rappresenta l'anno in cui l'album è stato pubblicato.

Includa almeno due album nell'array `albums`.

Una volta fatto, dovrebbe scrivere una stringa nella variabile `bandInfo`, che conterrà una breve biografia dettagliando il loro nome, nazionalità, anni di attività e stile, e il titolo e la data di pubblicazione del loro primo album.

Provi ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio completo:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scaricare il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics2-download.html) per lavorare nel suo editor o in un editor online.

## Basi degli oggetti 3

In questo compito, vogliamo che torni all'oggetto letterale `cat` del Compito 1. Vogliamo che lei riscriva il metodo `greeting()` in modo che registri `"Hello, said Bertie the Cymric."` nella console del browser, ma in un modo che funzionerà su _qualsiasi_ oggetto gatto della stessa struttura, indipendentemente dal suo nome o dalla razza.

Quando ha finito, scriva il suo proprio oggetto chiamato `cat2`, che ha la stessa struttura, esattamente lo stesso metodo `greeting()`, ma un diverso `name`, `breed` e `color`.

Chiami entrambi i metodi `greeting()` per verificare che registrino saluti appropriati nella console.

Provi ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio completo:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scaricare il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics3-download.html) per lavorare nel suo editor o in un editor online.

## Basi degli oggetti 4

Nel codice che ha scritto per il Compito 3, il metodo `greeting()` è definito due volte, una per ogni gatto. Questo non è ideale (specificamente, viola un principio di programmazione talvolta chiamato [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) o "Don't Repeat Yourself").

In questo compito vogliamo che migliori il codice in modo che `greeting()` sia definito solo una volta, e ogni istanza di `cat` ottenga il proprio metodo `greeting()`. Suggerimento: dovrebbe utilizzare un costruttore JavaScript per creare le istanze di `cat`.

Provi ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio completo:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scaricare il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics4-download.html) per lavorare nel suo editor o in un editor online.
