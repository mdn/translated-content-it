---
title: Quanto costa fare qualcosa sul Web?
slug: Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost
l10n:
  sourceCommit: afcdfa050626bb7eb05ee693df8997020db9ff2e
---

Essere coinvolti nel Web non è economico come sembra. In questo articolo viene discusso quanto potrebbe essere necessario spendere e perché.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        È necessario comprendere già
        <a href="/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need"
          >quale software serve</a
        >, la differenza tra
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
          >una pagina web, un sito web, ecc.</a
        >, e che cos'è
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name"
          >un nome di dominio</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Esaminare il processo completo per creare un sito web e scoprire quanto
        può costare ogni passaggio.
      </td>
    </tr>
  </tbody>
</table>

## Riepilogo

Quando si avvia un sito web, si potrebbe non spendere nulla oppure i costi potrebbero salire alle stelle. In questo articolo viene discusso quanto costa ogni cosa e come si ottiene ciò per cui si paga (o non si paga).

## Software

### Editor di testo

Probabilmente è già disponibile un editor di testo, ad esempio Notepad su Windows, Gedit su Linux, TextEdit su Mac. Scrivere codice sarà più semplice scegliendo un editor che evidenzi la sintassi con colori, la controlli e assista nella strutturazione del codice.

Molti editor sono gratuiti, ad esempio [NotePad++](https://notepad-plus-plus.org/), [Brackets](https://brackets.io/), [Bluefish](https://bluefish.openoffice.nl/index.html), [TextWrangler](https://www.barebones.com/products/textwrangler/), [Eclipse](https://www.eclipse.org/), [NetBeans](https://netbeans.apache.org/), e [Visual Studio Code](https://code.visualstudio.com/). Alcuni, come [Sublime Text](https://www.sublimetext.com/), possono essere provati per tutto il tempo desiderato, ma viene incoraggiato il pagamento. Altri, come [PhpStorm](https://www.jetbrains.com/phpstorm/), possono costare da alcune decine fino a 200 dollari, a seconda del piano acquistato. Alcuni di essi, come [Microsoft Visual Studio](https://visualstudio.microsoft.com/), possono costare centinaia o migliaia di dollari; tuttavia Visual Studio Community è gratuito per gli sviluppatori individuali o per i progetti open source. Spesso gli editor a pagamento dispongono di una versione di prova.

Per iniziare, si suggerisce di provare vari editor per capire quale funziona meglio. Se si scrivono soltanto semplici {{Glossary("HTML", "HTML")}}, {{Glossary("CSS", "CSS")}} e {{Glossary("JavaScript", "JavaScript")}}, è sufficiente un editor semplice.

Il prezzo non riflette in modo affidabile la qualità o l'utilità di un editor di testo. Occorre provarlo personalmente e decidere se soddisfa le proprie esigenze. Ad esempio, Sublime Text è economico, ma include molti plugin gratuiti che possono estenderne notevolmente le funzionalità.

### Editor di immagini

Il sistema probabilmente include un editor o un visualizzatore di immagini: Paint su Windows, Eye of GNOME su Ubuntu, Preview su Mac. Questi programmi sono relativamente limitati e presto sarà necessario un editor più robusto per aggiungere livelli, effetti e raggruppamenti.

Gli editor possono essere gratuiti ([GIMP](https://www.gimp.org/), [Paint.NET](https://paint.net/)), moderatamente costosi ([PaintShop Pro](https://www.paintshoppro.com/), meno di 100 dollari), oppure costare alcune centinaia di dollari ([Adobe Photoshop](https://www.adobe.com/products/photoshop.html)).

È possibile usare qualunque di essi, poiché offrono funzionalità simili, anche se alcuni sono così completi che non verranno mai utilizzate tutte le loro funzionalità. Se a un certo punto è necessario scambiare progetti con altri designer, occorre scoprire quali strumenti usano. Tutti gli editor possono esportare i progetti finiti in formati di file standard, ma ogni editor salva i progetti in corso nel proprio formato specializzato. La maggior parte delle immagini su Internet è protetta da copyright, quindi è meglio verificare la licenza del file prima di utilizzarlo. Siti come [Pixabay](https://pixabay.com/) forniscono immagini con licenza CC0, quindi è possibile usarle, modificarle e pubblicarle, anche con modifiche per uso commerciale.

### Editor multimediali

Se si desidera includere video o audio nel sito web, è possibile incorporare servizi online (ad esempio YouTube, Vimeo o Dailymotion), oppure includere video propri (vedere sotto per i costi della larghezza di banda).

Per i file audio, è possibile trovare software gratuiti ([Audacity](https://www.audacityteam.org/), [Wavosaur](https://www.wavosaur.com/)), oppure software a pagamento fino ad alcune centinaia di dollari ([Sound Forge](https://www.vegascreativesoftware.com/sound-forge/), [Adobe Audition](https://www.adobe.com/products/audition.html)). Analogamente, il software di montaggio video può essere gratuito ([PiTiVi](https://www.pitivi.org/), [OpenShot](https://www.openshot.org/) per Linux, [iMovie](https://support.apple.com/imovie) per Mac), costare meno di 100 dollari ([Adobe Premiere Elements](https://www.adobe.com/products/premiere-elements.html)), oppure alcune centinaia di dollari ([Adobe Premiere Pro](https://www.adobe.com/products/premiere.html), [Avid Media Composer](https://www.avid.com/media-composer), [Final Cut Pro](https://www.apple.com/final-cut-pro/)). Il software fornito con la fotocamera digitale potrebbe soddisfare tutte le necessità.

### Strumenti di pubblicazione

È inoltre necessario un modo per caricare file dal disco rigido a un server web remoto. Per farlo, è opportuno usare uno strumento di pubblicazione come un {{Glossary("FTP", "client (S)FTP")}}, [RSync](https://en.wikipedia.org/wiki/Rsync), oppure [Git/GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

Ogni sistema operativo include un client (S)FTP come parte del proprio file manager. Windows Explorer, Nautilus (un comune file manager Linux) e Finder su Mac includono tutti questa funzionalità. Tuttavia, spesso vengono scelti client (S)FTP dedicati per visualizzare affiancate le directory locali e remote e memorizzare le password dei server.

Se si desidera installare un client (S)FTP, esistono varie opzioni affidabili e gratuite: ad esempio [FileZilla](https://filezilla-project.org/) per tutte le piattaforme, [WinSCP](https://winscp.net/eng/index.php) per Windows, [Cyberduck](https://cyberduck.io/) per Mac o Windows.

Poiché FTP è intrinsecamente insicuro, è necessario assicurarsi di usare SFTP, la versione sicura e crittografata di FTP che la maggior parte dei siti di hosting offre oggi per impostazione predefinita, oppure un'altra soluzione sicura come Rsync su SSH.

## Browser

Probabilmente è già disponibile un browser, oppure è possibile ottenerne uno gratuitamente. Se necessario, scaricare [Firefox](https://www.firefox.com/en-US/download/all/) o [Google Chrome](https://www.google.com/chrome/).

## Accesso al Web

### Computer / modem

È necessario un computer. I costi possono variare enormemente, a seconda del budget e del luogo in cui si vive. Per pubblicare un sito web essenziale, è sufficiente un computer di base in grado di avviare un editor e un browser Web, quindi il livello di ingresso può essere piuttosto basso.

Naturalmente, sarà necessario un computer più potente se si desidera produrre design complessi, ritoccare fotografie o creare file audio e video.

È necessario caricare i contenuti su un server remoto (vedere _Hosting_ sotto), quindi serve un modem. Il proprio {{Glossary("ISP", "ISP")}} può fornire la connettività Internet in affitto per alcuni dollari al mese, anche se il costo può variare in base alla località.

### Accesso ISP

Assicurarsi di avere una {{Glossary("Bandwidth", "larghezza di banda")}} sufficiente:

- Un accesso a bassa larghezza di banda può essere adeguato per supportare un sito web "semplice": immagini di dimensioni ragionevoli, testi, un po' di CSS e JavaScript. Probabilmente costerà alcune decine di dollari, incluso il noleggio del modem.
- D'altra parte, sarà necessaria una connessione ad alta larghezza di banda, come accesso DSL, via cavo o in fibra, se si desidera un sito web più avanzato con centinaia di file oppure distribuire file video/audio pesanti direttamente dal server web. Il costo potrebbe essere uguale a quello di un accesso a bassa larghezza di banda, fino ad alcune centinaia di dollari al mese per esigenze più professionali.

## Hosting

### Comprendere la larghezza di banda

I fornitori di hosting applicano tariffe in base alla quantità di {{Glossary("Bandwidth", "larghezza di banda")}} consumata dal sito web. Ciò dipende da quante persone e robot di crawling del Web accedono ai contenuti in un determinato periodo e da quanto spazio sul server occupano i contenuti. Ecco perché di solito i video vengono archiviati su servizi dedicati come YouTube, Dailymotion e Vimeo. Ad esempio, il fornitore potrebbe offrire un piano che include fino a diverse migliaia di visitatori al giorno, per un utilizzo "ragionevole" della larghezza di banda. Tuttavia, fare attenzione, perché questa definizione varia da un fornitore di hosting all'altro. Tenere presente che un hosting personale affidabile e a pagamento può costare circa dieci-quindici dollari al mese.

> [!NOTE]
> Non esiste una larghezza di banda "illimitata". Se si consuma un'enorme quantità di larghezza di banda, occorre aspettarsi di pagare un'enorme quantità di denaro.

### Nomi di dominio

Il nome di dominio deve essere acquistato tramite un fornitore di nomi di dominio, detto registrar. Il fornitore di hosting può essere anche un registrar (ad esempio, [Ionos](https://www.ionos.com/) e [Gandi](https://www.gandi.net/en-US) sono contemporaneamente registrar e fornitori di hosting). Il nome di dominio costa solitamente 5-15 dollari all'anno. Il costo varia in base a:

- Obblighi locali: alcuni nomi di dominio di primo livello nazionali sono più costosi, poiché i diversi paesi stabiliscono prezzi differenti.
- Servizi associati al nome di dominio: alcuni registrar forniscono protezione dallo spam nascondendo l'indirizzo postale e l'indirizzo email dietro i propri indirizzi; l'indirizzo postale può essere fornito presso il registrar e l'indirizzo email può essere oscurato tramite un alias del registrar.

### Hosting fai-da-te rispetto all'hosting "preconfezionato"

Quando si desidera pubblicare un sito web, è possibile fare tutto in autonomia: configurare un database, se necessario, un Content Management System, o {{Glossary("CMS", "CMS")}} (come [WordPress](https://wordpress.org/), [Dotclear](https://dotclear.org/), [spip](https://www.spip.net/en_rubrique25.html), ecc.), e caricare template predefiniti o propri.

È possibile usare l'ambiente del fornitore di hosting, per circa dieci-quindici dollari al mese, oppure abbonarsi direttamente a un servizio di hosting dedicato con CMS preconfezionati (ad esempio [WordPress](https://wordpress.com/), [Tumblr](https://www.tumblr.com/), [Blogger](https://www.blogger.com/)). Per questi ultimi non sarà necessario pagare nulla, ma si potrebbe avere meno controllo sui template e su altre opzioni.

### Hosting gratuito rispetto all'hosting a pagamento

Ci si potrebbe chiedere perché pagare l'hosting quando esistono così tanti servizi gratuiti.

- Con un servizio a pagamento si ha maggiore libertà. Il sito web appartiene al suo proprietario e può essere migrato senza problemi da un fornitore di hosting all'altro.
- I fornitori di hosting gratuiti potrebbero aggiungere pubblicità ai contenuti, senza alcun controllo.

È meglio scegliere un hosting a pagamento anziché affidarsi a un hosting gratuito, poiché è possibile spostare facilmente i file e il tempo di attività è garantito dalla maggior parte dei siti a pagamento. La maggior parte dei fornitori di hosting offre un grande sconto iniziale.

Alcune persone scelgono un approccio misto. Ad esempio, il blog principale può essere ospitato su un host a pagamento con un nome di dominio completo, mentre contenuti spontanei e meno strategici possono essere pubblicati su un servizio di hosting gratuito.

## Agenzie per siti web professionali e hosting

Se si desidera un sito web professionale, probabilmente verrà incaricata un'agenzia web di realizzarlo.

In questo caso, i costi dipendono da molteplici fattori, come:

- Si tratta di un sito web semplice con poche pagine di testo? Oppure di un sito web più complesso, lungo migliaia di pagine?
- Si desidera aggiornarlo regolarmente? Oppure sarà un sito web statico?
- Il sito web deve connettersi alla struttura IT dell'azienda per raccogliere contenuti, ad esempio dati interni?
- Si desidera una nuova funzionalità appariscente e popolare al momento? Al momento della scrittura, i clienti cercano pagine singole con parallasse complessa.
- Sarà necessario che l'agenzia elabori user story o risolva complessi problemi di {{Glossary("UX", "UX")}}? Ad esempio, creare una strategia per coinvolgere gli utenti o eseguire test A/B per scegliere una soluzione tra varie idee.

Per l'hosting, occorre inoltre considerare le seguenti scelte:

- Si desiderano server ridondanti nel caso in cui il server non sia disponibile?
- Un'affidabilità del 95% è adeguata oppure è necessario un servizio professionale attivo 24 ore su 24?
- Si desiderano server dedicati di alto profilo e ultra-reattivi, oppure è possibile gestire una macchina condivisa più lenta?

A seconda delle risposte a queste domande, il sito potrebbe costare da migliaia a centinaia di migliaia di dollari.

## Passaggi successivi

Ora che è chiaro quanto potrebbe costare il sito web, è il momento di iniziare a progettarlo e [configurare l'ambiente di lavoro](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

- Proseguire leggendo [come scegliere e installare un editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors).
- Se l'attenzione è maggiormente rivolta al design, consultare l'[anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
