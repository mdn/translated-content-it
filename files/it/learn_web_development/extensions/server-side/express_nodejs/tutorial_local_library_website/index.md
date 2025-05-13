---
title: "Tutorial Express: Il sito web della Biblioteca Locale"
short-title: "1: Tutorial della biblioteca locale"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerà e fornisce una panoramica del sito web di esempio "biblioteca locale" con cui lavoreremo e che evolveremo nei successivi articoli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Legga l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction">Introduzione a Express</a>.
        Per i seguenti articoli avrà anche bisogno di <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">configurare un ambiente di sviluppo Node</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Introdurre l'applicazione di esempio utilizzata in questo tutorial e permettere ai lettori di comprendere quali argomenti verranno trattati.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Benvenuto nel tutorial Express (Node) "Biblioteca Locale" di MDN, in cui svilupperemo un sito web che potrebbe essere utilizzato per gestire il catalogo per una biblioteca locale.

In questa serie di articoli tutorial lei:

- Utilizzerà lo strumento _Express Application Generator_ per creare un sito web e un'applicazione scheletrica.
- Avvierà e fermerà il server web Node.
- Utilizzerà un database per memorizzare i dati della sua applicazione.
- Creerà rotte per richiedere diverse informazioni e modelli ("views") per renderizzare i dati come HTML da visualizzare nel browser.
- Lavorerà con i moduli.
- Distribuirà la sua applicazione in produzione.

Ha già appreso alcuni di questi argomenti e toccato brevemente altri. Alla fine della serie di tutorial dovrebbe sapere abbastanza per sviluppare semplici applicazioni Express da solo.

## Il sito web LocalLibrary

_LocalLibrary_ è il nome del sito web che creeremo ed evolveremo nel corso di questa serie di tutorial. Come ci si aspetterebbe, lo scopo del sito web è fornire un catalogo online per una piccola biblioteca locale, dove gli utenti possono sfogliare i libri disponibili e gestire i propri account.

Questo esempio è stato scelto con attenzione perché può scalare per mostrare tanto o poco dettaglio quanto necessario e può essere utilizzato per mostrare quasi tutte le funzionalità di Express. Ancora più importante, ci consente di fornire un percorso _guidato_ attraverso le funzionalità di cui avrà bisogno in qualsiasi sito web:

- Nei primi articoli del tutorial definiremo una semplice biblioteca _solo navigazione_ che i membri della biblioteca possono utilizzare per scoprire quali libri sono disponibili. Questo ci permette di esplorare le operazioni comuni a quasi tutti i siti web: leggere e visualizzare contenuti da un database.
- Man mano che progrediamo, l'esempio della biblioteca si estende naturalmente per dimostrare funzionalità del sito web più avanzate. Ad esempio, possiamo estendere la biblioteca per consentire la creazione di nuovi libri e utilizzare ciò per dimostrare come utilizzare i moduli e supportare l'autenticazione dell'utente.

Anche se questo è un esempio molto estensibile, è chiamato _**Local**Library_ per una ragione — speriamo di mostrare le informazioni minime che l'aiuteranno a iniziare rapidamente con Express. Di conseguenza, memorizzeremo informazioni su libri, copie di libri, autori e altre informazioni chiave. Non memorizzeremo tuttavia informazioni su altri oggetti che una biblioteca potrebbe prestare, né forniremo l'infrastruttura necessaria per supportare più sedi della biblioteca o altre funzionalità da "grande biblioteca".

## Sono bloccato, dove posso ottenere il codice sorgente?

Mentre prosegue nel tutorial, forniremo i frammenti di codice appropriati per copiare e incollare in ogni punto, e ci sarà altro codice che speriamo che estenda da solo (con alcune indicazioni).

Invece di copiare e incollare tutti i frammenti di codice, provi a digitarli direttamente, questo le sarà di beneficio a lungo termine, poiché sarà più familiare con il codice la prossima volta che si troverà a scrivere qualcosa di simile.

Se si trova bloccato, può trovare la versione completamente sviluppata del sito web [su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

> [!NOTE]
> Le versioni specifiche di node, Express e degli altri moduli su cui è stata testata questa documentazione sono elencate nel [package.json del progetto](https://github.com/mdn/express-locallibrary-tutorial/blob/main/package.json).

## Sommario

Ora che sa qualcosa di più sul sito web _LocalLibrary_ e su ciò che imparerà, è tempo di iniziare a creare un [progetto scheletrico](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website) per contenere il nostro esempio.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
