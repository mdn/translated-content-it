---
title: Il modello di standard web
slug: Learn_web_development/Getting_started/Web_standards/The_web_standards_model
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/How_the_web_works", "Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites", "Learn_web_development/Getting_started/Web_standards")}}

Questo articolo fornisce alcune informazioni di base utili sul web e sugli standard web — come sono nati, cosa sono le tecnologie standard web e come funzionano insieme.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Familiarità di base con il sistema operativo del proprio computer, browser web e tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Standard web e i principi chiave su cui sono costruiti.</li>
          <li>Come funzionano le organizzazioni di standard — per esempio il <a href="https://www.w3.org/">W3C</a>, <a href="https://whatwg.org/">WHATWG</a>, <a href="https://tc39.es/">TC39</a>, e <a href="https://www.khronos.org/">Khronos Group</a>; il processo di creazione degli standard.</li>
          <li>Le principali tecnologie degli standard web e come funzionano insieme.</li>
          <li>File lato server (dinamico) vs file lato client (statico).</li>
          <li>Migliori pratiche web.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Breve storia del web

Alla fine degli anni '60, l'esercito degli Stati Uniti sviluppò una rete di comunicazione chiamata {{Glossary("Arpanet", "ARPANET")}}. Questo può essere considerato un precursore di **internet**, poiché funzionava sul [commutazione di pacchetto](https://en.wikipedia.org/wiki/Packet_switching), e presentava la prima implementazione della suite di protocolli [TCP/IP](https://en.wikipedia.org/wiki/Internet_protocol_suite). Queste due tecnologie costituiscono la base dell'infrastruttura su cui è costruito internet.

Nel 1980, [Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee) (spesso conosciuto come TimBL) scrisse un programma di taccuino chiamato ENQUIRE, che presentava il concetto di collegamenti tra diversi nodi. Suona familiare?

Avanti veloce fino al 1989, TimBL scrisse [Information Management: A Proposal](https://www.w3.org/History/1989/proposal.html) e HyperText al CERN; queste due pubblicazioni insieme fornirono il contesto di come il web avrebbe funzionato. Ricevettero un buon interesse, abbastanza per convincere i capi di TimBL a permettergli di procedere e creare un sistema di ipertesto globale.

Nel 1990-91, TimBL aveva creato tutto il necessario per eseguire la prima versione del **World Wide Web** (generalmente chiamato **web**) — [HTTP](/it/docs/Web/HTTP), [HTML](/it/docs/Web/HTML), il primo browser web, che fu chiamato [WorldWideWeb](https://en.wikipedia.org/wiki/WorldWideWeb), un server web, e alcune pagine web da visualizzare.

> [!NOTE]
> A volte le persone usano "il web" e "internet" in modo intercambiabile, ma sono cose diverse. Internet è l'infrastruttura che permette alle informazioni di essere trasportate in tutto il mondo tra diversi server e client, mentre il web è un sistema costruito su internet. Il web definisce tipi di informazione (contenuto e codice) che sono trasportati tramite internet e protocolli di comunicazione per gestire quel trasporto.

Nel 1994, TimBL fondò il [World Wide Web Consortium](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium) (W3C), un'organizzazione che riunisce rappresentanti di molte aziende diverse per lavorare insieme sulla creazione di tecnologie web. Il W3C ha lavorato alla standardizzazione e al miglioramento delle tecnologie web esistenti come HTML e HTTP, e alla creazione di nuove tecnologie come [CSS](/it/docs/Web/CSS) e [JavaScript](/it/docs/Web/JavaScript). CSS e JavaScript in particolare erano vitali per dare al web stile e interattività, facendolo apparire più simile al web che conosciamo oggi.

Negli anni successivi, il web esplose, con il rilascio di più browser, l'istituzione di migliaia di server web, e la creazione di milioni di pagine web. Apparvero anche altre organizzazioni di standard per aiutare a standardizzare diversi aspetti delle tecnologie web.

> [!NOTE]
> Se sei interessato a leggere un resoconto più dettagliato della storia del web, prova a cercare "storia del web" nel tuo [motore di ricerca](/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web#search_engine) preferito e vedi cosa riesci a trovare.

## Standard web

Gli **standard web** sono le tecnologie che utilizziamo per costruire siti web. Questi standard esistono come documenti tecnici lunghi chiamati specifiche, che dettagliano esattamente come dovrebbe funzionare la tecnologia. Questi documenti non sono molto utili per imparare a usare le tecnologie che descrivono (per questo motivo abbiamo siti come MDN Web Docs). Invece, sono destinati ad essere utilizzati da ingegneri del software per implementare queste tecnologie (di solito nei browser web).

### Organizzazioni di standard e processi

Gli standard web sono creati da organizzazioni di standard — istituzioni che invitano gruppi di persone di diverse aziende tecnologiche a unirsi e concordare su come le tecnologie dovrebbero funzionare nel modo migliore per soddisfare tutti i loro casi d'uso.

Il W3C è l'organizzazione di standard web più conosciuta, ma ce ne sono altre. Per esempio:

- [WHATWG](https://whatwg.org/) mantiene l'[HTML Living Standard](https://html.spec.whatwg.org/multipage/), che descrive esattamente come HTML (tutti gli elementi HTML e le loro API associate, e altre tecnologie circostanti) dovrebbe essere implementato.
- [TC39](https://tc39.es/) e [ECMA](https://ecma-international.org/) specificano e pubblicano lo standard per ECMAScript, su cui si basa modern JavaScript.
- [Khronos](https://www.khronos.org/) pubblica tecnologie per la grafica 3D, come WebGL.

I processi completi attraverso cui vengono creati gli standard possono diventare profondi e complessi. Tuttavia, a meno che non si voglia creare le proprie funzionalità tecnologiche web, non è necessario comprendere la maggior parte di essi. Se desideri contribuire alla discussione sulle nuove tecnologie e fornire feedback, di solito si tratta di unirsi alla mailing list pertinente o ad altro meccanismo di discussione. Le discussioni sugli standard sono condotte in pubblico, da cui il termine ["Open" standards](#open_standards).

Per ora, ti forniremo una comprensione generale e ad alto livello di come funzionano i processi di standard:

1. Qualcuno nota la necessità di una nuova funzionalità standard web che renderà la vita degli sviluppatori più facile. Per esempio, potrebbe esserci un modello comune che è comunemente utilizzato nelle interfacce utente web, ma è difficile da implementare. Una funzione dedicata CSS renderebbe molto più facile. Il qualcuno potrebbe essere chiunque — un singolo sviluppatore, o un ingegnere che lavora per una grande azienda tecnologica.
2. La persona discute questa caratteristica con altri sviluppatori, ingegneri dei browser, ecc., e inizia a creare interesse per l'implementazione della funzione. Di solito scrivono un documento che spiega la necessità della funzionalità e come funzionerà, e una demo di codice che mostra come apparirebbe la funzione in azione.
3. Se c'è abbastanza interesse nella caratteristica, viene discussa formalmente all'interno del gruppo di lavoro dell'organizzazione di standard pertinente. Per esempio, le funzionalità CSS sono di solito discusse dal [CSS Working Group](https://www.w3.org/groups/wg/css/) (WG) (vedi anche la [pagina Wikipedia del CSS Working Group](https://en.wikipedia.org/wiki/CSS_Working_Group) per una descrizione e una storia un po' più dettagliata). Prima che una nuova tecnologia web venga accettata, deve essere valutata in modo rigoroso per assicurarsi che sia buona per il web — per esempio, non introduce problemi di sicurezza, è [accessibile e compatibile](#accessibile_e_interoperabile) con altre tecnologie web, e non si basa su brevetti.
4. Per dimostrare la validità della funzione, succedono diverse cose. Questi punti possono verificarsi intorno allo stesso tempo del Punto 3., o anche prima (i fornitori di browser a volte implementano funzionalità proprietarie/non standard e poi tentano di standardizzarle in seguito):

   1. Uno o più fornitori di browser implementeranno una versione sperimentale della nuova funzionalità, spesso disabilitata di default, ma che può essere abilitata da chi vuole testare e fornire feedback.
   2. Un membro del gruppo di lavoro la aggiungerà anche a una specifica tecnologica in modo che i fornitori di browser possano implementarla in modo coerente.
   3. Cercheranno anche di ottenere feedback da altri fornitori di browser per vedere quali problemi hanno con la proposta, e quanto probabilmente la implementeranno. Queste sono chiamate posizioni sugli standard. Vedi per esempio [Mozilla Standards Positions](https://mozilla.github.io/standards-positions/).
   4. Le persone coinvolte scriveranno anche un'ampia suite di test per dimostrare che la funzionalità funziona come descritto.

5. Infine, se tutto va bene, la funzione verrà implementata su tutti i browser e può iniziare a essere utilizzata nella creazione dei siti web.

> [!NOTE]
> È perfettamente possibile che le persone che suggeriscono la funzione, la implementano in un browser, creano la specifica, scrivono test e raccolgono feedback su di essa siano le stesse persona/e.

È possibile trovare ulteriori informazioni sui processi specifici delle organizzazioni di standard. Vedi per esempio:

- [W3C Process Document](https://www.w3.org/policies/process/)
- [WHATWG — Working Mode](https://whatwg.org/working-mode)
- [The TC39 Process](https://tc39.es/process-document/)

## Principi chiave degli standard web

I principi chiave del web, che rendono il web un'industria unica ed emozionante in cui essere coinvolti, sono i seguenti:

- Aperto per contribuire e utilizzare, e quindi non gravato da brevetti o controllato da una singola entità privata.
- Accessibile e interoperabile.
- Non spezzano il web.

Esaminiamo ciascuno di questi principi in dettaglio.

### Standard "Open"

Uno degli aspetti chiave degli standard web, su cui TimBL e il W3C hanno concordato fin dall'inizio, è che il web (e le tecnologie web) dovrebbero essere **open**. Questo significa che sono liberi sia da contribuire che da usare, e non gravati da brevetti/licenze. Questo è importante: se una tecnologia web si basa su tecnologie brevettate/licenziate per funzionare, il titolare del brevetto può quindi addebitare ai fornitori di browser una grande quantità di denaro, e quei costi sarebbero poi passati agli utenti del browser.

Inoltre, poiché le tecnologie web sono create apertamente, in collaborazione tra molte aziende diverse, significa che nessuna azienda può controllarle, il che è davvero una buona cosa. Non vorresti che un'azienda decidesse improvvisamente di mettere tutto il web dietro un muro di pagamento, rilasciasse una nuova versione di HTML che tutti devono acquistare per continuare a creare siti web, o peggio ancora, decidesse che non sono più interessati e semplicemente lo spegnesse.

Gli standard open permettono al web di rimanere una risorsa pubblica liberamente disponibile, dove chiunque può scrivere il codice per costruire un sito web gratuitamente e chiunque può contribuire al processo di creazione degli standard.

### Accessibile e interoperabile

Il web e i browser web sono fondamentalmente progettati affinché il contenuto web sia **accessibile** alle persone con disabilità. Era originariamente concepito come un grande livellatore, consentendo alle persone di accedere alle informazioni indipendentemente dalle circostanze. Questo significa che, per esempio:

- Le persone che non sono in grado di utilizzare un mouse o un dispositivo di puntamento possono utilizzare la tastiera per navigare nel web.
- Le persone con disabilità visive possono ingrandire il contenuto o utilizzare un programma chiamato **lettore di schermo** per leggere ad alta voce il contenuto e descrivere i controlli in un modo che abbia senso.

> [!NOTE]
> Imparerai di più sull'[Accessibilità](/it/docs/Learn_web_development/Core/Accessibility) più avanti nel percorso di apprendimento.

Inoltre, le tecnologie web sono intese per essere **interoperabili**. Poiché le tecnologie web sono implementate secondo standard pubblicati, i browser dovrebbero fornire lo stesso output reso per un dato input (per esempio, codice HTML, CSS o JS) — in altre parole, un sito web dovrebbe funzionare in modo coerente su più browser.

### Non spezzare il web

Un'altra frase che sentirai intorno agli standard web open è "non spezzare il web". L'idea alla base di questo è che ogni nuova tecnologia web dovrebbe essere retrocompatibile con ciò che è venuto prima, in modo che i siti web esistenti continuino a funzionare allo stesso modo di prima.

I fornitori di browser web dovrebbero essere in grado di implementare nuove tecnologie web senza causare una differenza nel rendering o nella funzionalità che indurrebbe i loro utenti a pensare che un sito web sia rotto e cercare un altro browser come risultato.

## Panoramica delle tecnologie web moderne

Ci sono numerose tecnologie da apprendere se si desidera essere uno sviluppatore web front-end. In questa sezione le descriveremo brevemente.

### HTML, CSS, e JavaScript

[HTML](/it/docs/Web/HTML), [CSS](/it/docs/Web/CSS), e [JavaScript](/it/docs/Web/JavaScript) sono le tre tecnologie principali che utilizzerai per costruire un sito web.

- HTML è per la struttura e la semantica (significato).
- CSS è per lo stile e il layout.
- JavaScript e le API sono per controllare il comportamento dinamico.

#### HTML

HyperText Markup Language, o **HTML**, è un linguaggio di markup composto da diversi elementi che puoi avvolgere (marcare) il contenuto per dargli significato (semantica) e struttura. Un semplice HTML appare così:

```html
<h1>This is a top-level heading</h1>

<p>This is a paragraph of text.</p>

<img src="cat.jpg" alt="A picture of my cat" />
```

Se adotteremo un'analogia di costruzione di una casa, HTML sarebbe come le fondamenta e le pareti della casa, che le danno struttura e la tengono insieme.

#### CSS

Cascading Style Sheets (**CSS**) è un linguaggio basato su regole utilizzato per applicare stili al tuo HTML — per esempio, impostare i colori del testo e dello sfondo, aggiungere bordi, animare gli oggetti o impaginare una pagina in un certo modo. Come esempio semplice, il seguente codice renderebbe tutti i paragrafi HTML rossi:

```css
p {
  color: red;
}
```

Nell'analogia della casa, CSS è come la vernice, la carta da parati, i tappeti e i dipinti che useresti per far sembrare la casa bella.

#### JavaScript (e API)

**JavaScript** è il linguaggio di programmazione che usiamo per aggiungere interattività ai siti web, dal cambio dinamico dello stile, all'ottenimento di aggiornamenti dal server, fino alla grafica 3D complessa. Il semplice JavaScript seguente memorizza un riferimento a un paragrafo nella memoria e cambia il testo al suo interno:

```js
let pElem = document.querySelector("p");
pElem.textContent = "We changed the text!";
```

Sentirai anche il termine **API** insieme a JavaScript. API sta per **Application Programming Interface**. In termini generali, un'API è un po' di codice che ti consente di controllare altri pezzi di codice più complessi o altre funzionalità sul tuo computer (come dispositivi hardware come la webcam o il microfono) in modo gestibile.

Per esempio, scrivere la tua interfaccia per comunicare con la tua webcam e catturare un flusso video da essa sarebbe piuttosto difficile, ma il metodo API JavaScript [`getUserMedia()`](/it/docs/Web/API/MediaDevices/getUserMedia#examples) ti permette di farlo abbastanza facilmente. Fa tutto il duro lavoro per te, dietro le quinte, in modo che non sia necessario reinventare la ruota ogni volta.

Il semplice frammento di codice sopra utilizza anche un'API. [`querySelector()`](/it/docs/Web/API/Document/querySelector) e [`textContent`](/it/docs/Web/API/Node/textContent) fanno entrambi parte della famiglia di API del [Document Object Model (DOM)](/it/docs/Learn_web_development/Core/Scripting/DOM_scripting), che ti consente di utilizzare JavaScript per manipolare documenti web.

Nell'analogia della casa, JavaScript è come il fornello, la TV, il microonde, o l'asciugacapelli — le cose che danno alla tua casa funzionalità utili.

### Altre tecnologie web

Ci sono altre tecnologie utilizzate sul web, per esempio:

- [HTTP](/it/docs/Web/HTTP) per comunicare tra client e server, come menzionato in precedenza.
- [SVG](/it/docs/Web/SVG) per la creazione e la manipolazione di grafica vettoriale.
- [MathML](/it/docs/Web/MathML) per descrivere formule matematiche.

Tuttavia, HTML, CSS, e JavaScript sono di gran lunga le tecnologie più importanti da apprendere, quindi ci concentreremo principalmente su quelle nel nostro percorso di apprendimento.

## Strumenti

Una volta appreso delle tecnologie standard e fondamentali utilizzate per costruire pagine web (come HTML, CSS, e JavaScript), presto inizierai a incontrare vari strumenti che possono essere utilizzati per rendere il tuo lavoro più facile o più efficiente. Esempi includono:

- [Strumenti di sviluppo](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) all'interno dei browser moderni che possono essere utilizzati per eseguire il debug del tuo codice.
- [Strumenti di test](/it/docs/Learn_web_development/Extensions/Testing) che possono essere utilizzati per eseguire test per mostrare se il tuo codice si comporta come previsto.
- [Frameworks e librerie](/it/docs/Learn_web_development/Core/Frameworks_libraries) costruiti su JavaScript che ti permettono di costruire determinati tipi di sito web in modo molto più rapido ed efficace.
- I cosiddetti **Linters** e **formatter**, che prendono un insieme di regole per lo stile di codifica, esaminano il tuo codice e aggiornano il tuo codice per seguire quelle regole. Prettier, che hai [conosciuto in precedenza nel corso](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors#enhancing_your_code_editor_with_extensions), è un esempio di formatter.

## Linguaggi lato server e framework

HTML, CSS, e JavaScript sono linguaggi lato client (o front-end), il che significa che sono eseguiti dal browser per produrre un'interfaccia di sito web che i tuoi utenti possono utilizzare.

Esiste un'altra classe di linguaggi chiamati linguaggi lato server (o back-end), il che significa che vengono eseguiti sul server prima che il risultato venga inviato al browser per essere visualizzato. Un uso tipico di un linguaggio lato server è prelevare dati da un database, generare un HTML per contenere i dati, quindi inviare l'HTML al browser per visualizzarlo all'utente.

Esempi di framework e linguaggi lato server includono ASP.NET (C#), Django (Python), Laravel (PHP), e Next.js (JavaScript).

Queste tecnologie non sono considerate "standard web" — sono sviluppate da organizzazioni al di fuori dei processi di standard web di organizzazioni come il W3C e il WHATWG — anche se alcune di esse avranno processi simili che sono comunque aperti.

### Statico vs dinamico

Un altro modo in cui i linguaggi lato client e server sono spesso descritti è **statico** e **dinamico**:

- Un file HTML semplice è memorizzato sul server. Quando richiesto, viene consegnato al client, invariato, e reso dal browser. Poiché non cambia, viene definito "statico".
- Quando il codice lato server (per esempio, uno script Python o una pagina ASP.NET) genera un po' di HTML contenente dati e restituisce quell'HTML al client, i contenuti dell'HTML cambiano a seconda di ciò che fa il codice lato server. È quindi definito "dinamico".

C'è spesso un po' di sovrapposizione tra i concetti di codice statico e dinamico. I linguaggi lato server di solito definiscono le strutture HTML all'interno di un file modello, che tendono ad essere principalmente HTML statico con alcune sezioni dinamiche speciali incluse che cambiano a seconda dei dati che devono essere inseriti.

## Migliori pratiche web

Abbiamo parlato brevemente delle tecnologie che utilizzerai per costruire siti web. Ora parliamo delle migliori pratiche che gli sviluppatori web generalmente adottano per assicurarsi che i loro siti web siano utilizzabili dal maggior numero possibile di persone.

Quando si fa sviluppo web, la principale causa di incertezza deriva dal fatto che non si sa quale combinazione di tecnologie ciascun utente utilizzerà per visualizzare il proprio sito web:

- L'utente 1 potrebbe guardarlo su un iPhone, con uno schermo piccolo e stretto.
- L'utente 2 potrebbe guardarlo su un laptop Windows con un monitor widescreen collegato a esso.
- L'utente 3 potrebbe avere disabilità visive e utilizzare un lettore di schermo per leggere e interagire con la pagina web.
- L'utente 4 potrebbe utilizzare una macchina desktop molto vecchia che non può eseguire browser moderni.

Poiché non si sa esattamente cosa utilizzeranno gli utenti, è necessario progettare in modo difensivo — rendere il proprio sito web il più flessibile possibile, in modo che tutti gli utenti sopra descritti possano utilizzarlo, anche se potrebbero non ottenere la stessa esperienza.

Incontrerai i seguenti concetti a un certo punto nei tuoi studi, che rappresentano le migliori pratiche a cui i tuoi siti web dovrebbero idealmente aderire. Non preoccuparti troppo di questi per ora. Durante la maggior parte del corso cerchiamo di insegnare questi implicitamente, il che significa che quando ti insegniamo HTML, CSS e JavaScript, i nostri esempi seguiranno le migliori pratiche laddove possibile. Più avanti nel tuo percorso di apprendimento probabilmente esplorerai insegnamenti espliciti in queste aree.

- **Progressive enhancement**
  - : Creare un'esperienza minima che fornisce la funzionalità essenziale a tutti gli utenti, e stratificare un'esperienza migliore e altri miglioramenti nei browser che possono supportarli. Il progressive enhancement è spesso visto come non importante, perché i browser tendono a supportare nuove funzionalità in modo più coerente in questi giorni, e le persone tendono ad avere connessioni internet più veloci con limiti più alti sull'uso dei dati. Tuttavia, considera esempi come ridurre la decorazione per rendere un'esperienza mobile più fluida e risparmiare sui dati o fornire un'esperienza più leggera e a bassa larghezza di banda per gli utenti che pagano in base ai megabyte o hanno connessioni misurate.
- **Compatibilità cross-browser**
  - : Cercare di garantire che la tua pagina web funzioni su quanti più dispositivi possibile. Questo include l'utilizzo di tecnologie che tutti i browser supportano, fornendo esperienze migliori ai browser che possono gestirle (progressive enhancement), e/o scrivendo codice in modo che ricada a un'esperienza più semplice ma comunque utilizzabile nei browser più vecchi (definito **degrado graduale**). Richiede anche il test per vedere se qualcosa fallisce in alcuni browser e quindi più lavoro per risolvere quei fallimenti.
- **Separare i livelli**
  - : Mettere i tuoi contenuti (HTML), lo stile (CSS), e il comportamento (JavaScript) in file di codice diversi, piuttosto che raggrupparli tutti insieme nello stesso posto. È un'idea intelligente per molti motivi, compresa la gestione e la comprensione del codice e il lavoro di squadra/separazione dei ruoli. In realtà, la separazione non è sempre chiara. È un ideale a cui mirare quando possibile, piuttosto che un assoluto.
- **Responsive web design**
  - : Rendere la tua funzionalità e i layout flessibili in modo che possano adattarsi automaticamente a diversi browser. Un esempio ovvio è un sito web che è impaginato in un modo in un browser widescreen sul desktop, ma visualizzato come un layout più compatto e a colonna singola su browser di telefoni cellulari. Prova ad aggiustare la larghezza della tua finestra del browser ora e guarda cosa succede al layout del sito.
- **Performance**
  - : Far caricare i siti web il più velocemente possibile, ma anche renderli intuitivi e facili da utilizzare in modo che gli utenti non si frustrino e vadano altrove.
- **Internazionalizzazione**
  - : Rendere i siti web utilizzabili da persone di culture diverse, che parlano lingue diverse dalla tua. Ci sono considerazioni tecniche qui (come alterare il tuo layout in modo che funzioni bene anche per lingue scritte da destra a sinistra o dall'alto al basso), e umane (come utilizzare un linguaggio semplice e privo di slang affinché le culture diverse abbiano più probabilità di comprendere il tuo testo).
- **Privacy** e **Sicurezza**
  - : Questi due concetti sono correlati ma diversi. La Privacy si riferisce a permettere alle persone di svolgere la loro attività in modo privato e non spiarle o raccogliere più dei loro dati di quanto tu abbia assolutamente bisogno. La Sicurezza si riferisce a costruire il tuo sito web in modo sicuro in modo che gli utenti malintenzionati non possano rubare informazioni contenute su di esso da te o dai tuoi utenti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/How_the_web_works", "Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites", "Learn_web_development/Getting_started/Web_standards")}}
