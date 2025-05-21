---
title: "Metti alla prova le tue abilità: WAI-ARIA"
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

Lo scopo di questa prova di abilità è valutare se hai compreso il nostro articolo sui [fondamenti di WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## WAI-ARIA 1

Nel nostro primo compito ARIA, ti presentiamo una sezione di markup non semantico, che evidentemente dovrebbe essere una lista. Supponendo che non sia possibile cambiare gli elementi utilizzati, come puoi permettere agli utenti di lettori di schermo di riconoscere questo contenuto come una lista?

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/aria/aria1.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/aria/aria1-download.html) per lavorare nel tuo editor o in un editor online.

## WAI-ARIA 2

Nel nostro secondo compito WAI-ARIA, ti presentiamo un semplice modulo di ricerca e vogliamo che tu aggiunga un paio di funzionalità WAI-ARIA per migliorarne l'accessibilità:

1. Come puoi fare in modo che il modulo di ricerca venga definito come un punto di riferimento separato nella pagina dai lettori di schermo, rendendolo facilmente individuabile?
2. Come puoi assegnare un'etichetta appropriata al campo di input della ricerca, senza aggiungere esplicitamente un'etichetta di testo visibile nel DOM?

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/aria/aria2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/aria/aria2-download.html) per lavorare nel tuo editor o in un editor online.

## WAI-ARIA 3

Per questo compito finale WAI-ARIA, torniamo ad un esempio che abbiamo visto in precedenza nel [test delle abilità CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript).
Come in precedenza, abbiamo un'app che presenta un elenco di nomi di animali. Cliccando su uno dei nomi degli animali, appare una descrizione ulteriore di quell'animale in una casella sotto l'elenco. Qui, stiamo iniziando con una versione accessibile sia da mouse che da tastiera.

Il problema che abbiamo ora è che quando il DOM cambia per mostrare una nuova descrizione, i lettori di schermo non possono vedere cosa è cambiato. Puoi aggiornarlo affinché i cambiamenti nella descrizione vengano annunciati dal lettore di schermo?

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/js/aria/aria-js1-download.html) per lavorare nel tuo editor o in un editor online.
