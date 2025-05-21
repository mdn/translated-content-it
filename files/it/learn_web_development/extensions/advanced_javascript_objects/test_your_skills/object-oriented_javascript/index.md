---
title: "Test delle tue competenze: JavaScript orientato agli oggetti"
short-title: JavaScript orientato agli oggetti
slug: Learn_web_development/Extensions/Advanced_JavaScript_objects/Test_your_skills/Object-oriented_JavaScript
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

Lo scopo di questo test di abilità è valutare se hai compreso il nostro articolo [Classi in JavaScript](/it/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Classes_in_JavaScript).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se c'è un errore nel tuo codice, verrà registrato nel pannello dei risultati su questa pagina o nella console JavaScript.
>
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## OOJS 1

In questo compito ti forniamo l'inizio di una definizione per una classe `Shape`. Ha tre proprietà: `name`, `sides`, e `sideLength`. Questa classe modella solo forme per cui tutti i lati hanno la stessa lunghezza, come un quadrato o un triangolo equilatero.

Vorremmo che tu:

- Aggiunga un costruttore a questa classe. Il costruttore prende argomenti per le proprietà `name`, `sides` e `sideLength`, e le inizializza.
- Aggiunga un nuovo metodo `calcPerimeter()` alla classe, che calcola il suo perimetro (la lunghezza del bordo esterno della forma) e registra il risultato nella console.
- Crei una nuova istanza della classe `Shape` chiamata `square`. Dalle un `name` di `square`, `4` `sides`, e un `sideLength` di `5`.
- Chiami il tuo metodo `calcPerimeter()` sull'istanza, per vedere se registra il risultato del calcolo nella console del browser come previsto.
- Crei una nuova istanza di `Shape` chiamata `triangle`, con un `name` di `triangle`, `3` `sides` e un `sideLength` di `3`.
- Chiami `triangle.calcPerimeter()` per controllare che funzioni correttamente.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/oojs/oojs1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/oojs/oojs1-download.html) per lavorare nel tuo editor o in un editor online.

## OOJS 2

Successivamente, vorremmo che tu crei una classe `Square` che eredita da `Shape`, e aggiunga un metodo `calcArea()` che calcola l'area del quadrato. Imposta anche il costruttore in modo che la proprietà `name` delle istanze dell'oggetto `Square` sia automaticamente impostata su `square`, e la proprietà `sides` sia automaticamente impostata su `4`. Quando invochi il costruttore, dovresti quindi solo fornire la proprietà `sideLength`.

Crea un'istanza della classe `Square` chiamata `square` con le proprietà appropriate, e chiama i suoi metodi `calcPerimeter()` e `calcArea()` per mostrare che funziona correttamente.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/oojs/oojs2.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/oojs/oojs2-download.html) per lavorare nel tuo editor o in un editor online.
