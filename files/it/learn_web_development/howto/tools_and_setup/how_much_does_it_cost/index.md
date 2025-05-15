---
title: Quanto costa fare qualcosa sul Web?
slug: Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost
l10n:
  sourceCommit: e488eba036b2fee56444fd579c3759ef45ff2ca8
---

Partecipare al Web non è economico come sembra. In questo articolo discutiamo quanto potresti dover spendere e perché.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Lei dovrebbe già comprendere
        <a href="/it/docs/Learn_web_development/Howto/Tools_and_setup/What_software_do_I_need"
          >quale software è necessario</a
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
        Esaminare il processo completo per creare un sito web e scoprire quanto
        può costare ogni passaggio.
      </td>
    </tr>
  </tbody>
</table>

## Sommario

Quando si lancia un sito web, si potrebbe non spendere nulla o i costi potrebbero salire alle stelle. In questo articolo discutiamo quanto costa tutto e come ottenere ciò per cui si paga (o non si paga).

## Software

### Editor di testo

Probabilmente ha già un editor di testo: come Notepad su Windows, Gedit su Linux, TextEdit su Mac. Avrà un'esperienza più facile nella scrittura del codice scegliendo un editor che codifica i colori, controlla la sintassi e l'assiste con la struttura del codice.

Molti editor sono gratuiti, per esempio [Brackets](https://brackets.io/), [Bluefish](https://bluefish.openoffice.nl/index.html), [TextWrangler](https://www.barebones.com/products/textwrangler/), [Eclipse](https://www.eclipse.org/), [NetBeans](https://netbeans.apache.org/), e [Visual Studio Code](https://code.visualstudio.com/). Alcuni, come [Sublime Text](https://www.sublimetext.com/), possono essere testati quanto si desidera, ma è incoraggiato a pagare. Altri, come [PhpStorm](https://www.jetbrains.com/phpstorm/), possono costare da alcune decine a 200 dollari, a seconda del piano acquistato. Alcuni di essi, come [Microsoft Visual Studio](https://visualstudio.microsoft.com/), possono costare centinaia o migliaia di dollari; tuttavia, Visual Studio Community è gratuito per sviluppatori individuali o progetti open source. Spesso, gli editor a pagamento avranno una versione di prova.

Per iniziare, suggeriamo di provare diversi editor per capire quale funziona meglio per lei. Se sta scrivendo solo semplice {{Glossary("HTML", "HTML")}}, {{Glossary("CSS", "CSS")}}, e {{Glossary("JavaScript", "JavaScript")}}, scelga un editor semplice.

Il prezzo non riflette in modo affidabile la qualità o l'utilità di un editor di testo. Deve provarli personalmente e decidere se rispondono alle sue esigenze. Ad esempio, Sublime Text è economico, ma viene fornito con molti plugin gratuiti che possono estenderne notevolmente la funzionalità.

### Editor di immagini

Il suo sistema probabilmente include un editor o visualizzatore di immagini: Paint su Windows, Eye of GNOME su Ubuntu, Preview su Mac. Questi programmi sono relativamente limitati; presto vorrà un editor più robusto per aggiungere livelli, effetti e raggruppamenti.

Gli editor possono essere gratuiti ([GIMP](https://www.gimp.org/), [Paint.NET](https://www.getpaint.net/)), moderatamente costosi ([PaintShop Pro](https://www.paintshoppro.com/), meno di $100), o diverse centinaia di dollari ([Adobe Photoshop](https://www.adobe.com/products/photoshop.html)).

Può usare uno qualsiasi di essi, poiché avranno funzionalità simili, anche se alcuni sono talmente completi che non utilizzerà mai tutte le funzionalità. Se a un certo punto deve scambiare progetti con altri designer, dovrebbe scoprire quali strumenti utilizzano. Tutti gli editor possono esportare progetti finiti in formati di file standard, ma ogni editor salva i progetti ongoing nel proprio formato specializzato. La maggior parte delle immagini su Internet è protetta da copyright, quindi è meglio controllare la licenza del file prima di utilizzarlo. Siti come [Pixabay](https://pixabay.com/) forniscono immagini sotto licenza CC0, permettendo di usarle, modificarle e pubblicarle anche con modifiche a scopi commerciali.

### Editor multimediali

Se si desidera includere video o audio nel suo sito web, può incorporare servizi online (ad esempio YouTube, Vimeo o Dailymotion) oppure includere i propri video (vedi più avanti per i costi della larghezza di banda).

Per i file audio, può trovare software gratuiti ([Audacity](https://www.audacityteam.org/), [Wavosaur](https://www.wavosaur.com/)), o pagare fino a alcune centinaia di dollari ([Sound Forge](https://www.magix.com/us/music-editing/sound-forge/), [Adobe Audition](https://www.adobe.com/products/audition.html)). Allo stesso modo, il software di editing video può essere gratuito ([PiTiVi](https://www.pitivi.org/), [OpenShot](https://www.openshot.org/) per Linux, [iMovie](https://support.apple.com/imovie) per Mac), costare meno di $100 ([Adobe Premiere Elements](https://www.adobe.com/products/premiere-elements.html)) o diverse centinaia di dollari ([Adobe Premiere Pro](https://www.adobe.com/products/premiere.html), [Avid Media Composer](https://www.avid.com/media-composer), [Final Cut Pro](https://www.apple.com/final-cut-pro/)). Il software ricevuto con la sua fotocamera digitale potrebbe coprire tutte le sue necessità.

### Strumenti di pubblicazione

Ha anche bisogno di un modo per caricare i file: dal suo hard disk a un server web remoto. Per farlo dovrebbe usare uno strumento di pubblicazione come un client {{Glossary("FTP", "(S)FTP")}}, [RSync](https://en.wikipedia.org/wiki/Rsync), o [Git/GitHub](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site).

Ogni sistema operativo include un client (S)FTP, come parte del suo gestore di file. Esplora Risorse, Nautilus (un gestore di file comune su Linux) e il Finder del Mac includono questa funzionalità. Tuttavia, spesso le persone scelgono client (S)FTP dedicati per visualizzare le directory locali o remote fianco a fianco e memorizzare le password del server.

Se desidera installare un client (S)FTP, ci sono diverse opzioni affidabili e gratuite: ad esempio, [FileZilla](https://filezilla-project.org/) per tutte le piattaforme, [WinSCP](https://winscp.net/eng/index.php) per Windows, [Cyberduck](https://cyberduck.io/) per Mac o Windows, [e altro](https://en.wikipedia.org/wiki/List_of_FTP_server_software).

Poiché FTP è intrinsecamente insicuro, dovrebbe assicurarsi di utilizzare SFTP — la versione sicura e criptata di FTP che la maggior parte degli host con cui avrà a che fare oggi offrirà per default — o un'altra soluzione sicura come Rsync su SSH.

## Browser

Probabilmente ha già un browser o può ottenerne uno gratuitamente. Se necessario, scarichi [Firefox](https://www.mozilla.org/en-US/firefox/all/) o [Google Chrome](https://www.google.com/chrome/).

## Accesso al web

### Computer / modem

Ha bisogno di un computer. I costi possono variare enormemente, a seconda del suo budget e di dove vive. Per pubblicare un sito web essenziale, le serve solo un computer di base in grado di avviare un editor e un browser Web, quindi il livello di ingresso può essere abbastanza basso.

Naturalmente, avrà bisogno di un computer più serio se vuole produrre design complicati, ritoccare foto o produrre file audio e video.

Deve caricare contenuti su un server remoto (vedi _Hosting_ qui sotto), quindi le serve un modem. Il suo {{Glossary("ISP", "ISP")}} può affittarle la connettività Internet per alcuni dollari al mese, anche se il suo budget potrebbe variare a seconda della sua posizione.

### Accesso ISP

Assicurati di avere sufficiente {{Glossary("Bandwidth", "larghezza di banda")}}:

- Un accesso a bassa larghezza di banda può essere adeguato per supportare un sito web "semplice": immagini di dimensioni ragionevoli, testi, alcuni CSS e JavaScript. Probabilmente le costerà alcune decine di dollari, incluso l'affitto del modem.
- D'altra parte, avrà bisogno di una connessione ad alta larghezza di banda, come DSL, cavo o fibra ottica, se desidera un sito web più avanzato con centinaia di file, o se desidera fornire direttamente dal suo server web file video/audio pesanti. Potrebbe costare lo stesso dell'accesso a bassa larghezza di banda, fino a diverse centinaia di dollari al mese per esigenze più professionali.

## Hosting

### Comprendere la larghezza di banda

I provider di hosting le addebitano in base a quanta {{Glossary("Bandwidth", "larghezza di banda")}} il suo sito web consuma. Questo dipende da quante persone e robot di scansione web accedono al suo contenuto in un dato momento, e quanto spazio sul server il suo contenuto occupa. Questo è il motivo per cui le persone solitamente memorizzano i loro video su servizi dedicati come YouTube, Dailymotion e Vimeo. Ad esempio, il suo provider può avere un piano che include fino a diverse migliaia di visitatori al giorno, per un uso "ragionevole" della larghezza di banda. Faccia attenzione, tuttavia, poiché questo è definito diversamente da un provider di hosting all'altro. Tenga presente che un hosting personale affidabile e a pagamento può costare circa dieci o quindici dollari al mese.

> [!NOTE]
> Non esiste la "larghezza di banda illimitata". Se consuma una quantità enorme di larghezza di banda, si aspetti di pagare una quantità enorme di denaro.

### Nomi di dominio

Il suo nome di dominio deve essere acquistato tramite un provider di nomi di dominio (un registrar). Il suo provider di hosting può anche essere un registrar ([Ionos](https://www.ionos.com/), [Gandi](https://www.gandi.net/en-US) ad esempio sono allo stesso tempo registrar e provider di hosting). Il nome di dominio di solito costa 5-15 dollari all'anno. Questo costo varia a seconda di:

- Obblighi locali: alcuni nomi di dominio di primo livello di paese sono più costosi, poiché diversi paesi fissano prezzi differenti.
- Servizi associati al nome di dominio: alcuni registrar forniscono protezione dallo spam nascondendo il suo indirizzo postale e l'indirizzo email dietro i propri indirizzi: l'indirizzo postale può essere fornito tramite il registrar, e il suo indirizzo email può essere oscurato tramite l'alias del registrar.

### Hosting fai-da-te vs. hosting "pacchettizzato"

Quando vuole pubblicare un sito web, potrebbe fare tutto da sola: impostare un database (se necessario), un Sistema di Gestione dei Contenuti, o {{Glossary("CMS", "CMS")}} (come [WordPress](https://wordpress.org/), [Dotclear](https://dotclear.org/), [spip](https://www.spip.net/en_rubrique25.html), ecc.), caricare modelli preimpostati o propri.

Potrebbe usare l'ambiente del suo provider di hosting, per circa dieci o quindici dollari al mese, o iscriversi direttamente a un servizio di hosting dedicato con CMS pre-pacchettizzati (es. [WordPress](https://wordpress.com/), [Tumblr](https://www.tumblr.com/), [Blogger](https://www.blogger.com/)). Per quest'ultimo, lei non dovrà pagare nulla, ma potrebbe avere meno controllo sulla personalizzazione e altre opzioni.

### Hosting gratuito vs. hosting a pagamento

Potrebbe chiedersi, perché dovrei pagare per il mio hosting quando ci sono tanti servizi gratuiti?

- Ha più libertà quando paga. Il suo sito web è suo, e può migrare senza problemi da un provider di hosting all'altro.
- I provider di hosting gratuiti possono aggiungere pubblicità al suo contenuto, senza il suo controllo.

È meglio optare per un hosting a pagamento piuttosto che affidarsi a un hosting gratuito, poiché è possibile spostare i file facilmente e il tempo di attività è garantito dalla maggior parte dei siti a pagamento. La maggior parte dei provider di hosting le offre uno sconto enorme per iniziare.

Alcune persone optano per un approccio misto. Ad esempio, il loro blog principale su un host a pagamento con un nome di dominio completo, e contenuti spontanei e meno strategici su un servizio host gratuito.

## Agenzie di siti web professionali e hosting

Se desidera un sito web professionale, probabilmente chiederà a un'agenzia web di farlo per lei.

Qui, i costi dipendono da più fattori, come:

- È un sito web semplice con poche pagine di testo? O un sito web più complesso con migliaia di pagine?
- Vuole aggiornarlo regolarmente? O sarà un sito web statico?
- Il sito web deve connettersi alla struttura IT della sua azienda per raccogliere contenuti (ad esempio, dati interni)?
- Vuole qualche nuova caratteristica brillante che è popolare al momento? Al momento della stesura, i clienti cercano pagine singole con complessi effetti parallasse.
- Ha bisogno che l'agenzia concepisca storie utente o risolva problemi complessi di {{Glossary("UX", "UX")}}? Ad esempio, creando una strategia per coinvolgere gli utenti, o effettuando test A/B per scegliere una soluzione tra diverse idee.

E per l'hosting deve considerare le seguenti scelte:

- Vuole server ridondanti, nel caso il suo server si blocchi?
- È adeguato il 95% di affidabilità, o ha bisogno di un servizio professionale, 24 ore su 24?
- Vuole server dedicati di alto profilo, ultra reattivi, o può adattarsi a una macchina condivisa più lenta?

A seconda di come risponde a queste domande, il suo sito potrebbe costare migliaia a centinaia di migliaia di dollari.

## Prossimi passi

Ora che ha compreso quanto il suo sito web potrebbe costarle, è ora di iniziare a progettare quel sito web e [configurare il suo ambiente di lavoro](/it/docs/Learn_web_development/Howto/Tools_and_setup/set_up_a_local_testing_server).

- Continui a leggere su [come scegliere e installare un editor di testo](/it/docs/Learn_web_development/Howto/Tools_and_setup/Available_text_editors).
- Se è più focalizzata sul design, dia un'occhiata all'[anatomia di una pagina web](/it/docs/Learn_web_development/Howto/Design_and_accessibility/Common_web_layouts).
