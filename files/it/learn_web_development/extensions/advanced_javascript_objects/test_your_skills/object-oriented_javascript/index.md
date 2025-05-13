---
title: "Metta alla prova le sue abilità: JavaScript orientato agli oggetti"
short-title: JavaScript orientato agli oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo [Classi in JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se c'è un errore nel suo codice, verrà registrato nel pannello dei risultati su questa pagina o nella console JavaScript.
>
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## OOJS 1

In questo compito le forniamo l'inizio di una definizione per una classe `Shape`. Essa ha tre proprietà: `name`, `sides` e `sideLength`. Questa classe modella solo forme per le quali tutti i lati hanno la stessa lunghezza, come un quadrato o un triangolo equilatero.

Vorremmo che lei:

- Aggiungesse un costruttore a questa classe. Il costruttore prende argomenti per le proprietà `name`, `sides` e `sideLength`, e le inizializza.
- Aggiungesse un nuovo metodo `calcPerimeter()` alla classe, che calcoli il suo perimetro (la lunghezza del contorno della forma) e registri il risultato nella console.
- Creasse una nuova istanza della classe `Shape` chiamata `square`. Le dia un `name` di `square`, `4` `sides` e un `sideLength` di `5`.
- Chiamasse il suo metodo `calcPerimeter()` sull'istanza, per vedere se registra il risultato del calcolo nella console del browser come previsto.
- Creasse una nuova istanza di `Shape` chiamata `triangle`, con `name` di `triangle`, `3` `sides` e un `sideLength` di `3`.
- Chiamasse `triangle.calcPerimeter()` per verificare che funzioni correttamente.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio completo:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/oojs/oojs1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/oojs/oojs1-download.html) per lavorare nel suo editor o in un editor online.

## OOJS 2

Successivamente, vorremmo che creasse una classe `Square` che erediti da `Shape`, e aggiungesse un metodo `calcArea()` che calcola l'area del quadrato. Imposti anche il costruttore in modo che la proprietà `name` delle istanze degli oggetti `Square` sia automaticamente impostata su `square`, e la proprietà `sides` sia automaticamente impostata su `4`. Quindi, quando si invoca il costruttore, dovrebbe solo fornire la proprietà `sideLength`.

Crei un'istanza della classe `Square` chiamata `square` con valori di proprietà appropriati, e chiami i suoi metodi `calcPerimeter()` e `calcArea()` per dimostrare che funziona correttamente.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio completo:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/oojs/oojs2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/oojs/oojs2-download.html) per lavorare nel suo editor o in un editor online.
