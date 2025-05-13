---
title: "Sfida: Creare carta intestata elegante"
short-title: "Sfida: Carta intestata elegante"
slug: Learn_web_development/Core/Styling_basics/Fancy_letterheaded_paper
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics/Cool-looking_box", "Learn_web_development/Core/Styling_basics")}}

Se desidera fare una buona impressione, scrivere una lettera su carta intestata elegante può essere un ottimo inizio. In questa sfida creerà un modello online per ottenere tale aspetto.

## Punto di partenza

Per iniziare questa sfida, dovrebbe:

- Creare copie locali dell'[HTML](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/letterheaded-paper-start/index.html) e del [CSS](https://github.com/mdn/learning-area/blob/main/css/styling-boxes/letterheaded-paper-start/style.css) — salvarli come `index.html` e `style.css` in una nuova directory.
- Salvare copie locali delle immagini [superiore](https://raw.githubusercontent.com/mdn/learning-area/master/css/styling-boxes/letterheaded-paper-start/top-image.png), [inferiore](https://raw.githubusercontent.com/mdn/learning-area/master/css/styling-boxes/letterheaded-paper-start/bottom-image.png) e del [logo](https://raw.githubusercontent.com/mdn/learning-area/master/css/styling-boxes/letterheaded-paper-start/logo.png) nella stessa directory dei suoi file di codice.

In alternativa, potrebbe utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
Potrebbe incollare l'HTML e compilare il CSS in uno di questi editor online.

> [!NOTE]
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Le sono stati forniti i file necessari per creare un modello di carta intestata. Deve soltanto assemblare i file. Per farlo, deve:

### La lettera principale

- Applicare il CSS all'HTML.
- Aggiungere una dichiarazione di sfondo alla lettera che:

  - Fissi l'immagine superiore in cima alla lettera
  - Fissi l'immagine inferiore in fondo alla lettera
  - Aggiunga un gradiente semitrasparente sopra entrambi gli sfondi precedenti che conferisca alla lettera un po' di texture. Deve essere leggermente scuro vicino alla parte superiore e inferiore, ma completamente trasparente per una larga parte del centro.

- Aggiungere un'altra dichiarazione di sfondo che aggiunga solo l'immagine superiore in cima alla lettera, come opzione di riserva per i browser che non supportano la precedente dichiarazione.
- Aggiungere un colore di sfondo bianco alla lettera.
- Aggiungere un bordo solido superiore e inferiore di 1mm alla lettera, in un colore coerente con il resto dello schema cromatico.

### Il logo

- All'{{htmlelement("Heading_Elements", "h1")}}, aggiungere il logo come immagine di sfondo.
- Aggiungere un filtro al logo per dare un'ombra sottile.
- Ora commentare il filtro e implementare l'ombra in un modo diverso (leggermente più compatibile tra i browser), che segua ancora la forma dell'immagine circolare.

## Suggerimenti e consigli

- Ricordi che può creare un fallback per i browser più vecchi mettendo la versione di riserva di una dichiarazione per prima, seguita dalla versione che funziona solo nei browser più recenti. I browser più vecchi applicheranno la prima dichiarazione e ignoreranno la seconda, mentre i browser più recenti applicheranno la prima, quindi la sostituiranno con la seconda.
- Sentiti libero di creare i suoi grafici per la sfida, se lo desidera.

## Esempio

Lo screenshot seguente mostra un esempio di come potrebbe apparire il design finito:

![Pagina A4 completa con bordi decorativi superiore e inferiore composti da forme arancioni e rosse, e un distintivo rosso e verde con scritto Awesome company, sotto il bordo superiore. Sopra il bordo inferiore c'è un indirizzo postale.](letterhead.png)

{{PreviousMenuNext("Learn_web_development/Core/Styling_basics/Fundamental_CSS_comprehension", "Learn_web_development/Core/Styling_basics/Cool-looking_box", "Learn_web_development/Core/Styling_basics")}}
