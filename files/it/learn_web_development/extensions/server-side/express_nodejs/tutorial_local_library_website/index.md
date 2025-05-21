---
title: "Tutorial Express: Il sito web della Biblioteca Locale"
short-title: "1: Tutorial della biblioteca locale"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerai e fornisce una panoramica del sito web di esempio "biblioteca locale" che svilupperemo e evolveremo negli articoli successivi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggi l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Introduction">Introduzione a Express</a>.
        Per gli articoli seguenti sarà inoltre necessario <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment">configurare un ambiente di sviluppo Node</a>.
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

Benvenuto al tutorial Express (Node) "Biblioteca Locale" di MDN, in cui svilupperemo un sito web che potrebbe essere utilizzato per gestire il catalogo di una biblioteca locale.

In questa serie di articoli tutorial:

- Utilizzerai lo strumento _Express Application Generator_ per creare un sito web e un'applicazione di base.
- Avvierai e fermerai il server web Node.
- Utilizzerai un database per memorizzare i dati della tua applicazione.
- Creerai route per richiedere diverse informazioni e template ("views") per renderizzare i dati come HTML da mostrare nel browser.
- Lavorerai con i form.
- Distribuirai la tua applicazione in produzione.

Hai già imparato alcuni di questi argomenti, e hai accennato brevemente ad altri. Alla fine della serie di tutorial dovresti conoscere abbastanza per sviluppare semplici applicazioni Express da solo.

## Il sito web LocalLibrary

_LocalLibrary_ è il nome del sito web che creeremo e svilupperemo nel corso di questa serie di tutorial. Come ti aspetteresti, lo scopo del sito web è fornire un catalogo online per una piccola biblioteca locale, dove gli utenti possono sfogliare i libri disponibili e gestire i loro account.

Questo esempio è stato attentamente scelto perché può scalare per mostrare tanto o poco dettaglio quanto necessario e può essere utilizzato per mostrare praticamente qualsiasi funzionalità di Express. Più importante, ci consente di fornire un percorso _guidato_ attraverso le funzionalità di cui avrai bisogno in qualsiasi sito web:

- Nei primi articoli del tutorial definiremo una semplice biblioteca _solo di consultazione_ che i membri della biblioteca possono utilizzare per scoprire quali libri sono disponibili. Questo ci consente di esplorare le operazioni comuni a quasi ogni sito web: leggere e visualizzare contenuti da un database.
- Man mano che progrediamo, l'esempio della biblioteca si estende naturalmente per dimostrare caratteristiche del sito web più avanzate. Ad esempio, possiamo estendere la biblioteca per consentire la creazione di nuovi libri e utilizzare questo per dimostrare come utilizzare form e supportare l'autenticazione degli utenti.

Anche se questo è un esempio molto estensibile, si chiama _**Local**Library_ per una ragione — stiamo cercando di mostrare le informazioni minime che ti aiuteranno a iniziare rapidamente con Express. Di conseguenza, memorizzeremo informazioni su libri, copie di libri, autori e altre informazioni chiave. Non memorizzeremo tuttavia informazioni su altri articoli che una biblioteca potrebbe prestare, né forniremo l'infrastruttura necessaria per supportare più siti di biblioteca o altre funzionalità da "grande biblioteca".

## Sono bloccato, dove posso trovare il codice sorgente?

Mentre lavori nel tutorial, forniremo i frammenti di codice appropriati da copiare e incollare in ogni punto, e ci saranno altri codici che speriamo estenderai tu stesso (con alcune indicazioni).

Invece di copiare e incollare tutti i frammenti di codice, prova a scriverli a mano, ti sarà utile a lungo termine poiché sarai più familiare con il codice la prossima volta che dovrai scrivere qualcosa di simile.

Se ti blocchi, puoi trovare la versione completamente sviluppata del sito web [su GitHub qui](https://github.com/mdn/express-locallibrary-tutorial).

> [!NOTE]
> Le versioni specifiche di node, Express e degli altri moduli su cui questa documentazione è stata testata sono elencate nel progetto [package.json](https://github.com/mdn/express-locallibrary-tutorial/blob/main/package.json).

## Riassunto

Ora che sai qualcosa in più sul sito web _LocalLibrary_ e su cosa imparerai, è il momento di iniziare a creare un [progetto base](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website) per contenere il nostro esempio.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/development_environment", "Learn_web_development/Extensions/Server-side/Express_Nodejs/skeleton_website", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
