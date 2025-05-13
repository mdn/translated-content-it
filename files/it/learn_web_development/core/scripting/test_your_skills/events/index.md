---
title: "Metti alla prova le sue capacità: Eventi"
short-title: Events
slug: Learn_web_development/Core/Scripting/Test_your_skills/Events
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di competenza è valutare se ha compreso il nostro articolo [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events).

> [!NOTE]
> Può provare le soluzioni scaricando il codice e inserendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se c'è un errore, verrà registrato nel pannello dei risultati sulla pagina o nella console JavaScript del browser per aiutarla.
>
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: considerata utile

Alcune delle domande sottostanti richiedono di scrivere del codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle, come creare nuovi elementi HTML, impostare il loro contenuto di testo per corrispondere a specifici valori di stringa, e annidarli all'interno di elementi esistenti sulla pagina, tutto tramite JavaScript.

Non abbiamo ancora insegnato esplicitamente ciò nel corso, ma avrà visto alcuni esempi che lo utilizzano, e ci piacerebbe che faccia ricerca sui DOM API necessari per rispondere con successo alle domande. Un buon punto di partenza è il nostro tutorial [Introduzione alla programmazione del DOM](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Eventi 1

Nel nostro primo compito correlato agli eventi, deve creare un gestore eventi che faccia cambiare il testo all'interno del bottone (`btn`) quando viene cliccato, e cambiare di nuovo quando viene ricliccato.

Non si deve modificare l'HTML; solo il JavaScript.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/events/events1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/events/events1-download.html) per lavorare nel proprio editor o in un editor online.

## Eventi 2

Ora esamineremo gli eventi della tastiera. Per superare questa valutazione deve costruire un gestore eventi che sposta il cerchio attorno al canvas fornito quando i tasti WASD vengono premuti sulla tastiera. Il cerchio è disegnato con la funzione `drawCircle()`, che prende i seguenti parametri come input:

- `x` — la coordinata x del cerchio.
- `y` — la coordinata y del cerchio.
- `size` — il raggio del cerchio.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/events/events2.html", '100%', 650)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/events/events2-download.html) per lavorare nel proprio editor o in un editor online.

## Eventi 3

Nel prossimo compito correlato agli eventi, deve impostare un ascoltatore di eventi sull'elemento padre dei `<button>` (`<div class="button-bar"> … </div>`), che quando invocato cliccando su qualsiasi dei bottoni imposterà lo sfondo della `button-bar` al colore contenuto nell'attributo `data-color` del bottone.

Vogliamo che risolva questo problema senza il bisogno di eseguire un ciclo attraverso tutti i bottoni e dare a ciascuno il proprio ascoltatore di eventi.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/events/events3.html", '100%', 600)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/events/events3-download.html) per lavorare nel proprio editor o in un editor online.
