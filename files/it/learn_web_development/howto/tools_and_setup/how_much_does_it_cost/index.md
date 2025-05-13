---
title: Quanto costa fare qualcosa sul Web?
slug: Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost
l10n:
  sourceCommit: 479ea4c8bff4b900a7968413287c77dde2b0c20f
---

Coinvolgersi nel Web non è economico come sembra. In questo articolo discutiamo quanto potrebbe costare e perché.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Dovrebbe già comprendere
        <a href="/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need">quale software è necessario</a>, la differenza tra
        <a href="/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web">una pagina web, un sito web, ecc.</a>, e cos'è
        <a href="/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name">un nome di dominio</a>.
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivo:</th>
      <td>
        Rivedere il processo completo per creare un sito web e scoprire quanto può costare ogni fase.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Quando si lancia un sito web, si può spendere nulla, oppure i costi possono diventare esorbitanti. In questo articolo discutiamo quanto costa tutto, e come si ottiene ciò per cui si paga (o non si paga).

## Software

### Editor di testo

Probabilmente ha già un editor di testo: ad esempio, Blocco Note su Windows, Gedit su Linux, TextEdit su Mac. Scrivere codice sarà più facile se sceglie un editor che evidenzi la sintassi, controlli la sintassi e la assista con la struttura del codice.

Molti editor sono gratuiti, ad esempio [Brackets](https://brackets.io/), [Bluefish](https://bluefish.openoffice.nl/index.html), [TextWrangler](https://www.barebones.com/products/textwrangler/), [Eclipse](https://www.eclipse.org/), [NetBeans](https://netbeans.apache.org/), e [Visual Studio Code](https://code.visualstudio.com/). Alcuni, come [Sublime Text](https://www.sublimetext.com/), possono essere testati indefinitamente, ma si è incoraggiati a pagare. Altri, come [PhpStorm](https://www.jetbrains.com/phpstorm/), possono costare tra poche decine e 200 dollari, a seconda del piano acquistato. Alcuni di essi, come [Microsoft Visual Studio](https://visualstudio.microsoft.com/), possono costare centinaia o migliaia di dollari; tuttavia Visual Studio Community è gratuito per sviluppatori individuali o progetti open source. Spesso, gli editor a pagamento offriranno una versione di prova.

Per iniziare, suggeriamo di provare diversi editor per capire quale funziona meglio per lei. Se sta solo scrivendo semplice {{Glossary("HTML", "HTML")}}, {{Glossary("CSS", "CSS")}} e {{Glossary("JavaScript", "JavaScript")}}, opti per un editor semplice.

Il prezzo non riflette in modo affidabile la qualità o l'utilità di un editor di testo. Deve provarlo personalmente e decidere se soddisfa le sue esigenze. Ad esempio, Sublime Text è economico, ma viene fornito con molti plugin gratuiti che possono estenderne notevolmente la funzionalità.

### Editor di immagini

Il suo sistema probabilmente include un editor o visualizzatore di immagini: Paint su Windows, Eye of GNOME su Ubuntu, Preview su Mac. Questi programmi sono relativamente limitati, presto vorrà un editor più robusto per aggiungere livelli, effetti e raggruppamenti.

Gli editor possono essere gratuiti ([GIMP](https://www.gimp.org/), [Paint.NET](https://www.getpaint.net/)), moderatamente costosi ([PaintShop Pro](https://www.paintshoppro.com/), meno di 100 dollari), o arrivare a diverse centinaia di dollari ([Adobe Photoshop](https://www.adobe.com/products/photoshop.html)).

Può usarne uno qualsiasi, poiché avranno funzionalità simili, sebbene alcuni siano così completi che non utilizzerà mai tutte le funzionalità. Se a un certo punto ha bisogno di scambiare progetti con altri designer, dovrebbe scoprire quali strumenti stanno usando loro. Gli editor possono esportare tutti i progetti finiti in formati di file standard, ma ciascun editor salva i progetti in corso nel proprio formato specializzato. La maggior parte delle immagini su Internet è protetta da copyright, quindi è meglio controllare la licenza del file prima di utilizzarlo. Siti come [Pixabay](https://pixabay.com/) forniscono immagini sotto licenza CC0, in modo che possa utilizzare, modificare e pubblicare anche con modifiche per uso commerciale.

### Editor multimediali

Se vuole includere video o audio nel suo sito web, può incorporare servizi online (ad esempio YouTube, Vimeo o Dailymotion), oppure includere i suoi video (vedi sotto per i costi di larghezza di banda).

Per i file audio, può trovare software gratuiti ([Audacity](https://www.audacityteam.org/), [Wavosaur](https://www.wavosaur.com/)) o pagare fino a qualche centinaio di dollari ([Sound Forge](https://www.magix.com/us/music-editing/sound-forge/), [Adobe Audition](https://www.adobe.com/products/audition.html)). Allo stesso modo, il software di editing video può essere gratuito ([PiTiVi](https://www.pitivi.org/), [OpenShot](https://www.openshot.org/) per Linux, [iMovie](https://support.apple.com/imovie) per Mac), costare meno di 100 dollari ([Adobe Premiere Elements](https://www.adobe.com/products/premiere-elements.html)), o arrivare a diverse centinaia di dollari ([Adobe Premiere Pro](https://www.adobe.com/products/premiere.html), [Avid Media Composer](https://www.avid.com/media-composer), [Final Cut Pro](https://www.apple.com/final-cut-pro/)). Il software fornito con la sua fotocamera digitale potrebbe coprire tutte le sue esigenze.

### Strumenti di pubblicazione

Ha anche bisogno di un modo per caricare file: dal suo disco rigido a un server web remoto. Per fare ciò, dovrebbe usare uno strumento di pubblicazione come un {{Glossary("FTP", "(S)FTP client")}}, [RSync](https://en.wikipedia.org/wiki/Rsync), o [Git/GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

Ogni sistema operativo include un client (S)FTP, come parte del suo file manager. Windows Explorer, Nautilus (un comune file manager per Linux) e il Finder di Mac includono tutti questa funzionalità. Tuttavia, le persone spesso scelgono client (S)FTP dedicati per visualizzare directory locali o remote fianco a fianco e memorizzare le password del server.

Se desidera installare un client (S)FTP, ci sono diverse opzioni affidabili e gratuite: ad esempio, [FileZilla](https://filezilla-project.org/) per tutte le piattaforme, [WinSCP](https://winscp.net/eng/index.php) per Windows, [Cyberduck](https://cyberduck.io/) per Mac o Windows, [e altro](https://en.wikipedia.org/wiki/List_of_FTP_server_software).

Poiché FTP è intrinsecamente insicuro, dovrebbe assicurarsi di utilizzare SFTP — la versione sicura e criptata di FTP che la maggior parte dei siti di hosting con cui si interfaccia offriranno di default — o un'altra soluzione sicura come Rsync su SSH.

## Browser

Ha già un browser o può ottenerne uno gratuitamente. Se necessario, scarichi Firefox [qui](https://www.mozilla.org/en-US/firefox/all/) o Google Chrome [qui](https://www.google.com/chrome/).

## Accesso al Web

### Computer / modem

Ha bisogno di un computer. I costi possono variare notevolmente, a seconda del suo budget e di dove vive. Per pubblicare un sito web di base, serve solo un computer semplice capace di avviare un editor e un browser Web, quindi il livello di ingresso può essere piuttosto basso.

Ovviamente, avrà bisogno di un computer più serio se vuole produrre design complessi, ritoccare foto o creare file audio e video.

Deve caricare contenuti su un server remoto (vedi _Hosting_ sotto), quindi è necessario un modem. Il suo {{Glossary("ISP", "ISP")}} può affittarle la connettività Internet per pochi dollari al mese, tuttavia il suo budget potrebbe variare, a seconda della sua posizione.

### Accesso ISP

Assicurarsi di avere una sufficiente {{Glossary("Bandwidth", "larghezza di banda")}}:

- L'accesso a bassa larghezza di banda può essere adeguato per supportare un sito web 'semplice': immagini di dimensioni ragionevoli, testi, alcuni CSS e JavaScript. Ciò probabilmente le costerà poche decine di dollari, incluso l'affitto del modem.
- D'altra parte, avrà bisogno di una connessione ad alta larghezza di banda, come DSL, cavo o accesso in fibra, se desidera un sito web più avanzato con centinaia di file o se intende consegnare direttamente dal suo server web file pesanti di video/audio. Potrebbe costare lo stesso dell'accesso a bassa larghezza di banda, fino a diverse centinaia di dollari al mese per esigenze più professionali.

## Hosting

### Comprensione della larghezza di banda

I fornitori di hosting la addebitano in base a quanta {{Glossary("Bandwidth", "larghezza di banda")}} il suo sito web consuma. Questo dipende da quante persone, e robot di crawling del web, accedono ai suoi contenuti in un dato momento e quanto spazio server i suoi contenuti occupano. Questo è il motivo per cui le persone normalmente memorizzano i loro video su servizi dedicati come YouTube, Dailymotion, e Vimeo. Ad esempio, il suo fornitore può avere un piano che include fino a migliaia di visitatori al giorno, per un uso di larghezza di banda "ragionevole". Faccia attenzione, comunque, poiché questo è definito in modo diverso da un fornitore di hosting all'altro. Tenga presente che un hosting personale affidabile, a pagamento, può costare circa dieci-quindici dollari al mese.

> [!NOTE]
> Non esiste una cosa come la larghezza di banda "illimitata". Se consuma una grande quantità di larghezza di banda, si aspetti di pagare una grande quantità di denaro.

### Nomi di dominio

Il suo nome di dominio deve essere acquistato tramite un fornitore di nomi di dominio (un registrar). Il suo fornitore di hosting può anche essere un registrar ([Ionos](https://www.ionos.com/), [Gandi](https://www.gandi.net/en-US) per esempio sono contemporaneamente registrar e fornitori di hosting). Di solito il costo di un nome di dominio è di 5-15 dollari all'anno. Questo costo varia a seconda di:

- Obblighi locali: alcuni nomi di dominio di primo livello di paese sono più costosi, poiché diversi paesi stabiliscono prezzi differenti.
- Servizi associati al nome di dominio: alcuni registrar forniscono protezione da spam nascondendo il suo indirizzo postale e indirizzo email dietro i loro indirizzi: l'indirizzo postale può essere fornito a cura del registrar e il suo indirizzo email può essere oscurato tramite l'alias del registrar.

### Hosting fai da te vs. "pacchetti" di hosting

Quando desidera pubblicare un sito web, può fare tutto da solo: impostare un database (se necessario), Sistema di Gestione dei Contenuti o {{Glossary("CMS", "CMS")}} (come [WordPress](https://wordpress.org/), [Dotclear](https://dotclear.org/), [spip](https://www.spip.net/en_rubrique25.html), ecc.), caricare modelli preconfezionati o personali.

Potrebbe usare l’ambiente del suo fornitore di hosting, per circa dieci-quindici dollari al mese, o sottoscrivere direttamente un servizio di hosting dedicato con CMS preconfezionati (ad esempio, [WordPress](https://wordpress.com/), [Tumblr](https://www.tumblr.com/), [Blogger](https://www.blogger.com/)). In quest'ultimo caso, non dovrà pagare nulla, ma potrebbe avere meno controllo sui modelli e altre opzioni.

### Hosting gratuito vs. hosting a pagamento

Può chiedersi, perché dovrei pagare per il mio hosting quando ci sono così tanti servizi gratuiti?

- Ha più libertà quando paga. Il suo sito web è suo, e può migrare senza problemi da un fornitore di hosting all'altro.
- I fornitori di hosting gratuito possono aggiungere pubblicità ai suoi contenuti, oltre il suo controllo.

È meglio optare per l’hosting a pagamento piuttosto che affidarsi all’hosting gratuito, poiché è possibile spostare facilmente i suoi file e il tempo di attività è garantito dalla maggior parte dei siti a pagamento. La maggior parte dei fornitori di hosting le offre un grande sconto per iniziare.

Alcune persone optano per un approccio misto. Ad esempio, hanno il loro blog principale su un host a pagamento con un nome di dominio completo e, spontaneità, contenuti meno strategici su un servizio di host gratuito.

## Agenzie professionali di siti web e hosting

Se desidera un sito web professionale, probabilmente chiederà a un'agenzia web di farlo per lei.

Qui, i costi dipendono da vari fattori, come:

- Questo è un sito semplice con poche pagine di testo? O un sito più complesso, lungo mille pagine?
- Vuole aggiornarlo regolarmente? O sarà un sito statico?
- Il sito web deve connettersi alla struttura IT della sua azienda per raccogliere contenuti (ad esempio, dati interni)?
- Vuole qualche nuovissima funzionalità che è popolare al momento? Al momento della scrittura, i clienti cercano singole pagine con parallasse complesso.
- Avrà bisogno che l'agenzia pensi a user stories o risolva problemi complessi di {{Glossary("UX", "UX")}}? Per esempio, creare una strategia per coinvolgere gli utenti, o A/B testing per scegliere una soluzione tra diverse idee.

E per l'hosting bisogna considerare le seguenti scelte:

- Vuole server ridondanti, nel caso il suo server vada giù?
- È sufficiente un'affidabilità del 95%, o ha bisogno di un servizio professionale, 24 ore su 24, 7 giorni su 7?
- Vuole server dedicati di alto profilo, ultra reattivi, o può far fronte a una macchina condivisa e più lenta?

A seconda di come risponde a queste domande, il suo sito potrebbe costare da migliaia a centinaia di migliaia di dollari.

## Passi successivi

Ora che comprende quali tipi di costi potrebbe avere il suo sito web, è il momento di iniziare a progettare quel sito web e [configurare il suo ambiente di lavoro](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

- Legga come [scegliere e installare un editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors).
- Se è più interessato al design, dia un'occhiata all'[anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
