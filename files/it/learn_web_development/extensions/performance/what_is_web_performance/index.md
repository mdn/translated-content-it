---
title: Che cos'è la performance web?
slug: Learn_web_development/Extensions/Performance/What_is_web_performance
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/why_web_performance", "Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance")}}

La performance web riguarda la velocizzazione dei siti web, incluso far sembrare i processi lenti come _veloci_. Il sito si carica rapidamente, permette all'utente di iniziare a interagire con esso velocemente e offre un feedback rassicurante se qualcosa richiede tempo per caricarsi (ad esempio, un indicatore di caricamento)? Lo scorrimento e le animazioni sono fluidi? Questo articolo fornisce una breve introduzione alla performance web oggettiva e misurabile\*, esaminando le tecnologie, le tecniche e gli strumenti coinvolti nell'ottimizzazione web.

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
        Acquisire familiarità di base con ciò che è coinvolto nella performance web.
      </td>
    </tr>
  </tbody>
</table>

_\* rispetto a quella soggettiva, [performance percepita](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance), trattata nel prossimo articolo_

## Che cos'è la performance web?

La performance web è la misurazione oggettiva e l'esperienza utente percepita di un sito web o applicazione. Include le seguenti aree principali:

- **Riduzione del tempo di caricamento complessivo**: Quanto tempo impiegano i file necessari per renderizzare il sito web a scaricarsi sul computer dell'utente? Questo tende a essere influenzato dalla [latenza](/it/docs/Web/Performance/Guides/Understanding_latency), da quanto sono grandi i file, da quanti file ci sono, e da altri fattori. Una strategia generale è rendere i file il più piccoli possibile, ridurre il numero di richieste HTTP effettuate quanto più possibile, e impiegare tecniche di caricamento intelligenti (come il [preload](/it/docs/Web/HTML/Reference/Attributes/rel/preload)) per rendere i file disponibili più rapidamente.
- **Rendere il sito utilizzabile il prima possibile**: Questo significa essenzialmente caricare le risorse del sito in un ordine sensato in modo che l'utente possa iniziare a usarlo rapidamente. Qualsiasi altra risorsa può continuare a caricarsi in background mentre l'utente si occupa dei compiti principali, e a volte carichiamo le risorse solo quando sono effettivamente necessarie (questo è chiamato [lazy loading](/it/docs/Web/Performance/Guides/Lazy_loading)). La misurazione di quanto tempo ci vuole affinché il sito diventi usabile dopo che ha iniziato a caricarsi è chiamata {{Glossary("Time_to_interactive", "time to interactive")}}.
- **Fluidità e interattività**: L'applicazione sembra affidabile e piacevole da usare? Lo scorrimento è fluido? I pulsanti sono cliccabili? I popup si aprono rapidamente e si animano in modo fluido mentre lo fanno? Esistono molte buone pratiche da considerare per rendere le app fluide, ad esempio utilizzare le animazioni CSS piuttosto che JavaScript per l'animazione, e minimizzare il numero di ridipinture richieste dalla UI a causa dei cambiamenti nel DOM.
- **[Performance percepita](/it/docs/Learn_web_development/Extensions/Performance/Perceived_performance)**: La velocità con cui un sito web sembra all'utente ha un impatto maggiore sull'esperienza utente rispetto a quanto velocemente il sito è effettivamente. Come un utente percepisce le tue performance è importante quanto, o forse più importante di, qualsiasi statistica oggettiva, ma è soggettiva e non facilmente misurabile. La performance percepita è una prospettiva dell'utente, non una metrica. Anche se un'operazione richiederà molto tempo (a causa della latenza o altro), è possibile mantenere l'utente coinvolto mentre aspetta mostrando un indicatore di caricamento, o una serie di suggerimenti e consigli utili (o battute, o qualsiasi altra cosa tu ritenga appropriata). Un tale approccio è molto migliore rispetto a non mostrare nulla, il che farà sembrare che stia impiegando molto più tempo e potrebbe portare gli utenti a pensare che sia rotto e a rinunciare.
- **[Misurazioni delle performance](/it/docs/Learn_web_development/Extensions/Performance/Measuring_performance)**: Le performance web implicano misurare le velocità effettive e percepite di un'applicazione, ottimizzare dove possibile, e quindi monitorare le performance, per assicurarsi che ciò che hai ottimizzato resti ottimizzato. Questo coinvolge un certo numero di metriche (indicatori misurabili che possono indicare il successo o il fallimento) e strumenti per misurare queste metriche, che discuteremo in tutto questo modulo.

In sintesi, molte caratteristiche influenzano la performance inclusa la latenza, la dimensione dell'applicazione, il numero di nodi DOM, il numero di richieste di risorse effettuate, le performance di JavaScript, il carico della CPU, e altro ancora. È importante minimizzare i tempi di caricamento e risposta, e aggiungere ulteriori caratteristiche per coprire la latenza rendendo l'esperienza il più disponibile e interattiva possibile, il prima possibile, mentre si caricano in modo asincrono le parti dell'esperienza più lunghe da caricare.

> [!NOTE]
> Le performance web includono sia misurazioni oggettive come il tempo di caricamento, i frame al secondo, e il {{Glossary("Time_to_interactive", "tempo all'interattività")}}, sia esperienze soggettive di quanto ci sia voluto per caricare il contenuto.

## Come viene renderizzato il contenuto

Per comprendere efficacemente le performance web, le problematiche dietro di esse, e le principali aree tematiche citate sopra, è utile comprendere alcuni dettagli su come funzionano i browser. Questo include:

- **Come funziona il browser**. Quando richiedi un URL e premi <kbd>Enter</kbd> / <kbd>Return</kbd>, il browser scopre dov'è il server che ospita i file del sito web, stabilisce una connessione con esso, e richiede i file. Consulta [Populating the page: how the browser works](/it/docs/Web/Performance/Guides/How_browsers_work) per una panoramica dettagliata.
- **Ordine delle sorgenti**. L'ordine delle sorgenti del tuo file indice HTML può influenzare significativamente le performance. Il download delle risorse aggiuntive collegate al file indice è generalmente sequenziale, basato sull'ordine delle sorgenti, ma questo può essere manipolato e dovrebbe essere ottimizzato, riconoscendo che alcune risorse bloccano download aggiuntivi finché il loro contenuto non viene analizzato ed eseguito.
- **Il percorso critico**. Questo è il processo che il browser utilizza per costruire il documento web una volta che i file sono stati scaricati dal server. Il browser segue una serie di passaggi ben definita, e ottimizzare il percorso di rendering critico per dare priorità alla visualizzazione del contenuto che si riferisce all'azione corrente dell'utente porterà a significativi miglioramenti nei tempi di rendering del contenuto. Consulta [Critical rendering path](/it/docs/Web/Performance/Guides/Critical_rendering_path) per ulteriori informazioni.
- Il **document object model**. Il document object model, o DOM, è una struttura ad albero che rappresenta il contenuto e gli elementi del tuo HTML come un albero di nodi. Ciò include tutti gli attributi HTML e le relazioni tra i nodi. Una vasta manipolazione DOM dopo che le pagine sono state caricate (ad esempio, aggiunta, eliminazione o spostamento di nodi) può influire sulle performance, quindi vale la pena capire come funziona il DOM, e come si possono mitigare tali problemi. Scopri di più su [Document Object Model](/it/docs/Web/API/Document_Object_Model).
- **Latenza**. Accennato brevemente in precedenza, in breve, la latenza è il tempo necessario per i tuoi asset del sito web per viaggiare dal server al computer di un utente. C'è un sovraccarico coinvolto nell'instaurare connessioni TCP e HTTP, e una certa latenza inevitabile nel spingere i byte di richiesta e risposta avanti e indietro attraverso la rete, ma ci sono certi modi per ridurre la latenza (ad esempio, riducendo il numero di richieste HTTP che fai scaricando meno file, usando un {{Glossary("CDN", "CDN")}} per rendere il tuo sito più performante universalmente in tutto il mondo, e usando HTTP/2 per servire i file più efficientemente dal server). Puoi leggere tutto su questo argomento in [Understanding Latency](/it/docs/Web/Performance/Guides/Understanding_latency).

## Conclusione

Ecco tutto per ora; speriamo che la nostra breve panoramica sull'argomento delle performance web ti abbia aiutato a farti un'idea di cosa riguarda e ti abbia reso entusiasta di imparare di più. Prossimamente esamineremo la performance percepita e come si possano utilizzare alcune tecniche intelligenti per far sembrare meno gravi agli occhi dell'utente alcuni inevitabili colpi di performance o per mascherarli completamente.

{{PreviousMenuNext("Learn_web_development/Extensions/Performance/why_web_performance", "Learn_web_development/Extensions/Performance/Perceived_performance", "Learn_web_development/Extensions/Performance")}}
