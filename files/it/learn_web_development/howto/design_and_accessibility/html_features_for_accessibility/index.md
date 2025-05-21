---
title: Quali funzionalità HTML promuovono l'accessibilità?
slug: Learn_web_development/Howto/Design_and_accessibility/HTML_features_for_accessibility
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Il seguente contenuto descrive le funzionalità specifiche di HTML che dovrebbero essere utilizzate per rendere una pagina web più accessibile alle persone con diverse disabilità.

## Testo dei link

Se hai un link che non è autoesplicativo, o la destinazione del link potrebbe beneficiarne se spiegata in modo più dettagliato, puoi aggiungere informazioni a un link utilizzando gli attributi [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) o [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby).

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

Nota che, nella maggior parte dei casi, è meglio invece scrivere un testo del link utile:

```html
<p>
  I wrote a
  <a href="capable.html">blog post about how good I am at writing link text</a>.
</p>
```

## Link di salto

Per facilitare la tabulazione, puoi fornire un [link di salto](/it/docs/Web/HTML/Reference/Elements/a#skip_links) che consente agli utenti di saltare blocchi della tua pagina web. Potresti voler permettere a qualcuno di saltare una serie di link di navigazione che si trovano su ogni pagina. Questo consente agli utenti della tastiera di passare rapidamente oltre i contenuti ripetuti e andare direttamente al contenuto principale della pagina:

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

## Attributo Alt per le immagini

Ogni immagine dovrebbe avere un attributo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt). Se l'immagine è puramente decorativa e non aggiunge significato al contenuto o al contesto del documento, l'attributo `alt` dovrebbe essere presente, ma vuoto. Puoi opzionalmente anche aggiungere [`role="presentation"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/presentation_role). Tutte le altre immagini dovrebbero includere un attributo `alt` fornendo [testo alternativo che descrive l'immagine](/it/docs/Web/API/HTMLImageElement/alt#usage_notes) in modo utile per gli utenti che possono leggere il resto del contenuto ma non possono vedere l'immagine. Pensa a come descriveresti l'immagine a qualcuno che non può caricarla: queste sono le informazioni che dovresti includere come valore dell'attributo `alt`.

```html
<!-- decorative image -->
<img alt="" src="blueswish.png" role="presentation" />
<img
  alt="The Open Web Docs logo: Carle the book worm smiling"
  src="carle.svg"
  role="img" />
```

L'attributo `alt` per lo stesso contenuto può variare in base al contesto. Nel seguente esempio, viene utilizzata una gif animata invece di una barra di avanzamento per mostrare il progresso del caricamento della pagina in un documento che insegna agli sviluppatori come utilizzare l'elemento HTML [`<progress>`](/it/docs/Web/HTML/Reference/Elements/progress):

```html
<img alt="20% complete" src="load-progress.gif" />
<img
  alt="The progress bar is a thick green square to the left of the thumb and a thin grey line to the right. The thumb is a circle with a diameter the height of the green area."
  src="screenshot-progressbar.png" />
```

## Attributo di ruolo ARIA

Di default, tutti gli elementi semantici in HTML hanno un [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles); per esempio, `<input type="radio">` ha il ruolo `radio`. Gli elementi non semantici in HTML non hanno un ruolo. I ruoli ARIA possono essere utilizzati per descrivere elementi che non esistono nativamente in HTML, come un widget [`tablist`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role). I ruoli sono utili anche per elementi più recenti che esistono ma non hanno ancora un supporto completo dai browser. Per esempio, quando si usano immagini SVG, aggiungi `role="img"` al tag di apertura, poiché c'è un [bug SVG VoiceOver](https://webkit.org/b/216364) per cui VoiceOver non annuncia correttamente le immagini SVG.

```html
<img src="mdn.svg" alt="MDN logo" role="img" />
```
