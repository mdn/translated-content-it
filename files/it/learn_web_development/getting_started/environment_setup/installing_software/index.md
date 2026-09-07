---
title: Installazione del software di base
short-title: Installazione del software
slug: Learn_web_development/Getting_started/Environment_setup/Installing_software
l10n:
  sourceCommit: afcdfa050626bb7eb05ee693df8997020db9ff2e
---

{{NextMenu("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup")}}

In questo articolo vengono illustrati il software necessario per svolgere semplice sviluppo web e quello da installare ora, incluso un editor di codice e alcuni browser web moderni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo (OS) del computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere quale software è necessario per iniziare.</li>
          <li>Installare un editor di codice, alcuni browser moderni e un server di test locale.</li>
          <li>Esplorare le opzioni per altri tipi comuni di app.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Editor di codice

Un buon editor di codice è una delle cose più importanti che ogni sviluppatore dovrebbe avere a disposizione sul proprio computer. Oltre a essere il luogo in cui si scrive il codice, gli editor di codice offrono molte altre funzionalità. Più avanti nella serie è dedicato un intero articolo agli editor di codice.

Per ora, si consiglia di installare [Visual Studio Code](https://code.visualstudio.com/), poiché è disponibile su diverse piattaforme, dispone di un ottimo insieme di funzionalità e supporto, ed è l'editor usato principalmente. Installarlo ora consentirà di seguire il resto di questo articolo.

## Browser web moderni

Avere browser web moderni a disposizione è essenziale per lo sviluppo web, affinché sia possibile testare siti web o app sui browser utilizzati dai visitatori per accedervi. È inoltre necessario mantenere aggiornati i browser web, in modo che supportino le tecnologie web più recenti e dispongano delle ultime correzioni di sicurezza.

I browser più comuni sono i seguenti:

- Browser desktop:
  - Basati su [Chromium](<https://en.wikipedia.org/wiki/Chromium_(web_browser)>): [Google Chrome](https://www.google.com/chrome/), [Opera](https://www.opera.com/opera), [Brave](https://brave.com/download/), [Microsoft Edge](https://explore.microsoft.com/en-us/edge), [Vivaldi](https://vivaldi.com/).
  - Basati su [Gecko](<https://en.wikipedia.org/wiki/Gecko_(software)>): [Mozilla Firefox](https://www.firefox.com/en-US/).
  - Basati su [WebKit](https://en.wikipedia.org/wiki/WebKit): [Apple Safari](https://www.apple.com/safari/).
- Browser per dispositivi mobili/alternativi:
  - Basati su Chromium (Android): [Google Chrome](https://www.google.com/chrome/go-mobile/), [Opera](https://www.opera.com/opera), [Brave](https://brave.com/download/), [Microsoft Edge](https://explore.microsoft.com/en-us/edge/mobile), [Samsung Internet](https://www.samsung.com/us/support/owners/app/samsung-internet), [Vivaldi](https://vivaldi.com/android/).
  - Basati su Gecko (Android): [Mozilla Firefox](https://www.firefox.com/en-US/download/android/).
  - Basati su WebKit (iOS): [Apple Safari](https://www.apple.com/safari/).
    > [!NOTE]
    > La maggior parte dei browser Android elencati sopra dispone anche di versioni per iOS, ma storicamente erano tutti basati internamente sul motore WebKit di Apple a causa delle regole dell'App Store di Apple. Al momento della stesura di questo testo, a seguito di modifiche normative, i browser stanno iniziando a creare versioni dei propri browser per iOS basate sui rispettivi motori di rendering. Consultare [Apple is finally allowing full versions of Chrome and Firefox to run on the iPhone](https://www.theverge.com/2024/1/25/24050478/apple-ios-17-4-browser-engines-eu).

La maggior parte dei browser moderni tende a installare automaticamente gli aggiornamenti, applicando le modifiche al riavvio. Di solito è possibile verificare la presenza di aggiornamenti nella pagina "Informazioni" del browser. Questa è disponibile in posizioni leggermente diverse a seconda del browser e del sistema operativo, per esempio:

- Firefox: disponibile in _Firefox_ > _Informazioni su Firefox_ su macOS e nell'icona del menu > _Aiuto_ > _Informazioni su Firefox_ su Windows.
- Chrome: disponibile in _Chrome_ > _Informazioni su Google Chrome_ su macOS e nell'icona del menu > _Aiuto_ > _Informazioni su Google Chrome_ su Windows.

### Quali browser installare

Per ora, è consigliabile installare un paio di browser desktop e per dispositivi mobili/alternativi in cui testare il codice. Installare browser basati su almeno due motori di rendering diversi (per esempio Chromium e Gecko), in modo da non effettuare test soltanto in più browser basati sullo stesso motore di rendering. Questo è importante perché il codice potrebbe contenere bug che interessano un solo motore di rendering.

I browser basati su WebKit non sono disponibili per i sistemi operativi Windows, Linux e Android. Per testare il codice in tutti e tre i principali motori di rendering su un computer basato su Windows, sarà necessario avere accesso a un dispositivo di test basato su macOS o iOS, oppure utilizzare una soluzione software come una macchina virtuale o una piattaforma di test. In questa fase non è tuttavia necessario preoccuparsi di eseguire test completi: per ora è sufficiente riconoscere che il codice dovrebbe essere testato su diversi motori di rendering e fare un po' di pratica.

Le strategie di test vengono esaminate più in dettaglio nel modulo [Test](/it/docs/Learn_web_development/Extensions/Testing).

## Server web locali

Normalmente, quando si digita un indirizzo web in un browser per caricare un sito web, i file che il browser combina per eseguire il rendering di quel sito vengono recuperati da un server web remoto ospitato su un computer server in qualche parte del mondo. Nel prossimo articolo della serie verrà spiegato più approfonditamente come funziona questo processo.

Quando si crea un sito web localmente, cioè sul proprio computer, spesso è possibile caricare direttamente in un browser il file di indice HTML principale per testarlo. Tuttavia, alcuni esempi dovranno essere eseguiti tramite un server web installato localmente per funzionare correttamente.

### Installazione di un server web locale

Una delle opzioni più semplici per rendere disponibile un server locale consiste nell'utilizzare un'estensione dell'editor di codice: in questo modo, sarà disponibile direttamente all'interno dell'editor di codice. In Visual Studio Code, procedere come segue:

1. Aprire il pannello _Extensions_ usando l'opzione di menu _View_ > _Extensions_.
2. Nella casella "Search..." nella parte superiore di questo pannello, digitare "live preview". Il primo risultato della ricerca dovrebbe essere l'estensione [_Live Preview_](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server), creata da Microsoft.
3. Fare clic su tale opzione per aprire una pagina con informazioni sull'estensione, incluse le istruzioni per il suo utilizzo.
4. Premere il pulsante _Install_ per installare l'estensione.
5. Ora, quando si lavora su un file HTML nell'editor, dovrebbe essere possibile fare clic sul pulsante "Show Preview" per aprire l'esempio live in una scheda separata.

L'opzione descritta sopra è semplice, ma non molto flessibile. In futuro, potrebbe essere utile disporre di un'opzione di server locale più flessibile, utilizzabile per caricare esempi in qualsiasi browser disponibile. Per altre opzioni, e maggiori informazioni sul motivo per cui i server locali sono necessari, consultare [Come si configura un server di test locale?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

## Editor di grafica

Gli sviluppatori web devono spesso manipolare file di immagini da utilizzare nei siti web che creano. Questo può significare progettare o creare risorse grafiche, ma altrettanto spesso la grafica viene fornita da un grafico, che può essere un membro del team o un soggetto esterno. In questo caso, allo sviluppatore web potrebbe essere richiesto di ritagliare o ridimensionare i file ricevuti.

Nessuno degli articoli di apprendimento su MDN richiede la creazione di grafica personale, anche se alcuni potrebbero richiedere la manipolazione dei file forniti.

Si consiglia di non installare un editor di grafica finché non sarà necessario più avanti nel percorso di apprendimento. In particolare, non è opportuno spendere denaro per un costoso prodotto commerciale, a meno che non si ritenga davvero che apporti valore.

Esistono molti strumenti software gratuiti e servizi online che probabilmente saranno più che adeguati per ora, per esempio:

- macOS include uno strumento chiamato [Preview](https://support.apple.com/en-gb/guide/preview/welcome/mac). Viene utilizzato principalmente per visualizzare immagini e PDF, ma dispone anche di alcune funzionalità molto utili per modificare le immagini, tra cui ridimensionamento, rotazione, ritaglio, annotazione e conversione tra diversi tipi di file.
- L'app [Photos](https://support.microsoft.com/en-us/windows/apps/photos/manage-photos-and-videos-with-microsoft-photos-app) integrata in Windows offre molte funzionalità simili.
- Il sito web [tinypng](https://tinypng.com/) fornisce un servizio gratuito che consente di comprimere PNG, JPEG e altro. Si tratta di un'attività molto comune durante la preparazione delle risorse per l'uso in un sito web.

Per quanto riguarda le offerte commerciali, [Adobe Photoshop](https://www.adobe.com/products/photoshop.html) è da tempo lo standard del settore, soprattutto per il fotoritocco, mentre programmi come [Sketch](https://www.sketch.com/) sono più adatti al lavoro su icone e UI. Esistono anche nuovi prodotti popolari come [Figma](https://www.figma.com/), [The Affinity Suite](https://www.affinity.studio/) e [Canva](https://www.canva.com/).

La maggior parte delle app sopra elencate dispone di versioni di prova o modalità gratuite che vale la pena esplorare. Sono inoltre disponibili alcune app gratuite molto apprezzate, come [GIMP](https://www.gimp.org/), [Adobe Express](https://www.adobe.com/express/) e [Paint.NET](https://paint.net/).

## Strumenti di controllo versione

Gli strumenti di **controllo versione** sono utilizzati dagli sviluppatori per gestire file sui server, collaborare a un progetto con un team, condividere codice e risorse ed evitare conflitti di modifica. Attualmente, [Git](https://git-scm.com/) è il sistema di controllo versione più popolare, insieme a servizi di hosting come [GitHub](https://github.com/) o [GitLab](https://about.gitlab.com/).

Sebbene gli strumenti di controllo versione siano essenziali per i team di sviluppo web, non è necessario preoccuparsene ora. Verso la fine della serie di moduli fondamentali è presente un modulo dedicato al [Controllo versione](/it/docs/Learn_web_development/Core/Version_control).

## App per la pubblicazione dei siti

Dopo aver terminato lo sviluppo di un sito web o di un'app, sul computer locale o magari su un server di sviluppo, sarà necessario trasferirlo su un server web remoto affinché gli utenti possano digitare l'indirizzo web associato e visualizzarlo sul web.

Esistono vari modi per farlo: acquistare un hosting e utilizzare un'[app SFTP](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server#sftp), utilizzare un servizio come [GitHub Pages](https://pages.github.com/) o [Netlify](https://www.netlify.com/), oppure persino realizzare rapidamente una demo da condividere con altri usando strumenti come [CodePen](https://codepen.io/) o [JSFiddle](https://jsfiddle.net/).

Un elenco di opzioni di questo tipo potrebbe sembrare eccessivo, ma non c'è da preoccuparsi: al momento non è necessario sapere nulla sulla pubblicazione di siti web. Questo argomento verrà affrontato più volte nel corso. L'esperienza pratica arriverà presto, nel modulo [Il primo sito web](/it/docs/Learn_web_development/Getting_started/Your_first_website).

{{NextMenu("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup")}}
