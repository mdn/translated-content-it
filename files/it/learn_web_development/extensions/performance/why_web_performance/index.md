---
title: Il "perché" delle prestazioni web
slug: Learn_web_development/Extensions/Performance/why_web_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance")}}

Le prestazioni web riguardano la velocità dei siti, inclusa la capacità di far sembrare veloci i processi lenti. Questo articolo offre un'introduzione al motivo per cui le prestazioni web sono importanti per i visitatori del sito e per gli obiettivi aziendali.

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
        Ottenere una familiarità di base sul perché le prestazioni web sono importanti per una buona esperienza utente.
      </td>
    </tr>
  </tbody>
</table>

Le prestazioni web si riferiscono a quanto rapidamente i contenuti di un sito vengono **caricati** e **renderizzati** in un browser, e a quanto bene rispondono all'interazione dell'utente. I siti con cattive prestazioni sono lenti a essere visualizzati e a rispondere agli input. I siti con cattive prestazioni aumentano l'abbandono del sito. Nel peggiore dei casi, la cattiva prestazione rende il contenuto completamente inaccessibile. Un buon obiettivo per le prestazioni web è che gli utenti non notino la prestazione. Sebbene la percezione delle prestazioni di un sito da parte di un individuo sia soggettiva, il caricamento e il rendering possono essere misurati. Una buona prestazione potrebbe non essere ovvia per la maggior parte dei visitatori del sito, ma la maggior parte riconoscerà immediatamente un sito lento. Ecco perché ci interessa.

## Perché interessarsi alle prestazioni?

Le prestazioni web — e le pratiche migliori associate — sono essenziali affinché i visitatori del suo sito web abbiano una buona esperienza. In un certo senso, le prestazioni web si possono considerare un sottoinsieme dell'[accessibilità web](/it/docs/Learn_web_development/Core/Accessibility). Con le prestazioni, come con l'accessibilità, si considera quale dispositivo utilizza un visitatore del sito per accedere al sito e la velocità della connessione del dispositivo.

Per esempio, consideri l'esperienza di caricamento di CNN.com, che al momento della scrittura aveva oltre 400 richieste HTTP con una dimensione di file superiore a 22,6MB.

- Immagini di caricarlo su un computer desktop collegato a una rete in fibra ottica. Sembrerebbe relativamente veloce e la dimensione del file sarebbe in gran parte irrilevante.
- Immagini di caricare lo stesso sito utilizzando dati mobili collegati a un iPad di nove anni mentre torna a casa con i mezzi pubblici. Lo stesso sito sarà lento nel caricamento, possibilmente vicino a risultare inutilizzabile a seconda della copertura cellulare. Potrebbe rinunciare prima che termini il caricamento.
- Immagini di caricare lo stesso sito su un dispositivo a basso costo in un'area con copertura limitata. Il sito sarà molto lento nel caricamento—se riesce a caricarsi del tutto—con script bloccanti che potrebbero scadere e un impatto avverso sulla CPU che potrebbe causare crash del browser se riesce a caricarsi.

Un sito di 22,6 MB potrebbe impiegare fino a 83 secondi per caricarsi su una rete 3G, con [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event) (che significa la struttura base HTML del sito) a 31,86 secondi.

E non è solo il tempo impiegato per il download a rappresentare un problema importante. In alcune regioni, le connessioni internet sono addebitate per megabyte, rendendo proibitivi i download di grandi dimensioni. La nostra esperienza di 22,6 MB su CNN.com costerebbe una parte significativa della quota giornaliera di dati mobili di un utente o addirittura comportare costi elevati in certi piani di roaming internazionale.

### Migliorare i tassi di conversione

Ridurre il tempo di download e di rendering di un sito migliora i tassi di conversione e la retention degli utenti.

Un **tasso di conversione** è il tasso con cui i visitatori del sito compiono un'azione misurata o desiderata. Per esempio, potrebbe trattarsi di effettuare un acquisto, leggere un articolo o iscriversi a una newsletter. L'azione misurata come tasso di conversione dipende dagli obiettivi aziendali del sito.

Le prestazioni incidono sulla conversione; migliorare le prestazioni web migliora la conversione. I visitatori del sito si aspettano che un sito si carichi in due secondi o meno; a volte anche meno su mobile (dove generalmente ci vuole più tempo). Gli stessi visitatori del sito iniziano ad abbandonare i siti lenti dopo 3 secondi.

La velocità con cui un sito si carica è un fattore. Se il sito è lento a reagire all'interazione dell'utente, o appare instabile, questo porta i visitatori del sito a perdere interesse e fiducia.

Ecco alcuni esempi reali di miglioramenti delle prestazioni:

- [Tokopedia ha ridotto il tempo di rendering da 14s a 2s per le connessioni 3G e ha registrato un aumento del 19% dei visitatori, del 35% del numero totale di sessioni, del 7% di nuovi utenti, del 17% di utenti attivi e del 16% di sessioni per utente.](https://wpostats.com/2018/05/30/tokopedia-new-users.html)
- [Ricostruire le pagine di Pinterest per migliorare le prestazioni ha comportato una diminuzione del tempo di attesa del 40%, un aumento del 15% del traffico SEO e un aumento del 15% del tasso di conversione per le iscrizioni.](https://wpostats.com/2017/03/10/pinterest-seo.html)

Per costruire siti web e applicazioni che le persone vogliono usare; per attrarre e mantenere i visitatori del sito, è necessario creare un sito accessibile che offra una buona esperienza utente. La costruzione di siti web richiede HTML, CSS e JavaScript, includendo tipicamente file binari come immagini e video. Le decisioni che si prendono e gli strumenti che si scelgono mentre si costruisce il sito possono influire notevolmente sulle prestazioni del lavoro finito.

Buone prestazioni sono un asset. Cattive prestazioni sono una passività. La velocità del sito influisce direttamente sui tassi di abbandono, conversione, ricavi, soddisfazione degli utenti e classifica nei motori di ricerca. È stato dimostrato che i siti performanti aumentano la retention dei visitatori e la soddisfazione degli utenti. I contenuti lenti hanno dimostrato di portare all'abbandono del sito, con alcuni visitatori che se ne vanno per non tornare mai più. Ridurre la quantità di dati che passa tra il server e il client riduce i costi per tutte le parti. Ridurre le dimensioni dei file HTML/CSS/JavaScript e dei file multimediali riduce sia il tempo di caricamento sia il consumo di energia di un sito (vedi [performance budgets](/it/docs/Web/Performance/Guides/Performance_budgets)).

Monitorare le prestazioni è importante. Molteplici fattori, tra cui la velocità della rete e le capacità del dispositivo, influiscono sulle prestazioni. Non esiste un unico metro di prestazione; e diversi obiettivi aziendali potrebbero richiedere metriche diverse che sono più rilevanti per gli obiettivi del sito o dell'organizzazione che supporta. Come viene percepita la prestazione del suo sito è l'esperienza utente!

## Conclusione

Le prestazioni web sono importanti per l'accessibilità e anche per altre metriche del sito web che servono gli obiettivi di un'organizzazione o azienda. Le prestazioni buone o cattive di un sito web hanno una correlazione potente con l'esperienza utente, oltre che con l'efficacia complessiva della maggior parte dei siti. Questo è il motivo per cui dovrebbe preoccuparsi delle prestazioni web.

{{NextMenu("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance")}}
