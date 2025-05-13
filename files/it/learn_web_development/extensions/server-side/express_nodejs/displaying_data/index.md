---
title: "Tutorial Express Parte 5: Visualizzare i dati della libreria"
short-title: "5: Visualizzazione dei dati"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Siamo ora pronti per aggiungere le pagine che visualizzano i libri e altri dati del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Le pagine includeranno una home page che mostrerà quanti record abbiamo per ciascun tipo di modello e pagine con elenchi e dettagli per tutti i nostri modelli. Durante il processo, acquisiremo esperienza pratica nel recuperare record dal database e nell'utilizzare i template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completi gli argomenti precedenti del tutorial (incluso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes">Tutorial Express Parte 4: Rotte e controller</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come eseguire operazioni asincrone sul database utilizzando <code>async</code>/<code>await</code>, come utilizzare il linguaggio di template Pug, e come ottenere dati dall'URL nelle nostre funzioni controller.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Nei nostri precedenti articoli del tutorial, abbiamo definito [modelli Mongoose](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose) che possiamo utilizzare per interagire con un database e abbiamo creato alcuni record iniziali della libreria. Abbiamo poi [creato tutte le rotte](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes) necessarie per il sito LocalLibrary, ma con funzioni "controller fittizie" (si tratta di funzioni controller scheletriche che restituiscono solo un messaggio "non implementato" quando si accede a una pagina).

Il passo successivo è fornire implementazioni appropriate per le pagine che _visualizzano_ le informazioni della nostra libreria (vedremo come implementare pagine con moduli per creare, aggiornare o eliminare informazioni negli articoli successivi). Ciò include l'aggiornamento delle funzioni controller per recuperare i record utilizzando i nostri modelli e la definizione di template per visualizzare queste informazioni agli utenti.

Inizieremo fornendo argomenti di introduzione su come gestire operazioni asincrone nelle funzioni controller e su come scrivere template utilizzando Pug. Successivamente forniremo implementazioni per ciascuna delle nostre principali pagine "solo lettura" con una breve spiegazione di eventuali caratteristiche speciali o nuove che utilizzano.

Alla fine di questo articolo, dovrebbe avere una buona comprensione end-to-end di come funzionano in pratica rotte, funzioni asincrone, viste e modelli.

## Tutorial sulla visualizzazione dei dati della libreria - sottoarticoli

I seguenti sottoarticoli illustrano il processo di aggiunta delle diverse funzionalità necessarie per visualizzare le pagine richieste del sito web.
È necessario leggere e completare ciascuno in ordine, prima di passare al successivo.

1. [Introduzione ai template](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer)
2. [Il template base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template)
3. [Pagina principale](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page)
4. [Pagina elenco dei libri](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page)
5. [Pagina elenco delle istanze dei libri](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page)
6. [Formattazione della data utilizzando luxon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment)
7. [Sfida: pagina elenco degli autori e pagina elenco dei generi](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page)
8. [Pagina dettaglio del genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page)
9. [Pagina dettaglio del libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page)
10. [Pagina dettaglio dell'autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page)
11. [Pagina dettaglio dell'istanza del libro e sfida](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge)

## Riassunto

Abbiamo ora creato tutte le pagine "solo lettura" per il nostro sito: una home page che visualizza il conteggio delle istanze di ciascuno dei nostri modelli, e pagine di elenco e dettaglio per i nostri libri, istanze dei libri, autori e generi. Lungo la strada, abbiamo acquisito molte conoscenze fondamentali sui controller, la gestione del controllo del flusso quando si utilizzano operazioni asincrone, la creazione di viste utilizzando _Pug_, l'interrogazione del database del sito utilizzando modelli, il passaggio di informazioni a una vista e la creazione ed estensione di template. Le sfide avranno anche insegnato ai lettori un po' sulla gestione delle date usando _Luxon_.

Nel nostro prossimo articolo, costruiremo sulla nostra conoscenza, creando moduli HTML e codice di gestione dei moduli per iniziare a modificare i dati memorizzati dal sito.

## Vedere anche

- [Utilizzare motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express)
- [Pug](https://pugjs.org/api/getting-started.html) (documentazione Pug)
- [Luxon](https://moment.github.io/luxon/#/) (documentazione Luxon)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
