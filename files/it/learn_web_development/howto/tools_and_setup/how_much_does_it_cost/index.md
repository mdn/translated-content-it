---
title: Quanto costa fare qualcosa sul Web?
slug: Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

Coinvolgersi sul Web non è economico come sembra. In questo articolo discutiamo quanto potresti dover spendere e perché.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovresti già capire
        <a href="/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need"
          >quale software ti serve</a
        >, la differenza tra
        <a
          href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web"
          >una pagina web, un sito web, ecc.</a
        >, e cosa è
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name"
          >un nome di dominio</a
        >.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Rivedi il processo completo per creare un sito web e scopri quanto può costare ciascuna fase.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Quando lanci un sito web, potresti non spendere nulla, o i tuoi costi potrebbero salire. In questo articolo discutiamo quanto costa tutto, e come ottenere ciò per cui paghi (o non paghi).

## Software

### Editor di testo

Probabilmente hai già un editor di testo: ad esempio, Blocco Note su Windows, Gedit su Linux, TextEdit su Mac. Avrai un tempo più facile a scrivere codice se scegli un editor che usa il colore per il codice, controlla la sintassi e ti assiste nella struttura del codice.

Molti editor sono gratuiti, ad esempio [Brackets](https://brackets.io/), [Bluefish](https://bluefish.openoffice.nl/index.html), [TextWrangler](https://www.barebones.com/products/textwrangler/), [Eclipse](https://www.eclipse.org/), [NetBeans](https://netbeans.apache.org/) e [Visual Studio Code](https://code.visualstudio.com/). Alcuni, come [Sublime Text](https://www.sublimetext.com/), puoi testarli quanto vuoi, ma è incoraggiato il pagamento. Alcuni, come [PhpStorm](https://www.jetbrains.com/phpstorm/), possono costare tra alcune decine e 200 dollari, a seconda del piano che acquisti. Altri, come [Microsoft Visual Studio](https://visualstudio.microsoft.com/), possono costare centinaia o migliaia di dollari; tuttavia Visual Studio Community è gratuito per sviluppatori individuali o progetti open source. Spesso, gli editor a pagamento avranno una versione di prova.

Per iniziare, suggeriamo di provare diversi editor per avere un'idea di quale funzioni meglio per te. Se stai solo scrivendo semplice {{Glossary("HTML", "HTML")}}, {{Glossary("CSS", "CSS")}} e {{Glossary("JavaScript", "JavaScript")}}, scegli un editor semplice.

Il prezzo non riflette in modo affidabile la qualità o l'utilità di un editor di testo. Devi provarlo tu stesso e decidere se soddisfa le tue esigenze. Per esempio, Sublime Text è economico ma viene fornito con molti plugin gratuiti che possono estendere notevolmente la sua funzionalità.

### Editor di immagini

Il tuo sistema probabilmente include un editor o visualizzatore di immagini: Paint su Windows, Eye of GNOME su Ubuntu, Anteprima su Mac. Questi programmi sono relativamente limitati e presto desidererai un editor più robusto per aggiungere livelli, effetti e gruppi.

Gli editor possono essere gratuiti ([GIMP](https://www.gimp.org/), [Paint.NET](https://www.getpaint.net/)), moderatamente costosi ([PaintShop Pro](https://www.paintshoppro.com/), meno di $100), o diversi centinaia di dollari ([Adobe Photoshop](https://www.adobe.com/products/photoshop.html)).

Puoi usarne uno qualsiasi, poiché avranno funzionalità simili, anche se alcuni sono così completi che non userai mai tutte le funzionalità. Se a un certo punto hai bisogno di scambiare progetti con altri designer, dovresti scoprire quali strumenti stanno usando. Gli editor possono esportare tutti i progetti finiti in formati di file standard, ma ogni editor salva i progetti in corso nel suo proprio formato specializzato. La maggior parte delle immagini su Internet è protetta da copyright, quindi è meglio controllare la licenza del file prima di usarlo. Siti come [Pixabay](https://pixabay.com/) forniscono immagini sotto licenza CC0, in modo che tu possa usarle, modificarle e pubblicarle anche con modifiche per uso commerciale.

### Editor multimediali

Se vuoi includere video o audio nel tuo sito web, puoi incorporare servizi online (ad esempio YouTube, Vimeo o Dailymotion), oppure includere i tuoi video (vedi sotto per i costi di banda).

Per i file audio, puoi trovare software gratuiti ([Audacity](https://www.audacityteam.org/), [Wavosaur](https://www.wavosaur.com/)), o spendere fino a qualche centinaio di dollari ([Sound Forge](https://www.magix.com/us/music-editing/sound-forge/), [Adobe Audition](https://www.adobe.com/products/audition.html)). Allo stesso modo, il software di editing video può essere gratuito ([PiTiVi](https://www.pitivi.org/), [OpenShot](https://www.openshot.org/) per Linux, [iMovie](https://support.apple.com/imovie) per Mac), meno di $100 ([Adobe Premiere Elements](https://www.adobe.com/products/premiere-elements.html)), o diversi centinaia di dollari ([Adobe Premiere Pro](https://www.adobe.com/products/premiere.html), [Avid Media Composer](https://www.avid.com/media-composer), [Final Cut Pro](https://www.apple.com/final-cut-pro/)). Il software che hai ricevuto con la tua fotocamera digitale potrebbe soddisfare tutte le tue esigenze.

### Strumenti di pubblicazione

Hai anche bisogno di un modo per caricare file: dal tuo hard disk a un server web distante. Per fare ciò dovresti usare uno strumento di pubblicazione come un {{Glossary("FTP", "client (S)FTP")}}, [RSync](https://en.wikipedia.org/wiki/Rsync), o [Git/GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

Ogni sistema operativo include un client (S)FTP, come parte del suo file manager. Windows Explorer, Nautilus (un comune file manager Linux) e il Mac Finder includono questa funzionalità. Tuttavia, spesso si scelgono client (S)FTP dedicati per visualizzare le directory locali o remote affiancate e memorizzare le password del server.

Se desideri installare un client (S)FTP, ci sono diverse opzioni affidabili e gratuite: ad esempio, [FileZilla](https://filezilla-project.org/) per tutte le piattaforme, [WinSCP](https://winscp.net/eng/index.php) per Windows, [Cyberduck](https://cyberduck.io/) per Mac o Windows, [e altri](https://en.wikipedia.org/wiki/List_of_FTP_server_software).

Poiché FTP è intrinsecamente insicuro, dovresti assicurarti di usare SFTP — la versione sicura e crittografata di FTP che la maggior parte dei siti di hosting con cui avrai a che fare oggi offrono di default — o un'altra soluzione sicura come Rsync su SSH.

## Browser

Probabilmente hai già un browser o puoi ottenerne uno gratuitamente. Se necessario, scarica [Firefox](https://www.mozilla.org/en-US/firefox/all/) o [Google Chrome](https://www.google.com/chrome/).

## Accesso al web

### Computer / modem

Hai bisogno di un computer. I costi possono variare enormemente, a seconda del tuo budget, e di dove vivi. Per pubblicare un sito web base, hai bisogno solo di un computer di base in grado di lanciare un editor e un browser web, quindi il livello di ingresso può essere piuttosto basso.

Ovviamente, avrai bisogno di un computer più serio se vuoi produrre progetti complicati, ritoccare foto o produrre file audio e video.

Devi caricare contenuti su un server remoto (vedi sotto _Hosting_), quindi hai bisogno di un modem. Il tuo {{Glossary("ISP", "ISP")}} può affittarti la connettività Internet per alcuni dollari al mese, anche se il tuo budget potrebbe variare, a seconda della tua posizione.

### Accesso ISP

Assicurati di avere una sufficiente {{Glossary("Bandwidth", "banda larga")}}:

- Un accesso a bassa larghezza di banda potrebbe essere adeguato per supportare un sito web 'semplice': immagini di dimensioni ragionevoli, testi, alcuni CSS e JavaScript. Ti costerà probabilmente qualche decina di dollari, incluso l'affitto del modem.
- D'altra parte, avrai bisogno di una connessione ad alta larghezza di banda, come DSL, cavo o fibra, se desideri un sito web più avanzato con centinaia di file, o se vuoi distribuire direttamente file video/audio pesanti dal tuo server web. Potrebbe costare lo stesso dell'accesso a bassa larghezza di banda, fino a diverse centinaia di dollari al mese per esigenze più professionali.

## Hosting

### Comprendere la larghezza di banda

I fornitori di hosting ti addebitano in base a quanta {{Glossary("Bandwidth", "larghezza di banda")}} consuma il tuo sito web. Questo dipende da quante persone, e robot di crawling web, accedono al tuo contenuto in un dato momento, e quanto spazio occupa il tuo contenuto sul server. È per questo motivo che le persone di solito memorizzano i loro video su servizi dedicati come YouTube, Dailymotion e Vimeo. Ad esempio, il tuo fornitore potrebbe avere un piano che include fino a diverse migliaia di visitatori al giorno, per un uso di larghezza di banda "ragionevole". Fai attenzione, tuttavia, poiché questo è definito in modo diverso da un fornitore di hosting all'altro. Ricorda che un hosting personale affidabile a pagamento può costare intorno ai dieci-quindici dollari al mese.

> [!NOTE]
> Non esiste qualcosa come la larghezza di banda "illimitata". Se consumi una quantità enorme di larghezza di banda, preparati a pagare una quantità enorme di denaro.

### Nomi di dominio

Il tuo nome di dominio deve essere acquistato tramite un fornitore di nomi di dominio (un registrar). Il tuo fornitore di hosting potrebbe anche essere un registrar ([Ionos](https://www.ionos.com/), [Gandi](https://www.gandi.net/en-US) ad esempio sono allo stesso tempo registrar e fornitori di hosting). Il nome di dominio di solito costa $5-15 all'anno. Questo costo varia a seconda di:

- Obblighi locali: alcuni nomi di domini di primo livello nazionali sono più costosi, poiché diversi paesi stabiliscono prezzi diversi.
- Servizi associati al nome di dominio: alcuni registrar forniscono protezione da spam nascondendo il tuo indirizzo postale e indirizzo email dietro ai loro indirizzi: l'indirizzo postale può essere fornito per conto del registrar, e il tuo indirizzo email può essere oscurato tramite l'alias del tuo registrar.

### Hosting fai-da-te vs. hosting "confezionato"

Quando vuoi pubblicare un sito web, potresti fare tutto da solo: configurare un database (se necessario), un Content Management System, o CMS (come [WordPress](https://wordpress.org/), [Dotclear](https://dotclear.org/), [spip](https://www.spip.net/en_rubrique25.html), ecc.), caricare modelli preconfezionati o realizzati da te.

Potresti usare l'ambiente del tuo fornitore di hosting, per circa dieci-quindici dollari al mese, o iscriverti direttamente a un servizio di hosting dedicato con CMS preconfezionati (ad es., [WordPress](https://wordpress.com/), [Tumblr](https://www.tumblr.com/), [Blogger](https://www.blogger.com/)). Per quest'ultimo, non dovrai pagare nulla, ma potresti avere meno controllo sulla personalizzazione e altre opzioni.

### Hosting gratuito vs. hosting a pagamento

Potresti chiederti, perché dovrei pagare per il mio hosting quando ci sono così tanti servizi gratuiti?

- Hai più libertà quando paghi. Il tuo sito web è tuo, e puoi migrare senza problemi da un fornitore di hosting all'altro.
- I fornitori di hosting gratuito potrebbero aggiungere pubblicità ai tuoi contenuti, al di fuori del tuo controllo.

È meglio optare per un hosting a pagamento piuttosto che affidarsi all'hosting gratuito, poiché è possibile spostare i tuoi file facilmente e il tempo di attività è garantito dalla maggior parte dei siti a pagamento. La maggior parte dei fornitori di hosting ti offre un grande sconto per iniziare.

Alcune persone optano per un approccio misto. Ad esempio, il loro blog principale su un host a pagamento con un nome di dominio completo, e contenuti spontanei e meno strategici su un servizio di hosting gratuito.

## Agenzie professionali per siti web e hosting

Se desideri un sito web professionale, probabilmente chiederai a un'agenzia web di realizzarlo per te.

Qui, i costi dipendono da fattori multipli, come:

- Si tratta di un sito semplice con poche pagine di testo? O di un sito più complesso, con migliaia di pagine?
- Vuoi aggiornarlo regolarmente? O sarà un sito web statico?
- Il sito deve connettersi alla struttura IT della tua azienda per raccogliere contenuti (ad esempio, dati interni)?
- Vuoi qualche nuova funzione brillante che è popolare al momento? Al momento della scrittura, i clienti stanno cercando pagine singole con complesso parallasse.
- Hai bisogno che l'agenzia crei storie utente o risolva problemi complessi di {{Glossary("UX", "UX")}}? Ad esempio, creando una strategia per coinvolgere gli utenti o test A/B per scegliere una soluzione tra varie idee.

E per l'hosting devi considerare le seguenti scelte:

- Vuoi server ridondanti, nel caso il tuo server si blocchi?
- È adeguata un'affidabilità del 95%, o hai bisogno di un servizio professionale disponibile 24/7?
- Vuoi server dedicati di alto profilo e altamente reattivi, o puoi accontentarti di una macchina condivisa più lenta?

A seconda di come rispondi a queste domande, il tuo sito potrebbe costare da migliaia a centinaia di migliaia di dollari.

## Passaggi successivi

Ora che capisci quanto può costarti il tuo sito web, è tempo di iniziare a progettare quel sito e [configurare il tuo ambiente di lavoro](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

- Continua a leggere su [come scegliere e installare un editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors).
- Se sei più concentrato sul design, dai un'occhiata all'[anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
