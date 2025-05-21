---
title: Installazione del software di base
short-title: Installazione del software
slug: Learn_web_development/Getting_started/Environment_setup/Installing_software
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup")}}

In questo articolo, discutiamo quale software è necessario per sviluppare semplici applicazioni web e cosa bisogna installare ora, incluso un editor di codice e alcuni browser web moderni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del proprio computer.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Comprendere quale software è necessario per iniziare.</li>
          <li>Installare un editor di codice, alcuni browser moderni e un server di test locale.</li>
          <li>Esplorare opzioni per altri tipi comuni di app.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Editor di codice

Un editor di codice decente è una delle cose più importanti che ogni sviluppatore dovrebbe avere disponibile sul proprio computer. Oltre a essere il luogo dove scrivere il codice, gli editor di codice offrono un'ampia gamma di funzionalità. Abbiamo dedicato un intero articolo sulla serie agli editor di codice.

Per ora, consigliamo di installare [Visual Studio Code](https://code.visualstudio.com/), poiché è disponibile su diverse piattaforme, ha un'ottima serie di funzionalità e supporto, ed è l'editor che usiamo principalmente. Dovresti installarlo ora per seguire il resto di questo articolo.

## Browser web moderni

Avere a disposizione browser web moderni è essenziale per lo sviluppo web, in modo da poter testare i siti web o le app sui browser che i visitatori utilizzano per accedervi. È anche necessario mantenere i browser web aggiornati affinché supportino le ultime tecnologie web e abbiano applicate le ultime correzioni di sicurezza.

> [!NOTE]
> La maggior parte dei browser tende a installare aggiornamenti automaticamente, applicando le modifiche quando vengono riavviati. Di solito puoi controllare gli aggiornamenti nella pagina "Informazioni" del browser, ad esempio disponibile nel menu su _Firefox_ > _Informazioni su Firefox_ o _Chrome_ > _Informazioni su Google Chrome_ su Firefox/Chrome per macOS, o icona menu > _Guida_ > _Informazioni su Firefox_ o icona menu > _Guida_ > _Informazioni su Google Chrome_ su Firefox/Chrome per Windows.

Per ora, dovresti installare un paio di browser per dispositivi desktop e mobile/alternativi per testare il tuo codice. Incontrerai più frequentemente browser web su desktop, laptop e dispositivi mobili, ma troverai browser web anche su altri dispositivi come tablet, orologi e TV. Se possibile, assicurati di avere installato e disponibile per test uno browser per ciascun motore di rendering (così da non testare solo in diversi browser basati sullo stesso motore di rendering):

- Browser per desktop:
  - Chromium: [Google Chrome](https://www.google.com/chrome/), [Opera](https://www.opera.com/opera), [Brave](https://brave.com/download/), [Microsoft Edge](https://www.microsoft.com/en-us/edge), [Vivaldi](https://vivaldi.com/).
  - Gecko: [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/new/).
  - WebKit: [Apple Safari](https://www.apple.com/safari/).
- Browser per dispositivi mobili/alternativi:
  - Chromium (Android): [Google Chrome](https://www.google.com/chrome/go-mobile/), [Opera](https://www.opera.com/opera), [Brave](https://brave.com/download/), [Microsoft Edge](https://www.microsoft.com/en-us/edge/mobile), [Samsung Internet](https://www.samsung.com/us/support/owners/app/samsung-internet), [Vivaldi](https://vivaldi.com/android/).
  - Gecko (Android): [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/browsers/mobile/android/).
  - WebKit (iOS): [Apple Safari](https://www.apple.com/safari/).
    > [!NOTE]
    > La maggior parte dei browser Android elencati sopra ha versioni per iOS, ma storicamente erano tutti alimentati dal motore WebKit di Apple a causa delle regole dell'App Store di Apple. Al momento della scrittura, i browser stanno iniziando a creare versioni dei loro browser iOS basate sui propri motori di rendering, a causa di cambiamenti normativi. Vedi [Apple permette finalmente alle versioni complete di Chrome e Firefox di funzionare su iPhone](https://www.theverge.com/2024/1/25/24050478/apple-ios-17-4-browser-engines-eu).

## Server web locali

Normalmente, quando digiti un indirizzo web in un browser per caricare un sito, i file combinati per visualizzare quel sito dal tuo browser vengono recuperati da un server web remoto ospitato su un altro computer situato altrove nel mondo. Imparerai di più su come funziona nel prossimo articolo della serie.

Quando crei un sito web localmente (sul tuo computer), puoi spesso caricare direttamente il file HTML index principale in un browser per testarlo. Tuttavia, alcuni esempi dovranno essere eseguiti tramite un server web installato localmente per funzionare correttamente.

Una delle opzioni più semplici che abbiamo trovato per rende disponibile un server locale è utilizzare un'estensione dell'editor di codice — in questo modo è disponibile direttamente all'interno del tuo editor di codice. Fai quanto segue all'interno di Visual Studio Code:

1. Apri il riquadro _Estensioni_ utilizzando l'opzione _Visualizza_ > _Estensioni_.
2. Nella casella "Cerca..." in cima a questo riquadro, digita "live preview". Il primo risultato della ricerca dovrebbe essere l'estensione [_Live Preview_](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server), creata da Microsoft.
3. Clicca su quell'opzione per aprire una pagina di informazioni su di essa, che include come utilizzarla.
4. Premi il pulsante _Installa_ per installare l'estensione.
5. Ora, quando lavori su un file HTML nell'editor, dovresti essere in grado di cliccare il pulsante "Mostra anteprima" per aprire l'esempio dal vivo in una scheda separata.

L'opzione sopra è semplice, ma non molto flessibile. In futuro, potresti desiderare una opzione di server locale più flessibile che possa essere utilizzata per caricare esempi in qualsiasi browser tu abbia. Per altre opzioni (e ulteriori informazioni sul perché i server locali sono necessari), vedi [Come impostare un server di test locale?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

## Editor grafici

Gli sviluppatori web spesso devono manipolare file immagine per l'uso sui siti che creano. Questo può significare progettare/creare risorse grafiche, ma spesso le grafiche sono fornite da un designer grafico (può essere un compagno di squadra o un terzo), nel qual caso lo sviluppatore web potrebbe essere chiamato a ritagliare o ridimensionare i file ricevuti.

Nessuno degli articoli di apprendimento su MDN richiede di creare le proprie grafiche, sebbene alcuni possano richiedere di manipolare i file che forniamo.

Sono disponibili molti strumenti di editing grafico. Consigliamo di non spendere soldi per un prodotto commerciale costoso fino a più avanti nel tuo percorso di apprendimento, _se_ ritieni di averne realmente bisogno. Ci sono molti strumenti software gratuiti e servizi online che probabilmente saranno sufficientemente buoni per ora.

Ad esempio:

- macOS include uno strumento chiamato [Anteprima](https://support.apple.com/en-gb/guide/preview/welcome/mac). Questo è principalmente utilizzato per visualizzare immagini e PDF, ma ha anche alcune funzionalità utili per modificare immagini, tra cui ridimensionamento, rotazione, ritaglio, annotazione e conversione tra diversi tipi di file.
- L'app [Foto di Windows](https://support.microsoft.com/en-gb/windows/manage-photos-and-videos-with-microsoft-photos-app-c0c6422f-d4cb-2e3d-eb65-7069071b2f9b) integrata offre molte funzionalità simili.
- Il sito web [tinypng](https://tinypng.com/), fornisce un servizio gratuito che permette di comprimere PNG, JPEG e altro. Questo è un compito molto comune che dovrai fare quando prepari asset per l'uso su un sito web.

Se hai esigenze più sostanziali di editing/creazione grafica, desidererai un pacchetto grafico completo. In termini di offerte commerciali, [Adobe Photoshop](https://www.adobe.com/products/photoshop.html) è stato a lungo lo standard del settore specialmente per l'editing fotografico, mentre programmi come [Sketch](https://www.sketch.com/) sono più adatti per lavori di icone e interfaccia utente. Ci sono anche nuovi arrivati popolari come [Figma](https://www.figma.com/), [The Affinity Suite](https://affinity.serif.com/en-us/), e [Canva](https://www.canva.com/).

Se il tuo budget è limitato, la maggior parte delle app sopra elencate ha prove o modalità gratuite che vale la pena esplorare. Sono anche disponibili alcune app gratuite ben considerate come [GIMP](https://www.gimp.org/), [Adobe Express](https://www.adobe.com/express/), e [Paint.NET](https://www.getpaint.net/).

## Strumenti di controllo versione

Gli strumenti di **controllo versione** sono utilizzati dagli sviluppatori per gestire file su server, collaborare a un progetto con un team, condividere codice e asset, ed evitare conflitti di modifica. Al momento, [Git](https://git-scm.com/) è il sistema di controllo versione più popolare insieme a servizi di hosting come [GitHub](https://github.com/) o [GitLab](https://about.gitlab.com/).

Sebbene gli strumenti di controllo versione siano essenziali per i team di sviluppo web, non è necessario preoccuparsi di essi in questo momento. Abbiamo un modulo dedicato al [controllo versione](/it/docs/Learn_web_development/Core/Version_control) verso la fine della nostra serie di moduli Core.

## App di distribuzione dei siti

Dopo aver terminato lo sviluppo di un sito web o app (sul tuo computer locale, o magari su un server di sviluppo), vorrai metterlo su un server web remoto in modo che i tuoi utenti possano digitare l'indirizzo web associato ad esso e vederlo sul web!

Ci sono vari modi in cui puoi fare questo, dall'acquisto di hosting e utilizzo di un'app [SFTP](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server#sftp), utilizzando un servizio come [GitHub Pages](https://pages.github.com/) o [Netlify](https://www.netlify.com/), o persino impostare una demo rapida da condividere con altri usando qualcosa come [Glitch](https://glitch.com/) o [CodePen](https://codepen.io/).

Una tale lista di opzioni potrebbe sembrare schiacciante, ma non preoccuparti — non è necessario sapere nulla sulla pubblicazione di siti web in questo momento. Esamineremo questo argomento molte volte nel corso. Avrai presto un'esperienza pratica a riguardo, nel nostro modulo [Il tuo primo sito web](/it/docs/Learn_web_development/Getting_started/Your_first_website).

{{NextMenu("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup")}}
