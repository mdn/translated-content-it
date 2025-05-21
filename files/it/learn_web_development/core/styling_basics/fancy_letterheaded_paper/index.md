---
title: "Sfida: Creare carta intestata elegante"
short-title: "Sfida: Carta intestata elegante"
slug: Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics/Cool-looking_box", "Learn_web_development/Core/Styling_basics")}}

Se vuoi fare una buona impressione, scrivere una lettera su una bella carta intestata può essere un ottimo inizio. In questa sfida creerai un modello online per ottenere tale aspetto.

## Punto di partenza

Per iniziare questa sfida, dovresti:

- Creare copie locali dell'[HTML](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/letterheaded-paper-start/index.html) e del [CSS](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/letterheaded-paper-start/style.css) — salvali come `index.html` e `style.css` in una nuova directory.
- Salvare copie locali delle immagini [superiore](https://raw.githubusercontent.com/mdn/learning-area/master/css/styling-boxes/letterheaded-paper-start/top-image.png), [inferiore](https://raw.githubusercontent.com/mdn/learning-area/master/css/styling-boxes/letterheaded-paper-start/bottom-image.png) e del [logo](https://raw.githubusercontent.com/mdn/learning-area/master/css/styling-boxes/letterheaded-paper-start/logo.png) nella stessa directory dei tuoi file.

In alternativa, potresti usare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
Potresti incollare l'HTML e riempire il CSS in uno di questi editor online.

> [!NOTE]
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Ti sono stati forniti i file necessari per creare un modello di carta intestata. Devi solo mettere insieme i file. Per arrivarci, devi:

### La lettera principale

- Applicare il CSS all'HTML.
- Aggiungere una dichiarazione di sfondo alla lettera che:

  - Fissa l'immagine superiore alla parte superiore della lettera
  - Fissa l'immagine inferiore alla parte inferiore della lettera
  - Aggiunge un gradiente semitrasparente sopra entrambi gli sfondi precedenti che conferisce alla lettera un po' di texture. Rendilo leggermente scuro quasi nella parte superiore e inferiore, ma completamente trasparente per una grande parte del centro.

- Aggiungere un'altra dichiarazione di sfondo che aggiunga solo l'immagine superiore alla parte superiore della lettera, come soluzione alternativa per i browser che non supportano la dichiarazione precedente.
- Aggiungere un colore di sfondo bianco alla lettera.
- Aggiungere un bordo solido superiore e inferiore di 1mm alla lettera, in un colore che si abbina al resto della combinazione di colori.

### Il logo

- Al {{htmlelement("Heading_Elements", "h1")}}, aggiungere il logo come immagine di sfondo.
- Aggiungere un filtro al logo per conferirgli un'ombra leggera.
- Ora commenta il filtro e implementa l'ombra in un modo diverso (leggermente più compatibile tra i browser), che segua comunque la forma dell'immagine rotonda.

## Suggerimenti e consigli

- Ricorda che puoi creare una soluzione alternativa per i browser più vecchi mettendo la versione alternativa di una dichiarazione per prima, seguita dalla versione che funziona solo sui browser più recenti. I browser più vecchi applicheranno la prima dichiarazione e ignoreranno la seconda, mentre i browser più recenti applicheranno la prima e poi la sovrascriveranno con la seconda.
- Sentiti libero di creare le tue grafiche per la sfida, se lo desideri.

## Esempio

Il seguente screenshot mostra un esempio di come potrebbe apparire il design finale:

![Intera pagina A4 con bordo decorativo superiore e inferiore composto da forme arancioni e rosse, e un badge rosso e verde con scritto "Awesome company" sotto il bordo superiore. Sopra il bordo inferiore c'è un indirizzo postale.](letterhead.png)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics/Cool-looking_box", "Learn_web_development/Core/Styling_basics")}}
