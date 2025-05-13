---
title: Che cos'è la performance web?
slug: Learn_web_development/Extensions/Performance/What_is_web_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/why_web_performance", "Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance")}}

La performance web riguarda rendere i siti web veloci, incluso far sembrare veloci i processi lenti. Il sito si carica rapidamente, permette all'utente di iniziare a interagire velocemente e offre feedback rassicuranti se qualcosa sta impiegando tempo a caricarsi (ad esempio, un indicatore di caricamento)? Lo scrolling e le animazioni sono fluide? Questo articolo fornisce una breve introduzione alla performance web oggettiva e misurabile, esaminando quali tecnologie, tecniche e strumenti sono coinvolti nell'ottimizzazione web.

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
        Ottenere una conoscenza di base di ciò che è coinvolto nella performance web.
      </td>
    </tr>
  </tbody>
</table>

_\* versus la [perceived performance](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance), trattata nel prossimo articolo_

## Che cos'è la performance web?

La performance web è la misurazione oggettiva e l'esperienza utente percepita di un sito web o applicazione. Questo include le seguenti aree principali:

- **Riduzione del tempo di caricamento complessivo**: Quanto tempo ci vuole per scaricare sul computer dell'utente i file necessari per rendere il sito web? Questo tende a essere influenzato dalla [latenza](/it/docs/Web/Performance/Guides/Understanding_latency), dalla dimensione dei file, dalla quantità di file presenti e altri fattori. Una strategia generale è rendere i file il più piccoli possibile, ridurre il numero di richieste HTTP quanto più possibile, e utilizzare tecniche di caricamento intelligenti (come [preload](/it/docs/Web/HTML/Reference/Attributes/rel/preload)) per rendere disponibili i file prima.
- **Rendere il sito utilizzabile il prima possibile**: Questo significa essenzialmente caricare le risorse del sito web in un ordine sensato in modo che l'utente possa iniziare a usarlo velocemente. Le altre risorse possono continuare a caricarsi in background mentre l'utente svolge i compiti principali, e a volte carichiamo le risorse solo quando sono effettivamente necessarie (questo si chiama [lazy loading](/it/docs/Web/Performance/Guides/Lazy_loading)). La misurazione di quanto tempo impiega il sito a diventare utilizzabile dopo che ha iniziato a caricarsi è chiamata {{Glossary("Time_to_interactive", "time to interactive")}}.
- **Fluidità e interattività**: L'applicazione sembra affidabile e piacevole da usare? Lo scrolling è fluido? I pulsanti sono cliccabili? I popup si aprono rapidamente e si animano fluidamente mentre lo fanno? Ci sono molte best practice da considerare per rendere le applicazioni fluide, ad esempio utilizzare animazioni CSS piuttosto che JavaScript per l'animazione, e minimizzare il numero di ridisegni necessari dell'interfaccia utente a causa delle modifiche al DOM.
- **[Perceived performance](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance)**: La velocità con cui un sito web sembra all'utente ha un impatto maggiore sull'esperienza utente rispetto a quanto effettivamente veloce sia il sito web. Come un utente percepisce la sua performance è importante quanto, o forse più importante, di qualsiasi statistica oggettiva, ma è soggettiva e non facilmente misurabile. La perceived performance è una prospettiva dell'utente, non una metrica. Anche se un'operazione impiegherà molto tempo (a causa della latenza o altro), è possibile mantenere l'utente coinvolto mentre attende mostrando un indicatore di caricamento, o una serie di suggerimenti utili e consigli (o battute, o qualsiasi altra cosa lei pensi possa essere appropriata). Un approccio del genere è molto meglio che non mostrare nulla, il che farà sembrare che stia impiegando molto più tempo e potrebbe portare gli utenti a pensare che sia rotto e abbandonare.
- **[Misurazioni delle performance](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance)**: La performance web comprende la misurazione delle velocità reale e percepita di un'applicazione, l'ottimizzazione dove possibile, e poi il monitoraggio della performance, per garantire che ciò che è stato ottimizzato rimanga ottimizzato. Questo comporta una serie di metriche (indicatori misurabili che possono indicare successo o fallimento) e strumenti per misurare queste metriche, di cui parleremo durante questo modulo.

Per riassumere, molte caratteristiche influenzano le performance, tra cui la latenza, la dimensione dell'applicazione, il numero di nodi DOM, il numero di richieste di risorse effettuate, la performance di JavaScript, il carico della CPU e altro ancora. È importante minimizzare i tempi di caricamento e risposta, e aggiungere funzionalità aggiuntive per nascondere la latenza rendendo l'esperienza il più disponibile e interattiva possibile, il prima possibile, mentre le parti più lunghe dell'esperienza vengono caricate in modo asincrono.

> [!NOTE]
> La performance web include sia misurazioni oggettive come il tempo di caricamento, frame per secondo, e {{Glossary("Time_to_interactive", "time to interactive")}}, sia esperienze soggettive di quanto tempo realmente sembra essere trascorso per caricare il contenuto.

## Come viene reso il contenuto

Per capire efficacemente la performance web, i problemi che la influenzano e le grandi aree tematiche che abbiamo menzionato sopra, dovrebbe davvero capire alcune specifiche su come funzionano i browser. Questo include:

- **Come funziona il browser**. Quando richiede un URL e preme <kbd>Enter</kbd> / <kbd>Return</kbd>, il browser scopre dove si trova il server che ospita i file del sito web, stabilisce una connessione con esso e richiede i file. Veda [Populating the page: how the browser works](/it/docs/Web/Performance/Guides/How_browsers_work) per una panoramica dettagliata.
- **Ordine delle sorgenti**. L'ordine delle sorgenti del file HTML index può influire significativamente sulla performance. Il download delle risorse aggiuntive collegate al file index è generalmente sequenziale, basato sull'ordine delle sorgenti, ma questo può essere manipolato e dovrebbe essere ottimizzato, rendendosi conto che alcune risorse bloccano ulteriori download fino a quando il loro contenuto non è analizzato ed eseguito.
- **Il path critico**. Questo è il processo che il browser utilizza per costruire il documento web una volta che i file sono stati scaricati dal server. Il browser segue una serie ben definita di passi, e ottimizzare il critical rendering path per dare priorità alla visualizzazione del contenuto relativo all'azione utente corrente porterà a significativi miglioramenti nei tempi di rendering del contenuto. Veda [Critical rendering path](/it/docs/Web/Performance/Guides/Critical_rendering_path) per ulteriori informazioni.
- Il **document object model**. Il document object model, o DOM, è una struttura ad albero che rappresenta il contenuto e gli elementi del suo HTML come un albero di nodi. Questo include tutti gli attributi HTML e le relazioni tra i nodi. La manipolazione estensiva del DOM dopo che le pagine sono state caricate (ad esempio, aggiunta, eliminazione o spostamento di nodi) può influire sulle performance, quindi vale la pena capire come funziona il DOM e come questi problemi possono essere mitigati. Scopra di più presso [Document Object Model](/it/docs/Web/API/Document_Object_Model).
- **Latenza**. Abbiamo accennato brevemente questo in precedenza, ma in breve, la latenza è il tempo impiegato affinché le risorse del suo sito web viaggino dal server al computer di un utente. C'è un overhead coinvolto nello stabilire connessioni TCP e HTTP, e una latenza inevitabile nel trasferimento dei byte di richiesta e risposta attraverso la rete, ma ci sono modi per ridurre la latenza (ad esempio, ridurre il numero di richieste HTTP scaricando meno file, usare un {{Glossary("CDN", "CDN")}} per rendere il suo sito più performante a livello globale, e usare HTTP/2 per servire i file in modo più efficiente dal server). Può leggere tutto su questo argomento in [Understanding Latency](/it/docs/Web/Performance/Guides/Understanding_latency).

## Conclusione

Questo è tutto per ora; speriamo che la nostra breve panoramica sul tema della performance web l'abbia aiutata a farsi un'idea di cosa si tratti e l'abbia resa entusiasta di imparare di più. Nel prossimo articolo esamineremo la perceived performance e come può utilizzare alcune tecniche intelligenti per far apparire alcuni inevitabili colpi alle performance meno severi per l'utente, o nasconderli completamente.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/why_web_performance", "Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance")}}
