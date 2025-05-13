---
title: "Tutorial di Django: Il sito web Local Library"
short-title: "1: Tutorial della biblioteca locale"
slug: Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django")}}

Il primo articolo della nostra serie di tutorial pratici spiega cosa imparerà e fornisce una panoramica del sito web di esempio "biblioteca locale" che sviluppiamo ed evolviamo negli articoli successivi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere l'<a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction">introduzione a Django</a>.
        Per gli articoli successivi avrà anche bisogno di <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment">configurare un ambiente di sviluppo Django</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Introdurre l'applicazione di esempio utilizzata in questo tutorial e consentire ai lettori di comprendere quali argomenti verranno trattati.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Benvenuto nel tutorial "Local Library" di Django su MDN, in cui sviluppiamo un sito web che potrebbe essere utilizzato per gestire il catalogo di una biblioteca locale.

In questa serie di articoli del tutorial lei:

- Utilizzerà gli strumenti di Django per creare un sito web scheletro e un'applicazione.
- Avvierà e fermerà il server di sviluppo.
- Creerà modelli per rappresentare i dati della sua applicazione.
- Utilizzerà il sito admin di Django per popolare i dati del suo sito.
- Creerà viste per recuperare dati specifici in risposta a diverse richieste e template per renderizzare i dati come HTML da visualizzare nel browser.
- Creerà mappatori per associare diversi modelli URL a viste specifiche.
- Aggiungerà l'autorizzazione degli utenti e le sessioni per controllare il comportamento e l'accesso al sito.
- Lavorerà con i form.
- Scriverà del codice di test per la sua app.
- Utilizzerà la sicurezza di Django in modo efficace.
- Distribuirà la sua applicazione in produzione.

Ha già appreso alcuni di questi argomenti e ha toccato brevemente altri. Al termine della serie di tutorial, dovrebbe sapere abbastanza per sviluppare semplici app Django da solo.

## Il sito web LocalLibrary

_LocalLibrary_ è il nome del sito web che creeremo ed evolveremo nel corso di questa serie di tutorial. Come ci si aspetterebbe, lo scopo del sito web è fornire un catalogo online per una piccola biblioteca locale, dove gli utenti possono sfogliare i libri disponibili e gestire i loro account.

Questo esempio è stato scelto con cura perché può essere scalato per mostrare tanto o poco dettaglio quanto necessario e può essere utilizzato per mostrare quasi qualsiasi funzionalità di Django. Ancora più importante, ci consente di fornire un percorso _guidato_ attraverso la funzionalità più importante nel framework web Django:

- Nei primi articoli del tutorial definiremo una semplice biblioteca _solo per consultazione_ che i membri della biblioteca possono utilizzare per scoprire quali libri sono disponibili. Questo ci consente di esplorare le operazioni comuni a quasi tutti i siti web: leggere e visualizzare contenuti da un database.
- Man mano che progrediamo, l'esempio della biblioteca si estende naturalmente per dimostrare funzionalità più avanzate di Django. Ad esempio, possiamo estendere la biblioteca per consentire agli utenti di prenotare libri e usare questo per dimostrare come utilizzare i form e supportare l'autenticazione degli utenti.

Anche se questo è un esempio molto estensibile, è chiamato _**Local**Library_ per un motivo: speriamo di mostrare le informazioni minime che la aiuteranno a iniziare a utilizzare Django rapidamente. Di conseguenza, memorizzeremo informazioni sui libri, copie di libri, autori e altre informazioni chiave. Tuttavia, non memorizzeremo informazioni su altri oggetti che una biblioteca potrebbe archiviare, né forniremo l'infrastruttura necessaria per supportare più siti di biblioteca o altre funzionalità di "grande biblioteca".

## Sono bloccato, dove posso ottenere il codice sorgente?

Man mano che procede nel tutorial, forniremo gli opportuni frammenti di codice da copiare e incollare in ogni fase, e ci sarà altro codice che speriamo estenderà da solo (con alcune indicazioni).

Se si trova bloccato, può trovare la versione completamente sviluppata del sito web [su GitHub qui](https://github.com/mdn/django-locallibrary-tutorial).

## Riepilogo

Ora che sa qualcosa in più sul sito web _LocalLibrary_ e su cosa imparerà, è ora di iniziare a creare un [progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) per contenere il nostro esempio.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django")}}
