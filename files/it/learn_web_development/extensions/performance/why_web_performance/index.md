---
title: Il "perché" delle prestazioni web
slug: Learn_web_development/Extensions/Performance/why_web_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance")}}

Le prestazioni web riguardano la velocità dei siti web, incluso come rendere i processi lenti _apparentemente_ veloci. Questo articolo fornisce un'introduzione al motivo per cui le prestazioni web sono importanti per i visitatori del sito e per gli obiettivi aziendali.

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
        Acquisire familiarità di base sul perché le prestazioni web siano importanti per una buona esperienza utente.
      </td>
    </tr>
  </tbody>
</table>

Le prestazioni web si riferiscono a quanto rapidamente il contenuto del sito **si carica** e **viene renderizzato** in un browser web, e a quanto bene risponde all'interazione dell'utente. I siti con cattive prestazioni sono lenti nel visualizzare e nel rispondere agli input. I siti con cattive prestazioni aumentano l'abbandono del sito. Nei casi peggiori, le cattive prestazioni rendono il contenuto completamente inaccessibile. Un buon obiettivo per le prestazioni web è che gli utenti non notino le prestazioni. Mentre la percezione individuale delle prestazioni del sito è soggettiva, il caricamento e il rendering possono essere misurati. Buone prestazioni possono non essere evidenti alla maggior parte dei visitatori del sito, ma quasi tutti riconosceranno immediatamente un sito lento. Ecco perché ci importa.

## Perché preoccuparsi delle prestazioni?

Le prestazioni web — e le relative migliori pratiche — sono fondamentali affinché i visitatori del tuo sito abbiano una buona esperienza. In un certo senso, le prestazioni web possono essere considerate un sottoinsieme della [accessibilità web](/it/docs/Learn_web_development/Core/Accessibility). Con le prestazioni, come con l'accessibilità, si considera quale dispositivo un visitatore del sito stia utilizzando per accedere al sito e la velocità di connessione del dispositivo.

Ad esempio, considera l'esperienza di caricamento di CNN.com, che al momento della stesura aveva oltre 400 richieste HTTP con una dimensione dei file di oltre 22.6MB.

- Immagina di caricarlo su un computer desktop collegato a una rete in fibra ottica. Sembrerebbe relativamente veloce, e la dimensione del file sarebbe in gran parte irrilevante.
- Immagina di caricare lo stesso sito usando dati mobili su un iPad di nove anni fa mentre si torna a casa sui mezzi pubblici. Lo stesso sito sarà lento da caricare, possibilmente al limite dell'inutilizzabile a seconda della copertura cellulare. Potresti rinunciare prima che finisca il caricamento.
- Immagina di caricare lo stesso sito su un dispositivo a basso costo in un'area con copertura limitata. Il sito sarà molto lento da caricare—se si carica—con script bloccanti che potrebbero andare in timeout e un impatto negativo sulla CPU che potrebbe causare arresti del browser se si carica.

Un sito di 22.6 MB potrebbe impiegare fino a 83 secondi per caricarsi su una rete 3G, con [`DOMContentLoaded`](/it/docs/Web/API/Document/DOMContentLoaded_event) (che significa la struttura base HTML del sito) a 31,86 secondi.

E non è solo il tempo impiegato per il download a rappresentare un problema significativo. In alcune regioni, le connessioni internet sono fatturate per megabyte, rendendo i download di grandi dimensioni proibitivamente costosi. Il nostro esempio di CNN.com da 22.6 MB consumerebbe una porzione significativa della diaria di un utente di dati mobili o porterebbe a costi elevati in alcuni piani di roaming internazionale.

### Migliorare i tassi di conversione

Ridurre il tempo di download e rendering di un sito migliora i tassi di conversione e la fidelizzazione degli utenti.

Il **tasso di conversione** è il tasso con cui i visitatori del sito eseguono un'azione misurata o desiderata. Ad esempio, potrebbe essere effettuare un acquisto, leggere un articolo, o iscriversi a una newsletter. L'azione misurata come tasso di conversione dipende dagli obiettivi aziendali del sito web.

Le prestazioni influenzano la conversione; migliorare le prestazioni web migliora la conversione. I visitatori del sito si aspettano che un sito si carichi in due secondi o meno; a volte anche meno su mobile (dove generalmente impiega più tempo). Questi stessi visitatori del sito iniziano ad abbandonare i siti lenti a 3 secondi.

La velocità con cui un sito si carica è un fattore. Se il sito è lento a reagire all'interazione dell'utente, o appare scoordinate, questo porta i visitatori del sito a perdere interesse e fiducia.

Ecco alcuni esempi reali di miglioramenti delle prestazioni:

- [Tokopedia ha ridotto il tempo di rendering da 14s a 2s per le connessioni 3G e ha visto un aumento del 19% dei visitatori, un aumento del 35% delle sessioni totali, un 7% di aumento dei nuovi utenti, un 17% di aumento degli utenti attivi e un 16% di aumento delle sessioni per utente.](https://wpostats.com/2018/05/30/tokopedia-new-users.html)
- [La ricostruzione delle pagine di Pinterest per migliorare le prestazioni ha portato a una riduzione del tempo di attesa del 40%, un aumento del 15% del traffico SEO e un aumento del 15% del tasso di conversione in iscrizioni.](https://wpostats.com/2017/03/10/pinterest-seo.html)

Per costruire siti web e applicazioni che le persone vogliono utilizzare, per attrarre e trattenere i visitatori del sito, è necessario creare un sito accessibile che offra una buona esperienza utente. Costruire siti web richiede HTML, CSS e JavaScript, tipicamente includendo tipi di file binari come immagini e video. Le decisioni che prendi e gli strumenti che scegli durante la creazione del tuo sito possono influenzare notevolmente le prestazioni del lavoro finito.

Buone prestazioni sono un vantaggio. Cattive prestazioni sono una passività. La velocità del sito influisce direttamente sui tassi di rimbalzo, conversione, entrate, soddisfazione dell'utente e posizionamento nei motori di ricerca. I siti performanti si sono dimostrati in grado di aumentare la fidelizzazione dei visitatori e la soddisfazione degli utenti. Contenuti lenti hanno portato all'abbandono del sito, con alcuni visitatori che se ne vanno per non tornare mai più. Ridurre la quantità di dati che passa tra il client e il server riduce i costi per tutte le parti. Ridurre le dimensioni dei file HTML/CSS/JavaScript e dei media riduce sia il tempo di caricamento sia il consumo energetico di un sito (vedi [budget delle prestazioni](/it/docs/Web/Performance/Guides/Performance_budgets)).

Monitorare le prestazioni è importante. Molteplici fattori, tra cui la velocità della rete e le capacità del dispositivo, influenzano le prestazioni. Non esiste un unico metrica di prestazione; e diversi obiettivi aziendali possono significare che metriche diverse siano più rilevanti per gli obiettivi del sito o dell'organizzazione che supporta. La percezione delle prestazioni del tuo sito è l'esperienza utente!

## Conclusione

Le prestazioni web sono importanti per l'accessibilità e anche per altre metriche del sito web che servono gli obiettivi di un'organizzazione o azienda. Le prestazioni del sito web, buone o cattive, correlano in modo potente con l'esperienza utente, oltre che con l'efficacia complessiva della maggior parte dei siti. Ecco perché dovresti preoccuparti delle prestazioni web.

{{NextMenu("Learn_web_development/Extensions/Performance/What_is_web_performance", "Learn_web_development/Extensions/Performance")}}
