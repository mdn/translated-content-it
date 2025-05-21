---
title: "Tutorial Express Parte 5: Visualizzare i dati della libreria"
short-title: "5: Visualizzare i dati"
slug: Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}

Siamo ora pronti per aggiungere le pagine che visualizzano i libri e gli altri dati del sito web [LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Tutorial_local_library_website). Le pagine includeranno una home page che mostra quanti record abbiamo di ciascun tipo di modello e pagine di elenco e dettaglio per tutti i nostri modelli. Nel frattempo, acquisiremo un'esperienza pratica nella recupero dei record dal database e nell'uso dei template.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Completare i argomenti tutorial precedenti (compreso <a href="/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes">Tutorial Express Parte 4: Rotte e controller</a>).
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Comprendere come eseguire operazioni database asincrone utilizzando <code>async</code>/<code>await</code>, come usare il linguaggio di template Pug, e come ottenere dati dall'URL nelle nostre funzioni controller.
      </td>
    </tr>
  </tbody>
</table>

## Panoramica

Nei nostri articoli tutorial precedenti, abbiamo definito i [modelli Mongoose](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/mongoose) che possiamo utilizzare per interagire con un database e creato alcuni record iniziali della libreria. Abbiamo poi [creato tutte le rotte](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/routes) necessarie per il sito web LocalLibrary, ma con funzioni "dummy controller" (queste sono funzioni controller scheletro che restituiscono solo un messaggio "non implementato" quando si accede a una pagina).

Il passo successivo è fornire implementazioni adeguate per le pagine che _visualizzano_ le informazioni della nostra libreria (vedremo come implementare pagine con moduli per creare, aggiornare o eliminare informazioni in articoli successivi). Questo include l'aggiornamento delle funzioni controller per recuperare i record usando i nostri modelli e la definizione dei template per visualizzare queste informazioni agli utenti.

Inizieremo fornendo argomenti di revisione/introduzione che spiegano come gestire le operazioni asincrone nelle funzioni controller e come scrivere template usando Pug. Quindi forniremo implementazioni per ciascuna delle nostre principali pagine "sola lettura" con una breve spiegazione di eventuali funzionalità speciali o nuove che utilizzano.

Alla fine di questo articolo, dovresti avere una buona comprensione end-to-end di come funzionano in pratica rotte, funzioni asincrone, viste e modelli.

## Sottoarticoli del tutorial sulla visualizzazione dei dati della libreria

I seguenti sottoarticoli percorrono il processo di aggiunta delle diverse funzionalità necessarie per visualizzare le pagine richieste del sito web.
Devi leggere e lavorare su ciascuno di essi a turno, prima di passare al successivo.

1. [Primer sui template](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Template_primer)
2. [Il template base di LocalLibrary](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/LocalLibrary_base_template)
3. [Home page](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Home_page)
4. [Pagina elenco libri](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_list_page)
5. [Pagina elenco BookInstance](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_list_page)
6. [Formattazione delle date usando luxon](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Date_formatting_using_moment)
7. [Sfida pagina elenco Autori e Genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_list_page)
8. [Pagina dettaglio Genere](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Genre_detail_page)
9. [Pagina dettaglio Libro](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Book_detail_page)
10. [Pagina dettaglio Autore](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/Author_detail_page)
11. [Pagina dettaglio BookInstance e sfida](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs/Displaying_data/BookInstance_detail_page_and_challenge)

## Riassunto

Abbiamo ora creato tutte le pagine "sola lettura" per il nostro sito: una home page che mostra il conteggio delle istanze di ciascuno dei nostri modelli, e pagine di elenco e dettaglio per i nostri libri, istanze di libri, autori e generi. Nel frattempo, abbiamo acquisito molta conoscenza fondamentale sui controller, sulla gestione del controllo di flusso quando si utilizzano operazioni asincrone, sulla creazione di viste usando _Pug_, sull'interrogazione del database del sito utilizzando modelli, sul passaggio di informazioni a una vista e sulla creazione ed estensione di template. Le sfide avranno anche insegnato ai lettori qualcosa sulla gestione delle date usando _Luxon_.

Nel nostro prossimo articolo, svilupperemo la nostra conoscenza, creando moduli HTML e codice di gestione dei moduli per iniziare a modificare i dati memorizzati dal sito.

## Vedi anche

- [Utilizzo di motori di template con Express](https://expressjs.com/en/guide/using-template-engines.html) (documentazione Express)
- [Pug](https://pugjs.org/api/getting-started.html) (documentazione Pug)
- [Luxon](https://moment.github.io/luxon/#/) (documentazione Luxon)

{{PreviousMenuNext("Learn_web_development/Extensions/Server-side/Express_Nodejs/routes", "Learn_web_development/Extensions/Server-side/Express_Nodejs/forms", "Learn_web_development/Extensions/Server-side/Express_Nodejs")}}
