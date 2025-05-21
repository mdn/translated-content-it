---
title: Pubblicare il tuo sito web
short-title: Publishing
slug: Learn_web_development/Getting_started/Your_first_website/Publishing_your_website
l10n:
  sourceCommit: 427efbee9e0da53517f45420af87a66a2a6b6e19
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Web_standards", "Learn_web_development/Getting_started/Your_first_website")}}

Una volta terminato di scrivere il codice e organizzare i file che compongono il tuo sito web, devi metterlo online affinché le persone possano trovarlo. Questo articolo spiega come mettere online il tuo sito web di esempio con poco sforzo.

> [!NOTE]
> Avrai bisogno di un sito web di esempio disponibile sul tuo computer locale per seguire questo articolo. Dovrebbe contenere almeno un file `index.html` valido. Se non l'hai fatto già, ti consigliamo di crearne uno lavorando attraverso gli articoli precedenti in questo modulo, a partire da [Come sarà il tuo sito web?](/it/docs/Learn_web_development/Getting_started/Your_first_website/What_will_your_website_look_like).

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base del sistema operativo del tuo computer, del software di base che utilizzerai per costruire un sito web e dei sistemi di file.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati d'apprendimento:</th>
      <td>
        <ul>
          <li>Gli strumenti e i concetti di base coinvolti nella pubblicazione di un sito web — hosting, domini, programmi FTP.</li>
          <li>Quali opzioni di hosting alternative sono disponibili, ad esempio Google App Engine, GitHub e CodePen.</li>
          <li>Pubblicare un sito web utilizzando GitHub Pages.</li>
          <li>Hosting, come acquistarlo e come mettere un sito web online.</li>
          <li>Come registrare un dominio.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Quali sono le opzioni?

Pubblicare un sito web è un argomento complesso perché ci sono molti modi per farlo. Questo articolo non tenta di documentare tutti i metodi possibili. Invece, spiega i vantaggi e gli svantaggi di tre approcci pratici per i principianti. Poi illustra un metodo che può funzionare subito per molti lettori.

### Ottenere un hosting e un nome di dominio

Per avere più controllo sul contenuto e sull'aspetto del sito web, la maggior parte dei professionisti/aziende sceglie di acquistare un web hosting e un nome di dominio:

- L'hosting web è uno spazio file affittato su un [server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server) di una società di hosting. Metti i file del sito web sul server web. Il server web fornisce il contenuto del sito web ai visitatori del sito.
- Un [nome di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name) è l'indirizzo web unico dove le persone trovano il tuo sito, come `https://www.mozilla.org` o `https://www.bbc.co.uk`. Puoi affittare il tuo nome di dominio per il numero di anni che desideri da un **registrant di domini**.

Se ottieni il tuo web hosting _e_ il nome di dominio dalla stessa azienda, tendono ad essere configurati automaticamente per comunicare tra loro. Tuttavia, se li ottieni da aziende separate o vuoi cambiare il tuo hosting con un'altra azienda, devi fare un po' di configurazione per puntare il nome di dominio al server corretto. Questo affinché le persone vedano il tuo sito web quando navigano a quell'indirizzo web. Questo di solito si fa accedendo al sito web del tuo registrar di domini e impostando i [nameserver](https://kinsta.com/knowledgebase/what-is-a-nameserver/) del tuo dominio su quelli forniti dalla tua società di hosting.

Le aziende utilizzano vari meccanismi per trasferire file ai loro server web. Molte avranno più di un'opzione; le opzioni tipiche includono:

- Un'interfaccia drag and drop (vedrai un esempio di questo in [Pubblicazione tramite GitHub](#pubblicazione_tramite_github), più avanti).
- Un programma {{Glossary("FTP", "File Transfer Protocol (FTP)")}}. I programmi FTP variano ampiamente, ma in generale, devi connetterti al tuo server web utilizzando i dettagli forniti dalla tua società di hosting (tipicamente nome utente, password, hostname). Quindi il programma ti mostra i tuoi file locali e i file del server web in due finestre e ti fornisce un modo per trasferire i file avanti e indietro.
- Mantenere il codice sorgente del sito web in un repo GitHub (vedi sotto) e concedere l'accesso alla società di hosting affinché possano recuperare il codice sorgente, costruirlo se necessario e pubblicarlo.
- Alcune aziende forniranno [strumenti da riga di comando](/it/docs/Learn_web_development/Getting_started/Environment_setup/Command_line) per trasferire i tuoi file.

#### Suggerimenti per trovare hosting e domini

- MDN non promuove specifiche società di hosting commerciali o registrant di nomi di dominio. Per trovare società di hosting e registrant, basta cercare "web hosting" e "nomi di dominio". Tutti i registrant avranno una funzionalità per permetterti di controllare se il nome di dominio che desideri è disponibile.
- Il tuo {{Glossary("ISP", "fornitore di servizi internet")}} di casa o dell'ufficio potrebbe fornire un hosting limitato per un piccolo sito web. La serie di funzionalità disponibili sarà limitata, ma potrebbe essere perfetta per i tuoi primi esperimenti.
- Ci sono anche servizi gratuiti disponibili come [Neocities](https://neocities.org/), [Google Sites](https://sites.google.com/) e [WordPress](https://wordpress.com/). Tali servizi possono essere limitati nell'ambito, ma sono sufficienti per esperimenti iniziali.

### Utilizzare uno strumento online come GitHub o Google App Engine

Alcuni strumenti ti permettono di pubblicare il tuo sito web online:

- [GitHub](https://github.com/) è un sito di "social coding". Ti permette di caricare repository di codice per la memorizzazione nel sistema di controllo versione [Git](https://git-scm.com/). Puoi quindi collaborare a progetti di codice e il sistema è open-source per impostazione predefinita, il che significa che chiunque nel mondo può trovare il tuo codice GitHub, usarlo, imparare da esso e migliorarlo. GitHub ha una funzionalità molto utile chiamata [GitHub Pages](https://pages.github.com/), che ti permette di esporre il codice del sito web live sul web.
- [Google App Engine](https://cloud.google.com/appengine) è una piattaforma potente che ti permette di costruire ed eseguire applicazioni sull'infrastruttura di Google — sia che tu abbia bisogno di costruire un'applicazione web multi-livello da zero sia di ospitare un sito web statico. Vedi [Come ospitare il tuo sito web su Google App Engine?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_do_you_host_your_website_on_Google_App_Engine) per maggiori informazioni.

Queste opzioni sono generalmente gratuite, con una serie limitata di funzionalità.

### Utilizzare un IDE basato sul web come CodePen

Ci sono un certo numero di app web che emulano un ambiente di sviluppo di siti web, permettendoti di scrivere HTML, CSS e JavaScript, che viene poi reso e visualizzato in un pannello di output. In generale, questi strumenti sono facili da usare, ottimi per l'apprendimento, buoni per condividere codice (ad esempio, se vuoi condividere una tecnica con o chiedere aiuto per il debugging a colleghi in un ufficio diverso) e gratuiti (per caratteristiche di base). Ospitano la tua pagina resa a un indirizzo web univoco. Tuttavia, le funzionalità sono limitate e queste app spesso non forniscono spazio di hosting per risorse (come immagini).

Prova a giocare con alcuni di questi esempi per scoprire quale funziona meglio per te:

- [Scrimba](https://scrimba.com/new?via=mdn) <sup>[_Partner di apprendimento MDN_](/it/docs/MDN/Writing_guidelines/Learning_content#partner_links_and_embeds)</sup>
- [JSFiddle](https://jsfiddle.net/)
- [Glitch](https://glitch.com/)
- [JS Bin](https://jsbin.com/)
- [CodePen](https://codepen.io/)

## Pubblicazione tramite GitHub

Ora esaminiamo come pubblicare il tuo sito tramite GitHub Pages.

1. Prima di tutto, [iscriviti a GitHub](https://github.com/) e verifica il tuo indirizzo email.
2. Successivamente, devi [creare un repository](https://github.com/new) per memorizzare i file. In questa pagina:
   1. nella casella _Nome del repository_, inserisci _username_.github.io, dove _username_ è il tuo nome utente. Ad esempio, il nostro amico Bob Smith inserirebbe _bobsmith.github.io_.
   2. Clicca sul pulsante _Crea repository_ in fondo alla pagina.
3. Nella pagina successiva, trova il link _caricamento di un file esistente_ e cliccaci sopra. Questo dovrebbe portarti alla pagina di caricamento dei file.
4. A questo punto, dovresti essere in grado di trascinare e rilasciare i file dal tuo sistema di file locale sulla pagina web per caricarli nel repository GitHub. Per farlo:
   1. Apri una finestra del file explorer/finder sul tuo computer.
   2. Assicurati di poter vedere la finestra del file explorer _e_ del browser web — posizionale una accanto all'altra sul tuo schermo.
   3. Naviga con la finestra del file explorer nella cartella che contiene il tuo sito web di esempio.
      > [!NOTE]
      > Assicurati che la tua cartella abbia un file `index.html`.
   4. Seleziona tutti i file del tuo sito web di esempio (ad esempio utilizzando la scorciatoia da tastiera <kbd>Ctrl</kbd> + <kbd>A</kbd>, o <kbd>Cmd</kbd> + <kbd>A</kbd> su macOS).
   5. Trascina i file dal tuo file explorer sopra la sezione "Trascina i file qui per aggiungerli al tuo repository" della pagina di GitHub.
   6. Il bordo e il testo della sezione cambiano per indicare che un rilascio è possibile. Rilascia i file a questo punto.
   7. Clicca sul pulsante _Conferma modifiche_ in fondo alla pagina.
5. Naviga con il tuo browser su _username_.github.io per vedere il tuo sito web online. Ad esempio, per il nome utente _chrisdavidmills_, vai su [_chrisdavidmills_.github.io](https://chrisdavidmills.github.io/).

   > [!NOTE]
   > Potrebbero essere necessari alcuni minuti per mettere online il tuo sito web. Se il tuo sito web non viene visualizzato immediatamente, aspetta alcuni minuti e riprova.

Per saperne di più, vedi [Guida di GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages).

## Ulteriori letture

- [Cos'è un server web](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server)
- [Comprendere i nomi di dominio](/it/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_domain_name)
- [Quanto costa fare qualcosa sul web?](/it/docs/Learn_web_development/Howto/Tools_and_setup/How_much_does_it_cost)
- [Distribuire un Sito Web](https://www.codecademy.com/learn/deploy-a-website): Un bel tutorial da Codecademy che va un po' oltre e mostra alcune tecniche aggiuntive.

{{PreviousMenuNext("Learn_web_development/Getting_started/Your_first_website/Adding_interactivity", "Learn_web_development/Getting_started/Web_standards", "Learn_web_development/Getting_started/Your_first_website")}}
