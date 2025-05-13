---
title: Misurare le prestazioni
slug: Learn_web_development/Extensions/Performance/Measuring_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance")}}

Misurare le prestazioni fornisce un parametro importante per aiutarla a valutare il successo della sua app, sito web o servizio web.

Ad esempio, si possono utilizzare le metriche di prestazione per determinare come si comporta la sua app rispetto a quella di un concorrente o confrontare le prestazioni della sua app tra diversi rilasci. Le sue metriche dovrebbero essere rilevanti per gli utenti, il sito e gli obiettivi aziendali. Dovrebbero essere raccolte, misurate in modo coerente e analizzate in un formato comprensibile da parte di stakeholder non tecnici.

Questo articolo introduce strumenti per accedere alle metriche di prestazione web, che possono essere utilizzati per misurare e ottimizzare le prestazioni del suo sito.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        >, e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        <p>
          Fornire informazioni sulle metriche di prestazione web che si possono
          raccogliere tramite varie API di prestazione web e strumenti che si possono
          utilizzare per visualizzare quei dati.
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti di prestazione

Ci sono diversi strumenti disponibili per aiutarla a misurare e migliorare le prestazioni. Possono generalmente essere classificati in due categorie:

- Strumenti che indicano o misurano le prestazioni, come [PageSpeed Insights](https://pagespeed.web.dev/) o il [Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) e il [Performance Monitor](https://firefox-source-docs.mozilla.org/devtools-user/performance/index.html) di Firefox. Questi strumenti le mostrano quanto velocemente o lentamente il suo sito web si carica. Indicano anche le aree che possono essere migliorate per ottimizzare la sua app web.
- [API di Prestazione](/it/docs/Web/API/Performance_API) che può utilizzare per costruire strumenti di prestazione personalizzati.

## Strumenti generali di reportistica delle prestazioni

Strumenti come [PageSpeed Insights](https://pagespeed.web.dev/) possono fornire misurazioni di prestazione rapide. Può inserire un URL e ottenere un report di prestazioni in pochi secondi. Il report contiene punteggi che indicano come il sito web si comporta per dispositivi mobili e desktop. Questo è un buon inizio per comprendere cosa si sta facendo bene e cosa potrebbe essere migliorato.

Al momento della scrittura, il sommario del report di prestazioni di MDN appare simile al seguente:

![Uno screenshot del report di PageSpeed Insights per la homepage di Mozilla.](pagespeed-insight-mozilla-homepage.png)

Un report di prestazioni contiene informazioni su cose come quanto tempo un utente deve aspettare prima che _qualsiasi cosa_ venga visualizzata sulla pagina, quanti byte devono essere scaricati per visualizzare una pagina, e molto altro. Informa anche se i valori misurati sono considerati buoni o cattivi.

[webpagetest.org](https://www.webpagetest.org/) è un altro esempio di uno strumento che testa automaticamente il suo sito e restituisce metriche preziose.

Può provare a testare il suo sito web preferito con questi strumenti e vedere i punteggi.

## Strumenti di monitoraggio della rete

I browser moderni dispongono di strumenti che può utilizzare per analizzare le pagine caricate e determinare come stanno performando; la maggior parte di questi strumenti funziona in modo simile. Ad esempio, il [Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) di Firefox restituisce informazioni dettagliate su tutte le risorse scaricate dalla rete, insieme a un grafico a cascata che mostra quanto tempo ciascuna ha impiegato per essere scaricata.

![Monitor di rete Firefox che mostra un elenco di risorse caricate insieme al tempo di caricamento per risorsa](network-monitor.png)

Dovrebbe anche consultare la [documentazione del Network Monitor di Chrome](https://developer.chrome.com/docs/devtools/network/).

## Strumenti di monitoraggio delle prestazioni

Può anche utilizzare strumenti di prestazione del browser come il [Performance Monitor](https://firefox-source-docs.mozilla.org/devtools-user/performance/index.html) di Firefox per misurare le prestazioni di un'app web o dell'interfaccia utente del sito mentre esegue diverse azioni. Questo indica le funzionalità che potrebbero rallentare la sua app web o sito.

![Pannello degli strumenti per sviluppatori che mostra il grafico a cascata della registrazione #1.](perf-monitor.png)

Vedere anche la [documentazione dello strumento di Prestazioni di Chrome](https://developer.chrome.com/docs/devtools/performance/).

## API di Prestazione

Quando scrive codice per il web, molte [Web API](/it/docs/Web/API) sono disponibili per creare i suoi strumenti di misurazione delle prestazioni.

Può utilizzare la [Navigation Timing API](/it/docs/Web/API/Performance_API/Navigation_timing) per misurare le prestazioni web lato client, incluso il tempo necessario per scaricare la pagina precedente, quanto durano le ricerche di dominio, il tempo totale trascorso nell'esecuzione dell'handler di carico della finestra, e altro ancora. Può utilizzare l'API per metriche correlate a tutti gli eventi di navigazione nel diagramma sottostante.

![I vari handler che la Navigation Timing API può gestire inclusi i parametri della Navigation Timing API Prompt for unload redirect unload App cache DNS TCP Request Response Processing onLoad navigationStart redirectStart redirectEnd fetchStart domainLookupEnd domainLookupStart connectStart (secureConnectionStart) connectEnd requestStart responseStart responseEnd unloadStart unloadEnd domLoading domInteractive domContentLoaded domComplete loadEventStart loadEventEnd](navigationtimingapi.jpg)

La [Performance API](/it/docs/Web/API/Performance_API), che fornisce accesso a informazioni relative alle prestazioni per la pagina corrente, include diverse API tra cui la [Navigation Timing API](/it/docs/Web/API/Performance_API/Navigation_timing), la [User Timing API](/it/docs/Web/API/Performance_API/User_timing), e la [Resource Timing API](/it/docs/Web/API/Performance_API/Resource_timing). Queste interfacce permettono la misurazione accurata del tempo necessario per completare i compiti di JavaScript.

L'oggetto [PerformanceEntry](/it/docs/Web/API/PerformanceEntry) fa parte della _timeline delle prestazioni_. Una _entry di prestazione_ può essere creata direttamente facendo una _[`mark`](/it/docs/Web/API/PerformanceMark)_ o una _[`measure`](/it/docs/Web/API/PerformanceMeasure)_ di prestazione (ad esempio chiamando il metodo [`mark()`](/it/docs/Web/API/Performance/mark)) in un punto esplicito dell'applicazione. Anche le entry di prestazione vengono create in modi indiretti, come il caricamento di una risorsa come un'immagine.

La [PerformanceObserver API](/it/docs/Web/API/PerformanceObserver) può essere utilizzata per osservare eventi di misurazione delle prestazioni e per notificare nuove [entry di prestazione](/it/docs/Web/API/PerformanceEntry) man mano che vengono registrate nella timeline delle prestazioni del browser.

Sebbene questo articolo non entri nel dettaglio sull'uso di queste API, è utile sapere che esistono. Consultare l'articolo [Navigazione e tempistiche](/it/docs/Web/Performance/Guides/Navigation_and_resource_timings) per ulteriori esempi sull'utilizzo delle Web API di prestazione.

## Conclusione

Questo articolo offre una breve panoramica di alcuni strumenti che possono aiutarla a misurare le prestazioni di un'app o sito web. Nel prossimo articolo, vedremo come può ottimizzare le immagini sul suo sito per migliorare le prestazioni.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance")}}
