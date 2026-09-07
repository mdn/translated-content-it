---
title: Il modello degli standard web
slug: Learn_web_development/Getting_started/Web_standards/The_web_standards_model
l10n:
  sourceCommit: 077d3774b2de6f345b8552fe59ff9deb8b67ebd5
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/How_the_web_works", "Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites", "Learn_web_development/Getting_started/Web_standards")}}

Questo articolo fornisce alcune utili informazioni di contesto sul web e sugli standard web: come sono nati, quali sono le tecnologie degli standard web e come funzionano insieme.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        Conoscenza di base del sistema operativo del proprio computer, dei browser web e delle tecnologie web.
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati di apprendimento:</th>
      <td>
        <ul>
          <li>Gli standard web e i principi chiave su cui si basano.</li>
          <li>Come operano gli organismi di standardizzazione, ad esempio <a href="https://www.w3.org/">W3C</a>, <a href="https://whatwg.org/">WHATWG</a>, <a href="https://tc39.es/">TC39</a> e <a href="https://www.khronos.org/">Khronos Group</a>; il processo di creazione degli standard.</li>
          <li>Le principali tecnologie degli standard web e il modo in cui funzionano insieme.</li>
          <li>File lato server (dinamici) rispetto a file lato client (statici).</li>
          <li>Le migliori pratiche web.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Breve storia del web

Alla fine degli anni Sessanta, l'esercito statunitense sviluppò una rete di comunicazione chiamata {{Glossary("Arpanet", "ARPANET")}}. Può essere considerata un precursore di **internet**, poiché funzionava tramite [commutazione di pacchetto](https://en.wikipedia.org/wiki/Packet_switching) e presentava la prima implementazione della suite di protocolli [TCP/IP](https://en.wikipedia.org/wiki/Internet_protocol_suite). Queste due tecnologie costituiscono la base dell'infrastruttura su cui è costruita internet.

Nel 1980, [Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee) (spesso chiamato TimBL) scrisse un programma per appunti chiamato ENQUIRE, che introduceva il concetto di collegamenti tra nodi diversi. Suona familiare?

Facciamo un salto al 1989: TimBL scrisse [Information Management: A Proposal](https://www.w3.org/History/1989/proposal.html) e HyperText at CERN; queste due pubblicazioni fornirono insieme le basi per il funzionamento del web. Ricevettero un discreto interesse, sufficiente a convincere i superiori di TimBL a consentirgli di procedere con la creazione di un sistema ipertestuale globale.

Tra il 1990 e il 1991, TimBL aveva creato tutto ciò che era necessario per eseguire la prima versione del **World Wide Web** (generalmente chiamato **web**): [HTTP](/it/docs/Web/HTTP), [HTML](/it/docs/Web/HTML), il primo browser web, chiamato [WorldWideWeb](https://en.wikipedia.org/wiki/WorldWideWeb), un server web e alcune pagine web da visualizzare.

> [!NOTE]
> Talvolta si usano in modo intercambiabile i termini "web" e "internet", ma sono cose diverse. Internet è l'infrastruttura che consente il trasporto delle informazioni nel mondo tra diversi server e client, mentre il web è un sistema costruito sopra internet. Il web definisce tipi di informazioni (contenuti e codice) trasportati tramite internet e protocolli di comunicazione per gestire tale trasporto.

Nel 1994, TimBL fondò il [World Wide Web Consortium](https://en.wikipedia.org/wiki/World_Wide_Web_Consortium) (W3C), un'organizzazione che riunisce rappresentanti di molte aziende diverse affinché collaborino alla creazione di tecnologie web. Il W3C lavorò alla standardizzazione e al miglioramento delle tecnologie web esistenti come HTML e HTTP, e alla creazione di nuove tecnologie come [CSS](/it/docs/Web/CSS) e [JavaScript](/it/docs/Web/JavaScript). In particolare, CSS e JavaScript furono fondamentali per dotare il web di stile e interattività, rendendolo più simile al web che conosciamo oggi.

Negli anni successivi, il web esplose: vennero rilasciati più browser, furono configurati migliaia di server web e create milioni di pagine web. Comparvero anche altre organizzazioni di standardizzazione per contribuire a standardizzare diversi aspetti delle tecnologie web.

> [!NOTE]
> Per leggere una storia più dettagliata del web, provare a cercare "history of the web" nel proprio [motore di ricerca](/it/docs/Learn_web_development/Getting_started/Environment_setup/Browsing_the_web#search_engine) preferito e vedere cosa si riesce a trovare.

## Standard web

Gli **standard web** sono le tecnologie utilizzate per creare siti web. Questi standard esistono sotto forma di lunghi documenti tecnici chiamati specifiche, che descrivono esattamente come deve funzionare la tecnologia. Questi documenti non sono molto utili per imparare a usare le tecnologie che descrivono (per questo esistono siti come MDN Web Docs). Sono invece destinati agli ingegneri del software per implementare tali tecnologie, di solito nei browser web.

### Organismi e processi di standardizzazione

Gli standard web sono creati da organismi di standardizzazione: istituzioni che invitano gruppi di persone di diverse aziende tecnologiche a riunirsi e accordarsi su come le tecnologie dovrebbero funzionare, nel modo migliore per soddisfare tutti i loro casi d'uso.

Il W3C è l'organismo di standardizzazione web più noto, ma ne esistono altri. Per esempio:

- [WHATWG](https://whatwg.org/) mantiene l'[HTML Living Standard](https://html.spec.whatwg.org/multipage/), che descrive esattamente come deve essere implementato HTML (tutti gli elementi HTML, le relative API e altre tecnologie correlate).
- [TC39](https://tc39.es/) e [ECMA](https://ecma-international.org/) specificano e pubblicano lo standard per ECMAScript, su cui si basa il moderno JavaScript.
- [Khronos](https://www.khronos.org/) pubblica tecnologie per la grafica 3D, come WebGL.

I processi completi attraverso cui vengono creati gli standard possono essere approfonditi e complessi. Tuttavia, a meno che non si desideri creare funzionalità tecnologiche web personali, non è necessario comprenderne la maggior parte. Per contribuire alla discussione sulle nuove tecnologie e fornire feedback, di solito è sufficiente iscriversi alla mailing list pertinente o a un altro meccanismo di discussione. Le discussioni sugli standard si svolgono pubblicamente, da qui il termine standard ["aperti"](#open_standards).

Per ora, viene fornita una comprensione generale e di alto livello del funzionamento dei processi di standardizzazione:

1. Qualcuno rileva la necessità di una nuova funzionalità di uno standard web che semplifichi la vita degli sviluppatori. Per esempio, potrebbe esistere un pattern comune spesso utilizzato nelle interfacce utente web, ma difficile da implementare. Una funzionalità CSS dedicata lo renderebbe molto più semplice. Questo qualcuno può essere chiunque: uno sviluppatore individuale o un ingegnere che lavora per una grande azienda tecnologica.
2. La persona discute questa funzionalità con altri sviluppatori, ingegneri dei browser e così via, e inizia a suscitare interesse per la sua implementazione. Di solito scrive un documento esplicativo che illustra la necessità della funzionalità e il suo funzionamento, nonché una demo di codice che mostra come apparirebbe la funzionalità in azione.
3. Se l'interesse per la funzionalità è sufficiente, essa viene discussa formalmente all'interno del gruppo di lavoro dell'organismo di standardizzazione pertinente. Per esempio, le funzionalità CSS vengono generalmente discusse dal [CSS Working Group](https://www.w3.org/groups/wg/css/) (WG) (vedere anche la [pagina Wikipedia del CSS Working Group](https://en.wikipedia.org/wiki/CSS_Working_Group) per una descrizione e una storia più dettagliate). Prima che una nuova tecnologia web venga accettata, deve essere valutata rigorosamente per garantire che sia positiva per il web: per esempio, non deve introdurre problemi di sicurezza, deve essere [accessibile e compatibile](#accessibili_e_interoperabili) con altre tecnologie web e non deve dipendere da brevetti.
4. Per convalidare la funzionalità, accadono diverse cose. Questi punti possono verificarsi tutti nello stesso periodo del punto 3, o persino prima (i fornitori di browser talvolta implementano funzionalità proprietarie/non standard e cercano poi di standardizzarle):
   1. Uno o più fornitori di browser implementano una versione sperimentale della nuova funzionalità, spesso disabilitata per impostazione predefinita, ma che può essere abilitata dalle persone che desiderano testarla e fornire feedback.
   2. Un membro del gruppo di lavoro la aggiunge anche a una specifica tecnologica, affinché i fornitori di browser possano implementarla in modo coerente.
   3. Viene inoltre richiesto il feedback di altri fornitori di browser per conoscere i problemi che riscontrano con la proposta e quanto sia probabile che la implementino. Queste sono chiamate posizioni sugli standard. Vedere, per esempio, [Mozilla Standards Positions](https://mozilla.github.io/standards-positions/).
   4. Le persone coinvolte scrivono anche un'ampia suite di test per dimostrare che la funzionalità opera come descritto.

5. Alla fine, se tutto va bene, la funzionalità viene implementata in tutti i browser e può iniziare a essere usata nella creazione di siti web.

> [!NOTE]
> È perfettamente possibile che le persone che propongono la funzionalità, la implementano in un browser, creano la specifica, scrivono i test e raccolgono il feedback siano la stessa persona o le stesse persone.

È possibile trovare ulteriori informazioni sui processi di specifici organismi di standardizzazione. Vedere, per esempio:

- [W3C Process Document](https://www.w3.org/policies/process/)
- [WHATWG — Working Mode](https://whatwg.org/working-mode)
- [The TC39 Process](https://tc39.es/process-document/)

## Principi chiave degli standard web

I principi chiave del web, che lo rendono un settore unico ed entusiasmante in cui impegnarsi, sono i seguenti:

- Aperto alla contribuzione e all'uso e, pertanto, non gravato da brevetti né controllato da una singola entità privata.
- Accessibile e interoperabile.
- Non compromettere il web.

Esaminiamo ciascuno di questi aspetti in modo più dettagliato.

### Standard "aperti"

Uno degli aspetti chiave degli standard web, su cui TimBL e il W3C concordarono fin dall'inizio, è che il web (e le tecnologie web) dovessero essere **aperti**. Ciò significa che sono liberi sia da contribuire sia da utilizzare, e non sono gravati da brevetti/licenze. Questo è importante: se una tecnologia web si basa su tecnologie brevettate/concesse in licenza per funzionare, il titolare del brevetto può addebitare ai fornitori di browser che la implementano somme potenzialmente elevate, e tali costi verrebbero poi trasferiti agli utenti del browser.

Inoltre, poiché le tecnologie web vengono create apertamente, attraverso la collaborazione tra molte aziende diverse, nessuna singola azienda può controllarle, ed è un fatto davvero positivo. Non sarebbe desiderabile che una singola azienda decidesse improvvisamente di rendere l'intero web accessibile solo a pagamento, o rilasciasse una nuova versione di HTML che tutti devono acquistare per continuare a creare siti web, o, peggio ancora, decidesse di non essere più interessata e semplicemente lo disattivasse.

Gli standard aperti consentono al web di rimanere una risorsa pubblica disponibile gratuitamente, in cui chiunque può scrivere gratuitamente il codice per creare un sito web e contribuire al processo di creazione degli standard.

### Accessibili e interoperabili

Il web e i browser web sono progettati fondamentalmente affinché i contenuti web siano **accessibili** alle persone con disabilità. Originariamente era concepito come un grande strumento di equità, che consentisse alle persone di accedere alle informazioni indipendentemente dalle circostanze. Ciò significa che, per esempio:

- Le persone che non possono utilizzare un mouse o un dispositivo di puntamento possono usare la tastiera per navigare sul web.
- Le persone con disabilità visive possono ingrandire i contenuti oppure utilizzare un programma chiamato **screen reader** per leggere loro il contenuto ad alta voce e descrivere i controlli in modo comprensibile.

> [!NOTE]
> Si approfondirà il tema dell'[accessibilità](/it/docs/Learn_web_development/Core/Accessibility) più avanti nel percorso di apprendimento.

Inoltre, le tecnologie web devono essere **interoperabili**. Poiché le tecnologie web vengono implementate secondo standard pubblicati, i browser dovrebbero fornire lo stesso output renderizzato per un determinato input, ad esempio codice HTML, CSS o JS. In altre parole, un sito web dovrebbe funzionare in modo coerente su più browser.

### Non compromettere il web

Un'altra espressione che si sente nel contesto degli standard web aperti è "don't break the web". L'idea è che ogni nuova tecnologia web dovrebbe essere retrocompatibile con ciò che l'ha preceduta, in modo che i siti web esistenti continuino a funzionare come prima.

I fornitori di browser web dovrebbero essere in grado di implementare nuove tecnologie web senza provocare differenze nel rendering o nelle funzionalità che inducano gli utenti a pensare che un sito web sia rotto e, di conseguenza, a provare un altro browser.

## Panoramica delle moderne tecnologie web

Se si desidera diventare sviluppatori web front-end, occorre imparare diverse tecnologie. In questa sezione vengono descritte brevemente.

### HTML, CSS e JavaScript

[HTML](/it/docs/Web/HTML), [CSS](/it/docs/Web/CSS) e [JavaScript](/it/docs/Web/JavaScript) sono le tre principali tecnologie utilizzate per creare un sito web. Sono state incontrate nel [modulo precedente](/it/docs/Learn_web_development/Getting_started/Your_first_website), ma, per ricapitolare:

- HyperText Markup Language, o **HTML**, è un linguaggio di markup costituito da diversi elementi con cui è possibile avvolgere (marcare) i contenuti per attribuire loro significato (semantica) e struttura. Con un'analogia legata alla costruzione di una casa, HTML sarebbe come le fondamenta e i muri della casa, che le danno struttura e la tengono insieme.
- Cascading Style Sheets (**CSS**) è un linguaggio basato su regole utilizzato per applicare stili al proprio HTML, per esempio impostando i colori del testo e dello sfondo, aggiungendo bordi, animando elementi o disponendo una pagina in un determinato modo. Nell'analogia della casa, CSS è come la vernice, la carta da parati, i tappeti e i quadri usati per rendere bella la casa.
- **JavaScript** è il linguaggio di programmazione usato per aggiungere interattività ai siti web, dal cambio dinamico degli stili al recupero di aggiornamenti dal server, fino alla grafica 3D complessa.
  - Insieme a JavaScript si sente anche il termine **API**, che significa **Application Programming Interface**. Una API JavaScript è una funzionalità costruita sopra JavaScript che consente di controllare in modo gestibile altre porzioni di codice più complesse o altre funzionalità del computer, come dispositivi hardware quali webcam o microfono.
  - Nell'analogia della casa, JavaScript è come il fornello, il televisore, il microonde o l'asciugacapelli: le cose che conferiscono alla casa funzionalità utili.

### Altre tecnologie web

Sul web vengono usate anche altre tecnologie, per esempio:

- [HTTP](/it/docs/Web/HTTP) per comunicare tra client e server, come menzionato in precedenza.
- [SVG](/it/docs/Web/SVG) per creare e manipolare grafica vettoriale.
- [MathML](/it/docs/Web/MathML) per descrivere formule matematiche.

Tuttavia, HTML, CSS e JavaScript sono di gran lunga le tecnologie più importanti da imparare, quindi il percorso di apprendimento si concentrerà principalmente su queste.

## Strumenti

Dopo aver appreso le tecnologie standard e fondamentali utilizzate per creare pagine web, come HTML, CSS e JavaScript, si inizieranno presto a incontrare vari strumenti che possono rendere il lavoro più facile o più efficiente. Alcuni esempi includono:

- Gli [strumenti di sviluppo](/it/docs/Learn_web_development/Howto/Tools_and_setup/What_are_browser_developer_tools) nei browser moderni, che possono essere utilizzati per eseguire il debug del codice.
- Gli [strumenti di test](/it/docs/Learn_web_development/Extensions/Testing), che possono essere utilizzati per eseguire test e verificare se il codice si comporta come previsto.
- [Framework e librerie](/it/docs/Learn_web_development/Core/Frameworks_libraries) costruiti sopra JavaScript, che consentono di creare determinati tipi di siti web in modo molto più rapido ed efficace.
- I cosiddetti **linter** e **formatter**, che prendono un insieme di regole per lo stile di codifica, analizzano il codice e lo aggiornano affinché segua tali regole. Prettier, incontrato [in precedenza nel corso](/it/docs/Learn_web_development/Getting_started/Environment_setup/Code_editors#enhancing_your_code_editor_with_extensions), è un esempio di formatter.

## Linguaggi e framework lato server

HTML, CSS e JavaScript sono linguaggi front-end, o lato client, il che significa che vengono eseguiti dal browser per produrre un front-end di sito web utilizzabile dagli utenti.

Esiste un'altra classe di linguaggi chiamati linguaggi back-end, o lato server, il che significa che vengono eseguiti sul server prima che il risultato venga inviato al browser per essere visualizzato. Un uso tipico di un linguaggio lato server consiste nel recuperare dati da un database, generare HTML che contenga i dati e poi inviare l'HTML al browser affinché lo visualizzi all'utente.

Esempi di framework e linguaggi lato server includono ASP.NET (C#), Django (Python), Laravel (PHP) e Next.js (JavaScript).

Queste tecnologie non sono considerate "standard web": sono sviluppate da organizzazioni esterne ai processi di standardizzazione web di organizzazioni come W3C e WHATWG, sebbene alcune possano adottare processi altrettanto aperti.

### Statico rispetto a dinamico

Un altro modo in cui vengono spesso descritti i linguaggi lato client e lato server è **statico** e **dinamico**:

- Un semplice file HTML viene archiviato sul server. Quando viene richiesto, è consegnato al client senza modifiche e renderizzato dal browser. Poiché non cambia, viene definito "statico".
- Il codice lato server, come uno script Python o una pagina ASP.NET, genera contenuto HTML che varia in base ai dati passati al codice e restituisce poi tale HTML al client. Viene quindi definito "dinamico". Per esempio, la stessa pagina di previsioni meteorologiche può mostrare dati diversi a seconda che il tempo sia soleggiato o piovoso, della posizione dell'utente e delle sue preferenze: alcuni utenti potrebbero voler vedere il conteggio dei pollini e l'umidità, mentre altri potrebbero non essere interessati a tali dati e selezionare preferenze per nasconderli.

Spesso esiste una certa sovrapposizione tra i concetti di codice statico e dinamico. I linguaggi lato server definiscono di solito strutture HTML all'interno di un file di template, che tende a essere composto per lo più da HTML statico con alcune sezioni dinamiche speciali incluse, le quali cambiano a seconda dei dati da inserire.

## Migliori pratiche web

Sono state brevemente trattate le tecnologie utilizzate per creare siti web. Ora discutiamo delle migliori pratiche che gli sviluppatori web adottano generalmente per garantire che i loro siti web siano utilizzabili dal maggior numero possibile di persone.

Nello sviluppo web, la principale causa di incertezza deriva dal fatto che non si conosce quale combinazione di tecnologie utilizzerà ciascun utente per visualizzare il sito web:

- L'utente 1 potrebbe visualizzarlo su un iPhone, con uno schermo piccolo e stretto.
- L'utente 2 potrebbe visualizzarlo su un laptop Windows collegato a un monitor widescreen.
- L'utente 3 potrebbe avere una disabilità visiva e utilizzare uno screen reader per leggere e interagire con la pagina web.
- L'utente 4 potrebbe utilizzare un computer desktop molto vecchio, incapace di eseguire browser moderni.

Poiché non è possibile sapere esattamente cosa utilizzeranno gli utenti, occorre progettare in modo difensivo: rendere il sito web il più flessibile possibile, affinché tutti gli utenti sopra descritti possano usarlo, anche se non tutti potrebbero avere la stessa esperienza.

Durante gli studi si incontreranno i concetti seguenti, che rappresentano le migliori pratiche a cui idealmente i siti web dovrebbero aderire. Per il momento non è necessario preoccuparsene troppo. Nella maggior parte del corso cerchiamo di insegnarli in modo implicito: quando vengono insegnati HTML, CSS e JavaScript, gli esempi seguono le migliori pratiche quando possibile. Più avanti nel percorso di apprendimento verranno probabilmente esplorati insegnamenti espliciti in questi ambiti.

- **Miglioramento progressivo**
  - : Creare un'esperienza minima che fornisca le funzionalità essenziali a tutti gli utenti e sovrapporre un'esperienza migliore e altri miglioramenti nei browser che possono supportarli. Il miglioramento progressivo è spesso considerato poco importante, perché i browser tendono al giorno d'oggi a supportare le nuove funzionalità in modo più coerente e le persone tendono ad avere connessioni internet più veloci con limiti di utilizzo dei dati più elevati. Tuttavia, si considerino esempi come ridurre gli elementi decorativi per rendere più fluida un'esperienza mobile e risparmiare dati, oppure fornire un'esperienza più leggera e a bassa larghezza di banda agli utenti che pagano al megabyte o dispongono di connessioni a consumo.
- **Compatibilità cross-browser**
  - : Cercare di assicurarsi che la pagina web funzioni sul maggior numero possibile di dispositivi. Ciò include l'uso di tecnologie supportate da tutti i browser, la fornitura di esperienze migliori ai browser in grado di gestirle (miglioramento progressivo) e/o la scrittura di codice che ricorra a un'esperienza più semplice ma comunque utilizzabile nei browser meno recenti, definita **degradazione graduale**. Richiede inoltre test per verificare se qualcosa non funziona in determinati browser e ulteriore lavoro per correggere tali problemi.
- **Separazione dei livelli**
  - : Collocare contenuto (HTML), stile (CSS) e comportamento (JavaScript) in file di codice diversi, anziché accumularli tutti nello stesso punto. È una buona idea per molte ragioni, tra cui la gestione e la comprensione del codice, il lavoro di squadra e la separazione dei ruoli. In realtà, la separazione non è sempre netta. È un ideale a cui mirare quando possibile, piuttosto che un assoluto.
- **Responsive web design**
  - : Rendere flessibili funzionalità e layout affinché possano adattarsi automaticamente a browser diversi. Un esempio evidente è un sito web disposto in un modo in un browser desktop widescreen, ma visualizzato come un layout più compatto a colonna singola nei browser dei telefoni cellulari. Provare ora a regolare la larghezza della finestra del browser e osservare cosa accade al layout del sito.
- **Prestazioni**
  - : Fare in modo che i siti web si carichino il più rapidamente possibile, ma anche renderli intuitivi e facili da usare affinché gli utenti non si frustrino e non vadano altrove.
- **Internazionalizzazione**
  - : Rendere i siti web utilizzabili da persone di culture diverse, che parlano lingue diverse dalla propria. Vi sono considerazioni tecniche, come modificare il layout affinché funzioni comunque correttamente per lingue da destra a sinistra o dall'alto verso il basso, e considerazioni umane, come usare un linguaggio semplice e privo di gergo affinché culture diverse abbiano maggiori probabilità di comprendere il testo.
- **Privacy** e **sicurezza**
  - : Questi due concetti sono correlati ma diversi. La privacy riguarda il consentire alle persone di svolgere le proprie attività privatamente, senza spiarle né raccogliere più dati di quelli strettamente necessari. La sicurezza riguarda la costruzione del sito web in modo sicuro, affinché utenti malintenzionati non possano rubare informazioni in esso contenute allo sviluppatore o ai suoi utenti.

{{PreviousMenuNext("Learn_web_development/Getting_started/Web_standards/How_the_web_works", "Learn_web_development/Getting_started/Web_standards/How_browsers_load_websites", "Learn_web_development/Getting_started/Web_standards")}}
