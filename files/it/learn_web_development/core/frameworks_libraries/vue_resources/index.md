---
title: Risorse Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_resources
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}

Ora concluderemo il nostro studio di Vue fornendo un elenco di risorse che puoi utilizzare per approfondire il tuo apprendimento, oltre ad alcuni altri suggerimenti utili.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/riga di comando</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi basata su HTML che mappa la
          struttura del DOM sottostante. Per l'installazione e per utilizzare alcune delle
          funzionalità più avanzate di Vue (come Componenti a File Singolo o funzioni di rendering), avrai bisogno di un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove andare per trovare ulteriori informazioni su Vue, per continuare
        il tuo apprendimento.
      </td>
    </tr>
  </tbody>
</table>

## Ulteriori risorse

Ecco dove dovresti andare per saperne di più su Vue:

- [Vue Docs](https://vuejs.org/) — Il sito principale di Vue. Contiene documentazione completa, inclusi esempi, ricettari e materiale di riferimento. Questo è il posto migliore per iniziare a imparare Vue in profondità.
- [Vue GitHub Repo](https://github.com/vuejs/vue) — Il codice di Vue stesso. Qui puoi segnalare problemi e/o contribuire direttamente al codice di Vue. Studiare il codice sorgente di Vue può aiutarti a comprendere meglio come funziona il framework e a scrivere codice migliore.
- [Vue Discussions](https://github.com/vuejs/core/discussions) — Il forum ufficiale per ricevere aiuto su Vue.
- [Vue CLI Docs](https://cli.vuejs.org/) — Documentazione per Vue CLI. Contiene informazioni su come personalizzare ed estendere l'output che stai generando tramite il CLI.
- [Nuxt](https://nuxt.com/) — Nuxt è un framework server-side per Vue, con alcune opinioni architettoniche utili per creare applicazioni manutenibili, anche se non utilizzi nessuna delle funzionalità di Server Side Rendering che fornisce. Questo sito fornisce documentazione dettagliata su come utilizzare Nuxt.
- [Vue Mastery](https://www.vuemastery.com/courses/) — Una piattaforma di formazione a pagamento che si specializza in Vue, inclusa qualche lezione gratuita.
- [Vue School](https://vueschool.io/) — Un'altra piattaforma di formazione a pagamento specializzata in Vue.

## Costruire e pubblicare la tua app Vue

Vue CLI ci fornisce anche strumenti per preparare la nostra app per la pubblicazione sul web. Puoi farlo così:

- Se il tuo server locale è ancora in esecuzione, terminare premendo <kbd>Ctrl</kbd> \+ <kbd>C</kbd> nel terminale.

- Successivamente, esegui il comando `npm run build` (o `yarn build`) nella console.

Questo comando creerà una nuova directory `dist` contenente tutti i tuoi file pronti per la produzione. Per pubblicare il tuo sito sul web, copia il contenuto di questa cartella nel tuo ambiente di hosting.

> [!NOTE]
> I documenti del Vue CLI includono anche una [guida specifica su come pubblicare la tua app](https://cli.vuejs.org/guide/deployment.html#platform-guides) su molte delle piattaforme di hosting comuni.

## Vue 2

Il supporto per Vue 2 terminerà il 31 dicembre 2023 e la versione predefinita di Vue per tutti gli strumenti CLI sarà la versione 3 e successive. La [Composition API](https://vuejs.org/guide/extras/composition-api-faq.html) funziona come alternativa alla API basata su proprietà dove viene utilizzata una funzione `setup()` sul componente. Solo ciò che restituisci da questa funzione è disponibile nei tuoi `<template>`. È necessario essere espliciti sulle proprietà "reattive" quando si utilizza questa API. Vue si occupa di questo per te utilizzando l'[Options API](https://vuejs.org/guide/extras/composition-api-faq.html#trade-offs). Questo rende la nuova API tipicamente considerata un caso d'uso più avanzato.

Se stai effettuando l'aggiornamento da Vue 2, si consiglia di dare un'occhiata alla [guida alla migrazione di Vue 3](https://v3-migration.vuejs.org/).

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}
