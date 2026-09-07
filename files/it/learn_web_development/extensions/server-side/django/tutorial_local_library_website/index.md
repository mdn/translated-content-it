---
title: "Tutorial Django: il sito web Local Library"
short-title: "1: Tutorial sulla libreria locale"
slug: Learn_web_development/Extensions/Server-side/Django/Tutorial_local_library_website
l10n:
  sourceCommit: e89cf8c2d91de5ac01b7153f833eb8abc30364ad
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django")}}

Questo articolo offre una panoramica del tutorial Django di MDN e introduce il sito web di esempio "local library" che verrà utilizzato nelle prossime pagine.
Verranno illustrati gli argomenti trattati dal tutorial, come iniziare, come chiedere aiuto e tutto ciò che serve per creare e distribuire la prima app Python lato server.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Leggere <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/Introduction">Introduzione a Django</a>.
        Per gli articoli successivi sarà inoltre necessario aver <a href="/it/docs/Learn_web_development/Extensions/Server-side/Django/development_environment">configurato un ambiente di sviluppo Django</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Introdurre l'applicazione di esempio utilizzata in questo tutorial e consentire ai lettori di comprendere gli argomenti che verranno trattati.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Benvenuti nel tutorial Django "Local Library" di MDN, nel quale verrà sviluppato un sito web che potrebbe essere utilizzato per gestire il catalogo di una biblioteca locale.

In questa serie di articoli tutorial si:

- Utilizzeranno gli strumenti di Django per creare lo scheletro di un sito web e di un'applicazione.
- Avvierà e arresterà il server di sviluppo.
- Creeranno modelli per rappresentare i dati dell'applicazione.
- Utilizzerà il sito di amministrazione Django per popolare i dati del sito.
- Creeranno viste per recuperare dati specifici in risposta a richieste diverse e template per effettuare il rendering dei dati come HTML da visualizzare nel browser.
- Creeranno mapper per associare diversi pattern URL a viste specifiche.
- Aggiungeranno autorizzazione utente e sessioni per controllare il comportamento e l'accesso al sito.
- Lavorerà con i form.
- Scriverà codice di test per l'app.
- Utilizzerà efficacemente la sicurezza di Django.
- Distribuirà l'applicazione in produzione.

Alcuni di questi argomenti sono già stati affrontati, mentre altri sono stati solo brevemente introdotti. Al termine della serie di tutorial, le conoscenze acquisite dovrebbero essere sufficienti per sviluppare autonomamente semplici app Django.

## Il sito web LocalLibrary

_LocalLibrary_ è il nome del sito web che verrà creato e sviluppato nel corso di questa serie di tutorial. Come prevedibile, lo scopo del sito web è fornire un catalogo online per una piccola biblioteca locale, nel quale gli utenti possano esplorare i libri disponibili e gestire i propri account.

Questo esempio è stato scelto con cura perché può essere ampliato per mostrare più o meno dettagli secondo necessità e può essere utilizzato per illustrare quasi ogni funzionalità di Django. Ancora più importante, consente di fornire un percorso _guidato_ attraverso le funzionalità più importanti del framework web Django:

- Nei primi articoli del tutorial verrà definita una semplice biblioteca di sola _consultazione_ che i membri della biblioteca potranno usare per scoprire quali libri sono disponibili. Ciò consente di esplorare le operazioni comuni a quasi ogni sito web: leggere e visualizzare contenuti da un database.
- Con il proseguire del tutorial, l'esempio della biblioteca verrà naturalmente esteso per dimostrare funzionalità Django più avanzate. Ad esempio, sarà possibile estendere la biblioteca per consentire agli utenti di prenotare libri e utilizzare questa funzionalità per dimostrare come usare i form e supportare l'autenticazione utente.

Sebbene questo esempio sia molto estensibile, viene chiamato _**Local**Library_ per una ragione: l'obiettivo è mostrare le informazioni minime necessarie per iniziare rapidamente a utilizzare Django. Di conseguenza, verranno memorizzate informazioni su libri, copie dei libri, autori e altre informazioni essenziali. Non verranno però memorizzate informazioni su altri elementi che una biblioteca potrebbe conservare, né verrà fornita l'infrastruttura necessaria per supportare più sedi bibliotecarie o altre funzionalità da "grande biblioteca".

## Non riesco a proseguire, dove posso trovare il codice sorgente?

Durante il tutorial verranno forniti gli snippet di codice appropriati da copiare e incollare in ogni passaggio; sarà inoltre presente altro codice che si auspica venga esteso autonomamente, con alcune indicazioni.

In caso di difficoltà, è possibile trovare la versione completamente sviluppata del sito web [qui su GitHub](https://github.com/mdn/django-locallibrary-tutorial).

## Riepilogo

Ora che si conosce meglio il sito web _LocalLibrary_ e ciò che verrà appreso, è il momento di iniziare a creare un [progetto scheletro](/it/docs/Learn_web_development/Extensions/Server-side/Django/skeleton_website) che conterrà l'esempio.

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Django/development_environment", "Learn_web_development/Extensions/Server-side/Django/skeleton_website", "Learn_web_development/Extensions/Server-side/Django")}}
