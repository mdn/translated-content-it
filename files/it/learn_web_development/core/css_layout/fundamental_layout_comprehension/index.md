---
title: "Sfida: Comprensione fondamentale del layout"
short-title: "Sfida: Layout fondamentale"
slug: Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/Scripting", "Learn_web_development/Core/CSS_layout")}}

Se ha lavorato su questo modulo, allora avrà già coperto le basi di ciò che deve sapere per fare il layout CSS oggi e per lavorare anche con CSS più vecchi. Questo compito metterà alla prova alcune delle sue conoscenze attraverso lo sviluppo di un layout semplice di una pagina web usando una varietà di tecniche.

## Punto di partenza

Può scaricare l'HTML, il CSS e un set di sei immagini [nel nostro repository dell'area di apprendimento](https://github.com/mdn/learning-area/tree/main/css/css-layout/fundamental-layout-comprehension).

Salvi il documento HTML e il foglio di stile in una directory sul suo computer, e aggiunga le immagini in una cartella chiamata `images`. Aprendo il file `index.html` in un browser dovrebbe ottenere una pagina con uno stile di base ma senza layout, che dovrebbe assomigliare a qualcosa come l'immagine sottostante.

![Punto di partenza del compito di layout. Gli elementi non sono disposti ordinatamente. C'è un titolo del sito web, sopra una barra di navigazione nera con 5 collegamenti allineati a sinistra, seguiti dal titolo del post del blog e dal contenuto del post. Tra il titolo del blog e il contenuto del blog c'è una foto allineata a sinistra.](layout-task-start.png)

Questo punto di partenza ha tutto il contenuto del suo layout come visualizzato dal browser nel flusso normale.

In alternativa, si potrebbe utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
Se utilizza un editor online, dovrà caricare le immagini e sostituire i valori negli attributi `src` in modo da puntare alle nuove posizioni delle immagini.

> [!NOTE]
> Se si blocca, può contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Resoconto del progetto

Le sono stati forniti alcuni HTML grezzi, CSS di base e immagini — ora deve creare un layout per il design.

### I suoi compiti

Ora deve implementare il suo layout. I compiti che deve raggiungere sono:

1. Visualizzare gli elementi di navigazione in una riga, con una quantità uguale di spazio tra gli elementi.
2. La barra di navigazione dovrebbe scorrere con il contenuto e quindi diventare fissa nella parte superiore del viewport quando la raggiunge.
3. L'immagine che è all'interno dell'articolo dovrebbe avere il testo che la avvolge.
4. Gli elementi {{htmlelement("article")}} e {{htmlelement("aside")}} dovrebbero essere visualizzati come un layout a due colonne. Le colonne dovrebbero avere una dimensione flessibile in modo che se la finestra del browser si riduce, le colonne diventino più strette.
5. Le fotografie dovrebbero essere visualizzate come una griglia a due colonne con un divario di 1 pixel tra le immagini.

## Suggerimenti e consigli

Non sarà necessario modificare l'HTML per raggiungere questo layout e le tecniche che dovrebbe usare sono:

- Flexbox
- Griglia
- Fluttuazione
- Posizionamento

Ci sono diversi modi in cui potrebbe raggiungere alcuni di questi compiti, e spesso non c'è un unico modo giusto o sbagliato di fare le cose. Provi diversi approcci e veda quale funziona meglio. Prenda appunti mentre sperimenta.

## Esempio

Lo screenshot seguente mostra un esempio di come dovrebbe apparire il layout finale del design:

![Sito web del compito di layout finito. Gli elementi sono disposti ordinatamente. C'è un titolo del sito web, sopra una barra di navigazione nera contenente 5 collegamenti equidistanti. Sotto la barra di navigazione, ci sono due sezioni. A sinistra c'è un post del blog: Un titolo di post seguito dal contenuto del post. Il contenuto del blog avvolge una foto che è allineata a sinistra. Sul lato destro c'è un titolo 'fotografia' sopra un gruppo di immagini disposte in una griglia larga due immagini.](layout-task-complete.png)

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/Scripting", "Learn_web_development/Core/CSS_layout")}}
