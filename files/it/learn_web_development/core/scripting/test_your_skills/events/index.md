---
title: "Metti alla prova le tue abilità: Eventi"
short-title: Events
slug: Learn_web_development/Core/Scripting/Test_your_skills/Events
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se hai compreso il nostro articolo [Introduzione agli eventi](/it/docs/Learn_web_development/Core/Scripting/Events).

> [!NOTE]
> Puoi provare le soluzioni scaricando il codice e inserendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se c'è un errore, verrà registrato nel pannello dei risultati sulla pagina o nella console JavaScript del browser per aiutarti.
>
> Se rimani bloccato, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Manipolazione del DOM: considerato utile

Alcune delle domande seguenti richiedono di scrivere del codice di manipolazione del {{Glossary("DOM", "DOM")}} per completarle, come creare nuovi elementi HTML, impostare il loro contenuto di testo per corrispondere a specifici valori di stringa, e annidarli all'interno di elementi esistenti sulla pagina, tutto tramite JavaScript.

Non abbiamo ancora esplicitamente insegnato questo nel corso, ma avrai visto alcuni esempi che ne fanno uso, e vorremmo che tu facessi un po' di ricerca sugli API del DOM di cui hai bisogno per rispondere con successo alle domande. Un buon punto di partenza è il nostro tutorial [Introduzione al DOM scripting](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting).

## Eventi 1

Nel nostro primo compito relativo agli eventi, devi creare un gestore di eventi che fa sì che il testo all'interno del pulsante (`btn`) cambi quando viene cliccato e torni indietro quando viene cliccato di nuovo.

L'HTML non dovrebbe essere modificato; solo il JavaScript.

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/events/events1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/events/events1-download.html) per lavorare nel tuo editor o in un editor online.

## Eventi 2

Ora esamineremo gli eventi della tastiera. Per superare questa valutazione è necessario costruire un gestore di eventi che muove il cerchio intorno alla canvas fornita quando vengono premuti i tasti WASD sulla tastiera. Il cerchio è disegnato con la funzione `drawCircle()`, che prende i seguenti parametri come input:

- `x` — la coordinata x del cerchio.
- `y` — la coordinata y del cerchio.
- `size` — il raggio del cerchio.

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/events/events2.html", '100%', 650)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/events/events2-download.html) per lavorare nel tuo editor o in un editor online.

## Eventi 3

Nel prossimo compito relativo agli eventi, devi impostare un listener di eventi sull'elemento genitore dei `<button>` (`<div class="button-bar"> … </div>`), che quando viene attivato cliccando su uno qualsiasi dei bottoni, imposterà lo sfondo della `button-bar` al colore contenuto nell'attributo `data-color` del bottone.

Vogliamo che tu risolva questo problema senza fare un ciclo attraverso tutti i bottoni e dare a ciascuno il proprio listener di eventi.

Prova ad aggiornare il codice dal vivo qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/building-blocks/tasks/events/events3.html", '100%', 600)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/building-blocks/tasks/events/events3-download.html) per lavorare nel tuo editor o in un editor online.
