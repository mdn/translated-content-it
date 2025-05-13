---
title: Risorse per le prestazioni del web
slug: Learn_web_development/Extensions/Performance/Web_Performance_Basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}

Ci sono molte [ragioni](https://web.dev/learn/performance/why-speed-matters) per cui il vostro sito web dovrebbe avere le migliori prestazioni possibili. Di seguito è riportata una rapida panoramica delle migliori pratiche, strumenti, API con link per fornire maggiori informazioni su ciascun argomento.

## Migliori pratiche

- Inizi con l'apprendimento del [percorso di rendering critico](/it/docs/Web/Performance/Guides/Critical_rendering_path) del browser. Conoscerlo vi aiuterà a comprendere come migliorare le prestazioni del sito.
- Utilizzo di _suggerimenti sulle risorse_ come [`rel=preconnect`](/it/docs/Web/HTML/Reference/Attributes/rel/preconnect), [`rel=dns-prefetch`](/it/docs/Web/HTML/Reference/Attributes/rel/dns-prefetch), [`rel=prefetch`](/it/docs/Web/HTML/Reference/Attributes/rel/prefetch), [`rel=preload`](/it/docs/Web/HTML/Reference/Attributes/rel/preload).
- Mantenere le dimensioni di JavaScript al [minimo](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4). Utilizzare solo tanto JavaScript quanto necessario per la pagina corrente.
- Fattori di prestazione del [CSS](/it/docs/Learn_web_development/Extensions/Performance/CSS)
- Utilizzare {{Glossary("HTTP_2", "HTTP/2")}} sul server (o CDN).
- Utilizzare un CDN per le risorse che possono ridurre significativamente i tempi di caricamento.
- Comprimere le risorse utilizzando [gzip](https://www.gnu.org/software/gzip/), [Brotli](https://github.com/google/brotli) e [Zopfli](https://github.com/google/zopfli).
- Ottimizzazione delle immagini (usare animazione CSS o SVG se possibile).
- Caricamento lento delle parti della vostra applicazione al di fuori della finestra di visualizzazione. In questo caso, avere un piano di backup per la SEO (ad esempio, rendere la pagina completa per il traffico bot); ad esempio, utilizzando l'attributo [`loading`](/it/docs/Web/HTML/Reference/Elements/img#loading) sull'elemento {{HTMLElement("img")}}
- È anche fondamentale comprendere ciò che è veramente importante per i vostri utenti. Potrebbe non essere il tempo assoluto, ma la [percezione dell'utente](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance).

## Vantaggi rapidi

### CSS

Le prestazioni del web riguardano l'esperienza dell'utente e la percezione delle prestazioni. Come abbiamo appreso nel documento sul [percorso di rendering critico](/it/docs/Web/Performance/Guides/Critical_rendering_path), collegare il CSS con un tag di collegamento tradizionale con rel="stylesheet" è sincrono e blocca il rendering. Ottimizzate il rendering della vostra pagina rimuovendo il CSS bloccante.

Per caricare CSS in modo asincrono si può impostare il tipo di media su print e poi cambiarlo in all una volta caricato. Il seguente snippet include un attributo onload, richiedendo JavaScript, quindi è importante includere un tag noscript con un fallback tradizionale.

```html
<link
  rel="stylesheet"
  href="/path/to/my.css"
  media="print"
  onload="this.media='all'" />
<noscript><link rel="stylesheet" href="/path/to/my.css" /></noscript>
```

Lo svantaggio di questo approccio è il flash di testo non stilizzato (FOUT). Il modo più semplice per affrontare questo problema è includere inline il CSS necessario per qualsiasi contenuto visualizzato sopra la piega, o ciò che si vede nella finestra del browser prima di scorrere. Questi stili miglioreranno la percezione delle prestazioni poiché il CSS non richiede una richiesta di file.

```html
<style>
  /* Insert your CSS here */
</style>
```

### JavaScript

Evitare il blocco di JavaScript utilizzando gli attributi [async](/it/docs/Web/HTML/Reference/Elements/script) o [defer](/it/docs/Web/HTML/Reference/Elements/script), o collegare le risorse JavaScript dopo gli elementi del DOM della pagina. JavaScript blocca solo il rendering degli elementi che appaiono dopo il tag script nell'albero DOM.

### Web Fonts

I formati EOT e TTF non sono compressi per impostazione predefinita. Applichi la compressione come GZIP o Brotli per questi tipi di file. Utilizzi WOFF e WOFF2. Questi formati hanno la compressione incorporata.

All'interno di @font-face utilizzi font-display: swap. Utilizzando il font display swap il browser non bloccherà il rendering e utilizzerà i font di sistema di backup definiti. Ottimizzi il [peso del font](/it/docs/Web/CSS/font-weight) per corrispondere al carattere web il più possibile.

#### Icon web fonts

Se possibile, evitare i font di icone web e utilizzare SVG compressi. Per ottimizzare ulteriormente, includa inline i dati SVG all'interno del markup HTML per evitare richieste HTTP.

## Strumenti

- Impari a utilizzare i [Dev Tools di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/performance/index.html) per profilare il vostro sito.
- [PageSpeed Insights](https://pagespeed.web.dev/) può analizzare la vostra pagina e fornire alcuni suggerimenti generali per migliorare le prestazioni.
- [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) può fornire una descrizione dettagliata di molti aspetti del vostro sito, incluse le prestazioni, la SEO e l'accessibilità.
- Testi la velocità della vostra pagina utilizzando [WebPageTest.org](https://www.webpagetest.org/), dove è possibile utilizzare diversi tipi di dispositivi reali e posizioni.
- Provi il [Rapporto sull'esperienza degli utenti di Chrome](https://developer.chrome.com/docs/crux/) che quantifica le metriche reali degli utenti.
- Definisca un [budget delle prestazioni](/it/docs/Web/Performance/Guides/Performance_budgets).

### API

- Raccogliere le metriche degli utenti utilizzando la libreria [boomerang](https://github.com/akamai/boomerang).
- Oppure raccogliere direttamente con [window.performance.timing](/it/docs/Web/API/Performance/timing)

### Cose da non fare (cattive pratiche)

- Scaricare tutto
- Utilizzare file multimediali non compressi

## Vedere anche

- <https://github.com/filamentgroup/loadCSS>
