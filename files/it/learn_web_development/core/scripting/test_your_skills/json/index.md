---
title: "Metti alla prova le tue competenze: JSON"
short-title: JSON
slug: Learn_web_development/Core/Scripting/Test_your_skills/JSON
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

Lo scopo di questo test di abilità è valutare se hai compreso il nostro articolo [Lavorare con JSON](/it/docs/Learn_web_development/Core/Scripting/JSON).

> [!NOTE]
> Puoi provare le soluzioni scaricando il codice e mettendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/), o [Glitch](https://glitch.com/).
> Se c'è un errore, sarà registrato nel pannello dei risultati nella pagina o nella console JavaScript del browser per aiutarti.
>
> Se ti blocchi, puoi contattarci in uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## JSON 1

L'unico compito in questo articolo riguarda l'accesso ai dati JSON e il loro utilizzo nella tua pagina. I dati JSON su alcune mamme gatte e i loro cuccioli sono disponibili in [sample.json](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/json/sample.json). Il JSON è caricato nella pagina come stringa di testo e reso disponibile nel parametro `catString` della funzione `displayCatInfo()`. Il tuo compito è completare le parti mancanti della funzione `displayCatInfo()` per memorizzare:

- I nomi delle tre mamme gatte, separati da virgole, nella variabile `motherInfo`.
- Il numero totale di cuccioli, e quanti sono maschi e femmine, nella variabile `kittenInfo`.

I valori di queste variabili sono poi stampati sullo schermo all'interno di paragrafi.

Alcuni suggerimenti/domande:

- I dati JSON sono forniti come testo all'interno della funzione `displayCatInfo()`. Dovrai analizzarli in JSON prima di poter ottenere qualsiasi dato.
- Probabilmente vorrai usare un ciclo esterno per scorrere i gatti e aggiungere i loro nomi alla stringa della variabile `motherInfo`, e un ciclo interno per scorrere tutti i cuccioli, sommare il totale di tutti i cuccioli maschi/femmine e aggiungere quei dettagli alla stringa della variabile `kittenInfo`.
- L'ultimo nome delle mamme gatte dovrebbe avere un "e" prima, e un punto alla fine. Come fai a essere sicuro che questo funzioni, indipendentemente dal numero di gatti nel JSON?
- Perché le righe `para1.textContent = motherInfo;` e `para2.textContent = kittenInfo;` sono all'interno della funzione `displayCatInfo()`, e non alla fine dello script? Questo ha a che fare con il codice asincrono.

Prova a aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/json/json1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarica il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/json/json1-download.html) per lavorare nel tuo editor o in un editor online.
