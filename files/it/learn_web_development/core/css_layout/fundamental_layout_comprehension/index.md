---
title: "Sfida: Comprensione fondamentale del layout"
short-title: "Sfida: Fondamenti di layout"
slug: Learn_web_development/Core/CSS_layout/Fundamental_Layout_Comprehension
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/Scripting", "Learn_web_development/Core/CSS_layout")}}

Se hai lavorato attraverso questo modulo, avrai già coperto le basi di ciò che hai bisogno di sapere per fare il layout CSS oggi e anche lavorare con vecchi CSS. Questo compito testerà parte della tua conoscenza tramite lo sviluppo di un semplice layout di pagina web usando una varietà di tecniche.

## Punto di partenza

Puoi scaricare l'HTML, CSS e un set di sei immagini [nel nostro repository learning-area](https://github.com/mdn/learning-area/tree/main/css/css-layout/fundamental-layout-comprehension).

Salva il documento HTML e il foglio di stile in una directory sul tuo computer e aggiungi le immagini in una cartella chiamata `images`. Aprendo il file `index.html` in un browser dovresti vedere una pagina con uno stile di base ma senza layout, simile all'immagine qui sotto.

![Punto di partenza del compito di layout. Gli elementi non sono disposti ordinatamente. C'è un titolo del sito web, sopra una barra di navigazione nera con 5 link allineati a sinistra, seguito dal titolo del post del blog e dal contenuto del post. Tra il titolo del blog e il contenuto del blog c'è una foto allineata a sinistra.](layout-task-start.png)

Questo punto di partenza ha tutto il contenuto del tuo layout come mostrato dal browser nel flusso normale.

In alternativa, potresti utilizzare un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
Se utilizzi un editor online, dovrai caricare le immagini e sostituire i valori negli attributi `src` per puntare alle nuove posizioni delle immagini.

> [!NOTE]
> Se incontri difficoltà, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## Descrizione del progetto

Ti è stato fornito del codice HTML, CSS di base e immagini — ora devi creare un layout per il design.

### I tuoi compiti

Ora devi implementare il tuo layout. I compiti che devi raggiungere sono:

1. Visualizzare gli elementi di navigazione su una riga, con una quantità uguale di spazio tra gli elementi.
2. La barra di navigazione dovrebbe scorrere con il contenuto e poi restare fissa in cima alla finestra del browser quando la raggiunge.
3. L'immagine che si trova all'interno dell'articolo dovrebbe avere il testo che la circonda.
4. Gli elementi {{htmlelement("article")}} e {{htmlelement("aside")}} dovrebbero essere visualizzati come un layout a due colonne. Le colonne dovrebbero avere una dimensione flessibile in modo che se la finestra del browser si riduce, le colonne diventino più strette.
5. Le fotografie dovrebbero essere visualizzate come una griglia a due colonne con uno spazio di 1 pixel tra le immagini.

## Suggerimenti e consigli

Non sarà necessario modificare l'HTML per ottenere questo layout e le tecniche che dovresti usare sono:

- Flexbox
- Griglia
- Galleggiamento
- Posizionamento

Ci sono diversi modi in cui potresti raggiungere alcuni di questi compiti e spesso non c'è un modo giusto o sbagliato per fare le cose. Prova diversi approcci e vedi quale funziona meglio. Prendi appunti mentre sperimenti.

## Esempio

La schermata seguente mostra un esempio di come dovrebbe apparire il layout finale del design:

![Layout finale del sito web del compito. Gli elementi sono disposti ordinatamente. C'è un titolo del sito web, sopra una barra di navigazione nera contenente 5 link equamente distanziati. Sotto la barra di navigazione, ci sono due sezioni. A sinistra c'è un post del blog: un titolo del post del blog seguito dal contenuto del post. Il contenuto del blog circonda una foto allineata a sinistra. Sul lato destro c'è un titolo 'fotografia' su un gruppo di immagini disposte in una griglia larga due immagini.](layout-task-complete.png)

{{PreviousMenuNext("Learn_web_development/Core/CSS_layout/Media_queries", "Learn_web_development/Core/Scripting", "Learn_web_development/Core/CSS_layout")}}
