---
title: "Metti alla prova le tue competenze: Stile avanzato"
short-title: Stile avanzato
slug: Learn_web_development/Extensions/Forms/Test_your_skills/Advanced_styling
l10n:
  sourceCommit: 93f54b6e1fdfef1375233abb265f101bd6866f99
---

L'obiettivo di questo test delle competenze è valutare se ha compreso i nostri articoli su [Stile avanzato dei moduli](/it/docs/Learn_web_development/Extensions/Forms/Advanced_form_styling) e [Pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes).

> [!NOTE]
> Può provare le soluzioni negli editor interattivi su questa pagina o in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
>
> Se trova difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Stile avanzato del modulo 1

Nei nostri primi compiti di stile avanzato, desideriamo che gestisca un campo di input di ricerca per renderlo il più uniforme possibile su diversi browser — un compito più complicato rispetto ai normali input di testo, anche nei browser moderni.

Le abbiamo già fornito un reset di base su cui costruire.

1. Per prima cosa, provi a dare alla casella di ricerca una larghezza, un'altezza, un padding e un colore del bordo uniformi su tutti i browser.
2. Scoprirà che alcuni browser non si comportano coerentemente riguardo l'altezza dell'elemento del modulo. Ciò è dovuto allo stile nativo del sistema operativo utilizzato in alcuni casi. Come può rimuovere questo stile nativo?
3. Una volta rimosso lo stile nativo, dovrà reintegrare una delle caratteristiche che forniva, per mantenere lo stesso aspetto originale. Come può farlo?
4. Una cosa che è inconsistente tra i browser (soprattutto guardando a Safari) è la posizione del contorno blu standard per il focus. Come può rimuoverlo?
5. C'è un grosso problema nel rimuovere semplicemente il contorno blu per il focus. Qual è? Può aggiungere qualche tipo di stile in modo che gli utenti possano capire quando la casella di ricerca è in fase di passaggio del mouse o di focus?
6. Un'altra cosa che comunemente denota una casella di ricerca è un'icona a forma di lente d'ingrandimento. Ne abbiamo resa disponibile una nella stessa directory dei nostri file HTML — veda [search-24px.png](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/search-24px.png) — oltre a un elemento `<div>` dopo l'input di ricerca per aiutarla ad allegarlo, se necessario. Riesce a trovare un modo sensato per allegarlo, e può usare un po' di CSS per farlo posizionare a destra della casella di ricerca e allinearlo verticalmente?

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/html/forms/tasks/advanced-styling/advanced-styling1.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/advanced-styling1-download.html) per lavorare nel suo editor o in un editor online.

## Stile avanzato del modulo 2

Nel nostro prossimo compito le forniamo un set di tre pulsanti radio. Vogliamo che conferisca loro uno stile personalizzato.

Le abbiamo già fornito un reset di base su cui costruire.

1. Innanzitutto, elimini il loro stile predefinito.
2. Successivamente, conferisca ai pulsanti radio uno stile di base ragionevole — lo stile che hanno quando la pagina viene caricata per la prima volta. Può essere qualsiasi cosa le piace, ma probabilmente vorrà impostare una larghezza e un'altezza (tra 18 e 24 pixel), e un bordo e/o un colore di sfondo sottile.
3. Ora conferisca ai pulsanti radio uno stile diverso quando sono selezionati.
4. Allinei bene i pulsanti radio con le etichette.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finale:

{{EmbedGHLiveSample("learning-area/html/forms/tasks/advanced-styling/advanced-styling2.html", '100%', 700)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/advanced-styling2-download.html) per lavorare nel suo editor o in un editor online.

## Stile avanzato del modulo 3

Nel nostro compito finale per questa serie di valutazioni, le forniamo un modulo di feedback che è già ben stilizzato — ha già visto qualcosa di simile se ha lavorato attraverso il nostro articolo sulle [Pseudo-classi UI](/it/docs/Learn_web_development/Extensions/Forms/UI_pseudo-classes), e ora vogliamo che trovi la sua soluzione.

Quello che vorremmo che facesse è utilizzare alcune pseudo-classi avanzate per fornire alcuni indicatori utili di validità.

1. Per prima cosa, vogliamo che fornisca qualche specifico stile per indicare visivamente quali input devono essere compilati — non possono essere lasciati vuoti.
2. In secondo luogo, vogliamo che fornisca un utile indicatore visivo di se i dati inseriti all'interno di ciascun input sono validi o meno.

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/html/forms/tasks/advanced-styling/advanced-styling3-download.html) per lavorare nel suo editor o in un editor online.
