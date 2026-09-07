---
title: "Tutorial su Express: il sito web della biblioteca locale"
short-title: "1: Tutorial sulla biblioteca locale"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website
l10n:
  sourceCommit: 3143a6094e7b87cf1a96b61f9551fb4d95049777
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Questo articolo offre una panoramica del tutorial su Express di MDN e presenta il sito web di esempio della "biblioteca locale" che verrà utilizzato nelle prossime pagine.
Verranno illustrati gli argomenti trattati dal tutorial, come iniziare, come chiedere aiuto e tutto ciò che serve per creare e distribuire la prima app JavaScript lato server.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction">Introduzione a Express</a>.
        Per gli articoli seguenti sarà inoltre necessario <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">configurare un ambiente di sviluppo Node</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Presentare l'applicazione di esempio utilizzata in questo tutorial e consentire ai lettori di comprendere quali argomenti verranno trattati.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Benvenuti al tutorial su Express (Node) "Local Library" di MDN, nel quale verrà sviluppato un sito web che potrebbe essere utilizzato per gestire il catalogo di una biblioteca locale.

In questa serie di articoli del tutorial verranno svolte le seguenti attività:

- Usare lo strumento _Express Application Generator_ per creare lo scheletro di un sito web e di un'applicazione.
- Avviare e arrestare il web server Node.
- Usare un database per memorizzare i dati dell'applicazione.
- Creare route per richiedere informazioni diverse e template ("views") per eseguire il rendering dei dati come HTML da visualizzare nel browser.
- Lavorare con i moduli.
- Distribuire l'applicazione in produzione.

Alcuni di questi argomenti sono già stati trattati, mentre altri sono stati solo accennati. Al termine della serie di tutorial, si dovrebbero avere conoscenze sufficienti per sviluppare autonomamente semplici app Express.

## Il sito web LocalLibrary

_LocalLibrary_ è il nome del sito web che verrà creato e sviluppato nel corso di questa serie di tutorial. Come previsto, lo scopo del sito web è fornire un catalogo online per una piccola biblioteca locale, nel quale gli utenti possono sfogliare i libri disponibili e gestire i propri account.

Questo esempio è stato scelto con attenzione perché può essere ampliato per mostrare più o meno dettagli secondo necessità e può essere usato per illustrare quasi tutte le funzionalità di Express. Ancora più importante, consente di fornire un percorso _guidato_ attraverso le funzionalità necessarie in qualsiasi sito web:

- Nei primi articoli del tutorial verrà definita una semplice biblioteca di sola _consultazione_ che i membri della biblioteca possono usare per sapere quali libri sono disponibili. Questo consente di esplorare le operazioni comuni a quasi tutti i siti web: leggere e visualizzare contenuti da un database.
- Procedendo, l'esempio della biblioteca si estende naturalmente per dimostrare funzionalità più avanzate del sito web. Ad esempio, la biblioteca può essere estesa per consentire la creazione di nuovi libri e usare questa funzionalità per dimostrare come utilizzare i moduli e supportare l'autenticazione degli utenti.

Sebbene questo sia un esempio molto estensibile, è chiamato _**Local**Library_ per un motivo: l'obiettivo è mostrare le informazioni minime necessarie per iniziare rapidamente a usare Express. Di conseguenza, verranno memorizzate informazioni su libri, copie di libri, autori e altre informazioni chiave. Non verranno tuttavia memorizzate informazioni su altri elementi che una biblioteca potrebbe prestare, né verrà fornita l'infrastruttura necessaria per supportare più sedi della biblioteca o altre funzionalità da "grande biblioteca".

## Sono bloccato, dove posso trovare il codice sorgente?

Durante lo svolgimento del tutorial verranno forniti gli opportuni frammenti di codice da copiare e incollare in ogni passaggio; sarà inoltre presente altro codice che dovrebbe essere esteso autonomamente, con alcune indicazioni.

Invece di copiare e incollare tutti i frammenti di codice, provare a digitarli. Questo sarà utile nel lungo periodo, poiché il codice risulterà più familiare quando sarà necessario scrivere qualcosa di simile.

In caso di difficoltà, è possibile trovare la versione completamente sviluppata del sito web [su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

> [!NOTE]
> Le versioni specifiche di node, Express e degli altri moduli con cui è stata testata questa documentazione sono elencate nel file [package.json](https://github.com/mdn/express-locallibrary-tutorial/blob/main/package.json) del progetto.

## Riepilogo

Ora che sono disponibili maggiori informazioni sul sito web _LocalLibrary_ e su ciò che verrà appreso, è il momento di iniziare a creare un [progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website) che conterrà l'esempio.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
