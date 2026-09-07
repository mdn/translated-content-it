---
title: Strategie per eseguire i test
short-title: Strategie di test
slug: Learn_web_development/Extensions/Testing/Testing_strategies
l10n:
  sourceCommit: c53bfa01f3bf436d486f4032c16f592855a2af2c
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Introduction","Learn_web_development/Extensions/Testing/HTML_and_CSS", "Learn_web_development/Extensions/Testing")}}

Questo articolo spiega come eseguire test cross-browser: come scegliere quali browser e dispositivi testare, come testare effettivamente tali browser e dispositivi e come effettuare test con gruppi di utenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi fondamentali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; una conoscenza
        dei principi di alto livello del
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >testing cross-browser</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire una comprensione dei concetti di alto livello coinvolti
        nel testing cross-browser.
      </td>
    </tr>
  </tbody>
</table>

## Scegliere quali browser e dispositivi testare

Poiché non è possibile testare ogni combinazione di browser e dispositivo, è sufficiente assicurarsi che il sito funzioni su quelle più importanti. Nelle applicazioni pratiche, «importanti» spesso significa «comunemente usati dal pubblico di destinazione».

È possibile classificare browser e dispositivi in base al livello di supporto che si intende fornire. Ad esempio:

1. Livello A: browser comuni/moderni — noti per essere capaci. Testarli approfonditamente e fornire supporto completo.
2. Livello B: browser meno recenti/meno capaci — noti per non essere capaci. Testarli e fornire un'esperienza più basilare che dia accesso completo alle informazioni e ai servizi essenziali.
3. Livello C: browser rari/sconosciuti — non testarli, ma presumere che siano capaci. Fornire il sito completo, che dovrebbe funzionare, almeno con i fallback offerti dalla programmazione difensiva.

Nelle sezioni seguenti, verrà costruita una tabella di supporto in questo formato.

> [!NOTE]
> Yahoo ha reso popolare per prima questo approccio, con il proprio approccio [Graded browser Support](https://github.com/yui/yui3/wiki/Graded-Browser-Support).

### Prevedere i browser più comunemente usati dal pubblico

In genere, ciò comporta fare ipotesi informate basate sui dati demografici degli utenti. Ad esempio, si supponga che gli utenti si trovino in Nord America e nell'Europa occidentale:

Una rapida ricerca online indica che la maggior parte delle persone in Nord America e nell'Europa occidentale usa computer desktop/laptop Windows o Mac, nei quali i browser principali sono Chrome, Firefox, Safari ed Edge. Probabilmente sarà opportuno testare soltanto le versioni più recenti di questi browser, poiché ricevono aggiornamenti regolari. Tutti questi dovrebbero rientrare nel livello A.

La maggior parte delle persone di questo gruppo demografico usa anche telefoni iOS o Android, quindi probabilmente sarà opportuno testare le versioni più recenti di iOS Safari, le ultime due versioni del vecchio browser Android stock, nonché Chrome e Firefox per iOS e Android. Idealmente, questi dovrebbero essere testati sia su telefono sia su tablet, per assicurarsi che i design responsive funzionino.

Opera Mini non è molto adatto all'esecuzione di JavaScript complesso, pertanto dovrebbe essere incluso anch'esso nel livello B.

La scelta dei browser da testare è quindi basata sui browser che ci si aspetta vengano usati dagli utenti.
Finora, ciò produce la seguente tabella di supporto:

1. Livello A: Chrome e Firefox per Windows/Mac, Safari per Mac, Edge per Windows, iOS Safari per iPhone/iPad, browser Android stock (ultime due versioni) su telefono/tablet, Chrome e Firefox per Android (ultime due versioni) su telefono/tablet
2. Livello B: Opera Mini
3. Livello C: n/d

Se il pubblico di destinazione si trova prevalentemente altrove, i browser e i sistemi operativi più comuni potrebbero differire da quelli indicati sopra.

> [!NOTE]
> Anche «L'amministratore delegato dell'azienda usa un Blackberry, quindi è meglio assicurarsi che l'aspetto sia adeguato su quel dispositivo» può essere un aspetto da considerare.

### Statistiche sui browser

Alcuni siti web mostrano quali browser sono popolari in una determinata regione. Ad esempio, [Statcounter](https://gs.statcounter.com/) fornisce un'idea delle tendenze in Nord America.

### Uso delle analitiche

Una fonte di dati molto più precisa, se disponibile, è un'app di analitiche come [Google Analytics](https://marketingplatform.google.com/about/analytics/), che indica esattamente quali browser le persone usano per navigare il sito. Naturalmente, questo presuppone che esista già un sito su cui utilizzarla, quindi non è adatta per siti completamente nuovi.

Si può anche considerare l'uso di piattaforme di analitiche open source e incentrate sulla privacy, come [Open Web Analytics](https://www.openwebanalytics.com/) e [Matomo](https://matomo.org/). Queste richiedono l'hosting autonomo della piattaforma di analitiche.

#### Configurare Google Analytics

1. Prima di tutto, è necessario un account Google. Usare questo account per accedere a [Google Analytics](https://marketingplatform.google.com/about/analytics/).
2. Scegliere l'opzione [Google Analytics](https://analytics.google.com/analytics/web/) (web) e fare clic sul pulsante _Sign Up_.
3. Inserire i dettagli del sito web/app nella pagina di registrazione. La configurazione è piuttosto intuitiva; il campo più importante da compilare correttamente è l'URL del sito web. Deve essere l'URL radice del sito/app.
4. Dopo aver compilato tutti i dati, premere il pulsante _Get Tracking ID_, quindi accettare i termini di servizio visualizzati.
5. La pagina successiva fornisce alcuni frammenti di codice e altre istruzioni. Per un sito web di base, occorre copiare il blocco di codice _Website tracking_ e incollarlo in tutte le pagine che si desidera monitorare con Google Analytics sul sito. È possibile collocare i frammenti sotto il tag di chiusura `</body>` oppure in un'altra posizione appropriata che eviti di confonderli con il codice dell'applicazione.
6. Caricare le modifiche sul server di sviluppo, o ovunque sia necessario caricare il codice.

Fatto! Il sito dovrebbe ora essere pronto per iniziare a riportare dati analitici.

#### Analizzare i dati delle analitiche

Ora dovrebbe essere possibile tornare alla homepage di [Analytics Web](https://analytics.google.com/analytics/web/) e iniziare a esaminare i dati raccolti sul sito (naturalmente, è necessario attendere un po' affinché vengano effettivamente raccolti dei dati).

Per impostazione predefinita, dovrebbe essere visualizzata la scheda dei report, come segue:

![Come Google Analytics raccoglie i dati nella sua dashboard principale di reportistica](analytics-reporting.png)

Esiste un'enorme quantità di dati che è possibile consultare tramite Google Analytics — report personalizzati in diverse categorie, ecc. — e non c'è tempo per discuterli tutti.
[Getting started with Analytics](https://support.google.com/analytics/answer/9304153) fornisce alcune indicazioni utili sui report (e altro) per principianti.

È possibile vedere quali browser e sistemi operativi usano gli utenti selezionando _Audience > Technology > Browser & OS_ dal menu a sinistra.

> [!NOTE]
> Quando si usa Google Analytics, occorre prestare attenzione a distorsioni fuorvianti; ad esempio, «Non abbiamo utenti Firefox Mobile» potrebbe indurre a non preoccuparsi di supportare Firefox mobile. Tuttavia, non ci saranno utenti Firefox Mobile se il sito era già non funzionante in Firefox mobile.

### Altre considerazioni

L'accessibilità dovrebbe essere inclusa come requisito di test di livello A.

Occorre inoltre essere consapevoli delle esigenze specifiche della situazione. Ad esempio, se il prodotto è destinato a un mercato in cui i telefoni cellulari sono il principale mezzo di accesso a Internet, probabilmente sarà opportuno dare priorità al supporto dei browser mobili.

### Tabella di supporto finale

La tabella di supporto finale sarà quindi la seguente:

1. Livello A: Chrome e Firefox per Windows/Mac, Safari per Mac ed Edge (ultime due versioni di ciascuno), iOS Safari per iPhone/iPad, browser Android stock (ultime due versioni) su telefono/tablet, Chrome e Firefox per Android (ultime due versioni) su telefono/tablet. Accessibilità che supera i test comuni.
2. Livello B: Opera Mini.
3. Livello C: Opera, altri browser moderni di nicchia.

## Che cosa verrà testato?

Quando viene aggiunta una nuova funzionalità al codebase che necessita di test, prima di iniziare i test occorre redigere un elenco di requisiti di test che devono essere soddisfatti affinché venga accettata. Questi requisiti possono essere visivi o funzionali: entrambi si combinano per creare una funzionalità del sito web utilizzabile.

Si consideri il seguente esempio (vedere il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/strategies/hidden-info-panel.html), nonché l'[esempio in esecuzione](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/strategies/hidden-info-panel.html)):

![Come preparare uno scenario di test che includa requisiti di design e degli utenti](sliding-box-demo.png)

I criteri di test per questa funzionalità potrebbero essere scritti come segue:

Livelli A e B:

- Il pulsante deve poter essere attivato tramite il meccanismo di controllo principale dell'utente, qualunque esso sia — ciò dovrebbe includere mouse, tastiera e tocco.
- L'attivazione/disattivazione del pulsante deve far apparire/scomparire il riquadro informativo.
- Il testo deve essere leggibile.
- Gli utenti ipovedenti che usano lettori di schermo devono poter accedere al testo.

Livello A:

- Il riquadro informativo deve animarsi in modo fluido quando appare/scompare.
- Il gradiente e l'ombra del testo devono apparire per migliorare l'aspetto del riquadro.

Si potrebbe notare che il pulsante non è utilizzabile solo con la tastiera. Si potrebbe rimediare usando JavaScript per implementare un controllo da tastiera per l'attivazione/disattivazione, oppure usando un altro approccio.

Questi criteri di test sono utili perché:

- Forniscono un insieme di passaggi da seguire durante l'esecuzione dei test.
- Possono essere facilmente trasformati in insiemi di istruzioni che i gruppi di utenti devono seguire durante i test (ad esempio, «provare ad attivare il pulsante usando il mouse, quindi la tastiera…») — vedere [Test degli utenti](#test_degli_utenti), più avanti.
- Possono inoltre fornire una base per scrivere test automatizzati. È più facile scrivere tali test se si sa esattamente che cosa si desidera testare e quali sono le condizioni di successo (vedere [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#selenium), più avanti nella serie).

## Allestire un laboratorio di test

Un'opzione per eseguire test sui browser consiste nel fare personalmente i test. Per farlo, probabilmente verrà utilizzata una combinazione di dispositivi fisici reali e ambienti emulati, usando un emulatore oppure una macchina virtuale.

### Dispositivi fisici

In generale, è preferibile avere un dispositivo reale che esegua il browser da testare — questo garantisce la massima accuratezza in termini di comportamento ed esperienza utente complessiva. Per un laboratorio di dispositivi di base ragionevole, probabilmente sarà necessario qualcosa come quanto segue:

- Un Mac, con installati i browser da testare — possono includere Firefox, Chrome, Opera e Safari.
- Un PC Windows, con installati i browser da testare — possono includere Edge (o IE), Chrome, Firefox e Opera.
- Un telefono e un tablet Android di fascia alta con installati i browser da testare — possono includere Chrome, Firefox e Opera Mini per Android, oltre al browser Android stock originale.
- Un telefono e un tablet iOS di fascia alta con installati i browser da testare — possono includere iOS Safari e Chrome, Firefox e Opera Mini per iOS.

Sono inoltre buone opzioni, se disponibili:

- Un PC Linux, nel caso sia necessario testare bug specifici delle versioni Linux dei browser. Gli utenti Linux usano comunemente Firefox, Opera e Chrome. Se è disponibile una sola macchina, si può considerare di creare una macchina dual boot che esegua Linux e Windows su partizioni separate.
- Un paio di dispositivi mobili di fascia inferiore, per testare le prestazioni di funzionalità come le animazioni su processori meno potenti.

La macchina di lavoro principale può anche essere un luogo in cui installare altri strumenti per scopi specifici, come strumenti di audit dell'accessibilità, lettori di schermo ed emulatori/macchine virtuali.

Alcune aziende più grandi dispongono di laboratori di dispositivi con una selezione molto ampia di dispositivi diversi, che consentono agli sviluppatori di individuare bug in combinazioni browser/dispositivo molto specifiche. Le aziende più piccole e i singoli individui generalmente non possono permettersi un laboratorio così sofisticato, quindi tendono ad accontentarsi di laboratori più piccoli, emulatori, macchine virtuali e app commerciali di testing.

Ciascuna delle altre opzioni verrà trattata di seguito.

> [!NOTE]
> Sono stati compiuti alcuni sforzi per creare laboratori di dispositivi accessibili pubblicamente — vedere [Open Device Labs](https://www.smashingmagazine.com/2016/11/worlds-best-open-device-labs/).

> [!NOTE]
> È inoltre necessario considerare l'accessibilità — esistono diversi strumenti utili che è possibile installare sulla propria macchina per facilitare i test di accessibilità, ma saranno trattati nell'articolo Gestione dei problemi comuni di accessibilità, più avanti nel corso.

### Emulatori

Gli emulatori sono essenzialmente programmi eseguiti all'interno del computer che emulano un dispositivo o particolari condizioni di un dispositivo, consentendo di eseguire alcuni test in modo più pratico rispetto alla ricerca di una particolare combinazione hardware/software da testare.

Un emulatore può essere semplice quanto testare una condizione del dispositivo. Ad esempio, per eseguire un test rapido e approssimativo delle media query di larghezza/altezza per il design responsive, è possibile usare la [Responsive Design Mode](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/index.html) di Firefox. Anche Safari dispone di una modalità simile, che può essere attivata andando in _Safari > Preferences_, selezionando _Show Develop menu_, quindi scegliendo _Develop > Enter Responsive Design Mode_. Anche Chrome dispone di qualcosa di simile: Device mode (vedere [Simulate Mobile Devices with Device Mode](https://developer.chrome.com/docs/devtools/device-mode/)).

Nella maggior parte dei casi, tuttavia, sarà necessario installare un qualche tipo di emulatore. I dispositivi/browser più comuni da testare sono i seguenti:

- L'[IDE Android Studio](https://developer.android.com/studio/) ufficiale per lo sviluppo di app Android è piuttosto pesante se serve soltanto per testare siti web in Google Chrome o nel vecchio browser Android Stock, ma include un robusto [emulatore](https://developer.android.com/studio/run/emulator.html).
- Apple fornisce un'app chiamata [Simulator](https://help.apple.com/simulator/mac/current/) che funziona sopra l'ambiente di sviluppo [Xcode](https://developer.apple.com/xcode/) ed emula iPad/iPhone/Apple Watch/Apple TV. Include il browser nativo iOS Safari. Sfortunatamente, funziona solo su Mac.

Spesso è possibile trovare simulatori anche per altri ambienti di dispositivi mobili, ad esempio:

- È possibile emulare Opera Mini autonomamente, se si desidera testarlo.

> [!NOTE]
> Molti emulatori richiedono effettivamente l'uso di una macchina virtuale (vedere sotto); in tal caso, spesso vengono fornite istruzioni e/o l'uso della macchina virtuale è integrato nell'installer dell'emulatore.

### Macchine virtuali

Le macchine virtuali sono applicazioni eseguite sul computer desktop e consentono di eseguire emulazioni di interi sistemi operativi, ciascuno isolato nel proprio disco rigido virtuale, spesso rappresentato da un singolo grande file presente sul disco rigido della macchina host. Sono disponibili diverse app popolari per macchine virtuali, come [Parallels](https://www.parallels.com/), [VMware](https://www.vmware.com/) e [Virtual Box](https://www.virtualbox.org/wiki/Downloads); quest'ultima è particolarmente consigliata perché è gratuita.

> [!NOTE]
> Per eseguire emulazioni di macchine virtuali è necessario molto spazio libero sul disco rigido; ogni sistema operativo emulato può occupare molta memoria. Generalmente si sceglie lo spazio su disco desiderato per ciascuna installazione; potrebbero bastare 10 GB, ma alcune fonti raccomandano fino a 50 GB o più, affinché il sistema operativo funzioni in modo affidabile. Una buona opzione fornita dalla maggior parte delle app per macchine virtuali è creare un disco rigido **allocato dinamicamente**, che cresce e si riduce secondo necessità.

Per usare Virtual Box, è necessario:

1. Procurarsi un disco o un'immagine di installazione, ad esempio un file ISO, del sistema operativo da emulare. Virtual Box non è in grado di fornirli; molti, come i sistemi operativi Windows, sono prodotti commerciali che non possono essere distribuiti liberamente.
2. [Scaricare l'installer appropriato](https://www.virtualbox.org/wiki/Downloads) per il proprio sistema operativo e installarlo.
3. Aprire l'app; verrà visualizzata una schermata simile alla seguente: ![La finestra dell'applicazione mostra nel pannello sinistro gli emulatori del sistema operativo Windows e di Opera TV. Il pannello destro include diversi sottopannelli, tra cui generale, sistema, schermo, impostazioni, audio, rete e un'anteprima.](virtualbox.png)
4. Per creare una nuova macchina virtuale, premere il pulsante _New_ nell'angolo in alto a sinistra.
5. Seguire le istruzioni e compilare le seguenti finestre di dialogo secondo necessità. Sarà necessario:
   1. Fornire un nome per la nuova macchina virtuale.
   2. Scegliere il sistema operativo e la versione da installare.
   3. Impostare la quantità di RAM da allocare (si consiglia qualcosa come 2048 MB, ovvero 2 GB).
   4. Creare un disco rigido virtuale (scegliere le opzioni predefinite nelle tre finestre di dialogo contenenti _Create a virtual hard disk now_, _VDI (virtual disk image)_ e _Dynamically allocated_).
   5. Scegliere la posizione e la dimensione del file per il disco rigido virtuale (scegliere un nome e una posizione adeguati in cui conservarlo e, per le dimensioni, specificare circa 50 GB oppure quanto ci si senta di assegnare).

Ora la nuova macchina virtuale dovrebbe apparire nel menu a sinistra della finestra principale dell'interfaccia di Virtual Box. A questo punto, è possibile fare doppio clic per aprirla: inizierà l'avvio della macchina virtuale, ma il sistema operativo (OS) non sarà ancora installato. A questo punto occorre indicare nella finestra di dialogo l'immagine o il disco di installazione, quindi verranno eseguiti i passaggi di installazione del sistema operativo proprio come su una macchina fisica.

![Come installare Virtual Box per un sistema operativo specifico](virtualbox-installer.png)

> [!WARNING]
> A questo punto è necessario assicurarsi di avere disponibile l'immagine del sistema operativo da installare sulla macchina virtuale e installarla subito. Se il processo viene annullato a questo punto, la macchina virtuale potrebbe diventare inutilizzabile e potrebbe essere necessario eliminarla e ricrearla. Non è fatale, ma è fastidioso.

Una volta completato il processo, dovrebbe essere disponibile una macchina virtuale che esegue un sistema operativo all'interno di una finestra sul computer host.

![Screenshot di Windows XP, ospitato in Virtual Box ed eseguito su macOS](virtualbox-running.png)

Questa installazione del sistema operativo virtuale deve essere trattata proprio come qualsiasi installazione reale: ad esempio, oltre a installare i browser da testare, occorre installare un programma antivirus per proteggerla dai virus.

Avere più macchine virtuali è molto utile, in particolare per il testing di Windows IE/Edge — su Windows non è possibile avere più versioni del browser predefinito installate affiancate, quindi potrebbe essere utile creare una libreria di macchine virtuali per gestire diversi test secondo necessità, ad esempio:

- Windows 10 con Edge 14
- Windows 10 con Edge 13

> [!NOTE]
> Un altro vantaggio delle macchine virtuali è che le immagini dei dischi virtuali sono piuttosto autonome. Se si lavora in un team, è possibile creare un'immagine di disco virtuale, quindi copiarla e distribuirla. È sufficiente assicurarsi di disporre delle licenze necessarie per eseguire tutte quelle copie di Windows o di qualsiasi altro prodotto con licenza in esecuzione.

### Automazione e app commerciali

Come accennato nel capitolo precedente, è possibile ridurre notevolmente la difficoltà del testing sui browser utilizzando un qualche tipo di sistema di automazione. È possibile configurare il proprio sistema di automazione dei test ([Selenium](https://www.selenium.dev/) è l'app popolare per eccellenza), che richiede una certa configurazione, ma può essere molto vantaggioso una volta compreso il suo funzionamento.

Sono inoltre disponibili strumenti commerciali come [Sauce Labs](https://saucelabs.com/) e [Browser Stack](https://www.browserstack.com/), che eseguono questo tipo di attività senza doversi preoccupare della configurazione, se si desidera investire del denaro nel testing.

Un'altra alternativa consiste nell'utilizzare strumenti di automazione dei test no-code come [Endtest](https://endtest.io/).

Vedremo come usare tali strumenti più avanti nel modulo.

## Test degli utenti

Prima di proseguire, questo articolo si conclude parlando brevemente dei test degli utenti — possono essere una buona opzione se si dispone di un gruppo di utenti disponibile a testare la nuova funzionalità. Occorre tenere presente che possono essere semplici o sofisticati quanto si desidera: il gruppo di utenti può essere un gruppo di amici, un gruppo di colleghi oppure un gruppo di volontari non retribuiti o retribuiti, a seconda del budget disponibile per i test.

In generale, agli utenti verrà chiesto di esaminare la pagina o la vista contenente la nuova funzionalità su un qualche tipo di server di sviluppo, in modo da non pubblicare il sito o la modifica finale finché non è completata. Dovrebbero seguire alcuni passaggi e riferire i risultati ottenuti. È utile fornire una serie di passaggi, talvolta chiamata script, per ottenere risultati più affidabili relativi a ciò che si intendeva testare. Questo è stato menzionato nella sezione [Che cosa verrà testato?](#what_are_you_going_to_test) sopra — è facile trasformare i criteri di test dettagliati in quella sezione in passaggi da seguire. Ad esempio, quanto segue funzionerebbe per un utente vedente:

- Fare clic alcune volte sul pulsante con il punto interrogativo usando il mouse sul computer desktop. Aggiornare la finestra del browser.
- Selezionare e attivare alcune volte il pulsante con il punto interrogativo usando la tastiera sul computer desktop.
- Toccare alcune volte il pulsante con il punto interrogativo sul dispositivo con schermo tattile.
- L'attivazione/disattivazione del pulsante dovrebbe far apparire/scomparire il riquadro informativo. Questo accade in tutti e tre i casi precedenti?
- Il testo è leggibile?
- Il riquadro informativo si anima in modo fluido quando appare/scompare?

Durante l'esecuzione dei test, può inoltre essere una buona idea:

- Configurare, quando possibile, un profilo del browser separato, con estensioni del browser e altri elementi simili disabilitati, ed eseguire i test in quel profilo (vedere [Use the Profile Manager to create and remove Firefox profiles](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles) e [Share Chrome with others or add personas](https://support.google.com/chrome/answer/2364824), ad esempio).
- Usare la funzionalità di navigazione privata del browser durante i test, quando disponibile, ad esempio [Private Browsing](https://support.mozilla.org/en-US/kb/private-browsing-use-firefox-without-history) in Firefox e [Incognito Mode](https://support.google.com/chrome/answer/95464) in Chrome, affinché elementi quali cookie e file temporanei non vengano salvati.

Questi passaggi sono progettati per assicurarsi che il browser in cui vengono effettuati i test sia il più «puro» possibile, ovvero che non vi sia nulla di installato che possa influenzare i risultati dei test.

> [!NOTE]
> Un'altra utile opzione semplice, se l'hardware è disponibile, consiste nel testare i siti su telefoni/dispositivi di fascia bassa — man mano che i siti diventano più grandi e includono più effetti, aumenta la possibilità che rallentino, quindi è necessario iniziare a considerare maggiormente le prestazioni. Cercare di far funzionare le funzionalità su un dispositivo di fascia bassa rende più probabile che l'esperienza sia buona anche su dispositivi di fascia alta.

> [!NOTE]
> Alcuni ambienti di sviluppo lato server forniscono meccanismi utili per distribuire modifiche al sito solo a un sottoinsieme di utenti, offrendo un meccanismo utile per testare una funzionalità con un sottoinsieme di utenti senza la necessità di un server di sviluppo separato. Un esempio è [Django Waffle Flags](https://github.com/django-waffle/django-waffle).

## Riepilogo

Dopo aver letto questo articolo, dovrebbe esserci una buona idea di ciò che è possibile fare per identificare il pubblico di destinazione/elenco di browser di destinazione e quindi eseguire efficacemente test cross-browser su tale elenco.

Successivamente, l'attenzione verrà rivolta ai problemi effettivi nel codice che i test potrebbero individuare, iniziando da HTML e CSS.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Introduction","Learn_web_development/Extensions/Testing/HTML_and_CSS", "Learn_web_development/Extensions/Testing")}}
