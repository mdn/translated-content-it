---
title: Quali funzionalità HTML promuovono l'accessibilità?
slug: Learn_web_development/Howto/Design_and_accessibility/HTML_features_for_accessibility
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Il seguente contenuto descrive caratteristiche specifiche di HTML che dovrebbero essere utilizzate per rendere una pagina web più accessibile a persone con diverse disabilità.

## Testo del link

Se ha un link che non è autoesplicativo, o la destinazione del link potrebbe beneficiare di una spiegazione più dettagliata, può aggiungere informazioni a un link utilizzando gli attributi [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) o [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby).

```html
<p>
  I'm really bad at writing link text.
  <a
    href="inept.html"
    aria-label="Why I'm rubbish at writing link text: An explanation and an apology."
    >Click here</a
  >
  to find out more.
</p>
<p>
  I'm really <span id="incompetence">bad at writing link text</span>.
  <a href="inept.html" aria-labelledby="incompetence">Click here</a> to find out
  more.
</p>
```

Si noti che, nella maggior parte dei casi, è meglio invece scrivere un testo del link utile:

```html
<p>
  I wrote a
  <a href="capable.html">blog post about how good I am at writing link text</a>.
</p>
```

## Link di salto

Per aiutare la navigazione tramite tasti tab, può fornire un [link di salto](/it/docs/Web/HTML/Reference/Elements/a#skip_links) che consente agli utenti di saltare porzioni della sua pagina web. Potrebbe voler consentire a qualcuno di saltare una serie di link di navigazione presenti in ogni pagina. Questo permette agli utenti che utilizzano la tastiera di scorrere rapidamente il contenuto ripetuto e andare direttamente al contenuto principale della pagina:

```html
<header>
  <h1>The Heading</h1>
  <a href="#content">Skip to content</a>
</header>

<nav>
  <!-- navigation stuff -->
</nav>

<section id="content">
  <!--your content -->
</section>
```

## Attributo alt per immagini

Ogni immagine dovrebbe avere un attributo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt). Se l'immagine è puramente decorativa e non aggiunge significato al contenuto o al contesto del documento, l'attributo `alt` dovrebbe essere presente, ma vuoto. Può anche, opzionalmente, aggiungere [`role="presentation"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/presentation_role). Tutte le altre immagini dovrebbero includere un attributo `alt` che fornisce [testo alternativo che descrive l'immagine](/it/docs/Web/API/HTMLImageElement/alt#usage_notes) in un modo utile per gli utenti che possono leggere il resto del contenuto ma non possono vedere l'immagine. Pensi a come descriverebbe l'immagine a qualcuno che non può caricare la sua immagine: queste sono le informazioni che dovrebbe includere come valore dell'attributo `alt`.

```html
<!-- decorative image -->
<img alt="" src="blueswish.png" role="presentation" />
<img
  alt="The Open Web Docs logo: Carle the book worm smiling"
  src="carle.svg"
  role="img" />
```

L'attributo `alt` per lo stesso contenuto potrebbe variare a seconda del contesto. Nel seguente esempio, viene utilizzata una gif animata invece di una barra di progressione per mostrare l'avanzamento del caricamento della pagina per un documento che insegna ai sviluppatori come utilizzare l'elemento HTML [`<progress>`](/it/docs/Web/HTML/Reference/Elements/progress):

```html
<img alt="20% complete" src="load-progress.gif" />
<img
  alt="The progress bar is a thick green square to the left of the thumb and a thin grey line to the right. The thumb is a circle with a diameter the height of the green area."
  src="screenshot-progressbar.png" />
```

## Attributo ARIA role

Di default, tutti gli elementi semantici in HTML hanno un [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles); per esempio, `<input type="radio">` ha il ruolo `radio`. Gli elementi non semantici in HTML non hanno un ruolo. I ruoli ARIA possono essere utilizzati per descrivere elementi che non esistono nativamente in HTML, come un widget [`tablist`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role). I ruoli sono anche utili per elementi più recenti che esistono ma non hanno ancora il pieno supporto del browser. Ad esempio, quando si utilizzano immagini SVG, aggiunga `role="img"` al tag di apertura, poiché c'è un [bug di SVG VoiceOver](https://webkit.org/b/216364) per cui VoiceOver non annuncia correttamente le immagini SVG.

```html
<img src="mdn.svg" alt="MDN logo" role="img" />
```
