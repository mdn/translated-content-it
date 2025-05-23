---
title: Strategie per eseguire i test
short-title: Strategie di collaudo
slug: Learn_web_development/Extensions/Testing/Testing_strategies
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Introduction","Learn_web_development/Extensions/Testing/HTML_and_CSS", "Learn_web_development/Extensions/Testing")}}

Questo articolo spiega come eseguire il collaudo cross-browser: come scegliere quali browser e dispositivi testare, come testare effettivamente quei browser e dispositivi, e come testare con gruppi di utenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi principali <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei principi di alto livello
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >del collaudo cross-browser</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire una comprensione dei concetti di alto livello coinvolti nel
        collaudo cross-browser.
      </td>
    </tr>
  </tbody>
</table>

## Scegliere quali browser e dispositivi testare

Poiché non puoi testare ogni combinazione di browser e dispositivo, è sufficiente assicurarsi che il tuo sito funzioni sui più importanti. Nelle applicazioni pratiche, "importante" spesso significa "comunemente utilizzato dal pubblico di riferimento".

Puoi classificare browser e dispositivi in base al livello di supporto che intendi fornire. Ad esempio:

1. A-grade: Browser comuni/moderni — Conosciuti per essere capaci. Test approfonditi e supporto completo.
2. B-grade: Browser più vecchi/meno capaci — noti per non essere particolarmente capaci. Testare e fornire un'esperienza più basilare che consenta comunque pieno accesso alle informazioni e servizi di base.
3. C-grade: Browser rari/sconosciuti — non testare, ma assumere che siano capaci. Servire il sito completo, che dovrebbe funzionare, almeno con i fallback forniti dalla nostra codifica difensiva.

Nelle sezioni seguenti, costruiremo una tabella di supporto in questo formato.

> [!NOTE]
> Yahoo ha reso popolare questo approccio per primo, con il loro approccio [Graded Browser Support](https://github.com/yui/yui3/wiki/Graded-Browser-Support).

### Prevedere i browser più comunemente usati dal tuo pubblico

Questo tipicamente comporta fare congetture informate basate su demografia degli utenti. Ad esempio, supponiamo che i tuoi utenti si trovino in Nord America e Europa occidentale:

Una rapida ricerca online ti dice che la maggior parte delle persone in Nord America e Europa occidentale utilizza desktop/laptop Windows o Mac, dove i browser principali sono Chrome, Firefox, Safari ed Edge. Probabilmente vorrai testare solo le versioni più recenti di questi browser, poiché ricevono aggiornamenti regolari. Questi dovrebbero essere tutti inseriti nel livello A-grade.

La maggior parte delle persone in questa demografia utilizza anche telefoni iOS o Android, quindi probabilmente vorresti testare le versioni più recenti di Safari per iOS, le ultime due versioni del vecchio browser stock di Android, e Chrome e Firefox per iOS e Android. Idealmente, dovresti testare questi su entrambi un telefono e un tablet, per assicurarti che i design responsive funzionino.

Opera Mini non è molto capace di eseguire JavaScript complesso, quindi dovremmo metterlo anche nel grado B.

Così, abbiamo basato la nostra scelta di quali browser testare sui browser che ci aspettiamo che i nostri utenti utilizzino.
Questo ci dà la seguente tabella di supporto finora:

1. A-grade: Chrome e Firefox per Windows/Mac, Safari per Mac, Edge per Windows, Safari per iOS su iPhone/iPad, browser stock Android (ultime due versioni) su telefono/tablet, Chrome, e Firefox per Android (ultime due versioni) su telefono/tablet
2. B-grade: Opera Mini
3. C-grade: n/a

Se il tuo pubblico di riferimento si trova principalmente altrove, allora i browser e i sistemi operativi più comuni potrebbero differire da quelli sopra riportati.

> [!NOTE]
> "Il CEO della mia azienda usa un Blackberry, quindi faremmo meglio a garantire che sembri buono su quello" potrebbe essere anche qualcosa da considerare.

### Statistiche sui browser

Alcuni siti web mostrano quali browser sono popolari in una determinata regione. Ad esempio, [Statcounter](https://gs.statcounter.com/) fornisce un'idea delle tendenze in Nord America.

### Utilizzare analytics

Una fonte di dati molto più accurata, se puoi ottenerla, è un'app di analytics come [Google Analytics](https://marketingplatform.google.com/about/analytics/), che ti dice esattamente quali browser le persone stanno usando per navigare il tuo sito. Naturalmente, ciò si basa sul fatto che tu abbia già un sito su cui utilizzarlo, quindi non è utile per siti completamente nuovi.

Potresti anche considerare l'uso di piattaforme di analisi open-source e incentrate sulla privacy come [Open Web Analytics](https://www.openwebanalytics.com/) e [Matomo](https://matomo.org/). Queste si aspettano che tu ospiti autonomamente la piattaforma di analisi.

#### Configurazione di Google Analytics

1. Prima di tutto, avrai bisogno di un account Google. Usa questo account per accedere a [Google Analytics](https://marketingplatform.google.com/about/analytics/).
2. Scegli l'opzione [Google Analytics](https://analytics.google.com/analytics/web/) (web) e fai clic sul pulsante _Sign Up_.
3. Inserisci i dettagli del tuo sito/app nella pagina di registrazione. Questo è abbastanza intuitivo da configurare; il campo più importante da impostare correttamente è l'URL del sito web. Questo deve essere l'URL radice del tuo sito/app.
4. Una volta che hai finito di compilare tutto, premi il pulsante _Get Tracking ID_, quindi accetta i termini di servizio che appaiono.
5. La pagina successiva ti fornisce alcuni frammenti di codice e altre istruzioni. Per un sito web di base, quello di cui hai bisogno è copiare il blocco di codice di _Website tracking_ e incollarlo in tutte le pagine diverse che vuoi tracciare utilizzando Google Analytics sul tuo sito. Potresti posizionare i frammenti sotto il tuo tag di chiusura `</body>`, o in un'altra posizione appropriata che li tenga separati dal tuo codice applicativo.
6. Carica le modifiche sul server di sviluppo o ovunque altro tu abbia bisogno del tuo codice.

Ecco fatto! Il tuo sito dovrebbe ora essere pronto per iniziare a riportare i dati analitici.

#### Studio dei dati analitici

Ora dovresti essere in grado di tornare alla homepage di [Analytics Web](https://analytics.google.com/analytics/web/), e iniziare a guardare i dati che hai raccolto sul tuo sito (devi aspettare un po' di tempo per raccogliere effettivamente dei dati, ovviamente).

Per impostazione predefinita, dovresti vedere la scheda dei report, come segue:

![Come Google Analytics raccoglie i dati nella sua dashboard principale di reportistica](analytics-reporting.png)

Ci sono una grandissima quantità di dati che puoi osservare usando Google Analytics — report personalizzati in diverse categorie, ecc. — e non abbiamo il tempo di discuterne tutti.
[Introduzione a Google Analytics](https://support.google.com/analytics/answer/9304153) fornisce alcune utili indicazioni sulla reportistica (e altro) per i principianti.

Puoi vedere quali browser e sistemi operativi i tuoi utenti stanno utilizzando selezionando _Audience > Technology > Browser & OS_ dal menu a sinistra.

> [!NOTE]
> Quando si utilizza Google Analytics, è necessario stare attenti ai bias fuorvianti, ad esempio, "Non abbiamo utenti di Firefox Mobile" potrebbe spingerti a non supportare Firefox Mobile. Ma non avrai utenti di Firefox Mobile se il sito era rotto su Firefox Mobile in primo luogo.

### Altre considerazioni

Dovresti includere l'accessibilità come requisito di test di grado A.

Inoltre, dovresti essere consapevole delle esigenze specifiche della situazione. Ad esempio, se il tuo prodotto prende di mira un mercato in cui i telefoni cellulari sono il mezzo principale per accedere a internet, probabilmente vorrai dare priorità al supporto del browser mobile.

### Tabella finale di supporto

Quindi, la nostra tabella finale di supporto finirà per apparire così:

1. A-grade: Chrome e Firefox per Windows/Mac, Safari per Mac, ed Edge (ultime due versioni di ciascuno), Safari per iOS su iPhone/iPad, browser stock Android (ultime due versioni) su telefono/tablet, Chrome, e Firefox per Android (ultime due versioni) su telefono/tablet. Accessibilità che supera i test comuni.
2. B-grade: Opera Mini.
3. C-grade: Opera, altri browser moderni di nicchia.

## Cosa stai per testare?

Quando hai un nuovo elemento nel tuo codice che necessita di collaudo, prima di iniziare è bene scrivere una lista di requisiti di test che devono essere superati per essere accettati. Questi requisiti possono essere visivi o funzionali — entrambi si combinano per formare una funzionalità web utilizzabile.

Considera il seguente esempio (vedi il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/strategies/hidden-info-panel.html), e anche l'[esempio live in esecuzione](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/strategies/hidden-info-panel.html)):

![Come preparare uno scenario di test che include i requisiti di design e dell'utente](sliding-box-demo.png)

I criteri di test per questa funzionalità potrebbero essere scritti in questo modo:

Gradi A e B:

- Il pulsante dovrebbe essere attivabile dal meccanismo di controllo principale dell'utente, qualunque esso sia — questo dovrebbe includere mouse, tastiera, e touch.
- Attivando il pulsante dovrebbe far apparire/scomparire la casella delle informazioni.
- Il testo dovrebbe essere leggibile.
- Gli utenti ipovedenti che utilizzano lettori di schermo dovrebbero essere in grado di accedere al testo.

Grado A:

- La casella delle informazioni dovrebbe animarsi fluidamente mentre appare/scompare.
- Il gradient e l'ombra del testo dovrebbero apparire per migliorare l'aspetto della casella.

Potresti notare che il pulsante non è utilizzabile solo con la tastiera. Potremmo rimediare a questo usando JavaScript per implementare un controllo da tastiera per il toggle, o usare un altro approccio.

Questi criteri di test sono utili, perché:

- Ti danno una serie di passaggi da seguire quando esegui i test.
- Possono essere facilmente trasformati in serie di istruzioni per i gruppi di utenti da seguire durante l'esecuzione dei test (ad esempio, "prova ad attivare il pulsante usando il mouse, e poi la tastiera...") — vedi [User testing](#test_utenti), sotto.
- Possono anche fornire una base per scrivere test automatizzati. È più facile scrivere tali test se si sa esattamente cosa si vuole testare, e quali sono le condizioni di successo (vedi [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#selenium), più avanti nella serie).

## Mettere insieme un laboratorio di test

Un'opzione per eseguire test sui browser è di fare i test da soli. Per fare ciò, probabilmente userai una combinazione di dispositivi fisici reali e ambienti emulati (utilizzando un emulatore o una macchina virtuale).

### Dispositivi fisici

In generale, è meglio avere un dispositivo reale che esegua il browser che vuoi testare — questo fornisce la massima accuratezza in termini di comportamento ed esperienza utente complessiva. Probabilmente vorrai qualcosa come il seguente, per un laboratorio di dispositivi di livello ragionevolmente basso:

- Un Mac, con installati i browser che devi testare — tale elenco può includere Firefox, Chrome, Opera e Safari.
- Un PC Windows, con installati i browser che devi testare — tale elenco può includere Edge (o IE), Chrome, Firefox e Opera.
- Un telefono e un tablet Android di fascia alta con il browser installato che devi testare — questo può includere Chrome, Firefox e Opera Mini per Android, così come il browser Android stock originale.
- Un telefono e un tablet iOS di fascia alta con i browser installati che devi testare — questo può includere Safari per iOS e Chrome, Firefox, e Opera Mini per iOS.

Le seguenti sono anche buone opzioni, se puoi ottenerle:

- Un PC Linux disponibile, nel caso fosse necessario testare bug specifici delle versioni di browser Linux. Gli utenti Linux utilizzano comunemente Firefox, Opera e Chrome. Se hai solo una macchina disponibile, potresti considerare di creare una macchina a doppio avvio che esegue Linux e Windows su partizioni separate. L'installatore di Ubuntu rende questo abbastanza semplice da configurare; vedi [WindowsDualBoot](https://help.ubuntu.com/community/WindowsDualBoot) per aiuto in questo senso.
- Un paio di dispositivi mobili di fascia bassa, così puoi testare le prestazioni di funzionalità come le animazioni su processori meno potenti.

La tua macchina di lavoro principale può anche essere un luogo dove installare altri strumenti per scopi specifici, come strumenti di auditing per l'accessibilità, lettori di schermo e emulatori/macchine virtuali.

Alcune aziende più grandi hanno laboratori di dispositivi che vantano una selezione molto ampia di dispositivi differenti, consentendo agli sviluppatori di scovare bug su combinazioni molto specifiche di browser/dispositivi. Aziende più piccole e singoli individui generalmente non possono permettersi un laboratorio così sofisticato, quindi si arrangiano con laboratori più piccoli, emulatori, macchine virtuali e app di collaudo commerciali.

Parleremo di ciascuna delle altre opzioni qui sotto.

> [!NOTE]
> Alcuni sforzi sono stati fatti per creare laboratori di dispositivi accessibili al pubblico — vedi [Open Device Labs](https://www.smashingmagazine.com/2016/11/worlds-best-open-device-labs/).

> [!NOTE]
> Dobbiamo anche considerare l'accessibilità — ci sono diversi strumenti utili che puoi installare sulla tua macchina per facilitare i test di accessibilità, ma li copriremo nell'articolo Gestire i problemi comuni di accessibilità, più avanti nel corso.

### Emulatori

Gli emulatori sono sostanzialmente programmi che girano all'interno del tuo computer ed emulano un dispositivo o particolari condizioni di dispositivo in qualche modo, permettendoti di fare alcuni dei tuoi test in modo più comodo che dover trovare una particolare combinazione di hardware/software da testare.

Un emulatore potrebbe essere semplice come testare una condizione di dispositivo. Ad esempio, se vuoi fare rapidamente alcuni test approssimativi delle tue media query di larghezza/altezza per il design responsive, puoi usare la [Modalità Responsive Design](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/index.html) di Firefox. Anche Safari ha una modalità simile, che può essere abilitata andando su _Safari > Preferences_, e selezionando _Show Develop menu_, quindi scegliendo _Develop > Enter Responsive Design Mode_. Chrome ha anche qualcosa di simile: la modalità dispositivo (vedi [Simulate Mobile Devices with Device Mode](https://developer.chrome.com/docs/devtools/device-mode/)).

Tuttavia, più spesso, dovrai installare qualche tipo di emulatore. I dispositivi/browser più comuni che vorrai testare sono i seguenti:

- L'IDE ufficiale [Android Studio](https://developer.android.com/studio/) per sviluppare app Android è un po' pesante per testare solo siti web su Google Chrome o sul vecchio browser stock Android, ma viene fornito con un robusto [emulatore](https://developer.android.com/studio/run/emulator.html). Se vuoi qualcosa di un po' più leggero, [Andy](https://www.andyroid.net/) è un'opzione ragionevole che funziona su entrambi Windows e Mac.
- Apple fornisce un'app chiamata [Simulator](https://help.apple.com/simulator/mac/current/) che gira sopra all'ambiente di sviluppo [XCode](https://developer.apple.com/xcode/) ed emula iPad/iPhone/Apple Watch/Apple TV. Questo include il browser nativo Safari per iOS. Sfortunatamente, questo funziona solo su un Mac.

Spesso puoi trovare simulatori per altri ambienti di dispositivi mobili, per esempio:

- Puoi emulare Opera Mini da solo se vuoi testarlo.

> [!NOTE]
> Molti emulatori richiedono in realtà l'uso di una macchina virtuale (vedi sotto); quando è così, le istruzioni spesso sono fornite, e/o l'uso della macchina virtuale è incorporato nell'installatore dell'emulatore.

### Macchine virtuali

Le macchine virtuali sono applicazioni che girano sul tuo computer desktop e ti permettono di eseguire emulazioni di sistemi operativi completi, ciascuna compartimentata nel proprio hard disk virtuale (spesso rappresentata da un unico grande file esistente sul disco rigido della macchina host). Ci sono numerose app di macchine virtuali popolari disponibili, come [Parallels](https://www.parallels.com/), [VMware](https://www.vmware.com/), e [Virtual Box](https://www.virtualbox.org/wiki/Downloads); personalmente ci piace l'ultima, perché è gratuita.

> [!NOTE]
> Hai bisogno di molto spazio su disco rigido per eseguire emulazioni di macchine virtuali; ogni sistema operativo che emulavi può occupare molta memoria. Tendi a scegliere lo spazio su disco rigido che vuoi per ogni installazione; potresti cavartela con probabilmente 10GB, ma alcune fonti consigliano fino a 50GB o più, così il sistema operativo può girare in modo affidabile. Una buona opzione fornita dalla maggior parte delle app di macchine virtuali è di creare un hard disk **allocato dinamicamente** che cresce e si restringe al bisogno.

Per usare una Virtual Box, devi:

1. Procurarti un disco di installazione o un'immagine (ad es., file ISO) per il sistema operativo che vuoi emulare. Virtual Box non è in grado di fornire questi; la maggior parte, come i sistemi operativi Windows, sono prodotti commerciali che non possono essere distribuiti liberamente.
2. [Scarica l'installer appropriato](https://www.virtualbox.org/wiki/Downloads) per il tuo sistema operativo e installalo.
3. Apri l'app; ti verrà presentata una visualizzazione simile alla seguente: ![La finestra dell'applicazione nel pannello a sinistra elenca sistema operativo Windows e emulatori di Opera TV. Il pannello di destra include numerosi sottopannelli tra cui generale, sistema, display, impostazioni, audio, rete e un'anteprima.](virtualbox.png)
4. Per creare una nuova macchina virtuale, premi il pulsante _New_ nell'angolo in alto a sinistra.
5. Segui le istruzioni e compila le seguenti finestre di dialogo in modo appropriato. Dovrai:

   1. Fornire un nome per la nuova macchina virtuale
   2. Scegliere quale sistema operativo e versione stai installando
   3. Impostare quanta RAM dovrebbe essere allocata (consigliamo qualcosa come 2048MB, o 2GB)
   4. Creare un hard disk virtuale (scegli le opzioni predefinite su tutte e tre le finestre di dialogo che contengono _Create a virtual hard disk now_, _VDI (virtual disk image)_, e _Dynamically allocated_).
   5. Scegli il nome del file e la dimensione per l'hard disk virtuale (scegli un nome sensibile e un luogo per mantenerlo, e per la dimensione specifica circa 50GB, o quanto sei a tuo agio nel specificare).

Ora la nuova box virtuale dovrebbe apparire nel menu a sinistra dell'interfaccia utente principale di Virtual Box. A questo punto, puoi fare doppio clic per aprirla — inizierà ad avviare la macchina virtuale, ma non avrà ancora il sistema operativo (OS) installato. A questo punto devi puntare la finestra di dialogo all'immagine/disco dell'installatore, e eseguirà i passaggi per installare l'OS proprio come su una macchina fisica.

![Come installare la virtual Box per un sistema operativo specifico](virtualbox-installer.png)

> [!WARNING]
> Devi assicurarti di avere l'immagine del sistema operativo che vuoi installare sulla macchina virtuale disponibile a questo punto, e installarla subito. Se annulli il processo a questo punto, può rendere la macchina virtuale inutilizzabile, e rendere necessario eliminarla e crearla di nuovo. Questo non è fatale, ma è fastidioso.

Dopo che il processo è completato, dovresti avere una macchina virtuale che esegue un sistema operativo all'interno di una finestra sul tuo computer host.

![Screenshot di Windows XP, ospitato in Virtual box, e in esecuzione su macOS](virtualbox-running.png)

Devi trattare questa installazione di sistema operativo virtuale proprio come faresti con qualsiasi installazione reale — per esempio, oltre a installare i browser che vuoi testare, installa un programma antivirus per proteggerti dai virus.

Avere più macchine virtuali è molto utile, in particolare per i test su Windows IE/Edge — su Windows, non sei in grado di avere più versioni del browser predefinito installate affiancate, quindi potresti voler costruire una libreria di macchine virtuali per gestire diversi test come necessario, ad esempio:

- Windows 10 con Edge 14
- Windows 10 con Edge 13

> [!NOTE]
> Un'altra buona cosa delle macchine virtuali è che le immagini del disco virtuale sono abbastanza autonome. Se stai lavorando su un team, puoi creare un'immagine di disco virtuale, quindi copiarla e passarla in giro. Assicurati solo di avere le licenze richieste per eseguire tutte quelle copie di Windows o qualsiasi altro prodotto che stai eseguendo se è un prodotto con licenza.

### Automazione e app commerciali

Come menzionato nell'ultimo capitolo, puoi eliminare gran parte del dolore del collaudo su browser utilizzando qualche tipo di sistema di automazione. Puoi impostare il tuo sistema di automazione dei test ([Selenium](https://www.selenium.dev/) essendo l'app popolare di scelta), che richiede un po' di configurazione, ma può essere molto gratificante quando riesci a farlo funzionare.

Ci sono anche strumenti commerciali disponibili come [Sauce Labs](https://saucelabs.com/), [Browser Stack](https://www.browserstack.com/) e [LambdaTest](https://www.lambdatest.com/) che fanno questo tipo di cosa per te, senza dover preoccuparti della configurazione, se desideri investire un po' di denaro nel tuo collaudo.

Un'altra alternativa è usare strumenti di automazione dei test senza codice come [Endtest](https://www.endtest.io/).

Esamineremo come utilizzare questi strumenti più avanti nel modulo.

## Test utenti

Prima di procedere, finiremo questo articolo parlando un po' di test utenti — questo può essere una buona opzione se hai un gruppo di utenti disposti a testare la tua nuova funzionalità. Tieni presente che questo può essere tanto lo-fi quanto sofisticato desideri — il tuo gruppo di utenti potrebbe essere un gruppo di amici, un gruppo di colleghi, o un gruppo di volontari non pagati o pagati, a seconda se hai a disposizione del denaro per i test.

Generalmente farai vedere agli utenti la pagina o la vista contenente la nuova funzionalità su qualche tipo di server di sviluppo, quindi non pubblicherai il sito finale o la modifica finché non è finito. Dovresti far loro seguire alcuni passi e riportare i risultati che ottengono. È utile fornire una serie di passi (a volte chiamata script) così che ottieni risultati più affidabili relativi a ciò che stavi cercando di testare. Abbiamo menzionato questo nella sezione [Cosa stai per testare](#what_are_you_going_to_test) sopra — è facile trasformare i criteri di test dettagliati lì in passi da seguire. Ad esempio, il seguente funzionerebbe per un utente vedente:

- Clicca il pulsante con il punto interrogativo usando il mouse sul tuo computer desktop alcune volte. Aggiorna la finestra del browser.
- Seleziona e attiva il pulsante con il punto interrogativo usando la tastiera sul tuo computer desktop alcune volte.
- Tocca il pulsante con il punto interrogativo alcune volte sul tuo dispositivo touch screen.
- Attivare il pulsante dovrebbe far apparire/scomparire la casella delle informazioni. Lo fa, in ciascuno dei tre casi sopra menzionati?
- Il testo è leggibile?
- La casella delle informazioni si anima fluidamente mentre appare/scompare?

Quando si eseguono i test, può anche essere una buona idea:

- Impostare un profilo del browser separato, se possibile, con estensioni del browser e altre cose simili disabilitate, ed eseguire i tuoi test in quel profilo (vedi [Utilizzare il Profile Manager per creare e rimuovere profili di Firefox](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles) e [Condividi Chrome con altri o aggiungi persone](https://support.google.com/chrome/answer/2364824), per esempio).
- Utilizzare la funzionalità modalità privata del browser quando si eseguono test, dove disponibile (ad esempio, [Navigazione Privata](https://support.mozilla.org/en-US/kb/private-browsing-use-firefox-without-history) in Firefox, [Modalità Incognito](https://support.google.com/chrome/answer/95464) in Chrome) in modo che cose come cookie e file temporanei non vengano salvati.

Questi passaggi sono progettati per assicurarsi che il browser che stai testando sia il più "puro" possibile, cioè non ci sia nulla installato che potrebbe influenzare i risultati dei test.

> [!NOTE]
> Un'altra utile opzione lo-fi, se hai l'hardware disponibile, è testare i tuoi siti su telefoni di fascia bassa/altri dispositivi — poiché i siti diventano sempre più grandi e presentano più effetti, c'è una maggiore probabilità che il sito rallenti, quindi devi iniziare a dare maggiore considerazione alle prestazioni. Cercare di far funzionare la tua funzionalità su un dispositivo di fascia bassa renderà più probabile che l'esperienza sia buona su dispositivi di fascia alta.

> [!NOTE]
> Alcuni ambienti di sviluppo lato server forniscono meccanismi utili per implementare modifiche al sito solo a un sottoinsieme di utenti, fornendo un meccanismo utile per far testare una funzionalità da un sottoinsieme di utenti senza bisogno di un server di sviluppo separato. Un esempio è [Django Waffle Flags](https://github.com/jazzband/django-waffle).

## Riepilogo

Dopo aver letto questo articolo dovresti ora avere una buona idea di cosa puoi fare per identificare il tuo pubblico di riferimento/elenco di browser target, quindi eseguire efficacemente il collaudo cross-browser su quell'elenco.

Successivamente, rivolgeremo la nostra attenzione ai problemi di codice reali che i tuoi test potrebbero scoprire, iniziando con HTML e CSS.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Introduction","Learn_web_development/Extensions/Testing/HTML_and_CSS", "Learn_web_development/Extensions/Testing")}}
