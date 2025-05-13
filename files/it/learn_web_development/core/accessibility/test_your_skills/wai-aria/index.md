---
title: "Metti alla prova le tue competenze: WAI-ARIA"
short-title: WAI-ARIA
slug: Learn_web_development/Core/Accessibility/Test_your_skills/WAI-ARIA
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

Lo scopo di questo test di competenze è valutare se ha compreso il nostro articolo sulle [basi di WAI-ARIA](/it/docs/Learn_web_development/Core/Accessibility/WAI-ARIA_basics).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se dovesse incontrare difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## WAI-ARIA 1

Nel nostro primo compito ARIA, le presentiamo una sezione di markup non semantico, che è ovviamente intesa come lista. Supponendo che non sia in grado di cambiare gli elementi utilizzati, come può consentire agli utenti di lettori di schermo di riconoscere questo come una lista?

Provi ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/aria/aria1.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/aria/aria1-download.html) per lavorare nel suo editor o in un editor online.

## WAI-ARIA 2

Nel nostro secondo compito WAI-ARIA, le presentiamo un semplice modulo di ricerca e le chiediamo di aggiungere un paio di funzionalità WAI-ARIA per migliorarne l'accessibilità:

1. Come può permettere che il modulo di ricerca venga riconosciuto come un landmark separato nella pagina dai lettori di schermo, per renderlo facilmente individuabile?
2. Come può fornire al campo di ricerca un'etichetta appropriata, senza aggiungere esplicitamente un'etichetta di testo visibile al DOM?

Provi ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/aria/aria2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/aria/aria2-download.html) per lavorare nel suo editor o in un editor online.

## WAI-ARIA 3

Per questo compito finale WAI-ARIA, torniamo a un esempio che abbiamo già visto nel [test di competenze su CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript). Come prima, abbiamo un'app che presenta un elenco di nomi di animali. Cliccando su uno dei nomi degli animali, appare una descrizione ulteriore di quell'animale in una casella sotto la lista. Qui, partiamo da una versione accessibile tramite mouse e tastiera.

Il problema attuale è che quando il DOM cambia per mostrare una nuova descrizione, i lettori di schermo non riescono a vedere cosa è cambiato. Può aggiornarlo in modo che le modifiche nella descrizione vengano annunciate dal lettore di schermo?

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/js/aria/aria-js1-download.html) per lavorare nel suo editor o in un editor online.
