---
title: "Metta alla prova le tue abilità: Accessibilità di CSS e JavaScript"
short-title: CSS e JavaScript
slug: Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo sulle [migliori pratiche di accessibilità di CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se trova difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Accessibilità CSS 1

Nel primo compito viene presentata una lista di link. Tuttavia, la loro accessibilità è piuttosto scarsa — non c'è alcun modo di capire realmente che sono link, o di capire su quale link l'utente sia focalizzato.

Vorremmo che assumesse che il set di regole esistente con il selettore `a` sia fornito da un CMS, e che non possa cambiarlo — invece, deve creare nuove regole per fare in modo che i link appaiano e si comportino come link, e per permettere all'utente di capire dove si trova nella lista.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/css/css-a11y1.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/css/css-a11y1-download.html) per lavorare nel suo editor o in un editor online.

## Accessibilità CSS 2

Nel prossimo compito le viene presentato un semplice contenuto — solo intestazioni e paragrafi. Ci sono problemi di accessibilità con i colori e le dimensioni del testo; vorremmo che lei:

1. Spiegasse quali sono i problemi, e quali sono le linee guida che stabiliscono i valori accettabili per colore e dimensioni.
2. Selezionasse nuovi valori per il colore e la dimensione del carattere che risolvano il problema.
3. Aggiornasse il CSS con questi nuovi valori per risolvere il problema.
4. Testasse il codice per assicurarsi che il problema sia ora risolto. Spieghi quali strumenti o metodi ha utilizzato per selezionare i nuovi valori e testare il codice.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/css/css-a11y2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/css/css-a11y2-download.html) per lavorare nel suo editor o in un editor online.

## Accessibilità JavaScript 1

Nel nostro compito finale qui, ha del JavaScript da gestire. Abbiamo un'applicazione che presenta una lista di nomi di animali. Cliccando su uno dei nomi degli animali appare una descrizione ulteriore di quell'animale in una casella sotto la lista.

Ma non è molto accessibile — nello stato attuale può essere operata solo con il mouse. Vorremmo che aggiungesse all'HTML e al JavaScript per renderlo accessibile anche tramite tastiera.

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/js/js/js1-download.html) per lavorare nel suo editor o in un editor online.
