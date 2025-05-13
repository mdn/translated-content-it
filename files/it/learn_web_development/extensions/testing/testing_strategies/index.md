---
title: Strategie per eseguire i test
short-title: Strategie di testing
slug: Learn_web_development/Extensions/Testing/Testing_strategies
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Introduction","Learn_web_development/Extensions/Testing/HTML_and_CSS", "Learn_web_development/Extensions/Testing")}}

Questo articolo spiega come eseguire il test cross-browser: come scegliere quali browser e dispositivi testare, come testare effettivamente quei browser e dispositivi, e come testare con gruppi di utenti.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità con i linguaggi di base <a href="/it/docs/Learn_web_development/Core/Structuring_content">HTML</a>,
        <a href="/it/docs/Learn_web_development/Core/Styling_basics">CSS</a> e
        <a href="/it/docs/Learn_web_development/Core/Scripting">JavaScript</a>; un'idea
        dei principi di alto livello
        <a
          href="/it/docs/Learn_web_development/Extensions/Testing/Introduction"
          >del testing cross-browser</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Acquisire una comprensione dei concetti di alto livello coinvolti nel
        testing cross-browser.
      </td>
    </tr>
  </tbody>
</table>

## Scegliere quali browser e dispositivi testare

Poiché non si può testare ogni combinazione di browser e dispositivo, è sufficiente garantire che il sito funzioni sui più importanti. In applicazioni pratiche, "importante" spesso significa "comunemente usato tra il pubblico target."

È possibile classificare browser e dispositivi in base all'ammontare di supporto che si intende fornire. Ad esempio:

1. Grado A: Browser comuni/moderni — Noti per essere capaci. Testare accuratamente e fornire pieno supporto.
2. Grado B: Browser più vecchi/meno capaci — Noti per non essere capaci. Testare e fornire un'esperienza più basilare che dia pieno accesso alle informazioni e ai servizi principali.
3. Grado C: Browser rari/sconosciuti — non testare, ma assumere che siano capaci. Offrire il sito completo, che dovrebbe funzionare, almeno con i fallback previsti dal nostro codice difensivo.

Nelle sezioni seguenti, costruiremo una tabella di supporto in questo formato.

> [!NOTE]
> Yahoo ha reso popolare per la prima volta questo approccio, con il loro approccio di [Graded Browser Support](https://github.com/yui/yui3/wiki/Graded-Browser-Support).

### Prevedere i browser più comunemente usati dal Suo pubblico

Questo di solito comporta fare ipotesi informate basandosi sui dati demografici degli utenti. Ad esempio, supponiamo che i Suoi utenti siano in Nord America e Europa occidentale:

Una rapida ricerca online Le dirà che la maggior parte delle persone in Nord America e in Europa occidentale utilizza desktop/laptop Windows o Mac, dove i principali browser sono Chrome, Firefox, Safari ed Edge. Probabilmente vorrà testare solo le versioni più recenti di questi browser, poiché questi ricevono aggiornamenti regolari. Tutti questi dovrebbero rientrare nella categoria di grado A.

La maggior parte delle persone in questa demografia usa anche telefoni iOS o Android, quindi probabilmente vorrà testare le versioni più recenti di iOS Safari, le ultime due versioni del vecchio browser stock Android, e Chrome e Firefox per iOS e Android. Idealmente, dovrebbe testarli sia su un telefono che su un tablet, per garantire che i design responsivi funzionino.

Opera Mini non è molto capace di eseguire JavaScript complesso, quindi dovremmo inserirlo nel grado B.

Quindi, abbiamo basato la nostra scelta di quali browser testare sui browser che ci aspettiamo che i nostri utenti utilizzino.
Questo ci dà la seguente tabella di supporto fino ad ora:

1. Grado A: Chrome e Firefox per Windows/Mac, Safari per Mac, Edge per Windows, iOS Safari per iPhone/iPad, browser stock Android (ultime due versioni) su telefono/tablet, Chrome e Firefox per Android (ultime due versioni) su telefono/tablet
2. Grado B: Opera Mini
3. Grado C: n/d

Se il Suo pubblico target è per lo più situato altrove, allora i browser e i sistemi operativi più comuni potrebbero differire da quelli sopra indicati.

> [!NOTE]
> "Il CEO della mia azienda usa un Blackberry, quindi meglio assicurarci che abbia un aspetto gradevole su quel dispositivo" potrebbe anche essere qualcosa da considerare.

### Statistiche sui browser

Alcuni siti web mostrano quali browser sono popolari in una determinata regione. Ad esempio, [Statcounter](https://gs.statcounter.com/) fornisce un'idea delle tendenze in Nord America.

### Utilizzo degli analytics

Una fonte di dati molto più accurata, se può ottenerla, è un'app di analisi come [Google Analytics](https://marketingplatform.google.com/about/analytics/), che Le dice esattamente quali browser le persone usano per navigare nel Suo sito. Naturalmente, questo si basa sul fatto che lei abbia già un sito su cui utilizzarlo, quindi non è adatto per siti completamente nuovi.

Potrebbe anche considerare l'uso di piattaforme di analisi open-source e orientate alla privacy come [Open Web Analytics](https://www.openwebanalytics.com/) e [Matomo](https://matomo.org/). Queste prevedono che lei ospiti l'analisi in autonomia.

#### Configurare Google Analytics

1. Prima di tutto, avrà bisogno di un account Google. Utilizzi questo account per accedere a [Google Analytics](https://marketingplatform.google.com/about/analytics/).
2. Scegliere l'opzione [Google Analytics](https://analytics.google.com/analytics/web/) (web), e cliccare sul pulsante _Iscriviti_.
3. Immettere i dettagli del Suo sito web/app nella pagina di iscrizione. Questo è abbastanza intuitivo da configurare; il campo più importante da immettere correttamente è l'URL del sito web. Questo deve essere l'URL di radice del Suo sito/app.
4. Una volta terminato di compilare tutto, premere il pulsante _Ottieni ID di monitoraggio_, quindi accettare i termini di servizio che compaiono.
5. La pagina successiva Le fornisce alcuni frammenti di codice e altre istruzioni. Per un sito Web di base, ciò che deve fare è copiare il blocco di codice _Monitoraggio del sito web_ e incollarlo in tutte le diverse pagine che desidera monitorare utilizzando Google Analytics nel Suo sito. Potrebbe posizionare i frammenti sotto il tag di chiusura `</body>`, o in un altro luogo appropriato che impedisca che si confondano con il codice dell'applicazione.
6. Caricare le modifiche sul server di sviluppo, o altrove dove è necessario il codice.

Questo è tutto! Il Suo sito dovrebbe ora essere pronto per iniziare a segnalare i dati di analisi.

#### Studiare i dati di analisi

Ora dovrebbe essere in grado di tornare alla homepage di [Analytics Web](https://analytics.google.com/analytics/web/) e iniziare a guardare i dati raccolti sul Suo sito (deve lasciare un po' di tempo affinché qualche dato venga effettivamente raccolto, naturalmente).

Di default, dovrebbe vedere la scheda di report come segue:

![Come Google Analytics raccoglie i dati nel suo cruscotto principale di report](analytics-reporting.png)

Ci sono enormi quantità di dati che potrebbe esaminare usando Google Analytics — report personalizzati in diverse categorie, ecc. — e non abbiamo il tempo di discuterne tutti.
[Introduzione ad Analytics](https://support.google.com/analytics/answer/9304153) fornisce alcune utili indicazioni sul reporting (e altro) per i principianti.

È possibile vedere quali browser e sistemi operativi i Suoi utenti stanno utilizzando selezionando _Publico > Tecnologia > Browser e OS_ dal menu a sinistra.

> [!NOTE]
> Quando si usano i Google analytics, bisogna fare attenzione ai pregiudizi fuorvianti, ad esempio, "Non abbiamo utenti di Firefox Mobile" potrebbe portarla a non preoccuparsi di supportare Firefox mobile. Ma non avrà utenti di Firefox Mobile se il sito era rotto su Firefox mobile in primo luogo.

### Altre considerazioni

Dovrebbe includere l'accessibilità come un requisito di testing di grado A.

Inoltre, dovrebbe essere consapevole delle esigenze specifiche della situazione. Ad esempio, se il Suo prodotto si rivolge a un mercato in cui i telefoni cellulari sono il mezzo principale per accedere a Internet, probabilmente vorrà fare del supporto per il browser mobile una priorità.

### Tabella di supporto finale

Quindi, la nostra tabella di supporto finale sarà simile alla seguente:

1. Grado A: Chrome e Firefox per Windows/Mac, Safari per Mac, e Edge (ultime due versioni di ciascuno), iOS Safari per iPhone/iPad, browser stock Android (ultime due versioni) su telefono/tablet, Chrome e Firefox per Android (ultime due versioni) su telefono tablet. Passaggio dei test di accessibilità comuni.
2. Grado B: Opera Mini.
3. Grado C: Opera, altri browser moderni di nicchia.

## Cosa testerà?

Quando ha un nuovo aggiustamento al Suo codice che necessita di essere testato, prima di iniziare i test dovrebbe scrivere un elenco dei requisiti di test che devono essere soddisfatti per essere accettati. Questi requisiti possono essere visivi o funzionali — entrambi combinano per creare una funzionalità del sito web utilizzabile.

Consideri il seguente esempio (veda il [codice sorgente](https://github.com/mdn/learning-area/blob/main/tools-testing/cross-browser-testing/strategies/hidden-info-panel.html), e anche [l'esempio in esecuzione dal vivo](https://mdn.github.io/learning-area/tools-testing/cross-browser-testing/strategies/hidden-info-panel.html)):

![Come preparare uno scenario di test caratterizzato dal design e dai requisiti degli utenti](sliding-box-demo.png)

I criteri di test per questa funzione potrebbero essere scritti come segue:

Grado A e B:

- Il pulsante dovrebbe essere attivabile dal meccanismo di controllo primario dell'utente, qualunque esso sia — ciò dovrebbe includere mouse, tastiera e touch.
- Il pulsante basculante dovrebbe far apparire/scomparire la casella di informazione.
- Il testo dovrebbe essere leggibile.
- Gli utenti ipovedenti che usano lettori di schermo dovrebbero essere in grado di accedere al testo.

Grado A:

- La casella delle informazioni dovrebbe animarsi in modo fluido mentre appare/scompare.
- Il gradiente e l'ombra del testo dovrebbero apparire per migliorare l'aspetto della casella.

Potrebbe notare che il pulsante non è utilizzabile solamente con la tastiera. Potremmo risolvere questo problema usando JavaScript per implementare un controllo da tastiera per l'interruttore, o usare qualche altro approccio.

Questi criteri di test sono utili, perché:

- Le forniscono una serie di passaggi da seguire quando sta eseguendo i test.
- Possono essere facilmente trasformati in set di istruzioni per gruppi di utenti da seguire quando eseguono i test (ad es., "provare ad attivare il pulsante usando il mouse, e poi la tastiera…") — vedere [Test degli utenti](#test_degli_utenti), sotto.
- Possono anche fornire una base per scrivere test automatizzati. È più facile scrivere tali test se sa esattamente cosa vuole testare e quali sono le condizioni di successo (vedere [Selenium](/it/docs/Learn_web_development/Extensions/Testing/Your_own_automation_environment#selenium), più avanti nel corso).

## Mettere insieme un laboratorio di test

Un'opzione per eseguire i test dei browser è farlo personalmente. Per fare ciò, probabilmente userà una combinazione di dispositivi fisici reali e ambienti emulati (usando un emulatore o una macchina virtuale).

### Dispositivi fisici

È generalmente meglio avere un dispositivo reale in esecuzione sul browser che si desidera testare — questo fornisce la massima accuratezza in termini di comportamento e esperienza utente complessiva. Probabilmente vorrà qualcosa di simile al seguente, per un laboratorio di dispositivi di basso livello ragionevole:

- Un Mac, con i browser installati che deve testare — ciò può includere Firefox, Chrome, Opera e Safari.
- Un PC Windows, con i browser installati che deve testare — ciò può includere Edge (o IE), Chrome, Firefox e Opera.
- Un telefono e tablet Android con le specifiche più alte con i browser installati che Le servono testare — ciò può includere Chrome, Firefox e Opera Mini per Android, nonché il browser stock originale di Android.
- Un telefono e tablet iOS con le specifiche più alte con i browser installati che Le servono per testare — ciò può includere iOS Safari e Chrome, Firefox e Opera Mini per iOS.

Le seguenti sono anche buone opzioni, se può ottenerle:

- Un PC Linux disponibile, nel caso in cui debba testare bug specifici delle versioni Linux dei browser. Gli utenti Linux comunemente usano Firefox, Opera e Chrome. Se si dispone di una sola macchina disponibile, potrebbe considerare di creare una macchina con doppio avvio eseguendo Linux e Windows su partizioni separate. L'installer di Ubuntu rende abbastanza facile configurararlo; vedere [WindowsDualBoot](https://help.ubuntu.com/community/WindowsDualBoot) per aiuto su questo.
- Un paio di dispositivi mobili con specifiche inferiori, in modo da poter testare le prestazioni di funzionalità come le animazioni su processori meno potenti.

La Sua macchina da lavoro principale può anche essere un luogo in cui installare altri tools per scopi specifici, come strumenti di audit per accessibilità, lettori di schermo ed emulatore/macchine virtuali.

Alcune aziende più grandi hanno laboratori di dispositivi che forniscono una selezione molto ampia di dispositivi differenti, consentendo agli sviluppatori di rintracciare bug su combinazioni molto specifiche di browser/dispositivi. Aziende più piccole e individui generalmente non sono in grado di permettersi un laboratorio così sofisticato, quindi tendono a fare affidamento su laboratori più piccoli, emulatori, macchine virtuali e app di test commerciali.

Ogni altra opzione copriremo di seguito.

> [!NOTE]
> Sono stati fatti alcuni sforzi per creare laboratori di dispositivi accessibili al pubblico — vedere [Open Device Labs](https://www.smashingmagazine.com/2016/11/worlds-best-open-device-labs/).

> [!NOTE]
> Dobbiamo anche considerare l'accessibilità — ci sono un certo numero di strumenti utili che può installare sul Suo computer per facilitare il test dell'accessibilità, ma ne parleremo nell'articolo gestire i problemi comuni di accessibilità, più avanti nel corso.

### Emulatori

Gli emulatori sono essenzialmente programmi che girano all'interno del Suo computer ed emulano un dispositivo o condizioni particolari del dispositivo, consentendo di fare alcuni dei Suoi test in modo più conveniente che dover trovare una particolare combinazione hardware/software da testare.

Un emulatore potrebbe essere semplice come testare una condizione del dispositivo. Ad esempio, se vuole fare qualche rapido test delle Sue query multimediali di larghezza/altezza per design responsivo, potrebbe usare la [Modalità Design Responsivo](https://firefox-source-docs.mozilla.org/devtools-user/responsive_design_mode/index.html) di Firefox. Anche Safari ha una modalità simile, che può essere abilitata andando su _Safari > Preferenze_, e controllando _Mostra menu Sviluppo_, quindi scegliendo _Sviluppo > Entra in modalità Design Responsivo_. Anche Chrome ha qualcosa di simile: Modalità Dispositivo (vedere [Simulare dispositivi mobili con Modalità Dispositivo](https://developer.chrome.com/docs/devtools/device-mode/)).

Più spesso, però, dovrà installare un qualche tipo di emulatore. I dispositivi/browser più comuni che vorrà testare sono come segue:

- L'IDE ufficiale [Android Studio](https://developer.android.com/studio/) per sviluppare app Android è un po' pesante solo per testare siti Web su Google Chrome o il vecchio browser Stock Android, ma viene fornito con un robusto [emulatore](https://developer.android.com/studio/run/emulator.html). Se desidera qualcosa di più leggero, [Andy](https://www.andyroid.net/) è un'opzione ragionevole che funziona sia su Windows che su Mac.
- Apple fornisce un'app chiamata [Simulator](https://help.apple.com/simulator/mac/current/) che gira sopra l'ambiente di sviluppo [XCode](https://developer.apple.com/xcode/), e emula iPad/iPhone/Apple Watch/Apple TV. Ciò include il browser nativo iOS Safari. Purtroppo questo funziona solo su Mac.

Può spesso trovare simulatori per altri ambienti di dispositivi mobili, ad esempio:

- Può emulare Opera Mini autonomamente se desidera testarlo.

> [!NOTE]
> Molti emulatori richiedono effettivamente l'uso di una macchina virtuale (vedere sotto); quando questo è il caso, vengono spesso fornite istruzioni, e/o l'uso della macchina virtuale è incorporato nel programma di installazione dell'emulatore.

### Macchine virtuali

Le macchine virtuali sono applicazioni che girano sul Suo computer desktop e consentono di eseguire emulazioni di interi sistemi operativi, ciascuno compartimentato nel proprio disco rigido virtuale (spesso rappresentato da un singolo grande file esistente sul disco rigido della macchina host). Ci sono un numero di applicazioni popolari di macchine virtuali disponibili, come [Parallels](https://www.parallels.com/), [VMware](https://www.vmware.com/), e [Virtual Box](https://www.virtualbox.org/wiki/Downloads); personalmente ci piace quest'ultimo, perché è gratuito.

> [!NOTE]
> Avrà bisogno di molto spazio su disco disponibile per eseguire emulazioni di macchine virtuali; ogni sistema operativo che emula può occupare molta memoria. Di solito si sceglie lo spazio su disco rigido desiderato per ogni installazione; potrebbe cavarsela probabilmente con 10GB, ma alcune fonti raccomandano fino a 50GB o più, in modo che il sistema operativo funzioni in modo affidabile. Una buona opzione fornita dalla maggior parte delle applicazioni di macchine virtuali è creare un disco rigido **allocato dinamicamente** che cresce e si riduce secondo necessità.

Per usare un Virtual Box, deve:

1. Procurarsi un disco installante o un'immagine (ad esempio, file ISO) per il sistema operativo che desidera emulare. Virtual Box non è in grado di fornirli; la maggior parte, come i sistemi operativi Windows, sono prodotti commerciali che non possono essere distribuiti liberamente.
2. [Scarichi l'installer adatto](https://www.virtualbox.org/wiki/Downloads) per il Suo sistema operativo e lo installi.
3. Apre l'app; Le verrà presentata una vista simile alla seguente: ![La finestra dell'applicazione elenca il sistema operativo Windows e gli emulatori Opera TV nel pannello sinistro. Il pannello destro include diversi sotto pannelli tra cui generale, sistema, schermo, impostazioni, audio, rete e un'anteprima.](virtualbox.png)
4. Per creare una nuova macchina virtuale, premi il pulsante _Nuovo_ nell'angolo in alto a sinistra.
5. Seguira le istruzioni e compilare le finestre di dialogo seguenti come appropriato. Lei:

   1. Fornisca un nome per la nuova macchina virtuale
   2. Sceglierà quale sistema operativo e versione sta installando su di essa
   3. Imposterà quanta RAM dovrebbe essere allocata (consigliamo qualcosa come 2048MB, o 2GB)
   4. Creerà un disco rigido virtuale (scegliere le opzioni predefinite tra le tre finestre di dialogo contenenti _Crea un disco rigido virtuale ora_, _VDI (virtual disk image)_, e _Allocato dinamicamente_).
   5. Sceglierà la posizione del file e la dimensione del disco rigido virtuale (scegliere un nome e una posizione sensati per conservarlo, e per la dimensione specificare circa 50GB, o quanto è comodo specificare).

Ora la nuova virtual box dovrebbe apparire nel menu a sinistra della finestra principale dell'interfaccia utente del Virtual Box. A questo punto, può fare doppio clic per aprirlo — inizierà ad avviare la macchina virtuale, ma non avrà ancora il sistema operativo (SO) installato. A questo punto deve indicare la finestra di dialogo verso l'immagine/disco di installazione, e procederà attraverso i passaggi per installare il sistema operativo proprio come su una macchina fisica.

![Come installare il Virtual Box per un sistema operativo specifico](virtualbox-installer.png)

> [!WARNING]
> È necessario assicurarsi di avere l'immagine del sistema operativo che si desidera installare sulla macchina virtuale disponibile a questo punto, e installarla immediatamente. Se si annulla il processo a questo punto, può rendere la macchina virtuale inutilizzabile, e fare in modo che sia necessario eliminarla e crearla di nuovo. Questo non è un problema fatale, ma è fastidioso.

Dopo che il processo è completato, dovrebbe avere una macchina virtuale in esecuzione con un sistema operativo all'interno di una finestra sul Suo computer host.

![Screenshot di Windows XP, ospitato in Virtual Box, e in esecuzione su macOS](virtualbox-running.png)

Ha bisogno di trattare questa installazione virtuale del sistema operativo come farebbe con qualsiasi reale installazione — ad esempio, oltre a installare i browser che desidera testare, installa un programma antivirus per proteggerlo dai virus.

Avere più macchine virtuali è molto utile, in particolare per i test di Windows IE/Edge — su Windows, non è possibile avere più versioni del browser predefinito installate fianco a fianco, quindi potrebbe voler costruire una libreria di macchine virtuali per gestire diversi test secondo necessità, ad esempio:

- Windows 10 con Edge 14
- Windows 10 con Edge 13

> [!NOTE]
> Un altro vantaggio delle macchine virtuali è che le immagini del disco virtuale sono piuttosto auto-contenute. Se sta lavorando in un team, può creare un'immagine del disco virtuale, quindi copiarla e passarla. Basta assicurarsi di avere le licenze necessarie per utilizzare tutte quelle copie di Windows o qualsiasi altra cosa stia eseguendo se è un prodotto con licenza.

### Automazione e app commerciali

Come detto nel capitolo precedente, può eliminare molti dei problemi del testing dei browser utilizzando un qualche tipo di sistema di automazione. È possibile configurare il proprio sistema di automazione per i test ([Selenium](https://www.selenium.dev/) è l'app popolare scelta), che richiede un po' di configurazione, ma può essere molto gratificante quando si riesce ad attuarlo.

Ci sono anche strumenti commerciali disponibili come [Sauce Labs](https://saucelabs.com/), [Browser Stack](https://www.browserstack.com/) e [LambdaTest](https://www.lambdatest.com/) che fanno questo tipo di cose per voi, senza doversi preoccupare della configurazione, se si desidera investire un po' di soldi nei vostri test.

Un'altra alternativa è utilizzare strumenti di automazione dei test senza codice come [Endtest](https://www.endtest.io/).

Andremo a vedere come utilizzare tali strumenti in seguito nel modulo.

## Test degli utenti

Prima di andare avanti, concluderemo questo articolo parlando un po' del test degli utenti — questo può essere una buona opzione se ha un gruppo di utenti disposti a testare la nuova funzionalità. Tenga presente che questo può essere sia a basso costo sia avanzato quanto desidera — il Suo gruppo di utenti può essere un gruppo di amici, un gruppo di colleghi o un gruppo di volontari non pagati o pagati, a seconda che abbia denaro da spendere per i test.

In generale, farà esaminare agli utenti la pagina o visualizzare la nuova funzionalità su qualche tipo di server di sviluppo, in modo da non rendere il sito finale o la modifica live finché non è completata. Dovrebbe fargli seguire alcuni passaggi e segnalare i risultati ottenuti. È utile fornire una serie di passaggi (a volte chiamati script) in modo da ottenere risultati più affidabili relativi a ciò che stava cercando di testare. Abbiamo menzionato questo nella sezione [Cosa testerà](#what_are_you_going_to_test) in alto — è facile trasformare i criteri di test dettagliati lì in passaggi da seguire. Ad esempio, quanto segue sarebbe funzionale per un utente vedente:

- Fare clic sul pulsante col punto interrogativo usando il mouse sul computer desktop un paio di volte. Aggiorni la finestra del browser.
- Selezionare e attivare il pulsante col punto interrogativo usando la tastiera sul computer desktop alcune volte.
- Toccare il pulsante col punto interrogativo alcune volte sul Suo dispositivo touchscreen.
- Il pulsante a leva dovrebbe far apparire/scomparire la casella di informazione. Lo fa, in ognuno dei tre casi precedenti?
- Il testo è leggibile?
- La casella delle informazioni si anima in modo fluido mentre appare/scompare?

Quando esegue i test, può essere anche una buona idea:

- Impostare un profilo del browser separato dove possibile, con estensioni del browser e altre cose disabilitate, e eseguire i Suoi test in quel profilo (vedere [Usare il gestore dei profili per creare e rimuovere i profili di Firefox](https://support.mozilla.org/en-US/kb/profile-manager-create-remove-switch-firefox-profiles) e [Condividere Chrome con altri o aggiungere persone](https://support.google.com/chrome/answer/2364824), per esempio).
- Utilizzare la funzionalità della modalità privata del browser quando si eseguono i test, dove disponibile (ad es., [Navigazione privata](https://support.mozilla.org/en-US/kb/private-browsing-use-firefox-without-history) in Firefox, [Modalità incognito](https://support.google.com/chrome/answer/95464) in Chrome) in modo che cose come cookie e file temporanei non vengano salvati.

Questi passaggi sono progettati per assicurarsi che il browser in cui sta testando sia il più "puro" possibile, cioè che non ci sia nulla installato che potrebbe influenzare i risultati dei test.

> [!NOTE]
> Un'altra utile opzione a basso costo, se ha l'hardware disponibile, è testare i suoi siti su telefoni/altro dispositivi a bassa gamma — poiché i siti diventano più grandi e presentano più effetti, c'è una maggiore possibilità che il sito rallenti, quindi deve iniziare a considerare di più le prestazioni. Cercare di far funzionare la Sua funzionalità su un dispositivo a bassa gamma renderà più probabile che l'esperienza sia positiva sui dispositivi di fascia alta.

> [!NOTE]
> Alcuni ambienti di sviluppo lato server forniscono meccanismi utili per distribuire le modifiche del sito solo a un sottoinsieme di utenti, fornendo un utile meccanismo per ottenere una funzionalità testata da un sottoinsieme di utenti senza la necessità di un server di sviluppo separato. Un esempio è i [Django Waffle Flags](https://github.com/jazzband/django-waffle).

## Sommario

Dopo aver letto questo articolo dovrebbe ora avere una buona idea di ciò che può fare per identificare il Suo pubblico/target elenco dei browser, quindi eseguire efficacemente il test cross-browser su quell'elenco.

Successivamente, rivolgeremo la nostra attenzione ai problemi reali di codice che il Suo test potrebbe presentare, partendo con HTML e CSS.

{{PreviousMenuNext("Learn_web_development/Extensions/Testing/Introduction","Learn_web_development/Extensions/Testing/HTML_and_CSS", "Learn_web_development/Extensions/Testing")}}
