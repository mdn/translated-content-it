---
title: "Tutorial su Express Parte 5: Visualizzare i dati della biblioteca"
short-title: "5: Visualizzare i dati"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data
l10n:
  sourceCommit: afcdfa050626bb7eb05ee693df8997020db9ff2e
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Ora è possibile aggiungere le pagine che visualizzano i libri e gli altri dati del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Le pagine includeranno una home page che mostra quanti record sono disponibili per ciascun tipo di modello e pagine di elenco e di dettaglio per tutti i modelli. Durante il percorso, verrà acquisita esperienza pratica nel recupero di record dal database e nell'uso dei template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare gli argomenti dei tutorial precedenti (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes">Tutorial su Express Parte 4: Route e controller</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come eseguire operazioni asincrone sul database usando <code>async</code>/<code>await</code>, come usare il linguaggio di template Pug e come ottenere dati dall'URL nelle funzioni del controller.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Negli articoli precedenti di questo tutorial, sono stati definiti [modelli Mongoose](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose) utilizzabili per interagire con un database e sono stati creati alcuni record iniziali della biblioteca. Sono state poi [create tutte le route](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes) necessarie per il sito web LocalLibrary, ma con funzioni "controller fittizie" (funzioni controller scheletriche che restituiscono semplicemente un messaggio "non implementato" quando si accede a una pagina).

Il passaggio successivo consiste nel fornire implementazioni appropriate per le pagine che _visualizzano_ le informazioni della biblioteca (gli articoli successivi analizzeranno l'implementazione di pagine con moduli per creare, aggiornare o eliminare informazioni). Ciò include l'aggiornamento delle funzioni controller per recuperare record usando i modelli e la definizione di template per visualizzare queste informazioni agli utenti.

Si inizierà fornendo argomenti introduttivi che spiegano come gestire operazioni asincrone nelle funzioni controller e come scrivere template usando Pug. Verranno quindi fornite implementazioni per ciascuna delle principali pagine di "sola lettura", con una breve spiegazione delle funzionalità speciali o nuove utilizzate.

Alla fine di questo articolo, dovrebbe essere disponibile una buona comprensione completa di come funzionano nella pratica route, funzioni asincrone, viste e modelli.

## Sottoarticoli del tutorial sulla visualizzazione dei dati della biblioteca

I seguenti sottoarticoli illustrano il processo di aggiunta delle diverse funzionalità necessarie per visualizzare le pagine richieste del sito web.
È necessario leggere e seguire ciascuno di essi nell'ordine indicato prima di passare a quello successivo.

1. [Introduzione ai template](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer)
2. [Il template di base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template)
3. [Home page](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page)
4. [Pagina dell'elenco dei libri](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page)
5. [Pagina dell'elenco di BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page)
6. [Formattazione delle date usando luxon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment)
7. [Sfida sulla pagina dell'elenco degli autori e sulla pagina dell'elenco dei generi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page)
8. [Pagina di dettaglio del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page)
9. [Pagina di dettaglio del libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page)
10. [Pagina di dettaglio dell'autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page)
11. [Pagina di dettaglio di BookInstance e sfida](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge)

## Riepilogo

Sono state ora create tutte le pagine di "sola lettura" del sito: una home page che visualizza il numero di istanze di ciascuno dei modelli e pagine di elenco e di dettaglio per libri, istanze dei libri, autori e generi. Durante il percorso, sono state acquisite molte conoscenze fondamentali sui controller, sulla gestione del flusso di controllo durante l'uso di operazioni asincrone, sulla creazione di viste usando _Pug_, sull'interrogazione del database del sito usando modelli, sul passaggio di informazioni a una vista e sulla creazione e sull'estensione di template. Le sfide avranno inoltre insegnato ai lettori qualcosa sulla gestione delle date usando _Luxon_.

Nel prossimo articolo, verranno approfondite queste conoscenze creando moduli HTML e codice per la gestione dei moduli, per iniziare a modificare i dati archiviati dal sito.

## Vedi anche

- [Uso dei motori di template con Express](https://expressjs.com/en/guide/using-template-engines/) (documentazione di Express)
- [Pug](https://pugjs.org/api/getting-started.html) (documentazione di Pug)
- [Luxon](https://moment.github.io/luxon/#/) (documentazione di Luxon)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
