---
title: Flussi di lavoro e processi
slug: Learn_web_development/Getting_started/Soft_skills/Workflows_and_processes
l10n:
  sourceCommit: 48d220a8cffdfd5f088f8ca89724a9a92e34d8c0
---

{{PreviousMenuNext("Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork", "Learn_web_development/Getting_started/Soft_skills/Finding_a_job", "Learn_web_development/Getting_started/Soft_skills")}}

Un aspetto importante dei progetti tecnici che i principianti spesso trascurano è l'idea del quadro generale. Potrebbero imparare uno strumento o un linguaggio individuale, ma non essere consapevoli di tutte le librerie, gli strumenti, i sistemi e i ruoli lavorativi che si uniscono per fornire un'intera applicazione web. Le sezioni seguenti coprono diversi aspetti del quadro generale ad un livello elevato.

<table>
  <tbody>
    <tr>
      <th scope="row">Prerequisiti:</th>
      <td>
        N/A
      </td>
    </tr>
    <tr>
      <th scope="row">Risultati dell'apprendimento:</th>
      <td>
        <ul>
          <li>Combinazioni tecnologiche tipiche nei progetti web.</li>
          <li>Ruoli tipici in una squadra di sviluppo web.</li>
          <li>Fasi tipiche di un progetto tecnico, e dove entrano in gioco i diversi ruoli.</li>
          <li>Processi comuni di gestione del lavoro, come agile e waterfall.</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Combinazioni tecnologiche tipiche

Quando si costruisce un sito web, si utilizzerà una combinazione di tecnologie diverse, comunemente nota come **tech stack**. Man mano che i siti web diventano più grandi e complessi, lo stessa tech stack diventa più complesso. Potrebbe iniziare con semplicità quando si crea una demo e solo lei e pochi colleghi la visioneranno. Tuttavia, la tech stack di un sito web di produzione apparentemente semplice potrebbe essere più complessa di quanto inizialmente pensato quando si considera che deve:

- Caricarsi rapidamente (questo è l'obiettivo delle [prestazioni](/it/docs/Learn_web_development/Extensions/Performance/why_web_performance)).
- Gestire un gran numero di utenti contemporaneamente (deve **scalare**).
- Essere ben progettato, in modo che gli utenti possano facilmente accedere alle informazioni e ai servizi che contiene.
- Essere facile da mantenere e su cui lavorare per un team.

Ad un livello molto alto, una tech stack di un'applicazione web potrebbe assomigliare a questa:

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
> Vedrà spesso acronimi che si riferiscono a stack tecnologici popolari, come [MEAN](https://www.mongodb.com/resources/languages/mean-stack) (MongoDB, Express, Angular, Node) o [LAMP](<https://en.wikipedia.org/wiki/LAMP_(software_bundle)>) (Linux, Apache, MySQL, PHP o Python).

Su MDN, ci occupiamo principalmente della parte front-end, ma anche quella può essere suddivisa in molti pezzi diversi. Prenda per esempio il front-end:

- Probabilmente utilizzerà un framework JavaScript (come [React](/it/docs/Learn_web_development/Core/Frameworks_libraries/React_getting_started)) per definire i componenti che si uniscono per creare l'interfaccia utente.
- Il framework probabilmente utilizzerà qualche tipo di linguaggio di template (come [Mustache](https://mustache.github.io/)) per definire la struttura HTML ma anche per fornire funzionalità che consentono di includere dinamicamente contenuto variabile.
- Includerà informazioni per stilizzare il suo contenuto tramite CSS in modo compatibile con il framework. Questo potrebbe essere scritto in puro CSS, o in un framework CSS (come [Tailwind](https://tailwindcss.com/)) o pre-processore (come [Sass](https://sass-lang.com/)).
- Un progetto JavaScript dovrebbe includere test, per assicurarsi che eventuali nuove aggiunte di codice non compromettano la sua funzionalità. I test sono solitamente implementati utilizzando un framework di test (come [Jest](https://jestjs.io/)).
- I siti web più grandi utilizzeranno uno strumento di impacchettamento/costruzione (come [Parcel](https://parceljs.org/)) per migliorare le prestazioni riducendo le dimensioni dei file, rimuovendo componenti non utilizzati dal codice di produzione, ecc.
- E così via.

> [!NOTE]
> Spesso sentirà descrivere i siti web e le applicazioni come costruiti utilizzando modelli **architetturali** specifici. Ad esempio, il [model-view-controller (MVC)](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller) è un modello che molti framework JavaScript seguono, mentre il [publish–subscribe (pub/sub)](https://dev.to/willvelida/the-publisher-subscriber-pattern-pubsub-messaging-10in) è comunemente usato dalle applicazioni di messaggistica. Non è particolarmente importante che comprenda questi modelli in dettaglio, ma una certa familiarità può essere utile quando si cerca di comprendere un nuovo framework o strumento.

Esistono anche strumenti esterni alla tech stack per aiutarla a gestirla o creare asset per il sito web, come:

- Strumenti di pianificazione per aiutarla a pianificare ciò che intende fare nel corso del progetto ad un livello elevato (come [Miro](https://miro.com/)).
- Sistemi di controllo versione (VCS). Probabilmente utilizzerà un VCS basato su [git](https://git-scm.com/), come [GitHub](https://github.com/).
- Pacchetti di design grafico/interfaccia (come [Figma](https://www.figma.com/) o [Canva](https://www.canva.com/)).
- Strumenti di gestione progetti come [Trello](https://trello.com/) o [Asana](https://asana.com/).

OK, è un bel po' da assimilare. Il nostro consiglio è **non panichi!** L'obiettivo di questo articolo non è di preoccuparla facendole pensare che all'improvviso ha 10 volte più cose da imparare di quante ne avesse prima. L'idea è semplicemente quella di renderla consapevole del quadro generale in termini di progetti di siti web e darle una familiarità di base con alcuni dei termini che potrebbe incontrare.

Alla fine, svilupperà una conoscenza di diversi degli strumenti e delle tecnologie sopra citati, ma non sarà un esperto in tutti loro, né avrà bisogno di esserlo — è per questo che esistono i team. Per il momento, sta assolutamente facendo la cosa giusta imparando le competenze di base come HTML, CSS e JavaScript. Più strumenti e specializzazioni verranno più avanti nella sua carriera.

## Ruoli professionali

In un team di sviluppo web, ci sono molti ruoli diversi coinvolti; è utile capire cosa comporta ciascuno di essi:

- **Product manager**
  - : Responsabile del sito web nel suo complesso da una prospettiva di prodotto — come sta performando il prodotto sul mercato, rispetto ai suoi concorrenti? Quali sono i suoi punti di forza e di debolezza? Quali nuove funzionalità richiede il pubblico target e quali sono le priorità più elevate? Quali sono i principali criteri di successo del sito web e come hanno contribuito le recenti nuove funzionalità a soddisfare tali criteri? Il product manager raccoglierà dati e scriverà report per aiutare il team a comprendere quanto sia efficace il loro lavoro, e a dare priorità ai lavori futuri.
- **Project manager**
  - : Responsabile dell'organizzazione del lavoro che il team deve fare. Il project manager creerà un piano di progetto con attività prioritarie e date di scadenza, assegnerà il personale per ciascuna attività, terrà riunioni regolari per verificare se i target di progresso vengono raggiunti e evidenziare eventuali problemi, e aggiusterà il piano secondo necessità.
- **User experience (UX) designer**
  - : Responsabile della comprensione delle esigenze del pubblico target del prodotto, e della progettazione del flusso di lavoro/dell'esperienza del prodotto in modo che le esigenze di quel pubblico siano soddisfatte in modo più efficace. Domande tipiche dell'UX sono: "dove dovremmo indirizzare l'utente prima quando atterra sulla nostra homepage?" e "come possiamo rendere la registrazione di un account il più semplice e intuitiva possibile?" Questo lavoro è spesso accoppiato con ricerca e test degli utenti per comprendere meglio il pubblico target, e creando wireframe per comunicare idee. L'UX designer è uno dei principali consumatori dei report del product manager.
- **Graphic designer**
  - : Responsabile del lavoro di design visivo sul progetto del sito web. I graphic designer sono responsabili di una varietà di discipline come la tipografia, la scelta di schemi di colore, la creazione di icone ed altri elementi grafici, e la creazione di mockup del sito web basati sui wireframe dell'UX designer.
- **Front-end developer**
  - : Questo è (probabilmente) a cosa sta puntando se sta leggendo questo! I front-end developer utilizzano HTML, CSS e JavaScript per creare la parte visiva del sito web con cui gli utenti interagiscono, dando vita ai mockup comportamentali e visivi creati dagli UX e graphic designer.
- **Back-end developer**
  - : Responsabile delle parti non visuali del sito web. Scrivono codice back-end per richiedere dati interni, generare pagine HTML dai template, e processare i dati esterni inviati dagli utenti. Inoltre gestiscono la configurazione del server web, mantenendo il sito sicuro, ecc.
- **Full-stack developer**
  - : Gestisce le attività sia di sviluppo front-end che back-end.
- **Quality assurance (QA) engineer**
  - : Responsabile del test delle nuove funzionalità per assicurarsi che funzionino correttamente e della segnalazione di bug, comunicando con gli sviluppatori per aiutarli a dare priorità alle correzioni necessarie.
- **Content specialist/technical writer**
  - : Responsabile di garantire che il contenuto testuale del sito web funzioni al meglio per il pubblico target. Questo include la struttura delle informazioni e come viene navigata, le etichette del testo dell'interfaccia utente, i post del blog, il testo di marketing e la documentazione del prodotto.

### Ruoli professionali meno comuni

Altri ruoli lavorativi meno comuni includono:

- **User researcher**
  - : I team più grandi avranno spesso un ricercatore dedicato per fare ricerca e test sugli utenti.
- **Search Engine Optimization (SEO) specialist**
  - : Analizza il contenuto e la struttura del sito web e apporta modifiche che renderanno il sito più visibile nei risultati pertinenti dei motori di ricerca. Vedere {{Glossary("SEO", "SEO")}} per ulteriori informazioni.

## Fasi di progetto tecnico

Un tipico progetto tecnico potrebbe svolgersi così:

1. Il product manager identifica un nuovo set di requisiti utente per il sito web.
2. Discute con il team e si decide che questi requisiti possono essere soddisfatti aggiungendo una nuova funzionalità al sito web.
3. Il project manager discute con il team quali sono gli elementi di lavoro individuali necessari per creare la nuova funzionalità, e crea un [processo di lavoro per gestirli](#processi_di_gestione_del_lavoro).
4. L'UX designer progetta un flusso di lavoro per la nuova funzionalità che descrive come dovrebbe funzionare, e un wireframe per fornire un'idea di dove potrebbe adattarsi al sito.
5. Il graphic designer progetta un mockup che mostra come la funzionalità apparirà sul sito web, insieme ai font scelti e alla palette di colori.
6. Lo specialista del contenuto scrive il testo dell'interfaccia utente richiesto dalla funzionalità e la documentazione necessaria per supportarla.
7. Il back-end developer crea i sistemi necessari per memorizzare e gestire in modo sicuro i dati che alimentano la funzionalità.
8. Il front-end developer crea la funzionalità interattiva basata sui mockup del graphic designer e la collega al back-end in modo che recuperi i dati di cui ha bisogno.
9. Il QA engineer testa a fondo la nuova funzionalità e redige un rapporto dettagliato sui problemi riscontrati.
10. Gli sviluppatori correggono i bug considerati sufficientemente gravi da bloccare il rilascio della funzionalità.
11. Una volta corretti i bug (bloccanti) e il progetto è stato approvato, la funzionalità può essere messa online sul sito web.

Questa è una visione semplificata — esisteranno altre fasi attorno all'implementazione della funzionalità stessa, e le fasi non necessariamente saranno tutte completate nell'ordine mostrato, ma questo le dà un'idea di cosa implica.

## Processi di gestione del lavoro

Il project manager utilizzerà un qualche tipo di processo per gestire il progetto del sito web, monitorando i progressi sui vari elementi di lavoro, assicurandosi che vengano eseguiti nell'ordine corretto e in tempo, ecc. I due principali tipi di processo sono:

- **Waterfall**
  - : Si riferisce alla gestione di un progetto in fasi chiare e fisse, dove ciascuna è dipendente dalla precedente e non si prevedono troppi cambiamenti nei requisiti. Generalmente un unico grande risultato viene consegnato alla fine del progetto. La gestione del team tende a essere più burocratica, con meno autonomia.
    - I progetti Waterfall tendono ad essere meglio definiti all'inizio e hanno meno modifiche al campo d'azione (aggiunta di requisiti nel mezzo del progetto). Inoltre, rilasci di prodotto più grandi e meno frequenti sono più facili da gestire in termini di pianificazione del rilascio, marketing, consegna della formazione e documentazione, ecc.
    - Tuttavia, il Waterfall tende a essere meno flessibile e i cambiamenti avvengono molto più lentamente. Aspettare diversi mesi per una correzione di un bug può essere frustrante.
- **Agile**
  - : Si riferisce alla gestione di un progetto in modo più flessibile, dove possono progredire simultaneamente più fasi e molti piccoli risultati tendono a essere consegnati in vari punti di riferimento diversi durante il progetto. I cambiamenti nei requisiti sono previsti e possono essere gestiti modificando le priorità secondo necessità. I team sono generalmente più autonomi.
    - I progetti Agile sono flessibili e possono adattarsi più facilmente ai cambiamenti nei requisiti. Può essere anche piacevole avere rilasci più frequenti — i bug vengono corretti più velocemente, l'innovazione avviene più spesso, e c'è sempre qualcosa di cui il team di marketing può parlare. I team Agile spesso parlano di miglioramento continuo.
    - Tuttavia, esiste un maggiore rischio di modifiche nel campo d'azione e slittamento delle scadenze, i progetti spesso non sembrano mai finiti veramente, e c'è più continuità e pressione per consegnare.

> [!NOTE]
> I team di sviluppo web spesso preferiscono lavorare con un processo agile, poiché lo sviluppo software per sua natura è soggetto a (a volte rapidi) cambiamenti nei requisiti dovuti ai nuovi bug, feedback degli utenti, strategia aziendale, ecc.

### Scrum e kanban

Esiste un tipo specifico di metodologia agile chiamata **scrum**, che ha un set fisso di regole su come viene gestito un progetto. Ad esempio:

- La persona responsabile dello scrum si chiama scrum master. Questo è spesso solo il project manager con un altro nome.
- Il lavoro da fare è diviso in cicli, chiamati **sprint**, che durano tipicamente due settimane.
- Prima di ogni sprint, vengono discussi i nuovi potenziali elementi di lavoro, e se accettati nello sprint, vengono messi in un backlog.
- Gli elementi di lavoro vengono presi dal backlog e passano attraverso diverse fasi verso il completamento, come "in progresso" e "in revisione".
- Lo scrum master tiene brevi **riunioni in piedi** quotidiane in cui tutti parlano dei progressi fatti e di eventuali problemi che potrebbero avere, in modo che i problemi possano essere individuati presto.
- Alla fine di ogni sprint, lo scrum master tiene una riunione di retrospettiva per rivedere cosa è andato bene, cosa non è andato così bene e quali lezioni possono essere apprese prima del prossimo sprint.

Un altro tipo di metodologia agile è chiamata **kanban**, che ha meno regole rispetto allo scrum, non usa sprint, e tende a concentrarsi di più sugli aspetti di miglioramento continuo dell'agile. Il kanban è particolarmente utile per gestire processi continui che non hanno una chiara fine definita, come i ticket di supporto clienti.

### Kanban board

Strumenti come [Trello](https://trello.com/) e [Asana](https://asana.com/) forniscono visualizzazioni che mostrano lo stato dei diversi elementi di lavoro in un progetto. Di solito sono chiamate **Kanban board**, anche se possono essere usate per gestire diversi tipi di processo, non solo kanban. Le Kanban board consistono in diverse colonne, che possono rappresentare diversi stati di lavoro in un progetto scrum ("backlog", "da fare", "in progresso", ecc.), diversi tipi di lavoro ("ricerca", "design", "sviluppo", ecc.) o qualsiasi altra cosa sia utile per il suo progetto.

[GitHub Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects) offre un'altra buona opzione di strumenti, ed è gratuito da usare — deve solo iscriversi per un account GitHub.

> [!CALLOUT]
>
> **Provi**
>
> Dovrebbe leggere i processi sopra descritti, e praticare il tracciamento di alcuni dei suoi progetti lavorativi o personali usando una kanban board. Non si preoccupi di utilizzare una metodologia scrum complessa; il kanban di base va bene per il momento. Anche se sta facendo qualcosa da solo, può essere utile praticare il flusso di lavoro di:
>
> 1. Creare compiti.
> 2. Decidere quanto sono grandi o quanto tempo richiederanno.
> 3. Prioritizzare i compiti.
> 4. Metterli in ordine con date di scadenza.
> 5. Iniziare a lavorare sui diversi compiti.
> 6. Impostare i loro stati ("in progresso", "bloccato", "fatto", ecc.) man mano che il lavoro avanza.
>
> Monitorare il progresso di un progetto completo dall'inizio alla fine — lo provi con il suo stesso sito web o un progetto personale di qualche tipo. Inoltre, provi a [contribuire a un progetto open source](/it/docs/Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork#participate_in_open_source) o due; molti di loro utilizzeranno un processo per tenere traccia del loro lavoro simile a quello che abbiamo descritto sopra.

## Vedere anche

- [Cos'è un Tech Stack e Come Funziona?](https://www.mongodb.com/resources/basics/technology-stack), mongodb.com
- [Struttura del team di sviluppo del sito web: ruoli e processi](https://www.truemark.dev/blog/web-development-team-structure-role-process/), truemark.dev (2017)
- [Agile vs. Waterfall](https://www.productplan.com/learn/agile-vs-waterfall/), ProductPlan
- [Che cos'è Scrum?](https://www.scrum.org/learning-series/what-is-scrum/), scrum.org

{{PreviousMenuNext("Learn_web_development/Getting_started/Soft_skills/Collaboration_and_teamwork", "Learn_web_development/Getting_started/Soft_skills/Finding_a_job", "Learn_web_development/Getting_started/Soft_skills")}}
