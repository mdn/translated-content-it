---
title: "Sfida: Pagina di lancio di Mozilla"
slug: Learn_web_development/Core/Structuring_content/Mozilla_splash_page
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content")}}

In questa sfida, metteremo alla prova la tua conoscenza di alcune delle tecniche discusse nelle ultime lezioni, facendoti aggiungere alcune immagini e video a una simpatica pagina di lancio tutta su Mozilla!

## Punto di partenza

Per iniziare questa valutazione, devi prendere l'HTML e tutte le immagini disponibili nella directory [mdn-splash-page-start](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/mdn-splash-page-start) su GitHub. Salva il contenuto di [index.html](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/mdn-splash-page-start/index.html) in un file chiamato `index.html` sul tuo disco locale, in una nuova directory. Poi salva [pattern.png](https://github.com/mdn/learning-area/blob/main/html/multimedia-and-embedding/mdn-splash-page-start/pattern.png) nella stessa directory (clic destro sull'immagine per ottenere un'opzione per salvarla).

Accedi alle diverse immagini nella directory [originals](https://github.com/mdn/learning-area/tree/main/html/multimedia-and-embedding/mdn-splash-page-start/originals) e salvatele allo stesso modo; per ora, vorrai salvarle in una directory diversa, poiché dovrai manipolare (alcune di) loro usando un editor grafico prima che siano pronte per essere utilizzate.

In alternativa, puoi usare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).

> [!NOTE]
> Il file HTML di esempio contiene un bel po' di CSS, per stilizzare la pagina. Non devi toccare il CSS, solo l'HTML all'interno dell'elemento {{htmlelement("body")}} — finché inserisci il markup corretto, lo stile lo farà apparire correttamente.
>
> Se sei bloccato, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Brief del progetto

In questa valutazione ti presentiamo una pagina di lancio di Mozilla quasi completata, che mira a dire qualcosa di bello e interessante su cosa rappresenta Mozilla e a fornire alcuni link a ulteriori risorse. Purtroppo, non sono state ancora aggiunte immagini o video — questo è il tuo compito! Devi aggiungere alcuni media per rendere la pagina piacevole e più comprensibile. Le sottosezioni seguenti dettagliano ciò che devi fare:

### Preparare le immagini

Usando il tuo editor di immagini preferito, crea versioni di 400px di larghezza di:

- `firefox_logo-only_RGB.png`
- `firefox-addons.jpg`
- `mozilla-dinosaur-head.png`

Insieme a `mdn.svg`, queste immagini saranno le tue icone per collegarti a risorse aggiuntive, all'interno dell'area `further-info`. Collegherai anche il logo di Firefox nell'intestazione del sito. Salva copie di tutte queste immagini nella stessa directory di `index.html`.

Poi, crea una versione in formato panorama di 1200px di larghezza di `red-panda.jpg`. Dalle un nome sensato in modo da poterla identificare facilmente. Salva una copia nella stessa directory di `index.html`.

> [!NOTE]
> Dovresti ottimizzare le tue immagini JPG e PNG per renderle il più piccole possibile, pur mantenendo una buona resa. [tinypng.com](https://tinypng.com/) è un ottimo servizio per farlo facilmente.

### Aggiungere un logo all'intestazione

All'interno dell'elemento {{htmlelement("header")}}, aggiungi un elemento {{htmlelement("img")}} che incorpori la versione piccola del logo di Firefox nell'intestazione.

### Aggiungere un video al contenuto principale dell'articolo

Proprio all'interno dell'elemento {{htmlelement("article")}} (subito sotto il tag di apertura), incorpora il video di YouTube trovato su <https://www.youtube.com/watch?v=ojcNcvb1olg>, utilizzando gli strumenti appropriati di YouTube per generare il codice. Il video dovrebbe essere largo 400px.

> [!NOTE]
> Questo è un obiettivo un po' avanzato, poiché non abbiamo discusso il codice necessario per incorporare video di YouTube nel nostro corso. Prova a cercare come incorporare un video di YouTube online.

### Aggiungere immagini ai link di ulteriori informazioni

All'interno del {{htmlelement("div")}} con la classe `further-info` troverai quattro elementi {{htmlelement("a")}} — ognuno collegato a una pagina interessante relativa a Mozilla. Per completare questa sezione dovrai inserire un elemento {{htmlelement("img")}} all'interno di ciascuno per incorporare l'immagine appropriata.

Assicurati di abbinare le immagini corrette con i link corretti!

### Aggiungere il panda rosso

All'interno del {{htmlelement("div")}} con la classe `red-panda`, vogliamo inserire un elemento {{htmlelement("img")}} che visualizzi l'immagine del panda rosso.

## Suggerimenti e consigli

- Puoi usare il [Controllo Nu HTML del W3C](https://validator.w3.org/nu/) per individuare errori nel tuo HTML.
- Non hai bisogno di conoscere il CSS per fare questa valutazione; hai solo bisogno del file HTML fornito. La parte CSS è già stata fatta per te.
- L'HTML fornito (incluso lo stile CSS) fa già la maggior parte del lavoro per te, quindi puoi concentrarti solo sull'incorporazione dei media.

## Esempio

Gli screenshot seguenti mostrano come dovrebbe apparire la pagina di lancio.

![Una vista ampia della nostra pagina di lancio di esempio](wide-shot.png)

{{PreviousMenuNext("Learn_web_development/Core/Structuring_content/HTML_video_and_audio", "Learn_web_development/Core/Structuring_content/HTML_table_basics", "Learn_web_development/Core/Structuring_content")}}
