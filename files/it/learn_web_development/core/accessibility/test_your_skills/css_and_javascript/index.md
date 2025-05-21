---
title: "Metti alla prova le tue abilità: accessibilità CSS e JavaScript"
short-title: CSS e JavaScript
slug: Learn_web_development/Core/Accessibility/Test_your_skills/CSS_and_JavaScript
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test di abilità è valutare se hai compreso il nostro articolo sulle [migliori pratiche di accessibilità per CSS e JavaScript](/it/docs/Learn_web_development/Core/Accessibility/CSS_and_JavaScript).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se ti trovi in difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Accessibilità CSS 1

Nel primo compito, ti viene presentato un elenco di collegamenti. Tuttavia, la loro accessibilità è piuttosto scarsa — non c'è modo di capire davvero che sono collegamenti, o di sapere quale sia quello su cui l'utente è concentrato.

Vogliamo che tu assuma che il set di regole esistente con il selettore `a` sia fornito da un CMS e che non puoi modificarlo — invece, devi creare nuove regole per far sì che i link appaiano e si comportino come link, e che l'utente possa sapere dove si trova nella lista.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/css/css-a11y1.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/css/css-a11y1-download.html) per lavorare nel tuo editor o in un editor online.

## Accessibilità CSS 2

Nel compito successivo ti viene presentato un semplice contenuto — solo intestazioni e paragrafi. Ci sono problemi di accessibilità con i colori e le dimensioni del testo; ci piacerebbe che tu:

1. Spiegassi quali sono i problemi e quali sono le linee guida che indicano i valori accettabili per colore e dimensioni.
2. Selezionassi nuovi valori per colore e dimensione del font che risolvono il problema.
3. Aggiornassi il CSS con questi nuovi valori per risolvere il problema.
4. Testassi il codice per assicurarti che il problema sia ora risolto. Spiega quali strumenti o metodi hai utilizzato per selezionare i nuovi valori e testare il codice.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/accessibility/tasks/html-css/css/css-a11y2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/html-css/css/css-a11y2-download.html) per lavorare nel tuo editor o in un editor online.

## Accessibilità JavaScript 1

Nel nostro ultimo compito qui, devi fare un po' di scripting JavaScript. Abbiamo un'applicazione che presenta un elenco di nomi di animali. Cliccando su uno dei nomi degli animali, appare una descrizione aggiuntiva di quell'animale in un riquadro sotto l'elenco.

Ma non è molto accessibile — nello stato attuale è possibile operarla solo con il mouse. Vorremmo che tu aggiungessi all'HTML e a JavaScript per renderlo accessibile anche tramite tastiera.

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/accessibility/tasks/js/js/js1-download.html) per lavorare nel tuo editor o in un editor online.
