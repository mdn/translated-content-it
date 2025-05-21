---
title: "Metti alla prova le tue abilità: Nozioni di base sugli oggetti"
short-title: Objects
slug: Learn_web_development/Core/Scripting/Test_your_skills/Object_basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se hai compreso il nostro articolo sulle [basi degli oggetti JavaScript](/it/docs/Learn_web_development/Core/Scripting/Object_basics).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se c'è un errore nel tuo codice, verrà registrato nel pannello dei risultati su questa pagina o nella console JavaScript.
>
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Nozioni di base sugli oggetti 1

In questo compito ti viene fornito un oggetto letterale e i tuoi compiti sono:

- Memorizzare il valore della proprietà `name` nella variabile `catName`, utilizzando la notazione delle parentesi quadre.
- Eseguire il metodo `greeting()` usando la notazione a punti (verrà registrato il saluto nella console del browser).
- Aggiornare il valore della proprietà `color` a `black`.

Prova a aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics1-download.html) per lavorare nel tuo editor o in un editor online.

## Nozioni di base sugli oggetti 2

Nel nostro prossimo compito, vogliamo che provi a creare il tuo proprio oggetto letterale per rappresentare una delle tue band preferite. Le proprietà richieste sono:

- `name`: Una stringa che rappresenta il nome della band.
- `nationality`: Una stringa che rappresenta il paese di origine della band.
- `genre`: Il tipo di musica suonato dalla band.
- `members`: Un numero che rappresenta il numero di membri della band.
- `formed`: Un numero che rappresenta l'anno in cui la band si è formata.
- `split`: Un numero che rappresenta l'anno in cui la band si è sciolta, o `false` se sono ancora insieme.
- `albums`: Un array che rappresenta gli album pubblicati dalla band. Ogni elemento dell'array dovrebbe essere un oggetto contenente i seguenti membri:

  - `name`: Una stringa che rappresenta il nome dell'album.
  - `released`: Un numero che rappresenta l'anno in cui l'album è stato pubblicato.

Includi almeno due album nell'array `albums`.

Una volta fatto, dovresti poi scrivere una stringa nella variabile `bandInfo`, che conterrà una piccola biografia con dettagli sul nome, nazionalità, anni di attività e stile, e il titolo e la data di rilascio del loro primo album.

Prova a aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics2-download.html) per lavorare nel tuo editor o in un editor online.

## Nozioni di base sugli oggetti 3

In questo compito, vogliamo che tu ritorni all'oggetto letterale `cat` del Compito 1. Vogliamo che tu riscriva il metodo `greeting()` in modo che registri `"Hello, said Bertie the Cymric."` nella console del browser, ma in un modo che funzioni su _qualsiasi_ oggetto `cat` della stessa struttura, indipendentemente dal suo nome o razza.

Quando hai finito, scrivi il tuo oggetto chiamato `cat2`, che ha la stessa struttura, esattamente lo stesso metodo `greeting()`, ma un diverso `name`, `breed`, e `color`.

Chiama entrambi i metodi `greeting()` per verificare che registrino i saluti appropriati nella console.

Prova a aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics3.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics3-download.html) per lavorare nel tuo editor o in un editor online.

## Nozioni di base sugli oggetti 4

Nel codice che hai scritto per il Compito 3, il metodo `greeting()` è definito due volte, una per ogni gatto. Questo non è ideale (in particolare, viola un principio di programmazione a volte chiamato [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) o "Don't Repeat Yourself").

In questo compito vogliamo che migliori il codice in modo che `greeting()` sia definito solo una volta, e ogni istanza di `cat` ottenga il proprio metodo `greeting()`. Suggerimento: dovresti usare un costruttore JavaScript per creare istanze di `cat`.

Prova a aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/object-basics/object-basics4.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/object-basics/object-basics4-download.html) per lavorare nel tuo editor o in un editor online.
