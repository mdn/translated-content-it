---
title: Misurare le prestazioni
slug: Learn_web_development/Extensions/Performance/Measuring_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance")}}

Misurare le prestazioni fornisce una metrica importante per aiutare a valutare il successo della tua app, sito, o servizio web.

Ad esempio, puoi utilizzare le metriche di prestazione per determinare come si comporta la tua app rispetto a un concorrente o confrontare le prestazioni della tua app tra diverse release. Le tue metriche dovrebbero essere rilevanti per i tuoi utenti, sito e obiettivi aziendali. Dovrebbero essere raccolte, misurate in modo coerente, e analizzate in un formato che i portatori di interesse non tecnici possano comprendere.

Questo articolo introduce strumenti che puoi utilizzare per accedere alle metriche di prestazione web, che possono essere utilizzate per misurare e ottimizzare le prestazioni del tuo sito.

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
          Fornire informazioni sulle metriche di prestazione web che puoi
          raccogliere attraverso varie API di prestazioni web e strumenti che puoi
          utilizzare per visualizzare tali dati.
        </p>
      </td>
    </tr>
  </tbody>
</table>

## Strumenti per le prestazioni

Esistono diversi strumenti disponibili per aiutarti a misurare e migliorare le prestazioni. Questi possono generalmente essere classificati in due categorie:

- Strumenti che indicano o misurano le prestazioni, come [PageSpeed Insights](https://pagespeed.web.dev/) o il [Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) di Firefox e il [Performance Monitor](https://firefox-source-docs.mozilla.org/devtools-user/performance/index.html). Questi strumenti ti mostrano quanto velocemente o lentamente si carica il tuo sito web. Indicano anche le aree che possono essere migliorate per ottimizzare la tua app web.
- [Performance API](/it/docs/Web/API/Performance_API) che puoi utilizzare per costruire strumenti di prestazione personalizzati.

## Strumenti generali di reportistica sulle prestazioni

Strumenti come [PageSpeed Insights](https://pagespeed.web.dev/) possono fornire misurazioni rapide delle prestazioni. Puoi inserire un URL e ottenere un report sulle prestazioni in pochi secondi. Il report contiene punteggi che indicano come si comporta il tuo sito web su dispositivi mobili e desktop. Questo è un buon punto di partenza per capire cosa stai facendo bene e cosa potrebbe essere migliorato.

Al momento della scrittura, il sommario del report sulle prestazioni di MDN appare simile al seguente:

![Una schermata del report PageSpeed Insights per la homepage di Mozilla](pagespeed-insight-mozilla-homepage.png)

Un report sulle prestazioni contiene informazioni su aspetti come quanto tempo un utente deve aspettare prima che _qualsiasi_ cosa venga visualizzata sulla pagina, quanti byte devono essere scaricati per visualizzare una pagina, e molto altro. Ti fa anche sapere se i valori misurati sono considerati buoni o cattivi.

[webpagetest.org](https://www.webpagetest.org/) è un altro esempio di uno strumento che testa automaticamente il tuo sito e restituisce metriche preziose.

Puoi provare ad eseguire questi strumenti sul tuo sito preferito e vedere i punteggi.

## Strumenti di monitoraggio della rete

I browser moderni dispongono di strumenti che puoi utilizzare per eseguire test su pagine caricate e determinare come si stanno comportando; la maggior parte di essi funziona in modo simile. Ad esempio, il [Network Monitor](https://firefox-source-docs.mozilla.org/devtools-user/network_monitor/index.html) di Firefox restituisce informazioni dettagliate su tutte le risorse scaricate dalla rete, insieme a un grafico a cascata che mostra quanto tempo ciascuna ha impiegato per essere scaricata.

![Monitor di rete di Firefox che mostra un elenco di risorse che sono state caricate e il tempo di caricamento per ciascuna risorsa](network-monitor.png)

Dovresti anche controllare la [documentazione del Network Monitor di Chrome](https://developer.chrome.com/docs/devtools/network/).

## Strumenti di monitoraggio delle prestazioni

Puoi anche utilizzare strumenti di prestazioni del browser come il [Performance Monitor di Firefox](https://firefox-source-docs.mozilla.org/devtools-user/performance/index.html) per misurare le prestazioni dell'interfaccia utente di un'app web o di un sito mentre esegui diverse azioni. Questo indica le caratteristiche che potrebbero rallentare la tua app web o sito.

![Pannello delle prestazioni degli strumenti per sviluppatori che mostra il grafico a cascata della registrazione #1](perf-monitor.png)

Consulta anche la [documentazione dello strumento Performance di Chrome](https://developer.chrome.com/docs/devtools/performance/).

## Performance API

Quando si scrive codice per il Web, sono disponibili molte [Web API](/it/docs/Web/API) per creare i tuoi strumenti di misurazione delle prestazioni.

Puoi utilizzare la [Navigation Timing API](/it/docs/Web/API/Performance_API/Navigation_timing) per misurare le prestazioni web lato client, compreso il tempo necessario per scaricare la pagina precedente, quanto tempo impiegano le ricerche di dominio, il tempo totale speso nell'esecuzione del gestore di caricamento della finestra, e molto altro. Puoi utilizzare l'API per metriche relative a tutti gli eventi di navigazione nel diagramma sottostante.

![I vari gestori che possono essere gestiti dalla API di temporizzazione di navigazione che comprendono le metriche API di temporizzazione di navigazione Prompt per scaricare reindirizzamento scaricare Cache App DNS TCP Richiesta Risposta Elaborazione onLoad navigationStart redirectStart redirectEnd fetchStart domainLookupEnd domainLookupStart connectStart (secureConnectionStart) connectEnd requestStart responseStart responseEnd unloadStart unloadEnd domLoading domInteractive domContentLoaded domComplete loadEventStart loadEventEnd](navigationtimingapi.jpg)

La [Performance API](/it/docs/Web/API/Performance_API), che fornisce l'accesso a informazioni relative alle prestazioni per la pagina corrente, include diverse API tra cui la [Navigation Timing API](/it/docs/Web/API/Performance_API/Navigation_timing), la [User Timing API](/it/docs/Web/API/Performance_API/User_timing) e la [Resource Timing API](/it/docs/Web/API/Performance_API/Resource_timing). Queste interfacce permettono la misurazione accurata del tempo necessario per completare i compiti JavaScript.

L'oggetto [PerformanceEntry](/it/docs/Web/API/PerformanceEntry) fa parte della _timeline delle prestazioni_. Un _entry di prestazione_ può essere creato direttamente facendo un _[`mark`](/it/docs/Web/API/PerformanceMark)_ o un _[`measure`](/it/docs/Web/API/PerformanceMeasure)_ di prestazione (ad esempio chiamando il metodo [`mark()`](/it/docs/Web/API/Performance/mark)) in un punto esplicito in un'applicazione. Le entry di prestazione vengono create anche in modi indiretti, come il caricamento di una risorsa come un'immagine.

La [PerformanceObserver API](/it/docs/Web/API/PerformanceObserver) può essere utilizzata per osservare gli eventi di misurazione delle prestazioni e per notificarti nuovi [performance entry](/it/docs/Web/API/PerformanceEntry) mentre vengono registrati nella timeline delle prestazioni del browser.

Sebbene questo articolo non entri nel dettaglio dell'uso di queste API, è utile sapere che esistono. Consulta l'articolo [Navigazione e tempistiche](/it/docs/Web/Performance/Guides/Navigation_and_resource_timings) per ulteriori esempi di utilizzo delle API Web di prestazione.

## Conclusione

Questo articolo offre una panoramica di alcuni strumenti che possono aiutarti a misurare le prestazioni su un'app web o sito. Nell'articolo successivo, vedremo come ottimizzare le immagini sul tuo sito per migliorarne le prestazioni.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance/Multimedia", "Learn_web_development/Extensions/Performance")}}
