---
title: "Verifica delle tue competenze: JSON"
short-title: JSON
slug: Learn_web_development/Core/Scripting/Test_your_skills/JSON
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

L'obiettivo di questo test di abilità è valutare se ha compreso il nostro articolo [Lavorare con JSON](/it/docs/Learn_web_development/Core/Scripting/JSON).

> [!NOTE]
> Può provare le soluzioni scaricando il codice e inserendolo in un editor online come [CodePen](https://codepen.io/), [JSFiddle](https://jsfiddle.net/) o [Glitch](https://glitch.com/).
> Se c'è un errore, verrà registrato nel pannello dei risultati sulla pagina o nella console JavaScript del browser per aiutarla.
>
> Se rimane bloccato, può contattarci attraverso uno dei nostri [canali di comunicazione](/it/docs/MDN/Community/Communication_channels).

## JSON 1

L'unico compito in questo articolo riguarda l'accesso ai dati JSON e il loro utilizzo nella sua pagina. I dati JSON su alcune gatte madri e i loro gattini sono disponibili in [sample.json](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/json/sample.json). Il JSON è caricato nella pagina come stringa di testo ed è fornito nel parametro `catString` della funzione `displayCatInfo()`. Il suo compito è completare le parti mancanti della funzione `displayCatInfo()` per memorizzare:

- I nomi delle tre gatte madri, separati da virgole, nella variabile `motherInfo`.
- Il numero totale di gattini, e quanti sono maschi e femmine, nella variabile `kittenInfo`.

I valori di queste variabili vengono poi stampati a schermo all'interno di paragrafi.

Alcuni suggerimenti/domande:

- I dati JSON sono forniti come testo all'interno della funzione `displayCatInfo()`. Avrà bisogno di analizzarli in JSON prima di poter estrarre alcun dato.
- Probabilmente vorrà utilizzare un ciclo esterno per attraversare le gatte e aggiungere i loro nomi alla stringa della variabile `motherInfo`, e un ciclo interno per attraversare tutti i gattini, sommare il totale di tutti i gattini maschi/femmine, e aggiungere questi dettagli alla stringa della variabile `kittenInfo`.
- L'ultimo nome della gatta madre dovrebbe avere un "e" prima di esso, e un punto dopo. Come fa a garantire che questo funzioni, indipendentemente da quanti gatti ci sono nel JSON? 
- Perché le righe `para1.textContent = motherInfo;` e `para2.textContent = kittenInfo;` sono all'interno della funzione `displayCatInfo()`, e non alla fine dello script? Questo ha a che fare con il codice asincrono.

Provi ad aggiornare il codice live qui sotto per ricreare l'esempio finito:

{{EmbedGHLiveSample("learning-area/javascript/oojs/tasks/json/json1.html", '100%', 400)}}

> [!CALLOUT]
>
> [Scarichi il punto di partenza per questo compito](https://github.com/mdn/learning-area/blob/main/javascript/oojs/tasks/json/json1-download.html) per lavorare nel suo editor o in un editor online.
