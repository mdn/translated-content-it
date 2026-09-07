---
title: Introduzione al testing cross-browser
short-title: Introduction
slug: Learn_web_development/Extensions/Testing/Introduction
l10n:
  sourceCommit: 6030ef1aadf967b80e2c79c3d3463cccc8ea0c95
---

{{NextMenu("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing")}}

Questo articolo fornisce una panoramica del testing cross-browser: che cos'è, alcuni problemi comuni e alcuni approcci per il debug/la risoluzione dei problemi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire una comprensione dei concetti generali coinvolti nel testing cross-browser.
      </td>
    </tr>
  </tbody>
</table>

## Che cos'è il testing cross-browser?

Il testing cross-browser è la pratica di assicurarsi che un sito web funzioni su vari browser e dispositivi. Gli sviluppatori web dovrebbero considerare:

- Browser diversi, compresi quelli leggermente più vecchi che non supportano tutte le più recenti funzionalità JS/CSS.
- Dispositivi diversi, dai computer desktop e portatili ai tablet e smartphone, fino alle smart TV, con capacità hardware variabili.
- Persone con disabilità, che potrebbero fare affidamento su tecnologie assistive come gli screen reader oppure utilizzare soltanto una tastiera.

È importante ricordare che gli sviluppatori non coincidono con i propri utenti: solo perché un sito funziona su un MacBook Pro o su un Galaxy Nexus di fascia alta, non significa che funzionerà per tutti gli utenti.

> [!NOTE]
> [Make the web work for everyone](https://hacks.mozilla.org/2016/07/make-the-web-work-for-everyone/) analizza i diversi browser, la loro quota di mercato e i relativi problemi di compatibilità cross-browser.

I siti web dovrebbero essere accessibili da browser e dispositivi diversi e dalle persone con disabilità (ad esempio, essere adatti agli screen reader). Un sito non deve necessariamente offrire esattamente la stessa esperienza su tutti i browser e dispositivi, purché le funzionalità principali siano in qualche modo accessibili. Ad esempio, un browser moderno potrebbe mostrare qualcosa di animato, tridimensionale e brillante, mentre i browser più vecchi potrebbero limitarsi a mostrare un'immagine piatta con le stesse informazioni.

Inoltre, è praticamente impossibile fare in modo che un sito web funzioni su TUTTI i browser e dispositivi, quindi uno sviluppatore web dovrebbe concordare con il proprietario del sito l'intervallo di browser e dispositivi sui quali il codice funzionerà.

## Perché si verificano problemi cross-browser?

Esistono molte ragioni diverse per cui si verificano problemi cross-browser; qui si parla di problemi in cui elementi diversi si comportano in modo differente a seconda del browser, del dispositivo o delle preferenze di navigazione. Prima ancora di affrontare i problemi cross-browser, dovrebbero essere già stati corretti i bug nel codice (vedere [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML), [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS) e [What went wrong? Troubleshooting JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong) negli argomenti precedenti, se necessario, per rinfrescare la memoria).

I problemi cross-browser si verificano comunemente perché:

- talvolta i browser presentano bug o implementano le funzionalità in modo diverso. Questa situazione è molto meno grave di quanto non fosse in passato; quando IE4 e Netscape 4 erano in competizione per diventare il browser dominante negli anni Novanta, le aziende produttrici di browser implementavano deliberatamente le funzionalità in modo differente per cercare di ottenere un vantaggio competitivo, rendendo la vita degli sviluppatori un inferno. Oggi i browser rispettano molto meglio gli standard, ma differenze e bug possono ancora verificarsi.
- alcuni browser possono avere livelli di supporto diversi per le funzionalità tecnologiche rispetto ad altri. Questo è inevitabile quando si utilizzano funzionalità all'avanguardia che i browser stanno appena iniziando a implementare, oppure quando è necessario supportare browser molto vecchi che non vengono più sviluppati e che potrebbero essere stati congelati (ovvero, non ricevono più aggiornamenti) molto tempo prima ancora che una nuova funzionalità fosse inventata. Ad esempio, se si desidera utilizzare nel sito funzionalità JavaScript all'avanguardia, queste potrebbero non funzionare nei browser più vecchi. Se è necessario supportare browser meno recenti, potrebbe essere necessario non utilizzare tali funzionalità o convertire il codice in una sintassi tradizionale usando, dove necessario, un qualche tipo di cross-compiler.
- alcuni dispositivi possono avere limitazioni che fanno sì che un sito web venga eseguito lentamente o visualizzato male. Ad esempio, se un sito è stato progettato per apparire bene su un PC desktop, probabilmente risulterà minuscolo e difficile da leggere su un dispositivo mobile. Se il sito include molte grandi animazioni, potrebbero funzionare bene su un tablet dalle specifiche elevate, ma risultare lente o scattose su un dispositivo di fascia bassa.

…oltre ad altre ragioni.

Negli articoli successivi verranno esaminati i problemi cross-browser più comuni e le relative soluzioni.

## Flussi di lavoro per il testing cross-browser

Tutto questo testing cross-browser può sembrare dispendioso in termini di tempo e preoccupante, ma non deve necessariamente esserlo: è sufficiente pianificarlo attentamente e assicurarsi di eseguire test adeguati nei punti giusti, per evitare problemi imprevisti. Se si lavora a un progetto di grandi dimensioni, dovrebbe essere testato regolarmente per assicurarsi che le nuove funzionalità funzionino per il pubblico di destinazione e che le nuove aggiunte al codice non compromettano vecchie funzionalità che in precedenza funzionavano.

Se tutti i test vengono lasciati alla fine di un progetto, eventuali bug scoperti saranno molto più costosi e richiederanno più tempo per essere corretti rispetto a scoprirli e correggerli man mano che si procede.

Il flusso di lavoro per i test e le correzioni di bug in un progetto può essere suddiviso approssimativamente nelle seguenti quattro fasi (questa è solo una suddivisione molto generale: persone diverse potrebbero procedere in modo piuttosto differente):

**Pianificazione iniziale** > **Sviluppo** > **Testing/individuazione** > **Correzioni/iterazione**

I passaggi 2–4 tendono a essere ripetuti tutte le volte necessarie per completare l'implementazione. Le diverse parti del processo di testing verranno analizzate molto più nel dettaglio negli articoli successivi, ma per ora è utile riassumere ciò che può avvenire in ciascun passaggio.

### Pianificazione iniziale

Nella fase di pianificazione iniziale, probabilmente si terranno diverse riunioni con il proprietario del sito/cliente (potrebbe trattarsi del proprio responsabile oppure di qualcuno di un'azienda esterna per cui si sta realizzando un sito web), durante le quali si stabilisce esattamente che cosa dovrà essere il sito web: quali contenuti e funzionalità dovrà avere, quale aspetto dovrà avere e così via. A questo punto, sarà necessario sapere anche quanto tempo è disponibile per sviluppare il sito: qual è la scadenza e quanto verrà pagato il lavoro? Non verranno approfonditi molto questi aspetti, ma i problemi cross-browser possono avere un effetto significativo su tale pianificazione.

Una volta ottenuta un'idea dell'insieme di funzionalità richiesto e delle tecnologie con cui probabilmente verranno realizzate, si dovrebbe iniziare a esplorare il pubblico di destinazione: quali browser, dispositivi e così via utilizzerà il pubblico del sito? Il cliente potrebbe già disporre di dati derivati da ricerche precedenti, ad esempio da altri siti web di sua proprietà o da versioni precedenti del sito su cui si sta lavorando. In caso contrario, sarà possibile farsi una buona idea consultando altre fonti, come le statistiche di utilizzo dei concorrenti o dei Paesi in cui il sito verrà offerto. È possibile anche usare un po' di intuizione.

Ad esempio, potrebbe essere in fase di sviluppo un sito di e-commerce rivolto a clienti del Nord America. Il sito dovrebbe funzionare completamente nelle ultime versioni dei browser desktop e mobili più popolari, includendo Chrome (ed Edge e Opera, poiché si basano sullo stesso motore di rendering di Chrome), Firefox e Safari.
Dovrebbe inoltre essere accessibile con conformità WCAG AA.

Ora che sono note le piattaforme di test di destinazione, si dovrebbe tornare a esaminare l'insieme di funzionalità richiesto e le tecnologie da utilizzare.
Ad esempio, se il proprietario del sito di e-commerce desidera un tour 3D basato su WebGL per ogni prodotto, integrato nelle pagine dei prodotti, dovrà accettare che questo semplicemente non funzionerà in tutte le versioni dei browser legacy.

Si dovrebbe compilare un elenco delle potenziali aree problematiche.

> [!NOTE]
> È possibile trovare informazioni sul supporto dei browser per le tecnologie cercando le diverse funzionalità su MDN, il sito su cui ci si trova. È inoltre consigliabile consultare [caniuse.com](https://caniuse.com/) per ulteriori dettagli utili.

Una volta concordati questi dettagli, è possibile procedere e iniziare a sviluppare il sito.

### Sviluppo

Si passa ora allo sviluppo del sito. Le diverse parti dello sviluppo dovrebbero essere suddivise in moduli; ad esempio, si potrebbero dividere le diverse aree del sito: pagina iniziale, pagina del prodotto, carrello degli acquisti, flusso di pagamento e così via. Queste potrebbero poi essere ulteriormente suddivise: implementare un header e un footer comuni al sito, implementare la visualizzazione dettagliata della pagina prodotto, implementare un widget persistente per il carrello degli acquisti e così via.

Esistono diverse strategie generali per lo sviluppo cross-browser, ad esempio:

- Fare in modo che tutte le funzionalità funzionino nel modo più simile possibile in tutti i browser di destinazione. Questo può comportare la scrittura di diversi percorsi di codice che riproducono le funzionalità in modi differenti, destinati a browser diversi, oppure l'uso di un {{Glossary("Polyfill", "Polyfill")}} per simulare il supporto mancante mediante JavaScript o altre tecnologie, o ancora l'uso di una libreria che consente di scrivere un singolo blocco di codice e svolge poi operazioni diverse in background a seconda di ciò che il browser supporta.
- Accettare che alcuni elementi non funzioneranno allo stesso modo in tutti i browser e fornire soluzioni diverse, ma accettabili, nei browser che non supportano tutte le funzionalità. Talvolta questo è inevitabile a causa delle limitazioni dei dispositivi: uno schermo cinematografico panoramico non offrirà la stessa esperienza visiva di uno schermo mobile da 4", indipendentemente da come venga programmato il sito.
- Accettare che il sito non funzionerà in alcuni browser più vecchi e procedere oltre. Questo va bene, a condizione che il cliente/la base di utenti lo accetti.

Normalmente lo sviluppo comporterà una combinazione dei tre approcci precedenti. La cosa più importante è testare ogni piccola parte prima di integrarla: non rimandare tutti i test alla fine.

### Testing/individuazione

Dopo ogni fase di implementazione, sarà necessario testare le nuove funzionalità. Per iniziare, è necessario assicurarsi che non vi siano problemi generali nel codice che impediscano alla funzionalità di funzionare:

1. Testarla in un paio di browser stabili presenti sul sistema, come Firefox, Safari, Chrome o Edge.
2. Eseguire alcuni test di accessibilità a bassa fedeltà, ad esempio provando a utilizzare il sito soltanto con la tastiera oppure tramite uno screen reader per verificare se è navigabile.
3. Testare su una piattaforma mobile, come Android o iOS.

A questo punto, correggere qualsiasi problema riscontrato nel nuovo codice.

Successivamente, si dovrebbe provare ad ampliare l'elenco dei browser di test fino a includere un elenco completo dei browser utilizzati dal pubblico di destinazione e iniziare a concentrarsi sull'eliminazione dei problemi cross-browser (per ulteriori informazioni sulla [determinazione dei browser di destinazione](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies), vedere l'articolo successivo). Ad esempio:

- Provare a testare l'ultima modifica su tutti i browser desktop moderni disponibili, inclusi Firefox, Chrome, Opera, Edge e Safari su desktop (idealmente Mac, Windows e Linux).
- Testarla nei browser comuni su telefoni e tablet, ad esempio Safari su iOS su iPhone/iPad, Chrome e Firefox su iPhone/iPad/Android.
- Eseguire inoltre test in qualsiasi altro browser incluso nell'elenco di destinazione.

L'opzione più a bassa fedeltà consiste nell'eseguire personalmente tutti i test possibili, coinvolgendo i membri del team per aiutare se si lavora in squadra. Dove possibile, si dovrebbe cercare di testare su dispositivi fisici reali.

Se non si dispone dei mezzi per testare tutte le diverse combinazioni di browser, sistemi operativi e dispositivi su hardware fisico, è possibile utilizzare anche emulatori (per emulare un dispositivo tramite software sul computer desktop) e macchine virtuali (software che consente di emulare più combinazioni di sistema operativo/software sul computer desktop). Questa è una scelta molto diffusa, soprattutto in alcune circostanze: ad esempio, Windows non consente di avere più versioni di Windows installate simultaneamente sulla stessa macchina, quindi spesso l'unica opzione consiste nell'usare più macchine virtuali.

Un'altra opzione sono i gruppi di utenti: utilizzare un gruppo di persone esterne al team di sviluppo per testare il sito. Potrebbe trattarsi di un gruppo di amici o familiari, un gruppo di altri dipendenti, una classe di un'università locale oppure un servizio professionale di test con utenti, in cui le persone vengono pagate per provare il sito e fornire risultati.

Infine, è possibile rendere più intelligente il testing tramite strumenti di audit o automazione; questa è una scelta sensata quando i progetti diventano più grandi, poiché eseguire tutti questi test manualmente può iniziare a richiedere davvero molto tempo. È possibile configurare un proprio sistema di automazione dei test ([Selenium](https://www.selenium.dev/) è l'applicazione più diffusa) che potrebbe, ad esempio, caricare il sito in diversi browser e:

- verificare se il clic su un pulsante produce correttamente un risultato, come la visualizzazione di una mappa, mostrando i risultati una volta completati i test;
- acquisire uno screenshot di ciascuno, consentendo di verificare se il layout è coerente tra i diversi browser.

Se si desidera investire denaro nel testing, esistono anche strumenti commerciali che possono automatizzare gran parte della configurazione e dei test (come [Sauce Labs](https://saucelabs.com/) e [Browser Stack](https://www.browserstack.com/)). Questi tipi di strumenti di solito consentono un flusso di lavoro di {{Glossary("continuous_integration", "integrazione continua")}}, in cui le modifiche al codice vengono testate automaticamente prima di essere autorizzate all'inserimento nel repository del codice.

#### Testing sui browser in versione preliminare

Spesso è una buona idea eseguire test sulle versioni preliminari dei browser; consultare i seguenti collegamenti:

- [Firefox Developer Edition](https://www.firefox.com/en-US/channel/desktop/developer/)
- [Microsoft Edge Insider](https://explore.microsoft.com/en-us/edge/download/insider)
- [Safari Technology Preview](https://developer.apple.com/safari/technology-preview/)
- [Chrome Canary](https://www.google.com/chrome/canary/)
- [Opera Developer](https://www.opera.com/opera/developer)

Questo è particolarmente comune se nel sito vengono utilizzate tecnologie molto nuove e si desidera testare le implementazioni più recenti, oppure se si riscontra un bug nell'ultima versione rilasciata di un browser e si desidera verificare se gli sviluppatori del browser lo hanno corretto in una versione più recente.

### Correzioni/iterazione

Una volta scoperto un bug, è necessario cercare di correggerlo.

La prima cosa da fare è restringere il più possibile le condizioni in cui si verifica il bug. Ottenere quante più informazioni possibile dalla persona che segnala il bug: quali piattaforme, dispositivi, versioni del browser e così via. Provare configurazioni simili, ad esempio la stessa versione del browser su diverse piattaforme desktop, oppure alcune versioni diverse dello stesso browser sulla stessa piattaforma, per verificare quanto sia diffuso il bug.

Potrebbe non essere colpa dello sviluppatore: se un bug esiste in un browser, si spera che il fornitore lo corregga rapidamente. Potrebbe essere già stato corretto: ad esempio, se un bug è presente in Firefox release 49, ma non è più presente in Firefox Nightly (versione 52), significa che è stato corretto. Se non è stato corretto, potrebbe essere opportuno segnalare un bug (vedere [Segnalazione di bug](#segnalazione_di_bug), di seguito).

Se invece è colpa dello sviluppatore, occorre correggerlo. Individuare la causa del bug richiede la stessa strategia di qualsiasi bug nello sviluppo web (anche in questo caso, vedere [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML), [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS) e [What went wrong? Troubleshooting JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)). Una volta scoperta la causa del bug, è necessario decidere come aggirarlo nel particolare browser in cui provoca problemi: non è possibile semplicemente modificare direttamente il codice problematico, poiché questo potrebbe danneggiare il codice in altri browser. L'approccio generale consiste solitamente nel biforcare il codice in qualche modo, ad esempio utilizzando codice JavaScript per il rilevamento delle funzionalità, per individuare le situazioni in cui una funzionalità problematica non funziona ed eseguire in tali casi un codice alternativo che invece funziona.

Dopo aver applicato una correzione, sarà necessario ripetere il processo di testing per assicurarsi che la correzione funzioni correttamente e non abbia causato malfunzionamenti del sito in altre parti o in altri browser.

## Segnalazione di bug

Per ribadire quanto detto sopra, se vengono scoperti bug nei browser, è opportuno segnalarli:

- [Firefox Bugzilla](https://bugzilla.mozilla.org/)
- [Safari](https://bugs.webkit.org/)
- [Chrome](https://issues.chromium.org/issues)
- [Opera](https://opera.atlassian.net/servicedesk/customer/portal/9)

## Riepilogo

Questo articolo dovrebbe aver fornito una comprensione generale dei concetti più importanti del testing cross-browser. Con queste conoscenze, è possibile proseguire e iniziare a imparare le strategie di testing cross-browser.

{{NextMenu("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing")}}
