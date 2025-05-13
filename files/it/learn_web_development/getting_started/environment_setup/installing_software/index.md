---
title: Installazione di software di base
short-title: Installazione del software
slug: Learn_web_development/Getting_started/Environment_setup/Installing_software
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{NextMenu("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup")}}

In questo articolo, discuteremo quale software è necessario per fare un semplice sviluppo web e cosa è necessario installare ora, inclusi un editor di codice e alcuni browser web moderni.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del suo computer.
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

Un editor di codice decente è una delle cose più importanti che qualsiasi sviluppatore dovrebbe avere a disposizione sul proprio computer. Oltre a essere il luogo in cui si scrive il codice, gli editor di codice offrono una serie di altre funzionalità. Abbiamo dedicato un intero articolo agli editor di codice più avanti nella serie.

Per ora, le consigliamo di installare [Visual Studio Code](https://code.visualstudio.com/), in quanto è disponibile su diverse piattaforme, ha un ottimo set di funzionalità e supporto, ed è l'editor che utilizziamo principalmente. Dovrebbe installarlo ora per seguire il resto di questo articolo.

## Browser web moderni

Avere browser web moderni è essenziale per lo sviluppo web, in modo da poter testare i suoi siti web o app sui browser che i suoi visitatori usano per accedervi. È anche necessario mantenere i browser aggiornati in modo che supportino le ultime tecnologie web e abbiano applicate le ultime correzioni di sicurezza.

> [!NOTE]
> La maggior parte dei browser tende a installare gli aggiornamenti automaticamente, applicando le modifiche quando vengono riavviati. Di solito può verificare la presenza di aggiornamenti nella pagina "Informazioni" del browser, ad esempio disponibile nel menu in _Firefox_ > _Informazioni su Firefox_ o _Chrome_ > _Informazioni su Google Chrome_ su Firefox/Chrome per macOS, o l'icona del menu > _Aiuto_ > _Informazioni su Firefox_ o l'icona del menu > _Aiuto_ > _Informazioni su Google Chrome_ su Firefox/Chrome per Windows.

Per ora, dovrebbe installare un paio di browser per desktop e dispositivi mobili/alternativi per testare il suo codice. Troverà più comunemente browser web su dispositivi desktop, portatili e mobili, ma troverà anche browser web su altri dispositivi come tablet, orologi e TV. Se possibile, si assicuri di avere un browser per ogni linea installato e disponibile per il test (così non si limiterà a testare in più browser basati sullo stesso motore di rendering):

- Browser desktop:
  - Chromium: [Google Chrome](https://www.google.com/chrome/), [Opera](https://www.opera.com/opera), [Brave](https://brave.com/download/), [Microsoft Edge](https://www.microsoft.com/en-us/edge), [Vivaldi](https://vivaldi.com/).
  - Gecko: [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/new/).
  - WebKit: [Apple Safari](https://www.apple.com/safari/).
- Browser per dispositivi mobili/alternativi:
  - Chromium (Android): [Google Chrome](https://www.google.com/chrome/go-mobile/), [Opera](https://www.opera.com/opera), [Brave](https://brave.com/download/), [Microsoft Edge](https://www.microsoft.com/en-us/edge/mobile), [Samsung Internet](https://www.samsung.com/us/support/owners/app/samsung-internet), [Vivaldi](https://vivaldi.com/android/).
  - Gecko (Android): [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/browsers/mobile/android/).
  - WebKit (iOS): [Apple Safari](https://www.apple.com/safari/).
    > [!NOTE]
    > La maggior parte dei browser Android elencati sopra ha versioni iOS, ma storicamente erano tutti alimentati dal motore WebKit di Apple a causa delle regole dell'App Store di Apple. Al momento della scrittura, i browser stanno iniziando a creare versioni dei loro browser iOS basate sui propri motori di rendering, a causa di cambiamenti normativi. Veda [Apple is finally allowing full versions of Chrome and Firefox to run on the iPhone](https://www.theverge.com/2024/1/25/24050478/apple-ios-17-4-browser-engines-eu).

## Server web locali

Normalmente, quando si digita un indirizzo web in un browser per caricare un sito, i file che vengono combinati per renderizzare quel sito dal suo browser vengono recuperati da un server web remoto ospitato su un computer server da qualche altra parte nel mondo. Imparerà di più su come funziona questo nel prossimo articolo della serie.

Quando crea un sito web localmente (sul suo computer), può spesso caricare direttamente il file HTML principale nel browser per testarlo. Tuttavia, alcuni esempi avranno bisogno di essere eseguiti attraverso un server web installato localmente per funzionare correttamente.

Una delle opzioni più facili che abbiamo trovato per rendere disponibile un server locale è utilizzare un'estensione dell'editor di codice — in questo modo, è disponibile direttamente all'interno del suo editor di codice. Faccia quanto segue in Visual Studio Code:

1. Apra il riquadro _Estensioni_ usando l'opzione di menu _Visualizza_ > _Estensioni_.
2. Nella casella "Cerca..." in alto a questo riquadro, digiti "live preview". Il primo risultato della ricerca dovrebbe essere l'estensione [_Live Preview_](https://marketplace.visualstudio.com/items?itemName=ms-vscode.live-server), creata da Microsoft.
3. Clicchi su quell'opzione per aprire una pagina di informazioni su di essa, che include come utilizzarla.
4. Premere il pulsante _Installa_ per installare l'estensione.
5. Ora, quando sta lavorando su un file HTML nell'editor, dovrebbe essere in grado di cliccare sul pulsante "Mostra anteprima" per aprire l'esempio dal vivo in una scheda separata.

L'opzione sopra è semplice, ma non così flessibile. In futuro, potrebbe voler avere un'opzione di server locale più flessibile che possa essere utilizzata per caricare esempi in qualsiasi browser che ha. Per ulteriori opzioni (e maggiori informazioni di contesto sul perché i server locali sono necessari), veda [How do you set up a local testing server?](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

## Editor grafici

Spesso agli sviluppatori web è richiesto di manipolare file immagine per l'uso sui siti web che creano. Questo può spesso significare progettare/creare asset grafici, ma allo stesso modo, le grafiche sono spesso fornite da un designer grafico (potrebbe essere un membro del team o un terzo), nel qual caso lo sviluppatore web può essere chiamato a ritagliare o ridimensionare i file che riceve.

Nessuno degli articoli di apprendimento su MDN richiede di creare le proprie grafiche, anche se alcuni potrebbero richiedere di manipolare i file che noi forniamo.

Ci sono molti strumenti di editing grafico disponibili. Le consigliamo di non spendere soldi su un prodotto commerciale costoso fino a quando non sarà più avanti nel suo percorso di apprendimento, _se_ ritiene che ne abbia davvero bisogno. Ci sono molti strumenti software gratuiti e servizi online che probabilmente saranno sufficientemente buoni per ora.

Ad esempio:

- macOS viene fornito di uno strumento chiamato [Preview](https://support.apple.com/en-gb/guide/preview/welcome/mac). Questo è utilizzato principalmente per visualizzare immagini e PDF, ma ha anche alcune funzionalità davvero utili per l'editing delle immagini, inclusi il ridimensionamento, la rotazione, il ritaglio, l'annotazione e la conversione tra diversi tipi di file.
- L'app Windows [Photos](https://support.microsoft.com/en-gb/windows/manage-photos-and-videos-with-microsoft-photos-app-c0c6422f-d4cb-2e3d-eb65-7069071b2f9b) incorporata offre molte funzionalità simili.
- Il sito web [tinypng](https://tinypng.com/) offre un servizio gratuito che consente di comprimere PNG, JPEG e altro. Questo è un compito molto comune che dovrà fare quando prepara asset per l'uso su un sito web.

Se ha esigenze più sostanziali di modifica/creazione di grafiche, vorrà un pacchetto grafico completo. In termini di offerte commerciali, [Adobe Photoshop](https://www.adobe.com/products/photoshop.html) è da tempo lo standard del settore, soprattutto per il fotoritocco, mentre programmi come [Sketch](https://www.sketch.com/) sono più adatti al lavoro di icona e UI. Ci sono anche nuovi arrivi popolari come [Figma](https://www.figma.com/), [La Suite Affinity](https://affinity.serif.com/en-us/), e [Canva](https://www.canva.com/).

Se il suo budget è limitato, la maggior parte delle app sopra elencate offre prove o modalità gratuite che vale la pena esplorare. Ci sono anche alcune app gratuite ben considerate disponibili come [GIMP](https://www.gimp.org/), [Adobe Express](https://www.adobe.com/express/), e [Paint.NET](https://www.getpaint.net/).

## Strumenti di controllo di versione

Gli strumenti di **controllo di versione** sono utilizzati dagli sviluppatori per gestire i file sui server, collaborare a un progetto con un team, condividere codice e risorse, ed evitare conflitti di modifica. Al momento, [Git](https://git-scm.com/) è il sistema di controllo di versione più popolare insieme a servizi di hosting come [GitHub](https://github.com/) o [GitLab](https://about.gitlab.com/).

Mentre gli strumenti di controllo di versione sono essenziali per i team di sviluppo web, non ha bisogno di preoccuparsene ora. Abbiamo un modulo dedicato al [Controllo di versione](/it/docs/Learn_web_development/Core/Version_control) verso la fine della nostra serie di moduli Core.

## App per il deploy dei siti

Dopo aver finito di sviluppare un sito web o un'app (sul suo computer locale, o forse su un server di sviluppo), vorrà metterlo su un server web remoto in modo che i suoi utenti possano digitare l'indirizzo web associato ed esaminarlo sul web!

Ci sono vari modi per farlo, dall'acquisto di hosting e utilizzo di un'app [SFTP](/it/docs/Learn_web_development/Howto/Tools_and_setup/Upload_files_to_a_web_server#sftp), utilizzando un servizio come [GitHub Pages](https://pages.github.com/) o [Netlify](https://www.netlify.com/), o anche creando una demo veloce da condividere con altri usando qualcosa come [Glitch](https://glitch.com/) o [CodePen](https://codepen.io/).

Tale elenco di opzioni potrebbe sembrare travolgente, ma non si preoccupi — non deve sapere nulla sulla pubblicazione di siti web in questo momento. Affronteremo questo argomento molte volte nel corso. Acquisirà presto esperienza pratica in merito, nel nostro modulo [Il suo primo sito web](/it/docs/Learn_web_development/Getting_started/Your_first_website).

{{NextMenu("Learn_web_development/Getting_started/Environment_setup/Browsing_the_web", "Learn_web_development/Getting_started/Environment_setup")}}
