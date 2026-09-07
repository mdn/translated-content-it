---
title: Quali funzionalità HTML favoriscono l'accessibilità?
slug: Learn_web_development/Howto/Design_and_accessibility/HTML_features_for_accessibility
l10n:
  sourceCommit: 1f00512e3c9a20b5bb927db529bb5d639e346d96
---

Il contenuto seguente descrive funzionalità specifiche di HTML che dovrebbero essere usate per rendere una pagina web più accessibile alle persone con diverse disabilità.

## Testo del link

Se è presente un link che non è autoesplicativo, oppure se la destinazione del link potrebbe trarre vantaggio da una spiegazione più dettagliata, è possibile aggiungere informazioni a un link usando gli attributi [`aria-label`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-label) o [`aria-labelledby`](/it/docs/Web/Accessibility/ARIA/Reference/Attributes/aria-labelledby).

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

Si noti che, nella maggior parte dei casi, è preferibile scrivere invece un testo del link utile:

```html
<p>
  I wrote a
  <a href="capable.html">blog post about how good I am at writing link text</a>.
</p>
```

## Link per saltare contenuti

Per facilitare la navigazione tramite Tab, è possibile fornire un [link per saltare contenuti](/it/docs/Web/HTML/Reference/Elements/a#skip_links) che consenta agli utenti di saltare sezioni della pagina web. Potrebbe essere utile consentire di saltare una moltitudine di link di navigazione presenti in ogni pagina. Questo permette agli utenti della tastiera di superare rapidamente i contenuti ripetuti tramite Tab e di raggiungere direttamente il contenuto principale della pagina:

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

## Attributo alt per le immagini

Ogni immagine dovrebbe avere un attributo [`alt`](/it/docs/Web/HTML/Reference/Elements/img#alt). Se l'immagine è puramente decorativa e non aggiunge alcun significato al contenuto o al contesto del documento, l'attributo `alt` dovrebbe essere presente, ma vuoto. Facoltativamente, è possibile aggiungere anche [`role="presentation"`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/presentation_role). Tutte le altre immagini dovrebbero includere un attributo `alt` che fornisca un [testo alternativo che descriva l'immagine](/it/docs/Web/HTML/Reference/Elements/img#accessibility) in modo utile per gli utenti che possono leggere il resto del contenuto ma non possono vedere l'immagine. Occorre pensare a come descrivere l'immagine a qualcuno che non può caricarla: questa è l'informazione da includere come valore dell'attributo `alt`.

```html
<!-- decorative image -->
<img alt="" src="blueswish.png" role="presentation" />
<img
  alt="The Open Web Docs logo: Carle the book worm smiling"
  src="carle.svg"
  role="img" />
```

L'attributo `alt` per lo stesso contenuto può variare in base al contesto. Nell'esempio seguente, viene usata una GIF animata invece di una barra di avanzamento per mostrare l'avanzamento del caricamento della pagina di un documento che insegna agli sviluppatori come usare l'elemento HTML [`<progress>`](/it/docs/Web/HTML/Reference/Elements/progress):

```html
<img alt="20% complete" src="load-progress.gif" />
<img
  alt="The progress bar is a thick green square to the left of the thumb and a thin grey line to the right. The thumb is a circle with a diameter the height of the green area."
  src="screenshot-progressbar.png" />
```

## Attributo ARIA role

Per impostazione predefinita, tutti gli elementi semantici in HTML hanno un [`role`](/it/docs/Web/Accessibility/ARIA/Reference/Roles); ad esempio, `<input type="radio">` ha il ruolo `radio`. Gli elementi non semantici in HTML non hanno un ruolo. I ruoli ARIA possono essere usati per descrivere elementi che non esistono nativamente in HTML, come un widget [`tablist`](/it/docs/Web/Accessibility/ARIA/Reference/Roles/tablist_role). I ruoli sono utili anche per elementi più recenti che esistono ma non hanno ancora un supporto completo nei browser. Ad esempio, quando si usano immagini SVG, aggiungere `role="img"` al tag di apertura, poiché esiste un [bug di SVG VoiceOver](https://webkit.org/b/216364) per cui VoiceOver non annuncia correttamente le immagini SVG.

```html
<img src="mdn.svg" alt="MDN logo" role="img" />
```
