---
title: Informazioni su Imparare lo sviluppo web
slug: Learn_web_development/About
l10n:
  sourceCommit: be1922d62a0d31e4e3441db0e943aed8df736481
---

MDN Imparare lo sviluppo web si propone di insegnare le competenze e le conoscenze fondamentali che un sviluppatore web front-end dovrebbe avere per l'occupabilità e la longevità nell'industria web odierna. Incorpora i valori che riteniamo il web debba avere: accessibilità, sostenibilità, usabilità, prestazioni e comunità. Ci piacerebbe che educatori, sviluppatori e studenti utilizzassero questa risorsa e promuovessero questi valori nel loro lavoro, nei loro insegnamenti e nei prodotti che costruiscono.

Questo contenuto è stato creato dalla comunità MDN con la revisione e il feedback degli esperti all'interno di Mozilla e nell'intera comunità web. Grazie per i tuoi preziosi contributi; sai chi sei!

## Contesto e motivazione

Abbiamo originariamente [lanciato la sezione Imparare lo Sviluppo Web di MDN nel 2016](https://hacks.mozilla.org/2016/06/learning-to-code-for-the-web-the-mdn-learning-area-welcomes-you/) con l'obiettivo di rendere MDN più accessibile ai non esperti e aiutare i principianti sviluppatori web a passare da "principiante a confortevole".

Il contenuto ebbe un buon successo, ma andando avanti di qualche anno, abbiamo notato che la struttura era sotto la media. I principianti desiderano davvero un percorso solido che possano seguire per ottenere le conoscenze di cui hanno bisogno, piuttosto che essere costretti a capire cosa imparare e quando.

Inoltre, Mozilla parla quotidianamente con professionisti del settore e riceviamo regolarmente feedback sui vuoti di conoscenza nei nuovi assunti. I responsabili delle assunzioni spesso osservano:

- Troppa attenzione sull'utilizzo dei framework per costruire rapidamente app web, combinata con una mancanza di comprensione delle tecnologie sottostanti questi framework. Questo porta a una mancanza di capacità di risoluzione dei problemi e a una minore occupabilità a lungo termine con il cambiamento degli strumenti.
- Una mancanza di pratiche core come la semantica, l'accessibilità e il design responsivo. Questo porta a una mancanza di focus sull'utente, portando a limitazioni di usabilità.
- Lacune nella conoscenza di come funzionano fondamentalmente i browser, come mostrano le informazioni e l'interattività che ottieni gratuitamente. Questo causa soluzioni sovracomplicate e spesso inaccessibili.
- Problem solving limitato, lavoro di squadra, ricerca e altre abilità trasversali fondamentali.

Di conseguenza, abbiamo creato un curriculum per aiutare a guidare le persone verso l'apprendimento di un set di competenze migliore, rendendole più occupabili e permettendo loro di costruire un web migliore, più accessibile e responsabile di domani. Vogliamo che abbiano le migliori possibilità di successo. Abbiamo [lanciato il Curriculum di MDN all'inizio del 2024](/en-US/blog/mdn-curriculum-launch/).

Tuttavia, abbiamo rapidamente ricevuto feedback in cui gli utenti trovavano confuso avere due risorse di apprendimento su MDN, con il curriculum/percorso di apprendimento in un posto e il contenuto di apprendimento in un altro. Di conseguenza, abbiamo [fuso il Curriculum nell'area di apprendimento a dicembre 2024](/it/docs/Learn_web_development/Changelog#december_2024).

## Pubblico di riferimento

### Studenti

Questo curriculum è utile per diversi gruppi di studenti:

- Studenti che vogliono ottenere un lavoro nell'industria, il che può comportare l'ottenimento di una qualifica o certificazione correlata. Il curriculum fungerà da guida per ciò che dovrebbero studiare.
- Sviluppatori web esistenti che vogliono "mantere aggiornate" le loro competenze, assicurandosi che il loro set di competenze sia attuale e identificando lacune nella loro conoscenza che dovrebbero approfondire.
- Sviluppatori web non front-end che hanno esperienza di sviluppo in altri ambiti (ad esempio, sviluppatori back-end o sviluppatori specifici per piattaforma), che desiderano entrare nello sviluppo web front-end e vogliono una guida sugli argomenti da apprendere.

### Educatori

Gli educatori possono utilizzare questo contenuto come guida per creare programmi, unità e specifiche di valutazione per un corso universitario, corso di college, coding school o simili in ambito web. Conformarsi agli obiettivi di apprendimento nei nostri articoli aiuterà a garantire che i corsi insegnino tecniche attuali e best practices, evitando pratiche scorrette e informazioni obsolescenti.

Per saperne di più, consulta la nostra pagina [Risorse per gli Educatori](/it/docs/Learn_web_development/Educators).

> [!NOTE]
> Il completo Curriculum MDN Imparare lo Sviluppo Web è disponibile come PDF comodo da condividere con i tuoi studenti e colleghi. [Scarica il Curriculum](https://github.com/mdn/curriculum/releases/latest/download/MDN-Curriculum.pdf).

## Scopo

Il termine _sviluppatore front-end_ può essere ambiguo; può significare cose diverse per persone diverse e chi lavora sul front-end può essere aspettato di svolgere una varietà di compiti diversi.

### Cosa è coperto

Questo insieme di articoli non tenta di insegnare ogni argomento che un sviluppatore web potrebbe essere ragionevolmente aspettato di conoscere in profondità. Il curriculum copre i seguenti aspetti:

- Competenze tecniche core come HTML semantico, CSS e JavaScript fondamentali.
- Best practices come accessibilità, design responsivo e teoria del design UI.
- Strumenti chiave come framework e controllo di versione.
- Abilità trasversali per promuovere il mindset e l'atteggiamento necessari per ottenere un lavoro.
- Conoscenza dell'ambiente come sistemi informatici e di file, navigazione web, basi della riga di comando e editor di codice.
- Diverse "estensioni" che riteniamo costituiscano competenze aggiuntive utili da apprendere man mano che gli sviluppatori iniziano ad espandere le loro conoscenze e sviluppare specializzazioni. Questo include:
  - Trasformazioni e animazione CSS
  - Categorie comuni di Web API (ad esempio, media, grafica e storage lato client)
  - Fondamentali dello sviluppo web lato server
  - Prestazioni
  - Sicurezza e privacy
  - Collaudo

### Livello di dettaglio

Gli argomenti presentati sono coperti in diversi livelli di dettaglio.

- Alcuni sono coperti in profondità, ad esempio i fondamenti di HTML e CSS. È importante avere una chiara comprensione di questi prima che uno studente proceda troppo lontano nel suo percorso di apprendimento.
- Alcuni sono coperti in modo più superficiale, ad esempio il controllo di versione o il collaudo. È importante capire cosa sono questi argomenti e iniziare con alcune basi, ma queste abilità possono essere ampliate mentre continui la tua carriera.

### Cosa non è coperto

Ci sono anche diverse aree che non copriamo esplicitamente in questo curriculum, ovvero:

- Copertura esaustiva delle lingue/pattaforme lato server. Forniamo un'introduzione a [Node.js (Express)](/it/docs/Learn_web_development/Extensions/Server-side/Express_Nodejs) e [Python (Django)](/it/docs/Learn_web_development/Extensions/Server-side/Django) poiché è utile per ogni sviluppatore web comprendere come funzionano le tecnologie HTTP e lato server. Tuttavia, non forniamo una copertura esaustiva su più piattaforme; sarebbe fuori portata per MDN.
- Copertura approfondita dei database relazionali tradizionali (ad esempio, [MySQL](https://dev.mysql.com/doc/) o [Postgres](https://www.postgresql.org/)) e di altri storage di dati lato server (ad esempio, database cloud come [MongoDB](https://www.mongodb.com/) o [Google Cloud Datastore](https://cloud.google.com/products/datastore)). Forniamo una breve introduzione a tali tecnologie nei nostri moduli di [Programmazione lato server](/it/docs/Learn_web_development/Extensions/Server-side).
- Argomenti approfonditi DevOps come piattaforme cloud per il provisioning e l'automazione (ad esempio, [Amazon AWS](https://aws.amazon.com/), [Google Cloud Platform](https://console.cloud.google.com/), e [Microsoft Azure](https://azure.microsoft.com/)) e strumenti di containerizzazione (ad esempio, [Kubernetes](https://kubernetes.io/) e [Docker](https://www.docker.com/)). Tocchiamo leggermente alcuni strumenti che sono considerati in ambito DevOps — come GitHub e strumenti di collaudo automatizzato — ma hanno un distinto crossover nello spazio dello sviluppatore front-end.
- Design grafico oltre le conoscenze di base descritte in [Design per sviluppatori](/it/docs/Learn_web_development/Core/Design_for_developers).
- Competenze relative a ruoli come la gestione di prodotto e programma (ad esempio, organizzazione, ricerca e pianificazione).

## Attribuzione

Questa risorsa è gratuita per chiunque da utilizzare. Se la trovi utile, ti chiediamo di considerare di fare quanto segue:

- Linkarla. Ad esempio, un educatore potrebbe includere il seguente nella loro informazione pubblica del programma:

  ```html
  <p>
    This course is based on
    <a href="https://developer.mozilla.org/en-US/curriculum/"
      >MDN Learn Web Development</a
    >.
  </p>
  ```

- Parlare ad altri di essa! Adoreremmo che il maggior numero possibile di studenti ed educatori iniziassero a utilizzare questo materiale e a convergere intorno ad esso come standard per la conoscenza di base di uno sviluppatore web.

> [!NOTE]
> Gli educatori dovrebbero usare questo materiale come guida, ma il suo utilizzo non implica approvazione da parte di Mozilla.

## Processo di aggiornamento

L'industria dello sviluppo web sta cambiando costantemente e rapidamente. Per mantenere le nostre raccomandazioni aggiornate, rivedremo regolarmente il nostro materiale, aggiorneremo il nostro [changelog](/it/docs/Learn_web_development/Changelog) e faremo un annuncio ogni anno, contattando i creatori di corsi conosciuti conformanti per informarli del cambiamento del corso e incoraggiarli a rivedere/aggiornare i loro corsi secondo necessità.

Intendiamo farlo nel Q2 di ogni anno, per dare il tempo agli educatori nel Q2/Q3 di implementare i cambiamenti prima dell'inizio del successivo anno accademico.

## Domande frequenti

### Domande sulla partnership Scrimba

#### Come fa MDN a sapere che i corsi di Scrimba sono di alta qualità e seguono le best practices?

Scrimba aveva già un'ottima reputazione prima che iniziassimo a parlare con loro di una partnership. Tuttavia, non ci siamo accontentati solo della parola della comunità. Abbiamo fatto una revisione approfondita del [Percorso di Carriera per Sviluppatori Frontend](https://scrimba.com/the-frontend-developer-career-path-c0j:details?via=mdn) (FDCP) di Scrimba e abbiamo fornito loro feedback sui possibili miglioramenti, concentrandoci sull'aumento della copertura delle best practices e sulla conformità ai nostri [moduli Core](/it/docs/Learn_web_development/Core). Scrimba ha implementato tutti i nostri feedback, e l'FDCP è ancora migliore di prima. Ora che è conforme al nostro Curriculum Core, siamo certi che sia allineato con gli standard di MDN.

#### MDN condivide i dati degli utenti con Scrimba?

Diamo priorità alla privacy e alla trasparenza degli utenti. L'unica informazione che MDN condivide con Scrimba è la navigazione dell'utente verso Scrimba da MDN, e questo avviene attraverso le proprie azioni seguendo un link contrassegnato come esterno.

Nei casi in cui incorporiamo contenuti Scrimba su MDN, Scrimba non vedrà i dati degli utenti fino a quando un utente non sceglie di interagire con i contenuti di Scrimba.

#### I contenuti di Scrimba non sono gratuiti. Non è in conflitto con la filosofia di MDN di fornire contenuti gratuiti?

Molti dei contenuti di Scrimba richiedono un abbonamento a pagamento, ma offrono anche diversi corsi completi che sono accessibili gratuitamente dopo la registrazione.

È anche importante sottolineare che i corsi di Scrimba non sono necessari per utilizzare MDN Imparare lo Sviluppo Web — sono un miglioramento per chi desidera pagare per un corso strutturato che copra il nostro curriculum core. Puoi comunque imparare tutti i nostri risultati di apprendimento gratuitamente lavorando attraverso i nostri articoli.

#### Viene rilasciata una certificazione al completamento del Percorso di Carriera per Sviluppatori Frontend di Scrimba?

Sì, una volta completati tutti gli argomenti del Percorso di Carriera per Sviluppatori Frontend, è possibile accedere a un certificato di completamento da condividere con potenziali datori di lavoro o includere nel tuo portfolio. Vedi [Dove posso trovare il mio certificato di completamento?](https://forum.scrimba.com/t/where-can-i-find-my-completion-certificate/43?via=mdn) per ulteriori informazioni.
