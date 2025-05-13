---
title: Risorse su Vue
slug: Learn_web_development/Core/Frameworks_libraries/Vue_resources
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}

Ora completeremo il nostro studio di Vue fornendole un elenco di risorse che può utilizzare per approfondire il suo apprendimento, oltre ad alcuni altri utili consigli.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <p>
          Familiarità con i principali linguaggi <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
          <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
          <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>,
          conoscenza del
          <a
            href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line"
            >terminale/command line</a
          >.
        </p>
        <p>
          I componenti Vue sono scritti come una combinazione di oggetti JavaScript che
          gestiscono i dati dell'app e una sintassi di template basata su HTML che mappa
          la struttura del DOM sottostante. Per l'installazione, e per utilizzare alcune delle
          funzionalità più avanzate di Vue (come i Componenti a File Singolo o le funzioni di render),
          sarà necessario un terminale con node + npm installati.
        </p>
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Imparare dove trovare ulteriori informazioni su Vue per continuare ad apprendere.
      </td>
    </tr>
  </tbody>
</table>

## Ulteriori risorse

Ecco dove dovrebbe andare per saperne di più su Vue:

- [Vue Docs](https://vuejs.org/) — Il sito principale di Vue. Contiene documentazione completa, inclusi esempi, ricettari e materiale di riferimento. Questo è il miglior punto di partenza per imparare Vue in profondità.
- [Vue GitHub Repo](https://github.com/vuejs/vue) — Il codice Vue stesso. Qui è possibile segnalare problemi e/o contribuire direttamente al codice di Vue. Studiare il codice sorgente di Vue può aiutarla a comprendere meglio come funziona il framework e a scrivere codice migliore.
- [Vue Discussions](https://github.com/vuejs/core/discussions) — Il forum ufficiale per ottenere aiuto con Vue.
- [Vue CLI Docs](https://cli.vuejs.org/) — Documentazione per il Vue CLI. Questo include informazioni su come personalizzare ed estendere i risultati generati dal CLI.
- [Nuxt](https://nuxt.com/) — Nuxt è un framework Vue lato server, con alcune opinioni architettoniche che possono essere utili per creare applicazioni manutenibili, anche se non utilizza nessuna delle funzionalità di rendering lato server che offre. Questo sito fornisce una documentazione dettagliata su come utilizzare Nuxt.
- [Vue Mastery](https://www.vuemastery.com/courses/) — Una piattaforma educativa a pagamento che si specializza in Vue, inclusi alcuni lezioni gratuite.
- [Vue School](https://vueschool.io/) — Un'altra piattaforma educativa a pagamento specializzata in Vue.

## Costruire e pubblicare la sua app Vue

Il Vue CLI ci fornisce anche gli strumenti per preparare la nostra app per la pubblicazione sul web. Può farlo come segue:

- Se il suo server locale è ancora attivo, termini la sua esecuzione premendo <kbd>Ctrl</kbd> \+ <kbd>C</kbd> nel terminale.

- Successivamente, esegua il comando `npm run build` (o `yarn build`) nella console.

Questo creerà una nuova directory `dist` contenente tutti i file pronti per la produzione. Per pubblicare il suo sito sul web, copi il contenuto di questa cartella nel suo ambiente di hosting.

> [!NOTE]
> I documenti di Vue CLI includono anche una [guida specifica su come pubblicare la sua app](https://cli.vuejs.org/guide/deployment.html#platform-guides) su molte delle piattaforme di hosting comuni.

## Vue 2

Il supporto per Vue 2 finirà il 31 dicembre 2023 e la versione predefinita di Vue per tutti gli strumenti CLI sarà la versione 3 e successive.
La [Composition API](https://vuejs.org/guide/extras/composition-api-faq.html) funziona come alternativa all'API basata sulle proprietà dove viene utilizzata una funzione `setup()` sul componente. Solo ciò che restituisce da questa funzione è disponibile nei suoi `<template>`. È necessario essere espliciti riguardo le proprietà "reattive" quando si utilizza questa API. Vue gestisce questo per lei utilizzando la [Options API](https://vuejs.org/guide/extras/composition-api-faq.html#trade-offs). Questo rende la nuova API tipicamente considerata un caso d'uso più avanzato.

Se sta eseguendo un aggiornamento da Vue 2, è consigliabile consultare la [guida alla migrazione a Vue 3](https://v3-migration.vuejs.org/).

{{PreviousMenu("Learn_web_development/Core/Frameworks_libraries/Vue_refs_focus_management", "Learn_web_development/Core/Frameworks_libraries")}}
