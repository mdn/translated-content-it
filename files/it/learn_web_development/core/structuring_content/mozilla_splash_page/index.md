---
title: "Sfida: Pagina di apertura di Mozilla"
slug: Learn_web_development/Core/Structuring_content/Mozilla_splash_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content")}}

In questa sfida, testeremo la Sua conoscenza di alcune delle tecniche discusse nelle ultime lezioni, facendole aggiungere alcune immagini e video a una vivace pagina di apertura completamente dedicata a Mozilla!

## Punto di partenza

Per iniziare questa valutazione, deve ottenere l'HTML e tutte le immagini disponibili nella directory [mdn-splash-page-start](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/mdn-splash-page-start) su GitHub. Salvi il contenuto di [index.html](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/mdn-splash-page-start/index.html) in un file chiamato `index.html` sul Suo disco locale, in una nuova directory. Poi salvi [pattern.png](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/mdn-splash-page-start/pattern.png) nella stessa directory (clic destro sull'immagine per ottenere l'opzione di salvataggio).

Acceda alle diverse immagini nella directory [originals](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/mdn-splash-page-start/originals) e le salvi nello stesso modo; vorrà salvarle temporaneamente in una directory diversa, poiché dovrà manipolarle (alcune di esse) utilizzando un editor grafico prima che siano pronte per essere utilizzate.

In alternativa, potrebbe utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).

> [!NOTE]
> L'esempio di file HTML contiene un bel po' di CSS per stilizzare la pagina. Non è necessario toccare il CSS, solo l'HTML all'interno dell'elemento {{htmlelement("body")}} — finché inserisce il markup corretto, lo stile lo farà apparire correttamente.
>
> Se si trova in difficoltà, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

In questa valutazione le presentiamo una pagina di apertura di Mozilla per lo più completata, che punta a dire qualcosa di interessante su ciò che rappresenta Mozilla, e a fornire alcuni link a risorse ulteriori. Purtroppo, non sono state ancora aggiunte immagini o video — questo è il Suo compito! Deve aggiungere dei media per rendere la pagina visivamente accattivante e più sensata. Le sezioni seguenti spiegano cosa deve fare:

### Preparazione delle immagini

Utilizzando il Suo editor di immagini preferito, crei versioni larghe 400px di:

- `firefox_logo-only_RGB.png`
- `firefox-addons.jpg`
- `mozilla-dinosaur-head.png`

Insieme a `mdn.svg`, queste immagini saranno le Sue icone per collegarsi a risorse ulteriori, all'interno dell'area `further-info`. Collegherà anche il logo di Firefox nell'intestazione del sito. Salvi copie di tutte queste immagini nella stessa directory di `index.html`.

In seguito, crei una versione in orizzontale larga 1200px di `red-panda.jpg`, Le dia un nome sensato in modo che possa facilmente identificarla. Salvi una copia nella stessa directory di `index.html`.

> [!NOTE]
> Dovrebbe ottimizzare le immagini JPG e PNG per renderle il più piccole possibile, mantenendole comunque visivamente accattivanti. [tinypng.com](https://tinypng.com/) è un ottimo servizio per farlo facilmente.

### Aggiungere un logo all'intestazione

Dentro l'elemento {{htmlelement("header")}}, aggiunga un elemento {{htmlelement("img")}} che incorporerà la piccola versione del logo di Firefox nell'intestazione.

### Aggiungere un video al contenuto principale dell'articolo

Appena dentro l'elemento {{htmlelement("article")}} (subito sotto il tag di apertura), incorpori il video di YouTube trovato a <https://www.youtube.com/watch?v=ojcNcvb1olg>, utilizzando gli strumenti appropriati di YouTube per generare il codice. Il video dovrebbe essere largo 400px.

> [!NOTE]
> Questo è un obiettivo un po' avanzato, in quanto non abbiamo discusso il codice necessario per incorporare video di YouTube nel nostro corso. Provi a cercare come incorporare un video di YouTube online.

### Aggiungere immagini ai link di ulteriori informazioni

Dentro il {{htmlelement("div")}} con la classe `further-info` trova quattro elementi {{htmlelement("a")}} — ciascuno dei quali collega a un'interessante pagina correlata a Mozilla. Per completare questa sezione deve inserire un elemento {{htmlelement("img")}} dentro ciascuno per incorporare l'immagine appropriata.

Si assicuri di abbinare correttamente le immagini ai link corretti!

### Aggiungere il panda rosso

Dentro il {{htmlelement("div")}} con la classe `red-panda`, vogliamo inserire un elemento {{htmlelement("img")}} che mostri l'immagine del panda rosso.

## Suggerimenti e consigli

- Può usare il [W3C Nu HTML Checker](https://validator.w3.org/nu/) per identificare errori nel Suo HTML.
- Non è necessario conoscere il CSS per fare questa valutazione; le serve solo il file HTML fornito. La parte di CSS è già stata fatta per lei.
- L'HTML fornito (incluso lo stile CSS) ha già fatto la maggior parte del lavoro per lei, così può concentrarsi sull'incorporazione dei media.

## Esempio

I seguenti screenshot mostrano come dovrebbe apparire la pagina di apertura.

![A wide shot of our example splash page](wide-shot.png)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content")}}
