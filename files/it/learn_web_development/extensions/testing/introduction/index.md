---
title: Introduzione al test cross-browser
short-title: Introduction
slug: Learn_web_development/Extensions/Testing/Introduction
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing")}}

Questo articolo offre una panoramica del test cross-browser: cosa significa, alcuni problemi comuni e alcuni approcci per il debug/risoluzione dei problemi.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con le basi delle lingue <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a>, e
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

## Che cos'è il test cross-browser?

Il test cross-browser è la pratica di garantire che un sito web funzioni su vari browser e dispositivi. Gli sviluppatori web dovrebbero considerare:

- Browser diversi, inclusi quelli leggermente più vecchi che non supportano tutte le ultime funzionalità JS/CSS.
- Dispositivi diversi, dai desktop e laptop ai tablet e smartphone, fino ai televisori intelligenti, con diverse capacità hardware.
- Persone con disabilità, che possono fare affidamento su tecnologie assistive come lettori di schermo, o utilizzare solo una tastiera.

Ricordi che lei non è i suoi utenti — solo perché il suo sito funziona sul suo MacBook Pro o Galaxy Nexus di fascia alta, non significa che funzionerà per tutti i suoi utenti!

> **Nota:** [Make the web work for everyone](https://hacks.mozilla.org/2016/07/make-the-web-work-for-everyone/) discute i diversi browser, la loro quota di mercato e i relativi problemi di compatibilità cross-browser.

I siti web dovrebbero essere accessibili su diversi browser e dispositivi, e alle persone con disabilità (ad esempio, compatibile con i lettori di schermo). Un sito non ha bisogno di offrire la stessa esperienza esatta su tutti i browser e dispositivi, a condizione che la funzionalità di base sia accessibile in qualche modo. Ad esempio, un browser moderno potrebbe avere qualcosa di animato, 3D e brillante, mentre i browser più vecchi potrebbero mostrare solo un'immagine piatta con le stesse informazioni.

Inoltre, è praticamente impossibile per un sito web funzionare su TUTTI i browser e dispositivi, quindi uno sviluppatore web dovrebbe raggiungere un accordo con il proprietario del sito sulla gamma di browser e dispositivi su cui il codice funzionerà.

## Perché si verificano problemi cross-browser?

Ci sono molte ragioni diverse per cui si verificano problemi cross-browser, e noti che qui stiamo parlando di problemi in cui le cose si comportano in modo diverso su diversi browser/dispositivi/preferenze di navigazione. Prima ancora di arrivare ai problemi cross-browser, dovrebbe già aver risolto bug nel suo codice (si veda [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML), [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS), e [What went wrong? Troubleshooting JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong) dagli argomenti precedenti per rinfrescare la sua memoria se necessario).

I problemi cross-browser si verificano comunemente perché:

- a volte i browser hanno bug o implementano le caratteristiche in modo diverso. Questa situazione è molto meno drammatica di quanto fosse in passato; quando IE4 e Netscape 4 competivano per diventare il browser dominante negli anni '90, le aziende di browser implementavano deliberatamente le cose in modo diverso l'una dall'altra per cercare di ottenere un vantaggio competitivo, il che rendeva la vita infernale per gli sviluppatori. I browser sono molto più bravi a seguire gli standard al giorno d'oggi, ma a volte le differenze e i bug riescono comunque a insinuarsi.
- alcuni browser potrebbero avere livelli di supporto differenti per le funzionalità tecnologiche rispetto ad altri. Questo è inevitabile quando si ha a che fare con funzionalità all'avanguardia che i browser stanno appena iniziando a implementare, o se deve supportare browser molto vecchi che non sono più in fase di sviluppo, il che potrebbe essere stato congelato (cioè, non si fa più nuovo lavoro su di essi) molto prima che una nuova funzionalità fosse addirittura inventata. Ad esempio, se vuole utilizzare funzionalità JavaScript all'avanguardia nel suo sito, potrebbero non funzionare nei browser più vecchi. Se deve supportare browser più vecchi, potrebbe dover non usare quelli, o convertire il suo codice in sintassi vecchio stile usando qualche tipo di cross-compilatore dove necessario.
- alcuni dispositivi possono avere vincoli che causano l'esecuzione lenta o la visualizzazione errata di un sito web. Ad esempio, se un sito è stato progettato per avere un bell'aspetto su un PC desktop, probabilmente sembrerà piccolo e sarà difficile da leggere su un dispositivo mobile. Se il suo sito include un carico di grandi animazioni, potrebbe andare bene su un tablet di fascia alta ma potrebbe essere lento o scattante su un dispositivo di fascia bassa.

…e altre ragioni ancora.

In articoli successivi, esploreremo i problemi comuni cross-browser e esamineremo le soluzioni a tali problemi.

## Flussi di lavoro per i test cross-browser

Tutta questa questione dei test cross-browser può sembrare dispendiosa in termini di tempo e spaventosa, ma non dovrebbe esserlo — basta pianificare attentamente e assicurarsi di fare abbastanza test nei posti giusti per assicurarsi di non incorrere in problemi inaspettati. Se sta lavorando su un grande progetto, dovrebbe testarlo regolarmente, per assicurarsi che le nuove funzionalità funzionino per il suo pubblico di destinazione, e che le nuove aggiunte al codice non rompano le vecchie funzionalità che funzionavano in precedenza.

Se lascia tutti i test alla fine di un progetto, eventuali bug scoperti saranno molto più costosi e dispendiosi in termini di tempo da correggere rispetto a se li scoprisse e li correggesse lungo il percorso.

Il flusso di lavoro per i test e le correzioni di bug su un progetto può essere suddiviso approssimativamente nelle seguenti quattro fasi (questo è solo molto approssimativo — persone diverse possono fare le cose in modo abbastanza diverso da questo):

**Pianificazione iniziale** > **Sviluppo** > **Testing/scoperta** > **Correzioni/iterazione**

I passaggi 2-4 tendono a essere ripetuti tutte le volte necessarie per completare tutta l'implementazione. Esamineremo le diverse parti del processo di test in dettaglio molto maggiore in articoli successivi, ma per ora, riassumiamo solo ciò che può accadere in ciascun passaggio.

### Pianificazione iniziale

Nella fase di pianificazione iniziale, probabilmente avrà diversi incontri di pianificazione con il proprietario/cliente del sito (potrebbe essere il suo capo o qualcuno di un'azienda esterna per cui sta costruendo un sito web), in cui determinerà esattamente cosa dovrebbe essere il sito web — quale contenuto e funzionalità dovrebbe avere, come dovrebbe apparire, ecc. A questo punto, vorrà anche sapere quanto tempo ha per sviluppare il sito — qual è la loro scadenza e quanto le pagheranno per il suo lavoro? Non entreremo nei dettagli a riguardo, ma i problemi cross-browser possono avere un effetto significativo su tale pianificazione.

Una volta che avrà un'idea del set di funzionalità richiesto, e delle tecnologie con cui probabilmente costruirà queste funzionalità, dovrebbe iniziare a esplorare il pubblico di destinazione — quali browser, dispositivi, ecc. utilizzerà il pubblico di destinazione per questo sito? Il cliente potrebbe già avere dati su questo da ricerche precedenti che hanno fatto, ad esempio, da altri siti che possiedono, o da versioni precedenti del sito web su cui sta lavorando ora. Se no, sarà in grado di ottenere una buona idea esaminando altre fonti, come le statistiche di utilizzo dei concorrenti, o i paesi a cui il sito sarà rivolto. Può anche usare un po' di intuizione.

Quindi, ad esempio, potrebbe costruire un sito di e-commerce che serve clienti nel Nord America. Il sito dovrebbe funzionare interamente nelle ultime versioni dei browser desktop e mobili più popolari — questo dovrebbe includere Chrome (e Edge, Opera in quanto sono basati sullo stesso motore di rendering di Chrome), Firefox, e Safari.
Dovrebbe anche essere accessibile con conformità WCAG AA.

Ora conosce le piattaforme di test di destinazione, dovrebbe tornare a rivedere il set di funzionalità richiesto e quali tecnologie intende utilizzare.
Ad esempio, se il proprietario del sito di e-commerce vuole un tour 3D alimentato da WebGL di ciascun prodotto integrato nelle pagine dei prodotti, dovrà accettare che questo semplicemente non funzionerà in tutte le vecchie versioni del browser.

Dovrebbe compilare un elenco delle potenziali aree problematiche.

> [!NOTE]
> Può trovare informazioni sul supporto ai browser per le tecnologie cercando le diverse funzionalità su MDN — il sito su cui si trova! Dovrebbe anche consultare [caniuse.com](https://caniuse.com/), per ulteriori dettagli utili.

Una volta concordati questi dettagli, può procedere e iniziare a sviluppare il sito.

### Sviluppo

Ora passiamo allo sviluppo del sito. Dovrebbe dividere le diverse parti dello sviluppo in moduli, ad esempio potrebbe dividere le diverse aree del sito — pagina principale, pagina prodotto, carrello, flusso di pagamento, ecc. Potrebbe poi suddividere ulteriormente queste — implementare un'intestazione e un piè di pagina comune al sito, implementare la vista dettagliata della pagina prodotto, implementare il widget del carrello persistente, ecc.

Ci sono varie strategie generali per lo sviluppo cross-browser, ad esempio:

- Fare in modo che tutta la funzionalità funzioni il più possibile in tutti i browser di destinazione. Ciò potrebbe comportare la scrittura di percorsi di codice diversi che riproducono la funzionalità in modi diversi destinati a browser diversi, o l'uso di un {{Glossary("Polyfill", "Polyfill")}} per imitare qualsiasi supporto mancante usando JavaScript o altre tecnologie, o l'uso di una libreria che consenta di scrivere un singolo pezzo di codice e poi fare cose diverse in background a seconda di cosa supporta il browser.
- Accettare che alcune cose non funzioneranno allo stesso modo su tutti i browser, e fornire soluzioni diverse (accettabili) nei browser che non supportano la piena funzionalità. A volte questo è inevitabile a causa dei vincoli del dispositivo — un widescreen del cinema non darà la stessa esperienza visiva di uno schermo mobile da 4", indipendentemente da come programma il suo sito.
- Accettare che il suo sito semplicemente non funzionerà in alcuni browser più vecchi, e andare avanti. Questo va bene, a condizione che il suo cliente/base di utenti sia d'accordo.

Normalmente lo sviluppo coinvolgerà una combinazione dei tre approcci sopra menzionati. La cosa più importante è che testi ogni piccola parte prima di confermarla — non lasci tutti i test fino alla fine!

### Testing/scoperta

Dopo ogni fase di implementazione, sarà necessario testare la nuova funzionalità. Per iniziare, dovrebbe assicurarsi che non ci siano problemi generali nel suo codice che impediscano al suo feature di funzionare:

1. Lo testi in un paio di browser stabili sul suo sistema, come Firefox, Safari, Chrome, o Edge.
2. Faccia alcuni test di accessibilità a bassa fedeltà, come provare a usare il suo sito solo con la tastiera, o usare il suo sito tramite un lettore di schermo per vedere se è navigabile.
3. Lo testi su una piattaforma mobile, come Android o iOS.

A questo punto, risolva eventuali problemi che trova con il suo nuovo codice.

Successivamente, dovrebbe provare a espandere il suo elenco di browser di test a uno elenco completo di browser del pubblico di destinazione e iniziare a concentrarsi sull'eliminazione dei problemi cross-browser (vedere il prossimo articolo per ulteriori informazioni su [determinare i browser di destinazione](/it/docs/Learn_web_development/Extensions/Testing/Testing_strategies)). Ad esempio:

- Tentare di testare l'ultima modifica su tutti i browser desktop moderni possibili — inclusi Firefox, Chrome, Opera, Edge e Safari su desktop (Mac, Windows e Linux, idealmente).
- Lo testi nei browser comuni su telefoni e tablet (ad esempio, iOS Safari su iPhone/iPad, Chrome e Firefox su iPhone/iPad/Android),
- Esegua test anche in qualsiasi altro browser che ha incluso nel suo elenco di destinazione.

L'opzione più a bassa fedeltà è fare tutti i test che può da solo (coinvolgendo i compagni di squadra per aiutare se sta lavorando in un team). Dovrebbe cercare di testarlo su dispositivi fisici reali ove possibile.

Se non ha i mezzi per testare tutte quelle diverse combinazioni di browser, sistemi operativi e dispositivi su hardware fisico, può anche fare uso di emulatori (emulare un dispositivo utilizzando il software sul suo computer desktop) e macchine virtuali (software che le consente di emulare più combinazioni di sistemi operativi/software sul suo computer desktop). Questa è una scelta molto popolare, specialmente in alcune circostanze — ad esempio, Windows non le consente di avere più versioni di Windows installate contemporaneamente sulla stessa macchina, quindi l'uso di più macchine virtuali è spesso l'unica opzione qui.

Un'altra opzione sono i gruppi di utenti — utilizzare un gruppo di persone al di fuori del suo team di sviluppo per testare il suo sito. Potrebbe essere un gruppo di amici o familiari, un gruppo di altri dipendenti, una classe presso un'università locale, o una configurazione di test utente professionale, in cui le persone vengono pagate per testare il suo sito e fornire risultati.

Infine, può diventare più intelligente con i suoi test utilizzando strumenti di auditing o automazione; questa è una scelta sensata man mano che i suoi progetti diventano più grandi, poiché fare tutti questi test manualmente può iniziare a richiedere davvero molto tempo. Può impostare il suo sistema di automazione dei test ([Selenium](https://www.selenium.dev/) essendo l'app popolare di scelta) che potrebbe ad esempio caricare il suo sito in una serie di diversi browser, e:

- vedere se un clic su un pulsante fa accadere qualcosa con successo (come per esempio, una mappa che viene visualizzata), mostrando i risultati una volta completati i test,
- scattare uno screenshot di ciascuno, consentendole di vedere se un layout è coerente tra i diversi browser.

Se desidera investire denaro nel test, ci sono anche strumenti commerciali che possono automatizzare gran parte dell'impostazione e del test per lei (come [Sauce Labs](https://saucelabs.com/) e [Browser Stack](https://www.browserstack.com/)). Questi tipi di strumenti di solito consentono un flusso di lavoro di integrazione continua, in cui le modifiche al codice vengono testate automaticamente prima che siano consentite di essere inviate nel suo repository di codice.

#### Testare su browser prerelease

Spesso è una buona idea testare su versioni prerelease dei browser; vedere i seguenti link:

- [Firefox Developer Edition](https://www.mozilla.org/en-US/firefox/developer/)
- [Microsoft Edge Insider](https://www.microsoft.com/en-us/edge/download/insider)
- [Safari Technology Preview](https://developer.apple.com/safari/technology-preview/)
- [Chrome Canary](https://www.google.com/chrome/canary/)
- [Opera Developer](https://www.opera.com/opera/developer)

Questo è particolarmente rilevante se sta usando tecnologie molto nuove nel suo sito, e vuole testare le implementazioni più recenti, o se sta affrontando un bug nell'ultima versione di rilascio di un browser, e vuole vedere se gli sviluppatori del browser hanno corretto il bug in una versione più recente.

### Correzioni/iterazione

Una volta scoperto un bug, è necessario cercare di risolverlo.

La prima cosa da fare è restringere il campo in cui si verifica il bug il più possibile. Ottenere quante più informazioni possibile dalla persona che segnala il bug — che piattaforma(e), dispositivo(i), versione(i) del browser, ecc. Lo provi su configurazioni simili (ad esempio, la stessa versione del browser su piattaforme desktop diverse, o alcune versioni diverse dello stesso browser sulla stessa piattaforma) per vedere quanto ampiamente persiste il bug.

Potrebbe non essere colpa sua — se un bug esiste in un browser, allora si spera che il fornitore lo risolva rapidamente. Potrebbe essere già stato corretto — ad esempio se un bug è presente nella versione 49 di Firefox, ma non è più presente in Firefox Nightly (versione 52), allora lo hanno corretto. Se non è stato corretto, potrebbe voler segnalare un bug (vedere [Segnalare bug](#segnalare_bug), sotto).

Se è colpa sua, deve correggerlo! Scoprire la causa del bug comporta la stessa strategia di qualsiasi bug di sviluppo web (ancora una volta, si veda [Debugging HTML](/it/docs/Learn_web_development/Core/Structuring_content/Debugging_HTML), [Debugging CSS](/it/docs/Learn_web_development/Core/Styling_basics/Debugging_CSS), e [What went wrong? Troubleshooting JavaScript](/it/docs/Learn_web_development/Core/Scripting/What_went_wrong)). Una volta scoperto cosa causa il suo bug, deve decidere come aggirarlo nel particolare browser in cui sta causando problemi — non può semplicemente cambiare il codice del problema tout court, poiché ciò potrebbe rompere il codice in altri browser. L'approccio generale è solitamente quello di biforcare il codice in qualche modo, ad esempio usare il codice di rilevamento delle funzionalità JavaScript per rilevare situazioni in cui una funzionalità problematica non funziona, ed eseguire del codice diverso in quei casi che funziona.

Una volta effettuata una correzione, vorrà ripetere il suo processo di test per assicurarsi che la sua correzione funzioni correttamente, e non abbia causato malfunzionamenti del sito in altri punti o in altri browser.

## Segnalare bug

Per ribadire quanto detto sopra, se scopre bug nei browser, dovrebbe segnalarli:

- [Firefox Bugzilla](https://bugzilla.mozilla.org/)
- [Safari](https://bugs.webkit.org/)
- [Chrome](https://issues.chromium.org/issues)
- [Opera](https://opera.atlassian.net/servicedesk/customer/portal/9)

## Sommario

Questo articolo dovrebbe averle dato una comprensione di alto livello dei concetti più importanti che deve conoscere riguardo al test cross-browser. Armato di questa conoscenza, è ora pronto a passare oltre e iniziare a imparare le strategie di test cross-browser.

{{NextMenu("Learn_web_development/Extensions/Testing/Testing_strategies", "Learn_web_development/Extensions/Testing")}}
