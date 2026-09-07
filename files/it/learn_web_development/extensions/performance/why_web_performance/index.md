---
title: Il "perché" delle prestazioni web
slug: Learn_web_development/Extensions/Performance/why_web_performance
l10n:
  sourceCommit: 87adaa5384b1015690f3435ce0ba64ac097764eb
---

{{NextMenu("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance")}}

Le prestazioni web consistono nel rendere i siti web veloci, incluso far _sembrare_ veloci i processi lenti. Questo articolo fornisce un'introduzione al motivo per cui le prestazioni web sono importanti per i visitatori di un sito e per gli obiettivi aziendali.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Installing_software"
          >Software di base installato</a
        > e conoscenza di base delle
        <a href="/it/docs/Learn_web_development/Getting_started/Your_first_website"
          >tecnologie web lato client</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire familiarità di base con il motivo per cui le prestazioni web sono importanti per una buona
        esperienza utente.
      </td>
    </tr>
  </tbody>
</table>

Le prestazioni web si riferiscono alla rapidità con cui il contenuto di un sito viene **caricato** ed **eseguito il rendering** in un browser web e alla qualità della sua risposta all'interazione dell'utente. I siti con prestazioni scarse visualizzano lentamente i contenuti e rispondono lentamente agli input. I siti con prestazioni scarse aumentano l'abbandono del sito. Nel caso peggiore, le scarse prestazioni rendono il contenuto completamente inaccessibile. Un buon obiettivo per le prestazioni web è fare in modo che gli utenti non notino le prestazioni. Sebbene la percezione delle prestazioni di un sito da parte del singolo sia soggettiva, il caricamento e il rendering possono essere misurati. Le buone prestazioni potrebbero non essere evidenti alla maggior parte dei visitatori del sito, ma quasi tutti riconoscono immediatamente un sito lento. Ecco perché sono importanti.

## Perché interessarsi alle prestazioni?

Le prestazioni web — e le best practice associate — sono fondamentali affinché i visitatori del sito abbiano una buona esperienza. In un certo senso, le prestazioni web possono essere considerate un sottoinsieme dell'[accessibilità web](/it/docs/Learn_web_development/Core/Accessibility). Per le prestazioni come per l'accessibilità, occorre considerare il dispositivo usato dal visitatore per accedere al sito e la velocità di connessione del dispositivo.

Come esempio, si consideri l'esperienza di caricamento di CNN.com, che al momento della stesura di questo testo effettuava oltre 400 richieste HTTP con una dimensione dei file superiore a 22,6 MB.

- Si immagini di caricarlo su un computer desktop connesso a una rete in fibra ottica. Sembrerebbe relativamente veloce e la dimensione dei file sarebbe in gran parte irrilevante.
- Si immagini di caricare lo stesso sito usando dati mobili in tethering su un iPad di nove anni durante il tragitto verso casa con i mezzi pubblici. Lo stesso sito sarà lento da caricare, e potrebbe risultare quasi inutilizzabile a seconda della copertura cellulare. Potrebbe essere abbandonato prima del completamento del caricamento.
- Si immagini di caricare lo stesso sito su un dispositivo economico in un'area con copertura limitata. Il sito sarà molto lento da caricare — se verrà caricato — con possibili timeout degli script bloccanti e un impatto negativo sulla CPU che potrebbe causare il crash del browser anche se il caricamento riesce.

Un sito di 22,6 MB potrebbe richiedere fino a 83 secondi per il caricamento su una rete 3G, con [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event) (ovvero la struttura HTML di base del sito) a 31,86 secondi.

E il tempo necessario per il download non è l'unico problema principale. In alcune regioni, le connessioni internet sono fatturate per megabyte, rendendo i download di grandi dimensioni proibitivamente costosi. L'esperienza di CNN.com da 22,6 MB del nostro esempio consumerebbe una parte significativa della quota giornaliera di un utente di dati mobili o porterebbe persino a costi elevati in determinati piani di roaming internazionale.

### Migliorare i tassi di conversione

Ridurre i tempi di download e rendering di un sito migliora i tassi di conversione e la fidelizzazione degli utenti.

Un **tasso di conversione** è la percentuale con cui i visitatori del sito eseguono un'azione misurata o desiderata. Per esempio, potrebbe trattarsi di effettuare un acquisto, leggere un articolo o iscriversi a una newsletter. L'azione misurata come tasso di conversione dipende dagli obiettivi aziendali del sito web.

Le prestazioni incidono sulla conversione; migliorare le prestazioni web migliora la conversione. I visitatori si aspettano che un sito venga caricato in due secondi o meno; talvolta anche meno sui dispositivi mobili, dove in genere il caricamento richiede più tempo. Gli stessi visitatori iniziano ad abbandonare i siti lenti dopo 3 secondi.

La velocità di caricamento di un sito è un fattore. Se il sito reagisce lentamente all'interazione dell'utente o appare scattoso, i visitatori perdono interesse e fiducia.

Ecco alcuni esempi reali di miglioramenti delle prestazioni:

- [Tokopedia ha ridotto il tempo di rendering da 14 s a 2 s per le connessioni 3G e ha registrato un aumento del 19% dei visitatori, del 35% delle sessioni totali, del 7% dei nuovi utenti, del 17% degli utenti attivi e del 16% delle sessioni per utente.](https://wpostats.com/2018/05/30/tokopedia-new-users.html)
- [La ricostruzione delle pagine di Pinterest in ottica di prestazioni ha comportato una riduzione del 40% del tempo di attesa, un aumento del 15% del traffico SEO e un aumento del 15% del tasso di conversione per la registrazione.](https://wpostats.com/2017/03/10/pinterest-seo.html)

Per creare siti web e applicazioni che le persone desiderano usare, e per attirare e trattenere i visitatori del sito, è necessario creare un sito accessibile che offra una buona esperienza utente. La creazione di siti web richiede HTML, CSS e JavaScript, includendo in genere tipi di file binari come immagini e video. Le decisioni prese e gli strumenti scelti durante la creazione del sito possono influire notevolmente sulle prestazioni del risultato finale.

Le buone prestazioni sono una risorsa. Le scarse prestazioni sono una passività. La velocità del sito influisce direttamente sui tassi di rimbalzo, sulle conversioni, sui ricavi, sulla soddisfazione degli utenti e sul posizionamento nei motori di ricerca. È stato dimostrato che i siti dalle buone prestazioni aumentano la fidelizzazione dei visitatori e la soddisfazione degli utenti. È stato dimostrato che i contenuti lenti portano all'abbandono del sito, con alcuni visitatori che se ne vanno per non tornare mai più. Ridurre la quantità di dati trasferiti tra client e server riduce i costi per tutte le parti. Ridurre le dimensioni dei file HTML/CSS/JavaScript e dei file multimediali riduce sia il tempo di caricamento sia il consumo energetico del sito (vedere i [budget delle prestazioni](/it/docs/Web/Performance/Guides/Performance_budgets)).

Monitorare le prestazioni è importante. Molteplici fattori, tra cui la velocità della rete e le capacità del dispositivo, influiscono sulle prestazioni. Non esiste una singola metrica delle prestazioni; inoltre, obiettivi aziendali diversi possono rendere metriche diverse più rilevanti per gli obiettivi del sito o dell'organizzazione che supporta. Il modo in cui vengono percepite le prestazioni del sito è esperienza utente!

## Conclusione

Le prestazioni web sono importanti per l'accessibilità e anche per altre metriche del sito web che servono gli obiettivi di un'organizzazione o di un'azienda. Le buone o cattive prestazioni di un sito web sono fortemente correlate all'esperienza utente, oltre che all'efficacia complessiva della maggior parte dei siti. Ecco perché è importante interessarsi alle prestazioni web.

{{NextMenu("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance")}}
