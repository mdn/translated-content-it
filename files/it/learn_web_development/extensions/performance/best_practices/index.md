---
title: Best practice e suggerimenti per le prestazioni web
short-title: Best practice e suggerimenti
slug: Learn_web_development/Extensions/Performance/Best_practices
l10n:
  sourceCommit: 8db892b3e7ca294621898441e7db2481e0e6d939
---

{{PreviousMenu("Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}

Esistono molte [ragioni](https://web.dev/learn/performance/why-speed-matters) per cui il sito web dovrebbe offrire le migliori prestazioni possibili.
Di seguito è riportata una rapida panoramica delle best practice, degli strumenti e delle API, con link per fornire ulteriori informazioni su ciascun argomento.

## Best practice

- Iniziare imparando il [critical rendering path](/it/docs/Web/Performance/Guides/Critical_rendering_path) del browser. Conoscerlo aiuterà a comprendere come migliorare le prestazioni del sito.
- Usare _resource hints_ quali [`rel=preconnect`](/it/docs/Web/HTML/Reference/Attributes/rel/preconnect), [`rel=dns-prefetch`](/it/docs/Web/HTML/Reference/Attributes/rel/dns-prefetch), [`rel=prefetch`](/it/docs/Web/HTML/Reference/Attributes/rel/prefetch), [`rel=preload`](/it/docs/Web/HTML/Reference/Attributes/rel/preload).
- Ridurre al [minimo](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4) le dimensioni di JavaScript. Usare solo la quantità di JavaScript necessaria per la pagina corrente.
- Fattori che influenzano le prestazioni di [CSS](/it/docs/Learn_web_development/Extensions/Performance/CSS)
- Usare {{Glossary("HTTP_2", "HTTP/2")}} sul server (o CDN).
- Usare una CDN per le risorse, in grado di ridurre significativamente i tempi di caricamento.
- Comprimere le risorse usando [gzip](https://www.gnu.org/software/gzip/), [Brotli](https://github.com/google/brotli) e [Zopfli](https://github.com/google/zopfli).
- Ottimizzare le immagini (usare animazioni CSS o SVG, se possibile).
- Caricare in modo differito le parti dell'applicazione al di fuori della viewport. In questo caso, predisporre un piano di riserva per la SEO (ad esempio, eseguire il rendering dell'intera pagina per il traffico dei bot); per esempio, usando l'attributo [`loading`](/it/docs/Web/HTML/Reference/Elements/img#loading) sull'elemento {{HTMLElement("img")}}, o analogamente sugli elementi {{HTMLElement("iframe")}}, {{HTMLElement("video")}}, {{HTMLElement("audio")}}.
- È inoltre fondamentale capire cosa sia davvero importante per gli utenti. Potrebbe non trattarsi del tempo assoluto, ma della [percezione dell'utente](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance).

## Risultati rapidi

### CSS

Le prestazioni web riguardano l'esperienza utente e le prestazioni percepite. Come appreso nel documento sul [critical rendering path](/it/docs/Web/Performance/Guides/Critical_rendering_path), il collegamento di CSS con un tag link tradizionale con `rel="stylesheet"` è sincrono e blocca il rendering. Ottimizzare il rendering della pagina rimuovendo il CSS che blocca il rendering.

Per caricare CSS in modo asincrono, è possibile impostare il tipo di media su `print` e poi cambiarlo in `all` una volta completato il caricamento. Ciò richiede JavaScript, pertanto è importante includere un tag `<noscript>` con un fallback tradizionale.

```html
<link
  id="my-stylesheet"
  rel="stylesheet"
  href="/path/to/my.css"
  media="print" />
<noscript><link rel="stylesheet" href="/path/to/my.css" /></noscript>
```

```js
const stylesheet = document.getElementById("my-stylesheet");
stylesheet.addEventListener("load", () => {
  stylesheet.media = "all";
});
```

Lo svantaggio di questo approccio è il flash di testo non formattato (FOUT). Il modo più semplice per risolvere il problema consiste nell'includere inline il CSS richiesto per qualsiasi contenuto sottoposto a rendering above the fold, ovvero quello visibile nella viewport del browser prima dello scorrimento. Questi stili miglioreranno le prestazioni percepite poiché il CSS non richiede una richiesta di file.

```html
<style>
  /* Insert your CSS here */
</style>
```

### JavaScript

Evitare che JavaScript blocchi il rendering usando gli attributi [`async`](/it/docs/Web/HTML/Reference/Elements/script) o [`defer`](/it/docs/Web/HTML/Reference/Elements/script), oppure collegare le risorse JavaScript dopo gli elementi DOM della pagina. JavaScript blocca il rendering solo degli elementi che compaiono dopo il tag script nell'albero DOM.

### Web font

I formati EOT e TTF non sono compressi per impostazione predefinita. Applicare una compressione come GZIP o Brotli a questi tipi di file. Usare WOFF e WOFF2. Questi formati includono la compressione.

All'interno di @font-face usare font-display: swap. Usando font display swap, il browser non bloccherà il rendering e utilizzerà i font di sistema di riserva definiti. Ottimizzare il [peso del font](/it/docs/Web/CSS/Reference/Properties/font-weight) affinché corrisponda il più possibile al web font.

#### Web font per icone

Se possibile, evitare i web font per icone e usare SVG compressi. Per ottimizzare ulteriormente, includere inline i dati SVG nel markup HTML per evitare richieste HTTP.

## Strumenti

- Imparare a usare i [Firefox Dev Tools](https://firefox-source-docs.mozilla.org/devtools-user/performance/index.html) per creare il profilo del sito.
- [PageSpeed Insights](https://pagespeed.web.dev/) può analizzare la pagina e fornire alcuni suggerimenti generali per migliorare le prestazioni.
- [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) può fornire una panoramica dettagliata di molti aspetti del sito, incluse prestazioni, SEO e accessibilità.
- Testare la velocità della pagina usando [WebPageTest.org](https://www.webpagetest.org/), dove è possibile utilizzare diversi tipi di dispositivi reali e località.
- Provare il [Chrome User Experience Report](https://developer.chrome.com/docs/crux/), che quantifica le metriche degli utenti reali.
- Definire un [budget delle prestazioni](/it/docs/Web/Performance/Guides/Performance_budgets).

### API

- Raccogliere metriche degli utenti usando la libreria [boomerang](https://github.com/akamai/boomerang).
- Oppure raccoglierle direttamente con [window.performance.timing](/it/docs/Web/API/Performance/timing)

### Cose da non fare (pratiche errate)

- Scaricare tutto.
- Usare file multimediali non compressi.

## Vedi anche

- <https://github.com/filamentgroup/loadCSS>
