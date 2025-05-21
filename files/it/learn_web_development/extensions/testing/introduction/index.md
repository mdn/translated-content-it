---
title: Introduzione al test cross-browser
short-title: Introduction
slug: Learn_web_development/Extensions/Testing/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing")}}

Questo articolo offre una panoramica del test cross-browser: cosa sono i test cross-browser, alcuni problemi comuni e alcuni approcci per il debug/risoluzione dei problemi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali come <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire una comprensione dei concetti di alto livello coinvolti nel test cross-browser.
      </td>
    </tr>
  </tbody>
</table>

## Cos'è il test cross-browser?

Il test cross-browser è la pratica di garantire che un sito web funzioni su diversi browser e dispositivi. Gli sviluppatori web dovrebbero considerare:

- Diversi browser, inclusi quelli leggermente più vecchi che non supportano tutte le ultime funzionalità di JS/CSS.
- Diversi dispositivi, dai desktop e laptop ai tablet e smartphone, fino alle smart TV, con varie capacità hardware.
- Persone con disabilità, che potrebbero fare affidamento su tecnologie assistive come i lettori di schermo o usare esclusivamente una tastiera.

Ricorda che non sei i tuoi utenti — solo perché il tuo sito funziona su un MacBook Pro o un Galaxy Nexus di fascia alta, non significa che funzionerà per tutti i tuoi utenti!

> **Nota:** [Make the web work for everyone](https://hacks.mozilla.org/2016/07/make-the-web-work-for-everyone/) discute i diversi browser, la loro quota di mercato e i problemi di compatibilità cross-browser correlati.

I siti web dovrebbero essere accessibili su diversi browser e dispositivi, e alle persone con disabilità (ad esempio, compatibili con i lettori di schermo). Un sito non deve offrire esattamente la stessa esperienza su tutti i browser e dispositivi, purché la funzionalità principale sia accessibile in qualche modo. Ad esempio, un browser moderno potrebbe avere qualcosa di animato, 3D e appariscente, mentre i browser più vecchi potrebbero mostrare solo un grafico piatto con la stessa informazione.

Inoltre, è praticamente impossibile che un sito web funzioni su TUTTI i browser e dispositivi, quindi uno sviluppatore web dovrebbe raggiungere un accordo con il proprietario del sito sulla gamma di browser e dispositivi su cui il codice funzionerà.

## Perché si verificano problemi di cross-browser?

Ci sono molte ragioni diverse per cui si verificano problemi di cross-browser, e nota che qui stiamo parlando di problemi in cui le cose si comportano diversamente su diversi browser/dispositivi/preferenze di navigazione. Prima di arrivare ai problemi di cross-browser, dovresti aver già risolto i bug nel tuo codice (vedi [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML), [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS), e [What went wrong? Troubleshooting JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong) dai temi precedenti per rinfrescarti la memoria se necessario).

I problemi di cross-browser comunemente si verificano perché:

- a volte i browser hanno bug o implementano le funzionalità in modo diverso. Questa situazione è molto meno grave di quanto lo fosse in passato; quando IE4 e Netscape 4 competevano per essere il browser dominante negli anni '90, le compagnie di browser implementavano deliberatamente le funzionalità in modo diverso l'una dall'altra per cercare di ottenere un vantaggio competitivo, il che rendeva la vita infernale per gli sviluppatori. Oggi i browser seguono molto meglio gli standard, ma le differenze e i bug fanno comunque capolino a volte.
- alcuni browser possono avere diversi livelli di supporto per le funzionalità tecnologiche rispetto ad altri. Questo è inevitabile quando si tratta di funzionalità all'avanguardia che i browser stanno iniziando a implementare, o se devi supportare browser molto vecchi che non sono più in sviluppo, che potrebbero essere stati 'congelati' (ossia, non verranno più svolti nuovi lavori su di essi) molto tempo prima che una nuova funzionalità fosse persino inventata. Ad esempio, se vuoi utilizzare funzionalità JavaScript all'avanguardia nel tuo sito, potrebbero non funzionare nei browser più vecchi. Se hai bisogno di supportare browser più vecchi, potrebbe essere necessario non utilizzarle o convertire il tuo codice in sintassi tradizionale usando un qualche tipo di cross-compilatore dove necessario.
- alcuni dispositivi possono avere restrizioni che causano un'esecuzione lenta o una cattiva visualizzazione di un sito web. Ad esempio, se un sito è stato progettato per apparire bene su un PC desktop, potrebbe apparire minuscolo e difficile da leggere su un dispositivo mobile. Se il tuo sito include una serie di grandi animazioni, potrebbe andare bene su un tablet di fascia alta, ma potrebbe essere lento o scattante su un dispositivo di fascia bassa.

…e altre ragioni oltre a queste.

Nei prossimi articoli esamineremo problemi comuni di cross-browser e vedremo le soluzioni a questi.

## Flussi di lavoro per il test cross-browser

Tutto questo parlare di test cross-browser può sembrare dispendioso in termini di tempo e spaventoso, ma non è necessario che lo sia — basta pianificare con cura e assicurarsi di fare abbastanza test nei punti giusti per non incorrere in problemi imprevisti. Se stai lavorando su un progetto di grandi dimensioni, dovresti testarlo regolarmente, per assicurarti che le nuove funzionalità funzionino per il tuo pubblico di destinazione e che le nuove aggiunte al codice non rompano le vecchie funzionalità che funzionavano precedentemente.

Se lasci tutti i test alla fine di un progetto, eventuali bug che scoprirai saranno molto più costosi e dispendiosi nel tempo da correggere rispetto a se li scoprissi e correggessi man mano.

Il flusso di lavoro per test e correzioni di bug in un progetto può essere suddiviso in modo approssimativo nelle seguenti quattro fasi (questo è solo un approccio molto approssimativo — persone diverse potrebbero fare cose piuttosto diverse da questo):

**Pianificazione iniziale** > **Sviluppo** > **Test/scoperta** > **Correzioni/iterazione**

I passaggi 2-4 tenderanno ad essere ripetuti tutte le volte necessarie per completare tutta l'implementazione. Esamineremo le diverse parti del processo di test in modo molto più dettagliato negli articoli successivi, ma per ora, riassumiamo ciò che può avvenire in ciascun passaggio.

### Pianificazione iniziale

Nella fase di pianificazione iniziale, probabilmente avrai diverse riunioni di pianificazione con il proprietario del sito/cliente (potrebbe essere il tuo capo, o qualcuno di una società esterna per cui stai costruendo un sito web), in cui determinerai esattamente cosa dovrebbe essere il sito web — quale contenuto e funzionalità dovrebbe avere, come dovrebbe apparire, ecc. A questo punto, vorrai anche sapere quanto tempo hai per sviluppare il sito — qual è la scadenza stabilita, e quanto pagheranno per il tuo lavoro? Non entreremo nei dettagli su questo, ma i problemi di cross-browser possono avere un effetto serio su tale pianificazione.

Una volta che hai un'idea delle funzionalità richieste e delle tecnologie che probabilmente userai per costruire queste funzionalità, dovresti iniziare ad esplorare il pubblico di destinazione — quali browser, dispositivi, ecc. useranno il pubblico di destinazione per questo sito? Il cliente potrebbe già avere dati su questo ottenuti da ricerche precedenti che hanno fatto, ad esempio, da altri siti web che possiedono, o da versioni precedenti del sito web su cui stai lavorando ora. In caso contrario, potrai ottenere una buona idea osservando altre fonti, come le statistiche di utilizzo dei concorrenti, o i paesi in cui il sito sarà servito. Puoi anche usare un po' di intuizione.

Ad esempio, potresti costruire un sito e-commerce che serve clienti in Nord America. Il sito dovrebbe funzionare completamente nelle ultime versioni dei browser desktop e mobile più popolari — questo dovrebbe includere Chrome (e Edge, Opera poiché si basano sullo stesso motore di rendering di Chrome), Firefox e Safari. Dovrebbe anche essere accessibile con conformità WCAG AA.

Ora che conosci le piattaforme di test di destinazione, dovresti tornare indietro e rivedere il set di funzionalità richiesto e le tecnologie che intendi utilizzare. Ad esempio, se il proprietario del sito e-commerce vuole un tour 3D alimentato da WebGL di ogni prodotto integrato nelle pagine dei prodotti, dovranno accettare che questo non funzionerà su tutte le versioni legacy dei browser.

Dovresti compilare un elenco delle potenziali aree problematiche.

> [!NOTE]
> Puoi trovare informazioni sul supporto dei browser per le tecnologie cercando le diverse funzionalità su MDN — il sito su cui ti trovi! Dovresti anche consultare [caniuse.com](https://caniuse.com/) per ulteriori dettagli utili.

Una volta concordati questi dettagli, puoi procedere e iniziare a sviluppare il sito.

### Sviluppo

Passiamo ora allo sviluppo del sito. Dovresti suddividere le diverse parti dello sviluppo in moduli, ad esempio potresti suddividere le diverse aree del sito — homepage, pagina del prodotto, carrello, flusso di pagamento, ecc. Potresti quindi suddividere ulteriormente queste aree — implementare un'intestazione e un piè di pagina comuni al sito, implementare la visualizzazione dei dettagli pagina prodotto, implementare un widget di carrello della spesa persistente, ecc.

Esistono molteplici strategie generali per lo sviluppo cross-browser, ad esempio:

- Far funzionare tutte le funzionalità il più vicino possibile in tutti i browser di destinazione. Questo può comportare la scrittura di percorsi di codice diversi che riproducono funzionalità in modi diversi destinati a diversi browser, o l'uso di un {{Glossary("Polyfill", "Polyfill")}} per mimare il supporto mancante usando JavaScript o altre tecnologie, o l'uso di una libreria che ti consente di scrivere un unico pezzo di codice e poi fare cose diverse in background a seconda di ciò che il browser supporta.
- Accettare che alcune cose non funzioneranno allo stesso modo su tutti i browser, e fornire soluzioni diverse (accettabili) nei browser che non supportano la funzionalità completa. A volte questo è inevitabile a causa delle limitazioni del dispositivo — uno schermo cinematografico largo non darà la stessa esperienza visiva di uno schermo mobile da 4", indipendentemente da come programmi il tuo sito.
- Accettare che il tuo sito semplicemente non funzionerà su alcuni browser più vecchi e andare avanti. Questo va bene, a condizione che il cliente/utenza base sia d'accordo.

Normalmente, il tuo sviluppo coinvolgerà una combinazione dei tre approcci sopra citati. La cosa più importante è testare ogni piccola parte prima di confermarla — non lasciare tutto il test alla fine!

### Test/scoperta

Dopo ogni fase di implementazione, dovrai testare la nuova funzionalità. Per iniziare, dovresti assicurarti che non ci siano problemi generali con il tuo codice che impediscono al tuo feature di funzionare:

1. Testalo in un paio di browser stabili sul tuo sistema, come Firefox, Safari, Chrome o Edge.
2. Effettua alcuni test di accessibilità di base, come provare a usare il tuo sito solo con la tastiera o usare il tuo sito tramite un lettore di schermo per vedere se è navigabile.
3. Testalo su una piattaforma mobile, come Android o iOS.

A questo punto, correggi eventuali problemi che riscontri con il tuo nuovo codice.

Successivamente, dovresti espandere la tua lista di browser di test a un elenco completo di browser per il pubblico di destinazione e iniziare a concentrarti sull'eliminazione dei problemi di cross-browser (vedi l'articolo successivo per maggiori informazioni su [determinare i browser di destinazione](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies)). Ad esempio:

- Prova a testare l'ultima modifica su tutti i moderni browser desktop che puoi — inclusi Firefox, Chrome, Opera, Edge, e Safari su desktop (Mac, Windows e Linux, idealmente).
- Testalo nei browser comuni per telefoni e tablet (ad es., Safari su iOS su iPhone/iPad, Chrome e Firefox su iPhone/iPad/Android),
- Effettua anche test in qualsiasi altro browser che hai incluso nella tua lista di destinazione.

L'opzione più semplicistica è fare tu stesso tutti i test che puoi (coinvolgendo i compagni di squadra per aiutarti se stai lavorando in un team). Dovresti cercare di testarlo su dispositivi fisici reali ove possibile.

Se non hai i mezzi per testare tutte quelle diverse combinazioni di browser, sistemi operativi e dispositivi su hardware fisico, puoi anche fare uso di emulatori (emulare un dispositivo usando software sul tuo computer desktop) e macchine virtuali (software che ti consente di emulare più combinazioni di sistema operativo/software sul tuo computer desktop). Questa è una scelta molto popolare, specialmente in alcune circostanze — ad esempio, Windows non ti consente di avere più versioni di Windows installate contemporaneamente sulla stessa macchina, quindi l'uso di più macchine virtuali è spesso l'unica opzione qui.

Un'altra opzione è i gruppi di utenti — utilizzare un gruppo di persone al di fuori del tuo team di sviluppo per testare il tuo sito. Questo potrebbe essere un gruppo di amici o familiari, un gruppo di altri dipendenti, una classe in un'università locale o un assetto di test utente professionale, dove le persone sono pagate per testare il tuo sito e fornire risultati.

Infine, puoi migliorare i tuoi test usando strumenti di audizione o automazione; questa è una scelta sensibile man mano che i tuoi progetti diventano più grandi, poiché fare tutti questi test a mano può iniziare a richiedere davvero molto tempo. Puoi impostare un tuo sistema di automazione dei test ([Selenium](https://www.selenium.dev/) essendo l'applicazione popolare scelta) che potrebbe, ad esempio, caricare il tuo sito in un numero di browser diversi, e:

- vedere se un clic su un pulsante fa sì che qualcosa accada con successo (come ad esempio, la visualizzazione di una mappa), visualizzando i risultati una volta completati i test
- scattare uno screenshot di ciascuno, permettendoti di vedere se un layout è coerente tra i diversi browser.

Se desideri investire denaro nei test, ci sono anche strumenti commerciali che possono automatizzare gran parte della configurazione e dei test per te (come [Sauce Labs](https://saucelabs.com/) e [Browser Stack](https://www.browserstack.com/)). Questi tipi di strumenti di solito abilitano un flusso di lavoro di integrazione continua, dove le modifiche al codice vengono testate automaticamente prima che siano consentite nell'archivio del tuo codice.

#### Test sui browser in versione beta

Spesso è una buona idea testare sui browser in versione beta; vedi i link seguenti:

- [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)
- [Microsoft Edge Insider](https://www.microsoft.com/en-us/edge/download/insider)
- [Safari Technology Preview](https://developer.apple.com/safari/technology-preview/)
- [Chrome Canary](https://www.google.com/chrome/canary/)
- [Opera Developer](https://www.opera.com/opera/developer)

Questo è particolarmente rilevante se stai usando tecnologie molto nuove nel tuo sito e vuoi testare le ultime implementazioni, o se ti stai imbattendo in un bug nella versione più recente di un browser e vuoi vedere se gli sviluppatori del browser hanno risolto il bug in una versione più recente.

### Correzioni/iterazione

Una volta scoperto un bug, devi provare a risolverlo.

La prima cosa da fare è restringere il più possibile il luogo in cui si verifica il bug. Ottieni quante più informazioni puoi dalla persona che segnala il bug — quale piattaforma(e), dispositivo(i), versione(i) del browser, ecc. Provalo su configurazioni simili (ad esempio, la stessa versione del browser su piattaforme desktop diverse, o alcune versioni diverse dello stesso browser sulla stessa piattaforma) per vedere quanto ampiamente il bug persiste.

Potrebbe non essere colpa tua — se un bug esiste in un browser, allora si spera che il fornitore lo corregga rapidamente. Potrebbe essere già stato risolto — ad esempio se un bug è presente nella versione release 49 di Firefox, ma non è più presente in Firefox Nightly (versione 52), allora lo hanno risolto. Se non è risolto, potresti voler segnalare un bug (vedi [Segnalare bug](#segnalare_bug), sotto).

Se è colpa tua, devi risolverlo! Scoprire la causa del bug comporta la stessa strategia di qualsiasi bug di sviluppo web (ancora una volta, vedi [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML), [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS), e [What went wrong? Troubleshooting JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)). Una volta scoperto cosa sta causando il tuo bug, devi decidere come aggirarlo nel browser particolare in cui sta causando problemi — non puoi semplicemente cambiare il codice problematico del tutto, poiché questo potrebbe rompere il codice in altri browser. L'approccio generale è di solito biforcare il codice in qualche modo, ad esempio usare del codice di rilevamento delle funzionalità JavaScript per rilevare situazioni in cui una funzionalità problematica non funziona, ed eseguire qualche codice diverso in quei casi che funziona.

Una volta effettuata una correzione, vorrai ripetere il tuo processo di test per assicurarti che la tua correzione stia funzionando correttamente, e non abbia causato la rottura del sito in altri punti o altri browser.

## Segnalare bug

Solo per ribadire quanto detto sopra, se scopri bug nei browser, dovresti segnalarli:

- [Firefox Bugzilla](https://bugzilla.mozilla.org/)
- [Safari](https://bugs.webkit.org/)
- [Chrome](https://issues.chromium.org/issues)
- [Opera](https://opera.atlassian.net/servicedesk/customer/portal/9)

## Sommario

Questo articolo dovrebbe averti fornito una comprensione ad alto livello dei concetti più importanti da conoscere sui test cross-browser. Forte di questa conoscenza, sei ora pronto a passare oltre e iniziare a conoscere le strategie di test cross-browser.

{{NextMenu("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing")}}
