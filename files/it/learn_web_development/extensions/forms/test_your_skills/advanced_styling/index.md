---
title: "Metti alla prova le tue abilità: Stile avanzato"
short-title: Stile avanzato
slug: Learn_web_development/Extensions/Forms/Test_your_skills/Advanced_styling
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test di abilità è valutare se hai compreso i nostri articoli su [Stile avanzato dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) e [Pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes).

> [!NOTE]
> Puoi provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
>
> Se incontri difficoltà, puoi contattarci tramite uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Stile avanzato dei moduli 1

Nel nostro primo compito di stile avanzato, vogliamo che tu renda un input di ricerca il più coerente possibile tra i browser — un compito più complesso rispetto agli input di testo standard, anche nei browser moderni.

Ti abbiamo già fornito un reset di base su cui lavorare.

1. Innanzitutto, prova a dare alla casella di ricerca una larghezza, altezza, padding e colore del bordo coerenti tra i browser.
2. Scoprirai che alcuni browser non si comportano bene riguardo all'altezza dell'elemento del modulo. Questo è dovuto al fatto che in alcuni casi viene utilizzato lo stile nativo del sistema operativo. Come puoi eliminare questo stile nativo?
3. Una volta rimosso lo stile nativo, dovrai aggiungere una delle caratteristiche che forniva, per mantenere lo stesso aspetto che avevamo inizialmente. Come fai a farlo?
4. Una cosa che è incoerente tra i browser (particolarmente in Safari) è la posizione del contorno blu standard del focus. Come puoi rimuoverlo?
5. C'è un problema importante nel semplicemente eliminare il contorno blu del focus. Qual è? Puoi aggiungere in qualche modo uno stile che consenta agli utenti di capire quando la casella di ricerca è in hover o in focus?
6. Un'altra cosa che comunemente denota una casella di ricerca è un'icona a forma di lente d'ingrandimento. Ne abbiamo resa disponibile una nella stessa directory dei nostri file HTML — vedi [search-24px.png](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/search-24px.png) — più un elemento `<div>` dopo l'input di ricerca per aiutarti a collegarla, se necessario. Riesci a trovare un modo sensato per collegarla, e puoi usare un po' di CSS per farla posizionare a destra della casella di ricerca, e allinearla verticalmente?

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/html/forms/tasks/advanced-styling/advanced-styling1.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di inizio per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/advanced-styling1-download.html) per lavorare nel tuo editor o in un editor online.

## Stile avanzato dei moduli 2

Nel nostro prossimo compito ti forniamo un insieme di tre pulsanti radio. Vogliamo che tu dia loro uno stile personalizzato.

Ti abbiamo già fornito un reset di base su cui lavorare.

1. Per prima cosa, elimina il loro stile predefinito.
2. Successivamente, dai ai pulsanti radio uno stile di base ragionevole — lo stile che hanno quando la pagina viene caricata per la prima volta. Può essere qualsiasi cosa ti piaccia, ma probabilmente vuoi impostare una larghezza e un'altezza (tra circa 18 e 24 pixel), e un bordo e/o un colore di sfondo sobrio.
3. Ora dai ai pulsanti radio uno stile diverso per quando sono selezionati.
4. Allinea bene i pulsanti radio con le etichette.

Prova ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/html/forms/tasks/advanced-styling/advanced-styling2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarica il punto di inizio per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/advanced-styling2-download.html) per lavorare nel tuo editor o in un editor online.

## Stile avanzato dei moduli 3

Nel nostro compito finale per questa serie di valutazione, ti forniamo un modulo di feedback che è già ben stilizzato — hai già visto qualcosa di simile se hai seguito il nostro articolo sulle [pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes), e ora vogliamo che tu realizzi la tua soluzione.

Quello che vorremmo che tu facessi è utilizzare alcune pseudoclassi avanzate per fornire degli indicatori utili sulla validità.

1. Innanzitutto, vogliamo che tu fornisca uno stile specifico per indicare visivamente quali input devono essere compilati — non possono essere lasciati vuoti.
2. In secondo luogo, vogliamo che tu fornisca un indicatore visivo utile per mostrare se i dati inseriti in ciascun input sono validi o meno.

> [!CALLOUT]
>
> [Scarica il punto di inizio per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/advanced-styling3-download.html) per lavorare nel tuo editor o in un editor online.
