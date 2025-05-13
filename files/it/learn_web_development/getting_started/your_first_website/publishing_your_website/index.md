---
title: Pubblicare il suo sito web
short-title: Publishing
slug: Learn_web_development/Getting_started/Your_first_website/Publishing_your_website
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Web_standards", "Learn_web_development/Getting_started/Your_first_website")}}

Una volta terminato di scrivere il codice e organizzare i file che compongono il suo sito web, dovrà metterlo online affinché le persone possano trovarlo. Questo articolo spiega come mettere online il suo sito web di esempio con poco sforzo.

> [!NOTE]
> Avrà bisogno di un sito web di esempio disponibile sul suo computer locale per seguire questo articolo. Dovrebbe contenere almeno un file `index.html` valido. Se non lo ha ancora fatto, le consigliamo di crearne uno lavorando attraverso gli articoli precedenti in questo modulo, a partire da [Come sarà il suo sito web?](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base del sistema operativo del suo computer, del software di base che utilizzerà per costruire un sito web e dei sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Gli strumenti e i concetti di base coinvolti nella pubblicazione di un sito web — hosting, domini, programmi FTP.</li>
          <li>Quali alternative di hosting sono disponibili, ad esempio Google App Engine, GitHub e CodePen.</li>
          <li>Pubblicare un sito web utilizzando GitHub Pages.</li>
          <li>Hosting, come acquistarlo e come mettere un sito web online.</li>
          <li>Come registrare un dominio.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali sono le opzioni?

Pubblicare un sito web è un argomento complesso perché ci sono molti modi per farlo. Questo articolo non tenta di documentare tutti i metodi possibili. Invece, spiega i vantaggi e gli svantaggi di tre approcci pratici per i principianti. Poi analizza un metodo che può funzionare subito per molti lettori.

### Ottenere hosting e un nome di dominio

Per avere maggiore controllo sul contenuto e sull'aspetto del sito web, la maggior parte dei professionisti/aziende sceglie di acquistare un hosting web e un nome di dominio:

- L'hosting web è uno spazio file affittato su un [server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) di un'azienda di hosting. Lei mette i file del sito web sul server web. Il server web fornisce il contenuto del sito web ai visitatori del sito.
- Un [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) è l'indirizzo web unico dove le persone possono trovare il suo sito, come `https://www.mozilla.org` o `https://www.bbc.co.uk`. Lei può affittare il suo nome di dominio per quanti anni desidera da un **registrar di domini**.

Se ottiene l'hosting web _e_ il nome di dominio dalla stessa azienda, tendono ad essere configurati automaticamente per comunicare tra loro. Tuttavia, se li ottiene da aziende separate, o desidera cambiare il suo hosting con un'altra azienda, sarà necessario fare un po' di configurazione per puntare il nome di dominio al server corretto. Questo è necessario affinché le persone vedano il suo sito web quando navigano verso quell'indirizzo web. Di solito si fa accedendo al sito web del registrar del dominio e impostando i [nameservers](https://kinsta.com/knowledgebase/what-is-a-nameserver/) del dominio su quelli forniti dall'azienda di hosting.

Le aziende usano vari meccanismi per trasferire i file ai loro server web. Molte avranno più di un'opzione; le opzioni tipiche includono:

- Un'interfaccia drag and drop (vedrà un esempio di questo in [Pubblicazione tramite GitHub](#pubblicazione_tramite_github), più avanti).
- Un programma {{Glossary("FTP", "File Transfer Protocol (FTP)")}}. I programmi FTP variano ampiamente, ma generalmente deve connettersi al suo server web utilizzando i dettagli forniti dalla sua azienda di hosting (tipicamente nome utente, password, nome host). Poi il programma le mostra i suoi file locali e i file del server web in due finestre, e le fornisce un modo per trasferire i file avanti e indietro.
- Mantenere il codice sorgente del sito web in un repository GitHub (vedi sotto) e concedere all'azienda di hosting l'accesso in modo che possano recuperare il codice sorgente, costruirlo se necessario e pubblicarlo.
- Alcune aziende forniranno [strumenti da riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) che può utilizzare per trasferire i suoi file.

#### Suggerimenti per trovare hosting e domini

- MDN non promuove specifiche aziende di hosting commerciali o registrar di nomi di dominio. Per trovare aziende di hosting e registrar, basta cercare "hosting web" e "nomi di dominio". Tutti i registrar avranno una funzione per permetterle di controllare se il nome di dominio che vuole è disponibile.
- Il suo {{Glossary("ISP", "provider di servizi internet")}} domestico o dell'ufficio può fornire un hosting limitato per un piccolo sito web. Il set di funzionalità disponibile sarà limitato, ma potrebbe essere perfetto per i suoi primi esperimenti.
- Sono disponibili anche servizi gratuiti come [Neocities](https://neocities.org/), [Google Sites](https://sites.google.com/) e [WordPress](https://wordpress.com/). Tali servizi possono essere limitati in portata, ma sono abbastanza buoni per esperimenti iniziali.

### Utilizzando uno strumento online come GitHub o Google App Engine

Alcuni strumenti le permettono di pubblicare il suo sito web online:

- [GitHub](https://github.com/) è un sito di "codifica sociale". Le permette di caricare repository di codice per la memorizzazione nel sistema di controllo versione [Git](https://git-scm.com/). Può quindi collaborare ai progetti di codice, e il sistema è open source per impostazione predefinita, il che significa che chiunque nel mondo può trovare il suo codice GitHub, usarlo, imparare da esso e migliorarlo. GitHub ha una caratteristica molto utile chiamata [GitHub Pages](https://pages.github.com/), che le permette di esporre il codice del sito web in diretta sul web.
- [Google App Engine](https://cloud.google.com/appengine) è una potente piattaforma che le permette di costruire ed eseguire applicazioni sull'infrastruttura di Google, sia che abbia bisogno di costruire un'applicazione web multi-livello da zero o ospitare un sito web statico. Veda [Come ospitare il suo sito web su Google App Engine?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_do_you_host_your_website_on_Google_App_Engine) per maggiori informazioni.

Queste opzioni sono generalmente gratuite, con una serie di funzionalità limitata.

### Utilizzando un IDE basato sul web come CodePen

Ci sono diverse app web che emulano un ambiente di sviluppo web, consentendo di scrivere HTML, CSS e JavaScript, che vengono poi visualizzati e mostrati in una finestra di output. In generale, questi strumenti sono facili da usare, ottimi per l'apprendimento, buoni per condividere il codice (ad esempio, se vuole condividere una tecnica o chiedere aiuto per il debugging a colleghi in un ufficio diverso) e gratuiti (per le funzionalità di base). Ospitano la sua pagina renderizzata a un indirizzo web unico. Tuttavia, le funzionalità sono limitate, e queste app spesso non forniscono spazio di hosting per i materiali (come le immagini).

Provi a sperimentare con alcuni di questi esempi per scoprire quale funziona meglio per lei:

- [Scrimba](https://scrimba.com/new?via=mdn) <sup>[_Partner didattico di MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
- [JSFiddle](https://jsfiddle.net/)
- [Glitch](https://glitch.com/)
- [JS Bin](https://jsbin.com/)
- [CodePen](https://codepen.io/)

## Pubblicazione tramite GitHub

Ora vediamo come pubblicare il suo sito tramite GitHub Pages.

1. Per prima cosa, [si iscriva a GitHub](https://github.com/) e verifichi il suo indirizzo email.
2. Successivamente, deve [creare un repository](https://github.com/new) per memorizzare i file. In questa pagina:
   1. nella casella _Repository name_, inserisca _username_.github.io, dove _username_ è il suo nome utente. Ad esempio, il nostro amico Bob Smith inserirebbe _bobsmith.github.io_.
   2. Clicchi sul pulsante _Create repository_ in fondo alla pagina.
3. Nella pagina successiva, trovi il link _uploading an existing file_ e clicchi su di esso. Questo dovrebbe portarla alla pagina di caricamento dei file.
4. A questo punto, dovrebbe essere in grado di trascinare e rilasciare file dal suo sistema di file locale sulla pagina web per caricarli nel repository GitHub. Per farlo:
   1. Apra una finestra di esplora file/finder sul suo computer.
   2. Assicuri che possa vedere la finestra esplora file _e_ la finestra del browser web — le posizioni una accanto all'altra sullo schermo.
   3. Navighi la finestra di esplora file verso la cartella contenente il suo sito web di esempio.
      > [!NOTE]
      > Assicuri che la sua cartella abbia un file `index.html`.
   4. Selezioni tutti i file del suo sito web di esempio (ad esempio utilizzando la scorciatoia da tastiera <kbd>Ctrl</kbd> + <kbd>A</kbd>, o <kbd>Cmd</kbd> + <kbd>A</kbd> su macOS).
   5. Trascini i file dal suo esplora file sulla sezione "Drag files here to add them to your repository" della pagina GitHub.
   6. Il bordo e il testo della sezione cambieranno per indicare che è possibile un rilascio. Rilasci i file a questo punto.
   7. Clicchi sul pulsante _Commit changes_ in fondo alla pagina.
5. Navighi il suo browser su _username_.github.io per vedere il suo sito web online. Ad esempio, per il nome utente _chrisdavidmills_, vada a [_chrisdavidmills_.github.io](https://chrisdavidmills.github.io/).

   > [!NOTE]
   > Potrebbe volerci qualche minuto prima che il suo sito web sia attivo. Se il suo sito web non viene visualizzato immediatamente, aspetti qualche minuto e provi ancora.

Per ulteriori informazioni, consulti [GitHub Pages Help](https://docs.github.com/en/pages/getting-started-with-github-pages).

## Ulteriori letture

- [Che cos'è un server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server)
- [Comprendere i nomi di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name)
- [Quanto costa fare qualcosa sul web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)
- [Deploy a Website](https://www.codecademy.com/learn/deploy-a-website): Un bel tutorial di Codecademy che va un po' oltre e mostra alcune tecniche aggiuntive.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Web_standards", "Learn_web_development/Getting_started/Your_first_website")}}
