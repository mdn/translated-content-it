---
title: Pubblicare il proprio sito web
short-title: Publishing
slug: Learn_web_development/Getting_started/Your_first_website/Publishing_your_website
l10n:
  sourceCommit: 06e6e54baef7032c4e81ca93291fde0a0585de8b
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Web_standards", "Learn_web_development/Getting_started/Your_first_website")}}

Dopo aver finito di scrivere il codice e organizzato i file che compongono il sito web, è necessario pubblicare tutto online affinché le persone possano trovarlo. Questo articolo spiega come pubblicare online il sito web di esempio con poco sforzo.

> [!NOTE]
> Per seguire questo articolo, è necessario avere un sito web di esempio disponibile sul computer locale. Dovrebbe contenere almeno un file `index.html` valido. Se non è già stato fatto, si consiglia di crearne uno seguendo gli articoli precedenti di questo modulo, a partire da [Come sarà il sito web?](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del computer, il software di base utilizzato per creare un sito web e i file system.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Gli strumenti e i concetti di base coinvolti nella pubblicazione di un sito web — hosting, domini, programmi FTP.</li>
          <li>Quali opzioni di hosting alternative sono disponibili, per esempio Google App Engine, GitHub e CodePen.</li>
          <li>Pubblicare un sito web usando GitHub Pages.</li>
          <li>L'hosting, come acquistarlo e come pubblicare online un sito web.</li>
          <li>Come registrare un dominio.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali sono le opzioni?

Pubblicare un sito web è un argomento complesso perché esistono molti modi per farlo. Questo articolo non tenta di documentare tutti i metodi possibili. Spiega invece i vantaggi e gli svantaggi di tre approcci pratici per i principianti. Successivamente, descrive un metodo che può funzionare subito per molti lettori.

### Ottenere hosting e un nome di dominio

Per avere maggiore controllo sui contenuti e sull'aspetto del sito web, la maggior parte dei professionisti e delle aziende sceglie di acquistare web hosting e un nome di dominio:

- Il web hosting è spazio file affittato su un [web server](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) di una società di hosting. I file del sito web vengono collocati sul web server, che fornisce i contenuti del sito ai visitatori.
- Un [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) è l'indirizzo web univoco presso cui le persone trovano il sito web, come `https://www.mozilla.org` o `https://www.bbc.co.uk`. È possibile affittare un nome di dominio per tutti gli anni desiderati da un **registrar di domini**.

Se si ottengono il web hosting _e_ il nome di dominio dalla stessa azienda, generalmente vengono configurati automaticamente per comunicare tra loro. Tuttavia, se vengono ottenuti da aziende separate oppure se si desidera cambiare hosting passando a un'altra azienda, è necessario eseguire una breve configurazione per indirizzare il nome di dominio al server corretto. In questo modo, le persone vedranno il sito web quando raggiungono quell'indirizzo web. Generalmente questa operazione viene effettuata accedendo al sito web del registrar di domini e impostando i [nameserver](https://kinsta.com/blog/what-is-a-nameserver/) del dominio su quelli forniti dalla società di hosting.

Le aziende utilizzano vari meccanismi per trasferire file ai propri web server. Molte offrono più di un'opzione; le opzioni tipiche includono:

- Un'interfaccia drag and drop (un esempio sarà mostrato più avanti in [Pubblicare tramite GitHub](#pubblicare_tramite_github)).
- Un programma {{Glossary("FTP", "File Transfer Protocol (FTP)")}}. I programmi FTP variano molto, ma in generale è necessario connettersi al web server usando i dettagli forniti dalla società di hosting, solitamente nome utente, password e hostname. Il programma mostra quindi i file locali e quelli del web server in due finestre e fornisce un modo per trasferire file in entrambe le direzioni.
- Mantenere il codice sorgente del sito web in un repository GitHub (vedere sotto) e concedere alla società di hosting l'accesso affinché possa recuperare il sorgente, compilarlo se necessario e pubblicarlo.
- Alcune aziende forniscono [strumenti da riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) da usare per trasferire i file.

#### Suggerimenti per trovare hosting e domini

- MDN non promuove specifiche aziende commerciali di hosting o registrar di nomi di dominio. Per trovare aziende di hosting e registrar, è sufficiente cercare "web hosting" e "domain names". Tutti i registrar dispongono di una funzionalità che permette di verificare se il nome di dominio desiderato è disponibile.
- Il {{Glossary("ISP", "provider di servizi Internet")}} di casa o dell'ufficio potrebbe fornire un hosting limitato per un piccolo sito web. L'insieme delle funzionalità disponibili sarà limitato, ma potrebbe essere perfetto per i primi esperimenti.
- Sono disponibili anche servizi gratuiti come [Neocities](https://neocities.org/), [Google Sites](https://sites.google.com/) e [WordPress](https://wordpress.com/). Tali servizi possono avere un ambito limitato, ma sono sufficienti per gli esperimenti iniziali.

### Usare uno strumento online

Alcuni strumenti consentono di pubblicare online il proprio sito web:

- [GitHub](https://github.com/) è un sito di "social coding". Consente di caricare repository di codice per archiviarli nel sistema di **controllo di versione** [Git](https://git-scm.com/). È quindi possibile collaborare a progetti di codice e il sistema è open source per impostazione predefinita, il che significa che chiunque nel mondo può trovare il codice su GitHub, usarlo, imparare da esso e migliorarlo. GitHub dispone di una funzionalità molto utile chiamata [GitHub Pages](https://pages.github.com/), che consente di rendere disponibile sul web il codice di un sito web.
- [Netlify](https://www.netlify.com/) è una piattaforma di web hosting che fornisce hosting per siti web statici direttamente dal repository GitHub. Fornisce inoltre diverse funzionalità aggiuntive, come anteprime di deployment, funzioni serverless e gestione dei moduli.
- [Fly.io](https://fly.io/) è una piattaforma che consente di distribuire applicazioni e database vicino agli utenti. È più adatta per applicazioni web che richiedono servizi backend.

Queste opzioni sono generalmente gratuite, con un insieme limitato di funzionalità.

### Usare un IDE basato sul web come CodePen

Esistono numerose app web che emulano un ambiente di sviluppo per siti web, consentendo di scrivere HTML, CSS e JavaScript, che vengono poi renderizzati e visualizzati in un pannello di output. In generale, questi strumenti sono facili da usare, ottimi per imparare, utili per condividere codice (per esempio, per condividere una tecnica con colleghi in un altro ufficio o chiedere loro aiuto per il debugging) e gratuiti per le funzionalità di base. Ospitano la pagina renderizzata a un indirizzo web univoco. Tuttavia, le funzionalità sono limitate e queste app spesso non forniscono spazio di hosting per asset come le immagini.

Provare alcuni di questi esempi per scoprire quale funziona meglio:

- [Scrimba](https://scrimba.com/new?via=mdn) <sup>[_partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
- [JSFiddle](https://jsfiddle.net/)
- [JSBin](https://jsbin.com/)
- [CodePen](https://codepen.io/)

## Pubblicare tramite GitHub

Vediamo ora come pubblicare il sito tramite GitHub Pages.

1. Innanzitutto, [registrarsi su GitHub](https://github.com/) e verificare il proprio indirizzo email.
2. Successivamente, è necessario [creare un repository](https://github.com/new) per archiviare i file. In questa pagina:
   1. nella casella _Repository name_, inserire _username_.github.io, dove _username_ è il proprio nome utente. Per esempio, il nostro amico Bob Smith inserirebbe _bobsmith.github.io_.
   2. Fare clic sul pulsante _Create repository_ in fondo alla pagina.
3. Nella pagina successiva, trovare il collegamento _uploading an existing file_ e farvi clic. Dovrebbe aprirsi la pagina di caricamento dei file.
4. A questo punto, dovrebbe essere possibile trascinare e rilasciare file dal file system locale sulla pagina web per caricarli nel repository GitHub. Per farlo:
   1. Aprire una finestra dell'esplora file/finder sul computer.
   2. Assicurarsi di poter vedere le finestre dell'esplora file _e_ del browser web, posizionandole una accanto all'altra sullo schermo.
   3. Nella finestra dell'esplora file, passare alla cartella contenente il sito web di esempio.
      > [!NOTE]
      > Assicurarsi che la cartella contenga un file `index.html`.
   4. Selezionare tutti i file del sito web di esempio, per esempio usando la scorciatoia da tastiera <kbd>Ctrl</kbd> + <kbd>A</kbd>, oppure <kbd>Cmd</kbd> + <kbd>A</kbd> su macOS.
   5. Trascinare i file dall'esplora file sulla sezione "Drag files here to add them to your repository" della pagina GitHub.
   6. Il bordo e il testo della sezione cambiano per indicare che è possibile rilasciare i file. Rilasciare i file a questo punto.
   7. Fare clic sul pulsante _Commit changes_ in fondo alla pagina.
5. Nel browser, raggiungere _username_.github.io per vedere il sito web online. Per esempio, per il nome utente _chrisdavidmills_, andare a [_chrisdavidmills_.github.io](https://chrisdavidmills.github.io/).

   > [!NOTE]
   > Potrebbero essere necessari alcuni minuti prima che il sito web venga pubblicato. Se il sito web non viene visualizzato immediatamente, attendere alcuni minuti e riprovare.

Per ulteriori informazioni, vedere [Guida di GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages).

## Ulteriori letture

- [Che cos'è un web server](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server)
- [Comprendere i nomi di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name)
- [Quanto costa fare qualcosa sul web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)
- [Deploy a Website](https://www.codecademy.com/learn/deploy-a-website): un buon tutorial di Codecademy che approfondisce un po' l'argomento e mostra alcune tecniche aggiuntive.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Web_standards", "Learn_web_development/Getting_started/Your_first_website")}}
