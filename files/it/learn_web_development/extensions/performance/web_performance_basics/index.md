---
title: Risorse per le prestazioni web
slug: Learn_web_development/Extensions/Performance/Web_Performance_Basics
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Extensions/Performance/business_case_for_performance", "Learn_web_development/Extensions/Performance")}}

Ci sono molte [ragioni](https://web.dev/learn/performance/why-speed-matters) per cui il tuo sito web dovrebbe avere le migliori prestazioni possibili. Di seguito è riportata una rapida panoramica delle migliori pratiche, strumenti, API con link per fornire ulteriori informazioni su ciascun argomento.

## Migliori pratiche

- Inizia imparando il [percorso di rendering critico](/it/docs/Web/Performance/Guides/Critical_rendering_path) del browser. Conoscere questo ti aiuterà a capire come migliorare le prestazioni del sito.
- Utilizzo di _resource hints_ come [`rel=preconnect`](/it/docs/Web/HTML/Reference/Attributes/rel/preconnect), [`rel=dns-prefetch`](/it/docs/Web/HTML/Reference/Attributes/rel/dns-prefetch), [`rel=prefetch`](/it/docs/Web/HTML/Reference/Attributes/rel/prefetch), [`rel=preload`](/it/docs/Web/HTML/Reference/Attributes/rel/preload).
- Mantieni la dimensione di JavaScript al [minimo](https://medium.com/@addyosmani/the-cost-of-javascript-in-2018-7d8950fbb5d4). Utilizza solo il JavaScript necessario per la pagina corrente.
- Fattori di prestazione del [CSS](/it/docs/Learn_web_development/Extensions/Performance/CSS)
- Utilizza {{Glossary("HTTP_2", "HTTP/2")}} sul tuo server (o CDN).
- Usa un CDN per le risorse che può ridurre significativamente i tempi di caricamento.
- Comprimi le tue risorse usando [gzip](https://www.gnu.org/software/gzip/), [Brotli](https://github.com/google/brotli) e [Zopfli](https://github.com/google/zopfli).
- Ottimizzazione delle immagini (usa animazioni CSS o SVG se possibile).
- Caricamento ritardato di parti della tua applicazione al di fuori della viewport. Se lo fai, adottare un piano di backup per la SEO (ad es., renderizza la pagina completa per il traffico dei bot); per esempio, utilizzando l'attributo [`loading`](/it/docs/Web/HTML/Reference/Elements/img#loading) sull'elemento {{HTMLElement("img")}}
- È anche cruciale capire cosa è veramente importante per i tuoi utenti. Potrebbe non essere un tempo assoluto, ma la [percezione dell'utente](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance).

## Vittorie rapide

### CSS

Le prestazioni web riguardano l'esperienza utente e la percezione delle prestazioni. Come abbiamo appreso nel documento sul [percorso di rendering critico](/it/docs/Web/Performance/Guides/Critical_rendering_path), collegare il CSS con un tradizionale tag di collegamento con rel="stylesheet" è sincrono e blocca il rendering. Ottimizza il rendering della tua pagina rimuovendo il CSS bloccante.

Per caricare il CSS in modo asincrono, si può impostare il tipo di media su print e poi cambiare su all una volta caricato. Il seguente snippet include un attributo onload, che richiede JavaScript, quindi è importante includere un tag noscript con un fallback tradizionale.

```html
<link
  rel="stylesheet"
  href="/path/to/my.css"
  media="print"
  onload="this.media='all'" />
<noscript><link rel="stylesheet" href="/path/to/my.css" /></noscript>
```

Lo svantaggio di questo approccio è il flash di testo non stilizzato (FOUT). Il modo più semplice per affrontarlo è includendo inline il CSS richiesto per qualsiasi contenuto che viene renderizzato sopra la piega, o ciò che vedi nella viewport del browser prima di scorrere. Questi stili miglioreranno la percezione della prestazione in quanto il CSS non richiede una richiesta di file.

```html
<style>
  /* Insert your CSS here */
</style>
```

### JavaScript

Evita il blocco di JavaScript utilizzando gli attributi [async](/it/docs/Web/HTML/Reference/Elements/script) o [defer](/it/docs/Web/HTML/Reference/Elements/script), oppure collega i file JavaScript dopo gli elementi DOM della pagina. JavaScript blocca il rendering solo per gli elementi che appaiono dopo il tag script nell'albero DOM.

### Font Web

I formati EOT e TTF non sono compressi di default. Applica compressione come GZIP o Brotli per questi tipi di file. Usa WOFF e WOFF2. Questi formati hanno la compressione integrata.

All'interno di @font-face utilizza font-display: swap. Utilizzando il font display swap, il browser non bloccherà il rendering e utilizzerà i font di sistema di backup che sono definiti. Ottimizza il [peso del font](/it/docs/Web/CSS/font-weight) per farlo corrispondere il più possibile al font web.

#### Font per icone web

Se possibile, evita font per icone web e usa SVG compressi. Per ottimizzare ulteriormente, inserisci inline i tuoi dati SVG all'interno del markup HTML per evitare richieste HTTP.

## Strumenti

- Impara a utilizzare i [Firefox Dev Tools](https://firefox-source-docs.mozilla.org/devtools-user/performance/index.html) per effettuare il profiling del tuo sito.
- [PageSpeed Insights](https://pagespeed.web.dev/) può analizzare la tua pagina e fornire alcuni suggerimenti generali per migliorare le prestazioni.
- [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) può fornire una suddivisione dettagliata di molti aspetti del tuo sito, comprese prestazioni, SEO e accessibilità.
- Testa la velocità della tua pagina su [WebPageTest.org](https://www.webpagetest.org/), dove puoi utilizzare diversi tipi di dispositivi reali e posizioni.
- Prova il [Chrome User Experience Report](https://developer.chrome.com/docs/crux/) che quantifica le metriche reali degli utenti.
- Definisci un [budget di prestazione](/it/docs/Web/Performance/Guides/Performance_budgets).

### API

- Raccogli metriche degli utenti utilizzando la libreria [boomerang](https://github.com/akamai/boomerang).
- Oppure raccoglili direttamente con [window.performance.timing](/it/docs/Web/API/Performance/timing)

### Cose da non fare (cattive pratiche)

- Scaricare tutto
- Utilizzare file multimediali non compressi

## Vedi anche

- <https://github.com/filamentgroup/loadCSS>
