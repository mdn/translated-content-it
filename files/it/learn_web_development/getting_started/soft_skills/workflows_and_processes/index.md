---
title: Flussi di lavoro e processi
slug: Learn_web_development/Getting_started/Soft_skills/Workflows_and_processes
l10n:
  sourceCommit: f542ed344953b3312fc92150bba11536667e288a
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork", "Learn_web_development/Getting_started/Soft_skills/Finding_a_job", "Learn_web_development/Getting_started/Soft_skills")}}

Un aspetto importante dei progetti tecnici che spesso sfugge ai principianti è avere un'idea del quadro generale. Si può imparare un singolo strumento o linguaggio, senza però conoscere tutte le librerie, gli strumenti, i sistemi e i ruoli professionali che concorrono alla realizzazione di un'intera applicazione web. Le sezioni seguenti trattano a grandi linee diversi aspetti del quadro generale.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        N/D
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Combinazioni tecnologiche tipiche nei progetti web.</li>
          <li>Ruoli professionali tipici in un team di sviluppo web.</li>
          <li>Fasi tipiche di un progetto tecnico e punti in cui intervengono i diversi ruoli professionali.</li>
          <li>Processi comuni di gestione del lavoro, quali agile e waterfall.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Combinazioni tecnologiche tipiche

Quando si crea un sito web, si utilizza una combinazione di tecnologie diverse, comunemente chiamata **tech stack**. Man mano che i siti web diventano più grandi e complessi, aumenta anche la complessità del tech stack. All'inizio potrebbe essere semplice, quando si crea una demo che verrà visualizzata solo da chi sviluppa e da pochi colleghi. Tuttavia, il tech stack di un sito web in produzione apparentemente semplice potrebbe essere più complesso di quanto sembri inizialmente, considerando che deve:

- Caricarsi rapidamente (questo è lo scopo delle [prestazioni](/it/docs/Learn_web_development/Extensions/Performance/why_web_performance)).
- Gestire contemporaneamente un grande numero di utenti (deve poter **scalare**).
- Essere ben progettato, in modo che gli utenti possano accedere facilmente alle informazioni e ai servizi che contiene.
- Essere facile da sviluppare e mantenere per un team.

A un livello molto generale, il tech stack di un'applicazione web potrebbe assomigliare a questo:

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
> Spesso si incontreranno acronimi che si riferiscono a tech stack popolari, come [MEAN](https://www.mongodb.com/resources/languages/mean-stack) (MongoDB, Express, Angular, Node) o [LAMP](<https://en.wikipedia.org/wiki/LAMP_(software_bundle)>) (Linux, Apache, MySQL, PHP o Python).

Su MDN ci occupiamo principalmente della parte front-end, ma anche questa può essere suddivisa in molti elementi diversi. Prendiamo ad esempio il front-end:

- Probabilmente verrà utilizzato un framework JavaScript, come [React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started), per definire i componenti che insieme costituiscono l'interfaccia utente.
- Il framework probabilmente utilizzerà un qualche tipo di linguaggio di templating, come [Mustache](https://mustache.github.io/), per definire la struttura HTML e fornire anche funzionalità per includere dinamicamente contenuti variabili.
- Verranno incluse informazioni per applicare stili ai contenuti tramite CSS in un modo compatibile con il framework. Questo può essere scritto in CSS puro, oppure con un framework CSS, come [Tailwind](https://tailwindcss.com/), o un preprocessore, come [Sass](https://sass-lang.com/).
- Un progetto JavaScript dovrebbe includere test, per assicurarsi che l'aggiunta di nuovo codice non comprometta le funzionalità. I test vengono in genere implementati utilizzando un framework di testing, come [Jest](https://jestjs.io/).
- I siti web più grandi utilizzeranno uno strumento di packaging/build, come [Parcel](https://parceljs.org/), per migliorare le prestazioni riducendo le dimensioni dei file, rimuovendo i componenti inutilizzati dal codice di produzione e così via.
- E così via.

> [!NOTE]
> Spesso si sentirà descrivere siti web e applicazioni come costruiti utilizzando specifici **pattern architetturali**. Ad esempio, [model-view-controller (MVC)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) è un pattern seguito da molti framework JavaScript, mentre [publish–subscribe (pub/sub)](https://dev.to/willvelida/the-publisher-subscriber-pattern-pubsub-messaging-10in) viene comunemente utilizzato dalle applicazioni di messaggistica. Non è particolarmente importante comprendere questi pattern nel dettaglio, ma una certa familiarità può essere utile quando si cerca di comprendere un nuovo framework o strumento.

Saranno inoltre coinvolti strumenti esterni al tech stack vero e proprio, utili per gestirlo o per creare risorse per il sito web, quali:

- Strumenti di pianificazione, utili per pianificare a grandi linee ciò che verrà fatto durante il progetto, come [Miro](https://miro.com/).
- Sistemi di controllo versione (VCS). Probabilmente verrà utilizzato un VCS basato su [git](https://git-scm.com/), come [GitHub](https://github.com/).
- Pacchetti per la progettazione grafica/dell'interfaccia, come [Figma](https://www.figma.com/) o [Canva](https://www.canva.com/).
- Strumenti di gestione dei progetti, come [Trello](https://trello.com/) o [Asana](https://asana.com/).

Va bene, sono molte informazioni da assimilare. Il consiglio è: **non farsi prendere dal panico!** Lo scopo di questo articolo non è preoccupare, facendo pensare che improvvisamente ci siano 10 volte più cose da imparare rispetto a prima. L'idea è semplicemente rendere consapevoli del quadro generale dei progetti web e fornire una familiarità di base con alcuni dei termini che si potrebbero incontrare.

Con il tempo si acquisirà una certa conoscenza di molti degli strumenti e delle tecnologie elencati sopra, ma non si diventerà esperti in tutti, né sarà necessario esserlo: è proprio a questo che servono i team. Per il momento, imparare le competenze fondamentali come HTML, CSS e JavaScript è assolutamente la scelta giusta. Altri strumenti e specializzazioni arriveranno più avanti nella carriera.

## Ruoli professionali

In un team di sviluppo web sono coinvolti molti ruoli professionali diversi; è utile comprendere cosa comporta ciascuno di essi:

- **Product manager**
  - : Responsabile dell'intero sito web dal punto di vista del prodotto: come si comporta il prodotto sul mercato rispetto ai concorrenti? Quali sono i suoi punti di forza e di debolezza? Quali nuove funzionalità richiede il pubblico di riferimento e quali hanno la massima priorità? Quali sono i principali criteri di successo del sito web e in che modo le nuove funzionalità recenti hanno contribuito a soddisfarli? Il product manager raccoglie dati e redige report per aiutare il team a comprendere l'efficacia del proprio lavoro e a definire le priorità del lavoro futuro.
- **Project manager**
  - : Responsabile dell'organizzazione del lavoro che il team deve svolgere. Il project manager crea un piano di progetto con attività prioritarie e scadenze, assegna il personale a ciascuna attività, tiene riunioni di aggiornamento regolari per verificare se gli obiettivi di avanzamento vengono raggiunti e far emergere eventuali problemi, quindi modifica il piano secondo necessità.
- **Designer dell'esperienza utente (UX)**
  - : Responsabile della comprensione delle esigenze del pubblico di riferimento del prodotto e della progettazione del flusso/dell'esperienza del prodotto affinché tali esigenze siano soddisfatte nel modo più efficace. Domande UX tipiche sono: "dove dovrebbe essere indirizzato l'utente quando arriva sulla homepage?" e "come rendere la registrazione di un account il più semplice e intuitiva possibile?". Questo lavoro è spesso associato a ricerche e test sugli utenti, per comprendere meglio il pubblico di riferimento, e alla creazione di wireframe per comunicare le idee. Il designer UX è uno dei principali utilizzatori dei report del product manager.
- **Graphic designer**
  - : Responsabile del lavoro di progettazione visiva nel progetto del sito web. I graphic designer si occupano di varie discipline quali tipografia, scelta delle combinazioni di colori, creazione di icone e altre risorse grafiche, e creazione di mockup del sito web basati sui wireframe del designer UX.
- **Sviluppatore front-end**
  - : È probabilmente il ruolo a cui si aspira se si sta leggendo questo contenuto. Gli sviluppatori front-end utilizzano HTML, CSS e JavaScript per creare la parte visiva del sito web con cui gli utenti interagiscono, dando vita ai mockup comportamentali e visivi creati dai designer UX e graphic designer.
- **Sviluppatore back-end**
  - : Responsabile delle parti non visive del sito web. Scrive codice back-end per richiedere dati interni, generare pagine HTML da template e elaborare dati esterni inviati dagli utenti. Si occupa inoltre della configurazione del server web, della sicurezza del sito e così via.
- **Sviluppatore full-stack**
  - : Gestisce sia le attività di sviluppo front-end sia quelle di sviluppo back-end.
- **Ingegnere di quality assurance (QA)**
  - : Responsabile del testing delle nuove funzionalità, per assicurarsi che funzionino correttamente, e della segnalazione di bug, comunicando con gli sviluppatori per aiutarli a definire le priorità delle correzioni necessarie.
- **Specialista dei contenuti/redattore tecnico**
  - : Responsabile di assicurare che il contenuto testuale del sito web funzioni nel miglior modo possibile per il pubblico di riferimento. Questo include la struttura delle informazioni e il modo in cui vi si naviga, le etichette testuali dell'interfaccia utente, i post del blog, i testi di marketing e la documentazione del prodotto.

### Ruoli professionali meno comuni

Altri ruoli professionali meno comuni includono:

- **Ricercatore utente**
  - : I team più grandi spesso dispongono di un ricercatore dedicato alle ricerche e ai test sugli utenti.
- **Specialista in Search Engine Optimization (SEO)**
  - : Analizza il contenuto e la struttura del sito web e apporta modifiche che rendono il sito più visibile nei risultati pertinenti dei motori di ricerca. Per ulteriori informazioni, vedere {{Glossary("SEO", "SEO")}}.

## Fasi di un progetto tecnico

Un progetto tecnico tipico potrebbe svolgersi in questo modo:

1. Il product manager individua una nuova serie di requisiti utente per il sito web.
2. Ne discute con il team e si decide che tali requisiti possono essere soddisfatti aggiungendo una nuova funzionalità al sito web.
3. Il project manager discute con il team quali sono le singole attività necessarie per creare la nuova funzionalità e crea un [processo di lavoro per gestirle](#processi_di_gestione_del_lavoro).
4. Il designer UX progetta un flusso di lavoro per la nuova funzionalità, descrivendo come dovrebbe funzionare, e un wireframe per fornire un'idea di dove potrebbe inserirsi nel sito.
5. Il graphic designer progetta un mockup che mostra l'aspetto della funzionalità sul sito web, insieme ai font scelti e alla palette di colori.
6. Lo specialista dei contenuti scrive il testo dell'interfaccia utente richiesto dalla funzionalità e la documentazione necessaria a supportarla.
7. Lo sviluppatore back-end crea i sistemi necessari per archiviare e gestire in sicurezza i dati che alimentano la funzionalità.
8. Lo sviluppatore front-end crea la funzionalità interattiva sulla base dei mockup del graphic designer e la collega al back-end affinché recuperi i dati necessari.
9. L'ingegnere QA testa approfonditamente la nuova funzionalità e redige un report dettagliato sui problemi rilevati.
10. Gli sviluppatori correggono i bug ritenuti sufficientemente gravi da dover bloccare il rilascio della funzionalità.
11. Una volta corretti i bug bloccanti e approvato il progetto, la funzionalità può essere pubblicata sul sito web.

Questa è una visione semplificata: esisteranno altre fasi attorno all'implementazione della funzionalità stessa e le fasi non verranno necessariamente completate tutte nell'ordine mostrato, ma questo fornisce un'idea di ciò che è coinvolto.

## Processi di gestione del lavoro

Il project manager utilizzerà un qualche tipo di processo per gestire il progetto del sito web, monitorando l'avanzamento delle diverse attività, assicurandosi che vengano completate nell'ordine corretto e nei tempi previsti, e così via. I due principali tipi di processo sono:

- **Waterfall**
  - : Indica la gestione di un progetto in fasi chiare e fisse, in cui ciascuna dipende dalla precedente e non sono previste troppe modifiche ai requisiti. Generalmente, al termine del progetto viene consegnato un unico grande risultato. La gestione del team tende a essere più burocratica, con minore autonomia.
    - I progetti waterfall tendono a essere meglio specificati all'inizio e hanno meno scope creep, ovvero aggiunte di requisiti durante il progetto. Inoltre, rilasci di prodotto più grandi e meno frequenti sono più semplici da gestire in termini di pianificazione dei rilasci, marketing, erogazione di formazione e documentazione e così via.
    - Tuttavia, waterfall tende a essere meno flessibile e i cambiamenti avvengono molto più lentamente. Attendere diversi mesi per una correzione di bug può essere frustrante.
- **Agile**
  - : Indica la gestione di un progetto in modo più flessibile, in cui più fasi possono avanzare contemporaneamente e diversi risultati più piccoli tendono a essere consegnati in varie milestone del progetto. Sono previste modifiche ai requisiti, che possono essere gestite spostando le priorità secondo necessità. I team sono generalmente più autonomi.
    - I progetti agile sono flessibili e possono adattarsi più facilmente alle modifiche dei requisiti. Può inoltre essere utile avere rilasci più frequenti: i bug vengono corretti più rapidamente, l'innovazione avviene più spesso e il team di marketing ha sempre qualcosa di cui parlare. I team agile parlano spesso di miglioramento continuo.
    - Tuttavia, esiste un rischio maggiore di scope creep e slittamento delle scadenze, i progetti spesso non sembrano mai davvero conclusi e vi sono un ritmo e una pressione costanti per consegnare.

> [!NOTE]
> I team di sviluppo web spesso preferiscono lavorare con un processo agile, poiché lo sviluppo software è per sua natura soggetto a cambiamenti dei requisiti, talvolta rapidi, dovuti a nuovi bug, feedback degli utenti, strategia aziendale e così via.

### Scrum e kanban

Esiste un tipo specifico di metodologia agile chiamato **scrum**, che dispone di un insieme fisso di regole su come viene gestito un progetto. Ad esempio:

- La persona responsabile dello scrum è chiamata scrum master. Spesso si tratta semplicemente del project manager con un nome diverso.
- Il lavoro da svolgere è suddiviso in cicli, chiamati **sprint**, che durano tipicamente due settimane.
- Prima di ogni sprint vengono discusse le potenziali nuove attività e, se accettate nello sprint, vengono inserite in un backlog.
- Le attività vengono prelevate dal backlog e attraversano diverse fasi fino al completamento, come "in progress" e "in review".
- Lo scrum master tiene brevi **stand-up meeting** giornalieri, durante i quali tutti parlano dei progressi compiuti e di eventuali problemi riscontrati, in modo che i problemi possano essere intercettati tempestivamente.
- Al termine di ogni sprint, lo scrum master tiene una riunione retrospettiva per esaminare ciò che è andato bene, ciò che non è andato altrettanto bene e quali insegnamenti possono essere tratti prima dello sprint successivo.

Un altro tipo di metodologia agile è chiamato **kanban**; ha meno regole rispetto a scrum, non utilizza sprint e tende a concentrarsi maggiormente sugli aspetti di miglioramento continuo di agile. Kanban è particolarmente utile per gestire processi continui che non hanno una conclusione chiaramente definita, come i ticket di assistenza clienti.

### Bacheche kanban

Strumenti come [Trello](https://trello.com/) e [Asana](https://asana.com/) forniscono visualizzazioni che mostrano lo stato delle diverse attività di un progetto. Sono generalmente chiamate **bacheche kanban**, anche se possono essere utilizzate per gestire diversi tipi di processi, non solo kanban. Le bacheche kanban sono composte da diverse colonne, che possono rappresentare diversi stati del lavoro in un progetto scrum ("backlog", "todo", "in progress" e così via), diversi tipi di lavoro ("research", "design", "development" e così via) o qualsiasi altra cosa utile per il progetto.

[GitHub Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects) offre un'altra valida opzione di strumenti ed è gratuito: basta registrarsi per un account GitHub.

## Esercitarsi con i flussi di lavoro di progetto

È consigliabile approfondire i processi descritti sopra ed esercitarsi a tracciare parte del proprio lavoro o dei propri progetti personali utilizzando una bacheca kanban. Non è necessario preoccuparsi di utilizzare una metodologia scrum complessa: per il momento è sufficiente il kanban di base. Anche lavorando da soli, può essere utile esercitarsi nel flusso di lavoro di:

1. Creare attività.
2. Decidere quanto sono grandi o quanto tempo richiederanno.
3. Stabilire le priorità delle attività.
4. Ordinarle con delle scadenze.
5. Iniziare a lavorare su diverse attività.
6. Impostare i relativi stati ("in progress", "blocked", "done" e così via) man mano che il lavoro avanza.

Tracciare l'avanzamento di un progetto completo dall'inizio alla fine: provare con il proprio sito web o con un progetto secondario di qualche tipo. Inoltre, provare a [contribuire a uno o due progetti open source](/it/docs/Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork#participate_in_open_source); molti di essi utilizzeranno un processo di tracciamento del lavoro simile a quello descritto sopra.

## Vedere anche

- [Che cos'è un Tech Stack e come funziona?](https://www.mongodb.com/resources/basics/technology-stack), mongodb.com
- [Struttura di un team di sviluppo web: ruoli e processi](https://www.truemark.dev/blog/web-development-team-structure-role-process/), truemark.dev (2017)
- [Agile vs. Waterfall](https://www.productplan.com/learn/agile-vs-waterfall), ProductPlan
- [Che cos'è Scrum?](https://www.scrum.org/learning-series/what-is-scrum/), scrum.org

{{PreviousMenuNext("Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork", "Learn_web_development/Getting_started/Soft_skills/Finding_a_job", "Learn_web_development/Getting_started/Soft_skills")}}
