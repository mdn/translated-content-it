---
title: Flussi di lavoro e processi
slug: Learn_web_development/Getting_started/Soft_skills/Workflows_and_processes
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork", "Learn_web_development/Getting_started/Soft_skills/Finding_a_job", "Learn_web_development/Getting_started/Soft_skills")}}

Un aspetto importante dei progetti tecnici che i principianti spesso trascurano è l'idea del quadro generale. Potrebbero imparare uno strumento o un linguaggio individuale, ma essere ignari di tutte le librerie, strumenti, sistemi e ruoli lavorativi che si combinano per fornire un'intera applicazione web. Le sezioni seguenti coprono diversi aspetti del quadro generale ad un livello alto.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        N/A
      </td>
    </tr>
    <tr>
      <th scope="row">Obiettivi di apprendimento:</th>
      <td>
        <ul>
          <li>Tipiche combinazioni di tecnologie nei progetti web.</li>
          <li>Ruoli lavorativi tipici in un team di sviluppo web.</li>
          <li>Fasi tipiche di un progetto tecnico, e dove sono coinvolti i diversi ruoli lavorativi.</li>
          <li>Processi comuni di gestione del lavoro, come agile e waterfall.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Combinazioni di tecnologie tipiche

Quando si costruisce un sito web, si utilizza una combinazione di diverse tecnologie, comunemente chiamata **tech stack**. Man mano che i siti web diventano più grandi e complessi, lo diventa anche il tech stack. Potrebbe iniziare in modo semplice quando si crea un demo e solo tu e alcuni colleghi lo visionerete. Tuttavia, il tech stack di un sito web di produzione apparentemente semplice potrebbe essere più complesso di quanto si pensi inizialmente, considerando che deve:

- Caricarsi rapidamente (questo è lo scopo delle [prestazioni](/it/docs/Learn_web_development/Extensions/Performance/why_web_performance)).
- Gestire un gran numero di utenti contemporaneamente (deve **scalare**).
- Essere ben progettato, in modo che gli utenti possano accedere facilmente alle informazioni e ai servizi che contiene.
- Essere facile per un team da gestire e mantenere.

A un livello molto alto, un tech stack di applicazioni web potrebbe apparire così:

```plain
Front-end
HTML, CSS, JavaScript
|
Back-end
Node.js, .NET, PHP, Python, or some other server-side language
|
Database
MySQL, Postgres, MongoDB, or some other database
|
Web server
Your own, built around a server product such as Apache, or a service like Netlify
```

> [!NOTE]
> Si vedranno spesso acronimi che si riferiscono a stack di tecnologie popolari, come [MEAN](https://www.mongodb.com/resources/languages/mean-stack) (MongoDB, Express, Angular, Node) o [LAMP](<https://en.wikipedia.org/wiki/LAMP_(software_bundle)>) (Linux, Apache, MySQL, PHP o Python).

Su MDN, siamo principalmente interessati alla parte front-end, ma anche questo può essere suddiviso in molteplici parti diverse. Prendi il front-end per esempio:

- Probabilmente utilizzerai un framework JavaScript (come [React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started)) per definire i componenti che si combinano per creare l'interfaccia utente.
- Il framework probabilmente utilizzerà un qualche tipo di linguaggio di template (come [Mustache](https://mustache.github.io/)) per definire la struttura HTML ma anche fornire funzionalità per includere dinamicamente contenuti variabili.
- Includerai informazioni per stilizzare il tuo contenuto tramite CSS in un modo compatibile con il framework. Questo potrebbe essere scritto in puro CSS, o in un framework CSS (come [Tailwind](https://tailwindcss.com/)) o in un preprocessore (come [Sass](https://sass-lang.com/)).
- Un progetto JavaScript dovrebbe includere test, per garantire che le nuove aggiunte di codice non compromettano la sua funzionalità. I test sono solitamente implementati utilizzando un framework di test (come [Jest](https://jestjs.io/)).
- I siti web più grandi utilizzeranno uno strumento di packaging/build (come [Parcel](https://parceljs.org/)) per migliorare le prestazioni riducendo le dimensioni dei file, rimuovendo componenti inutilizzati dal codice di produzione, ecc.
- E così via.

> [!NOTE]
> Sentirai spesso descrivere siti web e applicazioni come costruiti utilizzando specifici **pattern architetturali**. Ad esempio, [model-view-controller (MVC)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) è un pattern che molti framework JavaScript seguono, mentre [publish–subscribe (pub/sub)](https://dev.to/willvelida/the-publisher-subscriber-pattern-pubsub-messaging-10in) è comunemente usato dalle applicazioni di messaggistica. Non è particolarmente importante che tu comprenda questi pattern nel dettaglio, ma una certa familiarità può essere utile quando si cerca di capire un nuovo framework o strumento.

Ci saranno anche strumenti coinvolti al di fuori del tech stack effettivo per aiutarti a gestirlo o creare risorse per il sito web, come:

- Strumenti di pianificazione per aiutarti a pianificare ciò che farai nel corso del progetto a un livello alto (come [Miro](https://miro.com/)).
- Sistemi di controllo delle versioni (VCS). Probabilmente utilizzerai un VCS basato su [git](https://git-scm.com/), come [GitHub](https://github.com/).
- Pacchetti di progettazione grafica/interfaccia (come [Figma](https://www.figma.com/) o [Canva](https://www.canva.com/)).
- Strumenti di gestione dei progetti come [Trello](https://trello.com/) o [Asana](https://asana.com/).

OK, quindi c'è molto da assimilare. Il nostro consiglio è **non andare nel panico!** L'obiettivo di questo articolo non è preoccuparti facendoti pensare che improvvisamente hai 10 volte tante cose da imparare rispetto a prima. L'idea è semplicemente farti conoscere il quadro insieme dei progetti web, e darti una familiarità di base con alcuni dei termini che potresti incontrare.

Col tempo svilupperai alcune conoscenze su diversi degli strumenti e delle tecnologie sopra menzionati, ma non sarai un esperto in tutti, né avrai bisogno di esserlo — ecco a cosa servono i team. Per il momento, stai assolutamente facendo la cosa giusta imparando le competenze di base come HTML, CSS e JavaScript. Altri strumenti e specializzazioni verranno più avanti nella tua carriera.

## Ruoli lavorativi

In un team di sviluppo web, ci sono molti diversi ruoli lavorativi coinvolti; è utile capire cosa ciascuno di essi comporta:

- **Product manager**
  - : Responsabile dell'intero sito web da una prospettiva di prodotto — come si sta comportando il prodotto sul mercato rispetto ai suoi concorrenti? Quali sono i suoi punti di forza e di debolezza? Quali nuove funzionalità sta richiedendo il pubblico di riferimento, e quali sono le priorità più elevate? Quali sono i criteri principali di successo del sito web, e in che modo le nuove funzionalità recenti hanno contribuito a soddisfare quei criteri? Il product manager raccoglierà dati e redigerà report per aiutare il team a comprendere quanto sia efficace il loro lavoro e a prioritizzare il lavoro futuro.
- **Project manager**
  - : Responsabile dell'organizzazione del lavoro che il team deve fare. Il project manager creerà un piano di progetto con compiti prioritari e scadenze, assegnerà il personale per svolgere ciascun compito, terrà riunioni di verifica regolari per vedere se vengono raggiunti gli obiettivi di progresso ed emergere eventuali problemi, e aggiusterà il piano secondo necessità.
- **User experience (UX) designer**
  - : Responsabile di comprendere i bisogni del pubblico target del prodotto e progettare il flusso di lavoro/esperienza del prodotto in modo tale che quei bisogni siano soddisfatti nel modo più efficace possibile. Le tipiche domande UX sono "dove dovremmo indirizzare l'utente quando arriva sulla nostra homepage?" e "come possiamo rendere la registrazione per un account il più semplice e intuitiva possibile?" Questo lavoro è spesso accompagnato da ricerca e test degli utenti per comprendere meglio il pubblico di riferimento e creare wireframe per comunicare idee. L'UX designer è uno dei principali consumatori dei report del product manager.
- **Graphic designer**
  - : Responsabile del lavoro di design visivo sul progetto del sito web. I graphic designer sono responsabili per una varietà di discipline come la tipografia, la scelta delle combinazioni di colori, la creazione di icone e altre risorse grafiche, e la creazione di modelli di sito web basati sui wireframe dell'UX designer.
- **Front-end developer**
  - : Questo è (probabilmente) quello che intendi diventare se stai leggendo questo! I front-end developers usano HTML, CSS e JavaScript per creare la parte visiva del sito web con cui gli utenti interagiscono, dando vita ai mockup comportamentali e visuali creati dagli UX e graphic designers.
- **Back-end developer**
  - : Responsabile delle parti non visive del sito web. Scrivono codice back-end per richiedere dati interni, generare pagine HTML da template e elaborare dati esterni inviati dagli utenti. Gestiscono anche la configurazione del server web, mantenendo il sito sicuro, ecc.
- **Full-stack developer**
  - : Gestisce sia i compiti di sviluppo front-end che quelli di sviluppo back-end.
- **Quality assurance (QA) engineer**
  - : Responsabile del test delle nuove funzionalità per assicurarsi che funzionino correttamente e della segnalazione di bug, comunicando con gli sviluppatori per aiutarli a prioritizzare le correzioni necessarie.
- **Content specialist/technical writer**
  - : Responsabile di assicurare che il contenuto testuale del sito web funzioni nel miglior modo possibile per il pubblico di riferimento. Questo include la struttura delle informazioni e come viene navigata, le etichette del testo dell'interfaccia utente, i post del blog, il testo di marketing e la documentazione del prodotto.

### Ruoli lavorativi meno comuni

Altri ruoli lavorativi meno comuni includono:

- **User researcher**
  - : I team più grandi avranno spesso un ricercatore dedicato per fare ricerca e test sugli utenti.
- **Search Engine Optimization (SEO) specialist**
  - : Analizza il contenuto e la struttura del sito web e apporta modifiche che renderanno il sito web più visibile nei risultati di ricerca pertinenti. Vedi {{Glossary("SEO", "SEO")}} per ulteriori informazioni.

## Fasi tecniche del progetto

Un tipico progetto tecnico potrebbe avvenire in questo modo:

1. Il product manager identifica un nuovo set di requisiti degli utenti per il sito web.
2. Ne discute con il team, e si decide che questi requisiti possono essere soddisfatti aggiungendo una nuova funzionalità al sito web.
3. Il project manager discute con il team quali sono gli elementi di lavoro individuali necessari per creare la nuova funzionalità, e crea un [processo di lavoro per gestirli](#processi_di_gestione_del_lavoro).
4. L'UX designer progetta un flusso di lavoro per la nuova funzionalità che descrive come dovrebbe funzionare, e un wireframe per fornire un'idea di dove potrebbe adattarsi al sito.
5. Il graphic designer disegna un mockup che mostra come la funzionalità apparirà sul sito web, insieme ai font scelti e alla palette di colori.
6. Il content specialist scrive il testo UI richiesto dalla funzionalità, e la documentazione necessaria per supportarla.
7. Il back-end developer crea i sistemi necessari per archiviare e gestire in sicurezza i dati che alimentano la funzionalità.
8. Il front-end developer crea la funzionalità interattiva basata sui mockup del graphic designer e la collega al back-end in modo che recuperi i dati di cui ha bisogno.
9. Il QA engineer testa a fondo la nuova funzionalità, e redige un rapporto dettagliato sui problemi che trova.
10. Gli sviluppatori risolvono i bug che sono considerati abbastanza seri da bloccare il rilascio della funzionalità.
11. Una volta che i bug (che bloccano) sono stati risolti e il progetto è stato approvato, la funzionalità può essere messa in diretta sul sito web.

Questa è una visione semplificata — esisteranno altre fasi intorno all'implementazione della funzionalità stessa, e le fasi non saranno necessariamente tutte completate nell'ordine mostrato, ma ciò ti dà un'idea di cosa comporta.

## Processi di gestione del lavoro

Il project manager utilizzerà un qualche tipo di processo per gestire il progetto del sito web, monitorando i progressi sui diversi elementi di lavoro, assicurandosi che vengano svolti nell'ordine giusto e in tempo, ecc. I due principali tipi di processi sono:

- **Waterfall**
  - : Si riferisce alla gestione di un progetto in fasi chiare e fisse, dove ciascuna è dipendente dalla precedente, e non ci si aspetta troppi cambiamenti nei requisiti. In generale, viene consegnato un unico grande risultato alla fine del progetto. La gestione del team tende ad essere più burocratica, con meno autonomia.
    - I progetti waterfall tendono a essere meglio specificati all'inizio e hanno meno crescita dell'ambito (aggiunta di requisiti a metà progetto). Inoltre, rilasci di prodotto più grandi e meno frequenti sono più facili da gestire in termini di pianificazione del rilascio, marketing, erogazione di formazione e documentazione, ecc.
    - Tuttavia, il waterfall tende ad essere meno flessibile, e i cambiamenti avvengono molto più lentamente. Aspettare diversi mesi per una correzione di bug può essere frustrante.
- **Agile**
  - : Si riferisce alla gestione di un progetto in modo più flessibile, dove più fasi possono avanzare simultaneamente, e più piccoli risultati possono essere consegnati in vari traguardi differenti durante il progetto. Si prevedono cambiamenti nei requisiti e possono essere gestiti spostando le priorità secondo necessità. I team sono generalmente più autonomi.
    - I progetti agile sono flessibili e possono adattarsi più facilmente ai cambiamenti nei requisiti. Può anche essere piacevole avere rilasci più frequenti — i bug vengono corretti più rapidamente, l'innovazione avviene più spesso, e c'è sempre qualcosa di cui il team di marketing può parlare. I team agile parlano spesso di miglioramento continuo.
    - Tuttavia, c'è un rischio maggiore di crescita dell'ambito e slittamento delle scadenze, i progetti spesso non sembrano mai veramente finiti, e c'è un ritmo e una pressione costante per consegnare.

> [!NOTE]
> I team di sviluppo web spesso preferiscono lavorare con un processo agile, poiché lo sviluppo software è per sua natura soggetto a cambiamenti (a volte rapidi) nei requisiti a causa di nuovi bug, feedback degli utenti, strategie aziendali, ecc.

### Scrum e kanban

Esiste un tipo specifico di metodologia agile chiamata **scrum**, che ha un insieme fisso di regole su come viene eseguito un progetto. Ad esempio:

- La persona responsabile dello scrum è chiamata scrum master. Spesso questa è solo una figura di project manager con un altro nome.
- Il lavoro da fare è diviso in cicli, chiamati **sprint**, che sono tipicamente di due settimane.
- Prima di ogni sprint, vengono discussi potenziali nuovi elementi di lavoro, e se accettati nello sprint, vengono inseriti in un backlog.
- Gli elementi di lavoro vengono presi dal backlog e passano attraverso diverse fasi verso il completamento, come "in corso" e "in revisione".
- Lo scrum master tiene brevi **riunioni in piedi** quotidiane in cui ognuno parla dei progressi fatti e di eventuali problemi che potrebbero avere, così i problemi possono essere individuati presto.
- Alla fine di ogni sprint, lo scrum master tiene una riunione retrospettiva per rivedere cosa è andato bene, cosa non è andato così bene, e quali lezioni si possono apprendere prima del prossimo sprint.

Un altro tipo di metodologia agile è chiamata **kanban**, che ha meno regole rispetto a scrum, non utilizza sprint, e tende a concentrarsi di più sugli aspetti di miglioramento continuo di agile. Kanban è particolarmente utile per gestire processi continui che non hanno un chiaro fine definito, come i ticket di supporto clienti.

### Kanban boards

Strumenti come [Trello](https://trello.com/) e [Asana](https://asana.com/) forniscono visualizzazioni che mostrano lo stato di diversi elementi di lavoro in un progetto. Sono solitamente chiamati **Kanban boards**, anche se possono essere utilizzati per gestire diversi tipi di processi, non solo kanban. I Kanban boards consistono in colonne diverse, che possono rappresentare diversi stati del lavoro in un progetto scrum ("backlog", "da fare", "in corso", ecc.), diversi tipi di lavoro ("ricerca", "design", "sviluppo", ecc.) o qualsiasi altra cosa sia utile per il tuo progetto.

[GitHub Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects) fornisce un'altra buona opzione di strumenti, ed è gratuito da utilizzare — è sufficiente iscriversi per un account GitHub.

> [!CALLOUT]
>
> **Provalo**
>
> Dovresti informarti su questi processi e praticare a tracciare alcuni dei tuoi lavori o progetti personali utilizzando un kanban board. Non preoccuparti di utilizzare un complesso metodo scrum; kanban base va bene per il momento. Anche se stai facendo qualcosa da solo, può essere utile praticare il flusso di lavoro di:
>
> 1. Creare compiti.
> 2. Decidere quanto sono grandi o quanto tempo richiederanno.
> 3. Prioritizzare i compiti.
> 4. Metterli in ordine con date di scadenza.
> 5. Iniziare a lavorare su diversi compiti.
> 6. Impostare i loro stati ("in corso", "bloccato", "fatto", ecc.) man mano che il lavoro progredisce.
>
> Traccia il progresso di un progetto completo dall'inizio alla fine — prova a farlo con il tuo sito web o un progetto secondario di qualche tipo. Inoltre, prova a [contribuire a un progetto open source](/it/docs/Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork#participate_in_open_source) o due; molti di loro utilizzeranno un processo per tracciare il loro lavoro simile a quello che abbiamo descritto sopra.

## Vedi anche

- [What is a Tech Stack and How Do They Work?](https://www.mongodb.com/resources/basics/technology-stack), mongodb.com
- [Website development team structure: roles and processes](https://www.truemark.dev/blog/web-development-team-structure-role-process/), truemark.dev (2017)
- [Agile vs. Waterfall](https://www.productplan.com/learn/agile-vs-waterfall/), ProductPlan
- [What is Scrum?](https://www.scrum.org/learning-series/what-is-scrum/), scrum.org

{{PreviousMenuNext("Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork", "Learn_web_development/Getting_started/Soft_skills/Finding_a_job", "Learn_web_development/Getting_started/Soft_skills")}}
