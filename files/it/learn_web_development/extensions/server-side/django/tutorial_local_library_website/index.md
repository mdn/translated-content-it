---
title: "Tutorial di Django: Il sito web della Biblioteca Locale"
short-title: "1: Tutorial della biblioteca locale"
slug: Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django")}}

Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerai e fornisce una panoramica del sito web di esempio "biblioteca locale" che svilupperemo ed evolveremo nei successivi articoli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggi l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction">Introduzione a Django</a>.
        Per gli articoli successivi avrai anche bisogno di avere <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment">configurato un ambiente di sviluppo Django</a>.
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

Benvenuto nel tutorial di Django "Biblioteca Locale" su MDN, in cui sviluppiamo un sito web che potrebbe essere utilizzato per gestire il catalogo di una biblioteca locale.

In questa serie di articoli del tutorial si imparerà a:

- Utilizzare gli strumenti di Django per creare un sito web e un'applicazione scheletrica.
- Avviare e fermare il server di sviluppo.
- Creare modelli per rappresentare i dati della tua applicazione.
- Utilizzare il sito di amministrazione di Django per popolare i dati del tuo sito.
- Creare viste per recuperare dati specifici in risposta a diverse richieste e modelli per renderizzare i dati come HTML da visualizzare nel browser.
- Creare mapper per associare diversi schemi URL a viste specifiche.
- Aggiungere autorizzazione dell'utente e sessioni per controllare il comportamento del sito e l'accesso.
- Lavorare con i form.
- Scrivere codice di test per la tua app.
- Utilizzare efficacemente la sicurezza di Django.
- Distribuire la tua applicazione in produzione.

Hai già appreso alcuni di questi argomenti e toccato brevemente altri. Al termine della serie di tutorial dovresti sapere abbastanza per sviluppare semplici app Django da solo.

## Il sito web LocalLibrary

_LocalLibrary_ è il nome del sito web che creeremo ed evolveremo nel corso di questa serie di tutorial. Come prevedibile, lo scopo del sito web è fornire un catalogo online per una piccola biblioteca locale, dove gli utenti possono sfogliare i libri disponibili e gestire i loro account.

Questo esempio è stato accuratamente scelto perché può scalare per mostrare quanti più o meno dettagli abbiamo bisogno, e può essere utilizzato per mostrare quasi tutte le funzionalità di Django. Più importante ancora, ci permette di fornire un percorso _guidato_ attraverso le funzionalità più importanti del framework web di Django:

- Nei primi articoli del tutorial definiremo una semplice biblioteca _solo per consultazione_ che i membri della biblioteca possono utilizzare per scoprire quali libri sono disponibili. Questo ci permette di esplorare le operazioni comuni a quasi tutti i siti web: leggere e visualizzare contenuti da un database.
- Man mano che progrediamo, l'esempio di biblioteca si estende naturalmente per dimostrare funzionalità più avanzate di Django. Ad esempio possiamo estendere la biblioteca per permettere agli utenti di prenotare libri, e usarlo per dimostrare come utilizzare i form e supportare l'autenticazione degli utenti.

Anche se questo è un esempio molto estensibile, si chiama _**Local**Library_ per una ragione — speriamo di mostrare le informazioni minime necessarie per aiutarti a iniziare rapidamente con Django. Di conseguenza memorizzeremo informazioni sui libri, copie di libri, autori e altre informazioni chiave. Tuttavia, non memorizzeremo informazioni su altri elementi che una biblioteca potrebbe conservare, né forniremo l'infrastruttura necessaria per supportare più siti di biblioteche o altre funzionalità di "grande biblioteca".

## Sono bloccato, dove posso ottenere il codice sorgente?

Mentre lavori al tutorial, forniremo i frammenti di codice appropriati affinché tu possa copiarli e incollarli in ogni punto, e ci sarà altro codice che speriamo tu estenda da solo (con una certa guida).

Se rimani bloccato, puoi trovare la versione completamente sviluppata del sito web [su GitHub qui](https://github.com/mdn/django-locallibrary-tutorial).

## Sommario

Ora che sai qualcosa di più sul sito web _LocalLibrary_ e su cosa imparerai, è il momento di iniziare a creare un [progetto scheletrico](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) per contenere il nostro esempio.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django")}}
